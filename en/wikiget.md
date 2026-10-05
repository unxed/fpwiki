# wikiget

│ **English (en)** │

## Introduction

Wikiget is a console utility to download the Lazarus wiki. Maybe it also works for other MediaWikis sites. You find it in the basic installation of Lazarus in the directory LazarusDir/components/wiki. 

## Using wikiget

`wikiget -h`

Shows all parameters and examples. 

### Command line parameters

    \--dir=<directory> : directory in which to store the files. Default:
    \--images=<directory> : directory in which to store the images. Default: images
    \--baseurl=<URL> : URL of the wiki. Default: http://wiki.freepascal.org/
    \--page=<pagename> : download this wiki page. Can be given multiple times.
    \--allmissing : download all wiki pages, if page file not already there.
    \--recent=<days> : download pages again if changed in the last days on the site. includes --allmissing.
    \--ignore-recent=<minutes> : do not download again files younger than this on disk. combine with --recent. Default:
    \--shownotusedpages : show unused files in the output directory.
    \--deletenotusedpages : delete the files in the output directory that are not used.
    \--shownotusedimages : show inused files in the images directory.
    \--deletenotusedimages : delete the files in the images directory that are not used.
    \--nowrite : do not write files, just print what would be written.

---

_Source: [https://wiki.freepascal.org/wikiget](https://web.archive.org/web/20250420071344/https://wiki.freepascal.org/wikiget)_
