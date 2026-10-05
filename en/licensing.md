# licensing

│ **English (en)** │  **[русский (ru)](<../ru/licensing.md>)** │

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** This page refers to Lazarus in a sometimes misleading way. It either refers to Lazarus as a whole, or makes a distinction between "the libraries" and "the environment" (IDE).  
However "the libraries" in this context should be restricted to the LCL (code in the lcl folder). Other libraries (e.g. in the components folder) sometimes follow the licensing of the LCL, but sometimes have entirely different licenses. 

  


## Contents

  * 1 Licensing relating to FPC/Lazarus usage
  * 2 Licensing relating to FPC (the compiler)/Lazarus program (IDE) itself
  * 3 GPL related Source retention
  * 4 The MPL problem
    * 4.1 MPL issues relating to Lazarus
      * 4.1.1 Applications developed with Lazarus
      * 4.1.2 Source and preinstalled commercial components
    * 4.2 Commercial 3rd party plugins
    * 4.3 MPL issues relating to FPC
      * 4.3.1 Maintenance/release engineering
    * 4.4 Choosing different licenses for FPC distributions
  * 5 Dual licensed packages in the FPC/Lazarus distributions
  * 6 Licensing related to Lazarus Reference Documentation
  * 7 Licensing related to Other Lazarus Reference Documentation
  * 8 Licensing/copyright of Wiki articles



## Licensing relating to FPC/Lazarus usage

Free Pascal licensing is pretty liberal, including some additional clauses to avoid some people's fear and some practical concerns regarding [LGPL](<LGPL.md> "LGPL")/[GPL](<GPL.md> "GPL") licensing. The primary difference is that static linking to FPC runtime libraries is allowed on top of the LGPL, which allows shared linking only. Making such exception clauses are pretty standard for runtime libraries: [[GPL with linking exception](<http://en.wikipedia.org/wiki/GPL_linking_exception>)]. The LGPL is such an exception that is target to linking of shared objects; the FPC modification goes one step beyond and also allows static linking. 

The base licensing principle is that usage of FPC and its libraries should be possible in a normal responsible way. This means that anything that is linked into end users programs should be usable in commercial programs in the classical sense, without heaps of additional clauses that make licensing a puzzle. This is realised by choosing for the LGPL license, with additional clauses that remove any doubt or double interpretations about static linking. This combined license (LGPL formal text + additional FPC disambiguation) is commonly referred to as "[FPC modified LGPL](<FPC_modified_LGPL.md> "FPC modified LGPL")". The condition is: 
    
    
    As a special exception, the copyright holders of this library give you permission to link this library with independent modules to produce an executable, regardless of the license terms of these independent modules, [..]
    

This clause is mainly meant to avoid (maybe even justified) uncertainties about the "shared linking only" and "reverse engineering" clauses in the LGPL. 

Lazarus follows the same principles. 

## Licensing relating to FPC (the compiler)/Lazarus program (IDE) itself

The FPC and Lazarus binaries are mostly GPL. However products made by GPL programs are not automatically subject to the GPL, so this only applies if you modify or integrate (by static linking) the core compiler binary itself. Since there are many misconceptions about the actual meaning of GPL, in case of doubt **ASK** (on the forum or mailing lists)! 

Below are some of the more popular misconceptions and their rebuttal: 

  * If I distribute FPC with my application for internal scripting, I must also package the FPC source.



    No, a link to the FPC site in some copyright addendum is enough, though you should keep the source yourself for three years, see the next paragraph

  * If I modify FPC and distribute it with my application (but am not linked to it), I must ship the source of my application.



    No, only if you link your application to GPLed code. If you package it, modifications (the modified source) on your site or distribution media is enough.

  * If I link to GPL code, I must publish my own sources.



    Yes, but only when you distribute them outside your organization. See Lazarus licensing section and [[1]](<http://www.gnu.org/licenses/gpl-faq.html#InternalDistribution>)) Note that FPC is set up in a way that linking to GPL code is usually not needed.

So in short the GPL only applies if you try to integrate a core FPC binary (fpc.exe, ppudump.exe etc) into your application. Not if you merely call it. There is one unpleasant side detail though, see the Lazarus licensing section. 

(Note: Strictly speaking the GPL specifies that the source must be provided on request by the one shipping it for a "nominal fee covering costs", but since work is also cost, you can charge based on a commercial hourly rate for that, and as high as 25-50 Euro per incident. This makes this avenue no problem in practice, since everybody will rather download, and the few that don't, pay for the privilege.) 

## GPL related Source retention

If you distribute anything under the GPL, you should keep the complete sources of the GPLed part (your own and any 3rd party parts) for three years to fulfill requests for the source (for which you can charge a nominal fee). 

Note that for the average Lazarus application this should not be possible. The most likely case is if you use the mysql (client) without separate commercial license requirement. 

  * See paragraph 4.1.2 in [[2]](<http://www.softwarefreedom.org/resources/2008/compliance-guide.html>), which, in general, is a practical licensing resource.



## The MPL problem

According to GNU, the [Mozilla Public License](<Mozilla_Public_License.md> "Mozilla Public License") v1.1 is GPL incompatible on hard to explain technicalities (which to be honest I don't understand myself either). If this is true or not doesn't matter, the uncertainty alone makes the MPL-GPL combination dangerous, at least until either of the licensing authorities clarifies and resolves their stance. 

This is a big problem for the FPC project since it uses several binaries that are GPL, while the majority of the open source components in the Delphi world are MPL (that being Borland's choice for Jedi and companion CDs). 

Relicensing on the FPC side is hard, since the project is over 14 years old, and has numerous contributors. Fortunately, there is an escape in some cases: the MPL provides a clause to dual license LGPL-MPL, though this clause must be explicitely invoked in the license statement. This is effectively a relicensing (even though it is more a technicality to satisfy GNU than a massive licensing change. 

As far as I can gather from the license text and the web, the MPL-LGPL differences are more a difference in viewpoint and wording, respectively: 

  * open source ideological (GNU)
  * corporate legalese(MPL)



than in content). 

However: for any relicensing, even if in practice a technicality, you must obtain consent from all (major) contributors. 

If your MPL project still is in touch with all its contributors, you could ask their permission to relicense under the MPL dual license. In the past Jedi APILib and Jedi SDL did this (and are now part of the FPC distribution). Jedi Math completed it (and JediMath will be included in the FPC distribution at a later point). 

The Jedi JCL had discussions about it but the exact final outcome is unknown (to do: insert details once found). Their case is probably also more complicated by more frequently accepting large external donations. 

A link that does a reasonable job of summing up GPL and MPL: <http://www.tomhull.com/ocston/docs/mozgpl.html>

### MPL issues relating to Lazarus

Licensing of Lazarus is pretty much the same as with FPC itself. The libraries are modified LGPL, the environment itself is GPL. However, designtime libraries link into Lazarus, making them subject to the GPL. 

The Lazarus teams defuses this situation by using a property of the GPL: GPL doesn't apply until you distribute the result. In this case, the result/product in the GPL license sense is the Lazarus binary with plugged-in components itself. 

#### Applications developed with Lazarus

The GPL does not cover any application binary created with Lazarus. The application binary itself is only limited by the components you actually link to, and in the FPC/Lazarus project those are all LGPL_with_exception. So though potentially confusing this licensing is not a problem when developing binaries with Lazarus, even if you have commercial components with designtime parts. This because you only have to open the source when you distribute the result (i.e. Lazarus with preinstalled components). 

#### Source and preinstalled commercial components

As indicated above, you can also merrily distribute your commercial components and let the users compile them into Lazarus themselves. 

However, there is one side effect to this. If you distribute a Lazarus binary with components preinstalled, these must be GPL compatible. In other words, it makes it impossible for e.g. component vendors to provide a Lazarus binary with components installed, or create a Lazarus distro with the MPL-only Jedi components preinstalled. 

**Note:** Contrary to popular opinion, this situation **doesn't** change at all when designtime packages would be dynamically loaded using a **[packages system](<packages.md> "packages")**. The entire linked form is still a "larger work" in the GPL sense. **Note:** : BigChimp, June 2013: I'd like to see the reasoning behind the statement above, as I'm not sure it applies. Regardless, some extra factual information to justify this statement will be nice. **Note:** : Marcov, finally answers in April 2024: if it were not, people would simply put GPL codebases in DLLs/.sos and could build anything on top. Moreover, the GPL license test doesn't say anything about the form of link (while LGPL does) 

### Commercial 3rd party plugins

The above **MPL issues relating to Lazarus** paragraph also applies to e.g. 3rd party plugins. 

### MPL issues relating to FPC

MPL issues in general only affect the official FPC release distribution, not the programs generated with Free Pascal (unless the generated programs are GPL), since unlike Lazarus, no linkage to the compiler's GPL code is needed for normal operation. 

We wish to keep the FPC distribution licensing somewhat uniform for the benefit of our users. This means the following licenses: for 

  * libraries: LGPL-modified, or something fully compatible (like e.g. a (modified/modern) BSD license), and
  * for end user binaries: GPL, LGPL, BSD or PD (public domain). In general anything free (the FSF or the normal way) that is GPL compatible (e.g. MIT).
  * build scripts, Makefiles and the like are mostly considered throw away tools, and their licensing is usually not explicitely locked down.



The result of this policy is that strict MPL libraries must not be added to the core FPC distribution at this point. Several projects (Jedi's Apilib, -Math and -SDL ) kindly dual-licensed their offerings to LGPL-modified though to work around this, and were added, for which we are eternally grateful. Some 3rd party component distributors (like VirtualStringTree) appear to have changed even without asking, which I hope means it will turn up in Lazarus soon. 

Some other projects (e.g destructor.de's libtar) moved away from the original license of their own design and re-licensed their sources under a GPL/LGPL compatible license, for consistency's sake. Again, our eternal gratitude will accompany him. Especially because it was a package we urgently needed :-) 

#### Maintenance/release engineering

Note that inclusion of a package in the general FPC or Lazarus distribution does not imply that the FPC/Lazarus core development teams take control/maintainership of a package. 

It is merely done to create a more clear and synchronized versioning (of the package with the last FPC/Lazarus release), and ease of use. It also moves the FPC specific release engineering of the build process to FPC, allowing the source projects to focus on Delphi release engineering. 

In general these directories in the FPC repository are multi-licensed (original license and modified LGPL), and all fixes accepted for them are considered such. (In other words, contributions to Jedi winapi are considered to be also MPLed, so that the original authors of the packages can merge them back) 

### Choosing different licenses for FPC distributions

Third party groups can still create custom FPC distributions that are a patchwork of licenses, as long as they are compatible with each other. The GPL/LGPL compability of the official distribution is self imposed only to make licensing of the main distribution consistent. 

## Dual licensed packages in the FPC/Lazarus distributions

As said before, some packages like SDL are kindly dual-licensed by their original authors. We respect that, and therefore only will accept contributions that are similarly dual-licensed, since we must be able to pass the fixes back to the original authors. 

## Licensing related to Lazarus Reference Documentation

Lazarus distributes reference documentation for the LCL (Lazarus Component Library) and LazUtils (Lazarus Utilities) packages. The documentation is published using the Creative Commons Attribution-ShareAlike 4.0 International public license. The license is available at <https://creativecommons.org/licenses/by-sa/4.0/legalcode.txt>, as well as in the $(LazarusDir)/docs/ directory. 

The Creative Commons license ensures that you are free to: 

  * Share — copy and redistribute the material in any medium or format for any purpose, even commercially.


  * Adapt — remix, transform, and build upon the material for any purpose, even commercially.


  * The licensor cannot revoke these freedoms as long as you follow the license terms.



Under the following terms: 

  * Attribution - You must give appropriate credit, provide a link to the license, and indicate if changes were made. You may do so in any reasonable manner, but not in any way that suggests the licensor endorses you or your use.


  * ShareAlike - If you remix, transform, or build upon the material, you must distribute your contributions under the same license as the original.


  * No additional restrictions - You may not apply legal terms or technological measures that legally restrict others from doing anything the license permits.



Notices: 

You do not have to comply with the license for elements of the material in the public domain or where your use is permitted by an applicable exception or limitation. 

No warranties are given. The license may not give you all of the permissions necessary for your intended use. For example, other rights such as publicity, privacy, or moral rights may limit how you use the material. 

## Licensing related to Other Lazarus Reference Documentation

In the future, it is anticipated that reference materials for other packages will be distributed with Lazarus. These materials may be subject to another license as specified in their content. 

## Licensing/copyright of Wiki articles

As there is no specific license indicated on the wiki, all author's rights and copyrights of submissions remain with the original authors. The only rights they grant to others for contributions they authored themselves are the ones mentioned in the submission form: 

> Please note that all contributions to Lazarus wiki may be edited, altered, or removed by other contributors. If you do not want your writing to be edited mercilessly, then do not submit it here.  
>  You are also promising us that you wrote this yourself, or copied it from a public domain or similar free resource (see $1 for details). **Do not submit copyrighted work without permission!**

Content that was copied from elsewhere must be public domain or a similar free resource, and the license conditions of the original work must be respected (e.g. in terms of possible requirements regarding mentioning the source or original author).

---

_Source: [https://wiki.freepascal.org/licensing](https://web.archive.org/web/20250512082052/https://wiki.freepascal.org/licensing)_
