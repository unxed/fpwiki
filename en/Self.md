# Self

│ **[Deutsch (de)](</Self/de> "Self/de")** │  **English (en)** │  **[Esperanto (eo)](</Self/eo> "Self/eo")** │  **[suomi (fi)](</Self/fi> "Self/fi")** │    
****

` Self` is a [keyword](<Keyword.md> "Keyword") which can be used in instance [methods](<Method.md> "Method") to refer to the object on which the currently executing method has been invoked. [Reserved word](<Reserved_word.md> "Reserved word") `self` used to represent an instance of the [class](<Class.md> "Class") in which it appears. `Self` can be used to access class members and as a reference to the current instance. 

  

    
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      // Self stands for the TForm1 class in this example
      Self.Caption := 'Test program';
      Self.Visible := True;
    end;

---

_Source: [https://wiki.freepascal.org/Self](https://web.archive.org/web/20250121225524/https://wiki.freepascal.org/Self)_
