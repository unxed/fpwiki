# Using Lazarus components

│ **English (en)** │    
****

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** Tutorial under construction

## Contents

  * 1 What is a component?
  * 2 Components vs Controls
  * 3 Lazarus components
  * 4 Component properties
  * 5 Component events
  * 6 See also



# What is a component?

A component is a [Class](<Class.md> "Class") \- a module of code - typically comprising a data definition and a number of methods, which defines and describes a particular action or series of actions. Examples of components include buttons, labels, checkboxes, timers and dialogs. 

# Components vs Controls

While every **control** is a **component** , not every component is a control. Confused? Let's clear up the confusion: a control is a visual component that a user of your application can see and can interact with (control) using the keyboard and/or the mouse. For example, a [TButton](<TButton.md> "TButton") which you drop on a [TForm](<TForm.md> "TForm") is a visible component that a user can see and interact with using the keyboard and/or mouse. Visual components are typically user interface elements like buttons, labels, check boxes, radio buttons and dialogs. 

On the other hand, there are components which have no visual control associated with them. For example, a [TTimer](<TTimer.md> "TTimer") which you drop on a TForm is a non-visual component. While you can see the TTimer at design time when you drop its icon on a form, there is nothing for the user to see or interact with at runtime. 

# Lazarus components

Lazarus comes with a number of useful components on its [Component Palette](<Component_Palette.md> "Component Palette"). A list of the components can be found [here](<Lazarus_Components_Directory.md> "Lazarus Components Directory"). There are also additional components which can be downloaded separately from the [Lazarus Code and Component Repository](<Components_and_Code_examples.md> "Components and Code examples") and installed. 

There is also the Lazarus [Online Package Manager](<Online_Package_Manager.md> "Online Package Manager") which automates the downloading, installing and configuring of packages. [Lazarus Packages](<Lazarus_Packages.md> "Lazarus Packages") are collections of units and components, containing information about how they can be compiled and how they can be used by projects or other packages or the IDE itself. The Online Package Manager can be found in the Lazarus IDE in the Package Menu. 

# Component properties

Component properties determine how the component appears and how it behaves. At design time, most properties have a sensible default, but you can alter them using the [Object Inspector](<IDE_Window__Object_Inspector.md> "IDE Window: Object Inspector"). You will most frequently need to edit the `Caption` and `Name` of components (eg forms, buttons and labels). When you edit the property in the Object Inspector, the visual appearance of the form or other component will be automatically updated by Lazarus. 

# Component events

t/c 

# See also

  * [How to write a Lazarus component](<How_To_Write_Lazarus_Component.md> "How To Write Lazarus Component") \- a tutorial describing how to create a new component.
  * [Adding an About dialog as a property to a custom component](<Adding_an_About_dialog_as_a_property_to_a_custom_component.md> "Adding an About dialog as a property to a custom component") \- a tutorial for writers of Lazarus visual components.

---

_Source: [https://wiki.freepascal.org/Using_Lazarus_components](https://web.archive.org/web/20240121085248/https://wiki.freepascal.org/Using_Lazarus_components)_
