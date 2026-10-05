# libc unit

## Contents

  * 1 libc deprecated
    * 1.1 The problem with unit libc
    * 1.2 So what to replace it with?
    * 1.3 But I only want....
  * 2 Will unit libc be removed for Linux/i386?
  * 3 Resources



## libc deprecated

The libc unit (not to be confused with the [libc library](<libc_library.md> "libc library") (.so)) is a header to the libc library created for Kylix compatibility. 

The libc _unit_ is deprecated, and frozen in time for Kylix compatibility, and won't be ported to other architectures or operating systems. 

### The problem with unit libc

There is a lot of confusion about why unit libc has been deprecated, because FPC has different policies on **library libc** and **unit libc**. An often heard argument is that (library) libc use is strictly forbidden, which is totally unrelated to the problem of the libc unit. 

The **libc library** policy is way simpler and unrelated: the core bootstrap and the base RTL( system, sysutils, baseunix) are kept free from libc for bootstrapping and long term compatibility reasons. Functionality that needs the **libc library** is plugged in by extra units that only need to be included once (using units like cthreads, clocale, cmem, cwstrings). 

This policy is from a pragmatic viewpoint, and not set in stone forever. An example: the macOS port does not follow it, because on that platform system calls can change even between minor revisions, while backwards compatibility of libc is guaranteed as much as possible by Apple. In essence, libc is _the_ system interface on macOS as far as user level applications are concerned. 

The _**actual problem**_ with the **UNIT libc** is that Borland followed the same model with Kylix as they did on Windows. While writing Kylix, they simply stuffed anything platform dependent they could find into unit libc without any discrimination. This caused the unit to be stuffed from top to bottom with types, consts and calls from various origins, and with various portability concerns: 

  * linuxisms ( symbols/behaviour that only exist on Linux)
  * glibcisms ( symbols/behaviour that only exists with glibc, but not e.g. with uclibc)
  * libraries that are in fact external, but linked into the glibc library on stock linux (but not part of the actual glibc distribution). This means stuff like iconv.
  * kernel version dependent symbols
  * x86 specific issues
  * other system libraries like libdl.
  * all records and structures are visible without any discrimination, even if they are kernel version dependent. No attempt was made to clean up, limit visibility or even just mark internal fields.
  * Linux specific temporary (migratory) constructs like -32 and -64 versions of calls. (There is an architecture dependent aspect too, because such constructs are often x86 specific because that architecture has the largest legacy)



Note that this clearly makes this more than a "Linux vs the rest" situation. A lot of Linux developers will sooner or later have to face have to face the following, too: 

  * Other architectures (64-bit)
  * embedded uclibc
  * long term compatibility: what if libc no longer exports fstat64 because the grace period is over and off_t is always 64-bit? Migration of time_t to 64-bit...



For Borland this never was a problem, only the last version is supported and reasonably up to date, and they expected you to upgrade once in a while. At least if Kylix hadn't been killed. 

### So what to replace it with?

That's not easily answered, since that is the exact problem with unit libc. It is a mix of stuff with a lot of different tradeoffs. Below are some options in decreasing preference. 

  1. Try to use something wholly portable (e.g. from sysutils)
  2. For basic Unix calls use existing units like baseunix, unix, termio (terminal), sockets (base networking), x86 (32-bit x86 cpu specific stuff like portio)
  3. For less basic, but common functionality, use the units that already abstract these as much as possible: 
     * dynlibs (dynamic loading of libraries). Also works on Windows.
     * users (2.2.2+ Unix users, groups and passwords) or
     * iconvenc (2.2.3+, iconv unicode support)
     * cnetdb (calls libc library) or netdb (independent DNS resolver). See also [fcl-net](<fcl-net.md> "fcl-net") package)
  4. If the functionality you desire is not yet abstracted, create and submit a package where the functionality you desire is abstracted in a relatively platform independent way. (e.g. a unit that provides an abstract notification mechanism, using [inotify](<inotify.md> "inotify") on Linux and Kpoll on FreeBSD and macOS). At first, limit yourself to the base functionality. This will require some compromises, but rule of thumb is that 90% of the people only use the base functionality anyway.
  5. If it is Linux only, maintain your own headers, and put them on the contributed units page.
  6. If it is a single Linux system call that can't be abstracted, add it to unit linux.



If you don't like any of these options, dig up as much information as you can about portability, and about what you want to achieve, and post it to the "fpc-devel" list. 

A quick question on the FPC IRC channel might get you updated as to the current status. 

### But I only want....

If we first commit raw Linux/x86 headers (and thus often Linux/x86-glibc specific ones), and only then start worrying about portability, we may as well fork the RTL for each *nix target immediately, and forget about even attempting cross-unix portability. Moreover this often introduces a bad published interface that might become a compatibility hazard to maintain when later crossplatform versions become available. 

While it is easy to just throw a barely tested basic header into SVN, without any research into portability, this will only cause frustration for users that have to change their code, because later on the interfaces change again. 

So all people wanting to contribute must invest some time in learning to expose functionality in a responsible and portable way. Note that for it to be of any use for other projects like Lazarus, the database engines etc, it must be portable anyway, so better start with it. Everybody can make a quick header translation. However we need good, consistent and portable ones. 

Also keep in mind that there are various portability concerns. Not just say "macOS" vs Linux, but also more subtle problems like revealing kernel details, endianness, 32 vs 64-bit issues, using glibcisms (embedded Linuxes, also x86, _can_ use alternate libc types) etc. 

It is unfair to drop all portability concerns onto the unix users that don't use only mainstream linux/x86 (FreeBSD, macOS, non x86 Linux and Linux uclibc programmers). Specially because quite a lot of Linux/x86 users tend to develop for one of them too at some point. 

## Will unit libc be removed for Linux/i386?

There are no such plans. As long as we have the feeling that people are actively using it, and it is reasonably up to date it probably will be included for sake of old Kylixers. Maybe we will add some harmless deprecated warnings in 2.4 to avoid accidental use though. 

Note that this policy is based on the libc unit effectively being an header, needing minimal patches from time to time. As soon as a lot of code needs to be inserted to keep unit libc working (effectively starting an emulation of an older Linux system), and/or the changes required to keep it working start grossly violating Kylix compatibility (and hence the purpose of this unit), this policy has to be reconsidered. 

From 2008 to now (mid 2012), there were only two bugfixes in unit libc and one cleanup. 

## Resources

Nobody expects that you prepare a perfect new unit at once, and without help. Here are some sources that can give assistance: 

  * the fpc-devel maillist
  * the fpc-pascal maillist
  * FPC irc. (irc.freenode.net #fpc)
  * The [Open Group site](<http://www.opengroup.org/onlinepubs/009695399/toc.htm>) has a lot of information about what unix functionality is standarized. If you have troubles with interpretation, ask on IRC or the maillist.
  * Several *nix vendors have their manpages online, which can be used as a quick check to see if a certain call is supported. Don't take this as absolute truths though, there are exceptions.[FreeBSD man page](<http://www.freebsd.org/cgi/man.cgi>)
  * And in all other cases, Google is a good resource. Searching for "freebsd name_of_call or " freebsd name_of_call porting" can yield good results.

---

_Source: [https://wiki.freepascal.org/libc_unit](https://web.archive.org/web/20250427011845/https://wiki.freepascal.org/libc_unit)_
