# FPC Cleanroom

In 2007 it was found that some functions could need a new implementation because they were too similar to the implementation from Delphi. FPC 2.2.2 will be the first release after the cleanroom implementation of those routines. 

The core FPC developers already know the old implementation, so impartial contributiors not familiar with the old code or the Delphi code need to be found. The functions alse need to be tested and specifications for them written. 

## Contents

  * 1 Volunteers to implement the functions
  * 2 Volunteers to write specs and test apps
  * 3 Downloading the cleanroom branch
  * 4 Affected functions
    * 4.1 TDataset.CalculateFields
    * 4.2 TDataset.DataEvent
      * 4.2.1 Specifications
    * 4.3 TDataset.EnableControls
    * 4.4 TDataLink.CalcFirstRecord
      * 4.4.1 Specifications
    * 4.5 TField.CalcLookupValue
    * 4.6 TField.RefreshLookupList
      * 4.6.1 Specifications
  * 5 TODO for the RTL
  * 6 Variant streaming implementation
    * 6.1 General considerations
      * 6.1.1 Storing property of type Variant
      * 6.1.2 Reading property of type Variant
  * 7 Bug reports



## Volunteers to implement the functions

  * [Vincent](</User:Vincent> "User:Vincent")
  * Almindor 
  *   * 


## Volunteers to write specs and test apps

  * [Sekelsenmat](</index.php?title=Sekelsenmat&action=edit&redlink=1> "Sekelsenmat \(page does not exist\)")
  * Joost 



## Downloading the cleanroom branch

One can download it using subversion with: 
    
    
    svn co <http://svn.freepascal.org/svn/fpc/branches/cleanroom> cleanroom
    

Or use viewvc to view it: <http://svn.freepascal.org/cgi-bin/viewvc.cgi/branches/cleanroom/>

## Affected functions

The affected functions are marked with "tainted" on the branch. 

### TDataset.CalculateFields

**location**
    
    
    cleanroom/packages/fpc-db/src/base/dataset.inc
    

**declaration**
    
    
    (private) Procedure TDataset.CalculateFields(Buffer: PChar);
    

**Implementor: Almindor (untested)**

**Specifications**

Basicly this procedure sets the CalcBuffer, clears it and calculates the lookupvalues for all lookup fields and then calls DoOnCalcFields. 

So first it assigns the supplied Buffer to FCalcBuffer. Then if the state of the dataset is not dsInternalCalc and IsUniDirectional is False, ClearCalcFields is called to clear the CalcBuffer and for each field in the Fields collection of which the FieldKind is fkLookup, CalcLookupValue is called. 

Finally DoOnCalcFields is called. 

### TDataset.DataEvent

**location**
    
    
    cleanroom/packages/fpc-db/src/base/dataset.inc
    

**declaration**
    
    
    (private) Procedure TDataset.DataEvent(Event: TDataEvent; Info: Ptrint);
    

**Impementor** : [Vincent](</User:Vincent> "User:Vincent")

#### Specifications

Handles an event 

When the event is equal to deFieldChange then the provided 'Info' parameter is a TField. If it's fieldkind is fkData or fkInternalCalc then SetModified is called to set Modified to 'true'. 

If the event is deFieldChange and the state of the dataset is not dssetkey then there are first two checks: if FInternalCalcFields and info's FieldKind is equal to fkData, then RefreshInternalCalcfields is called with ActiveBuffer as parameter. Second check is that if the first check is not true but the info's fieldkind is equal to fkData, FAutoCalcFields is true and FCalcFieldsSize is not equal to 0, then CalculateFields is called for the ActiveBuffer. After those two checks, Info.Change is always called. 

If the event is equal to deDatasetChange or deDatasetScroll and TDataset.State is not dsInsert then UpdateCursorPos is called. 

Thereafter, if ControlsDisabled is false, the event is distributed to all datasources linked to this dataset by calling the datasource's ProcessEvent with 'Event' and 'Info' as parameters. 

### TDataset.EnableControls

**location**
    
    
    cleanroom/packages/fpc-db/src/base/dataset.inc
    

**declaration**
    
    
    (public) Procedure TDataset.EnableControls;
    

**Implementor: Almindor (untested)**

**Specifications**

Reactivates the displaying of data on screen after a call to DisableControls. 

EnableControls should decrement the disabled count on the dataset, up to a minimum of zero. If it's zero, it will tell the controls using the dataset they can show their values on screen. 

In detail: If FDisableControlsCount is larger then zero then it's value is decremented by 1. If thereafter FDisabledControlsCount is equal to zero then there is a check if the state of the dataset is changed since the call to DisableControls. This is done by comparing the current state with FDisableControlsState. If the state is changed, a deUpdateState event is fired, using TDataset.DataEvent. If the current state and the FDisableControlState are both unequal to dsInactive, then an FEnablecontrolsEvent is fired. 

### TDataLink.CalcFirstRecord

**location**
    
    
    cleanroom/packages/fpc-db/src/base/datasource.inc
    

**declaration**
    
    
    (private) Function TDataLink.CalcFirstRecord(Index : Integer) : Integer;
    

**Implementor: Almindor (untested)**

#### Specifications

This procedure returns the number of records the buffer should be scrolled if the current cursor moves 'index' places. And it sets FFirstRecord to it's new place, taking the shift of the buffer into account. 

This function compare DataSource.Dataset.FActiveRecord with FFirstRecord +Index+FBuffercount-1. If the former is larger then the latter, then the result of the function is set to the difference between the two. Else if DataSource.Dataset.FActiveRecord is smaller then FFirstRecord +Index then the result is set to the difference between those two values. (The result is negative) If both evaluations are false, no shift of the buffer is necessary and the result is set to 0. 

Finally the FFirstRecord is incremented by Index and the Result is added to it. 

### TField.CalcLookupValue

**location**
    
    
    cleanroom/packages/fpc-db/src/base/fields.inc
    

**declaration**
    
    
    (private) procedure TField.CalcLookupValue;
    

**Implementor: Almindor (untested)**

**Specifications**

It sets 'value' to to the lookupvalue corresponding to the fieldvalues of the keyfields. 

If FLookupCache is true, then Value is set to the value of the key in 'LookupList' which corresponds to the 'FieldValues' of 'FKeyFields' of 'FDataset'. Or else if the FLookupDataSet is not nil and this dataset is active then then the value of 'Value' is determined using FLookupDataset.Lookup. The keyfields are the 'FLookupKeyFields', the keyvalues the FieldValues of the FDataset for the FKeyFields again, and the resultfield is the FLookupResultField. 

### TField.RefreshLookupList

**location**
    
    
    cleanroom/packages/fpc-db/src/base/fields.inc
    

**declaration**
    
    
    (public) procedure TField.RefreshLookupList;
    

**Implementor: Almindor (untested)**

#### Specifications

This procedure does nothing if FLookupDataset is nil or if any of FLookupkeyFields, FLookupResultField or FKeyFields is an empty string. 

First of all the value of FLookupDataset is stored. The rest of the procedure is executed in an expcetion-block and whatever happens, FLookupDataset.Active is set to the stored value before the procedure ends. (Even in case of an exception) 

Thereafter FLookupDataset.Active is set to true. Then FFields.CheckFieldNames is called for FLookupKeyFields and FLookupDataset.FieldByName(FLookupResultField) is called to check if that field does exist. 

Then LookupList.Clear is called and the controls are disabled. (DisableControle). Thereafter an exception-block is started, so that EnableControls is always called after the code in the exception block. In the exception block FLookupList.Add is called for each record in FLookupDataset. The first parameter of FLookupList.Add is FieldValues[FLookupKeyFields] and the second is FieldValues[FLookupResultField] 

(As said, here Enablecontrols is called and the value of FLookupDataset.Active is restored.) 

**Related to**

  * TField.LookupCache 
  * TField.LookupDataSet 
  * TField.LookupKeyFields 
  * TField.LookupList 
  * TField.LookupResultField 



## TODO for the RTL

  * [Done] There is a memleak in TReader.ReadPropValue. A FFixups object is created, but never destroyed. Where should it be freed. Possible places: in TReader.EndReference or TReader.Destroy. 
  * [Done] Fix test for streaming of enumeration with value equal to low(TMyEnum) and no explicit 



default value, which means a default value of low(TMyEnum). Currently fpc 2.2.0 behaves differently from Delphi, the tests don't reflect this. 

  * [Done] There is a memleak in TTESTRESOLVEREFERENCE tests. SetupARef1A is called but the 



returned object is not freed. 

  * There are no tests for TReader.FindComponentClass 



## Variant streaming implementation

The variant streaming is very important not only for COM/CORBA compatibility, but also for general runtime access to the properties of object using GetPropValue/SetPropValue. Besides the variant is type, widely used in database components, and absence of Variant streaming limits the usage of variant as published properties. 

### General considerations

Variant itself is a structure, holding the type selector for the data and the data itself (in some cases it holds a reference). Some very special cases include variant holding reverence to COM/Corba object, which streaming would be considered later. 

One of the powerful concept of FPC Object Pascal RTL is the presence of abstract writer and reader, which can be easily subclassed. For our task we have to extend the functionality of basic class TAbstractObjectReader/Writer, to allow in derived classes use the methods - ReadVariant/WriteVariant. 

Current FPC-implemented Delphi-compatible writer (TWriter) works in such manner - 

  * It enumerates properties, determining property type. 
  * Using the property type as selector it branches to specified property storage routine. 
  * Using RTTI functions writer gets the property value, determines if the property have to be stored (is not default value and stored-checker method allows storage). 
  * Using methods of internal TBinaryObjectWriter object, referred as Driver, Writer stores value in the stream. 



#### Storing property of type Variant

For variant value there should be one additional consideration: Once we got property type and have determined that it is variant - we have to extract from variant the kind of basic pascal type which variant holds. 

There is routine VarType(arg:Variant), which can help to separate if the variant is Ordinal, Floating-point or String. These three kinds of variants can be handled by corresponding separate routines. Ordinal variants include: 

  * varShortInt,varSmallInt,varInteger,varInt64,varQWord, which can be stored using fDriver.WriteInteger() 
  * varBoolean, which can be stored using fDriver.WriteBoolean() 



Floating-point variants include: 

  * varSingle, which can be stored by fDriver.WriteSingle 
  * varDouble,varDate,varCurrency, which can be handled as double and stored using fDriver.WriteFloat 



String-holding variants: All the flavours of strings. IMO the string should be extracted as widestring and then stored as UTF8 representation. 

Other variant flavours (varXXXX) should be considered for now as non-implemented. 

One special case - is a variant holding NULL reference, and variant holding none (empty variant). These are different cases, and special measures should be taken to distinguish such conditions. Empty variant should not be stored. NULL variant is the value, like other. 

#### Reading property of type Variant

Reading of variants consists of the following: 

  * Once we have determined the property is variant, we have to read value type (ReadValue) from stream. 
  * Depending of the value type we have to read using fDriver methods the value of specified type, to the variable of compatible basic type without precision loss. 
  * We have to initialise variant using RTL assignment magic ( Variant := SomeGenericValue; ), which would implicitly invoke corresponding variant initialiser) 
  * Once we got assigned variant, we return it 



Special consideration is made for vaTrue, vaFalse - stored in stream. Variant should contain boolean value set to true if we got vaTrue and set to false if we got vaFalse. 

The very special consideration should be done for string Zoo. Read string should be converted to WideString and stored in variant. 

## Bug reports

  * [11060](<http://bugs.freepascal.org/view.php?id=11060>): Reader fails to assign class properties 
  * [11059](<http://bugs.freepascal.org/view.php?id=11059>): TComponent.DefineProperties not called

---

_Source: [https://wiki.freepascal.org/FPC_Cleanroom](https://web.archive.org/web/20200101000000/https://wiki.freepascal.org/FPC_Cleanroom)_
