# Perlin Noise

│ **English (en)** │  [**français (fr)**](</Perlin_Noise/fr> "Perlin Noise/fr") │  [**中文（中国大陆）‎ (zh_CN)**](</Perlin_Noise/zh_CN> "Perlin Noise/zh CN") │    
****

This page is the start of a tutorial about using Perlin Noise on LCL applications to generate natural looking images. It will cover both basic theory and real usage examples, with a focus on compilable examples. 

Perlin Noise was invented by [Ken Perlin](<http://mrl.nyu.edu/~perlin/>) to generate textures for a movie called Tron. Today it is widely used on movies and video games to produce natural looking smoke, landscapes, clouds and any texture including marble, irregular glass, etc. 

## Contents

  * 1 Getting Started
  * 2 First Example
  * 3 Persistence Example
  * 4 Use Perlin noise to create textures
  * 5 Subversion
  * 6 External Links



## Getting Started

Perlin Noise is based on the idea of fractals, that things in nature show different degrees of change. On a rocky mountain landscape for example when can see changes with a very big amplitude, which are the mountains themselves. Smaller changes represent irregularities on those mountains and even smaller ones represent rocks. 

[![mountain landscape.png](https://wiki.freepascal.org/images/6/6d/mountain_landscape.png)](</File:mountain_landscape.png>)

  


## First Example

This application demonstrates a simple noise function with the following properties: 

  * Only 1 harmonic present
  * Amplitude of 250 pixels
  * Wavelength of 20 pixels
  * Frequency of 0.05
  * You can use a combo box to choose between Linear, Cossine and Cubic interpolation



[![Noise1D.png](https://wiki.freepascal.org/images/8/86/Noise1D.png)](</File:Noise1D.png>)

Files: 

  * noise1d.lpi
  * noise1d.dpr
  * noise.pas



## Persistence Example

This application demonstrates how to sum many noise functions to get a perlin noise function. It has the following properties: 

  * 3 harmonics present
  * Amplitudes of 250, 125 and 62 pixels
  * Wavelength of 20, 10, and 5 pixels
  * Frequency of 0.05, 0.1, 0.2
  * You can use a combo box to choose between Linear, Cossine and Cubic interpolation



[![Perlin1D.png](https://wiki.freepascal.org/images/8/85/Perlin1D.png)](</File:Perlin1D.png>)

Files: 

  * perlin1d.lpi
  * perlin1d.dpr
  * noise.pas



## Use Perlin noise to create textures

It is possible to create tilable textures of stone, water, wood... with Perlin noise. 

Here is a tutorial on how to do this: [BGRABitmap tutorial 8](<BGRABitmap_tutorial_8.md> "BGRABitmap tutorial 8")

## Subversion

You can download the source code for the examples and the library using this command: 

svn co <https://svn.code.sf.net/p/lazarus-ccr/svn/examples/noise> noise 

## External Links

  * [Article](<http://web.archive.org/web/20160325134143/http://freespace.virgin.net/hugo.elias/models/m_perlin.htm>) with the theory of Perlin Noise.

---

_Source: [https://wiki.freepascal.org/Perlin_Noise](https://web.archive.org/web/20200921101902/https://wiki.freepascal.org/Perlin_Noise)_
