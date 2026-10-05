# JSON

│ **English (en)** │  **[русский (ru)](<../ru/JSON.md>)** │

## Contents

  * 1 Overview
  * 2 JSON Object
  * 3 Valid JSON format
  * 4 Implementations
    * 4.1 The official JSON Specification syntax
    * 4.2 The Javascript syntax
  * 5 See also
  * 6 External links



## Overview

[JSON](<http://www.json.org>) (JavaScript Object Notation) is a form of data representation that provides a standardised data exchange format using simple text. As the name suggests, it is based on a subset of the [JavaScript](<JavaScript.md> "JavaScript") programming language; however, it is completely language-agnostic. Besides being easy for humans to read and write, it is also easy for machines to parse and generate. 

Compared to [XML](<XML.md> "XML"), it is more human-readable. 

## JSON Object

A JSON Object is nothing more than a collection of comma-separated name/value pairs (often called members) enclosed in curly brackets: 
    
    
    {"name1":value1, "name2":value2 ...}
    

To make it easier for humans to read and visualize, the name/value pairs are often listed vertically: 
    
    
    {
    	"name1": value1,
    	"name2": value2
    }
    

The name is supposed to be a string in double quotes, while the value can be any of the following: 

  * a simple string, numeric, boolean or null value (strings are enclosed in double quotes):  
`{"id":1, "name":"John Doe", "married":false}`


  * an array; which is a collection of comma-separated values in square brackets:  
`{"primeNumbers":[2, 3, 5, 7, 11, 13, 17], "oddNumbers":[1,3,5,7]}`


  * an object; which is a collection of name:value pairs enclosed in curly brackets:  
`{"address":{"street":"145 Koinange Street", "City":"Nairobi", "Country":"Kenya"}}`



  
JSON objects can always be nested arbitrarily to create even more complex objects: 
    
    
    {"user":
    	{	"userid": 1900,
    		"username": "jsmith",
    		"password": "secret",
    		"groups": [ "admins", "users", "maintainers"]
    	}
    }
    

Check this [link](<fcl-json.md> "fcl-json") for extracting the information when you have several users. 

## Valid JSON format

There are a few rules and guidelines that define the JSON syntax (see RFC 4627): 

  * JSON objects are encapsulated within opening and closing brackets `{ }`. An empty object can be represented by `{ }`
  * Arrays are encapsulated within opening and closing square brackets `[ ]`. An empty array can be represented by `[ ]`
  * A member is represented by a key-value pair
  * The key of a member should be contained in double quotes
  * Each member SHOULD have a unique key within an object structure
  * The value of a member must be contained in double quotes if it is a string
  * Boolean values are represented using the **true** or **false** literals in lower case
  * Number values are represented using double-precision floating-point format; Scientific notation is supported; Numbers should not have leading zeroes
  * "Offensive" characters in a string need to be escaped using the backslash character
  * Null values are represented by the **null** literal in lower case
  * Other object types, such as dates, are not natively supported and should be converted to strings; to be managed by the parser/client
  * Each member of an object or each array value must be followed by a comma if it is not the last one
  * The common extension for json files is **.json**
  * The MIME type for json files is **application/json**



## Implementations

JSON implementation is not very strict, and a lot of leeway is granted to the client application and/or parser to enforce the guidelines. As usual when writing code: 

  * be liberal in what you accept
  * be strict (adhering to the standard) in what you send.



There are two main implementations of JSON: 

### The official JSON Specification syntax

This implementation adheres strictly to the RFC 4627 guidelines above and does not allow deviation from the specification. 

### The Javascript syntax

This implementation follows the implementation of the Javascript programming language and as such allows for a few deviations from the official specification. For example: 

  1. allows un-quoted keys eg ` {name: "John Doe" }`
  2. allows single quotes for keys and/or string values; and a liberal mix of single and double quotes eg `{'name': "John Doe", "language":'Pascal'}`
  3. allows trailing comma after the last member of an array and/or object eg `{"keywords":["if","begin","for",], "IDEs":["Lazarus","fpIDE","MSEide"],}`



## See also

  * [fcl-json](<fcl-json.md> "fcl-json") package that implements JSON for Free Pascal and Lazarus
  * [Streaming JSON](<Streaming_JSON.md> "Streaming JSON") \- (English translation from original German wiki page)



## External links

  * [Official JSON website](<http://www.json.org>)
  * [JSON Formatter](<https://jsonformatter.org>)




---

_Source: [https://wiki.freepascal.org/JSON](https://web.archive.org/web/20241209224033/https://wiki.freepascal.org/JSON)_
