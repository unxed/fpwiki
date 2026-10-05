# Networking libraries

│ **English (en)** │

Name  | Developers  | Platforms  | License  | Supported protocols  | Remarks   
---|---|---|---|---|---  
[lNet](<lNet.md> "lNet") | Aleš Katona | Windows, Linux, macOS, FreeBSD | Modified LGPL | TCP, UDP, HTTP, HTTPS, FTP, SMTP, TELNET | Lazarus/FPC   
[Synapse](<Synapse.md> "Synapse") | Lukas Gebauer | Windows, Linux, macOS, FreeBSD | [BSD style license](<http://www.ararat.cz/synapse/doku.php/license>) | TCP, UDP, HTTP, HTTPS, FTP, SMTP, SNMP, NTP, POP3, PING, IMAP, LDAP, FTPS, DNS | Runs on both Delphi and Lazarus/FPC   
[Indy](<Indy_with_Lazarus.md> "Indy with Lazarus") | team | Windows, Linux, macOS, iOS, Android | MPL, modified BSD | numerous protocols | Runs on both Delphi and Lazarus/FPC   
[Internet Tools](<Internet_Tools.md> "Internet Tools") | Benito van der Zander | Windows, Linux, macOS, Android | GPL | HTTP, HTTPS   
[**IP*Works!**](<http://www.nsoftware.com/ipworks/#plat-delphi>) | team | Windows, Linux, macOS | Commercial | numerous protocols | Runs on both Delphi and Lazarus/FPC; requires .NET Framework 2.0 - 4.8.   
[**ICS**](<http://www.overbyte.eu/>) | François Piette | Windows | Freeware(*) | numerous protocols | Delphi/FPC. Kylix/FPC is a separate, abandoned codebase   
[**miniSocket**](<https://github.com/parmaja/minilib>) | Zaher Dirkey, Bilal Alhamad | Windows, Linux, macOS | Modified LGPL | TCP, HTTP, HTTPS, SMTP, OpenSSL, TLS1.3 | Delphi and FPC   
[**CrossSocket**](<https://github.com/winddriver/Delphi-Cross-Socket>) | WindDriver | Windows, Linux, macOS, Android | LGPL-3.0 license | numerous protocols | Delphi and FPC   
  
(*) request to send a postcard when used in production. 

## Web Frameworks

It's actually hard to define what particular functionality a web-framework should provide. At least it should be able to communicate with a web-server OR even provide web-server functionality itself. 

The content and a coverage of each framework listed below varies. Some libraries provide functionality that is implemented in other libraries, i.e. HTML creation, database interaction, encryption, handling archive files, while others do not. It's questionable, if such functionality is mandatory for a web-framework. 

Library  | Link  | Notes   
---|---|---  
[fcl-web](<fcl-web.md> "fcl-web") | FPC packages  |   
ExtPascal  | <https://github.com/farshadmohajeri/extpascal> | GPLv3   
Brook  | <https://github.com/risoflora/brookfreepascal> | LGPLv2.1   
[mORMot](<mORMot.md> "mORMot") | <https://github.com/synopse/mORMot> | MPLv1.1 GPLv2.0 LGPLv2.1   
Fano Framework  | <https://github.com/fanoframework/fano> <https://fanoframework.github.io/> | MIT   
[Powtils](<Powtils.md> "Powtils") | <https://github.com/z505/powtils> |   
FastPlaz  | <https://github.com/fastplaz/fastplaz> <https://github.com/fastplaz> | Freeware? The library is based on fcl-web in order to handle the communication. Provides high level MVC routines.   
  
### Comparison by WebServer Communication

Library  | CGI  | FastCGI  | SCGI  | Apache Module  | uWSGI   
---|---|---|---|---|---  
fcl-web  | Yes  | Yes  | No  | Yes  | No   
ExtPascal  | Yes  | Yes  | No  | No  | No   
Brook  | Yes  | Yes  | No  | No  | No   
mORMot  | Yes  | Yes  | No  | No  | No   
Fano  | Yes  | Yes  | Yes  | No  | Yes   
Powtils  | Yes  | No  | No  | No  | No

---

_Source: [https://wiki.freepascal.org/Networking_libraries](https://web.archive.org/web/20250429010340/https://wiki.freepascal.org/Networking_libraries)_
