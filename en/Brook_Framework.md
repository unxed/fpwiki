# Brook Framework

## Contents

  * 1 About
  * 2 Homepage
  * 3 Comparison with Brook for Free Pascal
  * 4 Alternatives



# About

**Brook Framework** is a cross-platform microframework which helps to develop web Pascal applications built by Delphi or Lazarus IDE and Free Pascal. Its core has been developed using [libsagui](<https://risoflora.github.io/libsagui>), a cross-platform C library incorporating GNU libmicrohttpd, uthash, PCRE2, ZLib and GnuTLS. 

Author: Silvio Clecio 

License: GNU LGPL 

# Homepage

  * Get started, documentation, license, download and others details: [Home page](<https://risoflora.github.io/brookframework>).
  * [GitHub repository](<https://github.com/risoflora/brookframework>)



# Comparison with Brook for Free Pascal

[Brook for Free Pascal](<Brook_for_Free_Pascal.md> "Brook for Free Pascal") is an earlier web application library by the same developer. 

  * **Brook Framework** uses libmicrohttpd and GnuTLS, wrapped into libsagui, for its underlying HTTP/S functionality;
  * **Brook for Free Pascal** is pure Pascal and relies on the HTTP functionality provided by fcl-web, covering CGI, FastCGI and standalone.



For deployment of Brook Framework applications, it is necessary to bundle the application with the libsagui DLL/dylib/so file; depending on how libsagui is built, it may be necessary to also bundle other dynamic library files that libsagui depends on. 

# Alternatives

  * [mORMot](<https://github.com/synopse/mORMot>) \- Synopse mORMot ORM/SOA/MVC framework.
  * [FreeSpider](<https://github.com/motaz/freespider>) \- Web development package for Free Pascal/Lazarus.
  * [FCL-Web](<fcl-web.md>) Built-in Free Pascal web library.
  * [Fano Framework](<https://fanoframework.github.io>) Web application framework for modern Pascal programming language.

---

_Source: [https://wiki.freepascal.org/Brook_Framework](https://web.archive.org/web/20250424104929/https://wiki.freepascal.org/Brook_Framework)_
