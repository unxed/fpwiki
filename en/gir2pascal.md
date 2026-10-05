# gir2pascal

## Contents

  * 1 About
  * 2 License
  * 3 Author
  * 4 Source code



# About

gir2pascal is a program to convert gobject-introspection (*.gir) xml files into into Pascal files which can be compiled (hopefully) without modification. 

More information about gobject-introspection technology can be found here: <http://live.gnome.org/GObjectIntrospection>

Some of the features of gir2pascal are:

  * Converts the .gir file used as an argument into a .pas file
  * Also converts any .gir files required into .pas files.
  * GObject's are mapped to pascal objects (not classes) for easier use.
  * Creating an instance of an object is done through Foo := _TSomeGObjectType_.new(_Parameters_).
  * Can currently generate bindings for gtk3, glib2, atk1, pango1 and webkit without the resulting files needing to be modified to be compiled and linked.
  * Many other .gir files may work as well
  * Can create test units that help verify the conversion is correct.



Some limitations in generated code

  * The objects are just memory mapped to the C structs that are allocated.
  * The pascal new and dispose functions cannot be used with the objects.
  * You cannot create a new object type that inherits the generated objects. (Technically possible if you don't add any additional fields or virtual methods)
  * The objects created cannot wrap the c varargs methods. However the flat functions with varargs are available as before.



gir2pascal is currently being used to generate the [Gtk+3](<Gtk+3.md> "Gtk+3") bindings for pascal. 

# License

[GPL-2.0](<http://www.opensource.org/licenses/GPL-2.0>)

# Author

Andrew Haines

Email: (andrewd207 at aol dot com)

If you find this program useful you can donate with paypal [here](<https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=SYYDBL2TAQMPY>).

# Source code

Since Lazarus 2.3, this tool is part of Lazarus source code tree and located in [tools/gir2pascal](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/tree/main/tools/gir2pascal>) directory. 

Historic sources are located in [Lazarus-CCR SVN](<http://sourceforge.net/p/lazarus-ccr/svn/HEAD/tree/applications/gobject-introspection/>). Later the tool has received maintenance on [GitHub](<https://github.com/n1tehawk/gir2pascal>) by [Bnortmann](</index.php?title=User:Bnortmann&action=edit&redlink=1> "User:Bnortmann \(page does not exist\)") ([talk](</index.php?title=User_talk:Bnortmann&action=edit&redlink=1> "User talk:Bnortmann \(page does not exist\)")) and finally was imported from his repository into Lazarus source tree.

---

_Source: [https://wiki.freepascal.org/gir2pascal](https://web.archive.org/web/20250114033242/https://wiki.freepascal.org/gir2pascal)_
