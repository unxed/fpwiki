# pas2js html2form

## Contents

  * 1 Html2form
  * 2 Usage
  * 3 Class map
  * 4 Binding elements
  * 5 Event handling
  * 6 Command-line options
  * 7 Navigation



# Html2form

The HTML2Form application is a small tool that converts a html file to a pascal class. For each tag in the HTML that has an ID, it will create a field in the class, and adds code to link all the fields to the HTML tags. The type of the fields is fully customizable, by default the basic HTML Element classes as defined in the **Web.pas** unit are used. The tool can generate event handlers for the HTML events. 

# Usage

This is a command-line tool. It takes an HTML input file, and produces a pascal unit. In it's simplest form it is invoked as: 
    
    
    html2form -i mypage.html -o mypage.pp
    

Assuming the following html 
    
    
    <html>
    <body>
      <div id="abc"></div>
    </body>
    </html>
    

This will create a simple class TMyPage 
    
    
      TMyForm = class(TComponent) 
      Published
        abc : TJSHTMLElement;
      Public
        Constructor create(aOwner : TComponent); override;
        Procedure BindElements; virtual;
      end;
    

The BindElements call will bind the elemtents to their DOM counterparts. (it is not shown here, but the implementation is generated) 

You can also speciy to create a LFM/DFM file to be created. 
    
    
    ./htmltoform -i mypage.html -o mypage.pas -m webcore.map -F mypage.dfm -p TWebForm
    

This will create a unit that can be used in TMS Web core: 
    
    
    unit mypage;
    {$MODE ObjFPC}
    {$H+}
    
    interface
    
    uses js, web;
    
    Type
      TMyForm = class(TWebForm) 
      Published
        abc : TWebHTMLDiv;
      Public
      end;
    
    implementation
    
    
    {$R *.dfm}
    
    end.
    

# Class map

As indicated before, by default the classes used to map the HTML tags are the ones from the web unit: the basic HTML DOM elements. You can change this with the **-m** map option. This option can be used to specify a JSON file that maps HTML tags and attributes to pascal elements: 

It has the following format, where you can specify the class to be used, the html TAG and attributes that must be present to use the classname. 
    
    
      [
       {
         "class" : "YourClassName",
         "tag" : "YOURTAG",
         "attrs" : {
            name0 : null, // name0 Not present
            name1 : "value",  // name1 equals value
            name2 : "-value", // name2 does not equal value 
            name3 : "~value"  // name3 contains value
           }
       }
      ]
    

The first matching rule is used. 

A sample file that maps elements to the classes used in TMS Web core is included in the tool. 

# Binding elements

By default the tool will use _document.getElementById()_ from the browser to bind the class fields to the HTML element. When you specify a form file to be generated, the converter will assume that the LFM reader does the heavy lifting, and no bind call is generated. You can specify another function to use with the **-g** or **\--getelementfunction** command-line option. 

By default, the constructor will be overridden to call the _BindElements_ function. You can disable this with the _-n_ or _\--no-bind_ option. (for instance if you create a descendent of a class which already calls bindelements); 

If you wish to bind only the HTML Tags that are children of a certain TAG, then you can specify the _-b_ or _\--below-id_ option: elements that are children of the indicated tag are included. 

# Event handling

The tool can generate event handlers for html events if you specify the _-e_ or _\--events_ option. 

The names of these events can be specified in the html file using an attribute that has the event name enclosed in underscores, the attribute value is the name of the function to generate: 
    
    
    <html>
    <body>
      <div id="abc" _onclick_="doclick"></div>
    </body>
    

will be converted to: 
    
    
     TMyForm = class(TComponent) 
      Published
        abc : TJSHTMLElement;
        Procedure doclick(Event : TJSEvent); virtual; abstract;
      Public
        Constructor create(aOwner : TComponent); override;
        Procedure BindElements; virtual;
        Procedure BindElementEvents; virtual;
      end
    

By default, the methods are abstract: the idea is that you can use the above class as a base class, make a descendent of it, and code the event handlers and any additional logic for your form in the descendent. Whenever you change the HTML and need to regenerate the class, additional event handlers may appear (or some disappear) and you can add the various event handlers in your descendent. 

# Command-line options

  * _-h_ _\--help_ : Show a help message.
  * _-b_ _\--below-id=ID_ : Only create elements for child elements of element ID.
  * _-f_ _\--formclass=NAME_ : name of pascal form class to create.
  * _-F_ _\--form-file_ : Generate a form file.
  * _-g_ _\--getelementfunction=NAME_ : Name of getelement function.
  * _-e_ _\--events_ : emit code to bind events.
  * _-i_ _\--input=file_ : html file to read.
  * _-m_ _\--map=file_ : read tag to pascal class map from file.
  * _-n_ _\--no-bind_ : Do not call bindelements in constructor.
  * _-o_ _\--output=file_ : pascal file to write.
  * _-p_ _\--parentclass=NAME_ : name of pascal form parent class.
  * _-x_ _\--exclude=List_ : Comma-separated list of IDs to exclude. if starts with @, then load from file.



# Navigation

  * Back to [pas2js](<pas2js.md> "pas2js")
  * Back to [lazarus pas2js integration](<lazarus_pas2js_integration.md> "lazarus pas2js integration")

---

_Source: [https://wiki.freepascal.org/pas2js_html2form](https://web.archive.org/web/20240304091257/https://wiki.freepascal.org/pas2js_html2form)_
