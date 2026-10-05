# LCL AutoSizing

│ **[English (en)](<../en/LCL_AutoSizing.md>)** │  **русский (ru)** │

## Contents

  * 1 Обзор / Терминология
  * 2 Свойства
  * 3 Порядок
    * 3.1 Обзор
    * 3.2 Алгоритм выравнивания и привязки
    * 3.3 DoAutoSize
    * 3.4 DisableAutoSizing/EnableAutosizing
    * 3.5 Отличия от Delphi
  * 4 FAQ



## Обзор / Терминология

На этой странице объясняется алгоритм автомасштабирования в LCL. AutoSizing означает здесь: автоматическое изменение размера и перемещение элементов управления LCL. 

Если вам просто интересно, как использовать свойства AutoSize, см. [Autosize/Layout](<Autosize_/_Layout.md> "Autosize / Layout/ru"). 

## Свойства

Следующие свойства определяют поведение автомасштабирования в LCL: 

  * Current Left,Top,Width,Height,ClientWidth,ClientHeight
  * Loaded Left,Top,Width,Height,ClientWidth,ClientHeight
  * AutoSize
  * Anchors
  * AnchorSides
  * Align
  * BorderSpacing
  * ChildSizing
  * Constraints



Для совместимости с Delphi LCL поддерживает метод AdjustClientRect, который является расширением свойств ChildSize.LeftRightSpacing/TopBottomSpacing. 

## Порядок

### Обзор

Автомасштабирование работает в несколько этапов: 

  * DisableAutosizing. На первом этапе приложение изменяет свойства, создает или удаляет элементы управления. Вся форма заблокирована. Отсутствует автоматическая настройка, никакие границы в виджет не отправляются .
  * EnableAutoSizing. Вызывается [методом] DoAllAutoSize родительской формы.
  * Все необходимые дескрипторы создаются рекурсивно [методом] TWinControl.DoAllAutoSize. Они еще не сделаны видимыми.
  * [Метод] TControl.DoAllAutoSize вычисляет границы, вызывая DoAutoSize для всех элементов управления. Он делает это в цикле, пока никакие границы не изменены.
  * Границы отправляются в виджет (TWinControl.RealizeBoundsRecursive вызывает RealizeBounds).
  * Дескрипторы становятся видимыми (если [свойство] HandleObjectShouldBeVisible [имеет значение True] и not Showing, то [вызывается] UpdateShowing).



В основном свойство AutoSizing работает в следующем порядке: 

  * Ограничения применяются к каждому шагу.
  * Сначала идет выравнивание в порядке alTop, alBottom, alLeft, alRight и, наконец, alClient. Например, alLeft имеет 3 выровненных стороны и одну свободную сторону. Свободная сторона обрабатывается приведенными ниже правилами. BorderSpacing применяется к выровненным сторонам. alCustom и alNone не имеют выровненных сторон.
  * Далее идет ChildSizing. Если ChildSizing.Layout не [в значении] cclNone, все дочерние элементы управления со [значениями] Align=alNone, Anchors=[akLeft,akTop] и элементы управления без AnchorSide масштабируются/репозиционируются. Эти дочерние элементы управления называются на этой странице "элементы управления без выравнивания".
  * Затем идет AutoSize. Если ChildSizing.Layout [имеет значение] cclNone, тогда все "элементы управления без выравнивания" перемещаются кверху и влево, а элемент управления сужается или расширяется, чтобы соответствовать всем его дочерним элементам (а не только "элементам управления без выравнивания").
  * Потом идут якоря (привязки). Только те стороны [считаются] привязанными, которые не обрабатываются [свойством] Align (например, akRight в alLeft). Если у элемента управления установлено [свойство] AnchorSide, сторона [этого контрола] будет выровнена к стороне другого элемента управления. В противном случае применяются правила привязки по умолчанию, которые в основном аналогичны Delphi.
  * Каждое изменение границ запускает событие OnResize/OnChangeBounds, которое может использоваться приложением для выполнения произвольных действий.



### Алгоритм выравнивания и привязки

Основным методом является TWinControl.AlignControls. 

  1. инициация RemainingClientRect в ClientRect и RemainingBorderSpace в Rect(0,0,0,0)
  2. вызов AdjustClient, чтобы настроить RemainingClientRect
  3. применение ChildSizing.LeftRightSpacing,ChildSizing.TopBottomSpacing
  4. вызов DoAlign с alTop,alBottom,alLeft,alRight,alClient,alCustom,alNone 
     1. DoAlign собирает дочерние элементы управления с этим значением Align и вызывает для каждого такого контрола DoPosition 
        1. DoPosition вычисляет новые Left,Top,Width,Height



### DoAutoSize

Этот [метод] вызывается [методом] DoAllAutoSize и не должен вызываться приложением. Приложения должны вызывать AdjustSize и/или InvalidatePreferredSize. 

Если он не переопределен, нормальный TWinControl.DoAutoSize делает следующее, когда AutoSize=true: 

Он будет перемещать "элементы управления без выравнивания" - дочерние элементы управления с привязками по умолчанию (Anchors=[akLeft,akTop],Align=alNone). Он переместит все их одинаковое количество, поэтому их общий ограничивающий прямоугольник растягивается влево и вверх. Рассматривается интервал между BorderSpacing и ChildSize.  


Перед перемещением есть место слева и над кнопками: 

[![Autosize before move childs.png](https://wiki.freepascal.org/images/a/ab/Autosize_before_move_childs.png)](</File:Autosize_before_move_childs.png>)

После перемещения: 

[![Autosize after move childs.png](https://wiki.freepascal.org/images/6/61/Autosize_after_move_childs.png)](</File:Autosize_after_move_childs.png>)

Затем вычисляется предпочтительный размер элемента управления. В этом расчете учитываются дочерние элементы управления, Align, Anchors, Constraints, ChildSizing.Layout и другие свойства LCL. 

Размер элемента управления изменен: 

[![Autosize after shrink.png](https://wiki.freepascal.org/images/3/33/Autosize_after_shrink.png)](</File:Autosize_after_shrink.png>)

  
См. также: [Выравнивание и свойство AutoSize родителя](<Autosize_/_Layout.md> "Autosize / Layout/ru")

### DisableAutoSizing/EnableAutosizing

см [здесь](<Autosize_/_Layout.md> "Autosize / Layout/ru"). 

### Отличия от Delphi

  * Delphi не имеет BorderSpacing, AnchorSides, ChildSizing.
  * Delphi не поддерживает AutoSize и Align на всех элементах управления.
  * Алгоритм автомасштабирования Delphi не поддерживает вложенные автомасштабируемые элементы управления со свойством Align. Например, Panel1 с Align=alTop в Panel2 и обе панели имеют AutoSize=true. Panel2 не будет уменьшаться/увеличиваться по горизонтали, что означает, что Panel1 также не будет уменьшаться/увеличиваться. Таким образом, Panel1 не имеет автомасштабирования для своих дочерних элементов.



## FAQ

см. [Autosize / Layout](<Autosize_/_Layout.md> "Autosize / Layout/ru").

---

_Source: [https://wiki.freepascal.org/LCL_AutoSizing/ru](https://web.archive.org/web/20250325120518/https://wiki.freepascal.org/LCL_AutoSizing/ru)_
