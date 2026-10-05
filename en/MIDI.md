# MIDI

## Contents

  * 1 MIDI file format
  * 2 Playing MIDI files
    * 2.1 Cross Platform
    * 2.2 Linux
    * 2.3 macOS
    * 2.4 Windows
  * 3 External links
  * 4 See also



## MIDI file format

Musical Instrument Digital Interface (MIDI) is a technical standard that describes a communications protocol, digital interface, and electrical connectors that connect a wide variety of electronic musical instruments, computers, and related audio devices for playing, editing and recording music. 

Standard MIDI Files (SMF) contain all the MIDI instructions to generate notes, control individuals volumes, select instrument sounds, and even control reverb and other effects. The files are typically created by a "MIDI sequencer" (software or hardware) and then played on some kind of MIDI synthesizer. 

Unlike digital audio files (.wav, .aiff, etc.) or even compact discs, a MIDI file does not need to capture and store actual sounds. Instead, the MIDI file can be just a list of events which describe the specific steps that a soundcard or other playback device must take to generate certain sounds. This way, MIDI files are very much smaller than digital audio files, and the events are also editable, allowing the music to be rearranged, edited, even composed interactively, if desired. 

The format also allows tagging the file and the data in the file with copyright notices and other text "meta-events". 

All popular computing platforms can play MIDI files (*.mid) and there are thousands of web sites offering files for sale or even for free. Anyone can make and share a MIDI file, using software that is readily available on smart phones, tablets and computers. 

The Standard MIDI File Specification is included in the [Complete MIDI 1.0 Detailed Specification document (1996)](<https://www.midi.org/specifications/item/the-midi-1-0-specification>). A number of [changes/additions](<https://www.midi.org/specifications/item/the-midi-1-0-specification#addenda>) became part of the MIDI 1.0 Specification after the "96.1" publication and should be consulted to have a current understanding of MIDI technology. 

## Playing MIDI files

Standard MIDI Files (SMF) contain sound events that indicate the notes and instruments in a musical performance, but do not include the digital waveform of the audio. They usually have the extension .mid or .midi. To play a MIDI file, software has to synthesize the music, which usually requires reading digital samples of musical instruments from a large file. 

### Cross Platform

  * [Two_Track_MIDI_Generator_Version_0034_Lazarus_project.zip](<https://forum.lazarus.freepascal.org/index.php?action=dlattach;topic=39981.0;attach=34650>) from the Forums. See [discussion](<https://forum.lazarus.freepascal.org/index.php/topic,39981.0.html>).
  * The [BASS audio library](<https://www.un4seen.com>) (with BASSMIDI extension enabling the playback of MIDI files and custom event sequences, using SF2 and SFZ soundfonts to provide the sounds, including support for packed soundfonts. MIDI input is also supported) for Linux, macOS and Windows. It is free for non-commercial use.



### Linux

  * [Pascal port of FluidSynth headers](<https://gitlab.com/bunnylin/pasfluidsynth/>)



### macOS

  * [macOS MIDI Player](<macOS_MIDI_Player.md> "macOS MIDI Player") Example of native code for a minimal application to play MIDI and iMelody files.



### Windows

  * [Components and functions for MIDI communication for Delphi](<https://bitbucket.org/h4ndy/midiio-dev/src/default/>) (source)
  * [MIDI sequencer components to implement MIDI handling for Delphi](<https://sourceforge.net/projects/midisequencer/files/src/>) (source)
  * Original Delphi components are MPL licensed and can still be found here: 
    * <https://web.archive.org/web/20120106202644fw_/http://www.wilsonc.demon.co.uk:80/delphi_2006.htm>
    * <https://web.archive.org/web/20100119020313fw_/http://www.wilsonc.demon.co.uk:80/d10midicomponents.htm>
  * Original Delphi 3 examples: 
    * <https://web.archive.org/web/20080509150159fw_/http://www.wilsonc.demon.co.uk/delphi3.htm>
  * [TMidiInput and TMidiOutput components](<http://breakoutbox.de/pascal/pascal.html#TMidi>)
  * [Alan Warriner TMidiGen components](<http://www.delphipages.com/comp/tmidigen-4231.html>)
  * [LazPlayMidi](<https://sourceforge.net/projects/lazprojects/files/LazPlayMidi>) \- Lazarus MIDI Player
  * [PianoEx](<https://torry.net/quicksearchd.php?String=pianoex&Title=Yes>) \- This is a very simple application, which can open MIDI file and simulate play using virtual keyboard: Play Midifile simulating a piano; Support MIDI IN/OUT; Colorful Piano key down; Tracks use different color key. Includes MidiFile, MidiPlayer, MidiIn / MidiOut, PianoKeyboard, PianoTracks / PianoChannels compontents. [Delphi 7]
  * <https://bitbucket.org/avra/ct4laz/downloads/pl_win_midi.zip>



## External links

  * [Playing MIDI with VLC](<https://wiki.videolan.org/Midi/>)
  * [MIDI Association](<https://www.midi.org>)



## See also

  * [Multimedia Programming](<Multimedia_Programming.md> "Multimedia Programming")

---

_Source: [https://wiki.freepascal.org/MIDI](https://web.archive.org/web/20250217101924/https://wiki.freepascal.org/MIDI)_
