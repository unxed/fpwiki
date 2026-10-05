# LiteSound

## Contents

  * 1 About
  * 2 Download
  * 3 Examples
  * 4 Third party components used



## About

LiteSound is a large set of wrappers, classes and structures for working with sound. Here you can find a set of classes for playing, recording and converting audio files and audio streams, for resampling, spectrum extraction and filtering of audio signal. Uses the principles of dynamic linking of libraries. Supports audio formats WAV, FLAC, Vorbis and Opus (including for the organization of streaming audio frames). The structure of the packages has not yet been fully determined. Author continues to work on structuring the class hierarchy. 

Author: Ilya "iLya2IK" Medvedkov. 

License: LGPL-2.1. 

Requirements: 

  * Free Pascal Compiler
  * Lazarus IDE (optional / for examples)
  * CommonUtils
  * SoundUtils
  * libOpenALsoft_ilya2ik - free pascal bindings and wrapper around OpenAL library
  * libOGG_ilya2ik - free pascal bindings and wrapper around OGG library
  * libFLAC_ilya2ik - free pascal bindings and wrapper around FLAC library
  * libVorbis_ilya2ik - free pascal bindings and wrapper around Vorbis library
  * libOpus_ilya2ik - free pascal bindings and wrapper around Opus library
  * HTTP2 Web Camera Client (optional / for WCRadioClient example)
  * REST Web Camera server (optional / for WCRadioClient example)



## Download

Main repository: <https://github.com/iLya2IK/LiteSound>

Requirements: 

  * <https://github.com/iLya2IK/commonutils>
  * <https://github.com/iLya2IK/SoundUtils>
  * <https://github.com/iLya2IK/libOpenALsoft_dyn>
  * <https://github.com/iLya2IK/libOGG_litedyn>
  * <https://github.com/iLya2IK/libFLAC_litedyn>
  * <https://github.com/iLya2IK/libVorbis_litedyn>
  * <https://github.com/iLya2IK/libOpus_litedyn>
  * <https://github.com/iLya2IK/wccurlclient> (optional / for WCRadioClient example)
  * <https://github.com/iLya2IK/wcwebcamserver> (optional / for WCRadioClient example)



## Examples

  * Player example - This example emulates the operation of a simple audio player, demonstrates working with comments, metadata, playlists.
  * Recorder example - This example emulates the operation of a simple audio recorder.
  * Converter example - This example emulates the operation of a simple audio file converter.
  * FrameStreaming example - In this example, an audio-ogg file is opened and decoded into a data stream. The resulting stream is then reencoded into a set of encoded frames. A set of encoded frames is saved to the files on disk in the user’s format. Frame-files are then opened, decoded into a data stream and saved in a new file in selected format.
  * Resampler - This example emulates the operation of a simple speex resampler.
  * fft - This example emulates the operation of a simple fast discrete Fourier transform and inverse.
  * Spectre - This example shows a visualization of a fast discrete Fourier transform for an audio stream using OpenGL.
  * WCRadioClient example - An example of the implementation of an Internet radio source and receiver.



## Third party components used

  * OpenAL soft
  * OGG
  * libFLAC
  * Vorbis/VorbisEnc/VorbisFile
  * Opus/OpusEnc/OpusFile
  * OpenGL v3.3 (optional - for Spectre example)

---

_Source: [https://wiki.freepascal.org/LiteSound](https://web.archive.org/web/20241106160629/https://wiki.freepascal.org/LiteSound)_
