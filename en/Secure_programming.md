# Secure programming

│ **English (en)** │

## Contents

  * 1 Foreword
  * 2 Don't trust input
    * 2.1 Why can't I trust an input?
    * 2.2 What type of problems can we expect?
      * 2.2.1 Buffer Overflow
      * 2.2.2 DoS Attack
      * 2.2.3 Injections
  * 3 Access to your data and modification
    * 3.1 Encryption: is it enough?
  * 4 Myths and Assumptions
    * 4.1 myths
    * 4.2 assumptions
  * 5 Analyse to understand threats and security
  * 6 Specific solutions
    * 6.1 Overflow
      * 6.1.1 Buffer overflow
      * 6.1.2 Number Overflow
      * 6.1.3 What are the security risks in Overflows ?
    * 6.2 Denial of Service
    * 6.3 Injection
      * 6.3.1 Ineffective sanitising
      * 6.3.2 Restricting input by SQL query parameters
    * 6.4 Efficient code
  * 7 Beyond the document



## Foreword

This wiki page is an attempt to teach a different approach in how to create software. The page uses very simple examples to show that many problems can be taken advantage of in order to create a security attack on a computer, a program or on an entire system. 

Please note that the document is only a start to teach how to write better and somewhat more secure code, but it does not attempt to be a complete guide on how to do so. In fact, it's only a brief overview of how we need to see our code and program, and how to avoid many common problems our there. 

Please remember that this document is about educating for better coding, not a guide for hacking and cracking programs. 

You may also be interested in [The Power of 10](<The_Power_of_10.md> "The Power of 10") for concise advise. 

## Don't trust input

When developing a program, it is likely that it will interact with the user in some way, even if that means only reading files in the system and presenting the data. 

Usually at schools and at universities when one starts to write programs, that person learns how to receive input, while teachers usually say to that person “assume that the data you receive is valid”. That's when the problems begin: 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** We can not trust any input that we can not control as its contents are unknown and might exploit a vulnerability in our software.

Reading from a file is reading an untrusted input, and so is reading users input, or accepting input from a network for example. 

### Why can't I trust an input?

In order to understand why an input is dangerous, we first need to understand what is an input. 

An input can be from a key stroke, and mouse movement or mouse button clicks, or from reading and accepting information from many other ways like a data stream or even system functions. In fact, anything your program gets from the outside is input. 

It does not matter what the type of input is: for example a user or another system can give us wrong input, and the reasons can be intentional or a mistake. You can not control this input, and the main reason is that you can't guess what the input will be. 

The results could be empty (NULL) “data” that the user provides us, a number that is out of our expected range, or a larger amount of chars than we expected, or even an attempt to change the address of the variable that accepts the input from the user. We just can not know what the user is going to provide. 

Any “unsafe” handling of input can cause retrieval of critical information that the user is unauthorized to see, or modification of data that the user is not permitted to do, corruption of data, or even breaking (crashing) the program itself. 

### What type of problems can we expect?

On every type of bug you probably will find a type of attack, but I wish to give a small list of very common types of attacks, instead of writing a lot of the attack types. 

The most common attack types are: 

#### Buffer Overflow

When a given data overflows the amount of memory that was allocated for it: 
    
    
    var
      iNums : array [0..9] of integer;
      ...
      FillChar (iNums[-1], 100, #0);
      ...
      for i := -10 to 10 do
      readln (iNums[i]);
      ...
    

In this example we can see that for the static array of iNums we gave the ability to accept only 10 numbers, while we entered to the variable a content of 21 numbers. 

Please note that while the compiler might warn in simple cases, it won't in more elaborate forms. 

If the user can input data that is sent to the buffer, he can supply some values that can be interpreted as machine code instructions, which will be written outside our buffer. The computer could then execute this code instead of the code that should have been there. That's a buffer overflow. 

#### DoS Attack

Denial of Service is not only a network problem, but can exist also in many other ways: 
    
    
    procedure Recurse;
    begin
      while (True) do
        begin
          Recurse;
        end;
    end;
    

This procedure will run until the system is out of resources as it allocates more stack memory every recursion, and will cause the system to stop responding, or even crash. Although some systems – like Linux – will try to give you the ability to stop running the program, it will take a lot of time from you to do it. 

Please note that this is only a static example, but we made a DoS attack on a system running this code. 

  
Another known DoS attack is the lack of freeing system resources such as memory, sockets, file descriptors, etc... 

For example: 
    
    
    ...
      begin
        while True do
          begin
            Getmem(OurPtr, 10);
            OurPtr := Something;
          end;
      end.
    

This example displays a memory allocation (Getmem is like the C malloc: it reserves memory for use), but we exit the execution without freeing the memory at the end of its use. 

#### Injections

When the user gives us an input, and we are working on the given input directly without sanitizing it, the user can place in some SQL tags or code (like script code, or machine code) for example, that will cause our program to perform some action (e.g. delete some records/tables, send the user some restricted data such as database/table structure, database user and password, content of directory or file, or even execute a program at the computer). 

An SQL injection example: 

User Input: 
    
    
     Please enter your name: a' OR 1=1
    

Inside the code: 
    
    
    ...
    Write('Please enter your name: ');
    ReadLn(sName);
    Query1.SQL.Add('SELECT Password FROM tblUsers WHERE Name='#32 + sName + #32);
    ...
    

By submitting this SQL statement as user input, it will cause our query to add OR 1=1 to the SQL statement passed to the database: in this case, this always results in true, and the user has gained unauthorized access to the program. 

## Access to your data and modification

Not only your program will have access to the data it uses. If you store the data in files or (remote) databases, an attacker can gain access through the operating system (and/or database and/or network layer/database protocol). 

### Encryption: is it enough?

To counter the thread described above, programmers often use encryption. It can be used to provide: 

  * protection of the confidentiality of data
  * non-repudiation (is the data created by the person who said he did) and integrity (is the data unchanged) (using additional mechanisms such as digital signatures/hashing)



both for 

  * communication of data
  * storing/retrieving data



If you use these methods, your data is not automatically safe, however. 

There are multiple attacks possible on encrypted data: 

  * attack on insecure algorithms or implementation (e.g. using a known plaintext attack)
  * attack on the encryption keys (e.g. by reverse-engineering your program e.g. if it stores keys in a file or itself, or by patching the program so it intercepts encryption keys/passwords entered by the user).



If you don't know exactly what you are doing (and you won't, unless you have cryptographic training), please use (in decreasing order of preference): 

  * well known libraries that are maintained/patched (e.g. the built in FPC libraries, and libraries such as DCPCrypt, or external libraries like openssl or cryptlib) that use well known/widely used protocols that use well known/widely used algorithms 
    * Use e.g. Trusted Authentication/SSPI for SQL Server/Firebird database connectivity to avoid sending passwords over the wire (Firebird 2 and lower: in clear text!) and rely on operating system security for authentication.
    * use cryptlib or openssl with Synapse to implement SSL/TLS instead of rolling your own solution
  * well known/widely used protocols/APIs instead of rolling your own. These protocols must use well understood/widely used crypto/hashing algorithms. Examples: 
    * GPG/PGP
    * TLS (SSL) with PKI/CAs (preferably don't trust only the CAs you need, and perform client certificate authentication if your threat analysis requires it)
    * SSH (e.g. with public/private key authentication, if necessary reinforced with passphrases for the keys)
    * on Windows, use the API to get the currently authenticated user. If you enforce proper OS security (password length, changes, physical access etc), you don't need to manage your own application leverl username/password mechanism.
  * well understood crypto/hashing algorithms (such as AES/Rijndael, 3DES and SHA512). Be conservative in what algorithms you accept (e.g. MD5 is insecure for message signing).



Encryption is part of a set of possible security measures; the effort/money spent on it should be evaluated as part of a seecurity analysis (see below). 

## Myths and Assumptions

Many of the security issues exist because of ignoring important warnings and information that was given by the compiler, and by thinking that your program does not contain any exploitable problem. 

Here are some examples for this type of problem: 

### myths

  * Security by Obscurity – When no one knows about a problem no one can take advantage of it; e.g. use an obscure column name for storing passwords in your database.
  * Secure programming language – There are languages such as Perl that many people think are secure from buffer overflows and other vulnerabilities while that is not true.
  * Hash password is secure – A file that has a hashed password is not secure. Hash can only passed one and you can not retrieve the original data. _I don't get this. Does the author mean that a hashed password can be retrieved by a brute force or rainbow table attack and that it therefore needs a salt, or multiple hash rounds? --[BigChimp](</User:BigChimp> "User:BigChimp") 16:19, 24 July 2011 (CEST)_
  * Nothing can break my program – Believing you're the only programmer in the world who writes faultless code is probably a bit optimistic. Maybe you're lucky and you just write code that doesn't work right without exploitable security vulnerabilies...



### assumptions

  * The QA team will find and fix my security bugs.
  * The user (or somebody else) will not attack my program and its data.
  * My program will be used only for its original use.
  * All exceptions can remain unhandled.



## Analyse to understand threats and security

In general, a programmer (or his employers/business owners) should perform an analysis of all threats (from physical access/attack through logical and social engineering attacks) should be done for the entire system (including the infrastructure – OS, database, network as well as physical machines/cabling/buildings/external connections) to analyse whether you're not leaving open a security hole that is unacceptable (from a risk/benefit perspective). 

The extent of the analysis should depend on the value of the data/processes that the system protects. It obviously makes no sense to go crazy trying to analyse your house's physical security when developing a hobby program to keep track of your bridge scores. 

Benefits of these kind of analyses: 

  * risks remaining after security measures are made known or explicit. Often, there is some discussion on the chances of this risk occurring, or the impact associated with it, but the fact that there is a remaining attack vector is at least clear and decisions can be made based on that information
  * you can fairly easily see what security measures are overengineered (“too secure”, waste of money) or underengineered (“not secure enough”). If you can't use this information now, you can at least learn from it for other projects, including future maintenance/modification of your program



## Specific solutions

Now we know some problems we can encounter when developing programs, we should learn how to fix these problems. All of the problems we saw above manifest into two types: assumptions and lack of careful programming. And for learning how to fix them, we first need to learn to think in a different way than we have up to now. 

### Overflow

For fixing overflow of data, like buffers and other type of input, we first of all need to identify the type of data we need to work with. 

#### Buffer overflow

If we return to our example of: 
    
    
    var
      iNums: array [0..9] of Integer;
      ...
      FillChar(iNums[-1], 100, #0);
      ...
      for i := -10 to 10 do
        ReadLn(iNums[i]);
      ...
    

  
We see here a range that was overflowed by our values, without even checking if the index number is correct. 

In dynamic/open arrays in Pascal we can know the limits of the allocated memory. So all we need to do is check if the size is too small or too big for our buffer, and limit the accepting for the size we wish it to be. 

So the example should be changed into: 
    
    
    var
      iNums: array [0..9] of Integer;
      
      ...
      FillChar (iNums[Low(iNums)], High(iNums), #0);
      ...
      
      for i := Low(iNums) to High(iNums) do
        ReadLn(iNums[i]);
      ...
    

But wait, something is not right yet! 

The readln will accept an unlimited amount of chars, and no one promises us that it will be an integer or even in the range we can handle. 

#### Number Overflow

While a string in Pascal is a pure array (hrmm hrmm.. not really, at least not in FPC, but let's pretend it is for a second, OK ?) so _readln_ will try to find and see what are its limits and will not try to overflow the range we gave that type, but numbers are not the same. 

Numbers have limits, a computer/compiler has limits of many kinds regarding memory and numbers. It can give only a “small” amount of memory for numbers (floating point and integer numbers). And many times we do not need a large range of numbers to use (like boolean variable that needs only two numbers usually). 

In the above example we may have a buffer overflow that will cause a range check error that will give us the wrong number (Carry Flag reminder issues... I'm not going to explain them in here), and we also have a DoS effect, because our program will halt from that point. 

So what can we do to fix that? 

First of all we may wish to work with a string variable that will be the length of the largest number +1 (for minus sign), or we can create our own readln procedure/function that will specialize with the integer type. 

For the first option we can do the following (copied from the FPC documentation): 
    
    
    Program Example74;
    
    { Program to demonstrate the Val function. }
    Var
      I, Code: Integer;
    begin
      Val(ParamStr(1), I, Code);
      If Code <> 0 then
        Writeln('Error at position ', code, ' : ', Paramstr(1)[Code])
      else
        Writeln('Value : ', I);
    end.
    

Here we see how to convert a string into an integer with some very easy error handling. The function _StrToInt_ may also do the trick but it then we need to capture an exception in any error dealing. 

  
Here is a small example for a small _readln_ like procedure for integer numbers. 
    
    
    program MyReadln;
    uses
      CRT;
    
    procedure MyIntReadLn(var Param: Integer; ParamLength: Integer);
    var
      Line: string;
      ch: char;
      Error: Integer;
    begin
      Line  := '';
      
      repeat
        ch := ReadKey;
        if (Length (Line) <> ParamLength) then
        begin
          if (ch in ['0'..'9']) then
          begin
            Line := Line + ch;
            write (ch);
          end
          else
          if (ch = '-') and (Length(Line) = 0) then
          begin
            Line := '-';
            write (ch);
          end;
        end;
        
        if (ch = #8) and (Length(Line) <> 0) then // backspace
        begin
          Line := Copy(Line, 1, Length(Line) - 1);
          gotoxy(WhereX - 1, WhereY);
          write(' ');
          gotoxy(WhereX - 1, WhereY);
        end;
      until (ch = #13);
      
      val(Line, Param, Error);
      
      if (Error <> 0) then
        Param := 0;
      
      writeln;
    end;
    
    var
      Num : Integer;
    begin
      Write('Number: ');
      MyIntReadLn(Num, 2);
      WriteLn('The number is: ', Num);
    end.
    

Please note that you can make it even better, and more efficient if you wish. This is only a very small example to show how to do it. 

#### What are the security risks in Overflows ?

Overflow of memory can allow arbitrary CPU code to be executed and users may run whatever type of code they wish, and nothing can stop them. 

### Denial of Service

Denial of Service (DoS) is one of the hardest types of attacks to prevent. The reasons are: 

  * The denial of service can even be executed without any exploitable bug, like using the “ping” program on a lot of machines to DoS a machine connected to the internet.
  * Every system resource can be a possible denial of service, like opening sockets, reading files or allocating memory.
  * Removal of files like a kernel module can cause a big problem. _I don't get this line --[BigChimp](</User:BigChimp> "User:BigChimp") 19:32, 24 July 2011 (CEST)_
  * Lack of configuration or wrong configuration can cause a denial of service as well if it allows vulnerable resources to be misused.
  * Too much permissions or lack of them _I don't get that not enough permissions can be a problem. Neither too many, either? Is a security risk, obviously but not a DoS problem. --[BigChimp](</User:BigChimp> "User:BigChimp") 19:32, 24 July 2011 (CEST) --[BigChimp](</User:BigChimp> "User:BigChimp") 19:32, 24 July 2011 (CEST)_.
  * Almost any type of exploit can result in a denial of service.



So as you can see, a denial of service can be almost anything that can stop the system from working as it should, because of exploitation or buggy code or just a program that captures system resources.. 

In the above denial of service example: 
    
    
    procedure Recurse;
    begin
      while (True) do
        begin
          Recurse;
        end;
    end;
    

I created also a stack overflow (another type of buffer overflow), that caused the computer to need more memory resources to continue executing the code. 

Any system resource that is available to the program can be abused by not returning it back to the system when the program “does not need it anymore”. The keeping of system resources like memory, or sockets remove from other programs the ability to perform some of their actions. That way most programs will stop their execution and report an error, and some will hang and keep on looking for the system resources. 

Please note that some of the abuse of system resources exists because of a bug in the programming, like waiting for a 150k buffer, while the actual buffer is only 2 bytes, and when the program is still looking for the 150k buffer a new request for a 150k buffer is made etc.. until the system is not able to answer any of the requests anymore (this is a known type of attack). 

A good workaround for this bug is to limit how many non full buffers can be allocated at one time. If the buffer is not full after a timeout, it should be free. However, this solution will also cause a Denial of Service, because the communication will stop anyway at some point, or a slow connection can cause data loss. 

### Injection

There are many ways to inject some type of code into our programs. As we saw at the above example: 

User Input: 
    
    
     Please enter your name: a' OR 1=1
    

Inside the code: 
    
    
    ...
    write('Please enter your name: ');
    readln(sName);
    Query1.SQL.Add('SELECT Password FROM tblUsers WHERE Name='#32 + sName + #32);
    ...
    

The injection occurred when we do not filter our code (sanitize is the more professional word :)): this means checking that we receive only the exact type of input that we are looking for, and nothing else. 

For example, we could check if _sName_ has spaces. If so, do not continue checking for the rest of the variable. This helps if the username is only allowed to be one word consisting of letters, maybe the tick sign (`'`) and maybe even underscore (`_`) and then it's over. If we enter a number, this should be illegal (unless we wish to use “hacker like language” (leetspeak), or allow the use of numbers. 

There are many ways to sanitize your data. The less effective one (but often used) is the following: 

#### Ineffective sanitising
    
    
    function ValidVar (const S: AnsiString; AllowChars: TCharset): Boolean;
    var
      i: Word;
    begin
      i := 0;
      Result := True;
      
      while (Result) and (i <= Length(S)) do
      begin
        Inc(i);
        Result := S[i] in AllowChars;
      end;
    end;
    

The function return true if we have a valid structure of content given by the _AllowChars_ in the _S_ variable. Please note that this function is only a proof of concept and may need more work in order to be fully used. 

Another way to do the same is to use regular expression as the following (this is a Proof of concept only in the Perl language. FPC does not have a fully supported regular expression engine that allows to modify strings): 
    
    
    $sName =~ s/[^a-z0-9\_\']//gi;
    

The regular expression removes any non valid chars from the string and returns to us the purged string. Please note that as far as I know, this regular expression will work also in ereg engines, but with minimal adjustments (_g_ flag instructs Perl to replace all the matching patterns found. _i_ is for _case insensitivity_). 

Now when we know that our input is valid, we need to see what is the use of the variable content. If the variable content is going into a database, or a cgi script, or anything else that has its own syntax, we must escape the content according to the non-allowed or control characters of the relevant language (e.g. SQL for databases). 

There are many ways to escape this type of content. Let's assume for now that this content is going into a query of a database. Now first of all we must make sure that our escaping will not increase our data size above the length limits of our database fields. Because if they will, then we can change from an injection to a data loss/denial of server/buffer overflow problems (a respected database usually will truncate the data and sometimes not in a good location). 

Usually the only escaping we need to do for using a string in a database is to escape only the ticks (') char (although some databases may have problems with more chars then ticks). So all we should do is to represent ticks in a way that will not effect the database engine, like backslash tick (\') or double every single tick to two ticks (''), or maybe even use another char that will be replace the ticks in the query and replace again when we will show it to the user. 

#### Restricting input by SQL query parameters

Obligatory [visual illustration](<https://xkcd.com/327/>). 

After we made sure that we respect the limits, we can continue in our attempts. To escape the code we can use several approaches. A less debugging friendly way, but a sure way of correct escaping is to use the parameters technique: 
    
    
    Query1.SQL.Add('SELECT Password FROM tblUsers WHERE Name=?');
    Query1.Parameters.Add(sName);
    if (Query1.Execute) then
    ...
    

This technique allows the database engine to escape the parameter in a way that we could use the content without any problems of illegal characters. Also, some databases have increased performance for repeated calls to this code as it can prepare an internal statement with parameters for this. The down side is that we can never debug the outcome of the query. That is, we can not see how the content of _sName_ embedded in the SQL statement, and we can never see if our query was correct because of that. 

However, once you have tested the query without parameters, adding the parameters is quite easy, so in practice, this problem is not as big as it seems. 

### Efficient code

The security measures mentioned above complicate code if you used the most efficient code to get needed functionality. Fortunately, you can sometimes pass off code complications to a framework/library that deals with it. For example: in the SQL DoS example you can let the database engine deal with escaping data using parametrised queries at the small added cost of having to use parameters in your code. 

However, writing performance efficient but security-vulnerable code doesn't help anybody except liability lawyers, and forensic experts. 

## Beyond the document

While in this document I gave some short (yeah, I know it's an understatement 😉) examples and information on how to create better code, there are many issues that I did not touch in this document. Part of them are user privileges for execution of the programs, system root kits and other problems that our code needs to take in consideration (environment variable is only one example). 

Please read more resources out there for security issues like 

Buffer Overflows: 

  * <http://www.securiteam.com/securityreviews/5QP0L0AFFS.html>
  * <http://www.securiteam.com/securityreviews/5OP0B006UQ.html>
  * <http://www.securiteam.com/securityreviews/5IP0G1P8AM.html>
  * <http://www.securiteam.com/securityreviews/6Q00L00BFI.html>
  * <http://www.securiteam.com/securityreviews/5UP0B0A60Y.html>



Denial Of Service: 

  * <http://www.securiteam.com/securityreviews/5AP0V0AA1W.html>
  * <http://www.securiteam.com/securityreviews/6Z00T0K95O.html>



SQL Injection: 

  * <http://www.securiteam.com/securityreviews/5DP0N1P76E.html>
  * <http://www.securiteam.com/securityreviews/5UP010A6AA.html>
  * <http://www.securiteam.com/securityreviews/5GP0E2K7FO.html>

---

_Source: [https://wiki.freepascal.org/Secure_programming](https://web.archive.org/web/20250401015135/https://wiki.freepascal.org/Secure_programming)_
