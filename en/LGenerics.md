# LGenerics

## Contents

  * 1 About
  * 2 Download
  * 3 Features
    * 3.1 Implemented primitives
    * 3.2 Algorithms on graphs
    * 3.3 Algorithms on arrays and vectors
    * 3.4 Algorithms on strings and sequences
    * 3.5 Other features
  * 4 Support for JSON Patch
  * 5 Exporting structures to JSON format
    * 5.1 Serialization
    * 5.2 Deserialization
  * 6 Support for JSON Type Definition
    * 6.1 Why a schema for JSON?
    * 6.2 Why JSON Typedef?
    * 6.3 Validation
    * 6.4 Code generation from schemas
    * 6.5 Schema inference
  * 7 Support for JSONPath
    * 7.1 About JSONPath
    * 7.2 Syntax
    * 7.3 Implementation
  * 8 Performance
    * 8.1 Containers
      * 8.1.1 Performance of hash maps
    * 8.2 Algorithms on arrays/vectors
      * 8.2.1 Sorting
    * 8.3 Algorithms on graphs
    * 8.4 Algorithms on strings/sequences
    * 8.5 Performance of JSON operations
      * 8.5.1 Performance of export to JSON
      * 8.5.2 Performance of JSON Type Definition



## About

Collection of generic algorithms and data structures for Free Pascal. 

Requires: FPC 3.2.2+, Lazarus 2.2+. 

Author: A.Koverdyaev (avk) 

License: Apache License 2.0 

## Download

GitHub: <https://github.com/avk959/LGenerics>

## Features

#### Implemented primitives

  * stack (unit lgStack)
  * queue (unit lgQueue)
  * deque (unit lgDeque)
  * vector (unit lgVector)
  * vector of bits (unit lgVector)
  * priority queue based on binary heap (unit lgPriorityQueue)
  * priority queue with key update and melding based on pairing heap (unit lgPriorityQueue)
  * sorted list (unit lgList)
  * hashed list - array based list with the ability to fast search by key (unit lgList)
  * hashset (unit lgHashSet)
  * fine-grained concurrent hashset (unit lgHashSet)
  * sorted set (unit lgTreeSet)
  * set of arbitrary size (unit lgUtil, TGSet)
  * hash multiset (unit lgHashMultiSet)
  * fine-grained concurrent hashmultiset (unit lgHashMultiSet)
  * sorted multiset (unit lgTreeMultiSet)
  * hashmap (unit lgHashMap)
  * fine-grained concurrent hashmap (unit lgHashMap)
  * sorted map (unit lgTreeMap)
  * hash multimap (unit lgMultiMap)
  * tree multimap (unit lgMultiMap)
  * list miltimap (unit lgMultiMap)
  * bijective map (unit lgBiMap)
  * sparse 2D table (unit lgTable2D)
  * disjoint set (unit lgHashSet)
  * AVL tree (unit lgAvlTree)
  * red-black tree (unit lgRbTree)
  * some treap variants (unit lgTreap)
  * general rooted tree (unit lgRootTree)
  * sparse labeled undirected graph (unit lgSimpleGraph)
  * sparse labeled directed graph (unit lgSimpleDigraph)
  * lite containers based on advanced records
  * extended IEnumearble interface - filtering, mapping, etc.



#### Algorithms on graphs

  * core functions: 
    * vertices/edges addition/removal/query/enumeration, edge contraction, degree
    * load/save to own binary format, primitive export to DOT format
  * connectivity: 
    * connected/strongly connected components, bipartite detection, degeneracy, k-core
    * articulation points, bridges, biconnected components
    * edge-connectivity
  * traversals: 
    * BFS/DFS traversals with visitors,
    * cycle/negative cycle detection,
    * topological sort
  * operations: 
    * induced subgraphs, complement, reverse, union, intersect, symmetric difference,
  * chordality testing
  * planarity testing: FMR Left-Right Planarity algorithm
  * distance within graph: 
    * eccentricity, radius, diameter, center, periphery
  * matching: 
    * maximum cardinality matching on bipartite/arbitrary graphs
    * minimum/maximum weight matching on bipartite graphs
  * dominators in flowgraps: simple iterative and Semi-NCA algorithms
  * some suggestions for NP-hard problems: 
    * maximum independent set, maximal independent sets enumeration
    * maximum clique, cliques enumeration
    * minimum vertex cover, minimal vertex covers enumeration
    * vertex coloring, approximations and exact
    * minimum dominating set
    * Hamiltonian cycles and paths
    * local search TSP approximations, BnB TSP solver
  * minimum spanning trees: Prims's and Kruskal's algorithms
  * single source shortest paths: 
    * Dijkstra with pairing heap, Bellman-Ford-Moor with Tarjan's subtree disassembly(BFMT)
  * single pair shortest paths: 
    * Dijkstra with binary heap, BFMT, bidirection Dijkstra, A*, NBA*
  * all pairs shortest paths: 
    * Floyd–Warshall, Johnson, BFMT
  * networks: 
    * maximum flow: push/relabel, capacity scaling Dinitz
    * minimum-cost flow: Busacker-Gowen, cost scaling push/relabel algorithm
    * global minimum cut: Stoer–Wagner, Nagamochi-Ibaraki



#### Algorithms on arrays and vectors

(mostly unit lgArrayHelpers) 

  * reverse, right/left cyclic shifts
  * permutations
  * binary search
  * N-th order statistics
  * inversion counting
  * distinct values selection
  * introsort
  * mergesort
  * timsort (unit lgMiscUtils)
  * counting sort
  * radix sort
  * translation of Orson Peters' PDQSort algorithm
  * static segment tree
  * longest increasing subsequence
  * ...



#### Algorithms on strings and sequences

(units lgStrHelpers, lgSeqUtils) 

  * exact string matching 
    * Boyer-Moore string matching algorithm(in Fast Search variant, unit lgStrHelpers)
    * Boyer-Moore-Horspool-Raita algorithm (unit lgStrHelpers)
    * Aho-Corasick automation for multiple string matching
  * longest common subsequence of two sequences: 
    * reducing the LCS problem to LIS
    * Kumar-Rangan algorithm for LCS
    * Myers algorithm for LCS
  * the Levenshtein distance: 
    * simple DP algorithm
    * modified Berghel-Roach algorithm
    * Myers bit-vector algorithm with cut-off heuristic
  * restricted Damerau-Levenshtein distance: 
    * modified Berghel-Roach algorithm
  * LCS distance: 
    * Myers algorithm for LCS distance
  * fuzzy string matching 
    * k differences 
      * Ukkonen EDP algorithm
    * k mismatches 
      * bitap algorithm
  * fuzzy string matching with preprocessing(something similar to fuzzywuzzy)



#### Other features

  * non-cryptogarphic hashes (unit lgHash): 
    * Yann Collet's xxHash32, xxHash64
    * Austin Appleby's MurmurHash2, MurmurHash2A, MurmurHash3_x86_32, MurmurHash64A
  * brief and dirty implementation of futures concept (unit lgAsync)
  * brief channel implementation (unit lgAsync)
  * brief implementation of thread pool (unit lgAsync)
  * 128-bit integers (unit lgInt128)
  * JSON validator/parser/generator (unit lgJson)
  * JSON Patch/Diff (unit lgJson)
  * JSON serialization/deserialization of native Pascal data structures using RTTI(unit lgPdo)
  * JSON Type Definition schemas(unit lgJsonTypeDef) 
    * validating a JSON document against a JTD schema(unit lgJsonTypeDef)
    * generating Pascal code from a JTD schema(unit lgJtdCodegen)
    * inferring JTD schemas from example data(unit lgJtdInfer)
  * JSONPath implementation(unit LgJsonPath)
  * CSV document processing (unit lgCsvUtils)
  * Eisel-Lemire fast string-to-double conversion algorithm (unit lgJson)
  * Ryū double-to-string conversion algorithm (unit lgJson)



## Support for JSON Patch

The TJsonPatch class from the lgJson unit implements the JSON Patch ([RFC 6902](<https://datatracker.ietf.org/doc/html/rfc6902>)) functionality. JSON Patch is a JSON document format (media type is application/json-patch+json) for expressing a sequence of operations applied to a target JSON document; it is always an array of objects, each representing a single operation, supported set of operations: ADD, REMOVE, REPLACE, MOVE, COPY, and TEST. 

For example, for this document: 
    
    
        {
            "foo": [1, 3],
            "boo": ["bar", "baz"]
        }
    

applying this patch 
    
    
        [
            {"op": "add", "path": "/foo/1", "value": "fizz"},
            {"op": "replace", "path": "/boo/1", "value": "qux"},
            {"op": "add", "path": "/ozz", "value": "zoo"}
        ]
    

will have the following result: 
    
    
        {
            "foo": [1, "fizz", 3],
            "boo": ["bar", "qux"],
            "ozz": "zoo"
        }
    

The TJsonPatch class also contains a POC implementation of the Diff utility, which generates a patch by comparing source and target documents. 

For example, for this source: 
    
    
        {
            "a": 1,
            "b": null,
            "c": ["test", "plop"]
        }
    

and for this target: 
    
    
        {
            "a": 6,
            "c": ["test", "flop"],
            "d": "baz"
        }
    

it will generate a patch like this: 
    
    
        [
            {"op": "replace", "path": "/a", "value": 6},
            {"op": "remove", "path": "/b"},
            {"op": "replace", "path": "/c/1", "value": "flop"},
            {"op": "add", "path": "/d", "value": "baz"}
        ]
    

## Exporting structures to JSON format

A unit has been added to LGenerics for exporting Pascal data structures to JSON format and vice versa. The following types are currently supported (called PDOs for short): 

  * numeric, boolean or string types, some limited support of variants;
  * enumerations (exported in string form as the name of the corresponding constant);
  * sets (exported as an array of its elements);
  * regular records, it is possible to register a list of field names or a custom serialization callback;
  * classes, using published properties or by registering a custom callback (TStrings and TCollection as a special case);
  * objects (only by registering a custom serialization callback);
  * static arrays of PDO (only one-dimensional, multidimensional arrays are written as one-dimensional);
  * dynamic arrays of PDO;
  * variant arrays (currently only one-dimensional);



### Serialization

Function PdoToJson() saves native data structure to JSON format. 

For example, this code: 
    
    
    program test;
    
    {$mode objfpc}{$h+}{$optimization noorderfields}
    
    uses
      SysUtils, lgUtils, lgJson, lgPdo;
    
    type
      TDayOfWeek = 1..7;
      TDaySet = set of TDayOfWeek;
      TMember = record
        Name: string;
        Age: Integer;
        VisitDays: TDaySet;
      end;
    
    const
      Members: array of TMember = (
        (Name: 'Alice'; Age: 21; VisitDays: [1, 3, 5]),
        (Name: 'Bob'; Age: 25; VisitDays: [2, 5, 6]));
    var
      n: specialize TGAutoRef<TJsonNode>;
      s: string;
    
    begin
      RegisterRecordFields(TypeInfo(TMember), ['name', 'age', 'visitDays']);
      s := PdoToJson(TypeInfo(Members), Members);
      if not n.Instance.Parse(s) then
        WriteLn('Oops, something went wrong :(')
      else
        WriteLn(n.Instance.FormatJson([jfoEgyptBrace, jfoSingleLineArray]));
    end.
    

produces this JSON: 
    
    
    [
        {
            "name": "Alice",
            "age": 21,
            "visitDays": [1, 3, 5]
        },
        {
            "name": "Bob",
            "age": 25,
            "visitDays": [2, 5, 6]
        }
    ]
    

### Deserialization

Procedure PdoLoadJson() is used to load JSON back to the native data structure. 

Let's change the previous example a bit: 
    
    
    program test;
    
    {$mode objfpc}{$h+}{$optimization noorderfields}
    
    uses
      SysUtils, lgJson, lgPdo, lgMiscUtils;
    
    type
      TDayOfWeek = 1..7;
      TDaySet = set of TDayOfWeek;
      TMember = record
        Name: string;
        Age: Integer;
        VisitDays: TDaySet;
      end;
    
    const
      Members: array of TMember = (
        (Name: 'Alice'; Age: 21; VisitDays: [1, 3, 5]),
        (Name: 'Bob'; Age: 25; VisitDays: [2, 5, 6]));
    var
      s: string;
      MemberList: array of TMember = nil;
      I: Integer;
    begin
      RegisterRecordFields(TypeInfo(TMember), ['name', 'age', 'visitDays']);
      s := PdoToJson(TypeInfo(Members), Members);
      //Let's try to read the resulting JSON back into the MemberList array
      PdoLoadJson(TypeInfo(MemberList), MemberList, s);
      for I := 0 to High(MemberList) do begin
        WriteLn('Member ', I);
        WriteLn('  Name:      ', MemberList[I].Name);
        WriteLn('  Age:       ', MemberList[I].Age);
        WriteLn('  VisitDays: ', Stringify(TypeInfo(TDaySet), MemberList[I].VisitDays));
      end; 
    end.
    

it prints 
    
    
    Member 0
      Name:      Alice
      Age:       21
      VisitDays: {1, 3, 5}
    Member 1
      Name:      Bob
      Age:       25
      VisitDays: {2, 5, 6}
    

## Support for JSON Type Definition

[JSON Type Definition (aka RFC 8927)](<https://jsontypedef.com>) is an easy-to-learn, standardized way to define a schema for JSON data. 

##### Why a schema for JSON?

The schema provides a way to describe the structure and to some extent the semantics of the document, with which: 

  * it becomes possible to validate the document, i.e. to distinguish the "correct" document from the "incorrect" one;
  * a validated document can be safely deserialized into a native data structure;
  * and finally, it becomes possible to automatically generate this particular native data structure;



##### Why JSON Typedef?

It is simple, standardized, well documented, and there are a fair number of implementations for various programming languages(of course, Pascal is not among them). 

Strictly speaking, JSON Typedef is a subset of JSON Schema. According to the author, the expressiveness of the JTD was intentionally limited to be no more powerful than what can be expressed in the type systems of of major programming languages. 

In short, for most projects this may be exactly what you need. 

TL;DR 

There is not much point in describing the features of the JTD schemas, it is well explained in the documentation. 

##### Validation

Suppose we should to receive JSON messages like 
    
    
    {
        "firstName": "John",
        "lastName": "Doe",
        "age": 42
    }
    

such JSON can be described by the JTD scheme 
    
    
    {
        "properties": {
            "firstName": {"type": "string"},
            "lastName": {"type": "string"},
            "age": {"type": "uint8"}
        }
    }
    

And let us get the message 
    
    
    {
        "firstName": "Bill",
        "lastName": "Smith",
        "age": -5
    }
    

which we need to check for consistency with the scheme. 

First we need to load the JSON representation of the schema into the corresponding Pascal structure - TJtdSchema class. For example, let our schema be located in the file PersonSchema.json, code 
    
    
      Schema := TJtdSchema.Create;
      try
        Schema.LoadFromFile('PersonSchema.json')
      except
        //something to do in case of an exception
      end;
    

will load the schema and then it can be used repeatedly. In turn, the received JSON(let it be the string S) should be loaded into TJsonNode class, for example like this: 
    
    
      Person := TJsonNode.Create;
      Person.AsJson := S;
    

The Validate(yep!) function is used to validate against the schema; its main parameters are the loaded JSON sample, the loaded schema, and the (out) list of errors found. The error list is an array of InstancePath/SchemaPath pairs, which are JSON pointers. The first one points to the part of the document(instance) that is invalid, and the second one points to the part of the schema that rejected that input. The purpose of the other parameters is described in detail in the lgJsonTypeDef source. 

So this code 
    
    
    var
      ErrList: TJtdErrorList;
      e: TValidateError;
      ...
      case Validate(Person, Schema, ErrList) of
        jvrOk: {It's okay, the document is correct};
        jvrErrors:
          for e in ErrList do
            WriteLn('Error: instance path = ', e.InstancePath, ', schema path = ', e.SchemaPath);        
      else
        //dealing with the remaining cases
      end;
    

will print 
    
    
     
    Error: instance path = /age, schema path = /properties/age/type
    

##### Code generation from schemas

Because of RTTI's limited capabilities, it was decided to create a separate hierarchy of classes(see lgJtdTypes unit) to generate the Pascal code according to the given JTD scheme. 

This hierarchy would have to reflect the JSON Typedef type system and be able to load/save itself from/to different JSON representations. 

The part of the implementation responsible for code generation, specifically the TJtdPasCodegen class, can be found in the lgJtdCodegen unit. 

To generate a Pascal unit, the TJtdPasCodegen class needs the schema itself (mandatory), the name of the unit (desirable), the name of the root class (optional), and if the code is to be saved to a file, the name of the file. 

For example, suppose we have the following scheme: 
    
    
    {
        "definitions": {
            "fruits": {"enum": ["apple", "banana", "strawberry"],
                "metadata": {
                    "description": "this is fruits enumeration description",
                    "enumDescription": {
                        "apple": "this is apple description",
                        "banana": "this is banana description",
                        "strawberry": "this is strawberry description"
                    }
                }
            },
            "nameList": {"elements": {"type": "string"}}
        },
        "properties": {
            "firstName": {"type": "string"},
            "lastName": {"type": "string"},
            "age": {"type": "uint8"},
            "friends": {"ref": "nameList", "nullable": true},
            "favoriteFruit": {"ref": "fruits", "nullable": true}
        },
        "metadata": {
            "description": "this is Person description",
            "preferredName": "person"
        }
    }
    

and we have loaded it into a TJtdSchema instance named PersonSchema, code 
    
    
      with TJtdPasCodegen.Create(PersonSchema) do
        try
          UnitName := 'Person';
          SaveToFile('person.pas');
        finally
          Free;
        end;
    

will create a (nice-looking :)) unit in the application folder, whose root class will be able to load/save JSONs compliant to the schema. 
    
    
    {
      This unit was automatically created by JtdPasCodegen, do not edit.
    }
    unit Person;
    
    {$MODE OBJFPC}{$H+}{$B-}
    
    interface
    
    uses
      SysUtils, lgJson, lgJtdTypes;
    
    type
    
      TNameList = class sealed(specialize TJtdList<TJtdString>)
        class function GetJtdClass: TJtdEntityClass; override;
      end;
    
    { this is fruits enumeration description }
      TFruits = (
        apple, // this is apple description
        banana, // this is banana description
        strawberry // this is strawberry description
      );
    
    { Container for some TFruits enumeration element }
      TFruitsElem = class sealed(specialize TJtdEnum<TFruits>)
        class function GetJtdClass: TJtdEntityClass; override;
      end;
    
    { this is Person description }
      TPerson = class sealed(TJtdObject)
      private
        FFirstName: TJtdString;
        FLastName: TJtdString;
        FAge: TJtdUInt8;
        FFriends: TNameList;
        FFavoriteFruit: TFruitsElem;
        procedure SetFirstName(aValue: TJtdString);
        procedure SetLastName(aValue: TJtdString);
        procedure SetAge(aValue: TJtdUInt8);
        procedure SetFriends(aValue: TNameList);
        procedure SetFavoriteFruit(aValue: TFruitsElem);
      protected
        procedure DoReadJson(aNode: TJsonNode); override;
        procedure DoReadJson(aReader: TJsonReader); override;
        procedure DoWriteJson(aWriter: TJsonStrWriter); override;
      public
        class function GetJtdClass: TJtdEntityClass; override;
        procedure Clear; override;
      { refers to "firstName" JSON property }
        property FirstName: TJtdString read FFirstName write SetFirstName;
      { refers to "lastName" JSON property }
        property LastName: TJtdString read FLastName write SetLastName;
      { refers to "age" JSON property }
        property Age: TJtdUInt8 read FAge write SetAge;
      { refers to "friends" JSON property; is nullable }
        property Friends: TNameList read FFriends write SetFriends;
      { refers to "favoriteFruit" JSON property; is nullable }
        property FavoriteFruit: TFruitsElem read FFavoriteFruit write SetFavoriteFruit;
      end;
    
    implementation
    
    { TNameList }
    
    class function TNameList.GetJtdClass: TJtdEntityClass;
    begin
      Result := TNameList;
    end;
    
    { TFruitsElem }
    
    class function TFruitsElem.GetJtdClass: TJtdEntityClass;
    begin
      Result := TFruitsElem;
    end;
    
    { TPerson }
    
    class function TPerson.GetJtdClass: TJtdEntityClass;
    begin
      Result := TPerson;
    end;
    
    procedure TPerson.Clear;
    begin
      FreeAndNil(FFirstName);
      FreeAndNil(FLastName);
      FreeAndNil(FAge);
      FreeAndNil(FFriends);
      FreeAndNil(FFavoriteFruit);
    end;
    
    procedure TPerson.SetFirstName(aValue: TJtdString);
    begin
      if aValue = FFirstName then exit;
      FFirstName.Free;
      FFirstName := aValue;
    end;
    
    procedure TPerson.SetLastName(aValue: TJtdString);
    begin
      if aValue = FLastName then exit;
      FLastName.Free;
      FLastName := aValue;
    end;
    
    procedure TPerson.SetAge(aValue: TJtdUInt8);
    begin
      if aValue = FAge then exit;
      FAge.Free;
      FAge := aValue;
    end;
    
    procedure TPerson.SetFriends(aValue: TNameList);
    begin
      if aValue = FFriends then exit;
      FFriends.Free;
      FFriends := aValue;
    end;
    
    procedure TPerson.SetFavoriteFruit(aValue: TFruitsElem);
    begin
      if aValue = FFavoriteFruit then exit;
      FFavoriteFruit.Free;
      FFavoriteFruit := aValue;
    end;
    
    {$PUSH}{$WARN 5057 OFF}
    procedure TPerson.DoReadJson(aNode: TJsonNode);
    var
      p: TJsonNode.TPair;
      Flags: array[0..4] of Boolean;
      I: Integer;
    begin
      if not aNode.IsObject then ReadError;
      Clear;
      System.FillChar(Flags, SizeOf(Flags), 0);
      for p in aNode.Entries do
        case p.Key of
          'firstName':
            begin
              FFirstName := TJtdString(TJtdString.ReadJson(p.Value));
              Flags[0] := True;
            end;
          'lastName':
            begin
              FLastName := TJtdString(TJtdString.ReadJson(p.Value));
              Flags[1] := True;
            end;
          'age':
            begin
              FAge := TJtdUInt8(TJtdUInt8.ReadJson(p.Value));
              Flags[2] := True;
            end;
          'friends':
            begin
              FFriends := TNameList(TNameList.ReadJson(p.Value));
              Flags[3] := True;
            end;
          'favoriteFruit':
            begin
              FFavoriteFruit := TFruitsElem(TFruitsElem.ReadJson(p.Value));
              Flags[4] := True;
            end;
        else
          UnknownProp(p.Key);
        end;
      for I := 0 to System.High(Flags) do
        if not Flags[I] then
          case I of
            0: PropNotFound('firstName');
            1: PropNotFound('lastName');
            2: PropNotFound('age');
            3: PropNotFound('friends');
            4: PropNotFound('favoriteFruit');
          else
          end;
    end;
    {$POP}
    
    {$PUSH}{$WARN 5057 OFF}
    procedure TPerson.DoReadJson(aReader: TJsonReader);
    var
      Flags: array[0..4] of Boolean;
      I: Integer;
    begin
      if aReader.TokenKind <> tkObjectBegin then ReadError;
      Clear;
      System.FillChar(Flags, SizeOf(Flags), 0);
      repeat
        if not aReader.Read then ReadError;
        if aReader.TokenKind = tkObjectEnd then break;
        case aReader.Name of
          'firstName':
            begin
              FFirstName := TJtdString(TJtdString.ReadJson(aReader));
              Flags[0] := True;
            end;
          'lastName':
            begin
              FLastName := TJtdString(TJtdString.ReadJson(aReader));
              Flags[1] := True;
            end;
          'age':
            begin
              FAge := TJtdUInt8(TJtdUInt8.ReadJson(aReader));
              Flags[2] := True;
            end;
          'friends':
            begin
              FFriends := TNameList(TNameList.ReadJson(aReader));
              Flags[3] := True;
            end;
          'favoriteFruit':
            begin
              FFavoriteFruit := TFruitsElem(TFruitsElem.ReadJson(aReader));
              Flags[4] := True;
            end;
        else
          UnknownProp(aReader.Name);
        end;
      until False;
      for I := 0 to System.High(Flags) do
        if not Flags[I] then
          case I of
            0: PropNotFound('firstName');
            1: PropNotFound('lastName');
            2: PropNotFound('age');
            3: PropNotFound('friends');
            4: PropNotFound('favoriteFruit');
          else
          end;
    end;
    {$POP}
    
    procedure TPerson.DoWriteJson(aWriter: TJsonStrWriter);
    begin
      aWriter.BeginObject;
      aWriter.AddName('firstName');
      FirstName.WriteJson(aWriter);
      aWriter.AddName('lastName');
      LastName.WriteJson(aWriter);
      aWriter.AddName('age');
      Age.WriteJson(aWriter);
      aWriter.AddName('friends');
      Friends.WriteJson(aWriter);
      aWriter.AddName('favoriteFruit');
      FavoriteFruit.WriteJson(aWriter);
      aWriter.EndObject;
    end;
    
    end.
    

##### Schema inference

Also added support for inferring JTD schemas from JSON samples. 

At least the examples from [this article](<https://jsontypedef.com/docs/jtd-infer/>) run as expected. 

For example, this simple program: 
    
    
    program infer_test;
    
    {$mode objfpc}{$H+}
    
    uses
      SysUtils, lgUtils, lgJson, lgJtdInfer;
    
    var
      Schema: specialize TGUniqRef<TJsonNode>;
    
    const
      Lincoln = '{"name":"Abraham Lincoln","isAdmin": true}';
      Sherman = '{"name":"William Sherman","isAdmin": false,"middleName": "Tecumseh"}';
    
    begin
      Schema.Instance := TJtdInferrer.Infer([Lincoln, Sherman], []);
      WriteLn(Schema.Instance.FormatJson(DefaultJsonFmtStyle));
    end.
    

generates such a schema: 
    
    
    {
        "properties": {
            "name": {"type": "string"},
            "isAdmin": {"type": "boolean"}
        },
        "optionalProperties": {
            "middleName": {"type": "string"}
        }
    }
    

The /example/json/jtd folder contains a small schema editor to demonstrate the functionality of JSON Typedef. 

## Support for JSONPath

#### About JSONPath

    JSONPath enables you to search for particular data in a JSON document using a simple query syntax somewhat similar to XPath.

It is based on Stefan Gessner's popular proposal( [2007-02-21](<https://goessner.net/articles/JsonPath/>)), and now finally achieved Proposed Standard status: [RFC 9535](<https://www.rfc-editor.org/rfc/rfc9535>) . 

A JSONPath query might look like this: 
    
    
    $.store.book[?@.price < 10].title
    

or 
    
    
    $['store']['book'][?@['price'] < 10]['title']
    

or 
    
    
    $["store"]["book"][?@["price"] < 10]["title"]
    

These are just different ways of writing a query to select book titles which price is less than 10. 

#### Syntax

The implementation of JSONPath in LGenerics tries to follow the syntax described in the [RFC 9535](<https://www.rfc-editor.org/rfc/rfc9535>). 

Any query starts with $ sign followed by some (possibly zero) number of segments, leading and trailing spaces are not allowed. 

The whole process can be depicted as a pipe, where each step selects some number of child nodes ( possibly zero), which are then passed to the next step. The result of this is a list of Nodes, Nodelist in terms of specifications. 

It is worth noting that in specs terminology, a JSONPath Node represents a pair, the JSON value along with its location within the query argument. 

Basic syntax elements  Element | Descrition   
---|---  
$ | root node identifier (query argument)   
@ | current node identifier   
[<selectors>] | child segment: selects zero or more children of a node; contains one or more selectors, separated by commas   
.name | shorthand for ['name'] or ["name"]   
.* | shorthand for [*]   
..[<selectors>] | descendant segment: selects zero or more descendants of a node; contains one or more selectors, separated by commas   
..name | shorthand for ..['name']   
..* | shorthand for ..[*]   
'name' | name selector: selects a named child of an object   
* | wildcard selector: selects all children of a node   
7 | index selector: selects an indexed child of an array (0-based)   
0:42:5 | array slice selector: start:end:step for arrays   
?<logical-expr> | filter selector: selects particular children using a logical expression   
length(@.foo) | function extension: invokes a function in a filter expression   
  
Index and slice selectors have certain peculiarities: 

    

  * `$.book[1]` matches the second book
  * `$.book[-1]` matches the last book
  * `$.book[2:]` matches the third and all following books
  * `$.book[-2:]` matches the last two books
  * `$.book[:2]` matches the first two books
  * `$.book[::]` matches all books
  * `$.book[::-1]` matches all books in reverse order



The filter selector is a logical expression containing logical operators, comparison operators, and function calls. 

Filter expressions operator precedence  Precedence | Operator type | Syntax   
---|---|---  
5 | Grouping | ( ... )   
4 | Logical NOT | !   
3 | Relations | ==, !=, >, <, >=, <=   
2 | Logical AND | &&  
1 | Logical OR |  ||   
  
It is also highly recommended to implement 5 functions whose syntax and semantics are predefined in the specifications: 

  * `length()`
  * `count()`
  * `match()`
  * `search()`
  * `value()`



The match() and search() functions use a special flavor of regular expression (I-Regexp, [RFC 9485](<https://www.rfc-editor.org/rfc/rfc9485>)) for interoperability purposes. 

#### Implementation

In LGenerics, the JSONPath implementation can be found in the LgJsonPath unit. 

JSONPath functionality is represented by the IJsonPath COM interface, which currently (end of February 2024) contains 12 basic matching methods for different types of input and output data and an additional Params[] property for setting parameters in parametric queries. 

To get a reference to the IJsonPath interface, one can use an overloaded version of the JpParseQuery() function with the required set of parameters. 

In addition to the built-in functions defined in the specification, some extra functions of greater or lesser utility have been added; a list and description can be found in the unit header. 

It is also possible to register user-defined functions, which, however, requires some caution. 

The specs seems to be realized in its entirety and the implementation is successfully passing the current compliance tests. 

A small example, JSON document taken from the specifications: 
    
    
    program test_jp;
    {$mode objfpc}{$h+}
    uses
      SysUtils, LgUtils, LgJson, lgJsonPath;
    
    const
      Json: string =
        '{ "store": {                              ' +
        '    "book": [                             ' +
        '      { "category": "reference",          ' +
        '        "author": "Nigel Rees",           ' +
        '        "title": "Sayings of the Century",' +
        '        "price": 8.95                     ' +
        '      },                                  ' +
        '      { "category": "fiction",            ' +
        '        "author": "Evelyn Waugh",         ' +
        '        "title": "Sword of Honour",       ' +
        '        "price": 12.99                    ' +
        '      },                                  ' +
        '      { "category": "fiction",            ' +
        '        "author": "Herman Melville",      ' +
        '        "title": "Moby Dick",             ' +
        '        "isbn": "0-553-21311-3",          ' +
        '        "price": 8.99                     ' +
        '      },                                  ' +
        '      { "category": "fiction",            ' +
        '        "author": "J. R. R. Tolkien",     ' +
        '        "title": "The Lord of the Rings", ' +
        '        "isbn": "0-395-19395-8",          ' +
        '        "price": 22.99                    ' +
        '      }                                  ' +
        '    ],                                    ' +
        '    "bicycle": {                          ' +
        '      "color": "red",                     ' +
        '      "price": 399                        ' +
        '    }                                     ' +
        '  }                                       ' +
        '}                                         ';
    
    var
      Doc, CurrNode: specialize TGAutoRef<TJsonNode>;
      Path: IJsonPath;
      List: TJpNodeList;
      Node: TJpNode;
    begin
      if not Doc.Instance.Parse(Json) then begin
        WriteLn('Oops, failed to parse document');
        Halt;
      end;
    
      if JpParseQuery('$..author', Path) then begin
        WriteLn('All authors: ');
        List := Path.Match(Doc.Instance);
        for Node in List do begin
          CurrNode.Instance.Parse(Node.AsJson);
          WriteLn(CurrNode.Instance.FormatJson(DefaultJsonFmtStyle, 2));
        end;
        WriteLn;
      end;
    
      if JpParseQuery('$..[?@.isbn]', Path) then begin
        WriteLn('Anything that has an ISBN:');
        List := Path.Match(Doc.Instance);
        for Node in List do begin
          CurrNode.Instance.Parse(Node.AsJson);
          WriteLn(CurrNode.Instance.FormatJson(DefaultJsonFmtStyle, 2));
        end;
        WriteLn;
      end;
    
      if JpParseQuery('$..[?@.color == "red"]', Path) then begin
        WriteLn('Anything that is red: ');
        List := Path.Match(Doc.Instance);
        for Node in List do begin
          CurrNode.Instance.Parse(Node.AsJson);
          WriteLn(CurrNode.Instance.FormatJson(DefaultJsonFmtStyle, 2));
        end;
        WriteLn;
      end;
    
      if JpParseQuery('$..[?search(@.title, "[Hh]ono")]', Path) then begin
        WriteLn('Anything that contains "Hono" or "hono" in the title: ');
        List := Path.Match(Doc.Instance);
        for Node in List do begin
          CurrNode.Instance.Parse(Node.AsJson);
          WriteLn(CurrNode.Instance.FormatJson(DefaultJsonFmtStyle, 2));
        end;
      end;
    end.
    

It prints: 
    
    
    All authors:
      {"location": "$['store']['book'][0]['author']", "value": "Nigel Rees"}
      {"location": "$['store']['book'][1]['author']", "value": "Evelyn Waugh"}
      {"location": "$['store']['book'][2]['author']", "value": "Herman Melville"}
      {"location": "$['store']['book'][3]['author']", "value": "J. R. R. Tolkien"}
    
    Anything that has an ISBN:
      {
          "location": "$['store']['book'][2]",
          "value": {
              "category": "fiction",
              "author": "Herman Melville",
              "title": "Moby Dick",
              "isbn": "0-553-21311-3",
              "price": 8.99
          }
      }
      {
          "location": "$['store']['book'][3]",
          "value": {
              "category": "fiction",
              "author": "J. R. R. Tolkien",
              "title": "The Lord of the Rings",
              "isbn": "0-395-19395-8",
              "price": 22.99
          }
      }
    
    Anything that is red:
      {
          "location": "$['store']['bicycle']",
          "value": {"color": "red", "price": 399}
      }
    
    Anything that contains "Hono" or "hono" in the title:
      {
          "location": "$['store']['book'][1]",
          "value": {
              "category": "fiction",
              "author": "Evelyn Waugh",
              "title": "Sword of Honour",
              "price": 12.99
          }
      }
    
    

## Performance

### Containers

#### Performance of hash maps

There is a [wonderful benchmark](<https://www.benibela.de/fpc-map-benchmark_en.html>) from _BeniBela_ , which also covers hashmaps from LGenerics. 

### Algorithms on arrays/vectors

#### Sorting

(This section is outdated.) 

The benchmark sorts 1000000 integers and calculates the number of CPU cycles spent on one element and involves: 

  * QuickSort_ItemList_Context from unit SortBase (Rtl/SortBase)
  * TOrderingArrayUtils.Sort from Fcl-Stl.GArrayUtils (Fcl-Stl)
  * TArrayHelper.Sort from Rtl-Generics.Generics.Collections (Rtl-Generics)
  * std::sort from libstdc++ (std::sort)
  * std::stable_sort from libstdc++ (std::stable_sort)
  * TGOrdinalArrayHelper.QuickSort (LG/QuickSort)
  * TGOrdinalArrayHelper.IntroSort (LG/IntroSort)
  * TGOrdinalArrayHelper.DualPivotQuickSort (LG/DualPivotQuickSort)
  * TGOrdinalArrayHelper.PDQSort (LG/PDQSort)
  * TGComparableArrayHelper.MergeSort (LG/MergeSort)
  * TGComparableTimSort.Sort (LG/TimSort)
  * TGOrdinalArrayHelper.Sort (LG/Sort)
  * A negative value indicates that the algorithm exhibits quadratic behavior.



[![sort bench 1000000.png](https://wiki.freepascal.org/images/a/a2/sort_bench_1000000.png)](</File:sort_bench_1000000.png>)

### Algorithms on graphs

For performance comparison the AGraph library (taken from [here](<https://github.com/zamtmn/agraphlaz>)) was chosen as a reference implementation. All tests were compiled for x86 and run on win64 machine (unfortunately the 64-bit version of AGraph crashes on these tests). 

So, AGraph vs LGenerics: 

problem | graph | AGraph time(ms) | LGenerics time(ms) | notes   
---|---|---|---|---  
BFS traversal | undirected, V=500000, E=4000000 | 640 | 109 |   
BFS traversal | directed, V=500000, E=4000000 | 484 | 94 |   
Connected components | undirected, V=500000, E=4000000 | 764 | 0 | If no edges were removed, TGSimpleGraph always knows the number and ownership of connected components   
Strongly connected components | directed, V=500000, E=4000000 | 1060 | 265 |   
Single-source shortest path | undirected, V=500000, E=4000000 | 3011 | 842 |   
Single-source shortest path | directed, V=500000, E=4000000 | 2153 | 718 |   
Single-pair shortest path | undirected, V=500000, E=4000000 | 2777 | 31 |   
Single-pair shortest path | directed, V=500000, E=4000000 | 1825 | 47 |   
Minimum spanning tree | undirected, V=500000, E=4000000 | 5974 | 640 |   
Maximum flow | dense, V=1000, E=499500 | 29499 | 47 |   
Maximum flow | sparse, V=64000, E=512000 | 3339 | 63 |   
Planarity test | undirected, V=219086, E=657252 | 1419 | 172 |   
Maximum clique | DIMACS brock200_2.clq, V=200, E=9876 | 84210 | 16 |   
Maximum clique | DIMACS brock200_4.clq, V=200, E=13089 | Cancelled(timeout 1 h) | 312 |   
  
### Algorithms on strings/sequences

It was curious to compare the performance of the SimRatioLevEx() function (which is inspired by FuzzyWuzzy) with some incarnations of the FuzzyWuzzy ([listed here](<https://pypi.org/project/fuzzywuzzy/>)) on benchmark datasets. Disclamer: SimRatioLevEx() does not reproduce FuzzyWuzzy, but it does some things in a similar way, in particular, SimRatioLevEx() in smTokenSetEx mode and token_set_ratio() do roughly the same job. 

It seems the C++ version only supports single-byte strings, so only compared to the single-byte version of SimRatioLevEx(): 
    
    
           Dataset            SimRatioLevEx() token_set_ratio()
         
        Short/big_dist             1154              6440
        Short/small_dist            967              3020
        Medium/big_dist             811              3450
        Medium/small_dist           702              1470
        Long/big_dist              1966             15000
        Long/small_dist            1061              2250
    

The numbers indicate the run time in milliseconds; the C++ version was compiled with gcc-8.1.0 with options -O3 -m64; the Pascal version was compiled with FPC-3.3.1-9941-g8e6e9bbf33, -O3. The benchmark was run on a Windows x64 machine. 

The Go version, on the contrary, always works with Unicode strings: 
    
    
           Dataset          SimRatioLevExUtf8() TokenSetRatio()
         
        Short/big_dist             2143             18705
        Short/small_dist           1593              2224
        Medium/big_dist            1266             15062
        Medium/small_dist           853              1742
        Long/big_dist              3853             79851
        Long/small_dist            1269              3126
    

Go version: go1.10.4 linux/amd64; FPC-3.3.1-10683-g2a19e152b7 -O3. The benchmark was run on a virtual Linux machine. 

### Performance of JSON operations

A small performance comparison of the lgJson. TJsonNode class with some other implementations, the benchmark was compiled for x86_64 and run on a win64 machine. 

description | FpJson | JsonTools | lgJson | notes   
---|---|---|---|---  
Average parsing speed on a set of JSON samples from the example/json/parse_bench folder, MB/s | 34,45 | 46,11 | 68,12 |   
Average validating speed on a set of JSON samples from the example/json/parse_bench folder, MB/s | 34,4 | 46,02 | 349,48 | FpJson does not provide any function to validate JSON, so a parser was used   
Recursive traversal (using the built-in enumerator) of the document of moderate size (34 KB) with all values read (50000 repeats), milliseconds | 8315 | 6802 | 639 |   
Search in a moderately sized document using path (750000 repeats), milliseconds | 8549 | 5398 | 873 |   
Dynamic creation of a small document and reading it as formatted JSON (150000 repeats), milliseconds | 7597 | 4150 | 1903 |   
Saving a moderate size document to a stream (10000 repeats), milliseconds | 4820 | 4587 | 827 |   
RFC 8259 compliance test, 295 test files from test/json_testset/testset | 5 failed | 23 failed | all passed |   
  
#### Performance of export to JSON

Datasets used: a collection of simple items (with three string properties and one integer property) and an array of records whose fields contain values that coincide with the values of the collection's item properties (i.e. the resulting JSONs should be the same). The number of elements in the datasets was chosen so that the size of the resulting JSON approximately corresponded to the values (20KB, 100KB, 1MB, 10MB, 50MB). 

TJSONStreamer.CollectionToJSON() was taken as a reference implementation (FpJsonStreamer for short), other members: 

  * PdoToJson() with a collection as parameter (PdoCollection);
  * PdoToJson() with array as parameter and field name list registration (PdoRecordFieldMap);
  * PdoToJson() with array as parameter and custom callback registration (PdoRecordCallback);



Each member exports the data several times (number of attempts is pre-defined), the best time is taken as the result. 

The full benchmark code can be found in the folder LGenerics/example/json/json_export_bench. 

Results on author's machine: 
    
    
    -------<JSON size 20 KB>-------
      FpJsonStreamer......1,045 ms.
      Rating:             22,23 MB/s.
      PdoCollection.......0,123 ms.
      Rating:             164,36 MB/s.
      PdoRecordFieldMap...0,089 ms.
      Rating:             226,76 MB/s.
      PdoRecordCallback...0,057 ms.
      Rating:             356,65 MB/s.
    -------<JSON size 100 KB>-------
      FpJsonStreamer......5,647 ms.
      Rating:             20,51 MB/s.
      PdoCollection.......0,626 ms.
      Rating:             160,7 MB/s.
      PdoRecordFieldMap...0,443 ms.
      Rating:             226,72 MB/s.
      PdoRecordCallback...0,285 ms.
      Rating:             352,29 MB/s.
    -------<JSON size 1 MB>-------
      FpJsonStreamer......80,298 ms.
      Rating:             14,34 MB/s.
      PdoCollection.......6,552 ms.
      Rating:             152,63 MB/s.
      PdoRecordFieldMap...4,701 ms.
      Rating:             212,71 MB/s.
      PdoRecordCallback...3,107 ms.
      Rating:             321,85 MB/s.
    -------<JSON size 10 MB>-------
      FpJsonStreamer......3554,483 ms.
      Rating:             3,24 MB/s.
      PdoCollection.......71,202 ms.
      Rating:             140,57 MB/s.
      PdoRecordFieldMap...53,309 ms.
      Rating:             187,75 MB/s.
      PdoRecordCallback...37,39 ms.
      Rating:             267,69 MB/s.
    -------<JSON size 50 MB>-------
      FpJsonStreamer......84343,47 ms.
      Rating:             0,68 MB/s.
      PdoCollection.......350,224 ms.
      Rating:             142,99 MB/s.
      PdoRecordFieldMap...260,963 ms.
      Rating:             191,89 MB/s.
      PdoRecordCallback...183,328 ms.
      Rating:             273,16 MB/s.
    

#### Performance of JSON Type Definition

Test schema: 
    
    
    {
        "definitions": {
           "point": {
              "properties": {
                 "x": {"type": "int32"},
                 "y": {"type": "int32"}
              }
           },
           "shape": {"enum": ["circle", "ellipce", "rectangle", "rhombus", "triangle"]},
           "figure": {
               "properties": {
                  "name": {"type": "string"},
                  "shape": {"ref": "shape"},
                  "area": {"type": "float64"},
                  "center": {"ref": "point"}
               }
           }
        },
        "elements": {"ref": "figure"}
    }
    

The TJSONDeStreamer loading a collection of figures was taken as a reference implementation. To create more or less extreme conditions, a test JSON of size 62267256 bytes was generated, JTD class(unit Figures) was generated using TJtdPasCodegen. 

Full source code of the test application: 
    
    
    program jtd_test;
    
    {$mode objfpc}{$H+}
    
    uses
      Classes, SysUtils, fpJsonRtti, lgJson, lgJsonTypeDef, Figures;
    
    type
    {$PUSH}{$M+}
      TMyPoint = class
      private
        FX,
        FY: Integer;
      published
        property x: Integer read FX write FX;
        property y: Integer read FY write FY;
      end;
    {$POP}
    
      TMyFigure = class(TCollectionItem)
      private
        FArea: Double;
        FCenter: TMyPoint;
        FName: string;
        FShape: TShape;
      public
        constructor Create(ACollection: TCollection); override;
        destructor Destroy; override;
      published
        property name: string read FName write FName;
        property shape: TShape read FShape write FShape;
        property area: Double read FArea write FArea;
        property center: TMyPoint read FCenter;
      end;
    
    constructor TMyFigure.Create(ACollection: TCollection);
    begin
      inherited Create(ACollection);
      FCenter := TMyPoint.Create;
    end;
    
    destructor TMyFigure.Destroy;
    begin
      FCenter.Free;
      inherited;
    end;
    
    var
      s: string;
    
    procedure TestCollection;
    var
      c: TCollection;
      Start, Stop: QWord;
    begin
      c := TCollection.Create(TMyFigure);
      Start := GetTickCount64;
      with TJSONDeStreamer.Create(nil) do
      try
        JSONToCollection(s, c);
      finally
        Free;
      end;
      Stop := GetTickCount64;
      c.Free;
      WriteLn('TJSONDeStreamer.JSONToCollection:   ', Stop - Start);
    end;
    
    procedure TestJtd;
    var
      Node: TJsonNode;
      Schema: TJtdSchema;
      eList: TJtdErrorList;
      Figures: TFigureList;
      Start, Stop: QWord;
    begin
      Start := GetTickCount64;
      if not TJsonNode.TryParse(s, Node) then begin
        WriteLn('Failed to load data');
        exit;
      end;
      Stop := GetTickCount64;
      WriteLn('Loading data into TJsonNode:        ', Stop - Start);
    
      if not TJtdSchema.TryLoadFromFile('schema.json', Schema) then begin
        WriteLn('Failed to load schema');
        exit;
      end;
    
      Start := GetTickCount64;
      if Validate(Node, Schema, eList) <> jvrOk then begin
        WriteLn('Failed to validate data');
        exit;
      end;
      Stop := GetTickCount64;
      Schema.Free;
      WriteLn('TJsonNode data validation:          ', Stop - Start);
    
      Start := GetTickCount64;
      Figures := TFigureList.ReadJson(Node) as TFigureList;
      Stop := GetTickCount64;
      Figures.Free;
      Node.Free;
      WriteLn('Loadind JTD class from TJsonNode:   ', Stop - Start);
    
      Start := GetTickCount64;
      Figures := TFigureList.ReadJson(s) as TFigureList;
      Stop := GetTickCount64;
      Figures.Free;
      WriteLn('Direct loading of JTD class:        ', Stop - Start);
    end;
    
    procedure LoadData;
    begin
      with TStringStream.Create do
        try
          LoadFromFile('data.json');
          s := DataString;
        finally
          Free;
        end;
    end;
    
    begin
      LoadData;
    
      TestCollection;
      TestJtd;
    
    end.
    

On my machine, it prints results like this: 
    
    
    TJSONDeStreamer.JSONToCollection:   5304
    Loading data into TJsonNode:        1513
    TJsonNode data validation:          1732
    Loadind JTD class from TJsonNode:   515
    Direct loading of JTD class:        1560
    

Conclusion: the total validation and loading time of the JTD class is still noticeably better than the TCollection load time (3760 vs 5304). 

The direct load time of the JTD class is better than the load time of the TCollection by more than three times.

---

_Source: [https://wiki.freepascal.org/LGenerics](https://web.archive.org/web/20240920204139/https://wiki.freepascal.org/LGenerics)_
