# GIF

GIF is a graphic format. From about 2004 year, code for LZW compressing (for GIF) is free. So it's free. 

To show GIF image, animated or not, you can use [AnimatedGif library](<http://consume.o2switch.net/lazarus/>). It gives unit which self-registers in Free Pascal classes, and also in Lazarus' TPicture: 
    
    
    initialization
    
      //Free Pascal Image
      ImageHandlers.RegisterImageReader ('Animated GIF', TAnimatedGif.GetFileExtensions, TFPReaderGIF);
    
      //Lazarus Picture
      TPicture.RegisterFileFormat(TAnimatedGif.GetFileExtensions,'Animated GIF', TAnimatedGif);

---

_Source: [https://wiki.freepascal.org/AnimatedGif](https://web.archive.org/web/20190818192619/https://wiki.freepascal.org/AnimatedGif)_
