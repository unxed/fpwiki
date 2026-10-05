# Hungarian Translation Guide

From Free Pascal wiki

  
Magyar Fordítási Kalauz

  


## Contents

  * 1 Bevezetés
  * 2 Oldalak
    * 2.1 Oldalak fejléce
    * 2.2 Oldalak tartalma
      * 2.2.1 Formázások
      * 2.2.2 Táblázatok
  * 3 Kifejezések
    * 3.1 Hardver
    * 3.2 Szoftverkörnyezet
    * 3.3 Programozástechnika
    * 3.4 Egyéb

  
---  
  
  


#  Bevezetés

Ez az oldal ajánlásokat tartalmaz annak érdekében, hogy a Magyar nyelvű fordítások megjelenése, valamint szó- és kifejezéshasználata a lehető legegységesebb legyen. 

A magyarra fordított oldalak használhatósága érdekében a következők betartása alapvetően fontos: 

  * Úgy fogalmazd a mondatokat, hogy azokat mindenki megérthesse! 
  * Figyelj az írásjelek/központozás megfelelő használatára! Például: minden felszólító mondat végére ! jelet kell írni, függetlenül az eredeti szöveg írásmódjától! 
  * Ne használj szleng szavakat, chat-es kifejezéseket valamint vigyorokat! :) 
  * Ügyelj, hogy a mozaikszavak csupa nagybetűből álljanak! Például fpc helyett FPC! 
  * Ügyelj a nyelvi helyességre! Például: "_**Bent** van a ház**ban**_ " és nem "_Bent van a házba_ ", de "_**Be** megy a ház**ba**_ " 
  * Ügyelj az "**-e** " kérdőszócska megfelelő használatára! Például: "_**lefordítja-e**_ " nem pedig "_le-e fordítja_ ", valamint "_**jól fut-e**_ " és nem "_jól-e fut_ " vagy "_nem-e jól fut_ " 
  * Ügyelj az igekötők helyes használatára! Például: "_**meg kell** oldani_" és nem pedig "_megkell oldani_ ", de "_a problémát**meg** oldani **kell**_ " 
  * Ügyelj az egybe- és különírás szabályaira! Például: "_**nem tudom** leírni_" írandó a "_nemtudom leírni_ " helyett 
  * Ha bizonytalan vagy, inkább ellenőrizd az Online Helyesírási Szótárban! [[1]](<http://www.magyarhelyesiras.hu/>)
  * A helyesírás ellenőrzése mindig legyen bekapcsolva! Egyszer még Te is elgépelhetsz valamit... 
  * A csak részben fordított bekezdéseket tartsd megjegyzésben! Így: <!--félig fordított english text-->
  * Ha valamelyik oldalt lefordítottad magyarra, ne felejtsd el módosítani a rá mutató hivatkozást is, hogy egyből magyarul jelenjen meg! Például [[Main_Page|Főoldal]]-ról [[Main_Page/hu|Főoldal]]-ra. 
  * A "_user:..._ " oldaladon hivatkozz erre az oldalra! 



A fordítási munkában résztvevő személyekkel a [Magyar Lazarus Közösség](<http://lazarus.freepascal.hu/>) honlapján található [FreePascal/Lazarus Wiki magyarítás project](<http://lazarus.freepascal.hu/index.php?option=com_fireboard&Itemid=39&func=showcat&catid=6>) fórumban veheted fel a kapcsolatot. 

#  Oldalak

Az oldalak fordításakor nem ajánljuk önálló magyar című oldalak létrehozását, helyette az eredeti angol elnevezést javasoljuk /hu kiegészítéssel. Ajánlott elnevezés: 
    
    
    Main_Page/hu
    

Nem ajánlott: 
    
    
    Főoldal
    

##  Oldalak fejléce

Minden oldalon javasoljuk az oldal címének feltüntetését magyarul is a következő módon: 
    
    
    {{Hungarian Translation Guide}}
     
     
     <font size="7">Magyar Fordítási Kalauz</font>
     
     
     __TOC__

A tartalomjegyzék helyét a __TOC__ határozza meg, amennyiben nincs vagy nem kell tartalomjegyzék a __NOTOC__ használandó a helyette. 

##  Oldalak tartalma

###  Formázások

Új szakaszok szerkesztésénél érdemes sorkizárt formázást alkalmazni, így sokkal áttekinthetőbb és rendezettebb lesz a szöveg. Így lehet megadni: 
    
    
    ===Új szakasz===
    <div style="text-align: justify;">
    ...
    Szakasz szövege
    ...
    </div>
    

###  Táblázatok

Táblázatok kialakításánál ajánlott az eredeti forma megtartása, azonban ha új táblázatokat kell készíteni, akkor javasoljuk a látható keret nélküli megvalósítást: 
    
    
    {| border=0
    |- 
    | első sor, első mező
    | második mező
    | harmadik mező
    |- <!-- új sor a táblában-->
    | második sor, első mező
    | - <!--ez nem új sor a táblában hanem csak egy kötőjel a második mezőben-->
    | harmadik mező
    |}

Az létrehozott táblázatokban a szövegekre a wiki formázási szabályai érvényesek. 

#  Kifejezések

Egyes kifejezések magyar megfelelője a szövegkörnyezettől függően más és más lehet, ezért ebben a részben összefoglaljuk a legfontosabbakat. 

Alkalmazások, csomagok, függvénytárak, komponensek, stb. (Free Pascal, Free Vision, stb. ) neveit az egyértelmű azonosíthatóság érdekében ne fordítsd le, esetleg csak zárójelben vagy kötőjellel elválasztva ha az adott elnevezés lényeges információt tartalmaz! Esetleg használd a közkeletű magyar megnevezést ha már van olyan! Ugyanezt javasoljuk hardverek, technológiák és minden más név esetén is. 

A "free" kifejezés olyan esetekben, mint a freeware, free software, free pascal, free vision, stb. ne legyen "ingyen"-nek fordítva! Már csak azért se, mert ezen szoftverek többségének használata a GPL elfogadásához kötött, melynek majdnem a legelején a következő áll: 

"_When we speak of free software, we are referring to freedom, not price. Our General Public Licenses are designed to make sure that you have the freedom to distribute copies of free software (and charge for them if you wish), that you receive source code or can get it if you want it, that you can change the software or use pieces of it in new free programs, and that you know you can do these things._ " [gnu.org](<http://www.gnu.org/licenses/gpl.html>)

"_A szabad szoftver megjelölés nem azt jelenti, hogy a szoftvernek nem lehet ára. A GPL licencek célja, hogy garantálja a szabad szoftver másolatainak szabad terjesztését (és e szolgáltatásért akár díj felszámítását), a forráskód elérhetőségét, hogy bárki szabadon módosíthassa a szoftvert, vagy felhasználhassa a részeit új szabad programokban; és hogy mások megismerhessék ezt a lehetőséget._ " [gnu.hu](<http://gnu.hu/gplv3.html>)

##  Hardver

_Hardverek és azok működésével kapcsolatos kifejezések._

**architecture** |  \-  |  **gépkiépítés** , **géptípus** (processzor és egyéb hardverelemek együttese)   
---|---|---  
  
##  Szoftverkörnyezet

_Szoftverkörnyezettel, szoftverekkel és azok működésével kapcsolatos kifejezések._

**platform** |  \-  |  _lehetőleg ne legyen magyarra fordítva_ (operációs rendszer, processzor és egyéb harverelemek együttese)   
---|---|---  
**directory** |  \-  |  **könyvtár** (ajánlott), **mappa** (grafikus felülettel kapcsolatos szövegekben)   
**folder** |  \-  |  **könyvtár** (ajánlott), **mappa** (grafikus felülettel kapcsolatos szövegekben)   
  
##  Programozástechnika

_Programozással kapcsolatos kifejezések._

**build** |  \-  |  **építés** (fordítás + összefűzés)   
---|---|---  
**compile** |  \-  |  **fordítás**  
**data dictiorary** , _**datadict**_ |  \-  |  **adatszótár**  
**dataset** |  \-  |  **adatkészlet** , adathalmaz   
**function** |  \-  |  **függvény** , **eljárás** , **művelet**  
**implementation** (of unit/code)  |  \-  |  **kivitelezés** , **kidolgozás**  
**interface** (of unit/code)  |  \-  |  **felület**  
**interface** (user interface)  |  \-  |  **felület** , **kezelőfelület** , **felhasználói felület** , **ablakkialakítás**  
**library** |  \-  |  **függvénytár** (ajánlott, olvashatóbb), **függvénykönyvtár** Lásd még: _type library, typelib_  
**linking** |  \-  |  **összefűzés** (bináris objektumok egybefűzése), **építés** (ha az összefűzés zavaróan hangzik)   
**postfix** |  \-  |  **utótag**  
**prefix** |  \-  |  **előtag**  
**procedure** |  \-  |  **eljárás** (!)   
**resource file** |  \-  |  **erőforrás fájl** (ajánlott, közkeletű)   
**section** |  \-  |  **rész** (ajánlott), **szakasz** (ha ez jobban fedi a valóságot)   
**session** |  \-  |  **munkamenet**  
**type library** , _**typelib**_ |  \-  |  **típustár** (ActiveX)   
**widget** , **widgetset** |  \-  |  **ablakelemek** , **ablakelemkészlet** (op.rendszer beépített {pl.:win32}, gtk1, gtk2, Qt, FpcGUI vagy egyéb)   
  
##  Egyéb

_Egyéb kifejezések._

...  |  \-  |  ...   
---|---|---

---

_Source: [https://wiki.freepascal.org/Hungarian_Translation_Guide](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/Hungarian_Translation_Guide)_
