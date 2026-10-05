# Binutils

│ [**English (en)**](<../en/Binutils.md> "Binutils") │  [**magyar (hu)**](</Binutils/hu> "Binutils/hu") │  **русский (ru)** │    
****

В большинстве случаев для кросс-компиляции с одной платформы на другую кросс-утилиты (Binutils), то есть ассемблер, компоновщик и т.д. 

Если нет готовых утилит для кросс-компиляции с основной платформы (**host**) на целевую (**target**) платформу то вы можете их скомпилировать, самостоятельно взяв исходные тексты с домашней страницы проекта (<http://www.gnu.org/software/binutils/>). 

Типичный пример конфигурирования кросс-утилит для использования с fpc: 
    
    
     ./configure --target=arm-linux \
     --prefix=/usr/local/lib/arm-linux \
     --bindir=/usr/local/bin --mandir=/usr/local/share/man --infodir=/usr/local/share/info \
     --program-prefix=arm-linux- \
     --disable-werror
    

Для Unix подобных систем цель (**target**) и префикс определены довольно чётко, для других систем такого определения нет. 

Список параметров: 

Платформа  | Цель (Target)  | Префикс   
---|---|---  
Windows (32 bit) | i386-mingw32 | i386-win32   
Win CE (Intel) | i386-wince | i386-wince   
Win CE (arm) | arm-wince-pe | arm-wince   
Win CE (i386) | i386-wince | i386-wince   
Solaris | i386-? | i386-?   
OS/2 | i386-? | i386-?   
Symbian OS | i386-? | i386-?   
Mac OS X (Intel) | i386-darwin | i386-darwin   
Mac OS X (PowerPC) | powerpc-darwin | powerpc-darwin   
iPhoneOS | arm-darwin? | arm-darwin?   
Amiga OS | powerpc-? | powerpc-?   
MorphOS | powerpc-? | powerpc-?   
arm-gba | ? | arm-gba   
arm-nds | ? | arm-nds   
arm-embedded | ? | ?   
arm-symbian | ? | ?   
arm-PalmOS | ? | ?

---

_Source: [https://wiki.freepascal.org/Binutils/ru](https://web.archive.org/web/20220101000000/https://wiki.freepascal.org/Binutils/ru)_
