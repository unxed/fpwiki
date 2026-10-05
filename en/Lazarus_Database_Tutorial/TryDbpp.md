# Lazarus Database Tutorial/TryDbpp

From Free Pascal wiki
    
    
    program qtest;
     
    uses mysql_v4;
     
    Const
      DataBase : Pchar = 'testdb';
     
    var
     count,num : longint;
     code : integer;
     sock : PMYSQL;
     qmysql : TMYSQL;
     qbuf : string [160];
     rowbuf : MYSQL_ROW;
     dummy, dummy1, dummy2, dummy3, dummyquery, dumquery : string;
     recbuf : PMYSQL_RES;
     alloc : PMYSQL;
     host, user, passwd, Query : Pchar;
     
    begin
     if paramcount=1 then
       begin
       Dummy:=Paramstr(1)+#0;
       DataBase:=@Dummy[1];
       end;
    Write ('Enter Hostname: '); readln (dummy1); dummy1 := dummy1 + #0; host := @dummy1[1];
    write ('Enter Username: '); readln (dummy2); dummy2 := dummy2 + #0; user := @dummy2[1];
    write ('Enter Password: '); readln (dummy3); dummy3 := dummy3 + #0; passwd := @dummy3[1];  
     
    Writeln ('Allocating Space...');
     alloc := mysql_init(PMYSQL(@qmysql));
     Write ('Connecting to MySQL...');
     sock :=  mysql_real_connect(alloc, host, user, passwd, database, 0, nil, 0);
     if sock=Nil then
       begin
       Writeln (stderr,'Couldn''t connect to MySQL.');
       Writeln (stderr, 'Error was: ', mysql_error(@qmysql));
       halt(1);
       end;
     Writeln ('Done.');
     Writeln ('Connection data:');
    {$ifdef Unix}
     writeln ('Mysql_port      : ',mysql_port);
     writeln ('Mysql_unix_port : ',mysql_unix_port);
    {$endif}
     writeln ('Host info       : ',mysql_get_host_info(sock));
     writeln ('Server info     : ',mysql_stat(sock));
     writeln ('Client info     : ',mysql_get_client_info);
     
     Writeln ('Selecting Database ',DataBase,'...');
     if mysql_select_db(sock,DataBase) < 0 then
       begin
       Writeln (stderr,'Couldn''t select database ',Database);
       Writeln (stderr,mysql_error(sock));
       halt (1);
       end;
     
    repeat
    write ('Enter SQL Query: '); readln (dumquery); dummyquery := dumquery + #0; 
    query := @dummyquery[1];
     
     writeln ('Executing query : ',Query,'...');
       if (mysql_query(sock,Query) < 0) then
         begin
         Writeln (stderr,'Query failed ');
         writeln (stderr,mysql_error(sock));
         Halt(1);
         end;
     
     recbuf := mysql_store_result(sock);
     if RecBuf=Nil then
       begin
       Writeln ('Query returned nil result.');
       mysql_close(sock);
       halt (1);
       end;
     Writeln ('Number of records returned  : ',mysql_num_rows (recbuf));
     Writeln ('Number of fields per record : ',mysql_num_fields(recbuf));
     
     rowbuf := mysql_fetch_row(recbuf);
     while (rowbuf <>nil) do
          begin
          Write  ('(Id: ', rowbuf[0]);
          Write  (', Name: ', rowbuf[1]);
          Writeln(', Email : ', rowbuf[2],')');
          rowbuf := mysql_fetch_row(recbuf);
          end;
    until dumquery = 'quit';
     
     Writeln ('Freeing memory occupied by result set...');
     mysql_free_result (recbuf);
     
     Writeln ('Closing connection with MySQL.');
     mysql_close(sock);
     halt(0);
    end.
     
     $Log: testdb.pp,v $
     
     
     Revision  2004/04/13 by Chris Kirkpatrick: renamed as trydb.pp, inserted lots for mysql v4, made more interactive
     
     Revision 1.3  2002/09/07 15:42:53  peter
       * old logs removed and tabs fixed
     
     Revision 1.2  2002/05/31 11:54:33  marco
     * Renamefest for 1.0, many 1.1.x spots patched also.
     
     Revision 1.1  2002/01/29 17:54:54  peter
       * splitted to base and extra

---

_Source: [https://wiki.freepascal.org/Lazarus_Database_Tutorial/TryDbpp](https://web.archive.org/web/20121029192608/https://wiki.freepascal.org/Lazarus_Database_Tutorial/TryDbpp)_
