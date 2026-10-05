# Case Studies

│ **English (en)** │

The following **case studies** provide the reader with examples, where Lazarus and Free Pascal have been used for successful development of professional applications for business, game development, science, health-care and other fields. 

## Contents

  * 1 Business Case Studies
    * 1.1 HCR Imaging, Inc.
    * 1.2 OpenLab
    * 1.3 ssn.at / ssnDocCenter
  * 2 Game development
    * 2.1 Game Maker
  * 3 Scientific and Medical Case Studies
    * 3.1 CityCAT
      * 3.1.1 External links
    * 3.2 SimThyr and SPINA Thyr: Two biomedical applications for investigation of thyroid homeostasis
      * 3.2.1 Download links
      * 3.2.2 Academic citation
  * 4 High-performance computing
    * 4.1 Free Pascal on Mainframes



## Business Case Studies

### HCR Imaging, Inc.

My day job is Director of IT for a document management company. We scan around 60,000 images of paper medical records on a daily basis. We use a mixture of Linux and Windows servers and workstations, and my network pushes around 30 GB of data around per work shift. We're a small business (14 employees), and providing solutions for our customers is our priority, not just selling a service or project. Read on for how we use Lazarus in our day to day operations. 

I'm a strong advocate of Linux, so I've attempted to switch as many machines in the office to Linux as possible. This has left me with a mixture of Windows and Linux machines, because the main application we use to produce our product is Win32 only. 

Generally what we do is scan medical records for large hospitals, and produce those on CD or DVD for archive purposes. The original paper is then shredded. Most of our customers access the information directly from the CD or DVD, but a few have special servers in place to act as repositories. 

In the healthcare field, there is no one standard for management software. This has left us the task of integrating data we produce with our software into various types of management packages. Due to the large volume of data we work with, we have special servers designed only for document and index conversion. 

Lazarus and Free Pascal have allowed me to write very quick and easy (RAD) utilities that allow us to convert the data and images over to the appropriate format, after being exported from our own server. Since I have some machines in Linux and some in Windows, in most cases I'm able to compile the same conversion utility for either environment, depending upon what server is available at the time. The ease of the Lazarus IDE and RAD design allow us to take a basic framework application that I've developed as a series of objects and modify it for use for new formats as they arise, in a very short period of time. 

We also acquired some tape backups from old AIX servers that have MS Word documents embedded within a proprietary database. Windows won't even recognize the tapes. Using a combination of Lazarus, OpenOffice and the standard Linux tape utilities, I'm able to extract the indexes and embedded documents from those tapes, convert them to TIFF images using OpenOffice and burn them in a format ready for import in a new system, completely unattended. 

Lazarus is the glue that helps us provide that "value add" to our customers that many companies lack. Its RAD design, similarity with previous Pascal IDE's that we were already familiar with and cross-platform nature have allowed us to provide services we never would have been able to before. 

[Tony Maro](</User:Tonymaro> "User:Tonymaro")

Dir. Information Technology 

HCR Imaging, Inc. 

<http://www.hcrimaging.com>

* * *

### OpenLab

See [Case Study: Using Lazarus to demystify Linux](<http://lxer.com/module/newswire/view/47883/index.html>). 

  


* * *

### ssn.at / ssnDocCenter

We were requested to write a solution for a VoIP-Provider to establish a Fax-Service for their customers. It had to accept faxes as a Windows-Printer-Driver and manage in- and outgoing faxes via a Windows application mainly residing in the Taskbar-Notification Area. Additionally all communication should go through HTML - Port 80. 

As the initial budget was really low and we have a lot of Pascal experience we decided to use Free Pascal / Lazarus for the entire project. We already did some minor jobs with this combination, but this should be the first written entirely with it. 

Now, as the application is up, running and deployed to some customers, we're quite happy with the decision towards FPC. 

The application consists of several parts: 

First there is a Linux daemon talking to a Firebird RDBMS, using sockets to talk to a Hylafax server and providing a proprietary sockets-based interface to some client process. 

Second, there is an Apache CGI module, providing the interface between the proprietary sockets-based interface of the daemon and HTTP. 

Third, there are some command-line utilities for user management, enabling the whole application to be embedded in a script-driven user management. 

And fourth there is a Windows client for the end user containing a user interface and a Windows printer monitor to catch printouts and deliver it to the server daemon either via the socket interface or via HTTP. 

Everything is coded in 100% Object Pascal using Lazarus as IDE and Lazarus for creating the user interface - even the DLLs providing the printer monitor! 

[Lukas Gradl](</index.php?title=User:Grl&action=edit&redlink=1> "User:Grl \(page does not exist\)")

<http://www.ssn.at>

## Game development

### Game Maker

[Game Maker](<http://www.yoyogames.com/gamemaker>) allows you to make computer games using easy to learn drag-and-drop actions. You can create professional looking games within very little time. You can make games with backgrounds, animated graphics, music and sound effects, and even 3D games! And when you've become more experienced, there is a built-in programming language, which gives you the full flexibility of creating games with Game Maker. What is best, is the fact that Game Maker can be used free of charge. 

Originally written in Delphi, Lazarus/Free Pascal was used to port it from Win32 to Mac OS X and could be used in future for Linux and other platforms. 

## Scientific and Medical Case Studies

### CityCAT

Urban flood risk modelling is a highly topical example of intensive computational processing. Such processing is increasingly required by a range of organisations including local government, engineering consultancies and the insurance industry to fulfil statutory requirements and provide professional services. [CityCAT](<http://www.ncl.ac.uk/ceser/researchprogramme/integratedsystemsdemonstrationscities/citycaturbanfloodmodel/>) (City Catchment Analysis Tool), developed at Newcastle University, is a unique software tool for modelling, analysis and visualisation of surface water flooding. It enables rapid assessment of combined pluvial and fluvial flood risk and effects of different flood alleviation measures. By using Lazarus and Free Pascal CityCAT could be successfully ported from Windows to additional platforms, thus supporting Cloud Computing. Parameter sweep work on an Infrastructure as a Service Cloud allowed for very large simulation runs (∼21 months of processing in a single calendar month). 

#### External links

  * [Information about CityCAT at Newcastle University](<http://www.ncl.ac.uk/ceser/researchprogramme/integratedsystemsdemonstrationscities/citycaturbanfloodmodel/>)
  * Scientific paper describing the implementation of CityCAT: V. Glenis, A. S. McGough, V. Kutija, C. Kilsby and S. Woodman. [Flood modelling for cities using Cloud computing](<http://www.journalofcloudcomputing.com/content/2/1/7>) Journal of Cloud Computing: Advances, Systems and Applications 2013, 2:7. doi: [10.1186/2192-113X-2-7](<http://dx.doi.org/10.1186/2192-113X-2-7>)



  


### SimThyr and SPINA Thyr: Two biomedical applications for investigation of thyroid homeostasis

[![thyrson.png](https://wiki.freepascal.org/images/8/8c/thyrson.png)](</File:thyrson.png>)

Thyroid hormones play an essential role for the control of metabolism, growth and differentiation. Therefore, exact regulation of thyroid hormone levels is vital for most organisms, and its disorders lead to significant - and in extremities even life-threatening - diseases. This is the more important as at least 5 percent of adult persons in developed countries suffer from different forms of functional thyroid disease. This corresponds to more than 6 million affected people in Japan, 16 million individuals in the USA and 25 million persons in the European Union. 

Although the principles of thyrotropic feedback control have been described decades ago we still have not yet gained a comprehensive understanding of its dynamics. Additionally, diagnosis of thyroid disorders may be complex and ambiguous in certain situations. This was the initial point of an interdisciplinary research programme aiming at a better understanding and improved diagnosis of thyroid homeostasis from a cybernetic basis. It was conducted in international cooperation of three German universities and researchers in the United Kingdom, the Netherlands, Singapore and Australia. The programme resulted in a new mathematical theory of thyroid feedback control that helped to explain multiple previously poorly understood phenomena like partial central hypothyroidism and non-thyroidal illness syndrome. As a part of this scientific programme two software applications have been developed, **SimThyr** , a continuous numerical simulation program of thyroid homeostasis and **SPINA Thyr** , an application that helps to determine constant structure parameters of the feedback loop. 

Both applications have been developed in the end of the 20th century for classical Mac OS in [THINK Pascal](<http://www.think-pascal.org>) and CodeWarrior Pascal. Alternative versions of SPINA Thyr were subsequently made available for Windows and Linux with Delphi or Kylix, respectively, and for Palm OS with [winsoft PocketStudio](<http://www.winsoft.sk/pstudio.htm>). Fundamental changes in the landscape of operating systems, first of all the advent of Mac OS X and new Windows versions, required a redesign of these applications. 

In appreciation of the "Write Once - Compile Anywhere" priniciple Lazarus and Free Pascal were selected. This decision allowed for reuse of large portions of the old Pascal-based source-code while providing a modern common code base for all GUI parts on several operating systems. Porting to Lazarus and Free Pascal was done in a few weeks for both applications, although simultaneously multiple new features have been implemented, too. 

Today, both SPINA Thyr and SimThyr are available in mature versions for Mac OS X, Windows and Linux. Code maintenance is easy and straightforward due to the common code base. 

[![SimThyr for Linux](https://wiki.freepascal.org/images/a/a6/pr.simthyr_suse_linux_11.jpg)](</File:pr.simthyr_suse_linux_11.jpg> "SimThyr for Linux") | [![SPINA Thyr for Mac OS X](https://wiki.freepascal.org/images/2/2b/SPINA_Thyr_3.3_for_Mac_2.png)](</File:SPINA_Thyr_3.3_for_Mac_2.png> "SPINA Thyr for Mac OS X")  
---|---  
SimThyr for Linux | SPINA Thyr for Mac OS X   
  
Both software tools facilitated the publication of multiple [scientific papers](<http://sourceforge.net/p/simthyr/home/References/>) that helped to lift our understanding of thyroid homeostasis to a new level and that also demonstrated improved options for the diagnosis of thyroid disorders. SPINA Thyr and SimThyr are today employed by professional users in more than 100 countries for research, education and patient-care. 

#### Download links

  * [SPINA Thyr:](<http://spina.sourceforge.net>) Open source software for calculating thyroid' secretory capacity (GT or SPINA-GT) and total deiodinase activity (GD or SPINA-GD).
  * [SimThyr:](<http://simthyr.sourceforge.net>) A continuous simulator of thyroid homeostasis.



#### Academic citation

  * Dietrich JW et al. (1994-2016) SimThyr 3.3. Fairfax, VA: sourceforge, <http://simthyr.sf.net>. [RRID:SCR_014351](<http://scicrunch.org/browse/resources/SCR_014351>).
  * Dietrich JW et al. (1994-2016) SPINA Thyr 4.0. Fairfax, VA: sourceforge, <http://spina.sf.net>. [RRID:SCR_014352](<http://scicrunch.org/browse/resources/SCR_014352>).



## High-performance computing

### Free Pascal on Mainframes

Free Pascal is currently being ported to IBM zSeries mainframes. Current progress is documented in a series of [articles](<ZSeries.md> "ZSeries") in this wiki.

---

_Source: [https://wiki.freepascal.org/Marketing_Case_Studies](https://web.archive.org/web/20241207110158/https://wiki.freepascal.org/Marketing_Case_Studies)_
