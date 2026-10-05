# Poweredby

│ **[English (en)](<../en/Poweredby.md> "Poweredby")** │  **[français (fr)](</Poweredby/fr> "Poweredby/fr")** │  **русский (ru)** │    
****  


## Contents

  * 1 Компнент TPoweredBy
    * 1.1 by minesadorada@charcodelvalle.com
      * 1.1.1 Графика Windows
        * 1.1.1.1 Графика Linux/macOS
    * 1.2 Другие логотипы и баннеры
    * 1.3 Описание
    * 1.4 Загрузка
    * 1.5 Установка
    * 1.6 Использование
      * 1.6.1 Использование по-другому
    * 1.7 Лицензия
    * 1.8 Платформа
      * 1.8.1 Windows
      * 1.8.2 Linux
      * 1.8.3 macOS
    * 1.9 Проверено
    * 1.10 Версия
      * 1.10.1 Поддержка
    * 1.11 См.также



## Компнент TPoweredBy

### by minesadorada@charcodelvalle.com

#### Графика Windows

[![powered by graphic.png](https://wiki.freepascal.org/images/d/da/powered_by_graphic.png)](</File:powered_by_graphic.png>)

##### Графика Linux/macOS

[![linux powered by graphic.jpg](https://wiki.freepascal.org/images/7/7b/linux_powered_by_graphic.jpg)](</File:linux_powered_by_graphic.jpg>)

### Другие логотипы и баннеры

  * [Logos and Banners](<../en/Logos_and_Banners.md> "Logos and Banners")



* * *

### Описание

  * Это визуальный компонент (устанавливается на вкладку 'Additional'), который отображается в виде значка на форме и выветает через 1 секунду (или в Linux/MacOS отображается на 1 секунду).
  * Перетащите в событие `form.create()`



### Загрузка

Загрузить можно из lazarus CCR [отсюда](<https://sourceforge.net/p/lazarus-ccr/svn/HEAD/tree/components/>)

### Установка

  1. Создайте новую папку 'poweredby'
  2. Распакуйте туда архив
  3. В IDE Lazarus выберите пнкт меню 'Package' (Пакеты) --> 'Open Package File (*.lpk) (Открыть файл пакета) и откройте poweredby.lpk
  4. На вопрос 'Open as a project' (Открыть как проект) ответьте 'yes'
  5. Нажмите пункт Compile (Компилировать)
  6. Нажмите пункт Use/Install (Использовать/Установить)
  7. На вопрос 'would you like to compile Lazarus?' (Хотите пересобрать Lazarus?) ответьте 'yes'
  8. После перезагрузки Lazarus'а нажмите на вкладку компонентов 'Additional', чтобы найти вновь установленный компонент 'poweredby'



### Использование

  * Создайте проект нового приложения
  * Бросьте компонент 'poweredby' на форму
  * Дважды щелкните по форме, чтобы показать метод TForm1.Create
  * Добавьте в код `poweredby1.showpoweredbyform`
  * Запустите приложение



Вот и все! 

#### Использование по-другому

Компонент PoweredBy хорошо подходит для добавления в качестве субкомпонента к существующему настраиваемому компоненту: 
    
    
    Uses uPoweredBy, Propedits, ..//другие модули
    
    Type
    TMyComponent = Class(TComponent)
    private 
      fPoweredBy:TPoweredBy;
      //.... что-то подобное
    public
      procedure ShowPoweredByLogo; // вызываем метод fPoweredBy.ShowPoweredByForm в этой процедуре
      //.... что-то подобное
    published
      property PoweredBy:TPoweredBy read fPoweredBy write fPoweredBy;
      //.... что-то подобное
    end;
    
    procedure Register;
    RegisterPropertyEditor(TypeInfo(TPoweredBy),
        TMyComponent, 'PoweredBy', TClassPropertyEditor);
    
    Constructor TMyComponent.Create()
    // Используем tPoweredBy акк субкомпонент
    // Регистрируем TClassPropertyEditor для его правильного отображения
      fPoweredBy := TPoweredBy.Create(Self);
      fPoweredBy.SetSubComponent(true);  // велим IDE сохранить измененные свойства
      fPoweredBy.Name:='PoweredBy';
    

### Лицензия

Лицензия LGPL 

### Платформа

#### Windows

  * PoweredBy появится в виде таящего изображения



#### Linux

  * PoweredBy отобразиться квадратного изображения 
    * Это связано с неспособностью набора виджетов GTK работать с экранами прозрачной формы.



#### macOS

  * PoweredBy отображает квадратный рисунок, аналогичный версии для Linux.



### Проверено

Windows 7 32/64-bit Laz v1.x fpc 2.6.x Linux 32-bit Laz v0.9.x fpc 2.2.x 

### Версия

V1.0.1.2 

#### Поддержка

[Email автора](<mailto:minesadorada@charcodelvalle.com>) для любых вопросов 

### См.также

  * [Logos_and_Banners](<../en/Logos_and_Banners.md> "Logos and Banners")
  * [Standalone Scrolling Text component](<../en/ScrollText.md> "ScrollText")
  * [Components and code examples](<../en/Components_and_Code_examples.md> "Components and Code examples")

---

_Source: [https://wiki.freepascal.org/Poweredby/ru](https://web.archive.org/web/20250214011945/https://wiki.freepascal.org/Poweredby/ru)_
