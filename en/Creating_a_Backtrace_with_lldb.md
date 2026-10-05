# lldb

lldb is the Xcode (macOS) default debugger. It's available when you have installed Xcode and/or the Xcode standalone command line tools. 

It's still possible to build and install the gdb debugger. However, there are certain security requirements, and not everyone would like to take that approach. 

## Contents

  * 1 LLDB and Lazarus
    * 1.1 Crashing issues
  * 2 Creating a backtrace
  * 3 Breakpoint issues
  * 4 DevToolsSecurity
  * 5 See also



## LLDB and Lazarus

LLDB is now the standard debugger in the Lazarus IDE for macOS. The recommended setting is "Dwarf with sets" (-gw -godwarfsets). 

### Crashing issues

Sometimes the LLDB crashes when running an app from the IDE. In many cases this is caused by a corrupt bundle file. Rebuilding the bundle uses to help in these cases. 

## Creating a backtrace

1\. Open an Applications > Utilities > Terminal. 

2\. Change to the application bundle (if you're debugging a GUI application). Example: 
    
    
     cd project1.app/Contents/MacOS/
    

If you're debugging a command-line application, you don't need to do that. 

3\. Run lldb with the program to debug. Launching a project with lldb takes longer than launching without it. 
    
    
    $ lldb project1
    (lldb) target create "project1"
    Current executable set to 'project1' (x86_64).
    (lldb) 
    

**Note** launching an application with lldb for the first time might require you have admin rights - you might be prompted for your account password. This happens only on the initial launch of lldb. However, after a reboot, the authentication might be needed again. 

4\. You probably want to set a breakpoint for raised exceptions: 
    
    
    (lldb) breakpoint set -n fpc_raiseexception
    

5\. When you see lldb is ready ("lldb" would show up in the Terminal) execute the "r" command (run). Example: 
    
    
    (lldb) r
    Process 12974 launched: '/Users/dmitry/Desktop/Cocoa/buffer/project1.app/Contents/MacOS/project1' (x86_64)
    

6\. You should now switch back to your application and take the actions that are necessary to reproduce the crash. 

7\. When the crash occurs the lldb console is available again. If debugging information and sources are available, lldb would typically show you the line of code where crash occurred. 
    
    
     Process 12974 stopped
    * thread #1: tid = 0xd4503, 0x000000010003d9b1 project1`UNIT1$_$TFORM1_$__$$_BUTTON1CLICK$TOBJECT + 97 at unit1.pas:47, queue = 'com.apple.main-thread', stop reason = EXC_ARITHMETIC (code=EXC_I386_DIV, subcode=0x0)
        frame #0: 0x000000010003d9b1 project1`UNIT1$_$TFORM1_$__$$_BUTTON1CLICK$TOBJECT + 97 at unit1.pas:47
       44  	  b:=5;
       45  	  b:=b div 5;
       46  	  b:=b-b;
    -> 47  	  writeln(a div b);
       48  	end;
       49  	
       50  	end.
    (lldb) 
    

8\. Run "bt" command to get the backtrace. 
    
    
    (lldb) bt
    * thread #1: tid = 0xd4503, 0x000000010003d9b1 project1`UNIT1$_$TFORM1_$__$$_BUTTON1CLICK$TOBJECT + 97 at unit1.pas:47, queue = 'com.apple.main-thread', stop reason = EXC_ARITHMETIC (code=EXC_I386_DIV, subcode=0x0)
      * frame #0: 0x000000010003d9b1 project1`UNIT1$_$TFORM1_$__$$_BUTTON1CLICK$TOBJECT + 97 at unit1.pas:47
        frame #1: 0x00000001001eeb88 project1`CONTROLS$_$TCONTROL_$__$$_CLICK + 104 at control.inc:2916
        frame #2: 0x00000001002588da project1`STDCTRLS$_$TBUTTONCONTROL_$__$$_CLICK + 42 at buttoncontrol.inc:55
        frame #3: 0x000000010025909f project1`STDCTRLS$_$TCUSTOMBUTTON_$__$$_CLICK + 79 at buttons.inc:169
        frame #4: 0x00000001002587e2 project1`STDCTRLS$_$TBUTTONCONTROL_$__$$_WMDEFAULTCLICKED$TLMESSAGE + 66 at buttoncontrol.inc:21
        frame #5: 0x0000000100012103 project1`SYSTEM$_$TOBJECT_$__$$_DISPATCH$formal + 203
        frame #6: 0x00000001001eda95 project1`CONTROLS$_$TCONTROL_$__$$_WNDPROC$TLMESSAGE + 517 at control.inc:2246
        frame #7: 0x00000001001e0926 project1`CONTROLS$_$TWINCONTROL_$__$$_WNDPROC$TLMESSAGE + 806 at wincontrol.inc:5406
        frame #8: 0x00000001001cc980 project1`LCLMESSAGEGLUE_$$_DELIVERMESSAGE$TOBJECT$formal$$INT64 + 224 at lclmessageglue.pas:112
        frame #9: 0x00000001001cca8b project1`LCLMESSAGEGLUE_$$_SENDSIMPLEMESSAGE$TCONTROL$LONGWORD$$INT64 + 59 at lclmessageglue.pas:143
        frame #10: 0x000000010029200a project1`COCOAWSSTDCTRLS$_$TLCLBUTTONCALLBACK_$__$$_BUTTONCLICK + 58 at cocoawsstdctrls.pp:401
        frame #11: 0x00000001001b2a3f project1`-[TCocoaButton actionButtonClick:] + 87 at cocoaprivate.pp:2294
        frame #12: 0x00007fffbec8dc41 libsystem_trace.dylib`_os_activity_initiate + 61
        frame #13: 0x00007fffa7a5c770 AppKit`-[NSApplication(NSResponder) sendAction:to:from:] + 456
        frame #14: 0x00007fffa75450d8 AppKit`-[NSControl sendAction:to:] + 86
        frame #15: 0x00007fffa7545000 AppKit`__26-[NSCell _sendActionFrom:]_block_invoke + 136
        frame #16: 0x00007fffbec8dc41 libsystem_trace.dylib`_os_activity_initiate + 61
        frame #17: 0x00007fffa7544f58 AppKit`-[NSCell _sendActionFrom:] + 128
        frame #18: 0x00007fffa75878d9 AppKit`-[NSButtonCell _sendActionFrom:] + 98
        frame #19: 0x00007fffbec8dc41 libsystem_trace.dylib`_os_activity_initiate + 61
        frame #20: 0x00007fffa754383c AppKit`-[NSCell trackMouse:inRect:ofView:untilMouseUp:] + 2481
        frame #21: 0x00007fffa7587616 AppKit`-[NSButtonCell trackMouse:inRect:ofView:untilMouseUp:] + 798
        frame #22: 0x00007fffa75421f7 AppKit`-[NSControl mouseDown:] + 832
        frame #23: 0x00000001001b31ab project1`-[TCocoaButton mouseDown:] + 139 at cocoaprivate.pp:2410
        frame #24: 0x00007fffa7bd291f AppKit`-[NSWindow(NSEventRouting) _handleMouseDownEvent:isDelayedEvent:] + 6341
        frame #25: 0x00007fffa7bcf13c AppKit`-[NSWindow(NSEventRouting) _reallySendEvent:isDelayedEvent:] + 1942
        frame #26: 0x00007fffa7bce5da AppKit`-[NSWindow(NSEventRouting) sendEvent:] + 541
        frame #27: 0x00000001001b15df project1`-[TCocoaWindow sendEvent:] + 919 at cocoaprivate.pp:1943
        frame #28: 0x00007fffa7a586f5 AppKit`-[NSApplication(NSEvent) sendEvent:] + 1145
        frame #29: 0x00000001001a405c project1`COCOAINT$_$TCOCOAWIDGETSET_$__$$_APPPROCESSMESSAGES + 268 at cocoaobject.inc:105
        frame #30: 0x000000010003733d project1`FORMS$_$TAPPLICATION_$__$$_HANDLEMESSAGE + 45 at application.inc:1276
        frame #31: 0x00000001000378e1 project1`FORMS$_$TAPPLICATION_$__$$_RUNLOOP + 209 at application.inc:1419
        frame #32: 0x00000001001a3d17 project1`-[TCocoaApplication run] + 71 at cocoaint.pas:368
        frame #33: 0x00000001001a3f4a project1`COCOAINT$_$TCOCOAWIDGETSET_$__$$_APPRUN$TAPPLICATIONMAINLOOP + 106 at cocoaobject.inc:83
        frame #34: 0x0000000100037808 project1`FORMS$_$TAPPLICATION_$__$$_RUN + 96 at application.inc:1401
        frame #35: 0x0000000100001514 project1`PASCALMAIN + 92 at project1.lpr:19
        frame #36: 0x0000000100020ef9 project1`FPC_SYSTEMMAIN + 41
        frame #37: 0x0000000100001494 project1`start + 52
    (lldb)
    

9\. At this point you can give the "q" command (quit) to the debugger. 
    
    
    (lldb) q
    Quitting LLDB will kill one or more processes. Do you really want to proceed: [Y/n] y
    

10 If you need to stop the application running under the debugger: 

    10.1 Hit Ctrl+C on the Terminal tab. This should signal to the debugger to pause the executed program.
    10.2 Give "q" command (quit) to the debugger.

## Breakpoint issues

Issue: There seems to be an issue starting with Xcode 11.4 (and still in Xcode 11.5). If stopped at a breakpoint within an objcclass type subclass, the “self” item is missing from the debugger. I still see the “this” for any objects descended from fpc’s TObject, so this seems to only apply to objcclass types. 

Background: At some point, lldb gained "support" for Pascal. Unfortunately, that support was barely non-existent, so rather than treating debug information in a Pascal program the same as it did in C/C++ programs like it did before, suddenly a bunch of things got broken when debugging a program whose debug information indicated it was a Pascal program. 

Possible solution: What you can try is to use the **-godwarfcpp** command line option. Then FPC will encode in the debug information that it's a C++ program which solves several of those issues. 

## DevToolsSecurity

The `DevToolsSecurity` command line utility changes the security authorisation policies for use of the Apple-code-signed debugger and performance analysis tools on development systems. 

On normal user systems, the first time in a given login session that any such Apple code-signed debugger or performance analysis tools are used to examine one of the user's processes, the user is queried for an administrator password for authorisation. The tool changes the authorisation policies, such that a user who is a member of either the admin group or the _developer group does not need to enter an additional password to use the Apple-code-signed debugger or performance analysis tools. 

To enable, open an Applications > Utilities > Terminal window and execute the following command: 
    
    
    sudo DevToolsSecurity -enable
    

to enable the change of security authorisation policies; 
    
    
    sudo DevToolsSecurity -disable
    

to disable the change of security authorisation policies; and 
    
    
    DevToolsSecurity -status
    

to check whether the change of security authorisation policies is enabled or disabled. 

## See also

  * [Apple: Appendix A: LLDB for GDB Users – Command Summary](<https://developer.apple.com/library/archive/documentation/General/Conceptual/lldb-guide/chapters/A3-GDB-Summary.html>)
  * [Creating a Backtrace with GDB](<Creating_a_Backtrace_with_GDB.md> "Creating a Backtrace with GDB")

---

_Source: [https://wiki.freepascal.org/Creating_a_Backtrace_with_lldb](https://web.archive.org/web/20250512095911/https://wiki.freepascal.org/Creating_a_Backtrace_with_lldb)_
