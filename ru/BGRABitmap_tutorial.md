# BGRABitmap tutorial

│ **[Deutsch (de)](</BGRABitmap_tutorial/de> "BGRABitmap tutorial/de")** │  **[English (en)](<../en/BGRABitmap_tutorial.md> "BGRABitmap tutorial")** │  **[español (es)](</BGRABitmap_tutorial/es> "BGRABitmap tutorial/es")** │  **[français (fr)](</BGRABitmap_tutorial/fr> "BGRABitmap tutorial/fr")** │  **русский (ru)** │    
****

[ **Home**](<../en/BGRABitmap_tutorial.md> "BGRABitmap tutorial") | [ **Tutorial 1**](<../en/BGRABitmap_tutorial_1.md> "BGRABitmap tutorial 1") | [ **Tutorial 2**](<../en/BGRABitmap_tutorial_2.md> "BGRABitmap tutorial 2") | [ **Tutorial 3**](<../en/BGRABitmap_tutorial_3.md> "BGRABitmap tutorial 3") | [ **Tutorial 4**](<../en/BGRABitmap_tutorial_4.md> "BGRABitmap tutorial 4") | [ **Tutorial 5**](<../en/BGRABitmap_tutorial_5.md> "BGRABitmap tutorial 5") | [ **Tutorial 6**](<../en/BGRABitmap_tutorial_6.md> "BGRABitmap tutorial 6") | [ **Tutorial 7**](<../en/BGRABitmap_tutorial_7.md> "BGRABitmap tutorial 7") | [ **Tutorial 8**](<../en/BGRABitmap_tutorial_8.md> "BGRABitmap tutorial 8") | [ **Tutorial 9**](<../en/BGRABitmap_tutorial_9.md> "BGRABitmap tutorial 9") | [ **Tutorial 10**](<../en/BGRABitmap_tutorial_10.md> "BGRABitmap tutorial 10") | [ **Tutorial 11**](<../en/BGRABitmap_tutorial_11.md> "BGRABitmap tutorial 11") | [ **Tutorial 12**](<../en/BGRABitmap_tutorial_12.md> "BGRABitmap tutorial 12") | [ **Tutorial 13**](<../en/BGRABitmap_tutorial_13.md> "BGRABitmap tutorial 13") | [ **Tutorial 14**](<../en/BGRABitmap_tutorial_14.md> "BGRABitmap tutorial 14") | [ **Tutorial 15**](<../en/BGRABitmap_tutorial_15.md> "BGRABitmap tutorial 15") | [ **Tutorial 16**](<../en/BGRABitmap_tutorial_16.md> "BGRABitmap tutorial 16") | Edit

Добро пожаловать в набор уроков для библиотеки [BGRABitmap](<../en/BGRABitmap.md> "BGRABitmap"). Вы можете просматривать уроки по номеру с помощью панели сверху или по следующим категориям: 

## Contents

  * 1 Установка BGRABitmap и рисование основных фигур
  * 2 Текстуры и сканеры
  * 3 Другие возможности рисования
  * 4 More



### Установка BGRABitmap и рисование основных фигур

TBGRABitmap изображения имеют функции для рисования по целочисленным координатам или с плавающей запятой. 

  * [Установка BGRABitmap (No. 1)](<BGRABitmap_tutorial_1.md> "BGRABitmap tutorial 1/ru")
  * [Загрузка и отображение изображения (No. 2)](<BGRABitmap_tutorial_2.md> "BGRABitmap tutorial 2/ru")
  * [Рисование с помощью мыши (No. 3)](<../en/BGRABitmap_tutorial_3.md> "BGRABitmap tutorial 3")
  * [Стили линий (No. 6)](<../en/BGRABitmap_tutorial_6.md> "BGRABitmap tutorial 6")
  * [Сплайны и кривые Безье (No. 7)](<../en/BGRABitmap_tutorial_7.md> "BGRABitmap tutorial 7")
  * [Текстовые функции (No. 12)](<../en/BGRABitmap_tutorial_12.md> "BGRABitmap tutorial 12")
  * [Целочисленные координаты и с плавающей запятой (No. 13)](<../en/BGRABitmap_tutorial_13.md> "BGRABitmap tutorial 13")



### Текстуры и сканеры

Пиксели - это таблица в памяти, содержащая значения в формате TBGRAPixel. На этом уровне мы можем выполнять различные операции: 

  * [Осуществлять прямой доступ к пикселям с помощью свойства Scanline (No. 4)](<../en/BGRABitmap_tutorial_4.md> "BGRABitmap tutorial 4")
  * [Объединять слои пикселей (No. 5)](<../en/BGRABitmap_tutorial_5.md> "BGRABitmap tutorial 5")
  * [Создавать текстуры (No. 8)](<../en/BGRABitmap_tutorial_8.md> "BGRABitmap tutorial 8")
  * [Затенять по Фонгу с использованием текстур (No. 9)](<BGRABitmap_tutorial_9.md> "BGRABitmap tutorial 9/ru")
  * [Преобразовывать текстуры (No. 10)](<../en/BGRABitmap_tutorial_10.md> "BGRABitmap tutorial 10")
  * [Использовать сканеры для объединения преобразований (No. 11)](<../en/BGRABitmap_tutorial_11.md> "BGRABitmap tutorial 11")



### Другие возможности рисования

Больше возможностей можно получить, если использовать другие основные функции рисования: 

  * Стандартные свойства: Canvas и CanvasOpacity (избегайте использования их из-за медленного преобразования растровых данных);
  * Свойства Canvas с возможностями представленными в BGRABitmap (CanvasBGRA, Brush и Pen имеют в себе свойство Opacity (прозрачность)); 
    * [Как конвертировать ваше приложение из TCanvas в CanvasBGRA (видео)](<http://www.youtube.com/watch?v=HGYSLgtYx-U>)
  * [Рисование на 2D холсте с аффинными преобразованиями (No. 14)](<../en/BGRABitmap_tutorial_14.md> "BGRABitmap tutorial 14")
  * [Настоящий 3D рендеринг (No. 15)](<../en/BGRABitmap_tutorial_15.md> "BGRABitmap tutorial 15")
  * [Использование текстур в 3D рендеринге (No. 16)](<../en/BGRABitmap_tutorial_16.md> "BGRABitmap tutorial 16")



### More

You can use BGRABitmap to [improve TAChart rendering](<../en/BGRABitmap_tutorial_TAChart.md> "BGRABitmap tutorial TAChart"). 

More classes are available (you need to create them when you need them): 

  * TBGRATextEffect, in unit BGRATextFX, allows to prepare the drawing of text line, to add effects like contour and shadow.
  * TBGRALayeredBitmap, in unit BGRALayers, allow to create a multi-layered bitmap. Units BGRAPaintNet and BGRAOpenRaster contain implementations to read and write in Paint.NET format (read only) and OpenRaster format (read and write).
  * Units BGRAGradientScanner and BGRATransform contain scanners to do various effects.
  * Unit BGRAGradients contain procedures to generate gradients and TPhongShading class for Phong shading.
  * TBGRACompressableBitmap, in unit BGRACompressableBitmap, allow to store and compress images.



Other units contient low level functions, and you should not need to use them for a normal usage.

---

_Source: [https://wiki.freepascal.org/BGRABitmap_tutorial/ru](https://web.archive.org/web/20231201170430/https://wiki.freepascal.org/BGRABitmap_tutorial/ru)_
