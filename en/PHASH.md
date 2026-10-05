# PHASH

## Calculating Perceptual hashes (PHASH) and comparing image similarity

Here is a program to calculate the perceptual hash for images different than a CRC or md5 hash a perceptual hash will be similar for similar images. A slight change in the image (sharpen, blur, resize) will give perceptual hash that is similar where a CRC or MD5 hash will be completely different. I looked for a PHASH program for pascal and could not find any. I did find three methods for getting a perceptual hash. The first was using a DCT transform but the code was written in C and was not easy to link the program in. The second way is to resize it and make a hash based on whether each pixel was more or less then its next neighbor. The third way was to use image moments. ImageMagick(IM) uses this method but I found that after a certain threshold the images where radically different. Also calculating the differences required a lot of computing power as you had to square the difference of 42 numbers for each image comparison. I tried all three methods, only the first had good promise. The last one was not even part of the IM Lazarus interface. 

This program uses ImageMagick and I want to express my appreciation for the work of the translators of this tool to work with Lazarus. However I had a lot of problems getting it to work properly. First because it has not kept up with the code there were some routines that did not work because they have been deprecated (and no longer available) in ImageMagick. Second because some of the newer routines were missing entirely. And third because I was using the Q8 version of ImageMagick and it appeared that the translation was for the Q16 version. PPixelPacket for instance assumes the colors are 16 bit and not 8 bit so I had to change how I handled the structure). I use 8 bit ImageMagick because 99.99999% of the pictures I wanted to process are 8 bit or less and the Q8 program is much faster then the Q16 version. 

To get this to work under windows 7 you need to compile under 32bit mode (and call 32 bit ImageMagick) as the calling conventions for 64 bit are quite different and Lazarus does not support the 64 bit calling conventions. (this may change but keep it in mind). 

This program takes one main argument - the directory to parse for images (currently only JPG and GIF but you can add more). The program maintains a file 'yalls' that contains information for each image it finds in that directory - deleting files that no longer exists, adding new files and if the md5 does not match recalculating data for that file. (for speed you can have it skip the md5 check and just rewrite existing lines for files by placing SKIP before the directory argument. Since I had a specific set of directories to parse if you do not use any argument it will parse those directories. The output file will consist of one line for each image it finds consisting of fields separated by blanks. The program expects the filenames are without spaces and would need modification otherwise. There was a memory leak with the program and after about 200,000 images it crashed. This may be gone but if the program dies just move the yall file to yalls and restart with JPEGINFO SKIP to restart from where it ended. 

The output file contains: 
    
    
     MD5 - 32 hex bytes of the MD5 sum of the entire file.
     Signature - 64 hex bytes that comes from IM identify that is I think the md5 of the image only
     PHASH - 16 hex bytes of the DCT hash of the image
     Date of the file in YYYYMMDDHHMMSS format (from ImageMagick identify)
     Size in bytes of the file
     Bitdepth of the image
     Width of the image
     Height of the image
     Filename of the image
     Complete fileid of the image
     Example:
     md5       fb54e12c44d2b14d4d3266251109799e
     signature 2318170444932902d0541895584df4c2f8f38ad8b1d4dad899870760817422f2
     phash     CCFB4EAEF3F30C0D
     date      20151001010847
     size      901
     bitdepth  8
     width     32
     width     32
     filename  01.gif
     fid       F:\CCC\01.gif
    

  
I will add another program later if there is interest that calculates phash similarity. This program was written on a windows7 platform. Basically to get phash similarity you count the number of bits that are different. If that number is 0 the pictures are likely to be the same, if it is over 10 the pictures are likely to be different, otherwise the pictures may be similar to some degree. Note it takes about 24 hours to process 700000 images with this program on my computer. I estimate it may take another 24 hours to compare these hashes up to 4 bit differences. At least twice that for 8 bit differences. 
    
    
    program JPEGInfo;
    
    {$APPTYPE CONSOLE} {$R-}
    
    uses strutils,sysutils,classes, md5,magick_wand, ImageMagick,DateUtils;
    
    
    { Bytes = array[0..0] of Byte;}
    { LongWords = array[0..255] of Byte;}
    
    
    type
      clr = record
        red:byte;
        green:byte;
        blue:byte;
        opacity:byte;
      end;
    var
     allfile:text;
     infile:text;
     donedir,donefname, inrec:string;
     skipped,deleted,added,modified,count:longword;
     wand: PMagickWand;
     updatefileexists, skipmd5check,notstarted,readnextrec :boolean;
     dctmatrix : array[0..7,0..31] of real;
     status: MagickBooleanType;
     starttime:tdatetime;
     filename:pchar;
     udir,ufname:string;
     Size : Integer;
     timed:longint;
     DirName: string;
    
    procedure ThrowWandException(wand: PMagickWand);
    var
      description: PChar;
      severity: ExceptionType;
    begin
      description := MagickGetException(wand, @severity);
      WriteLn(Format('An error occurred. Description: %s', [description]));
      description := MagickRelinquishMemory(description);
      Abort;
    end;
    
    function pad(msg:string;len:byte):string;
    var par : byte;
    begin
       result := msg;
       if length(msg) > len then result := copy(msg,1,len);
       while length(result) < len do result := result + ' ';
       for par := 1 to len do if ord(result[par]) < 32 then result[par] := ' ';
    end;
    
    {read the next old file record into inrec and fileid into donefname,donedir}
    procedure getnextok;
    var i: integer;
    
    procedure skipitem;
    begin
       while (i < length(inrec)) and (inrec[i] <> ' ') do inc(i);
    end;
    
    procedure skipspaces;
    begin
       while inrec[i] = ' ' do inc(i)
    end;
    begin
       if eof(infile) then donedir := chr(255)
       else begin
          readln(infile,inrec);
          i := 1;
          skipitem; {md5}  skipspaces;   {fb54e12c44d2b14d4d3266251109799e}
          skipitem; {md6}  skipspaces;   {2318170444932902d0541895584df4c2f8f38ad8b1d4dad899870760817422f2}
          skipitem; {phash} skipspaces;  {CCFB4EAEF3F30C0D}
          skipitem; {date} skipspaces;   {20151001010847}
          skipitem; {size} skipspaces;   {         901}
          skipitem; {depth} skipspaces;  {  8}
          skipitem; {width} skipspaces;  {     32}
          skipitem; {height} skipspaces; {     32}
          skipitem; {name} skipspaces;   {01.gif                   }
          donefname:= copy(inrec,i,999); {F:\CCC\01.gif}
          i := rpos('\',donefname);
          if i = 0 then writeln('error reading line from yalls:',inrec);
          donedir := StringReplace(uppercase(copy(donefname,1,i)),'\',' ',[rfReplaceAll]);;
          donefname := uppercase(copy(donefname,i+1,9999));
       end;
       readnextrec := false;
    end;
    function GetDif(time:longint):string;
    var part: longint; t:longint;
    begin
       result := '';
       t := time;
       if time > 86400 then begin
          part := t div 86400;
          t := t mod 86400;
          result := result + format('%.2d',[part])+' '; end;
       if time >  3600 then begin
          part := t div  3600;
          t := t mod  3600;
          result := result + format('%.2d',[part])+':';  end;
       if time >    60 then begin
          part := t div    60;
          t := t mod    60;
          result := result + format('%.2d',[part])+':'; end;
       if time >     0 then
          result := result + format('%.2d',[t]);
    end;
    
    function gethash:string;
    var
      intermediate : array[0..7,0..31] of real;
      i,j,r,c,k:byte;
      img:array[0..31,0..31] of byte;
      dctresult:array[0..7,0..7] of real;
      pimg: Pimage;
      pack:record case integer of
         1:( pkt:PPixelPacket);
         2:( co:^clr);
      end;
      hash,one:qword; mean:DOUBLE;
    begin
       result := 'xxxxxxxxxxxxxxxx'; { in case an error occurs}
       try
          {Resize the image to 32x32. If it is my test case file
           'f:\ccc\01.gif' then it is already at the correct size and I
            found that if imagemagic resizes it the file gets changed ---
            I did not want that, it might be faster to grayscale the image
            first then resize but not if you use MagickQuantizeImage too slow}
          if uppercase(filename) <> 'F:\CCC\01.GIF' then
             MagickResizeImage(wand, 32, 32, LanczosFilter, 1.0);
          {now we convert the image to grayscale - this procedure is similar to
            MagickQuantizeImage(wand, 256, GRAYColorspace, iszero, iszero, iszero);
             but is a LOT faster. BTW I don't know why I had to use variables
             for this function. The conversion numbers were selected to be
             close to what MagickQuantizeImage used. Note I had an issue
             here. Under Q8 IM GetAuthenticPixels it returns an array of
             byte values for colors but the lazarus interface mapped it to word sizes.
             You could get an entire array of values with getauthenticpixels but
             the speed won't be much different so I didn't bother.
             We dump the results into the img array}
          pimg := GetImageFromMagickWand(wand);
          for j := 0 to 31 do begin
             for i := 0 to 31 do begin
                pack.pkt := GetAuthenticPixels(pimg, i, j, 1, 1, nil);
                img[i,j] :=round(pack.co^.red*0.072+
                                 pack.co^.green*0.715+
                                 pack.co^.blue*0.213);
             end;
          end;
          {results are in the img array. Now do matrix multiplcation
              DCTMATRIX * img * transposed(DCTMATRIX)
           since we only need the top 8x8 array we only do the
           calculations for that part}
          {intermediate = DCTMATRIX*IMG}
          fillchar(intermediate,sizeof(intermediate),0);
          for r := 0 to 7 do
             for c := 0 to 31 do
                for k := 0 to 31 do
                    intermediate[r,c] := intermediate[r,c] +
                                         dctmatrix[r,k]*img[k,c];
          {dctresult  = intermediate*transposed(DCTMATRIX)}
          fillchar(dctresult,sizeof(dctresult),0);
          for r := 0 to 7 do
             for c := 0 to 7 do
                for k := 0 to 31 do
                    dctresult[r,c] := dctresult[r,c] +
                    intermediate[r,k]*dctmatrix[c,k]{inverted dctmatrix};
          {now we need to calculate the arithmetic mean. Note the first value
           (0,0) is much too large and affects the mean value too much.
           To fix it is set to 0 but perhaps some other solution can be done
           such as do a log on the value}
          mean := 0;
          dctresult[0,0] := 0;
          for r := 0 to 7 do
             for c := 0 to 7 do
                mean := mean + dctresult[r,c];
          mean := mean / 64;
          {the hash is calculated by setting the bit of
           each value greater than the mean}
          one := 1;
          hash := 0;
          for r := 0 to 7 do
             for c := 0 to 7 do begin
                if dctresult[r,c] > mean then inc(hash,one);
                one := one shl 1;
             end;
          one := 1;
          {return the phash}
          result := inttohex(hash,16);
       finally
       end;
    end;
    
    procedure processfile(dir:string;fname:string);
    var
      F: file;
      idents:pchar;
      i:integer;
      md5,md6, date, fsize,depth,imageheight,imagewidth,fno:string;
      P: Pointer;
    begin
       inc(count);
       if (count mod 1000) = 1 then begin
          timed := SecondsBetween(Now,starttime);
          writeln('at count ',count-1,' E:'+ GetDif(timed)+' D:',deleted,' A:',added,' M:',modified,' S:',skipped,' ',dir,fname);
       end;
       filename := pchar(dir+fname);
       udir := StringReplace(uppercase(dir),'\',' ',[rfReplaceAll]);;
       ufname := uppercase(fname);
       if updatefileexists then begin
       if readnextrec then getnextok;
       while (udir > donedir) or ((udir = donedir) and (ufname > donefname)) do begin getnextok; inc(deleted); end;
       if skipmd5check then
          if (udir+ufname) = (donedir+donefname) then begin
                inc(skipped);
                writeln(allfile,inrec);
                readnextrec := true;
             exit;
          end;
        end;
       TRY
          Assign(F,FileName);
          FileMode:=fmOpenRead + fmShareDenyNone;
          Reset(F, 1);
          Size := FileSize(F);
          GetMem(P, Size);
          BlockRead(F, P^, Size);
          md5 := md5print(md5buffer(p^,Size));
       finally
          FreeMem(P);
          CloseFile(F);
       end;
       if updatefileexists then begin
          if (udir+ufname) = (donedir+donefname) then begin
             if copy(inrec,1,32) = md5 then begin
                inc(skipped);
                writeln(allfile,inrec);
                readnextrec := true;
                exit;
             end;
             inc(modified);
          end
          else inc(added);
       end;
       if notstarted then begin
          notstarted := false;
          writeln('Started at line:',count,' with file:',filename);
       end;
       try
          wand := NewMagickWand;
          status := MagickReadImage(wand, filename);
          if (status = MagickFalse) then ThrowWandException(wand);
          idents   := magickidentifyimage(wand);
          md6   := copy(idents,pos('signature:',idents)+11,64);
          i := pos('date:modify:',idents)+13;
          if i = 0 then i := pos('date:create:',idents)+13;
          date  := copy(idents,i   ,4)+copy(idents,i+ 5,2)+copy(idents,i+ 8,2)+
                   copy(idents,i+11,2)+copy(idents,i+14,2)+copy(idents,i+17,2);
          fsize := copy(idents,pos('Filesize:',idents)+10,64);
          i := POS(Chr(10),FSIZE);
          if i > 0 then fsize := copy(fsize,1,i-1);
          i := pos('Depth:',   idents)+7;
          depth := copy(idents, i,1);
          i := pos('Geometry:',idents)+10;
          imageheight    := copy(idents, i,99);
          i := pos('x',imageheight);
          imagewidth := copy(imageheight,i+1,99);
          imageheight := copy(imageheight,1,i-1);
          i := pos('+',imagewidth);
          imagewidth := copy(imagewidth,1,i-1);
          fno := fname;
          if length(fno) < 22 then fno := pad(fno,25);
          writeln(allfile,md5,' ',md6,' ',gethash,' ',date:14,' ',
               size:12, depth:2,imageheight:7,imagewidth:7,' ',
               fno,' ',filename);
        finally
           wand := DestroyMagickWand(wand);
        end;
    end;
    
    // Recursive procedure to build a list of files
    procedure FindFiles(StartDir: string);
    var
      SR: TSearchRec;
      IsFound: Boolean;
    begin
      if StartDir[length(StartDir)] <> '\' then  StartDir := StartDir + '\';
      { Build a list of the files in directory StartDir
        (but not the directories!)                         }
      IsFound := FindFirst(StartDir+'*.*', faAnyFile-faDirectory, SR) = 0;
      while IsFound do begin
        if (uppercase(extractFileExt(sr.name)) = '.JPG')
           or (uppercase(extractFileExt(sr.name)) = '.GIF')
              then processfile(StartDir,SR.Name);
        IsFound := FindNext(SR) = 0;
      end;
      FindClose(SR);
      // Build a list of subdirectories
      IsFound := FindFirst(StartDir+'*.*', faDirectory, SR) = 0;
      while IsFound do begin
        if ((SR.Attr and faDirectory) <> 0) and (SR.Name[1] <> '.') then
           FindFiles(StartDir + SR.Name);
        IsFound := FindNext(SR) = 0;
      end;
      FindClose(SR);
    end;
    
    {$R *.res}
    procedure makedct;
    var c1:real; ii,p,q: byte;
    begin
       c1 := 1/sqrt(32);
       for ii := 0 to 31 do dctmatrix[0,ii] := c1;
       c1 := c1*2;
       for p := 1 to 7 do
          for q := 0 to 31 do
              dctmatrix[p,q] := c1 * cos(pi/64*p*(2*q+1));
    end;
    
    begin
      starttime := now;   {keep track of time}
      {have we started processing files as opposed to copy old recs}
      notstarted := true;
      skipmd5check := false; {on copy skip md5 check}
      DirName := ParamStr(1); {did he pass a filename}
      if length(dirname) > 3 then
         if copy(upcase(dirname),1,4) = 'SKIP' then begin
            dirname := paramstr(2);
            skipmd5check := true;
      end;
      count := 0;
      deleted := 0; modified := 0; added := 0; skipped := 0;
      {allow access to file while writing it}
      FileMode:=fmOpenwrite + fmShareDenyNone;
      assign(allfile,'yall');
      rewrite(allfile);
      readnextrec := true;
      updatefileexists := fileexists('yalls');
      if updatefileexists then begin
         {allow access to file while reading it}
         FileMode:=fmOpenwrite + fmShareDenyNone;
         assign(infile,'yalls');
         reset(infile);
         getnextok;
      end;
    
      MagickWandGenesis;
      makedct;  {make DCT matrix (or at least what is needed)}
      {This is a set of directories that I desired to maintain}
      if (dirname = 'F:\') or (dirname = 'F:\') or
         (length(dirname) < 3) then  begin
         FindFiles('F:\CCC\');
         FindFiles('F:\CD\');
         FindFiles('F:\CHECK\');
         FindFiles('F:\QDISK\');
      end
      else FindFiles(DirName); {or use a user supplied directory name}
      writeln('found:',count,' files ',' Deleted:',deleted,' Added:',added,' Changed:',modified,' SKIPPED:',skipped);
      close(allfile);
      if updatefileexists then close(infile);
      MagickWandTerminus;
      { I get paranoid and like to keep a few versions of the files around}
      if fileexists('yalls.005') then deletefile('yalls.005');
      if fileexists('yalls.004') then renamefile('yalls.004','yalls.005');
      if fileexists('yalls.003') then renamefile('yalls.003','yalls.004');
      if fileexists('yalls.002') then renamefile('yalls.002','yalls.003');
      if fileexists('yalls.001') then renamefile('yalls.001','yalls.002');
      if fileexists('yalls') then renamefile('yalls','yalls.001');
      renamefile('yall','yalls');
    end.

---

_Source: [https://wiki.freepascal.org/PHASH](https://web.archive.org/web/20241207124201/https://wiki.freepascal.org/PHASH)_
