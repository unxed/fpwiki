# XML Tutorial

│ **[Deutsch (de)](</XML_Tutorial/de> "XML Tutorial/de")** │  **English (en)** │  **[español (es)](</XML_Tutorial/es> "XML Tutorial/es")** │  **[français (fr)](</XML_Tutorial/fr> "XML Tutorial/fr")** │  **[magyar (hu)](</XML_Tutorial/hu> "XML Tutorial/hu")** │  **[Bahasa Indonesia (id)](</XML_Tutorial/id> "XML Tutorial/id")** │  **[italiano (it)](</XML_Tutorial/it> "XML Tutorial/it")** │  **[日本語 (ja)](</XML_Tutorial/ja> "XML Tutorial/ja")** │  **[한국어 (ko)](</XML_Tutorial/ko> "XML Tutorial/ko")** │  **[português (pt)](</XML_Tutorial/pt> "XML Tutorial/pt")** │  **[русский (ru)](<../ru/XML_Tutorial.md> "XML Tutorial/ru")** │  **[中文（中国大陆） (zh_CN)](</XML_Tutorial/zh_CN> "XML Tutorial/zh CN")** │    
****

The Extensible Markup Language (XML) is a World Wide Web Consortium (or "[W3C](<http://www.w3.org/>)") recommended language created to interchange information between different systems. 

It is a text based way to store information as opposed to storing the information in a binary format. 

Modern data interchange languages such as XHTML, as well as most WebServices technologies, are based on XML. This wiki can only really give a thumbnail overview of XML, with the primary focus being the parsing and use of XML files in Free Pascal applications. If you are interested in a more comprehensive explanation of XML and how it is used, see <http://en.wikipedia.org/wiki/XML>. 

## Contents

  * 1 Introduction
  * 2 Usage Examples
    * 2.1 Units in the uses clause
    * 2.2 Reading a text node
    * 2.3 Printing the names of nodes and attributes
    * 2.4 Populating a TreeView with XML
    * 2.5 Modifying a XML document
    * 2.6 Create a TXMLDocument from a string
    * 2.7 Validating a document
    * 2.8 Whitespace characters
    * 2.9 Streamed reading
    * 2.10 Generating a XML file
    * 2.11 Encoding
  * 3 See also
  * 4 External Links



## Introduction

Currently there is a set of units that provides support for XML on Free Pascal. These units are called "XMLRead", "XMLWrite" and "DOM" and they are part of the Free Component Library (FCL) from the Free Pascal Compiler. The FCL is already on the default search path for the compiler on Lazarus, so you only need to add the units to your uses clause in order to get XML support. The FCL is not documented currently (October / 2005), so this short tutorial aims at introducing XML access using those units. 

The XML DOM (Document Object Model) is a set of standardized objects that provide a similar interface for using XML on different languages and systems. The standard only specifies the methods, properties and other interface parts of the object, leaving the implementation free for different languages. The FCL currently fully supports the [XML DOM 2.0](<http://www.w3.org/TR/2000/REC-DOM-Level-2-Core-20001113/>) and a subset of [XML DOM 3.0](<http://www.w3.org/TR/2004/REC-DOM-Level-3-Core-20040407/>) listed [here](<dom.md> "dom"). 

## Usage Examples

Below there is a list of XML data manipulation examples with growing complexity. 

### Units in the uses clause

FPC comes with XML units that supports UTF8 and UTF16. They may not have the latest updates, so there is as well a version of those units (prefixed with "laz2_") in the Lazarus package LazUtils. The units are compatible and one can change from to the other just by changing the uses clause. 

The units for using the FPC XML support which uses strings encoded in the system encoding are: 

  * DOM
  * XMLRead
  * XMLWrite
  * XMLCfg
  * XMLUtils
  * XMLStreaming



The units for using the Lazarus XML support which has full UTF-8 Unicode support are: 

  * laz2_DOM
  * laz2_XMLRead
  * laz2_XMLWrite
  * laz2_XMLCfg
  * laz2_XMLUtils
  * laz_XMLStreaming.



Not all of them are needed in every example, though. You will need DOM as it defines several types including TXMLDocument. 

### Reading a text node

For Delphi Programmers: 

Note that when working with TXMLDocument, the text within a Node is considered a separate TEXT Node. As a result, you must access a node's text value as a separate node. Alternatively, the **TextContent** property may be used to retrieve content of all text nodes beneath the given one, concatenated together. 

The **ReadXMLFile** procedure always creates a new **TXMLDocument** , so you don't have to create it beforehand. However, be sure to destroy the document by calling **Free** when you are done. 

For instance, consider the following XML: 
    
    
    <?xml version="1.0"?>
    <request>
      <request_type>PUT_FILE</request_type>
      <username>123</username>
      <password>abc</password>
    </request>
    

The following code example shows both the correct and the incorrect ways of getting the value of the text node (add the units **laz2_XMLRead** and **laz2_DOM** to the used units list): 
    
    
    var
      PassNode: TDOMNode;
      Doc: TXMLDocument;
    begin
      try
        // Read in xml file from disk
        ReadXMLFile(Doc, 'test.xml');
        // Retrieve the "password" node
        PassNode := Doc.DocumentElement.FindNode('password');
        // Write out value of the selected node
        WriteLn(PassNode.NodeValue); // will be blank
        // The text of the node is actually a separate child node
        WriteLn(PassNode.FirstChild.NodeValue); // correctly prints "abc"
        // alternatively
        WriteLn(PassNode.TextContent);
      finally
        // finally, free the document
        Doc.Free;
      end;
    end;
    

Note that ReadXMLFile(...) ignores all leading whitespace characters when parsing a document. The section whitespace characters describes how to keep them. 

### Printing the names of nodes and attributes

If you want to navigate the DOM tree: when you need to access nodes in sequence, it is best to use **FirstChild** and **NextSibling** properties (to iterate forward), or **LastChild** and **PreviousSibling** (to iterate backward). 

For random access it is possible to use **ChildNodes** or **GetElementsByTagName** methods, but these will create a TDOMNodeList object which eventually must be freed. This differs from other DOM implementations like MSXML, because the FCL implementation is object-based, not interface-based. 

The following example shows how to print the names of nodes to a TMemo placed on a form. 

Below is the XML file called 'test.xml': 
    
    
    <?xml version="1.0"?>
    <images directory="mydir">
      <imageNode URL="graphic.jpg" title="">
        <Peca DestinoX="0" DestinoY="0">Pecacastelo.jpg1.swf</Peca>
        <Peca DestinoX="0" DestinoY="86">Pecacastelo.jpg2.swf</Peca>
      </imageNode>
    </images>
    

And here the Pascal code to execute the task: 
    
    
    var
      Doc: TXMLDocument;
      Child: TDOMNode;
      j: Integer;
    begin
      try
        ReadXMLFile(Doc, 'test.xml');
        Memo.Lines.Clear;
        // using FirstChild and NextSibling properties
        Child := Doc.DocumentElement.FirstChild;
        while Assigned(Child) do
        begin
          Memo.Lines.Add(Child.NodeName + ' ' + Child.Attributes.Item[0].NodeValue);
          // using ChildNodes method
          with Child.ChildNodes do
          try
            for j := 0 to (Count - 1) do
              Memo.Lines.Add(format('%s %s (%s=%s; %s=%s)',
                                    [
                                      Item[j].NodeName,
                                      Item[j].FirstChild.NodeValue,
                                      Item[j].Attributes.Item[0].NodeName,  // 1st attribute details
                                      Item[j].Attributes.Item[0].NodeValue,
                                      Item[j].Attributes.Item[1].NodeName,  // 2nd attribute details
                                      Item[j].Attributes.Item[1].NodeValue
                                    ]));
          finally
            Free;
          end;
          Child := Child.NextSibling;
        end;
      finally
        Doc.Free;
      end;
    end;
    

This will print: 
    
    
    imageNode graphic.jpg
    Peca Pecacastelo.jpg1.swf (DestinoX=0; DestinoY=0)
    Peca Pecacastelo.jpg2.swf (DestinoX=0; DestinoY=86)
    

### Populating a TreeView with XML

One common use of XML files is to parse them and show their contents in a tree like format. You can find the TTreeView component on the "Common Controls" tab on Lazarus. 

The function below will take a XML document previously loaded from a file or generated on code, and will populate a TreeView with it´s contents. The caption of each node will be the content of the first attribute of each node. 
    
    
    procedure TForm1.XML2Tree(tree: TTreeView; XMLDoc: TXMLDocument);
    var
      iNode: TDOMNode;
    
      procedure ProcessNode(Node: TDOMNode; TreeNode: TTreeNode);
      var
        cNode: TDOMNode;
        s: string;
      begin
        if Node = nil then Exit; // Stops if reached a leaf
        
        // Adds a node to the tree
        if Node.HasAttributes and (Node.Attributes.Length>0) then
          s := Node.Attributes[0].NodeValue
        else
          s := ''; 
        TreeNode := tree.Items.AddChild(TreeNode, s);
    
        // Goes to the child node
        cNode := Node.FirstChild;
    
        // Processes all child nodes
        while cNode <> nil do
        begin
          ProcessNode(cNode, TreeNode);
          cNode := cNode.NextSibling;
        end;
      end;
        
    begin
      iNode := XMLDoc.DocumentElement.FirstChild;
      while iNode <> nil do
      begin
        ProcessNode(iNode, nil); // Recursive
        iNode := iNode.NextSibling;
      end;
    end;
    

Another example that displays the complete XML structure including all attribute values (note: the long line referencing TreeView has been split so it will word wrap for this wiki; when writing it in code you do not have to break the line unless you like the formatting) : 
    
    
    procedure XML2Tree(XMLDoc:TXMLDocument; TreeView:TTreeView);
    
      // Local function that outputs all node attributes as a string
      function GetNodeAttributesAsString(pNode: TDOMNode):string;
      var i: integer;
      begin
        Result:='';
        if pNode.HasAttributes then
          for i := 0 to pNode.Attributes.Length -1 do
            with pNode.Attributes[i] do
              Result := Result + format(' %s="%s"', [NodeName, NodeValue]);
    
        // Remove leading and trailing spaces
        Result:=Trim(Result);
      end;
    
      // Recursive function to process a node and all its child nodes 
    
      procedure ParseXML(Node:TDOMNode; TreeNode: TTreeNode);
      begin
        // Exit procedure if no more nodes to process
        if Node = nil then Exit;
    
        // Add node to TreeView
        TreeNode := TreeView.Items.AddChild(TreeNode, 
                                              Trim(Node.NodeName+' '+ 
                                               GetNodeAttributesAsString(Node)+ 
                                               Node.NodeValue)
                                            );
    
        // Process all child nodes
        Node := Node.FirstChild;
        while Node <> Nil do
        begin
          ParseXML(Node, TreeNode);
          Node := Node.NextSibling;
        end;
      end;
    
    begin
      TreeView.Items.Clear;
      ParseXML(XMLDoc.DocumentElement,nil);
    end;
    

### Modifying a XML document

The first thing to remember is that TDOMDocument is the "handle" to the DOM. You can get an instance of this class by creating one or by loading a XML document. 

Nodes on the other hand cannot be created like a normal object. You *must* use the methods provided by TDOMDocument to create them, and later use other methods to put them in the correct place in the tree. This is because nodes must be "owned" by a specific document in DOM. 

Below are some common methods from TDOMDocument: 
    
    
    function CreateElement(const tagName: DOMString): TDOMElement; virtual;
    function CreateTextNode(const data: DOMString): TDOMText;
    function CreateCDATASection(const data: DOMString): TDOMCDATASection; virtual;
    function CreateAttribute(const name: DOMString): TDOMAttr; virtual;
    

`CreateElement` creates a new element. 

`CreateTextNode` creates a text node. 

`CreateAttribute` creates an attribute node. 

`CreateCDATASection` creates a CDATA section: regular XML markup characters such as <> are not interpreted within the CDATA section. See [Wikipedia article on CDATA](<https://secure.wikimedia.org/wikipedia/en/wiki/CDATA>)

A more convenient method to manipulate attributes is to use `TDOMElement.SetAttribute` method, which is also represented as the default property of `TDOMElement`: 
    
    
    // these two statements are equivalent
    Element.SetAttribute('name', 'value');
    Element['name'] := 'value';
    

And here an example method that will locate the selected item on a TTreeView and then insert a child node to the XML document it represents. The TreeView must be previously filled with the contents of an XML file using the [XML2Tree function](<XML_Tutorial.md> "XML Tutorial"). 
    
    
    procedure TForm1.actAddChildNode(Sender: TObject);
    var
      position: Integer;
      NovoNo: TDomNode;
    begin
      {*******************************************************************
      *  Detects the selected element
      *******************************************************************}
      if TreeView1.Selected = nil then Exit;
    
      if TreeView1.Selected.Level = 0 then
      begin
        position := TreeView1.Selected.Index;
    
        NovoNo := XMLDoc.CreateElement('item');
        TDOMElement(NovoNo).SetAttribute('nome', 'Item');
        TDOMElement(NovoNo).SetAttribute('arquivo', 'Arquivo');
        with XMLDoc.DocumentElement.ChildNodes do
        begin
          Item[position].AppendChild(NovoNo);
          Free;
        end;
    
        {*******************************************************************
        *  Updates the TreeView
        *******************************************************************}
        TreeView1.Items.Clear;
        XML2Tree(TreeView1, XMLDoc);
      end
      else if TreeView1.Selected.Level >= 1 then
      begin
        {*******************************************************************
        *  This function only works on the first level of the tree,
        *  but can easily be modified to work for any number of levels
        *******************************************************************}
      end;
    end;
    

### Create a TXMLDocument from a string

Given an XML document in string variable _MyXmlString_ , the following code will create it's DOM: 
    
    
    var
      S: TStringStream;
      XML: TXMLDocument;
    begin
      S := TStringStream.Create('');
      try
        // Read complete XML document
        ReadXMLFile(XML, S);             
        // Alternatively: read only an XML Fragment
        ReadXMLFragment(AParentNode, S); 
      finally
        S.Free;
      end;
    end;
    

### Validating a document

Since March 2007, DTD validation facility has been added to the FCL XML parser. Validation is checking that logical structure of the document conforms to the predefined rules, called _Document Type Definition_ (DTD). 

Here is an example of XML document with a DTD: 
    
    
    <?xml version='1.0'?>
    <!DOCTYPE root [
    <!ELEMENT root (child)+ >
    <!ELEMENT child (#PCDATA)>
    ]>
    <root>
      <child>This is a first child.</child>
      <child>And this is the second one.</child>
    </root>
    

This DTD specifies that 'root' element must have one or more 'child' elements, and that 'child' elements may have only character data inside. If parser detects any violations from these rules, it will report them. 

Loading such document is slightly more complicated. Let's assume we have XML data in a TStream object: 
    
    
    procedure TMyObject.DOMFromStream(AStream: TStream);
    var
      Parser: TDOMParser;
      Src: TXMLInputSource;
      TheDoc: TXMLDocument;
    begin
      try
        // create a parser object
        Parser := TDOMParser.Create;
        // and the input source
        Src := TXMLInputSource.Create(AStream);
        // we want validation
        Parser.Options.Validate := True;
        // assign a error handler which will receive notifications
        Parser.OnError := @ErrorHandler;
        // now do the job
        Parser.Parse(Src, TheDoc);
        // ...and cleanup
      finally
        Src.Free;
        Parser.Free;
      end;
    end;
    
    procedure TMyObject.ErrorHandler(E: EXMLReadError);
    begin
      if E.Severity = esError then  // we are interested in validation errors only
        writeln(E.Message);
    end;
    

### Whitespace characters

If you want to preserve leading whitespace characters in node texts, the above method is the way to load your XML document. Leading whitespace characters are ignored by default. That is the reason why the ReadXML(...) function never returns any leading whitespace characters in node texts. Before calling _Parser.Parse(Src, TheDoc)_ insert the line 
    
    
    Parser.Options.PreserveWhitespace := True;
    

This will force the parser to return all whitespace characters including all the newline characters that exist in an XML document to make it more readable! 

The following code is similar to the `ReadXMLFile()` procedure, but preserves leading whitespace: 
    
    
    procedure ReadXMLFilePreserveWhitespace(out Doc: TXMLDocument; FileName: string);
    var
      Parser: TDOMParser;
      Src: TXMLInputSource;
      InFile: TFileStream;
    begin
      try
        InFile := TFileStream.Create(FileName, fmOpenRead);
        Src := TXMLInputSource.Create(InFile);
        Parser := TDOMParser.Create;
        Parser.Options.PreserveWhitespace := True;
        Parser.Parse(Src, Doc);
      finally
        Src.Free;
        Parser.Free;
        InFile.Free;
      end;
    end;
    

Note that the xml document `Doc` is an `out` parameter created by the local `Parser`. It is the user's responsibility to free the `Doc` himself when it is no longer needed. 

### Streamed reading

DOM-based processing requires the entire document loaded into memory. This may be not desirable, or not possible if document is huge. FCL provides functionality to read XML data one node at a time, using TXMLReader class and its descendants. This is similar to SAX, but works without callbacks. TXMLReader closely resembles .NET XmlReader class. A basic example follows: 
    
    
    uses
      Classes,xmlreader,xmltextreader,xmlutils;
    
    procedure readme(AStream: TStream);
    var
      xtr: TXmlReader;
      settings: TXMLReaderSettings;
      inp: TXMLInputSource;
    begin
      settings := TXMLReaderSettings.Create;
      try
        settings.PreserveWhiteSpace := True;
        settings.Namespaces := True;
        inp := TXMLInputSource.Create(AStream);
        try
          xtr := TXmlTextReader.Create(inp,settings);
          try
            // Here the reading starts
            while xtr.Read do
            begin
              write(xtr.NodeType:25);
              if xtr.name<>'' then
                write(xtr.Name:9)
              else
                write('*no name* ');
              write(xtr.Value);
              writeln;
              if xtr.NodeType=ntElement then
              begin
                // print attributes
                if xtr.MoveToFirstAttribute then
                begin
                  repeat
                    writeln('---',xtr.NodeType:21,xtr.Name:10,xtr.Value:10);
                  until not xtr.MoveToNextAttribute;
                  xtr.MoveToContent;
                end;  
              end;
            end;
          // Cleanup follows  
          finally
            xtr.Free;
          end;
        finally
          inp.Free;
        end;
      finally
        settings.Free;
      end;
    end;
    

### Generating a XML file

Below is the complete code to write a XML file. (This was taken from a tutorial in the DeveLazarus blog) Please, remember to include the DOM and XMLWrite units in your uses clause. 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, LResources, Forms, Controls, Graphics, Dialogs, StdCtrls,
      DOM, XMLWrite;
    
    type
      { TForm1 }
      TForm1 = class(TForm)
        Button1: TButton;
        Label1: TLabel;
        Label2: TLabel;
        procedure Button1Click(Sender: TObject);
      private
        { private declarations }
      public
        { public declarations }
      end;
      
    var
      Form1: TForm1;
      
    implementation
    
    { TForm1 }
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      Doc: TXMLDocument;                                  // variable to document
      RootNode, parentNode, nofilho: TDOMNode;                    // variable to nodes
    begin
      try
        // Create a document
        Doc := TXMLDocument.Create;
    
        // Create a root node
        RootNode := Doc.CreateElement('register');
        Doc.Appendchild(RootNode);                           // save root node
      
        // Create a parent node
        RootNode:= Doc.DocumentElement;
        parentNode := Doc.CreateElement('usuario');
        TDOMElement(parentNode).SetAttribute('id', '001');       // create atributes to parent node
        RootNode.Appendchild(parentNode);                          // save parent node
    
        // Create a child node
        parentNode := Doc.CreateElement('nome');                // create a child node
        // TDOMElement(parentNode).SetAttribute('sexo', 'M');     // create atributes
        nofilho := Doc.CreateTextNode('Fernando');         // insert a value to node
        parentNode.Appendchild(nofilho);                         // save node
        RootNode.ChildNodes.Item[0].AppendChild(parentNode);       // insert child node in respective parent node
     
        // Create a child node
        parentNode := Doc.CreateElement('idade');               // create a child node
        // TDOMElement(parentNode).SetAttribute('ano', '1976');   // create atributes
        nofilho := Doc.CreateTextNode('32');               // insert a value to node
        parentNode.Appendchild(nofilho);                         // save node
        RootNode.ChildNodes.Item[0].AppendChild(parentNode);       // insert a childnode in respective parent node
    
        writeXMLFile(Doc, 'test.xml');                     // write to XML
      finally
        Doc.Free;                                          // free memory
      end;
    end;
    
    initialization
      {$I unit1.lrs}
    
    end.
    

The result will be the XML file below: 
    
    
    <?xml version="1.0"?>
    <register>
      <usuario id="001">
        <nome>Fernando</nome>
        <idade>32</idade>
      </usuario>
    </register>
    

An example where you don't need to reference an item by index. 
    
    
    procedure TForm1.Button2Click(Sender: TObject);
    var
      Doc: TXMLDocument;
      RootNode, ElementNode,ItemNode,TextNode: TDOMNode;
      i: integer;
    begin
      try
        // Create a document
        Doc := TXMLDocument.Create;
        // Create a root node
        RootNode := Doc.CreateElement('Root');
        Doc.Appendchild(RootNode);
        RootNode:= Doc.DocumentElement;
        // Create nodes
        for i := 1 to 20 do
        begin
          ElementNode:=Doc.CreateElement('Element');
          TDOMElement(ElementNode).SetAttribute('id', IntToStr(i));
    
          ItemNode:=Doc.CreateElement('Item1');
          TDOMElement(ItemNode).SetAttribute('Attr1', IntToStr(i));
          TDOMElement(ItemNode).SetAttribute('Attr2', IntToStr(i));
          TextNode:=Doc.CreateTextNode('Item1Value is '+IntToStr(i));
          ItemNode.AppendChild(TextNode);
          ElementNode.AppendChild(ItemNode);
    
          ItemNode:=Doc.CreateElement('Item2');
          TDOMElement(ItemNode).SetAttribute('Attr1', IntToStr(i));
          TDOMElement(ItemNode).SetAttribute('Attr2', IntToStr(i));
          TextNode:=Doc.CreateTextNode('Item2Value is '+IntToStr(i));
          ItemNode.AppendChild(TextNode);
          ElementNode.AppendChild(ItemNode);
    
          RootNode.AppendChild(ElementNode);
        end;
        // Save XML
        WriteXMLFile(Doc,'TestXML_v2.xml');
      finally
        Doc.Free;
      end;
    

Generated XML: 
    
    
    <?xml version="1.0"?>
    <Root>
      <Element id="1">
        <Item1 Attr1="1" Attr2="1">Item1Value is 1</Item1>
        <Item2 Attr1="1" Attr2="1">Item2Value is 1</Item2>
      </Element>
      <Element id="2">
        <Item1 Attr1="2" Attr2="2">Item1Value is 2</Item1>
        <Item2 Attr1="2" Attr2="2">Item2Value is 2</Item2>
      </Element>
      <Element id="3">
        <Item1 Attr1="3" Attr2="3">Item1Value is 3</Item1>
        <Item2 Attr1="3" Attr2="3">Item2Value is 3</Item2>
      </Element>
    </Root>
    

### Encoding

Starting from FPC version 2.4, the XML reader is able to process data in any encoding by using external decoders. See [XML_Decoders](<XML_Decoders.md> "XML Decoders") for more details. 

According to the XML standard, the encoding attribute in the first line of the XML is optional in case the actual encoding is UTF-8 (**without** BOM - Byte Order Marker) or UTF-16 (UTF-16 BOM). 

TXMLDocument has an encoding property since FPC 2.4. It is ignored as WriteXMLFile always uses UTF-8. 

  * FPC 2.4 doesn´t generate an encoding attribute in the first line of the XML file
  * FPC 2.6.0 and later explicitly write an UTF8 encoding attribute, as this is needed for some programs that cannot handle the XML without it.



## See also

  * [XML Decoders](<XML_Decoders.md> "XML Decoders")
  * [Using INI Files](<Using_INI_Files.md> "Using INI Files")
  * [fcl-xml](<fcl-xml.md> "fcl-xml")
  * [Internet Tools](<Internet_Tools.md> "Internet Tools"), for XPath 2 / XQuery processing



## External Links

  * [W3Schools](<http://www.w3schools.com/xml/default.asp>) Xml Tutorial


  * [Thomas Zastrow article](<http://www.thomas-zastrow.de/texte/fpcxml/index.php>) [Alternate link](<http://web.archive.org/web/20080802150722/http://www.thomas-zastrow.de/texte/fpcxml/index.php>) FPC and XML

---

_Source: [https://wiki.freepascal.org/XML_Tutorial](https://web.archive.org/web/20250114085833/https://wiki.freepascal.org/XML_Tutorial)_
