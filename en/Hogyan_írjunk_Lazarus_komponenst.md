# Hogyan írjunk Lazarus komponenst

From Free Pascal wiki

## Contents

  * 1 Bevezetés
  * 2 1\. lépés: Csomag létrehozása
  * 3 2\. lépés: Unit készítése
    * 3.1 Új unit készítése
  * 4 Egyéb megjegyzések a témával kapcsolatban

  
---  
  
##  Bevezetés

Ez egy alap útmutató. Abban segít, hogy létre tudjunk hozni saját komponenseket. A művelet Windows 7 -en lett tesztelve Lazarus 0.9.30 segítségével. 

##  1\. lépés: Csomag létrehozása

  * A Lazarus IDE menüjén kattints a **Package > New Package** menüelemre, hogy futtasd Package Manager -t. 



[![package menu.png](https://wiki.freepascal.org/images/f/fb/package_menu.png)](</File:package_menu.png>)

[![package menu \(lazarus-1.0 RC1-fpc-2.6.0-win64\).png](https://wiki.freepascal.org/images/c/ce/package_menu_%28lazarus-1.0_RC1-fpc-2.6.0-win64%29.png)](</File:package_menu_\(lazarus-1.0_RC1-fpc-2.6.0-win64\).png>)

  


  * Egy **Save dialog** fog megjelenni. Válassz egy mappát, és egy fájlnevet majd nyomd meg a save(mentés)-t. Ékezetes elnevezést ne használj. 



[![save dialog \(lazarus-1.0 RC1-fpc-2.6.0-win64\).png](https://wiki.freepascal.org/images/5/51/save_dialog_%28lazarus-1.0_RC1-fpc-2.6.0-win64%29.png)](</File:save_dialog_\(lazarus-1.0_RC1-fpc-2.6.0-win64\).png>)

Ha az IDE szól, hogy kisbetűs legyen a fájlnév, nyomj 'Yes'-t. 

[![lowercase filenames\(lazarus-1.0 RC1-fpc-2.6.0-win64\).png](https://wiki.freepascal.org/images/a/ab/lowercase_filenames%28lazarus-1.0_RC1-fpc-2.6.0-win64%29.png)](</File:lowercase_filenames\(lazarus-1.0_RC1-fpc-2.6.0-win64\).png>)

  * És gratulálok, elkészítetted az első csomagod. 



[![How to write lazarus component package maker\(lazarus-1.0 RC1-fpc-2.6.0-win64\).png](https://wiki.freepascal.org/images/1/16/How_to_write_lazarus_component_package_maker%28lazarus-1.0_RC1-fpc-2.6.0-win64%29.png)](</File:How_to_write_lazarus_component_package_maker\(lazarus-1.0_RC1-fpc-2.6.0-win64\).png>) [![Package Maker](https://wiki.freepascal.org/images/3/37/How_to_write_lazarus_component_package_maker.png)](</File:How_to_write_lazarus_component_package_maker.png> "Package Maker")

##  2\. lépés: Unit készítése

Csinálhatsz egy új unitot vagy használhatsz egy már meglévőt. 

###  Új unit készítése

  * Használd az **Add button > New component** lehetőséget. 



[![package new component.png](https://wiki.freepascal.org/images/6/60/package_new_component.png)](</File:package_new_component.png>)

  * Válassz egy komponenst, például TComboBox. 
  * Válaszd mondjuk például a _customcontrol1.pas_ fájlnevet. Ékezeteket itt se használj. 
  * Kattints OK gombra. 


  * A forráskód szerkesztőben az alábbi kód jelenik meg. A példa egy komponens létrehozását mutatja be, ezért most a kódhoz nem nyúlunk. 


    
    
    unit CustomControl1;
     
    {$mode objfpc}{$H+}
     
    interface
     
    uses
      Classes, SysUtils, LResources, Forms, Controls, Graphics, Dialogs, StdCtrls;
     
    type
      TCustomControl1 = class(TComboBox)
      private
        { Private declarations }
      protected
        { Protected declarations }
      public
        { Public declarations }
      published
        { Published declarations }
      end;
     
    procedure Register;
     
    implementation
     
    procedure Register;
    begin
      RegisterComponents('Standard',[TCustomControl1]);
    end;
     
    end.

  


  * Telepítsd a csomagot az 'Install' gombbal amely a package editor tetején van. 



[![package install.png](https://wiki.freepascal.org/images/2/2a/package_install.png)](</File:package_install.png>)

  * Figyelem! Például a Lazarus-1.0_RC1 -ben az 'Install' már máshol található! 



[![package install \(lazarus-1.0 RC1-fpc-2.6.0-win64\).png](https://wiki.freepascal.org/images/d/d3/package_install_%28lazarus-1.0_RC1-fpc-2.6.0-win64%29.png)](</File:package_install_\(lazarus-1.0_RC1-fpc-2.6.0-win64\).png>)

  * Utána az IDE meg kérdezi tőled, hogy maga az IDE újra fordítódjon-e. Nekünk most ez kell, kattints a 'Yes' -re. 



[![package rebuild \(lazarus-1.0 RC1-fpc-2.6.0-win64\).png](https://wiki.freepascal.org/images/b/b0/package_rebuild_%28lazarus-1.0_RC1-fpc-2.6.0-win64%29.png)](</File:package_rebuild_\(lazarus-1.0_RC1-fpc-2.6.0-win64\).png>)[![package rebuild.png](https://wiki.freepascal.org/images/6/67/package_rebuild.png)](</File:package_rebuild.png>)

  * Fordítás után újraindul a Lazarus, és látnod kellene a komponens palettán az újonnan telepített saját komponensed. Gratulálok: Ezzel telepítetted az első csomagod az első komponenseddel. 



[![package installed.png](https://wiki.freepascal.org/images/7/79/package_installed.png)](</File:package_installed.png>)

  * _Megjegyzés:_ Ha nem látod az új komponensed a komponens palettán: a legtöbb esetben ez azért van mert neked nem az újrafordított Lazarus fut. Be kell állítanod mondjuk, hogy a Lazarus melyik mappába fordítódjon le: Kattints a Tool-> Options-> Environment -> Environment options -> Files -> Lazarus directory(default for all porjects). Ahelyett hogy a Lazarust közvetlen hívnád, használhatod a startlazarus, ezzel valóban az újonnan fordított Lazarust indítod. Például a Lazarus futtatható bináris benne van a ~/.lazarus mappában, ha nincs írási jogod erre a mappára, akkor hiába fordítasz, a fordítatlan Lazaruson kívül mást nem tudsz futtatni. 



##  Egyéb megjegyzések a témával kapcsolatban

1\. Új komponens (csomag) készítésénél figyeljünk oda az ékezetes nevekre. Ne használjuk azokat. Csomagunk mentésénél például ne csináljunk ilyet: d:\Program Files\Lazarus\Gyakorlás\ <-(á betű!) Emiatt később abba a hibába üktüzünk, hogy a fordító nem fogja találni a Gyakorlás mappánkat, és végső soron a Lazarus nem fog újból lefordítódni a csomagunkkal. 

[![error1 \(lazarus-1.0 RC1-fpc-2.6.0-win64\).png](https://wiki.freepascal.org/images/f/fa/error1_%28lazarus-1.0_RC1-fpc-2.6.0-win64%29.png)](</File:error1_\(lazarus-1.0_RC1-fpc-2.6.0-win64\).png>)

---

_Source: [https://wiki.freepascal.org/Hogyan_%C3%ADrjunk_Lazarus_komponenst](https://web.archive.org/web/20121104052403/https://wiki.freepascal.org/Hogyan_%C3%ADrjunk_Lazarus_komponenst)_
