# IDE Window: Assembler

│ **English (en)** │

## Contents

  * 1 Navigation
  * 2 Important
  * 3 Assembler
    * 3.1 Data displayed
    * 3.2 Interface
    * 3.3 Using the same or different keys for Pascal and ASM stepping
    * 3.4 Improve speed using the power button
  * 4 Scope
  * 5 See also



## Navigation

[Main Menu](<Main_menu.md> "Main menu") > [View](<Main_menu.md> "Main menu") > Debug Windows > Assembler 

[CallStack Window](<IDE_Window__Call_Stack.md> "IDE Window: Call Stack") > Context menu > View Assembler 

## Important

You must [setup the debugger](<Debugger_Setup.md> "Debugger Setup") and start the project to debug it. Only then this window will be useful. 

## Assembler

[![Assembler.png](https://wiki.freepascal.org/images/a/ae/Assembler.png)](</File:Assembler.png>)

Shows an assembler view of the current code. This can be used with the [Registers](<IDE_Window__Registers.md> "IDE Window: Registers") window. 

### Data displayed

Address
    
Hex dump
    
Asm statement
    The output depends on your CPU

Pascal lines
    If Pascal sources are available, then they are shown in front of their ASM representation.

### Interface

_Vertical Srollbar_
    The vertical scrollbar behaves different from normal scrollbars. Since the assembly data may continue to an unknown extend in both directions, there is no fixed range. Move and hold the slider in either direction to scroll. The further you move the slider from the center position the faster the scroll. If released the slider returns to the center.

_Toolbar_

[![debugger power.png](https://wiki.freepascal.org/images/b/ba/debugger_power.png)](</File:debugger_power.png>) Power
    Switch off the disassembler. The current disassembled data is kept as is. If the current location changes, it will be indicated only within the data. Introduced to deal with slow data loading since fetching any data from gdb takes noticeable time. It offers a quick way to temporarily disable loading so that other debug windows may be able to display their data sooner.
[![menu stepinto instr.png](https://wiki.freepascal.org/images/7/7f/menu_stepinto_instr.png)](</File:menu_stepinto_instr.png>) Step over or into (one asm instruction)
    Single step (into) one asm instructions, instead of on entire pascal instructions. Will enter subroutines.
[![menu stepover instr.png](https://wiki.freepascal.org/images/6/6e/menu_stepover_instr.png)](</File:menu_stepover_instr.png>) Step over (one asm instruction)
    Single step (over )one asm instructions, instead of on entire pascal instructions. Will skip over subroutines.
[![debugger current line.png](https://wiki.freepascal.org/images/d/d5/debugger_current_line.png)](</File:debugger_current_line.png>) Return to current address
    Returns to the current execution-point.
[![callstack show.png](https://wiki.freepascal.org/images/c/c8/callstack_show.png)](</File:callstack_show.png>) Goto custom address
    Shows the data at the address entered in the input field.
[![laz copy.png](https://wiki.freepascal.org/images/c/c7/laz_copy.png)](</File:laz_copy.png>) Copy to clipboard
    Copies the selected line(s) to the clipboard.

_Context menu_

[![Assembler popup.png](https://wiki.freepascal.org/images/c/c7/Assembler_popup.png)](</File:Assembler_popup.png>)

Copies the selected line(s) to the clipboard.   


### Using the same or different keys for Pascal and ASM stepping

The default keyboard settings are: 

  * `F7` or `F8`: step into/over one pascal instruction
  * `Alt`-`F7` or `Alt`-`F8`: step into/over one asm instruction



Those keys will act, independent of the focused window. 

In the keyboard mapping (editor options) under "Run Menu" are also entries: 

  * `Alt`-`⇧ Shift`-`F7` or `Alt`-`⇧ Shift`-`F8`: "Step into,over (Context)



Those will step asm only if the assembler window is focused, and step pascal otherwise. 

The key mapping can be changed, for the unmodified F7/F8 to act on context. 

### Improve speed using the power button

The debugger will disassemble the required memory range after each step, or each run. This may require some time. 

During single stepping the next execution point often is in already disassembled data. If you code is not self-modifying, then you can use the power button to disable the repeated disassembling. Once you stepped to the border of the disassembled range, you need to turn power on to allow disassembling the next block of data. Once you have the next block of data turn it off again. And so on. 

## Scope

The assembler window does _not_ follow any scope. 

The Stack window (context menu) allows to jump to the address of any stackframe. This acts like jumping to a custom address, the [![debugger current line.png](https://wiki.freepascal.org/images/d/d5/debugger_current_line.png)](</File:debugger_current_line.png>) button will always return to the execution point at the top stack frame. 

## See also

  * [Registers](<IDE_Window__Registers.md> "IDE Window: Registers")

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Assembler](https://web.archive.org/web/20230322004152/https://wiki.freepascal.org/IDE_Window%3A_Assembler)_
