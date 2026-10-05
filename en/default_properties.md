# default properties

Default properties allow "hoisting" or exposing of object members into the caller name space to facilitate wrapper types and (possibly) delegation patterns. They could be seen as "with" statements which encompass an entire structures namespace. 

This feature is under development and everything is subject to change. 

## Contents

  * 1 Download Development Branch
  * 2 Supports
  * 3 Hoisting Members
  * 4 Precedence Rules
  * 5 Passing
  * 6 Assignment Rules
  * 7 Multiple Defaults
  * 8 Hiding Fields
  * 9 Examples Usages
    * 9.1 Nullable types
    * 9.2 Auto managed classes using management operators
    * 9.3 Array indexing
    * 9.4 Delegation
    * 9.5 Default "Implements" Properties
  * 10 COMPILER: Implemention Details



## Download Development Branch

<https://github.com/genericptr/freepascal/tree/defaultprops>

## Supports

  * records, objects, classes.
  * arithmetic, compare, binary, unary, in overloads. (<https://www.freepascal.org/docs-html/ref/refch15.html>)
  * visibility sections.
  * array indexing with []
  * if, while, repeat, case, for..do statements



## Hoisting Members

By making "m_obj" a default property we can omit m_obj. to subscript into the members. This is like wrapping every instance of "m_obj" in a "with" statement. This makes it possible to bring members from other structures into the current structure without prefixing "m_obj." before accessing fields. 
    
    
     type
       THelper = record
         num: integer;
       end;
     
     type
       TWrapper = record
         m_obj: THelper;
         property helper: THelper read m_obj; default;
         procedure Inc;
       end;
     
     procedure TWrapper.Inc;
     begin
       num += 1;
     end;
     
     var
       wrapper: TWrapper;
     begin
       wrapper.num := 100;
       wrapper.Inc;
       writeln(wrapper.num);
     end.
    

## Precedence Rules

For default properties which are structures (objects, records, classes) it's possible to have method overloads in both the base structure and the default property (or unit scope even). For this reason overload resolution must follow a predicable pattern. 

Precedence order for overloads is as follows: base, default (last to first). 
    
    
     type
       THelper = class
         num: integer;
         procedure DoThis; overload;
         procedure DoThis (param: integer); overload;
         procedure DoThis (param: string); overload;
       end;
     
     procedure THelper.DoThis;
     begin
       writeln('THelper.DoThis:',num);
     end;
     
     procedure THelper.DoThis (param: integer);
     begin
       writeln('THelper.DoThis:',param,':',num);
     end;
     
     procedure THelper.DoThis (param: string);
     begin
       writeln('THelper.DoThis:',param,':',num);
     end;
     
     type
       TParent = class
         m_helper: THelper;
         property helper: THelper read m_helper; default;
         procedure DoThis (param:single);overload;
       end;
     
     procedure TParent.DoThis (param:single);
     begin
       writeln('TParent.DoThis:',param:1:1);
     end;
     
     type
       TMyObject = class (TParent)
         procedure Call;
         procedure DoThis;overload;
       end;
     
     procedure TMyObject.DoThis;
     begin
       writeln('TMyObject.DoThis');
     end;
     
     procedure TMyObject.Call;
     begin
       DoThis(100);      // THelper.DoThis
       DoThis(10.5);     // TParent.DoThis
       DoThis('hello');  // THelper.DoThis
       DoThis;           // TMyObject.DoThis
     end;
     
     var
       obj: TMyObject;
     begin
       obj := TMyObject.Create;
       obj.m_helper := THelper.Create;
       obj.Call;
     end.
    

Operator overloads are possible also. Precedence can get complicated considering it's possible to have overloads in the current unit for the default type as well as overloads in the structure itself. 
    
    
     operator + (l : integer; r : ansistring): integer;
     begin  
       result := l + StrToInt(r);  
     end;
     
     type
       TWrapper = record
         m_value: integer;
         property value: integer read m_value write m_value; default;
       end;
     
     var
       wrapper: TWrapper;
     begin
       wrapper += 1;               // default property takes precedence
       wrapper += '128';           // unit + operator takes precedence because default property type (integer) doesn't support string  overloads
       writeln(wrapper.value);     // 129
       if wrapper = 129 then       // = operator works also
         writeln('got 129'); 
     end.
    

Type helpers behave as normal on the default type. 
    
    
     type
       TIntegerHelper = type helper for integer
         function Str: string;
       end;
     
     function TIntegerHelper.Str: string;
     begin
       result := 'string->'+IntToStr(self);
     end;
     
     type
       TWrapper = record
         m_value: integer;
         property value: integer read m_value write m_value; default;
       end;
     
     var
       wrapper: TWrapper;
     begin
       wrapper := 100;
       writeln(wrapper.str);
     end.
    

## Passing

Passing a structure with default properties follows normal move semantics. For example, if there is a record with a default property of the type "integer" passing the record will not copy the integer value but rather the entire record will be passed. 
    
    
     type
       TIntRec = record
         m_int: integer;
         property value: integer read m_obj; default;
       end;
     
     procedure Print (int: TIntRec); overload;
     begin
       writeln(int.value);
     end;
     
     procedure Print (int: integer); overload;
     begin
       writeln(int);
     end;
    
     var
       int: TIntRec;
     begin
       int := 100;
       Print(int);    // Print(TIntRec) is called
       writeln(int)   // ERROR: can't write this type (TIntRec)
     end.
    

## Assignment Rules

Default properties introduce an ambiguity with assignments which is resolved by type. 
    
    
     type
       TWrapper = class
         m_value: integer;
         m_string: string;
         property value: integer read m_value write m_value; default;
       end;
     
     var
       wrapper: TWrapper;
     begin
       wrapper := TWrapper.Create;   // TWrapper is same type so assignment is as normal
       wrapper := 100;               // integer matches "value"
       writeln(wrapper.value);
     end.
    

## Multiple Defaults

Currently for development default properties are "multidimensional", meaning you can declare more than one per type and overloads will be resolved according to precedence. 

They have been developed this way from the outset because the nature of the search is inherently multidimensional, i.e base object, default property 1, default property 2 etc... so I wanted to keep the option available while the concept is being explored. 

This is of course a highly contested idea given the capacity to introduce difficult to understand namespace collisions. 

## Hiding Fields

If multiple defaults are allowed then it would be possible to hide fields from other default properties and cause some potential bugs. 
    
    
     type
       THelper = record
         data: integer;
       end;
     
     type
       TWrapper = record
         m_objA: THelper;
         m_objB: THelper;
         property helperA: THelper read m_objA; default;
         property helperB: THelper read m_objB; default;
       end;
     
     var
       wrapper: TWrapper;
     begin
       wrapper.data := 1;
       writeln('helperA:',wrapper.helperA.data); // 0
       writeln('helperB:',wrapper.helperB.data); // 1
     end.
    

## Examples Usages

### Nullable types
    
    
     type
      generic TNullable<T>= record
        private
          FValue : T;
        public
          isAssigned : boolean;
          function IsNull: boolean;
          procedure SetValue (newValue: T); 
          Property Value : T read FValue write SetValue; default;
          class operator Initialize(var a: TNullable);
        end;
     
     procedure TNullable.SetValue (newValue: T); 
     begin
       FValue := newValue;
       isAssigned := true;
     end;
     
     function TNullable.IsNull: boolean;
     begin
       result := not isAssigned;
     end;
     
     class operator TNullable.Initialize(var a: TNullable);
     begin
       a.isAssigned := Default(T);
     end;
     
     type
       TBoolean = specialize TNullable<boolean>;
     Var
       bool: TBoolean;
     begin
       bool := true;
       if bool then
         writeln('bool is true');
     end.
    

### Auto managed classes using management operators
    
    
     type
       generic TAuto<T> = record
         private
           m_object: T;
           class operator Initialize(var a: TAuto);
           class operator Finalize(var a: TAuto);
         public
           property obj: T read m_object; default;
       end;
     
     type
       TStringList = specialize TFPGList<String>;
       TStringListAuto = specialize TAuto<TStringList>;
     
     class operator TAuto.Initialize(var a: TAuto);
     begin
       a.m_object := T.Create;
     end;
     
     class operator TAuto.Finalize(var a: TAuto);
     begin
       a.m_object.Free;
     end;
     
     var
       list: TStringListAuto;
       str: string;
     begin
       list.Add('foo');
       list.Add('bar');
       for str in list do
         writeln(str);
     end.
    

### Array indexing

Because it is technically possible arrays are exposed using default properties. This however does conflict with the existing [] indexer properties so this may be a problem. 

As it stands however default properties on array types are an interesting method to make array wrappers. 
    
    
     type
       TIntArray = array of integer;
       TWrapper = record
         m_value: TIntArray;
         property value: TIntArray read m_value write m_value; default;
       end;
     
     var
       wrapper: TWrapper;
       i: integer;
     begin
       wrapper := TIntArray.Create(100,200,300);  // NOTE: we can't use array constructors [] due to bug in compiler
       wrapper[0] += 50;
       wrapper[1] += 1;
       for i in wrapper do
         writeln(i);
     end.
    

### Delegation

If delegate properties are allowed be multidimensional in the final version they can be used for delegation patterns that would otherwise only be possible with multiple inheritance (in languages such as C++, Java). 
    
    
      
     type
       TStats = record
         hp: integer;
         exp: integer;
         procedure InitStats;
       end;
     
     // Base Module:
     
     type
       TEntity = class;
       TBaseHandler = class
         entity: TEntity;
         constructor Create (inEntity: TEntity);
         procedure Clear;
       end;
     
     // Physics Module:
     
       TPhysicsHandler = class (TBaseHandler)
         pos: TVec3;
         acc: TVec3;
         vel: TVec3;
         procedure Integrate; virtual;
       end;
     
     // Renderer Module:
     
       TRendererHandler = class (TBaseHandler)
         procedure Render; virtual;
       end;
     
     // Entity:
     
       TEntity = class
         private
           m_physics: TPhysicsHandler;
           m_renderer: TRendererHandler;
           m_stats: TStats;
         public
           property physics: TPhysicsHandler read m_physics; default;
           property renderer: TRendererHandler read m_renderer; default;
           property stats: TStats read m_stats; default;
         public
           procedure Update;
       end;
     
     // Implemention:
     
     type
       TMonster = class (TEntity)
         public
           procedure AfterConstruction; override;
       end;
     
     type
       TMonsterRenderer = class (TRendererHandler)
         procedure Render; override;
       end;
     
     procedure TStats.InitStats;
     begin
       hp := 100;
       exp := 0;
     end;
     
     procedure TBaseHandler.Clear;
     begin
       writeln(classname,' clear');
     end;
     
     constructor TBaseHandler.Create (inEntity: TEntity);
     begin
       entity := inEntity;
     end;
     
     procedure TPhysicsHandler.Integrate;
     begin
       pos.x += vel.x;
       pos.y += vel.y;
       pos.z += vel.z;
     end;
     
     procedure TRendererHandler.Render;
     begin
     end;
     RSS
     procedure TEntity.Update;
     begin
       Integrate;
       Render;
     end;
     
     procedure TMonster.AfterConstruction;
     begin
       m_physics := TPhysicsHandler.Create(self);
       m_renderer := TMonsterRenderer.Create(self);
     
       InitStats;
     
       vel.x := 80;
       vel.y := 20;
       vel.z := 0;
     end;
     
     procedure TMonsterRenderer.Render;
     begin
       writeln('render monster at ', entity.pos.str, ' hp:', entity.hp);
     end;
     
     var
       entity: TMonster;
       i: integer;
     begin
       entity := TMonster.Create;
       entity.Clear;
       for i := 0 to 2 do
         entity.Update;
     end.
    

### Default "Implements" Properties

"implements" properties are more complete by exposing their namespace using "default". If multiple defaults are allowed one could imagine this being a possible method to implement multiple inheritance in Pascal. 
    
    
     type
       IWrapper = interface ['IWrapper']
         procedure DoThis;
       end;
     
     type
       TWrapper_Handler = class (IWrapper)
         procedure DoThis;
       end;
     
     procedure TWrapper_Handler.DoThis;
     begin
       writeln('TWrapper_Handler.DoThis');
     end;
     
     type
       TWrapper = class (IWrapper)
         m_wrapper: TWrapper_Handler;
         property handler: TWrapper_Handler read m_wrapper implements IWrapper; default;
         procedure AfterConstruction; override;
       end;
     
     procedure TWrapper.AfterConstruction;
     begin
       m_wrapper := TWrapper_Handler.Create;
     end;
     
     procedure HandleWrapper (wrapper: IWrapper);
     begin
       wrapper.DoThis;
     end;
     
     var
       wrapper: TWrapper;
     begin
       wrapper := TWrapper.Create;
       HandleWrapper(wrapper);
     end.
    

## COMPILER: Implemention Details

Default properties are implemented primarily at the parser layer and overloads (proc and operator) are handled through tcallcandiates (must be explictly requested in create params). They could be handled in the node layer during typechecking instead if that was deemed more appropriate. I used the parser layer because regular properties are handled this way and I wasn't sure about the ramifications of making nodes convert themselves to properties in all instances across the entire platform. Furthermore I personally see default properties as a sort of "meta-layer" which doesn't really exist but is more of a re-routing scheme which is applied in advance of normal syntax. 

For reference here are some of the important changes made to support default properties. 

  * symtable.pas



searchsym_xxx() functions now contain parameters which explicitly request defaults to be included in the result (see ssf_search_defaults) and return a tpropertysym. searchsym_with_defaults() is added for unit level searching. 

  * pexpr.pas


    
    
     do_member_read_internal() - handles the default property sym returned by searchsym_xxx using handle_read_property().
     do_proc_call() - after proc params are parsed default read property is handled via handle_default_proc_call().
     
    

Other relevant functions: 
    
    
     handle_default_assignment()
     handle_default_proc_call()
     handle_default_unary_operator()
     handle_default_binary_operator()
    

  * htypcheck.pas



Procdefs returned from tcallcandidates (if requested in constructor) can return procdefs from default properties. To help facilitate this tprocoverloadentry class was added (see collect_overloads_in_struct). 

The new overload for choose_best will return a tpropertysym (if the the best procdef belongs to a default property) so it can be handled properly. 

  * pstatmnt.pas



Expressions of type torddef are converted to properties using handle_expr_default_property_access(). 

  * nutils.pas



Various helper functions for default properties: 
    
    
     handle_read_property()
     handle_write_property()
     try_default_read_property()
    

  * nflw.pas



Uses try_default_read_property() to resolve default read properties for for..in enumerators. This is only possible for default properties that support the for..in loop, such as arrays, sets etc...

---

_Source: [https://wiki.freepascal.org/default_properties](https://web.archive.org/web/20250516143700/https://wiki.freepascal.org/default_properties)_
