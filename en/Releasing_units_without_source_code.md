# Releasing units without source code

│ **English (en)** │  **[русский (ru)](<../ru/Releasing_units_without_source_code.md>)** │

It can be useful to release a FreePascal unit without publishing its source code: 

  * The source code contains proprietary information.
  * In teaching, because you want to force students to use a unit by its interface (contract) only, and not by looking at its implementation.



FreePascal allows you to do so in the following way. 

The **provider** of the unit (and owner of its source code) should: 

  * Compile the unit separately; it is recommended to use the compiler option **-Ur** (_Generate release unit files_ ; see _User's Manual_ for details)
  * Publish both the resulting ***.ppu** and ***.o** files. Also see Section 3.3 of the _User's Manual_ (_Compiling a unit_).



The **user** of the provided unit should: 

  * Compile the using program (the _client_), such that the compiler can find both the ***.ppu** and ***.o** files of the unit (e.g. through the compiler option **-Fu**).



Thus, there are two compiler contexts that matter: 

  * The compiler installation of the provider
  * The compiler installation of the user (client)



## Notes

  * The provider and user should use the **same compiler version**. Although backwards compatibility between compiled units is never broken on purpose, this regularly happens in order to support new features or to fix bugs.
  * The **Target OS** of the provided unit should match the target OS used for compiling the client program.
  * If the provided unit depends on another unit _U_ , then the unit _U_ of the client context needs to be _compatible_ with the unit _U_ in the provider context. For that purpose, the providing compiler embeds a checksum of the **interface section** of unit _U_ in the ***.ppu** file of the provided unit. The client compiler checks the embedded checksum against the checksum of unit _U_ in the client context. If the checksums differ, then the client compiler will attempt to recompile the provided unit, and this will fail because the source is missing. With the compiler option **-vu** you get more information on the handling of unit files, and you can spot a line stating _Recompiling ..., checksum changed for ..._.
  * In particular, the **System unit** of the provider context should be _compatible_ with the System unit of the client context, because every unit implicitly depends on the System unit. Therefore, it is recommended to use a _stable release_ of the compiler to compile the provided unit.
  * There may be some other compiler options to consider (besides setting the Target OS): 
    * **-M** (_Mode_)
    * **-C** (_Checking_), such as **-Cr** (_range checking_), **-Ci** (_i/o checking_), **-Co** (_overflow checking_), **-Ct** (_stack checking_)
    * **-Sa** (_Include assert statements in compiled code_)
    * **-O** (_Optimization_)
    * **-gl** (_Generating lineinfo code_)
  * For older versions of the FreePascal compiler, the name of the provided unit's source file should be in all lower case letters. For recent versions of the compiler, this is no longer an issue. (The _User's Manual_ is not up to date on this topic, I believe. **If you know more details, e.g. from which version on this changed, then please put it here.**)



## See also

  * [Creating a closed source package](<Lazarus_Packages.md> "Lazarus Packages")

---

_Source: [https://wiki.freepascal.org/Releasing_units_without_source_code](https://web.archive.org/web/20240920204214/https://wiki.freepascal.org/Releasing_units_without_source_code)_
