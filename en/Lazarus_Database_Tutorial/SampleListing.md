# Lazarus Database Tutorial/SampleListing

kirkpatc@anglican:~/FreePascal/fpc/packages/base/mysql> ./trydb
     
    Enter Hostname: localhost
    Enter Username: kirkpatc
    Enter Password: ****
    Allocating Space...
    Connecting to MySQL...Done.
    Connection data:
    Mysql_port      : 3306
    Mysql_unix_port : /var/lib/mysql/mysql.sock
    Host info       : Localhost via UNIX socket
    Server info     : Uptime: 11505  Threads: 1  Questions: 11  Slow queries: 0  Opens: 7   Flush tables: 1  Open tables: 1  Queries per second avg: 0.001
    Client info     : 4.0.15
    Selecting Database testdb...
    Enter SQL Query: select * from FPdev
    Executing query : select * from FPdev...
    Number of records returned  : 8
    Number of fields per record : 3
    (Id: 1, Name: Michael Van Canneyt, Email : Michael...tfdec1.fys.kuleuven.ac.be)
    (Id: 2, Name: Florian Klaempfl, Email : ba2395...fen.baynet.de)
    (Id: 3, Name: Carl-Eric Codere, Email : codc01...gel.usherb.ca)
    (Id: 4, Name: Daniel Mantione, Email : d.s.p.mantione...twi.tudelft.nl)
    (Id: 5, Name: Pierre Muller, Email : muller...europe.u-strasbg.fr)
    (Id: 6, Name: Jonas Maebe, Email : jmaebe...mail.dma.be)
    (Id: 7, Name: Peter Vreman, Email : pfv...worldonline.nl)
    (Id: 8, Name: Gerry Dubois, Email : gerry...webworks.ml.org)
    Enter SQL Query: select id, username from FPdev where id<6
    Executing query : select id, username from FPdev where id<6...
    Number of records returned  : 5
    Number of fields per record : 2
    (Id: 1, Name: Michael Van Canneyt, Email : )
    (Id: 2, Name: Florian Klaempfl, Email : )
    (Id: 3, Name: Carl-Eric Codere, Email : )
    (Id: 4, Name: Daniel Mantione, Email : )
    (Id: 5, Name: Pierre Muller, Email : )
    Enter SQL Query: quit
    Executing query : quit...

---

_Source: [https://wiki.freepascal.org/Lazarus_Database_Tutorial/SampleListing](https://web.archive.org/web/20210122091015/https://wiki.freepascal.org/Lazarus_Database_Tutorial/SampleListing)_
