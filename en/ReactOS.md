# ReactOS

│ **English (en)** │  **[español (es)](</ReactOS/es> "ReactOS/es")** │  **[polski (pl)](</ReactOS/pl> "ReactOS/pl")** │  **[русский (ru)](<../ru/ReactOS.md> "ReactOS/ru")** │ 

## Contents

  * 1 Overview
  * 2 ReactOS installation
  * 3 FPC installation
  * 4 Lazarus installation
  * 5 Bugs
  * 6 See also



## Overview

[ReactOS](<http://www.reactos.org>) is a free, open source clone of Windows aiming for compatibility with Windows XP/2003, along with some extensions from later versions. ReactOS has been in development for a long time and has not yet reached beta stage, but Lazarus can run on it. 

[![ReactOS_v03.15](https://wiki.freepascal.org/images/e/eb/ReactOS.png)](</File:ReactOS.png> "ReactOS_v03.15")

## ReactOS installation

If you want to experiment with ReactOS, it's a good idea to use a Virtual Machine (e.g. VirtualBox) with a snapshot boot CD as ReactOS development is quick and a lot of bugs get fixes between binary releases. It is easy to upgrade an existing VM install by running an upgrade from a newer boot CD. 

## FPC installation

Because Lazarus can be installed without problems, presumably the same applies for installing FPC. [fpcup](<fpcup.md> "fpcup") is periodically tried on ReactOS as well (it requires installing an SVN client first); without success at least up to ReactOS r63093. 

## Lazarus installation

ReactOS aims to emulate Windows; it is possible to just run the Lazarus installer. However, ReactOS includes a software installation program (RAPPS) which allows you to download the normal Lazarus installer and install it with a few mouse clicks. 

## Bugs

ReactOS bugs are tracked in their bugtracker, e.g. this bug about flipped icons when running Lazarus: [[1]](<https://jira.reactos.org/browse/CORE-6320>)

## See also

  * [Small Virtual Machines](<Small_Virtual_Machines.md> "Small Virtual Machines")

---

_Source: [https://wiki.freepascal.org/ReactOS](https://web.archive.org/web/20241118073236/https://wiki.freepascal.org/ReactOS)_
