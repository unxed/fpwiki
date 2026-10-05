# macOS Energy Efficiency Guide

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** Work in progress

## Contents

  * 1 Overview
  * 2 Quality of Service Levels
  * 3 Event-Based APIs and Services
  * 4 App Nap
  * 5 Minimise I/O
  * 6 Activity Monitor and other utilities
  * 7 See also
  * 8 External links



## Overview

All applications consume energy whenever they update the user interface, perform networking operations, write to disk, or run code on the CPU. As users rely increasingly on battery power energy efficiency becomes integral to the user experience. 

macOS employs advanced energy-saving technologies to help users get the most out of their Macs. These features help the operating system make smart decisions about how to use resources and run code as efficiently as possible. 

The advent of the Apple M1 line of System-On-a-Chip ARM64 processors which come with Performance (high energy) and Efficiency (low energy) cores is a natural progression of managing a computer's efficient use of energy. macOS currently runs the lowest Quality of Service (QoS) processes on the Efficiency cores alone (four in the original M1 chip, two in the M1 Pro and Max chips), so that the Performance cores remain available to run high QoS processes. 

## Quality of Service Levels

Tasks that affect the user, such as downloading and playing music, take priority over background and discretionary work. Quality of Service (QoS) class APIs allow you to assign distinct priority levels to the work your application performs, giving you fine grained control over task prioritization. 

The use of QoS has a substantial effect in reducing energy use, something which is incredibly important for notebook users who are running their laptops on battery rather than mains power. This is even more pronounced with Apple's M1 processors where using a low QoS will result in a process running on the Efficiency cores which may reduce an otherwise heavyweight energy use to as little as 20% of that which would be consumed if running the same process at a high QoS on the Performance cores. 

## Event-Based APIs and Services

Timers deliver events, or fire, at prescribed time intervals. If a timer fires when the system is idle, the CPU and numerous other systems awake from their low-power states. Yet many of these systems aren’t always needed to perform the work invoked by the timer. If the work can be performed when the system hardware is already running, the additional cost is not incurred and the CPU can remain idle longer. macOS provides services and APIs that efficiently deliver events without unnecessarily waking the CPU. 

## App Nap

When your application is not busy performing user-initiated work, the system may put it in App Nap. App Nap conserves energy by regulating your application’s CPU usage, I/O, and timers. As soon as the user resumes interacting with your application, macOS switches it back to full speed. Transitions are so seamless that the user thinks your application has been running at full speed all along. You can enhance App Nap by implementing notifications that tell your application when it becomes inactive, so it can immediately start reducing activity. 

For further details about App Nap, including FPC code, see [macOS App Nap](<macOS_App_Nap.md> "macOS App Nap"). 

## Minimise I/O

I/O-related tasks, such as writing to disk, bring the system out of an idle state. Writing to flash memory is especially energy-draining. By writing data less, aggregating writes together, using caching wisely, scheduling network transactions, and minimizing overall I/O, you can boost the energy efficiency and performance of your application. 

## Activity Monitor and other utilities

The Activity Monitor application lets you quickly identify applications that are using significant amounts of energy in macOS. The Xcode [Instruments application](<Profiling.md> "Profiling"), and numerous command-line tools, help you identify and address energy problems as you develop your application, rather than after those problems are encountered by users. 

The command line `fs_usage` utility provides a running list of system calls and page faults related to activity in the file system. It has options that allow you to specify a timeout, filtering, and more. For details of its operation and options, open an Applications > Utilities > Terminal and type `man`space`fs_usage`Return`` to view the manual page. 

The command line `powermetrics` utility gathers and displays CPU usage statistics (divided into time spent in user mode and supervisor mode), timer and interrupt wakeup frequency (total and, for near-idle workloads, those that resulted in package idle exits), and on supported platforms, interrupt frequencies, package C-state statistics (an indication of the time the core complex + integrated graphics, if any, were in low-power idle states), as well as the average execution frequency for each CPU when not idle. For more details of its operation and options, open an Applications > Utilities > Terminal and and type `man`space`powermetrics`Return`` to view the manual page. 

## See also

  * [macOS App Nap](<macOS_App_Nap.md> "macOS App Nap")



## External links

  * [Apple: Power Efficiency Guidelines](<https://developer.apple.com/library/archive/documentation/Performance/Conceptual/power_efficiency_guidelines_osx/>).


  * [Apple: Tuning Your Code’s Performance for Apple Silicon](<https://developer.apple.com/documentation/apple-silicon/tuning-your-code-s-performance-for-apple-silicon>)


  * [Apple: DispatchQoS](<https://developer.apple.com/documentation/dispatch/dispatchqos>)


  * [Apple: NSOperationQueuePriority](<https://developer.apple.com/documentation/foundation/nsoperationqueuepriority>)


  * [Apple: NSQualityOfService](<https://developer.apple.com/documentation/foundation/nsqualityofservice>)


  * [Apple: SetThreadPriority](<https://developer.apple.com/documentation/foundation/thread/1407523-setthreadpriority>)


  * Related Apple Patents for M1 Performance and Efficiency cores: [US20170357302A1](<https://patents.google.com/patent/US20170357302A1/en>), [US11080095B2](<https://patents.google.com/patent/US11080095B2/en>), [US10884811B2](<https://patents.google.com/patent/US10884811B2/en>).

---

_Source: [https://wiki.freepascal.org/macOS_Energy_Efficiency_Guide](https://web.archive.org/web/20241213022306/https://wiki.freepascal.org/macOS_Energy_Efficiency_Guide)_
