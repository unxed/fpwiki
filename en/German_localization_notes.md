# German localization notes

_This page contains some useful notes and guidelines specific to the German translation of the Lazarus IDE. Since such information is heavily language specific and targeted towards a German speaking translator, the following text is written in German._

## Contents

  * 1 Einführung
  * 2 Lazarus übersetzen
    * 2.1 Ziel der Übersetzung
    * 2.2 Übersetzungsvorgang
    * 2.3 Lazarus-spezifische Dateien für die dt. Übersetzung
    * 2.4 Software
  * 3 Allgemeines bei dt. Übersetzungen
  * 4 Richtlinien für die deutsche Lazarus-Übersetzung
    * 4.1 Wortwahl und Formulierungen
    * 4.2 Kleinigkeiten
    * 4.3 GUI-spezifisches
    * 4.4 Oft verwendete Übersetzungen
  * 5 Die besten Übersetzungen und Wortschöpfungen ;-)
  * 6 Interessante Originaltexte



## Einführung

Diese Wiki-Seite enthält (hoffentlich) nützliche Informationen für alle, die bei der deutschen Übersetzung mithelfen wollen. Dies umfasst generelle Informationen über den Übersetzungsvorgang, verwendbare nützliche Software um den Prozess zu vereinfachen, Hinweise für den eigentlichen Vorgang und Informationen über bereits verwendete Übersetzungen für einzelne Begriffe. 

## Lazarus übersetzen

In diesem Abschnitt wird kurz das Ziel und der eigentliche Vorgang skizziert, und danach auf spezielle Software zur Arbeitserleichterung verwiesen. 

### Ziel der Übersetzung

Das Ziel der Übersetzung der Texte des Programmes ist zusammengefasst ein professioneller Eindruck: Das umfasst ordentliche Rechtschreibung und Interpunktion und eine dem Programm und dem Benutzer angemessene, gewohnte und möglichst konsistente Wortwahl. 

In den nächsten Abschnitten wird versucht, hoffentlich nützliche Hinweise und bereits angewandte Richtlinien zur Erreichung dieses Ziel zu zeigen. 

### Übersetzungsvorgang

Lazarus verwendet zur Zeit den GNU gettext()-Mechanismus für seine Übersetzungen. 

Bei diesem werden die vom Programmierer zur Übersetzung vorgesehenen Zeichenketten in einer speziellen Datei mit der Endung .po abgelegt. Dies ist eine einfache Textdatei in dem die zu übersetzenden Zeichenketten nacheinander abgelegt sind. 

Beispiel für eine zu übersetzende Zeichenkette im Ursprungszustand (aus der .po-Datei der Lazarus-IDE): 
    
    
    #: lazarusidestrconsts:lismenuproject
    msgid "&Project"
    msgstr ""
    

Die erste Zeile ist ein Kommentar (alle mit '#' beginnenden Zeilen sind Kommentare), der bei der von Lazarus gewählten Methode zur Erstellung der .po-Datei erzeugt wird, und ignoriert werden kann. 

In der zweiten Zeile befindet sich zwischen den Anführungszeichen die zu übersetzende Zeichenkette. Das in diesem Fall vorhandene '&'-Zeichen gibt, da es sich um einen Menüeintrag handelt, dessen Kurztaste an. 

Die dritte Zeile muss vom Übersetzer mit der richtigen Übersetzung ausgefüllt werden. 

Dieser Vorgang wird für möglichst alle Zeichenketten in der Datei wiederholt. Dann wird das zu übersetzende Programm gestartet, die gerade übersetzte Sprache eingestellt und das Ergebnis bewundert... 

... und in der Theorie wäre das Ergebnis dann auch ordentlich. 

In der Praxis stellt sich häufig das Problem, dass sich die Übersetzung an den Originaltext hält, dann aber, da vom Kontext losgelöst übersetzt wurde, doch oft völlig unsinnig ist. Außerdem ist so eine Übersetzung, besonders wenn im Team durchgeführt, uneinheitlich. Das ist auch ein Grund weswegen diese Wiki-Seite entstand, siehe weiter unten. 

Vorher noch ein paar technische Anmerkungen: 

### Lazarus-spezifische Dateien für die dt. Übersetzung

Die für Lazarus zu übersetzenden .PO-Dateien befinden sich im Unterverzeichnis "languages" des Hauptprogramms, mit den Namen "lazaruside.de.po" (für die IDE selbst) und "objinspstrconsts.de.po" (für den Objektinspektor). Die Sprachdateien für Synedit sind in components/synedit, die Datei für die Codetools ist in components/codetools/languages, die Sprachdatei für lcl-Dateien in lcl/languages. 

### Software

Das Arbeiten mit einem Texteditor ist recht mühsam und außerdem sehr fehlerträchtig, daher werden in diesem Abschnitt ein paar verwendete Übersetzungshilfen mit ihren Funktionen kurz vorgestellt: 

  * poEdit (<http://poEdit.sourceforge.net/>) - folgende Funktionalität: 
    * Gleichzeitige Ansicht von Original und Übersetzung in einer Liste.
    * Farbliche Unterscheidung zwischen bereits übersetztem Text, noch zu übersetzendem Text und gerade übersetztem Text.
    * Die Zuweisung zusätzlicher Attribute (frisch übersetzt, überprüft usw.) ist möglich.
    * Automatische Erstellung von .MO-Dateien.
    * Verfügbar für Windows und Unix/Linux.


  * KBabel (<http://i18n.kde.org/tools/kbabel/>): 
    * nur für Unix/Linux.


  * POEditor (<http://poeditor.com/>) 
    * webbasierte Lokalisierungstool.



## Allgemeines bei dt. Übersetzungen

In diesem Abschnitt werden kurz einige allgemeine Regeln für die Erstellung von Übersetzungen ins Deutsche erwähnt. 

  * In jedem Teilprojekt sollte abgesprochen werden, welche Rechtschreibung intern gilt. In der IDE ist das die klassische deutsche Schreibung wie sie heute in der FAZ und der Toolbox Anwendung findet ;-)
  * Die Rechtschreibung sollte durchgängig korrekt sein. Dazu ist es oft notwendig, besonders bei Unklarheiten, in einer verlässlichen Quelle nachschlagen zu können. Für die deutsche Sprache sollte der Duden in einer nicht zu alten Ausgabe die Hauptreferenz darstellen. Da mit der Rechtschreibreform das Duden-Monopol gebrochen ist, sind auch andere Nachschlagewerke zulässig. Pseudo-"Quellen" wie allgemeine Suchmaschinen (Google etc.) sind nicht maßgebend und sollten nicht konsultiert werden.
  * Die Interpunktion sollte sich an die Standards halten. Stark vereinfacht: Für das Deutsche sind alle Satzzeichen wie Punkt, Komma, Ausrufezeichen usw. direkt an das vorherige Wort zu schreiben, mit _einem_ nachfolgendem Leerzeichen. Genaueres kann unter folgendem Link zu [Textverarbeitung](<http://www.duden.de/service/download/textverarbeitung_duden1.pdf>) nachgeschlagen werden.



Und hier noch ein wichtige speziellere Anmerkung: 

  * falls eine bestimmte Wortwahl unglücklich gewählt scheint, sollen eine (oder besser mehrere) professionelle Anwendungen, die eine ähnliche Funktionalität enthalten, nach deren Übersetzung untersucht werden. Dies hilft besonders bei Spezialbezeichnungen von Fachbegriffen oft weiter.



## Richtlinien für die deutsche Lazarus-Übersetzung

Hier folgen viele Hinweise für die "richtige" Übersetzung speziell der Lazarus-IDE, die sich durch ihre Anwendung bei der aktuellen deutschen Übersetzung als gut herausgestellt haben. Es enthält Erfahrungen aus einigen anderen Anwendungen und Ausprobieren verschiedener Stile, Bemerkungen über häufig vorkommende Fehler in vorigen Versionen der Übersetzung und durch Diskussion mit den Entwicklern erhaltene Information. 

### Wortwahl und Formulierungen

  * kein modernes l33t-speak, auch "lockere" Sprache vermeiden. Eine Meldung


    
    
    "Uupsi, leider hat der Debugger Mist gebaut"
    

klingt lächerlich, wenn nicht gar peinlich für den Benutzer. 

  * Anglizismen (Denglish) ebenso vermeiden, insbesondere wenn es ein vollständig gleichwertiges Wort oder Ausdruck im Deutschen gibt. Beispiel:


    
    
    "Viewer" -> "Betrachter"
    

Und nicht "Viewer" einfach belassen. 

  * höflich, sachlich formulieren, wenn notwendig "Sie"-zen und nicht "Du"zen.
  * Meldungen werden oft verwendet um dem Benutzer ein Problem zu beschreiben. Es ist vorteilhaft, wirklich das Problem trotz vielleicht anderer Meinung des Originalautors und nicht zum Beispiel den Zustand der IDE zu beschreiben.
  * Wort-für-Wort-Übersetzungen sind nicht notwendig und manchmal nicht einmal erstrebenswert - die Originalautoren sind auch nur Menschen, deren Gedanken zum Schreibzeitpunkt sicher eher bei technischen Problemen lagen
  * das gleiche gilt für die Satzstellung, schon um seltsam klingende, aber grammatikalisch korrekte, Formulierungen zu vermeiden. Dabei muss wegen Einschränkungen der Lokalisierungsbibliothek aber darauf geachtet werden, dass die Platzhalter die gleiche relative Position wie vorher behalten.
  * falls man keine Ahnung hat, was ein englischer Text bedeutet, folgendes versuchen: 
    * Aus dem Formular dessen Funktion herauslesen: Text in der IDE suchen, ansehen und aus diesem auf die Funktion schließen.
    * Im Quelltext nachsehen: zuerst nach der Zeichenkette suchen, das liefert einen Namen einer Konstante in "lazarusidestrconsts.pas". Mit diesem Bezeichner in den Sourcen weiter suchen, und versuchen, aus dem Code die Funktion abzuleiten - wenn man viel Glück hat, findet man auch eine Erklärung im Source.
    * Fragen. Dazu kann entweder die Mailingliste (Link) dienen, der IRC-Kanal (#lazarus-ide@irc.freenode.org) oder das Forum (Link)
    * Nicht übersetzen: Es gibt kaum Schlimmeres als ein vollständig falsch oder nur teilweise übersetzter Satz. Auch besteht die Gefahr, dass dieser Text bei einer weiteren Bearbeitung von anderen übersehen wird.


    
    
    "All packages" -> "Alle Packages"
    

Besser erst einmal auslassen und entweder auf eine Eingebung warten oder einer anderen Person überlassen. Besser eine gute und vielleicht etwas lückenhafte Übersetzung (zur Not sollte jeder Benutzer einer IDE mit Englisch zurechtkommen) als auf Teufel komm' raus alles übersetzen. 

  * Die Sprache verwenden - Deutsch ist flexibel in der Bildung zusammengesetzter Worte. Solange diese nicht zu Wortmonstern über mehrere Zeilen ausarten, sollten sie anstatt umständlich wirkender Umschreibungen verwendet werden. Außerdem sind Konkatenierungen immer kürzer, und Platz ist sowieso meist Mangelware. Eventuell Wortteile, die übersetzte und nicht übersetzte Teile (wegen eines Fachbegriffs) enthalten, mit einem Bindestrich trennen.


    
    
    "Form load error" -> "Formularladefehler"
    

nicht 
    
    
    "Fehler beim Laden des Formulars"
    

für ein (zugegeben extremes) Beispiel; da speziell dieses Beispiel wahrscheinlich nur in einem Erklärungstext vorkommt, könnte man das auch so lassen. 

  * Bei der Übersetzung dürfen (und müssen!) Umlaute geschrieben werden.
  * Nicht wie oft im Original (Englisch) jedes Wort großschreiben (dort u.U. erlaubt):


    
    
    Alle Meine Entlein Sind Schon Da
    

sondern richtig 
    
    
    Alle meine Entlein sind schon da
    

Wir sind hier nicht bei der Werbung =) 

  * bezüglich der verwendeten Wortstellung: 
    * bei Menübefehlen möglichst die Wortstellung Subjekt-Verb (Infinitiv) verwenden, bei Meldungen der IDE bei (länger dauernden) Aktionen eher Verb-Subjekt.


    
    
    Lazarus erstellen
    

Der Befehl an das Programm: "(Du sollst) Lazarus erstellen". Leicht lesbar, und eigentlich Standard für diese Art von Texten. 

  *     * Andererseits bei Benachrichtigungen der IDE ist


    
    
    Erstelle Lazarus
    

als Meldung des Programmes an den Benutzer; sozusagen "Erstelle (gerade) Lazarus" als Meldung bei laufendem Vorgang. Auch ist oft die letztere Form länger, da das Verb dann im Deutschen oft aufgeteilt werden muss, im folgenden ein Beispiel: 
    
    
    Datei abspeichern
    

besser: 
    
    
    Datei speichern
    

Bzw. 
    
    
    Speichere Datei ab
    

es ist bei Benachrichtigungen der IDE oft mehr Platz vorhanden, daher möglich. 

  * Mehrfachnegationen in Texten wenn möglich vermeiden


    
    
    "Die Komponente darf nicht auf einem Nicht-TControl abgelegt werden."
    

besser: 
    
    
    "Die Komponente darf nur auf einem TControl abgelegt werden."
    

Ist sogar um einiges kürzer, und sicher leichter verständlich. 

  * Spezifische Verweise auf die LCL nicht übersetzen.


    
    
    "ImageList-Editor" -> "ImageList-Editor"
    

Dies ist ein Editor für eine bestimmte Klasse namens TImageList. Diese sollte nicht übersetzt werden ("Bildlisteneditor") oder durch zufällig eingeführte Kleinschreibung übersetzt werden ("Listview-Editor") da es sich eben um einen Editor für eine bestimmte Klasse handelt. 

  * Aufpassen auf Einzahl und Mehrzahl im Originaltext. Besonders unregelmäßige Endungen werden oft übersehen.
  * Kurztasten in Menüs möglichst beibehalten, ansonsten besteht die Gefahr von Überschneidungen.


    
    
    "&Add breakpoint" -> "H&altepunkt hinzufügen"
    

  * Unübliche Abkürzungen vermeiden 
    * "+" ist kein Ersatz für "und", und "plus" ist erst recht keine Übersetzung


    
    
    "Zieldateiname+Erweiterung..."
    

  * Besondere Vorsicht beim Übersetzen von sehr kurzen Sätzen


    
    
    "As-Is" 
    

ist nicht 
    
    
    "Wie es ist"
    

sondern soll für die Pascal-Schlüsselwörter "as" und "is" stehen. In solchen Fällen immer den Dialog suchen. 

### Kleinigkeiten

  * Die offizielle, standardisierte Einheit für Zeit ist "s". Nicht "sek", "sec" oder ähnliches.
  * Rufzeichen im Originaltext können oft durch einen einfachen Punkt ersetzt werden.
  * Der Name des verwendeten Compilers lautet "Free Pascal". Die offizielle Abkürzung ist "FPC".
  * Wenn möglich, sollten in Fließtexten Abkürzungen vermieden werden, da sie den Schreibfluss hemmen. "Sekunde" ist also noch besser als "s".
  * Viele Worte sind eigentlich nur Füller. Dazu gehören insbesondere "verwenden" oder auch "erstellen". Sie sollten durch die eigentliche Tätigkeitsbeschreibung ersetzt werden, also bespielsweise "kompilieren" oder "schreiben" statt erstellen.
  * neue Rechtschreibung: "dass", aber "außerdem"
  * der, die, das bei Relativsätzen und nicht welcher, welche, welches



### GUI-spezifisches

Dieser Abschnitt enthält spezielle Richtlinien für Beschriftungen und Texte. 

  * Bezeichnungen von Karteireitern und gruppierenden Elementen (TGroupBox usw.) haben keinen abschließenden Doppelpunkt
  * Bezeichnungen von Drop-Down-Listen oder Eingabefeldern sollen einen abschließenden Doppelpunkt haben
  * Niemals mittels Leerzeichen formatieren, gleich richtig positionieren. Das Problem sind sowohl abschließende Leerzeichen, die beim Übersetzungsvorgang einfach verlorengehen, als dass, was auch viel schlimmer ist, bei Änderungen der Schriftart die Abstände nicht mehr passen.
  * Hinweise auf Menüs in Fließtext sollten durch Anführungszeichen ausgezeichnet, und die Teile der Pfadangabe durch "->" getrennt werden.


    
    
    Bitte überprüfen Sie die Angabe in "Einstellungen->Umgebungseinstellungen->Dateien"
    

  * Menüeinträge, die Fenster öffnen, sollten im Normalfall drei Punkte am Ende enthalten. Beispiel:


    
    
    Speichern unter...
    

Öffnet einen Dialog zum Speichern von Dateien unter einem anderen Namen. 

### Oft verwendete Übersetzungen

In diesem Abschnitt werden oft verwendete Begriffe die im Zusammenhang mit einer IDE auftreten, gesammelt, und mit deren "Standardübersetzungen" aufgelistet. 

Hauptwörter: 

  * "ancestor type" -> "Basistyp"
  * "backup" -> "Sicherung"
  * "block" -> "Block" (im Sinne von Quelltextblock)
  * "bookmark" -> "Lesezeichen"
  * "breakpoint" -> "Haltepunkt"
  * "call stack" -> "Aufrufstack"
  * "character map" -> "Zeichentabelle"
  * "child node" -> "Untereintrag" (in einer Liste, einem Baum)
  * "class" -> "Klasse"
  * "code explorer" (und Varianten) -> "CodeExplorer" - mangels guter Übersetzung diesen als Spezialausdruck behandeln
  * "code" -> "Quelltext"
  * "command" -> "Anweisung" oder "Befehl"
  * "compiler" -> "Compiler"
  * "component" -> "Komponente"
  * "declaration" -> "Deklaration"
  * "default" -> "Standard" oder "Voreinstellung"
  * "dependency" -> "Abhängigkeit"
  * "description" -> "Beschreibung"
  * "directory" -> "Verzeichnis"
  * "file extension" -> "Dateiendung"
  * "file name" oder "filename" -> "Dateiname"
  * "font" -> "Schriftart"
  * "form" -> "Formular"
  * "gutter" -> "Randleiste" (die normalerweise graue Randleiste links im Quelltexteditor)
  * "identifier" -> "Bezeichner"
  * "item" -> "Eintrag" (einer Liste, eines Baumes) oder "Element"
  * "key" -> "Taste"
  * "keyword" -> "Schlüsselwort"
  * "lazarus" (alle Varianten) -> "Lazarus"
  * "library" -> "Bibliothek"
  * "macro" -> "Makro"
  * "node" -> "Eintrag" (in Bäumen)
  * "object" -> "Objekt"
  * "options" -> "Einstellungen"
  * "package" -> "Paket"
  * "project" -> "Projekt"
  * "resource" -> "Ressource"
  * "resourcestring" -> "Resourcestring" (Spezialbegriff für Zeichenketten im resourcestring-Abschnitt einer Unit
  * "section" -> "Abschnitt" (in Kontext als Abschnitt im Quellcode)
  * "selection" -> "Auswahl"
  * "source" -> "Quelltext"
  * "stack" -> "Stack"
  * "statement" -> "Anweisung"
  * "syntax highlighting" -> "Syntaxhervorhebung"
  * "syntax" -> "Syntax"
  * "template" -> "Vorlage"
  * "tool(s)" -> "Werkzeug(e)"
  * "type" -> "Typ"
  * "unique" -> "einzigartig"
  * "unit name" -> "Unitname"
  * "unit" -> "Unit"
  * "user" -> "Benutzer"
  * "variable" -> "Variable"
  * "view" -> "Ansicht)
  * "viewer" -> "Betrachter"
  * "watch" -> "Überwachter Ausdruck"
  * "xyz file" -> ".xyz-Datei"



Verben: 

  * "to add" -> "aufnehmen" oder "hinzufügen"
  * "to build" -> "erstellen"
  * "to clean" -> "säubern" oder "aufräumen" (Im Zusammenhang mit Löschen der alten Daten)
  * "to compile" -> "kompilieren"
  * "to convert" -> "konvertieren" oder "umwandeln"
  * "to paste" -> "einfügen" (in Zusammenhang mit Zwischenablage)
  * "to publish" -> "veröffentlichen"
  * "to rebuild" -> "neu erstellen"
  * "to register" -> "registrieren"
  * "to scan" -> "durchsuchen" (in Zusammenhang mit Daten)
  * "to select" -> "auswählen"
  * "to transform" -> "umwandeln"
  * "to update" -> "aktualisieren"



Sonstiges: 

  * "for example" -> "Zum Beispiel" oder "Z.B." oder "beispielsweise"
  * "major.minor.release.build" -> "Major.Minor.Release.Build" (Spezialbegriff)
  * "Plz fix the error shown in the message window." -> "Bitte korrigieren Sie den im Meldungsfenster gezeigten Fehler."
  * "Sorry" -> "Entschuldigung"
  * "unable to <verb> xyz" -> "Konnte xyz nicht <verb>"
  * "use display" -> "Display verwenden" (Display ist eine X-Windows spezifische Umgebungsvariable)
  * "broken dependency" -> "Abhängigkeit nicht erfüllt"



Überlegenswerte Punkte 

  * "ambiguous" -> "unklar" oder "mehrdeutig"



## Die besten Übersetzungen und Wortschöpfungen ;-)

keine 

## Interessante Originaltexte

  * symantec checking - Symantecs Beitrag zu Lazarus - ein integrierter Quelltextvirenscanner ;-)

---

_Source: [https://wiki.freepascal.org/German_localization_notes](https://web.archive.org/web/20241210000140/https://wiki.freepascal.org/German_localization_notes)_
