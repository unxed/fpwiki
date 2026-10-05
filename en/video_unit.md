# video unit

The `video` unit in conjunction with the [`keyboard`](</index.php?title=keyboard_unit&action=edit&redlink=1> "keyboard unit \(page does not exist\)") unit provide a system-independent way to program interactions with the console, a text terminal. It is part of the [FPC](<FPC.md> "FPC")’s standard [run-time library](<RTL.md> "RTL"). 

## functionality

The provided routines are rather _low-level_. There is no facility to just print a string at a certain position. Instead, the `video` unit maintains a buffer of the entire screen. It records every cell’s contents and possibly attributes like colors. The buffer then needs to be written with the `updateScreen` procedure. 

## example

Using the following enhancement unit: 
    
    
    unit videoEnhanced;
    {$modeSwitch typeHelpers+}
    interface
    uses
    	video;
    type
    	{$push}
    	{$scopedEnums on}
    	color = (black := video.black, blue := video.blue, green := video.green,
    		cyan := video.cyan, red := video.red, magenta := video.magenta,
    		brown := video.brown, lightGray := video.lightGray,
    		darkGray := video.darkGray, lightBlue := video.lightBlue,
    		lightGreen := video.lightGreen, lightCyan := video.lightCyan,
    		lightRed := video.lightRed, lightMagenta := video.lightMagenta,
    		yellow := video.yellow, white := video.white);
    	{$pop}
    	attribute = bitpacked record
    			foreground: color;
    			background: color.black..color.lightGray;
    			blinking: Boolean;
    		end;
    	cell = packed record
    			{$ifDef endian_big}
    				display: attribute;
    				character: char;
    			{$else}
    				character: char;
    				display: attribute;
    			{$endIf}
    		end;
    	{$if sizeOf(cell) <> sizeOf(tVideoCell)}
    		{$fatal wrong cell size}
    	{$endIf}
    	dimension = 0..16382{$ifNDef CPU16}+16377{$endIf};
    	
    	bufferHelper = type helper for pVideoBuf
    			function getCell(const x, y: dimension): cell;
    			procedure setCell(const x, y: dimension; const content: cell);
    			property cell[x, y: dimension]: cell
    				read getCell write setCell;
    		end;
    
    implementation
    
    function bufferHelper.getCell(const x, y: dimension): cell;
    type
    	cast = cell; // otherwise `cell` means the _property_
    begin
    	getCell := cast(self^[x + y * screenWidth]);
    end;
    
    procedure bufferHelper.setCell(const x, y: dimension; const content: cell);
    begin
    	self^[x + y * screenWidth] := tVideoCell(content);
    end;
    end.
    

The following program displays a yellow D in the fifth column of the fifth row: 
    
    
    program videoDemo(input, output, stdErr);
    uses
    	video, videoEnhanced;
    var
    	c: cell;
    begin
    	initVideo;
    	if errorCode <> errOK then
    	begin
    		halt(errorCode);
    	end;
    	
    	c := default(cell);
    	c.character := 'D';
    	c.display.foreground := color.yellow;
    	videoBuf.cell[4, 4] := c;
    	
    	updateScreen(false);
    	
    	readLn;
    	doneVideo;
    	ClearScreen; // Clean up console upon exit
    end.
    

This demo fails to make range checks. This demo assumes colors are available. 

## see also

  * [`video` unit documentation](<https://www.freepascal.org/docs-html/rtl/video/index.html>)
  * [`nCurses`](<ncurses.md> "ncurses")
  * [Free Vision](<Free_Vision.md> "Free Vision")
  * [Forum topic “How to change a character's text and background color in video unit?”](<https://forum.lazarus.freePascal.org/index.php/topic,53214.0.html#>)
  * [Forum topic “Video unit and writeln color?”](<https://forum.lazarus.freePascal.org/index.php/topic,24187.0.html#>)
  * [Forum topic “Video unit and updating the screen”](<https://forum.lazarus.freePascal.org/index.php/topic,53072.msg392128.html#msg392128>)
  * [`txtVideo` unit](<http://web.tiscali.it/licciardi/prog/txtvideo.pas>), a more extensive unit providing convenient routines

---

_Source: [https://wiki.freepascal.org/video_unit](https://web.archive.org/web/20250522064452/https://wiki.freepascal.org/video_unit)_
