# Migration to mantis

The FPC bug repository will be migrated to mantis. This page collects the necessary steps to do so. 

The current FPC bug db has the following fields with the given mapping: 

BugId int(11) => custom field old bug id 

Title varchar(128) default NULL => Summary 

Name varchar(80) => additional information 

Email varchar(80) => additional information 

AddDate date => date submitted 

FixDate date => last updated/entry in mantis_bug_history_table 

Fixer varchar(80) => Fixer account/entry in mantis_bug_history_table 

FixVersion varchar(30) >= Fixed in Version 

Category tinyint(4) => Category 

Descr => description 

Prog text => program upload 

Status` enum('Unfixed','Fixed','Unreproducable','Not a bug') Fixed => Resolved/Closed; Unfixed => New; Unreproducable => Comment/Closed; Not a bug => Comment/Closed 

    Alternatively, use a combination of status and resolution: Fixed => Closed / Fixed; Unfixed => New / Open; Unrepoducable => Closed / Unable to reproduce; Not a bug => Closed / No change required. [Vincent](</User:Vincent> "User:Vincent") 00:40, 17 May 2006 (CEST)

BugVersion varchar(8) => Product version 

bugtype tinyint(4) => Severity 

`comment` text 

os varchar(10)

---

_Source: [https://wiki.freepascal.org/Migration_to_mantis](https://web.archive.org/web/20250323220414/https://wiki.freepascal.org/Migration_to_mantis)_
