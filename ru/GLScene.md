# GLScene

│ **[English (en)](<../en/GLScene.md>)** │  **русский (ru)** │

## Contents

  * 1 О GLScene
  * 2 Снимики экрана (Screenshots)
  * 3 Автор
  * 4 Скачать
    * 4.1 Последняя версия (v1.0.0.2)
    * 4.2 Устаревшая версия
  * 5 SVN
  * 6 Отчет об ошибках
  * 7 Изменения
  * 8 Установка версии 1.0.0.2
  * 9 Установка версии 0.9.6.1
  * 10 Помощь



### О GLScene

GLScene это основанная на OpenGL 3D библиотека. Изначально была написана для Delphi, но теперь спокойно работает с C++ Builder, Kylix, Delphi и Lazarus. 

Основные характеристики : 

Описание сцены 

  * иерархическая структура объектов с множеством объектов (количество зависит от ОЗУ) и простотой расширения
  * интерактивное управление сценой (numerous Delphi experts)
  * простые функции вращения и перемещения для каждого объекта
  * предопределенные объекты (все стандартные от куба до чайника и спрайтов), легко расширяемые
  * структурные объекты (каркасный куб, прокси-объект) для управления композитными объектами
  * процедурные объекты (heightfield, вращение твердых тел, трубы, tesselators...)
  * поддержка 2D и 1D объектов (спрайты, линии с поддержкой сплайнов) полностью интегрирована
  * HUD-объекты (спрайты визуализированные в 2D координатах)
  * утилитные объекты (сетки, небо, стрелки...)
  * поддержка классов для прямого доступа OpenGL
  * объекты камеры и света могут быть использованы в любом месте иерархии объектов сцены
  * поведенческие объекты могут быть привязаны к объектам для их анимации с возможностью расщирения
  * специальные эффекты на объект (до/после визуализации объекта и сцены), с возможностью простого расширения
  * поддержка систем частиц, любые объекты или структуры объектов могут выступать в роли частиц
  * импорт 3D Studio файлов с автоматическим вычислением векторов нормалей (using the given *smoothing groups of the file) и текстурных координат
  * поддержка других форматов импорта: OBJ/OBJF, SMD, MD2, STL, TIN, PLY



Материалы 

  * простые в использовании, оптимизированные, объекты материалов
  * библиотека материалов для совместного использования
  * поддержка света: окружающего, диффузного, излучаемого, отраженного и светимости
  * поддержка режимов смешивание (прозрачность, наложения...)
  * автогенерация альфа-канала
  * поддержка форматов OpenGL текстур, включающих сжатие (DXT, S3TC и т.д.)
  * общая поддержка шэйдеров
  * высокоуровневая поддержка классов Cg шэйдеров
  * полиморфные изображения для текстурирования (позволяют использовать множество форматов также хорошо как и процедурные текстуры, с простым расширением
  * более чем 150 предопределенных цветов таких как clrCornflowerBlue или clrCoolCopper (легко расширяемые) *в дополнение к стандартным цветам и прямому использованию спецификации RGBA
  * простое использование перемещения и масштабирования свойств текстур, независимо от координат текстур
  * поддержка 32-битны растровых классов



Визуализация 

  * автоматическое использование аппаратных возможностей OpenGL, при их наличие
  * использование с моделью камеры, фокуса, расстояния и цели
  * множество точек обзора для одной или больше сцен, простая смена вида через выбор камеры
  * поддержка тумана и глубины вида
  * визуализация в файл в виде растровой картинки или на принтер в любом цвете и разрешениии
  * поддержка полноэкранного режима с динамической сменой разрешения
  * эффекты отражения и зеркала
  * динамические тени
  * высокопроизводительная поддержка системы прозрачных частиц во всей сцене
  * automatic frustrum culling (hierarchical or per-object)



Анимация 

  * propagation of time progression events
  * скелетная анимация (множество костей на вершину)
  * кадровая интерполяция и морфинг целей
  * простое расширяемое Поведение
  * динамика: инерция, ускорение, затухание и применение силы
  * автоматическая модуляция сцены



Интерфейс 

  * простые функции для определения выбранных объектов
  * вспомогательные функции для перемещения камеры
  * вспомогательные функции для для перемещения выбранных объектов
  * вспомогательные функции для для конвертирования экранных координат в мировые



Утилиты 

  * оптимизированные геометрические функции и утилиты (вектор, [кватернион](<http://ru.wikipedia.org/wiki/%D0%9A%D0%B2%D0%B0%D1%82%D0%B5%D1%80%D0%BD%D0%B8%D0%BE%D0%BD>) и матрицы...)
  * поддержка функций [кубических сплайнов](<http://ru.wikipedia.org/wiki/%D0%9A%D1%83%D0%B1%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B8%D0%B9_%D1%81%D0%BF%D0%BB%D0%B0%D0%B9%D0%BD>) и классов
  * манипуляции с [мэшами](<http://ru.wikipedia.org/wiki/%D0%9C%D0%B5%D1%88>) и оптимизация (stripification, vertex-cache awareness, и т.д.)
  * компонент для создания полноценнхы хранителей экрана в несколько щелчков
  * точное определение скорости смены кадров
  * асинхронный таймер (многопоточный)
  * поддержка джойстика
  * асинхронный доступ к клавиатуре и поддержка раскладок клавиатуры



### Снимики экрана (Screenshots)

[![Gl effect.jpg](https://wiki.freepascal.org/images/5/5c/Gl_effect.jpg)](</File:Gl_effect.jpg>) [![Gl cel.jpg](https://wiki.freepascal.org/images/f/f5/Gl_cel.jpg)](</File:Gl_cel.jpg>) [![Gl trinity.jpg](https://wiki.freepascal.org/images/5/50/Gl_trinity.jpg)](</File:Gl_trinity.jpg>)  
[![Gl ode.jpg](https://wiki.freepascal.org/images/4/49/Gl_ode.jpg)](</File:Gl_ode.jpg>)

### Автор

Author: изначально Mike Lischke  
at time maintained by Eric Grange  
Порт LCL: [Christian Ulrich](</User:Christian> "User:Christian")

### Скачать

#### Последняя версия (v1.0.0.2)

  * Новый снэпшот релиза можно найти на [Lazarus CCR Files page](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=185641>).



Этот снэпшот в основном совпадает с оригинальным cvs репозиторием по функциоанальности и исправлению ошибок, но до сих пор отсутствуют некоторые части, которые не появятся в "Lazarus-with-GLScene-Installer", пока кто нибудь не создаст их. 

\--[Crossbuilder](</User:Crossbuilder> "User:Crossbuilder") 20:16, 18 Июля 2007 (CEST) 

#### Устаревшая версия

Старый релиз (версия 0.9.1.6) можно найти по ссылке [Lazarus CCR Files page](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=98986&release_id=364007>) вместе с инсталятором lazarus и интегрированным в него GLScene. ОБНОВЛЕНИЕ: Это старая версия. Постарайтесь использовать версию из svn (см. ниже). 

### SVN

Вы можете проверить последнию версию здесь:  
<http://svn.freepascal.org/svn/fpcprojects/glscene/trunk/>  


Команда для проверки/получения:   

    
    
    svn co http://svn.freepascal.org/svn/fpcprojects/glscene/trunk glscene

Эта команда скачает основной репозиторий в каталог "glscene". 

В операционной системе Windows вы можете воспользоваться любым клиентом-SVN, например [tortoise](<http://tortoisesvn.tigris.org>). 

### Отчет об ошибках

[Bug Reporting/Featurerequest](<http://www.freepascal.org/mantis/>) Пожалуйста выберите Проект "Packages" и Категорию "glscene" для своего отчета. Спасибо. 

### Изменения

  * 18.07.2007 Snapshot release version 1.0.0.2 
    * adjusted some things, so it works with latest lazarus svn version
    * added object bitmaps for the SceneEditor
    * fixed another AV in SceneEditor
    * incorporated all patches and bugfixes from original glscene cvs repo like dynamic textures ..
  * 13.06.2007 Snapshot release version 1.0.0.1 
    * incorporated all patches and bugfixes from original glscene cvs repo
    * improved scene editor (crashes less, but still needs some love)  
**Tip:** Save your project frequently ;-)
    * seperated mmsystem dependent stuff from generic sound support so e.g. FMOD is available in linux, too
    * many more ..
  * 01.12.06 Added support for ODE, gtk2 and Cg shaders in Linux (ODE is limited to normal release, unsupported are cone, ccylinder and terrains)
  * 10.08.05 Too many changes to post here see cvs history
  * 16.07.05 GLScene 0.9.X Port
  * look at GLScene Sourceforge Account for complete Changelog



Статус: Beta 

### Установка версии 1.0.0.2

**Требуется fpc 2.1.4 или новее и lazarus 0.9.23 rev. 11348 или новее !!**

  * **распаковка исходного кода**
    * ZIP: 
      * в каталоге "lazarus/components/" выполните "unzip glscene-1.0.0.2.zip"   
При этом будет создан каталог lazarus/components/glscene/
    * SVN: 
      * в каталоге "lazarus/components/" выполните 
            
            svn co http://svn.freepascal.org/svn/fpcprojects/glscene/trunk glscene

При этом будет создан каталоге lazarus/componets/glscene/
    * Tortoise SVN: 
      * добавьте каталоге "glscene" вручную, так чтобы получилось lazarus/components/glscene. После этого выполните команду контекстного меню "SVN Checkout..." на созданном каталоге. Введите в поле "URL of repository"
            
            http://svn.freepascal.org/svn/fpcprojects/glscene/trunk

Подтвердите кнопкой "OK".



  


  * **Завершение установки:**
    * скопируйте "lazarus/components/glscene/globallinks/*.lpl" в "lazarus/packager/globallinks/" (только для *nix)
    * запустите lazarus
    * выполните команду "Найстройка установленных пакетов..." из меню "Пакет" (Пакет/Найстройка установленных пакетов...)
    * выберите "glscenelazarus" из списка "Доступные пакеты"
    * щелкните по кнопке "Установить выбранное"
    * щелкните по кнопке "Сохранить и перезапустить IDE" 
      * ВНИМАНИЕ: Может появится ошибка о том что пакет "jpegforlazarus" не найден. Если такая ошибка появилась, то выполните следующие действия:
      * выполните команду Пакеты/Открыть загруженный пакет...
      * выберите "glscenelazarus" и щелкните "Открыть..."
      * в самом низу появившегося списка "Требуемые пакеты" удалите зависимость от "jpegfromlazarus".
      * отредактирйте модуль "jpeg" в каталоге /lazarus/components/glscene/Source/base и удалите "LazJPEG" (в разделе uses) в модуле "LazJPG" (из пакета ImagesForLazarus).
      * сохраните и повторите последние 4 шага опять.
      * ПОДСКАЗКА: Вам следует установить пакет "ImagesForLazarus" (тем же самым способом что и пакет "glscenelazarus").
    * повторите последние 4 шага для остальных пакетов, которые вы собираетесь использовать. 
      * Заметка: glsceneode и glscenecgshader требуют наличия их библиотек в системе во время инсталяции!
    * **Заметка:** вы также можете попробовать выбрать и установить все пакеты одновременно.Тем не менее, я предпочитаю делать это шаг за шагом, чтобы видеть где произошла ошибка, если она произошла ;)
    * **Ошибки при установке "glscenelazarus":** (только для glscene-1.0.0.2.zip, в версии из svn это уже исправлено) 
      * Исправьте в модуле GLLCLViewer: измените procedure TGLSceneViewer.CreateParams(var Params: TCreateParams); - на procedure TGLSceneViewerLCL.CreateParams(var Params: TCreateParams);
      * Исправьте в модуле GLLCLViewer: procedure TGLSceneViewer.EraseBackground(DC: HDC); - на procedure TGLSceneViewerLCL.EraseBackground(DC: HDC);
      * Исправьте в модуле GLLCLFullscreenViewer: измените "RegisterClasses([TGLSceneViewer, TGLFullScreenViewer]);" на "RegisterClasses([TGLFullScreenViewer]);"


  * **Список пакетов:**
    * **glscenelazarus** Основной пакет.
    * **glscenecgshader** Компоненты Cg-шейдеров
    * **glsceneode** ODE компоненты
    * **glss_fmodlaz** FMOD компоненты
    * **gls_winonly** Только для Windows. Содержит поддержку звука через mmsystem, BASS и поддержку компонента GLScreenSaver


  * **Ошибки при компиляции "демок":**
    * Если не найден класс - TGLSceneViewer, то следует изменить в разделе uses GLLCLViewer на GLViewer



### Установка версии 0.9.6.1

  * Создайте каталог lazarus\components\GLScene
  * В созданном каталоге распакуйте/разархивируйте файлы из архива
  * Запустите Lazarus
  * Откройте пакет glscenelazarus.lpk командой Компоненты/Открыть пакет(.lpk) из каталоге lazarus\components\glscene\Lazarus
  * (Щелкните по кнопке Компилировать при необходимости, если не хотите установки компонент в IDE)
  * Щелкните по кнопке Установить
  * Повторите эти шаги (открыть .lpk, щелкните по кнопке Установить) для остальныъ пакетов, которые хотите использовать. Заметка: эти дополнительные пакеты требуют дополнительных библиотек (fmod, [ode](<http://www.ode.org>), [Cg](<http://developer.nvidia.com/object/cg_toolkit.htm>)). 
    * glscenecgshader.lpk : Cg шейдеры (только для карт nVidia)
    * glsceneode.lpk : поддержка библиотеки ode
    * glsss_fmodlaz.lpk : поддержка библиотеки fmod
    * gls_winonly.lpk : может быть установлено только в операционной системе Windows. Содержит средства создания хранителей экрана.



Подсказка: sometimes an Error "GLGraphics not found" occurs. Don't worry, just do Tools->Build Lazarus again and it should be ok. 

A full install of Lazarus with GLScene already in the IDE is available as [binary win32 release](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=185641&release_id=405739>) or <http://www.skinhat.com/lazarus>

Just a warning for (new) Linux users: GLScene in its current state is not working "out of the box" (lots of modifications/additions needed). UPDATE: this is not true anymore. glscene for lazarus is pretty up-to-date and most things work good right out of the box for windows as well as for linux. 

As of latest trunk version (see svn above) the linux version is working well. Some demos (about 20%) are still broken but the majority of glscene is now working. We're also moving towards making it work in other Unix platforms like FreeBSD. 

The missing/nonworking parts on non-windows are: glKeyboard (uses winapi, but is not required mostly), glSound(which uses mm, we need to use openal or somesuch) 

Пока в разработке [fully featured windows installer](<../en/Lazarus-_Game_Developers_Edition.md> "Lazarus- Game Developers Edition"). 

### Помощь

  * можно найти на [на основной странице GLScene](<http://www.glscene.org>)
  * присоедениться к каналу #glscene на сервере freenode.net
  * [русский сайт GLScene](<http://www.glscene.ru/>)

---

_Source: [https://wiki.freepascal.org/GLScene/ru](https://web.archive.org/web/20250117050051/https://wiki.freepascal.org/GLScene/ru)_
