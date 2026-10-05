# LazControls

**LazControls** is a set of controls that are often used in Lazarus IDE itself, but they can be used elsewhere, too. They are available under the [LazControls tab](<LazControls_tab.md> "LazControls tab") of the [Component Palette](<Component_Palette.md> "Component Palette"). 

## Contents

  * 1 FilterEdit controls
    * 1.1 TListFilterEdit
      * 1.1.1 Working with TCheckListBox
    * 1.2 TListViewFilterEdit
    * 1.3 TTreeFilterEdit
      * 1.3.1 Mode 1: Sub-branches under root nodes
      * 1.3.2 Mode 2: A whole tree
  * 2 SpinEx controls
    * 2.1 TSpinEditEx
    * 2.2 TFloatSpinEditEx



## FilterEdit controls

Controls inherited from TCustomControlFilterEdit provide a filtering edit box connected to a container control. Currently there are filters for TListBox, TListView and TTreeView implemented. When the container control is connected to the filter control, the filtering happens automatically as a user enters text. When the filter is empty and does not have focus, it shows a grey "(filter)" text. 

There are some common properties that control the behavior: 

  * property OnFilterItem -- a user can provide an event handler to give extra conditions for filtering, in addition to the default behavior. This feature has for example enabled filtering the Options windows in Lazarus IDE, based on captions of all controls on the options pages.


  * property OnCheckItem -- This has effect when items in the filtered container can be checked. ToDo: improve...


  * property UseFormActivate -- Sometimes the control's OnEnter and OnExit handlers are not called when focus is moved directly to/from another window.



Then the gray "(filter)" can remain visible even when the control gains focus. When UseFormActivate is True, it finds the parent form and registers handlers for its OnActivate and OnDeactivate events. They properly keep track when the control has focus. This setting is False by default to make sure no existing event handler is overwritten. (An improvent could be to save an existing handler before registering a new one.) 

### [TListFilterEdit](</index.php?title=TListFilterEdit&action=edit&redlink=1> "TListFilterEdit \(page does not exist\)")

This control can filter a ListBox. Properties: 

  * FilteredListBox -- must be assigned to the desired TListBox control either at design time or run time.



Data should be added to TListFilterEdit.Items, not to the container ListBox.Items list. The filter works on its own internal data list and copies to the ListBox only the items that pass the filtering tests. The Items property is also a TStringList and works the same way as ListBox.Items. 

You can also attach a filter to a ListBox which contains existing data. Then the existing items are copied to the filter's items initially. 

#### Working with TCheckListBox

ToDo: 

  * procedure RemoveItem(AItem: string);
  * procedure ItemWasClicked(AItem: string; IsChecked: Boolean);



### [TListViewFilterEdit](<TListViewFilterEdit.md> "TListViewFilterEdit")

This control can filter a ListView. Properties: 

  * FilteredListView -- must be assigned to the desired TListView control either at design time or run time.



Data should be added to TListViewFilterEdit.Items, not to the container ListView. 

ToDo... 

You can also attach a filter to a ListView which contains existing data. Then the existing items are copied to the filter's items initially. 

### [TTreeFilterEdit](</index.php?title=TTreeFilterEdit&action=edit&redlink=1> "TTreeFilterEdit \(page does not exist\)")

This control can filter a [TTreeView](<TTreeView.md> "TTreeView"). Important properties: 

  * FilteredTreeview -- must be assigned to the desired TTreeview control either at design time ot run time.



This control has 2 different operation modes. They are so different that there could be 2 separate controls as well. One mode maintains and filters sub-items of root-nodes in a tree, another mode filters a whole existing tree using TreeNode.Visible property. 

#### Mode 1: Sub-branches under root nodes

Items for each branch are maintained in TTreeFilterBranch class instance. The functions : 

  * TTreeFilterEdit.GetCleanBranch(ARootNode: TTreeNode): TTreeFilterBranch;
  * TTreeFilterEdit.GetExistingBranch(ARootNode: TTreeNode): TTreeFilterBranch;



can be used to get a new or existing branch. All its items will show under ARootNode. 

Items can be added to a branch with procedure 

  * AddNodeData(ANodeText: string; AData: TObject; AFullFilename: string = _);_



ToDo: explain better 

The branches can also show a directory hierarchy. It is controlled by property : 

  * ShowDirHierarchy



If enabled, it assumes the strings are directory names with separators and creates a multi-level tree structure imitating the directory structure. 

This mode of TreeFilterEdit is used in Lazarus IDE for Package and Project Inspectors. 

#### Mode 2: A whole tree

When no branches are defined (no calls made to GetBranch), the TreeFilterEdit control filters the whole tree automatically. It uses each TreeNode's Visible property to show/hide it. 

Nodes and their parent nodes are expanded during filtering as needed. There is also one property affecting the expansion: 

  * ExpandAllInitially



If enabled, all nodes will be expanded in the beginning. 

This mode of TreeFilterEdit is used in Lazarus IDE for Options windows, Object inspector, and others... 

## SpinEx controls

T(Float)SpinEditEx is a control similar to T(Float)SpinEdit, but it is implemented in a widgetset independant manor.  
Especially the behaviour of the control, when the text inside it does no represent a valid number, is consistent across widgetsets.  
This behaviour is controlled by the control's NullValueBehaviour property.  
Unlike T(Float)SpinEdit then result of GetValue is always derived from the actual text in the control.  
T(Float)SpinEdit also has some nice features that T(Float)SpinEdit does not have. 

### TSpinEditEx

  * Handles Int64 values
  * Can show a user defined ThousandSeparator



### TFloatSpinEditEx

  * Can display values in scientific notation
  * Has DecimalSpearator property that is independant from DefaultFormatSettings

---

_Source: [https://wiki.freepascal.org/LazControls](https://web.archive.org/web/20250121212205/https://wiki.freepascal.org/LazControls)_
