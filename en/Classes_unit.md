# Classes unit

The **Classes** unit is part of the Free Pascal standard [Runtime Library](<RTL.md> "RTL") and contains abstract classes that form the basis for derived classes, and also implements a number of general purpose concrete classes including: 

## Classes Defined

  * [TAbstractObjectReader](</index.php?title=TAbstractObjectReader&action=edit&redlink=1> "TAbstractObjectReader \(page does not exist\)")
    * [TBinaryObjectReader](</index.php?title=TBinaryObjectReader&action=edit&redlink=1> "TBinaryObjectReader \(page does not exist\)")
  * [TAbstractObjectWriter](</index.php?title=TAbstractObjectWriter&action=edit&redlink=1> "TAbstractObjectWriter \(page does not exist\)")
    * [TBinaryObjectWriter](</index.php?title=TBinaryObjectWriter&action=edit&redlink=1> "TBinaryObjectWriter \(page does not exist\)")
    * [TTextObjectWriter](</index.php?title=TTextObjectWriter&action=edit&redlink=1> "TTextObjectWriter \(page does not exist\)")
  * [TBits](</index.php?title=TBits&action=edit&redlink=1> "TBits \(page does not exist\)")
  * [TFiler](</index.php?title=TFiler&action=edit&redlink=1> "TFiler \(page does not exist\)")
    * [TReader](</index.php?title=TReader&action=edit&redlink=1> "TReader \(page does not exist\)")
    * [TWriter](</index.php?title=TWriter&action=edit&redlink=1> "TWriter \(page does not exist\)")
  * [TFPList](</index.php?title=TFPList&action=edit&redlink=1> "TFPList \(page does not exist\)") ([TFPListEnumerator](</index.php?title=TFPListEnumerator&action=edit&redlink=1> "TFPListEnumerator \(page does not exist\)"))
  * [TList](<TList.md> "TList") \- Manages a list of data type [Pointer](<Pointer.md> "Pointer"), can search and sort the list, and has an event notification feature. ([TListEnumerator](</index.php?title=TListEnumerator&action=edit&redlink=1> "TListEnumerator \(page does not exist\)"))
  * [TThread](</index.php?title=TThread&action=edit&redlink=1> "TThread \(page does not exist\)")
  * [TThreadList](</index.php?title=TThreadList&action=edit&redlink=1> "TThreadList \(page does not exist\)")
  * [TParser](</index.php?title=TParser&action=edit&redlink=1> "TParser \(page does not exist\)")
  * [TPersistent](</index.php?title=TPersistent&action=edit&redlink=1> "TPersistent \(page does not exist\)")
    * [TCollection](<TCollection.md> "TCollection") ([TCollectionEnumerator](</index.php?title=TCollectionEnumerator&action=edit&redlink=1> "TCollectionEnumerator \(page does not exist\)")) 
      * [TOwnedCollection](</index.php?title=TOwnedCollection&action=edit&redlink=1> "TOwnedCollection \(page does not exist\)")
    * [TCollectionItem](</index.php?title=TCollectionItem&action=edit&redlink=1> "TCollectionItem \(page does not exist\)")
    * [TComponent](</index.php?title=TComponent&action=edit&redlink=1> "TComponent \(page does not exist\)")
      * [TBasicAction](</index.php?title=TBasicAction&action=edit&redlink=1> "TBasicAction \(page does not exist\)")
      * [TDataModule](</index.php?title=TDataModule&action=edit&redlink=1> "TDataModule \(page does not exist\)")
    * [TInterfacedPersistent](</index.php?title=TInterfacedPersistent&action=edit&redlink=1> "TInterfacedPersistent \(page does not exist\)")
    * [TStrings](<TStrings.md> "TStrings")
      * [TStringList](<TStringList.md> "TStringList")
  * [TRecall](</index.php?title=TRecall&action=edit&redlink=1> "TRecall \(page does not exist\)")
  * [TStream](<TStream.md> "TStream")
    * [TCustomMemoryStream](</index.php?title=TCustomMemoryStream&action=edit&redlink=1> "TCustomMemoryStream \(page does not exist\)")
      * [TMemoryStream](</index.php?title=TMemoryStream&action=edit&redlink=1> "TMemoryStream \(page does not exist\)")
        * [TBytesStream](</index.php?title=TBytesStream&action=edit&redlink=1> "TBytesStream \(page does not exist\)")
      * [TResourceStream](</index.php?title=TResourceStream&action=edit&redlink=1> "TResourceStream \(page does not exist\)")
    * [THandleStream](</index.php?title=THandleStream&action=edit&redlink=1> "THandleStream \(page does not exist\)")
      * [TFileStream](<TFileStream.md> "TFileStream")
    * [TOwnerStream](</index.php?title=TOwnerStream&action=edit&redlink=1> "TOwnerStream \(page does not exist\)")
    * [TProxyStream](</index.php?title=TProxyStream&action=edit&redlink=1> "TProxyStream \(page does not exist\)")
    * [TStringStream](</index.php?title=TStringStream&action=edit&redlink=1> "TStringStream \(page does not exist\)")



## Exception Classes Defined

  * **EStreamError** \- Exception raised when an error occurs during read or write operations on a stream. 
    * **EFCreateError** \- Exception raised when an error occurred during creation of a [TFileStream](<TFileStream.md> "TFileStream") stream.
    * **EFOpenError** \- Exception raised when an error occurred during creation of a TFileStream.
    * **EFilerError** \- Exception raised by the component streaming system if an error occurs. 
      * **EReadError** \- Exception raised if an error occurs while reading from a stream.
      * **EWriteError** \- Exception raised when an error occurs during writing to a stream.
      * **EClassNotFound** \- Exception raised when an unknown class is referenced in a streamed component.
      * **EInvalidImage** \- Exception raised when the resource header needed for streaming of a component is invalid.
  * **EResNotFound** \- Exception raised when a resource, needed to initialize a component, is not found.
  * **EListError** (ifndef FPC_TESTGENERICS) - Exception raised when an error occurs in lists handling.
  * **EBitsError** \- Exception raised when an error occurs in a method of [TBits](</index.php?title=TBits&action=edit&redlink=1> "TBits \(page does not exist\)").
  * **EStringListError** \- Exception raised when an error occurs in a method of [TStrings](<TStrings.md> "TStrings").
  * **EComponentError** \- Exception raised when an error occurs in the component registration routines.
  * **EParserError** \- Exception raised when an error occurs during the parsing of streams.
  * **EOutOfResources** \- Exception raised when the system is out of resources.



## See also

  * [classes unit doc](<http://lazarus-ccr.sourceforge.net/docs/rtl/classes/> "doc:rtl/classes/")

---

_Source: [https://wiki.freepascal.org/Classes_unit](https://web.archive.org/web/20240318085500/https://wiki.freepascal.org/Classes_unit)_
