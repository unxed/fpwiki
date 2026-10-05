# Projects using Lazarus - Medical and Scientific software

**[Projects using  
Free Pascal](<Projects_using_Free_Pascal.md> "Projects using Free Pascal")** [Business Software](<Projects_using_Lazarus_-_Business_Software.md> "Projects using Lazarus - Business Software")  
---  
[Communications software](<Projects_using_Lazarus_-_Communications_software.md> "Projects using Lazarus - Communications software")  
[Components and Libraries](<Projects_using_Lazarus_-_Components_and_Libraries.md> "Projects using Lazarus - Components and Libraries")  
[Databases and Tools](<Projects_using_Lazarus_-_Databases_and_Tools.md> "Projects using Lazarus - Databases and Tools")  
[Developer utilities](<Projects_using_Lazarus_-_Developer_utilities.md> "Projects using Lazarus - Developer utilities")  
[Editors and IDEs](<Projects_using_Lazarus_-_Editors_and_IDEs.md> "Projects using Lazarus - Editors and IDEs")  
[Educational software](<Projects_using_Lazarus_-_Educational_software.md> "Projects using Lazarus - Educational software")  
[Games](<Projects_using_Lazarus_-_Games.md> "Projects using Lazarus - Games")  
[Hobby software](<Projects_using_Lazarus_-_Hobby_software.md> "Projects using Lazarus - Hobby software")  
Medical and Scientific software  
[Multimedia](<Projects_using_Lazarus_-_Multimedia.md> "Projects using Lazarus - Multimedia")  
[User utilities](<Projects_using_Lazarus_-_User_utilities.md> "Projects using Lazarus - User utilities")  
[Web](<Projects_using_Lazarus_-_Web.md> "Projects using Lazarus - Web")  
  
## Contents

  * 1 CapaCalc
  * 2 CheckMol/Matchmol
  * 3 Corona
  * 4 CyberUnits
  * 5 DataLogger
  * 6 DMV
  * 7 eLogSim
  * 8 ePnR
  * 9 ezANOVA
  * 10 Fertility Calendar
  * 11 Foobot Monitor
  * 12 FREE!ship Plus in Lazarus
  * 13 Grid InQuest II Coordinate Transformer
  * 14 GroundCAD
  * 15 Harmonux
  * 16 i6z Toolkit
  * 17 LazBacterias
  * 18 LazStats
  * 19 MRIcron
  * 20 MRIcroGL
  * 21 Nest-o-Patch
  * 22 OctaveGUI
  * 23 OpenSIMPLY
  * 24 ProofTools
  * 25 SimSEE
  * 26 SimThyr
  * 27 SimulaBeta
  * 28 SmallMap
  * 29 SPINA
  * 30 SysLinea
  * 31 Traverse Pro
  * 32 Unified Life Models
  * 33 UnitConv
  * 34 Woodland Potential Calculator
  * 35 Xoctave



## CapaCalc

The little application [CapaCalc](<https://github.com/wp-xyz/CapaCalc>) calculates the electrical capacitance of a variety of conductor arrangements: 

  * Parallel plate capacitor
  * Spherical capacitor
  * Cylindrical capacitor
  * Capacitance between rectangular conductor and conducting plane
  * Capacitance between cylindrical conductor and conducting plane
  * Capacitance between two cylinders



Moreover, the program calculates the 

  * effective capacitance of capacitors in series and in parallel, as well as the
  * capacitance of a semiconductor p/n junction.



[![capacalc screenshot.png](https://wiki.freepascal.org/images/5/54/capacalc_screenshot.png)](</File:capacalc_screenshot.png>)

## CheckMol/Matchmol

[Checkmol](<http://merian.pch.univie.ac.at/~nhaider/cheminf/cmmm.html>) is a command-line utility program that reads molecular structure files in different formats and analyzes the input molecule for the presence of various functional groups and structural elements. At present, approximately 200 different functional groups are recognized. GPL licensed. 

[Matchmol](<http://merian.pch.univie.ac.at/~nhaider/cheminf/cmmm.html>) complements the capabilities of checkmol. It compares two (or more) molecular structures and determines whether one of them is a substructure of the other one. GPL licensed. 

## Corona

[Corona](<https://github.com/wp-xyz/corona>) downloads daily accumulated case counts of the corona virus disease from the 2019 Novel Coronavirus COVID-19 (2019-nCoV) data repository by Johns Hopkins University CSSE, and from Robert-Koch-Institut, Germany. 

  * Displays the world-wide infection data in a color-coded map.
  * Scrolls through previous dates using the scrollbar below the map.
  * Plots the data as a function of time: confirmed, death, recovered, sick counts. Cumulative cases, new cases per day.
  * Estimates characteristic pandemic numbers: doubling time, reproduction number. (Disclaimer: due to various calculation methods, these numbers may differ from official data).



[![corona-1.2.png](https://wiki.freepascal.org/images/5/5c/corona-1.2.png)](</File:corona-1.2.png>)

## CyberUnits

[CyberUnits](<CyberUnits.md> "CyberUnits") is a collection of cross-platform units for medical cybernetics and systems biology. It supports [Object Pascal](<Object_Pascal.md> "Object Pascal"), S and Matlab. 

## DataLogger

[DataLogger](<https://github.com/wp-xyz/datalogger>) is a small application which reads measured values from a digital multimeter and plots them as a function of time. 

  * Interfaces to some digital multimeters with digital output via serial port. Use a serial/usb converter if your computer does not have a serial input any more.
  * Supported models: Conrad VC630, VC820, VC830, VC840, VC850, or "user-defined" (based on the DMM chips FS9721_LP3 (seven-segment output) and FS9922-DMM4 (digit output) by Fortune Semiconductor) Measurement interval: a few measurements per second, or slower. Interval can be varied during the measurement.
  * Transformations: User-provided equation to convert the measured quantity to some other quantity. The application initially was written to convert the analog voltage output of a pressure meter to pressure in mBar.
  * Linear or logarithmic display of the measured quantity.
  * Add comments during the measurement to mark special events. The comments are displayed along with the measured data.
  * Save measured curve as xml, csv or spreadsheet files. Re-load and overlay previously measured curves.



[![datalogger-main.png](https://wiki.freepascal.org/images/f/f7/datalogger-main.png)](</File:datalogger-main.png>)

## DMV

[DMV](<https://osf.io/4en3b/files/>) The Diffusion Model allows for modelling response times. The Diffusion Model Visualizer explores the effect of the seven model parameters (a, z, v, t0, sz, sv, and st0) upon the response time density curves and positive/negative response probabilities. It supports interactive parameter change with immediate update of the diagram. The diagram can be stored to disk for further use (e.g., in an article or educational context). The program is freeware. 

[![dmv1 1.png](https://wiki.freepascal.org/images/2/20/dmv1_1.png)](</File:dmv1_1.png>)

For details regarding technical details and usage see: 

The diffusion model visualizer: an interactive tool to understand the diffusion model parameters. Psychological Research 84(4), 1157–1165. <https://doi.org/10.1007/s00426-018-1112-6>

Update: A new version DMV 1.1 now also allows for modifying the within variance component s^2 with a slider along with further minor modifications and bug fixes. 

## eLogSim

[eLogSim](<https://elogsim.sourceforge.io>) is a digital circuit simulator with statistical fault simulation capability. 

[![Dual74161.png](https://wiki.freepascal.org/images/b/bf/Dual74161.png)](</File:Dual74161.png>)

## ePnR

[ePnR](<https://sourceforge.net/projects/epnr/>) is a simple Integrated Circuit (IC) block standard cell placement & routing tool. ePnR currently supports only circuit blocks using equal height standard cells arranged in one or more channels of user configurable length. Standard cells are described in a simple text based library (compliant with eLogSim). Placement follows initially the cell call order in the SPICE like circuit input netlist. However, a placement optimization, aiming at minimum weighted accumulated wire length, by simulated annealing is available. Routing consists of channel routing as first step. If un-routed connections are left, Maze routing can (optionally) be applied. ePnR does not guarantee completely finished routing. However, un-routed connections will be left with rubberband connections and marked start and end points for subsequent manual routing using a third party layout editor. ePnR outputs in CIF 2.0 format readable by e.g. the free KLayout editor. Read more on [ePnR](<https://sourceforge.net/projects/epnr/>). 

[![64bitCarryAdder preplaced.png](https://wiki.freepascal.org/images/5/53/64bitCarryAdder_preplaced.png)](</File:64bitCarryAdder_preplaced.png>)[![64bitCarryAdder fullyrouted.png](https://wiki.freepascal.org/images/5/5b/64bitCarryAdder_fullyrouted.png)](</File:64bitCarryAdder_fullyrouted.png>)

## ezANOVA

[ezANOVA](<http://people.cas.sc.edu/rorden/ezanova/index.html>) is a simple tool to calculate Analysis of Variance as well as pairwise comparisons. It is [open source](<https://github.com/neurolabusc/ezANOVA>). 

[![ezANOVAx.png](https://wiki.freepascal.org/images/6/6b/ezANOVAx.png)](</File:ezANOVAx.png>)

## Fertility Calendar

[Fertility calendar](<http://www.cvrk.com/mk/>) with current year expectance, boy or girl presumption. Versions for Windows and MacOS. 

[![fercal.png](https://wiki.freepascal.org/images/1/1d/fercal.png)](</File:fercal.png>)

## Foobot Monitor

  * Foobot(<http://foobot.io/>) is an InternetOfThings gadget that monitors indoor air quality
  * Unfortunately, the company currently only supplies mobile phone apps to access the Foobot
  * They do however publish an API which returns JSON data when queried, so I decided to port it to the PC using Lazarus and FPC
  * Wiki Page: [Foobot](<Foobot.md> "Foobot")



What you will need: A Foobot, a Foobot account, a free API Key (get one from the API page: <https://api.foobot.io/apidoc/index.html>) 

[![foobotmonitorscreenshot.jpg](https://wiki.freepascal.org/images/7/7e/foobotmonitorscreenshot.jpg)](</File:foobotmonitorscreenshot.jpg>) [![foobotmonitorscreenshot1.jpg](https://wiki.freepascal.org/images/2/29/foobotmonitorscreenshot1.jpg)](</File:foobotmonitorscreenshot1.jpg>)

## FREE!ship Plus in Lazarus

[FREE!ship Plus in Lazarus](<https://github.com/markmal/freeship-plus-in-lazarus>) is further development of the _FREE!ship Plus_ Windows program based on the free source code FREE!ship v3.x under GNU GPL license. This _FREE!ship Plus_ application is migrated into free open source Lazarus / Free Pascal environment to promote further development in various platforms and for various platforms (OS and architectures). 

_FREE!ship Plus_ is designed for the full parametric analysis of resistance and power prediction for a ship and other calculations of hydrodynamics of vessels and underwater vehicles. _FREE!ship Plus_ allows the designer to simulate and analyze condition of balance of a complex completely hull - rudders - keels - engine - propellers in different regimes and of service conditions of a ship. The analyzable system includes a hull, appendages, a propeller and the engine (i.e. resistance, power, a thrust and a torque), and also various service conditions (heaving, a wind, a shallow-water effect, a regime of tow / pushing, etc...). 

[![FreeShip+qt.png](https://wiki.freepascal.org/images/5/56/FreeShip%2Bqt.png)](</File:FreeShip%2Bqt.png>)

## Grid InQuest II Coordinate Transformer

The [Grid InQuest II](<https://bitbucket.org/PaulFMichell/gridinquestii/downloads>) desktop application and command line tools provide a means of transforming coordinates between global geodetic coordinates (ETRS89/WGS84) and the national systems of Great Britain and Ireland. It provides a fully three-dimensional transformation incorporating the latest geoid model (OSGM15) and the appropriate polynomial transformation model (OSTN15 or OSi/OSNI) for each of the projected coordinate systems. This was developed in Free Pascal and Lazarus by [Michell Computing](<http://michellcomputing.co.uk/>). It was commissioned jointly by the Land and Property Service of Northern Ireland, the Ordnance Survey of the Republic of Ireland and the Ordnance Survey of Great Britain. 

[![GIQ2.png](https://wiki.freepascal.org/images/1/1b/GIQ2.png)](</File:GIQ2.png>)

## GroundCAD

[GroundCAD](<https://sourceforge.net/projects/groundcad03/>) is new 2D CAD software for land surveying. working under windows and linux. 

[![groundcad.jpg](https://wiki.freepascal.org/images/4/48/groundcad.jpg)](</File:groundcad.jpg>) [![layermanager.jpg](https://wiki.freepascal.org/images/1/13/layermanager.jpg)](</File:layermanager.jpg>)

## Harmonux

Harmonux 0.1.4 Harmonic Analysis. Enter a table and get the harmonic function for the table. With the graphic of the points of the table and the function. Open Source GNU/GPL, pre-compiled for Linux and Windows. 

[![hamonux14.png](https://wiki.freepascal.org/images/d/db/hamonux14.png)](</File:hamonux14.png>)

## i6z Toolkit

The [i6z Toolkit](<https://github.com/pdudis/i6zToolkit>) is a Graphical User Interface (GUI) application for handling IUCLID 6 archives (i6z). The IUCLID (International Uniform ChemicaL Information Database) software application is developed by the European Chemicals Agency (ECHA) in association with the Organisation for Economic Co-operation and Development (OECD). The **i6z Toolkit** allows users to manipulate such archives, mainly in the areas of previewing and file management/organisation. Conveniently, this is done without the need to run the IUCLID 6 application. 

[![i6zToolkit-Main Window.png](https://wiki.freepascal.org/images/0/06/i6zToolkit-Main_Window.png)](</File:i6zToolkit-Main_Window.png>). [![i6zToolkit-XML Tree View.png](https://wiki.freepascal.org/images/0/07/i6zToolkit-XML_Tree_View.png)](</File:i6zToolkit-XML_Tree_View.png>)

The **i6z Toolkit** is available for MS Windows, macOS, and Linux. 

## LazBacterias

[LazBacterias](<http://code.google.com/archive/p/lazbacterias/>) is a program to simulate the growth of bacterial cultures using the rules of Conway's Game of Life. 

## LazStats

[LazStats](<https://openstat.info/LazStatsMain.htm>) is a free statistical analysis program written in Lazarus by Bill Miller. It contains a large variety of parametric, nonparametric, multivariate, measurement, statistical process control, financial and other procedures. One can also simulate a variety of data for tests, theoretical distributions, multivariate data, etc. 

The most recent sources are available at [the Lazarus Code and Component Repository](<https://sourceforge.net/p/lazarus-ccr/svn/HEAD/tree/applications/lazstats/>). 

[![LazStats ccr.png](https://wiki.freepascal.org/images/c/c0/LazStats_ccr.png)](</File:LazStats_ccr.png>)

## MRIcron

[MRIcron](<https://www.nitrc.org/projects/mricron/>) is a NIH-funded opensource project that allows users to visualize and volume render medical images (MRI, CT, PET). It includes tools for lesion mapping, non parametric statistical analysis ([npm](<http://www.mricro.com/npm/>)), and conversion from the medical DICOM format to the scientific NIfTI format ([dcm2nii](<http://www.mricro.com/mricron/dcm2nii.html>)). It is available for Windows, Linux and macOS. 

[![mricron.jpg](https://wiki.freepascal.org/images/8/8b/mricron.jpg)](</File:mricron.jpg>)

## MRIcroGL

[MRIcroGL](<http://www.mccauslandcenter.sc.edu/mricrogl/>) is an opensource project that uses the graphics card (using OpenGL) to visualize and volume render medical images. It is hosted on the [National Institutes of Health (NIH) Neuroimaging Informatics Tools and Resources Clearinghouse (NITRC)](<http://www.nitrc.org/projects/mricrogl/>). It can view images saved in NIfTI (.nii, .nii.gz, .hdr/.img), Bio-Rad Pic (.pic), NRRD, Philips (.par/.rec), ITK MetaImage (.mhd, .mha), AFNI (.head/.brik), Freesurfer (.mgh, .mgz), and many DICOM (extensions vary) formats. It is available for Windows, Linux and macOS. 

[![mricrogl overlay.png](https://wiki.freepascal.org/images/0/0a/mricrogl_overlay.png)](</File:mricrogl_overlay.png>) [![mricrogl visiblehuman.jpg](https://wiki.freepascal.org/images/b/b5/mricrogl_visiblehuman.jpg)](</File:mricrogl_visiblehuman.jpg>)

## Nest-o-Patch

[Nest-o-Patch](<https://sourceforge.net/projects/nestopatch/?source=navbar>), software for the analysis of patch-clamp, two-electrode-voltage clamp and other electrophysiological data. Directly works with files created by HEKA Pulse or Patch-Master data aquisition software or with CSV and text files. Designed mostly for the analysis of single channel recordings, was nevertheless successfully used for whole-cell data analysis. Several academic papers were published with the use of this software. 

[![nest-o-patch trace.png](https://wiki.freepascal.org/images/8/82/nest-o-patch_trace.png)](</File:nest-o-patch_trace.png>) [![nest-o-patch levels analysis.png](https://wiki.freepascal.org/images/6/6d/nest-o-patch_levels_analysis.png)](</File:nest-o-patch_levels_analysis.png>)

## OctaveGUI

[OctaveGUI](<http://code.google.com/p/octave-gui/>) is a(nother) GUI frontend for GNU Octave written fully in Free Pascal with Lazarus IDE, with the following feature goals: Cross platform (CPU, OS, and widgetset) - Portable - Small size - Fast execution & low memory consumption - Close interface to MATLAB. 

## OpenSIMPLY

[Project homepage](<http://opensimply.org>) OpenSIMPLY is an open source free simulation software based on discrete event simulation approach. The concept is suitable for a person of a different programming and simulation experience. Both blocks simulation and Simula-like simulation styles are available. Simula-models with some adaptation can be used as well. The project is supplied with detailed documentation, pop-up help and tutorial with executable examples. OpenSIMPLY can be used as a traffic simulation software, network simulation software and much more. [![OpenSIMPLY tutorial demonstration example](https://wiki.freepascal.org/images/5/5f/tutorial_demo_animation_s.gif)](</File:tutorial_demo_animation_s.gif> "OpenSIMPLY tutorial demonstration example")

## ProofTools

[ProofTools](<http://creativeandcritical.net/prooftools/>) automatically and graphically generates semantic tableaux, also known as proof trees, semantic trees and analytic tableaux, generally used to test whether a formula is a logical truth, or whether a proof/argument is deductively valid. ProofTools can generate proof trees for propositional, predicate and (normal) modal logic. It is available for Windows, Linux and macOS. 

[![ProofTools screenshot](https://wiki.freepascal.org/images/0/0a/ProofTools.png)](</File:ProofTools.png> "ProofTools screenshot")

## SimSEE

[SimSEE](<https://simsee.org/simsee/simsee/>) is a platform for Simulation of Systems of Electrical Energy. Using SimSEE we can simulate the optimal operation of systems with hydroelectrical plants, hydro-reservoirs, fuel fired plants, wind farms and interconnections with other countries. The platform has a very sophisticated tool for modelling stochastic processes like river inflows, fuel prices, wind speed, etc. The software was developed in Spanish but we are working to support other languages (help is welcome). 

## SimThyr

[SimThyr](<http://simthyr.sourceforge.net/>) is a simulation program for the pituitary thyroid feedback control that is based on a parametrically isomorphic model of the overall system. It aims in a better insight into the dynamics of thyrotropic feedback. Applications of this program cover research, including development of hypotheses, and education of students in biology and medicine, nurses and patients. 

[![SimThyr for macOS](https://wiki.freepascal.org/images/7/75/Simthyr_mac_os_x_leopard.png)](</File:Simthyr_mac_os_x_leopard.png> "SimThyr for macOS") [![SimThyr for Linux](https://wiki.freepascal.org/images/a/a6/pr.simthyr_suse_linux_11.jpg)](</File:pr.simthyr_suse_linux_11.jpg> "SimThyr for Linux") [![SimThyr for Windows](https://wiki.freepascal.org/images/c/ca/SimThyr_Win7.png)](</File:SimThyr_Win7.png> "SimThyr for Windows")

## SimulaBeta

[SimulaBeta](<https://sourceforge.net/projects/simulabeta/>) is a numeric simulation program for insulin-glucose homeostasis. It uses the [CyberUnits](<CyberUnits.md> "CyberUnits") framework for simulating the biological information processing structure. SimulaBeta can be used to predict the evolution of insulin and glucose concentrations in steady-state fasting conditions, to study the effects of changes in structure parameters of the feedback loop (e.g. in certain diabetes mellitus types or other diseases) and to simulate the response of the processing structure to special function tests (e.g. intravenous and oral glucose tolerance testing). 

[![SimulaBeta for macOS](https://wiki.freepascal.org/images/a/a1/SimulaBeta_on_macOS_Ventura.jpeg)](</File:SimulaBeta_on_macOS_Ventura.jpeg> "SimulaBeta for macOS") [![SimulaBeta 3.1.2 with LOREMOS on macOS Ventura](https://wiki.freepascal.org/images/1/18/SimulaBeta_3.1.2_on_macOS_Ventura.jpg)](</File:SimulaBeta_3.1.2_on_macOS_Ventura.jpg> "SimulaBeta 3.1.2 with LOREMOS on macOS Ventura")

## SmallMap

[SmallMap](<http://akarwowski.pl/index.php?page=smallmap&lang=en>) is a tool for creating vector maps using raster data as well as for processing spatial information. The application is based on Firebird database. It allows you to collect and process raster images and vector layers. Currently, the program works with spatial data in WGS84 system and WebMercator projection. It operates in two modes: preview and edition of objects. The default unit set in the program is one meter. 

[![FPW SmallMap 02 1.png](https://wiki.freepascal.org/images/f/f0/FPW_SmallMap_02_1.png)](</File:FPW_SmallMap_02_1.png>) [![FPW SmallMap 03 2.png](https://wiki.freepascal.org/images/3/33/FPW_SmallMap_03_2.png)](</File:FPW_SmallMap_03_2.png>)

## SPINA

[SPINA](<http://spina.medical-cybernetics.de/en/>) is software for determining constant structure-parameters of endocrine feedback control systems from hormone levels obtained in vivo. The first version of this cybernetic approach allows for evaluating the functional status of the thyroid gland. 

[![SPINA Thyr for macOS](https://wiki.freepascal.org/images/2/2b/SPINA_Thyr_3.3_for_Mac_2.png)](</File:SPINA_Thyr_3.3_for_Mac_2.png> "SPINA Thyr for macOS")

## SysLinea

SysLinea 0.1.2 Solves Linear Systems and calculates Linear and Non linear Regression. It gives the Pearson and Spearman coefficients of correlation and the t-test. Open Source GNU/GPL, pre-compiled for Linux and Windows. 

[![SysLinea 1.2 - Linear regression and non linear regression](https://wiki.freepascal.org/images/d/d8/syslinea12.png)](</File:syslinea12.png> "SysLinea 1.2 - Linear regression and non linear regression")

## Traverse Pro

Traversing is the type of survey in which a number of connected survey lines form the framework and the directions and lengths of the survey lines are measured with the help of an angle measuring instrument respectively. Traverse Pro is a freeware for calculation of single loop traverse. Traverse Pro desktop application especially designed for Civil / Surveyor. [Download](<https://www.priabroy.name/archives/sdm_downloads/traverse-pro-v2-63-build-6230-windows-64-bit>). 

[![Traverse Pro.png](https://wiki.freepascal.org/images/6/61/Traverse_Pro.png)](</File:Traverse_Pro.png>) [![Traverse Pro2.png](https://wiki.freepascal.org/images/c/c5/Traverse_Pro2.png)](</File:Traverse_Pro2.png>)

## Unified Life Models

[ULM (Unified Life Models)](<http://www.biologie.ens.fr/~legendre/ulm/ulm.html>) is an open-source software enabling the simulation and analysis of deterministic and stochastic discrete time dynamical systems for population dynamics modeling. It works natively on Windows, Linux and macOS. 

Models are described using a simple declaration language, close to the mathematical formulation. The system can be studied interactively by means of simple commands, producing convenient graphics and numerical results. 

[![screenshot ulm.png](https://wiki.freepascal.org/images/c/c4/screenshot_ulm.png)](</File:screenshot_ulm.png>)

## UnitConv

[UnitConv](<https://github.com/wp-xyz/UnitConv>) converts units of measurement data: Length, area, volume, mass, time, speed, acceleration, force, flow rate, pressure, temperature, energy, power, magnetic field, ozone concentration, angle, data volume. 

[![UnitConv.png](https://wiki.freepascal.org/images/b/ba/UnitConv.png)](</File:UnitConv.png>)

## Woodland Potential Calculator

The Forestry Commission and Natural England commissioned a bespoke data collection and presentation tool for calculating the potential for increasing the extent of tree cover across England. It is written entirely in FreePascal using the Lazarus IDE, the LCL and the Graphics32 library. WoodlandCalc is released under the LGPL v2 open source licence and freely available for download from SourceForge. [More info...](<http://www.michellcomputing.co.uk/woodlandcalc.html>). Application [Download](<http://sourceforge.net/projects/woodlandcalc/files/latest/download>). 

[![woodlandcalcoverview.jpg](https://wiki.freepascal.org/images/f/fa/woodlandcalcoverview.jpg)](</File:woodlandcalcoverview.jpg>)

## Xoctave

[Xoctave](<http://www.xoctave.com/>) is a Human interface to GNU Octave. Xoctave encapsulates GNU Octave uses pipes and provides extra useful tools to make GNU Octave more easier to use. XOctave is written in Pascal using Lazarus front-end and Free Pascal (aka FPK Pascal) libraries. It uses synedit for syntax highlighting, and uses the Lazarus Component Library (LCL) is a set of visual and non-visual component classes over a Widget toolkit-dependent layer with multi-language support (English-Turkish) 

[![xoctave.png](https://wiki.freepascal.org/images/6/63/xoctave.png)](</File:xoctave.png>)

---

_Source: [https://wiki.freepascal.org/Projects_using_Lazarus_-_Medical_and_Scientific_software](https://web.archive.org/web/20241029184528/https://wiki.freepascal.org/Projects_using_Lazarus_-_Medical_and_Scientific_software)_
