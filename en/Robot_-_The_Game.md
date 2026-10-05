# Robot - The Game

│ **[Deutsch (de)](</Robot_-_The_Game/de> "Robot - The Game/de")** │  **English (en)** │  **[français (fr)](</Robot_-_The_Game/fr> "Robot - The Game/fr")** │    
****

## Contents

  * 1 The Game
  * 2 Screenshots
  * 3 Code
  * 4 Control
  * 5 Control of Editor
  * 6 Versions
  * 7 Download
  * 8 License
  * 9 Author



## The Game

The idea originated in an old DOS game from TOM productions (homepage with download (it's a nice game!)): <http://www.tom-games.de/eng/download.htm>). You control a character through a world consisting of 5*5 rooms, each with 20*20 fields. Every field contains a special game object; this can be a wall, a bad robot who wants to kill you (because of the robots, the game is named Robot), a door (for each door, you need a special key) and so on. 

## Screenshots

Inside of Robot 1.5 while playing: 

[![robot1.5-shot1.png](https://wiki.freepascal.org/images/e/e2/robot1.5-shot1.png)](</File:robot1.5-shot1.png>)

Startscreen of Robot 1.7: 

[![robot1.7-shot1.png](https://wiki.freepascal.org/images/e/ec/robot1.7-shot1.png)](</File:robot1.7-shot1.png>)

Editor-mode of Robot 1.7: 

[![robot1.7-shot2.png](https://wiki.freepascal.org/images/7/71/robot1.7-shot2.png)](</File:robot1.7-shot2.png>)

Lazarus + Robot 1.7 under MacOSX (Carbon interface): 

[![Robot2mac 2.png](https://wiki.freepascal.org/images/a/af/Robot2mac_2.png)](</File:Robot2mac_2.png>)

## Code

The code was written to demonstrate the possibilities with a very small set of programming techniques. The code doesn't use OOP, it is completely procedural (but embedded into one little form). I started this project while I was showing Lazarus to a friend to show him how easy it is to program a little application. It is not very difficult to teach a little bit of procedural and imperative programming so that you can do a little bit stuff. Then I said, "I show you what you can do with this"... 

The code is well documented in English. 

## Control

Movement is controlled by the arrow-keys. You can pick up things lying around by simply going to them. They will be put into your knapsack. Select the thing in your knapsack you want to use with spacebar. You can use them by pressing enter. There are corrosive liquids filled in bottles lying around. You can remove walls with them (only the bright walls). To go thru a door you must have a key of the right color. You won't need to select the key - just having the right key in your knapsack is sufficient. There are 3 diamonds lying around somewhere. You need them to defeat the evil king. You have to put them near of the diamond-places (go there and select it in your knapsack). You can save the game with the clocks lying around. ... 

## Control of Editor

Go into the editor mode (menu). All game-objects are listed in your knapsack. Simply select an object and place it in the present room by left-clicking at the wanted position. You can also select the objects in the knapsack by clicking on them. With a right-click, you can remove an object. With Ctrl+Arrowkey, you can switch to other rooms. Don't forget to come back to the room where your body is, because else, you can't resume playing on exiting the editor mode. 

## Versions

The game itself is in German: all messages inside of the game (a little description and a few state messages) are German. Perhaps you can translate it? :) 

It should not disturb so much (I give you a description...) and the interesting part is also the source code. 

Versions from 1.5 to 1.7 are available at the moment. The code is extended and bugfixed a little bit in the newer version, but there is no real big difference. Version 1.7 contains an in game editor now, and there is a new world since version 1.7 (feel free to design your own worlds or perhaps a whole new game based on this engine; it should not be difficult). 

## Download

You can download the Robot 1.7 source and binaries for Windows, Linux (i386 and powerpc) from [Lazarus-CCR Sourceforge site](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=148359&release_id=440138>). 

The homepage of the project is <http://www.az2000.de/projects/robot2/?lang=en>, where you can find additional information. 

There is also a German [beginner tutorial for programming](<http://www.az2000.de/docs/coding_for_dummies>) which uses this game as one of the main examples. 

If you have questions, simply ask me. 

## License

It is under the LGPL. 

## Author

[Albert Zeyer](</index.php?title=User:Albert&action=edit&redlink=1> "User:Albert \(page does not exist\)")

---

_Source: [https://wiki.freepascal.org/Robot_-_The_Game](https://web.archive.org/web/20241207122759/https://wiki.freepascal.org/Robot_-_The_Game)_
