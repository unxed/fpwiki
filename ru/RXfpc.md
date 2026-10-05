# RXfpc

│ **[Deutsch (de)](</RXfpc/de> "RXfpc/de")** │  **[English (en)](<../en/RXfpc.md> "RXfpc")** │  **[español (es)](</RXfpc/es> "RXfpc/es")** │  **[français (fr)](</RXfpc/fr> "RXfpc/fr")** │  **[português (pt)](</RXfpc/pt> "RXfpc/pt")** │  **русский (ru)** │    
****

## Contents

  * 1 О пакете
  * 2 Снимки экрана
  * 3 Загрузка
  * 4 SVN
  * 5 Зависимости / Системные требования
  * 6 Установка
  * 7 Примечание



## О пакете

  * RxLib для Lazarus содержит некоторые хорошо известные компоненты пакета RxLib для Delphi.



## Снимки экрана

  * [![idebar rx controls.png](https://wiki.freepascal.org/images/f/f5/idebar_rx_controls.png)](</File:idebar_rx_controls.png>)



  


  * [![idebar rx dbaware.png](https://wiki.freepascal.org/images/a/ac/idebar_rx_dbaware.png)](</File:idebar_rx_dbaware.png>)



  


  * [![idebar rx tools.png](https://wiki.freepascal.org/images/f/fb/idebar_rx_tools.png)](</File:idebar_rx_tools.png>)



## Загрузка

Пакет может быть загружен по ссылке [Lazarus CCR SourceForge site](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=187197>). 

## SVN

Исходники доступны через SVN, последняя версия здесь: 
    
    
     svn co <https://lazarus-ccr.svn.sourceforge.net/svnroot/lazarus-ccr/components/rx>
    

или 
    
    
     <https://svn.code.sf.net/p/lazarus-ccr/svn/components/rx/trunk>
    

## Зависимости / Системные требования

Проверено на Linux. Изменения должны быть сделаны: 

  * Используется Canvas.BrushCopy. Вы должны заменить это вызовом Canvas.CopyRect.
  * Добавить в раздел uses [модуль] Types для использования в rxdbgrid.pas и rxtoolbar.pas



Обновленная версия будет опубликована как можно быстрее. 

## Установка

  * Скопируйте исходники в директорию <$LazarusDir>/components.
  * В меню "Пакет" -> "Открыть файл пакета (.lpk)", выберите rxnew.lpk.
  * Скомпилируйте пакет, чтобы проверить, что все собирается.
  * Установите пакет в Lazarus и пересоберите его.



## Примечание

Русскоязычный форум, посвященный RxLib для Lazarus: 
    
    
     [freepascal.ru RxLib Forum](<http://freepascal.ru/forum/viewforum.php?f=18&sid=3142b1db749549de7472ae010ab22b8c>)

---

_Source: [https://wiki.freepascal.org/RXfpc/ru](https://web.archive.org/web/20231203215856/https://wiki.freepascal.org/RXfpc/ru)_
