# Anchor Sides

│ **[Deutsch (de)](</Anchor_Sides/de> "Anchor Sides/de")** │  **[English (en)](<../en/Anchor_Sides.md> "Anchor Sides")** │  **[français (fr)](</Anchor_Sides/fr> "Anchor Sides/fr")** │  **[日本語 (ja)](</Anchor_Sides/ja> "Anchor Sides/ja")** │  **русский (ru)** │    
****

Есть несколько новых свойств и методов для автоматического расположения элементов управления. Теперь вы можете настроить элементы управления для поддержания определённого расстояния до других элементов управления или центрирования относительно других элементов управления. Смотрите пример ниже. 

Каждая из четырёх сторон элемента управления (Left, Top, Right, Bottom) теперь может быть прикреплена/привязана к стороне другого элемента управления. Например, теперь вы можете привязать левую сторону TEdit к правой стороне TLabel. Каждый раз, когда Label перемещается или меняет размер, левая сторона Edit будет следовать за ней, что обычно приводит к перемещению Edit параллельно Label. 

## Contents

  * 1 Пример 1
    * 1.1 В коде
    * 1.2 Примечания
    * 1.3 С помощью редактора якорей
  * 2 Пример 2
  * 3 Пример 3
  * 4 Новое свойство
  * 5 Новые методы для лёгкого конфигурирования общего расположения
  * 6 Привязка к невидимым элементам управления
  * 7 Циклические ссылки
  * 8 См. также



# Пример 1
    
    
    +--------+ +-------+
    | Label1 | | Edit1 |
    +--------+ |       |
               +-------+
    

## В коде
    
    
    Edit1.AnchorSide[akLeft].Side := asrRight;
    Edit1.AnchorSide[akLeft].Control := Label1;
    Edit1.Anchors := Edit1.Anchors + [akLeft];
    

Вы можете задать расстояние с помощью свойств BorderSpacing: 
    
    
    Edit1.BorderSpacing.Left := 10;
    

То-же самое может быть сделано с помощью метода: 
    
    
    Edit1.AnchorToNeighbour(akLeft, 10, Label1);
    

## Примечания

**Edit1.Left** будет следовать за **Label1.Left+Label1.Width** , но не наоборот. Это означает, что перемещение Label1 будет вызывать перемещение Edit1. Но перемещение Edit1 будет отменяться LCL. Если вы также привяжете правую сторону Label1 к левой стороне Edit1, вы создадите кольцо и в сочетании с некоторыми другими свойствами авторазмера это приведёт к циклу. Это игнорируется LCL или автоматически исправляется, когда Parent.AutoSize установлено в True. 

## С помощью редактора якорей

Редактор якорей - плавающее окно, доступное через меню **View / Anchor Editor** или через кнопку в свойстве **Anchors** в инспекторе объектов. 

[![Anchoreditor.png](https://wiki.freepascal.org/images/6/6d/Anchoreditor.png)](</File:Anchoreditor.png>)

# Пример 2

Вы можете привязать верхний край Edit для следования за верхним краем Label: 
    
    
    +--------+ +-------+
    | Label1 | | Edit1 |
    +--------+ |       |
               +-------+
    
    
    
    Edit1.AnchorSide[akTop].Side := asrTop;
    Edit1.AnchorSide[akTop].Control := Label1;
    Edit1.Anchors := Edit1.Anchors + [akTop];
    

То-же самое может быть сделано с помощью метода: 
    
    
    Edit1.AnchorParallel(akTop,0,Label1);
    

# Пример 3

Вертикальное центрирование Edit относительно Label: 
    
    
               +-------+
    +--------+ |       |
    | Label1 | | Edit1 |
    +--------+ |       |
               +-------+
    
    
    
    Edit1.AnchorSide[akTop].Side := asrCenter;
    Edit1.AnchorSide[akTop].Control := Label1;
    Edit1.Anchors := Edit1.Anchors + [akTop] - [akBottom];
    

То-же самое может быть сделано с помощью метода: 
    
    
    Edit1.AnchorVerticalCenterTo(Label1);
    

Очевидно, что привязка нижнего края Edit1 не имеет смысла при центрировании. 

# Новое свойство
    
    
    property AnchorSide[Kind: TAnchorKind]: TAnchorSide read GetAnchorSide;
    

Это свойство не опубликовано в [инспекторе объектов](<../en/IDE_Window__Object_Inspector.md> "IDE Window: Object Inspector"). Вы можете редактировать его в дизайнере с помощью [редактора якорей](<../en/IDE_Window__Anchor_Editor.md> "IDE Window: Anchor Editor")  
(меню **View - > View anchor editor** или щелчок на кнопке свойства **Anchors**). 

# Новые методы для лёгкого конфигурирования общего расположения
    
    
    procedure AnchorToNeighbour(Side: TAnchorKind; Space: integer;
                                Sibling: TControl);
    procedure AnchorParallel(Side: TAnchorKind; Space: integer;
                             Sibling: TControl);
    procedure AnchorHorizontalCenterTo(Sibling: TControl);
    procedure AnchorVerticalCenterTo(Sibling: TControl);
    procedure AnchorAsAlign(TheAlign: TAlign; Space: Integer);
    

**AnchorVerticalCenterTo** работает также с **Parent**. Тогда центрирование будет происходить по клиентской области, это означает, что центр элемента управления будет в _ClientHeight div 2_. 

Привязка по центру пока не полностью поддерживается при вычислении размера родителя. Например, когда вы помещаете привязанную по центру метку в Groupbox1 и устанавливаете Groupbox1.AutoSize в True, то высота Groupbox1 будет уменьшаться, не оставляя места для метки. Решение состоит в центрировании по элементу, который не центрирован. Например, центрируйте Label1 по ComboBox и используйте для ComboBox якоря по умолчанию (Anchors=[akLeft,akTop]). 

# Привязка к невидимым элементам управления

Начиная с 0.9.25 rev 12800 привязка к невидимым элементам управления была изменена, чтобы работать более интуитивно. Например, ниже элементы управления A, B, C привязаны (C.Left к B.Right и B.Left к A.Right): 
    
    
     +---+ +---+ +---+
     | A | | B | | C |
     +---+ +---+ +---+
    

Если B скрыт (Visible:=false), то C.Left будет пропускать B и использовать правый край A, в результате получим следующее: 
    
    
     +---+ +---+
     | A | | C |
     +---+ +---+
    

# Циклические ссылки

Вы можете создать циклические ссылки, привязав два края друг к другу, что создаёт невозможную привязку. LCL замечает это, но не генерирует исключения, поскольку этот цикл может быть времнным. Например, ряд кнопок: Button1 слева от Button2, Button2 слева от Button3. Теперь меняем порядок на следующий: Button3, Button1, Button2. Если перепривязка начата с Button3, временно создаётся цикл, который исправляется по завершению переупорядочивания. LCL обнаруживает цикл и не перемещает Button. Циклы - не единственная аномалия. 

Есть одно исключение из этого правила. Если **Parent.AutoSize** установлено в True, то LCL будет автоматически разрывать циклы на autosize и писать предупреждение через **debugln** (windows: --debug-log.txt, linux, et al: stdout). Которая из привязок будет нарушена, зависит от порядка элементов управления и сторон. Таким образом, временные циклы остаются разрешёнными при AutoSize=true, если вы заключите изменение в: 
    
    
    Parent.DisableAutosizing;
    try
      // change anchors, aligns, bounds...
    finally
      Parent.EnableAutosizing;
    end;
    

Алгоритм AutoSize не только разрушает циклы, но также исправляет несоответствия Align/AnchorSide. 

# См. также

  * [Autosize / Layout](<../en/Autosize_/_Layout.md> "Autosize / Layout")
  * [Example: Anchors. How to reliably align dynamically created controls under changing visibility](<../en/Example__Anchors.md> "Example: Anchors. How to reliably align dynamically created controls under changing visibility")

---

_Source: [https://wiki.freepascal.org/Anchor_Sides/ru](https://web.archive.org/web/20250213002206/https://wiki.freepascal.org/Anchor_Sides/ru)_
