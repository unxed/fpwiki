# Free disk space

It is not obvious how to get the free space, in a directory. Here is working code from the [CudaText](<CudaText.md> "CudaText") project. 
    
    
    uses SysUtils;
    
    function AppDiskGetFreeSpace(const fn: string): Int64;
    begin
      {$ifdef linux}
      //this crashes on FreeBSD 12 x64
      exit(SysUtils.DiskFree(SysUtils.AddDisk(ExtractFileDir(fn))));
      {$endif}
    
      {$ifdef windows}
      exit(SysUtils.DiskFree(SysUtils.GetDriveIDFromLetter(ExtractFileDrive(fn))));
      {$endif}
    
      //cannot detect
      exit(-1);
    end;
    

The "fn" parameter should be any filename in the needed directory. E.g. if we need to get free space in the directory '/home/user/media', pass parameter fn as '/home/user/media/a'. 

Note: code does not support Unixes other than Linux, because real tests show, that code for "$ifdef linux" part is crashing on FreeBSD 12 x64. Under FPC 3.2-fixes. [Gitlab issue](<https://gitlab.com/freepascal.org/fpc/source/-/issues/39428>).

---

_Source: [https://wiki.freepascal.org/Disk_free_space](https://web.archive.org/web/20250326012905/https://wiki.freepascal.org/Disk_free_space)_
