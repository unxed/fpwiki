# macOS Gatekeeper Debugging Tips

│ **English (en)** │

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

  
****

[Notarising](<Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+") your application is _necessary_ to pass Gatekeeper, but it is _not sufficient_. Gatekeeper has its own array of checks. 

To determine why Gatekeeper is not executing your application is quite tricky. In most cases where Gatekeeper denies execution, there is some evidence in the system log. It is not always easy to spot. Some key terms to look for are: 

  * gk, for Gatekeeper.
  * xprotect, an internal name for a Gatekeeper subsystem.
  * syspolicyd, see its [man page](<https://developer.apple.com/documentation/os/reading_unix_manual_pages>).
  * cmd, for Mach-O load command oddities.



Gatekeeper caches its assessments, and if you ‘hit’ that cache then you may not see anything interesting in the log (because the code that logs the interesting entries is not run). Try testing in a VM: 

  * Start with a fresh VM that’s never seen your application.
  * Set up the VM exactly how you need it set up (install the application, prime sudo, and so on).
  * Take a snapshot.
  * Run the program.
  * Collect the logs.



If you need to run it again, restore from the snapshot first. 

With regards to collecting logs, you can use the _sysdiagnose_ command line utility (see its man page) for that but it is generally faster to run _log collect_ (see the man page for log) and then export the log to analyse offline on your real Mac. 

In many cases, critical log entries have private info redacted. You can disable that system-wide using a configuration profile. See the discussion of Enable-Private-Data property in [SystemLogging](<https://developer.apple.com/documentation/devicemanagement/systemlogging>). IMPORTANT This is on the VM. You really don’t want to do it on any Mac you care about.

---

_Source: [https://wiki.freepascal.org/macOS_Gatekeeper_Debugging_Tips](https://web.archive.org/web/20240907100548/https://wiki.freepascal.org/macOS_Gatekeeper_Debugging_Tips)_
