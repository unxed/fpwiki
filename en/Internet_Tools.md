# Internet Tools

Internet Tools is a library to process web pages and is intended to be easily usable. 

## Contents

  * 1 Other Web and Networking Articles
  * 2 Overview
  * 3 HTTP/S connections
  * 4 X/HTML processing with XPath/XQuery
  * 5 Examples
    * 5.1 Load a web page:
    * 5.2 Download a file:
    * 5.3 Dealing with Sourceforge HTTP download mirrors
    * 5.4 Extract all links from a page
    * 5.5 Get external IP address
    * 5.6 Using Google Translate
    * 5.7 Using Google Mail, other Google APIs, or OAUTH2
  * 6 Use templates to read a form
  * 7 Calculate primes with XQuery
  * 8 Dealing with large numbers
    * 8.1 External links



## Other Web and Networking Articles

  * [Web Development Portal](<Portal_Web_Development.md> "Portal:Web Development")
  * [Networking](<Networking.md> "Networking")
  * [Networking libraries](<Networking_libraries.md> "Networking libraries") \- comparison of various networking libraries
  * [Brook for Free Pascal](<Brook_for_Free_Pascal.md> "Brook for Free Pascal") \- The perfect Free Pascal framework for your web applications. It's pure Pascal. You don't need to leave your preferred programming language.
  * [Sockets](<Sockets.md> "Sockets") \- TCP/IP Sockets components
  * [fcl-net](<fcl-net.md> "fcl-net") \- Networking library supplied with FPC
  * [lNet](<lNet.md> "lNet") \- Lightweight Networking Components
  * [Synapse](<Synapse.md> "Synapse") \- Serial port and synchronous TCP/IP Library
  * [XML Tutorial](<XML_Tutorial.md> "XML Tutorial") \- XML is often utilized on network communications
  * [FPC and Apache Modules](<FPC_and_Apache_Modules.md> "FPC and Apache Modules")
  * [fcl-web](<fcl-web.md> "fcl-web") \- Also known as fpWeb, this is a library to develop web applications which can be deployed as cgi, fastcgi or Apache modules.
  * [ Secure Programming](<Secure_programming.md> "Secure programming")
  * Internet Tools \- A wrapper around Synapse/wininet/Android's http components simplifying https and redirections, and a XPath/XQuery/CSS Selector/JSONiq engine to process the downloaded pages



# Overview

The Internet Tools provide units to process X/HTML data and to download them over a HTTP or HTTPS connection. 

The library is completely implemented in Pascal, thread-safe, GPLed. On Linux it requires [Synapse](<Synapse.md> "Synapse"), and for Unicode processing [FLRE](</index.php?title=FLRE&action=edit&redlink=1> "FLRE \(page does not exist\)"). 

# HTTP/S connections

The Internet Tools do not implement HTTPS connections on its own, but provide wrappers around wininet, synapse and Apache HttpComponents. 

  * Wininet (Windows Internet API) is installed on all Windows/WINE systems and supports all urls the Internet Explorer supports.
  * [Synapse](<Synapse.md> "Synapse") is a platform independent network library.
  * Apache HttpComponents is the old standard Android network library.



The wrappers are implemented as classes derived from an abstract interface, so the application can easily switch between both backends. However, it is recommended to use the wininet wrapper on Windows, the synapse wrapper on Linux and the Apache wrapper on Android. 

All data is uploaded or downloaded as strings, and the Internet Tools will automatically handle checking for local ssl libraries, cookies, referrers or redirections. 

# X/HTML processing with XPath/XQuery

This processing of X/HTML data is the main focus of the Internet Tools and it provides an X/HTML parser, a tree representator, an XPath/XQuery 3.0 interpreter and a template matcher. 

The X/HTML parser processes the X/HTML data and splits it in to tags and contents, and has a SAX-like interface. 

The tree representator find matching start and end tags and stores them in a linked list with additional links, which results in a DOM/XDM-like interface. This X/HTML parsing is not fully standard compliant, however it contains a lot of heuristics to parse real world websites, which are usually incorrect anyways and cannot be read by a standard compliant parser. 

The XPath/XQuery 3.0 layer implements the [XPath 2](<https://www.w3.org/TR/xpath20/>), [XQuery 1.0](<https://www.w3.org/TR/xquery>), [XPath 3.0](<https://www.w3.org/TR/xpath-30/>) and [XQuery 3.0](<https://www.w3.org/TR/xquery-30/>) languages (respectively passing between 100% and 98% of the tests in the XQuery Test Suite), which can be used to extract values from X/HTML trees. It also implements the [JSONiq](<http://www.jsoniq.org/>) standard for processing JSON. 

It also supports CSS 3 Selectors by converting them into XPath expressions and evaluating those. 

The template matcher uses pattern-matching templates to extract several, structured values from a HTML page. Such a template is like a HTML file, which has been annotated at the interesting parts (just like a regular expression is a string annotated with capture groups). 

# Examples

A few example for the simple layer of the Internet Tools (which is not as powerful and customizable as using directly the classes). 

## Load a web page:
    
    
    uses simpleinternet;
    ..
    str := retrieve('http://www.google.de');
    

## Download a file:
    
    
    uses bbutils, simpleinternet;
    ..
    strSaveToFileUTF8(TargetFileNameUTF8, retrieve('http://www.google.de'));
    

## Dealing with Sourceforge HTTP download mirrors
    
    
    uses bbutils, simpleinternet, internetaccess;
    ...
    var TargetFile, SourceForgeURL, Download: string;
    begin
      SourceForgeURL := 'http://sourceforge.net/projects/base64decoder/files/base64decoder/version%202.0/b64util.zip/download';
      TargetFile:='/tmp/download.zip';
    
      //set user agent (fails without it)
      defaultInternetConfiguration.userAgent:='curl/7.21.0 (i686-pc-linux-gnu) libcurl/7.21.0 OpenSSL/0.9.8o zlib/1.2.3.4 libidn/1.18';
    
      Download := retrieve(SourceForgeURL);
      if strBeginsWith(Download, '<!doctype html>') then begin
        //Download page
        //(this branch was never taken, when I tested, but since the synapse example has it, I include it)
        SourceForgeURL:=process(Download, '//a[@class="direct-download"]/@href').toString;
        Download := retrieve(SourceForgeURL);
        if strBeginsWith(Download, '<!doctype html>') then raise Exception.create('Multiple redirections');
      end;
    
      strSaveToFileUTF8(TargetFile, SourceForgeURL);
    end.
    

## Extract all links from a page
    
    
    uses simpleinternet;
    ...
    var link: IXQValue;
    ...
    for link in process('http://www.google.de', '//a/@href') do
      writeln(link.toString);
    

## Get external IP address
    
    
    uses simpleinternet;
    
    writeln(process('http://checkip.dyndns.org', 'extract(//body, "[0-9.]+")').toString);
    

## Using Google Translate

If you have purchased a Google Translate API key: 
    
    
    uses simpleinternet, internetaccess;
    var YourKey, OriginalText, SourceLang, TargetLang, TranslatedText: string;
    OriginalText := TInternetAccess.urlEncodeData(OriginalText);
    TranslatedText := process('https://www.googleapis.com/language/translate/v2' + 
                                 +'?key='+YourKey
                                 +'&source='+SourceLang
                                 +'&target='+TargetLang
                                 +'&q=' + OriginalText, 
                             '$json//translatedText').toString;
    

If you do not have a key using the webpage: 
    
    
    uses simpleinternet, xquery;
    var OriginalText, SourceLang, TargetLang, TranslatedText: string;
    [..]
      TranslatedText := xqvalue('https://translate.google.com')
                        .retrieve()
                        .map('form(//form, {"sl": $_1, "tl": $_2, "text": $_3})', [SourceLang, TargetLang, OriginalText])
                        .retrieve()
                        .map('css("#result_box")')
                        .toString;
    

## Using Google Mail, other Google APIs, or OAUTH2

Send a mail through GMail (see comments for explanation): 
    
    
    uses 
      LCLIntf, 
      simpleinternet, 
      xquery,  
      base64, 
      strutils,
      sysutils;
    
    var 
      ClientSecret, ClientId: String;
      UserSecret, accessToken: String;
      response: xquery.IXQValue;
      messageRFC2822: String;
    
    begin
      // First you need to obtain a client id and secret. These values are constant and never change within an application.
      // For Google's services you get them by registering on https://console.developers.google.com/ as "Installed application"
      ClientId := ....;
      ClientSecret := ....;
      
      // ------------------------------------
    
      // Next you need to obtain an access token.
      // This token is specific for a certain user and the function (scope) of the API we want to call. 
      // It will time out after a while. But it can be cached and reused several times. (thus the part surrounded by 
      // --- should only be executed once, or after the token times out)
      // Thereby the user must visit a webpage, which will show the user's secret, which we need to request the access key
       
      // Request the user secret
      Scope    := 'https://mail.google.com/'; //the API function (scope) we want to call
    
      // This will open a browser window, in which the user will see her secret 
      OpenURL('https://accounts.google.com/o/oauth2/auth?scope='+Scope+'&redirect_uri=urn:ietf:wg:oauth:2.0:oob&response_type=code&client_id='+ClientId);
    
      // Ask the user for her secret (in a console program, use an edit box or input dialog in a gui application)
      readln(UserSecret); 
    
      // With this secret, we can request the access token:
      response := process(httpRequest('https://www.googleapis.com/oauth2/v3/token', 'code='+urlHexEncode(UserSecret) + '&client_id='+ClientId+'&client_secret='+ClientSecret+'&redirect_uri=urn:ietf:wg:oauth:2.0:oob&grant_type=authorization_code'), '$json');
      accessToken := response.getProperty('access_token').toString; 
    
      // The access token will time out.  
      // We can use  response.getProperty('refresh_token').toString to get the refresh token, which will last longer
      
      // ------------------------------------
    
      // Any OAUTH 2 API can now be called by adding '?access_token=' + urlHexEncode(accessToken) after the URL.
    
      // For example sending a mail through GMail:
       
    
      // Construct a simple test mail
      messageRFC2822 :=
       'From: THE USER YOU WANT TO SEND A MAIL FROM@googlemail.com'#13#10+
       'Reply-To: THE USER YOU WANT TO SEND A MAIL FROM@googlemail.com'#13#10+
       'To: THE RECIPIENT YOU WANT TO SEND IT TO@example.org'#13#10+
       // 'Date: Fri, 4 Jul 2015 0:50:06 +0200'#13#10+ a current date, optional
       'Subject: test mail'#13#10+
       'Content-Type: text/plain'#13#10+
       #13#10+
       'Message body....'#13#10;
       
      defaultInternet.additionalHeaders.text := 'Content-Type: message/rfc822'; //set headers for next request
    
      // Send the message.  The call looks simple, but it is extremely picky about the parameters
      // When the token has timed out, this will raise an EInternetException with error code 400
      httpRequest('https://www.googleapis.com/upload/gmail/v1/users/me/messages/send?uploadType=media&access_token=' + urlHexEncode(accessToken), messageRFC2822);
      defaultInternet.additionalHeaders.text := ''; //reset headers
    
    end.
    

# Use templates to read a form
    
    
    uses simpleinternet;
    ...
    
    //html file to process (which you would of course usually not include in the program)
    const EXAMPLE_HTML: string =
      '<html><head><title>...</title></head>' +
      '<body>lorem ipsum' +
      '<form>lorem ipsum' +
      'foobar: <input type="text" name="abc" value="123"/>' +
      'foobar: <input type="text" name="def" value="456"/>' +
      'foobar: <input type="text" name="ghi" value="678"/>' +
      '</form>' +
      '</html>';
    
    //template (as you can see it is the html reduces on the relevant parts)
    const EXAMPLE_TEMPLATE: string =
      '<form>' +
      '<input type="text" name="abc">{abc:=@value}</input>' +
      '<input type="text" name="def">{def:=@value}</input>' +
      '<input type="text" name="ghi">{ghi:=@value}</input>' +
      '</form>';
    
    begin
      process(EXAMPLE_HTML, EXAMPLE_TEMPLATE);
      writeln(processedVariables.get('abc').toString);
      writeln(processedVariables.get('def').toString);
      writeln(processedVariables.get('ghi').toString);
    end.
    

The newest version has a special "XPath" function `form` to read a form. E.g. `form(//form[1]).url` would return `?abc=123&def=456&ghi=678` in the above example. But you still need the templates to do something complex that is not form related. 

# Calculate primes with XQuery
    
    
      uses xquery;
     [...]
      var v: IXQValue;
     [...]
      for v in query(
        'xquery version "1.0";'                                +
        'declare function local:isprime($p){'                  +
        '  every $i in 2 to $p - 1 satisfies ($p mod $i != 0)' +
        '};'                                                   +
        'for $i in 2 to 30 where local:isprime($i) return $i') do
        writeln(v.toString);
    

# Dealing with large numbers

There is no limit to size of numbers. For example you can use the extension function `x:product` to calculate `1000!` and output it with the standard XQuery 3.0 function `format-integer` 
    
    
      uses xquery;
      writeln(query('format-integer(x:product(1 to 1000), "w")').toString);
    

which will print 

` four hundred two quattorquinquagintaoctingentillion three hundred eighty-seven tresquinquagintaoctingentillion two hundred sixty duoquinquagintaoctingentillion seventy-seven unquinquagintaoctingentillion ninety-three quinquagintaoctingentillion seven hundred seventy-three novenquadragintaoctingentillion five hundred forty-three octoquadragintaoctingentillion seven hundred two septenquadragintaoctingentillion four hundred thirty-three sesquadragintaoctingentillion nine hundred twenty-three quinquaquadragintaoctingentillion three quattorquadragintaoctingentillion nine hundred eighty-five tresquadragintaoctingentillion seven hundred nineteen duoquadragintaoctingentillion three hundred seventy-four unquadragintaoctingentillion eight hundred sixty-four quadragintaoctingentillion two hundred ten noventrigintaoctingentillion seven hundred fourteen octotrigintaoctingentillion six hundred thirty-two septentrigintaoctingentillion five hundred forty-three sestrigintaoctingentillion seven hundred ninety-nine quinquatrigintaoctingentillion nine hundred ten quattortrigintaoctingentillion four hundred twenty-nine trestrigintaoctingentillion nine hundred thirty-eight duotrigintaoctingentillion five hundred twelve untrigintaoctingentillion three hundred ninety-eight trigintaoctingentillion six hundred twenty-nine novemvigintioctingentillion twenty octovigintioctingentillion five hundred ninety-two septemvigintioctingentillion forty-four sesvigintioctingentillion two hundred eight quinquavigintioctingentillion four hundred eighty-six quattorvigintioctingentillion nine hundred sixty-nine tresvigintioctingentillion four hundred four duovigintioctingentillion eight hundred unvigintioctingentillion four hundred seventy-nine vigintioctingentillion nine hundred eighty-eight novendecioctingentillion six hundred ten octodecioctingentillion one hundred ninety-seven septendecioctingentillion one hundred ninety-six sedecioctingentillion fifty-eight quinquadecioctingentillion six hundred thirty-one quattordecioctingentillion six hundred sixty-six tredecioctingentillion eight hundred seventy-two duodecioctingentillion nine hundred ninety-four undecioctingentillion eight hundred eight decioctingentillion five hundred fifty-eight novemoctingentillion nine hundred one octooctingentillion three hundred twenty-three septemoctingentillion eight hundred twenty-nine sexoctingentillion six hundred sixty-nine quinquaoctingentillion nine hundred forty-four quattoroctingentillion five hundred ninety tresoctingentillion nine hundred ninety-seven duooctingentillion four hundred twenty-four unoctingentillion five hundred four octingentillion eighty-seven novenonagintaseptigentillion seventy-three octononagintaseptigentillion seven hundred fifty-nine septenonagintaseptigentillion nine hundred eighteen senonagintaseptigentillion eight hundred twenty-three quinquanonagintaseptigentillion six hundred twenty-seven quattornonagintaseptigentillion seven hundred twenty-seven trenonagintaseptigentillion one hundred eighty-eight duononagintaseptigentillion seven hundred thirty-two unnonagintaseptigentillion five hundred nineteen nonagintaseptigentillion seven hundred seventy-nine novemoctogintaseptigentillion five hundred five octooctogintaseptigentillion nine hundred fifty septemoctogintaseptigentillion nine hundred ninety-five sexoctogintaseptigentillion two hundred seventy-six quinquaoctogintaseptigentillion one hundred twenty quattoroctogintaseptigentillion eight hundred seventy-four tresoctogintaseptigentillion nine hundred seventy-five duooctogintaseptigentillion four hundred sixty-two unoctogintaseptigentillion four hundred ninety-seven octogintaseptigentillion forty-three novenseptuagintaseptigentillion six hundred one octoseptuagintaseptigentillion four hundred eighteen septenseptuagintaseptigentillion two hundred seventy-eight seseptuagintaseptigentillion ninety-four quinquaseptuagintaseptigentillion six hundred forty-six quattorseptuagintaseptigentillion four hundred ninety-six treseptuagintaseptigentillion two hundred ninety-one duoseptuagintaseptigentillion fifty-six unseptuagintaseptigentillion three hundred ninety-three septuagintaseptigentillion eight hundred eighty-seven novensexagintaseptigentillion four hundred thirty-seven octosexagintaseptigentillion eight hundred eighty-six septensexagintaseptigentillion four hundred eighty-seven sesexagintaseptigentillion three hundred thirty-seven quinquasexagintaseptigentillion one hundred nineteen quattorsexagintaseptigentillion one hundred eighty-one tresexagintaseptigentillion forty-five duosexagintaseptigentillion eight hundred twenty-five unsexagintaseptigentillion seven hundred eighty-three sexagintaseptigentillion six hundred forty-seven novenquinquagintaseptigentillion eight hundred forty-nine octoquinquagintaseptigentillion nine hundred seventy-seven septenquinquagintaseptigentillion twelve sesquinquagintaseptigentillion four hundred seventy-six quinquaquinquagintaseptigentillion six hundred thirty-two quattorquinquagintaseptigentillion eight hundred eighty-nine tresquinquagintaseptigentillion eight hundred thirty-five duoquinquagintaseptigentillion nine hundred fifty-five unquinquagintaseptigentillion seven hundred thirty-five quinquagintaseptigentillion four hundred thirty-two novenquadragintaseptigentillion five hundred thirteen octoquadragintaseptigentillion one hundred eighty-five septenquadragintaseptigentillion three hundred twenty-three sesquadragintaseptigentillion nine hundred fifty-eight quinquaquadragintaseptigentillion four hundred sixty-three quattorquadragintaseptigentillion seventy-five tresquadragintaseptigentillion five hundred fifty-seven duoquadragintaseptigentillion four hundred nine unquadragintaseptigentillion one hundred fourteen quadragintaseptigentillion two hundred sixty-two noventrigintaseptigentillion four hundred seventeen octotrigintaseptigentillion four hundred seventy-four septentrigintaseptigentillion three hundred forty-nine sestrigintaseptigentillion three hundred forty-seven quinquatrigintaseptigentillion five hundred fifty-three quattortrigintaseptigentillion four hundred twenty-eight trestrigintaseptigentillion six hundred forty-six duotrigintaseptigentillion five hundred seventy-six untrigintaseptigentillion six hundred eleven trigintaseptigentillion six hundred sixty-seven novemvigintiseptigentillion seven hundred ninety-seven octovigintiseptigentillion three hundred ninety-six septemvigintiseptigentillion six hundred sixty-eight sesvigintiseptigentillion eight hundred twenty quinquavigintiseptigentillion two hundred ninety-one quattorvigintiseptigentillion two hundred seven tresvigintiseptigentillion three hundred seventy-nine duovigintiseptigentillion one hundred forty-three unvigintiseptigentillion eight hundred fifty-three vigintiseptigentillion seven hundred nineteen novendeciseptigentillion five hundred eighty-eight octodeciseptigentillion two hundred forty-nine septendeciseptigentillion eight hundred eight sedeciseptigentillion one hundred twenty-six quinquadeciseptigentillion eight hundred sixty-seven quattordeciseptigentillion eight hundred thirty-eight tredeciseptigentillion three hundred seventy-four duodeciseptigentillion five hundred fifty-nine undeciseptigentillion seven hundred thirty-one deciseptigentillion seven hundred forty-six novenseptigentillion one hundred thirty-six octoseptigentillion eighty-five septenseptigentillion three hundred seventy-nine seseptigentillion five hundred thirty-four quinquaseptigentillion five hundred twenty-four quattorseptigentillion two hundred twenty-one treseptigentillion five hundred eighty-six duoseptigentillion five hundred ninety-three unseptigentillion two hundred one septigentillion nine hundred twenty-eight novenonagintasescentillion ninety octononagintasescentillion eight hundred seventy-eight septenonagintasescentillion two hundred ninety-seven senonagintasescentillion three hundred eight quinquanonagintasescentillion four hundred thirty-one quattornonagintasescentillion three hundred ninety-two trenonagintasescentillion eight hundred forty-four duononagintasescentillion four hundred three unnonagintasescentillion two hundred eighty-one nonagintasescentillion two hundred thirty-one novemoctogintasescentillion five hundred fifty-eight octooctogintasescentillion six hundred eleven septemoctogintasescentillion thirty-six sexoctogintasescentillion nine hundred seventy-six quinquaoctogintasescentillion eight hundred one quattoroctogintasescentillion three hundred fifty-seven tresoctogintasescentillion three hundred four duooctogintasescentillion two hundred sixteen unoctogintasescentillion one hundred sixty-eight octogintasescentillion seven hundred forty-seven novenseptuagintasescentillion six hundred nine octoseptuagintasescentillion six hundred seventy-five septenseptuagintasescentillion eight hundred seventy-one seseptuagintasescentillion three hundred forty-eight quinquaseptuagintasescentillion three hundred twelve quattorseptuagintasescentillion twenty-five treseptuagintasescentillion four hundred seventy-eight duoseptuagintasescentillion five hundred eighty-nine unseptuagintasescentillion three hundred twenty septuagintasescentillion seven hundred sixty-seven novensexagintasescentillion one hundred sixty-nine octosexagintasescentillion one hundred thirty-two septensexagintasescentillion four hundred forty-eight sesexagintasescentillion four hundred twenty-six quinquasexagintasescentillion two hundred thirty-six quattorsexagintasescentillion one hundred thirty-one tresexagintasescentillion four hundred twelve duosexagintasescentillion five hundred eight unsexagintasescentillion seven hundred eighty sexagintasescentillion two hundred eight novenquinquagintasescentillion two hundred sixty-one septenquinquagintasescentillion six hundred eighty-three sesquinquagintasescentillion one hundred fifty-one quinquaquinquagintasescentillion twenty-seven quattorquinquagintasescentillion three hundred forty-one tresquinquagintasescentillion eight hundred twenty-seven duoquinquagintasescentillion nine hundred seventy-seven unquinquagintasescentillion seven hundred four quinquagintasescentillion seven hundred eighty-four novenquadragintasescentillion six hundred thirty-five octoquadragintasescentillion eight hundred sixty-eight septenquadragintasescentillion one hundred seventy sesquadragintasescentillion one hundred sixty-four quinquaquadragintasescentillion three hundred sixty-five quattorquadragintasescentillion twenty-four tresquadragintasescentillion one hundred fifty-three duoquadragintasescentillion six hundred ninety-one unquadragintasescentillion three hundred ninety-eight quadragintasescentillion two hundred eighty-one noventrigintasescentillion two hundred sixty-four octotrigintasescentillion eight hundred ten septentrigintasescentillion two hundred thirteen sestrigintasescentillion ninety-two quinquatrigintasescentillion seven hundred sixty-one quattortrigintasescentillion two hundred forty-four trestrigintasescentillion eight hundred ninety-six duotrigintasescentillion three hundred fifty-nine untrigintasescentillion nine hundred twenty-eight trigintasescentillion seven hundred five novemvigintisescentillion one hundred fourteen octovigintisescentillion nine hundred sixty-four septemvigintisescentillion nine hundred seventy-five sesvigintisescentillion four hundred nineteen quinquavigintisescentillion nine hundred nine quattorvigintisescentillion three hundred forty-two tresvigintisescentillion two hundred twenty-one duovigintisescentillion five hundred sixty-six unvigintisescentillion eight hundred thirty-two vigintisescentillion five hundred seventy-two novendecisescentillion eighty octodecisescentillion eight hundred twenty-one septendecisescentillion three hundred thirty-three sedecisescentillion one hundred eighty-six quinquadecisescentillion one hundred sixteen quattordecisescentillion eight hundred eleven tredecisescentillion five hundred fifty-three duodecisescentillion six hundred fifteen undecisescentillion eight hundred thirty-six decisescentillion five hundred forty-six novensescentillion nine hundred eighty-four octosescentillion forty-six septensescentillion seven hundred eight sesescentillion nine hundred seventy-five quinquasescentillion six hundred two quattorsescentillion nine hundred tresescentillion nine hundred fifty duosescentillion five hundred thirty-seven unsescentillion six hundred sixteen sescentillion four hundred seventy-five novenonagintaquingentillion eight hundred forty-seven octononagintaquingentillion seven hundred twenty-eight septenonagintaquingentillion four hundred twenty-one senonagintaquingentillion eight hundred eighty-nine quinquanonagintaquingentillion six hundred seventy-nine quattornonagintaquingentillion six hundred forty-six trenonagintaquingentillion two hundred forty-four duononagintaquingentillion nine hundred forty-five unnonagintaquingentillion one hundred sixty nonagintaquingentillion seven hundred sixty-five novemoctogintaquingentillion three hundred fifty-three octooctogintaquingentillion four hundred eight septemoctogintaquingentillion one hundred ninety-eight sexoctogintaquingentillion nine hundred one quinquaoctogintaquingentillion three hundred eighty-five quattoroctogintaquingentillion four hundred forty-two tresoctogintaquingentillion four hundred eighty-seven duooctogintaquingentillion nine hundred eighty-four unoctogintaquingentillion nine hundred fifty-nine octogintaquingentillion nine hundred fifty-three novenseptuagintaquingentillion three hundred nineteen octoseptuagintaquingentillion one hundred one septenseptuagintaquingentillion seven hundred twenty-three seseptuagintaquingentillion three hundred fifty-five quinquaseptuagintaquingentillion five hundred fifty-six quattorseptuagintaquingentillion six hundred two treseptuagintaquingentillion one hundred thirty-nine duoseptuagintaquingentillion four hundred fifty unseptuagintaquingentillion three hundred ninety-nine septuagintaquingentillion seven hundred thirty-six novensexagintaquingentillion two hundred eighty octosexagintaquingentillion seven hundred fifty septensexagintaquingentillion one hundred thirty-seven sesexagintaquingentillion eight hundred thirty-seven quinquasexagintaquingentillion six hundred fifteen quattorsexagintaquingentillion three hundred seven tresexagintaquingentillion one hundred twenty-seven duosexagintaquingentillion seven hundred sixty-one unsexagintaquingentillion nine hundred twenty-six sexagintaquingentillion eight hundred forty-nine novenquinquagintaquingentillion thirty-four octoquinquagintaquingentillion three hundred fifty-two septenquinquagintaquingentillion six hundred twenty-five sesquinquagintaquingentillion two hundred quinquaquinquagintaquingentillion fifteen quattorquinquagintaquingentillion eight hundred eighty-eight tresquinquagintaquingentillion five hundred thirty-five duoquinquagintaquingentillion one hundred forty-seven unquinquagintaquingentillion three hundred thirty-one quinquagintaquingentillion six hundred eleven novenquadragintaquingentillion seven hundred two octoquadragintaquingentillion one hundred three septenquadragintaquingentillion nine hundred sixty-eight sesquadragintaquingentillion one hundred seventy-five quinquaquadragintaquingentillion nine hundred twenty-one quattorquadragintaquingentillion five hundred ten tresquadragintaquingentillion nine hundred seven duoquadragintaquingentillion seven hundred eighty-eight unquadragintaquingentillion nineteen quadragintaquingentillion three hundred ninety-three noventrigintaquingentillion one hundred seventy-eight octotrigintaquingentillion one hundred fourteen septentrigintaquingentillion one hundred ninety-four sestrigintaquingentillion five hundred forty-five quinquatrigintaquingentillion two hundred fifty-seven quattortrigintaquingentillion two hundred twenty-three trestrigintaquingentillion eight hundred sixty-five duotrigintaquingentillion five hundred forty-one untrigintaquingentillion four hundred sixty-one trigintaquingentillion sixty-two novemvigintiquingentillion eight hundred ninety-two octovigintiquingentillion one hundred eighty-seven septemvigintiquingentillion nine hundred sixty sesvigintiquingentillion two hundred twenty-three quinquavigintiquingentillion eight hundred thirty-eight quattorvigintiquingentillion nine hundred seventy-one tresvigintiquingentillion four hundred seventy-six duovigintiquingentillion eighty-eight unvigintiquingentillion five hundred six vigintiquingentillion two hundred seventy-six novendeciquingentillion eight hundred sixty-two octodeciquingentillion nine hundred sixty-seven septendeciquingentillion one hundred forty-six sedeciquingentillion six hundred seventy-four quinquadeciquingentillion six hundred ninety-seven quattordeciquingentillion five hundred sixty-two tredeciquingentillion nine hundred eleven duodeciquingentillion two hundred thirty-four undeciquingentillion eighty-two deciquingentillion four hundred thirty-nine novenquingentillion two hundred eight octoquingentillion one hundred sixty septenquingentillion one hundred fifty-three sesquingentillion seven hundred eighty quinquaquingentillion eight hundred eighty-nine quattorquingentillion eight hundred ninety-three tresquingentillion nine hundred sixty-four duoquingentillion five hundred eighteen unquingentillion two hundred sixty-three quingentillion two hundred forty-three novenonagintaquadringentillion six hundred seventy-one octononagintaquadringentillion six hundred sixteen septenonagintaquadringentillion seven hundred sixty-two senonagintaquadringentillion one hundred seventy-nine quinquanonagintaquadringentillion one hundred sixty-eight quattornonagintaquadringentillion nine hundred nine trenonagintaquadringentillion seven hundred seventy-nine duononagintaquadringentillion nine hundred eleven unnonagintaquadringentillion nine hundred three nonagintaquadringentillion seven hundred fifty-four novemoctogintaquadringentillion thirty-one octooctogintaquadringentillion two hundred seventy-four septemoctogintaquadringentillion six hundred twenty-two sexoctogintaquadringentillion two hundred eighty-nine quinquaoctogintaquadringentillion nine hundred eighty-eight quattoroctogintaquadringentillion five tresoctogintaquadringentillion one hundred ninety-five duooctogintaquadringentillion four hundred forty-four unoctogintaquadringentillion four hundred fourteen octogintaquadringentillion two hundred eighty-two novenseptuagintaquadringentillion twelve octoseptuagintaquadringentillion one hundred eighty-seven septenseptuagintaquadringentillion three hundred sixty-one seseptuagintaquadringentillion seven hundred forty-five quinquaseptuagintaquadringentillion nine hundred ninety-two quattorseptuagintaquadringentillion six hundred forty-two treseptuagintaquadringentillion nine hundred fifty-six duoseptuagintaquadringentillion five hundred eighty-one unseptuagintaquadringentillion seven hundred forty-six septuagintaquadringentillion six hundred twenty-eight novensexagintaquadringentillion three hundred two octosexagintaquadringentillion nine hundred fifty-five septensexagintaquadringentillion five hundred seventy sesexagintaquadringentillion two hundred ninety-nine quinquasexagintaquadringentillion twenty-four quattorsexagintaquadringentillion three hundred twenty-four tresexagintaquadringentillion one hundred fifty-three duosexagintaquadringentillion one hundred eighty-one unsexagintaquadringentillion six hundred seventeen sexagintaquadringentillion two hundred ten novenquinquagintaquadringentillion four hundred sixty-five octoquinquagintaquadringentillion eight hundred thirty-two septenquinquagintaquadringentillion thirty-six sesquinquagintaquadringentillion seven hundred eighty-six quinquaquinquagintaquadringentillion nine hundred six quattorquinquagintaquadringentillion one hundred seventeen tresquinquagintaquadringentillion two hundred sixty duoquinquagintaquadringentillion one hundred fifty-eight unquinquagintaquadringentillion seven hundred eighty-three quinquagintaquadringentillion five hundred twenty novenquadragintaquadringentillion seven hundred fifty-one octoquadragintaquadringentillion five hundred sixteen septenquadragintaquadringentillion two hundred eighty-four sesquadragintaquadringentillion two hundred twenty-five quinquaquadragintaquadringentillion five hundred forty quattorquadragintaquadringentillion two hundred sixty-five tresquadragintaquadringentillion one hundred seventy duoquadragintaquadringentillion four hundred eighty-three unquadragintaquadringentillion three hundred four quadragintaquadringentillion two hundred twenty-six noventrigintaquadringentillion one hundred forty-three octotrigintaquadringentillion nine hundred seventy-four septentrigintaquadringentillion two hundred eighty-six sestrigintaquadringentillion nine hundred thirty-three quinquatrigintaquadringentillion sixty-one quattortrigintaquadringentillion six hundred ninety trestrigintaquadringentillion eight hundred ninety-seven duotrigintaquadringentillion nine hundred sixty-eight untrigintaquadringentillion four hundred eighty-two trigintaquadringentillion five hundred ninety novemvigintiquadringentillion one hundred twenty-five octovigintiquadringentillion four hundred fifty-eight septemvigintiquadringentillion three hundred twenty-seven sesvigintiquadringentillion one hundred sixty-eight quinquavigintiquadringentillion two hundred twenty-six quattorvigintiquadringentillion four hundred fifty-eight tresvigintiquadringentillion sixty-six duovigintiquadringentillion five hundred twenty-six unvigintiquadringentillion seven hundred sixty-nine vigintiquadringentillion nine hundred fifty-eight novendeciquadringentillion six hundred fifty-two octodeciquadringentillion six hundred eighty-two septendeciquadringentillion two hundred seventy-two sedeciquadringentillion eight hundred seven quinquadeciquadringentillion seventy-five quattordeciquadringentillion seven hundred eighty-one tredeciquadringentillion three hundred ninety-one duodeciquadringentillion eight hundred fifty-eight undeciquadringentillion one hundred seventy-eight deciquadringentillion eight hundred eighty-nine novenquadringentillion six hundred fifty-two octoquadringentillion two hundred eight septenquadringentillion one hundred sixty-four sesquadringentillion three hundred forty-eight quinquaquadringentillion three hundred forty-four quattorquadringentillion eight hundred twenty-five tresquadringentillion nine hundred ninety-three duoquadringentillion two hundred sixty-six unquadringentillion forty-three quadringentillion three hundred sixty-seven novenonagintatrecentillion six hundred sixty octononagintatrecentillion one hundred seventy-six septenonagintatrecentillion nine hundred ninety-nine senonagintatrecentillion six hundred twelve quinquanonagintatrecentillion eight hundred thirty-one quattornonagintatrecentillion eight hundred sixty trenonagintatrecentillion seven hundred eighty-eight duononagintatrecentillion three hundred eighty-six unnonagintatrecentillion one hundred fifty nonagintatrecentillion two hundred seventy-nine novemoctogintatrecentillion four hundred sixty-five octooctogintatrecentillion nine hundred fifty-five septemoctogintatrecentillion one hundred thirty-one sexoctogintatrecentillion one hundred fifty-six quinquaoctogintatrecentillion five hundred fifty-two quattoroctogintatrecentillion thirty-six tresoctogintatrecentillion ninety-three duooctogintatrecentillion nine hundred eighty-eight unoctogintatrecentillion one hundred eighty octogintatrecentillion six hundred twelve novenseptuagintatrecentillion one hundred thirty-eight octoseptuagintatrecentillion five hundred fifty-eight septenseptuagintatrecentillion six hundred seseptuagintatrecentillion three hundred one quinquaseptuagintatrecentillion four hundred thirty-five quattorseptuagintatrecentillion six hundred ninety-four treseptuagintatrecentillion five hundred twenty-seven duoseptuagintatrecentillion two hundred twenty-four unseptuagintatrecentillion two hundred six septuagintatrecentillion three hundred forty-four novensexagintatrecentillion six hundred thirty-one octosexagintatrecentillion seven hundred ninety-seven septensexagintatrecentillion four hundred sixty sesexagintatrecentillion five hundred ninety-four quinquasexagintatrecentillion six hundred eighty-two quattorsexagintatrecentillion five hundred seventy-three tresexagintatrecentillion one hundred three duosexagintatrecentillion seven hundred ninety unsexagintatrecentillion eighty-four sexagintatrecentillion twenty-four novenquinquagintatrecentillion four hundred thirty-two octoquinquagintatrecentillion four hundred thirty-eight septenquinquagintatrecentillion four hundred sixty-five sesquinquagintatrecentillion six hundred fifty-seven quinquaquinquagintatrecentillion two hundred forty-five quattorquinquagintatrecentillion fourteen tresquinquagintatrecentillion four hundred two duoquinquagintatrecentillion eight hundred twenty-one unquinquagintatrecentillion eight hundred eighty-five quinquagintatrecentillion two hundred fifty-two novenquadragintatrecentillion four hundred seventy octoquadragintatrecentillion nine hundred thirty-five septenquadragintatrecentillion one hundred ninety sesquadragintatrecentillion six hundred twenty quinquaquadragintatrecentillion nine hundred twenty-nine quattorquadragintatrecentillion twenty-three tresquadragintatrecentillion one hundred thirty-six duoquadragintatrecentillion four hundred ninety-three unquadragintatrecentillion two hundred seventy-three quadragintatrecentillion four hundred ninety-seven noventrigintatrecentillion five hundred sixty-five octotrigintatrecentillion five hundred thirteen septentrigintatrecentillion nine hundred fifty-eight sestrigintatrecentillion seven hundred twenty quinquatrigintatrecentillion five hundred fifty-nine quattortrigintatrecentillion six hundred fifty-four trestrigintatrecentillion two hundred twenty-eight duotrigintatrecentillion seven hundred forty-nine untrigintatrecentillion seven hundred seventy-four trigintatrecentillion eleven novemvigintitrecentillion four hundred thirteen octovigintitrecentillion three hundred forty-six septemvigintitrecentillion nine hundred sixty-two sesvigintitrecentillion seven hundred fifteen quinquavigintitrecentillion four hundred twenty-two quattorvigintitrecentillion eight hundred forty-five tresvigintitrecentillion eight hundred sixty-two duovigintitrecentillion three hundred seventy-seven unvigintitrecentillion three hundred eighty-seven vigintitrecentillion five hundred thirty-eight novendecitrecentillion two hundred thirty octodecitrecentillion four hundred eighty-three septendecitrecentillion eight hundred sixty-five sedecitrecentillion six hundred eighty-eight quinquadecitrecentillion nine hundred seventy-six quattordecitrecentillion four hundred sixty-one tredecitrecentillion nine hundred twenty-seven duodecitrecentillion three hundred eighty-three undecitrecentillion eight hundred fourteen decitrecentillion nine hundred noventrecentillion one hundred forty octotrecentillion seven hundred sixty-seven septentrecentillion three hundred ten sestrecentillion four hundred forty-six quinquatrecentillion six hundred forty quattortrecentillion two hundred fifty-nine trestrecentillion eight hundred ninety-nine duotrecentillion four hundred ninety untrecentillion two hundred twenty-two trecentillion two hundred twenty-one novenonagintaducentillion seven hundred sixty-five octononagintaducentillion nine hundred four septenonagintaducentillion three hundred thirty-nine senonagintaducentillion nine hundred one quinquanonagintaducentillion eight hundred eighty-six quattornonagintaducentillion eighteen trenonagintaducentillion five hundred sixty-six duononagintaducentillion five hundred twenty-six unnonagintaducentillion four hundred eighty-five nonagintaducentillion sixty-one novemoctogintaducentillion seven hundred ninety-nine octooctogintaducentillion seven hundred two septemoctogintaducentillion three hundred fifty-six sexoctogintaducentillion one hundred ninety-three quinquaoctogintaducentillion eight hundred ninety-seven quattoroctogintaducentillion seventeen tresoctogintaducentillion eight hundred sixty duooctogintaducentillion forty unoctogintaducentillion eight hundred eleven octogintaducentillion eight hundred eighty-nine novenseptuagintaducentillion seven hundred twenty-nine octoseptuagintaducentillion nine hundred eighteen septenseptuagintaducentillion three hundred eleven seseptuagintaducentillion twenty-one quinquaseptuagintaducentillion one hundred seventy-one quattorseptuagintaducentillion two hundred twenty-nine treseptuagintaducentillion eight hundred forty-five duoseptuagintaducentillion nine hundred one unseptuagintaducentillion six hundred forty-one septuagintaducentillion nine hundred twenty-one novensexagintaducentillion sixty-eight octosexagintaducentillion eight hundred eighty-four septensexagintaducentillion three hundred eighty-seven sesexagintaducentillion one hundred twenty-one quinquasexagintaducentillion eight hundred fifty-five quattorsexagintaducentillion six hundred forty-six tresexagintaducentillion one hundred twenty-four duosexagintaducentillion nine hundred sixty unsexagintaducentillion seven hundred ninety-eight sexagintaducentillion seven hundred twenty-two novenquinquagintaducentillion nine hundred eight octoquinquagintaducentillion five hundred nineteen septenquinquagintaducentillion two hundred ninety-six sesquinquagintaducentillion eight hundred nineteen quinquaquinquagintaducentillion three hundred seventy-two quattorquinquagintaducentillion three hundred eighty-eight tresquinquagintaducentillion six hundred forty-two duoquinquagintaducentillion six hundred fourteen unquinquagintaducentillion eight hundred thirty-nine quinquagintaducentillion six hundred fifty-seven novenquadragintaducentillion three hundred eighty-two octoquadragintaducentillion two hundred ninety-one septenquadragintaducentillion one hundred twenty-three sesquadragintaducentillion one hundred twenty-five quinquaquadragintaducentillion twenty-four quattorquadragintaducentillion one hundred eighty-six tresquadragintaducentillion six hundred forty-nine duoquadragintaducentillion three hundred fifty-three unquadragintaducentillion one hundred forty-three quadragintaducentillion nine hundred seventy noventrigintaducentillion one hundred thirty-seven octotrigintaducentillion four hundred twenty-eight septentrigintaducentillion five hundred thirty-one sestrigintaducentillion nine hundred twenty-six quinquatrigintaducentillion six hundred forty-nine quattortrigintaducentillion eight hundred seventy-five trestrigintaducentillion three hundred thirty-seven duotrigintaducentillion two hundred eighteen untrigintaducentillion nine hundred forty trigintaducentillion six hundred ninety-four novemvigintiducentillion two hundred eighty-one octovigintiducentillion four hundred thirty-four septemvigintiducentillion one hundred eighteen sesvigintiducentillion five hundred twenty quinquavigintiducentillion one hundred fifty-eight quattorvigintiducentillion fourteen tresvigintiducentillion one hundred twenty-three duovigintiducentillion three hundred forty-four unvigintiducentillion eight hundred twenty-eight vigintiducentillion fifteen novendeciducentillion fifty-one octodeciducentillion three hundred ninety-nine septendeciducentillion six hundred ninety-four sedeciducentillion two hundred ninety quinquadeciducentillion one hundred fifty-three quattordeciducentillion four hundred eighty-three tredeciducentillion seventy-seven duodeciducentillion six hundred forty-four undeciducentillion five hundred sixty-nine deciducentillion ninety-nine novenducentillion seventy-three octoducentillion one hundred fifty-two septenducentillion four hundred thirty-three seducentillion two hundred seventy-eight quinquaducentillion two hundred eighty-eight quattorducentillion two hundred sixty-nine treducentillion eight hundred sixty-four duoducentillion six hundred two unducentillion seven hundred eighty-nine ducentillion eight hundred sixty-four novenonagintacentillion three hundred twenty-one octononagintacentillion one hundred thirty-nine septenonagintacentillion eighty-three senonagintacentillion five hundred six quinquanonagintacentillion two hundred seventeen quattornonagintacentillion ninety-five trenonagintacentillion two duononagintacentillion five hundred ninety-seven unnonagintacentillion three hundred eighty-nine nonagintacentillion eight hundred sixty-three novemoctogintacentillion five hundred fifty-four octooctogintacentillion two hundred seventy-seven septemoctogintacentillion one hundred ninety-six sexoctogintacentillion seven hundred forty-two quinquaoctogintacentillion eight hundred twenty-two quattoroctogintacentillion two hundred forty-eight tresoctogintacentillion seven hundred fifty-seven duooctogintacentillion five hundred eighty-six unoctogintacentillion seven hundred sixty-five octogintacentillion seven hundred fifty-two novenseptuagintacentillion three hundred forty-four octoseptuagintacentillion two hundred twenty septenseptuagintacentillion two hundred seven seseptuagintacentillion five hundred seventy-three quinquaseptuagintacentillion six hundred thirty quattorseptuagintacentillion five hundred sixty-nine treseptuagintacentillion four hundred ninety-eight duoseptuagintacentillion eight hundred twenty-five unseptuagintacentillion eighty-seven septuagintacentillion nine hundred sixty-eight novensexagintacentillion nine hundred twenty-eight octosexagintacentillion one hundred sixty-two septensexagintacentillion seven hundred fifty-three sesexagintacentillion eight hundred forty-eight quinquasexagintacentillion eight hundred sixty-three quattorsexagintacentillion three hundred ninety-six tresexagintacentillion nine hundred nine duosexagintacentillion nine hundred fifty-nine unsexagintacentillion eight hundred twenty-six sexagintacentillion two hundred eighty novenquinquagintacentillion nine hundred fifty-six octoquinquagintacentillion one hundred twenty-one septenquinquagintacentillion four hundred fifty sesquinquagintacentillion nine hundred ninety-four quinquaquinquagintacentillion eight hundred seventy-one quattorquinquagintacentillion seven hundred one tresquinquagintacentillion two hundred forty-four duoquinquagintacentillion five hundred sixteen unquinquagintacentillion four hundred sixty-one quinquagintacentillion two hundred sixty novenquadragintacentillion three hundred seventy-nine octoquadragintacentillion twenty-nine septenquadragintacentillion three hundred nine sesquadragintacentillion one hundred twenty quinquaquadragintacentillion eight hundred eighty-nine quattorquadragintacentillion eighty-six tresquadragintacentillion nine hundred forty-two duoquadragintacentillion twenty-eight unquadragintacentillion five hundred ten quadragintacentillion six hundred forty noventrigintacentillion one hundred eighty-two octotrigintacentillion one hundred fifty-four septentrigintacentillion three hundred ninety-nine sestrigintacentillion four hundred fifty-seven quinquatrigintacentillion one hundred fifty-six quattortrigintacentillion eight hundred five trestrigintacentillion nine hundred forty-one duotrigintacentillion eight hundred seventy-two untrigintacentillion seven hundred forty-eight trigintacentillion nine hundred ninety-eight novemviginticentillion ninety-four octoviginticentillion two hundred fifty-four septemviginticentillion seven hundred forty-two sesviginticentillion one hundred seventy-three quinquaviginticentillion five hundred eighty-two quattorviginticentillion four hundred one tresviginticentillion sixty-three duoviginticentillion six hundred seventy-seven unviginticentillion four hundred four viginticentillion five hundred ninety-five novendecicentillion seven hundred forty-one octodecicentillion seven hundred eighty-five septendecicentillion one hundred sixty sedecicentillion eight hundred twenty-nine quinquadecicentillion two hundred thirty quattordecicentillion one hundred thirty-five tredecicentillion three hundred fifty-eight duodecicentillion eighty-one undecicentillion eight hundred forty decicentillion ninety-six novencentillion nine hundred ninety-six octocentillion three hundred seventy-two septencentillion five hundred twenty-four sexcentillion two hundred thirty quinquacentillion five hundred sixty quattorcentillion eight hundred fifty-five trescentillion nine hundred three duocentillion seven hundred uncentillion six hundred twenty-four centillion two hundred seventy-one novenonagintillion two hundred forty-three octononagintillion four hundred sixteen septenonagintillion nine hundred nine senonagintillion four quinquanonagintillion one hundred fifty-three quattornonagintillion six hundred ninety trenonagintillion one hundred five duononagintillion nine hundred thirty-three unnonagintillion nine hundred eighty-three nonagintillion eight hundred thirty-five novemoctogintillion seven hundred seventy-seven octooctogintillion nine hundred thirty-nine septemoctogintillion four hundred ten sexoctogintillion nine hundred seventy quinquaoctogintillion twenty-seven quattoroctogintillion seven hundred fifty-three tresoctogintillion four hundred seventy-two duooctogintillion `

## External links

  * [Official site](<http://benibela.de/sources_en.html#internettools>)
  * [CGI example to test the X/HTML processing (i.e. the XQuery expressions)](<http://www.videlibri.de/cgi-bin/xidelcgi>)

---

_Source: [https://wiki.freepascal.org/Internet_Tools](https://web.archive.org/web/20240304153621/https://wiki.freepascal.org/Internet_Tools)_
