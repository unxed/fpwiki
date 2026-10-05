# ATMEL ATtiny Embedded Systems

## Nanite-FPC

Nano-sized ATtiny85 development board with USB bootloader. 

NaniteLib ported to Free Pascal from <https://github.com/cpldcpu/Nanite>. 

As described in [this blog post](<https://cpldcpu.wordpress.com/2014/04/25/the-nanite-85/>) the bootloader you should use is the t85_default_entry_jumper_PB5.hex in the micronucleus directory. 

The repository holds the micronucleus firmware, the micronucleus commandline, the windows USB driver and a software example in Free Pascal to handle the soft reset-button. 

I made one using the through hole version from Danjovic (<https://github.com/Danjovic/Nanite>) on veroboard for fun ;) 

A recent Free Pascal v3.3.1 (trunk) was used (10/01/2020). Works like a charm! 

[Original Forum Post](<https://forum.lazarus.freepascal.org/index.php/topic,48112.msg345947/topicseen.html#new>)

---

_Source: [https://wiki.freepascal.org/ATMEL_ATtiny_Embedded_Systems](https://web.archive.org/web/20250124211552/https://wiki.freepascal.org/ATMEL_ATtiny_Embedded_Systems)_
