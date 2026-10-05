# Synapse - Email Examples

## Contents

  * 1 Example 1 - sending to Gmail
  * 2 Example 2 - sending attachments
  * 3 Example 3 - using the XMailer wrapper
  * 4 Example 4 - with error checking for debugging
  * 5 See also



## Example 1 - sending to Gmail

(forum user **ludob**). This works for me with Gmail using [Synapse](<Synapse.md> "Synapse"). 
    
    
    uses
      ..., smtpsend, ssl_openssl;
    
    function SendMail(const User, Password, MailFrom, MailTo, SMTPHost, SMTPPort, MailData: string): Boolean;
    var
      SMTP: TSMTPSend;
      sl:TStringList;
    begin
      Result:=False;
      SMTP:=TSMTPSend.Create;
      sl:=TStringList.Create;
      try
        sl.text:=Maildata;
        SMTP.UserName:=User;
        SMTP.Password:=Password;
        SMTP.TargetHost:=SMTPHost;
        SMTP.TargetPort:=SMTPPort;
        SMTP.AutoTLS:=true;
        if SMTPPort<> '25' then
          SMTP.FullSSL:=true;
        if SMTP.Login then
        begin
          Result:=SMTP.MailFrom(MailFrom, Length(MailData)) and
             SMTP.MailTo(MailTo) and
             SMTP.MailData(sl);
          SMTP.Logout;
        end;
      finally
        SMTP.Free;
        sl.Free;
      end;
    end;
    

## Example 2 - sending attachments

(forum user **y.ivanov**). Here is my small helper unit for [Synapse](<Synapse.md> "Synapse"). The password is the sender password (for SMTP). 
    
    
    unit mailsendu;
     
    {$mode objfpc}{$H+}
     
    interface
     
    uses
      Classes, SysUtils, smtpsend;
     
    type
     
      { TMySMTPSend }
     
      TMySMTPSend = class(TSMTPSend)
      private
        FSendSize: Integer;
      protected
        function SendToRaw(const AFrom, ATo: String; const AMailData: TStrings)
          : Boolean;
      public
        function SendMessage(AFrom, ATo, ASubject: String; AContent,
          AAttachments: TStrings): Boolean;
     
        property SendSize: Integer read FSendSize write FSendSize;
     
      end;
     
    implementation
     
    uses
      ssl_openssl, mimemess, mimepart, synautil, synachar;
     
    { TMySMTPSend }
     
    function TMySMTPSend.SendToRaw(const AFrom, ATo: String; const AMailData
      : TStrings): Boolean;
    var
      S, T: String;
    begin
      Result := False;
      if Self.Login then
      begin
        FSendSize := Length(AMailData.Text);
        if Self.MailFrom(GetEmailAddr(AFrom), FSendSize) then
        begin
          S := ATo;
          repeat
            T := GetEmailAddr(Trim(FetchEx(S, ',', '"')));
            if T <> '' then
              Result := Self.MailTo(T);
            if not Result then
              Break;
          until S = '';
          if Result then
            Result := Self.MailData(AMailData);
        end;
        Self.Logout;
      end;
    end;
     
    function TMySMTPSend.SendMessage(AFrom, ATo, ASubject: String; AContent,
      AAttachments: TStrings): Boolean;
    var
      Mime: TMimeMess;
      P: TMimePart;
      I: Integer;
    begin
      Mime := TMimeMess.Create;
      try
        // Set some headers
        Mime.Header.CharsetCode := UTF_8;
        Mime.Header.ToList.Text := ATo;
        Mime.Header.Subject := ASubject;
        Mime.Header.From := AFrom;
     
        // Create a MultiPart part
        P := Mime.AddPartMultipart('mixed', Nil);
     
        // Add as first part the mail text
        Mime.AddPartTextEx(AContent, P, UTF_8, True, ME_8BIT);
     
        // Add all attachments:
        if Assigned(AAttachments) then
          for I := 0 to Pred(AAttachments.Count) do
            Mime.AddPartBinaryFromFile(AAttachments[I], P);
     
        // Compose message
        Mime.EncodeMessage;
     
        // Send using SendToRaw
        Result := Self.SendToRaw(AFrom, ATo, Mime.Lines);
     
      finally
        Mime.Free;
      end;
    end;
     
    end.
    

This is how to use that unit: 
    
    
    program mailsend_test;
     
    uses
      Classes, SysUtils, mailsendu, blcksock;
     
    type
     
      { TSink }
     
      TSink = class(TObject)
        procedure Progress(Sender: TObject; Reason: THookSocketReason;const Value: String);
      end;
     
    var
      Content, Attach: TStringList;
      SMTP: TMySMTPSend;
      Sink: TSink;
      Written: Integer;
     
    { TSink }
     
    procedure TSink.Progress(Sender: TObject; Reason: THookSocketReason;
      const Value: String);
    begin
      case Reason  of
        {:Socket connected to IP and Port. Connected IP and Port is in parameter in
         format like: 'localhost.somewhere.com:25'.}
        HR_Connect: Written := 0;
        {:report count of bytes writed to socket. Number is in parameter string. If
         you need is in integer, you must use StrToInt function!}
        HR_WriteCount:
          begin
            Written := Written + StrToInt(Value);
            WriteLn('Written ', Written, ' of ', SMTP.SendSize, ' bytes');
          end;
        {:report situation where communication error occured. When raiseexcept is
         @true, then exception is called after this Hook reason.}
        HR_Error: WriteLn('Error: ', Value);
      end;
    end;
     
    begin
      Sink := TSink.Create;
      Content := TStringList.Create;
      Content.Add('Hello!');
      Content.Add('This is a SMTP send test.');
      Content.Add('Hello from the other side!');
      Content.Add('Regards,');
      Attach := TStringList.Create;
      Attach.Add('mismisc.pas');
      Attach.Add('notused.pas');
     
      SMTP := TMySMTPSend.Create;
      try
        SMTP.TargetHost := 'smtp.googlemail.com';
        SMTP.TargetPort := '465';
        SMTP.Username := 'user@gmail.com';
        SMTP.Password := 'password here';
        SMTP.FullSSL := True;
        SMTP.Sock.OnStatus := @Sink.Progress;
        SMTP.Sock.RaiseExcept := True;
        try
          if SMTP.SendMessage(
            'user@gmail.com', // AFrom
            'recipient@domain.com', // ATo
            'Test subject FullSSL 2', // ASubject
            Content,
            Attach)
          then
            WriteLn('Success.')
          else
          begin
            WriteLn('Failure!');
          end;
        except
          on E: Exception do
            WriteLn('EXCEPTION: ', E.Message);
        end;
     
        with SMTP do
        begin
          WriteLn;
          WriteLn('  ResultCode: ', ResultCode);
          WriteLn('ResultString: ', ResultString);
          WriteLn('  FullResult: ', FullResult.Text);
          WriteLn('    AuthDone: ', AuthDone) ;
        end;
      finally
        SMTP.Free;
      end;
     
    end.
    

## Example 3 - using the XMailer wrapper

(forum user **Silvio Clécio**). I use [XMailer plugin](<https://github.com/MFernstrom/xmailer>) (it uses Synapse framework). Works fine with Gmail, Hotmail, Yahoo etc. 

It is also available in the [Online Package Manager](<Online_Package_Manager.md> "Online Package Manager"). 

## Example 4 - with error checking for debugging

This version is "advanced" in so far as it does a lot of error checking and shows you the responses from the remote mail server. It is very useful for debugging especially when you do not have access to the remote mail server's logs. 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, Forms, Controls, Graphics, Dialogs, StdCtrls;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        Button1: TButton;
        Memo1: TMemo;
        procedure Button1Click(Sender: TObject);
      private
      public
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    uses
       // "ssl_openssl" unit is required to use SSL / TLS
       ssl_openssl, SMTPsend;
    
    // MailData is the text of the mail.
    procedure SendMail(User, Password, MailFrom, MailTo, SMTPHost, SMTPPort: string; MailData: string);
    var
      SMTP: TSMTPSend;
      email_lines: TStringList;
    begin
      SMTP := TSMTPSend.Create;
      email_lines := TStringList.Create;
    
      try
        email_lines.text := Maildata;
        SMTP.UserName := User;
        SMTP.Password := Password;
        SMTP.TargetHost := SMTPHost;
        SMTP.TargetPort := SMTPPort;
        //SMTP.AutoTLS := True;  // upgrade to SSL/TLS if remote server supports it
    
        if Trim(SMTPPort) <> '25' then
          SMTP.FullSSL := true; // if sending to port 25, don't use encryption
    
        Form1.Memo1.Clear; // clear memo text
    
        if not SMTP.Login() then
          begin
            Form1.Memo1.Append('SMTP ERROR: Login:' + SMTP.EnhCodeString);
            Form1.Memo1.Append('SMTP Login: Failed - does server exist? does it accept mail?');
            Exit
          end
        else
          begin
            Form1.Memo1.Append('SMTP Login: OK');
            Form1.Memo1.append('-- Remote response: ' + SMTP.FullResult[0]);
          end;
    
        if(SMTP.ESMTP) then
          Form1.Memo1.Append('SMTP: ' + SMTPHost + ' supports ESMTP')
        else
          Form1.Memo1.Append('SMTP: ' + SMTPHost + ' supports plain SMTP');
    
        // Insist on SSL/TLS connection to remote server
        // - if the server might not support it, omit and then
        //   uncomment SMTP.AutoTLS above
        if not SMTP.StartTLS() then
          Form1.Memo1.Append('SMTP ERROR: StartTLS:' + SMTP.EnhCodeString)
        else
          begin
            Form1.Memo1.Append('SMTP StartTLS: OK');
            Form1.Memo1.append('-- Remote response: ' + SMTP.FullResult[0]);
          end;
    
        if not SMTP.MailFrom(MailFrom, Length(MailFrom)) then
          Form1.Memo1.Append('SMTP ERROR: MailFrom:' + SMTP.EnhCodeString)
        else
          begin
            Form1.Memo1.Append('SMTP MailFrom: OK');
            Form1.Memo1.append('-- Remote response: ' + SMTP.FullResult[0]);
          end;
    
        if not SMTP.MailTo(MailTo) then
          Form1.Memo1.Append('SMTP ERROR: MailTo:' + SMTP.EnhCodeString)
        else
          begin
            Form1.Memo1.Append('SMTP MailTo: OK');
            Form1.Memo1.append('-- Remote response: ' + SMTP.FullResult[0]);
          end;
    
        if not SMTP.MailData(email_lines) then
          Form1.Memo1.Append('SMTP ERROR: MailData:' + SMTP.EnhCodeString)
        else
          begin
            Form1.Memo1.Append('SMTP MailData: OK');
            Form1.Memo1.append('-- Remote response: ' + SMTP.FullResult[0]);
          end;
    
        if not SMTP.Logout() then
          Form1.Memo1.Append('SMTP ERROR: Logout:' + SMTP.EnhCodeString)
        else
          begin
            Form1.Memo1.Append('SMTP Logout: OK');
            Form1.Memo1.append('-- Remote response: ' + SMTP.FullResult[0]);
          end;
    
      finally
        SMTP.Free;
        email_lines.Free;
      end;
    end;
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      SendMail(
        '',                              // Name if authentication required
        '',                              // Password if authentication required
        'trev@example.com',              // MailFrom
        'trev@example.org',              // MailTo
        'shadow.example.org',            // Mail server to send to
        '25',                            // Mail server port
        'To: trev@example.org'           // Otherwise To: is blank on receipt
        + LineEnding + 'Subject: Test'   // Otherwise Subject: is blank on receipt
        + LineEnding +'Gday!'            // Email body
        + LineEnding);
    end;
    
    end.
    

**Example Memo1 output on success**
    
    
    SMTP Login: OK
    -- Remote response: 250-shadow.example.org Hello macmini8.example.org [192.168.1.21], pleased to meet you
    SMTP: shadow.example.org supports ESMTP
    SMTP StartTLS: OK
    -- Remote response: 220 2.0.0 Ready to start TLS
    SMTP MailFrom: OK
    -- Remote response: 250 2.1.0 <trev@example.com>... Sender ok
    SMTP MailTo: OK
    -- Remote response: 250 2.1.5 <trev@example.org>... Recipient ok
    SMTP MailData: OK
    -- Remote response: 250 2.0.0 22G97AqL046395 Message accepted for delivery
    SMTP Logout: OK
    -- Remote response: 221 2.0.0 shadow.example.org closing connection
    

**Example Memo1 output on failure**
    
    
    SMTP Login: OK
    -- Remote response: 250-shadow.example.org Hello 103-216-191-138.dyn.launtel.net.au [103.216.191.138], pleased to meet you
    SMTP: shadow.example.org supports ESMTP
    SMTP StartTLS: OK
    -- Remote response: 220 2.0.0 Ready to start TLS
    SMTP MailFrom: OK
    -- Remote response: 250 2.1.0 <trev@example.com>... Sender ok
    SMTP ERROR: [MailTo:Persistent](<MailTo:Persistent>) Transient Failure-Delivery not authorized, message refused
    -- Remote response: 451 4.7.1 Spam alert: 103-116-191-138.dyn.launtel.net.au [103.116.191.138] mail delivery delayed
    SMTP ERROR: MailData:Permanent Failure-Other undefined Status
    -- Remote response: 503 5.0.0 Need RCPT (recipient)
    SMTP Logout: OK
    -- Remote response: 221 2.0.0 shadow.example.org closing connection
    

## See also

  * [RFC2821](<https://www.ietf.org/rfc/rfc2821.txt>) \- Simple Mail Transfer Protocol (SMTP).


  * [RFC3207](<https://www.ietf.org/rfc/rfc3207.txt>) \- SMTP Service Extension for Secure SMTP over Transport Layer Security (TLS).


  * [RFC5246](<https://www.ietf.org/rfc/rfc5246.txt>) \- The Transport Layer Security (TLS) Protocol Version 1.2.


  * [RFC8446](<https://www.ietf.org/rfc/rfc8446.txt>) \- The Transport Layer Security (TLS) Protocol Version 1.3.


  * [Synapse](<Synapse.md> "Synapse")


  * [Synapse documentation](<https://synapse.ararat.cz/doku.php/public:howto:tmimepart>) on sending attachments.


  * [Article (PDF)](<https://www.freepascal.org/~michael/articles/lazmail/lazmail-en.pdf>) that covers sending email, including attachments, using Synapse.


  * [lNet examples](<lNet_examples.md> "lNet examples") \- email example using [lNet](<lNet.md> "lNet") library.

---

_Source: [https://wiki.freepascal.org/Synapse_-_Email_Examples](https://web.archive.org/web/20240121091107/https://wiki.freepascal.org/Synapse_-_Email_Examples)_
