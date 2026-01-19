---
{"dg-publish":true,"permalink":"/nuke-kb/40-courses/keheka-courses/motion-blur-masterclass/"}
---

# The Importance Of Motion Blur

Motion blur has a major impact on photography.

It can drastically change the look and feel  
of both still photos and movies. By influencing the motion blur, you can  
change a shot from looking dreamy and soft, to hyper-realistic,  
staccato, or even frozen-in-time.

A specific amount of motion blur is also  
associated with the cinematic look of movies. Getting the motion blur  
wrong can actually pull the audience out of their state of suspended  
disbelief.

**So it’s important for compositors to know how to correctly work with motion blur.**

As we will explore further in this tutorial, motion blur is heavily affected by one of the three pillars of photography: [the shutter speed](https://photographylife.com/what-is-shutter-speed-in-photography).  
And the shutter speed has to be in harmony with the other two pillars,  
aperture and ISO (as well as the lighting on set), in order to get a  
balanced exposure. Motion blur is therefore intrinsically linked to  
depth of field and image noise.

Because it determines the available shutter  
speeds, the capture frame rate is another element that affects motion  
blur. And going further down the rabbit hole, there are more factors  
influencing motion blur as well, which we’ll look into.

It can be a fairly complicated subject, but  
it’s very much worth getting a better understanding of motion blur in  
order to produce more natural looking imagery (or to bend reality, if  
you so choose).

Let’s break it all down in a simple but  
thorough way, starting by analysing what motion blur is and how it’s  
linked to the way cameras work. Then, we'll delve deeper into advanced  
techniques for working with motion blur when compositing in Nuke.

# Motion Blur And Cameras

To understand motion blur, we first have to understand how cameras capture reality.

Cameras are similar to our eyes, in that they  
guide and focus the light from a scene into a specific, light sensitive  
area – which then reacts to the photons and captures images of the  
scene. In a camera, that process typically looks like this:

– We'll use a Digital Single Lens Reflex (DSLR) camera for this example:

Light travels from the scene that’s being  
photographed, to the lens of the camera. Once it enters the lens, the  
light is focused through the several layers and groups of glass elements  
inside.

[![](https://www.keheka.com/content/images/2023/02/LensAnatomy.jpg)](https://www.keheka.com/content/images/2023/02/LensAnatomy.jpg)

The anatomy of a lens.

> [!important]
> 
> The cornea and the various liquid layers on and inside our eyes  
> focus the light on the way to the retina. The glass elements inside a  
> lens work in a similar way, and focus the light en route to the image  
> sensor.

On its path through the lens and further into the camera body, the light encounters **three main obstacles**.

### 1. Aperture

The first main obstacle the light faces  
(assuming the lens cap is off) is a mechanical diaphragm in the lens.  
The diaphragm creates an adjustable opening called [the aperture](https://photographylife.com/what-is-aperture-in-photography).

The aperture determines the size of the area  
through which the light can pass. A wide opening lets in more light, and  
a narrow opening lets in less.

> [!important]
> 
> Our pupils control the amount of light that's allowed into our eyes. When dilated, they let more light in. When contracted, they let in less. The aperture works in the same way.

[![](https://www.keheka.com/content/images/2023/02/Aperture.jpg)](https://www.keheka.com/content/images/2023/02/Aperture.jpg)

Three different apertures (openings) resulting from three different positions of the diaphragm (blades).

The light that makes it through the aperture continues onward, and is focused through more layers of glass elements in the lens.

> [!important]
> 
> It should be noted that a little bit of light gets lost along the  
> way through the glass elements, due to reflection and because 100%  
> transmission isn't possible. See [F-Stop Vs T-Stop](https://www.picturecorrect.com/f-stop-vs-t-stop-whats-the-difference/) for more information. Let’s consider this loss of light to be minor, and continue on with the main obstacles on the light’s path.

### (2.) Mirror

Next, the light reaches the second main obstacle on its path: [a mirror](https://www.youtube.com/watch?v=ptfSW4eW25g).

The mirror redirects the light up and through a pentaprism, and out through the optical viewfinder.

[![](https://www.keheka.com/content/images/2023/02/DSLR.png)](https://www.keheka.com/content/images/2023/02/DSLR.png)

DSLR diagram showing how the mirror works.

When you take a picture, the mirror flips up and out of the way, letting the light pass through and further into the camera body. (Note that, as the name suggests, **mirrorless cameras do not have this second obstacle**. They have an electronic viewfinder instead of an optical one).

> [!important]
> 
> This is why the viewfinder goes dark for a brief moment when you  
> take a photo. While the mirror is tucked away, it no longer redirects  
> the light up to the viewfinder.

When the mirror is in the up-position (or in the absence of a mirror), the light continues further into the camera body.

### 3. Shutter

The light then faces the third and final main obstacle on its path (or second, depending on the camera): [the shutter](https://expertphotography.com/camera-shutter/).

The shutter is essentially a mechanical  
blackout curtain in front of the image sensor. It either lets the light  
through, or it blocks the light from hitting the sensor.

**It’s important to note that the shutter determines** _**for how long**_ **the image sensor is exposed to light.**

> [!important]
> 
> Our eyelids control whether or not light is allowed to enter our  
> eyes, and for how long. When open, they let light in. When closed, they  
> block the light from entering. The shutter works in the same way, it’s  
> just positioned later in the chain of events. While our eyelids are in  
> front of the ‘lens’ (our eyes), the shutter is behind the camera’s lens –  
> right in front of the image sensor. Still, its effect remains the same.

[![](https://www.keheka.com/content/images/2023/02/Shutter.jpg)](https://www.keheka.com/content/images/2023/02/Shutter.jpg)

The (closed) shutter in a DSLR camera.

> [!important]
> 
> The shutter doesn’t necessarily have to be a mechanical one; cameras can also have [an electronic shutter](https://www.canon-europe.com/pro/infobank/electronic-vs-mechanical-shutter/).

A fast shutter speed means the shutter opens  
and closes quickly, and only a small fraction of light is allowed to  
pass through to the image sensor. A slow shutter speed means the shutter  
stays open for longer, allowing more light to pass through.

> [!important]
> 
> The [image sensor](https://www.photometrics.com/learn/imaging-topics/what-happens-when-light-hits-a-pixel) works in a similar way to our retina. It’s where the final product is projected for further processing.

### ISO

For the sake of completing the camera-eye analogy, the third pillar of photography, [the ISO](https://photographylife.com/what-is-iso-in-photography), controls the camera’s ‘_sensitivity_’ to light (the mapping of how bright the output should be, given the input).

It’s similar to the photoreceptors on the retina. The rods and cones can [adjust their sensitivity to light](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2231822/), to adapt to higher or lower levels of light in the scene. The ISO setting works in the same way.

Note that increasing the ISO (or  
photoreceptor sensitivity) doesn’t physically capture more light, it  
essentially just brightens the photo you’ve already captured.

> [!important]
> 
> You can read more about how the eye’s properties relate to cameras [here](https://www.bhphotovideo.com/c/find/newsLetter/The-Photographic-Eye.jsp).

### The Impact Of These Obstacles On Motion Blur

The first two obstacles the light encounters,  
the aperture and the mirror, only have an indirect effect on the motion  
blur – in that they either allow light to pass through or not.

They only control whether or not motion blur has the _opportunity_ to happen. However, they don’t affect the look or the amount of motion blur (other than on/off).

(And for reference, ISO also has no direct  
effect on motion blur. It aids in getting the image brightness right  
when adjusting the other settings, though, allowing us to choose from  
more options of shutter speed and aperture size).

**However, the final obstacle – the shutter – plays an important role when it comes to motion blur.**

To explain why, we have to look at what actually happens when we take a picture.

# The Factors Of Motion Blur

When you snap a photo, the camera does not perfectly capture a single instant of time in the image it creates.

What happens, rather, is that the camera captures a narrow window of time. And importantly, **it captures any changes in the scene within that short time frame**.

Remember, the shutter controls the amount of  
time the image sensor is exposed to light. So it controls how long the  
window is open for things (or the camera) to move around or change in  
the scene.

If an object in the scene (and/or the camera  
itself) moves quickly enough, the camera will capture that motion in the  
picture. All the positions of the object or scene along the path it  
travels during that short time will be blended/smeared together in the  
exposure.

> [!important]
> 
> An exposure is the accumulated reaction to the light hitting the  
> film or image sensor in the camera, from the time the shutter opens  
> until it is closed again.

This blending/smearing is what we see in the picture as motion blur.

Since movies are just a series of pictures in a row, **the exact same behaviour applies when filming footage**.

### Simplifying Motion Blur

Calculating exact motion blur from scratch gets quite complicated.

[Papers have been written about it](https://www.cs.drexel.edu/~david/Classes/Papers/p389-potmesil.pdf) (right-click → Save As) and it's a hot topic, not only within the film and VFX community, but also in other fields such as [industrial machine vision](https://www.vision-doctor.com/en/camera-calculations/calculation-exposure-time.html) and [drone mapping/photogrammetry](https://www.hammermissions.com/post/preventing-motion-blur-in-drone-photogrammetry-flights). (Where motion blur ideally should be kept at a minimum).

Long story short, there are a lot of complex calculations involved.

We'll let Nuke and our 3D package of choice  
do the mathematical heavy lifting for us, and instead look closer at how  
motion blur works.

### Two Main Factors

To simplify it for practical purposes, the amount of motion blur you get in a picture or movie depends on two things:

1. The distance the object travels across  
    the frame (and/or the distance the scene travels across the frame, if  
    the camera is moving). That is, across how many pixels is the object or  
    scene traversing during a frame interval? (The window of time allocated  
    to each frame).

1. The duration of the exposure – which is determined by the shutter speed. That is, for how long is motion captured in the scene?

> [!important]
> 
> An accelerating object or camera, significant changes to the  
> lighting in the scene, heavy lens distortion, a changing focal length,  
> and/or a rolling shutter are some other factors that would also have an  
> impact – making the motion blur uneven.

The further the object or scene travels  
across the frame, and/or the longer the exposure time, the more motion  
blur you will get. The opposite is also true.

You'll for example see strong motion blur in [whip pans](https://en.wikipedia.org/wiki/Whip_pan) and long exposure photography.

> [!important]
> 
> The _direction_ of the motion blur intuitively depends on the direction of the movement of the object or scene in relation to the camera.

By knowing the factors at play (the simplified ones, at least), we can now influence the look of the motion blur. While we can’t always control the distance the object or camera  
travels during a frame interval, we _can_ control the duration of the exposure, i.e. the shutter speed.

# Exposing For Natural-Looking Motion

For nearly a century, it has been the standard to shoot cinematic movies at 24 pictures, or frames, per second.

> [!important]
> 
> 23.976 frames per second in digital cameras, due to [legacy bandwidth limitations](https://www.youtube.com/watch?v=mjYjFEp9Yx0).  
> At first, colour and sound were transmitted as fine patterns overlaid  
> with the black and white video signal. This caused noticeable [Moiré patterns](https://en.wikipedia.org/wiki/Moir%C3%A9_pattern) in the image. The solution was to reduce the frame rate by 0.1% so the patterns never fully synced up.

With 24 frames equally distributed per second, each frame gets allocated a window of time – i.e. the aforementioned _frame interval_ – that is 1/24th of a second long. After that, the camera advances to the next frame, and the next frame interval begins.

That means, at 24 frames per second, each frame can physically be exposed for a maximum of 1/24th of a second (in digital cameras) before the camera moves on to processing the next  
frame.

(There is even less time to expose a frame in traditional film cameras, to avoid smearing the exposure when the camera physically advances the film from one frame to the next).

### We Like What We Know

However, over the last century of filmmaking, cinema goers have collectively gotten used to – and subconsciously expect – the motion to have a certain look in a cinematic movie.

That look is partly the result of using a 180 degree shutter angle. Which means, **each frame gets exposed to light for half of the window of time allocated to it**, i.e. for half of the frame interval.

When each of the 24 frames per second gets exposed for 1/48th of a second (half of their 1/24th of a second frame interval), the motion looks cinematic to our eyes.

> [!important]
> 
> This applies to cinematic movies. We are used to seeing home videos  
> or sports broadcasts shot at higher frame rates and different shutter  
> speeds, and can tell the difference in how they look.

So, when filming at 24 frames per second with a 180 degree shutter angle, each frame captures the action that's happening inside a window of time that is 1/48th of a second long.

Keep that in mind for later. But first, a sidebar:

# Shutter Angle Vs Shutter Speed

You may be thinking, what’s a shutter angle? And how does it relate to shutter speed? – A camera setting you may be more familiar with.

Traditional cinema cameras used a [rotary disc shutter](https://en.wikipedia.org/wiki/Rotary_disc_shutter) (some cameras still do). It’s a disc that spins at the same speed as the frame rate. So, at a frame rate of 24 frames per second, it spins 24 full revolutions per second.

In order to let light pass through the rotary disc shutter, a part of the disc is opened up – you can think of it as being similar to removing a slice from a pizza.

The bigger the slice that's removed, i.e. by increasing the size of the opening in the disc, the more light is let onto the film or image sensor per revolution. This adjustable setting is referred to as the shutter angle.

### The Standard: 180 Degrees

Since a disc is 360 degrees around, opening  
up half of it (180 degrees) makes a semi-circle. As it's spinning, half  
of the disc lets light pass through while the other half blocks it.

That’s called a 180 degree shutter angle.

It means the shutter will let light pass through for half of the frame interval.

[![](https://www.keheka.com/content/images/2023/02/ShutterAngle.jpg)](https://www.keheka.com/content/images/2023/02/ShutterAngle.jpg)

A conceptual visualisation of a 180 degree shutter angle.

> [!important]
> 
> A 90 degree shutter angle would be equivalent to an opening the  
> size of one quarter of the disc. It would only let in light for a  
> quarter of the frame interval, and block it for the remaining 3/4.

When the shutter blocks the light from the film or image sensor, a new frame is pulled in (physically in film cameras, electronically in digital cameras), ready to be exposed when  
the shutter opens again.

By adjusting the shutter angle, you can control the proportion of time that the film or image sensor is exposed to light during each frame interval.

So how does it relate to shutter speed in modern cameras?

### The Shutter Angle Equation

You can calculate one from the other, because **the shutter angle is proportional to the time that each frame is exposed**.

Here is the equation:

```Plain
Shutter Angle / 360 Degrees = Exposure Time (i.e. Shutter Speed) / Frame Interval
```

By shuffling the equation around, you can isolate the shutter angle or shutter speed to convert between them:

```Plain
Shutter Angle = Exposure Time (i.e. Shutter Speed) / Frame Interval × 360 Degrees
```

```Plain
Exposure Time (i.e. Shutter Speed) = Shutter Angle / 360 Degrees × Frame Interval
```

At 24 frames per second, the frame interval is 1/24th of a second. An exposure time (shutter speed) of 1/48th of a second gives you a shutter angle of 180 degrees. (And vice versa).

### Shutter Speed Options

Modern digital cameras, which commonly use the shutter speed setting instead of shutter angle, each have a set of shutter speed options to choose from.

They normally don’t have 1/48th of a second shutter speed available, specifically. However, they do have shutter speed options that are close enough for there to not be any noticeable difference.

A typical digital camera may for example have a shutter speed of 1/50th of a second as an option to choose from. When shooting at 24 (23.976) frames per second, that’s accurate enough. The  
shutter angle would in that case be ~173 degrees.

As long as the shutter angle ends up being near 180 degrees, the motion will look cinematic at 24 frames per second.

### Shutter Speed Affects Brightness

The faster your shutter speed, the less light you physically allow onto the image sensor (because the shutter is only open for a small window of time).

That means, your image gets darker with faster shutter speeds. Ultra-high frame rate cameras with a very fast  
shutter speed require an enormous amount of light to expose each frame,  
for example. (Typically direct sunlight).

You can help balance the exposure by opening up the aperture and increasing the ISO, but there is a tradeoff with narrower depth of field and increased image noise, respectively.

You can also increase the amount of light on set, with the tradeoff of a higher cost (high-powered lamps are expensive) and more heat, and/or having to change the location, i.e. going outside to get sunlight.

# Exposure Time And Motion Blur

Okay, back to motion blur.

Remember that shooting at 24 frames per second with a shutter angle of 180 degrees means we're exposing each frame for 1/48th of a second?

That is, for half of the frame interval the camera is capturing light from the scene onto the frame. Any movement or change in the scene within that 1/48th of a second will get captured,  
smearing across the exposure as motion blur.

An object moving from point A to point B during that short time will look smeared along the path between the two positions.

### Example In Nuke, Part 1: Adjusting The Shutter Angle

Let’s look in Nuke at how the shutter speed affects the length of the motion blur.

Below is a ball moving linearly across the frame at **exactly one ball length per frame**. There is no motion blur, and the video is playing at 24 frames per second.

Non-motion blurred ball.

Now let’s see the same again, only rendered  
with a 180 degree shutter angle. In the ScanlineRender node, that equals  
a value of 0.5 in the _shutter_ knob in the MultiSample tab. (0.5 being half a frame interval).

> [!important]
> 
> Since a 180 degree shutter angle is so common for movies, 0.5 is the default value in the ScanlineRender node).

For reference, the _shutteroffset_ knob is set to _centred_ and the _samples_ knob is set to _200_ in the ScanlineRender node – for all of the following tests.

Motion blur applied to the ball, using a 180 degree shutter angle.

Let’s increase the shutter angle to 360 degrees, i.e. a value of 1 in the _shutter_ knob. This means each frame is exposed for the entire duration of the frame.

Motion blur applied to the ball, using a 360 degree shutter angle.

Now let’s compare the same still frame of all three, in order from top to bottom:

[![](https://www.keheka.com/content/images/2023/02/MB_Comparison.jpg)](https://www.keheka.com/content/images/2023/02/MB_Comparison.jpg)

Motion blur comparison.

If you measure the pixels, applying a 180 degree shutter angle means the ball’s length has increased by half of its original, non-motion blurred length.

A 360 degree shutter angle means the ball is exactly twice its original, non-motion blurred length.

> [!important]
> 
> A 90 degree shutter angle would add a quarter to the ball’s length.

### Long Exposure

In still photography you can expose a shot for longer, and go beyond the equivalent to a 360 degree shutter angle, since you’re not restricted by a frame rate.

> [!important]
> 
> It doesn’t really make sense to use the concept of shutter angle  
> for still photography. We would instead be using shutter speed. The  
> following example is just to say we’re slowing down the shutter speed  
> even more, for a still photo.

Doubling the exposure time again from a 360 degree shutter angle, for example, would double the motion blur, making the ball's length a total of four times its original length. Which is why exposing a picture for several seconds or minutes leads to the very long streaks of motion blur.

[![](https://www.keheka.com/content/images/2023/02/Long-Exposure-Photography.jpg)](https://www.keheka.com/content/images/2023/02/Long-Exposure-Photography.jpg)

Example of long exposure photography.

If you double the shutter angle, you double the exposure time. That is, you double the length of time an object can move across the frame and smear the exposure. And when an object moves linearly across the frame like the ball above, you therefore double the length of its motion blur.

Keep halving the shutter angle, and eventually you get close to zero motion blur. Keep doubling it, and you keep doubling the motion blur.

### Example In Nuke, Part 2: Adjusting The Speed Of The Ball

By doubling the speed of the ball, the motion blur also doubles.

Below, the ball is going at twice the speed as before, and is rendered with a 180 degree shutter angle. It looks exactly like the ball going at normal speed rendered with a 360 degree  
shutter angle:

[![](https://www.keheka.com/content/images/2023/02/MB05_2X_speed.jpg)](https://www.keheka.com/content/images/2023/02/MB05_2X_speed.jpg)

Ball going at double the original speed, with a 180 degree shutter angle.

Next, let’s halve the shutter angle to 90 degrees (0.25 _shutter_ value in the ScanlineRender node), while keeping the same 2x speed of the ball:

[![](https://www.keheka.com/content/images/2023/02/MB025_2X_speed.jpg)](https://www.keheka.com/content/images/2023/02/MB025_2X_speed.jpg)

Ball going at double the original speed, with a 90 degree shutter angle.

It looks exactly the same as the ball going at normal speed with a 180 degree shutter angle.

So as you can see, the distance the ball covers across the frame and the shutter speed both have the same impact on the motion blur. Doubling the shutter angle, or doubling the speed of the ball across the screen, doubles the motion blur. Halving them,  
halves the motion blur.

# What About Other Frame Rates?

So far, we’ve only talked about footage shot at 24 frames per second.

That's because this particular frame rate is still so widely used in cinema today.

The 180 degree shutter angle rule only really applies to frame rates that are at, or near, 24 frames per second*. If you shoot at 25 or 30 (29.97) frames per second, you can use the 180 degree shutter rule and your motion will look cinematic when playing back at the same frame rate.

When further increasing the frame rate, it  
gets a bit more fuzzy. The 180 degree shutter angle rule becomes less  
and less relevant for creating a cinematic looking motion blur the  
higher you go – ***if you’ll be playing back the footage at 24 frames per second** (or a nearby frame rate) which is often the case.

If you are shooting at 48 frames per second,  
for example, using a 180 degree shutter angle means you will get half of  
the motion blur per frame that you would get at 24 frames per second.  
(A 180 degree shutter angle with a frame interval of 1/48th of a second  
means your exposure time, or shutter speed, is 1/96th of a second – half  
the time per frame than at 24 frames per second).

And having only half of the motion blur  
compared to the typical cinematic amount is a noticeable change to the  
audience. Playing back at 24 frames per second, it now looks unnaturally  
crisp.

> [!important]
> 
> Just as an example, you _could_ increase the shutter angle to 360 degrees (on digital cameras), i.e. 1/48th of a second shutter speed. Then, each of the 48 frames per second would get an equal amount of motion blur to what they would get with a 180 degree shutter angle at 24 frames per second (because the shutter speed would be the same). If you then removed every second frame, the frames would be identical to their 24 frames per second counterparts.

Other than a few exceptions, though, most movies are both shot _and_ played back at 24 frames per second. Any footage captured at a higher frame rate is usually an element meant for use in VFX, and/or a scan that requires retiming to fit a 24 frames per second timeline.

And depending on the shutter angle they’ve used (typically they stick to ~180 degrees), **you may need to add motion blur when retiming**.  
Otherwise it won’t match the 24 frames per second, 180 degree shutter  
angle look. (We’ll discuss methods for doing that later in the  
tutorial).

> [!important]
> 
> When filming, you are physically capped at a maximum of a 360  
> degree shutter angle (a limitation of having a frame rate), so for  
> anything higher than 48 frames per second, it wouldn't be possible to  
> expose the footage to match the typical cinematic motion blur—when  
> playing back at 24 frames per second.

If you are shooting AND will be playing back the footage at 48 frames per second, then the 180 degree shutter angle rule still applies. While the shutter speed is twice as fast, there are  
twice as many frames to smear the motion blur across.

So you get the same amount of motion blur across two frames as you would get per frame at 24 frames per second.

The same extends to any other high frame rate. Keep in mind, though, that the look of the 24 frames per second frame rate is also engrained in movie culture. So it wouldn't necessarily look cinematic at higher frame rates, even though the motion blur would look natural.

# CGI And Motion Blur

While a real camera has to let time pass to capture an image, a CG camera doesn’t.

A CG camera essentially has an infinitely fast shutter speed, capturing a discrete moment in time for every frame. And so CG motion blur has to be simulated.

In real life we can’t precisely predict the future, i.e. what will happen at the time of the following frame, but with a CG camera and scene we have the benefit of knowing exactly what  
happens next.

Where the camera will go, what's happening in the scene, how things interact, and when. We can already look ahead at the upcoming keyframes while we're on the current frame, and use that  
data to calculate the motion blur.

As we saw earlier, there are many variables when it comes to real motion blur. Calculating everything precisely requires a lot of processing power. Fortunately, several simplifications  
are made 'under the hood' to speed up the process, while maintaining an acceptably accurate motion blur.

Let's look at the three main methods of generating CG motion blur:

### Motion Blur Baked In

The most accurate method would be to render out the CGI from the 3D software with motion blur baked in.

It's also the heaviest of the three methods  
to calculate, because it makes fewer simplifications and considers  
things like sub-frame deformation and rotation.

You'll also get nicer edges where there is overlapping or conflicting motion in the frame.

Sometimes, though, you may not have capacity  
to render images with the motion blur baked in. In those cases, there  
are other methods you can use:

### Motion Vector Pass

A less compute-heavy method is to render the  
images without motion blur baked in from the 3D software, but rather  
with the motion vectors being calculated and written out to an AOV.  
(Arbitrary Output Variable, i.e. a render pass).

With these motion vectors, you as the compositor can apply the motion blur in Nuke instead, [using the VectorBlur node](https://learn.foundry.com/nuke/content/comp_environment/3d_compositing/adding_motion_blur_vectorblur.html).

This method does have its limitations - the  
motion blur will smear the pixels in a single direction, which doesn't  
look correct for rotating or deforming content. And smudging a single  
image looks less realistic than capturing proper sub-frame motion and  
deformation.

However, for most purposes it will give you acceptably accurate motion blur.

### Generated In Nuke

Alternatively, there are several options in Nuke for generating motion blur.

The simplest way is if you’re transforming  
something in Nuke. In the Transform node, you can activate the motion  
blur by changing the _motionblur_ knob from 0 to 1. You’ll find [the same knob in other nodes](https://learn.foundry.com/nuke/content/comp_environment/transforming_elements/adding_motion_blur.html) in the Transform menu, such as the CornerPin and Tracker nodes.

An important thing to keep in mind if you are using multiple transformation nodes, is that **the last node in the concatenated stack controls the motion blur**.

So, by stacking the transformation nodes one  
after another, you only need to adjust the motion blur setting in the  
final node in the chain. Here is [how, and which, nodes concatenate](https://learn.foundry.com/nuke/content/comp_environment/transforming_elements/node_concatenation.html).

If for some reason you have to break the concatenation, [apply your transformations through an STMap](https://benmcewan.com/blog/2020/02/02/demystifying-st-maps/)  
instead, to only filter the image once. The STMap node doesn’t have a  
motion blur knob, though. However, there are a couple of ways you can  
add accurate motion blur to it:

Here is a technique where you [generate your own motion vectors](https://benmcewan.com/blog/2020/07/06/quick-tip-add-accurate-motion-blur-to-your-warps/) with the help of an expression.

Another great (but heavy) way is to [use the TimeBlur node](https://vimeo.com/456737145).  
The TimeBlur node is extremely versatile, and can be used to create  
motion blur in all sorts of scenarios, not just for STMaps.

Connect it directly after Roto nodes or ScanlineRender nodes, for example, or use it in combination with the modulus expression in the TimeBlur tutorial above. This node takes advantage of sub-frame information, which is very useful when it comes to calculating motion blur.

> [!important]
> 
> Just remember to use the [NoTimeBlur](https://learn.foundry.com/nuke/content/reference_guide/time_nodes/notimeblur.html) node upstream to avoid calculating the whole comp multiple times, unless absolutely necessary.

Moving on, Nuke has a few MotionBlur nodes, namely [MotionBlur](https://learn.foundry.com/nuke/content/reference_guide/filter_nodes/motionblur.html), [MotionBlur2D](https://learn.foundry.com/nuke/content/reference_guide/filter_nodes/motionblur2d.html), and [MotionBlur3D](https://learn.foundry.com/nuke/content/reference_guide/filter_nodes/motionblur3d.html).

The **MotionBlur** node uses the same techniques and technology as the motion blur in the Kronos node, but with simpler controls. The _shutterTime_ knob works exactly as previously described, with a value of 0.5 representing a 180 degree shutter angle.

This node both generates the motion vectors, _and_ applies the motion blur, all in one node. You can also connect previously calculated vectors to it, for example using the [VectorGenerator](https://learn.foundry.com/nuke/content/comp_environment/kronos_motionblur/vectorgenerator.html) node or third-party software.

The **MotionBlur2D** node generates motion vectors only, using the _2D transf_  
input (for example, a Transform node). You then have to connect a  
VectorBlur node below it, using the vectors from the MotionBlur2D to  
produce motion blur. The _shutter_ knob again works in the same way as in other nodes, with a value of 0.5 representing a 180 degree shutter angle.

The **MotionBlur3D** node also only generates motion vectors, and requires you to connect a VectorBlur node using its vectors to actually [apply motion blur](https://learn.foundry.com/nuke/11.1/content/comp_environment/3d_compositing/adding_motion_blur_vectorblur.html#3d_compositing_3567570779_826774) to the image. The difference between it and the MotionBlur2D node, is that the MotionBlur3D node takes a camera as an input.

Note that the MotionBlur3D node has a couple  
of limitations. The scene should be static or nearly static, and the  
camera has to move in order for it to work. If the camera is static, the  
MotionBlur3D node isn’t able to produce any output.

> [!important]
> 
> You can also output motion vectors from the [ScanlineRender](https://learn.foundry.com/nuke/11.1/content/comp_environment/3d_compositing/adding_motion_blur_vectorblur.html#3d_compositing_3567570779_826548) and [RayRender](https://learn.foundry.com/nuke/11.1/content/comp_environment/3d_compositing/adding_motion_blur_vectorblur.html#To_use_VectorBlur_with_RayRender) nodes.

### Creative Or Corrective Motion Blur

Sometimes, the motion blur won’t look quite  
right and you have to go in and manually adjust it in specific areas.  
You may even want to make a creative effect using motion blur. A great  
method for custom work like this is to [paint the motion blur in Nuke](https://www.keheka.com/painting-motion-blur-in-nuke/).

### Motion Blur Shutter Offset

You have likely encountered the _shutteroffset_ knob in the various transformation nodes, or in the ScanlineRender node.

There are four options to choose from in the drop-down menu, and they work like this:

- **Centred**: Centres the shutter around the current frame. If the shutter is open for 1 frame (a _shutter_ value of 1) and you are on frame 10, the shutter will be open from frame 9.5 to 10.5.

- **Start**: Opens the shutter on the current frame. If the shutter is open for 1 frame (a _shutter_ value of 1) and you are on frame 10, the shutter will be open from frame 10 to 11.

- **End**: Closes the shutter on the current frame. If the shutter is open for 1 frame (a _shutter_ value of 1) and you are on frame 10, the shutter will be open from frame 9 to 10.

- **Custom**: You can choose when the shutter opens by typing a value (in frames) in the _shuttercustomoffset_ knob next to the drop-down menu. If the shutter is open for 1 frame (a _shutter_ value of 1) and you are on frame 10, a custom value of -0.25 means the shutter will be open from frame 9.75 to 10.75.

### Precomp Motion Blur

Motion blur is heavy to calculate.

To avoid slowing down your Nuke script, you can make use of [precomping](https://learn.foundry.com/nuke/content/reference_guide/other_nodes/precomp.html), or add the [$gui expression](https://learn.foundry.com/nuke/content/comp_environment/rendering/bypass_gui_renders.html) to the _disable_ knob of the motion blur node.

# Retiming And Motion Blur

Retiming scans can introduce problems with the motion blur.

When you retime a scan in Nuke, you're not  
changing the exposure time of each frame, i.e. the shutter speed. That  
was baked into each frame already when they were captured. You're simply  
compressing or expanding the captured frame rate to fit your playback  
frame rate, either by skipping frames and/or by generating new ones.

Making changes to the frame rate but not to  
the shutter speed creates an imbalance in the motion blur. As mentioned  
previously, reducing a 48 frames per second frame rate to 24 frames per  
second by removing every second frame, each remaining frame now has half  
the expected motion blur of a 180 degree shutter angle.

The good news is that we can add more motion  
blur to each frame to compensate for this loss. In the Kronos node, you  
can do that by adjusting the settings in the Shutter menu.

The _shutterTime_ knob in the Kronos node works in the same way as the _shutter_  
knob in the ScanlineRender. It's the number of frames the shutter stays  
open when motion blurring. It defaults to a value of 0.5, i.e. a 180  
degree shutter angle.

That's also the ratio of the shutter angle to a full 360 degrees. If you remember the original shutter angle equation above, that's exactly what these knobs represent:

```Plain
Shutter Time (setting in the Kronos node) = Shutter (setting in the ScanlineRender node) = Shutter Angle / 360 Degrees = Exposure Time (i.e. Shutter Speed) / Frame Interval
```

By increasing the Shutter Time setting, you  
increase the amount of motion blur proportionally. Going from a value of  
0.5 to 1, is the same as going from a 180 degree shutter angle to a 360  
degree shutter angle. I.e., doubling the motion blur.

### Fixing It In Post

It is much harder to remove motion blur than it is to add it.

When shooting at a higher frame rate which  
will later go on a 24 frames per second timeline, it’s generally  
recommended to use a 180 degree shutter angle or faster.

While it initially may look less smooth, the additional clarity in each frame will be of help in post-production. For example when keying or doing other compositing work. We can always add  
more motion blur in post.

It's the lesser of two evils, so to speak, as the other way around involves a lot of clean up work. (At least until AI/ML can properly de-motion blur images at production quality).

### How To Adjust The Motion Blur When Retiming In Nuke

If you double the speed of a scan by applying a 2x retime, you also have to double the motion blur. (At 2x speed, per frame, an object will move twice as far across the frame as it does at  
1x speed). If you triple the speed with a 3x retime, you have to triple the motion blur.

That’s pretty straight forward, as you can just increase the _shutterTime_ knob in the Kronos node proportionally until you get the correct amount of motion blur.

But what about when you’re doing a variable retime with a non-linear retime curve?

For that, you can use the following expression in the _shutterTime_ knob in the Kronos node:

```Plain
((timingFrame2 - timingFrame2(frame-1)) + (timingFrame2(frame+1) - timingFrame2)) / 2 * 0.48
```

Replace the value of _0.48_ in the expression with the original shutter time. In this example, the  
footage was shot using a shutter angle of 172.8 degrees, making the shutter time: 172.8 / 360 = 0.48.

You can also find the original shutter time by dividing the shutter speed by the frame interval, i.e. 1/50 / 1/24 = 0.48.

The expression above looks at the difference between the current (retimed) frame value and the previous (retimed) frame value, and the difference between the next (retimed) frame value  
and the current (retimed) frame value. Then, it takes the average between the two, and finally multiplies that number by the original shutter time.

What this does is, it finds out how big of a  
jump the retime is making from one frame to the next, and increases or  
decreases the shutter time to account for the jump.

Let’s say there is no retime, and the footage plays like this: frame 1001, frame 1002, frame 1003, etc. We’re currently at frame 1002. Plugging that into the expression:

```Plain
((1002 - 1001) + (1003 - 1002)) / 2 * 0.48 = 0.48
```

No change, as expected.

Next, let’s double the speed of the footage.  
It now plays like this: frame 1001, frame 1003, frame 1005, etc. Every  
other frame. We’re currently at frame 1003. Plugging that into the  
expression:

```Plain
((1003 - 1001) + (1005 - 1003)) / 2 * 0.48 = 0.96
```

A doubling of the shutter time, as expected.

The expression works for any retime curve, whether linear or non-linear. Try adding it into your own Kronos node.

### Correcting Problem Frames

Sometimes, the jump between frames can be  
quite significant in the retime curve. To avoid the motion blur  
exploding on those frames with the above expression, you can animate the  
original shutter time value on the problem frames:

Add a floating point slider to the Kronos  
node by right-clicking on its properties panel and going to Manage User  
Knobs... → Add → Floating Point Slider…

Name the slider for example _shutterTimeCustom_.

Then, update the expression to:

```Plain
((timingFrame2 - timingFrame2(frame-1)) + (timingFrame2(frame+1) - timingFrame2)) / 2 * shutterTimeCustom
```

Now, you can animate the custom shutter time value in the problem areas, and bring it back to the original value when that works as expected again.

> [!important]
> 
> You can bake the expression into keyframes afterwards if you like, by right clicking on the _shutterTime_ knob and going to Edit → Generate…, then choosing your frame range, and hitting OK.