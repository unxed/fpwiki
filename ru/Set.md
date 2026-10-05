# Set

│ **[Deutsch (de)](</Set/de> "Set/de")** │  **[English (en)](<../en/Set.md> "Set")** │  **[suomi (fi)](</Set/fi> "Set/fi")** │  **[français (fr)](</Set/fr> "Set/fr")** │  **русский (ru)** │    
****

## Введение

**Set** представляет собой множество элементов из перечисленного набора значений и является порядковым типом. 

В качестве примера рассмотрим следующее перечисление: 
    
    
      TSpeed = (spVerySlow,spSlow,spAVerage,spFast,spVeryFast);
    

И такое множество: 
    
    
      TPossibleSpeeds = set of TSpeed
    

Значения множества TPossibleSpeeds могут быть определены с помощью скобок для перечисления элементов: 
    
    
      const
        RatherSlow = [spVerySlow,spSlow];
        RatherFast = [spFast,spVeryFast];
    

_RatherSlow_ и _RatherFast_ представляют некоторое множество из _TSpeed_. 

## Операции над множествами

В [модуле System](</index.php?title=System_unit/ru&action=edit&redlink=1> "System unit/ru \(page does not exist\)") библиотеки [RTL](<RTL.md> "RTL/ru") определены две функции, использующиеся для операций над множествами: [Include](</index.php?title=Include/ru&action=edit&redlink=1> "Include/ru \(page does not exist\)")(ASet,AValue) и [Exclude](</index.php?title=Exclude/ru&action=edit&redlink=1> "Exclude/ru \(page does not exist\)")(ASet,AValue). 
    
    
      var
        SomeSpeeds: TPossibleSpeeds;
      begin
        SomeSpeeds := [];
        Include(SomeSpeeds,spVerySlow);
        Include(SomeSpeeds,spVeryFast);
      end;
    

Множествами нельзя управлять напрямую, если они находятся в разделе _published_. Обычно в этом случае вам необходимо создать локальную копию, изменить её значение и затем вызвать _setter_. 
    
    
      procedure TSomething.DoSomething(Sender: TFarObject);
      var
        LocalCopy: TPossibleSpeeds;
      begin
        LocalCopy := Sender.PossibleSpeeds; // getter to local
        Include(LocalCopy,spVerySlow);
        Sender.PossibleSpeeds := LocalCopy; // local to setter.
      end;
    

С помощью [ключевого слова](<Keyword.md> "Keyword/ru") **[in](<../en/In.md> "In")** можно также проверить, принадлежит ли значение множеству. Обычно оно используется следующим образом: 
    
    
      var
        CanBeSlow: Boolean;
      const
        SomeSpeeds = [Low(TSpeed)..High(TSpeed)];
      begin
        CanBeSlow := (spVerySlow in SomeSpeeds) or (spSlow in SomeSpeeds);
      end;
    

## Битовые маски

Множества могут использоваться для создания битовых масок, как показано в следующем примере. 
    
    
    (*
      FLAG_A = 1;  // 1 shl 0
      FLAG_B = 2;  // 1 shl 1
      FLAG_C = 4;  // 1 shl 2 
    *)
    
    type
      TFlag = (FLAG_A, FLAG_B, FLAG_C);
      TFlags = set of TFlag;
    
    var
      Flags: TFlags;
    
    [..]
      Flags:= [FLAG_A, FLAG_C];
      if FLAG_A in Flags then ..  // проверяет, установлен ли FLAG_A в переменной Flags
    

Типы данных   
---  
Простые типы  | [Boolean](<Boolean.md> "Boolean/ru") | [Byte](<Byte.md> "Byte/ru") | [Cardinal](<Cardinal.md> "Cardinal/ru") | [Char](<Char.md> "Char/ru") | [Currency](<Currency.md> "Currency/ru") | [Extended](<Extended.md> "Extended/ru") | [Int64](<Int64.md> "Int64/ru") | [Integer](<Integer.md> "Integer/ru") | [Longint](<Longint.md> "Longint/ru") | [Pointer](<Pointer.md> "Pointer/ru") | [Real](<Real.md> "Real/ru") | [Shortint](<Shortint.md> "Shortint/ru") | [Smallint](<Smallint.md> "Smallint/ru") | [Word](<Word.md> "Word/ru")  
Сложные типы  | [Array](<Array.md> "Array/ru") | [Class](<Class.md> "Class/ru") | [Record](<Record.md> "Record/ru") | Set | [String](<String.md> "String/ru") | [Shortstring](</index.php?title=Shortstring/ru&action=edit&redlink=1> "Shortstring/ru \(page does not exist\)")  
  
  
****

---

_Source: [https://wiki.freepascal.org/Set/ru](https://web.archive.org/web/20241125165136/https://wiki.freepascal.org/Set/ru)_
