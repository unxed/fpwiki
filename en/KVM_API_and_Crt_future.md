# KVM API and Crt future

Recently, several people have shown interest in the working of the KVM API (Keyboard/Video/Mouse) units and fixing the problems of the Crt implementation for Unix and other platforms. I'm writing here some parts of the bigger picture, and how I see the future of these units. 

Since most of the problems lie in the Unix side of the spectrum, mostly due to the multitude of terminals, libraries and variants, so this topic mostly focuses on unix. However, as long as KVM units are supposedly available for other platforms, implementation based on those units might become completely platform independent. 

## Contents

  * 1 The original purpose of KVM
  * 2 The origins of Crt
  * 3 The problems
  * 4 What needs to be done
  * 5 console detection you say, what do you mean by that?
  * 6 References



## The original purpose of KVM

The original purpose of the KVM units was simply to be the OS dependent part of Free Vision (FV), the Turbo Vision (TV) clone that is used by the textmode IDE (from here on simply IDE). The interface is reasonably clean though, and there is potential for use without Free Vision or the IDE. Since DOS applications can read from the screen, video keeps track of a virtual screen that can be read. 

The base principle of FV is being event driven, and this also had its effect on the KVM API. The system allows for non-blocking polling of events. Also the requirements of the IDE/FV can be seen in the feature set offered by KVM: it tries to cater for all features that were used by Turbo Vision programs, including harder to port properties like being able to use nearly all keys on the keyboard, keyboard shift state and reading the screen. These properties in particular make the KVM API hard to port on Unix, but also give the FPC textmode apps a fairly rich textmode user experience. 

On Unix, the KVM units use the "terminfo" library (part of ncurses) for some basic terminal database access, but not ncurses itself. For mouse support, only GPM is supported, with some xterm tweaks. 

## The origins of Crt

The Crt unit is a standard Turbo Pascal unit to random access write to the screen using direct memory mapped access. A lot of Turbo Pascal programs use it. The non Unix systems (DOS, OS/2, Win32) have a reasonable implementation already, except that they are based on completely standalone codebases and partly duplicate functionality already provided in KVM, but the Unix version has always been a problem. In the FPC subversion repository there are currently two variants, and a third version is proposed. 

  * the default crt unit (rtl/unix/crt.pp), which is an attempt to create a Crt unit for (mostly) Linux, and targeted at the text mode console of Linux. Its main attraction is its simplicity, and being standalone.
  * the ncurses based ncrt unit. However Ncurses only supports the lowest common denominator on Unix without additional code. Moreover it already has its own windowing system, and very complex double buffering (that is not really necessary).
  * a KVM units based version has been proposed, and IMHO is the best solution long term (the old crt unit could be renamed for specialist use).



## The problems

The current setup has several problems: 

  * The default Unix/Crt unit is not portable enough, and one big hack.
  * Implementations for individual platforms are completely independent, so similar problems repeatedly appear and are fixed for one platform, while keeping the same problem in another platform.
  * Terminal specific handling (as far as already existing) has two different codebases, unix/Crt unit and KVM API.
  * Parts of the terminal detection are in FV and the IDE.
  * For the KVM unit there is no good example except the volumneous IDE.
  * KVM keyboard handling is still very Linux centric.
  * Video is in much better shape, but still needs some tweaks. Also the big problem is being able to detect the current background color
  * Current development is very uncoordinated and organic.
  * Crt and KVM API's can't be used at the same time (e.g. to get mouse support)



## What needs to be done

  * Unix terminal problems need to be inventorised.
  * New Crt unit based on KVM as default. This new implementation should be used for all platforms having KVM available (common include file with all platform independent functions - including those based on functionality provided in KVM). Basis for this already exists in /rtl/inc/crt.inc, but it still relies on platforms supplying their implementations specifically for Crt unit instead of using KVM functions.
  * All console detection should move to the KVM units. New API must be created for this.
  * If there is additional platform specific code in Crt which cannot be supported by current KVM implementation (e.g. non-buffered Video access), KVM units should be extended accordingly.
  * Functions not related to KVM (e.g. Delay) could be possibly implemented either in System unit directly, or some other unit (e.g. Dos).
  * A more complex KVM demo needs to be developed as better usable test/demo than the IDE.
  * a userprogram must be able to override the console detection. The API must allow for this.
  * event mechanism reviewed to see if it is flexible enough for multiple event sources (kbd and mouse not multiplexed on stdin)



## console detection you say, what do you mean by that?

The behaviour of the terminal is governed by a lot of factors, here are some: 

  * the value of $TERM
  * What terminal kind do we have (roughly physical console , local X , remote. Maybe also framebuffer separate) ?
  * What terminal do we really have (e.g. local X terminal with "xterm" defined could be XTerm/ETerm/KTerm etc) ?
  * How is the terminal configured (e.g. other color background, other keybindings etc)
  * Can we reprogram the kbd manager for local use? (e.g. Linux and FreeBSD physical consoles)
  * What is our host OS (not all terminal emulations in the OS understand all ANSI codes e.g. ESC[n~ codes are not universal for all consoles) ? E.g. a freebsd machine SSHing to a linux machine might work slightly different from a freebsd to freebsd machine.
  * What do we use for Mouse support (Sysmouse, X, GPM) and how (ioctls, ansi sequences on stdin, separate fd etc, async via signals) ?



  
The different ways of console mouse is a bit of a problem, since if the mouse data isn't somehow multiplexed on stdin, one can't do one blocking call to get both kbd+mouse events. 

## References

This part is reserved for reference information: 

  * The following applications seem to have more than a minimal console handling: mc, mp3blaster, aumixer
  * Xterm documentation,faqs and technical document.


    
    
     Links:
     <http://www.bluesock.org/~willg/dev/ansi.html>
     <http://www.dee.ufcg.edu.br/~rrbrandt/tools/ansi.html>
     ~~<http://www.linuxgazette.com/issue65/padala.html>~~
     <http://vt100.net/docs/vt100-ug/chapter3.html#DECID>
     <http://invisible-island.net/xterm/xterm.faq.html#bug_ansi_xterm>
     <http://www.kitebird.com/csh-tcsh-book/ctlseqs.pdf>
     <http://vt100.net/docs/vt220-rm/chapter4.html#S4.4>
     ~~<http://www.fh-jena.de/~gmueller/Kurs_halle/esc_vt100.html>~~
     <http://vt100.net/docs/vt100-ug/contents.html>
     ~~<http://krtkg1.rug.ac.be/~colle/C/vt102.codes>~~
     ~~<http://www.icewalkers.com/Linux/ManPages/console_codes-4.html>~~
     <http://www.kostis.net/charsets/cp850.htm>
     <http://www.kostis.net/charsets/cp437.htm>
     ~~<http://www.calsky.com/lexikon/en/txt/c/co/codepage_437.php>~~
     ~~<http://www.unet.univie.ac.at/aix/cmds/aixcmds6/xterm.htm>~~

---

_Source: [https://wiki.freepascal.org/KVM_API_and_Crt_future](https://web.archive.org/web/20250208184639/https://wiki.freepascal.org/KVM_API_and_Crt_future)_
