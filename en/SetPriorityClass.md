# SetPriorityClass

SetPriorityClass - a function that sets the priority class for a specified process. This function is specific to Windows. 

  
The SetPriorityClass may have one of the following priority classes: 

  * HIGH_PRIORITY_CLASS <= High performs tasks immediately.  

  * IDLE_PRIORITY_CLASS <= Idle performs tasks when the system is idle.  

  * NORMAL_PRIORITY_CLASS <= Normal performs tasks at regular speed.  

  * REALTIME_PRIORITY_CLASS <= Real Time performs tasks immediately and above all other tasks, including operating system tasks.  




Use caution when using REALTIME_PRIORITY_CLASS. It can potentially take so much of the CPU resources that there may not be anything left for other items to run, including operating system requests. 

  
EXAMPLE: 
    
    
    uses Windows; // Unit containing the SetPriorityClass function.
     
    procedure TForm1.FormCreate(Sender: TObject);
    begin
         SetPriorityClass(GetCurrentProcess(), HIGH_PRIORITY_CLASS);
         // Sets the current running application to High Priority.
    end;

---

_Source: [https://wiki.freepascal.org/SetPriorityClass](https://web.archive.org/web/20170521132407/https://wiki.freepascal.org/SetPriorityClass)_
