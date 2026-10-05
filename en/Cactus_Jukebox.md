# Cactus Jukebox

## Contents

  * 1 About
  * 2 Downloads
  * 3 Development
    * 3.1 Subversion
  * 4 Gallery
  * 5 See also



## About

Cactus Jukebox is an audio player and music organizing tool. It searches given folders for music files and shows them in a database. The main focus is on usability and speed. Cactus Jukebox still runs smoothly with 15GB of music files on an old computer like a P3 @ 500MHz. There's no reason to take a dual core machine just to listen to your music. Another nice feature: You can also syncronize your music collection with a mobile player device(USB). 

Features: 

  * Fast database
  * Audio CD ripper
  * Cover download from Last.fm
  * Webradio support
  * Upload/synchronize to mobile players
  * Plattform independent(GTK, GTK2, win32)
  * Browse your music collection by artist, albums and titles
  * Save and load m3u playlists
  * ID3 tag support
  * Easy tagging of mp3-files



Ideas for next versions: 

  * Support flac, wma
  * Audio CD Player
  * Burn CD's
  * Support more plattforms(MacOSX, QT,...)
  * And more... waiting for your suggestions



## Downloads

Cactus Jukebox uses MPlayer as playback engine. So take care mplayer is installed on your system before running Cactus Jukebox. Old libfmod is still supported but not recommended. 

**Linux**

_32bit - i386_

  * [cactusjukebox-0.4.2stable-gtk2_i386.deb](<https://web.archive.org/web/20120324090502/http://www.cactusjukebox.org/data/cactusjukebox-0.4.2stable-gtk2_i386.deb>) (K)Ubuntu, Debian package
  * [cactusjukebox-0.4.2stable-gtk2_i386.tar.gz](<https://web.archive.org/web/20120324090502/http://www.cactusjukebox.org/data/cactusjukebox-0.4.2stable-gtk2_i386.tar.gz>) zipped binary



_64bit - amd64_

  * [cactusjukebox-0.4.2stable-gtk2_amd64.deb](<https://web.archive.org/web/20120324090502/http://www.cactusjukebox.org/data/cactusjukebox-0.4.2stable-gtk2_amd64.deb>) (K)Ubuntu, Debian package
  * [cactusjukebox-0.4.2stable-gtk2_amd64.tar.gz](<https://web.archive.org/web/20120324090502/http://www.cactusjukebox.org/data/cactusjukebox-0.4.2stable-gtk2_amd64.tar.gz>) zipped binary



**Windows (still experimental)**

[cactusjukebox-0.4.1stable-win32.zip](<https://web.archive.org/web/20120324090502/http://www.cactusjukebox.org/data/cactusjukebox-0.4.1stable-win32.zip>) zipped binary 

GTK1.x binaries can still be build from source but there are no more packages yet. All current distributions dropped GTK1.x support. If you still need a GTK1.x version please contact me. If there is enough demand I will create official GTK1.x packages again... 

QT4 interface is nearly ready for release... No big showstoppers, just some minor graphic bugs to fix. You may try it out by building from source code. 

## Development

The original developer of the Cactus Jukebox was Sebastian Kraft (sebastian_kraft@gmx.de) but he is no longer developing the project. Now it has been moved to the Lazarus-ccr and the current maintainer is Felipe Monteiro de Carvalho, but if anyone is interrested in taking over this position I'd gladly hand it over, since I don't have much time to spend in this project, I am just improving it a little since I use it as my daily music player. 

The main things to do now are fixing freezes which happen, adding a more iTunes like interface with only 1 list of musics and 1 directory treeview instead of 3 lists. Also interresting would be adding new modules for basing on fpsound instead of mplayer, but only after fpsound can actually play mp3. 

### Subversion

The sourcecode is available in the lazarus subversion repository(SVN). 

Initial checkout with the command: 

svn co <https://sourceforge.net/p/lazarus-ccr/svn/HEAD/tree/applications/cactusjukebox/> cactusjukebox 

after that you can always update your local copy doing "svn update" 

## Gallery

[![Cactus jukebox.png](https://wiki.freepascal.org/images/a/a8/Cactus_jukebox.png)](</File:Cactus_jukebox.png>)

## See also

  * [Free Pascal Application Suite](<Free_Pascal_Application_Suite.md> "Free Pascal Application Suite")

---

_Source: [https://wiki.freepascal.org/Cactus_Jukebox](https://web.archive.org/web/20250121225806/https://wiki.freepascal.org/Cactus_Jukebox)_
