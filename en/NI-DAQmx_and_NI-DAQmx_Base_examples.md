# NI-DAQmx and NI-DAQmx Base examples

## Contents

  * 1 INTRODUCTION
    * 1.1 What National Instruments Hardware is supported
    * 1.2 NI-DAQmxBase library
    * 1.3 NI-DAQmx library
  * 2 HOW TO IDENTIFY PRESENT HARDWARE
  * 3 Library documentation
  * 4 PASCAL bindings
  * 5 FUNCTION CALLING
  * 6 NI-DAQmx examples
    * 6.1 DEVICE INFO
      * 6.1.1 Get device list
      * 6.1.2 Get channel names
    * 6.2 ANALOG INPUTS/OUTPUTS
      * 6.2.1 Acquire finite number of samples from one analog input
      * 6.2.2 Continuously acquire samples from three analog inputs
      * 6.2.3 Start acquisition on analog input by digital trigger
      * 6.2.4 Analog output software controlled
      * 6.2.5 Analog output hardware controlled
    * 6.3 DIGITAL INPUTS/OUTPUTS
      * 6.3.1 Software timed digital input
      * 6.3.2 Hardware timed digital input
      * 6.3.3 Software controlled digital output
      * 6.3.4 Hardware timed digital outputs with counter output used as source clock
    * 6.4 COUNTER INPUTS/OUTPUTS
      * 6.4.1 Pulse generation
      * 6.4.2 Event counting
    * 6.5 TESTED HARDWARE
  * 7 NI-DAQxmxBase examples
    * 7.1 DEVICE INFO
    * 7.2 ANALOG INPUTS/OUTPUTS
      * 7.2.1 Acquire finite number of samples from one analog channel
      * 7.2.2 Continuously acquire samples from three analog inputs
    * 7.3 DIGITAL INPUTS/OUTPUTS
      * 7.3.1 Software timed digital input
      * 7.3.2 Hardware timed digital input
      * 7.3.3 Software timed digital output
      * 7.3.4 Hardware timed digital output
    * 7.4 COUNTER INPUTS/OUTPUTS
      * 7.4.1 COUNTER OUTPUT
      * 7.4.2 COUNTER INPUT
    * 7.5 TESTED HARDWARE



## INTRODUCTION

National Instruments produce a wide range of [DAQ cards](<http://www.ni.com/data-acquisition/multifunction/>), which are generally used for acquiring and generating signals. 

These cards usually have a few analog inputs/outputs, digital inputs/outputs, counters and a frequency generator with hardware/software timing. Exact features (type, number, parameters) depends on the card type. 

NIDAQmxBase.pas and NIDAQmx.pas are provided pascal bindings to National Instruments libraries and enable control of NI DAQ cards from programs written in Free Pascal. 

### What National Instruments Hardware is supported

[Supported hardware list](<http://www.ni.com/white-paper/6910/en/>) by NI-DAQmx and NI-DAQ Driver for different operating systems. 

### NI-DAQmxBase library

NI-DAQmxBase library or driver is multiplatform library for Windows (Windows XP 32 bit, Windows 7 32/64 bit, Windows Vista 32/64 bit, Windows 8 32/64 bit), macOS 32/64 bit and Linux 32/64 bit (SUSE, Scientific Linux, RedHat, CentOS), but doesn't implement all device features. For example, digital input/output ports speed is limited by computer speed ~100kHz, because DMA data transfer is not supported. 

Download library [from here](<https://www.ni.com/en-au/support/downloads/drivers/download.ni-daqmx-base.html>). Read [readme file](<http://download.ni.com/support/softlib/multifunction_daq/nidaqmxbase/15.0/windows/readme.htm>) to find the list of supported hardware and hardware features. 
    
    
      Latest Linux version 15.0 supports 64-bit OS, but you still need to install some 32-bit library. 
      
      LIST OF LIBRARIES:
    
      compat-libstdc++.i686
      expat.i686
      glibc.i686
      glibc-devel.i686
      libdrm.i686
      libgcc.i686
      libselinux.i686
      libstdc++.i686
      libX11.i686
      libXau.i686
      libxcb.i686
      libXdamage.i686
      libXext.i686
      libXinerama.i686
      libXfixes.i686
      libXxf86vm.i686
      mesa-dri-drivers.i686
      mesa-libGL.i686
      nss-softokn-freebl.i686
      zlib.i686
    

### NI-DAQmx library

The NI-DAQmx library is only available for Windows 7 (SP1) 32/64 bit, Windows 8.1 32/64 bit, Windows 10 32/64 bit, Windows Embedded Standard 7 (SP1), Windows Server 2008 R2 (SP1) 32/64 bit). [Download here)](<https://www.ni.com/en-au/support/downloads/drivers/download.ni-daqmx.html>)

Read [readme file](<http://download.ni.com/support/softlib/multifunction_daq/nidaqmx/9.0/readme.html>) to find the list of supported hardware and hardware features. 

## HOW TO IDENTIFY PRESENT HARDWARE

To get list of devices present in your system Under Linux operation system use 
    
    
    1) **nilsdev** utility with NIDAQmx driver installed,
    2) **lsdaq** utility with NIDAQmxBase driver installed.
    

Under Windows use **NIMAX** utility to get device list or set device alias etc. 

## Library documentation

You can find description of functions and properties in NI-DAQmx and NI-DAQmx Base C reference Help: 

  * [NI-DAQmx](<http://digital.ni.com/manuals.nsf/websearch/934F66A25EB3831586257AFD00741B13?OpenDocument&seen=1>)
  * [NI-DAQmx Base](<http://digital.ni.com/manuals.nsf/websearch/C4B5B92E74F160C6862574560065F8BC>)



## PASCAL bindings

Your can download pascal bindings nidaqmxbase.pas and nidaqmx.pas in this forum thread [[1]](<http://forum.lazarus.freepascal.org/index.php/topic,7894.msg161883.html#msg161883>) or as a [part of the LazMer](<http://sourceforge.net/p/lazmer/code/HEAD/tree/trunk/daqlibs/>) project. 

## FUNCTION CALLING

Generally, when you need to acquire or gerenate data, call functions from **nidaqmx.pas** as follow 
    
    
    1. Create task: DAQmxCreateTask
    2. Channel create: 
       a)DAQmxCreateDIChan/DAQmxCreateDOChan (digital input(s)/output(s)) 
       b)DAQmxCreateAIVoltageChan/DAQmxCreateAOVoltageChan (analog input(s)/output(s))
    3. Set timing if you need hardware timimg: DAQmxCfgSamplClkTiming
    4. Start task: DAQmxStartTask
    5. Read or write data from/to channel: 
       a)read data from digital port: DAQmxReadDigitalU8(8-bit port),DAQmxReadDigitalU32 (32-bit port),
       b)write data to digital port:  DAQmxWriteDigitalU8(8-bit port),DAQmxWriteDigitalU32 (32-bit port), 
       c)read data from analog input(s): DAQmxReadAnalogF64,
       d)write data to analog output(s): DAQmxWriteAnalogF64.
    6. Do job
    7. End of program - stop and clear task: DAQmxStopTask and DAQmxClearTask
    
    For error handling create function like ErrorCheck.
    

from **nidaqmxbase.pas** as follow 
    
    
    1. Create task: DAQmxBaseCreateTask
    2. Channel create: 
       a)DAQmxBaseCreateDIChan/DAQmxBaseCreateDOChan (digital input(s)/output(s)) 
       b)DAQmxBaseCreateAIVoltageChan/DAQmxBaseCreateAOVoltageChan (analog input(s)/output(s))
    3. Set timing if you need hardware timimg: DAQmxBaseCfgSamplClkTiming
    4. Start task: DAQmxBaseStartTask
    5. Read or write data from/to channel: 
       a)read data from digital port: DAQmxBaseReadDigitalU8(8-bit port),DAQmxBaseReadDigitalU32 (32-bit port),
       b)write data to digital port:  DAQmxBaseWriteDigitalU8(8-bit port),DAQmxBaseWriteDigitalU32 (32-bit port), 
       c)read data from analog input(s): DAQmxBaseReadAnalogF64,
       d)write data to analog output(s): DAQmxBaseWriteAnalogF64.
    6. Do job
    7. End of program - stop and clear task: DAQmxBaseStopTask and DAQmxBaseClearTask
    
    For error handling create function like ErrorCheck.
    
    
    
     Probably, some examples need to add @ before variable, which use as function parameter!!!
    

## NI-DAQmx examples

### DEVICE INFO

#### Get device list

You can check device list names using NI MAX program under Windows or nilsdev utility under Linux. 
    
    
    program devicelist;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes,nidaqmx,SysUtils
      { you can add units after this };
    var
      devicenames:array of Char;
      infostr:string;
      DeviceNamesList:TStringList;
      buffersize:longint;
      product_type:array of char;
      devserialnum,is_simulated:longint;
      is_simulated_str:string;
      i:byte;
    begin
      DeviceNamesList:=TStringList.Create;
      try
         {**************************************************************************
                          Get device names present in the system
         **************************************************************************}
         //get buffer size or length of string of device list
         buffersize:=DAQmxGetSystemInfoAttribute(DAQmx_Sys_DevNames,devicenames[0],[0]);
         //set array of Char lenght
         if Buffersize<>0 then begin
           writeln('-------------------------------------------------------------');
           writeln('|Device name | Product Type | # Serial number | Is Simulated?');
           writeln('-------------------------------------------------------------');
           SetLength(devicenames,buffersize);
           //get the device list
           DAQmxGetSystemInfoAttribute(DAQmx_Sys_DevNames,devicenames[0],[buffersize]);
           //convert it to string: names are separated by comma
           SetString(infostr,PChar(@devicenames[0]),buffersize);
           //add names to TStringList
           DeviceNamesList.CommaText:=infostr;
           {************************************************************************
                                  Get Product info
            ***********************************************************************}
           for i:=0 to DeviceNamesList.Count-1 do begin
              //get buffer size or length of string of product type
              buffersize:=DAQmxGetDeviceAttribute(@DeviceNamesList.Strings[i][1],DAQmx_Dev_ProductType,Nil);
              //set lenght of array of char
              SetLength(product_type,buffersize);
              //get product type info
              DAQmxGetDeviceAttribute(@DeviceNamesList.Strings[i][1],DAQmx_Dev_ProductType,product_type[0],[buffersize]);
              {***********************************************************************
                                Get Serial Number
              **********************************************************************}
              DAQmxGetDeviceAttribute(@DeviceNamesList.Strings[i][1],DAQmx_Dev_SerialNum,devserialnum,[1]);
              //DeviceInfoGrid.Cells[3,i+1]:=IntToHex(devserialnum,8);
              {***********************************************************************
                                 Is simulated?
              ***********************************************************************}
              DAQmxGetDeviceAttribute(@DeviceNamesList.Strings[i][1],DAQmx_Dev_IsSimulated,is_simulated,[1]);
              if is_simulated = 0 then is_simulated_str:='No'
              else is_simulated_str:='Yes';
              //Show info
              writeln('| ',DeviceNamesList.Strings[i],' | ',PChar(@product_type[0]),' | $',IntToHex(devserialnum,8),' |',is_simulated_str);
          end;
      end
      else writeln('any device found');
      finally
         DeviceNamesList.Free;
      end;
    end.
    

Output example 
    
    
    -------------------------------------------------------------
    |Device name | Product Type | # Serial number | Is Simulated?
    -------------------------------------------------------------
    | Dev1 | PCI-6250 | $018D0B90 |No
    

#### Get channel names
    
    
    program chaninfo;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes,nidaqmx
      { you can add units after this };
    var
      buffersize,infotype:longint;
      channelnames:array of char;
      MsgChannelNames:array [0..255] of char;
      MsgNoChannelsText:array [0..255] of char;
      channeltype:byte;
      DeviceName:string;
    begin
     {**********INPUT Device name and interested channel type **********************
      **********WITHOUT CHEKING IF DEVICE EXITS IN THE SYSTEM**********************}
      write('Enter device name, for example Dev1, Dev2 etc.: ');
      readln(Devicename);
      writeln('0 - analog inputs');
      writeln('1 - analog outputs');
      writeln('2 - digital inputs');
      writeln('3 - digital outputs');
      writeln('4 - counter inputs');
      writeln('5 - counter outputs');
      write('choose the number of required channel type:');
      readln(channeltype);
      {**********CHANNEL TYPE CHOSEN**********************************************}
      case channeltype of
      {********************ANALOG INPUTS NAMES*************************************}
      0: begin
          infotype:=DAQmx_Dev_AI_PhysicalChans;
          MsgChannelNames:='Analog input names';
          MsgNoChannelsText:='There are no analog inputs';
      end;
      {********************ANALOG OUTPUTS NAMES************************************}
      1: begin
          infotype:=DAQmx_Dev_AO_PhysicalChans;
          MsgChannelNames:='Analog output names';
          MsgNoChannelsText:='There are no analog outputs';
      end;
      {********************DIGITAL INPUT PORTS NAMES*******************************}
      2: begin
          infotype:=DAQmx_Dev_DI_Ports;
          MsgChannelNames:='Digital input ports';
          MsgNoChannelsText:='There are no digital input ports';
      end;
      {********************DIGITAL OUTPUT PORTS NAMES******************************}
      3:begin
         infotype:=DAQmx_Dev_DO_Ports;
         MsgChannelNames:='Digital output ports';
         MsgNoChannelsText:='There are no digital output ports';
      end;
      {********************COUNTER INPUT  NAMES******************************}
      4:begin
         infotype:= DAQmx_Dev_CI_PhysicalChans;
         MsgChannelNames:='Digital counter inputs';
         MsgNoChannelsText:='There are no counter inputs ';
      end;
      {********************COUNTER OUTPUT  NAMES******************************}
      5:begin
         infotype:= DAQmx_Dev_CO_PhysicalChans;
         MsgChannelNames:='Digital counter outputs';
         MsgNoChannelsText:='There are no counter outputs ';
      end;
     end;
    {************GET CHANNEL NAMES*********************************************}
     if (channeltype>=0) and (channeltype<6) then begin
             //get buffer size or length of channelnames
             buffersize:=DAQmxGetDeviceAttribute(@DeviceName[1],infotype,Nil);
             if buffersize<>0 then begin
               SetLength(channelnames,buffersize);
               //get analog channel names separated by comma
               DAQmxGetDeviceAttribute(@DeviceName[1],infotype,channelnames[0],[buffersize]);
               //show info
               writeln(MsgChannelNames+': '+PChar(@channelnames[0]));
             end
             else writeln(MsgNoChannelsText);
      end;
    end.
    

Output example 
    
    
    Enter device name, for example Dev1, Dev2 etc.: Dev1
    0 - analog inputs                                   
    1 - analog outputs                                  
    2 - digital inputs                                  
    3 - digital outputs                                 
    4 - counter inputs                                  
    5 - counter outputs                                 
    choose the number of required channel type:0        
    Analog input names: Dev1/ai0, Dev1/ai1, Dev1/ai2, Dev1/ai3, Dev1/ai4, Dev1/ai5, Dev1/ai6, Dev1/ai7, Dev1/ai8, Dev1/ai9, Dev1/ai10, Dev1/ai11, Dev1/ai12, Dev1/ai13, Dev1/ai14, Dev1/ai15
    

### ANALOG INPUTS/OUTPUTS

#### Acquire finite number of samples from one analog input
    
    
    program aichan;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes, nidaqmx
      { you can add units after this };
    var
      AITaskHandle:TaskHandle;
      data:array[1..1000] of double;
      reads,i:longint;
    
    {************Error handling procedure *****************************************
        Example:
     ErrorCheck(DAQmxCreateAIVoltageChan(AITaskHandle,'Dev1/ai0','',DAQmx_Val_RSE,-10.0,10.0,DAQmx_Val_Volts,NIL),AITaskHandle);
    
    *******************************************************************************}
    
    procedure ErrorCheck(AError:longint;ATaskHandle:longint);
    var
      errBuffer:array[0..2048] of char;
    begin
       //get error description
       if Aerror<>0 then begin
         DAQmxGetExtendedErrorInfo(@ErrBuffer[0],2048);
         writeln(PChar(@ErrBuffer[0]));
       //stop and cleat task
       if Assigned(@ATaskHandle) then begin
         DAQmxStopTask(ATaskHandle);
         DAQmxClearTask(ATaskHandle);
       end;
       //stop program
       Halt;
      end;
    end;
    {*********************MAIN PROGRAM*********************************************
     ******************* without error handling************************************}
    begin
      //create task
      DAQmxCreateTask('',@AITaskHandle);
      //create analog input channel Dev1/ai0 (activate it)
      //voltage range -10 - 10
      DAQmxCreateAIVoltageChan(AITaskHandle,'Dev1/ai0','',DAQmx_Val_RSE,-10.0,10.0,DAQmx_Val_Volts,NIL);
      //set timing
      //Sample rate = 10000 (10 kHz), sample per channel =1000
      //acquire finite samples (1000 samples) defined by DAQmx_Val_FiniteSamps
      DAQmxCfgSampClkTiming(AITaskHandle,'OnboardClock',10000.0,DAQmx_Val_Rising,DAQmx_Val_FiniteSamps,1000);
      //start task
      DAQmxStartTask(AITaskHandle);
      //acquire 1000 samples
      DAQmxReadAnalogF64(AITaskHandle,1000,10.0,DAQmx_Val_GroupByChannel,@data[1],1000,@reads,NIL);
      //show first 10 samples
      writeln(reads,' samples were acquired');
      writeln('');
      writeln('Value of first 10 samples');
      for i:=1 to 10 do writeln('sample[',i,'] = ',data[i]:4:2,' V');
      //stop and clear task
      DAQmxStopTask(AITaskHandle);
      DAQmxClearTask(AITaskHandle);
    end.
    

Output example 
    
    
    1000 samples were acquired
    
    Value of first 10 samples
    sample[1] = 0.23 V
    sample[2] = 0.23 V
    sample[3] = 0.23 V
    sample[4] = 0.23 V
    sample[5] = 0.23 V
    sample[6] = 0.23 V
    sample[7] = 0.23 V
    sample[8] = 0.23 V
    sample[9] = 0.23 V
    sample[10] = 0.23 V
    

#### Continuously acquire samples from three analog inputs
    
    
    program multiaichannel;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes, nidaqmx,crt
      { you can add units after this };
    var
      AITaskHandle:TaskHandle;
      data:array of double;
      reads:longint;
      totalsamplereads:longint=0;
      i:longint;
    
    {************Error handling procedure *****************************************
        Example:
     ErrorCheck(DAQmxCreateAIVoltageChan(AITaskHandle,'Dev1/ai0','',DAQmx_Val_RSE,-10.0,10.0,DAQmx_Val_Volts,NIL),AITaskHandle);
    
    *******************************************************************************}
    
    procedure ErrorCheck(AError:longint;ATaskHandle:longint);
    var
      errBuffer:array[0..2048] of char;
    begin
       //get error description
       if Aerror<>0 then begin
         DAQmxGetExtendedErrorInfo(@ErrBuffer[0],2048);
         writeln(PChar(@ErrBuffer[0]));
       //stop and cleat task
       if Assigned(@ATaskHandle) then begin
         DAQmxStopTask(ATaskHandle);
         DAQmxClearTask(ATaskHandle);
       end;
       //stop program
       Halt;
      end;
    end;
    {*********************MAIN PROGRAM*********************************************
     ******************* without error handling************************************}
    begin
      //Set buffer size for data acquiring, sample per channel 1000
      //buffer size(data -array variable) = 3 channel * 1000 samples per channel
      Setlength(data,3*1000);
      //create task
      DAQmxCreateTask('',@AITaskHandle);
      //create analog input channel Dev1/ai0:2 (activate 3 channels)
      //voltage range -10 - 10
      DAQmxCreateAIVoltageChan(AITaskHandle,'Dev1/ai0:2','',DAQmx_Val_RSE,-10.0,10.0,DAQmx_Val_Volts,NIL);
      //set timing
      //Sample rate = 10000 (10 kHz), sample per channel =1000
      //acquire finite samples (1000 samples) defined by DAQmx_Val_Cont_Samps
      DAQmxCfgSampClkTiming(AITaskHandle,'OnboardClock',10000.0,DAQmx_Val_Rising,DAQmx_Val_ContSamps,1000);
      //start task
      DAQmxStartTask(AITaskHandle);
      //acquire 10000 samples for each channel
      while totalsamplereads<10000 do begin
        //1000- sample rate, 3000 - buffer size(data -array variable) = 3 channel * 1000 samples per channel
        DAQmxReadAnalogF64(AITaskHandle,1000,10.0,DAQmx_Val_GroupByChannel,@data[0],3000,@reads,NIL);
        totalsamplereads:=totalsamplereads+reads;
        writeln('aquired number of samples: ',totalsamplereads);
      end;
      //show first 10 samples from last 1000 samples acquired from each channel
      writeln('');
      writeln('Value of first 10 samples of last 1000 samples for each channel');
      writeln('| chan0 |  chan1 |  chan2 | ');
      for i:= 0 to 9 do writeln('|',data[i]:5:2,' V| ',data[i+1000]:5:2,' V| ',data[i+2000]:5:2,' V|');
      //stop and clear task
      DAQmxStopTask(AITaskHandle);
      DAQmxClearTask(AITaskHandle);
    end.
    

Output example 
    
    
    acquired number of samples: 1000                                
    aquired number of samples: 2000                                
    acquired number of samples: 3000                                
    acquired number of samples: 4000                                
    acquired number of samples: 5000                                
    acquired number of samples: 6000                                
    acquired number of samples: 7000                                
    acquired number of samples: 8000                                
    acquired number of samples: 9000                                
    acquired number of samples: 10000
    
    Value of first 10 samples of last 1000 samples for each channel
    | chan0 |  chan1 |  chan2 |
    | 5.07 V|  5.07 V|  5.06 V|
    | 5.07 V|  5.07 V|  5.06 V|
    | 5.07 V|  5.06 V|  5.07 V|
    | 5.06 V|  5.06 V|  5.07 V|
    | 5.06 V|  5.07 V|  5.07 V|
    | 5.07 V|  5.07 V|  5.06 V|
    | 5.07 V|  5.07 V|  5.07 V|
    | 5.06 V|  5.06 V|  5.07 V|
    | 5.06 V|  5.06 V|  5.06 V|
    | 5.07 V|  5.07 V|  5.06 V|
    

#### Start acquisition on analog input by digital trigger

This example demonstrates how to start acquisition by digital trigger. 
    
    
    program aidigtrig;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes,nidaqmx,crt
      { you can add units after this };
    var
       AITaskHandle:TaskHandle;
      channel:string = 'Dev1/ai0';
      SampleRate:double = 10000;
      SamplePerChannel:longint = 1000;
      PretriggerSamples: longint= 100; //MUST be > 2
      reads:longint = 0;
      i:longint;
      data:array of double;
    procedure ErrorCheck(AError:longint;ATaskHandle:longint);
    var
      errBuffer:array[0..2048] of char;
    begin
      //get error description
      if Aerror<>0 then begin
         DAQmxGetExtendedErrorInfo(@ErrBuffer[0],2048);
         writeln(ErrBuffer);
         //stop and cleat task
         if Assigned(@ATaskHandle) then begin
           DAQmxStopTask(ATaskHandle);
           DAQmxClearTask(ATaskHandle);
         end;
         //stop acquisition
         writeln('Program stoped');
         Halt;
      end;
    end;
    
    begin
        //set buffer length
        SetLength(data,SamplePerChannel);
        //create take
        DAQmxCreateTask('',@AITaskHandle);
        //create ai channel
        ErrorCheck(DAQmxCreateAIVoltageChan(AITaskHandle,@channel[1],'',DAQmx_Val_RSE,-10.0,10.0,DAQmx_Val_Volts,Nil),AITaskHandle);
        //configure sample clock
        ErrorCheck(DAQmxCfgSampClkTiming(AITaskHandle,'',SampleRate,DAQmx_Val_Rising,DAQmx_Val_FiniteSamps,SamplePerChannel),AITaskHandle);
        //configure triger with 100 pretriggered samples
        ErrorCheck(DAQmxCfgDigEdgeRefTrig(AITaskHandle,'/Dev1/PFI0',DAQmx_Val_Rising,PretriggerSamples),AITaskHandle);
        //start task
        ErrorCheck(DAQmxStartTask(AITaskHandle),AITaskHandle);
        //acquire data
        writeln('Press any key to stop acquisition');
        i := 0;
        repeat
          ErrorCheck(DAQmxReadAnalogF64(AITaskHandle,SamplePerChannel,10.0,DAQmx_Val_GroupByChannel,@data[0],Length(data),@reads,Nil),AITaskHandle);
          i := i + reads;
          writeln('Number of acquired samples: ', i);
        until keypressed;
        //show last readed number of samples
        for i:=Low(data) to High(data) do writeln('data[',i,'] = ', data[i]:4:2);
    
        //stop and clear task
        if Assigned(@AITaskHandle) then begin
          DAQmxStopTask(AITaskHandle);
          DAQmxClearTask(AITaskHandle);
        end;
    end.
    

#### Analog output software controlled

This example demonstrates sinusoidal waveform generation with software timed control analog output. 
    
    
    program aochansw;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes,Crt,SysUtils,nidaqmx
      { you can add units after this };
    var
      AOTaskHandle:TaskHandle;
      //data writted to analog output
      data:array [0..999] of double;
      writtensamples:integer;
      i:integer;
    
     {************Error handling procedure *****************************************
        Example:
     ErrorCheck(DAQmxCreateAIVoltageChan(AITaskHandle,'Dev1/ai0','',DAQmx_Val_RSE,-10.0,10.0,DAQmx_Val_Volts,NIL),AITaskHandle);
     *******************************************************************************}
    procedure ErrorCheck(AError:longint;ATaskHandle:longint);
    var
      errBuffer:array[0..2048] of char;
    begin
       //get error description
       if Aerror<>0 then begin
         DAQmxGetExtendedErrorInfo(@ErrBuffer[0],2048);
         writeln(PChar(@ErrBuffer[0]));
       //stop and cleat task
       if Assigned(@ATaskHandle) then begin
         DAQmxStopTask(ATaskHandle);
         DAQmxClearTask(ATaskHandle);
       end;
       //stop program
       Halt;
      end;
    end;
    
    begin
      //prepare data for analog output
      //sin waveform
      for i:=Low(data) to High(data) do data[i]:=9.95*sin(i*2.0*Pi/1000.0);
      //create task
      DAQmxCreateTask('',@AOTaskHandle);
      //configure analog output channel
      ErrorCheck(DAQmxCreateAOVoltageChan(AOTaskHandle,'Dev1/ao0','',-10.0,10.0,DAQmx_Val_Volts,Nil),AOTaskHandle);
      //start task
      ErrorCheck(DAQmxStartTask(AOTaskHandle),AOTaskHandle);
    
      writeln('Press any key to stop generation');
      //continuosly waveform generation
      //if you call task start function (DAQmxStartTask)
      //before output write function (for example DAQmxWriteAnalogScalarF64)
      //set AutoStart parameter to 1
      //if output function berofe start AutoStart = 0
      i:=0;
      repeat
        sleep(1);
        ErrorCheck(DAQmxWriteAnalogScalarF64(AOTaskHandle,1,10.0,data[i],Nil),AOTaskHandle);
        inc(i);
        if i >=1000 then i:=0;
      until keypressed;
    
      //stop and clear task
      if Assigned(@AOTaskHandle) then begin
        DAQmxStopTask(AOTaskHandle);
        DAQmxClearTask(AOTaskHandle);
      end;
    end.
    

#### Analog output hardware controlled

This example demonstrates how to generate finite number of samples on analog output channel. 
    
    
    program aochannel;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes,nidaqmx
      { you can add units after this };
    
    var
      AOTaskHandle:TaskHandle;
      //data writted to analog output
      data:array [0..3999] of double;
      writtensamples:integer;
      i:integer;
    
     {************Error handling procedure *****************************************
        Example:
     ErrorCheck(DAQmxCreateAIVoltageChan(AITaskHandle,'Dev1/ai0','',DAQmx_Val_RSE,-10.0,10.0,DAQmx_Val_Volts,NIL),AITaskHandle);
    
    *******************************************************************************}
    
    procedure ErrorCheck(AError:longint;ATaskHandle:longint);
    var
      errBuffer:array[0..2048] of char;
    begin
       //get error description
       if Aerror<>0 then begin
         DAQmxGetExtendedErrorInfo(@ErrBuffer[0],2048);
         writeln(PChar(@ErrBuffer[0]));
       //stop and cleat task
       if Assigned(@ATaskHandle) then begin
         DAQmxStopTask(ATaskHandle);
         DAQmxClearTask(ATaskHandle);
       end;
       //stop program
       Halt;
      end;
    end;
    
    {*********************MAIN PROGRAM*********************************************
     }
    begin
      //prepare data for analog output
      for i:=Low(data) to High(data) do data[i]:= 5.0*i/4000.0;
      //create task
      DAQmxCreateTask('',@AOTaskHandle);
      //configure analog output channel
      ErrorCheck(DAQmxCreateAOVoltageChan(AOTaskHandle,'Dev1/ao0','',-10.0,10.0,DAQmx_Val_Volts,Nil),AOTaskHandle);
      //configure clock
      ErrorCheck(DAQmxCfgSampClkTiming(AOTaskHandle,'',1000.0,DAQmx_Val_Rising,DAQmx_Val_FiniteSamps,4000),AOTaskHandle);
      //write data to buffer
      ErrorCheck(DAQmxWriteAnalogF64(AOTaskHandle,4000,0,10.0,DAQmx_Val_GroupByChannel,data,@writtensamples,Nil),AOTaskHandle);
      //start task
      ErrorCheck(DAQmxStartTask(AOTaskHandle),AOTaskHandle);
      //wait until all samples are generated
      ErrorCheck(DAQmxWaitUntilTaskDone(AOTaskHandle,10.0),AOTaskHandle);
      //stop and clear task
      if Assigned(@AOTaskHandle) then begin
        DAQmxStopTask(AOTaskHandle);
        DAQmxClearTask(AOTaskHandle);
      end;
    end.
    

### DIGITAL INPUTS/OUTPUTS

Most of DAQ cards have two types of digital ports (input/output): correlated and static. Static ports are software timed, correlated are software or hardware timed by the external or internal clock for example couter output signal (**/Dev1/Ctr0InternalOutput** \- couter0 as clock ). Check the documention to find it, for example NI-PCI6250 have three 8-bit ports: port0 - correlated, port1,port2 - static; NI-PCI6224 two ports: 32-bit port0 - correlated, 8-bit port1 - static. 

Read/Write data from/to 8-bit and 32-bit digital port use an appropriate function **DAQmxReadDigitalU8** , **DAQmxWriteDigitalU8** and **DAQmxReadDigitalU32** , **DAQmxWriteDigitalU32**. 

_Different behaviour of collerated port0, when it sets as input, occured under Linux and Windows: software timed works only under Windows, with external clock works under Windows and Linux!!!_

#### Software timed digital input

To check it, connected +5V(depend on hardware) to one of available port pins. 
    
    
    program diport;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes,nidaqmx,SysUtils,StrUtils
      { you can add units after this };
    var
      DITaskHandle:TaskHandle;
      data:array [0..9] of byte;
      reads:longint = 0;
      totalreads:longint = 0;
      i:longint ;
    {************Error handling procedure *****************************************
          Example:
       ErrorCheck(DAQmxCreateAIVoltageChan(AITaskHandle,'Dev1/ai0','',DAQmx_Val_RSE,-10.0,10.0,DAQmx_Val_Volts,NIL),AITaskHandle);
    
      *******************************************************************************}
    
      procedure ErrorCheck(AError:longint;ATaskHandle:longint);
      var
        errBuffer:array[0..2048] of char;
      begin
         //get error description
         if Aerror<>0 then begin
           DAQmxGetExtendedErrorInfo(@ErrBuffer[0],2048);
           writeln(PChar(@ErrBuffer[0]));
         //stop and cleat task
         if Assigned(@ATaskHandle) then begin
           DAQmxStopTask(ATaskHandle);
           DAQmxClearTask(ATaskHandle);
         end;
         //stop program
         Halt;
        end;
      end;
    {*******************MAIN PROGRAM WITH ERROR HANDLING*************************}
    begin
      //task create
      ErrorCheck(DAQmxCreateTask('',@DITaskHandle),DITaskHandle);
      //digital input port create
      ErrorCheck(DAQmxCreateDIChan(DITaskHandle,'Dev1/port1','',DAQmx_Val_ChanForAllLines),DITaskHandle);
      //Start Task
      ErrorCheck(DAQmxStartTask(DITaskHandle),DITaskHandle);
      //read 10 samples from 8-bit digital port (for example NI PCI-6250 card port1(pins P1.0-P1.7))
      //for 32-bit digital port use DAQmxReadDigitalU32 function
      ErrorCheck(DAQmxReadDigitalU8(DITaskHandle,10,10.0,DAQmx_Val_GroupByChannel,@data,10,@reads,Nil),DITaskHandle);
      writeln('number of readed samples: ',reads);
      for i:=0 to 9 do writeln('data [',i,'] = %',IntToBin(data[i],8));
      //stop and clear task
      if DITaskHandle<>0 then begin
        DAQmxStopTask(DITaskHandle);
        DAQmxClearTask(DITaskHandle);
      end;
    end.
    

Output example: +5V connected to pin P1.2 port1 on NI-PCI6250 card. 
    
    
    number of readed samples: 10
    data [0] = %00000100
    data [1] = %00000100
    data [2] = %00000100
    data [3] = %00000100
    data [4] = %00000100
    data [5] = %00000100
    data [6] = %00000100
    data [7] = %00000100
    data [8] = %00000100
    data [9] = %00000100
    

#### Hardware timed digital input

This example shows using of counter output signal as a clock for digital inputs port for hardware timed acquire data on digital inputs. This feature is supported only for correlated digital port. In this example 8-bit port0 on NI-PCI6250 card is used to check this function. 

To check it, connected +5V(depend on hardware) to one of available pins on correlated digital port. 
    
    
    program diporttimed;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes,nidaqmx,SysUtils,StrUtils
      { you can add units after this };
    var
      DITaskHandle,CtrTaskHandle:TaskHandle;
      data:array [0..9] of byte;
      reads:longint = 0;
      totalreads:longint = 0;
      i:longint ;
    {************Error handling procedure *****************************************
          Example:
       ErrorCheck(DAQmxCreateAIVoltageChan(AITaskHandle,'Dev1/ai0','',DAQmx_Val_RSE,-10.0,10.0,DAQmx_Val_Volts,NIL),AITaskHandle);
    
      *******************************************************************************}
    
      procedure ErrorCheck(AError:longint;ATaskHandle:longint);
      var
        errBuffer:array[0..2048] of char;
      begin
         //get error description
         if Aerror<>0 then begin
           DAQmxGetExtendedErrorInfo(@ErrBuffer[0],2048);
           writeln(PChar(@ErrBuffer[0]));
         //stop and cleat task
         if Assigned(@ATaskHandle) then begin
           DAQmxStopTask(ATaskHandle);
           DAQmxClearTask(ATaskHandle);
         end;
         //stop program
         Halt;
        end;
      end;
    {*******************MAIN PROGRAM WITH ERROR HANDLING*************************}
    begin
      {******CREATE COUNTER OUTPUT TO USE IT AS CLOCK FOR DIGITAL PORT INPUTS******
      *******INTERNALLY CONNECTED ON M-SERIES as /Dev1/Ctr0InternalOutput**********}
      //create couter task
      ErrorCheck(DAQmxCreateTask('Counter Task',@CtrTaskHandle),CtrTaskHandle);
      //create counter output channel
      //Frequency 1000 Hz, duty 0.5
      ErrorCheck(DAQmxCreateCOPulseChanFreq(CtrTaskHandle,'Dev1/ctr0','',DAQmx_Val_Hz,DAQmx_Val_Low,0,1000,0.5),CtrTaskHandle);
      //timing 1000 samples per second
      ErrorCheck(DAQmxCfgImplicitTiming(CtrTaskHandle,DAQmx_Val_ContSamps,1000),CtrTaskHandle);
      //start counter task
      ErrorCheck(DAQmxStartTask(CtrTaskHandle),CtrTaskHandle);
    
      //digital input task create
      ErrorCheck(DAQmxCreateTask('Digital input task',@DITaskHandle),DITaskHandle);
      //digital input port create
      ErrorCheck(DAQmxCreateDIChan(DITaskHandle,'Dev1/port0','',DAQmx_Val_ChanForAllLines),DITaskHandle);
      //timing use counter output as clock source internally connected
      //as /Dev1/Ctr0InternalOutput
      ErrorCheck(DAQmxCfgSampClkTiming(DITaskHandle,'/Dev1/Ctr0InternalOutput',1000.0,DAQmx_Val_Rising,DAQmx_Val_FiniteSamps,1000),DITaskHandle);
      //Start digital input Task
      ErrorCheck(DAQmxStartTask(DITaskHandle),DITaskHandle);
      //read 10 samples from 8-bit digital port for example correlated port0 
      //for 32-bit digital port use DAQmxReadDigitalU32 function
      ErrorCheck(DAQmxReadDigitalU8(DITaskHandle,10,10.0,DAQmx_Val_GroupByChannel,@data,10,@reads,Nil),DITaskHandle);
      writeln('number of readed samples: ',reads);
      for i:=0 to 9 do writeln('data [',i,'] = %',IntToBin(data[i],8));
      //stop and clear task
      DAQmxStopTask(DITaskHandle);
      DAQmxClearTask(DITaskHandle);
      DAQmxStopTask(CtrTaskHandle);
      DAQmxClearTask(CtrTaskHandle); 
    end.
    

Output example: +5V connected to pin P0.5 on port0 on NI-PCI6250 card. 
    
    
    number of readed samples: 10
    data [0] = %00100000
    data [1] = %00100000
    data [2] = %00100000
    data [3] = %00100000
    data [4] = %00100000
    data [5] = %00100000
    data [6] = %00100000
    data [7] = %00100000
    data [8] = %00100000
    data [9] = %00100000
    

#### Software controlled digital output

To check it connect digital output(s) to oscilloscope or to card analog input. 
    
    
    program doportlpi;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes,nidaqmx,SysUtils,StrUtils
      { you can add units after this };
    var
      DOTaskHandle:TaskHandle;
      data: byte;
      written:longint = 0;
    {************Error handling procedure *****************************************
          Example:
       ErrorCheck(DAQmxCreateAIVoltageChan(AITaskHandle,'Dev0/ai0','',DAQmx_Val_RSE,-10.0,10.0,DAQmx_Val_Volts,NIL),AITaskHandle);
    
      *******************************************************************************}
    
      procedure ErrorCheck(AError:longint;ATaskHandle:longint);
      var
        errBuffer:array[0..2048] of char;
      begin
         //get error description
         if Aerror<>0 then begin
           DAQmxGetExtendedErrorInfo(@ErrBuffer[0],2048);
           writeln(PChar(@ErrBuffer[0]));
         //stop and cleat task
         if Assigned(@ATaskHandle) then begin
           DAQmxStopTask(ATaskHandle);
           DAQmxClearTask(ATaskHandle);
         end;
         //stop program
         Halt;
        end;
      end;
    {*******************MAIN PROGRAM WITH ERROR HANDLING*************************}
    begin
      //task create
      ErrorCheck(DAQmxCreateTask('',@DOTaskHandle),DOTaskHandle);
      //digital input port create
      ErrorCheck(DAQmxCreateDOChan(DOTaskHandle,'Dev1/port0','',DAQmx_Val_ChanForAllLines),DOTaskHandle);
      //task start
      //no need to start task
      ErrorCheck(DAQmxStartTask(DOTaskHandle),DOTaskHandle);
      //for 32-bit digital port use DAQmxReadDigitalU32 function
      //set 1 on all lines of digital port
      //change data:=0 to set 0 or use DAQmxResetDevice function to
      data:=0;
      ErrorCheck(DAQmxWriteDigitalU8(DOTaskHandle,1,1,10.0,DAQmx_Val_GroupByChannel,@data,@written,Nil),DOTaskHandle);
      //use DAQmxResetDevice function here to set signal on Low state
      writeln('number of written samples: ',written);
      //stop and clear task
      if DOTaskHandle<>0 then begin
        DAQmxStopTask(DOTaskHandle);
        DAQmxClearTask(DOTaskHandle);
      end;
    end.
    

#### Hardware timed digital outputs with counter output used as source clock

To check it connect digital output(s) to oscilloscope or to card analog input. 
    
    
    program doporttimed;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes,nidaqmx,SysUtils,StrUtils,Crt
      { you can add units after this };
    var
      DOTaskHandle,CtrTaskHandle:TaskHandle;
      data:array[0..7] of byte = (1,2,4,8,16,32,64,128);
      written:longint = 0;
    {************Error handling procedure *****************************************
          Example:
       ErrorCheck(DAQmxCreateAIVoltageChan(AITaskHandle,'Dev0/ai0','',DAQmx_Val_RSE,-10.0,10.0,DAQmx_Val_Volts,NIL),AITaskHandle);
    
      *******************************************************************************}
    
      procedure ErrorCheck(AError:longint;ATaskHandle:longint);
      var
        errBuffer:array[0..2048] of char;
      begin
         //get error description
         if Aerror<>0 then begin
           DAQmxGetExtendedErrorInfo(@ErrBuffer[0],2048);
           writeln(PChar(@ErrBuffer[0]));
         //stop and cleat task
         if Assigned(@ATaskHandle) then begin
           DAQmxStopTask(ATaskHandle);
           DAQmxClearTask(ATaskHandle);
         end;
         //stop program
         Halt;
        end;
      end;
    {*******************MAIN PROGRAM WITH ERROR HANDLING*************************}
    begin
      {******CREATE COUNTER OUTPUT TO USE IT AS CLOCK FOR DIGITAL PORT OUTPUTS******
      *******INTERNALLY CONNECTED ON M-SERIES as /Dev1/Ctr0InternalOutput**********}
      //create couter task
      ErrorCheck(DAQmxCreateTask('Counter Task',@CtrTaskHandle),CtrTaskHandle);
      //create counter output channel
      //Frequency 1000 Hz, duty 0.5
      ErrorCheck(DAQmxCreateCOPulseChanFreq(CtrTaskHandle,'Dev1/ctr0','',DAQmx_Val_Hz,DAQmx_Val_Low,0,10000,0.5),CtrTaskHandle);
      //timing 1000 samples per second
      ErrorCheck(DAQmxCfgImplicitTiming(CtrTaskHandle,DAQmx_Val_ContSamps,1000),CtrTaskHandle);
      //start task
      //start counter task
      ErrorCheck(DAQmxStartTask(CtrTaskHandle),CtrTaskHandle);
    
      //task create
      ErrorCheck(DAQmxCreateTask('',@DOTaskHandle),DOTaskHandle);
      //digital input port create
      ErrorCheck(DAQmxCreateDOChan(DOTaskHandle,'Dev1/port0','',DAQmx_Val_ChanForAllLines),DOTaskHandle);
      //timing
      //frequency 1000Hz
      ErrorCheck(DAQmxCfgSampClkTiming(DOTaskHandle,'/Dev1/Ctr0InternalOutput',1000.0,DAQmx_Val_Rising,DAQmx_Val_ContSamps,1000),DOTaskHandle);
      //for 32-bit digital port use DAQmxWriteDigitalU32 function
      //function sets to start task automaticly 3-rd parameter
      //write 8 samples
      ErrorCheck(DAQmxWriteDigitalU8(DOTaskHandle,8,1,10.0,DAQmx_Val_GroupByChannel,data,@written,Nil),DOTaskHandle);
      writeln('number of written samples: ',written);
      //stop and clear task
      DAQmxStopTask(DOTaskHandle);
      DAQmxClearTask(DOTaskHandle);
      DAQmxStopTask(CtrTaskHandle);
      DAQmxClearTask(CtrTaskHandle);
    end.
    

### COUNTER INPUTS/OUTPUTS

#### Pulse generation

This example demonstrates pwm generaton with frequency 1000Hz and duty circle 0.5. 
    
    
    program doporttimed;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes,nidaqmx,SysUtils,StrUtils,Crt
      { you can add units after this };
    var
     CtrTaskHandle:TaskHandle;
     {************Error handling procedure *****************************************
          Example:
       ErrorCheck(DAQmxCreateAIVoltageChan(AITaskHandle,'Dev0/ai0','',DAQmx_Val_RSE,-10.0,10.0,DAQmx_Val_Volts,NIL),AITaskHandle);
    
      *******************************************************************************}
    
      procedure ErrorCheck(AError:longint;ATaskHandle:longint);
      var
        errBuffer:array[0..2048] of char;
      begin
         //get error description
         if Aerror<>0 then begin
           DAQmxGetExtendedErrorInfo(@ErrBuffer[0],2048);
           writeln(PChar(@ErrBuffer[0]));
         //stop and cleat task
         if Assigned(@ATaskHandle) then begin
           DAQmxStopTask(ATaskHandle);
           DAQmxClearTask(ATaskHandle);
         end;
         //stop program
         Halt;
        end;
      end;
    {*******************MAIN PROGRAM WITH ERROR HANDLING*************************}
    begin
      //create couter task
      ErrorCheck(DAQmxCreateTask('Counter Task',@CtrTaskHandle),CtrTaskHandle);
      //create counter output channel
      //Frequency 1000 Hz, duty 0.5
      ErrorCheck(DAQmxCreateCOPulseChanFreq(CtrTaskHandle,'Dev1/ctr0','',DAQmx_Val_Hz,DAQmx_Val_Low,0,10000,0.5),CtrTaskHandle);
      //timing 1000 samples per second
      ErrorCheck(DAQmxCfgImplicitTiming(CtrTaskHandle,DAQmx_Val_ContSamps,1000),CtrTaskHandle);
      //start task
      ErrorCheck(DAQmxStartTask(CtrTaskHandle),CtrTaskHandle);
       
      writeln('Press any key to stop generation');
      repeat
      until keypressed;  
      
      DAQmxStopTask(CtrTaskHandle);
      DAQmxClearTask(CtrTaskHandle);
    end.
    

#### Event counting

In this example first counter(Dev1/ctr0) is configurated as counter output for frequency generation, second (Dev1/ctr1) as counter input for pulse counting. First counter also used as clock source for second one. 
    
    
    program eventcounting;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes,nidaqmx,SysUtils,StrUtils,Crt
      { you can add units after this };
    var
      CtrOutTaskHandle,CtrInTaskHandle:TaskHandle;
      reads:cardinal=0;
      data:array [0..999] of longint;
      i:longint;
    {************Error handling procedure *****************************************
          Example:
       ErrorCheck(DAQmxCreateAIVoltageChan(AITaskHandle,'Dev0/ai0','',DAQmx_Val_RSE,-10.0,10.0,DAQmx_Val_Volts,NIL),AITaskHandle);
    
      *******************************************************************************}
      procedure ErrorCheck(AError:longint;ATaskHandle:longint);
      var
        errBuffer:array[0..2048] of char;
      begin
         //get error description
         if Aerror<>0 then begin
           DAQmxGetExtendedErrorInfo(@ErrBuffer[0],2048);
           writeln(PChar(@ErrBuffer[0]));
         //stop and cleat task
         if Assigned(@ATaskHandle) then begin
           DAQmxStopTask(ATaskHandle);
           DAQmxClearTask(ATaskHandle);
         end;
         //stop program
         Halt;
        end;
      end;
    {*******************MAIN PROGRAM WITH ERROR HANDLING*************************}
    begin
      {****************CREATE COUNTER OUTPUT***************************************}
      //create couter output task
      ErrorCheck(DAQmxCreateTask('Counter output task',@CtrOutTaskHandle),CtrOutTaskHandle);
      //create counter output channel
      //Frequency 1000 Hz, duty 0.5
      ErrorCheck(DAQmxCreateCOPulseChanFreq(CtrOutTaskHandle,'Dev1/ctr0','',DAQmx_Val_Hz,DAQmx_Val_Low,0,10000,0.5),CtrOutTaskHandle);
      //timing 1000 samples per second
      ErrorCheck(DAQmxCfgImplicitTiming(CtrOutTaskHandle,DAQmx_Val_ContSamps,1000),CtrOutTaskHandle);
      //start counter output task
      ErrorCheck(DAQmxStartTask(CtrOutTaskHandle),CtrOutTaskHandle);
    
      {*****************CREATE COUNTER INPUT***************************************}
      //create couter output task
      ErrorCheck(DAQmxCreateTask('Counter input task',@CtrInTaskHandle),CtrInTaskHandle);
      //create input counter for pulse counting
      ErrorCheck(DAQmxCreateCICountEdgesChan(CtrInTaskHandle,'Dev1/ctr1','',DAQmx_Val_Rising,0,DAQmx_Val_CountUp),CtrInTaskHandle);
      //timing
      ErrorCheck(DAQmxCfgSampClkTiming(CtrInTaskHandle,'/Dev1/Ctr0InternalOutput',1000.0,DAQmx_Val_Rising,DAQmx_Val_ContSamps,1000),CtrInTaskHandle);
       //start counter input task
      ErrorCheck(DAQmxStartTask(CtrInTaskHandle),CtrInTaskHandle);
      //counting pulses
      ErrorCheck(DAQmxReadCounterU32(CtrIntaskHandle,1000,10.0,@data,1000,@reads,Nil),CtrIntaskHandle);
      writeln('acquired samples: ',reads);
      for i:=0 to 10 do writeln('sample[',i,'] = ',data[i]);
    
      //stop and clear tasks
      DAQmxStopTask(CtrInTaskHandle);
      DAQmxClearTask(CtrInTaskHandle);
      DAQmxStopTask(CtrOutTaskHandle);
      DAQmxClearTask(CtrOutTaskHandle);
    end.
    

### TESTED HARDWARE

Examples were tested under Linux 32-bit (kernel 2.6.32) NI-DAQmx 8.0.2 and Windows 7 32-bit NI-DAQmx 14 on cards: 

  * **NI PCI-6250**
  * **NI PCI-6224**


  * **NI PXIe-6612 (Windows only)**


  * **NI USB-6356 (Windows only)**


  * **NI USB-6003 (Windows only)**



## NI-DAQxmxBase examples

Generally, NI-DAQmxBase is a light version of NI-DAQmx, but this library is **multiplatform**. You can use it under all supported operation systems without modification of NI-DAQmxBase function call. 

**Read readme file to check card supported features!!!**

### DEVICE INFO

Functions, which provide device and channel information, are not implemented in this library, yet. 

### ANALOG INPUTS/OUTPUTS

#### Acquire finite number of samples from one analog channel
    
    
    program aichannel;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes,nidaqmxbase;
      { you can add units after this }
    var
       AITaskHandle: PTaskHandle;
       NiBool:Pnibool;
       reads:longint=0;
       i:byte;
       data:array [1..1000] of double;
    procedure ErrorCheck(AError:longint;ATaskHandle:PTaskHandle);
    var
      errBuffer:array[0..2048] of char;
      errMessage:string;
    begin
       //get error description
       if DAQmxFailed(AError) = niTrue then begin
         DAQmxBaseGetExtendedErrorInfo(ErrBuffer,2048);
         writeln(ErrBuffer);
       //stop and cleat task
       if Assigned(ATaskHandle) then begin
           DAQmxBaseStopTask(ATaskHandle);
           DAQmxBaseClearTask(ATaskHandle);
       end;
       //stop program
       Halt;
      end;
    end;
    
    begin
      //create task
      ErrorCheck(DAQmxBaseCreateTask ('', AITaskHandle),AITaskHandle);
      //create analog input channel
      ErrorCheck(DAQmxBaseCreateAIVoltageChan(AITaskHandle,'Dev1/ai0','', DAQmx_Val_RSE,-10, 10,DAQmx_Val_Volts,''),AITaskHandle);
      //timing
      //clock frequency 1000 Hz, sample rate 1000 per second
      //if you use more than one active channel, generally sample rate = clock frequency/(active analog channles*10);
      ErrorCheck(DAQmxBaseCfgSampClkTiming (AITaskHandle,'OnboardClock',1000,DAQmx_Val_Rising,DAQmx_Val_FiniteSamps,1000),AITaskHandle);
      //start task
      ErrorCheck(DAQmxBaseStartTask(AITaskHandle),AITaskHandle);
      //read 1000 samples
      ErrorCheck(DAQmxBaseReadAnalogF64 (AITaskHandle, 1000, 10.0,DAQmx_Val_GroupByChannel,data,1000,reads,NIBool^),AITaskHandle);
      //acquired sample number
      writeln(reads,' samples were acqiured');
      //show value of first ten samples
      for i:=1 to 10 do writeln('sample[',i,'] = ',data[i]:4:2,' V');
      //stop and clear task
      DAQmxBaseStopTask(AITaskHandle);
      DAQmxBaseClearTask(AITaskHandle);
    end.
    

Output example +5V connected to pin 68 on NI PCI-6250 
    
    
    1000 samples were acqiured
    sample[1] = 5.07 V
    sample[2] = 5.06 V
    sample[3] = 5.06 V
    sample[4] = 5.06 V
    sample[5] = 5.06 V
    sample[6] = 5.06 V
    sample[7] = 5.06 V
    sample[8] = 5.06 V
    sample[9] = 5.06 V
    sample[10] = 5.07 V
    

#### Continuously acquire samples from three analog inputs
    
    
    program ai3contacq;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes,nidaqmxbase;
      { you can add units after this }
    var
       AITaskHandle: PTaskHandle;
       NiBool:Pnibool;
       reads:longint=0;
       totalreads:longint=0;
       i:byte;
       data:array of double;
       acqnumsamples:longint;
    procedure ErrorCheck(AError:longint;ATaskHandle:PTaskHandle);
    var
      errBuffer:array[0..2048] of char;
      errMessage:string;
    begin
       //get error description
       if DAQmxFailed(AError) = niTrue then begin
         DAQmxBaseGetExtendedErrorInfo(ErrBuffer,2048);
         writeln(ErrBuffer);
       //stop and cleat task
       if Assigned(ATaskHandle) then begin
           DAQmxBaseStopTask(ATaskHandle);
           DAQmxBaseClearTask(ATaskHandle);
       end;
       //stop program
       Halt;
      end;
    end;
    
    begin
      //enter number of total samples for each channel to be acquired
      write('Enter a number of samples for each channel to be acquired: ');
      readln(acqnumsamples);
      //Set buffer size = length of data array:=number of active channels * sample per channel
      Setlength(data,3*1000);
      //create task
      ErrorCheck(DAQmxBaseCreateTask ('', AITaskHandle),AITaskHandle);
      //create analog input channel - 3 active analog inputs
      ErrorCheck(DAQmxBaseCreateAIVoltageChan(AITaskHandle,'Dev1/ai0:2','', DAQmx_Val_RSE,-10, 10,DAQmx_Val_Volts,''),AITaskHandle);
      //timing
      //clock frequency 10000 Hz, sample rate 1000 per second
      //if you use more than one active channel, generally sample rate = clock frequency/(active analog channles*10);
      ErrorCheck(DAQmxBaseCfgSampClkTiming (AITaskHandle,'OnboardClock',10000,DAQmx_Val_Rising,DAQmx_Val_ContSamps,1000),AITaskHandle);
      //start task
      ErrorCheck(DAQmxBaseStartTask(AITaskHandle),AITaskHandle);
      //read samples
      while totalreads<acqnumsamples do begin
        ErrorCheck(DAQmxBaseReadAnalogF64(AITaskHandle, 1000, 10.0, DAQmx_Val_GroupByChannel,data,Length(data),reads,NIBool^),AITaskHandle);
        totalreads:=totalreads+reads;
        writeln('total samples readed for each channel: ',totalreads);
        //show first five values for each channel
        for i:=0 to 5 do writeln('chan1[',i,'] = ',data[i]:4:2,' V ','chan2[',i,'] = ',data[i+1000]:4:2,' V ','chan3[',i,'] = ',data[i+2000]:4:2,' V ');
      end;
      //stop and clear task
      DAQmxBaseStopTask(AITaskHandle);
      DAQmxBaseClearTask(AITaskHandle);
    end.
    

Output example +5V connected to pin 68 on NI PCI-6250 
    
    
    Enter a number of samples for each channel to be acquired: 2000
    total samples readed for each channel: 1000
    chan1[0] = 5.07 V chan2[0] = 0.00 V chan3[0] = 0.03 V
    chan1[1] = 5.06 V chan2[1] = -0.00 V chan3[1] = 0.03 V
    chan1[2] = 5.07 V chan2[2] = -0.00 V chan3[2] = 0.02 V
    chan1[3] = 5.07 V chan2[3] = -0.00 V chan3[3] = 0.02 V
    chan1[4] = 5.06 V chan2[4] = -0.00 V chan3[4] = 0.02 V
    chan1[5] = 5.06 V chan2[5] = -0.00 V chan3[5] = 0.01 V
    total samples readed for each channel: 2000
    chan1[0] = 5.06 V chan2[0] = -0.00 V chan3[0] = -0.02 V
    chan1[1] = 5.07 V chan2[1] = -0.00 V chan3[1] = -0.02 V
    chan1[2] = 5.06 V chan2[2] = -0.00 V chan3[2] = -0.02 V
    chan1[3] = 5.07 V chan2[3] = -0.00 V chan3[3] = -0.02 V
    chan1[4] = 5.06 V chan2[4] = -0.00 V chan3[4] = -0.02 V
    chan1[5] = 5.06 V chan2[5] = -0.00 V chan3[5] = -0.02 V
    

### DIGITAL INPUTS/OUTPUTS

Read readme file to check supported card features by NI-DAQmxBase. 

In case of M-series cards, correlated digital input/output is limited to about 100kHz, because DMA data transfer is not supported. 

#### Software timed digital input
    
    
    program diport;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes,nidaqmxbase,StrUtils
      { you can add units after this };
    var
      DITaskHandle:PTaskHandle;
      NiBool:Pnibool;
      data:byte;
      reads:longint=0;
    procedure ErrorCheck(AError:longint;ATaskHandle:PTaskHandle);
    var
      errBuffer:array[0..2048] of char;
    begin
       //get error description
       if DAQmxFailed(AError) = niTrue then begin
         DAQmxBaseGetExtendedErrorInfo(ErrBuffer,2048);
         writeln(ErrBuffer);
       //stop and cleat task
       if Assigned(ATaskHandle) then begin
           DAQmxBaseStopTask(ATaskHandle);
           DAQmxBaseClearTask(ATaskHandle);
       end;
       //stop program
       Halt;
      end;
    end;
    begin
      //create task
      ErrorCheck(DAQmxBaseCreateTask('', DITaskHandle),DITaskHandle);
      //create digital input port
      ErrorCheck(DAQmxBaseCreateDIChan(DITaskHandle,'Dev1/port0','',DAQmx_Val_ChanForAllLines),DITaskHandle);
      //start task
      ErrorCheck(DAQmxBaseStartTask(DITaskHandle),DITaskHandle);
      //read dat form port
      //use DAQmxBaseReadDigitalU8 for 8 bit port
      //use DAQmxBaseReadDigitalU32 for 32 bit port
      ErrorCheck(DAQmxBaseReadDigitalU8(DITaskHandle,1,10.0,DAQmx_Val_GroupByChannel,data,1,reads,NiBool^),DITaskHandle);
      writeln('digital port value %',IntToBin(data,8));
      //stop and clear task
      DAQmxBaseStopTask(DITaskHandle);
      DAQmxBaseClearTask(DITaskHandle);
    end.
    

Output example: +5V connected to line 1 of 8-bit digital port 
    
    
    digital port value %00000010
    

#### Hardware timed digital input
    
    
    program diporthwtimed;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes,nidaqmxbase,StrUtils
      { you can add units after this };
    var
      DITaskHandle,CtrTaskHandle:PTaskHandle;
      NiBool:Pnibool;
      data:array of byte;
      reads:longint=0;
      i:longint;
    procedure ErrorCheck(AError:longint;ATaskHandle:PTaskHandle);
    var
      errBuffer:array[0..2048] of char;
    begin
       //get error description
       if DAQmxFailed(AError) = niTrue then begin
         DAQmxBaseGetExtendedErrorInfo(ErrBuffer,2048);
         writeln(ErrBuffer);
       //stop and cleat task
       if Assigned(ATaskHandle) then begin
           DAQmxBaseStopTask(ATaskHandle);
           DAQmxBaseClearTask(ATaskHandle);
       end;
       //stop program
       Halt;
      end;
    end;
    begin
    
      {********CONFIGURE COUNTER TO USE AS CLOCK FOR DIGITAL INPUT*****************}
      //create counter task
      ErrorCheck(DAQmxBaseCreateTask('', CtrTaskHandle),CtrTaskHandle);
      //create counter output channel
      //Frequency 10000 Hz, duty 0.5
      ErrorCheck(DAQmxBaseCreateCOPulseChanFreq(CtrTaskHandle,'Dev1/ctr0','',DAQmx_Val_Hz,DAQmx_Val_Low,0,10000,0.5),CtrTaskHandle);
      //timing
      //last value is not important for continuesly sample mode, for finite sample mode = generate 10000 samples
      ErrorCheck(DAQmxBaseCfgImplicitTiming(CtrTaskHandle,DAQmx_Val_ContSamps,10000),CtrTaskHandle);
      //start counter task
      ErrorCheck(DAQmxBaseStartTask(CtrTaskHandle),CtrTaskHandle);
    
    
      {*******************HW TIMED DIGITAL INPUT***********************************
      **********LIMITED TO 10kHz,DMA IS NOT IMPLEMENTED****************************}
      //configure buffer for 1000 samples
      SetLength(data,1000);
      //create task
      ErrorCheck(DAQmxBaseCreateTask('', DITaskHandle),DITaskHandle);
      //create digital input port
      ErrorCheck(DAQmxBaseCreateDIChan(DITaskHandle,'Dev1/port0','',DAQmx_Val_ChanForAllLines),DITaskHandle);
      //timing frequency 10000Hz, 1000 sample per channel
      ErrorCheck(DAQmxBaseCfgSampClkTiming(DITaskHandle,'/Dev1/Ctr0InternalOutput',10000.0,DAQmx_Val_Rising,DAQmx_Val_ContSamps,10000),DITaskHandle);
      //start task
      ErrorCheck(DAQmxBaseStartTask(DITaskHandle),DITaskHandle);
      //read dat form port
      //use DAQmxBaseReadDigitalU8 for 8 bit port
      //use DAQmxBaseReadDigitalU32 for 32 bit port
      ErrorCheck(DAQmxBaseReadDigitalU8(DITaskHandle,10000,10.0,DAQmx_Val_GroupByChannel,data,10000,reads,NiBool^),DITaskHandle);
      writeln(reads,' samples were readed');
      //show first 10 samples
      for i:=0 to 9 do writeln('sample[',i,'] = %',IntToBin(data[i],8));
      //stop and clear task
      DAQmxBaseStopTask(DITaskHandle);
      DAQmxBaseClearTask(DITaskHandle);
      DAQmxBaseStopTask(CtrTaskHandle);
      DAQmxBaseClearTask(CtrTaskHandle);
    end.
    

Output example: +5V connected to line 1 of 8-bit digital port, executed with time command under Linux. 
    
    
    10000 samples were readed                                      
    sample[0] = %00000010                                          
    sample[1] = %00000010                                          
    sample[2] = %00000010
    sample[3] = %00000010
    sample[4] = %00000010
    sample[5] = %00000010
    sample[6] = %00000010
    sample[7] = %00000010
    sample[8] = %00000010
    sample[9] = %00000010
    
    real    0m4.483s
    user    0m4.261s
    sys     0m0.100s
    

#### Software timed digital output
    
    
    program doport;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes,nidaqmxbase,StrUtils,SysUtils
      { you can add units after this };
    var
      DOTaskHandle,CtrTaskHandle:PTaskHandle;
      NiBool:Pnibool;
      data: byte;
      written:longint=0;
      i:longint;
    procedure ErrorCheck(AError:longint;ATaskHandle:PTaskHandle);
    var
      errBuffer:array[0..2048] of char;
    begin
       //get error description
       if DAQmxFailed(AError) = niTrue then begin
         DAQmxBaseGetExtendedErrorInfo(ErrBuffer,2048);
         writeln(ErrBuffer);
       //stop and cleat task
       if Assigned(ATaskHandle) then begin
           DAQmxBaseStopTask(ATaskHandle);
           DAQmxBaseClearTask(ATaskHandle);
       end;
       //stop program
       Halt;
      end;
    end;
    begin
      //write data to buffer, produce digital waveform on all port lines
      data:=255;
      //create task
      ErrorCheck(DAQmxBaseCreateTask('', DOTaskHandle),DOTaskHandle);
      //create digital input port
      ErrorCheck(DAQmxBaseCreateDOChan(DOTaskHandle,'Dev1/port0','',DAQmx_Val_ChanForAllLines),DOTaskHandle);
      //use DAQmxBaseWriteDigitalU8 for 8 bit port
      //use DAQmxBaseWriteDigitalU32 for 32 bit port
      //set third parametr to 1 to enable autostart or set to 0 and use DAQmxBaseStartTask before
      //set digital "1"
      writeln('set digital 1 on all port lines');
      writeln('sleep 10s');
      ErrorCheck(DAQmxBaseWriteDigitalU8(DOTaskHandle,1000,1,1000,DAQmx_Val_GroupByChannel,data,written,NiTrue),DOTaskHandle);
      sleep(10000);
      writeln(written,' samples were written');
      //set digital "0"
      writeln('set digital 0 on all port lines');
      writeln('sleep 10s');
      sleep(10000);
      ErrorCheck(DAQmxBaseWriteDigitalU8(DOTaskHandle,1000,1,1000,DAQmx_Val_GroupByChannel,data,written,NiTrue),DOTaskHandle);
      writeln(written,' samples were written');
      //stop and clear task
      DAQmxBaseStopTask(DOTaskHandle);
      DAQmxBaseClearTask(DOTaskHandle);
      DAQmxBaseStopTask(CtrTaskHandle);
      DAQmxBaseClearTask(CtrTaskHandle);
    end.
    

#### Hardware timed digital output

Digital output generation frequency is limited to ~100kHz, because DMA is not supported by the driver . 
    
    
    program doporthwtimed;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes,nidaqmxbase,StrUtils
      { you can add units after this };
    var
      DOTaskHandle,CtrTaskHandle:PTaskHandle;
      NiBool:Pnibool;
      data:array of byte;
      written:longint=0;
      i:longint;
    procedure ErrorCheck(AError:longint;ATaskHandle:PTaskHandle);
    var
      errBuffer:array[0..2048] of char;
    begin
       //get error description
       if DAQmxFailed(AError) = niTrue then begin
         DAQmxBaseGetExtendedErrorInfo(ErrBuffer,2048);
         writeln(ErrBuffer);
       //stop and cleat task
       if Assigned(ATaskHandle) then begin
           DAQmxBaseStopTask(ATaskHandle);
           DAQmxBaseClearTask(ATaskHandle);
       end;
       //stop program
       Halt;
      end;
    end;
    begin
      {********CONFIGURE COUNTER TO USE AS CLOCK FOR DIGITAL INPUT*****************}
      //create counter task
      ErrorCheck(DAQmxBaseCreateTask('', CtrTaskHandle),CtrTaskHandle);
      //create counter output channel
      //Frequency 10000 Hz, duty 0.5
      ErrorCheck(DAQmxBaseCreateCOPulseChanFreq(CtrTaskHandle,'Dev1/ctr0','',DAQmx_Val_Hz,DAQmx_Val_Low,0,10000,0.5),CtrTaskHandle);
      //timing
      //last value is not important for continuesly sample mode, for finite sample mode = generate 10000 samples
      ErrorCheck(DAQmxBaseCfgImplicitTiming(CtrTaskHandle,DAQmx_Val_ContSamps,10000),CtrTaskHandle);
      //start counter task
      ErrorCheck(DAQmxBaseStartTask(CtrTaskHandle),CtrTaskHandle);
      {*******************HW TIMED DIGITAL INPUT***********************************
      **********LIMITED TO 10kHz,DMA IS NOT IMPLEMENTED****************************}
      //configure buffer for 1000 samples
      SetLength(data,1000);
      //write data to buffer, produce digital waveform on all port lines
      for i:=0 to 999 do if (i mod 2 = 0) then data[i]:=255;
      //create task
      ErrorCheck(DAQmxBaseCreateTask('', DOTaskHandle),DOTaskHandle);
      //create digital input port
      ErrorCheck(DAQmxBaseCreateDOChan(DOTaskHandle,'Dev1/port0','',DAQmx_Val_ChanForAllLines),DOTaskHandle);
      //timing frequency 10000Hz, 1000 sample per channel
      ErrorCheck(DAQmxBaseCfgSampClkTiming(DOTaskHandle,'/Dev1/Ctr0InternalOutput',10000.0,DAQmx_Val_Rising,DAQmx_Val_ContSamps,1000),DOTaskHandle);
      //use DAQmxBaseWriteDigitalU8 for 8 bit port
      //use DAQmxBaseWriteDigitalU32 for 32 bit port
      //set third parametr to 1 to enable autostart or set to 0 and use DAQmxBaseStartTask before
      ErrorCheck(DAQmxBaseWriteDigitalU8(DOTaskHandle,1000,1,1000,DAQmx_Val_GroupByChannel,data,written,NiTrue),DOTaskHandle);
      writeln(written,' samples were written');
      //stop and clear task
      DAQmxBaseStopTask(DOTaskHandle);
      DAQmxBaseClearTask(DOTaskHandle);
      DAQmxBaseStopTask(CtrTaskHandle);
      DAQmxBaseClearTask(CtrTaskHandle);
    end.
    

### COUNTER INPUTS/OUTPUTS

Counter input of M-series cards is limited to computer speed. 

#### COUNTER OUTPUT

Counter output can be timed by software or hardware. 

Example demontrate how to use couter ouput as frequency generator(clock signal for other hardware) or as basic pwm regulation. 

In this example counter output is hardware timed. 
    
    
    program ctrout;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes,nidaqmxbase,StrUtils,SysUtils
      { you can add units after this };
    var
      CtrTaskHandle:PTaskHandle;
      NiBool:Pnibool;
      data:array of byte;
      reads:longint=0;
    procedure ErrorCheck(AError:longint;ATaskHandle:PTaskHandle);
    var
      errBuffer:array[0..2048] of char;
    begin
       //get error description
       if DAQmxFailed(AError) = niTrue then begin
         DAQmxBaseGetExtendedErrorInfo(ErrBuffer,2048);
         writeln(ErrBuffer);
       //stop and cleat task
       if Assigned(ATaskHandle) then begin
           DAQmxBaseStopTask(ATaskHandle);
           DAQmxBaseClearTask(ATaskHandle);
       end;
       //stop program
       Halt;
      end;
    end;
    begin
    
      {********CONFIGURE COUNTER TO USE AS CLOCK FOR DIGITAL INPUT*****************}
      //create counter task
      ErrorCheck(DAQmxBaseCreateTask('', CtrTaskHandle),CtrTaskHandle);
      //create counter output channel
      //Frequency 10000 Hz, duty 0.5
      ErrorCheck(DAQmxBaseCreateCOPulseChanFreq(CtrTaskHandle,'Dev1/ctr0','',DAQmx_Val_Hz,DAQmx_Val_Low,0,10000,0.5),CtrTaskHandle);
      //timing
      //last value is not important for continuesly sample mode, for finite sample mode = generate 10000 samples
      ErrorCheck(DAQmxBaseCfgImplicitTiming(CtrTaskHandle,DAQmx_Val_ContSamps,10000),CtrTaskHandle);
      //start counter task
      ErrorCheck(DAQmxBaseStartTask(CtrTaskHandle),CtrTaskHandle);
      //generate frequency signal for 10 seconds
      sleep(10000);
      DAQmxBaseStopTask(CtrTaskHandle);
      DAQmxBaseClearTask(CtrTaskHandle);
    end.
    

#### COUNTER INPUT

Counter input is only software operated. It is limited by hardware driver. 
    
    
    program ctrin;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes,nidaqmxbase,StrUtils,SysUtils,Crt
      { you can add units after this };
    var
      CtrInTaskHandle:PTaskHandle;
      NiBool:Pnibool;
      data:dword=0;
      reads:longint=0;
    procedure ErrorCheck(AError:longint;ATaskHandle:PTaskHandle);
    var
      errBuffer:array[0..2048] of char;
    begin
       //get error description
       if DAQmxFailed(AError) = niTrue then begin
         DAQmxBaseGetExtendedErrorInfo(ErrBuffer,2048);
         writeln(ErrBuffer);
       //stop and cleat task
       if Assigned(ATaskHandle) then begin
           DAQmxBaseStopTask(ATaskHandle);
           DAQmxBaseClearTask(ATaskHandle);
       end;
       //stop program
       Halt;
      end;
    end;
    begin
      {*****************CREATE COUNTER INPUT***************************************}
      //create couter input task
      ErrorCheck(DAQmxBaseCreateTask('Counter input task',CtrInTaskHandle),CtrInTaskHandle);
      //create input counter for pulse counting
      ErrorCheck(DAQmxBaseCreateCICountEdgesChan(CtrInTaskHandle,'Dev1/ctr1','',DAQmx_Val_Rising,0,DAQmx_Val_CountUp),CtrInTaskHandle);
      //timing
      //ErrorCheck(DAQmxBaseCfgSampClkTiming(CtrInTaskHandle,'/Dev1/Ctr0InternalOutput',1000.0,DAQmx_Val_Rising,DAQmx_Val_ContSamps,1000),CtrInTaskHandle);
       //start counter input task
      ErrorCheck(DAQmxBaseStartTask(CtrInTaskHandle),CtrInTaskHandle);
      //counting pulses
      writeln('press any key to stop counting');
      repeat
        ErrorCheck(DAQmxBaseReadCounterU32(CtrIntaskHandle,1,10.0,data,1,reads,NiTrue),CtrIntaskHandle);
        writeln('counter value: ',data);
      until keypressed;
      //clear and stop task
      DAQmxBaseStopTask(CtrInTaskHandle);
      DAQmxBaseClearTask(CtrInTaskHandle);
    end.
    

### TESTED HARDWARE

Examples were tested under Scientific Linux 6.4, Mageia 2 32-bit and Windows 32-bit and should work under MacOS (**need test**) on cards: 

  * **NI PCI-6250**
  * **NI PCI-6224**
  * **NI USB-6356** (Windows only)

---

_Source: [https://wiki.freepascal.org/NI-DAQmx_and_NI-DAQmx_Base_examples](https://web.archive.org/web/20240526081755/https://wiki.freepascal.org/NI-DAQmx_and_NI-DAQmx_Base_examples)_
