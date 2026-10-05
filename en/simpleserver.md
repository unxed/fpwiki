# simpleserver

## Contents

  * 1 Intro
  * 2 Usage
    * 2.1 Command-line options
    * 2.2 Config file
      * 2.2.1 Config section
      * 2.2.2 Proxy section
      * 2.2.3 Locations section



## Intro

Simpleserver is a sample application that can be used to serve files using the HTTP protocol. While it's primary purpose is to showcase the possibilities of FCL-Web and the [fphttpserver](<fphttpserver.md> "fphttpserver") and [fpwebfile](<fpwebfile.md> "fpwebfile") units, it can be used as a small web server to aid in e.g. development of pas2js applications. 

## Usage

You can get an overview of the supported command-line arguments using the **-h** option: 
    
    
    > ./simpleserver -h
    Usage simpleserver [options] 
    Where options is one or more of : 
    -A --api=path,secret  Activate location API on path, using secret as accepted bearer token.
    -a --max-age=age      Set max-age expiry header on returned file requests.
    -c --config=file      Ini configuration file (default: simpleserver.ini)
    -b --background       fork to background
    -d --directory=dir    Base directory from which to serve files.
                          Default is current working directory: /home/michael/FPC/trunk/packages/fcl-web/examples/simpleserver
    -e --echo             Activate /echo URL.
    -h --help             This help text
    -H --hostname=NAME    Set hostname for self-signed SSL certificate
    -i --indexpage=name   Directory index page to use (default: index.html)
    -m --mimetypes=file   Path of mime.types. Loaded in addition to OS known types
    -n --noindexpage      Do not allow index page.
    -p --port=NNNN        TCP/IP port to listen on (default is 3000)
    -q --quiet            Do not write diagnostic messages
    -Q --quit=PWD         Register /quit URL. Send request with password variable equal to PWD to stop
    -s --ssl              Use SSL
    -x --proxy=proxydef   Add proxy definition. Definition is of form:
                          name:BaseURL
    
    Config file is ini file, section [Server]. Key names are long option names
    Proxies are defined in section [Proxy], Key is name, value is URL
    Locations are defined in section [Locations], Key is location name, value is path
    

### Command-line options

The command-line options are used as follows: 

-A --api=path,secret
    Specifying this option activates the location API [fpwebfile](<fpwebfile.md> "fpwebfile") on _path_ , using _secret_ as accepted bearer token.
-a --max-age=age
    This option sets the max-age expiry header on returned file requests The _age_ argument is a number in seconds.
-c --config=file
    Specify the name of an Ini configuration file.
    If this option is not specified, the program will look for a file _simpleserver.ini_ in the current working directory and load that if it exists.
-b --background
    Available on Unix only, this will fork the program so it runs in the background
-d --directory=dir
    Set the base directory from which to serve files.
    The default is current working directory.
    Additional locations can be specified in the **Locations** section of the config file.
-e --echo;
    When you provide this option, it will activate the /echo URL.
    This URL will simply echo the incoming request.
-h --help
    Prints the help text as shown above.
-H --hostname=NAME
    When SSL is enabled, this option sets the hostname for the self-signed SSL certificate.
-i --indexpage=name
    Set the directory index page to use when the URL refers to a directory.
    The default is _index.html_
-m --mimetypes=file;
    Set the path of the _mime.types_ file. This file is Loaded in addition to OS known types.
    On Windows, the server will look for and load a 'mime.types' file in the directory where the program is located.
-n --noindexpage
    Do not allow to serve an index page.
-p --port=NNNN
    Set the TCP/IP port to listen on (the default port is 3000)
-q --quiet
    Do not write diagnostic messages. By default a log message is written for every file served.
-Q --quit=PWD
    Register the /quit URL.
    If you send a request with password variable equal to PWD, the server will stop executing.
-s --ssl
    Use this option to enable SSL. You must also specify the _-H_ option if you use this.
-x --proxy=proxydef
    Add a proxy definition. This option can be specified multiple times.
    Each definition is of the form _name=URL_ where name is the first component of a URL path, the second is the URL to forward the request to.

### Config file

The configuration file can have 3 sections: 

Server

This section contains the configuration variables. 

Locations

This section contains the configuration variables. 

Proxy

This section contains the definition of proxy URLS. 

#### Config section

The following keys are supported 

Port (integer)
    Set the port to listen on
Directory (string)
    Directory to serve files from
IndexPage (string)
    Name of directory index page (default 'index.html')
HostName (string)
    Name of host when enabling SSL
Mimetypes (string)
    Name of _mime.types_ file to load.
SSL (boolean)
    Enable SSL mode or not.
Quiet (boolean)
    Do not output diagnostic messages.
Quit (string)
    If not-empty, register /quit url with value as password.
Echo (boolean)
    If true, register /echo to echo requests.
NoIndexPage (boolean)
    Do not serve index pages.
Background (boolean)
    Unix only: Fork to background process.
MaxAge (integer)
    Specify max age (in seconds) of files.
API (string)
    Enable locations API. The value is in the form _path,secret_.
    The path is the first component of the URL at which the location REST API is served.
    The 'secret' is used to secure the API: every request must have the value of secret in the URL as either a _Authorization_ header bearer token, or as the APIKey query parameter.

#### Proxy section

Every key in this section is registered as a proxy URL. This means the values must be valid URLS. 

Example: 
    
    
    [Proxy]
    drive=http://drive.google.com/
    

so an url 
    
    
    http://localhost:3030/drive/my/file/url.txt
    

will be forwarded to 
    
    
    http://drive.google.com/my/file/url.txt
    

#### Locations section

Every key in this section is registered as a file location URL. This means the values must be valid paths. 

Example: 
    
    
    [Locations]
    temp=/tmp
    

so an url 
    
    
    http://localhost:3030/temp/backup.log
    

will serve the file 
    
    
    /tmp/backup.log

---

_Source: [https://wiki.freepascal.org/simpleserver](https://web.archive.org/web/20240410132916/https://wiki.freepascal.org/simpleserver)_
