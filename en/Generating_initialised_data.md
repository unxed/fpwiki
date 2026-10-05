# Generating initialised data

│ **English (en)** │

## Contents

  * 1 History
  * 2 The need for typed initialised data
  * 3 Generating typed initialised data
    * 3.1 Setup
    * 3.2 Generating data
      * 3.2.1 Basic routines
      * 3.2.2 Generating composite expressions
      * 3.2.3 Generating aggregate data
        * 3.2.3.1 Dealing with variant records and TP-style objects
        * 3.2.3.2 Arbitrarily structured data
          * 3.2.3.2.1 Field names
          * 3.2.3.2.2 Placeholder data
      * 3.2.4 Helpers
    * 3.3 Finalising the data
    * 3.4 Contiguous data sections that can be dead-stripped (resourcestrings)
    * 3.5 Nested data generators
  * 4 Final remarks



# History

Originally, the only way to generate initialised data in the compiler was by directly generating [assembler statements](<The_assembler_output.md> "The assembler output") in the parser (e.g. while handling typed constants) or the code generator (e.g. for rtti and VMTs). In particular, the typed constant parser almost directly translated all definitions into tai* assembler constant entities. 

The typed constant parser was made more generic for the [JVM port](<FPC_JVM.md> "FPC JVM"). This was required because e.g. records had to be implemented as classes on that platform and it is not possible to define a class constant with pre-initialised fields in Java Bytecode. 

The existing typed constant parsing code (from `compiler/ptconst.pas`) was refactored into two classes (`compiler/ngtcon.pas`): a base class called _ttypedconstbuilder_ and its subclass _tasmlisttypedconstbuilder_. The base class mostly handles the parsing of the typed constants, while the subclass handles the generation of the initialised data into the assembler list (tai*). Additionally, a _tnodetreetypedconstbuilder_ subclass was implemented to handle data initialisation through the generation of parser nodes such as explicit assignment statements. These nodes are then added to the initialisation code of the current unit, which is used to initialise complex data structures on the JVM platform. 

This extra functionality by itself did not solve the issue of generating initialised data outside typed constant definitions from the original program. In part this was not necessary for the JVM port as e.g. RTTI and VMTs are automatically deduced by the JVM from the byte code. It was also alleviated by the introduction of `compiler/symcreat.str_parse_typedconst()` routine, which can be passed a string of (potentially compiler-internally generated) Pascal code and transforms it into initialised data. It requires the expression to be valid Pascal though, which means that no compiler-internal identifiers can be used and that all used types must be defined already and accessible via a valid Pascal identifier. 

# The need for typed initialised data

Unlike the JVM port, the LLVM port should be able to handle the same Pascal code that is supported on so-called "native" platforms. Its byte code can represent arbitrarily structured initialised data, but unlike other targets it requires type information to be attached to every bit of data. The existing _tasmlisttypedconstbuilder_ therefore had to be extended again, and furthermore all initialised data generated in other places also needs to get type information. 

This has been implemented in the `compiler/aamscnst.pas` unit via the _ttai_typedconstbuilder_ class and its descendants. All typed constant parsing (for non-JVM platforms) remained in the _tasmlisttypedconstbuilder_ , but the generation of the initialisation data itself is now handled via the _ttai_typedconstbuilder_. Since this latter class does not depend on Pascal code as input, it can also be used more easily elsewhere in the compiler to generate initialised data. 

# Generating typed initialised data

_Note that everything that follows only holds for non-JVM targets. Due to its specific nature, the JVM target has to be handled differently and is out of scope of the rest of this document._

The old method of directly generating tai* entities as initialised data still works for non-LLVM targets. This means that existing code shared by all targets can be gradually converted to the new approach, rather than that everything needs to be switched over at once. New shared code should however use either the aforementioned `compiler/symcreat.str_parse_typedconst()` or the _ttai_typedconstbuilder_ to ensure compatibility with the LLVM and potentially other "high level" targets. 

A major advantage of the new way is also that it will automatically insert padding bytes where required for alignment, which was a major source of errors in the old approach. 

## Setup

To start generating an initialised data entry, create a new specialised _ttai_typedconstbuilder_ instance 
    
    
    var
      tcb: ttai_typedconstbuilder;
    begin
      tcb:=ctai_typedconstbuilder.create;
      ...
    end;
    

_ctai_typedconstbuilder_ is a class reference variable that holds the _ttai_typedconstbuilder_ descendant type appropriate for the current target. 

## Generating data

All generated data consists of two aspects: the data itself and its type. The data is represented by tai* subclasses, just like before. The type is represented by a tdef subclass. 

There are three kinds of initialised data: 

  * **Fundamental or simple constants.** Examples are ordinal constants, floating point numbers and pointer constants (including addresses of static variables and procedures).
  * **Composite expression.** The result type of such an expression is a fundamental/simple constant, but the expression that initialises it is more complex. Examples are expressions that contain type conversions, array indexations (with a constant index, obviously) and record subscripts.
  * **Aggregate constants.** These are initialised records and (non-dynamic) arrays. Elements of aggregate constants can in turn be any these three kinds of initialised data.



### Basic routines

The most basic routine to generate a fundamental/simple constant, is 
    
    
      procedure ttai_typedconstbuilder.emit_tai(p: tai; def: tdef);
    

As mentioned above, the first argument is the tai* entity that you would normally directly concatenate to the tasmlist, while the second one is a def that describes the data. 

There is one variant of this method: 
    
    
      procedure ttai_typedconstbuilder.emit_tai_procvar2procdef(p: tai; pvdef: tprocvardef);
    

The reason is that when taking the address of a method or a nested routine, the result is a complex procedural variable (procedure of object, procedure is nested, ...). However, sometimes we are only interested in the address of the method rather than in a complete _tmethod_ record. The above method can be used in that case, and it will also create an appropriate tdef to describe this address based on the _complex_ procvardef described by _pvdef_. 

Example: 
    
    
       ...
       { create a byte value with value 5 }
       tcb.emit_tai(tai_const.create_8bit(5),u8inttype);
       ...
    

### Generating composite expressions

In case of a type-safe byte code, we cannot just add an arbitrary offset to a symbol or reinterpret data without encoding this typecast explicitly. We therefore have build a composite expression that contains all necessary information. 

Composite expressions can be constructed via a queue interface of the _ttai_typedconstbuilder_ class. First, initialise the queue: 
    
    
      procedure ttai_typedconstbuilder.queue_init(todef: tdef);
    

_todef_ is the def of the entity to which the expression will be assigned. This ensures the queue itself can insert any necessary type conversions when parsing e.g. `const x: byte = 100;`, as the parser will interpret the _100_ as a native integer by default. 

Once a queue has been initialised, intermediate operations can be queued from outer to inner: 
    
    
      { queue an array/string indexing operation (performs all range checking,
        so it doesn't have to be duplicated in all descendents). }
      procedure ttai_typedconstbuilder.queue_vecn(def: tdef; const index: tconstexprint);
      { queue a subscripting operation }
      procedure ttai_typedconstbuilder.queue_subscriptn(def: tabstractrecorddef; vs: tfieldvarsym);
      { queue indexing a record recursively via several field names. The fields
        are specified in the inner to outer order (i.e., def.field1.field2) }
      function ttai_typedconstbuilder.queue_subscriptn_multiple_by_name(def: tabstractrecorddef; const fields: array of TIDString): tdef;
      { queue a type conversion operation }
      procedure ttai_typedconstbuilder.queue_typeconvn(fromdef, todef: tdef);
    

The operations that form the expression must be added to the queue from outermost to innermost, i.e. in the order they would be encountered by the parser if it would process a node tree built from the equivalent Pascal expression. 

Finally, the data element onto which these queued operations should be applied must be supplied: 
    
    
      { finalise the queue (so a new one can be created) and flush the
        previously queued operations, applying them in reverse order on a...}
      { ... procdef }
      procedure ttai_typedconstbuilder.queue_emit_proc(pd: tprocdef);
      { ... staticvarsym }
      procedure ttai_typedconstbuilder.queue_emit_staticvar(vs: tstaticvarsym);
      { ... labelsym }
      procedure ttai_typedconstbuilder.queue_emit_label(l: tlabelsym);
      { ... constsym }
      procedure ttai_typedconstbuilder.queue_emit_const(cs: tconstsym);
      { ... asmsym/asmlabel }
      procedure ttai_typedconstbuilder.queue_emit_asmsym(sym: tasmsymbol; def: tdef);
      { ... an ordinal constant }
      procedure ttai_typedconstbuilder.queue_emit_ordconst(value: int64; def: tdef);
    

As documented, this final operation also flushes the queued operations, so it can/must be initialised anew when another composite expression is to be queued afterwards. 

Example: 
    
    
      ...
      { encode @recvar.arrayfield[2], with arrayfield an "array[0..3] of word" }
      tcb.queue_init(getpointerdef(u16inttype));
      tcb.queue_vecn(arrayfielddef,2);
      tcb.queue_subscriptn(recvardef,arrayfieldsym); { or, queue_subscriptn_multiple_by_name(recvardef,['ARRAYFIELD']); }
      tcb.queue_emit_staticvar(recvarsym);
      ...
    

### Generating aggregate data

Just like with composite expressions, a typesafe byte code needs to be explicitly told what the structure is of an aggregate (record, array). In particular, all data belonging to a single aggregate must be explicitly grouped together. One can no longer just emit a label and then sequentially dump all data after it. 

The most basic way to start and finish an aggregate is using the following methdos: 
    
    
      { begin a potential aggregate type. Must be called for any type
        that consists of multiple tai constant data entries, or that
        represents an aggregate at the Pascal level (a record, a non-dynamic
        array, ... }
      procedure ttai_typedconstbuilder.maybe_begin_aggregate(def: tdef);
      { end a potential aggregate type. Must be paired with every
        maybe_begin_aggregate }
      procedure ttai_typedconstbuilder.maybe_end_aggregate(def: tdef);
    

The reason for the "maybe_" at the start of those method names is that the way a particular Pascal data type is represented in the typed bytecode (or plain assembler code) may vary from platform to platform. On some platforms it may be represented as an aggregate, on others it may be a single, simple data element (e.g. a small set may just be an ordinal constant). As the comments indicate, they must nevertheless be called for any type for which you may emit multiple individual constant entities (e.g. multiple character bytes to represent a string) or that represent an aggregate in Pascal. 

Aggregates can, of course, be nested, and inside an aggregate you can emit both fundamental/simple constants and composite expressions. 

Example: 
    
    
      ...
      case def.stringtype of
        st_shortstring:
          begin
            ftcb.maybe_begin_aggregate(def);
            ftcb.emit_tai(Tai_const.Create_8bit(strlength),cansichartype);
            ftcb.emit_tai(Tai_string.Create_pchar(ca,def.size-1),getarraydef(cansichartype,def.size-1));
            ftcb.maybe_end_aggregate(def);
          end;
      ...
    

#### Dealing with variant records and TP-style objects

When initialising a variant record or an object, you always have to tell the _ttai_typedconstbuilder_ for which field is you'll be emitting data next, as this cannot be automatically determined. This can be done using the following property: 
    
    
      { set the fieldvarsym whose data we will emit next; needed
       in case of variant records, so we know which part of the variant gets
       initialised. Also in case of objects, because the fieldvarsyms are spread
       over the symtables of the entire inheritance tree }
      property ttai_typedconstbuilder.next_field: tfieldvarsym write set_next_field;
    

#### Arbitrarily structured data

In some cases, you do not have the tdef describing the structured data in advance. An example is RTTI info. Rather than manually constructing an appropriate tdef in advance, the _ttai_typedconstbuilder_ can do this for you: 
    
    
      { similar as above, but in case
        a) it's definitely a record
        b) the def of the record should be automatically constructed based on
           the types of the emitted fields
      }
      function ttai_typedconstbuilder.begin_anonymous_record(const optionalname: string; packrecords, recordalignmin, maxcrecordalign: shortint): trecorddef;
      function ttai_typedconstbuilder.end_anonymous_record: trecorddef;
    

If _optionalname_ is different from an empty string, a ttypesym with that name will be created for the constructed tdef (so it can be looked up again later under that name). The _packrecords_ parameter can be used to control the alignment of the fields. _recordalignmin_ and _maxcrecordalign_ can be specified because these settings are used internally by the _trecordsymtable_ , and may have to be different from the settings in the current source file (e.g. because we are generating data for use by routines in the system unit, which expect the default alignment settings to be active). 

The _ttai_typedconstbuilder.begin_anonymous_record()_ method immediately returns a trecorddef. Although the structure of the record is not yet known at that time, this is the tdef that will be completed once end_anonymous_record has been called. It can be used to e.g. already create a pointer to the recorddef, in case this is required for one of the fields inside. 

Example: 
    
    
      tcb.begin_anonymous_record('$fpc_intern_classtable_'+tostr(classtablelist.Count-1),packrecords,
        { use the target default settings }
        targetinfos[target_info.system]^.alignment.recordalignmin,
        targetinfos[target_info.system]^.alignment.maxCrecordalign);
      );
      tcb.emit_tai(Tai_const.Create_16bit(classtablelist.count),u16inttype);
      for i:=0 to classtablelist.Count-1 do
        begin
          classdef:=tobjectdef(classtablelist[i]);
          { type of the field }
          tcb.queue_init(voidpointertype);
          { reference to the vmt }
          tcb.queue_emit_asmsym(
            current_asmdata.RefAsmSymbol(classdef.vmt_mangledname,AT_DATA),
            tfieldvarsym(classdef.vmt_field).vardef);
        end;
      classtabledef:=tcb.end_anonymous_record;
    

##### Field names

The names of the field in the record are normally automatically generated. You can also specify the names, in case you later with to refer them through e.g. _ttai_typedconstbuilder. queue_subscriptn_multiple_by_name()_ , via the following property. It is automatically reset after emitting the next field. Usage: 
    
    
      { set the name of the next field that will be emitted for an anonymous
        record (also if that field is a nested anonymous record) }
      property ttai_typedconstbuilder.next_field_name: TIDString write set_next_field_name;
    

##### Placeholder data

You may not know the actual value of a data element before the data coming after it has been emitted, because it gets calculated while emitting this other data. A common example is a table count. While it's possible to process the data twice (only counting everything the first time), you can also use a placeholder instead: 
    
    
      { add a placeholder element at the current position that later can be
        filled in with the actual data (via ttypedconstplaceholder.replace)
    
        useful in case you have table preceded by the number of elements, and
        you cound the elements while building the table }
      function ttai_typedconstbuilder.emit_placeholder(def: tdef): ttypedconstplaceholder; virtual; abstract;
    

_def_ specifies the type of the data that will eventually be put at this location. Once you know the value, you can replace the placeholder returned by the above method with the actual data: 
    
    
      { same usage as ttai_typedconstbuilder.emit_tai }
      procedure ttypedconstplaceholder.replace(ai: tai; d: tdef); virtual; abstract;
    

Finally, free the _ttypedconstplaceholder_ instance. 

### Helpers

There are a number of helper routines that can be used to emit data for common datastructures: 
    
    
      { the datalist parameter specifies where the data for the string constant
        will be emitted (via an internal data builder) }
      function ttai_typedconstbuilder.emit_ansistring_const(list: TAsmList; data: pchar; len: asizeint; encoding: tstringencoding; newsection: boolean): tasmlabofs;
      function ttai_typedconstbuilder.emit_unicodestring_const(list: TAsmList; data: pointer; encoding: tstringencoding; winlike: boolean):tasmlabofs;
      { emits a tasmlabofs as returned by emit_*string_const }
      procedure ttai_typedconstbuilder.emit_string_offset(const ll: tasmlabofs; const strlength: longint; const st: tstringtype; const winlikewidestring: boolean; const charptrdef: tdef);virtual;
    
      { emit a shortstring constant, and return its def }
      function ttai_typedconstbuilder.emit_shortstring_const(const str: shortstring): tdef;
      { emit a guid constant }
      procedure ttai_typedconstbuilder.emit_guid_const(const guid: tguid);
      { emit a procdef constant }
      procedure ttai_typedconstbuilder.emit_procdef_const(pd: tprocdef);
      { emit an ordinal constant }
      procedure ttai_typedconstbuilder.emit_ord_const(value: int64; def: tdef);
    

## Finalising the data

Once all the data for a particular variable/constant/entity has been generated, it can be finalised. This means that this data will be associated with a tasmsymbol of a certain type (which should match the type of the expression), and some other options can be set as well: 
    
    
      { finalize the internal asmlist (if necessary) and return it.
        This asmlist will be freed when the builder is destroyed, so add its
        contents to another list first. This function should only be called
        once all data has been added. }
      function get_final_asmlist(sym: tasmsymbol; def: tdef; section: TAsmSectiontype; const secname: TSymStr; alignment: longint; const options: ttcasmlistoptions): tasmlist;
    

The _ttcasmlistoptions_ parameter is a set of the (currently) following flags: 
    
    
       { flags for the finalisation of the typed const builder asmlist }
       ttcasmlistoption = (
         { the tasmsymbol is a tasmlabel }
         tcalo_is_lab,
         { start a new section (e.g., because we don't know the current section
           type) }
         tcalo_new_section,
         { this symbol is the start of a block of data that should be
           dead-stripable/smartlinkable; may imply starting a new section, but
           not necessarily (depends on what the platform requirements are) }
         tcalo_make_dead_strippable
       );
    

Example: 
    
    
      list.concatList(tcb.get_final_asmlist(asmlab,getarraydef(cansichartype,len+1),sec_rodata_norel,'',sizeof(pint),[tcalo_is_lab]));
    

One the final asmlist has been extracted from a _ttai_typedconstbuilder_ , the only thing left to do is to free that builder. It is not possible to reuse it to construct a new initialised data entity. It will also free the list returned by _get_final_asmlist()_ (_concatList_ removes all elements from it when putting them in the target list, so they won't be freed). 

  


## Contiguous data sections that can be dead-stripped (resourcestrings)

Resourcestrings are unique compared to other kinds of data that are generated by the compiler. On the one hand, all resourcestrings in a single unit must appear right after each other in a section, so that they can be iterated like an array. On the other hand, resourcestrings that are not referenced anywhere in the final linked program should be removed by the linker. In terms of the high level typed constant builder, this kind of data is called a _vectorized dead-strippable section_. 

Such data requires one extra flag to be added to the flags of the _ctai_typedconstbuilder.create()_ constructor: 

  * for the first element of the array, add _tcalo_vectorized_dead_strip_start_
  * for every successive element, add _tcalo_vectorized_dead_strip_item_
  * for the marker of the end of the array, add _tcalo_vectorized_dead_strip_end_



Additionally, when getting the final asmlist, use the following method instead of _get_final_asmlist()_ like normally: 
    
    
    function ttypedconstplaceholder.get_final_asmlist_vectorized_dead_strip(def: tdef; const basename, itemname: TSymStr; st: TSymtable; alignment: longint): tasmlist;
    

Instead of a label, you provide a _basename_ , an _itemname_ and a symbol table (_st_) here, and the typed constant builder will create a label name out of that to use. The _basename_ identifies the kind of data (e.g. 'RESSTR' for resourcestrings), while the _itemname_ is an identifier to specify the individual element (e.g. the Pascal identifier for a resource string) The item name must be empty for the start and end sections, because it is automatically generated for consistency in those cases. The symbol table is the one passed to _make_mangledname()_ when creating the symbol names. 

## Nested data generators

Emitting e.g. a new pchar constant consists of two parts: on the one hand, there is the string data itself (a null-terminated array of char) and on the other hand, there is the pointer to this data. These two elements have different types (the pointer is relocatable, the string chars are not), and if you have e.g. an initialised array of pchar, then all of the string data can be placed in a single dead-strippable section/object (since either all of it will be referenced, or none of it — depending on whether the array with the pointers to all of this string data is referenced or not). 

These string chars are generally termed _internal data_ , as it is data that is only referenced internally by the initialised data entry that we are currently emitting. Creating and finalising a builder for such internal data is done using the following routines: 
    
    
      { returns a builder for generating data that is only referrenced by the
        typed constant date we are currently generating (e.g. string data for a
        pchar constant). Also returns the label that will be placed at the start
        of that data. list is the tasmlist to which the data will be added.
        secname can be empty to use a default }
      procedure ttai_typedconstbuilder.start_internal_data_builder(list: tasmlist; sectype: TAsmSectiontype; const secname: TSymStr; out tcb: ttai_typedconstbuilder; out l: tasmlabel);
      { finish a previously started internal data builder, including
        concatenating all generated data to the provided list and freeing the
        builder }
      procedure ttai_typedconstbuilder.finish_internal_data_builder(var tcb: ttai_typedconstbuilder; l: tasmlabel; def: tdef; alignment: longint);
    

ttai_typedconstbuilder.start_internal_data_builder() automatically generates the label to be placed at the start of this internal data entry, as its kind and name can depend on whether or not dead code/data stripping ("smart linking") is enabled, and on the target platform. _secname_ must either be empty, or the same for all internal data emitted for the same _sectype_ for a particular _ttai_typedconstbuilder_ instance (since all of that data must be placed in the same section). 

# Final remarks

All data that has been emitted via a _ttai_typedconstbuilder_ by definition is part of a single variable/constant, namely the one whose assembler symbol is passed to _get_final_asmlist()_. In particular, this means that if there is complex/structured data, it must be wrapped in an aggregate; you cannot just emit a label and then a bunch of bytes. The anonymous records functionality can be used to take away the associated complexity. 

  
[Back to contents](<FPC_internals.md> "FPC internals")

---

_Source: [https://wiki.freepascal.org/Generating_initialised_data](https://web.archive.org/web/20240226003350/https://wiki.freepascal.org/Generating_initialised_data)_
