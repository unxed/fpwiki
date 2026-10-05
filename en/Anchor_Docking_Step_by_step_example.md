# Anchor Docking Step by step example

│ **English (en)** │  **[русский (ru)](<../ru/Anchor_Docking_Step_by_step_example.md> "Anchor Docking Step by step example/ru")** │ 

## Contents

  * 1 Anchor Docking : A step by step example
  * 2 Step 1: Create the main page
  * 3 Step 2: Create our tool bar
  * 4 Step 3: Our tools pages
  * 5 Step 4: Our Message window
  * 6 Step 5: Putting it all together
  * 7 Debugging



# Anchor Docking : A step by step example

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** THIS EXAMPLE DOES NOT WORK 100% as intended**see Debugging for more info.**

We have [ anchor docking](<Anchor_Docking.md> "Anchor Docking") now... but I know that humans learn by doing, rather than reading... so, lets do it. 

First, I'll be creating a new application from scratch, and then (hopefully) we can see if we can "convert" an existing application to it (one that worked on the old "Panel" align way). 

For the first project, I'll be creating an application that should look like this: 
    
    
     +-----------------------------+
     | Main menu                   |
     +-----------------------------+
     | Toolbar (open, save etc.)   |
     +-----+-----------------------+
     | __= |                       |
     |     |                       |
     |  T  |   Main Application    |
     |  o  |                       | 
     |  o  |                       |
     |  l  +-----------------------+
     |  s  |   Output Messages     |
     +-----+-----------------------+
    

With the Toolbar, Tools (multiple pages), Output Messages parts all being "dockable". Because "drag" and "drop" isn't implemented yet, I'm not going to try to add functionality to move the parts around. (Compiling the example shows how this is possible)... This project will only be able to say "docked" or "not docked" per each of those parts. (This project would be able to be done with only panels.) 

# Step 1: Create the main page

In essence, we'll just be creating a new form, adding a main menu to the form, and then adding the dockmanager to the form. I also add a stringgrid just to show what it will look like. 

  * Create a new project
  * In Lazarus go to File -> New ... -> Select "Application"
  * The first form (unit1.pas) will be the "Main Application" part. 
    * Drop a TMainMenu component onto the form 
      * Double click on the TMainMenu component, and create: 
        * Main - File
        * FileSub - Exit
        * Main - Windows
        * WindowsSub - Dock/Undock Active Window (add a shortcut to F11 - so that we can have )
        * WindowsSub - Save Window Positions
    * Create an "OnCreate" event for the Form 
      * Select the form in the form designer
      * In the object inspector, click the "events" tab, and double click the "OnCreate" event
    * Create the docking manager 
      * Add this code to the OnCreate event: `**DockingManager:=TLazDockingManager.Create(Self);** `
      * Add the dockmanager variable to the main form's class. 
        * Press Ctrl-Shift-Up (this will take you to the OnCreate event's definition in the form's class
        * Add this code to the Form's **public** class definition : `**DockingManager : TLazDockingManager;** `
    * Add the Docking unit and the XMLPropStorage to the uses section, 
      * Scroll up to the uses section
      * Add `**, LDockCtrl, XMLPropStorage** ` to the uses section
    * Now we'll just add stuff to make it look like an application 
      * Add a StringGrid component 
        * Make **Align = Client** in the object inspector



# Step 2: Create our tool bar

I'm not going to pretend that I know how to create a proper toolbar... ala excel or even open office's stuff. So I'll just create a form with speed buttons that are all left aligned, and add some icons to it. Also add the dock control to it. 

  * Create a new form (File -> New Form) 
    * Resize it to look "realistic" in size. Mine's height is 61.
    * Add some load/save buttons to it (add some icons too, if you like) and **Align = Left** them. I added a Load and Save speedbuttons, got some glyphs from Lazarus/images/menu. In my other project I've added a new panel, and put in a combobox there. Do what you want!
    * Add the control docker 
      * On the OnCreate event add:


    
    
         ControlDocker1:=TLazControlDocker.Create(Self);
         ControlDocker1.Name:='ToolbarForm';
         ControlDocker1.Manager:=Form1.DockingManager;
    

  *     * Add the appropriate units to the uses part. (**unit1, LDockCtrl**)
    * Add the variable to the Form's **public** class definition: **ControlDocker1 : TLazControlDocker;**



# Step 3: Our tools pages

We'll just create 2 different forms which are _identifiably_ different from each other. And add the dock control to it. Do the following twice: 

  * Create a new form (File -> New Form) 
    * Resize it to look "realistic" in size. (I just made it about 150 wide)
    * Identify it. (I put a label on, saying tool window 1 and 2)
    * Add the control docker 
      * On the OnCreate event add:


    
    
     
         ControlDocker1:=TLazControlDocker.Create(Self);
         ControlDocker1.Name:='Toolwindow1Form';  // (or 2 for the second one)
         ControlDocker1.Manager:=Form1.DockingManager;
    

  *     *       * Add the appropriate units to the uses part. (**unit1, LDockCtrl**)
      * Add the variable to the Form's **public** class definition: **ControlDocker1 : TLazControlDocker;**



# Step 4: Our Message window

Just our last form... The output form. Follow the same steps as before... 

  * Create a new form (File -> New Form) 
    * Resize it to look "realistic" in size. (I just made it about 100 high, and quite wide)
    * Add a memo component, and **align = client**
    * Add the control docker 
      * On the OnCreate event add:


    
    
         ControlDocker1:=TLazControlDocker.Create(Self);
         ControlDocker1.Name:='OutputForm';  // (or 2 for the second one)
         ControlDocker1.Manager:=Form1.DockingManager;
    

  *     *       * Add the appropriate units to the uses part. (**unit1, LDockCtrl**)
      * Add the variable to the Form's **public** class definition: **ControlDocker1 : TLazControlDocker;**



# Step 5: Putting it all together

Now we'll just add everything together on the mainform... and see how it comes out. 

  * Add all the units to the uses section in your main unit. (**unit2, unit3, unit4, unit5**
  * Next, we'll want to add it to the docking manager... but the problem is that the forms are not created yet at the "OnCreate" event for the main form. So, I'll move the "assigning" of the docking to the "OnActivate" event, and just set a flag that it's been done (so it doesn't do it again)
  * So: Add an Initialized variable to form1's class: **Initialized : boolean;**
  * At OnCreate we'll set it to "False", and then set it to true during the OnActivate Method.
  * The current "OnActivate" method.


    
    
    procedure TForm1.FormActivate(Sender: TObject);
    begin
      if initialized then exit;
      Initialized := true;
      // Sets the toolbaar to the top
      DockingManager.Manager.InsertControl(Form2,alTop,Form1);
      
      // Set the tool window to the left
      DockingManager.Manager.InsertControl(Form3,alLeft,Form1);
      
      // Set the second tool window IN the first
      DockingManager.Manager.InsertControl(Form4,alClient,Form3);
      
      // Set the second Message Control to the bottom
      DockingManager.Manager.InsertControl(Form5,alBottom,Form1);
    end;
    

  


# Debugging

This did not work 100% as expected. 

Stuff that worked as expected: 

  1. The Toolbar looked correct and awesome
  2. The Tool Windows' width was as expected



Stuff that didn't work as expected: 

  1. The Second tool window didn't work properly, there was a page control, but the second page wasn't assigned, and the first page had the SECOND tool window assigned
  2. The Main Menu was missing
  3. The Memo was gone... though Form5 was there (maybe not? the "title" didn't read "Form5")
  4. Form5 took over all the space that was meant for the main application. Probably adding constraints to the main window would help.

---

_Source: [https://wiki.freepascal.org/Anchor_Docking_Step_by_step_example](https://web.archive.org/web/20240910053131/https://wiki.freepascal.org/Anchor_Docking_Step_by_step_example)_
