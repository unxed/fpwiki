# TMultiPropertyLink

TMultiPropertyLink enables linking all child RTTI controls' TIObject to a single component at once. 

## Selecting the controls

There are three methods to select the controls which should be changed by TMultiPropertyLink. This can be controlled by setting the appropriate properties. By Default all siblings e.g. on the same form are linked. Thus the properties have to be changed if you want to link controls to different objects. 

Property | Meaning   
---|---  
MaintainGrandChilds | Works only if ParentControl is set. If MaintainGrandChilds is not flagged, only the direct child controls of ParentControl will be linked. If there is another control between ParentControl and the RTTI control, you need to set this flag. Example: Panel (ParentControl) > Panel > TTiEdit   
MaintainSiblings | Link all components with the same owner. If you drop them on the form, all components and the TMultiPropertyLink have the same Owner (the form itself) regardless how you nest them e.g. in panels.   
ParentControl | Link all controls which are placed on this control.   
RootComponent | Link all components which are owned by this component.   
  
  


## Example

Create a form, drop a TEdit (name is Edit1), a TPanel (name is Panel1) and a TMultiPropertiLink (name is MultiPropertyLink1). Then drop two TTiEdit (names are TiEdit1 and TiEdit2) on the TPanel. 

Now, we'd like to display the width of Edit1 in TiEdit1, and the content of Edit1 (it's text property of Edit1) in TiEdit2. 

We can do this one by one --- setting Link.TIObject of both TiEdit1 and TiEdit2 as Edit1, and then setting their Link.TIPropertyName as "Width" and "Text" each. But as both TIEdit1 and TIEdit2 share parent and Link.TIObject, then can be put in a TMultiPropertyLink. 

Set MultiPropertyLink's properties as follows in Object Inspector. 
    
    
        MaintainSiblings=False
        ParentControl=Panel1
        TIObject=Edit1
    

Set following properties. 
    
    
        TIEdit1.Link.TIPropertyName='Width'
        TIEdit2.Link.TIPropertyName='Text'
    

When you run this program, you will see that both width and text of Edit1 are displayed within TTIEdits within Panel1. You can see the width of Edit1 changes if you change the width value manually by editing TiEdit1's value. You can change the text property either in the TIEdit2 of Edit1. 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** When running the application in Lazarus, you'll probably face EPropertyError exceptions. This is caused if you have non-RTTI components within the scope of TMultiPropertyLink like panels or buttons. This exception can be ignored.

---

_Source: [https://wiki.freepascal.org/TMultiPropertyLink](https://web.archive.org/web/20240101000000/https://wiki.freepascal.org/TMultiPropertyLink)_
