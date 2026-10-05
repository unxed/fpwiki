# Application Bundle

│ **[English (en)](<../en/Application_Bundle.md>)** │  **русский (ru)** │

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

Эта статья относится только к [macOS](</Category:macOS> "Category:macOS").

См. также: [Multiplatform Programming Guide](<../en/Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

  
****

## Contents

  * 1 Обзор
  * 2 Макет пакета приложения
  * 3 Создание пакета приложения
    * 3.1 Посредством Lazarus
    * 3.2 Посредством инструмента командной строки, поставляемым с Lazarus
    * 3.3 Посредством сценария оболочки
  * 4 Запуск приложения через Application Bundle
  * 5 См.также
  * 6 Внешние ссылки



## Обзор

Пакет приложений - это каталог с расширением «.app» в системах macOS. Он содержит исполняемый файл приложения, файлы ресурсов, файлы библиотеки (если есть), файлы справки и информацию о приложении и необходим для правильного выполнения приложений. Mac Finder рассматривает этот каталог .app как файл приложения и по умолчанию не отображает ни один из его подкаталогов. 

Изнутри Lazarus он используется для интерфейсов [Carbon](<../en/Carbon_Interface.md> "Carbon Interface") и [Cocoa](<../en/Cocoa_Interface.md> "Cocoa Interface"), но можно также создавать приложения с другими интерфейсами (например, Gtk или QAt) с помощью скриптов. 

Вы можете узнать больше о пакетах приложений в Apple [Bundle Programming Guide](<https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/BundleTypes/BundleTypes.html#//apple_ref/doc/uid/10000123i-CH101-SW19>). 

Настройки приложения (пакета) находятся в файле списка свойств: [Info.plist](<../en/macOS_property_list_files.md> "macOS property list files") , расположенном в каталоге bundle.app/Contents/. 

Чтобы получить доступ к набору приложений, вам нужно щелкнуть правой кнопкой мыши (ctrl-левой кнопкой мыши) на пакете и выбрать _Show Package Contents_(Показать содержимое пакета). 

[![openbundle.png](https://wiki.freepascal.org/images/d/d2/openbundle.png)](</File:openbundle.png>)

[![infoplist.png](https://wiki.freepascal.org/images/8/80/infoplist.png)](</File:infoplist.png>)

## Макет пакета приложения

Базовая структура пакета приложений Mac: 
    
    
    MyApp.app/
       Contents/
          Info.plist
          MacOS/
          Resources/
    

Подпапка каталога Contents | Описание использования   
---|---  
MacOS | (Обязательный) Содержит автономный исполняемый код приложения. Обычно этот каталог содержит только один двоичный файл с основной точкой входа вашего приложения и статически связанным кодом. Однако вы можете поместить в этот каталог и другие автономные исполняемые файлы (например, инструменты командной строки).   
Resources | Содержит все файлы ресурсов приложения. Содержимое этого каталога дополнительно организовано, чтобы различать локализованные и нелокализованные ресурсы. Для получения дополнительных сведений о структуре этого каталога см. [Adding a Help Book - Bundle layouts](<../en/Add_an_Apple_Help_Book_to_your_macOS_app.md> "Add an Apple Help Book to your macOS app").   
Frameworks | Содержит все частные разделяемые библиотеки и фреймворки, используемые исполняемым файлом. Платформы в этом каталоге привязаны к редакциям приложения и не могут быть заменены никакими другими, даже более новыми версиями, которые могут быть доступны для операционной системы. Другими словами, фреймворки, включенные в этот каталог, имеют приоритет над любыми другими фреймворками с аналогичными названиями в других частях операционной системы. Для получения информации о том, как добавить общие библиотеки в пакет приложений, см. [macOS Dynamic Libraries](<../en/macOS_Dynamic_Libraries.md> "macOS Dynamic Libraries").   
PlugIns | Содержит загружаемые пакеты, расширяющие базовые функции вашего приложения. Вы используете этот каталог для включения модулей кода, которые должны быть загружены в пространство процесса вашего приложения для использования. Вы не должны использовать этот каталог для хранения автономных исполняемых файлов.   
SharedSupport | Содержит дополнительные некритические ресурсы, которые не влияют на возможность запуска приложения. Вы можете использовать этот каталог для включения таких вещей, как шаблоны документов, картинки и учебные пособия, наличие которых может ожидать ваше приложение, но которые не влияют на способность вашего приложения работать.   
  
## Создание пакета приложения

### Посредством Lazarus

Откройте проект и перейдите в Project -> Project Options -> вкладку Application, и нажмите кнопку [Create Application Bundle](<IDE_Window__Project_Options.md> "IDE Window: Project Options/ru"). Полученный пакет приложений будет содержать символическую ссылку на реальный исполняемый файл. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Вы должны удалить символическую ссылку и скопировать реальный исполняемый файл "project1" в каталог project1.app/Contents/MacOS/, если хотите распространять приложение и использовать его на другом компьютере.

### Посредством инструмента командной строки, поставляемым с Lazarus

Откройте /Library/Lazarus/components/macfiles/examples/createmacapplication.lpi в IDE. Скомпилируйте. 

Откройте терминал по вашему выбору и введите: 
    
    
    cd project1/
    /Library/Lazarus/components/macfiles/examples/createmacapplication project1
    ln -s ../../../project1 project1.app/Contents/MacOS/project1
    

### Посредством сценария оболочки

You can adapt the following shell script to create a customized application bundle for your application. It allows the creation of either debug or release bundle. 

  * In the debug bundle, a link to the executable is placed, allowing for debugging using the Lazarus IDE,
  * In the release bundle, the executable is copied so the application bundle as a whole can be used stand-alone/copied to other locations.



Вы можете адаптировать следующий сценарий оболочки для создания настраиваемого пакета приложений для вашего приложения. Это позволяет создавать пакеты debug или release. 

  * В пакете debug размещена ссылка на исполняемый файл, что позволяет делать отладку с использованием Lazarus IDE,
  * В пакете release исполняемый файл копируется, поэтому комплект приложения в целом можно использовать отдельно/скопировать в другие места.


    
    
    #!/bin/sh
    # Force Bourne shell in case tcsh is default.
    #
    
    #
    # Reads the bundle type
    #
    
    echo "========================================================"
    echo "    Bundle creation script"
    echo "========================================================"
    echo ""
    echo " Please select which kind of bundle you would like to build:"
    echo ""
    echo " 1 > Debug bundle"
    echo " 2 > Release bundle"
    echo " 0 > Exit"
    
    read command
    
    case $command in
    
      1) ;;
    
      2) ;;
      
      0) exit 0;;
    
      *) echo "Invalid command"
         exit 0;;
    
    esac
    
    #
    # Creates the bundle
    #
    
    appname=Magnifier
    appfolder=$appname.app
    macosfolder=$appfolder/Contents/MacOS
    plistfile=$appfolder/Contents/Info.plist
    appfile=magnifier
    
    PkgInfoContents="APPLMAG#"
    
    #
    if ! [ -e $appfile ]
    then
      echo "$appfile does not exist"
    elif [ -e $appfolder ]
    then
      echo "$appfolder already exists"
    else
      echo "Creating $appfolder..."
      mkdir $appfolder
      mkdir $appfolder/Contents
      mkdir $appfolder/Contents/MacOS
      mkdir $appfolder/Contents/Frameworks  # optional, for including libraries or frameworks
      mkdir $appfolder/Contents/Resources
    
    #
    # For a debug bundle,
    # Instead of copying executable into .app folder after each compile,
    # simply create a symbolic link to executable.
    #
    if [ $command = 1 ]; then
      ln -s ../../../$appname $macosfolder/$appname
    else
      cp $appname $macosfolder/$appname
    fi  
    
    # Copy the resource files to the correct place
      cp *.bmp $appfolder/Contents/Resources
      cp icon3.ico $appfolder/Contents/Resources
      cp icon3.png $appfolder/Contents/Resources
      cp macicon.icns $appfolder/Contents/Resources
      cp docs/*.* $appfolder/Contents/Resources
    #
    # Create PkgInfo file.
      echo $PkgInfoContents >$appfolder/Contents/PkgInfo
    #
    # Create information property list file (Info.plist).
      echo '<?xml version="1.0" encoding="UTF-8"?>' >$plistfile
      echo '<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">' >>$plistfile
      echo '<plist version="1.0">' >>$plistfile
      echo '<dict>' >>$plistfile
      echo '  <key>CFBundleDevelopmentRegion</key>' >>$plistfile
      echo '  <string>English</string>' >>$plistfile
      echo '  <key>CFBundleExecutable</key>' >>$plistfile
      echo '  <string>'$appname'</string>' >>$plistfile
      echo '  <key>CFBundleIconFile</key>' >>$plistfile
      echo '  <string>macicon.icns</string>' >>$plistfile
      echo '  <key>CFBundleIdentifier</key>' >>$plistfile
      echo '  <string>org.magnifier.magnifier</string>' >>$plistfile
      echo '  <key>CFBundleInfoDictionaryVersion</key>' >>$plistfile
      echo '  <string>6.0</string>' >>$plistfile
      echo '  <key>CFBundlePackageType</key>' >>$plistfile
      echo '  <string>APPL</string>' >>$plistfile
      echo '  <key>CFBundleSignature</key>' >>$plistfile
      echo '  <string>MAG#</string>' >>$plistfile
      echo '  <key>CFBundleVersion</key>' >>$plistfile
      echo '  <string>1.0</string>' >>$plistfile
      echo '</dict>' >>$plistfile
      echo '</plist>' >>$plistfile
    fi
    

## Запуск приложения через Application Bundle

Вы можете запустить приложение из среды IDE с помощью значка Finder или в собственном MacOS-овском Terminal.app посредством "open project1.app". 

## См.также

  * [macOS property list files](<../en/macOS_property_list_files.md> "macOS property list files")
  * [Add an Apple Help Book to your macOS app](<../en/Add_an_Apple_Help_Book_to_your_macOS_app.md> "Add an Apple Help Book to your macOS app")
  * [Hiding a macOS app from the Dock](<../en/Hiding_a_macOS_app_from_the_Dock.md> "Hiding a macOS app from the Dock")



## Внешние ссылки

  * [Apple: NSBundle class](<https://developer.apple.com/documentation/foundation/nsbundle>)
  * [Apple: Bundle Resources](<https://developer.apple.com/documentation/bundleresources/>)
  * [Apple: CFBundle](<https://developer.apple.com/documentation/corefoundation/cfbundle>)
  * [Apple: Bundle Configuration](<https://developer.apple.com/documentation/bundleresources/information_property_list/bundle_configuration>)
  * [Apple: Resource Programming Guide](<https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/LoadingResources/Introduction/Introduction.html>)
  * [Apple: Bundle Programming Guide](<https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/Introduction/Introduction.html>)

---

_Source: [https://wiki.freepascal.org/Application_Bundle/ru](https://web.archive.org/web/20250210103449/https://wiki.freepascal.org/Application_Bundle/ru)_
