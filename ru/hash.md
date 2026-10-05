# hash

│ **[English (en)](<../en/hash.md>)** │  **русский (ru)** │

Пакет **hash** содержит реализации алгоритмов crc, md5, NTLM и crypt под Linux. 

## Модуль md5

Этот модуль содержит реализацию алгоритма дайджеста MD5 в соответствии со спецификацией RFC 1321. Так же имеет процедуры для вычисления хэшей из какого либо буфера или хэша какого либо целого файла. 

Тестовая программа md5test вычисляет значение хэша какой либо заданной строки. Нижеприведённый листинг предназначен для сравнения. Простой способ вычислить md5 хэш заданной строки это использовать функцию MD5String в качестве параметра функции MD5Print как в примере приведенном ниже: 
    
    
    uses md5;
    
    var
      Password, PasswordHash: string;
    begin
      PasswordHash := MD5Print(MD5String(Password));
    

Точно так же для получения MD5 хеша файла можно использовать: 
    
    
    uses md5;
    
    var
      PathToFile, FileHash: string;
    begin
      FileHash := MD5Print(MD5File(PathToFile));
    

## Модули хешей

В FPC имеются следующие модули хешей: 

  * Реализация NTLM версия 1.0, модуль алгоритма хеширования пароля : ntlm.pas
  * Реализация MD2 дайджест алгоритма (RFC 1319) - модуль : md5.pp
  * Реализация MD4 дайджест алгоритма (RFC 1320) - модуль : md5.pp
  * Реализация MD5 дайджест алгоритма (RFC 1321) - модуль : md5.pp
  * Реализация CRC алгоритма - модуль : crc.pas



Вернуться назад к [Packages List](<../en/Package_List.md> "Package List")

---

_Source: [https://wiki.freepascal.org/hash/ru](https://web.archive.org/web/20250117040408/https://wiki.freepascal.org/hash/ru)_
