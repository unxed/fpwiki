# iPhone/iPod development/ko

│ **English (en)** │

## Contents

  * 1 아래 내용은 번역 중입니다.
  * 2 Statement from Apple(애플社의 발표)
  * 3 Installation(설치)
    * 3.1 iPhone SDK 3.0



## 아래 내용은 번역 중입니다.

## Statement from Apple(애플社의 발표)

Apple has relaxed all restrictions on the development tools used to create iOS apps, as long as the resulting apps do not download any code. 

Apple은 iOS의 apps을 만들기위해 사용되는 개발툴에 대한 모든 제한을 완화하였다( 다만 결과물로서의 Apps가 어떠한 (악성)코드를 다운로드 하지 않는 다는 전제조건하에서..)[[1]](<http://www.apple.com/pr/library/2010/09/09statement.html>)

Before this change it was not legally possible to ship FPC-compiled applications for iPhone OS 4.0 and later. 

이러한 변화전에는 iPhone OS 4.0 또는 그 이후버젼을 위하여 FPC로 컴파일된 application을 배포하는 것은 법적으로 불가능하였었다. 

## Installation(설치)

Requirements(요구사항): 

  * Intel Mac (iPhone SDK 필요함)
  * Mac OS X 10.5.5 또는 그 이후 버젼 (iPhone SDK 필요함)
  * iPhone SDK 2.x 또는 그 이후 버젼이 설치되어 있어야함 (버젼 2.2 - 3.0에서는 테스트되었음).



**주의사항:** iPhone SDK 3.0 linker에는 버그가 존재한다. 더많은 정보를 위해서는 아래를 참조해라! 

특징: 

  * Simulator(시뮬레이터)지원


  * Xcode의 템플릿(template)은 하나의 응용프로그램 내에서 Pascal 과 Objective-C 소스파일들이 (동시에)통합되어 (사용되는)것을 명백히 지원(또는 사실상 요구)한다.


  * SDK에 포함된 OpenGL ES 1.1 C언어 헤더파일의 Pacal언어로 번역한 것은 gles11 유닛(unit)파일에서



이용가능하다.( 이것은 SGI open source license에 의해 배포가능하다) 

Please read the entire ReadMe.rtf on the installer disk image before asking for help. It contains quite a bit of information on things to watch out for (especially when using the Simulator), and how to change the included template when adding extra Pascal source files. 

Note that unlike other FPC releases, this installer will install the FPC sources and a script, and will then launch this script in order to compile FPC (after you tell it where you installed the iPhone SDK, so it can generate a Pascal version of some required C headers). This modus operandi is necessary to comply with the iPhone SDK agreement's stipulation that no derived works of the SDK may be distributed. If the script aborts for some reason, you can find it at `/Developer/FreePascalCompiler/2.4.0/InstallScript/finish_fpc_iphone_install.command` to relaunch it. 

There are two installation options, but both require [FPC 2.4.0](<ftp://ftp.freepascal.org/pub/fpc/dist/2.4.0/i386-macosx/fpc-2.4.0.intel-macosx.dmg>) for Mac OS X/i386 (65MB) to be installed first: 

  * The official [FPC 2.4.2 release for iPhone](<http://sourceforge.net/projects/freepascal/files/Mac%20OS%20X/2.4.2/fpc-2.4.2.arm-ios.dmg/download>) (21MB)
  * A snapshot of [FPC 2.5.1 (svn revision 14397) for iPhone](<ftp://ftp.freepascal.org/pub/fpc/snapshot/v25/arm-macosx/fpc-2.5.1v1.arm-iphone.dmg>) (23MB) (_WARNING_ : does not install anymore with current iOS SDKs; use the 2.4.2 installer above instead and then manually compile a new 2.5.1 version from source instead, as described below)



The main difference between the two is that the FPC 2.5.1 snapshot uses VFP instructions instead of software emulation for floating point calculations, and that the MacOSAll unit is available for iPhoneOS, to the extent that its functionality exists on that platform (with interfaces to, a.o., CoreFoundation and CoreAudio). This snapshot also contains support for [Objective-Pascal](<../../FPC_PasCocoa.md> "FPC PasCocoa"), but it does not yet automatically convert the iPhone-specific headers (and still contains various small errors in the Foundation translations). 

Note that to use the FPC 2.5.1 snapshot, you should use the included new 2.5.1 Xcode template. This template has been modified to make it trivial to switch to a different FPC version using the same project file, so that starting a project file from scratch should no longer be necessary in the future. See the included readme file for information on how to switch the used FPC version. 

If you encounter problems with the script that is launched when the installer finishes the installation, please always provide the entire contents of the Terminal window in which the script was executed when asking for help. 

**Update** : A small error has been found in the new Xcode template included in the above 2.5.1 template: it always passes -Cfvfpv2 to the compiler, which will cause it to give an error when not compiling for ARM (it selects the VFPv2 floating point unit, which does not exist for Intel). For now, you will have to remove this parameter from the FPC_COMMON_OPTIONS when compiling for the simulator, and re-add it when compiling for the device. See the readme for information on where to find this setting. 

(Xcode 4 compatibility: It appears the template works for Xcode 3 only, but not Xcode 4.) 

### iPhone SDK 3.0

The linker included with the iPhone SDK 3.0 contains a bug that is triggered by the Xcode template included with FPC. You can work around this bug by adding -Wl,-no_order_inits to the "Other Linker" flags in the Build settings of the project. This is already done for the Xcode templates included in the 2.4.0rc1 download above, but you may have to do this manually if you are using a project based on a template from a previous snapshot. 

The linker bug has been reported to Apple ([rdar://problem/7120987](<http://openradar.appspot.com/7120987>)), so it will hopefully be fixed in a future release of the iPhone SDK.

---

_Source: [https://wiki.freepascal.org/iPhone/iPod_development/kr](https://web.archive.org/web/20230322073357/https://wiki.freepascal.org/iPhone/iPod_development/kr)_
