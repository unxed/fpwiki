# Adventures of a Newbie

│ **English (en)** │

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** As indicated by the writer, this article was written in **2006** by a newbie who may not have known the best way of doing things. The code and solutions presented may very well not be optimal. Please be aware of this and verify solutions with other documentation; please add links to articles with improved code/solutions. However, having the thought processes of a new user described could be quite valuable for others in the same situation

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** This article was originally written years ago and a lot of things have changed/improved since then.

I'm writing this because like you I'm a newbie (as of today only five days under my belt). As a newbie I ran into many issues that I believe would have stopped others dead in their tracks. Not that I'm some sort of genius or better than others – just hard headed. To make matters worse I'm coming from the MS windows world and I'm learning Lazarus on a Linux system. Worse of all I have no Delphi experience. 

## Contents

  * 1 Getting started
    * 1.1 Installation
    * 1.2 Starting the IDE
    * 1.3 Finding the right mail-list
    * 1.4 Changing the display
  * 2 Using SQLdb
  * 3 SQLDB and connections
  * 4 Add a Login Screen



## Getting started

### Installation

I am using SUSE 10.0. That is important because Novell SUSE installs without the Gnome support development libraries installed. In fact I think most of the the Linux distributions by default do not install the development libraries. To make matters worse SUSE's default window manager is KDE. Again, that matters because the KDE uses the QT graphic libraries. Now that I have said that I need to tell you that Lazarus uses the GTK graphic libraries in the default installation. Although, as of today 19 March 2006 there are a few developers that are trying to setup the QT (KDE uses the QT) libraries for Lazarus. 

So before downloading the FPC (Free Pascal Compiler) and Lazarus IDE files you should install the development graphic libraries. In my case that was very easy. SUSE 10.0 has a utility called 'YAST'. Open YAST click on the software management icon. It opens a search window - type 'GTK' for the search string. The search screen returns a list of all the GTK items available. I checked everything because I really did not know what was required. YAST suggest other software packages to install. I just agreed to everything. I'm sure that someone out in the GTK world knows exactly what was needed but I sort of shotgun the process. It was simple. 

As of today there are two versions of FPC. I could just use the version that is available from SUSE 10. But I was told that it was better to use the latest version from the Lazarus web site. 

So, I downloaded the latest binaries of FPC and Lazarus. That means I did not have to compile the source code. It comes in an install package – 'RPM'. Again, I used YAST to install both packages. The FPC website suggested that I also download the FPC source. So I installed both the FPC compiler and FPC source packages using the YAST. Next I installed the Lazarus package. Yast complained. Yast suggested that something was wrong. It was trying to find a GTK+ library. It was installed with a slightly different spelling. So I took a chance and installed the package (YAST offers a way to override the errors). It worked for me. 

The first thing to figure out was how to start Lazarus. Well, I found two ways. I found 'startlazarus' and 'lazarus'. Both work well. Although I think the 'startlazarus' seems to open faster. I have no idea why there is two programs. (See [startlazarus](<startlazarus.md> "startlazarus") page for an explanation [Bart](</User:Bart> "User:Bart"), 28 march 2014.) You can then open a terminal window and type either of the names. Or you can create an application icon. Try right clicking on the desktop. 

### Starting the IDE

Lazarus opened! I was very excited. But after my first excitement I realized that the default display was butt ugly. Ofcourse, I was comparing Lazarus with other KDE programs I had been playing with – like kmail. I worked with Lazarus that way for four days. I'll explain later how to change the look of the IDE. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Context-sensitive help - pressing F1 on a keyword - is available for many FPC and Lazarus (LCL) keywords. Windows installers set this up for you by default. More details: [Installing Help in the IDE](<Installing_Help_in_the_IDE.md> "Installing Help in the IDE")

Next question. How do I use this IDE? Clicking on the help puts you on a website. So if you don't have a good connection it might take a while to come up and in my case the website was down for two days. So what was I to do? I turned to web and started reading from a Delphi tutorial how to do things. Why? Because Lazarus is patterned after Delphi. It sort of worked, but I have to say that in a big way Lazarus is NOT Delphi. Next I joined the Lazarus forum and the FPC forum. But I noticed immediately that not much is happening on either of the forums. Most importantly, there was not a FAQ section. Next, I joined the Lazarus CCR mailing list. Turns out that few, if any, were using the mail list. For a while, I thought I might have picked a dying Open Source project. I then tried the IRC Lazarus-ide on the Freenode server. I found a number of people using the IRC. That was at least encouraging. 

### Finding the right mail-list

Someone with a kind heart was able to direct me to the right mailing list “lazarus@miraclec.com”. There was a lot more traffic on that mail list. Normally, I try to sit back and read a mailing list to get the feel of what is discussed. But since I was already running into trouble with Lazarus I jumped right in with questions. The responses were very helpful. At this point I would like to point that although the mail list is there to help, it is expected that you will at least make some effort to google your questions for answers. But I bet you will run into trouble because Lazarus does not equal Delphi. Also be very careful with the IRC. The IRC has mostly the developers of FPC and Lazarus. In general they are a friendly group but I noticed they have little patience with newbies. 

### Changing the display

So how to fix the display of the IDE (or configure it to use gtk2). First how did I discover how to do this. I read a forum message that described the way to install a Lazarus package (in this case the report writer reportlivre). The message suggested that to install packages required that the user be 'root' due to a permission issue. So I logged in as root and clicked on tools->configure Lazarus. A dialog will pop up and I noticed I could configure Lazarus to use several graphic libraries. Currently, only gtk1 and gtk2 have everything required (almost everything). I chose gtk2 and saved. Then tools-> Build Lazarus. After the build completes – restart Lazarus and you should see a completely different look. I immediately ran into a problem. Recall Novel SUSE 10.0 uses KDE as the default. Many of the buttons and menu items captions were being cut off at the bottom. After some google searches I discovered that I can open the Utilities->Desktop->Gnome configuration tool->fonts->details and change the resolution to 93 (actually changing it to anything will restart the gtk-qt-engine). Then save (close). That fixed the issue of cutting off the bottoms of the captions but after each reboot I have to reset it. I currently have no solutions. It's not much trouble because I don't shut off my computer very often. I have noticed that Lazarus is not as stable using the GTK2 interface. Features appear to be missing – like the help hints for the tool bar. So in the end I returned to the GTK1 interface. 

My next article will be more informative with respect to programming. 

John Fabiani 

## Using SQLdb

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** See [SQLdb_Tutorial1](<SQLdb_Tutorial1.md> "SQLdb Tutorial1") and following tutorials if you are looking for tutorials on using databases with Lazarus

First and fore most this is not a tutorial. I really don't know how to write a tutorial. It is a statement of my journey to enlightenment – or how I learned to use the SQLdb unit. SQLdb are controls that you will place on your forms to allow you to access a database. 

Mind you this was written by a newbie and could contain information that might not work with your database. In my case I'm using the Posgtres database engine. Postgres is available on most platforms i.e. Linux, Windows and is one of the database engines supported by FPC (Free Pascal Compiler). Now that you have been warned I'll start at the beginning. 

First let me say that if you are like me you started reading on line descriptions of the database access tools for Delphi. Why Delphi? Because Lazarus is patterned after Delphi. In my case the articles just confused me (although they provided some insight). I quickly discovered that each version of Delphi had different tools and different procedures to access data. 

References to BDE, dbexpress and tools like Zeoslib were everywhere on the web. So when reading an article I had to be very careful which version of Delphi the article was discussing and if the article was discussing a third party tool (a tool not from Borland) like zeos. Also watch out for the Kylix articles. Most annoying was the terms the articles were using – like clientdatasets, sqlqueries, dbexpress datasets, dataset providers, datasnap, data-aware controls. After each article I read I'd go to the Lazarus IDE and attempt to find these controls. Of course they were not there. So I decided to write this article/how to/tutorial/rant (you decide what it is). 

So how do Lazarus SQLdb controls work? Normally I could find a tutorial that some kind soul provided on the web that would help me understand. But as of 24 March 06 I could not find a tutorial. (March 2014: See [SQLdb Tutorial1](<SQLdb_Tutorial1.md> "SQLdb Tutorial1") for a tutorial series that should be very usable) 

In fact very little was written about the SQLdb controls on the web. I had the source code to SQLdb (it is in the Free Pascal Compiler source) but as a newbie to Pascal I found reading the code almost impossible (although the comments were a little helpful). 

I did have a few examples that were in the FPC source code folder. I did not find these examples they had to be pointed out to me by the author of SQLdb via the Lazarus mail-list. So from the examples is where I started. I had only one advantage. I know the PostgreSQL database engine from a past programing job. Armed with the examples and the ability to setup a Postgres log I charged in. 

Before you can perform any operation on a database you must make a connection to it. Therefore the first thing is to click on the SQLdb tab on the Lazarus Editor toolbar. You will see several icons. For now we are only interested in the connection icon for Postgres: the icon with the elephant as the picture (elephants never forget – that why the elephant). 

Of course other database engines are supported – but like I said I only know what I have done to get postgres working. Let's continue, select and drag and drop a connection on to the form. Lazarus will name the control for you - so we will just use the name provided. 

Lazarus named my connection 'PQConnection1'. If you are using a different database connection your name will be different than mine. Also Lazarus will allow you to name the connection anything you would like. The placing of 'PQConnection' control on the form will open an object inspector (I would call this a property sheet – but what do I know). In the Object Inspector you will need to fill in several of the properties. Fill in the 'UserName', and 'Password' properties. This is the name of a user allowed access to the database. Then fill in the 'DatabaseName' with the name of the database you want to connect too. Next fill in the 'HostName'. This can be an IP address. If by chance you are running your database engine on the same computer as you are running Lazarus this could be either '127.0.0.1' or 'localhost'. 

Next is something I think is cool. While you are setting up the connection in the Lazarus IDE you can test it. Change the 'Connected' property to 'True'. If you can, you know that your connection to the database engine is working. If you get an error message - read it. The message is informative. Most issues having nothing to do with the SQLdb connection control but are issues of database access. In other words user permission to access the database. 

The 'PQConnection1' is like a light switch. Set 'Connected' to 'True' and you have turned on the connection and setting 'Connected' to 'False' and it's off. Like a light switch – on and off. You can also use two procedures: 

  * 'PQConnection1.open;' is the same as setting 'PQConnection.Connected:=True;'
  * 'PQConnection1.close;' is the same as setting 'PQConnection1.Connected:=False'.



There are many other things you can do with just the 'PQConnection1'. Mostly you can do DDL (Data Definition Language) commands using the 'ExecuteDirect'. There are more properties and methods but I don't know them. However, I do know that the method "OnLogin" does not work as of today. 

Next drag a 'TSQLTransaction' icon to the form – Lazarus will name it 'SQLTransaction1' for you. Nowhere did I find an article that even mentions the 'TSQLTransaction'. But the FPC source code helped provide hints. This is what I saw in the code: 
    
    
    procedure Commit; virtual;
    procedure CommitRetaining; virtual;
    procedure Rollback; virtual;
    procedure RollbackRetaining; virtual;
    procedure StartTransaction; override;
    constructor Create(AOwner : TComponent); override;
    destructor Destroy; override;
    property Handle: Pointer read GetHandle;
    procedure EndTransaction; override;
    

I do know SQL so I could figure out what it did. 

StartTransaction = It does a 'begin' but most likely more than just that because the Postgres log suggested other things were occurring but it was not stopping anything I was doing so I did not concern myself it. 

EndTransaction = Ends the transaction and is not a 'COMMIT', it is a 'ROLLBACK'. 

Commit = commit the transaction (i.e. save the changes in the transaction). 

Rollback = Rollback the Transaction (i.e. back out the changes made in the transaction). 

I don't know what the following are used for: CommitRetaining, RollbackRetaining. 

I think they have something to do with Delphi's BDE tools. If I understand correctly using 'CommitRetaining' or 'RollbackRetaining' will commit or rollback the data but keep the dataset open. Maybe somebody will tell me someday for sure. 

All you have to do is set the 'DataBase' property to the connection (remember in the object inspector). In my case that was the 'PQConnection1'. The 'SQLTransaction1' has a property of 'Active'. Again like the 'PQConnection1.Connected' it acts like a light switch to turn off and on the light. 

However, in this case the SQLTransaction1 control actually sends a 'BEGIN' to the database engine when 'Active' is set to True. And a 'Rollback' when set to 'False'. And just like the 'PQConnection1' you can use ' SQLTransaction1.StartTransaction' and 'StartTransaction.EndTransaction' to do the same thing. 

So in other words the 'SQLTransaction1' is nothing more than a way to control SQL transactions. Just like the name of the control implies. So in your code you will need to use the 'Begin', 'Commit', 'Rollback' SQL commands at some point. You can do it here in the SQLTransaction control. {this is not exactly right – see text below} 

Next comes what I call the SUPER control - the 'TSQLQuery' control. Again drag and drop the control on the form. Fill in the "Database" and the "Transaction" fields by clicking the far right down arrow. In fact if you fill in the "Database" field, the "Transaction" field should fill in automatically. Now double click on the "SQL" field and a dialog will open. You will now need to type a simple SQL select statement. Make it something simple – like 
    
    
    select * from customers
    

Save the statement. You will not see the statement in the field. But it is there. 

Ok, but what does it do? The object inspector does have the 'Active' property and it acts just like the other control turning off and on the 'SQLQuery' control. I found that the Object inspector was not really a lot of help. But that is where the FPC examples really helped me. 
    
    
    with Fquery do
        begin
    
        SQL.Clear;
        
        SQL.Add('select * from FPDEV');
    
        open;
        
        Edit;
        FieldByName('name').AsString := FPdevNames[1];
        FieldByName('birthdate').AsDateTime := FPdevBirthDates[1];
        Post;
        
        Append;
        FieldByName('id').AsInteger := 8;
        FieldByName('name').AsString := FPdevNames[8];
        FieldByName('email').AsString := FPdevEmails[8];
        FieldByName('birthdate').AsDateTime := FPdevBirthDates[8];
        post;
        
        ApplyUpdates;
    
        end;
      Ftransaction.Commit;
    
      Fquery.Free;
      Ftransaction.Free;
      Fconnection.Free;
    end.
    

In this example the 'Fquery" is a TSQLQuery control just renamed. Also in this example the connection and the transaction controls are already setup. The 'Fquery' has the 'Database' field set to a connection and the 'Transaction' field set to a transaction. Notice what the example is doing: 

  1. The program clears whatever SQL command is in the control with 'SQL.Clear' (this is actually 'Fquery.SQL.Clear')
  2. Next it uses an 'Add' procedure to add a SQL statement. The statement of what will be done.
  3. Then "open". This will carry out the request. In other words the SQL statement will execute.
  4. "edit" is next and the procedure will allow editing of the data that was retrieved. Works well with datacontrol aware objects.
  5. Next few lines actually change the data by field name.
  6. "post" saves the data to the local cache but does NOT commit the data or send a update statement to the database.
  7. "append" is another example of a routine that SQLQuery has to add a record/tuple or row.
  8. "applyupdate" will send a update statement to the database.
  9. And notice that the actual commit comes from the transaction control (in the example it is 'Ftransaction.commit').
  10. The rest of the commands close the controls with the normal 'free' procedures. Notice the order of the closing.



BTW why is it the super control – check out all the events it has in the object inspector. 

Since I have only gotten this far in my understanding I'll stop here. I hope later I can add more knowledge as I attain a better understanding of how to use the SQLdb controls in real programs. 

John Fabiani 

## SQLDB and connections

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** See [SQLdb Tutorial4](<SQLdb_Tutorial4.md> "SQLdb Tutorial4") for a solution to the problems mentioned below: using Data Modules which is the preferred way of managing a connection and a shared transaction for an entire application

I'm Back! 

Change my thinking or don't use SQLdb. 

In the windows world I have always used an ODBC connection to the database engine for my connections from FoxPro. I have come to expect that I controlled when a transaction starts and stops. 

But in the Lazarus/FPC world this is not the way it works. The act of setting the transaction.active:=True; causes the transaction control to send a 'begin' to the database engine. This means if you are just requesting data from the database as in “Select * from sometable” it is done within a SQL transaction. In other words a 'begin' was sent to the database engine. BTW the corresponding "Rollback" or "Commit" is not sent automatically after retrieving the data. So what has to occur is as follows: 
    
    
    transaction.starttransaction; // the same as transaction.active:=true;
    sqlquery.open;  //this already has the sql statement “select * from sometable”
    transaction.endtransaction; // the same as transaction.active:=False;
    

This of course is very different than what I'm used to. I sort got my head around using the transaction start and end. But that is not all that is different. Again, in the windows world I only used one connection to the database for everything (well almost everything). So in my coding with FPC I attempted to use one connection (one pqconnection). Man this proved to be difficult. It worked but it was a lot of coding. 

I asked the mailing list how everyone handled this question (use one connection or use multi connections) I got no response. I'm not sure what that means but I checked the web for delphi examples of data access with multiple forms. 

In each example (remember these were for delphi) the authors used multiple connections. One for each form. So now I'm using a new connection for each form I open. This actually made the coding much easier. I still had to setup the connection fields (databasename, username,password, etc) in code but the rest of the data access controls can be set at design time and required no setup coding. 

But what this means is I will more than one connection to the database per each user of my program. Image a user with 10 forms open and ten users. This means I could have 100 connections open to the database engine. Is this important? I know that the Postgres database engine can support thousands of connections. So in the end I guess not. I just have to set a parameter in the Postgres configuration. So I have changed my thinking. 

## Add a Login Screen

April 17, 2006 Adding a login screen (splash screen too). 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Your Lazarus installation has a ready made database login screen that you can use for multiple databases. See examples/database/sqldbtutorial3 for the code and [SQLdb tutorial 3](<SQLdb_Tutorial3.md> "SQLdb Tutorial3") for a description.

OK so I will need a login screen because my program accesses a database. It has to be the first screen the user will see but not the main screen. My main screen is a form with a bunch of buttons (sort of a menu of buttons) to call other forms and I want it to control and close all the other forms should the user close the main form. So how do I get a form to display before the main screen opens? Searching the internet provided part of the answer. Researching the Lazarus forum provided even more and including the the trick to making a splash screen to work. 

In the main program file 'project1.lpr' file is where you add your code. This is the code that starts the program. Yours should look something a like below and mine opens a login form: 
    
    
    program project1;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Interfaces, // this includes the LCL widgetset
      Forms, Controls,
      { add your units here }, menu1, login1, sqlunit, SQLDBLaz, custunit,
    contactunit, lookup, vendunit, speciesunit, varietyunit, wareunit, icunit;
       var loginform:Tlogin;
    begin
      Application.Initialize;
      loginform:=Tlogin.Create(nil);
      loginform.edit2.text:='johnf';
      if loginform.showmodal <> mrOk then
      begin
      loginform.free;
      loginform:=nil;
      end;
    
      Application.CreateForm(Tmenuform, menuform);
      Application.Run;
    end.
    

Notice that I create and open a form after the “Application.Initialize;” It is important that your code be added after the 'Application.Initialize;'! The above works as long as I call 'showmodal'. But if I call 'show' the form opens but nothing is displayed in the form. To get it to display correctly just add 'Application.ProcessMessages;' as in the example below. 
    
    
    Application.Initialize; //this line exists!
    splashScreen := TSplashScreen.Create(nil);
    SplashScreen.ShowOnTop;
    Application.ProcessMessages;  //need this line to allow display
    SplashScreen.Update;
    Delay(1000);
    Application.CreateForm(TForm1, Form1);
    SplashScreen.Hide;
    SplashScreen.Free;
    Application.Run;
    

I'm still learning and I hope more will come. Bye for now.

---

_Source: [https://wiki.freepascal.org/Adventures_of_a_Newbie](https://web.archive.org/web/20250314121721/https://wiki.freepascal.org/Adventures_of_a_Newbie)_
