# macOS Sound Utilities

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

  


## Contents

  * 1 Sound Utilities
    * 1.1 afinfo
    * 1.2 afplay
    * 1.3 afconvert
  * 2 See also



## Sound Utilities

Every Apple computer running macOS comes with several little-known, command-line sound utilities which can come in very handy when you are experimenting with audio files. 

### afinfo

The Audio File Info utility prints out information about an audio file to stdout. It will display the file format, the data format, the bit rate and other useful information. 

Open an Applications > Utilities > Terminal and run the following command: 
    
    
    afinfo '/Applications/iPhoto.app/Contents/Resources/Music/Minuet in G.mp3'
    

which should provide output similar to: 
    
    
     File:           /Applications/iPhoto.app/Contents/Resources/Music/Minuet in G.mp3 
     File type ID:   MPG3
     Num Tracks:     1
     ----
     Data format:     2 ch,  44100 Hz, '.mp3' (0x00000000) 0 bits/channel, 0 bytes/packet, 1152 frames/packet, 0 bytes/frame
                   no channel layout.
     estimated duration: 90.357550 sec
     audio bytes: 1807151
     audio packets: 3459
     bit rate: 160000 bits per second
     packet size upper bound: 1052
     maximum packet size: 523
     audio data file offset: 0
     optimized
     ----
    

As you can see, the output of afinfo provides a wealth of information about the audio file. 

### afplay

The Audio File Play utility plays an audio file to the default audio output. The volume, time, quality and playback rate can be set. 

Open an Applications > Utilities > Terminal and run the following command which sets the utility to play the specified file for 10 seconds at 50% volume and high quality: 
    
    
    afplay -t 10 -v 0.5 -q high '/Applications/iPhoto.app/Contents/Resources/Music/Minuet in G.mp3'
    

The afplay utility is particularly useful when experimenting with compressing a file at different bit rates and listening to how the result sounds. 

### afconvert

This perhaps the most useful of the three command-line utilities. Audio File Convert will convert a source audio file to a new audio file with the specified file and data types. These are the basic arguments: 
    
    
    afconvert -d [out data format] -f [out file format] [in file] [out file]
    

There are other possible arguments, open a Terminal and enter the following to detail the full list of arguments: 
    
    
    afconvert -h
    

It supports the following audio data and file formats: 
    
    
       '3gpp' = 3GP Audio (.3gp)
                  data_formats: 'Qclp' 'aac ' 'aace' 'aacf' 'aacg' 'aach' 
                                'aacl' 'aacp' 'samr' 
       '3gp2' = 3GPP-2 Audio (.3g2)
                  data_formats: 'Qclp' 'aac ' 'aace' 'aacf' 'aacg' 'aach' 
                                'aacl' 'aacp' 'samr' 
       'adts' = AAC ADTS (.aac, .adts)
                  data_formats: 'aac ' 'aach' 'aacp' 
       'ac-3' = AC3 (.ac3)
                  data_formats: 'ac-3' 
       'AIFC' = AIFC (.aifc, .aiff, .aif)
                  data_formats: I8 BEI16 BEI24 BEI32 BEF32 BEF64 UI8 'ulaw' 
                                'alaw' 'MAC3' 'MAC6' 'ima4' 'QDMC' 'QDM2' 
                                'Qclp' 'agsm' 
       'AIFF' = AIFF (.aiff, .aif)
                  data_formats: I8 BEI16 BEI24 BEI32 
       'amrf' = AMR (.amr)
                  data_formats: 'samr' 'sawb' 
       'm4af' = Apple MPEG-4 Audio (.m4a, .m4r)
                  data_formats: 'aac ' 'aace' 'aacf' 'aacg' 'aach' 'aacl' 
                                'aacp' 'ac-3' 'alac' 'ec-3' 'paac' 'pac3' 
                                'pec3' 
       'm4bf' = Apple MPEG-4 AudioBooks (.m4b)
                  data_formats: 'aac ' 'aace' 'aacf' 'aacg' 'aach' 'aacl' 
                                'aacp' 'paac' 
       'caff' = CAF (.caf)
                  data_formats: '.mp1' '.mp2' '.mp3' 'QDM2' 'QDMC' 'Qclp' 
                                'Qclq' 'aac ' 'aace' 'aacf' 'aacg' 'aach' 
                                'aacl' 'aacp' 'ac-3' 'alac' 'alaw' 'drms' 
                                'dvi8' 'ec-3' 'flac' 'ilbc' 'ima4' I8 BEI16 
                                BEI24 BEI32 BEF32 BEF64 LEI16 LEI24 LEI32 
                                LEF32 LEF64 'ms\x00\x02' 'ms\x00\x11' 'ms\x001' 
                                'opus' 'paac' 'pac3' 'pec3' 'qaac' 'qac3' 
                                'qach' 'qacp' 'qec3' 'samr' 'ulaw' 'zaac' 
                                'zac3' 'zach' 'zacp' 'zec3' 
       'ec-3' = EC3 (.ec3)
                  data_formats: 'ec-3' 
       'flac' = FLAC (.flac)
                  data_formats: 'flac' 
       'MPG1' = MPEG Layer 1 (.mp1, .mpeg, .mpa)
                  data_formats: '.mp1' 
       'MPG2' = MPEG Layer 2 (.mp2, .mpeg, .mpa)
                  data_formats: '.mp2' 
       'MPG3' = MPEG Layer 3 (.mp3, .mpeg, .mpa)
                  data_formats: '.mp3' 
       'mp4f' = MPEG-4 Audio (.mp4)
                  data_formats: 'aac ' 'aace' 'aacf' 'aacg' 'aach' 'aacl' 
                                'aacp' 'ac-3' 'ec-3' 
       'NeXT' = NeXT/Sun (.snd, .au)
                  data_formats: I8 BEI16 BEI24 BEI32 BEF32 BEF64 'ulaw' 
       'Sd2f' = Sound Designer II (.sd2)
                  data_formats: I8 BEI16 BEI24 BEI32 
       'WAVE' = WAVE (.wav)
                  data_formats: UI8 LEI16 LEI24 LEI32 LEF32 LEF64 'ulaw' 
                                'alaw' 
       'RF64' = WAVE (.wav)
                  data_formats: UI8 LEI16 LEI24 LEI32 LEF32 LEF64 'ulaw' 
                                'alaw
    

For example, this converts the .mp3 file into a .wav file using the Little Endian, 16 bit integer data format: 
    
    
    afconvert -f RF64 -d LEI16 '/Applications/iPhoto.app/Contents/Resources/Music/Minuet in G.mp3' test.wav
    

## See also

  * [macOS Audio Player](<macOS_Audio_Player.md> "macOS Audio Player")
  * [macOS Audio Recorder](<macOS_Audio_Recorder.md> "macOS Audio Recorder")
  * [macOS System Sound Services](<macOS_Play_Alert_Sound.md> "macOS Play Alert Sound")
  * [macOS NSSound](<macOS_NSSound.md> "macOS NSSound")
  * [Multimedia Programming](<Multimedia_Programming.md> "Multimedia Programming")

---

_Source: [https://wiki.freepascal.org/macOS_Sound_Utilities](https://web.archive.org/web/20210117115320/https://wiki.freepascal.org/macOS_Sound_Utilities)_
