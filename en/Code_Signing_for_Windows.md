# Code Signing for Windows

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │ 

  
****

## Contents

  * 1 Description of the problem
  * 2 Examples of certificate companies
  * 3 Signtool: code signing tool
  * 4 See also
  * 5 External links



## Description of the problem

**Question:**

I notice Windows 10 gives me a warning that the publisher is unknown after unzipping and attempting to run an executable. 

Has anyone ever "registered" and if so what authority did they register with, what was the experience like (slow/fast), cost etc? 

**Answer** from forum member Dmitry "skalogryz" Boyarintsev: 

In order to have the application launch without any "questions", you'll need to purchase an EV certificate. It costs up to $US 500 (prices vary, but I doubt you can find anything below $US 250). The approval might take about a week, since they will do the verification of your actual existence (the existence of your company). If they are prompt enough they might get you verified in a matter of a day or two. For me it took about three weeks. 

Note that EV certificates are usually "hardware" generated. Meaning you'll have some sort of hardware device in order to sign an executable. The hardware device also needs to be mailed to you... which adds the time to the point when you can finally sign an executable. 

You can get a simple certificate, but it will still show "Running application by ... Name of your company". Simple certificates are cheaper, about $100. 

Keep in mind that certificates expire and must be renewed - usually for the same price, or a bit more expensive if you used some promo when buying the first certificate. The renewal process is as fast as simply paying for it, but if you miss the renewal date, you might have to pass the approval process again. 

You can't use your HTTPS web site certificate. Your HTTPS certificate was given for a domain name, not an executable. However, the same authority that issued your HTTPS certificate might also be providing code signing certificates (and you might be eligible for a discount of some sort). 

You also cannot use a developer certificate issued by Apple for [code signing macOS applications](<Code_Signing_for_macOS.md> "Code Signing for macOS"). 

## Examples of certificate companies

  * [SSL2BUY](<https://www.ssl2buy.com/code-signing-certificate>). "OV Code Signing Certificate". $60 per year.
  * [Comodo EV](<https://comodosslstore.com/code-signing/comodo-ev-code-signing-certificate?gclid=CjwKCAjwndCKBhAkEiwAgSDKQfX5LQgnCeJ6VF95YAZ0FDIVSmosUBtDvhVyrqX5STHNvyJNk0JMuxoCEb8QAvD_BwE>). $US 399 per year for EV certificate, without a promotion.
  * [Digicert](<https://www.digicert.com/order/order-1.php>). $US 699 per year for EV certificate.
  * [KSoftware](<https://www.ksoftware.net/code-signing-certificates/>). "OV Code Signing Certificate". 80 Euros per year.
  * [Certera](<https://certera.com/code-signing>) Authentic Code Signing Certificates starting at just $199.99/year
  * [CheapSSLWeb](<https://cheapsslweb.com/code-signing>) Just $49.99/yr for OV Code Signing Certificate and just $199.99/yr for an EV Code Signing Certificate.



## Signtool: code signing tool

Signtool comes as a part of Windows 10 SDK. The binary is typically installed at: 
    
    
    C:\Program Files (x86)\Windows Kits\10\bin\__version__\x64
    

1\. Install (or generate the certificate) into Windows Certificate Center. EV certificates should also be installed, but signing them requires the hardware key to be present at the time of signing 

2\. Very basic sign command line: 
    
    
    signtool sign project1.exe
    

## See also

  * [Code signing](<Code_signing.md> "Code signing")



## External links

  * [Microsoft: official documentation for signtool](<https://docs.microsoft.com/en-us/windows/win32/seccrypto/signtool>)
  * [Microsoft: Windows 10 SDK download page](<https://developer.microsoft.com/en-us/windows/downloads/windows-10-sdk/>)

---

_Source: [https://wiki.freepascal.org/Code_Signing_for_Windows](https://web.archive.org/web/20240913203158/https://wiki.freepascal.org/Code_Signing_for_Windows)_
