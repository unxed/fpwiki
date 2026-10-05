# PascalMagick

│ **[English (en)](<../en/PascalMagick.md>)** │  **русский (ru)** │

## Contents

  * 1 ImageMagick
  * 2 MagickWand
  * 3 MagickCore
  * 4 PascalMagick
  * 5 Авторы
  * 6 Лицензия
  * 7 Загрузка
  * 8 Установка
  * 9 Демонстрационная программа
  * 10 Subversion
  * 11 Помощь и справка



## ImageMagick

[ImageMagick](<http://www.imagemagick.org/>) -это бесплатный программный пакет разработанный для создания, редактирования и комбинирования растровых изображений. Он поддерживает огромное количество форматов (более 90) включая такие как GIF, JPEG, JPEG-2000, PNG, PDF, PhotoCD, TIFF, DPX и т.д. Изображения можно обрезать, красить, обрабатывать с использованием некоторых эффектов, вращать, растягивать, комбинировать, добавлять текст, линии, полигоны, эллипсы и кривые Безье. 

Пакет запускается под управлением всех основных операционных систем, может работать в консольном режиме (через командную строку), и входит в состав большинства дистрибутивов Linux. 

Особенности и возможности: 

  * Преобразование изображений из одного формата в другой (например PNG в JPEG)
  * Масштабирование, вращение, повышение четкости, изменение насыщенности цвета, или добавление специальных эффектов к изображению
  * Выполнение монтажа миниатюрных эскизов изображений
  * Создание изображений, содержащих прозрачность для использования в интернете
  * Преобразование множества изображений в анимированный GIF
  * Создание комбинированного из нескольких отдельных изображений
  * Рисование простых форм или текста на изображении
  * Украшение изображений рамками или бордюрами
  * Формирование списка характеристик и информации о формате изображения



## MagickWand

[MagickWand API](<http://www.imagemagick.org/script/magick-wand.php>) \- это простой в использовании, программный интерфейс для работы с пакетом, рекомендованный командой разработчиков ImageMagic. 

## MagickCore

MagickCore API - это низкоуровневый интерфейс для ImageMagic. 

## PascalMagick

Этот порт - трансляция заголовков с языка C (си) на язык Pascal для возможности работы с MagickWand и Magick Core. 

## Авторы

[Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")

[Ángel Eduardo García](</User:Arcnor> "User:Arcnor")

## Лицензия

BSD-совместимая с GPL. 

Полный текст [здесь](<http://www.imagemagick.org/script/license.php>). 

## Загрузка

PascalMagick 0.4 доступен по адресу: [http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=174103&release_id=431432](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=174103&release_id=431432>)

Статус: заголовки Magick Wand полностью работоспособны как в Linux так и в Windows. 

## Установка

Текущая версия PascalMagic работает на Linux и Windows. Для тестирования под Mac OS X требуются бета тестеры. 

Чтобы приступить к установке, скачайте и запустите бинарные файлы ImageMagic с официального сайта: <http://www.imagemagick.org/script/binary-releases.php>

Теперь скачайте и распакуйте [PascalMagic](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=174103&release_id=431432>) в каталог для компонентов Lazarus 

Для проверки работоспособности откройте проект PascalMagick/wand/wanddemo.lpi. Теперь в главном меню Lazarus нажмите: Проект -> Опции компилятора..., и измените значения полей «Другие модули» и «Другие исходники» на «..\magick\;..\wand\», для того чтобы при сборке компилятор нашел файл PascalMagick/magick/ImageMagick.pas. После этого откомпилируйте и запустите тестовую программу. 

Если все работает правильно, программа загрузит файл image.png (расположенный в том же каталоге), изменит его фактический размер и сохранит под именем image.jpg. 

Один важный момент для пользователей Linux: переименуйте файл image.PNG в image.png для предотвращения возможных проблем. 

## Демонстрационная программа
    
    
    { 
      Демонстрационная программа для библиотеки ImageMagic 
    
      Эта программа была конвертирована: Felipe Monteiro de Carvalho
    
      Использование: Просто запустите программу, она изменит размер файла image.png
      расположенного в том же каталоге (106, 80) и преобразует его в jpg.
      
      Dez/2005
    }
    
    {Версия 0.1}
    program wanddemo;
    
    {$mode objfpc}{$H+}
    
    uses SysUtils, magick_wand, ImageMagick;
    
    procedure ThrowWandException(wand: PMagickWand);
    var
      description: PChar;
      severity: ExceptionType;
    begin
      description := MagickGetException(wand, @severity);
      WriteLn(Format('An error ocurred. Description: %s', [description]));
      description := MagickRelinquishMemory(description);
      Abort;
    end;
    
    var
      status: MagickBooleanType;
      wand: PMagickWand;
    begin
      { Чтение изображения }
      
      MagickWandGenesis;
    
      wand := NewMagickWand;
      
      try
        status := MagickReadImage(wand, 'image.png');
        if (status = MagickFalse) then ThrowWandException(wand);
    
        { Преобразование изображений в последовательность миниатюр }
    
        MagickResetIterator(wand);
    
        while (MagickNextImage(wand) <> MagickFalse) do
         MagickResizeImage(wand, 106, 80, LanczosFilter, 1.0);
    
        { Запись изображения как MIFF и его уничтожение }
      
        status := MagickWriteImages(wand, 'image.jpg', MagickTrue);
        if (status = MagickFalse) then ThrowWandException(wand);
    
      finally
        wand := DestroyMagickWand(wand);
    
        MagickWandTerminus;
      end;
    end.
    

## Subversion

Вы можете загрузить SVN этого проекта используя команду: 

svn checkout <http://svn.freepascal.org/svn/fpc/trunk/packages/base/imagemagick> imagemagick 

## Помощь и справка

За дополнительной информацией обращайтесь на [форум](<http://www.lazarus.freepascal.org/modules.php?op=modload&name=PNphpBB2&file=index>) Lazarus.

---

_Source: [https://wiki.freepascal.org/PascalMagick/ru](https://web.archive.org/web/20240714063736/https://wiki.freepascal.org/PascalMagick/ru)_
