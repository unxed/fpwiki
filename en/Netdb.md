# Netdb

│ **English (en)** │  **[Bahasa Indonesia (id)](</Netdb/id> "Netdb/id")** │    
****

## Overview

Netdb is an unit DNS resolving under *nix, grouped in the larger [fcl-net](<fcl-net.md> "fcl-net") package. It forms the basis of FCL level DNS resolving which is truly platform independent. 

After being broken for a while, after 1.9.8 some serious fixes were made, and the IPV4 calls should be in at least usable form for FreeBSD/Linux/Darwin on x86 and PowerPC. 

The most common misconception about netdb is that gethostbyname is a direct alternative for the libc call with the same name (which resolves a host). However, the netdb call gethostbyname really only checks /etc/hosts, and netdb resolvename only checks the DNS servers listed in /etc/resolv.conf. 

This has the following consequences: 

  * it takes two calls to resolve a name (one to check hosts, one to check DNS) (in other words testdns and testhost code is both needed)
  * the LOOKUP (OpenBSD) command in resolv.conf is not processed.
  * Other forms of resolving than /etc/hosts and (like YP/Yellow Pages) are not supported.
  * Setting FPC_USE_LIBC doesn't have any consequences yet, the non-libc code will still be used.



Note that [fcl-net](<fcl-net.md> "fcl-net") also contains a straight libc using resolve header, cnetdb since FPC 2.2.4.

---

_Source: [https://wiki.freepascal.org/Netdb](https://web.archive.org/web/20250118074334/https://wiki.freepascal.org/Netdb)_
