---
{"dg-publish":true,"permalink":"/nuke-kb/40-courses/keheka-courses/keying-by-keheka/"}
---

Summary :

- [[#1. Understanding The Brief]]
- [[#2. Analysing The Scan]]
- [[#3. Denoising The Scan]]
    - [[#Denoise Tricks]]
- [[#4. Prepping The Plate]]
    - [[#Prep Tricks]]
- [[#5. Separating The Alpha And The Colour]]
    - [[#Using Edge Mattes]]
- [[#6. Building The Alpha Branch]]
    - [[#Using Multiple Keyers]]
    - [[#1. Core Key]]
    - [[#2. Edge Key]]
    - [[#Screen Segmentation]]
    - [[#Merging The Core And Edge Keys]]
    - [[#Making A Clean Plate]]
    - [[#Grading The Plate]]
    - [[#Stabilising Lighting Changes]]
    - [[#Using Screen Cleaning Tools]]
    - [[#Using Alternative Colour Spaces]]
    - [[#Using Unconventional Keyers]]
    - [[#Treating The Alpha]]
    - [[#Using Garbage Mattes]]
    - [[#Clamping The Alpha]]

# 1. Understanding The Brief

If you haven’t already been briefed on the shot, ask your supervisor/lead/client to kindly go over the details with you.

Watch the shot in the context of the sequence, and make sure that you  
understand the assignment. Find out which direction you should take the  
shot in – i.e. which look they are going for.

Are there any sequence peculiarities, or any particular issues with your shot,  
that you need to be aware of? It’s best to know these things up front so  
that you don’t wander down the wrong path.

Aim to clarify the following:

- **Are there any specific continuity requirements for the shot to fit into the sequence?**

Check if there is a hero shot that you should be matching your shot to – for  
things like grading, atmos levels/direction, lighting, background  
lineup, camera shake, etc. Anything that impacts the flow of the shots  
in the sequence. Ask if there are any specific templates you should be  
using for your shot, for example a matte painting projection setup which  
is being reused across multiple shots.

- **Will you be getting roto, prep, CG, matte painting, camera track, or anything else for the shot?**

If so, when should you be expecting this? (I.e. you might have a WIP CG  
render for the background to start working with, and final renders  
coming at the end of the week). And if not, is there anything specific  
that you should be doing? Like prepping out a boom mic or a tripod, or  
tracking in a still frame background, for example.

- **Which specific scan(s) and frame range(s) will you be working on?**

There may be multiple scans, and the work range may be much shorter than the  
scan range. You can usually find this information in the count sheet or  
in ShotGrid or a similar project management tool, but it’s good to  
clarify any questions you may have sooner rather than later. Make sure  
that you know exactly which portion of the blue/green screen to extract,  
and which to leave out from your key. (There might be crew or set  
equipment visible, for example).

- **What will the replaced background look like?**

It might be another plate, it could be full or partial CG, a matte  
painting, etc. It could even be a screen insert. The keying work and any  
tweaks you make will heavily depend on the new background. Are you for  
example placing a daylit character into a night time scene?

- **Is there a recurring client note on the sequence/show?**

Is there anything you should look out for in terms of client preferences?  
They might abhor magenta tones, or be particularly sensitive to  
flickering lights or dark edges. It’s a good idea to pay extra attention  
to those specific things in order to avoid client kickbacks.

And vice versa, is there anything they have mentioned that they really like  
about similar shots in your sequence? Subtle atmos, godrays, or a  
handheld camera feel, for example. Adding those things (if they make  
sense in the context of your shot) may get you some brownie points.

- **Will the shot need any transformations (repo/camera shake/etc.) to match up to the editorial reference?**

Make sure that you are aware of any changes to the framing of the shot that you will need to make.

- **Will you need to add a retime to the shot to match up to the editorial reference?**

Make sure that you are aware of any changes to the timing of the shot that you will need to make.

- **What is the time frame for the work?**

Ask when you are expected to deliver a first look, WIP, creative final,  
and/or tech final so that you can plan ahead. Make sure to [communicate expectations](https://www.keheka.com/the-iron-triangle/) early and clearly, especially if you feel the targets are unrealistic.

– And ask any other relevant questions that will help you progress with the task.

Get a good overview of the practical details surrounding the shot, as well  
as a solid understanding of the story and intention behind the shot. By  
asking these questions during the brief you can clear up any doubts or  
concerns, up front.

By the end of the brief you should be  
fully aware of your responsibilities on the shot, what you should expect  
to receive from others, and the timeline for the scheduled work.

# 2. Analysing The Scan

After the brief, play your blue/green screen plate on loop and take a closer look at what you'll be working with.

This can save you a lot of headaches later down the line, as you will be  
able to spot problems early on and catch mistakes before they even  
happen.

There is a whole range of things to watch out for:

- **Motion blur**: Is there significant motion blur in your shot? You'll have to pay  
    particular attention to the motion blurred edges when keying, and you  
    may even have to clean up parts of the original background that's being  
    revealed through the motion blur.

- **Depth of field**: Is there a shallow depth of field in your shot? You'll need to treat  
    the defocused edges differently to the sharper edges, ensuring they  
    don't get eroded in or hardened.

- **Rack focus**: Is there a rack focus in the plate? This will usually impact your key,  
    and you may even have to animate the settings in your keyers.

- **Screen lighting**: Is the blue/green screen unevenly lit? You may need to clean up or even out the poorly lit areas of the screen, and split your keying into  
    sections.

- **Creases**: Are there  
    wrinkles or folds in the blue/green screen? You may have to clean them  
    up. The keyers often pick up the highlights and shadows in the creases,  
    and include them in the output.

- **Intersections**: Are there objects, tracking markers, screen seams, etc. that are  
    intersecting with your subject? You may have to clean them up to avoid  
    unwanted background bits being included in your key and artefacts  
    appearing.

- **Out of bounds movement**: Does your subject cross the boundary of the blue/green screen? You may need to roto (or request roto for) parts of the shot.

- **Lighting changes**: Are there any dynamic lighting changes in the shot that will disturb  
    your keyer? You may have to stabilise the lighting before keying, or  
    animate your keyer settings.

- **Frame range**: Is your shot quite long? The longer a shot is, the more you'll want to  
    rely on robust keying solutions and procedural methods, as opposed to  
    manual, frame-by-frame work. E.g. painting over an issue across a 20  
    frame shot is much less painful than doing it across a 700 frame shot.

- **Similar colours**: Does your subject wear clothes similar in colour to the blue/green  
    screen? It might be tricky to separate those from the background, and  
    even despill them accurately, without roto.

- **Shiny objects**: Is your subject wearing reflective jewellery, sunglasses, or other  
    things that will reflect the blue/green screen? These may cause holes in your key, and you may have to patch the holes using roto or other  
    techniques.

- **Background brightness**: Will your new background be very different in brightness compared to  
    the blue/green screen? You may need to fix bright or dark edges on your  
    subject that become apparent over the new background.

- **Spill**: Is there significant spill light in your plate? You may have to despill more than just the edges of your subject.

- **Motion**: Is the camera or subject moving? It may be useful to track the  
    background or subject to help with roto work, even for garbage matting.

- **Small gaps**: Are there any fine gaps, for example between fingers or other objects?  
    This can cause a 'sticky’ key, also known as 'webbing' (as in a duck's  
    webbed feet). Especially, if the gaps are opening and closing throughout the shot.

- **Foreseeable issues**: Are there any other issues that you can foresee, and is there anything  
    else that would be useful for you to have available? You may be able to  
    request additional roto support or camera tracking, which could for  
    example help with clean up work.

All this might seem like a lot to keep track of, but I listed it more for your awareness.  
The idea is to approach the shot with an analytical mind, and look for  
things that could become potential issues during the keying process.

Getting a good overview of what the requirements are for your specific shot,  
before you start working on it, will help you make more informed  
decisions about which tools to use and how to best approach the work.

And it will help you avoid painting yourself into a corner.

> [!important] At this stage you should also check any elements that you have received,
> 
>   
> and make sure that they are all working as expected. For example, that  
> the CG renders don’t have any corrupt or missing frames, or that the  
> roto or prep covers everything you need.
> 
>   

  

Going forward in the guide, we'll be building a robust framework that you can use as a base for any keying shot:

[![](https://www.keheka.com/content/images/2023/10/Full_Setup.png)](https://www.keheka.com/content/images/2023/10/Full_Setup.png)

_Bird's-eye-view of the setup which we will be building._

  

Let’s start off by removing the noise/grain from the scan.

# 3. Denoising The Scan

Keyers are very sensitive to colour differences, including the micro-changes in colour that grain introduces to an image.

In my experience, it’s better to denoise the scan before doing any keying  
work in the vast majority of cases (>99.5% of the time). You’ll  
typically get a smoother alpha with less chatter along the edges.

[![](https://www.keheka.com/content/images/2023/10/Key_Without_Denoise.jpg)](https://www.keheka.com/content/images/2023/10/Key_Without_Denoise.jpg)

Keying on the original scan means you often get noisy edges (and noise in general) in the alpha channel.

[![](https://www.keheka.com/content/images/2023/10/Key_With_Denoise.jpg)](https://www.keheka.com/content/images/2023/10/Key_With_Denoise.jpg)

The exact same key on the denoised scan yields a much cleaner result.

💡

The ReduceNoise plugin from [Neat Video](https://www.neatvideo.com/overview/what-is-it?ref=keheka.com) is excellent for denoising images.

Note that on rare occasions you can lose some detail when keying on the  
denoised scan. If that happens, first try to improve the denoise itself  
so that it retains more detail from the original scan:

Sample a different, more flatly lit area of the scan without any detail or pattern.

If you’re using the Neat Video ReduceNoise plugin, [adjust the advanced settings](https://www.neatvideo.com/support/tutorials/nv5/advanced-workflow?ref=keheka.com) to build a better grain profile.

If you don’t have the Neat Video ReduceNoise plugin, you can adjust Nuke’s native Denoise node. Here is a [tutorial by The Foundry](https://learn.foundry.com/nuke/content/comp_environment/denoise/removing_noise_denoise.html?ref=keheka.com).

If that fails, you can key only the necessary section (for example a piece  
of hair detail) using the original scan, and then Keymix that back in  
with the rest of the key.

  

### Denoise Tricks

Denoisers often don’t handle negative values in the scan very well.

Their algorithms usually expect values that are equal to or above 0 in the  
plate. And so negative values can throw a wrench in the works and  
introduce artefacts (or just poor denoising) in those areas.

A way around that is to add a gentle lift to the scan with an Add node –  
in order to nudge the values into the positives before denoising – and  
then reversing that lift afterwards:

[![](https://www.keheka.com/content/images/2023/10/Denoise_Add-Subtract.png)](https://www.keheka.com/content/images/2023/10/Denoise_Add-Subtract.png)

Add-Subtract setup for denoising.

Above, I expression-linked the _value_ from the Add1 node to the Add2 node, and put a minus in front to make it negative. In other words, in the Add2 node’s _value_, I added the following expression:

- **Add1.value**

That way, you only need to adjust the Add1 node, and the setup will correctly reverse the lift automatically for you.

To avoid killing any detail in the scan, you'll want to keep the lift value as low as possible. Only increase it _just_ enough to tip the darkest values out of the negatives.

Nudge the Add1 node's _value_  
slightly up to remove the negative pixels. (Usually somewhere between  
0.001 and 0.01). You can use the Up, Down, Left, and Right arrow keys to  
nudge values in the number fields in Nuke.

  

> [!important] Check multiple frames of your scan to ensure you get rid of all the negative values.

  

Keep in mind, the Viewer in Nuke is not great at displaying negative values.  
Any values below 0, or black, will also just appear black in the  
Viewer. So for example, the values 0 and -1 will both look the same.

To visually display the negative values so that you can more easily  
eliminate them with the Add1 node, you can use either of the following  
two simple setups:

[![](https://www.keheka.com/content/images/2023/10/Denoise_Check_Method_1.png)](https://www.keheka.com/content/images/2023/10/Denoise_Check_Method_1.png)

Method 1 for displaying negative values.

The first setup is quite straightforward, and can be built with three nodes:

Start by multiplying your scan by -1 with a Multiply node. That will make all  
positive values negative, and all negative values positive.

Then, add a Clamp node and clamp all the negative values. That will set all  
the originally positive values to 0, and leave you with only the  
originally negative values – now as faint, positive values.

Multiply these values by 1000 with another Multiply node to be able to easily see them in the Viewer.

You can now nudge the Add1 node’s _value_ up until you can no longer see any pixels when viewing the Multiply2 node.

  

> [!important] This Multiply-Clamp-Multiply setup is only for viewing your image’s negative values. It’s not part of the actual denoising.

  

Another way to visualise the negative values, is to use an Expression node instead of the Multiply-Clamp-Multiply setup:

[![](https://www.keheka.com/content/images/2023/10/Denoise_Check_Method_2.png)](https://www.keheka.com/content/images/2023/10/Denoise_Check_Method_2.png)

Method 2 for displaying negative values.

[![](https://www.keheka.com/content/images/2023/10/Negative_Values_Expression.png)](https://www.keheka.com/content/images/2023/10/Negative_Values_Expression.png)

Expression for checking negative values.

In the Expression1 node above, I’ve put the the same expression into all of the R, G, and B channels:

**r < 0 || g < 0 || b < 0? 1 : 0**

For each pixel, this Expression node will check if either the red, green,  
or blue channel has a negative value in it. If it does, it makes the  
pixel white. If it doesn’t, it makes the pixel black.

The resulting high-contrast image makes it super easy to spot any negative  
values. And, to see when they disappear as you increase the Add1 node’s _value_.

Lifting up the scan values by eye like this is usually more than good enough  
for denoising. But for the sake of showing another useful technique – If  
you wanted to be even more technically precise, you could:

Run the _Max Luma Pixel_ option in a CurveTool node connected to the scan, with the Region Of  
Interest covering the full format, and analyse the scan over the whole  
frame range.

And then, in the Curve Editor, find the single  
lowest value of the Minimum Luminance Pixel Value curves, and add an  
equal amount in the Add1 node’s _value_ for an exact offset.

[![](https://www.keheka.com/content/images/2023/10/Min_Luma_Pixel.png)](https://www.keheka.com/content/images/2023/10/Min_Luma_Pixel.png)

Finding the lowest value on the Minimum Luminance Pixel Value curves.

Select the lowest point of the three curves and copy that point’s value into the Add1 node’s _value_,  
removing the minus sign to make it a positive offset. (Double click on  
the underlined floating numbers by the circled selected point in order  
to bring up a box that you can copy the value from).

  

> [!important] It’s super overkill to do it this way, but I wanted to mention the technique
> 
>   
> anyway – as the same technique is useful for finding specific values in  
> other scenarios, for example when [normalising an animation curve](https://davemne.wordpress.com/2013/08/04/nuke-normalizing-animation-curves/?ref=keheka.com). ([Normalisation formula](https://en.wikipedia.org/wiki/Feature_scaling?ref=keheka.com#Rescaling_\(min-max_normalization\))).

  

Whichever way you decide to remove the negative values, continue by adjusting  
your denoise to remove the grain. The most important thing for achieving  
a solid denoise is to **sample a flatly lit area without any texture or details, such as a blank wall or surface**.

Once the denoising is complete, you may notice that it can be fairly heavy to compute.

To avoid unnecessary calculations and slowdown in your main comp script,  
make a separate denoise script and precomp the denoised scan.

  

> [!important] When rendering out your precomp, it's important to avoid compressing the
> 
>   
> image destructively and introducing artefacts. Keep as much colour data  
> as possible in order to give the keyers the best chance of pulling good  
> keys. So, instead of using lossy formats such as JPG, render to a  
> lossless format such as EXR – with at least 16-bit colour depth.

  

Then, import only the render of the denoised scan into your main script:

[![](https://www.keheka.com/content/images/2023/10/Full_Setup_Denoise_Precomp.png)](https://www.keheka.com/content/images/2023/10/Full_Setup_Denoise_Precomp.png)

[![](https://www.keheka.com/content/images/2023/10/Denoise_Precomp-1.png)](https://www.keheka.com/content/images/2023/10/Denoise_Precomp-1.png)

The precomped denoised scan imported into the main script.

Once you've got your precomped denoised scan, the next step is to prepare it further for keying.

  

> [!important] **Notice for the rest of the guide:**
> 
> While the following steps can be followed in a linear way, you will very likely have to go back and forth between them and **adjust as you build**.

  

# 4. Prepping The Plate

Preparing the plate before keying is useful for two reasons.

Firstly, you remove things from the scan that would otherwise interfere with your key, for example:

- Tracking markers

- Screen seams

- Rigs or wires

- Crew

- Lens dirt

- Set equipment

- Screen wrinkles

- Unwanted shadows

[![](https://www.keheka.com/content/images/2023/10/Bad_Green_Screen.jpg)](https://www.keheka.com/content/images/2023/10/Bad_Green_Screen.jpg)

Hopefully your screen won’t be as challenging as this one.

By removing the things in the list above – especially in the areas where they intersect with the subject that you're keying – **you'll get a cleaner key**.

Secondly, with a prepped plate **you will also get a better auxiliary key** (see point 8), without ghosting or other artefacts.

You may receive a cleaned up plate from the Prep department. However, often  
you'll have to do some basic clean up work yourself.

If you do end up doing any painting or patching, it's a good idea to  
precomp that work before you start keying. Some of the more advanced  
keying techniques ([like using the IBK](https://www.keheka.com/advanced-ibk-keying-techniques-and-setups-in-nuke/))  
can be quite heavy to calculate. And so working on a precomped plate  
ensures your script stays as fast and responsive as possible.

Like with the denoise, separating the prep work into its own prep script is a  
good idea to keep your main script running smoothly.

However, oftentimes you'll have to go a bit back and forth between the clean up  
work and the keying work – making tweaks to get the best result. And so  
keeping the prep work within the main script while still making use of  
precomping is also an option:

[![](https://www.keheka.com/content/images/2023/10/Full_Setup_Prep_Precomp.png)](https://www.keheka.com/content/images/2023/10/Full_Setup_Prep_Precomp.png)

[![](https://www.keheka.com/content/images/2023/10/Prep_Precomp.png)](https://www.keheka.com/content/images/2023/10/Prep_Precomp.png)

_In-script precomp of the prep._

  

In those scenarios, I like to use this little Switch setup for my precomps  
– for two reasons: To make it super easy to see that there is a precomp  
happening in the script (bright yellow backdrop), and to easily switch  
between the live and the rendered pipe using the Switch node.

  

> [!important] An in-script precomp like this won't help with any memory issues you may
> 
>   
> face (i.e. when your Nuke script gets too large for your computer or the  
> render farm to handle). But it _will_ help avoid unnecessary processing slowdown, because the prep work won't have to be calculated again. If you lack RAM, try making a separate  
> prep script instead.

  

**Make sure to include a matte for your prep work in the alpha channel when you render the precomp.** (See the Paint Tricks section under point 14 for tips). We’ll need it later for the grain matte.

### Prep Tricks

There are many different ways of cleaning up unwanted parts of the scan.

Outside  
of the more classic painting or patching methods, I thought I’d list  
some other useful techniques for doing your cleanup:

- The [Monochrome](https://www.chrisfryer.co.uk/post/monochrome-tutorial-combining-channels-theory-and-practical-applications?ref=keheka.com) technique is great for cleaning up tracking markers.

- The [Erode Dilate](https://www.keheka.com/erode-dilate-technique/) technique is useful for simple marker, screen seam, or wire removal.

- The [PxF_Filler](https://youtu.be/RN6-iA2nkdk?feature=shared&ref=keheka.com) tool is excellent for filling in all of the above, particularly for objects that are at an angle.

- My [DeFlicker](https://www.keheka.com/add-or-remove-flickering-in-nuke/) tool works really well for matching flickering light on a patch. Or,  
    for removing flicker from the plate in the alpha branch in order to  
    stabilise the key.

- You can also simply stencil  
    out unwanted parts from the alpha, post-keying. Roto the areas you want  
    to remove, and Merge (stencil) the roto (A) with your key (B).

Another great technique, especially for cleaning up parts of a plate with dynamic lighting, is to track in live clone strokes:

1. Track the area you want to clean up using a Tracker node, and set the  
    Tracker’s reference frame to the frame that you will paint on.

1. Paint your clone strokes on the reference frame of your choice using a RotoPaint node.

1. Set the Lifetime of the clone strokes to **all frames** to ensure they stay live throughout the shot.

1. Expression-link the tracking data from the Tracker node to the RotoPaint node:

1. Select the Root layer in the RotoPaint node’s properties and rename it to for  
    example RootTRACKED, as a visual reminder that tracking data is being  
    applied.

1. Open up the Transform tab of the RotoPaint node’s properties, and the Transform tab of the Tracker node’s properties.

1. Press Ctrl/Cmd and left-click-and-drag the corresponding animations across  
    from the Tracker node’s properties to the RotoPaint node’s properties.  
    Translate → translate, rotate → rotate, scale → scale, and center →  
    center. Rotation and scale are not always necessary (1-point track, for  
    example), but it’s a good habit to include them anyway, in case you add  
    more tracks to the Tracker later on.

[![](https://www.keheka.com/content/images/2023/10/Live_Cloning.png)](https://www.keheka.com/content/images/2023/10/Live_Cloning.png)

Live-clone technique.

The clone strokes will now resample on every frame, accounting for any lighting changes in the plate.

Use any combination of the techniques above (and/or others) to clean up the  
blue/green screen plate. Then, precomp the prep, and you'll be ready to  
start keying.

  

> [!important] The same rendering tips as described for the precomp of the denoised scan apply here.

  

The first step is to branch out.

  

# 5. Separating The Alpha And The Colour

Many keyers try to be a one-stop shop.

They output a despilled scan premultiplied by the generated alpha, sometimes even composited over the background.

**But a great key is almost** _**never**_ **a one-click or a one-keyer solution.**

Just like how you (should) separate your whites from your colours when doing  
laundry, you should also separate your alpha from your colours when  
keying.

That way, you're able to treat your image one way for extracting the best alpha, and a _different_ way for getting the best RGB colours for your composite.

  

> [!important] I pretty much never use the RGB colour output from the various keyers.
> 
>   
> With a split node tree you will have more options and finer control.

  

So, from your precomped prep plate, split your node tree into two branches:

1. **The alpha branch**, where you purely will be focusing on creating the best possible alpha.  
    Here, you can do whatever it takes to the scan in order to get a great  
    matte. Grade the scan, deflicker it, and/or alter it however you want  
    until you get the best result.

1. **The colour branch**, where you will be preserving the integrity of the scan as much as  
    possible. Here, you only despill, grade, and make minor alterations to  
    the scan where needed. That way, you keep the image fidelity pristine  
    and in keeping with the Director of Photography's vision.

[![](https://www.keheka.com/content/images/2023/10/Full_Setup_Branching_Out.png)](https://www.keheka.com/content/images/2023/10/Full_Setup_Branching_Out.png)

[![](https://www.keheka.com/content/images/2023/10/Alpha_Colour_Branches.png)](https://www.keheka.com/content/images/2023/10/Alpha_Colour_Branches.png)

_Branching out._

### Using Edge Mattes

Both in the alpha branch and in the colour branch, you'll have to treat the core and the edges of your subject differently.

To help with that, you can create an edge matte from your alpha and use it to Keymix between the core and the edge treatments.

Nuke's native EdgeDetect node is pretty good for that task, but it is quite limited. Instead, use for example [Perimeter](https://www.keheka.com/perimeter-advanced-edge-mattes/), or another edge matte gizmo from Nukepedia. They have a lot more options for finer control.

You can for instance create an edge matte with a soft, natural inner  
falloff. An edge matte like this is very useful for blending between the  
edge despill and the core despill.

[![](https://www.keheka.com/content/images/2023/10/Soft_Edge_Matte.png)](https://www.keheka.com/content/images/2023/10/Soft_Edge_Matte.png)

Soft edge matte.

Keep that in your back pocket as we move forward in the guide.

All right. Let's jump into the alpha branch first.

  

# 6. Building The Alpha Branch

In the alpha branch, you can do anything to the RGB values in the plate in order to extract the best matte for your key.

You can, for example:

- Make a clean plate to build a great key with the IBK.

- Grade the plate to even out colour gradients or lighting differences, and get a more consistent screen colour.

- Stabilise any lighting changes which are causing issues for the keyers.

- Change the colour space of the plate to be able to pull a (better) key.

- Paint or patch the plate to remove any screen imperfections that weren't taken care of in the Prep stage.

- Use screen cleaning tools, or do anything necessary to help the keyers create the best alpha.

> [!important] Just be careful that you're not introducing any artefacts or reducing edge
> 
>   
> detail when treating the RGB values before pulling the key.

  

[![](https://www.keheka.com/content/images/2023/10/Full_Setup_Pre-Key_Treatments.png)](https://www.keheka.com/content/images/2023/10/Full_Setup_Pre-Key_Treatments.png)

[![](https://www.keheka.com/content/images/2023/10/Pre-Key_Treatments.png)](https://www.keheka.com/content/images/2023/10/Pre-Key_Treatments.png)

_Applying some treatments to the plate in the alpha branch before keying._

  

> [!important] Before you start keying, it’s a good idea to remove any alpha from your plate
> 
>   
> so that it won’t accidentally interfere with your keying.

On top of these treatments, you can also mix and match different keyers  
for keying disparate parts of the blue/green screen, as well as for  
creating core and edge mattes.

And you can further treat the alpha in a number of ways to improve it.

We'll look at all of the above, starting with perhaps the most important one:

### Using Multiple Keyers

The perfect blue/green screen doesn't exist.

At least, not in the vast, _vast_ majority of shots that you'll be working on.

Most blue/green screens are not perfectly lit. They are not perfectly clean,  
not perfectly stretched out, not perfectly seamless, not perfectly…  
anything.

And so they can't be keyed with a one-click key.

**So instead, break down the overall keying task into multiple, smaller keying tasks.**

Start by dividing the keying work into two main branches: the core key and the edge key.

### 1. Core Key

The core key is for creating an opaque matte covering the inner core of your subject.

For this key, you'll want to make sure that there are no holes in the alpha  
which could lead to transparency issues. For example, if the matte is  
transparent over the subject's face, the subject might inadvertently  
look like a ghost over the background.

You can push and pull the keyer settings quite aggressively for the core key, and crunch  
the alpha to harden it. Retaining fine edges is not the objective for  
this key; you just want a solid white core (alpha = 1) in the areas that  
should be opaque.

In fact, you will be eroding in the  
edges of the core key anyway, to avoid hardening them and creating for  
example dark or lumpy edges when merged with the edge key.

You can use any keyer you like for the core key. Whichever gets the job

done the best for your shot, really. I tend to use the Keylight quite  
often.

I’ll colour-pick a representative screen colour  
somewhere close to the subject in the Screen Colour, and then adjust the  
Screen Gain to remove transparency in the alpha over the screen.

Then, I’ll adjust the settings in both the Screen Matte and Tuning menus  
until I get a solid white colour in the alpha for the core of the  
subject, as well as a solid black colour in the alpha for the  
surrounding blue/green screen backing.

  

> [!important] Lower the Viewer Gamma to help spot transparency issues in the core matte.
> 
>   
> And likewise, raise it to help spot low alpha values over the blue/green  
> screen backing.

  

Later on, we’ll be merging the  
alphas from the core key and the edge key. As mentioned above, the core  
key is quite hard-edged. And so you’ll want to post-process it:

- Add an Erode node to the core key so that you can shrink it a little to  
    stop the hard edges from interfering with the nice edges of the edge  
    key.

- Add a Blur node directly after the Erode node to be able to soften the transition between the two keys.

[![](https://www.keheka.com/content/images/2023/10/Full_Setup_Key.png)](https://www.keheka.com/content/images/2023/10/Full_Setup_Key.png)

[![](https://www.keheka.com/content/images/2023/10/Key_Core_Key.png)](https://www.keheka.com/content/images/2023/10/Key_Core_Key.png)

_Core key using the Keylight._

### 2. Edge Key

The edge key is for creating a semi-transparent matte for (as the name suggests) the edge details of your subject.

For this key, you'll want to make sure that you capture any fine details  
such as hair and motion blurred edges. Without them, the subject  
typically will look too cut out and hard-edged over the background.

You have to be much gentler with the keyer settings for the edge key. Don't  
worry about the core being opaque, that's already being taken care of  
by the core key. Here, you don't want to crunch the alpha too much, but  
instead focus on getting the most amount of detail in your alpha.

As with the core key, use any keyer which gets the job done the best for your particular shot. I tend to use the [IBK Dilate](https://www.keheka.com/advanced-ibk-keying-techniques-and-setups-in-nuke/) setup quite often. I’ll plug it in, set my screen type to blue or green  
depending on which screen I’m keying, and then adjust the red weight  
and blue/green weight values as needed.

[![](https://www.keheka.com/content/images/2023/10/Key_Edge_Key.png)](https://www.keheka.com/content/images/2023/10/Key_Edge_Key.png)

_Edge key using the IBK Dilate setup._

  

> [!important] Above, a clean plate is being generated for the IBK key. If you would have a
> 
>   
> real clean plate that was shot on set, or another clean plate that you  
> created yourself, you would instead connect that to the IBKGizmo’s _c_ input.

  

The IBK Dilate setup is excellent for creating an edge key. You don't want  
to push the IBKGizmo settings too hard, thickening up and ruining the  
fine edge details, so often there will be some transparency left in the  
core of your subject. – Which again is fine as the core key will take  
care of that.

You may also find that the matte from the  
IBK is a little bit noisy in some areas, particularly in the dark areas  
of the blue/green screen plate.

To fix these issues, Merge and/or Keymix the alpha that’s output by the IBK with the alpha output  
by other keyers (for example the Keylight) as necessary. You have to be a  
bit surgical and go in and Keymix other keyers in specific problem  
areas. Stacking a whole bunch of keyers together can harden your alpha  
edges too much.

And the same goes for the core key: use different keyers/settings for different problem areas, and then combine them.

So, let’s split up the keying work in each of the two branches further:

### Screen Segmentation

A blue/green screen often consists of multiple cloths with different shades of blue/green.

And/or, it may be lit quite differently on one end compared to the other.

One keyer may be able to extract great detail from one part, or even a few  
parts, of the blue/green screen, but usually not all at once.

So, it’s better to split up the differently lit or differently coloured  
sections of the blue/green screen, and key each of them separately  
instead of trying to get it all in one go.

And then, Keymix the mattes together to get the best of each.

[![](https://www.keheka.com/content/images/2023/10/Key_Keymixing_Mattes.png)](https://www.keheka.com/content/images/2023/10/Key_Keymixing_Mattes.png)

_Keymixing mattes from different keyers._

Above, I have mixed two sets of keyers for keying a green screen with two distinct shades of green.

  

> [!important] Feather or blur your roto mattes to soften the transition between the Keymixed
> 
>   
> alphas. Otherwise, you may for example see sharp staircasing along  
> edges, or get visible matte lines going through parts of your subject.

  

Next, let's combine the two branches:

### Merging The Core And Edge Keys

You have some options for merging the two keys (or, two sets of keys).

Using a Merge (over) is my default way of merging mattes together. However,  
in some cases it can harden the semi-transparent edges of your subject a  
little bit too much.

In those situations, you can use a Merge (max) instead, to avoid the edge hardening.

If you want more control over exactly how the two mattes blend together, you can merge them using an AddMix node.

Add points to the A and B curves in the AddMix by pressing Ctrl/Cmd and Alt  
and left-clicking on the curves, and manually adjust them to taste.  
(See point 12 for AddMix curve tips).

  

> [!important] Do not use a Merge (plus) for merging the mattes together. That usually
> 
>   
> leads to alpha values going above 1, which can cause issues when merging  
> your key over the background.

  

  

[![](https://www.keheka.com/content/images/2023/10/Key_Merge_Core_And_Edge_Key.png)](https://www.keheka.com/content/images/2023/10/Key_Merge_Core_And_Edge_Key.png)

_Merging the core and edge key together._

So far, we’ve only looked at working with fairly decent blue/green  
screens. But what happens when you get shots with poorly lit, uneven,  
and/or wrinkled blue/green screens?

There are several ways you can deal with those:

### Making A Clean Plate

A clean plate is just the blue/green screen on its own, without the subject present.

It can be used for extracting an even better key. And it's especially great for [advanced keying techniques using the IBK](https://www.keheka.com/advanced-ibk-keying-techniques-and-setups-in-nuke/), which is one of my all-time favourite keyers.

  

> [!important] A clean plate is also very useful for a specific type of edge treatment – see the Un-Comping section in point 7 of the guide.

  

If you’re _very_ lucky, you might receive a separate clean plate filmed for your shot.  
Or, there might be frames in your plate without the subject present in  
them. But in most cases, you’ll need to make a clean plate yourself.

There are multiple ways of generating a clean plate. Check out the IBK  
tutorial I linked just above for some really solid procedural  
techniques.

A more manual method is to patch in clean parts of the frame from earlier or later in the scan’s frame range.

[![](https://www.keheka.com/content/images/2023/10/Framehold_Patch.png)](https://www.keheka.com/content/images/2023/10/Framehold_Patch.png)

_Patching in clean sections of the plate._

However, if the camera is moving, it can be tricky to line up the patches _exactly_.

**And the more accurate the clean plate, the better the result you will achieve when keying.**

  

> [!important] Ideally, the clean plate should be a 1:1 match with the scan, only without the subject present.

  

A trick to checking that your patch lineup is pixel perfect (or as close as possible), is to use the [Invert Dissolve](https://youtu.be/8Wj_tpx4P1k?feature=shared&ref=keheka.com) technique. Make sure you’re changing as little of the plate as  
possible, within reason. If possible, only the area of the plate that  
the subject occupies.

There are many more ways of helping your keyer deal with difficult blue/green screens:

### Grading The Plate

Remember that you can do anything to the RGB colours of the plate in the alpha branch to help get a better key.

And keyers typically produce the best results with an evenly coloured blue/green screen.

So, if there are colour gradients or lighting differences on the blue/green  
screen, you can grade the plate using for example a Ramp as a mask  
input for the Grade node, in order to get a more consistent screen  
colour.

[![](https://www.keheka.com/content/images/2023/10/Full_Setup_Pre-Key_Treatments-1.png)](https://www.keheka.com/content/images/2023/10/Full_Setup_Pre-Key_Treatments-1.png)

[![](https://www.keheka.com/content/images/2023/10/Pre-Key_Treatments_Grade.png)](https://www.keheka.com/content/images/2023/10/Pre-Key_Treatments_Grade.png)

_Grading the plate pre-keying._

Gain up shadowy areas of the screen (or a vignette), darken down pools of  
light, or even out colour differences between different cloths.

It’s surprisingly effective.

You may end up needing fewer keyers in total, because each keyer will be  
able to successfully cover a larger area of the blue/green screen.

### Stabilising Lighting Changes

The same is true for blue/green screens where the lighting is changing or flickering.

You may find that your keyer creates a great alpha on some frames, but  
‘breaks’ on others due to a significantly different lighting.

In those cases, you can stabilise the lighting changes which are causing issues with keying the blue/green screen.

  

> [!important] As mentioned in the Prep Tricks section, my
> 
> [DeFlicker](https://www.keheka.com/add-or-remove-flickering-in-nuke/) tool is very useful for removing flicker or lighting changes from the plate to help stabilise the key.

  

You can also animate the settings in the keyer, but that is a much more  
manual way of doing it compared to automatically analysing the lighting  
changes in the plate and generating a counter-grade.

[![](https://www.keheka.com/content/images/2023/10/Pre-Key_Treatments_Stabilise_Lighting.png)](https://www.keheka.com/content/images/2023/10/Pre-Key_Treatments_Stabilise_Lighting.png)

_Stabilising the plate lighting pre-keying._

### Using Screen Cleaning Tools

If you are working with a tricky blue/green screen, another way to help  
potentially get a better key is to use screen cleaning tools.

By using a real or synthetic clean plate, the tools can can even out:

- Folds

- Seams

- Smudges

- Uneven lighting

- Blue/green set pieces

- Variation in the backing colour

- Unwanted shadows cast by set pieces, boom arms, etc.

– Replacing all of the above with a single blue/green colour for keying.

A couple of solid options for screen cleaning are:

- [PxF_ScreenClean](https://youtu.be/d_MbzYnM66I?feature=shared&ref=keheka.com)

- [ApScreenClean](https://adrianpueyo.com/gizmos/?ref=keheka.com)

These – or any other screen cleaning tool – are not a silver bullet, though.

[![](https://www.keheka.com/content/images/2023/10/Screen_Clean.png)](https://www.keheka.com/content/images/2023/10/Screen_Clean.png)

The screen cleaners can do a pretty good job, but **always inspect the edges**.

Just be careful that you are not eroding your edges too much, or introducing any artefacts to your image.

### Using Alternative Colour Spaces

A luma keyer typically works best when there is a clear separation in luminance between the subject and the background.

And so it can be useful to shuffle the channel with the most separation into RGB, and luma key on that.

For example, a green screen will usually have higher pixel values in the  
green channel than in both the red and blue channels combined:

[![](https://www.keheka.com/content/images/2023/10/Green_Screen_Red_Channel.jpg)](https://www.keheka.com/content/images/2023/10/Green_Screen_Red_Channel.jpg)

[![](https://www.keheka.com/content/images/2023/10/Green_Screen_Green_Channel.jpg)](https://www.keheka.com/content/images/2023/10/Green_Screen_Green_Channel.jpg)

[![](https://www.keheka.com/content/images/2023/10/Green_Screen_Blue_Channel.jpg)](https://www.keheka.com/content/images/2023/10/Green_Screen_Blue_Channel.jpg)

_The red, green, and blue channels of a green screen plate._

Notice how much brighter the green screen is in the green channel (second image).

This separation may not always be as clear. Sometimes the difference isn’t  
great enough for the keyer to clearly distinguish between (parts of) the  
subject and the background.

So the keyer may make a  
portion of the subject transparent which we actually want to keep  
opaque, because the values there are too similar to the background.

When that happens, it's a good idea to explore alternative colour spaces and  
look for ones which have more separation between the subject and the  
background in one of the channels:

Add a Colorspace node  
to your image and change the output to a different colour space, such as  
YCbCr or L*a*b. Run through the list and search for useful separation.

  

> [!important] Right after you select a colour space in the drop-down menu, you can use the
> 
>   
> Up and Down arrow keys on your keyboard to cycle through all of the  
> colour spaces. That way, you can quickly get an idea of which colour  
> spaces have substantial subject/background separation in a channel.

  

If you find a colour space which has a channel with more contrast and  
separation between the subject and background in the area you want to  
key, try keying on that channel. Shuffle the channel into RGB and apply a  
luminance key. You'll be surprised at how often an 'unsalvageable' shot  
can be keyed.

[![](https://www.keheka.com/content/images/2023/10/Luma_Key_Alternative_Colour_Space.png)](https://www.keheka.com/content/images/2023/10/Luma_Key_Alternative_Colour_Space.png)

_Luma keying in an alternative colour space._

  

Another way to help your keyer out is to change the colour space to a logarithmic one, using for example the Log2Lin node.

  

[![](https://www.keheka.com/content/images/2023/10/Luma_Keying_In_Log.png)](https://www.keheka.com/content/images/2023/10/Luma_Keying_In_Log.png)

_Luma keying in log space._

Setting the _operation_ to Lin2Log will compress all of the values in the image down to roughly a 0-1 range (usually some decimals above 1).

This simplifies the underlying process a bit and can help extract more  
detail, especially when luma keying using the Keyer node.

  

[![](https://www.keheka.com/content/images/2023/10/Luma_Key_Lin.png)](https://www.keheka.com/content/images/2023/10/Luma_Key_Lin.png)

[![](https://www.keheka.com/content/images/2023/10/Luma_Key_Log.png)](https://www.keheka.com/content/images/2023/10/Luma_Key_Log.png)

_Comparison of the detail extracted by a luma key without (left) and with (right) a log space conversion first._

Luma keying in log space often gives a less harsh alpha, with more subtle  
detail. Take for instance a look at the two main locks of hair. See how  
when luma keyed in log space, gentler detail is visible. Instead of  
getting quite solid black locks, there is more fine variation. (Note:  
The alpha above is white for the background and black for the hair, you  
would invert it to keep the hair).

Which, when you premultiply the alpha with the colours, will help avoid light or dark edges tagging along from the background.

You can also use Log space for merging your key over the new background,  
which again helps retain more fine detail. (See point 12).

  

> [!important] Also check out
> 
> [Ben McEwan’s blog post on colour spaces](https://benmcewan.com/blog/2018/08/23/when-to-utilize-a-different-colourspace/?ref=keheka.com).

  

### Using Unconventional Keyers

You're not limited to only using the off-the-shelf keyers that Nuke provides.

Try novel keying approaches, such as:

- [STMap keyer](https://www.chrisfryer.co.uk/post/artistically-driven-keying-despill-experiments-with-blinkscript?ref=keheka.com)

- [Quadratic luma keyer](https://youtu.be/yLnSZxwlOyA?feature=shared&ref=keheka.com)

- [Point cloud keyer](https://www.isaacspiegel.com/blog/take-keying-to-a-new-dimension-with-the-point-cloud-keyer?ref=keheka.com).

Experiment and try out new things. You may even come up with a clever technique that turns out to be super useful.

And there is always room for improvement.

Trying different colour spaces and unconventional keyers is about exploring  
your options and being solution-oriented. If 20 minutes of experimenting  
can save you 4 hours of roto, that’s definitely worth it.

### Treating The Alpha

The matte that a keyer outputs doesn't have to be the end result.

You can make changes to the alpha that is output by each keyer separately, or to the alpha as a whole.

For example, you may find that there is a little bit of unwanted  
transparency left in the matte after you have tweaked the keyer. Or,  
that a matte has become too dense along the edges of the subject.

Instead of pushing your keyer settings too far and breaking the work that you  
have already done (and that is working well in other areas), you can  
post-process the alpha:

- Use the [Erode Dilate](https://www.keheka.com/erode-dilate-technique/) technique to fill holes in the matte.

- Use the [Alpha Density](https://www.keheka.com/alpha-density-matte-control-tool-for-nuke/) tool to control the density of the matte.

- Grade the alpha with a Grade node set to affect the alpha channel. (But always enable _black clamp_ and _white clamp_ to keep the values between 0 and 1). You can for example raise the _blackpoint_ to remove fine noise, or lower the _whitepoint_ to fill in any transparency.

- Grade the alpha with a ColorLookup node. Same as with the Grade node, except  
    that you have more control over the falloff by adjusting the Alpha  
    curve. Add points to the curve by pressing Ctrl/Cmd and Alt, and  
    left-clicking on the curve.

- Use a combination  
    of Erode and Blur nodes to gently nudge an edge inward and soften it. If a keyer creates a little bit of a hard and sharp edged alpha, for  
    example, you can add an Erode (filter) set to a value of 1 and a Blur  
    set to a size of 3. For even finer control, use Spin VFX’s [Erode_Fine](https://github.com/SpinVFX/spin_nuke_gizmos/?ref=keheka.com).

- Paint or smear the alpha. Use in moderation, though, because any changes upstream can easily break the paint work.

[![](https://www.keheka.com/content/images/2023/10/Key_Alpha_Treatment.png)](https://www.keheka.com/content/images/2023/10/Key_Alpha_Treatment.png)

_Treating the alpha._

  

> [!important] For readability, I didn’t connect masks in the example above. However, try
> 
>   
> to be more of a sniper rifle than a shotgun when you perform any alpha  
> treatments. You don’t want to erode the entire alpha if you only really  
> need to erode in a small section, for example. So please mask the  
> treatments as needed.

  

### Using Garbage Mattes

It’s not always necessary to paint out or patch over unwanted things like  
set equipment in a plate and avoid it being picked up in the key.

Often, you can let the keyer include the equipment in the output alpha  
channel, and then remove it after. Especially, if the equipment doesn't  
intersect with your subject. Simply cut out any unwanted objects from  
the alpha using a garbage matte.

Roto out a [c-stand](https://en.m.wikipedia.org/wiki/C-stand?ref=keheka.com),  
for example. (Today I learned the C stands for Century). If the stand  
is off to the side by itself, you can just throw a big roto shape around  
it and stencil it out.

Or, sometimes easier, do a rough roto around the parts that you want to _keep_ and use it to mask your alpha:

[![](https://www.keheka.com/content/images/2023/10/Garbage_Matte_Roto.png)](https://www.keheka.com/content/images/2023/10/Garbage_Matte_Roto.png)

_Making a garbage matte._

  

> [!important] Make sure to soften your garbage matte to avoid introducing sharp matte
> 
>   
> lines to your composite, for example if there is subtle transparent  
> glows or edges being cut off.

  

[![](https://www.keheka.com/content/images/2023/10/Full_Setup_Garbage_Matte.png)](https://www.keheka.com/content/images/2023/10/Full_Setup_Garbage_Matte.png)

[![](https://www.keheka.com/content/images/2023/10/Garbage_Matte.png)](https://www.keheka.com/content/images/2023/10/Garbage_Matte.png)

_Garbage matting using both the mask and stencil options._

Here, you can also add any holdouts, such as roto for objects that should be  
in the foreground, or for example roto to remove webbing from a sticky  
key.

  

> [!important] You can also garbage matte each keyer individually if needed.

  

If your blue/green screen doesn’t have equipment in front of it, but  
you’re getting some noise in the alpha outside of the subject (for  
example due to lighting differences on the screen), you can make a  
procedural garbage matte to remove the noise:

Using for example the Primatte keyer, make a very hard matte which is completely  
black outside of the subject and completely white on the subject. Then,  
dilate and soften this matte, and Merge (mask) it with your main alpha  
to garbage matte out the noise.

You can also _fill in_  
holes in the alpha using the same technique, by instead eroding the  
hard alpha, and merging it with the main alpha using a Merge (over).  
This is useful for things like a reflective badge on the subject's  
shirt, for example.

And you can also just merge over a rough roto to fill in any gaps:

[![](https://www.keheka.com/content/images/2023/10/Full_Setup_Roto.png)](https://www.keheka.com/content/images/2023/10/Full_Setup_Roto.png)

[![](https://www.keheka.com/content/images/2023/10/Roto_Fill.png)](https://www.keheka.com/content/images/2023/10/Roto_Fill.png)

_Adding supplementary roto to the key._

  

> [!important] Make sure the fill roto doesn’t drift outside of the core of your subject.

  

### Clamping The Alpha

Finally, clamp the alpha to a range of 0-1 as the last thing in the alpha branch.

The various merge operations and other algorithms in Nuke expect alpha  
values between 0 and 1 for the maths behind them to work properly.

  

[![](https://www.keheka.com/content/images/2023/10/Full_Setup_Clamp.png)](https://www.keheka.com/content/images/2023/10/Full_Setup_Clamp.png)

[![](https://www.keheka.com/content/images/2023/10/Clamp_Alpha.png)](https://www.keheka.com/content/images/2023/10/Clamp_Alpha.png)

_Clamping the alpha._

In the cases where you are seeing values outside of the 0-1 range in your alpha, do **look for the root cause first**.

If you are adding mattes together using a Merge (Plus) and getting values  
above 1 that way, you should go back and fix the root cause (changing  
the Merge to an Over) instead of relying on the Clamp. That’s because  
merging mattes together with a Plus often creates unwanted hard edges  
around your subject, and clamping won’t fix that.

You may notice some alpha values going into the negatives, or just above 1 when  
using the Erode nodes. The Clamp node is more a safeguard to catch any  
such values caused by filtering.

Next, let's move on to the colour branch.