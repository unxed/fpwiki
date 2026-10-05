# End of Line

│ **[English (en)](<../en/End_of_Line.md> "End of Line")** │  **[suomi (fi)](</End_of_Line/fi> "End of Line/fi")** │  **русский (ru)** │    
****

` LineEnding` представляет собой маркер окончания строки. Он используется для обозначения окончания строк в текстовых файлах. 

## Обозначения конца строки

Для обозначения конца строки в разных системах используются символы: 

  * [Перевод строки](<Line_feed.md> "Line feed/ru") (LF, #10 ): Linux, OS X, BSDs, Unix
  * [Возврат каретки](<Carriage_return.md> "Carriage return/ru") \+ [перевод строки](<Line_feed.md> "Line feed/ru") (CRLF, #13#10): Microsoft Windows
  * [Возврат каретки](<Carriage_return.md> "Carriage return/ru") (CR, #13): Mac OS Classic



Mac OS X также принимает символы [перевода строки](<Line_feed.md> "Line feed/ru") и [возврата каретки](<Carriage_return.md> "Carriage return/ru"). 

## См. также

  * [LineEnding](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/lineending.html> "doc:rtl/system/lineending.html") \- константа, содержащая символ конца строки.
  * [SetTextLineEnding](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/settextlineending.html> "doc:rtl/system/settextlineending.html") \- процедура, задающая символ конца строки для указанного текстового файла.
  * [sLineBreak](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/slinebreak.html> "doc:rtl/system/slinebreak.html") \- псевдоним для LineEnding. Оставлен для совместимости.

---

_Source: [https://wiki.freepascal.org/End_of_Line/ru](https://web.archive.org/web/20250117044252/https://wiki.freepascal.org/End_of_Line/ru)_
