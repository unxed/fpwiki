# MySQL on Zaurus

1 - Create symlinks: 
    
    
    ln -s /usr/lib/libmysqlclient.so.6 /usr/lib/libmysqlclient.so
    ln -s /lib/libm.so.6 /usr/lib/libm.so
    ln -s /lib/libc.so.6 /usr/lib/libc.so
    

2 - To create this "libgcc.so" you need GCC on your zaurus, otherwise, just copy libgcc.so to your /usr/lib directory. 
    
    
    mkdir /mnt/card/tmpdir  
    cd /mnt/card/tmpdir  
    ar x /mnt/card/gcc/lib/gcc-lib/arm-linux/2.95.2/libgcc.a  
    gcc -shared -fno-shared-data -o libgcc.so /mnt/card/tmpdir/*.o
    cp libgcc.so /usr/lib
    

3 - Finally, compile your program using: 
    
    
    fpc -k-lgcc testdb.pp

---

_Source: [https://wiki.freepascal.org/MySQL_on_Zaurus](https://web.archive.org/web/20210927153358/https://wiki.freepascal.org/MySQL_on_Zaurus)_
