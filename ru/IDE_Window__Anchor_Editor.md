# IDE Window: Anchor Editor

│ **[English (en)](<../en/IDE_Window__Anchor_Editor.md>)** │  **русский (ru)** │

  
[![Anchor Editor en.png](https://wiki.freepascal.org/images/2/28/Anchor_Editor_en.png)](</File:Anchor_Editor_en.png>)

  
Редактор привязок позволяет редактировать свойства AnchorSide [(Сторона привязки)], Anchors [(якорь, привязка)] и BorderSpacing [(Зазор)] для выбранных элементов управления. 

На этой странице описывается, как работает редактор. Если вы хотите знать, как работают эти свойства, см. [Anchor Sides](<Anchor_Sides.md> "Anchor Sides/ru"). 

    Подсказка: это окно представляет собой плавающее окно, вы можете оставить его открытым при переключении между дизайнером и редактором привязки.

Флажки 'Enabled' соответствуют свойству Anchors элемента управления. Верхняя сторона использует перечисление akTop, левая сторона - перечисление akLeft и так далее. 

Sibling [(сородич)] - свойство AnchorSide[xxx].Control. Вы можете установить его в [значение] пусто (nil), [в значение] родителя элемента(ов) управления или одного из сородичей (элементы управления с одним и тем же родителем). 

Три SpeedButton'а соответствуют свойству AnchorSides[xxx].Side. 

У зазоров есть 5 свойств: Around[(вокруг)], Left[(слева)], Top[(сверху)], Right[(справа)] и Bottom[(снизу)]. Свойство Around является центральным полем редактирования. Top - это верхнее поле редактирования и т.д.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Anchor_Editor/ru](https://web.archive.org/web/20240803072520/https://wiki.freepascal.org/IDE_Window%3A_Anchor_Editor/ru)_
