# IDE Window: ToDo List

│ **[Deutsch (de)](</IDE_Window:_ToDo_List/de> "IDE Window: ToDo List/de")** │  **English (en)** │  **[suomi (fi)](</IDE_Window:_ToDo_List/fi> "IDE Window: ToDo List/fi")** │  **[français (fr)](</IDE_Window:_ToDo_List/fr> "IDE Window: ToDo List/fr")** │    
****  
****

The ToDo list shows the list of ToDo comments in all project units. 

Until Lazarus 1.6 it only searched the units listed in the project inspector. Since 1.7 it searches all used units (i.e. lpr uses section) too. 

Make comments in source-code like these: 
    
    
    {TODO 5 -oOwnerName -cCategoryName: Todo_text}
    {DONE 5 -oOwnerName -cCategoryName: Todo_text}
    {#todo 5 -oOwnerName -cCategoryName: Todo_text}
    {#done 5 -oOwnerName -cCategoryName: Todo_text}
    { ToDo: Todo_text}
    // ToDo: Todo_text
    (* ToDo: Todo_text *)
    

The number, '5' in examples (priority), -o and -c tags are optional. 

## Controls

Buttons: 

  * **Refresh** : Search again all files for ToDo comments and update the list.
  * **Goto** : Jump to the item in the source editor.
  * **Export** : Make report of todo items in a CSV file.
  * **Help** : Show this help page.



Options: 

  * **Listed** : Search units listed ion project inspector / package editor.
  * **Used** : Search units used by main source file.
  * **Editor** : Search units in the source editor.
  * **Packages** : Search units from used packages.



Listbox: 

  * Click the ToDo item in the list to jump to the ToDo item in the source editor.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_ToDo_List](https://web.archive.org/web/20241212120854/https://wiki.freepascal.org/IDE_Window%3A_ToDo_List)_
