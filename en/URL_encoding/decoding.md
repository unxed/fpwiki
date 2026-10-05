# URL encoding/decoding

**Question:** I need to pass some arbitrary text as a query string to a URL that will be opened in the default browser via OpenURL. Obviously, the query string will need to be URL-encoded - is there a standard function somewhere for this? 

**Answer-1:** You have httpprotocol.HTTPEncode and httpprotocol.HTTPDecode. 
    
    
    uses
      httpprotocol;
    var
      A, B: string;
    begin
      A := 'http://www.somesite.com/page1.asp?id=65';
      B := HTTPEncode(A);
      A := HTTPDecode(B);
    

**Answer-2:** Here are ready functions, written by forum member BenJones. 
    
    
    function EncodeUrl(const url: string): string;
    var
      x: integer;
      sBuff: string;
    const
      SafeMask = ['A'..'Z', '0'..'9', 'a'..'z', '*', '@', '.', '_', '-'];
    begin
      //Init
      sBuff := '';
    
      for x := 1 to Length(url) do
      begin
        //Check if we have a safe char
        if url[x] in SafeMask then
        begin
          //Append all other chars
          sBuff := sBuff + url[x];
        end
        else if url[x] = ' ' then
        begin
          //Append space
          sBuff := sBuff + '+';
        end
        else
        begin
          //Convert to hex
          sBuff := sBuff + '%' + IntToHex(Ord(url[x]), 2);
        end;
      end;
    
      Result := sBuff;
    end;
    
    function DecodeUrl(const url: string): string;
    var
      x: integer;
      ch: string;
      sVal: string;
      Buff: string;
    begin
      //Init
      Buff := '';
      x := 1;
      while x <= Length(url) do
      begin
        //Get single char
        ch := url[x];
    
        if ch = '+' then
        begin
          //Append space
          Buff := Buff + ' ';
        end
        else if ch <> '%' then
        begin
          //Append other chars
          Buff := Buff + ch;
        end
        else
        begin
          //Get value
          sVal := Copy(url, x + 1, 2);
          //Convert sval to int then to char
          Buff := Buff + char(StrToInt('$' + sVal));
          //Inc counter by 2
          Inc(x, 2);
        end;
        //Inc counter
        Inc(x);
      end;
      //Return result
      Result := Buff;
    end;
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      A, B: string;
    begin
      A := 'http://www.somesite.com/page1.asp?id=65';
      //Encode url
      B := EncodeUrl(A);
      //Show Encoded url
      ShowMessage(B);
      //Show deoded url
      ShowMessage(DecodeUrl(B));
    end;

---

_Source: [https://wiki.freepascal.org/URL_encoding/decoding](https://web.archive.org/web/20250326005427/https://wiki.freepascal.org/URL_encoding/decoding)_
