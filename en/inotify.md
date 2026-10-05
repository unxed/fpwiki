# inotify

## demo

### main program
    
    
    (**
    	\brief     Sample program showing inotify events.
    	\version   @0x5A4EEB4C
    	\author    Kai Burghardt <wiz at KaiBurghardt.de>
    	\date      2018
    	\copyright All rights reserved.
    	
    	This program prints a readable representation of caught inotify events.
    	It is non-interactive and does only watch a directory/file specified
    	at compile-time.
    *)
    program inotifytst(input, output, stderr);
    
    {$mode objfpc}
    
    {$modeswitch classicprocvars on}
    
    {$typedAddress on}
    
    uses
    	baseunix, linux, gettext, sysutils;
    
    const
    	(**
    		\brief directory/file to be monitored by inotify
    		
    		\note Not yet at run-time existing files are not treat specially.
    		This program does not test whether watchedPath is a valid path.
    		However, inotify does not add watches to non-existing paths.
    		So inotify_add_watch may raise an error.
    	*)
    	watchedPath = '/tmp/abc';
    	/// \brief holds size of largest expected inotify_event size
    	maxExpEventSize = sizeof(inotify_event) + name_max;
    
    // resource strings {{{
    resourcestring
    	warning = '⚠: ';
    	error = '↯: ';
    	closeInotifyFDfailed = 'closing inotify file descriptor failed';
    	disassocInwdFrInfdFailed = 'disassociating inwd from infd failed';
    	initializingInotifyFDfailed =
    		'initializing inotify file descriptor failed';
    	addingWatchedPathToInotifyWatchListFailed =
    		'adding ''%0:s'' to inotify watch list failed';
    	couldNotAllocateMemForInotifyEvents =
    		'could not allocate memory for inotify events';
    	preparingOfSignalHandlerInstallFailed =
    		'preparing of signal-handler install failed';
    	couldNotInstallSignalHandlerForSigint =
    		'could not install signal handler for sigint';
    	readingFromInotifyFDfailed =
    		'reading from inotify file descriptor failed';
    // end of resourcestring }}}
    
    var
    	/// \brief inotify file descriptor
    	infd: cint;
    	/// \brief inotify watch descriptor
    	inwd: cint;
    	/// \brief buffer for reading inotify events
    	inEventQueueBuffer: PChar;
    	bytesReadCount, bytesProcessedCount: TsSize;
    	/// \brief a pointer to handle a single inotify_event
    	inEvent: PInotify_event;
    	/// \brief pointer to a signal action record
    	newAction: PSigActionRec;
    
    (**
    	\brief Closes inotify file descriptor.
    
    	closeInFd closes the inotify file descriptor by invoking fpClose.
    	It prints a warning on stderr if it went wrong but does not abort.
    *)
    procedure closeInFD;
    begin
    	// when all fds referring to an inotify-instance have been closed,
    	// the underlying object and its resources are automatically freed
    	if fpClose(infd) <> 0 then
    	begin
    		writeln(stderr, warning + closeInotifyFDfailed);
    	end;
    end;
    
    (**
    	\brief Removes inotify watch
    
    	cleanUpInWdNFd removes the only installed inotify watch
    	from (the only) inotify watch descriptor.
    *)
    procedure cleanUpInWD;
    begin
    	if inotify_rm_watch(infd, inwd) <> 0 then
    	begin
    		writeln(stderr, warning + disassocInwdFrInfdFailed);
    	end;
    end;
    
    (**
    	\brief frees reserved memory
    	
    	cleanUpEverything frees allocated memory which is reserved for
    	operations in the main loop.
    	
    	\sa sigHandler
    *)
    procedure cleanUpMemory;
    begin
    	freeMem(inEventQueueBuffer, maxExpEventSize);
    end;
    
    (**
    	\brief procedure-wrapper for intended termination by signals
    	
    	sigHandler calls cleanUpEverything.
    	It writes a final newline and halts program execution.
    *)
    procedure sigHandler(
    		signal: longInt;
    		info: PSigInfo;
    		context: PSigContext
    	); noreturn;
    begin
    	writeln;
    	halt(0);
    end;
    
    
    (*                                                                      *)
    (* M A I N                                                              *)
    (*                                                                      *)
    
    begin
    	//translateResourcestrings('/usr/share/locale/%s/LC_MESSAGES/' +
    	//	applicationName() + '.mo');
    	translateResourcestrings(applicationName() + '.%s.mo');
    	
    	// function inotify_init() initializes a new inotify instance and
    	// returns a file descriptor associated with its inotify event queue
    	infd := inotify_init();
    	if infd < 0 then
    	begin
    		writeln(stderr, error + initializingInotifyFDfailed);
    		// optional: further error analysis with fpgeterrno()
    		halt(1);
    	end;
    	addExitProc(closeInFD);
    	
    	// next step: adding a watch to a previously initialized inotify instance
    	inwd := inotify_add_watch(infd, watchedPath, in_all_events);
    	if inwd < 0 then
    	begin
    		writeln(stderr, error +
    			format(addingWatchedPathToInotifyWatchListFailed, [watchedPath]));
    		// optional: retrieve errno with fpgeterrno()
    		halt(2);
    	end;
    	addExitProc(cleanUpInWD);
    	
    	// avoid a run-time error on getMem-failure
    	returnNilIfGrowHeapFails := true;
    	// let's occupy some more more heap!
    	inEventQueueBuffer := getMem(maxExpEventSize);
    	// check on nil-pointer (failure)
    	if not assigned(inEventQueueBuffer) then
    	begin
    		writeln(stderr, error + couldNotAllocateMemForInotifyEvents);
    		halt(3);
    	end;
    	addExitProc(cleanUpMemory);
    	
    	// install a signal handler for several signals
    	new(newAction);
    	if not assigned(newAction) then
    	begin
    		writeln(stderr, error + preparingOfSignalHandlerInstallFailed);
    		halt(4);
    	end;
    	
    	// preparing signal action record
    	// save address of sigHandler procedure
    	newAction^.sa_handler := sigActionHandler(@sigHandler);
    	fillchar(newAction^.sa_mask, sizeof(newAction^.sa_mask), #0);
    	newAction^.sa_flags := 0;
    	
    	// pass sigActionRec to sigaction(2) syscall
    	if fpSigAction(SigHup or SigInt or SigQuit or SigAbrt or SigTerm or
    		SigTStp, newAction, nil) <> 0 then
    	begin
    		writeln(stderr, error + couldNotInstallSignalHandlerForSigint);
    		halt(5);
    	end;
    	
    	// get rid of unused newAction
    	dispose(newAction);
    	
    	// TODO: find something better than
    	// a while-true-loop and signal handling
    	while true do
    	begin
    		// read() returns the amount of bytes have been read.
    		// read() reads at most maxExpEventSize bytes to the memory position
    		// specified by inEventQueueBuffer.
    		// It's only possible to read whole inotify events,
    		// but no fractions even though
    		// the _beginning_ of the next event may fit into remaining space.
    		bytesReadCount := fpRead(infd, inEventQueueBuffer, maxExpEventSize);
    		if bytesReadCount < 0 then
    		begin
    			writeln(stderr, error + readingFromInotifyFDfailed);
    			halt(6);
    		end;
    		
    		// as there may be multiple inotify_events saved at inEventQueueBuffer
    		// we have to keep a counter how much has been processed
    		bytesProcessedCount := 0;
    		
    		// reset inEvent
    		// typecast PChar to use it as a PInotify_event as such allows the
    		// usage of dot-fieldname-designators
    		inEvent := PInotify_event(inEventQueueBuffer);
    		
    		while bytesProcessedCount < bytesReadCount do
    		begin
    			// print a readable representation of current inotify_event
    			writeln('inotify_event:');
    			with inEvent^ do
    			begin
    				writeln('           wd: ', wd);
    				
    				writeln('         mask: %', binStr(mask, 32));
    				// several tests on the mask {{{
    				if (mask and IN_ACCESS) > 0 then
    				begin
    					writeln('               IN_ACCESS');
    				end;
    				if (mask and IN_MODIFY) > 0 then
    				begin
    					writeln('               IN_MODIFY');
    				end;
    				if (mask and IN_ATTRIB) > 0 then
    				begin
    					writeln('               IN_ATTRIB');
    				end;
    				if (mask and IN_CLOSE_WRITE) > 0 then
    				begin
    					writeln('               IN_CLOSE_WRITE');
    				end;
    				if (mask and IN_CLOSE_NOWRITE) > 0 then
    				begin
    					writeln('               IN_CLOSE_NOWRITE');
    				end;
    				if (mask and IN_OPEN) > 0 then
    				begin
    					writeln('               IN_OPEN');
    				end;
    				if (mask and IN_MOVED_FROM) > 0 then
    				begin
    					writeln('               IN_MOVED_FROM');
    				end;
    				if (mask and IN_MOVED_TO) > 0 then
    				begin
    					writeln('               IN_MOVED_TO');
    				end;
    				if (mask and IN_CLOSE) > 0 then
    				begin
    					writeln('               IN_CLOSE = ',
    						'IN_CLOSE_WRITE or IN_CLOSE_NOWRITE');
    				end;
    				if (mask and IN_MOVE) > 0 then
    				begin
    					writeln('               IN_MOVE = ',
    						'IN_MOVED_FROM or IN_MOVED_TO');
    				end;
    				if (mask and IN_CREATE) > 0 then
    				begin
    					writeln('               IN_CREATE');
    				end;
    				if (mask and IN_DELETE) > 0 then
    				begin
    					writeln('               IN_DELETE');
    				end;
    				if (mask and IN_DELETE_SELF) > 0 then
    				begin
    					writeln('               IN_DELETE_SELF');
    				end;
    				if (mask and IN_MOVE_SELF) > 0 then
    				begin
    					writeln('               IN_MOVE_SELF');
    				end;
    				if (mask and IN_UNMOUNT) > 0 then
    				begin
    					writeln('               IN_UNMOUNT');
    				end;
    				if (mask and IN_Q_OVERFLOW) > 0 then
    				begin
    					writeln('               IN_Q_OVERFLOW');
    				end;
    				if (mask and IN_IGNORED) > 0 then
    				begin
    					writeln('               IN_IGNORED');
    				end;
    				if (mask and IN_ISDIR) > 0 then
    				begin
    					writeln('               IN_ISDIR');
    				end;
    				// }}}
    				
    				writeln('       cookie: $', hexStr(cookie, 8));
    				
    				// note: len does not contain the actual length of the name but
    				// also counts any following null-characters needed for well
    				// address-alignment
    				writeln('          len: ', len);
    				
    				// The record's name field has been declared
    				// to be a /single/ character.
    				// Indeed we know it is a (dynamic) array of char
    				// what corresponds to C's null-terminated strings.
    				// Meanwhile writeln() is capable of
    				// printing null-terminated strings, too.
    				// All it needs is the information a given argument is such one.
    				// Referring to inEvent^.name (single char) does not
    				// but following typecasted pointer:
    				writeln('         name: ', PChar(@inEvent^.name));
    			// end of with-inEvent^-do statement
    			end;
    			
    			// inotify(7) says the size of an inotify_event is
    			// sizeof(inotify_event) + len. but wait! it's a trap!
    			// it's true for C, but in Pascal you got an off-by-one-error:
    			// the inotify_event-record already
    			// includes the first character of the name-field.
    			inc(bytesProcessedCount, sizeof(inotify_event)-1 + inEvent^.len);
    			inEvent := PInotify_event(@inEventQueueBuffer[sizeof(inotify_event)-1 +
    				inEvent^.len]);
    		// end of while-loop over every read inotify event
    		end;
    	// end of main-loop while true
    	end;
    end.
    // vim: set tw=78 ts=4 noet bs=eol,start,indent fdm=marker fdc=1:
    

### translations

`inotifytst.de.po`:
    
    
    #: inotifytst:warning
    msgid "warning: "
    msgstr "Warnung: "
    
    #: inotifytst:error
    msgid "error: "
    msgstr "Fehler: "
    
    #: inotifytst:closeinotifyfdfailed
    msgid "closing inotify file descriptor failed"
    msgstr "inotify Dateideskriptor schließen fehlgeschlagen"
    
    #: inotifytst:disassocinwdfrinfdfailed
    msgid "disassociating inwd from infd failed"
    msgstr "Trennung von inwd und infd fehlgeschlagen"
    
    #: inotifytst:initializinginotifyfdfailed
    msgid "initializing inotify file descriptor failed"
    msgstr "inotify Dateideskritpor initialisiieren fehlgeschlagen"
    
    #: inotifytst:addingwatchedpathtoinotifywatchlistfailed
    msgid "adding '%s' to inotify watch list failed"
    msgstr "hinzufügen von '%s' zur inotify Beobachtungsliste fehlgeschlagen"
    
    #: inotifytst:couldnotallocatememforinotifyevents
    msgid "could not allocate memory for inotify events"
    msgstr "konnte kein Speicher für inotify Ereignisse reservieren"
    
    #: inotifytst:preparingofsignalhandlerinstallfailed
    msgid "preparing of signal-handler install failed"
    msgstr "vorbereiten des Signalbehandlers Installation fehlgeschlagen"
    
    #: inotifytst:couldnotinstallsignalhandlerforsigint
    msgid "could not install signal handler for sigint"
    msgstr "konnte den Signalbehandler für das Unterbrechungssignal nicht installieren"
    
    #: inotifytst:readingfrominotifyfdfailed
    msgid "reading from inotify file descriptor failed"
    msgstr "vom inotify Dateideskriptor lesen fehlgeschlagen"
    

`inotifytst.en.po`:
    
    
    #: inotifytst:warning
    msgid "warning: "
    msgstr "warning: "
    
    #: inotifytst:error
    msgid "error: "
    msgstr "error: "
    
    #: inotifytst:closeinotifyfdfailed
    msgid "closing inotify file descriptor failed"
    msgstr "closing inotify file descriptor failed"
    
    #: inotifytst:disassocinwdfrinfdfailed
    msgid "disassociating inwd from infd failed"
    msgstr "disassociating inwd from infd failed"
    
    #: inotifytst:initializinginotifyfdfailed
    msgid "initializing inotify file descriptor failed"
    msgstr "initializing inotify file descriptor failed"
    
    #: inotifytst:addingwatchedpathtoinotifywatchlistfailed
    msgid "adding '%s' to inotify watch list failed"
    msgstr "adding '%s' to inotify watch list failed"
    
    #: inotifytst:couldnotallocatememforinotifyevents
    msgid "could not allocate memory for inotify events"
    msgstr "could not allocate memory for inotify events"
    
    #: inotifytst:preparingofsignalhandlerinstallfailed
    msgid "preparing of signal-handler install failed"
    msgstr "preparing of signal-handler install failed"
    
    #: inotifytst:couldnotinstallsignalhandlerforsigint
    msgid "could not install signal handler for sigint"
    msgstr "could not install signal handler for sigint"
    
    #: inotifytst:readingfrominotifyfdfailed
    msgid "reading from inotify file descriptor failed"
    msgstr "reading from inotify file descriptor failed"

---

_Source: [https://wiki.freepascal.org/inotify](https://web.archive.org/web/20210127113440/https://wiki.freepascal.org/inotify)_
