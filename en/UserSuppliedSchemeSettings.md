# UserSuppliedSchemeSettings

## Contents

  * 1 Introduction
    * 1.1 Adding custom schemes
    * 1.2 Related
  * 2 Color schemes
  * 3 Mouse setting schemes



# Introduction

It is possible to change how languages are [highlighted](<Syntax_highlighting.md> "Syntax highlighting"). Within the [IDE](<IDE.md> "IDE"), select Tools > Options (or shift-control-d [shift=option-d on Mac) A dialog box will appear. These options are in Editor > Display > Colors. 

One color scheme defines the color for all languages. However, in the options, you can specify which scheme to use language by language by using the second drop down menu. So you may override the language highlighting for one specific language by choosing a theme just for this language even if you don't use the rest of the color scheme. 

## Adding custom schemes

There are many color schemes available by default. However an additional color scheme can be described by an [XML](<XML.md> "XML") file. Additional schemes can be added by copying them into a folder called "userschemes" in the primary-config-path. The sub folder "userschemes" must be created if missing. The list of schemes is determined on Lazarus startup. 

To know where the folder is, click on the icon to save/export the selected scheme from the IDE (after the scheme drop down menu). This will popup a dialog with the directory of custom themes. The location may change if you have [ multiple installations of Lazarus](<Multiple_Lazarus.md> "Multiple Lazarus"). 

  


## Related

  * [ Color and Highlight options](<IDE_Window__Editor_Options_HighlightColors.md> "IDE Window: Editor Options HighlightColors")
  * [Announcement on developer Blog](<http://lazarus-dev.blogspot.com/2010/06/user-defined-color-schemes.html>)



# Color schemes

This is a selection of user-defined schemes for the Lazarus IDE: 

Name  | Example  | Comments   
---|---|---  
**Default** | [![Default colour scheme](https://wiki.freepascal.org/images/1/1c/Default_ColorScheme.png)](</File:Default_ColorScheme.png> "Default colour scheme") | Example of the default color scheme provided with Lazarus 1.2.x   
[ "Buena Noche"](<images/7/76/buenanoche.md> "buenanoche.xml") | [![Buena Noche colour scheme](https://wiki.freepascal.org/images/2/20/buenanoche_ColorScheme.png)](</File:buenanoche_ColorScheme.png> "Buena Noche colour scheme") | This is the dark theme (black background, white text) that I use. -Seth Grover   
[ "Cyber"](<images/6/6d/cyber.md> "cyber.xml") | [![Cyber colour scheme](https://wiki.freepascal.org/images/5/5b/Cyber_ColorScheme.png)](</File:Cyber_ColorScheme.png> "Cyber colour scheme") | Color scheme in the spirit of late mainframe consoles by Control Data Corporation (by [jwdietrich](</User:Jwdietrich> "User:Jwdietrich"))   
[ "Marina"](<images/5/5c/marina.md> "marina.xml") | [![Marina colour scheme](https://wiki.freepascal.org/images/a/a3/Marina_ColorScheme.png)](</File:Marina_ColorScheme.png> "Marina colour scheme") | by [jwdietrich](</User:Jwdietrich> "User:Jwdietrich")  
["Monokai"](<https://gist.github.com/cpicanco/525bd3d1a40a65e20a745b58d6fe3531>) | [![Monokai colour scheme](https://wiki.freepascal.org/images/1/15/Monokai-ColorScheme.png)](</File:Monokai-ColorScheme.png> "Monokai colour scheme") | Try Monospace, size 9, as font, by [cpicanco](</index.php?title=User:Cpicanco&action=edit&redlink=1> "User:Cpicanco \(page does not exist\)") and ["icetear"](<http://forum.lazarus.freepascal.org/index.php?topic=26981.0>)  
[ "Red Sand"](<images/0/04/redsand.md> "redsand.xml") | [![Red Sand colour scheme](https://wiki.freepascal.org/images/2/25/RedSand_ColorScheme.png)](</File:RedSand_ColorScheme.png> "Red Sand colour scheme") | Color scheme inspired by the "Red Sands" preset in Apple's terminal shell, by [jwdietrich](</User:Jwdietrich> "User:Jwdietrich")  
[ "Nortonic"](<images/a/ad/Nortonic.md> "Nortonic.xml") | [![Nortonic colour scheme](https://wiki.freepascal.org/images/a/a3/Nortonic_ColorScheme.png)](</File:Nortonic_ColorScheme.png> "Nortonic colour scheme") | best used with **FIXEDSYS** or **Consolas** fonts. By Avra   
[ "Zenburn"](<images/0/0f/ColorZenburn.md> "ColorZenburn.xml") | [![Zenburn colour scheme](https://wiki.freepascal.org/images/3/31/ZenBurn_ColorScheme.png)](</File:ZenBurn_ColorScheme.png> "Zenburn colour scheme") | by cjrh, See also ["here"](<http://bugs.freepascal.org/view.php?id=17025>)  
["Theos"](<http://www.theo.ch/lazarus/theos.xml>) | [![Theos colour scheme](https://wiki.freepascal.org/images/1/1d/Theos_ColorScheme.png)](</File:Theos_ColorScheme.png> "Theos colour scheme") | I don't know when I have started using this scheme. I was using it in D3-D6, K1-K3. Most people hate it, but I don't feel at home without it ;-) See also ["show"](<http://www.theo.ch/lazarus/theoscheme.png>)  
["IK Color Scheme"](<https://github.com/ik5/ik-lazarus-color-scheme-/blob/master/ik.xml>) | [![Ik-color-scheme.png](https://wiki.freepascal.org/images/0/0c/Ik-color-scheme.png)](</File:Ik-color-scheme.png>) |   
[ "Solarized"](<images/4/47/Solarized.md> "Solarized.xml") | [![Solarized colour scheme](https://wiki.freepascal.org/images/b/b9/Solarized_ColorScheme.png)](</File:Solarized_ColorScheme.png> "Solarized colour scheme") | Based on ["Solarized" palette](<http://ethanschoonover.com/solarized>) by Ask   
[ "Solarized2 day version"](<images/6/63/Solarized2Day.md> "Solarized2Day.xml") | [![Example screen shot of Solarized2 day version](https://wiki.freepascal.org/images/b/bf/Solarized2Day.png)](</File:Solarized2Day.png> "Example screen shot of Solarized2 day version") | A theme for comfortable coding 
    
    
    Based on ["Solarized"](<http://ethanschoonover.com/solarized>) palette but edited for better look in Lazarus
      
  
[ "Solarized2 night version"](<images/f/f0/Solarized2Night.md> "Solarized2Night.xml") | [![Example screen shot of Solarized2 night version](https://wiki.freepascal.org/images/b/b7/Solarized2Night.png)](</File:Solarized2Night.png> "Example screen shot of Solarized2 night version") | A theme for comfortable coding   
[ "Edited Solarized2 night version"](<images/a/ab/Solarized2NightEdited.md> "Solarized2NightEdited.xml") | [![Example screen shot of edited Solarized2 night version.](https://wiki.freepascal.org/images/f/f3/Solarized2NightEdited.png)](</File:Solarized2NightEdited.png> "Example screen shot of edited Solarized2 night version.") | A theme for comfortable coding   
[ "Antarctica"](<images/5/57/Antarctica.md> "Antarctica.xml") | [![Antarctica colour scheme](https://wiki.freepascal.org/images/0/06/Antarctica-ColourScheme.png)](</File:Antarctica-ColourScheme.png> "Antarctica colour scheme") | this is what I use --[Zoran](</User:Zoran> "User:Zoran")  
[ "Mellow Evening"](<images/0/07/MellowEvening.md> "MellowEvening.xml") | [![Mellow Evening color scheme](https://wiki.freepascal.org/images/7/7d/MellowEvening-ColorScheme.png)](</File:MellowEvening-ColorScheme.png> "Mellow Evening color scheme") | I created this theme to be easy on the eyes for many hours of continuous coding. --ddsol   
[ "creaothceann"](<images/4/45/creaothceann.md> "creaothceann.xml") | [![creaothceann's color scheme](https://wiki.freepascal.org/images/b/bf/color_scheme.png)](</File:color_scheme.png> "creaothceann's color scheme") | Classic Turbo Pascal colors, but with some modifications.  
font: Fixedsys Excelsior  
tab size: 8 characters, not replaced by spaces, "cursor skips tabs" enabled   
[ "FOREST"](<images/a/a6/FOREST.md> "FOREST.xml") | [![FOREST](https://wiki.freepascal.org/images/9/9b/FOREST.png)](</File:FOREST.png> "FOREST") | Into The Green :-)   
[ "INTO THE BLUE"](<images/7/73/INTO_THE_BLUE.md> "INTO THE BLUE.xml") | [![INTO THE BLUE](https://wiki.freepascal.org/images/e/ea/INTO_THE_BLUE.png)](</File:INTO_THE_BLUE.png> "INTO THE BLUE") | :-)   
[ "SNOW WALKER"](<images/5/5b/SNOW.md> "SNOW.xml") | [![SNOW WALKER](https://wiki.freepascal.org/images/3/3b/MEADOW.png)](</File:MEADOW.png> "SNOW WALKER") | If a white background is really necessary !!!   
[ "NEBULA"](<images/1/1e/NEB.md> "NEB.xml") | [![NEBULA](https://wiki.freepascal.org/images/2/2a/NEBULA.png)](</File:NEBULA.png> "NEBULA") | N I C E ! ! ! My favorite :-)   
[ "Breeze Dark"](<images/4/47/Breeze_Dark.md> "Breeze Dark.xml") | [![Breeze Dark](https://wiki.freepascal.org/images/0/08/Lazarus-Breeze-Dark-Screenshot.png)](</File:Lazarus-Breeze-Dark-Screenshot.png> "Breeze Dark") | This will fit nicely into the Kubuntu dark standard theme which happens to go by the same name ;-)   
[ "Deep Black"](<images/1/13/deep_black.md> "deep black.xml") | [![Deep Black](https://wiki.freepascal.org/images/8/89/deep_black.png)](</File:deep_black.png> "Deep Black") | Deep Black by Mariusz Kasperkiewicz where [ported from Notepad++](<http://gintasdx.blogspot.com/2011/02/lazarus-ide-custom-themes.html>) by GintasDX.   
[ "Hello Kitty"](<images/1/11/hello_kitty.md> "hello kitty.xml") | [![Hello Kitty](https://wiki.freepascal.org/images/9/9b/hello_kitty.png)](</File:hello_kitty.png> "Hello Kitty") | Hello Kitty theme where [ported from Notepad++](<http://gintasdx.blogspot.com/2011/02/lazarus-ide-custom-themes.html>) by GintasDX.   
[ "Obsidian"](<images/4/45/obsidian.md> "obsidian.xml") | [![Obsidian](https://wiki.freepascal.org/images/4/4c/obsidian.png)](</File:obsidian.png> "Obsidian") | Obsidian theme by Joni Eskelinen where [ported from Notepad++](<http://gintasdx.blogspot.com/2011/02/lazarus-ide-custom-themes.html>) by GintasDX.   
[ "GitHub like"](<images/f/fd/GitHub_theme.md> "GitHub theme.xml") | [![Obsidian](https://wiki.freepascal.org/images/f/fe/GitHub_theme_preview.png)](</File:GitHub_theme_preview.png> "Obsidian") | Dark theme inspired from GitHub syntax coloring.   
[ "Eclipse"](<images/3/31/Eclipse.md> "Eclipse.xml") | [![Eclipse colour scheme](https://wiki.freepascal.org/images/8/8f/EclipseEditorColors.png)](</File:EclipseEditorColors.png> "Eclipse colour scheme") | A dark, high contrast scheme (by 440bx)   
[ "VS Code Light"](<images/8/86/ColorVSCLight.md> "ColorVSCLight.xml") | [![VS Code Light](https://wiki.freepascal.org/images/8/8f/ColorVSCLight_scheme_screenshot.png)](</File:ColorVSCLight_scheme_screenshot.png> "VS Code Light") | Theme inspired by VS Code Light, made by [regs01](<https://gitlab.com/regs01>) ([original merge request](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/merge_requests/246>)).   
[ "VS Code Dark"](<images/4/43/vsclaz_theme.md> "vsclaz theme.xml") | [![VS Code Dark](https://wiki.freepascal.org/images/3/3c/vscode_dark.png)](</File:vscode_dark.png> "VS Code Dark") | Theme inspired by VS Code Dark, made by [assisdantas](<https://github.com/assisdantas>) ([Github repo](<https://github.com/assisdantas/vsclaz_theme>)).   
[ "Kurwish Dark"](<images/9/96/Kurwish_Dark.md> "Kurwish Dark.xml") | [![Kurwish Dark](https://wiki.freepascal.org/images/d/de/KurwishDark.png)](</File:KurwishDark.png> "Kurwish Dark") | A dark color scheme, clean, neat and featuring subdued colors. ([Github repo](<https://github.com/rlukasiak/Kurwish_Dark-Lazarus_color_scheme>)).   
  
Delphi themes can be exported to Lazarus by using the [Delphi IDE Theme Editor](<http://theroadtodelphi.wordpress.com/delphi-ide-theme-editor/>). 

# Mouse setting schemes

Some space to link or upload all the user defined schemes

---

_Source: [https://wiki.freepascal.org/UserSuppliedSchemeSettings](https://web.archive.org/web/20250219122143/https://wiki.freepascal.org/UserSuppliedSchemeSettings)_
