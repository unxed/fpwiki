# fpctwit

## Contents

  * 1 About
  * 2 Details
  * 3 License
  * 4 Download



## About

**fpctwit** is an OAuthv1, Twitter and Plurk library with example programs (console and GUI). 

It is written for Free Pascal, but meant to be usable in Delphi as well. It currently uses the **Synapse** network library (included) but can be adapted for other network libraries. 

Note: in the default configuration, you will need the OpenSSL libraries, e.g. sselay32.dll and libeay32.dll, and presumably zlib1.dll. 

Authors: [Reinier Olislagers](</User:BigChimp> "User:BigChimp"), Mario Ray Mahardhika, Ludo Brands. 

## Details

Twitter is a major user of the OAuthv1 authentication protocol, but there are others. 

The Twitter library's functionality includes: getting tweets using the Search API, tweeting, getting username info, authenticating using PIN/OOB or preset credentials (consumerkey+secret, authtoken+secret). 

It supports OAuthv1 secure authentication and TLS encryption. The library uses modified Free Pascal fpJSON units to allow getting UTF8 data out of the JSON data with Tweets returned from Twitter. For newer FPC versions, it will use the native fpJSON units as those support UTF8. 

Demos: 

  * The fpctwit console demo program and fpctwitgui GUI demo programs demonstrate the functionality of the Twitter and OAuthv1 libraries.
  * The fpctwit console demo also uses the Twitter library's Streaming API support to process and show Tweets and deletion messages.
  * The fpcplurktest console demo demonstrates use of the Plurk library (plurklib.pp).



## License

The applications and libraries are distributed under a very liberal license (MIT license; see Synapse and fpJSON units for their licenses) which allows commercial use as well as interoperability with GPL programs. 

## Download

Original GitHub repository is gone. There is a backup in GitHub repository: <https://github.com/serbod/reiniero/tree/master/fpctwit>

---

_Source: [https://wiki.freepascal.org/fpctwit](https://web.archive.org/web/20221006132341/https://wiki.freepascal.org/fpctwit)_
