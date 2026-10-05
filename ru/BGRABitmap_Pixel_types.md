# BGRABitmap Pixel types

│ **[English (en)](<../en/BGRABitmap_Pixel_types.md>)** │  **русский (ru)** │

Список типов пикселей и функций библиотеки [BGRABitmap](<../en/BGRABitmap.md> "BGRABitmap"). Они предоставляются модулем _BGRABitmapTypes_. 

### Типы пикселей и функции

_PBGRAPixel_ = ^TBGRAPixel;  
---  
| Указатель для прямого доступа к пикселям. Данные хранятся в виде последовательности _TBGRAPixel_. См. [BGRABitmap tutorial 4](<../en/BGRABitmap_tutorial_4.md> "BGRABitmap tutorial 4")  
_TBGRAPixel_ = **packed** **record** blue, green, red, alpha: byte;  
| Каждый пиксель представляет собой последовательность из 4 байтов, содержащую синий, зеленый, красный и альфа-канал. Диапазон значений от 0 до 255, цвет в цветовом пространстве sRGB. Альфа-значение: 0 - прозрачно, 255 - непрозрачно. В растровых данных, когда пиксель полностью прозрачен, значения RGB должны быть установлены на ноль.  
| **function** BGRA(red, green, blue, alpha: byte): TBGRAPixel; **overload** ; **inline** ;  
| | Создает пиксель с заданными значениями RGBA  
| **function** BGRA(red, green, blue: byte): TBGRAPixel; **overload** ; **inline** ;  
| | Создает непрозрачный пиксель с заданными значениями RGB  
| **operator** = (**const** c1, c2: TBGRAPixel): boolean; **inline** ;  
| | Проверяет, равны ли два пикселя. Если они оба прозрачны, значения RGB игнорируются  
| **function** GetIntensity(c: TBGRAPixel): word; **inline** ;  
| | Возвращает интенсивность пикселя. Интенсивность - это максимальное значение, достигаемое любым компонентом  
| **function** SetIntensity(c: TBGRAPixel; intensity: word): TBGRAPixel;  
| | Устанавливает интенсивность пикселя  
| **function** GetLightness(c: TBGRAPixel): word;  
| | Возвращает свечение пикселя. Свечение - это воспринимаемая яркость, 0 - черный, а 65535 - белый.  
| **function** SetLightness(c: TBGRAPixel; lightness: word): TBGRAPixel;  
| | Возвращает свечение пикселя  
| **function** ApplyLightnessFast(color: TBGRAPixel; lightness: word): TBGRAPixel; **inline** ;  
| | Быстро устанавливает свечение, уменьшая в сторону черного, если _lightness_ меньше 32768, и уменьшая в сторону белого, если _lightness_ больше 32768  
| **function** ApplyIntensityFast(color: TBGRAPixel; lightness: longword): TBGRAPixel;  
| | Быстро устанавливает интенсивность, уменьшая в сторону черного, если _lightness_ меньше 32768, и умножая все составляющие, если _lightness_ больше 32768. В случае насыщения, она исчезает в сторону белого  
| **function** CombineLightness(lightness1,lightness2: Int32or64): Int32or64;  
| | Объединяет две яркости вместе. Значение 32768 является нейтральным. Результат может превысить 65535  
| **function** BGRAToGrayscale(c: TBGRAPixel): TBGRAPixel;  
| | Преобразует цвет в оттенки серого  
| **function** GrayscaleToBGRA(lightness: word): TBGRAPixel;  
| | Создает серый цвет с заданной _яркостью_  
| **function** MergeBGRA(c1, c2: TBGRAPixel): TBGRAPixel; **overload** ;  
| | Слияние двух цветов без гамма-коррекции  
| **function** MergeBGRA(c1: TBGRAPixel; weight1: integer; c2: TBGRAPixel; weight2: integer): TBGRAPixel; **overload** ;  
| | Слияние двух цветов без гамма-коррекции. _weight1_ и _weight2_ указывает на вес цветового барицентра  
| **function** MergeBGRAWithGammaCorrection(c1: TBGRAPixel; weight1: byte; c2: TBGRAPixel; weight2: byte): TBGRAPixel;  
| | Слияние двух цветов с гамма-коррекцией. _weight1_ и _weight2_ указывает на вес цветового барицентра  
| **function** ColorToBGRA(color: TColor): TBGRAPixel; **overload** ;  
| | Преобразует значение _TColor_ в непрозрачный пиксель  
| **function** ColorToBGRA(color: TColor; opacity: byte): TBGRAPixel; **overload** ;  
| | Преобразует значение _TColor_ в пиксель с заданной _непрозрачностью_  
| **function** BGRAToColor(c: TBGRAPixel): TColor;  
| | Преобразует пиксель в значение _TColor_ , отбрасывая значение альфа  
| **function** FPColorToBGRA(AValue: TFPColor): TBGRAPixel;  
| | Преобразует значение _TFPColor_ в пиксель. Обратите внимание, что даже если _TFPColor_ имеет 16-битные значения, они не считаются расширенной гаммой  
| **function** BGRAToFPColor(AValue: TBGRAPixel): TFPColor; **inline** ;  
| | Преобразует пиксель в _TFPColor_  
| **function** BGRAWordDiff(c1, c2: TBGRAPixel): word;  
| | Вычисляет разницу (с гамма-коррекцией) между двумя пикселями с учетом всех размеров, включая прозрачность. Результат варьирует от 0 до 65535  
| **function** BGRADiff(c1, c2: TBGRAPixel): byte;  
| | Вычисляет разницу (с гамма-коррекцией) между двумя пикселями с учетом всех размеров, включая прозрачность. Результат варьирует от 0 до 255  
_ArrayOfTBGRAPixel_ = **array** **of** TBGRAPixel;  
| Массив пикселей  
| **function** MergeBGRA(**const** colors: **array** **of** TBGRAPixel): TBGRAPixel; **overload** ;  
| | Объединяет данные цвета без гамма-коррекции  
_TExpandedPixel_ = **packed** **record** red, green, blue, alpha: word;  
| Хранит гамму расширенного цвета RGB. Диапазон значений от 0 до 65535  
| **function** GammaExpansion(c: TBGRAPixel): TExpandedPixel; **inline** ;  
| | Преобразует пиксель из sRGB в расширенную гамму RGB  
| **function** GammaCompression(**const** ec: TExpandedPixel): TBGRAPixel; **inline** ;  
| | Преобразует пиксель из гамма-развернутого RGB в sRGB  
| **function** GammaCompression(red,green,blue,alpha: word): TBGRAPixel; **inline** ;  
| | Преобразует пиксель из гамма-развернутого RGB в sRGB  
| **function** GetIntensity(**const** c: TExpandedPixel): word; **inline** ;  
| | Возвращает интенсивность гамма-расширенного пикселя. Интенсивность - это максимальное значение, достигаемое любым компонентом  
| **function** SetIntensity(**const** c: TExpandedPixel; intensity: word): TExpandedPixel;  
| | Устанавливает интенсивность гамма-расширенного пикселя  
| **function** GetLightness(**const** c: TExpandedPixel): word; **inline** ;  
| | Возвращает яркость гамма-расширенного пикселя. lightness - это воспринимаемая яркость, 0 - черный, а 65535 - белый.  
| **function** SetLightness(**const** c: TExpandedPixel; lightness: word): TExpandedPixel;  
| | Устанавливает яркость гамма-развернутого пикселя  
| **function** SetLightness(**const** c: TExpandedPixel; lightness: word; curLightness: word): TExpandedPixel;  
| | Устанавливает яркость расширенного гамма-пикселя при условии, что вы уже знаете текущее значение яркости _curlightness_. Это немного быстрее, чем предыдущая функция  
| **function** ColorImportance(ec: TExpandedPixel): word;  
| | Возвращает важность цвета. Это похоже на насыщенность в цветовом пространстве HSL, за исключением того, что это гамма-коррекция. Значение ноль обозначает черный/серый/белый, а значение 65535 обозначает яркий цвет  
| **function** MergeBGRA(ec1, ec2: TExpandedPixel): TExpandedPixel; **overload** ;  
| | Объединение двух расширенных гамма-пикселей (с учетом коррекции гаммы)  
| **function** ExpandedDiff(ec1, ec2: TExpandedPixel): word;  
| | Вычисляет разницу (с гамма-коррекцией) между двумя пикселями с учетом всех размеров, включая прозрачность. Результат варьирует от 0 до 65535  
_THSLAPixel_ = **packed** **record**  
| Цвет пикселя определяется в цветовом пространстве HSL. Диапазон значений от 0 до 65535  
| _hue_ : word;  
| | Оттенок пикселя. Экстремальные значения 0 и 65535 - красные  
| _saturation_ : word;  
| | Насыщенность цвета. 0 - серый, а 65535 - самый яркий цвет  
| _lightness_ : word;  
| | Яркость цвета. 0 черный, 32768 нормальный, 65535 белый  
| _alpha_ : word;  
| | Непрозрачность пикселя. 0 прозрачный и 65535 непрозрачный  
| **function** HSLA(hue, saturation, lightness, alpha: word): THSLAPixel; **overload** ; **inline** ;  
| | Создает пиксель с заданными значениями HSLA, где A обозначает альфа  
| **function** HSLA(hue, saturation, lightness: word): THSLAPixel; **overload** ; **inline** ;  
| | Создает непрозрачный пиксель с заданными значениями HSL  
| **function** BGRAToHSLA(c: TBGRAPixel): THSLAPixel;  
| | Преобразует пиксель из sRGB в цветовое пространство HSL  
| **function** ExpandedToHSLA(**const** ec: TExpandedPixel): THSLAPixel;  
| | Преобразует пиксель из гамма-развернутого RGB в цветовое пространство HSL  
| **function** HSLAToExpanded(**const** c: THSLAPixel): TExpandedPixel;  
| | Преобразует пиксель из гамма-развернутого RGB в цветовое пространство HSL  
| **function** HSLAToBGRA(**const** c: THSLAPixel): TBGRAPixel;  
| | Преобразует пиксель из цветового пространства HSL в sRGB  
| **function** HueDiff(h1, h2: word): word;  
| | Вычисляет разницу оттенков  
| **function** GetHue(ec: TExpandedPixel): word;  
| | Возвращает оттенок расширенного гамма-пикселя  
_TGSBAPixel_ = THSLAPixel;  
| Цвет пикселя определяется в исправленном цветовом пространстве HSL. G обозначает исправленный оттенок, а B обозначает фактическую яркость. Диапазон значений от 0 до 65535  
| **function** BGRAToGSBA(c: TBGRAPixel): TGSBAPixel;  
| | Преобразует пиксель из sRGB в корректное цветовое пространство HSL  
| **function** ExpandedToGSBA(ec: TExpandedPixel): TGSBAPixel;  
| | Преобразует пиксель из гамма-развернутого RGB в корректное цветовое пространство HSL  
| **function** GtoH(ghue: word): word;  
| | Преобразует оттенок G (GSBA) в оттенок H (HSLA)  
| **function** HtoG(hue: word): word;  
| | Преобразует оттенок H (HSLA) в оттенок G (GSBA)  
| **function** GSBAToBGRA(c: TGSBAPixel): TBGRAPixel;  
| | Преобразует пиксель из исправленного HSL в sRGB  
| **function** GSBAToExpanded(c: TGSBAPixel): TExpandedPixel;  
| | Преобразует пиксель из корректного HSL в расширенный гамма RGB  
| **function** GSBAToHSLA(c: TGSBAPixel): THSLAPixel;  
| | Преобразует пиксель из корректного HSL в обычный HSL  
_TColorF_ = **packed** **array**[1..4] **of** single;  
| Цветовая переменная общего назначения со значениями с плавающей точкой одинарной точности  
| **function** ColorF(red,green,blue,alpha: single): TColorF;  
| | Создает структуру TColorF  
| **operator** \- (**const** c1, c2: TColorF): TColorF; **inline** ;  
| | Вычитает каждый компонент отдельно  
| **operator** \+ (**const** c1, c2: TColorF): TColorF; **inline** ;  
| | Добавляет каждый компонент отдельно  
| **operator** * (**const** c1, c2: TColorF): TColorF; **inline** ;  
| | Умножает каждый компонент отдельно  
| **operator** * (**const** c1: TColorF; factor: single): TColorF; **inline** ;  
| | Умножает каждый компонент на _factor_  
_TDrawMode_ = (  
| Возможные режимы при рисовании пикселя поверх другого  
| _dmSet_ ,  
| | Пиксель заменяется  
| _dmSetExceptTransparent_ ,  
| | Пиксель заменяется, если пиксель имеет значение альфа 255  
| _dmLinearBlend_ ,  
| | Пиксель смешивается с другим в соответствии с альфа-значениями, однако гамма-коррекция не применяется. Другими словами, цветовое пространство предполагается линейным  
| _dmDrawWithTransparency_ ,  
| | Пиксель смешивается с другим в соответствии с альфа-значениями, и применяется гамма-коррекция. Другими словами, цветовое пространство предполагается sRGB  
| _dmXor_);  
| | Значения всех каналов объединяются [посредством] Xor. Это полезно для вычисления двоичной разницы, однако не имеет смысла отображать на экране  
| _dmFastBlend_ = dmLinearBlend;  
| | Псевдоним для линейного наложения, потому что он быстрее, чем наложение с гамма-коррекцией  
_TBlendOperation_ = (  
| Расширенные режимы наложения. См. [Paint.NET blend modes](<http://www.brighthub.com/multimedia/photography/articles/18301.aspx>) и [Formulas](<http://www.pegtop.net/delphi/articles/blendmodes/>). Смешивание слоев имеет два этапа. Первый - применяются операции смешивания, перечисленные ниже, а второй - фактическое слияние цветов.  
| _boLinearBlend_ ,  
| | Простое смешивание, за исключением того, что оно вызывает линейное объединение, поэтому оно эквивалентно _dmLinearBlend_  
| _boTransparent_ ,  
| | Простое смешивание. Оно эквивалентно _dmLinearBlend_ или _dmDrawWithTransparency_  
| _boLighten_ , boScreen, boAdditive, boLinearAdd, boColorDodge, boDivide, boNiceGlow, boSoftLight, boHardLight,  
| | Режимы смешивания яркости (имеют тенденцию увеличивать свечение)  
| _boGlow_ , boReflect, boOverlay, boDarkOverlay, boDarken, boMultiply, boColorBurn,  
| | Маскировка режимов наложения (имеет тенденцию уменьшать свечение)  
| _boDifference_ , boLinearDifference, boExclusion, boLinearExclusion, boSubtract, boLinearSubtract, boSubtractInverse, boLinearSubtractInverse,  
| | Режимы смешивания различий  
| _boNegation_ , boLinearNegation,  
| | Режимы наложения негатива  
| _boXor_);  
| | Режим смешивания Xor. Он немного отличается от _dm Xor_ , поскольку альфа-значение используется, как и в других режимах наложения.  
| _boGlowMask_ = boGlow;  
| | Псевдоним свечения, который выражает, что этот режим смешивания маскирует часть, где верхний слой черный  
| _boLinearMultiply_ = boMultiply;  
| | Псевдоним, потому что линейные или нелинейные режимы умножения идентичны  
| _boNonLinearOverlay_ = boDarkOverlay;  
| | Псевдоним, чтобы выразить это темное наложение - это просто наложение с гамма-коррекцией  
| _BlendOperationStr_ : **array**[TBlendOperation] **of** **string**  
| | Строковые константы для режимов наложения  
| **function** StrToBlendOperation(str: **string**): TBlendOperation;  
| | Возвращает режим наложения, выраженный строкой  
_TChannel_ = (cRed, cGreen, cBlue, cAlpha);  
| Возможные каналы в растровом изображении с использованием любого цветового пространства RGBA  
| _TChannels_ = set **of** TChannel;  
| | Сочетание каналов  
_TAlphaChannelPaletteOption_ = (  
| Определяет, как палитра обрабатывает альфа-канал  
| _acIgnore_ ,  
| | Альфа-канал игнорируется. Считается, что альфа-канал хранится в другом месте.  
| _acTransparentEntry_ ,  
| | Одна точка входа выделяется полностью прозрачным цветом  
| _acFullChannelInPalette_);  
| | Альфа-канал полностью встроен в палитру, поэтому цвет определяется четырьмя каналами RGBA.  
_TDitheringAlgorithm_ = (  
| Алгоритмы сглаживания, которые определяют, как обрабатывать цвета, которые не найдены в палитре  
| _daNearestNeighbor_ ,  
| | Ближайший цвет должен использоваться вместо  
| _daFloydSteinberg_);  
| | Можно использовать ближайший цвет, но можно использовать и другой цвет для компенсации ошибки, следуя алгоритму Флойда-Стейнберга

---

_Source: [https://wiki.freepascal.org/BGRABitmap_Pixel_types/ru](https://web.archive.org/web/20250417204414/https://wiki.freepascal.org/BGRABitmap_Pixel_types/ru)_
