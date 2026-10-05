# File size and smartlinking

│ **[Deutsch (de)](</File_size_and_smartlinking/de> "File size and smartlinking/de")** │  **English (en)** │  **[français (fr)](</File_size_and_smartlinking/fr> "File size and smartlinking/fr")** │  **[日本語 (ja)](</File_size_and_smartlinking/ja> "File size and smartlinking/ja")** │  **[português (pt)](</File_size_and_smartlinking/pt> "File size and smartlinking/pt")** │  **[中文（中国大陆） (zh_CN)](</File_size_and_smartlinking/zh_CN> "File size and smartlinking/zh CN")** │    
****

This article is a work in progress about executable size and smartlinking on Lazarus. Feel free to contribute. 

## Contents

  * 1 Introduction
  * 2 Case study 1 in Windows
  * 3 Remove unused functions in a class
  * 4 Bytes Used
    * 4.1 by Lib
  * 5 See Also



## Introduction

In Free Pascal, "smartlinking" is removing unused code and variables from the final executable. This is done during the linking stage, when the executable is written using the object files created by the compiler earlier. 

## Case study 1 in Windows

This study was conducted on the 8th of February 2006 because a Lazarus fully compiled with Smartlinking was Released (version 0.9.12). It intends to establish the relationship between the varying results below with different Lazarus and Free Pascal versions as well as with Smartlinking and without. 

  
The Variables being studied are: 

  * Executable size after strip
  * Executable size after strip and UPX
  * Linking time



  
Compile time isn´t considered here because it´s too similar on all configurations and much less significant than the link time. 

  
Executable size without strip isn´t included. Notice that strip was used always from command line with the command: 
    
    
    strip --strip-all magnifier.exe
    

  
The program being compiled is the Virtual Magnifying Glass. The source and binaries for this program are freely available for download on: <http://magnifier.sourceforge.net>

  
About the linking time please note that the utilized computer is 3.2GHz Pentium 4 with Intel motherboard and dual core processor and 512MB of RAM. 

  
**Results**

  
The utilized OS is Windows XP and the 0.9.13 versions are from the same date when 0.9.12 was release. The comparison took place using the following software configurations: 

  * Lazarus 0.9.12 available [here](<http://sourceforge.net/project/showfiles.php?group_id=89339>). Free Pascal 2.0.2 that comes with the installer. LCL and RTL are smartlinked. Refered from now on as simply 0.9.12.


  * Lazarus 0.9.13 downloaded from Subversion from the same date. Free Pascal 2.0.2 installed separately. The LCL is not smartlinked. Refered from now on as simply 0.9.13 + 2.0.2.


  * Lazarus 0.9.13 snapshot. Free Pascal 2.1 that comes with the installer. The LCL is not smartlinked. Refered from now on as simply 0.9.13 + 2.1.


  * Lazarus 0.9.13 snapshot. Free Pascal 2.1 that comes with the installer. The LCL is smartlinked. Refered from now on as simply 0.9.13 + 2.1 + SL.

| 0.9.12 | 0.9.13 + 2.0.2 | 0.9.13 + 2.1 + SL | 0.9.13 + 2.1   
---|---|---|---|---  
File Size after strip (in bytes)  | 1,108,480  | 1,587,712  | 1,425,408  | 1,649,152   
File Size after UPX (in bytes)  | 318,976  | 438,272  | 388,608  | 454,144   
Linking time  | 15 seconds  | 5 seconds  | 45 seconds  | 10 seconds   
  
[![OuptutFileSizesComparisonChart.png](https://wiki.freepascal.org/images/a/a1/OuptutFileSizesComparisonChart.png)](</File:OuptutFileSizesComparisonChart.png>)

**Conclusion**

  
The 0.9.13 snapshot from the 8th of February 2006 features a unstable compiler from the 2.1 branch, which can cause the bigger executables and slower linking time as compared to the other versions. 

  
The 0.9.12 version has the best file size of all, both with UPX and without, showing that Smartlinking really can diminish the file size in Windows. This, however, does not come without a cost, and the cost is linking time, which is about 3 times higher then without smartlinking. 

  
The 0.9.12 version already comes fully configured for Smartlinking on Windows and no extra configuration is needed. This was not the case on previous releases. 

## Remove unused functions in a class

Q: If a virtual method of a class is not used at all in the programm will the compiler remove it from the executable ? 

A: If you use FPC 2.3.1 with whole-program optimization and the compiler can prove that it is never callable: yes. See [Whole Program Optimization](<Whole_Program_Optimization.md> "Whole Program Optimization") for more information. In case this is in relation to the thread on the Lazarus list about the big executables: note that it has only a limited effect on Lazarus programs, because almost all linked LCL code can potentially be executed (due to the way the LCL is constructed). In fact, I think most savings there come from making a number of virtual method calls non-virtual, rather than from throwing away unreachable code. 

The internal linker can also do it (only throwing away virtual method calls, not turning virtual method calls into static ones), but only on Windows platforms. It is however not currently enabled in the compiler, because the changes break the external linker. It should therefore be turned into a command line option (along with a check that produces an error if you try to link a unit compiled with the option using the external linker), but that hasn't been done yet. 

Q: How can the compiler determine that a virtual method is unused at all, 

A: If you have a class hierarchy TBase->TDerived1->TDerived2 with a virtual method called "vmethod", and nowhere in the program there is a call to vmethod, then it is unused. Or if it is only called using TDerived2 instances, then if the linker does not find any direct references to TDerived1.vmethod or TBase.vmethod (e.g., via "inherited" calls from TDerived2 methods), it knows that the VMT entries for "vmethod" in TDerived1 and TBase can be set to nil. 

## Bytes Used

Here's a layout by pascal units in bytes used for code. The size is based on Dwarf2 information generated. Doesn't include resources size used by a unit. 

The project is a single form application (no additional controls used). Lazarus used is a Trunk version of 23 Jan 2020. 

No optimization was used during compilation. 

Size  | Lib  | Package  | Category  | File name   
---|---|---|---|---  
238704 | lcl | base |  | controls.pp   
144064 | lcl | base |  | graphics.pp   
111280 | lcl | ws |  | win32/win32int.pp   
106880 | lcl | base |  | forms.pp   
98000 | rtl | rtl |  | system.pp   
94432 | lcl | base |  | intfgraphics.pas   
90352 | lcl | base |  | comctrls.pp   
90224 | rtl | rtl |  | classes.pp   
59552 | lcl | base |  | stdctrls.pp   
51888 | lcl | base |  | interfacebase.pp   
50336 | rtl | rtl |  | ../win/sysutils.pp   
37728 | lcl | base |  | imglist.pp   
35456 | rtl | rtl |  | rtl-objpas/src/inc/variants.pp   
33232 | lcl | lazutils |  | lazutf8.pas   
31456 | lcl | base |  | extctrls.pp   
30912 | fcl | image |  | fcl-image/src/fpreadtiff.pas   
30816 | lcl | ws |  | win32/win32wscomctrls.pp   
30800 | lcl | ws |  | win32/win32wsmenus.pp   
28832 | lcl | base |  | menus.pp   
25984 | lcl | base |  | maskedit.pp   
24480 | rtl | rtl |  | rtl-objpas/src/win/varutils.pp   
24048 | lcl | base |  | dialogs.pp   
23632 | fcl | json |  | fcl-json/src/fpjson.pp   
23456 | lcl | ws |  | win32/win32wsstdctrls.pp   
23408 | lcl | base |  | lclintf.pas   
18448 | lcl | base |  | lresources.pp   
17904 | lcl | lazutils |  | textstrings.pas   
16736 | fcl | image |  | fcl-image/src/fpcanvas.pp   
16544 | lcl | lazutils |  | graphtype.pp   
13408 | lcl | base |  | themes.pas   
13168 | lcl | ws |  | win32/win32proc.pp   
13136 | fcl | image |  | fcl-image/src/pixtools.pp   
13088 | lcl | lazutils |  | lazloggerbase.pas   
12368 | lcl | lazutils |  | laz_avl_tree.pp   
11872 | fcl | image |  | fcl-image/src/fpimage.pp   
11776 | lcl | base |  | actnlist.pas   
11488 | lcl | base |  | buttons.pp   
10576 | rtl | debug |  | ../inc/heaptrc.pp   
10544 | lcl | ws |  | win32/win32wsbuttons.pp   
10464 | rtl | rtl |  | ../objpas/typinfo.pp   
9744 | fcl | image |  | fcl-image/src/fpreadpng.pp   
9696 | lcl | base |  | graphmath.pp   
9536 | lcl | lazutils |  | lazlogger.pas   
9424 | lcl | lazutils |  | lazfileutils.pas   
9296 | lcl | base |  | buttonpanel.pas   
9248 | fcl | image |  | fcl-image/src/fpwritetiff.pas   
8864 | lcl | base |  | clipbrd.pp   
8768 | lcl | ws |  | win32/win32extra.pas   
8768 | fcl | jpeg |  | pasjpeg/src/jquant2.pas   
8720 | fcl | jpeg |  | pasjpeg/src/jdmarker.pas   
8160 | fcl | image |  | fcl-image/src/fpwritepng.pp   
8016 | fcl | image |  | fcl-image/src/fpwritebmp.pp   
7792 | fcl | zlib |  | paszlib/src/trees.pas   
7648 | fcl | zlib |  | paszlib/src/zdeflate.pas   
7536 | fcl | image |  | fcl-image/src/ellipses.pp   
7472 | lcl | ws |  | win32/win32wsforms.pp   
7456 | fcl | image |  | fcl-image/src/fpreadbmp.pp   
7328 | lcl | ws |  | win32/win32themes.pas   
6880 | fcl | jpeg |  | pasjpeg/src/jdcoefct.pas   
6864 | lcl | base |  | lclproc.pas   
6624 | lcl | lazutils |  | lazutf16.pas   
6400 | lcl | ws |  | win32/win32wscontrols.pp   
6240 | fcl | zlib |  | paszlib/src/infblock.pas   
5920 | lcl | base |  | lclrescache.pas   
5792 | fcl | image |  | fcl-image/src/fpreadjpeg.pas   
5728 | fcl | image |  | fcl-image/src/fppixlcanv.pp   
5680 | fcl | ws |  | win32/win32wsspin.pp   
5648 | lcl | lazutils |  | maps.pp   
5632 | lcl | ws |  | win32/win32wsimglist.pp   
5568 | fcl | jpeg |  | pasjpeg/src/jchuff.pas   
5520 | fcl | jpeg |  | pasjpeg/src/jmemmgr.pas   
5392 | fcl | jpeg |  | pasjpeg/src/jquant1.pas   
5264 | fcl | image |  | fcl-image/src/fpreadgif.pas   
5232 | fcl | jpeg |  | pasjpeg/src/jdphuff.pas   
5152 | lcl | base |  | imagelistcache.pas   
5104 | lcl | base |  | widgetset/wsstdctrls.pp   
5008 | fcl | json |  | fcl-json/src/jsonscanner.pp   
4992 | lcl | base |  | widgetset/wslclclasses.pp   
4880 | fcl | jpeg |  | pasjpeg/src/jcphuff.pas   
4848 | fcl | utils |  | fcl-base/src/contnrs.pp   
4816 | fcl | image |  | fcl-image/src/fptiffcmn.pas   
4768 | rtl | debug |  | ../inc/lnfodwrf.pp   
4640 | fcl | jpeg |  | pasjpeg/src/jcmaster.pas   
4592 | fcl | jpeg |  | pasjpeg/src/jdhuff.pas   
4512 | fcl | jpeg |  | pasjpeg/src/jcparam.pas   
4256 | fcl | jpeg |  | pasjpeg/src/jcsample.pas   
4208 | lcl | base |  | widgetset/wscomctrls.pp   
4160 | fcl | zlib |  | paszlib/src/infcodes.pas   
3936 | fcl | zlib |  | paszlib/src/inftrees.pas   
3616 | fcl | jpeg |  | pasjpeg/src/jcmarker.pas   
3584 | fcl | zlib |  | paszlib/src/zstream.pp   
3584 | fcl | jpeg |  | pasjpeg/src/jidctint.pas   
3488 | fcl | image |  | fcl-image/src/fpwritepnm.pp   
3472 | fcl | image |  | fcl-image/src/fpreadpnm.pp   
3360 | fcl | jpeg |  | pasjpeg/src/jdmerge.pas   
3312 | lcl | base |  | icnstypes.pas   
3248 | fcl | jpeg |  | pasjpeg/src/jccoefct.pas   
3216 | fcl | jpeg |  | pasjpeg/src/jdsample.pas   
3168 | fcl | jpeg |  | pasjpeg/src/jdmainct.pas   
3152 | lcl | base |  | widgetset/wsimglist.pp   
3120 | lcl | base |  | spin.pp   
3120 | fcl | jpeg |  | pasjpeg/src/jdmaster.pas   
3104 | rtl | winapi |  | winunits-base/src/uxtheme.pp   
2992 | lcl | base |  | widgetset/wscontrols.pp   
2944 | fcl | json |  | fcl-json/src/jsonparser.pp   
2896 | fcl | jpeg |  | pasjpeg/src/jccolor.pas   
2848 | fcl | zlib |  | paszlib/src/zinflate.pas   
2832 | fcl | jpeg |  | pasjpeg/src/jdcolor.pas   
2816 | fcl | jpeg |  | pasjpeg/src/jcdctmgr.pas   
2752 | lcl | lazutils |  | lazmethodlist.pas   
2736 | fcl | jpeg |  | pasjpeg/src/jidctred.pas   
2704 | rtl | debug |  | ../inc/exeinfo.pp   
2688 | fcl | jpeg |  | pasjpeg/src/jidctfst.pas   
2576 | fcl | jpeg |  | pasjpeg/src/jidctflt.pas   
2496 | rtl | rtl |  | ../objpas/fgl.pp   
2464 | fcl | jpeg |  | pasjpeg/src/jdinput.pas   
2448 | lcl | ws |  | win32/win32wsfactory.pas   
2352 | fcl | image |  | fcl-image/src/fpwritejpeg.pas   
2320 | fcl | jpeg |  | pasjpeg/src/jcprepct.pas   
2208 | lcl | lazutils |  | lclclasses.pp   
2032 | fcl | jpeg |  | pasjpeg/src/jerror.pas   
2016 | rtl | winapi |  | winunits-base/src/multimon.pp   
2016 | fcl | jpeg |  | pasjpeg/src/jfdctint.pas   
1952 | rtl | winapi |  | windows.pp   
1888 | fcl | jpeg |  | pasjpeg/src/jdapimin.pas   
1872 | fcl | image |  | fcl-image/src/clipping.pp   
1824 | fcl | zlib |  | paszlib/src/inffast.pas   
1808 | lcl | base |  | extendedstrings.pas   
1712 | lcl | base |  | customtimer.pas   
1648 | lcl | base |  | helpintfs.pas   
1584 | fcl | jpeg |  | pasjpeg/src/jfdctfst.pas   
1584 | fcl | jpeg |  | pasjpeg/src/jfdctflt.pas   
1584 | rtl | rtl |  | ../objpas/math.pp   
1472 | fcl | jpeg |  | pasjpeg/src/jdpostct.pas   
1360 | fcl | jpeg |  | pasjpeg/src/jdapistd.pas   
1280 | fcl | utils |  | fcl-base/src/custapp.pp   
1200 | lcl | lazutils |  | lazfilecache.pas   
1152 | lcl | base |  | widgetset/wsmenus.pp   
1152 | lcl | lazutils |  | lazclasses.pas   
1120 | fcl | jpeg |  | pasjpeg/src/jddctmgr.pas   
1056 | lcl | base |  | widgetset/wsforms.pp   
1040 | lcl | lazutils |  | integerlist.pas   
1024 | fcl | jpeg |  | pasjpeg/src/jcapimin.pas   
992 | lcl | base |  | lcltype.pp   
848 | lcl | lazutils |  | lazstringutils.pas   
832 | fcl | image |  | fcl-image/src/fpimgcanv.pp   
768 | lcl | base |  | widgetset/wsproc.pp   
688 | rtl | rtl |  | ../objpas/types.pp   
672 | lcl | ws |  | win32/win32wsdialogs.pp   
672 | lcl | lazutils |  | lazutf8classes.pas   
656 | fcl | jpeg |  | pasjpeg/src/jcmainct.pas   
656 | rtl | rtl |  | ../inc/fpintres.pp   
640 | lcl | lazutils |  | laztracer.pas   
624 | fcl | jpeg |  | pasjpeg/src/jdatasrc.pas   
560 | fcl | image |  | fcl-image/src/fpimgcmn.pp   
528 | fcl | zlib |  | paszlib/src/infutil.pas   
528 | fcl | utils |  | fcl-base/src/syncobjs.pp   
512 | lcl | base |  | lclmessageglue.pas   
496 | fcl | jpeg |  | pasjpeg/src/jutils.pas   
496 | fcl | jpeg |  | pasjpeg/src/jdatadst.pas   
480 | fcl | jpeg |  | pasjpeg/src/jcapistd.pas   
464 | rtl | winapi |  | winunits-base/src/commctrl.pp   
432 | lcl | base |  | widgetset/wsextctrls.pp   
432 | fcl | zlib |  | paszlib/src/zbase.pas   
384 | fcl | jpeg |  | pasjpeg/src/jmemnobs.pas   
368 | fcl | jpeg |  | pasjpeg/src/jcomapi.pas   
336 | rtl | rtl |  | ../objpas/objpas.pp   
288 | lcl | base |  | widgetset/wsdialogs.pp   
288 | fcl | xml |  | fcl-xml/src/htmldefs.pp   
256 | fcl | jpeg |  | pasjpeg/src/jinclude.pas   
256 | fcl | jpeg |  | pasjpeg/src/jcinit.pas   
256 | lcl | lazutils |  | lcltaskdialog.pas   
256 | lcl | lazutils |  | laz2_xmlread.pas   
224 | lcl | base |  | widgetset/wsspin.pp   
208 | lcl | base |  | widgetset/wsbuttons.pp   
208 | fcl | zlib |  | paszlib/src/adler.pas   
192 | rtl | rtl |  | ../objpas/unicodedata.pas   
144 | lcl | lazutils |  | avglvltree.pas   
128 | lcl | base |  | widgetset/wsreferences.pp   
112 | lcl | lazutils |  | stringhashlist.pas   
112 | main | app |  | project1.lpr   
96 | rtl | winapi |  | ../win/windirs.pp   
96 | rtl | winapi |  | ../win/dos.pp   
80 | lcl | base |  | lclversion.pas   
80 | lcl | base |  | lclplatformdef.pas   
80 | lcl | base |  | forms/calcform.pas   
80 | lcl | lazutils |  | fileutil.pas   
64 | lcl | ws |  | win32/interfaces.pp   
64 | lcl | lazutils |  | lazutilities.pas   
64 | lcl | lazutils |  | laz2_dom.pas   
64 | rtl | rtl |  | ../inc/strings.pp   
48 | lcl | lazutils |  | lconvencoding.pas   
48 | lcl | lazutils |  | lazsysutils.pas   
48 | lcl | lazutils |  | laz2_xmlutils.pas   
48 | lcl | lazutils |  | fpcadds.pas   
48 | fcl | utils |  | fcl-base/src/rttiutils.pp   
32 | fcl | utils |  | fcl-base/src/gettext.pp   
  
  * Note, that zlib is used for PNG images reading and writting



### by Lib

lib  | code  | percentage   
---|---|---  
rtl | 340752 | 15.46%   
lcl | 1491184 | 67.75%   
fcl | 372176 | 16.88%   
main | 122 | 0.01%   
Total | 2204234 | 100%   
  
## See Also

  * [Size Matters](<Size_Matters.md> "Size Matters")

---

_Source: [https://wiki.freepascal.org/File_size_and_smartlinking](https://web.archive.org/web/20250324064843/https://wiki.freepascal.org/File_size_and_smartlinking)_
