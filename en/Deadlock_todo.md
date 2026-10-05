# Deadlock todo

## TODO list for deadlock shutdown

  1. ftpmaster.freepascal.org 
     * Create equivalent of <http://www.nl.freepascal.org/logs/> directory (probably on <http://www.de.freepascal.org>), i.e. make ~fpc/logs/ directory accessible via http - ???
     * Change all snapshot scripts using FTP for upload to scp - John, Armin, Tomas, ???
     * Migrate all cronjobs (most of them or all should to berlin220) - that includes source zips and html zips for mirrors - ???
     * Contact mirror administrators and make sure all mirrors use ftpmaster.freepascal.org address - ???
     * Change the DNS record - Michael
     * Configure a special mirror login to the ftp server
     * Configure rsync.
  2. lists.freepascal.org 
     * Decide where to move (berlin220 x idefix) - all
     * Configure the mailing list software and setup the new mailing lists - ???
     * Temporarily make subscription changes on deadlock inaccessible - Daniel (?)
     * Setup e-mail address scrambling for list archives on the new server (depending on features of the new mailing list software) - ???
     * Transfer subscription information to new server - Daniel and ???
     * Change the DNS record - Michael
     * Migrate list archives (probably after some time to avoid lost e-mails due to different DNS refresh times) - ???
  3. community.freepascal.org 
     * Decide where to move (berlin220 x idefix) - all 
       * Current software needs PostgreSQL (or Oracle, don't know about Firebird) and AOLserver. [Daniel-fpc](</index.php?title=User:Daniel-fpc&action=edit&redlink=1> "User:Daniel-fpc \(page does not exist\)")
     * Decide whether we would continue using current forum software or different solution - ??? 
       * Current software totally outdated (no updates done since I put it live). Did some work to migrate to OpenACS 5, but am not satisfied with the available conversion scripts. [Daniel-fpc](</index.php?title=User:Daniel-fpc&action=edit&redlink=1> "User:Daniel-fpc \(page does not exist\)")
     * Setup the new system - ???
     * Import existing content, userbase and settings (e-mail alerts etc.) - Daniel and ???
     * Change DNS (and possibly link on our pages too if different port is used in the future) - Michael

---

_Source: [https://wiki.freepascal.org/Deadlock_todo](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/Deadlock_todo)_
