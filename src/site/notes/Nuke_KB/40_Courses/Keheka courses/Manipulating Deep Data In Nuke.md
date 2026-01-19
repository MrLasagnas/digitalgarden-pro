---
{"dg-publish":true,"permalink":"/nuke-kb/40-courses/keheka-courses/manipulating-deep-data-in-nuke/"}
---

# No Slice

Let’s say that you have a cave, a crater, a  
tunnel, or a corridor – some sort of space – that you want to haze up  
with smoke, fog, steam or similar. We’ll use a cave with smoke for this  
example. You might get an FX artist to simulate the smoke, or you might  
build a particle system yourself in Nuke, and use a Deep render of the  
cave to hold it out correctly.

The above approaches are great but time  
consuming. What if you instead set up a few cards with smoke elements  
along the length of the cave and hold them out by the Deep render of the  
cave?

Setting that up is easy enough, but when you  
hold out the cards by the Deep render of the cave, you might see  
something like this:

[![](https://www.keheka.com/content/images/2022/09/Thickness0.jpg)](https://www.keheka.com/content/images/2022/09/Thickness0.jpg)

Six cards layered in depth with smoke elements, held out by a Deep render of a cave.

This steppiness is happening because the cards have no thickness in depth, i.e. the _deep.front_ and _deep.back_  
values are the same. When holding the cards out by the cave, only  
extremely thin cross sections of the cave are actually interacting with  
each card.

Let’s look at how we can manipulate the Deep  
data of the cards and create thickness, in order to interact with larger  
cross sections of the cave. And turn the result above into something  
like the result below (using the exact same cards and cave holdout):

[![](https://www.keheka.com/content/images/2022/09/Thickness10.jpg)](https://www.keheka.com/content/images/2022/09/Thickness10.jpg)

The same setup as before, except the Deep  
data of the cards has been manipulated to add thickness. Notice the much  
more detailed holdout corresponding to the actual shape of the cave and  
not just cross sections of it.

First things first; we need a cave.

# Building A Cave

I don’t have a Deep render of a cave handy,  
so for demonstration purposes let’s create a provisional cave in Nuke  
and use its Deep data for holding out some cards with smoke elements.

Start by creating a Cylinder node. Adjust its _radius_ and _height_, and rotate it to lay down flat, to get the general shape of a tunnel. Increase its _rows/columns_  
to something like 200 by 200 to give you more geometry to work with.  
You’ll need the extra polygons to get enough detail out of the  
displacement in the next step.

[![](https://www.keheka.com/content/images/2022/09/Scene1.png)](https://www.keheka.com/content/images/2022/09/Scene1.png)

A Cylinder with plenty of polygons.

Connect a DisplaceGeo node to the Cylinder and connect a Noise node to the _displace_ input. Adjust the _x/ysize_ in the Noise node until the Cylinder looks like a cave-like structure.

[![](https://www.keheka.com/content/images/2022/09/Scene2.png)](https://www.keheka.com/content/images/2022/09/Scene2.png)

The Cylinder displaced by a Noise texture.

# Setting Up The Smoke

Next, lay out some cards along the length of  
the cave and put smoke elements on them. For this example, I just put  
different Noise textures on each card.

Then, create a camera and point it down the cave towards the cards.

[![](https://www.keheka.com/content/images/2022/09/Scene3.png)](https://www.keheka.com/content/images/2022/09/Scene3.png)

Six cards with smoke on them, laid out along the length of the cave in front of a camera.

💡 Notice where the cards intersect with the cave geometry in the  
picture above. Those are the cross sections that by default will be  
interacting with each card when you hold them out by the cave.

Next, connect all the cards to a Scene node  
and render them through a ScanlineRender node using the camera. The  
ScanlineRender will by default output Deep data.

Then, connect the displaced Cylinder to  
another ScanlineRender node to create Deep data for the cave. Using a  
DeepMerge node with the operation set to _holdout_, hold out the cards by the cave.

You should be seeing the same issue we  
encountered at the start of the article, where each card is only held  
out by a cross section of the cave. That is, it all looks a bit steppy  
and fake.

# DeepExpression To The Rescue

This is where we start to manipulate the Deep data of the cards. Remember how the _deep.front_ and _deep.back_ values are the same on each card?

What we need to do is to push the _deep.front_ value closer to the camera, and the _deep.back_ value further away, in order to generate thickness in depth. We can do that with a DeepExpression node.

After the ScanlineRender node which is rendering out the cards, add a DeepExpression node. If we want to push the _deep.front_ value 1 scene unit closer to the camera, and the _deep.back_ value 1 scene unit further away from the camera, we can add the following expressions to the _deep.front_ and _deep.back_ fields in the DeepExpression node, respectively:

**deep.front - 1**

**deep.back + 1**

To make it more convenient for ourselves,  
though, let's add a Floating Point Slider to the DeepExpression node:  
Right-click on its properties and select Manage User Knobs… → Add →  
Floating Point Slider. Name the slider _thickness_ and increase the maximum value to 10 or more. Then, change the previous expressions to:

**deep.front - ([value thickness]/2)**

**deep.back + ([value thickness]/2)**

That way, setting the slider to 1 will  
increase the thickness of the Deep by 1 scene unit. And it's much easier  
to change the thickness around to get the look that you want.

Animating the thickness of the cards from 0 to 10 scene units.

[![](https://www.keheka.com/content/images/2022/09/ThicknessExpression.png)](https://www.keheka.com/content/images/2022/09/ThicknessExpression.png)

The DeepExpression node with the expressions needed to create Deep thickness.

[![](https://www.keheka.com/content/images/2022/09/MDD_Setup.png)](https://www.keheka.com/content/images/2022/09/MDD_Setup.png)

The setup in the Node Graph.

And that’s it; adjust the _thickness_ slider until you are happy with the look.

I hope you found this tutorial useful. For more Nuke tips & tricks, see [Nuke](https://www.keheka.com/tag/nuke/).

[Sign up for the newsletter!](https://www.keheka.com/manipulating-deep-data/#/portal/signup)

# Companions Exclusive Tutorial: Deep Fade Out Near Camera

As a **thank you** to all Companions, I have added a bonus tutorial below.

If you wanted to move the camera through the  
smoke, or vice versa, you would run into another issue: popping. As the  
cards go past the camera, the smoke pops out of view in a way that  
doesn't look smooth or natural:

As the camera moves past the cards with smoke, they pop out of view.

This is true for any setup like this; when  
thin objects get close enough to the camera, they will suddenly go from  
in front of the camera on one frame to behind on the next. It happens  
with particle clouds, fog, steam, smoke; any situation where cards are  
flying past the camera.

Adding Deep thickness may help alleviate the  
issue, but not necessarily solve it. For that, there is a better method.  
Once again, the DeepExpression node comes to the rescue. By applying a  
few expressions to your render you can smoothly fade out the cards with  
the smoke elements when they get near the camera, and get a more natural  
transition:

With a DeepExpression node, you can smoothly fade out the smoke as it gets near the camera and avoid the popping.

Here is how to do it:

First, create a DeepExpression node and add a  
Floating Point Slider to it: Right-click on its properties and select  
Manage User Knobs… → Add → Floating Point Slider. Name the slider _fadeDistance_ and increase the maximum value to for example 100.

Then, make a variable called _fade_ at the top of the DeepExpression tab and add this expression to it:

**(deep.front / [value fadeDistance]) < 1 ? (deep.front / [value fadeDistance]) : 1**

In the RGBA fields below, add these expressions, respectively:

**rgba.red * fade**

**rgba.green * fade**

**rgba.blue * fade**

**rgba.alpha * fade**

[![](https://www.keheka.com/content/images/2022/09/FadeExpression.png)](https://www.keheka.com/content/images/2022/09/FadeExpression.png)

The DeepExpression node with all the expressions needed to fade out objects near the camera.

So, how does it work? The expression for the _fade_ variable will divide the _deep.front_ value by the _fadeDistance_  
value that you set, and check if it is less than 1. If it is, it will  
output this calculated value. If it is not, it will set the value to 1.

That means, the expressions below in which we multiply the RGBA channels with the _fade_  
value, will never output values higher than the original RGBA values.  
(Anything multiplied by 1 stays the same). And, as the object comes  
close enough to the camera to be within the fade distance that you set,  
only then does it start to fade.

So, the RGBA channels will be at full, original values at the distance from the camera that you set (_fadeDistance_)  
and beyond, and then smoothly get multiplied down as the object gets  
closer and closer to the camera, until they reach a value of 0 at the  
position of the camera, i.e. completely faded out.

Quite simple, yet very effective. Connect  
this DeepExpression node to your ScanlineRender node or other Deep  
render, and watch your objects smoothly fade out as they fly past the  
camera.

[![](https://www.keheka.com/content/images/2022/09/FadeSetup.png)](https://www.keheka.com/content/images/2022/09/FadeSetup.png)

Thanks again, I hope you found this **Companions Exclusive Tutorial** useful. For more Nuke tips & tricks, see [Nuke](https://www.keheka.com/tag/nuke/).