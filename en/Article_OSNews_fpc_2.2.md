# Article OSNews fpc 2.2.0

## Contents

  * 1 Cross-platform ontwikkelen met Free Pascal 2.2.0
    * 1.1 Waar schuilt de kracht van fpc?
    * 1.2 Cross-platform
    * 1.3 Grafische applicaties
    * 1.4 64-bits Windows
    * 1.5 Nieuwe features voor Pascal
    * 1.6 Snelheid
    * 1.7 Bibliotheken
    * 1.8 Conclusie



# Cross-platform ontwikkelen met Free Pascal 2.2.0

Onlangs is Free Pascal (fpc) versie 2.2.0 uitgekomen. Deze open-source Pascal-compiler is sinds de eerste versie in 1993 uitgegroeid tot een van de meest geavanceerde open-sourcecompilers die er vandaag de dag zijn. Steeds meer programmeurs ontdekken fpc en ontwikkelen hun applicaties in Object Pascal. Met name de ontwikkeling van Lazarus heeft daaraan bijgedragen. Lazarus is een grafische open-source-IDE voor fpc, met een uitgebreide toolkit om grafische applicaties te ontwikkelen. 

  


## Waar schuilt de kracht van fpc?

Voor veel programmeurs komt de hernieuwde aandacht voor Pascal als een verassing. Voor velen is het een oude taal, die vrijwel niet meer gebruikt wordt en de strijd met C al jaren geleden heeft verloren. Toch heeft fpc genoeg features die ervoor zorgen dat steeds meer programmeurs voor fpc kiezen. 

  


## Cross-platform

Het grootste voordeel van fpc is ongetwijfeld dat het cross-platform is. Daar is de ontwikkeling sinds de 2.x-reeks ook specifiek op gericht. 

  
Zo is het niet nodig om allerlei Makefiles te gebruiken die per systeem verschillen en ingesteld moeten worden met autoconf/configure. De programmeur hoeft ook niet zelf allerlei bibliotheken bij elkaar te zoeken. Het installeren van de compiler en het kopiëren van de source-code naar een ander platform is voldoende. Maar het is ook mogelijk om op het ene platform (bijvoorbeeld Linux) direct een applicatie voor een ander platform te maken (bijvoorbeeld Windows.) 

  
Verder is fpc-code zo portable omdat je niet zoals bij bijvoorbeeld C last hebt van de verschillen tussen Posix en de WinXX-API. Posix kan je met o.a. Cygwin en MinGW simuleren op een 32 bits Windowssysteem simuleren, en andersom lukt ook. Maar ideaal is dat niet. Vaak moet de simulatie-API ook op het systeem waar de applicatie moet draaien geïnstalleerd worden, en het programma past toch niet echt bij het OS waar het op draait. 

  
Met Free Pascal werkt dat anders. Fpc komt met uitgebreide bibliotheken. Zo zijn er de RTL, voor de basis functionaliteit als file-access, klassen, string bewerking en RTTI en de FCL voor allerlei extra's als XML, web applicaties, database componenten, plaatjes en een test-framework. Zolang een programmeur deze bibliotheken gebruikt, en niet direct system- of api-calls gebruikt, zal zijn programma niet alleen op Linux werken, maar ook direct op Windows, FreeBSD, Mac OS X, Windows CE en meer. Daarnaast past de ontwikkelings-tool veel beter in een Win32 omgeving. 

  
Het spreekt voor zich dat als een programmeur een directe link maakt naar 'c:\program files' of een configuratiebestand neerzet in '/etc/myprog.cfg', dit niet cross-platform is. Maar als hij de TRegistry-classe uit de FCL gebruikt, zorgen de RTL en de FCL ervoor dat als de programmeur dat wenst, op Linux een configuratie-file gemaakt wordt, en op Windows een registry-key. Het direct benaderen van de systeem-API's is praktisch nooit nodig. 

  


## Grafische applicaties

Ook grafische applicaties zijn geen probleem. De IDE Lazarus is ontwikkeld volgens dezelfde principes als fpc. De LCL-library die bij Lazarus geleverd wordt zorgt ervoor dat je programma's kunnen werken met de standaard Windows-32 of 64-bits API, GTK-1,GTK-2. QT en Carbon (voor OS X) zijn in ontwikkeling. 

Met Lazarus kan je een applicatie ontwikkelen in Linux, gebruik makend van GTK2. Als je van datzelfde programma een Windows-versie wilt hebben, geef je dat gewoon aan in de compiler-opties. Je compileert, en daar is je Windows .exe-executable die op elke Windowsmachine werkt zonder dat daarop iets extra's geinstalleerd hoeft te worden. Behalve Lazarus (die fpc mee installeert) op je (Linux?) ontwikkelmachine heb je niets nodig. 

  


## 64-bits Windows

Van Windows naar Linux compileren is moeilijker, omdat fpc voor ELF-platformen voorlopig nog de GNU-linker gebruikt, en ook de bibliotheken nodig heeft zoals die op een Linux-systeem geïnstalleerd zijn. Maar het idee is hetzelfde. Voor Windows heeft fpc een interne linker. Zo is het ook mogelijk dat Free Pascal versie 2.2.0 ook beschikbaar is voor 64-bits Windows, terwijl de GNU-utilities dat nog niet zijn. Daarmee is fpc de eerste grote open-sourcecompiler voor Win64. 

  


## Nieuwe features voor Pascal

Het fpc-team heeft in het verleden al meerdere nieuwe features aan de taal Pascal toegevoegd, zoals de operator overloading die later ook door andere commerciële Pascal-compilers zijn overgenomen. Nieuw in versie 2.2 is de experimentele support voor generics. Met name bij uitgebreide list-klassen kan dat zeer nuttig zijn. 

  


## Snelheid

Maar hoe zit het dan met de snelheid? Om te beginnen gaat het compileren van je code zeker 7 keer sneller dan dat gcc dat doet. Misschien lijkt dat weinig uit te maken, maar het verkort de ontwikkeltijd aanzienlijk. De snelheid van de gegenereerde code is goed te vergelijken met die van de gcc-compiler. Volgens de benchmarks van de Shootout genereert fpc versie 2.0.4 zo'n 30% langzamere code dan gcc. Free Pascal code gebruikt op zijn beurt dan wel weer minder werkgeheugen. Vergelijk je fpc-code met bijvoorbeeld Ruby, dan is fpc zo'n 50 keer sneller. Met PHP en Java is het verschil kleiner, maar deze talen gebruiken dan weer 14 (PHP) of 17 (Java) keer zoveel geheugen dan FPC programma's. 

  


Je kan je afvragen of de snelheid van benchmarks ook iets zegt over echte (grafische?) applicaties. Maar als je de respons van de java-IDE Eclips vergelijkt met die van Lazarus, dan zie je direct wat het verschil is. Native code is gewoon nog steeds sneller dan geïnterpreteerde en JIT-talen ook als ze pre-compiled zijn. Native code scheelt in het geheugengebruik en het vergemakkelijkt het installeren van de applicatie omdat er daarvoor geen extra's op de computers van de gebruikers geïnstalleerd hoeft te worden. 

  


## Bibliotheken

Zoals gezegd komt Free Pascal met de RTL en FCL. In de FCL zitten componenten voor directe databasetoegang voor o.a. Oracle, Firebird, MySQL, ODBC, SQLite en PostgreSQL. Maar er zijn ook bibliotheken voor o.a. XML, webprogragramma's (cgi- en apachemodules,) SDL en OpenGL. Verder zijn veel Delphi-componenten ook direct te gebruiken met Free Pascal, zoals de ZEOS-componenten en de netwerkcomponenten van Indy. Ook zijn er steeds meer commerciële partijen die hun Delphi-bibliotheken beschikbaar maken voor Lazarus en Free Pascal. 

  


## Conclusie

Free Pascal is een volwassen compiler die prima geschikt is voor ál het programmeerwerk. Maar fpc blinkt met name uit als het om cross-platform applicaties gaat. Het gemak waarmee applicaties voor bijvoorbeeld zowel Windows als Linux kunnen worden ontwikkeld is verbazingwekkend. Daarnaast biedt Lazarus een IDE voor het ontwikkelen van grafische applicaties zoals die vrijwel niet beschikbaar zijn voor C of C++ onder Linux. Deze zijn alleen beschikbaar voor geïnterpreteerde talen als Java en C#, maar die hebben dan vaak weer problemen als veel geheugengebruik, slechte prestaties en lastig onderhoud. Al met al is de combinatie fpc/Lazarus een goede keus voor het cross-platform ontwikkelen.

---

_Source: [https://wiki.freepascal.org/Article_OSNews_fpc_2.2.0](https://web.archive.org/web/20250317113322/https://wiki.freepascal.org/Article_OSNews_fpc_2.2.0)_
