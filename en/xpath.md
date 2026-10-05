# xpath

│ **English (en)** │    
****

Just an XPath implementation. Should be fairly complete, but there hasn't been further development recently. 

## Example

**ts.xml**
    
    
    <?xml version="1.0" ?><!DOCTYPE TS><TS language="es_ES" version="2.1">
    <context>
        <name>AboutDialog</name>
        <message>
            <location filename="../src/lib/other/aboutdialog.ui" line="60"/>
            <source>Authors</source>
            <translation>Autores</translation>
        </message>
        <message>
            <location filename="../src/lib/other/aboutdialog.cpp" line="56"/>
            <source>Authors and Contributors</source>
            <translation>Autores y contribuidores</translation>
        </message>
    </context>
    </TS>
    
    
    
    uses
      DOM, XMLRead, XPath;
    
    var
      Xml: TXMLDocument;
      XPathResult: TXPathVariable;
    begin
      ReadXMLFile(Xml, 'ts.xml');
      //Get text inside <translation> tag
      XPathResult := EvaluateXPathExpression('/TS/context[name="AboutDialog"]/message[source="Authors"]/translation', Xml.DocumentElement);
      ShowMessage(String(XPathResult.AsText));
      XPathResult.Free;
      Xml.Free;
    end;
    

  
Back to [fcl-xml](<fcl-xml.md> "fcl-xml") overview.

---

_Source: [https://wiki.freepascal.org/xpath](https://web.archive.org/web/20240121090041/https://wiki.freepascal.org/xpath)_
