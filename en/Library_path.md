# Library path

Q: Where do I add the path for source of 3rd party components? In Delphi it was in the "Library path". 

Answer 1, from forum member **wp** : 

Lazarus is very different from Delphi regarding the path to 3rd party units. 

3rd party units come to you in [packages](<packages.md> "packages") (extension .lpk). In order to use a 3rd party unit in a project you add the corresponding package to the requirements of the project: Open "Project" > "[Project inspector](<IDE_Window__Project_Inspector.md> "IDE Window: Project Inspector")". Click "Add" > "New requirement" and select the package from the list > "OK". Now the package appears in the lower part of the list in the project inspector ("Required packages"), and the IDE knows where to find the units contained in the package so that you can add any unit contained in the package to the `uses` clause. 

If the 3rd-party component is "installed" (normally: if there is an icon in the [Component Palette](<Component_Palette.md> "Component Palette")) simply drop the component on the form and the project is automatically extended by the related package (and the unit is added to the `uses` clause of the form). 

Unless you really know what you are doing you should never extend the unit path ("Other unit files" in the project inspector) by the path to 3rd party components - it is easy to create lots of compilation issues if you do this. 

Answer 2, from forum member **trev** : 

"3rd party units come to you in packages." - Not necessarily: some do, many do not especially if they're not Lazarus-specific or not visual components. Installation can be as simple as copying over all files to your application directory and adding the relevant units to your uses clause. Alternatively, Lazarus provides "Project > Project Options > Compiler Options > Paths - Other unit files".

---

_Source: [https://wiki.freepascal.org/Library_path](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/Library_path)_
