---
{"dg-publish":true,"permalink":"/nuke-kb/40-courses/keheka-courses/how-to-build-a-show-specific-grain-tool-in-nuke/"}
---

By building a show specific grain tool you  
create a plug-and-play Group or Gizmo in Nuke for the compositors on the  
show to use on every shot. With little to no adjustment they should be  
able to get a perfect grain match to their scans. Having a show specific  
grain tool is a great and easy way of **keeping the grain consistent** across the project, and **saving artists time**.

There are excellent tools out there such as [DasGrain](https://www.nukepedia.com/gizmos/other/dasgrain)  
which can do a great job of matching the grain. Compared to a custom  
grain tool, however, they are more complicated, prone to user error, and  
require third party instals. Making a show specific grain tool **makes it as easy as possible** for all the artists on the show to get the grain right, every time.

# Part 1 | Capturing The Necessary Material

Acquiring the source material needed for the  
grain tool starts on the film set. The VFX Supervisor and crew will be  
gathering all sorts of data and references during the shoot, and a few  
of them are important for building the grain tool:

### Grain Plates

To make the tool you will usually receive one or more **18% Grey card**  
grain plates shot on set. These grain plates will have been filmed  
using the same lenses, camera sensors, and ISO/ASA settings as the  
actual shots you will be working with on the show, to record the  
individual grain profiles.

[![](https://www.keheka.com/content/images/2022/10/Calibrite_CCGB.png)](https://www.keheka.com/content/images/2022/10/Calibrite_CCGB.png)

Example 18% Grey card.

If prepared and set up correctly, the grain plates should provide a perfect match to the grain in your shots.

Ideally, the 18% Grey card should be:

- **Clean**. The 18% Grey card should be free from dirt, smudges, or anything that would damage the integrity of the reference grey tone.

- **Lit as evenly as possible**. There should be no, or _very_ minimal, shadowing on the 18% Grey card, and no changing lighting such  
    as flickering. That way, you will be able to extract only the pure grain without any contaminating gradients or grade changes tagging along.

- **Covering the entire frame**. The 18% Grey card should be framed such that it is filling the entire  
    frame or working resolution. That way, you will only have pure grain on a clean grey background without any equipment, markers, or set to remove.

- **Correctly exposed**. When filming the 18% Grey card, the exposure should be adjusted so that it is correct when the aperture dial is set to the middle of its range. This will match up to the shooting of the companion charts in the next  
    section further below.

- **Filmed out of focus**. There should be no visible texture or detail present to interfere with  
    or pollute the grain. The method for extracting the grain is sensitive  
    and will pick up contaminations easily. That includes a piece of dust  
    flying across the frame. You do not want your grain tool to introduce  
    phantom edges or artefacts to your shots. Having a clean, out of focus  
    18% Grey card will save you some clean up work.

- **Filmed at the right conditions for at least 50 frames**. A good rule of thumb is that around 50 usable frames should be the  
    minimum per grain plate. That way, the grain plates can be looped and  
    used for adding grain to shots of any length, without obvious pattern  
    repetition. **Remember, the lens focusing or the camera itself may take some time to settle**, and so it's often necessary to shoot 100-150 frames or more to end up with 50 usable frames.

- **Filmed with every camera sensor and/or ISO/ASA setting** that is used on the show. You will be making grain setups for all the  
    active camera sensors and/or ISO/ASA settings to correctly match each  
    grain profile.

[![](https://www.keheka.com/content/images/2022/10/Grey_Card_Grain.jpg)](https://www.keheka.com/content/images/2022/10/Grey_Card_Grain.jpg)

One example frame from a clean and evenly  
lit, correctly exposed and out of focus 18% Grey card grain plate which  
is filling the entire frame, providing a perfect grain sample.

### Companion Charts

Additionally, for each grain plate, the crew on set will film companion charts: a **Grey steps card** (palette of standardised greyscales) and a **Macbeth chart** (palette of standardised colours and greyscales).

[![](https://www.keheka.com/content/images/2022/10/Sekonic_Exposure_Profile_Target.png)](https://www.keheka.com/content/images/2022/10/Sekonic_Exposure_Profile_Target.png)

Example Grey steps card.

[![](https://www.keheka.com/content/images/2022/10/Xrite_Macbeth_Chart.png)](https://www.keheka.com/content/images/2022/10/Xrite_Macbeth_Chart.png)

Example Macbeth chart.

These will be filmed using **the exact same settings and method** as described above for the 18% Grey card, except they don't have to fill the frame. And additionally, they will be **shot at multiple exposures**, as per the following:

Like before, the exposure should first be  
adjusted so that it is correct when the aperture dial is set to the  
middle of its range.

Then, the companion charts should be shot  
with the aperture dial set to the following five different positions for  
at least two seconds each time, so that you get a full range of  
exposures in one take:

1. The smallest aperture the lens can achieve.

1. Halfway (approximately) between the smallest aperture and the correct exposure.

1. The correct exposure.

1. Halfway (approximately) between the largest aperture and the correct exposure.

1. The largest aperture the lens can achieve.

💡 Remember, this should be done for each camera sensor and/or ISO/ASA setting used.

The reason the charts are filmed at five  
exposures is to get references for how the camera sensors react to  
different amounts of light from one extreme end to the other, and  
consequently how the grain behaves under these differing conditions.

These companion charts will be of great help when calibrating your grain tool. More on that later.

### There Is Some Wiggle Room

All the above are _ideal_  
conditions. If the on set VFX supervisor and crew managed to capture  
all of it like described above, they did a great job. Filming on set is  
usually quite hectic with short timelines to get things done, and no or  
few second attempts. So, there is a chance you might not receive  
everything you would like. Or, you might not receive the material in  
perfect condition.

However, don’t worry if the 18% Grey card  
doesn’t fill the entire frame; if there are some artefacts in the plate;  
or if they didn’t shoot the companion charts. **You can still make it work**. What you absolutely need are clean grain plates filmed out of focus, though – no compromising on that one.

# Part 2 | Building The Grain Tool

After the shoot is over and all the material  
described above has been transferred to your VFX studio, you can start  
the process of preparing the material and building the actual grain  
tool.

‼️ Going forward, I will be using an **imaginary example project** where they filmed using **two different cameras** with a total of **five ISO settings**. And so we will be working with five grain plates and five companion charts plates. **Your project may differ from this**.  
Just add, change, or remove camera types and ISO/ASA settings as needed  
in the following build. All of the information and the workflow will  
still apply.

### Locking Down The Colour Pipeline

First of all, make sure the correct colour  
pipeline has been set up for the show. Usually, you will already have  
been involved with your pipeline team in performing a **colour loop test**  
for the client, confirming that everything has been set up correctly.  
This process is quite different from studio to studio, and project to  
project, so make sure you thoroughly **match all the client specifications** and get a sign off on the colour loop test.

You will be making very specific and delicate  
adjustments in the grain tool, and so if the colour pipeline is not  
locked down the work might go to waste. Once that’s all good to go,  
though, we’re ready to move on

### Preparing The Grain Plates

Next, let’s set up the 18% Grey card grain plates so that the tool you will build can use them:

1. **Start a fresh Nuke script**. When the grain plates were ingested, a shot structure should have been set up for them which you can work in.

1. **Set the project resolution** to the show’s resolution. You should work with and be able to output  
    grain to fill the entire frame of the shots that you will be compositing later on.

1. **Import the grain plates** and sort them sensibly. In the imaginary example project, we have two  
    camera sensor types and a total of five ISO settings. And so, let’s sort the plates by camera sensor type, and then further by ISO value. Please sort your own plates accordingly.

1. **Look for usable frame ranges**. Play through the grain plates and find the frames where the grain  
    samples are as close to ideal as possible. The plates may include a  
    slate at the beginning and then a period for the exposure to be set, the lens to go out of focus, and the camera to settle. After all that,  
    hopefully you will be left with **at least 50 usable frames** for each plate. Make a note of the usable frame range for each plate to keep track, for example using FrameRange nodes.

1. **Clean up any artefacts** within the usable frame ranges, for example a dark speck of dust that  
    flies by on a few frames, or a smudge on the Grey card. Usually, you can **use a transform offset or a time offset** to cover up these artefacts. Use whole numbers in your Transform or set the filter to _impulse_ to **avoid softening your grain**. If you have longer grain plates, you may be able to use a different  
    frame range altogether to avoid some or all of the clean up work.

1. **Fill the frame with grain**. If the Grey card doesn’t fill the frame or working resolution, crop the usable area of the grey card, and transform offset it around to fill  
    out the frame. Again, use whole numbers in the Crop and Transform nodes, or the _impulse_ filter. The entire frame should be filled with only grey colour and grain. To avoid getting pattern repetitions, **apply different time offsets** to each of the duplicates and offset by a few frames. You can also  
    mirror transform the patches if there is a slight brightness gradient  
    and you are getting a visible seam between them. Make sure to do this  
    for any grain plate where it’s needed.

1. **Precomp the prepared grain plates**. Render out the usable frames for each grain plate. (Again, a minimum of about 50 frames – but 100 to 150 frames are plenty should you have  
    longer plates). **The grain tool should be responsive and efficient** and not need to calculate any clean up work when you use it. When  
    rendering the plates, use for example the EXR file format at 16-bit, to  
    retain accurate colour values.

1. **Publish the precomped renders** or place them in a safe folder so that they do not accidentally get overwritten or deleted.

### Setting Up The Grain Tool

With the grain plates all prepared and ready to go, we can start building the grain tool.

### Looping The Grain Plates

In a fresh Nuke script, import the prepared grain plates you just published. Sort them in the same way again, and then **loop the renders**.  
You can loop them in the Read nodes themselves, or use a Retime node  
for looping. I prefer the second option as it more explicitly shows what  
is happening in the script:

Add a Retime node after the grain plate and then type and lock in the first and last frames of the input using the _input.first_ and _input.last_ fields, and the _input.first_lock_ and _input.last_lock_ checkboxes. Then, select _loop_ in the _before_ and _after_ drop-down menus.

[![](https://www.keheka.com/content/images/2022/10/Retime_Looping.png)](https://www.keheka.com/content/images/2022/10/Retime_Looping.png)

Example of looping frames 1100-1200 of a grain plate using a Retime node.

Rinse and repeat for all the grain plates.  
You should now have endlessly looping, clean grain plates which can be  
used for shots with any frame range.

Then, connect them all up to a Switch node, in ascending order. Rename the Switch node to _Switch_Grain_Plates_. We will be using it later on to help select grain profiles in the tool.

In the image below I have connected the plates to the Switch_Output node from right (input 0) to left (input 4):

[![](https://www.keheka.com/content/images/2022/10/Looped_Grain_Plates.png)](https://www.keheka.com/content/images/2022/10/Looped_Grain_Plates.png)

Example setup of the cleaned up, precomped  
grain plates organised by camera type and ISO settings, then  
individually looped based on their frame ranges, and finally connected  
to a Switch node, in ascending order.

However, simply adding these grain plates to  
the end of your composites will not be sufficient. From the grain plates  
you have got the grain shape, size, and frequency for free. Yet, you  
need to set up a way for your grain to interact with your composite in a  
natural way, accurately responding to light and darkness – just like  
the original grain does in your shot scans.

💡 Grain is simply sensor noise in the digital camera, and has a  
proportional relationship to the amount of light that reaches the  
sensor. More light = less visible grain, and vice versa. Each camera  
setup has a specific grain profile – a specific grain response to  
different amounts of light – which you have to imitate in order to match  
your grain to the scan.

### Flattening The Composite

Currently, the grain is completely flat  
because it was shot on an evenly lit 18% Grey card. In order to tell  
your flat grain how to react correctly to your composite – where the  
grain should be stronger or weaker – you can use exactly that; your  
composite.

However, you need to flatten the values in  
the composite so that it lives in the same world as your flat grain and  
can interact with it correctly:

1. Create an Input node and name it _Inputimg_. This Input node will pipe through your composite when we later wrap the setup up into a Group node. Note: Anything after _Input_ in the Input node’s name corresponds to what that particular input pipe on the Group node will be labelled as. In this case, _img_. You can also simply name the node _img_.

1. From the Input node, branch off a pipe  
    to the side, add a Remove (keep) node, and keep only the RGB channels.  
    We will only be working on the RGB channels, before adding them back  
    into the main pipe later on, so we can remove all other channels in this branch to **keep the grain tool calculating as efficiently as possible**.

1. Create one ColorLookup node for each of  
    your grain plates and connect the ColorLookup nodes to the Remove (keep) node. In our example project, we need to create five of them. **Label the ColorLookup nodes** so that you know which one corresponds to which grain plate, and **sort them in the exact same way** that you sorted your grain plates.

1. Connect the ColorLookup nodes to another Switch node, **in exactly the same order** as you did for your grain plates. Name the Switch node _Switch_Grain_Response_. When you choose a grain profile in the tool later on, this Switch node will be selecting the correct grain response.

[![](https://www.keheka.com/content/images/2022/10/Flatten_the_Image.png)](https://www.keheka.com/content/images/2022/10/Flatten_the_Image.png)

Setup for flattening the composite.

You will use the curves in the ColorLookup  
node to flatten the composite and fine-tune your grain response. But  
before you start this calibration, let’s finish setting up the tool.

### Extracting The Grain

We need to quickly return to the precomped  
grain plates because we left out one part on purpose earlier. They still  
have the grey coloured background in them. This is just a precaution so  
that we can make a drop-down menu in the tool later on, to switch the  
view to the grain plates for easy troubleshooting.

Let’s remove that grey background and extract only the actual grain values:

1. Add a Blur node under the Switch_Grain_Plates node, and name it _Blur_Grain_. Set it to a value just high enough to completely blur out all the  
    grain, leaving you with only the smooth grey colour. Usually a Blur  
    value of around 10 or more is needed. **Keep the Blur value as low as you can** while still killing all the grain to avoid introducing a brightness  
    gradient to your composite when you add the grain to it later. (For  
    example, if there is a shadow or lighting difference across the 18% Grey card in your grain plate).

1. Then, subtract the result from the grain plate: Merge the blurred grain plate (A) using a Merge (from) with the  
    non-blurred grain plate (B). This will remove all the grey colour and  
    you will be left with what looks like a black image. Don’t worry, there  
    are actually still values there. Gamma up your Viewer and you should be  
    able to see them. These values represent the isolated grain which is  
    what you will use going forward.

[![](https://www.keheka.com/content/images/2022/10/Extract_Grain.png)](https://www.keheka.com/content/images/2022/10/Extract_Grain.png)

Setup for extracting the grain.

### Finishing Up The Basic Structure For The Tool

With the isolated grain and the setup for  
flattening the composite in place, you can connect the two and finish up  
the basic structure for the tool:

1. Merge (multiply) the isolated grain (A) with the (soon to be) flattened composite (B).

1. Add a Remove (keep) node at the end, and set it to only keep the RGB channels. You do not want to change the  
    alpha of the original input composite in the next step.

1. Merge (plus) the Remove (keep) node (A) with the original composite (B), and the basic structure is complete.

[![](https://www.keheka.com/content/images/2022/10/Basic_Structure.png)](https://www.keheka.com/content/images/2022/10/Basic_Structure.png)

Finishing up the basic structure.

Next, let’s add more controls for manual user  
adjustments, as well as a user interface to the tool before we start  
calibrating it.

### Adding Controls

As for the **basic controls**, the user should be able to:

- Choose between the various grain profiles in a drop-down selection.

- Manually adjust the strength of the grain per channel.

- Time offset the grain if needed.

The first point we have got covered for now,  
because we already made the Switch_Grain_Plates and  
Switch_Grain_Response nodes. We will connect these nodes up to our user  
interface a little bit later. (And add a third Switch, too).

To adjust the strength of the grain per  
channel, let’s add a Multiply node right after the point where we Merge  
(multiply) our flattened image with the flat grain. Name the Multiply  
node _Multiply_Grain_.

And, to time offset the grain, let’s add a TimeOffset node right after the Switch_Grain_Plates node, and name it _TimeOffset_Grain_.

For the more **advanced/troubleshooting controls**, the user should be able to:

- Adjust the grain response curves.

- Grade the grain plates.

- Adjust the grain saturation.

- Reformat the grain.

- Choose between viewing different outputs: the regrained comp, the adapted grain, and the grain plate.

The first point we have again got covered  
because we already set up all the ColorLookup nodes. We will link these  
nodes up to our user interface a little bit later.

To grade the grain plates, let’s place an Add  
(math) node after where we extracted the grain. This can be useful if  
there is a slight colour tint in the grain plates which you want to  
remove. Name the Add node _Add_Grain_.

To adjust the grain’s saturation, let’s add a  
Saturation node after our Multiply_Grain node. This can be helpful when  
adjusting the look of the grain to get it just right. Name the  
Saturation node _Saturation_Grain_.

To reformat the grain, add a Reformat node  
after the TimeOffset_Grain node. This is useful if you have for example  
6K plates reformatted down to 4K. You would render the grain plates in  
their native resolution so that, if needed, you could regrain at that  
resolution. By default, the Reformat would be set to the show  
resolution, e.g. 4K. Name the Reformat node _Reformat_Grain_.

To choose between viewing the different outputs, let’s create a Switch node and name it _Switch_Output_.  
Then, connect the 0 input to the regrained composite, the 1 input to  
the adapted grain, and the 2 input to the grain plate, after the  
Reformat_Grain node. We will connect this Switch_Output node to our user  
interface in a moment.

Additionally, if your scans come with black bars at the top and bottom, you should **set up the correct gate Crops** to avoid adding grain to the black bars:

After the Saturation_Grain node, add as many  
Crop nodes as needed with the correct crops for each grain setup, and  
sort them in the exact same order as you did with the grain plates and  
grain response curves. Then, connect the Crops in the exact same order  
as before to a Switch node and name it _Switch_Crops_.

❗ You may have fewer Crops set up than there are grain profiles. In  
this example I have two; one for each camera type. If so, connect the  
Switch_Crops node’s pipes multiple times to the same Crop node. So in my  
case, inputs 0, 1 and 2 connect to the first Crop (camera type A), and  
inputs 3 and 4 connect to the second Crop (camera type B). That will  
ensure the correct Crop gets selected with each grain profile.

[![](https://www.keheka.com/content/images/2022/10/Full_Setup.png)](https://www.keheka.com/content/images/2022/10/Full_Setup.png)

Adding the final controls to the setup.

### Grouping All The Nodes

With a working structure in place for the tool, let’s wrap everything up into a Group node:

Select all the nodes in the script, and hit  
Ctrl (Cmd) + G. This will wrap the nodes inside a Group which you can  
enter by selecting the Group node and hitting Ctrl (Cmd) + Enter.

You already created an Input node, so the  
Group node already knows what is coming in, and where. But when you  
create the Group you may have to select what it should output. Choose  
the Switch_Output node as the output for the Group.

[![](https://www.keheka.com/content/images/2022/10/Group_Output.png)](https://www.keheka.com/content/images/2022/10/Group_Output.png)

Selecting the Switch_Output node as the group’s output.

Now that our tool is wrapped inside a Group node, we can give it a name. Usually, I will name it _SHOW_Grain_,  
where SHOW is replaced by the show’s code name. You can name it  
anything you want as long as it is sensible and immediately understood  
by your artists. For this example, I will leave it as SHOW_Grain.

You can also give your node a colour if you want. For this example, I will choose black.

Next, let’s create a proper user interface for our artists.

### Building A User Interface

A good user interface should give the artists  
all the controls they need right at their fingertips, and hide all the  
behind-the-scenes clutter that they do not need to think about.

I prefer to create tabs in the tool’s  
interface, so that I can keep only the most important controls used in  
day-to-day compositing on the ‘front page’ of the tool. Then, put the  
advanced/troubleshooting controls on a second tab to avoid cluttering  
the user interface.

Let’s make the ‘front page’:

### Output Selection

Right off the bat, I like having an _output_  
selection drop-down menu. Right-click on the Group node’s properties  
panel and select Manage User Knobs… → Add → Pulldown Choice..

Name and label it _output_, and under Menu Items add three lines:

_regrained comp_

_adapted grain_

_grain plate_

[![](https://www.keheka.com/content/images/2022/10/Pulldown_Choice.png)](https://www.keheka.com/content/images/2022/10/Pulldown_Choice.png)

Adding a drop-down menu for selecting the output.

Next, jump into the Group and open up the properties for the Switch_Output node. In the _which_ knob, add the expression **output** which will link the drop-down menu to the Switch_Output node so you can control it from the user interface.

[![](https://www.keheka.com/content/images/2022/10/Switch_Output_Expression.png)](https://www.keheka.com/content/images/2022/10/Switch_Output_Expression.png)

Linking the Switch_Output node to the _output_ drop-down menu in our user interface.

### Grain Profile Selection

Next up, let’s add another drop-down menu for selecting the grain profile:

Right-click on the Group node’s properties panel and select Manage User Knobs… → Add → Pulldown Choice..

Name it _grainProfile_ and label it _grain profile_. Under Menu Items, add as many lines as you have grain profiles – in our example, five:

_camera type a - iso 400_

_camera type a - iso 800_

_camera type a - iso 1200_

_camera type b - iso 800_

_camera type b - iso 1600_

(You would name yours for example _Arri Alexa 65 - ASA 800_).

Then, jump back into the Group and add the expression **grainProfile** to the _which_  
knobs of the Switch_Grain_Plates, Switch_Grain_Response, and  
Switch_Crops nodes. That will link them all up to this new drop-down  
menu.

💡 This is why it was so important to sort and connect the nodes up to  
these switches in the exact same order, so that the correct ones get  
linked to each profile.

### Time Offset

Next, let’s add the time offset controls:

Right-click on the Group node’s properties  
panel, select Manage User Knobs… → Pick → TimeOffset_Grain → TimeOffset →  
time offset (frames).

### Multiply

To add the Multiply controls, right-click on  
the Group node’s properties panel and select Manage User Knobs… → Pick →  
Multiply_Grain → Multiply → value. Then, click on Edit and change the  
label from _value_ to _multiply_, to be more descriptive. I also like to click on the _4_ symbol to open up the individual channel values.

Next up, let’s make the _advanced_ tab:

Right-click on the Group node’s properties panel and select Manage User Knobs… → Add → Tab. Name and label it _advanced_.

Then, let’s add the advanced/troubleshooting controls in this tab.

### Grain Response Curves

Add all the grain response curves by  
right-clicking on the Group node’s properties panel and selecting Manage  
User Knobs… → Pick, and then selecting the _lut_  
property in the first ColorLookup node. Then repeat for all the  
ColorLookup nodes, in order. Make sure to hit Edit on each one and tick _Start new line_.

These grain response curves may take up a lot  
of user interface space, so you can add each one into its own  
collapsible group, by selecting it and going to Add → Group.

### Grade the Grain Plates

Next up, right-click on the Group node’s  
properties panel and select Manage User Knobs… → Pick → Add_Grain → Add →  
value. Then, click on Edit and change the label from _value_ to _grade grain plate_, to be more descriptive. Again, I like to click on the _4_ symbol to open up the individual channel values.

### Saturation

To add the Saturation controls, select Manage  
User Knobs… → Pick → Saturation_Grain → Saturation → saturation. Then,  
click on Edit and change the label from _saturation_ to _grain saturation_, to be more descriptive.

### Reformat

To add the Reformat controls, select Manage  
User Knobs… → Pick → Reformat_Grain → Reformat → output format. Change  
the label from _output format_ to _reformat grain_, to be more descriptive. Then, add the _filter_ property from the Reformat node as well.

### Tidy Up

Lastly, tidy up by adding some Divider Lines and splitting up the different sections.

Once you’re done, it might look something like this:

[![](https://www.keheka.com/content/images/2022/10/Show_Grain_UI_1.png)](https://www.keheka.com/content/images/2022/10/Show_Grain_UI_1.png)

The main tab of the user interface.

[![](https://www.keheka.com/content/images/2022/10/Show_Grain_UI_2.png)](https://www.keheka.com/content/images/2022/10/Show_Grain_UI_2.png)

The advanced tab of the user interface.

### Label the Node

If you want, you can add the expression **[value output]** to the Grain tool’s label in order to identify from the node graph what the tool is outputting.

# Part 3 | Calibrating The Grain Tool

After all that, the entire structure and user  
interface is finally complete. Now, the tool is ready to be calibrated  
for each grain profile.

### Preparing The Companion Charts

Start by importing and sorting the companion charts by camera sensor type and/or ISO/ASA values.

Then, denoise the companion charts as accurately as possible. I recommend using [Neat Video’s Reduce Noise plug-in](https://www.neatvideo.com/).

If you don’t have any companion charts, you can use representative shot scans instead. (And even if you do, _also_  
use representative shot scans in the calibration process). Just make  
sure the scans have as wide a range of luminance and colour values as  
possible, and represent each camera sensor type and/or ISO/ASA values.

To calibrate the tool you will be adding _your_ grain to the denoised companion charts (and/or representative denoised scans), and comparing that to the _originals_.

A good way to do that is by keymixing between  
the denoised chart with your grain tool applied (A) and the original  
chart (B), using a Checkerboard with a pure black and pure white alpha  
as the mask input. That way, you can see _your_ grain right next to the _original_ grain all throughout the image, and easily see where it is matching or not. **A checkerboard pattern will appear where it is not matching**.

Also, add an Invert (alpha) node to the  
Checkerboard, and enable/disable as needed to quickly swap the original  
and regrained areas of the chart around.

[![](https://www.keheka.com/content/images/2022/10/Calibration_Helper_Setup.png)](https://www.keheka.com/content/images/2022/10/Calibration_Helper_Setup.png)

Setup to assist with the calibration.

[![](https://www.keheka.com/content/images/2022/10/Checkerboard.png)](https://www.keheka.com/content/images/2022/10/Checkerboard.png)

The Checkerboard settings. Notice the value of 0 in the _color 0_ and _color 2_ alphas, and the value of 1 in the _color 1_ and _color 3_ alphas. Adjust the _format_ and _size_ as needed for your project.

### Adjusting The Response Curves

Next, you will be adjusting the response  
curves – per channel, per luminance level, from shadows to midtones to  
highlights, for each of the different grain plates.

The goal is to make the checkerboard pattern disappear. Let’s start by looking at the red channel only.

In the Viewer, colour pick the values of a  
square in the checkerboard pattern that you want to adjust, and look at  
the ColorLookup’s _lut_ window for  
the red channel. There are vertical bars for each channel, indicating  
where on the curve the colour picked values from the Viewer are located.  
Find where the red bar is.

Hit Ctrl (Cmd) + Alt and Left-click on the  
curve where the red vertical bar is, to add a point there. Then, raise  
or lower that point to adjust the grain response until the square  
completely blends in with the surrounding, original squares, and the  
checkerboard pattern disappears.

Calibrating the grain response for a section of the red channel.

💡 Gently flatten out the curves as needed in the sub-black and  
super-white areas so you don’t get crazy negative or super high values  
in the grain which do not match the grain values in the original scans.  
Gain/gamma the Viewer very high up and down to check that your grain is  
behaving as expected, and adjust the curves in those areas as necessary.

Keep doing this until all the squares match  
up to the original squares in the red channel. Remember to check using  
all the different exposures of the charts. And, invert the Checkerboard  
and check that the same is true for the other half. Then, repeat for the  
green and blue channels.

[![](https://www.keheka.com/content/images/2022/10/Grain_Response_Example.png)](https://www.keheka.com/content/images/2022/10/Grain_Response_Example.png)

Example response curves.

💡 The points on the response curve should always increase in value on  
the Y axis, going from left to right on the X axis. The slope may be  
steeper or more gradual in different areas, but should not move  
negatively in Y.

Once all the squares are matching in all the  
channels, the calibration for that grain profile should be pretty much  
complete. Swap in some different scans with the same camera sensor  
and/or ISO/ASA settings to test your calibration. See if the  
checkerboard pattern still disappears. You might have to make slight  
adjustments but they should be very minor.

Once you are happy with the grain profile,  
repeat all of the above for each of the other grain plates until your  
grain tool is complete.

If you like, you can convert your tool from a [Group to a Gizmo](https://learn.foundry.com/nuke/content/comp_environment/configuring_nuke/creating_sourcing_gizmos.html).

# Part 4 | Using The Grain Tool In Practice

Using the tool once it’s calibrated is super  
easy. Start by connecting the tool to the end of your composite in a new  
branch, and Keymix it (A) with the main pipe (B) using your grain matte  
(mask).

Next, select the grain profile that you want by using the _grain profile_  
drop-down menu. You can check the shot’s camera sheet and/or ShotGrid  
page to find the information you need to make the selection. Sometimes,  
you can even tell by the resolution of the scan – if each scan type is  
unique.

Once you have selected the correct grain profile, you would typically get a perfect match and your grain work is complete.

However, do nudge the multiply values to  
adjust if there is a slight mismatch in grain strength on a particular  
shot. You can also time offset the grain if needed to make the grain  
more unique between shots.

### Troubleshooting

At the beginning of a show when you create  
the tool it is very likely that you will not have received all the shot  
scans yet. You may later receive a scan which contains outlier values  
that have not yet been accurately calibrated for. This is more common in  
the cases where you don't receive all of the material, such as the  
multi-exposure companion charts. If so, just adjust the response curves  
as needed and update the tool.

If you find that the grain tool is introducing phantom edges or artefacts to your shots, change the _output_ drop-down menu to view the grain plate. Usually, you’ll find that there is a little clean up work still left to do.

If you need to fix something in the adapted grain itself, change the _output_  
drop-down menu to view the adapted grain, make your changes, and then  
Merge (plus) the result with your composite and mask it by your grain  
matte.

Usually, the only reasons the grain won’t be a  
near 100% match using this tool will be because the artist picked the  
wrong grain profile, or a shot’s scan was filmed with a camera or  
ISO/ASA which has not been profiled. In the first case it’s an easy fix;  
just swap the grain profile. (You _could_  
also set up expressions that pick the right profile automatically for  
you based on metadata or format size). In the second case, unless you  
have grain plates and are able to set up a new grain profile, I would  
use DasGrain or other grain tools for those particular shots.

I have found that in the overwhelming  
majority of cases, the grain tool will do an excellent job of matching  
the grain with basically no effort required.

### Re-Using The Tool

Now that you have built the tool once, it can  
easily be re-used on the next project, and the projects after that. All  
the structures are in place; you will just have to swap out the  
precomped grain plates and adjust the grain response curves, update the  
Crops and change the drop-down selection names in the tool to reflect  
the new camera sensor types and/or ISO/ASA values.

Your putting in this relatively small effort  
up front on a project makes the artists’ lives a lot easier during their  
time on the project – which could be many months. _And_,  
as a bonus to you and the Compositing Leads, it makes tech checking  
easier since you very rarely will have to kick back a shot due to grain  
issues.

I think it's worth it.