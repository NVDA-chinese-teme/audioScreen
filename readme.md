# AudioScreen: an experiment inimage accessibility for blind people
By Michael Curran, NV Access Limited
## Introduction
Audio Screen is an add-on for the [NVDA Screen Reading software](http://www.nvaccess.org/). Audio Screen can allow a blind person to move a finger around a Windows 8+ compatible touch screen, and hear the part of the image under their finger. If a touch screen is not available, the mouse can be moved instead, though this is some what less accurate for the user as mouse movement is relative.

As Audio Screen requires NVDA to run, the user will therefore also receive speech feedback such as the name of the control or text, directly under their finger. 

Audio Screen can be seen as an experimental alternative way for blind people to view basic images such as diagrams and maps when no other tactile format is available. 

AudioScreen has two modes of output: "pitch stereo grey" for investigating lines and contours of images (useful for maps and diagrams etc), and "HSV color" for investigating the color variation of images (useful for photographs).

## Pitch Stereo Grey mode

In this mode, AudioScreen represents the image under your finger as multiple tones that vary in pitch, volume and stereo position. The idea is based on the vOICe visual-to-auditory mapping system by Peter Meijor. (www.seeingwithsound.com) 
audio Screen strives to provide roughly the same information your finger would for tactile diagrams. You can tell when your finger moves over a line, both horizontally and vertically. 
For example if your finger crosses a horizontal line as it moves down the screen, you can hear the line move up over your finger. If your finger crosses a vertical line as you move across the screen from left to right, you will hear the line move across your finger from right to left. 

## HSV Color mode

In this mode, AudioScreen will convey the color (specifically hue, saturation and brightness) of the image under your finger. 

Hue (position in the color spectrum) is represented by a tone at a particular pitch. Currently, the lowest pitch is blue, moving up through green, yellow, orange, red (the highest pitch). As a spectrum is infact a circle, going from red, through purples back to blue, the top (red) pitch fades out, and the bottom (blue) pitch fades back in.

Saturation (how vivid the color is) is represented by brown noise (low random noise). When the color is its most vivid (full saturation) there is no random noise and the spectrum tone can be completely heard. As the saturation decreases towards grey (no saturation) the spectrum tone gets quieter and the random noise gets louder, eventually with only the random noise being heard at no saturation.

Brightness is represented by the over all volume of the sound as a whole.

Some examples:
* black: silence
* White: loud random noise
* Vivid blue: a very low tone
* Vivid yellow: a mid to high tone
* Faded red: a very high tone with some random noise.
* Dark green: a quiet low-mid tone.
* Dark grey: quiet random noise.

## System requirements
* An installed copy of NVDA 2015.4 or higher
* Windows 8 Operating system or later
* A Windows 8/10 compatible touch screen, otherwise a mouse. 

## Download
* Download [AudioScreen 1.1 [NVDA add-on file]](http://www.nvaccess.org/audioScreen/audioScreen-1.1.nvda-addon).
* Download [Example images [zip file]](http://www.nvaccess.org/audioScreen/audioScreenImages.zip).

## Running AudioScreen
After downloading the Audio Screen add-on, with NVDA running simply press enter on the file in Windows Explorer to have NVDA install it for you. Alternatively, you can open Manage Add-ons found under Tools in the NVDA menu, and add it from there. After the add-on is installed, NVDA will ask to be restarted. 

While NVDA is running with this add-on installed, open an interesting image in full-screen (For example, use Internet Explorer to display one of the example svg files, making sure to maximize it and put it in full-screen with f11). 

AudioScreen is off by default, so turn it on by switching to one of its modes by pressing NVDA+control+a. This toggles between Pitch stereo grey, HSV color, and off.

Now move your finger around the screen and start listening to the image under your finger. 
As NVDA can also speak controls and text under your finger, viewing an SVG map or diagram works great, as Internet Explorer will allow NVDA to speak the title and or description for any shape your finger moves over, assuming that a title and or description have been properly defined using the title and desc tags appropriately in the SVG file. 

## Settings
### Audio Mode (Press NVDA+Control+a)
This command toggles between several modes: 
* pitch stereo grey: for investigating lines and contours of images (useful for maps and diagrams etc)
* HSV color: for investigating the color variation of images (useful for photographs).
* Off [default]: completely disables AudioScreen.

### Reverse brightness (press NVDA+shift+a):
When in pitch stereo grey mode, this toggles between light on dark and dark on light. If viewing an image  made of black lines on a white background, it may be useful to switch to light on dark so that it considers black a s brighter than white.
 
## Bugs
There are probably heeps. This is really only an experiment, and I have only ever run it on two machines myself. 

## Background
For quite some time now, I have wanted a way to get access as a blind person to maps and basic diagrams with out the hassles of having to produce them in a tactile format. 

Although tactile formats are certainly very useful when they are available, they do have particular drawbacks, such as: 
* They are slow to produce
* Special materials are needed
* Sometimes a special machine is needed
* Rather non-environmental due to paper wastage
* Bulky to carry around
* Only very limited labeling can be included due to space constraints

A few years ago, I stumbled upon a very interesting peace of software called The vOICe, from www.seeingwithsound.com. This software could take images or video from a webcam, and play it to a blind person as audio, making use of pitch, volume and stereo panning. Although the vOICe software is more passive in the sence that it scans from left to right across an entire image, I could see many possibilities of using this image-to-sound mapping concept in a more active way, with the help of a touch screen. 

I should also note that I am aware of other research in to conveying images on touch screens with sound, but none of them that I have seen so far, have yet chosen to try using the vOICe mapping concept. 

When conveying information from one sence modality to another, I believe its very important not to loose information in the process. If you can provide roughly the same or better resolution in the second sence, the brain will have a much easier time of decoding the information. A mapping such as the vOICe I believe certainly gets extremely close to achieving this. 

### How it works (technical info)
The tone and noise generation is handled by [libaudioverse](https://github.com/camlorn/libaudioverse).

Audio Screen detects when your finger touches, moves around, or comes off the screen. 

Each time NVDA detects your finger moving to a set of coordinates, The add-on captures  a bitmap of the part of the screen image within a rectangle centered around those coordinates by using NVDA's screenBitmap module. 

Once it has this image, it submits it to audioScreen's imagePlayer module, whos job it is to actually map the image to sound. 