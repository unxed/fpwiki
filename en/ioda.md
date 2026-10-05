# ioda

## Contents

  * 1 About
  * 2 Features
    * 2.1 Master or Slave
    * 2.2 Logical Operators
    * 2.3 Wildcards and Regular Expressions
    * 2.4 Delete and Update Functions
    * 2.5 Sorting by Relevance
    * 2.6 Charsets
    * 2.7 Flexible Indexing through external Filters
    * 2.8 Tailor-made Data Structures
    * 2.9 Interfaces
  * 3 Example
  * 4 Compiling and Installing
  * 5 Download



## About

ioda is a fulltext database: a word indexing and retrieving engine. It stores unique words from a file or database source in a btree and their repeats in an flexible and highly space optimized list structure. Each stored word "knows" its source, position in the source and some (optional) info bytes. 

We use the term "database" for the summary of all files of an ioda data collection. I.e. if you have indexed your webserver's HTML files in an ioda database called "myserver", at the very least these ioda files make the database: myserver.config, myserver.btf, myserver.ocl and eventually myserver.ref. The only file you have to edit manually is the config file, where you describe the properties of the database. There can be some more helper files. 

## Features

### Master or Slave

ioda can be used standalone ("master mode") for archiving files. In this case it stores full file names and can archive whole directory trees - i.e. the whole webserver content - by one call. On the other hand, ioda can be used as an addon to an existing (i.e. SQL) database in "slave mode" and store the unique key of each database record as a reference to its words. 

### Logical Operators

For retrieving information, ioda handles logical operators (AND, OR, NOT, NEAR), parenthesis and optional word distance values (ie. AND.4). NEAR is an operator which means AND.50. The query parser of ioda is able to optimize a search path for complex queries like "(Albert or Alfred) and.1 Einstein) and Quant* not Physik*". 

### Wildcards and Regular Expressions

Beginning in Release 1.3, ioda can retrieve data with wildcards or regular expressions. I.e.: The word "barfooter" will be found with the query /foo/. This is similar to the wildcard notation *foo*. ioda internally converts wildcards mostly into regular expressions. 

### Delete and Update Functions

ioda can delete entries and update them by deleting the old version and inserting the new one. (Entries means the list of words from an article, a file etc.). ioda offers a merge function for merging two databases into one or for optimization purposes. In the last case, an existing database will be rebuilt with continuous word lists (which are impossible to create in the orginal archiving run without wasting much disk space). 

### Sorting by Relevance

There are some more features: ioda can sort hits by time (of file or database entry) or by weight. In the last case words (or combinations while using the AND operator) are appraised by their position in the text. ioda can (optionally) detect text doublettes using MD5 checksums and can ignore them or store them in a space optimized way. 

### Charsets

ioda can handle all ISO-8859-XX charsets and UTF-8. In the case of ISO charsets ioda can handle the casefolding (optional automatic uppercase function). When using UTF-8 the calling application has to handle all casefoldings. 

### Flexible Indexing through external Filters

For archiving whole directory trees, ioda needs the support of an external program. This can be written in any language and may work as a pipeline or may generate temporary files. ioda can store additional information on each word. Besides the mandatory information (source id, source position and a 16-bit-value for flags and other informations), each word can optionally have a timestamp and a 32-bit-value (instead of the 16-bit one). 

### Tailor-made Data Structures

The database structure of ioda consists of two or three parts, which are all designed by the author (non standard): 

  * The Bayer Baum, BTree, (*.btf): It stores all unique words, each pointing to...
  * The Word occurrence list (*.ocl): It stores information about the words, at least the file or database id (ie. unique key) as doubleword, the position (in word counts) in a word, the weight and an optional info byte. This can store information like "word is in title" or something else. ioda offers bigger data models for the occurrence list, ie. for storing a timestamp in each word or a source information. This bigger structure is mainly used for ioda standalone duties.
  * The File reference list (*.ref) is used for standalone service only. In this case, ioda manages the ids itself ("master mode") and the ids point to the entries in the fileref list (instead of getting ids from a master database). In the fileref list, a full path name is stored. It is possible to agree upon a base path at creating time of the ioda database which is a leading part of the full path and can be truncated (ie. a webserver root path) to avoid redundant information.



### Interfaces

From the source, four binaries can be built: 

  * ioda as a command line program (joda)
  * ioda as a server for client/server communicating over TCP sockets (jodad)
  * ioda as a linkable library (libjodafulltext.so). Interfaces to C, Perl, Python and PHP are published in the source package
  * ioda as a CGI program. This is only a trunc which does no HTML-formatting



## Example

ioda is in a production environment ie. as full text index to a Wikipedia mirror: <http://lexikon.rhein-zeitung.de>. Try a query with wildcard (*) to force a search or use this query: 
    
    
    ((Albert or Alfred) and.1 Einstein) and /^Quant.+sprung/) not Schrödinger
    

## Compiling and Installing

You can use the binares from the bin package immediatly under Linux. For compiling the sources, a Makefile is available in the source package. If you want to use the Perl and/or Python or PHP import modules, please install the source or the binary package first! To install all, you can extract the source package into one subdirectory. First call "make", then "make install" from the master Makefile to do all in one. The Free Pascal Compiler ≥ 1.9.3 is needed (recent version is 2.0). Important: Switch the Delphi mode in the fpc config file on (-S2)! No other libraries are required for the binaries. At the moment, it is only guaranteed that it runs under Linux. Under Windows, we have only tested read only until now. Theoretically it will be no or only little work to adapt ioda for all other OS which are supported by Free Pascal. 

## Download

Homepage: <http://ioda.sourceforge.net/>

---

_Source: [https://wiki.freepascal.org/ioda](https://web.archive.org/web/20231128155530/https://wiki.freepascal.org/ioda)_
