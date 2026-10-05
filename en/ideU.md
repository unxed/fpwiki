# ideU

ideU the ide. Universal. <https://github.com/fredvs/ideU>

Multi OS, multi languages, multi compilers. 

ideU is based on MSEide by Martin Schreiber. <http://sourceforge.net/projects/mseide-msegui/>

There are binaries-executable with plugin fpGUI uidesigner_ext_ideu and sak assistive voice speacher included. For Windows 64/32, Linux 64/32 and FreeBSD 64/32. 

To see ideU in operation (in polYdev) look at the last message in this forum thread (it has a link to a 15 MB video): <https://forums.freebsd.org/threads/53085/>

To see ideU in operation (in translating fpc applications into Java applications) watch this 6 MB video: <https://sites.google.com/site/designerext/fpc_java.mp4> and follow these instructions: <http://lists.freepascal.org/pipermail/fpc-pascal/2017-April/050711.html> with these two changes: 1) Replace "begin MainProc;" with "exports ..." 2) Add "-fPIC" to compile parameters 

Installation: 

1\. Download the IDE binary: <https://github.com/fredvs/ideU/releases>

2\. To install ideU, just unzip the release and run ideU. 

For Unix system: (The last character of ideU is a U majuscule) 

  1. /directory/of/ideU/ideU 



For Windows system: 

  1. C:\directory\of\ideU\ideU.exe 



3\. To reset the config to default, delete the folders: 

For Windows: C:\Users\You_User\AppData\Local\ideU and C:\Users\You_User\AppData\Roaming\\.ideu 

For Unix: /home_user_directory/.ideu and /home_user_directory/.config/ideU

---

_Source: [https://wiki.freepascal.org/ideU](https://web.archive.org/web/20190715073900/https://wiki.freepascal.org/ideU)_
