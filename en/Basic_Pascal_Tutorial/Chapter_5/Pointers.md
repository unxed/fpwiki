# Basic Pascal Tutorial/Chapter 5/Pointers

│ **[български (bg)](</Basic_Pascal_Tutorial/Chapter_5/Pointers/bg> "Basic Pascal Tutorial/Chapter 5/Pointers/bg")** │  **English (en)** │  **[français (fr)](</Basic_Pascal_Tutorial/Chapter_5/Pointers/fr> "Basic Pascal Tutorial/Chapter 5/Pointers/fr")** │  **[日本語 (ja)](</Basic_Pascal_Tutorial/Chapter_5/Pointers/ja> "Basic Pascal Tutorial/Chapter 5/Pointers/ja")** │  **[中文（中国大陆） (zh_CN)](</Basic_Pascal_Tutorial/Chapter_5/Pointers/zh_CN> "Basic Pascal Tutorial/Chapter 5/Pointers/zh CN")** │    
****

[ ◄ ](<Records.md> "Basic Pascal Tutorial/Chapter 5/Records") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<../Chapter_6/Final_words.md> "Basic Pascal Tutorial/Chapter 6/Final words")  
---|---|---  
  
5F - Pointers (author: Tao Yue, state: unchanged) 

A [pointer](<../../Pointer.md> "Pointer") is a data type which holds a memory address. A pointer can be thought of as a reference to that memory address, while a variable accesses that memory address directly. If a variable is someone's phone num‍ber, then a pointer is the page and line number where it's listed in the phone book. To access the data stored at that memory address, you dereference the pointer. 

## Memory routines

To declare a pointer data type, you must specify what it will point to. That data type is preceded with a caret (^). For example, if you are creating a pointer to an integer, you would use this code: 
    
    
    type
    	PointerType = ^integer;
    

You can then, of course, declare variables to be of type PointerType. 

Before accessing a pointer, you block off an area in memory for that pointer to access. This is done with: 
    
    
    New(PointerVariable);
    

To access the data at the pointer's memory location, you add a caret after the pointer name. For example, if PointerVariable was declared as type PointerType (from above), you can assign the memory location a value by using: 
    
    
    PointerVariable^ := 5;
    

After you are done with the pointer, you must deallocate the memory space. Otherwise, each time the program is run, it will allocate more and more memory until your computer has no more. To deallocate the memory, you use the `Dispose` command: 
    
    
    Dispose(PointerVariable);
    

A pointer can be assigned to another pointer. However, note that since only the **address** , not the value, is being copied, once you modify the data located at one pointer, the other pointer, when dereferenced, also yields modified data. Also, if you free (or deallocate) a pointer, the copied pointer now points to meaningless data. 

## Trivial usage example: singly linked lists

What is a pointer good for? Why can't you just use an integer in the examples above instead of a pointer to an integer? Well, the above is clearly a contrived example. The real power of pointers is that, in conjunction with records, it makes dynamically-sized data structures possible. If you need to store many items of one data type in order, you can use an array. However, your array has a predefined size. If you don't have a large enough size, you may not be able to accomodate all the data. If you have a huge array, you take up a lot of memory when sometimes that memory is not being used. 

A dynamic data structure, on the other hand, takes up only as much memory as is being used. What you do is to create a data type that points to a record. Then, the record has that pointer type as one of its fields. E. g. stacks and queues can all be implemented using this data structure: 
    
    
    type
    	PointerType = ^RecordType;
    	RecordType = record
    		data : integer;
    		next : PointerType;
    	end;
    

Each element points to the next. The last record in the chain indicates that there is no next record by setting its next field to a value of `nil`. 

[ ◄ ](<Records.md> "Basic Pascal Tutorial/Chapter 5/Records") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<../Chapter_6/Final_words.md> "Basic Pascal Tutorial/Chapter 6/Final words")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_5/Pointers](https://web.archive.org/web/20250405091407/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_5/Pointers)_
