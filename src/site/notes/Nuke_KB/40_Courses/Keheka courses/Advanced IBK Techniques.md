---
{"dg-publish":true,"permalink":"/nuke-kb/40-courses/keheka-courses/advanced-ibk-techniques/"}
---

# Unlock The IBKs Potential

The IBK (Image-Based Keyer) is one of my all-time favourite keyers.

It's very robust and consistently produces  
excellent results – even with difficult, unevenly lit blue/green  
screens. However, limiting yourself to using only the IBKColour and  
IBKGizmo node pair is a waste of the keyer's potential.

In this tutorial, I'll show you:

- **How the IBK works and how it's different from other keyers:**
    
    The IBK takes a whole other approach to keying than typical keyers, which has major benefits that we'll look into.
    

- **The hidden features of the IBK and why they are useful:**
    
    The  
    IBK is unique in that you can actually look under the hood and see how  
    it's made. And inside, there are additional options hidden away which  
    we'll explore.
    

- **Advanced IBK setups that will help you extract a detailed matte when keying:**
    
    These  
    setups will show you ways of thinking outside of the box and combining  
    other tools to enhance the off-the-shelf IBK nodes and pull a better  
    key.
    

- **My go-to, production proven approach to keying blue/green screens:**
    
    A detailed step-by-step guide that explains the advanced techniques I use in the majority of my keying work.
    

# How The IBK Works

Oscar-winning Paul Lambert [created the IBK](https://beforesandafters.com/2022/03/25/how-dune-vfx-supervisor-paul-lambert-invented-nukes-ibk-keyer/) almost 20 years ago, and it remains pretty much unchanged in Nuke today.

Instead of using a single colour picker to  
drive the key (like many keyers do), the IBK uses an input image – a  
clean plate with just the colour variations of the background.

This method is especially effective for  
dealing with unevenly lit blue/green screens, where there is a variation  
in the screen colour. Which, let’s face it, is most of the time.

The IBK system consists of two nodes in Nuke: the [IBKGizmo](https://learn.foundry.com/nuke/content/reference_guide/keyer_nodes/ibkgizmo.html) and the [IBKColour](https://learn.foundry.com/nuke/content/reference_guide/keyer_nodes/ibkcolor.html).

### The IBKGizmo

The IBKGizmo is the node that actually pulls  
the key. It has three inputs (and a hidden 4th one, which we'll talk  
about later in this tutorial):

- **FG**: The foreground, i.e. the blue/green screen plate.

- **BG**: The background you want to use as a replacement for the blue/green screen.
    
    (The _BG_  
    input is only relevant if you'll be using the RGB output from the  
    IBKGizmo – it’s usually left disconnected as we’re only interested in  
    the alpha).
    

- **C**: The clean plate, i.e. the blue/green screen alone, without the subject.

> [!important]
> 
> 💡 If you’re (very) lucky and receive a clean plate which was filmed  
> specifically for your shot, or you have generated a clean plate in  
> another way, then the IBKGizmo is the only IBK node of the two that  
> you’ll need to make the keyer work.

Let's look at each IBKGizmo setting:

[![](https://www.keheka.com/content/images/2023/05/IBKGizmo_Properties.png)](https://www.keheka.com/content/images/2023/05/IBKGizmo_Properties.png)

The properties panel of the IBKGizmo node.

**Screen type**

Choose between _C-blue_, _C-green_, or _Pick_ in the drop-down menu:

- _C-blue_ will use the image connected to the IBKGizmo's _C_ input for the keying calculation, and set the internal algorithm to pull a key from the blue colours.

- _C-green_ will also use the image connected to the IBKGizmo's _C_ input for the keying calculation, but will set the internal algorithm to pull a key from the green colours.

- _Pick_ will let you choose a custom screen colour for the keying calculation,  
    and will automatically choose the internal keying algorithm based on the values you select.
    
    I typically never use the _Pick_  
    option because it essentially turns the IBK into a standard keyer,  
    losing out on the great benefits of using a clean plate. Instead, I let  
    Keylight and Primatte handle that type of work.
    

**Colour**

If you do select _Pick_ in the _screen type_ drop-down menu, this is where you would choose the screen colour to be used for the keying calculation. If you only use _C-blue_ and _C-green_, the _colour_ option is irrelevant.

**Red weight**

This value determines how the red channel is weighted in the keying calculation.

Blue screens and green screens are never  
purely either blue or green. There will also be some red values in the  
colour. You may have seen blue/green screens with a brown tint before,  
for example.

Depending on the red component of your  
blue/green screen's colour, increase or decrease the red channel’s  
influence on the algorithm by adjusting the _red weight_ slider as needed.

Changing the _red weight_  
will affect the alpha that’s being output, so adjust it to finetune  
your key. It will also impact the RGB colours in the areas where the  
alpha is semi-transparent. (We’re typically not concerned with the RGB  
output of the IBK, though).

**Blue/green weight**

This value determines how the complement  
channel (blue for a green screen, and green for a blue screen) is  
weighted in the keying calculation.

Like mentioned above, blue screens and green  
screens are never purely either blue or green. There will also be some  
green values in a blue screen, and some blue values in a green screen.

Depending on the blue component of your green  
screen's colour, or the green component of your blue screen's colour,  
increase or decrease the blue or green channel’s influence on the  
algorithm by adjusting the _blue/green weight_ slider as needed.

Changing the _blue/green weight_  
will affect the alpha that’s being output, so adjust it to finetune  
your key. It will also impact the RGB colours in the areas where the  
alpha is semi-transparent. (Again, we’re typically not concerned with  
the RGB output of the IBK).

**Luminance match**

When this setting is enabled, the IBK will factor in luminance in the colour difference algorithm.

This helps to capture transparent foreground  
areas that are brighter than the background, firming up the alpha  
channel in the lighter areas. It’s always worth ticking this checkbox to  
see if it gives you a better alpha, and just unticking it again if it  
doesn’t.

**Screen range**

When _luminance match_ is enabled, lower the _screen range_ value until it stops changing the background.

Lowering the value will reduce some of the  
screen area noise, but it can eat into your foreground blacks and harden  
the edges if pushed too far. It’s a fine balance, and usually you won’t  
need to change this setting by much.

**Luminance level**

This setting lets you control the strength of the overall _luminance match_ effect.

A value of 0 means you get the full effect,  
and a value of 1 means it’s fully mixed back. Typically, it's not  
necessary to adjust this value from its default of 0, but it could be  
worth checking if tweaking the value gives you a better result.

**Enable**

Activates the _luminance level_ control above. Normally, this is left unticked, but again, it could be worth playing with the _luminance level_ setting to see if you can get a better result.

**Autolevels**

Enable this setting to reduce hard edges from foreground objects that have saturated colours.

If you enable the _autolevels_, the _weights_  
controls will no longer work as expected. And so it may be useful to  
create multiple IBKGizmos, and Keymix between them when using this  
feature.

**Yellow**

Tick this checkbox to prevent _autolevels_ from changing the saturated yellow colours in your foreground elements.

**Cyan**

Tick this checkbox to prevent _autolevels_ from changing the saturated cyan colours in your foreground elements.

**Magenta**

Tick this checkbox to prevent _autolevels_ from changing the saturated magenta colours in your foreground elements.

**Screen subtraction**

Enabling this checkbox will make the IBK  
subtract the foreground input colours from the keyed RGB – effectively  
despilling the edges and visually improving edge detail.

If disabled, it will just premultiply the original image with the alpha.

Note that this setting has no impact on the alpha, only on the RGB values.

**Use bkg luminance**

Enabling this checkbox will make the brightness of the _BG_ input affect the brightness of the edges of your key, essentially acting like an additive keyer.

Combine this with the _luminance match_ controls for the best results.

This can help with removing fringing artefacts such as light or dark edges in one or more of the colour channels.

If needed, brighten up or darken down the _BG_ input using a Grade node masked by a roto of the affected area. This will offset the effect and help integrate the edges.

Note that this setting has no impact on the alpha, only on the RGB values.

**Use bkg chroma**

Enabling this checkbox will make the colours of the _BG_ input affect the colours of the edges of your key.

Again, combine this with the _luminance match_ controls for the best results.

Like the _use bkg luminance_ feature, this can also help with removing fringing artefacts such as light or dark edges in one or more of the colour channels.

If needed, brighten up or darken down the _BG_ input using a Grade node masked by a roto of the affected area. This will offset the effect and help integrate the edges.

Note that this setting has no impact on the alpha, only on the RGB values.

> [!important]
> 
> 💡 Typically, you’re only using the IBK to generate an alpha, and so  
> the last three features in the IBKGizmo’s properties are irrelevant.  
> Most of the time, you’ll only actually need to adjust the _screen type_, _weights_, and _luminance match_ controls.

### The IBKColour

The IBKColour node is useful for when you don't have a clean plate available for your shot, and you have to create one yourself.

> [!important]
> 
> 💡 You don't _have_ to use the  
> IBKColour to create the clean plate, but that's what it's for, and it is  
> a great option. I'll also show you other methods using different nodes,  
> later on in the tutorial.

The IBKColour node only has a single input, which you connect to your blue/green screen plate.

Like the IBKGizmo, the IBKColour node also  
creates an alpha channel, using the same IBK plugin. But it's important  
to make a distinction here: The (very crunched) alpha generated by the  
IBKColour is used for expanding the blue/green screen only, and isn't  
further used by the IBKGizmo for keying.

**The Unpremult Trick**

The reason the IBKColour node generates an  
alpha is because it needs one for building the clean plate. It creates a  
clean plate by using the classic 'Premult → Blur → Unpremult'  
technique:

First, as mentioned, an alpha is created for the blue or green parts of the image, depending on the _screen type_ setting. It's a Gaussian-filtered high-contrast key, so it's quite harsh, but that's fine for this purpose.

Then, the image is premultiplied with that  
alpha so that anything that's not blue/green gets removed – leaving  
black gaps in the image where the subject was in frame.

Next, both the premultiplied blue/green values _and_  
the alpha are blurred out, to fill in the black gaps with values above  
0. Now, there are faint values of blue/green and alpha where there were  
previously black gaps.

The image is then unpremultiplied, causing the faint blue/green colour values to be restored to normal levels.

> [!important]
> 
> 💡 That's because the Unpremult node is dividing (unpremultiplying)  
> the faint blue/green values by the faint alpha values in the previously  
> black areas. Both of these values are just above 0, e.g. 0.01853... And  
> dividing (even a small) number by another small number (that's above 0  
> and below 1) will result in a bigger number.

With the faint blue/green colours restored to normal levels in the previously black areas, you now have a clean plate.

> [!important]
> 
> 💡 The IBKColour node uses the Unpremult trick twice. First, for a primary colour expansion controlled by the _size_ slider, and then for a secondary colour expansion which fills in any black gaps left behind, controlled by the _patch black_ slider.

**The Properties Of The IBKColour**

Let's look at each IBKColour setting:

[![](https://www.keheka.com/content/images/2023/05/IBKColour_Properties.png)](https://www.keheka.com/content/images/2023/05/IBKColour_Properties.png)

The properties panel of the IBKColour node.

**Screen type**

Choose either _blue_ or _green_ in the drop-down menu, depending on which colour the screen is in your plate.

This will internally switch between the expressions for keying each screen type in the IBK plugin.

**Size**

This setting controls the primary blur size  
for patching the black gaps in the image. Increasing this value will  
expand the blue/green colour of the screen and can kill some noise, but  
at the expense of eating into your edges.

A potential issue with increasing the _size_  
value is that it blurs the entire original blue/green screen plate,  
making the clean plate different to the original plate in all of the  
frame (and not just in the areas where the subject used to be). This  
change will be picked up by the difference matte operation in the  
IBKGizmo, which can introduce unwanted alpha values.

To avoid this, the _size_  
should be set as low as possible. Typically, I will set it to a value  
of 1 to retain the edge detail, before I expand the blue/green screen  
another way, which I will cover later in this tutorial.

**Darks**

This control is grading the input image using the _offset_ knob in a Grade node, _before_ the alpha is generated by the IBKColour.

It has a significant impact on the alpha  
being generated, because you can influence the proportion of each of the  
R, G, and B colours in your image.

Changing these values will have a bigger impact in the _dark_  
areas of the image, and a small value change will go a long way. It's  
just like lifting/crunching the black levels when you're grading a shot:  
only nudge the decimal values.

Adjust the _darks_  
values to remove any remaining edges from the subject on the clean  
plate. It helps to either open up the colour wheel and adjust the  
sliders, or use the up/down, left/right arrow keys to change the values  
in each box.

You want to adjust the values until your  
subject is fully covered by black, or as close as possible, including  
fine edge detail. Push the values until the black patches in the image  
start to 'explode', then pull back a little until just before that  
happens. That's your sweet spot.

**Lights**

This control is grading the input image using the _multiply_ knob in a Grade node, _before_ the alpha is generated by the IBKColour.

Like the _darks_, the _lights_  
also have a significant impact on the alpha being generated, because  
again you can influence the proportion of each of the R, G, and B  
colours in your image.

Changing these values will have a bigger impact in the _light_ areas of the image, and a small value change will also often go a long way here.

Adjust the _lights_  
values to remove any remaining edges from the subject on the clean  
plate. Again, it helps to either open up the colour wheel and adjust the  
sliders, or use the up/down, left/right arrow keys to change the values  
in each box.

Once again, you want to adjust the values  
until your subject is fully covered by black, or as close as possible,  
including fine edge detail. Push the values until the black patches in  
the image start to 'explode', then pull back a little until just before  
that happens. That's your sweet spot.

Oftentimes, it’s enough to only adjust the _darks_, but do check if you get a better result using the _lights_.

> [!important]
> 
> 💡 The artefacts in the image generated by the IBKColour appear as  
> specific colour shades, i.e. light or dark shades of red, green, or  
> blue. To remove them, adjust the corresponding RGB values in the _lights_ and _darks_ controls.

**Erode**

This knob simply erodes the alpha channel  
that is generated by the IBKColour. Because this alpha channel is  
inverted compared to the IBKGizmo's alpha channel, increasing the erode  
value is actually _dilating_ the alpha channel when looking at the IBKGizmo's output.

This can be used to fill in small holes in  
the key and to push the edges out slightly, but should be used sparingly  
to avoid ruining edge detail.

Use the _erode_ feature if you still see traces of the foreground edge colour in the output – _after_ you have adjusted the _darks_ and _lights_ values to get as far as possible with your clean plate.

**Patch black**

This setting controls the secondary blur size for patching the black gaps in the image.

The secondary patch is merged over the image  
after the first patch has been applied (which was previously created by  
adjusting the _size_ value above).  
This secondary patch is premultiplied by the inverted alpha (dilated to  
blend better). That means the primary colour expansion is retained, and  
only gaps in between and around it are filled in.

Which means, increasing the _patch black_ value will help restore edge detail where the edges weren't covered by the screen colour using the _size_  
slider. It's typically used for expanding out the blue/green colour  
into the remaining black gaps in the image after adjusting the other  
settings to taste.

> [!important]
> 
> 💡 In order for the _patch black_ to have any effect, the _size_ value has to be a value other than zero.

According to Lambert, it should only be used once the above _darks/lights_ have been set, and is an optional setting to remove black from the output.

Typically, I will set _patch black_  
to a value of 0, and again expand the blue/green screen another way.  
See further down in the tutorial for techniques on how to do that.

### The Lack Of Clean Plates

When using the IBK to key, most of the work  
lies in the clean plate creation. The more accurate the clean plate is,  
the better the output key from the IBK is going to be.

In a perfect world, they would film clean  
plates on set for every shot that could benefit from having one.  
However, that isn't the case:

When shooting on set, time is expensive.  
There is a large crew, support structure, and facilities in place, and  
often you're only permitted to shoot at a location for a limited time.  
(Imagine shutting down Times Square in New York for a movie shoot, for  
example).

Every minute counts. Which means there is often very little time for each department to do their job, including VFX.

By the time a shot has been captured and the  
director is happy, the crew is starting to set up the next shot. Most of  
the time, it's simply cheaper to do extra work in post than it is to  
spend more time on set filming clean plates.

Unfortunately, this means ~99.5% of shots  
won't have a clean plate filmed for them. I have worked on literally  
thousands of shots in my career, and only a small handful of them have  
ever been accompanied by clean plates.

### Clean Plate Creation

A clean plate filmed on set is usually just a  
few seconds of footage kept running either before or after the action  
has taken place in the shot. The actors have yet to enter, or have  
already left, the frame, and you can see the whole background isolated  
on its own from the same camera angle.

This is very useful because no background  
information is covered up. Which means, you have a lot more to work with  
should you need to patch something or pull a key.

> [!important]
> 
> 💡 Sometimes, you may only receive a still frame, and it may not have  
> been shot from a 1:1 matching camera angle. This just means there will  
> be a bit more work lining it up and tracking it in comp. It is still  
> usually preferable over not having a clean plate.

For the IBK in particular, having a great  
clean plate drastically improves the key. Alas, since you won't normally  
be given one, it's up to you to generate a clean plate that mimics what  
the real clean plate would look like, as closely as possible.

Ideally, the clean plate should:

- **Have the exact same gradients and colour variations as the original blue/green screen**.
    
    If  
    there is a lighting difference, a fold, and/or a shadow on the screen  
    that you don't want to include in your key, those should be present in  
    your clean plate exactly as they are in the blue/green screen plate.  
    Otherwise, you will have to stencil out unwanted parts from the alpha  
    later on.
    

- **Be particularly accurate around the edges of the subject**.
    
    It's  
    easy to garbage matte out for example a screen fold that is off to the  
    side, clear of the actor. It's much trickier to remove things that  
    intersect with the edges of the actor. The more accurate the clean plate  
    is around the edges of the actor, the less hassle you’ll have to deal  
    with.
    

- **Have any tracking markers removed before the key is applied**.
    
    Especially, if the markers are intersecting with the subject. The IBK  
    will very easily pick up orange tracking markers on a blue screen, for  
    example, and you will either have to paint them out or remove them in  
    some other way. Removing the tracking markers will also come in handy  
    for another part of the keying process, which we will look at later on  
    in this tutorial.
    

When you look at the clean plate produced by  
the IBKColour, you should ideally only see values of your original  
screen colour and (at first) black gaps. If you see colour artefacts  
around the edges of the matte, such as smudged ghost edges of your  
subject, you may have to adjust the _darks_, _lights_, and/or _erode_ values.

Only then should you expand the colour out to cover the black gaps over your subject.

As mentioned before, the IBKGizmo is very  
sensitive to the differences between the original blue/green screen  
plate and the clean plate. Ideally, the clean plate should be pixel for  
pixel exactly the same as the blue/green screen plate in the blue/green  
areas.

Only the areas where the subject and any  
shadows (if you want to keep them) are covering the blue/green screen  
should be changed in the clean plate. That way, the IBKGizmo can extract  
the most accurate matte possible.

Later on in this tutorial, we’ll look at exactly how to achieve this. But first, let’s take a look under the hood of the IBK.

# The Hidden Features Of The IBK

The IBK is old-school, and by Paul Lambert's  
request, the IBK nodes still load into Nuke as Gizmos to this day (as  
opposed to full-blown plugins).

That means you can convert the IBK nodes to  
Groups and take a look under the hood. To do that, go to the Node tab in  
the IBKGizmo and the IBKColour's properties and click on the _Copy to group_ button.

### Look Inside

Enter the Groups by selecting each and hitting Ctrl (Cmd) and Enter, or by clicking the little _S_  
button (show) in the top right of their properties. Inside, you can see  
both how the IBK nodes are structured, and partly how they work.

[![](https://www.keheka.com/content/images/2023/05/IBKColour_Inside.png)](https://www.keheka.com/content/images/2023/05/IBKColour_Inside.png)

A look inside the IBKColour.

[![](https://www.keheka.com/content/images/2023/05/IBKGizmo_Inside.png)](https://www.keheka.com/content/images/2023/05/IBKGizmo_Inside.png)

A look inside the IBKGizmo.

Although the main secret sauce is locked  
behind the IBK plugins inside of the Groups, you can still play around  
with the node setups and get a good understanding of how everything  
works, especially the IBKColour.

And, there are two layers of normally unavailable options hidden in the Groups which you can access:

### 1. Extra Options

If you open up the properties of the IBK plugins inside of the Groups, you'll find there are additional options available:

[![](https://www.keheka.com/content/images/2023/05/IBK_Plugin.png)](https://www.keheka.com/content/images/2023/05/IBK_Plugin.png)

A look at the additional options in the IBK plugin.

- **Clamp**: Clamps the alpha between 0 and 1. This option is checked by default.
    
    You  
    can untick this checkbox and experiment with how alpha values above or  
    below the 0-1 range affect the foreground image. The IBK may behave  
    differently to how you expect but can yield nice results in certain  
    scenarios.
    

- **RGBA legal**: Prevents RGBA values from going out of range and creating artefacts or  
    unexpected behaviour. This option is checked by default.
    
    Experiment at your own risk, disabling it may cause issues in Nuke.
    

- **No key**: Applies the background luminance and chroma to the foreground input, but does not pull a key.
    
    This  
    setting can be used to output RGB colours that are integrated with the  
    background, and then combine those with the alpha from a different  
    keyer.
    

### 2. Hidden Features

The second layer of hidden features can be found if you right-click on the IBKColour Group's properties and select _Manage User Knobs_.

[![](https://www.keheka.com/content/images/2023/05/IBKColour_Hidden_Knobs-1.png)](https://www.keheka.com/content/images/2023/05/IBKColour_Hidden_Knobs-1.png)

Hidden knobs in the IBKColour.

In the menu, you'll find a couple of settings which have been set to hidden, and labelled _INVISIBLE_:

- **A checkbox named** _**filt**_**:**
    
    This checkbox enables access via a Switch node to a different Erode (filter) node whose _size_ value is unaffected by the _patch black_ value.
    
    At higher _patch black_ values, the secondary patch will normally be blended more with the primary patch to avoid artefacts.
    
    You can change this behaviour by enabling the _filt_ checkbox and retaining more detail in the clean plate, at the expense of potentially introducing artefacts.
    

- **A 0-1 slider named** _**level**_**:**
    
    This is connected to the _multiply_ knob of a Grade node at the end of the Group.
    
    With this, you can multiply the RGB output, which can help remove noise from the main key pulled by the IBKGizmo.
    

Just select either _filt_ or _level_, choose _Edit_, and then untick the _Hide_ checkbox to make these options visible in the IBKColour's properties.

### The 4th Input

I mentioned previously that the IBKGizmo has a  
hidden 4th input. If you take a closer look at the IBK plugins inside  
the IBK Groups, they have a 4th input called _PFG_. (Preprocessed foreground).

[![](https://www.keheka.com/content/images/2023/05/IBK_PFG.png)](https://www.keheka.com/content/images/2023/05/IBK_PFG.png)

The four inputs of the IBK plugin. (The IBK plugin has been isolated and all four inputs have been connected for visibility).

This _PFG_ input is by default connected to the same input as the _FG_ input. The _PFG_ input is the input that is _actually_ used to pull the key.

That means, you can disconnect it and connect  
it to a different plate. If you for example wanted to use the original,  
grainy plate as your _FG_ input, you could plug in the denoised plate into the _PFG_ input to pull a cleaner key.

I denoise my plate before connecting it to  
the IBK anyway, but there are a number of other useful things that can  
be done to the _PFG_ input in order to pull a better key.

You could for example change the colour  
space, paint out problem areas, or apply corrective grading to fix  
discoloured areas of the blue/green screen that are causing issues for  
the keyer.

Note that this only applies if you want to  
control the RGB output of your IBK key. Normally, we’re only interested  
in the alpha, and so it doesn’t matter if we use the _FG_ or _PFG_ input to pull the key.

### A Look Into Recreating The Secret Sauce

Although the IBK’s keying expressions are  
hidden inside the IBK plugin, we can make an educated guess as to what  
they could be. Which is what the creators of Natron have done:

Natron is an open source, node-based  
compositing software, similar to Nuke. In Natron there is a keyer called  
PIK, which works in the same way as the IBK. The documentation even  
says the keyer [took inspiration from the IBK](https://natron.readthedocs.io/en/v2.3.15/plugins/net.sf.openfx.PIK.html).

The PIK keyer uses the following expression as a base for pulling the key (converted for use in a MergeExpression node in Nuke):

**(Ag-Ar*rw-Ab*bgw) <=0 ? 0 : 1-(Ag-Ar*rw-Ab*bgw)/(Bg-Br*rw-Bb*bgw)**

> [!important]
> 
> 💡 The above expression is for green screen keying, i.e. selecting _C-green_ in the _screen type_ drop-down menu in the IBK. For blue screen keying, you would swap the _Ag_'s with _Ab_'s, and the _Bg_'s with _Bb_'s – and vice versa – in the expression.

In the expression above (which you would paste into the alpha part of the MergeExpression node):

- **A** is connected to the _PFG_ input. _Ag_ means the green channel of the _PFG_ input.

- **B** is connected to the _C_ input. (Or to a Constant with the chosen colour, if you select _Pick_ in the _screen type_ drop-down menu). _Bg_ means the green channel of the _C_ input.

- **RW** is the value of the _red weight_.

- **BGW** is the value of the _blue/green weight_.

If you are more technically minded, this  
expression can serve as a good starting point for expanding and building  
upon the IBK keyer. But that’s a whole other tutorial and research  
topic on its own which we won’t go into here.

# Advanced IBK Setups

All right, let's get down to business.

> [!important]
> 
> 💡 In this tutorial we will be looking at advanced methods for  
> pulling the best possible key, and so I won't cover the traditional IBK  
> method. The Foundry has an [IBK tutorial](https://learn.foundry.com/nuke/content/tutorials/written_tutorials/tutorial3/image_based_keying.html)  
> describing how the keyer is used in the traditional way. It's still a  
> great method and can get you very nice results, even with just the two  
> IBK nodes.

The key (no pun intended) to pulling a great key with the IBK is to **create the best clean plate possible**.

And like we've seen, increasing the _size_ value too much in the IBKColour node can actually be counterproductive because it blurs the whole image, _adding_ differences between the original plate and the clean plate.

The trick to getting a great clean plate is  
to change as little as possible in the original plate. Try to keep as  
much of the blue/green screen as you can unaffected, and only change it  
over the subject.

There are a few ways we can do this.

### IBK Stack

You may already be familiar with the IBKColour-stacking technique.

If not, Tony Lyons has already covered it _really_ well in his tutorial, which you can watch below:

> [!info] Advanced Keying Breakdown: ALPHA 1.4 IBK Stacked Technique  
> Teaching what I know about the IBK keyer and how I use it by stacking IBK colours and focusing on the "cleanplate"  
> [https://youtu.be/izwwic6Q7cs](https://youtu.be/izwwic6Q7cs)  

In fact, if you haven't already done so, you  
should absolutely check out his whole series on keying (which includes  
the video above):

> [!info] Advanced Keying Breakdown: Introduction  
> Click timecodes to jump to sections of the video:  
> [https://youtu.be/nsg8drquDno?list=PLt2Nu4KGXJ2iXe7s-ydCQ9u1tTzzApmJX](https://youtu.be/nsg8drquDno?list=PLt2Nu4KGXJ2iXe7s-ydCQ9u1tTzzApmJX)  

It's a fantastic resource of keying knowledge.

> [!important]
> 
> 💡 One thing I have found is that the IBK stack can get quite heavy  
> and slow down your script. If you decide to precomp the stack, make sure  
> to use the EXR format and 32-bit to ensure you retain full float data  
> and accurate colour values. I was getting some odd problems with my key  
> and realised it was caused by precomping the stack to 16-bit EXRs.

### IBK Dilate

The IBK stacking technique is really great, but let's take it one step further.

Instead of stacking IBKColours to expand the blue/green screen, we can use a range of edge extension tools.

I have been using the ColourDilate node for years now, it’s just _that_ good. It almost always gives a better result than stacking IBKColour nodes, and requires minimal tweaking.

Other than setting the _size_ to 1 (down from 10) and choosing either _blue_ or _green_ in the _screen type_  
drop-down menu, I can often just leave the IBKColour node at the  
default values. (Do try to tweak the values to see if you get a better  
result, though).

[![](https://www.keheka.com/content/images/2023/05/IBK_Dilate.png)](https://www.keheka.com/content/images/2023/05/IBK_Dilate.png)

The IBK Dilate setup.

[IBK Dilate SetupDownload a copy of the IBK Dilate setup. (Copy/paste the text into your node graph).IBK_Dilate_Setup.txt21 KB](https://www.keheka.com/content/files/2023/05/IBK_Dilate_Setup.txt)

[IBK Dilate SetupDownload a copy of the IBK Dilate setup. (Copy/paste the text into your node graph).IBK_Dilate_Setup.txt21 KB](https://www.keheka.com/content/files/2023/05/IBK_Dilate_Setup.txt)

[![](https://www.keheka.com/content/images/2023/05/GreenScreen.png)](https://www.keheka.com/content/images/2023/05/GreenScreen.png)

[![](https://www.keheka.com/content/images/2023/05/IBK_Dilate_Alpha.png)](https://www.keheka.com/content/images/2023/05/IBK_Dilate_Alpha.png)

Right out of the box without any tweaking, the IBK_Dilate setup pulls a really great key with nice detail along the soft edges.

You can choose whichever colour dilate/edge  
extension tool you like, though. If you have a favourite, swap it in and  
see if it gives you an even better result.

A quick search on Nukepedia will give you a  
ton of options to choose from. And recently, The Foundry also joined  
into the edge extension tool foray, with its two nodes: _Inpaint_ and _EdgeExtend_.

They both give pretty much the same result when used in the IBK Dilate setup, but they have different options to tweak.

When using the Inpaint node, you have to change the default _Fill Region_ option to _Source Inverted Alpha_:

[![](https://www.keheka.com/content/images/2023/05/IBK_Inpaint.png)](https://www.keheka.com/content/images/2023/05/IBK_Inpaint.png)

[![](https://www.keheka.com/content/images/2023/05/Inpaint.png)](https://www.keheka.com/content/images/2023/05/Inpaint.png)

The IBK Dilate setup using the Inpaint node.

When using the EdgeExtend node, you have to tick the _Source Is Premultiplied_ checkbox, and untick the _Premultiply_ checkbox:

[![](https://www.keheka.com/content/images/2023/05/IBK_EdgeExtend.png)](https://www.keheka.com/content/images/2023/05/IBK_EdgeExtend.png)

[![](https://www.keheka.com/content/images/2023/05/EdgeExtend-1.png)](https://www.keheka.com/content/images/2023/05/EdgeExtend-1.png)

The IBK Dilate setup using the EdgeExtend node.

### IBKColour Alternatives

Let’s go even further.

I mentioned previously that you don’t have to use the IBKColour node to generate the clean plate.

If you think about what the IBKColour node is  
doing, it's only creating an alpha based on the blue/green screen, and  
using that alpha to expand the blue/green colour to cover the subject.

We can do that pretty easily, too:

1. Pull a key from your denoised blue/green screen plate using for example a Primatte. This should be a fairly hard key that removes as much of the blue/green screen as possible.

1. Since the Primatte removes the  
    blue/green screen from the alpha channel, and we only want to keep the  
    blue/green screen, we have to invert the alpha. So, connect an Invert  
    node.

1. Copy this inverted alpha back into the denoised blue/green screen plate and premultiply it.

1. You now have essentially the same output as what the IBKColour would give you: A premultiplied image with just  
    the blue/green screen in it, and black gaps where the subject is in  
    frame.

1. Apply your favourite colour dilate to fill in the black gaps, and voila, you’ve got a clean plate.

[![](https://www.keheka.com/content/images/2023/05/IBK_NoColour.png)](https://www.keheka.com/content/images/2023/05/IBK_NoColour.png)

Replacing the IBKColour with a custom keying setup.

The benefit to this method is that you can  
use any keyer and keying technique you like to pull the key, and tweak  
the alpha as much as you want before the colour dilation. In situations  
where the IBKColour doesn’t give you the clean plate result you want,  
try this method instead. Again, you can use any colour dilation node you  
like.

In specific scenarios, you can create a clean plate using actual parts of the blue/green screen:

If your subject is moving throughout the shot  
and is revealing parts of the blue/green screen behind it, you can  
patch in those parts over the subject for an even more accurate clean  
plate.

You can do this either by manually  
Frameholding and patching in sections where needed, or by using the  
Frameblend method explained here:

Richard Frazer explains how you can automate this technique:

[https://richardfrazer.com/tools-tutorials/auto-cleanplate-for-nuke/](https://richardfrazer.com/tools-tutorials/auto-cleanplate-for-nuke/)

Then, using the inverted alpha from the  
IBKColour or the alpha from your own keyer of choice (i.e. an alpha of  
the subject) as the matte, patch your blue/green screen plate with the  
clean areas extracted using the technique above.

It doesn’t happen very often, but sometimes,  
you can recreate the original blue/green screen and get a pretty close  
to perfect clean plate. Like when the camera isn’t moving in the shot  
but the subject moves quite a bit, fully revealing all parts of the  
blue/green screen at certain points during the shot.

# My Go-To Keying Approach

I'd like to preface this section by saying that pretty much all keying  
shots are different and will need custom setups and tweaks to get the  
key just right.

That said, I have found that a few workflows  
and setups consistently do the job really well, and I will usually try  
them out first. Here is my favourite keying approach:

### 1. Denoise The Image

I start off by denoising the blue/green  
screen plate. In most cases, the various keyers will create a better,  
less noisy alpha when using a denoised image as the input.

I use the ReduceNoise plugin from [Neat Video](https://www.neatvideo.com/overview/what-is-it)  
to denoise the image. It's still the number one option for denoising in  
Nuke, and it's the de facto industry standard at this point.

### 2. Remove Screen Imperfections

Next, if I don't already have Prep for the  
plate from the Prep department, I will paint out or patch any tracking  
markers, equipment, or screen imperfections that would otherwise affect  
my key.

Depending on how much prep is required, I may  
Precomp the clean up before moving on to the next step (3). When  
precomping, it's important to keep as much colour data as possible in  
order to give the keyers the best chance of pulling good keys. Render to  
a format such as EXR, with at least 16-bit colour depth. 32-bit to be  
safe.

> [!important]
> 
> 💡 Rendering to JPG or other lossy formats will introduce compression artefacts and make it harder to key enough detail.

Cleaning up the tracking markers and screen  
imperfections has another benefit, which we will take advantage of in  
step 8 to restore even more detail in our key.

### 3. Split The Stream

Then, I branch off two separate streams from the cleaned up blue/green screen plate in the Node Graph.

When keying, you typically want to treat your  
image one way for extracting the best alpha, and a different way for  
getting the best RGB colours for your composite.

So I make one branch just for pulling the key  
and creating the alpha, and I make one branch for treating the RGB  
colours, where I do the despilling and any necessary grading or edge  
expansion. I almost never use the RGB colour output from any keyer. With  
a split stream you have more options and finer control.

### 4. Build The Alpha Branch

In the alpha branch, you're both allowed and  
encouraged to do anything to the RGB values in the plate in order to  
extract the best key.

You can:

- Change the colour space of the plate, for example to Log space for a better luma key.

- Grade the plate however you want, for example to even out gradients and get a more consistent screen colour.

- Paint on the plate, for example to remove tracking markers or screen imperfections.

- Use screen cleaning tools, or do anything necessary to help the keyer create the best alpha.

> [!important]
> 
> 💡 Just make sure that you're not introducing any artefacts or  
> reducing edge detail when you treat the RGB values before pulling the  
> key.

### Pulling The Key

To pull a key, I will typically first try the  
IBK Dilate setup. Usually, that will already give me a great starting  
point, but it is often not the whole solution.

The IBK Dilate setup is excellent for  
creating a detail key. You don't want to push the IBKGizmo values too  
hard, thickening up and ruining the fine edge details, so often there  
will be some transparency left in the core matte of your subject.

You may also find that the matte from the IBK  
is a little bit noisy in some areas, particularly in the dark areas of  
the blue/green screen plate.

To fix these issues, I will Merge and/or  
Keymix the alpha that’s output by the IBK with the alpha output by other  
keyers (often the Keylight) as necessary. You have to be a bit surgical  
and go in and Keymix other keyers in specific problem areas. Stacking a  
whole bunch of keyers together can harden your alpha edges too much.

Then, I merge any roto that I’ve got, and do  
any eroding or blurring of the alpha as needed – also in a surgical  
manner. You don’t want to erode the entire alpha if you only really need  
to erode in a small section. Mask as needed.

Finally, I clamp the alpha to a range of 0-1 as the last thing in the alpha branch.

### 5. Build The Colour Branch

There are three main objectives for the colour branch:

1. **Remove any blue/green spill from the plate**.
    
    Spill  
    is often the culprit in revealing a blue/green screen composite. You  
    want to remove or reduce it so your footage looks natural composited  
    over your background.
    

1. **Grade the colours of the plate to sit in with the composite**.
    
    You may be compositing an overlit green screen element into a night time scene and will have to match the exposure, for example.
    
    Note:  
    It's not always necessary to grade the plate; very often it's the other  
    way around, and you grade the CG/background to sit in with the plate.  
    Other times, a bit of both is needed. Usually, though, we try to keep  
    the original plate as intact as possible.
    

1. **Grade, paint, or extend out problematic edges**.
    
    The  
    subject might be moving in front of a bright background in the  
    blue/green screen plate, leaving a bright edge which sticks out over a  
    dark (replaced) background. You can do your edge corrections in the  
    colour branch, before the premultiplication.
    

### Despill

Let’s start with the despill.

The importance of good despilling is perhaps  
understated. A lot of edge issues in a key can be solved just by  
adjusting the despill. And so it’s worth spending some time getting an  
accurate despill to integrate your edges nicely with your background.

There are a ton of great despill nodes out  
there. I use several different ones depending on what works best for a  
particular shot, but the following three are my go-to options, and I  
will always try them first:

- [apDespill](https://www.nukepedia.com/blink/colour/apdespill)

- [DespillMadness](https://www.nukepedia.com/gizmos/keyer/despillmadness)

- [PxF_KillSpill](https://www.nukepedia.com/gizmos/filter/pixelfudger)

Most of the time, one of these three will do an excellent job.

It's usually not necessary to despill the  
whole plate, unless there is a very heavy colour cast affecting the  
majority of the frame.

And in the majority of cases, there is no  
need to despill all the parts of the frame that are affected by spill  
equally. Typically, the edges of the subject over the blue/green screen  
will receive quite a lot of spill, while the core will be unaffected, or  
just slightly affected.

So, I will usually set up two different  
despills: one for the core, which has little to no despilling effect,  
and one for the edges of the subject, which has a stronger despilling  
effect.

I then Keymix the two despills together using an edge mask, which I create by connecting my [Perimeter](https://www.keheka.com/perimeter-advanced-edge-mattes/)  
edge matte tool to the alpha generated by the key. By adjusting the  
edge expansion and softness in the Perimeter node, I dial in the edge  
despill to cover exactly the area I need.

### Grading

After the despill process, I will apply any creative or corrective grading to the plate as needed.

This could for example be a sympathy grade to sit the plate in better with the background.

In most cases, though, the colours in the original plate should be left as untouched as possible.

### Edge Correction

Lastly, I will fix any problematic edges.  
That could for instance be dark edges over a bright background (or vice  
versa), or colour issues along the edges of the subject.

By creating an edge matte using the Perimeter  
tool and masking specific parts of the matte with a Roto, I’ll go in  
and surgically fix these issues with a Grade.

If grading doesn’t solve the problem, the  
next step will usually be to apply an edge extension. Keep in mind that a  
grade will mostly keep the original detail, but an edge extension  
won’t. So be very careful and delicate when applying edge extensions.  
You just want to nudge the values right inside of the edge slightly  
outwards, but avoid creating visible edge artefacts and losing too much  
detail.

You can use the colour dilation/edge  
extension tool of your choice for this task. There are lots of options  
on Nukepedia, and now the EdgeExtend node is included in Nuke as well. I  
often find myself using the [VectorExtendEdge](https://www.nukepedia.com/gizmos/filter/vectorextendedge) tool because it’s quite gentle and does a good job.

If you do apply an edge extension, remember  
that it can kill edge detail in your image. If you only need to remove a  
bright edge, it’s a good idea to Merge (min) the plate back over the  
edge extension to restore the lost dark details.

And vice versa, if you only need to remove a  
dark edge, it’s useful to Merge (max) the plate back over the edge  
extension to restore the lost bright details.

If all else fails, or if I just need to tweak  
a few frames, I will break out the RotoPaint node and paint out any  
problems in the edges. I tend not to do much paint work frame by frame,  
though, to avoid boiling edges.

### 6. Copy/Premult

Next, I will copy the alpha from the alpha  
branch into the colour branch and premultiply, so that it’s ready to be  
merged over the background.

### 7. Add Lightwrap And Merge

There are many tools and ways to apply  
lightwrap to your key. You can choose your own favourite method for  
doing this – just try to keep it subtle. Exaggerating the lightwrap so  
the edges are nearly glowing just looks fake.

You can add the lightwrap either before or after the premultiplication, just mask it as needed.

The way I do it is two-fold:

1. Create a soft edge matte for the  
    subject/foreground by connecting the Perimeter tool to the alpha of the  
    key. Make sure to set the _Source Interaction_ to _Mask_, so that any edge expansion or blur outside of the boundaries of the  
    original alpha is removed. That way, no lightwrap will accidentally be  
    added to the background.
    
    Then, blur the background by something  
    like 200, and mask that by the soft edge matte. Merge the result on top  
    of the key with a very low opacity, for example 0.05 in the _mix_ slider.
    
    This will bleed a bit of light and a bit of colour from the background into the subject/foreground.
    
    (There  
    are many tools that will do this automatically, I’ve just kept it  
    simple. Have a look around and see if you find a tool that works better  
    for you).
    

1. I’ll often use a custom Merge node such as the good old [L_Fuse](https://www.nukepedia.com/gizmos/merge/l_fuse) by Luma Pictures, which has options for lightwrapping and edge  
    bleeding. There are tons of these tools on Nukepedia, just pick your  
    favourite.
    
    I’ll generally use the setup in step 1 for a more  
    subtle, larger, and softer lightwrap, and step 2 for a tighter and a  
    little bit stronger lightwrap, just on the very edges of the  
    subject/foreground.
    

[![](https://www.keheka.com/content/images/2023/05/Soft_Edge_Matte.png)](https://www.keheka.com/content/images/2023/05/Soft_Edge_Matte.png)

Soft edge matte used for applying the lightwrap.

### 8. Add Detail With A Multiplication Key

For the final touch to the key, I’ll apply my [Multiplication Key Technique](https://www.keheka.com/multiply-key-technique-for-extracting-fine-detail/) to extract the finest detail like wispy hair and very soft edges that the keyers didn’t pick up.

You can also do a normal additive key here, either instead of, or in combination with, the multiplication technique.

### 9. Restore The Grain

So far, I've been working exclusively on the denoised blue/green screen plate.

To restore the grain, I will:

1. Connect a Merge (from) node to the  
    denoised plate (A) and to the original plate (B), which leaves me with  
    only the isolated grain.

1. Merge (plus) this isolated grain (A) over the comp (B) using the alpha from the key as a mask.

1. Grade the mask by lowering the _whitepoint_ to harden any soft edges, so the grain won’t go soft along the edges. I’ll also clamp it between 0 and 1.

1. Use my [Show Specific Grain Tool](https://www.keheka.com/how-to-build-a-show-specific-grain-tool/) or the DasGrain node to add matching grain to the rest of the  
    background and to any areas of the key that have been affected by Prep  
    or heavy grading.

> [!important]
> 
> 💡 If the restored grain introduces spill back into the image, apply  
> the exact same despill to both the denoised plate and to the original  
> plate before the Merge (from) node in step 1.

And that’s it!