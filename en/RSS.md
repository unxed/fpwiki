# RSS

│ **English (en)** │

## Overview

This is a small unit I created to read RSS feeds. Maybe it is useful. 

## RSS unit code
    
    
    unit rss;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils,
      lNetComponents, lNet, lhttp, lHTTPUtil, DOM, XMLRead, db;
    
    const
      RSSXML_RSS = 'rss';
      RSSXML_CHANNEL = 'channel';
      RSSXML_ITEM = 'item';
      RSSXML_TITLE = 'title';
      RSSXML_LINK = 'link';
      RSSXML_DESCRIPTION = 'description';
      RSSXML_PUBDATE = 'pubDate';
      RSSXML_COMMENTS = 'comments';
    
    type
    
      { TFeedItem }
    
      TFeedItem = class(TObject)
      private
        fTitle, fLink, fComments : String;
        fDescription: TStringList;
        fPubDate: TDateTime;
      public
        constructor create();
        destructor destroy(); override;
        class function CreateFromNode(aNode: TDOMNode): TFeedItem;
        property Title : String read fTitle write fTitle;
        property Link : String read fLink write fLink;
        property Comments : String read fComments write fComments;
        property Description : TStringList read fDescription write fDescription;
      end;
    
      TFeedReadState = (frReady, frRequest, frParse, frSave, frDone, frError);
    
      // Callback for result report
      TRSSResultEvent = procedure(const aFeedID: string) of object;
    
      { TRssFeed }
    
      TRssFeed = class(TObject)
      private
        fTitle, fLink, fLanguage, fError, fFeedID: String;
        fDescription: TStringList;
        fItems: TList;
        fXmlString: TStringList;
        fReadState: TFeedReadState;
        HTTPClient: TLHTTPClientComponent;
        HTTPBuffer: string;
        fDoc : TXMLDocument;
        fOnResult: TRSSResultEvent;
        fDatabase: TDatabase;
        procedure AddItem(aItem : TFeedItem);
        procedure ClearItems;
        procedure HTTPClientError(const msg: string; aSocket: TLSocket);
        function HTTPClientInput(ASocket: TLHTTPClientSocket; ABuffer: pchar; ASize: integer): integer;
        procedure HTTPClientDisconnect(aSocket: TLSocket);
      public
        constructor create(aLink: String; aFeedID: string; aDatabase: TDatabase);
        destructor destroy(); override;
        procedure RssRead;
        property Title: String read fTitle write fTitle;
        property Language: String read fLanguage write fLanguage;
        property Link: String read fLink write fLink;
        property Description: TStringList read fDescription write fDescription;
        property Items: TList read fItems write fItems;
        property ReadState: TFeedReadState read fReadState;
        property OnResult: TRSSResultEvent read fOnResult write fOnResult;
      end;
    
    
    implementation
    
    { TFeedItem }
    
    constructor TFeedItem.create();
    begin
      fDescription := TStringList.Create;
    end;
    
    destructor TFeedItem.destroy;
    begin
      FreeAndNil(fDescription);
      inherited destroy;
    end;
    
    class function TFeedItem.CreateFromNode(aNode: TDOMNode): TFeedItem;
    var
      propertynode: TDOMNode;
    begin
      Result := TFeedItem.Create();
      propertynode := aNode.FindNode(RSSXML_TITLE);
      if propertynode <> nil then
        Result.fTitle := propertynode.TextContent;
      propertynode := aNode.FindNode(RSSXML_LINK);
      if propertynode <> nil then
        Result.fLink := propertynode.TextContent;
      propertynode := aNode.FindNode(RSSXML_DESCRIPTION);
      if propertynode <> nil then
        Result.fDescription.Text := propertynode.TextContent;
      propertynode := aNode.FindNode(RSSXML_COMMENTS);
      if propertynode <> nil then
        Result.fComments := propertynode.TextContent;
    end;
    
    { TRssFeed }
    
    procedure TRssFeed.AddItem(aItem: TFeedItem);
    begin
      fItems.Add(aItem);
    end;
    
    procedure TRssFeed.ClearItems;
    var
      i: Integer;
      Item: TFeedItem;
    begin
      for i := 0 to fItems.Count - 1 do
      begin
        Item := TFeedItem(fItems[0]);
        FreeAndNil(Item);
      end;
      fItems.Clear;
    end;
    
    procedure TRssFeed.HTTPClientError(const msg: string; aSocket: TLSocket);
    begin
      fError := msg;
      fReadState := frError;
    end;
    
    function TRssFeed.HTTPClientInput(ASocket: TLHTTPClientSocket; ABuffer: pchar;
      ASize: integer): integer;
    var
      oldLength: dword;
    begin
      oldLength := Length(HTTPBuffer);
      setlength(HTTPBuffer,oldLength + ASize);
      move(ABuffer^,HTTPBuffer[oldLength + 1], ASize);
      Result := aSize; // tell the http buffer we read it all
    end;
    
    procedure TRssFeed.HTTPClientDisconnect(aSocket: TLSocket);
    var
      S: TStringStream;
      channel, propertynode: TDOMNode;
      newsitem: TFeedItem;
    begin
      fReadState:= frParse;
      try
        S := TStringStream.Create('');
        try
          ReadXMLFile(fDoc, S);
        finally
          S.Free;
        end;
      except
        on E : Exception do
        begin
          fReadState:= frError;
          fError := E.Message;
          Exit;
        end;
      end;
      if ((fDoc.documentElement.nodeName = RSSXML_RSS) and
          (fDoc.documentElement.hasChildNodes)) then
      begin
        channel := fDoc.DocumentElement.FindNode(RSSXML_CHANNEL);
        if channel <> nil then
        begin
          propertynode := channel.FindNode(RSSXML_TITLE);
          if propertynode <> nil then
            fTitle := propertynode.TextContent;
          propertynode := channel.FindNode(RSSXML_LINK);
          if propertynode <> nil then
            fLink := propertynode.TextContent;
          propertynode := channel.FindNode(RSSXML_DESCRIPTION);
          if propertynode <> nil then
            fDescription.Text := propertynode.TextContent;
          propertynode := channel.FindNode(RSSXML_ITEM);
          while propertynode <> nil do
          begin
            newsitem:= TFeedItem.CreateFromNode(propertynode);
            AddItem(newsitem);
            propertynode := propertynode.NextSibling;
          end;
        end;
      end;
      fReadState:= frSave;
    end;
    
    constructor TRssFeed.create(aLink: String; aFeedID: string; aDatabase: TDatabase);
    begin
      fFeedID := aFeedID;
      fLink := aLink;
      fDatabase := aDatabase;
      fReadState := frReady;
      fDescription := TStringList.Create;
      fItems := TList.Create;
      HTTPClient := TLHTTPClientComponent.Create(nil);
      fDoc := TXMLDocument.create;
    end;
    
    destructor TRssFeed.destroy;
    begin
      ClearItems;
      FreeAndNil(fDescription);
      FreeAndNil(HTTPClient);
      FreeAndNil(fDoc);
      inherited destroy;
    end;
    
    procedure TRssFeed.RssRead;
    var
      aHost, aURI: String;
      aPort: Word;
    begin
      DecomposeURL(fLink, aHost, aURI, aPort);
      HTTPClient.Host:= aHost;
      HTTPClient.Port:= aPort;
      HTTPClient.URI:= aURI;
      HTTPClient.OnError:= @HTTPClientError;
      HTTPClient.OnInput:= @HTTPClientInput;
      HTTPClient.OnDisconnect:= @HTTPClientDisconnect;
      HTTPClient.SendRequest;
      fReadState:= frRequest;
    end;
    
    end.

---

_Source: [https://wiki.freepascal.org/RSS](https://web.archive.org/web/20230321111332/https://wiki.freepascal.org/RSS)_
