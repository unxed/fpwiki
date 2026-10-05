# NI-Scope examples

## Contents

  * 1 INTRODUCTION
  * 2 Library documentation
  * 3 PASCAL bindings
  * 4 Code Examples
    * 4.1 Linux examples
      * 4.1.1 Autosetuped acquisition
      * 4.1.2 Binary acquisition
    * 4.2 Windows examples
      * 4.2.1 Autosetuped acquisition
      * 4.2.2 Binary acquisition
  * 5 TESTED HARDWARE



## INTRODUCTION

National Instruments produce a wide range of 8-,12-,14,16-,24-bit Oscilloscopes/Digitizers cards in PCI, PCIe, PXI and PXIe form factor with sample rate up to 12.5Gs/s. [[1]](<http://www.ni.com/digitizers/>)

This cards are supported by NI-Scope driver. Only Linux 32-bit and Windows 32/64 bit are supported. 

**NI Scope for Linux**

Hardware are supported by Ni-Scope 3.1 for Linux [(download)](<http://www.ni.com/download/ni-scope-3.1/1633/en/>). This driver works only on Linux distributions with kernel 2.6.x RHEL 5.x, 6.x, Scientific Linux 5.x, 6.x and some old for example Mandrake 10.1. Read readme file to find list of supported hardware. 

To install NI-Scope 3.1 properly under these distributions make this trick 
    
    
      Install NI-DAQmx 8.0.2 after NI-Scope.
    

[(NI-DAQmx8.0.2 download)](<http://www.ni.com/download/ni-daqmx-8.0.2/2322/en/>)

To check device presents in your system type **nilsdev** in terminal. Output should be like this 
    
    
      NI PXI-5122: "Dev2" 
      NI PXI-5122: "Dev1"
    

Two PXI-5122 cards are present in the system with aliases Dev1 and Dev2. 

**NI-Scope for Windows**

Windows OSes are supported by latest NI-Scope 14.1 driver [(download)](<http://www.ni.com/download/ni-scope-14.1/5000/en/>). 

Read readme to find list of supported hardware. 

Use NIMAX utility to setup device alias, for example Dev1. 

## Library documentation

Your can view or download latest documentation here [[2]](<http://digital.ni.com/manuals.nsf/websearch/06669B07C3B83BFD86257CE0005962FA>). Alternately NI-Scope installation copy it to your PC, for example in Linux PC /usr/local/natinst/niscope/doc. 

## PASCAL bindings

You can download bindings for your operation system from this forum thread [[3]](<http://forum.lazarus.freepascal.org/index.php/topic,26658.15.html>). 

To use it in your application copy bindings to your project directory and add **niscope.pas** to USES section. 

## Code Examples

There are some small differences between Pascal bindings for Windows and Linux - sometimes same function parameter is variable in Linux, but in Windows is a pointer, for example in Linux somefunction(sampleRate:double):longint; in Windows somefunction(sampleRate:Pdouble):longint; 

### Linux examples

#### Autosetuped acquisition

In this example by calling niScope_AutoSetup driver automatically try to setup acquisition parameters. 
    
    
    program autosetupacq;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes, niscope, visatype,sysutils
      { you can add units after this };
    var
      resourceName:string='Dev1';
      vi:ViSession=VI_NULL;
      wfmNum:ViInt32;
      actualRecordLength:dword;
      actualSampleRate:viReal64;
      data:array of viReal64;
      channel:string = '0';
      wfmInfo:array of niScope_wfmInfo;
      i:longint;
    procedure CheckError(AErrorCode: ViStatus; Avi: viSession);
    var
      errorMessage:string='';
      bufferSize:ViStatus;
    begin
      SetLength(errorMessage,MAX_ERROR_DESCRIPTION);
      if (AErrorCode<>VI_SUCCESS) then begin
        //due on unkown string length niScope_GetErrorMessage should call twice
        //get buffer size
        bufferSize:=niScope_GetError(Avi,@AErrorCode,0,@errorMessage[1]);
        //read error message
        SetLength(errorMessage,bufferSize);
        niScope_GetError(Avi,@AErrorCode,Length(errorMessage),@errorMessage[1]);
        writeln(errorMessage);
        if Avi<>VI_NULL then begin
           niScope_close(Avi);
        end;
        Halt;
      end;
     end;
    begin
      //Init Session
      CheckError(niScope_init(PChar(@resourceName[1]),NISCOPE_VAL_FALSE,NISCOPE_VAL_FALSE,@vi),vi);
      writeln('Vi handler value (should be 1):' ,vi);
      //auto setup measurement
      CheckError(niScope_AutoSetup(vi),vi);
      //read the actual record length and number of waveforms
      //setuped by autosetup
      CheckError(niScope_ActualRecordLength(vi,@actualRecordLength),vi);
      CheckError(niScope_SampleRate(vi,@actualSampleRate),vi);
      CheckError(niScope_ActualNumWfms(vi,PChar(@channel[1]),@wfmNum),vi);
    
      //set buffer for data (lenght of data array)
      SetLength(data,actualRecordLength*wfmNum);
      SetLength(wfmInfo,wfmNum);
    
      CheckError(niScope_Read(vi,PChar(@channel[1]),5.0,actualRecordLength,@data[0],@wfmInfo[0]),vi);
      //shows first 10 samples
      writeln('----first ten samples of record------------------');
      for i:=Low(data) to Low(data)+9 do writeln('sample[',i,'] = ',data[i]:4:2);
      //show Info
      writeln('---------------------------------------------------');
      writeln('Record lenght: ',actualRecordLength);
      writeln('Sample rate: ',actualSampleRate);
      writeln('Number of waveforms: ',wfmNum);
      //show Info from wfmInfo record
      writeln('Actual samples: ',wfmInfo[0].actualSamples);
      writeln('X Increament: ',wfmInfo[0].xIncrement);
      writeln('Gain: ',wfmInfo[0].Gain);
      //close Session
      if vi<>VI_NULL then niScope_close(vi)
    end.
    

#### Binary acquisition

Use an appropriate **niScope_FetchBinary8** or **niScope_FetchBinary16** or **niScope_FetchBinary32** function passed to your card to get right signal samples. 
    
    
    program binaryacq;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes,niscope,visatype
      { you can add units after this };
     var
       error:ViStatus = VI_SUCCESS;
       vi:ViSession;
       //Variables setuped hardware
       resourceName:string = 'Dev1';
       channelName:string = '0,1';  //measurement channel(s)
       verticalRange:ViReal64 = 10;//measurement range -5 - 5 V
       verticalOffset:ViReal64 = 0.0;
       minSampleRate:ViReal64 = 2e5;
       minRecordLength:ViInt32 = 1000;
       binaryDataType:ViInt32;
       numWaveform:ViInt32;
       actualRecordLength:ViInt32;
       //Default values
       triggerType:ViInt32 = 0; //Immediate trigger
       triggerCoupling:ViInt32 = NISCOPE_VAL_DC;
       triggerSlope:ViInt32 = NISCOPE_VAL_POSITIVE;
       triggerLevel:ViReal64 = 0.0;
       triggerHoldoff:ViReal64 = 0.0;
       triggerDelay:ViReal64 = 0.0;
       triggerSource:string = '0';//nastavit kanal pro trigger
       refPosition:ViReal64 = 50.0;
       timeout:ViReal64 = 5.0;
       stop:ViInt32;
       i,j:ViInt32;
    
       wfmInfo:array of niScope_wfmInfo;
       scaledWfm:array of ViReal64;
       //NI-5122 is 14-bit digitizer
       binaryWfm:array of ViInt16;
    procedure CheckError(AErrorCode: ViStatus; Avi: viSession);
    var
      errorMessage:string='';
      bufferSize:ViStatus;
    begin
      SetLength(errorMessage,MAX_ERROR_DESCRIPTION);
      if (AErrorCode<>VI_SUCCESS) then begin
        //due on unkown string length niScope_GetErrorMessage should call twice
        //get buffer size
        bufferSize:=niScope_GetError(Avi,@AErrorCode,0,@errorMessage[1]);
        //read error message
        SetLength(errorMessage,bufferSize);
        niScope_GetError(Avi,@AErrorCode,Length(errorMessage),@errorMessage[1]);
        writeln(errorMessage);
        if Avi<>VI_NULL then begin
           niScope_close(Avi);
        end;
        Halt;
      end;
     end;
    begin
       //Open the NI_SCOPE instrument handler
       CheckError(niScope_init(Pchar(@resourceName[1]),NISCOPE_VAL_FALSE,NISCOPE_VAL_FALSE,@vi),vi);
    
       //Configure the vertical parameters
       CheckError(niScope_ConfigureVertical(vi,PChar(@channelName[1]),verticalRange,verticalOffset,NISCOPE_VAL_DC,1.0,NISCOPE_VAL_TRUE),vi);
       //Configure the horizontal parameters
       //one record
    
       CheckError(niScope_ConfigureHorizontalTiming(vi,minSampleRate,minRecordLength,refPosition,1,VI_TRUE),vi);
       //Configure the trigger
    
       if triggerType<>0  //Edge
         then CheckError(niScope_ConfigureTriggerEdge(vi,PChar(@triggerSource[1]),triggerLevel,triggerSlope,triggerCoupling,triggerHoldoff,triggerDelay),vi)
         else CheckError(niScope_ConfigureTriggerImmediate(vi),vi);
    
       //Get number of waveforms
       CheckError(niScope_ActualNumWfms(vi,PChar(@channelName[1]),@numWaveform),vi);
    
       //Get record length
       CheckError(niScope_ActualRecordLength(vi,@ActualRecordLength),vi);
       writeln('Record length: ',ActualRecordLength);
    
       //Set buffer for data and waveform Info
       SetLength(binaryWfm,ActualRecordLength*numWaveform);
       SetLength(scaledWfm,ActualRecordLength*numWaveform);
       SetLength(wfmInfo,numWaveform);
    
       //signal acquisition  1 record
       stop:=NISCOPE_VAL_FALSE;
       while stop<>NISCOPE_VAL_TRUE do begin
         CheckError(niScope_InitiateAcquisition(vi),vi);
         CheckError(niScope_FetchBinary16(vi,PChar(@channelName[1]),timeout,actualRecordLength,@binaryWfm[0],@wfmInfo[0]),vi);
         stop:=NISCOPE_VAL_TRUE;
       end;
       niScope_Close(vi);
       //calculate scaled data from binary data
       for i:=0 to NumWaveform-1 do
         for j:=0 to ActualRecordLength-1 do scaledWfm[j+(i*ActualRecordLength-1)]:=binaryWfm[j+(i*ActualRecordLength-1)]*wfmInfo[i].Gain+wfmInfo[i].offset;
    
       for i:=0 to NumWaveform-1 do begin
         writeln('   Waveform channel   ',i+1);
         writeln('   First 10 samples ');
         writeln('----------------------');
         for j:=0 to 10 do writeln('value[',j,'] = ',scaledWfm[j+(i*ActualRecordLength-1)]:4:2);
         writeln('----------------------');
      end;
       writeln('program stoped');
    end.
    

### Windows examples

#### Autosetuped acquisition

In this example by calling niScope_AutoSetup driver automatically try to setup acquisition parameters. 
    
    
    program autosetupacq;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes, niscope, visatype,sysutils
      { you can add units after this };
    var
      resourceName:string='Dev3';
      vi:ViSession=VI_NULL;
      wfmNum:ViInt32;
      actualRecordLength:dword;
      actualSampleRate:viReal64;
      data:array of viReal64;
      channel:string = '0';
      wfmInfo:array of niScope_wfmInfo;
      i:longint;
    procedure CheckError(AErrorCode: ViStatus; Avi: viSession);
    var
      errorMessage:string;
      bufferSize:ViStatus;
    begin
      SetLength(errorMessage,MAX_ERROR_DESCRIPTION);
      if (AErrorCode<>VI_SUCCESS) then begin
        //should call twice because we don't know errorMessage length
        //get string length
        bufferSize:=niScope_GetErrorMessage(Avi,AErrorCode,0,@errorMessage[1]);
       //get Error message
        SetLength(errorMessage,bufferSize);
        niScope_GetErrorMessage(Avi,AErrorCode,Length(errorMessage),@errorMessage[1]);
        writeln(errorMessage);
        if Avi<>VI_NULL then begin
           niScope_close(Avi);
        end;
        Halt;
      end;
     end;
    begin
      //Init Session
      CheckError(niScope_init(PChar(@resourceName[1]),NISCOPE_VAL_FALSE,NISCOPE_VAL_FALSE,@vi),vi);
      writeln('Vi handler value (should be 1):' ,vi);
      //auto setup measurement
      CheckError(niScope_AutoSetup(vi),vi);
      //read the actual record length and number of waveforms
      //setuped by autosetup
      CheckError(niScope_ActualRecordLength(vi,@actualRecordLength),vi);
      CheckError(niScope_SampleRate(vi,@actualSampleRate),vi);
      CheckError(niScope_ActualNumWfms(vi,PChar(@channel[1]),@wfmNum),vi);
    
      //set buffer for data (lenght of data array)
      SetLength(data,actualRecordLength*wfmNum);
      SetLength(wfmInfo,wfmNum);
    
      CheckError(niScope_Read(vi,PChar(@channel[1]),5.0,actualRecordLength,@data[0],@wfmInfo[0]),vi);
      //shows first 10 samples
      writeln('----first ten samples of record------------------');
      for i:=Low(data) to Low(data)+9 do writeln('sample[',i,'] = ',data[i]:4:2);
      //show Info
      writeln('---------------------------------------------------');
      writeln('Record lenght: ',actualRecordLength);
      writeln('Sample rate: ',actualSampleRate);
      writeln('Number of waveforms: ',wfmNum);
      //show Info from wfmInfo record
      writeln('Actual samples: ',wfmInfo[0].actualSamples);
      writeln('X Increament: ',wfmInfo[0].xIncrement);
      writeln('Gain: ',wfmInfo[0].Gain);
      //close Session
      if vi<>VI_NULL then niScope_close(vi)
    end.
    

#### Binary acquisition

Use an appropriate **niScope_FetchBinary8** or **niScope_FetchBinary16** or **niScope_FetchBinary32** function passed to your card to get right signal samples. 
    
    
    program binaryacq;
     
    {$mode objfpc}{$H+}
     
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes,niscope,visatype
      { you can add units after this };
     var
       error:ViStatus = VI_SUCCESS;
       vi:ViSession;
       //Variables setuped hardware
       resourceName:string = 'Dev1';
       channelName:string = '0,1';  //measurement channel(s)
       verticalRange:ViReal64 = 10;//measurement range -5 - 5 V
       verticalOffset:ViReal64 = 0.0;
       minSampleRate:ViReal64 = 2e5;
       minRecordLength:ViInt32 = 1000;
       binaryDataType:ViInt32;
       numWaveform:ViInt32;
       actualRecordLength:ViInt32;
       //Default values
       triggerType:ViInt32 = 0; //Immediate trigger
       triggerCoupling:ViInt32 = NISCOPE_VAL_DC;
       triggerSlope:ViInt32 = NISCOPE_VAL_POSITIVE;
       triggerLevel:ViReal64 = 0.0;
       triggerHoldoff:ViReal64 = 0.0;
       triggerDelay:ViReal64 = 0.0;
       triggerSource:string = '0';//nastavit kanal pro trigger
       refPosition:ViReal64 = 50.0;
       timeout:ViReal64 = 5.0;
       stop:ViInt32;
       i,j:ViInt32;
     
       wfmInfo:array of niScope_wfmInfo;
       scaledWfm:array of ViReal64;
       //NI-5122 is 14-bit digitizer
       binaryWfm:array of ViInt16;
    procedure CheckError(AErrorCode: ViStatus; Avi: viSession);
    var
      errorMessage:string='';
      bufferSize:ViStatus;
    begin
      SetLength(errorMessage,MAX_ERROR_DESCRIPTION);
      if (AErrorCode<>VI_SUCCESS) then begin
        //due on unkown string length niScope_GetErrorMessage should call twice
        //get buffer size
        bufferSize:=niScope_GetError(Avi,@AErrorCode,0,@errorMessage[1]);
        //read error message
        SetLength(errorMessage,bufferSize);
        niScope_GetError(Avi,@AErrorCode,Length(errorMessage),@errorMessage[1]);
        writeln(errorMessage);
        if Avi<>VI_NULL then begin
           niScope_close(Avi);
        end;
        Halt;
      end;
     end;
    begin
       //Open the NI_SCOPE instrument handler
       CheckError(niScope_init(Pchar(@resourceName[1]),NISCOPE_VAL_FALSE,NISCOPE_VAL_FALSE,@vi),vi);
     
       //Configure the vertical parameters
       CheckError(niScope_ConfigureVertical(vi,PChar(@channelName[1]),verticalRange,verticalOffset,NISCOPE_VAL_DC,1.0,NISCOPE_VAL_TRUE),vi);
       //Configure the horizontal parameters
       //one record
     
       CheckError(niScope_ConfigureHorizontalTiming(vi,minSampleRate,minRecordLength,refPosition,1,VI_TRUE),vi);
       //Configure the trigger
     
       if triggerType<>0  //Edge
         then CheckError(niScope_ConfigureTriggerEdge(vi,PChar(@triggerSource[1]),triggerLevel,triggerSlope,triggerCoupling,triggerHoldoff,triggerDelay),vi)
         else CheckError(niScope_ConfigureTriggerImmediate(vi),vi);
     
       //Get number of waveforms
       CheckError(niScope_ActualNumWfms(vi,PChar(@channelName[1]),@numWaveform),vi);
     
       //Get record length
       CheckError(niScope_ActualRecordLength(vi,@ActualRecordLength),vi);
       writeln('Record length: ',ActualRecordLength);
     
       //Set buffer for data and waveform Info
       SetLength(binaryWfm,ActualRecordLength*numWaveform);
       SetLength(scaledWfm,ActualRecordLength*numWaveform);
       SetLength(wfmInfo,numWaveform);
     
       //signal acquisition  1 record
       stop:=NISCOPE_VAL_FALSE;
       while stop<>NISCOPE_VAL_TRUE do begin
         CheckError(niScope_InitiateAcquisition(vi),vi);
         CheckError(niScope_FetchBinary16(vi,PChar(@channelName[1]),timeout,actualRecordLength,@binaryWfm[0],@wfmInfo[0]),vi);
         stop:=NISCOPE_VAL_TRUE;
       end;
       niScope_Close(vi);
       //calculate scaled data from binary data
       for i:=0 to NumWaveform-1 do
         for j:=0 to ActualRecordLength-1 do scaledWfm[j+(i*ActualRecordLength-1)]:=binaryWfm[j+(i*ActualRecordLength-1)]*wfmInfo[i].Gain+wfmInfo[i].offset;
     
       for i:=0 to NumWaveform-1 do begin
         writeln('   Waveform channel   ',i+1);
         writeln('   First 10 samples ');
         writeln('----------------------');
         for j:=0 to 10 do writeln('value[',j,'] = ',scaledWfm[j+(i*ActualRecordLength-1)]:4:2);
         writeln('----------------------');
      end;
       writeln('program stoped');
    end.
    

## TESTED HARDWARE

  * NI PXI-5122 tested successfully under Windows7 32-bit and Scientific Linux 6.x 32-bit.

---

_Source: [https://wiki.freepascal.org/NI-Scope_examples](https://web.archive.org/web/20220521063155/https://wiki.freepascal.org/NI-Scope_examples)_
