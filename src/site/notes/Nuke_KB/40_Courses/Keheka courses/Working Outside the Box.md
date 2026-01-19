---
{"dg-publish":true,"permalink":"/nuke-kb/40-courses/keheka-courses/working-outside-the-box/"}
---

In this article we'll look at:

- Human senses and their limitations.

- How to fix flickering footage.

- How to deal with rolling shutter.

- How we can benefit from saccadic masking.

- Two different methods of frequency separation.

- A brief look at ultraviolet and infrared keying.

# Phantom Terrains

We are surrounded by a sea of invisible light and inaudible sounds.

Our human vision and hearing are quite limited on a grand scale. Each sense is confined to perceive only a narrow range within the broad spectrum of electromagnetic and mechanical waves in our surroundings.

[![](https://www.keheka.com/content/images/2023/06/UV_flower-1.jpg)](https://www.keheka.com/content/images/2023/06/UV_flower-1.jpg)

A flower bathed in ultraviolet light reveals previously unseen colours and patterns.

There are many things – often right in front of us – which we simply can’t see. Or hear.

### Human Hearing

The frequency range of human hearing is commonly given as 20 to 20,000 hertz.

Which means that we for example can’t hear the high pitched sounds of the Wi-Fi or radio signals around us.

Your Wi-Fi router at home is constantly 'singing' instructions back and forth with your phone, tablet, TV, and other devices. But because it happens at a frequency of 2.4 or 5 gigahertz, you simply can’t hear it.

Frank Swain and Daniel Jones translated urban network signals to audible sound in their project [Phantom Terrains](https://phantomterrains.com/?ref=keheka.com).  
← You can listen to a recording of a short walk through central London.  
It's a barrage of mechanical bird song and noise – in the form of  
clicks, pings, and tones, constantly broadcasting all around us.

Certain animals can hear and/or communicate at ultrasonic frequencies (above  
human hearing range), such as bats or dolphins. So can man-made  
equipment, like the ultrasound machines we use for imaging life inside  
of the womb.

Other animals and natural phenomena also operate at _infrasonic_ frequencies (below human hearing range), such as elephants, earthquakes, or volcanoes. Man-made sources like sonic booms and explosions also produce infrasonic sound.

**And so there is a whole world of sound which is hidden in plain sight, or rather, within plain earshot.**

Audio engineers have to handle a set of unique issues related to sound, such  
as interference or background noise. Things that our ears tune out but  
the recording equipment captures.

However, compositors rarely deal with audio other than purely as a reference – so let's move on to things we can('t) see.

### Human Vision

Our eyes respond to wavelengths of light of about 380 to 750 nanometers.

And so we can’t see infrared or ultraviolet light, for example.

[![](https://www.keheka.com/content/images/2023/06/Human_vs_bird_vision-1.jpg)](https://www.keheka.com/content/images/2023/06/Human_vs_bird_vision-1.jpg)

‌Humans just see a dark bird, while birds see vivid nuances with their ultraviolet vision.

**And so there is also a whole world of light and colours which is hidden in plain sight.**

We have known about this for a long time, and have been using it for years in all sorts of technology, such as remote controls, phones, medical devices, the [Microsoft Kinect](https://m.youtube.com/watch?v=dTKlNGSH9Po&ref=keheka.com), and tanning salons.

– Maybe UV imaging could be a good idea for a smart glasses app: a way to better visualise the world around us. ~~(Just remember to switch it off before you enter a motel room).~~

Anyway, let’s look at more practical scenarios:

# The Practical World Beyond Our Spectrum

You may have read stories a few years back about phone cameras with ["X-ray" vision](https://www.theverge.com/2020/5/15/21259723/oneplus-8-pro-x-ray-vision-infrared-filter-see-through-plastic?ref=keheka.com), where the phone's infrared sensor was picking up things that we can't see with our naked eye.

> [!important]
> 
> Security cameras and night vision cameras also often shine an infrared light to be able to see in low-light conditions. While _you_ can't see the infrared light, [your phone could reveal it](https://www.theverge.com/23550845/smartphone-hidden-camera-android-ios-how-to?ref=keheka.com).

You may also have seen [Nivea's marketing campaign](https://www.nivea.co.uk/highlights/uv-sun-camera?ref=keheka.com), showing the effect of sunscreen on your skin through an ultraviolet camera.

These are just a few examples of things happening outside of our range of vision.

But our vision is also limited by how quickly our brain can process what we see. In the context of moving images, it becomes harder and harder to distinguish between increases in the frame rate above ~90 frames per second. And typically at around 200 FPS images appear as real life  
motion. ([Source](https://azretina.sites.arizona.edu/node/837?ref=keheka.com)).

Cameras can capture footage much faster than that. As an example, the [Phantom T4040](https://www.phantomhighspeed.com/products/cameras/tseries/t4040?ref=keheka.com) can record 9,350 FPS at 2560 x 1664 resolution, and up to 444,440 FPS at lower resolutions.

It can easily capture things that we can’t perceive with our naked eye. The individual flaps of a bumblebee's wings, a fired bullet exiting the barrel of a rifle, or lightning forking out as it finds a path through the air and towards the ground.

However, you don't need an ultra slow motion camera like that to get superhuman vision – even normal cameras can catch things we don’t see. Things such as:

### Flickering Lights

Depending on where you live in the world, the utility frequency is typically either [50 or 60 hertz](https://www.kccscientific.com/50hz-60hz-guide-frequency-converter-power/?ref=keheka.com). That means the electric current in your house switches polarity back and forth again either 50 or 60 times per second.

When you connect for example a lamp to this alternating current (AC), the light turns on. However, even though it looks like it to us, the lamp doesn’t stay fully lit at a constant rate.

The filament in the lightbulb heats up and cools down with every half-cycle of the alternating current. It’s like a saw going back and forth: each time it changes direction it stops cutting the wood for a brief moment.

Because this happens so fast, the filament doesn’t have enough time to cool all the way down until there is no light emitting from it anymore. However, it does cool down enough to make the brightness of the light dip.

This fast flickering of light (100 or 120 times per second for 50 or 60 hertz AC, respectively) is quick enough that it appears to fuse into a continuous source of light (to our eyes). We don’t notice the  
flickering, but cameras do.

> [!important]
> 
> This happens with alternating current. Direct current (DC) doesn’t alternate the polarity, and so a battery powered flashlight won’t cause the same problems as for example fluorescent office lights when the camera is recording.

> [!info] Hallway Lights (Flickering)  
>  
> [https://youtu.be/SNy5e63rB1E](https://youtu.be/SNy5e63rB1E)  

Filming the lights in my hallway in slow motion. I can’t see this flickering in person.

> [!info] Flashlight (No Flickering)  
>  
> [https://youtu.be/-pnXVZOJS9I](https://youtu.be/-pnXVZOJS9I)  

Filming the flashlight from a phone in slow motion. With DC battery power, there is no flickering.

If you’re not careful, flickering can become a problem when shooting on location. Lights and monitors may flicker in frame, potentially making the footage unusable or difficult to use.

To remove flickering from your footage, you have at least four options:

1. Synchronise the frame rate with the lighting.

1. Synchronise the shutter speed with the lighting.

1. Change the lighting.

1. Fix the flickering in post.

If you decide to pursue options 1 or 2, RED has a free [online tool](https://www.red.com/tools?ref=keheka.com#flicker-free-video) for determining flicker safe camera settings.

For the third option, here are some tips for [on-set lighting](https://www.promotionhire.com/tips-advice/what-lighting-do-i-need-for-slow-motion/?ref=keheka.com).

And for the last option, you can for example use my [DeFlicker](https://www.keheka.com/add-or-remove-flickering-in-nuke/) tool in Nuke.

### Rolling Shutter

Rolling shutter is a synchronisation issue in the sensor of the camera.

Destin explains it really well [in his video](https://www.youtube.com/watch?v=dNVtMmLlnoE&ref=keheka.com). Essentially, unless the camera has a global shutter, the sensor doesn’t capture the whole image at exactly the same moment. Which can lead to  
interesting artefacts, such as skewed vertical lines, or wobbly motion.

To help prevent rolling shutter, [adjust the shutter speed to twice the frame rate](https://www.adobe.com/uk/creativecloud/video/discover/rolling-shutter-effect.html?ref=keheka.com). If your footage already contains rolling shutter and you need to fix it in post, there are tools for that such as the [Rolling Shutter Repair](https://helpx.adobe.com/uk/premiere-pro/using/rolling-shutter-repair.html?ref=keheka.com) in Adobe Premiere Pro.

> [!important]
> 
> Nuke used to have a RollingShutter node, but for some reason (perhaps licensing issues?) it’s unfortunately no longer available. In some cases, such as with the skewed verticals, you can get away with a simple Transform skew/CornerPin, but with more complex warping, you may have  
> to resort to other clean up methods or external tools.

Let’s move on to another interesting phenomenon, that we can benefit from in our compositing.

### Saccadic Masking

When we move our eyes quickly, our brain briefly blocks visual information to prevent motion blur and nausea.

And so for short moments at a time when we look around, we counterintuitively actually _stop_ looking. This phenomenon is called [saccadic masking](https://www.scienceabc.com/humans/what-is-saccadic-masking.html?ref=keheka.com).

Because we are naturally less adept at picking out details in motion blurred movement, it's a good idea to hide plate stitches or other transitions in motion blur. For example using a whip pan or a zoom transition.‌

> [!info] Whip Pan  
>  
> [https://www.youtube.com/watch?v=dxCnNSfPo10](https://www.youtube.com/watch?v=dxCnNSfPo10)  

Whip pan.

  

> [!info] Zoom Transition  
>  
> [https://youtu.be/esW1URXQguc](https://youtu.be/esW1URXQguc)  

Zoom transition.

Next, let’s look at the benefits of manipulating the different frequencies in an image:

### Frequency Separation

‌While we see the light hitting our eyes/camera as a cohesive image,  
it’s possible to separate the frequencies that make up the image in Nuke.

Frequency separation is a very useful technique where you split the image into two parts: the high frequency texture/detail, and the low frequency colour/shading.

This is great for shots where you want to retouch an actor’s skin, relight an object, or transfer or restore texture to an object.

The most common way to achieve this is by using a divide/multiply setup:

[![](https://www.keheka.com/content/images/2023/06/Divide_Multiply-1.png)](https://www.keheka.com/content/images/2023/06/Divide_Multiply-1.png)

‌Divide/multiply setup for separating image frequencies.

By increasing the _size_ value in the Blur node, you start to separate the image into low and high frequencies. You can then do any relevant work in the two pipes before combining them together at the end with a Merge (multiply).

> [!important]
> 
> Aim to keep the Blur value as low as possible. This method does introduce a faint halo around edges in your image which gets more pronounced the more you blur the original image.

### Fast Fourier Transformations

Another way to represent and manipulate the frequencies in an image is to use [Fast Fourier Transformations](https://www.jezzamon.com/fourier/?ref=keheka.com) (FFT).

You can access the FFT nodes in Nuke by pressing X in the node graph and typing **FFT** and **InvFFT** (case sensitive). The FFT node will convert your image to frequency space: a representation of the frequencies the image contains. The InvFFT node will invert the frequency space back to a normal image again.

> [!important]
> 
> > There is also an **FFTMultiply** node which can be accessed the same way. It lets you convolve an image using another image of equal size, very quickly, in Fourier space.

‌When you apply an FFT node to your image, the frequencies of the image are sorted and laid out across the frame, going from low frequencies on the left to high frequencies on the right.

[![](https://www.keheka.com/content/images/2023/06/FFT-1.png)](https://www.keheka.com/content/images/2023/06/FFT-1.png)

Example of a picture’s frequency space. (For viewing purposes, the Viewer gain has been lowered).

Typically, you should avoid changing much on the left hand side of the frequency space. Altering these low frequencies will have the greatest impact on your image and can cause severe artefacts. (Feel free to experiment, though!).

Most of the work you’ll do will usually be on the right hand side. You can stencil out or paint black over a fairly large section of the high frequencies (deleting them from the image) before you see any major structural changes in the image.

You can also clone one area (from the same frequency space, or from a different one) to another, to replace troublesome frequencies.

Use the FFT technique to remove (or add) image artefacts. For example, dust or scratches on the film, compression artefacts, noise, or fine interference patterns.

[![](https://www.keheka.com/content/images/2023/06/Interference_Before_After-1.png)](https://www.keheka.com/content/images/2023/06/Interference_Before_After-1.png)

‌Interference pattern before and after frequency clean up using FFT.

> [!important]
> 
> In the Advanced Options section of my [Concealer](https://www.keheka.com/concealer-advanced-retouching/) tool, you can enable frequency deletion. That option uses this same FFT technique, combined with a slider for easy control. Note that you may still have to go in and paint certain areas in the frequency space to remove very specific artefacts.

### UV-Keying And Thermo-Keying

There are other interesting VFX applications for light outside of the visible spectrum.

Using [ultraviolet](https://www.vfxvoice.com/a-generation-of-star-trek-effects-on-tv/?ref=keheka.com) or [infrared](https://nae-lab.org/project/thermo-key/?ref=keheka.com) as the key colour, either with or without a backdrop, you can generate mattes for subjects and objects instead of using the standard blue/green screens or rotoscoping.

On Star Trek: The Next Generation, the team successfully used a fluorescent orange backdrop lit by  
ultraviolet light for the miniature shoots.

You could also paint UV makeup or tracking markers on a performer’s face, or on a backdrop. And combine that with for example the Contour Reality Capture by [Mova](http://www.mova.com/?ref=keheka.com).

Imagination is really the limit here. If nothing else, I hope this tutorial has further opened your mind to look for solutions outside the human spectrum. I was recently watching this [video by Mark Rober](https://youtu.be/md75n8cyenA?ref=keheka.com) being chased by a bloodhound, and it just further cemented the fact in my mind that our human senses are limited.

– That a whole world exists outside our box.