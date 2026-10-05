# ZSeries/Part 4

_[Go back to zSeries](<../ZSeries.md> "ZSeries")_ — [Go Back to Part 3](<Part_3.md> "ZSeries/Part 3") — [Go Forward to Part 5](</index.php?title=ZSeries/Part_5&action=edit&redlink=1> "ZSeries/Part 5 \(page does not exist\)")

These observations were contributed by Paul "kerravon86" via the Hercules-390 mailing list, in response to the January/February 2012 thread in fpc-devel. Paul is responsible for the unofficial /380 Hercules patches and for the port of GCC to IBM OSes including MVS. 

They are posted here with minimal editing or comment lest they be lost. 

* * *

  
Anyway, here are some interesting topics: 

<http://lists.freepascal.org/pipermail/fpc-devel/2012-January/028050.html>

"if for no other reason than to keep C from being the only choice for developing programs on mainframes" 

Language wars - cool! 

  
<http://lists.freepascal.org/pipermail/fpc-devel/2012-January/028052.html>

Interesting that mainframes are relatively immune from hacking. 

  
<http://lists.freepascal.org/pipermail/fpc-devel/2012-January/028054.html> <http://lists.freepascal.org/pipermail/fpc-devel/2012-January/028192.html>

I read the link about the S370 being "broken", and I don't agree. S370 works fine. The "issue" about functions that are more than 4K in size being slower is not an issue in practice, because even if that were true, most functions do not exceed 4K anyway. At least from my experience with the GCC code. 

And even if GCC 4 doesn't support the older S370, GCCMVS 3.2.3 does, so I don't see an issue. 

  
<http://lists.freepascal.org/pipermail/fpc-devel/2012-January/028055.html>

I don't agree that using S370 instructions is like using 8086 instructions when you have an 80386 available. The fact is that S370 provides everything required to produce 31-bit executables that run ATL with more than 16 MiB of memory on z/OS, while still running as 24-bit on MVS 3.8j. 

You can't compare that to a 16-bit 8086 and a 32-bit 80386. 

  
<http://lists.freepascal.org/pipermail/fpc-devel/2012-January/028063.html>

With regard to CSECTS - I strongly suggest that you look at GCCMVS-generated code and stick to that. Which includes blank CSECTS. If you try automatically naming CSECTS, then when you have a name clash, you will not get a linker error. The bigger CSECT will just replace the smaller CSECT silently (from memory). 

  
I would also recommend taking a look at the MVSSUPA code in PDPCLIB. I believe that that code should support Pascal completely unchanged. 

Another comment I have is that if you wish to use S390 instructions to make life easier, it would be good if you could stick to instructions that fall into the list below, since those instructions are available in S380, even though they're not in S370, so your Pascal programs will still run under MVS 3.8j. 

BFN. Paul. 

  


  


/* The following ESA/390 features can be retrofitted to S/370 and 
    
    
      may be activated if desired by uncommenting the appropriate
      define statements below and performing a complete rebuild */
    

  1. define FEATURE_BASIC_FP_EXTENSIONS
  2. define FEATURE_BINARY_FLOATING_POINT
  3. define FEATURE_CHECKSUM_INSTRUCTION
  4. define FEATURE_COMPARE_AND_MOVE_EXTENDED
  5. define FEATURE_COMPRESSION
  6. define FEATURE_EXTENDED_TRANSLATION
  7. define FEATURE_EXTENDED_TRANSLATION_FACILITY_2
  8. define FEATURE_HFP_EXTENSIONS
  9. define FEATURE_HFP_MULTIPLY_ADD_SUBTRACT
  10. define FEATURE_HFP_UNNORMALIZED_EXTENSION
  11. define FEATURE_IMMEDIATE_AND_RELATIVE
  12. define FEATURE_SQUARE_ROOT
  13. define FEATURE_STRING_INSTRUCTION



/* The following ESAME features can be retrofitted to S/370 and 
    
    
      may be activated if desired by uncommenting the appropriate
      define statements below and performing a complete rebuild */
    

  1. define FEATURE_ESAME_N3_ESA390
  2. define FEATURE_ETF2_ENHANCEMENT
  3. define FEATURE_ETF3_ENHANCEMENT
  4. define FEATURE_EXECUTE_EXTENSIONS_FACILITY
  5. define FEATURE_EXTENDED_IMMEDIATE
  6. define FEATURE_EXTENDED_TRANSLATION_FACILITY_3
  7. define FEATURE_GENERAL_INSTRUCTIONS_EXTENSION_FACILITY
  8. define FEATURE_LONG_DISPLACEMENT
  9. define FEATURE_MESSAGE_SECURITY_ASSIST
  10. define FEATURE_MESSAGE_SECURITY_ASSIST_EXTENSION_1
  11. define FEATURE_MESSAGE_SECURITY_ASSIST_EXTENSION_2
  12. define FEATURE_PARSING_ENHANCEMENT_FACILIT



    _[Go back to zSeries](<../ZSeries.md> "ZSeries")_ — [Go Back to Part 3](<Part_3.md> "ZSeries/Part 3") — [Go Forward to Part 5](</index.php?title=ZSeries/Part_5&action=edit&redlink=1> "ZSeries/Part 5 \(page does not exist\)")

---

_Source: [https://wiki.freepascal.org/ZSeries/Part_4](https://web.archive.org/web/20250207043204/https://wiki.freepascal.org/ZSeries/Part_4)_
