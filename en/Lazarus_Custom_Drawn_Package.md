# Lazarus Custom Drawn Package

## Contents

  * 1 About
  * 2 Advantages
  * 3 Units
    * 3.1 customdrawncontrols.pas
    * 3.2 customdrawnextras.pas
  * 4 Maintainers
  * 5 See Also



## About

The package customdrawn.lpk is an under construction package containing all kinds of custom drawn components for Lazarus. Read more about it in [Lazarus Custom Drawn Controls](<Lazarus_Custom_Drawn_Controls.md> "Lazarus Custom Drawn Controls"). 

## Advantages

  * Can be fully reliable in look, behavior and size across platforms
  * Customizable look (skinnable in a reliable and cross-platform way)
  * Ability to mix native and custom drawn controls. This is major difference with with using [fpGUI] or the [fpGUI Interface]: it can't mix native and ownerdrawn (fpgui) components in one application.
  * They can be utilized in LCL interfaces if the native controls are too unreliable or if no native control exists or if the native controls cannot be used in a convinient way (because they are written in Java, for example)



## Units

### customdrawncontrols.pas

See [Lazarus Custom Drawn Controls](<Lazarus_Custom_Drawn_Controls.md> "Lazarus Custom Drawn Controls")

### customdrawnextras.pas

The control helps implementing an user interface which has buttons with an unusual look, such as is often found in games. The appearance of the button is defined by a set of raster images which are displayed for each state of the button, such as normal, focused, down. 

Location of the code in Lazarus: [http://svn.freepascal.org/cgi-bin/viewvc.cgi/trunk/components/customdrawn/customdrawnextras.pas?view=markup&root=lazarus](<http://svn.freepascal.org/cgi-bin/viewvc.cgi/trunk/components/customdrawn/customdrawnextras.pas?view=markup&root=lazarus>)

Usage example: 
    
    
    uses customdrawnextras;
    
    var
      ActionButton: TBitmappedButton;
    begin
      ActionButton := TBitmappedButton.Create(Self);
      ActionButton.Options := [bboUseImageForSelection];
      ActionButton.TabStop := True;
      ActionButton.OnClick := HandleActionButtonClick;
      ActionButton.Tag := i;
      ActionButton.Font.Size := 12;
      ActionButton.Font.Name := 'Times New Roman';
      ActionButton.Font.Color := clWhite;
      ActionButton.Parent := Self;
      ActionButton.Visible := True;
      ActionButton.Height := GetSkins().GetButton(sbSmall).Height;
      ActionButton.Width := GetSkins().GetButton(sbSmall).Width;
      ActionButton.Left := lLeft;
      ActionButton.Top := lTop;
      ActionButton.ImageBtn.Assign(GetSkins().GetButton(sbSmall));
      ActionButton.ImageBtnFocused.Assign(GetSkins().GetButtonFocused(sbSmall));
      ActionButton.ImageBtnDown.Assign(GetSkins().GetButtonDown(sbSmall));
      ActionButton.Caption := FModel.ActionButtons[i].Caption;
    

## Maintainers

  * [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")
  * JiXian Yang



## See Also

  * [Lazarus Components Directory](<Lazarus_Components_Directory.md> "Lazarus Components Directory")

---

_Source: [https://wiki.freepascal.org/Lazarus_Custom_Drawn_Package](https://web.archive.org/web/20250115202026/https://wiki.freepascal.org/Lazarus_Custom_Drawn_Package)_
