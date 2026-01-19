---
{"dg-publish":true,"permalink":"/nuke-kb/40-courses/keheka-courses/how-to-create-a-sense-of-depth-in-your-composite/"}
---

In this deep dive tutorial we will examine  
all of the different visual depth cues in detail, and walk through how  
to create a realistic sense of scale, space, and distance to objects in  
your 2D images in Nuke.

# Our Depth Perception

We humans can perceive depth, or the distance to objects, in many different ways.

Sound is an important cue; you can hear if a  
bus is driving by close to you or far away, for example. You can also  
feel the difference in the vibrations in the road you're standing on, or  
the displaced air being pushed against you as the bus drives by next to  
you. Temperature can be another cue; you can feel how close you are to a  
bonfire, for example.

However, _visual_  
cues contribute the most to our depth perception by far, and they are  
the ones you'll be working with on a daily basis as a compositor.

### Interpreting The Third Dimension

When we look at something around us in the  
real world, the three-dimensional scene is projected through our eyes  
and onto our retinas as two-dimensional images – one image for each eye.

**It’s up to our visual system to ‘recover’ the missing third dimension: depth.**

It does that by interpreting a number of visual depth cues, including:

- The observed physical phenomena related to depth in the scene.

- The differences between the images obtained by each eye.

- The so called _oculomotor_ cues – our ability to sense the position of our eyes and the tension in the eye muscles.

Together, all these signals of depth information let our brain estimate distances to the objects we see.

We can split the visual depth cues into two main categories: **binocular** depth cues (only possible to perceive using both eyes) and **monocular** depth cues (possible to perceive with a single eye, and/or in 2D images).

> [!important]
> 
> 💡 Binocular: from Latin _bini_ ‘two together’ + _oculus_ ‘eye’.

Monocular: from Greek _monos_ ‘alone, single’ + from Latin _oculus_ ‘eye’.

# 1. Binocular Depth Cues

There are three binocular depth cues: **binocular parallax**, **vergence**, and **shadow stereopsis**.

### Binocular Parallax

One of the most important ways of visually sensing depth for humans is through what's called binocular parallax.

We have two eyes positioned a small distance  
apart, and because of that they each see the same scene at a slightly  
different angle. Our brain is very sensitive to the differences in the  
images obtained by each eye. Based on the different angles in each  
image, we can triangulate the distances to objects and perceive depth.

[![](https://www.keheka.com/content/images/2023/04/Binocular_Depth_Cues.png)](https://www.keheka.com/content/images/2023/04/Binocular_Depth_Cues.png)

Each eye sees the scene at a slightly different angle.

> [!important]
> 
> 💡 The relative position of an object that is close to us will change  
> much more between each eye than an object that is far away from us.

For medium viewing distances, you could  
remove all other depth cues, and binocular parallax alone would still  
let you perceive depth.

### Vergence

Vergence is how our eyes move simultaneously in relation to each other when we are looking at an object.

When we look at something close to us, our eyes rotate slightly towards each other. This is called **convergence**. As an extreme example, if you look at your own nose, your eyes converge so much that you go cross eyed.

If you then look at something further out, your eyes rotate away from each other again. This is called **divergence**.  
As you look further and further into the distance, your eyes diverge  
until they become pretty much parallel to each other – fixated on the  
same point at a great distance away.

[![](https://www.keheka.com/content/images/2023/04/Convergence.png)](https://www.keheka.com/content/images/2023/04/Convergence.png)

Convergence is when our eyes are rotating  
toward each other; and divergence is when our eyes are rotating away  
from each other – toward becoming parallel with one another.

Because vergence is so closely related to  
distance, the way our eyes converge or diverge inherently provides us  
with useful depth information. Just by sensing the tension in the  
muscles in our eyes, our brain can estimate the distance to an object.

### Shadow Stereopsis

Shadow stereopsis is more of a special case of binocular depth perception.

If the images in each eye have no difference  
in parallax, it’s still possible to perceive depth if there is a  
difference in the shadows cast from the objects in the scene.

[![](https://www.keheka.com/content/images/2023/04/Shadow_Stereopsis_A.jpg)](https://www.keheka.com/content/images/2023/04/Shadow_Stereopsis_A.jpg)

[![](https://www.keheka.com/content/images/2023/04/Shadow_Stereopsis_B.jpg)](https://www.keheka.com/content/images/2023/04/Shadow_Stereopsis_B.jpg)

The ball is in the exact same position in the frame in the two images, but the shadow angle differs slightly.

Our brain can fuse the images stereoscopically, and gain information about the depth in the scene, just from the shadows.

### 3D Only

The binocular depth cues, especially  
binocular parallax and vergence, are very important for acquiring depth  
information when we view our three-dimensional world.

However, unless we're compositing stereo images, we don't have the benefit of using binocular depth cues in our composites.

In our day-to-day 2D compositing work we have  
to rely on other visual depth cues to create a sense of scale and  
distance to the objects in our images.

That’s where the second category comes into play: monocular depth cues.

# 2. Monocular Depth Cues

To estimate the distances to objects in the scene, our visual system interprets a wide variety of monocular depth cues.

As humans have evolved, we have learned by  
observation how the physical world behaves, and which visual phenomena  
are related to depth and distance.

Most of the time, our depth perception is  
relative, not absolute. We can quite often easily tell if one object is  
closer to us than another, but we can’t necessarily tell exactly how far  
away from us the objects are.

That’s not a problem, because to be able to  
get an understanding of our surroundings, we first and foremost need to  
know how things relate to each other. Early man didn’t need to know if a  
lion was exactly 30 metres or 50 metres away – only that it was close  
enough to be a danger.

When looking at 2D images, audiences rely  
heavily on the collective human understanding of how objects relate to  
each other according to distance in the physical world. If we as  
compositors mess up the visual depth cues in our composites, it will jar  
the audience (even though they may not consciously know why).

Let’s break down each monocular depth cue to gain a better understanding of how to layer things in depth in our composites.

### Linear Perspective

Linear perspective is one of the most widely recognisable monocular depth cues.

Parallel lines, such as roads or railway  
tracks, appear to get narrower the further away they are from us. From  
our viewpoint, the lines converge on a point at the horizon, called a _vanishing point_.

> [!important]
> 
> 💡 The closer the parallel lines are to each other, the further away  
> they are from the camera. Also, the closer an object is to a vanishing  
> point, the further away it is (or a part of it is) from the camera, and  
> the more foreshortened it becomes.

Depending on the number of vanishing points there are in the image, we can have three different perspectives:

### [1. One-Point Perspective](https://thevirtualinstructor.com/onepointperspective.html)

The perspective lines representing depth in  
the image all converge on a single vanishing point on the horizon line.  
The lines that represent height are parallel to each other in the frame,  
and they are perpendicular to the horizon line. The lines that  
represent width are parallel to each other in the frame, and are also  
parallel to the horizon line.

An interesting point made in the article  
linked above, is that the horizon line doesn't actually have to be at  
the horizon. It could for example also be positioned along a street  
between buildings when looking down from a top view.

> [!important]
> 
> 💡 The horizon line doesn't even have to be in the frame. The  
> perspective lines can meet at a vanishing point along the horizon line  
> outside of the frame. In fact, this is usually the case with two-point  
> and three-point perspectives.

In the example image below, the width lines  
are parallel with each other in the frame, and the height lines are  
parallel with each other in the frame. However, the depth lines all  
converge on one vanishing point. In this case, the vanishing point is in  
the central lower third of the frame, at the base of the background  
building.

[![](https://www.keheka.com/content/images/2023/04/One_Point_Perspective.png)](https://www.keheka.com/content/images/2023/04/One_Point_Perspective.png)

One-point perspective.

[![](https://www.keheka.com/content/images/2023/04/One_Point_Perspective_Lines.jpg)](https://www.keheka.com/content/images/2023/04/One_Point_Perspective_Lines.jpg)

One-point perspective lines.

The depth perspective lines (visualised in  
green) converge on one vanishing point on the horizon line (visualised  
in yellow). The width perspective lines (visualised in red) are parallel  
to each other in the frame, and the height perspective lines  
(visualised in blue) are parallel to each other in the frame.

### [2. Two-Point Perspective](https://thevirtualinstructor.com/twopointperspective.html)

The perspective lines representing width in  
the image converge on one vanishing point on the horizon line, and the  
perspective lines representing depth in the image converge on another  
vanishing point, elsewhere on the horizon line. The lines that represent  
height are parallel to each other in the frame, and they are  
perpendicular to the horizon line.

The vanishing points don't have to be visible  
in the frame, they can be outside of it, as long as they're found on  
the continued horizon line in either direction.

In the example image below, the height lines  
are parallel with each other in the frame, while the width and depth  
lines converge on their respective vanishing points on the horizon line.  
In this case, the vanishing points are to the left and right outside of  
the frame.

[![](https://www.keheka.com/content/images/2023/04/Two_Point_Perspective.jpg)](https://www.keheka.com/content/images/2023/04/Two_Point_Perspective.jpg)

Two-point perspective.

[![](https://www.keheka.com/content/images/2023/04/Two_Point_Perspective_Lines.jpg)](https://www.keheka.com/content/images/2023/04/Two_Point_Perspective_Lines.jpg)

Two-point perspective lines.

The width perspective lines (visualised in  
red) and the depth perspective lines (visualised in green) each converge  
on their respective vanishing point on the horizon line (visualised in  
yellow). The height perspective lines (visualised in blue) are parallel  
to each other in the frame.

### [3. Three-Point Perspective](https://thevirtualinstructor.com/threepointperspective.html)

The perspective lines representing width in  
the image converge on one vanishing point on the horizon line, and the  
perspective lines representing depth in the image converge on another  
vanishing point, elsewhere on the horizon line. The perspective lines  
representing height converge on a third vanishing point, away from the  
horizon line (either above or below it).

> [!important]
> 
> 💡 If the third vanishing point is below the horizon line, it will often be located outside of the frame.

The effects of the three-point perspective  
become more noticeable with more extreme vantage points, such as looking  
up at, or down from/to, an object.

In the example image below, the perspective lines representing width, height, and depth _all_  
converge on their respective vanishing point. In this case, on the two  
vanishing points along the horizon line outside of the frame (below it  
and to the left and right), and one vanishing point just outside the top  
of the frame.

[![](https://www.keheka.com/content/images/2023/04/Three_Point_Perspective.jpg)](https://www.keheka.com/content/images/2023/04/Three_Point_Perspective.jpg)

Three-point perspective.

[![](https://www.keheka.com/content/images/2023/04/Three_Point_Perspective_Lines.jpg)](https://www.keheka.com/content/images/2023/04/Three_Point_Perspective_Lines.jpg)

Three-point perspective lines.

The width perspective lines (visualised in  
red), the depth perspective lines (visualised in green), and the height  
perspective lines (visualised in blue) all converge on vanishing points.  
Only the width and depth vanishing points are located on the horizon  
line (visualised in yellow).

> [!important]
> 
> 💡 In the image above, the vanishing points reveal that the camera was  
> rolled (tilted sideways) about 2.5 degrees to the right when the photo  
> was taken. If we rotate the image 2.5 degrees clockwise, the horizon  
> line becomes level.

[![](https://www.keheka.com/content/images/2023/04/Three_Point_Perspective_Lines_Straight.jpg)](https://www.keheka.com/content/images/2023/04/Three_Point_Perspective_Lines_Straight.jpg)

Correcting the roll of the camera, straightening the horizon.

**A rule of thumb** for quickly determining the perspective in your image:

- If you are looking at an object head on, it may be a one-point perspective.

- If you are looking directly at the corner of the object, it may be a two-point perspective.

- If you are looking at the corner of an object, _and_ either upward or downward, it's likely to be a three-point perspective.

Objects in nature often don’t have straight lines. Yet, linear perspective still applies. [Winding roads or rivers](https://ranartblog.com/blogarticle19.html#roads-in-perspective) (scroll down a little bit on the linked page), for example, will have multiple vanishing points, depending on their path.

Also keep in mind, when there are several  
objects at different angles in the image, they each may have different  
vanishing points. Even in the one-point perspective example image above,  
the buildings are not all aligned exactly with the road and don't  
conform perfectly to a one-point perspective (but closely enough).

### → Linear Perspective In Nuke

Linear perspective follows pretty strict  
physical/mathematical laws, and so unless you’re deliberately making  
some sort of creative effect to bend reality, make sure that anything  
that you composite adheres to the perspective lines in your image.

When you render 3D geometry in Nuke through a  
camera, the perspective is automatically taken care of for you. The  
only things you have to worry about are positioning your objects  
correctly in 3D space (at realistic distances from the camera), and  
applying the correct lens distortion.

> [!important]
> 
> 💡 Speaking of lens distortion, straight lines will curve at the edges  
> of the frame, and so perspective lines aren't perfectly straight in  
> pictures. If there is heavy lens distortion, it may help to temporarily  
> undistort the image to get a better sense of where your perspective  
> lines are converging.

In 2D (and 2.5D) there is more room for  
error. When transforming your elements into place using the various  
transformation tools such as the Transform and CornerPin nodes, matching  
the perspective lines by eye is not always easy.

For that, it’s better to make use of a  
perspective line tool to visually guide you. There are various tools on  
Nukepedia, such as [dgPerspLines](http://www.nukepedia.com/gizmos/other/dg_persplines). Or, if you’re interested, you can [make your own perspective line tool in Nuke](https://www.youtube.com/watch?v=wVTut2qIXrw).

### Aerial Perspective

Aerial perspective (also known as atmospheric  
perspective) is a phenomenon where objects far away tend to fade and  
blend in with the atmospheric haze, the horizon, and the sky.

In the example image below, the distant  
mountains appear lighter, less contrasty, and less saturated than the  
foreground ones. And the further away they are, the more their colours  
shift towards the blue sky colour. The effect gets so strong, the hills  
start to look like a group of cardboard cut-outs.

[![](https://www.keheka.com/content/images/2023/04/Aerial_Perspective.jpg)](https://www.keheka.com/content/images/2023/04/Aerial_Perspective.jpg)

Depth cues provided by the aerial perspective.

The greater the distance is between the  
camera and an object, the more atmosphere there is between the two.  
Mist, fog, dust, smoke, water vapour, pollution, and even air itself  
scatter light in the atmosphere, making distant objects appear hazy.

The observed effects, which we can replicate in our composites, are:

- **Brightness increases with distance**. Distant objects appear lighter than those close to us. This effect is  
    mostly prominent in the low to mid range of the brightness values. (The  
    black levels and midtones are lifted).

- **Contrast decreases with distance**. Distant objects' tones flatten out and blend with the atmosphere. Visible details and patterns fade.

- **Saturation decreases with distance**. Distant objects' colours blend with and take on the colour and  
    saturation of the atmosphere (typically grey or light blue, but  
    sometimes also warmer tones, for example during sunsets).

Other characteristics to note:

- **Atmospheric haze levels impact the aerial perspective**. The more atmospheric haze there is, e.g. heavy pollution or fog, the  
    shorter the distance from the camera before an object becomes visibly  
    affected by aerial perspective.

- **Aerial perspective appears in media other than air**. We see a similar effect underwater, or on planets with a different atmosphere than Earth, such as Mars.

- **Aerial perspective is visible at night**. It doesn’t have to be daytime with a bright sky in order for the effect to appear. As long as there is atmosphere present to scatter the light, aerial perspective can be visible.

- **Aerial perspective doesn’t necessarily blur the background**. If the camera is focused at infinity, the edges of distant objects  
    usually remain fairly sharp. Effects such as heat haze may soften  
    objects far away, though.

- **Weather such as storms and rain impact the aerial perspective**. The visibility may be reduced in localised areas and the aerial  
    perspective may be less uniform and more random than in calm weather.

### → Aerial Perspective In Nuke

It's pretty easy to mimic this effect in  
Nuke. You can grade your CG/DMP/element/etc. based on distance by using  
either a depth pass or mattes separating out the objects based on their  
depth in the scene.

If your CG render has a depth channel,  
connect a Shuffle node in a new branch and shuffle the depth channel  
into the alpha channel.

Depending on how the depth was rendered, you  
may have values outside of the 0-1 range, and/or a ramp going the  
opposite way of what you need for your grading matte.

> [!important]
> 
> 💡 The alpha channel should only have values between 0 and 1 in it.  
> The various merge and mask operations in Nuke expect alpha values in  
> that range, and the maths may break otherwise.

To normalise the values between 0 (at the camera) and 1 (at the maximum distance from the camera):

1. Add a Grade node after the Shuffle node which you shuffled out your depth pass in.

1. Set the Grade node to affect the alpha channel.

1. View the Shuffle node and colour pick the Grade node's _blackpoint_ somewhere close to the camera in your depth pass.

1. Colour pick the Grade node's _whitepoint_ somewhere near the maximum distance from the camera in your depth pass.

1. Tick the _black clamp_ and _white clamp_ checkboxes in the Grade node to ensure your matte only has values between 0 and 1.

[![](https://www.keheka.com/content/images/2023/04/Depth.jpg)](https://www.keheka.com/content/images/2023/04/Depth.jpg)

[![](https://www.keheka.com/content/images/2023/04/Depth_Normalised.jpg)](https://www.keheka.com/content/images/2023/04/Depth_Normalised.jpg)

Depth pass (left) and normalised depth pass (right).

Once you have a normalised matte, add a Grade  
node to your CG (remember to un/premult) and connect the normalised  
matte to it as a mask. Increase the lift value in the Grade node and  
tint it toward the background colour to simulate aerial perspective. You  
can also connect a ColorCorrect node using the same mask and play with  
the saturation and contrast values.

> [!important]
> 
> 💡 Adjust the gamma of the grading matte to change the depth ramp and falloff.

### Accommodation

Accommodation describes how the muscles and  
fibres in our eyes contract and relax to adjust the shape of the lenses,  
in order to keep an object in focus as its distance varies from us.

When we look at something close to us, the  
muscles in our eyes relax to let the lenses bulge and focus the image  
onto our retinas.

If we look at something far away, the muscles instead contract to stretch the lenses thin in order to focus the image.

[![](https://www.keheka.com/content/images/2023/04/Accommodation.jpg)](https://www.keheka.com/content/images/2023/04/Accommodation.jpg)

Accommodation of the eye.

Although you can consciously control this muscle movement, it’s usually a reflex which is closely linked to vergence.

And because accommodation is so closely  
related to distance, how our eyes accommodate inherently provides us  
with useful depth information. Again, just by sensing the tension in the  
muscles in our eyes, our brain can estimate the distance to an object.

### → Accommodation In Nuke

The focal length of a camera lens is a  
measure of how strongly the lens converges or diverges light (i.e. bends  
the light inward or outward). A wide lens (short focal length)  
converges the light much more than a long lens (long focal length).

When the muscles in our eyes relax, the  
lenses bulge and converge the light more, similar to a shorter focal  
length in a camera lens. When the muscles contract and pull the lenses  
thin, they converge the light less, similar to a longer focal length in a  
camera lens.

**And so accommodation is related to focal length**.

When working with images in Nuke, pay  
attention to which focal lengths the scans and elements that you’re  
using have been filmed with. If you use an element shot on a telephoto  
lens – which heavily compresses space and distances – it may not look  
natural composited into a shot filmed with a wide angle lens, and vice  
versa.

In the image comparison example below, you  
can see how space is flattened and compressed when using a telephoto  
lens versus how space is opened up on a wide angle lens. Compositing  
these next to each other in the same scene wouldn’t look right due to  
the dramatic difference in perspective.

[![](https://www.keheka.com/content/images/2023/04/Wide_Angle_Vs_Telephoto.jpg)](https://www.keheka.com/content/images/2023/04/Wide_Angle_Vs_Telephoto.jpg)

The same person shot using a wide angle lens versus a telephoto lens.

### Defocus

When we adjust the lens on our camera to  
focus on an object, anything that is closer to and further away from the  
focal plane starts falling out of focus.

Objects on the same focal plane are at the  
same distance from the camera, and will have the same sharpness.  
Variation in the defocus across the image is therefore a distance cue.

In the example image below, which has a  
shallow depth of field, the toy wolf and the screen right foreground  
leaf on the ground are both in focus. That means they are both at the  
same distance from the camera.

The further in front or behind them in the  
picture you look, the more defocused the objects get, which helps give  
you an indication of relative distances.

[![](https://www.keheka.com/content/images/2023/04/Focus.jpg)](https://www.keheka.com/content/images/2023/04/Focus.jpg)

Depth cues provided by the focus.

### → Defocus In Nuke

To simulate the depth of field of your camera  
lens, you can use the ZDefocus node (or if you have Nuke 14 or higher,  
the often superior [Bokeh node](https://m.youtube.com/watch?v=E1iXIJJnSwY)) in conjunction with your CG depth pass.

Here is a tutorial about how the ZDefocus node works:

> [!info] zDefocus node - Nuke basics ep05 - Quick tutorial  
> Let's take a look at the ZDefocus node.  
> [https://youtu.be/YOtNgueQHX8](https://youtu.be/YOtNgueQHX8)  

To match the focus in your scan:

1. Connect the ZDefocus node to your CG/element/etc.

1. Select which channel to use as the depth channel in the ZDefocus node. You can also create your own depth  
    channel, for example by shuffling in a ramp to the depth.z channel  
    upstream.

1. Set the maths option in the ZDefocus  
    according to which render engine was used to create the CG render. Look  
    up your specific one, but common settings are _far=0_ for the ScanlineRender, RayRender, and RenderMan, and _depth_ for Arnold, Redshift, and VRay.

1. Set the focal plane to the same  
    distance it is in the scan by moving the focal point selector in the  
    Viewer, and animating it if needed.

1. Adjust your bokeh shape to match the one in the scan, or extract a bokeh kernel from the scan and use it as the  
    filter image. If you're matching the kernel manually, pay attention to  
    things like the number of diaphragm blades, the edge curvature, aspect  
    ratio, sharpness, aberrations, and the brightness along the radius of  
    the bokeh. (The inner part of the bokeh may be dimmer or brighter,  
    creating an outer edge, like a soap bubble effect).

1. Look at the scan and your CG/element,  
    and compare the sizes of the bokeh. Match the strength of the defocus to your scan by adjusting the size values.

> [!important]
> 
> 💡 Toward [the end of this article](https://www.keheka.com/creating-custom-lens-effects-by-pre-treating-your-images-in-nuke/), I show a technique for controlling the brightness of the bokeh.

A tip to match a focus pull in the scan:

1. Framehold a sharp frame just before the focus pull starts.

1. Track the frameheld frame to the scan so it follows the original movement and lines up with ‘itself’.

1. Connect your defocusing node to the frame held frame.

1. Animate the defocus on the Frameheld  
    frame to match what is happening in the area of the original scan that  
    you want to composite something into.

1. Disconnect the defocusing node from the  
    frameheld frame and connect it to your CG, patch, element, or anything  
    else that you’re compositing into the scan.

It should now be a perfect match.

> [!important]
> 
> 💡 Make sure you have enough slices in the ZDefocus node to create a smooth rack focus. Increase the value in the _depth layers_ knob as needed.

If you want to learn to simulate physically  
accurate depth of field in Nuke, here is a great in-depth tutorial and  
tool by Jed Smith:

> [!info] Simulating Physically Accurate Depth of Field in Nuke  
> A discussion of lenses and optics, and how they affect depth of field behavior in an image.  
> [https://youtu.be/Rv7L6M8f2lk](https://youtu.be/Rv7L6M8f2lk)  

### Overlapping

The way objects are layered and occlude each other in the scene provides strong, relative depth cues.

If one object is in front of another object  
from your perspective, then the first object must be closer to you than  
the second object.

In the image below, we can tell by the  
overlapping that the three smaller pyramids are closer to us than the  
larger ones. We can even tell the specific order of how the larger  
pyramids are positioned in depth.

[![](https://www.keheka.com/content/images/2023/04/Overlapping.jpg)](https://www.keheka.com/content/images/2023/04/Overlapping.jpg)

Depth cues provided by the overlapping of objects.

**Overlapping gives us a ‘ranking’ of the objects in depth**.

### → Overlapping In Nuke

When you composite in Nuke, it helps to  
organise the various parts of your script according to distance from the  
camera. Place the background elements at the top, and then the  
midground and foreground elements further and further down the stream,  
merging into the B-pipe. As you merge them together A over B, they  
naturally get layered correctly.

Next, keep track of your holdout mattes. Make  
sure that the objects that are supposed to be in the background are  
being held out by the foreground objects correctly. And vice versa,  
ensure that the foreground objects aren’t being held out by the  
background objects.

Compositing in Deep mostly takes care of all of this for you, at least for the CG elements, but if you use the [Deep Compositing 2D Workflow](https://www.keheka.com/deep-compositing-using-a-2d-workflow-in-nuke/), make sure that all the Deep renders are held out by each other.

### Object Scale

The size of the objects in the frame is usually a good indication of their distance from the camera.

We know for example roughly how big a person  
is, or how big a car is, and so if we see a person or car that are tiny  
in the frame, they're likely to be far away from the camera.

We can split object scale into four categories:

### 1. Relative Size

Relative size is based on linear perspective.  
If we know that two objects are similar in size, for example two trees,  
but their absolute size is unknown, their relative sizes in the frame  
gives us an indication of how far away they are from us. If one tree is  
larger in the frame than the other, it is likely to be closer to us.

[![](https://www.keheka.com/content/images/2023/04/Relative_Size.jpg)](https://www.keheka.com/content/images/2023/04/Relative_Size.jpg)

The relative sizes of the trees in the frame give us an indication of their distances from us.

### 2. Familiar Size

You may already know the rough size of an  
object in the image from beforehand. With that knowledge, depending on  
how large the object is in the frame, you can tell how far away it must  
be.

Take a look at the example image below. We  
already know roughly how big a bus typically is, and because it is quite  
small in the frame we can tell that there must be a fair distance  
between the camera and the bus.

[![](https://www.keheka.com/content/images/2023/04/Familiar_Size.jpg)](https://www.keheka.com/content/images/2023/04/Familiar_Size.jpg)

The familiar size of the bus versus its size in the frame gives us an indication of its distance from us.

### 3. Absolute Size

Even if we don’t know the actual size of an  
object, and there aren’t other objects to compare it to, we still assume  
that an object which is smaller in the frame is farther away than a  
larger object at the same location.

[![](https://www.keheka.com/content/images/2023/04/Absolute_Size_A.jpg)](https://www.keheka.com/content/images/2023/04/Absolute_Size_A.jpg)

[![](https://www.keheka.com/content/images/2023/04/Absolute_Size_B.jpg)](https://www.keheka.com/content/images/2023/04/Absolute_Size_B.jpg)

The absolute size of an object affects our perception of its distance from the camera.

### 4. Optical Expansion/Contraction

When an object is moving towards the camera –  
even though the object itself doesn’t change in size – over time it  
expands in size in the frame. And conversely, when an object is moving  
away from the camera, over time it diminishes in size in the frame.

The dynamic stimulus from this optical  
expansion and contraction enables us not only to see the object as  
moving, but to perceive the distance to the moving object.

> [!info] Optical Expansion  
>  
> [https://youtu.be/jsMyKBxyIVw](https://youtu.be/jsMyKBxyIVw)  

### → Object Scale In Nuke

When compositing, make sure to position your  
card or geometry at the correct distance from the camera in 3D space, or  
Transform (scale) your element to a realistic size based on its  
distance to the camera.

Look at the other objects in the frame at the  
distance you want to position your object, and use their sizes to help  
you judge which scale your object should be.

You can use perspective lines to guide you when positioning, rotating, and scaling your objects:

[![](https://www.keheka.com/content/images/2023/04/DuplicationA.jpg)](https://www.keheka.com/content/images/2023/04/DuplicationA.jpg)

[![](https://www.keheka.com/content/images/2023/04/DuplicationB.jpg)](https://www.keheka.com/content/images/2023/04/DuplicationB.jpg)

[![](https://www.keheka.com/content/images/2023/04/DuplicationC.jpg)](https://www.keheka.com/content/images/2023/04/DuplicationC.jpg)

Finding the centre line between two lamp posts in perspective.

Let’s say we have some lampposts of equal  
height going into the distance in our image (left). And we want to place  
something halfway in between the two nearest posts.

Your first instinct might be to place the  
object halfway between the posts horizontally in the frame. But that’s  
not quite how perspective works.

If you want to be accurate, first trace the  
perspective lines to the vanishing point (visualised by the green lines  
and the green circle in the middle image).

Then, trace the diagonals between the tops  
and bottoms of the posts (visualised by the purple lines in the image on  
the right). Where the diagonals cross is where the centre point is  
between the two posts.

Lastly, trace a vertical line through the  
intersection of the diagonal lines, parallel with the height lines in  
the image (the posts), to find the actual centre line and the centre  
ground point between the two posts (visualised by the dotted cyan line  
and the cyan circle in the image on the right).

As you can see in the image on the right, in  
perspective, the halfway point isn’t horizontally halfway between the  
two posts in the frame: it’s actually closer to the second post.

> [!important]
> 
> 💡 Now that you have a new vertical centre line (the dotted cyan line)  
> you can keep making new centre lines by halving the distance using the  
> diagonal intersection technique. For example, if you want something to  
> be positioned one quarter of the distance between the two posts.

### Texture Gradient

Texture gradient is closely related to linear perspective.

As objects get further and further away, we  
see a gradual change from course, distinct texture to fine, indistinct  
texture. Groups of objects appear denser and blend in together as the  
distance increases.

[![](https://www.keheka.com/content/images/2023/04/Texture_Gradient.jpg)](https://www.keheka.com/content/images/2023/04/Texture_Gradient.jpg)

Example of texture gradient.

In the picture above we can pick out  
individual cranberries in the foreground, but as they recede away from  
the camera into the distance they become less and less distinguishable  
from each other.

The same happens with cobblestone streets, or  
any pattern over a significant distance. Regardless of how far away an  
object is from us, its texture covers roughly the same amount of  
surface. And so texture ‘units’ changing in size indicates depth.  
Texture can therefore help us determine the actual size of an object and  
its distance from us.

### → Texture Gradient In Nuke

To mimic this effect in Nuke, simply avoid  
having overly sharp and clear details in distant objects. And, vice  
versa: avoid not having enough detail in your foreground objects.

You can also combine the texture gradient  
effect with the aerial perspective effect to further lift and blend  
distant objects together.

### Motion Parallax

When either we move around (i.e. the camera  
moves), or objects in the scene move around, the relative motion of the  
objects in the scene against the background provides us with information  
about the relative distances of the objects.

Let’s look at the example video below, showing motion parallax while driving:

> [!info] Motion Parallax Example  
> An example of Motion Parallax  
> [https://youtu.be/ce_Ugo77MuM](https://youtu.be/ce_Ugo77MuM)  

With foreshortening, things that are near to us pass by quickly, while objects in the distance appear more stationary.

> [!important]
> 
> 💡 Foreshortening is when an object's dimensions along the line of  
> sight appear shorter than its dimensions across the line of sight.

### → Motion Parallax In Nuke

To achieve correct motion parallax in Nuke, there are a few things to keep in mind:

- **Make sure that your camera is accurately tracked to your scan**. To correctly match the motion parallax at various depths in the scan, the CG camera has to match up with the real camera.

- **Ensure your scene scale is correct**. Different 3D software use different units of measurement. Maya uses  
    centimetres by default, while Houdini uses metres, for example. Others  
    may use a different unit, like decimetres, or imperial units.
    
    Nuke  
    just equates any value as being equal to one unit in 3D space. So, if  
    you import cameras, geometry, and/or renders from different software,  
    you may have to scale them up or down so they align with each other. If  
    you have a camera from Maya (cm) and a CG render from Houdini (m), you  
    can scale up the utility passes (such as the position pass) in the  
    Houdini render by 100 to get the correct scale in relation to the Maya  
    camera in Nuke.
    
    To scale your geometry or camera, you can plug an Axis node into a TransformGeo node, or directly to the Camera, and adjust the _uniform scale_  
    value in the Axis. To scale your CG render’s utility passes, you can  
    connect a Multiply (math) node and multiply up or down to scale them  
    accordingly. To scale a Deep render, connect a DeepTransform node and  
    adjust the _zscale_ value. Note that the _zscale_ value works inversely to a Multiply. So if you want to scale up the Deep by 100, set the _zscale_ to 0.01.
    

- **Check that your cards or geometry are positioned at the correct depth in relation to your camera in 3D space**. If you project a building that’s supposed to be 20 metres away on a  
    card that is 2 metres away, the motion parallax won’t be correct.

- **Confirm that you’re 2D tracking the correct plane of depth in the scan**. You normally have to track the area where you want to insert an  
    element. That means you may have to 2D track your scan several times if  
    you want to insert elements at different depths. For example one track  
    for the foreground, one for the midground, and one for the background. A background element tracked in using the foreground 2D track is unlikely to move correctly for its position in depth.

- **A nodal pan camera move gives no parallax to the shot**. Rotating a camera around its [no-parallax point](https://moviola.com/visual-glossary/no-parallax-point/), or nodal point, means there will be no shift in the position between  
    the foreground and background objects in the scene. For shots with this  
    specific type of camera move, avoid adding parallax to any elements that you composite in, as they will appear to slide against the background.

- **Make sure objects move correctly in relation to each other**. Objects further away from the camera generally move slower across the  
    frame than objects near the camera. However, do make sure that objects  
    move at realistic speeds depending on their distance from the camera. A  
    car in the far background wouldn't travel the same distance across the  
    frame as an airplane would in the same time.

- **Keep the point of fixation in mind**. Any static objects beyond the object you're fixating on will move  
    relatively in the frame in the direction of the camera motion. Any  
    objects nearer than that distance will move in the opposite direction of the camera motion.

> [!info] Point Of Fixation  
>  
> [https://youtu.be/w2R1P-K7HcQ](https://youtu.be/w2R1P-K7HcQ)  

Point of fixation example.

When fixated on the red sphere in the centre  
and moving the camera, the green foreground spheres move in the opposite  
direction to the camera motion in the frame, while the blue background  
spheres move in the same direction as the camera motion in the frame.

### Horizon Comparison

Another depth cue based on linear perspective is horizon comparison.

The position of an object relative to the horizon line plays a role in how far away we perceive it to be from the camera.

[![](https://www.keheka.com/content/images/2023/04/Horizon_Comparison.png)](https://www.keheka.com/content/images/2023/04/Horizon_Comparison.png)

Horizon comparison.

Objects which are closer to the horizon line  
tend to appear to be further away from us than objects which are higher  
up or lower down than the horizon line in the frame. Above the horizon  
line, closer objects are higher in the picture, and below the horizon  
line, closer objects are lower in the picture.

If an object moves from a position near the  
horizon line to a position higher up or lower down, it will appear to  
move closer to the viewer, and vice versa.

You have to look up to see objects above the  
horizon line, and you see objects below the line from above. Objects on  
the horizon line are in front of you, when you look straight ahead.

### → Horizon Comparison In Nuke

When you position objects in the frame, be  
conscious of where you place them in relation to the horizon line, as it  
may affect the depth perception of the scene.

Keep in mind that you may see the top of an  
object that's placed below the horizon line, and the bottom of an object  
placed above it.

A 2D element of a car, filmed side-on, may  
look odd if composited below the horizon line, because we would expect  
to see the top of the car.

If we wanted to composite another car racing  
next to the Tesla in the example image below, it would have to be filmed  
from a similar camera angle. A car shot side-on wouldn’t look right  
composited into this scene.

[![](https://www.keheka.com/content/images/2023/04/Below_Horizon.jpg)](https://www.keheka.com/content/images/2023/04/Below_Horizon.jpg)

A car below the horizon line (which is outside of the frame).

### Light Interaction

The ways objects interact with light provide  
effective cues for the brain to determine the shape and position of the  
objects in space.

We can break down light interaction into four categories:

### 1. How Light Hits An Object

The angle at which light falls on an object,  
and the intensity of the light, tell us where the object is in relation  
to the light source.

> [!important]
> 
> 💡 Unless there is a clear indication otherwise, our visual system assumes that light is falling from above.

In the example picture below, you can see  
that the top of the foreground lamppost receives more light than the  
bottom because it is closer to the light source. The fog is also  
brighter near the light bulbs, and the intensity of the light falls off  
the further away it travels.

You can also see that the silhouetted  
foreground sign is not receiving any light on the side facing the  
camera, and so it must be in front of the lampposts, blocking the light.

[![](https://www.keheka.com/content/images/2023/04/Light_Interaction_Falloff.jpg)](https://www.keheka.com/content/images/2023/04/Light_Interaction_Falloff.jpg)

Light falloff.

In the next image, we can tell by the light  
and shadows that the light source is off screen to the left. They also  
help 'shape' the object visually.

[![](https://www.keheka.com/content/images/2023/04/Cue_Ball.jpg)](https://www.keheka.com/content/images/2023/04/Cue_Ball.jpg)

The lighting, shadows, and reflections reveal the object’s 3D shape.

### 2. How Shadows Are Cast

Shadows are just the absence of light, but they can be very useful to help determine scene depth.

The way shadows appear in the scene reveals  
information about the shape and position of both the object casting the  
shadows and the object receiving the shadows.

In the example image below, we can tell by  
the shadows alone that there must be a row of arches behind us, off  
screen to the right. We can also tell by the change in the shadow angle  
that the shadows are going from hitting the floor, to hitting a wall.

[![](https://www.keheka.com/content/images/2023/04/Light_Interaction_Shadows.jpg)](https://www.keheka.com/content/images/2023/04/Light_Interaction_Shadows.jpg)

Shadow interaction.

Even without other depth cues, we can still  
get a sense of the relative distances between objects from just looking  
at the shadows:

[![](https://www.keheka.com/content/images/2023/04/Shadows.jpg)](https://www.keheka.com/content/images/2023/04/Shadows.jpg)

The objects are identical in size but their shadows indicate different positions in depth.

When the cast shadow is tighter between two  
objects (visually closer to the object casting the shadow) the two  
objects appear closer together. As the shadow moves further away, the  
objects appear further apart.

If you know the location of a light source,  
you can tell which of two objects is closer to it by looking at which  
object casts a shadow onto the other. The object shadowing the other is  
closer to the light source.

### 3. How Light Is Reflected

The way light is reflected off an object's surfaces can also provide a depth cue.

If you look at the reflection of the screen  
left iceberg in the picture below, you can see that it occludes the  
reflection of the aurora. This depth cue tells us that the aurora is  
further away than the iceberg because it is behind it in the reflection.

[![](https://www.keheka.com/content/images/2023/04/AuroraReflection.jpg)](https://www.keheka.com/content/images/2023/04/AuroraReflection.jpg)

Iceberg occluding aurora reflection.

Reflections also adhere to the rules of depth of field.

In the example image below, the camera is  
focused on the reflection of the scene behind the car. You can see that  
the wing mirror itself is out of focus while the reflection is in focus.

[![](https://www.keheka.com/content/images/2023/04/DoF_Reflection.jpg)](https://www.keheka.com/content/images/2023/04/DoF_Reflection.jpg)

Different focus in the wing mirror and in its reflection.

Knowing that with a shallow depth of field  
the focal plane keeps objects at the same distance from the camera in  
focus, we can tell that the reflection must be further away from us.  
It's not a picture stuck to the wing mirror, for example, because that  
would have been just as out of focus as the wing mirror itself.

### 4. How Light Travels Through An Object

The way light is refracted when passing through objects gives us another depth cue.

If you look at a glass bottle, any background  
objects are distorted through the bottle, indicating that they’re  
behind the bottle. Or, if you're at a lake, you can see relatively how  
shallow or deep the water is by how much of the lakebed you can see  
through the water.

[![](https://www.keheka.com/content/images/2023/04/Glass_Bottle.jpg)](https://www.keheka.com/content/images/2023/04/Glass_Bottle.jpg)

The background gets distorted through the glass bottle.

[![](https://www.keheka.com/content/images/2023/04/Lake_Dock.jpg)](https://www.keheka.com/content/images/2023/04/Lake_Dock.jpg)

Close to land the water is shallower and we can see more detail below the surface.

### → Light Interaction In Nuke

Make sure the lighting and shadows are  
consistent with the objects' distances to the camera and to the light  
sources in the scene.

This is especially important when using  
elements from an elements library, for example. If you place a crowd  
element lit from the left into a scene lit from the right, it will look  
out of place.

Ensure that any objects you composite behind  
refracting surfaces such as glasses, windows, bottles, water, etc. are  
distorted. Otherwise they won't look like they're 'sitting in' the shot.

Also make sure that any reflections you add  
are correctly based on the angle of the camera in relation to the  
reflective surface.

### Kinetic Depth Effect

We have an ability to extract information about the 3D shape of an object just by looking at its moving shadow or silhouette.

If you place for example a wire cube in front  
of a source of light so that it casts shadows onto a translucent  
screen, a person on the other side of the screen will only see a  
two-dimensional pattern of lines. But if you start rotating the cube,  
the person will start seeing the shape of the three-dimensional cube.

It doesn’t have to be a wire cube casting  
shadows, it can be a solid object as well, as long as the projected  
shadow has lines with definite corners or end points, and the lines  
change in both length and orientation as the object rotates:

> [!info] Kinetic Depth Effect Demo  
>  
> [https://youtu.be/mkhY5lANs-k](https://youtu.be/mkhY5lANs-k)  

### → Kinetic Depth Effect In Nuke

If you don’t want to fully reveal an object, the kinetic depth effect can be quite useful.

Let’s say you’re working on a sci-fi thriller  
and an alien spaceship has arrived on Earth. Before the grand reveal,  
you could have a shot where we only see the shadows cast by the  
spaceship. The audience then gets a sense of its shape first, without  
seeing it directly.

A related, and more common use of the effect is with an animated [Gobo](https://en.wikipedia.org/wiki/Gobo_\(lighting\)).  
You can place objects or cardboard cutouts in front of a light source  
and cast shadows to simulate an environment. For example, you can  
project the shadows of a canopy onto the ground so it looks like you are  
in a forest:

[![](https://www.keheka.com/content/images/2023/04/Gobo.jpg)](https://www.keheka.com/content/images/2023/04/Gobo.jpg)

Shadows cast by the canopy above.

[Grade your image](https://www.youtube.com/watch?v=LexSkfDQGaA) using a matte with the shape of the shadows you want, but [make sure not to double up the shadows](https://www.youtube.com/watch?v=Yb3Cn3JnkUI).

### Curvilinear Perspective

Just like in an image shot using a wide lens, straight lines become curved at the outer extremes of our visual field.

[![](https://www.keheka.com/content/images/2023/04/Lens_Distortion.jpg)](https://www.keheka.com/content/images/2023/04/Lens_Distortion.jpg)

Lens distortion curves straight lines in the image near the edges of the frame.

This lens distortion helps enhance the feeling in the viewer of being positioned within a real, three-dimensional scene.

It's part of why the large screens in cinemas  
help immerse you in the movie. The image fills up more of your visual  
field than the TV at home does.

360 dome theatres and IMAX theatres take this effect one step further and completely fill your peripheral vision.

The straight lines at the edges of the frame,  
either curved by the camera lens in the picture itself or bent by the  
lenses in your eyes, help visualise where things are in relation to you.  
The more they curve, the closer the objects feel, as they wrap around  
the 'sides of your eyes'.

Looking straight ahead, the image has no, or  
very little distortion in the centre, and the further out toward the  
edges of the image you look, the more distortion there is.

### → Curvilinear Perspective In Nuke

An important thing to do when compositing CG and projections, is to match the lens distortion of the scan.

CG images and undistorted projections have  
perfectly straight lines at the edges of the frame, which looks  
unrealistic. And when composited with the scan, the edges may look like  
they're sliding over the image.

You can also get creative with lens  
distortion, for example by greatly exaggerating it and/or animating it  
to portray the point of view of a character who is on drugs. By warping  
the edges of the frame, everything feels closer and more intense.

  

> [!info] Animating Lens Distortion  
>  
> [https://youtu.be/Ft6xS5EX6vg](https://youtu.be/Ft6xS5EX6vg)  

Animating the lens distortion.

> [!important]
> 
> 💡 Of all the various visual depth cues, only vergence, accommodation,  
> and familiar size provide absolute distance information. All the other  
> cues are relative, and can only be used to tell which objects are closer  
> relative to others.