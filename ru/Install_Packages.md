# Install Packages

│ **[English (en)](<../en/Install_Packages.md>)** │  **русский (ru)** │

## Contents

  * 1 Описание
  * 2 Файлы устанавливаемые с Lazarus'ом
  * 3 Adding packages using the IDE
    * 3.1 Adding known packages
    * 3.2 Adding new packages
    * 3.3 New versions
  * 4 Compile an IDE with packages at command line
  * 5 Questions
  * 6 See also



## Описание

По многочисленным просьбам вот небольшое руководство по "Установка пакетов HOW-TO". 

Мы будем рассматривать установку компонентов баз данных, которые не устанавливались ранее по умолчанию в предыдущих версиях Lazarus. Их до сих пор нет в минимальной установке IDE но они есть в полной инсталляции среды разработки. Поскольку последние компоненты баз данных по умолчанию присутствуют в Lasarus, принцип поиска и установки пакетов все ещё актуален и может быть полезен. 

Мы будем рассматривать и установку компонентов для соединения с базами данных (серверами) и получение данных с баз. 

Что ж, давайте посмотрим, что мы можем найти если поискать. 

## Файлы устанавливаемые с Lazarus'ом

В среде Linux Lasarus устанавливается по умолчанию в каталог **/usr/bin/lazarus** , В macOS Lazarus устанавливается в **/usr/local/share/lazarus** и конечно же в Windows установщик предлагает вам на выбор куда установить файлы без пробелов в имени пути. В моей Windows Lasarus установлен в **G:/Lazarus**. Не важно куда бы не был установлен Lasarus у Вас, Вы найдете некоторые поддиректории там. В данном случае многообещающая перспектива исходит от каталога под названием **components**. 

Как вы можете видеть в директории есть четыре поддиректории, которые напоминают нам о базах данных. Это **Interbase** , **mysql** , **sqlite** и **sqldb**. Первые три содержат компоненты для соединения с базами данных и TDataset потомки для соответствующих баз данных. Последняя, **sqldb** , является более общим, так как она состоит из TSQLQuery (TDataset) и TSQLTransaction которые используются для всех типов баз данных и Т??Connection, чтобы сделать подключение к базе данных. В этой статье мы установим пакет SQLDB вместе с Interbase. 

## Adding packages using the IDE

### Adding known packages

Open "Package -> Install/Uninstall Packages". The list on the left shows already installed packages or packages marked for installation (which have a green plus sign in their icon). The list to the right shows packages that you can install. Simply double-click on a package in either list to move them to the other - and mark them for installation/uninstall. 

Finally, click "Save and Rebuild" to rebuild the IDE and the packages. 

You can also compile and install packages the IDE knows nothing about. See below. 

### Adding new packages

To install a new package one goes to "Components -> Open Package File (.lpk)". In our example scenario, in the following dialog you navigate to **/usr/share/lazarus/components/sqldb** or the corresponding directory under OS X or Windows and choose the **sqldblaz.lpk** file. If everything works out as planned you will see the Package Manager. The Package Manager shows a treeview with the files in the package and the required packages. 

If you don't want to install the component into the IDE, e.g. because it is a non graphical component, press **Compile**. Otherwise you should press **Use >>** and then choose **Install**. Next we get a warning about Lazarus only supporting static linked libraries and the question if you want to rebuild Lazarus. Just press Yes and sit back. Lazarus will be rebuilt, and depending on your configuration restarted. 

If everything went well you'll see a new tab called **SQLdb**. This tab will contain two components: a TSQLConnection and a TSQLQuery. What we need next is a Connection component. Once again go to Components -> Open Package File and navigate to **/usr/share/lazarus/components/sqldb/interbase**. Open the ibconnectionlaz.lpk file and again press Compile and Install and Yes. :) 

Next time Lazarus is started the SQLdb tab also contains a component TIBConnection. 

### New versions

When receiving a new source version of an already installed package, you must 

  1. overwrite the previous source version with the new version
  2. either open the new .lpk and press **Use >>** and then choose **Install** as above,  
or rebuild the IDE using the menu "Tools" -> "Build Lazarus with Profile [...]" if you want more control on how the package is compiled.



## Compile an IDE with packages at command line

You can use [lazbuild](<../en/lazbuild.md> "lazbuild") to compile and install your package, and add --build-ide to rebuild the IDE. 

## Questions

If you have any questions, go to the lazarus [forum](<http://www.lazarus.freepascal.org/index.php?name=PNphpBB2&file=index>) or post it to the Lazarus mailing list. 

## See also

  * [Lazarus Packages](<../en/Lazarus_Packages.md> "Lazarus Packages")
  * [Install IDE packages without the IDE](<../en/Install_IDE_packages_without_the_IDE.md> "Install IDE packages without the IDE")

---

_Source: [https://wiki.freepascal.org/Install_Packages/ru](https://web.archive.org/web/20221126152706/https://wiki.freepascal.org/Install_Packages/ru)_
