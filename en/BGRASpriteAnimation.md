# BGRASpriteAnimation

│ **English (en)** │

## Contents

  * 1 Overview
  * 2 Beginning
  * 3 Animation
  * 4 Sprite
  * 5 Events
  * 6 How to Create Sprites?
  * 7 How to load images with mutiple rows?



## Overview

BGRASpriteAnimation is a graphic control developed with the [BGRABitmap](<BGRABitmap.md> "BGRABitmap") library. It is part of the [BGRAControls](<BGRAControls.md> "BGRAControls") package. This control is usefull to create animations from bitmaps. 

To use as replace of TImage just turn on the option 'AnimStatic' and you'll be able to load single images. 

## Beginning

Add a new BGRASpriteAnimation from the "BGRA Controls" component tab. 

The next step is to load a "sprite". A sprite is a bitmap that contains all the frames of an animation. To load a sprite select "BGRASpriteAnimation1". In the Object Inspector click on the property "Sprite (TBitmap)[...]". Load the sprite the same way as you load any other bitmap. 

This is the sprite we will use to explain how the component works: 

[![bgraspriteanimation main sprite.png](https://wiki.freepascal.org/images/e/e2/bgraspriteanimation_main_sprite.png)](</File:bgraspriteanimation_main_sprite.png>)

We can say the sprite dimensions are 100x500 px. It has 5 frames, so each frame is 100x100 px. 

The next step is to tell the component that the sprite has 5 frames. To set the number of frames in the Object Inspector click on the property "SpriteCount" and set the value to 5. 

Now you can run the application and see the resulting animation. 

## Animation

The animation properties are AnimInvert, AnimPosition, AnimRepeat, AnimRepeatLap, AnimSpeed & AnimStatic. 

**AnimInvert:** By default the animation is played from AnimPosition to SpriteCount. If you set AnimInvert to True the animation will be played inversely, that is from the last frame to the first. 

**AnimPosition:** The position of the frame in the animation. 

**AnimRepeat:** The number of times the animation will be played. Set 0 for infinite. 

**AnimRepeatLap:** The number of times the animation was played. If it reaches the value of AnimRepeat the animation will stop. 

**AnimSpeed:** The speed in milliseconds between frames. 

**AnimStatic:** Use to start & stop the animation. 

## Sprite

The sprite properties are Sprite, SpriteCount, SpriteFillOpacity, SpriteFlipMode, SpriteKeyColor, SpriteResampleFilter, SpriteResampleMode, SpriteRotation. 

**Sprite:** A bitmap that contains all the frames of the animation. 

**SpriteCount:** A number that indicates the amount of frames of the sprite. 

The following properties are sprite modifications. 

**SpriteFillOpacity:** The amount of opacity from 0 to 255. 

**SpriteFlipMode:** Flip none, horizontal, vertical or both. 

**SpriteKeyColor:** The color that will be replaced with transparency. 

**SpriteResampleFilter:** The filter that will be used on resizing. 

**SpriteResampleMode:** If you choose FineResample the selected resample filter will be used. 

**SpriteRotation:** Rotation none, clockwise, counterclockwise. 

## Events

OnRedrawBefore, OnRedrawAfter, OnPositionChanging, OnPositionChanged, OnLapChanging, OnLapChanged in that order. 

The following events are specific to sprites. 

**OnRedrawBefore:** This fires before applying the sprite modifications. 

**OnRedrawAfter:** This fires after applying the sprite modifications, but before applying resample. 

The following events are specific to animation. 

**OnPositionChanging:** This fires before the animation changes the AnimPosition value. 

**OnPositionChanged:** This fires when the AnimPosition value has changed (by the animation or by the user). 

**OnLapChanging:** This fires before the animation changes the AnimRepeatLap value. 

**OnLapChanged:** This fires when the AnimRepeatLap value has changed (by the animation or by the user). 

## How to Create Sprites?

Most Graphics software (Gimp, Photoshop) can create Animations as Image Sequences. In Windows you can use IrfanView's (Freeware) panoramamode to Join these Images into a sprite. (Image> Create Panorama Image…) 

## How to load images with mutiple rows?

Check [this forum post](<https://forum.lazarus.freepascal.org/index.php/topic,49164.msg355639.html#msg355639>) project attachment.

---

_Source: [https://wiki.freepascal.org/BGRASpriteAnimation](https://web.archive.org/web/20250417144038/https://wiki.freepascal.org/BGRASpriteAnimation)_
