# Using resourcestrings

│ **[English (en)](<../en/Using_resourcestrings.md> "Using resourcestrings")** │  **[español (es)](</Using_resourcestrings/es> "Using resourcestrings/es")** │  **[Bahasa Indonesia (id)](</Using_resourcestrings/id> "Using resourcestrings/id")** │  **русский (ru)** │    
****

Файл .rst создается, чтобы обеспечить механизм для локализации приложений. В настоящее время, доступен только один механизм локализации: gettext. 

  


Шаги алгоритма следующие: 

  1. Компилятор создает файлы .rst.
  2. Утилита rstconv преобразует .rst в .po (входные данные для gettext) Этот файл может быть переведен на множество языков. Могут быть использованы все стандартные gettext-утилиты.
  3. Gettext создает .mo файлы.
  4. .mo файлы считываются с помощью gettext модуля и все строковые ресурсы переведены на русский.



  
Хотя данный алгоритм и является рекомендуемым, вы можете локализовывать приложения и собственными методами. Причина выбора gettext для локализации приложений была в том, что это более-менее стандартный Unix подход. Однако, сам механизм локализации через gettext выглядит ужасным. 

В качестве альтернативного подхода в локализации ПО, видится использование .ini файлов для разных языков, например: 

english.ini: 
    
    
    [sysutils]
    SErrInvalidDateTime="%S" is not a valid date/time indication.
    

russian.ini: 
    
    
    [sysutils]
    SErrInvalidDateTime="%S" не корректное указание даты\времени.
    

  
Более подробную информацию, читайте [здесь](<Translations_/_i18n_/_localizations_for_programs.md> "Translations / i18n / localizations for programs/ru").

---

_Source: [https://wiki.freepascal.org/Using_resourcestrings/ru](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/Using_resourcestrings/ru)_
