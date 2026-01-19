---
{"dg-publish":true,"permalink":"/nuke-kb/40-courses/keheka-courses/professional-cg-compositing/"}
---

# The Basics That Most Missed

When it comes to CG rendering and compositing, the better you understand the **fundamental building blocks**, the better you'll be able to composite your CG renders at an advanced level.

You'll find there are **different layers of complexity**, and by having a strong grasp of the way everything is connected you'll be able to gain more control over your composite.

So let's start by going over the “basics” of CG rendering and compositing, in order to make sure that you fully **understand** what we'll be doing (and why) as we get into the more advanced tips and tricks further into the tutorial.

> [!important]
> 
> Tony Lyons has also covered the fundamentals in great detail in his [CG Compositing Series](https://www.youtube.com/watch?v=ep9_dsTsUT8&list=PLt2Nu4KGXJ2gjJn2eaF08UZdYUFp8xfwU&pp=iAQB&ref=keheka.com).

# The Beauty Render

In order for the 3D software to render out the CG beauty pass, i.e. the **main RGBA image depicting the scene**, it has to calculate things like:

- The position and orientation of the surfaces of the objects in the scene.

- The base colour/texture of the objects.

- [UVs](https://en.wikipedia.org/wiki/UV_mapping?ref=keheka.com) for sticking textures to the surfaces of objects.

- Where light is hitting objects, and where the light gets occluded, in the scene.

- How the light bounces off, or passes through, the objects in the scene.

- How far away the objects are from the camera.

- How fast the objects are moving.

- And _much_ more.

Rendering the beauty involves numerous separate calculations. The results of these calculations, along with other custom computations, can be output as **render passes** – often with minimal additional rendering time.

> [!important]
> 
> Render passes can be output as layers within the beauty render's image files or as separate image files, typically in the EXR format.

Using these render passes, we can **deconstruct** the beauty render, **make adjustments** to its individual components, and **rebuild** it in compositing.

This approach offers greater **creative freedom and flexibility**, providing more options for **adjusting the CG's look** and **integrating it with the plate or other elements** (if any).

It also allows **more time to refine the look in compositing**, as we can avoid re-rendering the CG for every change.

> [!important]
> 
> However, if a render is fundamentally flawed and fixing it in 3D is more efficient, it should be sent back for re-rendering.

We can broadly divide the render passes into **two categories**:

1. The **primary** render passes, which when combined together equal the beauty. Each primary pass **contributes directly to the beauty rebuild**. That is, you can **add** or **multiply** the pixel values in these passes together and you’ll end up with the beauty render.

1. The **auxiliary** render passes, which either **indirectly contribute to calculating the beauty** (e.g. data passes like normals), or which can otherwise be **helpful when compositing the CG** (e.g. utility passes such as cryptomattes).

# Primary Render Passes

When it comes to the **primary** render passes, there are two main ways a lighting artist will typically deconstruct the CG beauty render: by using **shading components** (a.k.a. material AOVs), or by using **light groups**.

### Shading Components Vs. Light Groups

↪ Deconstructing the CG beauty render into **shading components** means separating out the _unique qualities_ of **all of the lighting in the scene**.

That is, splitting the scene lighting into its individual components, like for example diffuse and specular reflections, or sub-surface scattering.  
(More on each individual shading component later in the guide).

And furthermore, splitting each of those components into their **direct** and **indirect** (bounce light) contributions. (Explained in more detail further below in the guide).

[![](https://www.keheka.com/content/images/2024/09/Shading_Components.jpg)](https://www.keheka.com/content/images/2024/09/Shading_Components.jpg)

_Example of a set of shading components._

> [!important]
> 
> The above are just a few examples of shading components. We’ll go through a more extensive list later in the guide.

↪ On the other hand, deconstructing the CG beauty render into **light groups** means separating out the _complete contributions_ of **each light source** – **or each type of light** – in the scene.

This means dividing the lighting into groups like key light, environment (HDRI) light, fill light, rim light, specific light sources, caustics, and light-emitting objects.

Each light group encompasses the complete contribution—that is, **all shading components combined**—from the light source(s) within that group.

[![](https://www.keheka.com/content/images/2024/09/Light_Groups.jpg)](https://www.keheka.com/content/images/2024/09/Light_Groups.jpg)

_Example of a set of light groups._

### Pros and Cons Of Each Deconstruction

Each of these two types of primary render passes has its pros and cons.

Let’s for example say that you would like to add some flickering to a Neon light sign in an alleyway. Or, that you would like to boost the headlights of a car.

Then, it would be useful to be able to animate a grade affecting the light group containing only the light contribution of the Neon sign, or grade up the light group containing only the headlights of the car.

In those cases, having access to – and separate control over – each **light group** would be very useful.

However, you might instead want to brighten up the specular highlights of a metallic surface as the sunlight hits it, or dial down the sub-surface scattering in a character's ears.

If so, being able to control each **shading component** would be a more useful option.

### Separate Deconstructions

The lighting artist will usually render out either one or both of these two types of primary render passes.

> [!important]
> 
> On rare occasions you might get the shading components split by light groups (in which case you would combine all of the passes in the same beauty rebuild), but in most cases you'll be dealing with one or the other.

It’s important to know that these two main types of primary render passes are **two different ways to reach the same goal**.

_**Separately**_, each of these two main types of primary render passes **adds up to – or** _**should**_ **add up to – the complete beauty render**. (We'll talk about _unassigned remains_ later on).

Therefore, when rebuilding the beauty render in your Nuke script, you should **composite each type of primary render passes separately** in order to avoid doubling up the pixel values.

> [!important]
> 
> Make sure to avoid adding different types of primary render passes together.

– So, make **one beauty rebuild** using the shading components, and make **another beauty rebuild** using the light groups.

To combine the two beauty rebuilds into one mega-rebuild for maximum control, **divide** the output of one of them – for example the shading components rebuild – by the original beauty render. Then, **multiply** that result with the output of the other rebuild – i.e. the light groups.

Now, you can adjust both types of passes and fine-tune every aspect of the beauty without doubling up any parts of it.

[![](https://www.keheka.com/content/images/2024/09/Combined_Beauty_Rebuild.png)](https://www.keheka.com/content/images/2024/09/Combined_Beauty_Rebuild.png)

_Combining both types of beauty rebuild._

We’ll take a closer look at beauty rebuilds later on in the guide, but first I’d like to go over some other topics to build up our knowledge.

### Light Vs. Texture

Whether you’re working with shading components, light groups, or both, you will likely be working with render passes which have **both the lighting and the textures baked together** in the same pass.

For example, using one of the most popular renderers in the world, [Arnold](https://en.m.wikipedia.org/wiki/Autodesk_Arnold?ref=keheka.com), a lighting artist will typically output the **diffuse** shading component into **three passes**.

The first two diffuse passes are:

- **diffuse direct**

- **diffuse indirect**

These two diffuse passes do not only contain the raw diffuse direct and indirect (bounce) lighting.

The third diffuse pass, the **diffuse albedo**  
pass – i.e. the raw (without lighting or shadowing) diffuse textures of  
the objects in the scene – will also be baked into these two passes:

↪ The **diffuse direct** pass is the product of the **diffuse albedo** pass multiplied by the **raw diffuse direct lighting** contribution.

[![](https://www.keheka.com/content/images/2024/09/Diffuse_Direct.jpg)](https://www.keheka.com/content/images/2024/09/Diffuse_Direct.jpg)

[![](https://www.keheka.com/content/images/2024/09/Diffuse_Albedo.jpg)](https://www.keheka.com/content/images/2024/09/Diffuse_Albedo.jpg)

[![](https://www.keheka.com/content/images/2024/09/Raw_Direct_Lighting.jpg)](https://www.keheka.com/content/images/2024/09/Raw_Direct_Lighting.jpg)

_The diffuse direct pass (left) equals the diffuse albedo pass (middle) multiplied by the raw direct lighting pass (right). In this example, there are two lights in the scene: a warm key light and a cool fill_  
_light._

↪ And likewise, the **diffuse indirect** pass is the product of the **diffuse albedo** pass multiplied by the **raw diffuse** _**indirect**_ **lighting** contribution.

> [!important]
> 
> The **diffuse albedo** pass can be used to separate out the raw lighting from the other diffuse passes – for treating the lighting and the textures individually. The pass (and also the other albedo passes) is not directly added to the other primary render passes in the beauty rebuild like how the **diffuse direct** and **diffuse indirect** passes are – it's sort of a **secondary** render pass that interplays with the primary render passes. We'll get to this a little bit further below in the tutorial.

### Quick Sidebar: Direct Vs. Indirect Lighting

– Just to clarify, any light that travels directly from a light  
source to a surface in the scene, and then reflects off the surface with  
a **single bounce** before it reaches the camera, is called **direct lighting**.

> [!important]
> 
> Not to be confused with **emission**, which is light travelling directly from a light source to the camera **without bouncing** off any surfaces.

The portion of light that instead reflects off the surface at different angles and hits other surfaces in the scene before bouncing to the camera is called **indirect lighting**. – I.e. **two or more bounces** in total.

In the real world, light will bounce around in the environment a great number of times – getting absorbed, reflected, and dispersed by the various surfaces with each hit – before it loses (transfers) all of its energy.

But all this bouncing is computationally heavy to calculate, and **the light contribution gets more and more negligible with each bounce**.  
– To the point where, after a few bounces, we typically won't see much  
of a difference in the image with further bounce calculations.

Which is why the lighting artist will **limit the number of bounces** to be included in the render calculation – speeding up the rendering while still producing a visually acceptable result.

The diffuse light contribution from this bounce light calculation is output into the **diffuse indirect** pass. (Which will typically be weaker/dimmer than the **diffuse direct** pass, but nonetheless contributes a great deal to the realism of the image).

### Okay, Back To Light Vs. Texture

– We saw that when it comes to shading components, the diffuse direct and diffuse indirect passes contain the raw lighting _and_ the raw texture, baked together.

> [!important]
> 
> This is also the case for other shading components like the specular reflections. (However, the specular albedo pass is typically not rendered out because its contribution rarely needs to be adjusted).

The same baking of light and texture also happens with light groups.

–  
If you look at a light group in the Viewer, you can see the same thing  
happening, where you've got a combination of light and texture baked  
together.

[![](https://www.keheka.com/content/images/2024/09/Light_Group.jpg)](https://www.keheka.com/content/images/2024/09/Light_Group.jpg)

_A light group containing the key light, with its diffuse and specular components baked together with the albedo._

### Double-Grading

That means, whether you're compositing shading components and/or light groups, any grade changes you make to the render passes **will affect both the lighting** _**and**_ **the texture**.

Let's say that you have a white CG car lit by a white light. If you would grade either the diffuse shading components or the relevant light groups in order to change the car's colour to yellow,you'd _also_ be grading the lighting towards a yellow colour.

Grading the passes directly means you're changing both the colour of the texture _and_ the colour of the lighting. – You're essentially double-grading.

This isn’t necessarily a problem, but if you instead wanted to keep the light white, and only grade the actual car yellow, you can separate the two:

### Separating Lighting From Texture

Depending on what you're working with, individually **divide** either the **diffuse passes** or the **light groups**, (A), by the **diffuse albedo** pass (B) using a Merge (divide) in order to isolate the raw lighting.

Now, you can grade the isolated raw lighting as well as the albedo on their own, and then **multiply** them back together again afterwards to get back to either the diffuse pass or the light group for further comping.

[![](https://www.keheka.com/content/images/2024/09/Separate_Light_From_Texture.png)](https://www.keheka.com/content/images/2024/09/Separate_Light_From_Texture.png)

_Isolating the raw direct diffuse lighting._

Separating the light and texture is not only useful for grading, but also for denoising. If a particular pass is noisy, you can denoise just the lighting of that pass (or denoise the lighting more than the texture), and still keep the texture detail intact.

Or, if you'd like to paint only on the texture, for example to fix or add some damage to a brick wall, you can separate it out and then add the clean lighting back on top after.

[![](https://www.keheka.com/content/images/2024/09/Bricks_Broken.jpg)](https://www.keheka.com/content/images/2024/09/Bricks_Broken.jpg)

[![](https://www.keheka.com/content/images/2024/09/Bricks_Paint_On_Baked.jpg)](https://www.keheka.com/content/images/2024/09/Bricks_Paint_On_Baked.jpg)

[![](https://www.keheka.com/content/images/2024/09/Bricks_Paint_On_Texture.jpg)](https://www.keheka.com/content/images/2024/09/Bricks_Paint_On_Texture.jpg)

_Erroring black brick in the original render (left). Cloning directly on the render which has the texture and lighting baked together makes the replacement brick too bright, because we’re sourcing a brighter brick (middle). Cloning only on the texture fixes the issue (right)._

This is especially great when the lighting is a little bit more complex, like the dappled light on a forest floor. Or, if there is a clear lighting shape or gradient going across the surface that you want to paint on. (Which would become messed up by cloning or patching).

### Different Types Of Primary Render Passes

Not everyone uses Arnold for rendering their CG.

There are many popular renderers on the market – and not all renderers output primary render passes which have the texture and the lighting baked together like Arnold normally does.

For example, you might be using Redshift. In which case, you could be outputting the diffuse pass as **DiffuseLightingRaw** and **DiffuseFilter** (albedo), and working with the lighting and the texture separately by default.

If so, you'll still have to multiply them together to create the ‘complete' primary pass which can then be added together with the others to rebuild the beauty. But you won't have to worry about separating out the lighting like described above.

### Additive Vs. Subtractive Method

When rebuilding the beauty using the primary render passes, there are two main approaches: **additive** or **subtractive**.

↪ The _**additive**_ method involves **shuffling out all of the primary render passes**, either shading components or light groups, adjusting them as needed, and then **adding them back together** using Merge (plus) nodes.

The benefit of this method is that all the passes are laid out clearly in your script, and therefore it's easy to keep an overview of what's going on. You have direct access to every pass in the node graph and can make changes on the fly.

It's especially great for when you need to make multiple adjustments to many render passes.

It is, however, the computationally heavier approach compared to the subtractive method. That's because you’re rebuilding the entire beauty – shuffling out and recombining all of the render passes, even the ones that you're not making any changes to.

The result is a larger script with more nodes, more data read from disk, and more data stored in the RAM.

[![](https://www.keheka.com/content/images/2024/09/Basic_Additive_Beauty_Rebuild.png)](https://www.keheka.com/content/images/2024/09/Basic_Additive_Beauty_Rebuild.png)

_A basic beauty rebuild using the additive method._

> [!important]
> 
> Note that adding all the passes together can screw up your alpha channel. Make sure to work only on the **unpremultiplied** passes to avoid edge issues, before copying in the clean alpha and premultiplying at the end.

  

> [!important]
> 
> After each Shuffle node in all of the AOV pipes, remove all the superfluous channels by adding a **Remove (keep)** node and set it to **keep only the RGB channels**. By doing this, you’ll avoid unnecessary processing from any nodes and gizmos which affect _all_ channels, and make your script faster.

↪ The _**subtractive**_ method involves **shuffling out only the render pass that you want to adjust**, and **subtracting** the pass (A) from the beauty render (B) with a Merge (from).

Then, branching off a new pipe from the Shuffle node and making adjustments to the pass, before **adding the altered pass back into the stream** using a Merge (plus).

[![](https://www.keheka.com/content/images/2024/09/Basic_Subractive_Beauty_Rebuild.png)](https://www.keheka.com/content/images/2024/09/Basic_Subractive_Beauty_Rebuild.png)

_A basic beauty rebuild using the subtractive method. Here, adjusting the diffuse AOV._

  

> [!important]
> 
> Note that some studios may denoise the beauty and the AOVs separately, adding minor discrepancies between the beauty and the beauty rebuild. This can affect the subtractive rebuild, causing for example negative values in the rebuild. It's something to look out for. If it happens to you, you can do an additive rebuild including the unassigned remains instead.

If you only need to make adjustments to a few render passes, this method may be the way to go. It's simpler, uses fewer nodes, and you can just daisy chain the setup above for each pass that you want to adjust.

However, if you're changing a lot of passes it can get messy, and it might be better to use the  
additive method instead for easier organisation and a clearer overview.

### Unassigned Remains

As previously mentioned, the sum of all the shading components, and the sum of all the light groups, should _**each**_ add up to the complete beauty render.

In an ideal world they should each be a 1:1 match with the beauty. However, the lighting artist will occasionally **miss grouping up a light or a shading component**, and there will be a **mismatch** between the sum of the render passes and the beauty render.

When that happens, it's a good idea to let the lighting artist know so that they can correct the issue in the next version.

However, to work with/around what you have been given, you can add together all the passes you _do_ have with a Merge (plus), and then subtract that result (A) from the beauty render (B) using a Merge (from).

This will give you the **unassigned remains**. Merge (plus) these remains (A) into the beauty rebuild stream (B) to complete the beauty rebuild.

[![](https://www.keheka.com/content/images/2024/09/Unassigned_Remains.png)](https://www.keheka.com/content/images/2024/09/Unassigned_Remains.png)

_A basic beauty rebuild using the additive method, with the unassigned remains restored._

> [!important]
> 
> It's good practice to do this step by default for any additive beauty rebuild, and catch any potentially missing bits and pieces. I would highly recommend making it a part of your template. But make sure to **only** make changes to the AOVs **after** where the pipes go into the Merge node for the unassigned remains. Otherwise, the changes will be cancelled out.

With the unassigned remains added to the beauty rebuild, you should now mathematically have rebuilt the beauty pass 1:1.

> [!important]
> 
> You _could_ combine the additive and subtractive rebuild methods, i.e. shuffling out _all_ the passes and subtracting/adding them with the beauty. This would take care of the unassigned remains automatically – i.e. you wouldn't have to add them back in – but you wouldn't then be able to grade the unassigned remains separately.

By manipulating the primary render passes in comp, there is a whole lot we can do to adjust the look of our CG. However, to help us really change up and fine-tune things exactly as we like, we can make use of the auxiliary render passes:

# Auxiliary Render Passes

As touched upon earlier, auxiliary render passes are **support passes** which we can use to gain more control over our CG composite.

They are either the results of calculations that were made along the way when rendering the beauty and its primary render passes. Or, they are the results of additional calculations we asked the renderer to do (such as making cryptomattes, or outputting other useful data).

Common examples of auxiliary render passes are the (world) **Position** and **Normal** passes, which contain positional and rotational information about the surfaces of the objects in the scene.

While these passes are not a part of the beauty rebuild, the information contained within them _was_ necessary for rendering the beauty and the primary render passes. And we can make good use of them in comp – for things like grading the CG with PMattes, relighting the CG using the normals, creating point clouds to use as reference for placing Cards in 3D space, and much more.

There are many different render passes with all sorts of uses. Let's take a look at the most common ones:

# AOVs

Both the primary and the auxiliary render passes are typically referred to as _AOVs_.

> [!important]
> 
> AOV stands for Arbitrary Output Variable.

Depending on which software was used to render the CG, the AOVs you'll get to work with will be slightly different. You'll have to adjust your setup a little bit to accommodate the differences, but the general idea behind them remains the same:

Add all the primary AOVs together, and if you have raw lighting and texture, multiply them together.

We'll continue using [Arnold](https://help.autodesk.com/view/ARNOL/ENU/?guid=arnold_user_guide_ac_output_aovs_ac_aovs_html&ref=keheka.com) as the example since it's so commonly used. However, you can find more information about other renderers and their AOVs here:

- [Redshift](https://help.maxon.net/c4d/en-us/Subsystems/Default/Content/html/Intro+to+AOVs.html?ref=keheka.com)

- [Octane](https://docs.otoy.com/cinema4d//RenderAOVs.html?ref=keheka.com)

- [V-Ray](https://docs.chaos.com/display/VMAYA/Render+Elements?ref=keheka.com)

- [RenderMan](https://renderman.pixar.com/resources/RenderMan_20/secondaryOutputs.html?ref=keheka.com)

- [Guerilla](http://guerillarender.com/doc/2.0/User%20Guide_Rendering_AOV.html?ref=keheka.com)

- [Keyshot](https://manual.keyshot.com/manual/render-4/render-output/layers-and-passes/?ref=keheka.com)

- [RayRender](https://learn.foundry.com/nuke/content/reference_guide/3d_nodes/rayrender.html?ref=keheka.com)

### Primary AOVs

Remember, there are two main ways to deconstruct the CG beauty render: with **shading components** and with **light groups**.

### Shading Components

Shading components are the more complex option, with the most  
information to remember in terms of what each of the passes contain, and  
how to rebuild the beauty.

(It's actually not very difficult, though – don't worry. As you’ll see, a lot of them follow the same pattern).

Let's go through each shading component, starting with reflections.

There are two main types of reflections, diffuse reflections and specular reflections:

### ↪ Diffuse

The **diffuse** pass contains the [diffuse reflections](https://en.m.wikipedia.org/wiki/Diffuse_reflection?ref=keheka.com) in the scene, i.e. the scattered, softer light which is reflected in all directions off of rough surfaces.

[![](https://www.keheka.com/content/images/2024/09/Diffuse_Reflections.jpg)](https://www.keheka.com/content/images/2024/09/Diffuse_Reflections.jpg)

_Light rays scatter as they hit an uneven surface, resulting in diffuse reflections._

Note that the surfaces don’t necessarily have to visually _look_ rough. It’s often microfacets in the surfaces which scatter the light.

If you look at a painted interior wall in a house, for example, it’s a smooth surface overall – but it's not completely smooth. There is a lot of microsurface detail which very effectively scatters light.

Shine your phone's flashlight against the wall and compare that to shining it against a glass pane, for instance. You likely won’t see the bright core of the light on the wall due to the light scattering. (Unless you get up really close, but even then the reflection won’t be as strong and  
clear as in the glass pane).

[![](https://www.keheka.com/content/images/2024/09/Drywall_resize.jpg)](https://www.keheka.com/content/images/2024/09/Drywall_resize.jpg)

[![](https://www.keheka.com/content/images/2024/09/Drywall_Glass_Pane_resize.jpg)](https://www.keheka.com/content/images/2024/09/Drywall_Glass_Pane_resize.jpg)

_A painted interior wall lit by a phone's flashlight (left), and the same setup again but with a glass pane held against the wall (right). The smooth glass pane gives off specular reflections, and we can see the light source reflected, while the bare wall mainly gives off evenly soft diffuse reflections._

The diffuse pass will give you the general colour and shape of the CG object, lit by the light sources  
in the scene, without any bright highlights or mirror-like reflections.

[![](https://www.keheka.com/content/images/2024/09/diffuse-1.png)](https://www.keheka.com/content/images/2024/09/diffuse-1.png)

_The diffuse pass contains no bright highlights or mirror-like reflections._

Unless it's perfectly smooth or completely transparent, there will typically always be some amount of diffuse reflections on every surface. And so this is one of the most common AOVs you’ll work with in comp.

The diffuse pass can be further split up into three sub-components:

**1. Diffuse albedo**:

The diffuse colour or texture pass, i.e. the raw diffuse colours/texture of the objects in the scene, without any lighting or shadowing.

As mentioned earlier, while not directly needed as part of the beauty rebuild, this pass can be very useful for dividing out the raw diffuse lighting contribution in order to adjust it separately from the texture.

**2. Diffuse direct**:

The diffuse albedo multiplied by the diffuse direct light contribution.

(Remember, direct light is the light which only bounces (reflects) **once** off a surface before it hits the camera).

This pass will typically be brighter than the diffuse indirect pass, because with only a single bounce, the light that reaches the camera will have conserved the most energy. (With each bounce, the light transfers away some energy due to absorption or scattering).

So changing this pass will normally have the greater impact on the look of the CG.

> [!important]
> 
> Note that for transparent objects, the diffuse direct pass will be empty (black), because the light is passing _through_ the object instead of reflecting off of it. (I.e. more than one light bounce).

**3. Diffuse indirect**:

The diffuse albedo multiplied by the diffuse indirect (bounce light) contribution.

(Remember, indirect light is the light which bounces **two or more times** in the scene before it hits the camera).

When you have multiple objects or surfaces in the scene, some diffuse light may bounce between them before reaching the camera – which is what this pass captures.

The indirect light will typically help provide some more definition and visibility to darker areas, giving objects a more natural looking lighting and realistic feel.

So while the pass may contribute less overall than the diffuse direct pass, its contribution is still important.

> [!important]
> 
> Because of the multiple light bounces, the diffuse indirect pass may contain more render noise. The more bounces, the more rays, the more potential for noise. This is also true for all other indirect render passes.

### ↪ Specular

The **specular** pass contains the [specular reflections](https://en.wikipedia.org/wiki/Specular_reflection?ref=keheka.com) in the scene, i.e. the more concentrated, directional light that's reflected off of smoother surfaces.

[![](https://www.keheka.com/content/images/2024/09/Specular_Reflections.jpg)](https://www.keheka.com/content/images/2024/09/Specular_Reflections.jpg)

_Light rays reflect evenly off a smooth surface, resulting in specular reflections._

Compared to the diffuse reflections, the specular reflections aren’t (nearly as) scattered, and so you can often see the light sources and the environment reflected in the objects in the scene.

[![](https://www.keheka.com/content/images/2024/09/spec-1.png)](https://www.keheka.com/content/images/2024/09/spec-1.png)

_The specular pass contains bright highlights and reflections._

Most surfaces in real life have some sort of shine to them, and so the specular pass is also one of the most common AOVs you’ll work with in comp.

Like the diffuse pass, the specular pass can also be further split up into three sub-components:

**1. Specular albedo**;

The specular texture pass, i.e. the raw specular colours/texture of the objects in the scene, without any lighting or shadowing.

Some objects have a rougher surface which breaks up the specular light, and this pass controls this breakup.

In my experience, the specular albedo pass isn't usually provided to compers, and we tend to break up specular reflections [in other ways](https://www.keheka.com/breaking-up-edges-and-surfaces-in-nuke/).

**2. Specular direct**:

The specular albedo multiplied by the specular direct light contribution.

The specular direct pass contains the [specular highlights](https://en.wikipedia.org/wiki/Specular_highlight?ref=keheka.com) in the scene, i.e. the bright highlights on objects, which are direct reflections of the light sources in the scene. (I.e. a single light bounce before reaching the camera).

[![](https://www.keheka.com/content/images/2024/09/Specular_direct.jpg)](https://www.keheka.com/content/images/2024/09/Specular_direct.jpg)

_The specular direct contribution, which directly reflects the light sources in the scene (one light bounce before reaching the camera). Depending on the roughness of the material, the reflected light sources can look more or less blurred._

**3. Specular indirect**:

The specular albedo multiplied by the specular indirect (bounce light) contribution.

The specular indirect pass (also sometimes called the **reflection** pass) contains the indirect [specular reflections](https://en.wikipedia.org/wiki/Specular_reflection?ref=keheka.com) in the scene, i.e. **mirror-like reflections**. These are indirectly reflecting the light sources in the scene. (I.e. with two or more light bounces).

For example, if there is an actual mirror in the scene, or a reflective surface such as a glass window or a chrome object, they’ll reflect other objects in the scene.

[![](https://www.keheka.com/content/images/2024/09/Specular_indirect.jpg)](https://www.keheka.com/content/images/2024/09/Specular_indirect.jpg)

_The specular indirect contribution, which indirectly reflects the light sources in the scene via other objects or surfaces (two or more light bounces before reaching the camera). Depending on the roughness of the material, the reflections can look more or less blurred._

The specular indirect pass works a little bit differently to the other shading components, because this pass contains **every shading component** in the scene baked together. It’s like a **mini-beauty render**, just for the mirror reflections. (Because it’s reflecting the whole scene).

– Which can be a bit frustrating to deal with if you're making significant adjustments to your other shading components in the comp. For example, if you change the diffuse colour of an object, it can be tricky to match up its reflection. (Because you can’t easily split out just the diffuse component from the specular indirect pass).

> [!important]
> 
> We’ll take a closer look at reflections in the _Reflections_ section, later on in the guide.

Okay, let’s move on to **transmissions**.

Similar to how there are diffuse and specular _reflections_ (i.e. light bouncing off of objects), there are also diffuse and specular _transmissions_ (i.e. light passing _through_ objects):

### ↪ Sub-Surface Scattering (SSS)

The **SSS** pass contains the [diffuse transmission](https://en.m.wikipedia.org/wiki/Subsurface_scattering?ref=keheka.com) of light in the scene.

That is, the light rays which penetrate a **trans**_**lucent**_ object’s surface, scatter inside the material of the object as they pass through it, and exit the object at different points/angles on the object’s surface.

[![](https://www.keheka.com/content/images/2024/09/Diffuse_Transmission.jpg)](https://www.keheka.com/content/images/2024/09/Diffuse_Transmission.jpg)

_Light rays scatter within a translucent object’s material and exit the object at different angles._

This commonly happens with translucent materials like skin or wax.

[![](https://www.keheka.com/content/images/2024/09/SSS.jpg)](https://www.keheka.com/content/images/2024/09/SSS.jpg)

_Light is scattered on the way through a hand._

The SSS pass can also be further split up into three sub-components:

**1. SSS albedo**:

The SSS texture pass, i.e. the raw SSS colours/texture of the objects in the scene, without any lighting or shadowing.

**2. SSS direct**:

The SSS albedo multiplied by the SSS direct light contribution.

SSS _is_ diffuse lighting, but it’s a separate calculation to the diffuse reflections (because it’s a transmission instead of a reflection), and so any objects with an SSS material will go in this pass instead of in the diffuse direct pass.

– The diffuse direct pass will then be just empty/black in that area. Or, there could be a mix between diffuse reflections and SSS in the material, depending on the weighting used by the lighter. Meaning, there would be pixel values in both passes: some diffuse reflections and some diffuse transmissions (SSS).

Essentially, the SSS direct pass replaces or augments the diffuse direct pass for just the objects with an SSS material applied to them, and you’d need both passes to complete the beauty rebuild.

**3. SSS indirect**:

The SSS albedo multiplied by the SSS indirect light contribution.

This pass works the same way as the SSS direct pass above, only with indirect lighting.

With sub-surface scattering, by definition, light has to penetrate a surface, scatter within the material, and then exit the surface at a different point. Which means the light _has_ to bounce two or more times.

So the typical SSS ‘effect’ (for example the glowy red effect showing through the hand in the picture above) will be isolated in the SSS indirect pass.

### ↪ Transmission (Refraction)

The **transmission** pass (often named **refraction**) contains the **specular transmission** of light in the scene.

That is, the light rays which penetrate a **trans**_**parent**_ object’s surface, pass through the material of the object without being scattered, and exit the object at a different point on the object’s surface.

[![](https://www.keheka.com/content/images/2024/09/Specular_Transmission.jpg)](https://www.keheka.com/content/images/2024/09/Specular_Transmission.jpg)

_Specular transmission is when light rays pass through a transparent object without scattering._

Light will often bend as it passes from one medium to another, which is called [refraction](https://en.wikipedia.org/wiki/Refraction?ref=keheka.com).  
So refraction is a subset of transmission. (All refractions are transmissions, but not all transmissions are refractions). However the two names are often used interchangeably.

> [!important]
> 
> You can read [more about refractions here](https://www.keheka.com/compositing-realistic-heat-distortion-in-nuke/).

[![](https://www.keheka.com/content/images/2024/09/Specular_Transmission_Refraction.jpg)](https://www.keheka.com/content/images/2024/09/Specular_Transmission_Refraction.jpg)

_Refraction is the common case of specular transmission when light rays pass through a transparent object without scattering, and the angle of the light rays change._

The transmission pass will usually contain things like the light which passes through a glass window or a bottle.

[![](https://www.keheka.com/content/images/2024/09/Refraction.jpg)](https://www.keheka.com/content/images/2024/09/Refraction.jpg)

_Sunlight is transmitted (passed through) a glass bottle. In this case, the light is being refracted (bent) on its way through. – You can see the sunlit sea in the glass bottle, but if there were no refraction happening, only transmission, you would see the blue sea (that’s directly behind it) through the bottle instead._

Similar to the **specular indirect** (reflection) pass, the transmission pass is _also_ like a **mini-beauty render** – it contains all the shading components of the refracted light. (Because it’s refracting the whole scene).

The transmission pass can also be further split up into three sub-components:

**1. Transmission albedo**:

The transmission texture pass, i.e. the raw transmission colours/texture of the objects in the scene, without any lighting or shadowing.

This pass may for example **tint the light** passing through an object, like a green glass bottle.

**2. Transmission direct**:

The transmission albedo multiplied by the transmission direct light contribution.

This pass will normally be **empty**, because, by definition, **all** light that is refracted will be bouncing at least two or more times before it hits the camera.

(The exception to this would be if the 3D model for example were a Card with no thickness, something which doesn't exist in the real world).

**3. Transmission indirect**:

The transmission albedo multiplied by the transmission indirect (bounce light) contribution.

This pass will typically contain the entire transmission light contribution, for the reason described above.

This, combined with the fact that we don’t usually need to change the transmission albedo, is why the transmission pass is often not split up any further. You’ll typically just get a single transmission pass.

### ↪ Emission

The **emission** pass contains lights and light-emitting objects directly visible from the camera.

These can be incandescent or luminescent objects, such as light bulbs, sparks, LED billboards, Neon signs, lava, glow-in-the-dark stickers, etc.

– Anything that emits light.

[![](https://www.keheka.com/content/images/2024/09/emission_heat.jpg)](https://www.keheka.com/content/images/2024/09/emission_heat.jpg)

[![](https://www.keheka.com/content/images/2024/09/LED_Billboard.jpg)](https://www.keheka.com/content/images/2024/09/LED_Billboard.jpg)

[![](https://www.keheka.com/content/images/2024/09/light_bulb.jpg)](https://www.keheka.com/content/images/2024/09/light_bulb.jpg)

[![](https://www.keheka.com/content/images/2024/09/emission_box.jpg)](https://www.keheka.com/content/images/2024/09/emission_box.jpg)

[![](https://www.keheka.com/content/images/2024/09/emission_lava.jpg)](https://www.keheka.com/content/images/2024/09/emission_lava.jpg)

_Examples of emission_.

These objects will usually have higher pixel values, and therefore bloom out more when glow is applied to the image.

> [!important]
> 
> Note that if the light-emitting object is illuminating other surfaces, that light will fall into the relevant indirect passes, e.g. diffuse indirect. (And only the light which travels directly from the  
> light-emitting object to the camera falls into the emission pass). The emission pass normally doesn’t get split up into any further sub-passes.  
> There are no “emission indirect” or “emission albedo” passes, for example – there exists only (direct) emission. (It’s likely possible to create that with [Light Path Expressions](https://help.autodesk.com/view/MAYAUL/2024/ENU/?guid=arnold_for_maya_aovs_am_Introduction_to_Light_Path_Expressions_html&ref=keheka.com), though).

### ↪ Coat

The **coat** pass simulates a dielectric material (e.g. plastic, glass, resin/enamel, many liquids) which absorbs light and tints all of the transmitted light.

(Metals, on the other hand, tend to filter the colour of whatever it is they are reflecting, even at grazing angles).

It acts as an infinitely thin, reflective clear-coat layer on top of all other shading effects. That is, the remaining non-reflected light is passed directly to the underlying layer without refraction.

Some examples of the use of the coat pass include: car paint, laminated objects, oil, slime, soap bubbles, protective film, or reflective coating.

[![](https://www.keheka.com/content/images/2024/09/clear_coat_carbon_fibre.JPG)](https://www.keheka.com/content/images/2024/09/clear_coat_carbon_fibre.JPG)

_Clear-coating over carbon fibre._

[![](https://www.keheka.com/content/images/2024/09/Slime.jpg)](https://www.keheka.com/content/images/2024/09/Slime.jpg)

_Coat effect seen on slime._

[![](https://www.keheka.com/content/images/2024/09/Coat_Octopus.png)](https://www.keheka.com/content/images/2024/09/Coat_Octopus.png)

_The isolated coat layer of an octopus._

The coat pass can be further split up into:

**1. Coat albedo**:

The coat texture pass, i.e. the raw coat colours/texture of the objects in the scene, without any lighting or shadowing.

**2. Coat direct**:

The coat albedo multiplied by the coat direct light contribution.

Similar to the specular direct pass, this pass will contain the direct reflections of light sources in the scene. However, the coat direct pass goes on top of, and may tint, the specular reflections.

**3. Coat indirect**:

The coat albedo multiplied by the coat indirect (bounce light) contribution.

This pass works the same way as the coat direct pass above, only for indirect lighting.

### ↪ Sheen

**Sheen** is used to approximate microfibre or cloth-like surfaces such as velvet and satin (of varying roughness), and to simulate leaves, fruit, or the peach fuzz on a face (..or on a  
peach).

It’s a soft, smooth shine that gives an extra lustre to the material of the object.

[![](https://www.keheka.com/content/images/2024/09/satin_sheen.jpg)](https://www.keheka.com/content/images/2024/09/satin_sheen.jpg)

_Sheen on a satin material._

[![](https://www.keheka.com/content/images/2024/09/Sheen_Chair.jpg)](https://www.keheka.com/content/images/2024/09/Sheen_Chair.jpg)

_Sheen on a chair._

[![](https://www.keheka.com/content/images/2024/09/Peach_Fuzz.jpg)](https://www.keheka.com/content/images/2024/09/Peach_Fuzz.jpg)

_Sheen on peaches._

The sheen pass can be further split up into:

**1. Sheen albedo**:

The sheen texture pass, i.e. the raw sheen colours/texture of the objects in the scene, without any lighting or shadowing.

Use this to tint the colour of the sheen.

**2. Sheen direct**:

The sheen albedo multiplied by the sheen direct light contribution.

Pretty straightforward: the sheen contribution from any direct lighting.

**3. Sheen indirect**:

The sheen albedo multiplied by the sheen indirect light contribution.

This pass works the same way as the sheen direct pass above, only for indirect lighting.

### ↪ Volume

The **volume** pass contains the volumetric scattering of light in the scene, for example in the form of godrays, light shafts, or the light scattering within fog or the atmosphere.

[![](https://www.keheka.com/content/images/2024/09/volumetric_lighting.jpg)](https://www.keheka.com/content/images/2024/09/volumetric_lighting.jpg)

[![](https://www.keheka.com/content/images/2024/09/volume_church.jpg)](https://www.keheka.com/content/images/2024/09/volume_church.jpg)

[![](https://www.keheka.com/content/images/2024/09/Light_Shaft.jpg)](https://www.keheka.com/content/images/2024/09/Light_Shaft.jpg)

[![](https://www.keheka.com/content/images/2024/09/ROV.png)](https://www.keheka.com/content/images/2024/09/ROV.png)

[![](https://www.keheka.com/content/images/2024/09/volume_spaceship.jpg)](https://www.keheka.com/content/images/2024/09/volume_spaceship.jpg)

_Examples of volumetric scattering of light._

Typically, to us, light is visible at its **source** and (when reflecting off of) its **target**.

Take a light bulb in a room, for example. We can see the light emanating from the bulb, and we can see the light being reflected off of the wall.

[Volumetric light](https://en.wikipedia.org/wiki/Volumetric_lighting?ref=keheka.com), however, is visible in the space _**between**_ the light source and its target. It’s light passing through an actual three-dimensional medium such as fog, dust, smoke, steam, or other particles.

> [!important]
> 
> Unless travelling in a vacuum, light will always hit particles along its path and scatter to some degree – and will as such be volumetric light. However, the atmosphere between the source and the target is usually sparse enough that we don’t see it. But if you were to fill the room in the example above with smoke, for instance, the light from the light bulb would immediately appear volumetric.

Volumetric light is typically soft, diffuse light that's bouncing off of particles in the air or in water – creating a hazy look.

But volumetric light can also appear as sharp, defined shafts of light – due to being shaped by objects blocking the light. For example, the rays through a church window, or through clouds ([crepuscular rays](https://en.m.wikipedia.org/wiki/Crepuscular_rays?ref=keheka.com)).

[![](https://www.keheka.com/content/images/2024/09/Godrays1.jpg)](https://www.keheka.com/content/images/2024/09/Godrays1.jpg)

[![](https://www.keheka.com/content/images/2024/09/Godrays2.jpg)](https://www.keheka.com/content/images/2024/09/Godrays2.jpg)

[![](https://www.keheka.com/content/images/2024/09/Godrays3.jpg)](https://www.keheka.com/content/images/2024/09/Godrays3.jpg)

_Godrays formed by church windows_.

[![](https://www.keheka.com/content/images/2024/09/Crepuscular_Rays_Over_Clear_Sky.jpg)](https://www.keheka.com/content/images/2024/09/Crepuscular_Rays_Over_Clear_Sky.jpg)

_Crepuscular rays over a clear sky, formed by dense cloud outlines._

[![](https://www.keheka.com/content/images/2024/09/Crepuscular_Rays_Through_Cloudy_Sky.jpg)](https://www.keheka.com/content/images/2024/09/Crepuscular_Rays_Through_Cloudy_Sky.jpg)

_Crepuscular rays coming down through open pockets in the cloud layers._

Interestingly, we often focus on the bright part of godrays, but the volumetric **light** is only one half of the equation.

The other half of the godrays equation is volumetric **shadows**.

So we can sort of invert our thinking of godrays, if you will. It’s the blocking of light – the creation of volumetric _shadows_ – which causes godrays to appear as shafts of light in the air. (Otherwise it would just look like a smooth gradient of light).

[![](https://www.keheka.com/content/images/2024/09/Crepuscular_Rays_From_ISS.jpg)](https://www.keheka.com/content/images/2024/09/Crepuscular_Rays_From_ISS.jpg)

_Crepuscular rays, or godrays in the sky, as seen from above. They’re essentially just cloud shadows that form shafts of unlit (or less lit) atmosphere. (Here, you can also see that the sun rays are parallel, while in the earlier picture at ground level, the rays appear to fan out due to the perspective)._

> [!important]
> 
> Depending on the size of, and geometric detail in, the scene, volumetric lighting can be fairly heavy to calculate, and may be output as a completely separate render from the beauty and other AOVs.

The volume pass can be further split up into:

**1. Volume albedo**:

The volume texture pass, i.e. the raw volume colours/texture of the objects in the scene, without any lighting or shadowing.

**2. Volume direct**:

The volume albedo multiplied by the volume direct light contribution.

The direct pass will typically have the largest light contribution as the light that's hitting the camera with only a single bounce will have preserved the most energy.

**3. Volume indirect**:

The volume albedo multiplied by the volume indirect light contribution.

While the direct pass may have the largest light contribution, the indirect pass still provides that important bounce light for added realism.

> [!important]
> 
> The volume pass is an additive AOV (it’s volumetric _lighting_), but keep in mind that the medium that it is lighting up, such as fog or smoke, usually isn't. While the volume pass is added to the comp with a Merge (plus), [fog or smoke elements should be added with a Merge (over)](https://www.keheka.com/compositing-smoke-in-nuke/).

Once in a while you may have to create 2D volumetric lighting in Nuke.

A great way to do that is by [using the GodRays node together with the world position pass](https://youtu.be/PqbqxnBFOHg?feature=shared&ref=keheka.com).

Another method is to [use the VolumeRays node multiplied with an element](https://youtu.be/thXx_LNlPSU?feature=shared&ref=keheka.com).

There are also custom gizmos out there for creating volumetric lighting, like the [GodRaysProjector](https://vimeo.com/476747690?ref=keheka.com) or [X_Aton](https://www.xaviermartinvfx.com/x_aton/?ref=keheka.com).

### ↪ Background

The **background** pass contains emission from the background and skydome lights visible to the camera.

[![](https://www.keheka.com/content/images/2024/09/Background_AOV.jpg)](https://www.keheka.com/content/images/2024/09/Background_AOV.jpg)

_An example of a background pass._

I just wanted to add this pass in for completionist purposes, in case you encounter it. But I have only ever seen this pass being rendered in a full-CG animation production, and never in a VFX production.

More than likely, you’ll be using either the plate, a DMP (digital matte painting), or a separate skydome in Nuke as the background for your comp, and not this pass.

### Variations On The Same Thing

As mentioned before, different render engines will produce different variations of the shading components.

Some won't combine the raw lighting with the albedo, for instance.

But the AOVs above are generally the shading components you'll be working with in Nuke.

And remember, several of the AOVs listed above may not be relevant to your shot. There may be no refractions, emissions, sub-surface scattering, or volume fog happening in your shot, for example.

And so, the beauty rebuild using the shading components may very well _not_ be particularly complex at all. I've just listed the commonly possible AOV options above.

There _are_ other variations possible when deconstructing the beauty, however, which stray more or less from the norm:

### Different Puzzle Pieces

Shading components are not always split out into **albedo**, **direct**, and **indirect**.

You might sometimes get just a **diffuse** pass, just a **specular** pass, or just a **volume** pass, for instance – which contain all of their sub-passes combined.

Using diffuse as an example – but it applies to all the passes which split up this way – the passes break down like this;

**Diffuse**

=

**Diffuse direct** + **Diffuse indirect**

=

(**Diffuse direct lighting** + **Diffuse indirect lighting**) * **Diffuse albedo**

Or, rearranged:

(**Diffuse direct lighting** * **Diffuse albedo**) + (**Diffuse indirect lighting** * **Diffuse albedo**)

The shading components can also be broken down in _other_ ways.

For example, you could get a pass called **direct** which would contain all of the direct lighting in the scene (**diffuse direct**, **specular direct**, etc.), and a pass called **indirect** with all the indirect lighting.

There is also the option to render the passes **occluded** (default) and **unoccluded**, i.e. without shadows (no objects will occlude/block the light from others).

This can be useful when you want to have more control over the shadows, and change baked-in shadows after the fact in comp. (Which we'll look at later in the tutorial).

– And so when outputting shading components there are plenty of different ways of shaping the individual pieces that make up the beauty puzzle.

Let's put the pieces together:

### Shading Components: Beauty Rebuild

Some example sets of additive AOVs for rebuilding the full beauty are:

- Direct, indirect, emission, background.

- Diffuse, specular, coat, transmission, sss, volume, emission, background.

- Diffuse_direct, diffuse_indirect, specular_direct, specular_indirect, coat, transmission, sss, volume, emission, background.

Simply adding together the AOVs in each set (plus any unassigned remains) is all that is needed for rebuilding the beauty AOV.

But let's take it one step further and do a complete rebuild with all the passes, including the albedo passes. The setup can get quite large – too large to clearly see in a screenshot:

[![](https://www.keheka.com/content/images/2024/09/Full_Beauty_Rebuild_Shading_Components.png)](https://www.keheka.com/content/images/2024/09/Full_Beauty_Rebuild_Shading_Components.png)

_Full beauty rebuild (additive method) using all the common shading components._

So instead, I’ll zoom in and show the principle of how to build it using the diffuse AOV:

[![](https://www.keheka.com/content/images/2024/09/Full_Beauty_Rebuild_Shading_Components_Diffuse.png)](https://www.keheka.com/content/images/2024/09/Full_Beauty_Rebuild_Shading_Components_Diffuse.png)

_Full beauty rebuild (additive method), here zoomed in on the diffuse AOV to show the principle of building the setup._

> [!important]
> 
> Normally, I wouldn’t have an A-pipe go vertically into a Merge node like this, but I’d have to swap the Merge (divide) node with a MergeExpression node which can swap the inputs, or add more Dot nodes, to have the A-pipes come in horizontally. I figured this is the simpler, less confusing way of showing the setup in a screenshot.

You would build exactly the same setup for the rest of the AOVs, with the exception of the Emission AOV which doesn’t have an albedo or direct/indirect sub-AOVs. That one you would just Merge (plus) straight as it is.

> [!important]
> 
> Notice that none of the albedo passes are included in the unassigned remains calculation. Like we saw earlier in the _Light Vs. Texture_ section, the albedos are already baked into the direct and indirect AOVs.

### Light Groups

Light groups are usually much simpler to work with when it comes to rebuilding the beauty.

You just add them all together:

- Light group 01

- Light group 02

- Light group 03

- …

- Light group N

Light groups may have specific names instead of the generic **light group 01**, **LG01**, etc. – such as **key**, **environment** (HDRI), **fill**, **rim**, etc. – but will nevertheless contain one or more light sources grouped together.

(Or, they can be empty in some cases – for example when there is no rim light in your particular shot but the lighter is keeping the same light group structure and naming convention throughout the sequence).

### Light Groups: Beauty Rebuild

For the beauty rebuild using the light groups, you can take the same  
setup as before and just swap in the shuffled out light groups:

[![](https://www.keheka.com/content/images/2024/09/Full_Beauty_Rebuild_Light_Groups.png)](https://www.keheka.com/content/images/2024/09/Full_Beauty_Rebuild_Light_Groups.png)

_Full beauty rebuild (additive method) using the light groups._

### Combined Beauty Rebuild

Like we saw before, to combine the two beauty rebuilds, divide one of them, for example the shading components rebuild, by the original beauty render. Then, multiply the result of that by the other rebuild, in this case the light groups:

[![](https://www.keheka.com/content/images/2024/09/Full_Combined_Beauty_Rebuild.png)](https://www.keheka.com/content/images/2024/09/Full_Combined_Beauty_Rebuild.png)

_Full combined beauty rebuild (additive method) using both the shading components and the light groups._

That wraps up the primary AOVs. Next, let’s take a look at the auxiliary AOVs.

### Auxiliary AOVs

A lighter can essentially make any custom auxiliary AOV you like, but below you'll find the more commonly used ones:

### ↪ Cryptomattes

The **cryptomatte** AOV is a patchwork of interconnected ID mattes covering all the various parts of the scene.

The ID mattes can be segmented in multiple different ways, and are usually rendered into three separate cryptomatte sub-AOVs:

- **Crypto_asset**: Each type of (complete) asset in the scene gets its own uniquely  
    coloured ID matte. A barrel, for example, can be one single asset – made up of multiple objects (wooden boards, metal hoops, cork). Any copiesof that same barrel will fall within the same asset group and will getthe same ID (colour) in this sub-AOV.

- **Crypto_material**: Each type of material in the scene gets its own uniquely coloured ID  
    matte. The barrel from the previous example may have a wood material, ametal material, and a cork material. Any objects (e.g. other barrels) in the scene with the same wood material will get the same ID in thissub-AOV.

- **Crypto_object**: Eachobject, or part of an object, in the scene gets its own uniquelycoloured ID matte. A barrel will have multiple wooden boards, metalhoops, and a cork. Each of those could have its own ID. Or, if there are multiple barrels in the scene, each barrel could have its own ID.

[![](https://www.keheka.com/content/images/2024/09/cryptomatte_beauty.jpg)](https://www.keheka.com/content/images/2024/09/cryptomatte_beauty.jpg)

_The scene to be segmented into cryptomattes._

[![](https://www.keheka.com/content/images/2024/09/cryptomatte_asset.jpg)](https://www.keheka.com/content/images/2024/09/cryptomatte_asset.jpg)

_Cryptomattes segmented by asset._

[![](https://www.keheka.com/content/images/2024/09/cryptomatte_material.jpg)](https://www.keheka.com/content/images/2024/09/cryptomatte_material.jpg)

_Cryptomattes segmented by material._

[![](https://www.keheka.com/content/images/2024/09/cryptomatte_object.jpg)](https://www.keheka.com/content/images/2024/09/cryptomatte_object.jpg)

_Cryptomattes segmented by object._

Cryptomattes are incredibly useful for masking certain parts of your CG (for example to assist with grading), because you can pinpoint exactly which parts to include in your mattes.

The resulting mattes track perfectly with your CG and get occluded correctly, and, because they're  
procedural, you avoid manual rotoscoping.

### ↪ Z Depth

The **Z depth** pass represents the distance from the camera to the objects in the scene.

The pass is typically used for adding realistic **depth-based defocus** (depth of field) and/or **depth hazing** to your shot.

Depth of field is quite a heavy effect to render in 3D, and once it's baked into the render it's difficult to change it. So it's typically a much better option to apply the effect in comp instead, using the Z depth pass.

There are several ways of outputting Z depth information, and different software will produce different Z depth passes.

It can for example be rendered so that the pixel values equal the number of scene units there are from the camera to an object (i.e. the Z value in camera space), with higher and higher values the further away from the camera the object is.

Or, like the ScanlineRender in Nuke outputs it, it can be rendered as 1/Z, i.e. 1 divided by the number of scene units from the camera to the object. And there are several other variations.

> [!important]
> 
> If you hover your mouse pointer over the _math_ knob in the ZDefocus node's properties, you'll see a description of the most common types of Z depth passes.

[![](https://www.keheka.com/content/images/2024/09/z_depth.jpg)](https://www.keheka.com/content/images/2024/09/z_depth.jpg)

_A Z depth pass with increasing pixel values the further away objects are from the camera. (The Viewer has been gained down to be able to see the details)._

> [!important]
> 
> Because there is only one channel in the depth layer (namely _Z,_ which you can view in the red channel), a depth pass will typically look red in the Viewer. But since the depth values can be very high, you may have to lower the Viewer Gain by a lot, or normalise the pass first, to see any detail.

See the _Defocus_ section later on in the guide for how to use the Z depth pass for adding depth of field.

See the _Depth Cueing_ section later on in the guide for how to use the Z depth pass for adding depth hazing.

> [!important]
> 
> If you have the **geometry** and the **camera** available, you can create the depth.z pass by connecting the two to a ScanlineRender node. It will by default output a depth.z pass. You can also use the **blue channel** of the **camera position pass** as a depth.z pass – see the _Camera Position (PCam)_ section below.

Note that there are some limitations to using a Z depth pass for defocusing a render instead of rendering a true depth of field. For example:

- There isn’t pixel information for what’s _behind_ objects in the scene, so there can sometimes be edge issues or artefacts in the defocus where objects overlap.

- It’s not possible to focus on an object that’s reflected in a mirror.

[![](https://www.keheka.com/content/images/2024/09/True_DoF_Mirror.jpg)](https://www.keheka.com/content/images/2024/09/True_DoF_Mirror.jpg)

[![](https://www.keheka.com/content/images/2024/09/Focus_Distance_Mirror.jpg)](https://www.keheka.com/content/images/2024/09/Focus_Distance_Mirror.jpg)

_A scene rendered with true depth of field (left) which makes it possible to focus on a mirror reflection. And, how to calculate the focus distance for objects reflected in a mirror (right)._

The Z depth pass will only provide depth values for the surface of the mirror, and not for the additional distance to the object reflected in the mirror.

> [!important]
> 
> An object’s reflection in a mirror is equally far away from the surface of the mirror as the object itself is, only on the other side – in the ‘virtual’ space, if you will.

### ↪ World Position (PWorld)

This pass contains coordinates which describe where objects in the scene are located in world space.

[![](https://www.keheka.com/content/images/2024/09/World_Position.jpg)](https://www.keheka.com/content/images/2024/09/World_Position.jpg)

_A world position pass._

The red, green, and blue pixel values in the pass correspond directly to an object’s X, Y, and Z position (i.e. distance from the origin, positive or negative) in world space, respectively.

The world coordinates are static, so any animated objects will move _through_ these coordinates, receiving new pixel values depending on the object’s position in world space.

Static objects like buildings will keep the same PWorld pixel values on the same surfaces throughout the shot, though, when the camera moves around in the scene.

The PWorld pass is very useful for many different things, including:

- Creating PMattes, PNoise, and PRamps – see the _Position Masking_ section.

- Projecting CG/elements without geometry – see the _Projections_ section.

- Relighting the CG – see the _Relighting The CG_ section.

- Creating [point clouds](https://learn.foundry.com/nuke/content/reference_guide/3d_nodes/positiontopoints.html?ref=keheka.com) to use as reference for positioning Cards or other objects in the 3D scene in Nuke.

> [!important]
> 
> If you have the **geometry** and the **camera** available, you can create the world position pass in Nuke by connecting the two to a ScanlineRender node. Enable the _output vectors_ checkbox in the _Shader_ tab of the ScanlineRender, and choose a layer to output the world position pass to in the _surface points_ drop-down menu. (Here, you can also output a world normal vectors pass using the _surface normal_ drop-down menu).

The PWorld pass has many uses, and there are several variations of it which add even more functionality:

### ↪ Object Position (PObject)

This pass also contains coordinates similar to the PWorld pass.

However, the origin of the coordinate system is placed at the centre of the object in the scene, and if the object is animated, the coordinates will stick to it.

[![](https://www.keheka.com/content/images/2024/09/Object_Position.jpg)](https://www.keheka.com/content/images/2024/09/Object_Position.jpg)

_An object position pass._

> [!important]
> 
> If there is more than one object, each object will have its own origin and coordinate system sticking to the object. (Which means that any PMattes etc. must be masked by a cryptomatte or similar for the specific object, so that you don't affect all of them).

And so the same part of an object will keep the same coordinates, even when the object is moving or deforming. I.e. each part of the object will always have the same pixel values in the PObject pass when it and/or the camera moves around.

The PObject pass is useful in similar ways to the PWorld pass, but the difference is that any PMattes, PNoise, or PRamps will stick to animated objects.

### ↪ Reference Position (PReference)

This pass is sort of a mix between the PWorld pass and the PObject pass.

The PReference pass will match up to the PWorld pass on a certain reference frame, but will otherwise stick to every (animated/deforming) object in the scene.

So, on the reference frame, the PReference and PWorld passes will be identical, but on the frames before and after, the PReference pass will track with the animated objects in the scene.

> [!important]
> 
> You can imagine it as the world position coordinates being attached onto all the geometry in the scene on the reference frame, and then sticking to the objects as they move from there.

The PReference pass is useful for the same things as a PObject pass, plus you can also transfer any work done using the PWorld pass directly onto the PReference pass on the reference frame, and it will line up perfectly.

> [!important]
> 
> You can [convert PWorld to PReference in Nuke](https://www.keheka.com/converting-pworld-to-pref-in-nuke/).

### ↪ Camera Position (PCam)

The camera position pass is like a PWorld pass, but where the origin tracks with the camera.

That means the camera is always positioned at the origin, even when the camera is animated. And all the coordinates follow the camera.

So:

- The X-axis will always go from left to right across the frame in the red channel.

- The Y-axis will always go from bottom to top of the frame in the green channel.

- The Z-axis will always go from out of to into the frame in the blue channel.

[![](https://www.keheka.com/content/images/2024/09/PCam.png)](https://www.keheka.com/content/images/2024/09/PCam.png)

_A camera position pass._

This pass isn't commonly rendered out, but you can make it yourself in Nuke (see below), and it can be quite useful in certain cases.

You could use it to **replace a broken or missing depth.z pass**, for example:

The blue channel in the PCam pass represents the Z-axis, with the camera at the origin. So it's a straight up depth pass, with the pixel values representing the number of scene units from the camera to the objects in the scene.

The PCam pass could also be used for **creating a laser pulse/scanning effect** from the camera's point of view, going across a terrain:

You could animate a large PMatte moving out from the camera along the Z-axis, turn it into an [edge matte](https://www.keheka.com/perimeter-advanced-edge-mattes/), and use that for grading or overlaying the environment.

As mentioned before, you can make the PCam pass in Nuke – by [converting the PWorld into PCam](https://community.foundry.com/discuss/topic/161848/pworld-to-pcam-conversion-with-cam-abc?ref=keheka.com) using your camera and some matrix maths.

> [!important]
> 
> Several tools have already been made which can do this job for you, like [SpaceTransform](https://youtu.be/CORgfC5u0kU?feature=shared&ref=keheka.com).

### ↪ World Normal Vectors (NWorld)

This pass contains vectors describing where the surfaces of objects in the scene are facing in world space.

[![](https://www.keheka.com/content/images/2024/09/NWorld.png)](https://www.keheka.com/content/images/2024/09/NWorld.png)

_A world normal vectors pass._

The red, green, and blue pixel values in the pass correspond directly to the surface’s X, Y, and Z rotational values (in relation to the origin) in world space, respectively.

The values will range from -1 to 1, with a value of 0 meaning the surface is exactly aligned with that particular axis (not facing either the positive or negative direction).

Like the coordinates in the world position pass, the world normal vectors are also static. And so any animated object will receive new pixel values in the NWorld pass depending on the object’s orientation in world space.

Static objects will keep the same pixel values on the same surfaces throughout, though, when the camera moves around in the scene.

Normal vectors are most commonly rendered in world space (and often _only_ rendered in world space), as that's typically the most useful normals representation to compositors.

However, there is another handy variation:

### ↪ Camera Normal Vectors (NCam)

This pass is similar to the NWorld pass, except the normals are always aligned with the camera (i.e. screen space).

[![](https://www.keheka.com/content/images/2024/09/NCam.png)](https://www.keheka.com/content/images/2024/09/NCam.png)

_A camera normal vectors pass._

So, just like in the PCam pass, the red channel (X-axis) is always aligned  
left to right in frame, the green channel (Y-axis) is always aligned top to bottom of frame, and the blue channel (Z-axis) is always aligned in and out of the frame.

That means, no matter how an object is animated, its surfaces will always have the same values depending on how they are oriented relative to the camera.

I.e. the surfaces that are currently directly facing the camera will always be  
blue – the Z component, or blue channel, will have a value near or at 1.

This is very useful for [creating a facing ratio pass in Nuke](https://www.keheka.com/upgrade-your-light-wraps-using-facing-ratios/). (And you can convert NWorld to NCam in Nuke, if you don't have the NCam pass – also shown in the linked guide).

### ↪ Motion Vectors

Because of the additional temporal calculations that are necessary (i.e. calculating frames before and after the current frame), motion blur is heavy to render in 3D.

And so compositors will often receive a motion vector pass for applying motion blur to the CG render in Nuke, instead.

[![](https://www.keheka.com/content/images/2024/09/Motion_Vectors.jpg)](https://www.keheka.com/content/images/2024/09/Motion_Vectors.jpg)

[![](https://www.keheka.com/content/images/2024/09/Motion_Vectors_NoMB_Sphere.jpg)](https://www.keheka.com/content/images/2024/09/Motion_Vectors_NoMB_Sphere.jpg)

[![](https://www.keheka.com/content/images/2024/09/Motion_Vectors_MB_Sphere.jpg)](https://www.keheka.com/content/images/2024/09/Motion_Vectors_MB_Sphere.jpg)

_A motion vector pass (left) applied to a non-motion blurred render (middle) via a VectorBlur node in Nuke (right)._

The **motion vector** pass contains pixel values in only the **u** and **v** channels (the red and green channels, respectively). These values (which can be both positive and negative) represent the **number of pixels** and the **direction** that the surface of an object is moving across the frame in **screen space**, from one frame to the next – in X and Y, respectively.

And so from the motion vector pass we have the **amount** of movement (number of pixels across the frame) _and_ the **direction** of the movement (the angle between the current and the next/previous X and Y positions in the frame) available to us.

The [VectorBlur](https://learn.foundry.com/nuke/content/comp_environment/3d_compositing/adding_motion_blur_vectorblur.html?ref=keheka.com) node in Nuke will normally blur pixels out in a **straight line**, by the same amount, and along the same angle as specified in the motion vector pass, resulting in motion blur.

This is, of course, a fast approximation of motion blur. It will often give realistic results, but it has some limitations:

- **There will be no curved motion blur.** If an object is moving fast along a curved path, the motion blur generated by the VectorBlur will still only be rendered as a straight line. (The exception being if the VectorBlur node has access to **multiple motion samples per frame**, for example via the ScanlineRender node. Then, you could get curved motion blur with the VectorBlur).

- **There will be limited rotational motion blur.** If an object is spinning around, its edges will move less in screen space than its centre. And so the motion blur generated by the  
    VectorBlur node will (incorrectly) be weaker along the edges of the object.

- **Overlapping objects can cause problems.** If one object goes in front of another at a different speed or in a different direction, the VectorBlur node may have trouble blending the  
    two motion blurs correctly.

And although it's much quicker than the 3D software, the VectorBlur node in Nuke is still heavy  
to render, and can slow down your script.

So make sure to **precomp as needed**.

If you don't have a motion vector pass in your CG render, you can create one in Nuke using your **geometry** and **camera** connected up to a ScanlineRender node:

In the Shader tab of the ScanlineRender, select a layer to output the motion vectors to in the _motion vector channels_ drop-down menu.

Make sure to set the _motion vectors_ drop-down menu to **distance**. This is the recommended option that usually produces the best results.

It also allows the VectorBlur node to produce curved motion blur (where  
interpolation between two frames is made according to a curve rather than linearly).

Then, adjust the motion vector characteristics in the _MultiSample_ tab of the [ScanlineRender](https://learn.foundry.com/nuke/content/reference_guide/3d_nodes/scanlinerender.html?ref=keheka.com), and the motion blur characteristics in the VectorBlur node.

One of the most important settings to get right is the _shutter_ value, which needs to match up to the shutter angle that your plate was filmed with.

The _shutter_ value controls the length of the motion blur, and if your motion blur doesn't match what’s in the plate, the composite will feel off.

If you know the shutter angle or the shutter speed which was used when filming the plate, you can calculate the _shutter_ value:

_**Shutter**_ **Value = Shutter Angle / 360**

**Shutter Angle = Exposure Time (i.e. Shutter Speed) / Frame Interval × 360 Degrees**

At 24 frames per second, the frame interval is 1/24th of a second. An exposure time (shutter speed) of 1/48th of a second gives you a shutter angle of 180 degrees. (And vice versa).

Here are some common shutter angles and their corresponding _shutter_ values:

180 degrees = 0.5

90 degrees = 0.25

45 degrees = 0.125

> [!important]
> 
> Motion blur is a large topic in and of itself. You can learn more here: [Motion Blur Masterclass For Compositors](https://www.keheka.com/motion-blur-masterclass-for-compositors/).

### ↪ UV

**UV**s are two-dimensional texture coordinates that correspond with the vertex information of the geometry.

They provide the link between a surface mesh and how an image texture gets applied onto that surface. – UVs are basically marker points that control which pixels on the texture correspond to which vertex on the 3D mesh.

In order to apply a non-procedural 2D image texture to a 3D object, the object has to be unwrapped first.

That's a process where the surfaces of the 3D object are laid out flat onto a 1:1 square format image: a UV coordinate system.

[![](https://www.keheka.com/content/images/2024/09/Unwrapped_Car.jpg)](https://www.keheka.com/content/images/2024/09/Unwrapped_Car.jpg)

_An unwrapped model of a car (with a wireframe overlay)._

By placing image textures so that they line up with the surfaces that are laid out flat on the square image, you can connect the textures to the surfaces via the UVs – texturing your object.

However, if you want to replace or touch up the texture, and add some extra detail to an object after-the-fact in Nuke, you can instead apply a special image to your geometry as a texture – that covers the entire UV square.

In this special image, called an ST map, each pixel has a unique combination of pixel values:

[![](https://www.keheka.com/content/images/2024/09/UV1.jpg)](https://www.keheka.com/content/images/2024/09/UV1.jpg)

_A standard ST map._

This is achieved by making a ramp with pixel values from 0 to 1 along the width of the image in the red channel, and making another 0-1 ramp along the height of the image in the green channel.

Another option for creating the ST map in Nuke is to use an Expression node, like this:

[![](https://www.keheka.com/content/images/2024/09/ST_Map_Expression.png)](https://www.keheka.com/content/images/2024/09/ST_Map_Expression.png)

_Expression node for creating an ST map in Nuke._

You may also see ST maps that look like this:

[![](https://www.keheka.com/content/images/2024/09/UV2.jpg)](https://www.keheka.com/content/images/2024/09/UV2.jpg)

_Another variant of the standard ST map._

> [!important]
> 
> The only difference between the two ST maps above is that in the first one, the blue channel has pixel values of 0, and in the second one the blue channel has pixel values of 1. However, the blue channel is irrelevant; only the red (S) and the green (T) channels are used in the ST map. So the two ST maps are effectively identical.

When an ST map is applied as a texture to a 3D object, it might look something like this:

[![](https://www.keheka.com/content/images/2024/09/UVs_Wrapped.png)](https://www.keheka.com/content/images/2024/09/UVs_Wrapped.png)

[![](https://www.keheka.com/content/images/2024/09/UVs_Car.jpg)](https://www.keheka.com/content/images/2024/09/UVs_Car.jpg)

_ST maps wrapped onto geometry. The coordinates (i.e. the unique ST map colours) are now in object space, which means they’re now referred to as UV maps._

The examples above show what you would receive as a UV pass in your CG render.

Because the UV coordinates are linked to the vertices of the object, they will stick to the geometry and follow any animation or deformation.

You would typically use the UV pass together with the STMap node in Nuke to stick textures to the surfaces of the 3D objects in your CG render.

The STMap node is expecting the same ST coordinates, i.e. the same unique combination of pixel values for every pixel in the ST map that we saw before. No matter where they have been wrapped onto an object, it knows how to map an image to those coordinates, thereby applying the image to the surfaces of the 3D object.

This is useful if you want to touch up the texture and add some extra detail to a section of your render, for example.

However, you'll need to know (or experiment and find out) how the 3D object is unwrapped (i.e. where the surfaces of the object are laid out flat on the UV coordinate system) in order to line the textures up correctly.

> [!important]
> 
> If you have the **geometry** available, you can connect a [Wireframe](https://learn.foundry.com/nuke/content/reference_guide/3d_nodes/wireframe.html?ref=keheka.com) shader to it in Nuke and render it through a ScanlineRender node (no camera needed) which has the _projection mode_ set to **uv**. That way, you can more easily see where each part of the geometry is laid out on the UV coordinate system.

[![](https://www.keheka.com/content/images/2024/09/Face_UV_Wireframe.jpg)](https://www.keheka.com/content/images/2024/09/Face_UV_Wireframe.jpg)

_A wireframe render of the geometry for a character’s head, which is unwrapped on the UV_ coordinate system_. From this, it’s pretty easy to see where in the frame you would need to paint, or make other changes, to have your new texture line up with the CG render when applied via the STMap node._

> [!important]
> 
> You might wonder what the letters ‘UV’ stand for – but it's not an acronym. X and Y (and Z) have been used for ages in mathematics as the names for the axes of coordinate systems, and in CG for an object's position in 3D space. UVs could just as well have been called XYs, but to avoid confusion and to separate them as concepts (UVs also being coordinates that are attached to geometry; object space coordinates), an earlier letter pair in the alphabet was chosen – and so they were named UVs. And in exactly the same way, the STMap node in Nuke could just as well have been called the UVMap node, or the XYMap node, but an earlier letter pair was used (STs also being coordinates in texture space). The names are all just referencing **two-dimensional coordinate systems** in different spaces**.**

If you don’t have a UV pass in your CG render, but you do have the **geometry** and **camera** available, you can create the pass yourself in Nuke:

Create a square ST map using an Expression node like shown before, and apply it to your geometry using an ApplyMaterial node. Then, render it out through a ScanlineRender node using your camera.

[![](https://www.keheka.com/content/images/2024/09/Create_UV_Pass.png)](https://www.keheka.com/content/images/2024/09/Create_UV_Pass.png)

_Creating a UV pass in the red and green channels._

> [!important]
> 
> Here are a couple of [additional](https://benmcewan.com/blog/2020/02/02/demystifying-st-maps/?ref=keheka.com) [resources](https://youtu.be/pwSFZOwTc98?feature=shared&ref=keheka.com) about using ST/UV maps.

Note that 3D models can use regions of UV space outside of the 1x1 square we’ve seen so far. In fact, there can be multiple such squares per model, and it’s common to use one texture for each 1x1 square.

To organise and identify each square, we use the UDIM numbering scheme. Here’s how you can [import multiple UDIM patches](https://learn.foundry.com/nuke/content/comp_environment/3d_compositing/importing_udim_patches.html?ref=keheka.com) in Nuke.

  

### ↪ Camera Facing Ratio

The **camera facing ratio** pass is a matte with values from 0 to 1:

Any surfaces that are facing directly towards the camera will have a value of 0, and any surfaces facing directly perpendicular to the camera (90 degrees off to the side) will have a value of 1.

Surfaces that are facing somewhere in between these two angles will have gradual values between 0 and 1.

> [!important]
> 
> The pass is essentially the inverted Z component (blue channel) of the camera normal vectors.

[![](https://www.keheka.com/content/images/2024/09/Camera_Facing_Ratio_Matte.jpg)](https://www.keheka.com/content/images/2024/09/Camera_Facing_Ratio_Matte.jpg)

_A camera facing ratio pass created for a displaced Sphere in Nuke._

This pass is useful for many things, for example to make [lightwraps](https://www.keheka.com/upgrade-your-light-wraps-using-facing-ratios/) or [fresnel reflections](https://www.keheka.com/fresnel-reflections-in-nuke/) more realistic.

> [!important]
> 
> If you have the **NWorld** and the **camera** available, you can create the facing ratio pass yourself in Nuke – also described in the same [lightwraps](https://www.keheka.com/upgrade-your-light-wraps-using-facing-ratios/) article linked above.

### ↪ Shadow Matte

The **shadow matte** pass contains a black and white (0-1 greyscale) matte of the **direct shadows** in the scene.

[![](https://www.keheka.com/content/images/2024/09/Shadow_Matte.jpg)](https://www.keheka.com/content/images/2024/09/Shadow_Matte.jpg)

[![](https://www.keheka.com/content/images/2024/09/AO_SM_Scene.jpg)](https://www.keheka.com/content/images/2024/09/AO_SM_Scene.jpg)

_Shadow matte pass (left) and the unoccluded scene (right)._

It's calculated as the ratio of occluded direct lighting over unoccluded direct lighting. – In other words:

**Occluded direct lighting / Unoccluded direct lighting = Shadow matte**

So if you have both the **occluded** and the **unoccluded** direct lighting passes available, but not the shadow matte pass, you can split out the pass to treat it separately simply by dividing it out like  
described above.

And then, you would multiply the adjusted shadow matte pass back with the unoccluded direct lighting pass to create the new occluded direct lighting pass:

**Unoccluded direct lighting * Adjusted shadow matte = New occluded direct lighting**

> [!important]
> 
> See the _Cast Shadows_ section in the _Shadows_ chapter for more details on direct shadows.

The shadow matte pass is often rendered out inverted to what’s been described above. I.e. white/grey where there _are_ direct shadows, and black where there aren’t any direct shadows.

[![](https://www.keheka.com/content/images/2024/09/Shadow_Matte_Inverted.jpg)](https://www.keheka.com/content/images/2024/09/Shadow_Matte_Inverted.jpg)

_Inverted shadow matte pass._

If that's the case, you can use it as a mask for a Grade node (lower the _multiply_ knob), or just invert it before you multiply it with the unoccluded direct lighting.

### ↪ Ambient Occlusion

The **ambient occlusion** pass contains a black and white (0-1 greyscale) matte of the **indirect shadows** in the scene.

[![](https://www.keheka.com/content/images/2024/09/AO_Pass.jpg)](https://www.keheka.com/content/images/2024/09/AO_Pass.jpg)

_Ambient occlusion pass (same scene used as for the shadow matte pass above)._

It can be calculated as the ratio of occluded indirect lighting over unoccluded indirect lighting. – In other words:

**Occluded indirect lighting / Unoccluded indirect lighting = Ambient occlusion**

So if you have both the **occluded** and the **unoccluded** indirect lighting passes available, but not the ambient occlusion pass, you can split out the pass to treat it separately simply by dividing it out like described above.

And then, you would multiply the adjusted ambient occlusion pass back with the unoccluded indirect lighting pass to create the new occluded indirect lighting pass:

**Unoccluded indirect lighting * Adjusted ambient occlusion = New occluded indirect lighting**

> [!important]
> 
> See the _Ambient Occlusion_ section in the _Shadows_ chapter for more details on indirect shadows.

The ambient occlusion pass is sometimes rendered out inverted to what’s been described above. I.e. white/grey where there _are_ indirect shadows, and black where there aren’t any indirect shadows.

[![](https://www.keheka.com/content/images/2024/09/AO_Pass_Inverted.jpg)](https://www.keheka.com/content/images/2024/09/AO_Pass_Inverted.jpg)

_Inverted ambient occlusion pass._

If that's the case, you can use it as a mask for a Grade node (lower the _multiply_ knob), or just invert it before you multiply it with the unoccluded indirect lighting.

### ↪ Raw Illumination

Depending on the renderer used, you may already be getting raw lighting passes (i.e. only the lighting with no texture) as part of your beauty rebuild. (Or, you could split them out using the albedo like described earlier).

In any case, you may also receive _other_ raw lighting passes, which are not part of the beauty. These can be used for more creative work in Nuke – as part of look development, or to have control over the timing of an effect in comp:

For example, you could get always-on lighting passes for lightning strikes, or for police vehicle lights, which you can then animate on and off in comp as you please.

And just like with other raw lighting passes, you would Merge (multiply) these passes with the albedo, and then Merge (plus) that on top of your beauty rebuild.

> [!important]
> 
> Like with other types of lighting, these AOVs could also be split into direct and indirect components.

If you're using the passes to relight the _plate_, the same applies. So you have to create a sort of **albedo pass for your plate** to multiply the passes with. I.e. you have to flatten your plate first.

You can do that using this technique:

[Grade Stacking In Nuke](https://www.keheka.com/grade-stacking/)

Otherwise, the relighting will affect the highlights more than the shadow areas, which, of course, doesn't happen in real life.

### ↪ Other Utility Passes

The sky is the limit for auxiliary AOVs, and you may encounter other passes that are not listed above.

Things like custom mattes, particle age passes (for colour grading or fading off the particles over time, for example), or anything else which could be useful to you in comp.

### ↪ Deep

**Deep** data is a separate data set to the standard 2D AOVs we’ve seen so far, with its own set of [dedicated nodes](https://learn.foundry.com/nuke/content/reference_guide/deep_nodes/deep_nodes.html?ref=keheka.com) and [workflows](https://youtu.be/OFu93YJIIx8?feature=shared&ref=keheka.com) in Nuke.

Instead of one set of RGBA colour values and a Z depth value per pixel, deep data can have **multiple samples** of both for each pixel.

> [!important]
> 
> This article by fxguide explains deep really well: [The Art of Deep Compositing](https://www.fxguide.com/fxfeatured/the-art-of-deep-compositing/?ref=keheka.com).

And so what you get with deep data is an accurate volumetric representation of objects in 3D space, and the ability to automatically merge them together with the correct (and free!) holdouts.

For example, if you have a scene like this:

[![](https://www.keheka.com/content/images/2024/09/Boats_Fog.png)](https://www.keheka.com/content/images/2024/09/Boats_Fog.png)

_Boats on the water in dense fog._

– You could have the water, each boat, and the fog all rendered out separately (without holdouts) as deep renders. And when DeepMerging them all together (in any order), the boats would automatically:

- Be held out correctly by the water surface.

- Be layered correctly in front of each other.

- Have the right amount of fog in front of them based on how far into the fog they are.

> [!important]
> 
> > You could go even further, and split out individual parts of the boats as separate renders, and they would still merge together correctly with the rest of the scene.

And, should the client want to move, change, add, or remove a boat, you wouldn’t need to re-render the fog pass, for example. All the data for the fog is there already.

Which means, there would be no holes/gaps where an old boat used to be, and any new boats would be correctly embedded into the fog no matter where they're positioned.

So deep compositing is extremely flexible compared to normal 2D compositing. It allows you to **composite your scene in a complex volumetric way**, as opposed to simply layering elements one on top of the other.

Deep also gives you **clean, artefact-free edges when merging and defocusing** detailed things like hair/fur, because you have information about what's behind the objects in the scene.

And it greatly simplifies the compositing of elements like dense crowds, smoke, or particles, especially where **objects partially hold out other objects** in the scene.

However, all of the extra samples per pixel make the deep data **heavier to work with in Nuke**, and **more taxing on your storage**. Plus, many of your favourite **(2D) gizmos won’t work with deep data**.

> [!important]
> 
> You can work around this, however, by [compositing deep using a 2D workflow](https://www.keheka.com/deep-compositing-using-a-2d-workflow-in-nuke/).

You can also use deep data in conjunction with your typical 2D beauty render and AOVs. In fact, **this is often how studios will work with deep data** – by combining the two ‘worlds’ using the [DeepRecolor](https://learn.foundry.com/nuke/content/reference_guide/deep_nodes/deeprecolor.html?ref=keheka.com) node in Nuke.

Let's take a look at the deep AOV itself.

While there can be many samples per pixel (you can get a list of all the samples by using the [DeepSample](https://learn.foundry.com/nuke/content/reference_guide/deep_nodes/deepsample.html?ref=keheka.com) node) – for example where there are overlapping objects in the frame – you'll only be interfacing with two samples per pixel in the Viewer.

That is, when you view the deep layer in the Viewer, you'll see two channels:

- **Deep.front** (red channel): the front-most (i.e. closest to the camera) depth value.

- **Deep.back** (green channel): the furthermost depth value.

  

> [!important]
> 
> Because you're viewing depth values (and remember, objects can be hundreds of scene units away from the camera), you'll typically have to gain down the Viewer by a lot in order to see any details in the deep pass – just like you would with a depth.z pass.

[![](https://www.keheka.com/content/images/2024/09/Deep_AOV.png)](https://www.keheka.com/content/images/2024/09/Deep_AOV.png)

_A deep pass (Viewer gained down)._

Depending on the number of deep samples that have been rendered, each object in 3D space could have deep values for its frontside and backside, even where there are multiple objects overlapping in the frame.

Let's for example say that there is a person standing in front of a brick wall, facing the camera, and that they are holding a box in front of themselves, towards the camera.

[![](https://www.keheka.com/content/images/2024/09/Man_Box_Brick_Wall.jpg)](https://www.keheka.com/content/images/2024/09/Man_Box_Brick_Wall.jpg)

_A man in front of a brick wall, holding a box._

If you were to DeepSample a pixel on the box, you might see:

- **RGBA** values and a **deep.front** value for the camera-facing side of the box at that pixel.

- **RGBA** values and a **deep.back** value for the opposite side of the box at that pixel.

- **RGBA** values and a **deep.front** value for the camera-facing side of the person behind the box at that pixel.

- **RGBA** values and a **deep.back** value for the opposite side of the person behind the box at that pixel.

- **RGBA** values and a **deep.front** value for the camera-facing side of the brick wall behind the person at that pixel.

- **RGBA** values and a **deep.back** value for the opposite side of the brick wall behind the person at that pixel.

Which means, looking at all the pixels in the frame, you have information about _where_ everything is in 3D space, and _how much space_ each object takes up – i.e. their thickness.

This makes it very easy to composite something else into the scene.

The [DeepMerge](https://learn.foundry.com/nuke/content/reference_guide/deep_nodes/deepmerge.html?ref=keheka.com) node will slot in any object right where it belongs in 3D space for you, automatically – holding out the object correctly if it's behind or in between the other objects in the scene.

And likewise, the objects that are already there in the scene will be held out correctly by the new object.

And using the [DeepTransform](https://learn.foundry.com/nuke/content/reference_guide/deep_nodes/deeptransform.html?ref=keheka.com) node, you can even move the objects around in 3D space. (They will look exactly the same in the Viewer – same size and position in frame, etc. But they will move behind and in front of other objects because you're transforming the deep data).

So it's a really powerful way to work. And there's much more to it.

  

> [!important]
> 
> You can find useful tips and tricks for working with deep in the _Deep Compositing_ section.

Deep is not a part of the beauty rebuild, but rather, is more of a layering ‘assistant’ that we can use to gain more control over our CG composite. And even though it's the odd one out among them, I would still classify it as an auxiliary AOV. (Which is why I included it in this chapter).

– Okay, that wraps up the auxiliary AOVs.

With a solid fundamental understanding of AOVs and beauty rebuilds in place, let's continue by taking a look at how we can use it in practice for compositing CG renders.

When you've been briefed on your shot and begin setting up your comp, always take a close look first at all the material available to you and verify that everything is in order:

# Inspect The Lighting Renders

Look at any CG and FX that's been rendered and make sure to play through all of the frames.

**Do you foresee any issues which can't reasonably be solved in comp?**

If so, make sure to let Production, the relevant Supervisors, and the Lighting artist know as soon as possible.

Here are things to look out for:

### ↪ Missing Frames Or Renders

**Are all the frames of your renders working?**

I.e. there are no missing, black (empty), or corrupted frames in your renders.

Sometimes, even just a single, seemingly random render pass can error out on a frame during render time. Make sure to play through and check for any problematic frames in your lighting renders.

A temporary solution for fixing broken or missing frames is to use a tool such as [](https://www.nukepedia.com/gizmos/time/framefiller?ref=keheka.com)[FrameFiller](https://www.nukepedia.com/gizmos/time/framefiller?ref=keheka.com), which can generate new frames using the surrounding frames.

> [!important]
> 
> Any 2D frame replacements like this will soften the new frame and can introduce artefacts. It should be seen as a temporary solution only – to help get slapcomps out in time for dailies or a client review, for instance.

Next, **do you have all the renders you need?**

Check that all the different layers (e.g. BG, MG, FG, fog passes, etc.) have been rendered for your shot. – Keep in mind, you might be supposed to re-use renders from a different shot in the sequence. (See _Re-Using CG Renders_). Ask your Lead or Supervisor if you're unsure.

Lastly, if there's been an update to the sequence, **have all the relevant renders for your shot been updated?**

Make sure that you've got all the latest and greatest versions in order to keep your shot consistent with the rest of the sequence.

If the answer to any of the questions above is _no_, flag it as soon as possible with Production, the relevant Supervisors, and the Lighting artist.

### ↪ Artefacts

Related to the previous point, **are there any artefacts in your renders?**

A common artefact that appears in CG renders is the so-called **fireflies** – rogue pixels that are significantly brighter than their surrounding pixels. They typically jump around in the frame and flicker.

[![](https://www.keheka.com/content/images/2024/09/Fireflies.jpg)](https://www.keheka.com/content/images/2024/09/Fireflies.jpg)

_Fireflies (random small white dots) in a CG render._

Another type of problematic pixels are ones with **NaN** (Not A Number) values, **inf** (infinite) values, or **very high** or **negative** values. (Note that very high or negative values are not always unwanted).

All of these types of artefacts can often be fixed in comp pretty easily, using tools such as:

- [FireflyKiller](https://stefanmuller.com/wp-content/uploads/FireflyKiller.gizmo?ref=keheka.com)

- [mtPixelFixer](https://www.nukepedia.com/gizmos/draw/mtpixelfixer/?ref=keheka.com) (To kill [nan/inf values](https://youtu.be/o1tawUXh_lQ?feature=shared&ref=keheka.com))

- _Soft Rolloff_ (To compress highlights. *See the code below)

- A simple Grade/Clamp node

It's important to remove these artefacts, because filtering (e.g. blurring and distorting) problematic pixels can make the problem worse.

Instead of having for example a single pixel with an abnormal value in your image, suddenly you might have a cluster of corrupted pixels – because blurs or distortions will smear/spread the bad values out.

So make sure to **fix any artefacts first**, _before_ continuing to composite the CG renders.

Note that the tools above work best when there are normal, ‘healthy’ pixel values surrounding the broken pixels. Replacing a single rogue pixel will almost always go unnoticed, but that may not be the case for a block of for example 30 x 30 corrupted pixels.

So if there are larger clusters of fireflies or NaN/inf/very high/negative pixels, or if there are other significant issues like broken motion blur, then the CG may need to be kicked back and re-rendered.

- The code for the _Soft Rolloff_ tool (copy/paste into Nuke):

```Plain
set cut_paste_input [stack 0]
version 13.2 v2
push $cut_paste_input
Expression {
expr0 "limit * (1 - exp(-r/limit))"
expr1 "limit * (1 - exp(-g/limit))"
expr2 "limit * (1 - exp(-b/limit))"
name Soft_Rolloff
tile_color 0xff3fff
note_font_size 12
selected true
xpos -171
ypos -212
addUserKnob {20 User l Limit}
addUserKnob {7 limit l "Limit at" R 1 17}
limit 13.5
}
```

  

### ↪ CG Render Noise

**Is there any obvious render noise in your renders?**

Particularly in the early stages of creating the CG for a show, the 3D scenes will often be rendered with a low number of samples (i.e. with only a few light paths traced per pixel). Which means, therenders are prone to render noise.

Later on in the project, when the CG renders get closer to final, lighters tend to increase the number of samples to make the images cleaner. However, some noise may still remain.

Luckily, there is a lot we can do in comp to remove or mitigate render noise. I have written an extensive guide here:

[Removing Noise From CG Renders In Nuke](https://www.keheka.com/removing-noise-from-cg-renders-in-nuke/)

  

> [!important]
> 
> Depending on how bad the noise is, the best solution might be to kick it back for a re-render.

I'll include an excerpt from the guide that covers a very useful CG denoising method – vector-assisted frame averaging – which averages multiple frames together to cancel out the render noise:

This technique uses motion vectors to warp a number of frames before and after to all line up with the current frame. It then takes the mean or median of those selected frames to reduce the flickering/buzzing. And because the frames are aligned, blending artefacts are minimised.

It works best with renders which have a slow moving camera and slow moving animation in the scene, and which have no overlapping shapes:

- If the camera or the subject/object moves too fast, there won’t be enough  
    relevant pixel information to average between (from the surrounding  
    frames) to produce an accurate result. And you may inadvertently  
    introduce artefacts.

- If an object moves in  
    front of another, then multiple positions of that object will be  
    included in the calculation of the average pixel values. This may  
    introduce blending artefacts.

Building the setup for this technique can look a little bit complicated, but it essentially boils down to TimeOffsetting frames and IDistorting them into position using the inverted motion vectors. And then, taking the mean or median of the frames to clean up the noise.

I’ll link to some great tools that you can explore:

- [VectorMedian](https://www.nukepedia.com/gizmos/filter/vectormedian?ref=keheka.com)

- [VectorFrameBlend](https://www.nukepedia.com/gizmos/time/vectorframeblend?ref=keheka.com)

### ↪ Format

**Do you have enough resolution and sharpness in your renders?**

It's very common to render CG images at half resolution up until, and sometimes including, the final renders.

Take note of which resolution your images have been rendered at, and check if they look soft or pixelated compared to the scan.

In most cases where the CG has been rendered at a lower resolution than the working resolution of the shot, you should reformat the renders to the working resolution **before** starting any comp work on them.

That way, any work that you do which is resolution dependent – such as roto masks, warps, or transformations – won't break when the CG is later changed to a higher resolution.

> [!important]
> 
> Treat all CG renders as if they are full-resolution renders, even if they have only been rendered at half resolution. That way, all the work you do will still line up correctly later on when you _do_ receive the full-resolution renders.

However, keep in mind that reformatting the CG renders means they get **filtered**. That is, the original pixel values in the render are changed very slightly in the reformatting process, because neighbouring pixels are used to calculate new pixel values.

This can affect data passes which require exact or highly accurate pixel values to work correctly, for example cryptomattes:

[![](https://www.keheka.com/content/images/2024/09/Cryptomatte_No_Filtering.jpg)](https://www.keheka.com/content/images/2024/09/Cryptomatte_No_Filtering.jpg)

[![](https://www.keheka.com/content/images/2024/09/Cryptomatte_Filtered.jpg)](https://www.keheka.com/content/images/2024/09/Cryptomatte_Filtered.jpg)

_Filtering the cryptomattes will ruin the edges and introduce a host of artefacts. Unfiltered (left) vs. filtered (right)._

There are two solutions to this problem:

1. Reformat the high accuracy AOVs only _after_ processing them in the comp – e.g. after colour picking a cryptomatte – separately to the rest of the render passes which are reformatted _before_ being composited.

1. Use the _impulse_ filter when reformatting the high accuracy AOVs. This filter keeps the **original pixel values** when reformatting. Shuffle out and make a separate stream for the data passes, reformat them using the _impulse_ filter, and then copy them back in with the rest of the AOVs which are reformatted with a normal filter like _cubic_. And only then do comp work on the combined, reformatted CG.

While I would recommend defaulting to the **second option**, there are pros and cons to both of these solutions:

With the first solution, you can use the same filter in the Reformat nodes, which will give you the most accurate line up along edges. I.e. the same antialiasing will be applied to all of the render passes.

However, when the CG is rendered at full resolution later on, the work you’ve done (which was lined up to the lower resolution) could break, meaning you may have to redo some of it.

With the second solution you won't have that problem, however because the data passes are using a different filter than the rest, there may be minor alignment issues along edges. A cryptomatte will then have some aliasing, for example.

[![](https://www.keheka.com/content/images/2024/09/Cryptomatte_Post_Filtering_Cubic.jpg)](https://www.keheka.com/content/images/2024/09/Cryptomatte_Post_Filtering_Cubic.jpg)

[![](https://www.keheka.com/content/images/2024/09/Cryptomatte_Post_Filtering_Impulse.jpg)](https://www.keheka.com/content/images/2024/09/Cryptomatte_Post_Filtering_Impulse.jpg)

_The same edge filtered with a cubic filter (left) and an impulse filter (right). The impulse-filtered edge is a bit rougher and might not perfectly line up with a cubic-filtered edge._

Often, this won't cause an issue when compositing, but if it turns out to be problematic, you can use the first solution to match up the filtering.

(And this would only be the case when you aren’t getting full resolution renders for the final comp. Because if you are, then reformatting no longer becomes a factor).

### ↪ Coverage

**Is there enough coverage in your renders?**

Check that your CG renders actually cover what they need to cover in the shot, and that they aren't being cropped anywhere they shouldn't be.

For example, has enough overscan been rendered to cover the lens distortion?

Or, if you are 2D tracking in a still frame – is there enough overscan to cover that?

Another thing to look out for is if there are any bright lights or pixel values in the CG which are entering/leaving the frame. When you apply a defocus or a glow to the CG, you may get flickering along the edges of the frame if there isn't enough overscan.

The shot might also have a repo applied to it which further adds to the overscan requirement.

Also look out for things sticking out behind the CG. For instance, if a CG object is meant to cover up an object in the plate, is the CG object large enough in frame to cover all of it? (And for the amount of time needed)? Or would you perhaps need to paint out something behind the CG?

You may also be able to scale up, move, or warp your CG to cover up small bits that are sticking out behind it.

To do that you can use the classic [SplineWarp](https://learn.foundry.com/nuke/content/reference_guide/transform_nodes/splinewarp.html?ref=keheka.com) and [GridWarp](https://learn.foundry.com/nuke/content/reference_guide/transform_nodes/gridwarp.html?ref=keheka.com) nodes in Nuke. But my favourite tool for doing this is the ITransform gizmo (you can find [different](https://www.nukepedia.com/gizmos/transform/itransform?ref=keheka.com) [ones](https://www.nukepedia.com/gizmos/transform/transformwarp?ref=keheka.com) on Nukepedia).

It lets you soft mask transformations, which is great for nudging things into place. As opposed to the TransformMasked node, where the soft areas in the matte control the opacity, in the ITransform gizmo the soft areas control the strength of the transformation.

### ↪ Plate Alignment And Tracking

**Does the CG line up with the plate?**

Make sure to check with the lens distortion applied. This is especially important for set extensions, where the CG is meant to blend seamlessly with the partial set.

Like we saw before, if there is a minor misalignment, you can nudge the CG and line it up with the scan with a SplineWarp/GridWarp node or an ITransform gizmo. You can also use any combination of transformations using the [Warp Transform](https://www.keheka.com/warp-transform/) technique.

**Does the matchmove stick?**

In a similar vein, check if the CG tracks with the plate. Are there any bumps in the track, or is the track sliding over time? If so, you can animate a counter-Transform to make it stick.

> [!important]
> 
> It often helps to stabilise the camera move in order to see whether a track is slipping or not.

### ↪ Layering

**Has everything been layered in a sensible way in your renders?**

Or would you perhaps benefit from having parts of the render be split out from the rest?

When constructing your Nuke script it's usually a good idea to build your composite ‘back to front’. Starting with the layers that are the farthest from the camera at the top of your script, and then working your way forward in space as you composite downward in your script. – Layering up and [building a sense of depth](https://www.keheka.com/how-to-create-a-sense-of-depth-in-your-composite/).

Check whether the CG scene has been split up into logical layers or not. For example, are there any rogue foreground objects in a background layer, or vice versa, that are problematic when layering up your composite?

Would it help if the layers were organised in a different way? You may be able to request those changes for the next version.

### ↪ Holdouts

**Are there any problematic holdouts baked into the render?**

Sometimes, the holdout geometry for an object in the scan is inaccurate, and you get misaligned holdouts baked into your renders. It may be better to get the renders without holdouts and create them yourself in comp.

Other times, a lighter will hold out each render from each other. When merging them all together in comp, we run into an issue where we can see black lines along the seams.

That's because the Merge (over) operation _isn't_ expecting the inputs to be holding each other out, and so we get a double stencilling happening.

> [!important]
> 
> The Merge (over) operation layers image **A** over image **B** according to the alpha of image **A**, **a**, using the algorithm: **A+B(1-a)**.  
> – It's punching a hole in the background using the alpha of the  
> foreground. But when that hole has already been made before the merge,  
> we get the issue described above, along the semi-transparent edges.

To fix that, use the **Merge (disjoint-over)** operation instead. Let’s take a quick look:

[![](https://www.keheka.com/content/images/2024/09/Render_A.jpg)](https://www.keheka.com/content/images/2024/09/Render_A.jpg)

_Render A._

[![](https://www.keheka.com/content/images/2024/09/Render_B_Held_Out_By_Render_A.jpg)](https://www.keheka.com/content/images/2024/09/Render_B_Held_Out_By_Render_A.jpg)

_Render B, held out by render A._

[![](https://www.keheka.com/content/images/2024/09/Merge_Over_Edge_Issue.jpg)](https://www.keheka.com/content/images/2024/09/Merge_Over_Edge_Issue.jpg)

_Renders A and B merged together using a Merge (over). Notice the dark edge in the seam._

[![](https://www.keheka.com/content/images/2024/09/Merge_Disjoint-Over_No_Edge_Issue.jpg)](https://www.keheka.com/content/images/2024/09/Merge_Disjoint-Over_No_Edge_Issue.jpg)

_Renders A and B merged together using a Merge (disjoint-over). Notice there is no longer a dark edge in the seam._

It's similar to the over operation, except that if a pixel is partially covered by both the alphas of image **A** and image **B**, **a** and **b**, disjoint-over will assume that the two objects do not overlap.

> [!important]
> 
> For reference, the disjoint-over algorithm is: **A+B(1-a)/b, A+B if a+b<1**.

**Are there any missing holdouts which can't easily be recreated in comp?**

Other times, there might be some complex layering needed, where parts of the CG go in front and other parts go behind something in the plate. And the lighter may have accurate holdout geometry available. In those cases, it can be useful to have a render come with certain holdouts baked in.

So make sure to check that you have everything you need.

### ↪ Shadows

**Have any problematic shadows been baked into the render?**

If so, do you have the necessary render passes to remove them?

Earlier, we briefly talked about the **occluded** and **unoccluded** render passes. The unoccluded passes contain no shadows, and so they can be used to remove baked-in shadows from a render.

By keymixing in the unoccluded pass with your occluded one, you can kill specific unwanted shadows in your renders.

Or, if you want to further _darken down_ the shadows, you can divide an occluded pass by its unoccluded counterpart. That way, you isolate the shadows. Multiply those with your beauty rebuild to darken down the shadow areas.

If you don't have the necessary passes to alter the shadows you can request them, or ask if the lighter can disable shadow casting/receiving for the problem area.

**Are there any missing shadows which can't easily be recreated in comp?**

Shadows that are moving across, or being cast by, complex geometry can be tricky to recreate in comp.

While we can do a lot (which we’ll take a look at later in the _Shadows_ chapter), it might instead be better to create them in 3D.

### ↪ Additional Necessities

**Do you need anything else which has not currently been provided?**

This could for example be Deep renders, or any particular AOVs such as cryptomattes, a position pass, or a normal pass.

It could be a camera (or a retimed camera), lens distortion information, or certain metadata.

Chances are that you'll get it – **if you request it** – especially if it's a big time saver.

### ↪ Other Concerns

**Is there anything else which seems wrong or doesn't make sense?**

Double-check everything to be sure, see if/where you run into issues, and ask for clarification if needed.

The best way to check that everything is in order, is to set up a slapcomp:

# Make A Slapcomp

It's good practice to make a slapcomp with all the elements represented – with minimal to no adjustments – and make sure that what's come out from 3D is working as intended.

The lighting artist will often do this, but you should also make one yourself to verify. Lighters are naturally often not that highly skilled in the technical aspects of compositing in Nuke, and **may make mistakes** in their slapcomps.

I've previously written a guide on how to make a slapcomp in Nuke, which you can read here:

[How To Make A Slapcomp In Nuke](https://www.keheka.com/how-to-make-a-slapcomp-in-nuke/)

You'll probably discover 90+% of any issues with the CG renders doing this task alone.

> [!important]
> 
> A slapcomp is not meant to have refined edge work and perfect integration – it's just a comp with all the various elements ‘slapped together’. However, that doesn't mean your Nuke script should be a tangled mess. Structure and organise your slapcomp as you would a real comp. Set up and use the elements as closely to the intended way as you can, so that you can spot actual problems that will appear once you comp it all for real.

Depending on your company’s workflow, you might publish the slapcomp and view it in dailies with the Supervisor to ensure nothing is broken. Or, you might make a bit more refined version 001 comp first.

In any case, once you’ve verified that everything is working as intended, you’re ready to comp the shot for real.

Before you start, let’s take a look at some quick tips for speeding up your workflow and making things easier for yourself:

# CG Compositing Tips

There are lots of things you can do to speed up your workflow, make your work more consistent, and make your composites less prone to error.

Here are some tips:

### Cycling Through Layers

When looking through the layers of your CG render, you don't have to  
manually select each one in the layer drop-down menu in the Viewer.

Instead, use the **PageUp** and **PageDown** keys on your keyboard with the Viewer panel highlighted in order to cycle through each render layer.

### Shuffling Out Layers

With often dozens of AOVs, it’s tedious to shuffle out all the layers and label them.

Instead, you can use a Python script to do all the work for you in seconds. I’ve written a guide showing how to make one (among other things), which you can [read here](https://www.keheka.com/automating-tedious-tasks-using-python-in-nuke/).

### Channel Management

To keep your Nuke script light and responsive, it's important to manage _which_ channels are available, and _where_, in your script.

CG renders often come with dozens of render passes, and many Nuke nodes/gizmos will by default process **all** the layers and channels. All of that processing can quickly grind a script to a halt, and a lot of it is completely unnecessary.

A ZDefocus node can (and should often) be set to affect only _rgba_ instead of _all_ channels, for example. The same goes for any other nodes and gizmos, particularly heavy ones, which are affecting all channels.

Complimenting that, use the Remove node to remove or keep only the channels you need along the way as you comp. It keeps your stream tidy, and serves as a backup safety if you accidentally leave a node set to process all channels.

So, remember to:

1. Only **keep** the channels you need to use downstream.

1. Only **affect** the channels you need to affect in your stream.

> [!important]
> 
> The default Remove node only lets you remove or keep four layers. If you need to remove or keep more, you can use a gizmo such as [k_Remove](https://www.nukepedia.com/gizmos/channel/k_remove?ref=keheka.com) instead of creating multiple Remove and Copy nodes.

### Making Templates

Once you get the jist of it, you’ll find that CG compositing – at least the base setup of it – is pretty formulaic and repetitive.

Especially, if the company you work for has standardised their AOVs and naming conventions.

And so you can save a ton of time by making a standard template in Nuke to use as a base for when you start a new CG composite.

The primary AOV beauty rebuilds we saw earlier are a great example of this. Make them once, save them as a template or ToolSet, and reuse them any time thereafter.

Okay, let’s dive into comping your CG.

# Full-CG Vs. Live-Action + CG

Like we saw in [How To Make A Slapcomp In Nuke](https://www.keheka.com/how-to-make-a-slapcomp-in-nuke/), there are basically three types of CG compositing, plus combinations of them:

1. **Full-CG shot**, for example a full-CG environment (note; a DMP sky or background is common in ‘full-CG’ shots).

1. **Scan on top of the CG**, for example background replacements.

1. **CG on top of the scan**, for example inserting a CG object into the scan.

The basic approach is essentially the same for all of them when it comes to working with the CG renders, except with full-CG shots you may not have a plate to match your CG to.

You should still aim to **keep your composite photographic**, and use real plate references to match the lens qualities to, taking the CG curse off.

However, I wanted to make an important point about compositing CG renders with a plate.

If any part of the plate should go in **front** of the CG, there are two ways compositors will usually deal with that:

**The wrong* way:**

1. Making a roto for the part of the plate which should go in front.

1. Using that roto to stencil out the CG render.

1. Merging the held out CG over the plate.

- The problem with doing it this way is that **the original background will show through** in the soft edges of the holdout. And you have **no control over adjusting the plate edges** that will go on top of the CG.

**The right way:**

1. Merging the CG over the plate with no holdouts.

1. Making a roto for the part of the plate which should go in front.

1. Copying the roto alpha (A) to the plate (B) using a Copy node, and then  
    premultiplying – resulting in a patch with just the area that should go  
    back on top.

1. Merging the patch back over the plate with the CG on it.

1. Adding any edge extensions, lightwrap, etc. to the patch to integrate it properly over the CG.

> [!important]
> 
> > Make sure to **mask the patch by the alpha of the CG** in step 4 so that you’re not introducing plate-on-plate edge extensions or other unwanted changes in step 5.

Now, you’ll for example have full control over the motion blurred edges of the foreground plate, and can remove any unwanted bits of the original background showing through.

# Colour Grading

Colour grading is a a major part of CG compositing, and there are two sides to grading the CG:

1. **Technical grading** - Making sure that the exposure and dynamic range of the CG matches the scan; that the colour temperature and black levels are correct; and  
    that there aren’t any problematic pixel values, like _NaN_ or _inf_.

1. **Creative grading** - Making sure that the CG looks good and tells the story in a clear  
    way. That might for example mean artificially brightening up or  
    increasing the saturation of a specific object in order to draw the  
    viewer’s attention to it, or creating a vignetting effect by darkening  
    down the surrounding environment.

When doing any sort of grading on CG renders which have an alpha channel, make sure to work on the unpremultiplied CG to avoid edge issues:

### Un-/Premultiply

While we typically work with the **RGBA** channels of an image – and these four channels belong to the same layer in Nuke – it’s important to make a distinction and separate the **two different concepts** at play:

We have to differentiate an object’s **colours** (the RGB channels) from the object’s **opacity** (the Alpha channel).

These are two separate things, which are combined together with a **multiplication** to get an image which can correctly be composited with other images. That is, the red, green, and blue channels are each multiplied by the alpha channel to get an image with the right colours _and_ the right opacity.

When you multiply an object’s colours (RGB) with the object’s opacity (Alpha), the colour values in the areas where the alpha channel has a value of 1 stay the same. (Because anything multiplied by 1 remains unchanged).

But where the alpha channel has a value lower than 1, the multiplied colours become darker – all the way down to becoming completely black where the alpha channel has a value of 0.

This is fine if you don’t apply any further grading to the image, as the colours and the opacity so far are correctly balanced.

However, most of the time, the CG renders you will receive already have this multiplication baked into them – they come “**premultiplied**”.

Which means, where the alpha channel has a value of 1, the corresponding RGB colour values are the true colours of the object.

But where the alpha is less than one, for example along the edges of the object, you’re no longer dealing with the true colours of the object. In these areas, the original true colours have been darkened down by the multiplication.

Naturally, these darker colours won’t respond the same way as the true colours of the object will to your various grading operations in Nuke. This normally means that if you grade on the premultiplied image, you’ll end up with dark edges along the outline of your object.

[![](https://www.keheka.com/content/images/2024/09/Grading_On_Premulted_Image_Edge_Issues.jpg)](https://www.keheka.com/content/images/2024/09/Grading_On_Premulted_Image_Edge_Issues.jpg)

[![](https://www.keheka.com/content/images/2024/09/Grading_On_Unpremulted_Image_No_Edge_Issues.jpg)](https://www.keheka.com/content/images/2024/09/Grading_On_Unpremulted_Image_No_Edge_Issues.jpg)

_Grading on the premultiplied image can cause edge issues (left). Grading on the unpremultiplied image fixes those edges issues (right)._

> [!important]
> 
> Grading on premultiplied images means that you’re not grading on the true colours of the objects in the images.

In order to grade on the true colours of your CG, you’ll have to undo the premultiplication that came baked in with the renders, first – you’ll have to **unpremultiply** your CG (which essentially means **dividing** it by the alpha).

[![](https://www.keheka.com/content/images/2024/09/Premulted_Image.jpg)](https://www.keheka.com/content/images/2024/09/Premulted_Image.jpg)

[![](https://www.keheka.com/content/images/2024/09/Unpremulted_Image.jpg)](https://www.keheka.com/content/images/2024/09/Unpremulted_Image.jpg)

_Premultiplied image (left) vs. unpremultiplied image (right)._

So:

1. Apply the _Unpremult_ node to your CG render to restore the true RGB colours.

1. Do all your grading operations after the _Unpremult_ node, i.e. on the true RGB colours.

1. Apply the _Premult_ node after the grading operations, to bring the image back into the state  
    where it can be composited correctly with other images.

1. Note: If any of your grading operations have affected the alpha, make sure to copy in the original alpha just before the _Premult_ node.

> [!important]
> 
> Most of the colour grading nodes in the _Color_ menu in Nuke have the option to perform an unpremultiplication before, and a premultiplication after, the grading operation – using the **(un)premult by** setting (a.k.a. the _unpremult_ knob).

This is also why it’s so important to always keep your alpha channel values between 0 and 1. Otherwise, all of the maths behind colour grading and opacity in Nuke will break.

So make sure to **clamp any alpha values below 0 or above 1**.

By understanding the relationship between RGB and A, you can also more easily deconstruct things from real life in order to replicate them.

For example, a thick, dark plume of smoke will have low RGB values (dark colours), but high Alpha values (very opaque).

Light cigarette smoke, on the other hand, will have high RGB values (light colours), but low Alpha values (nearly see-through).

Smoke from a volcanic eruption wouldn’t look correct if it was nearly see-through (i.e. a low alpha value), and cigarette smoke (unless it was dense vape smoke) wouldn’t look right if it was very opaque (i.e. a high alpha value).

Okay, with this in mind, let's move on and look at grading.

### Divide And Conquer

Colours have perceived properties such as hue, saturation, and brightness.

When colour grading your CG renders, it’s useful to break down the grading into individual tasks, focusing on one property at a time – similar to how you would look at each R, G, and B channel individually when matching grain.

Most of us don't have [super human colour vision](https://www.allaboutvision.com/conditions/retina/tetrachromacy/?ref=keheka.com), and even for those who do, separating the grading work can help with getting a better colour match.

In a Grade node, for example, you could use the _gain_ slider for dealing with the brightness only, and the _multiply_ sliders for adjusting the hue.

> [!important]
> 
> By using some hotkeys, you can also [individually adjust the hue, saturation, and brightness](https://www.keheka.com/secrets-of-the-grade-node/) of the knobs in a Grade node.

Let’s look at the individual grading tasks for colour grading your CG renders:

### Exposure & Dynamic Range

It's important to understand the [exposure](https://en.m.wikipedia.org/wiki/Exposure_\(photography\)?ref=keheka.com\#Optimum_exposure) and the [dynamic range](https://www.cambridgeincolour.com/tutorials/dynamic-range.htm?ref=keheka.com) of your plate, because together they determine the brightness boundaries that you must work within.

A camera is able to capture a certain range of light intensities, and will clip anything which is darker or brighter than that range. This is called the dynamic range.

And, unless we stack exposures or grade up or down dark or bright areas, only a part of that range will be visible to our naked eye in full detail at a time.

For example, if a shot is exposed for an interior room, then the sunlit exterior seen through the windows will look blown out (overexposed). Or, if it were exposed for the exterior seen through the windows, then the interior would look dark.

[![](https://www.keheka.com/content/images/2024/09/Living_Room.jpg)](https://www.keheka.com/content/images/2024/09/Living_Room.jpg)

_When exposing for the interior, we can often barely see anything outside._

Our eyes work the same way. You may be able to see perfectly outside in the sun, but when driving into a tunnel, your eyes have to adjust before you see clearly again.

This is just the way cameras work. Exposing for _both_ the interior and the exterior at the same time typically isn't possible without manipulating the exposure in some way.

> [!important]
> 
> Next time you're looking at real estate ads, pay attention to the interior shots. They will often stack exposures so that you can see the exterior in the same picture, and it often makes the images look doctored.

So, when grading your CG, make sure that you [match the dynamic range of the scan](https://www.keheka.com/matching-the-dynamic-range-of-a-scan-in-nuke/).

Ensure that you are correctly exposing the CG so that it sits in with the rest of the scene. Gain the Viewer up and down, and sample the brightness values in the plate and in your CG to check that they both live in the same world.

And if there are brightness changes in the plate which should affect your CG, make sure to [match the plate lighting](https://www.keheka.com/add-or-remove-flickering-in-nuke/).

With exposure, it's all about finding the right balance; where the CG looks visually interesting while still being plausible and believable.

To help get the brightness right, you can split the grading task into highlights, midtones, and shadows areas. See the _Matching The Dynamic Range Of A Scan In Nuke_ article linked above for tips.

> [!important]
> 
> If you need to tone down the highlights of an element, a great technique to use is [Grade Stacking](https://www.keheka.com/grade-stacking/).

And it _is_ quite important to get the brightness right:

### Luminance Vs. Colour

The human eye is much more sensitive to variations in brightness than in colour. It's why [chroma subsampling](https://en.m.wikipedia.org/wiki/Chroma_subsampling?ref=keheka.com) exists.

For viewing purposes, we can typically ‘get away with’ images with lossy compression. I.e. discarding a lot of the colour information while keeping the luminance information intact. Visually, the image still looks nearly identical to our eyes.

[![](https://www.keheka.com/content/images/2024/09/444.jpg)](https://www.keheka.com/content/images/2024/09/444.jpg)

[![](https://www.keheka.com/content/images/2024/09/422.jpg)](https://www.keheka.com/content/images/2024/09/422.jpg)

_Uncompressed 4:4:4 image (left) vs. compressing 50% of the colour information to 4:2:2 (right)._

> [!important]
> 
> The image on the left above doesn’t really look much different to the image on the right, and it’s the same resolution and image format – yet it’s almost half the file size due to the chroma subsampling. (360KB vs. 602KB).

Now, of course, for chroma keying and other comp tasks which require precise pixel values, the less compressed our footage is, the better. But this brightness-inclination of the human eye _does_ say something about where we should focus our creative efforts the most – at least initially:

Nail down the brightness of your CG first.

If there is something very bright in the frame, it will naturally attract the viewer’s attention first. (Or, if the majority of the frame is bright, the darkest part will).

So, make sure the most important element (a story point, for instance) is not competing against other unimportant but attention-grabbing elements in the shot.

– Grade up/increase the contrast of the important things in the frame, and grade/dull down the unimportant things.

You may for example get a CG render where, due to the camera angle, a certain part of a building or other object gets a really bright and distracting highlight at some point during the shot.

While probably technically correct, the distraction doesn't serve the shot, and so it's common to grade things like that down until they no longer are a distraction.

And brightness can be more important than that:

### Highlighting Story Elements Using Light And Shadow

Light and shadow can be crucial in telling the story.

For example, small but important elements of the frame can get lost in an environment if you don’t carefully highlight them:

[![](https://www.keheka.com/content/images/2024/09/Leading_Lines_DL.jpg)](https://www.keheka.com/content/images/2024/09/Leading_Lines_DL.jpg)

_The light is guiding your eyes to the important part of the image._

Below are some more examples of light and shadow being used to focus the viewer’s attention to particular areas of the frame:

[![](https://www.keheka.com/content/images/2024/09/Leading_Lines_TO.jpg)](https://www.keheka.com/content/images/2024/09/Leading_Lines_TO.jpg)

[![](https://www.keheka.com/content/images/2024/09/Leading_Lines_KE.jpg)](https://www.keheka.com/content/images/2024/09/Leading_Lines_KE.jpg)

[![](https://www.keheka.com/content/images/2024/09/Leading_Lines_CO.jpg)](https://www.keheka.com/content/images/2024/09/Leading_Lines_CO.jpg)

[![](https://www.keheka.com/content/images/2024/09/Leading_Lines_FL.jpg)](https://www.keheka.com/content/images/2024/09/Leading_Lines_FL.jpg)

Or, imagine it's night time in a sci-fi cyberpunk city. The main character of the story is exiting a building. The camera is far away, showing the entire building and a portion of the city surrounding it. The main character is tiny in the frame, and so could easily be missed by the viewer.

In shots like these, when we have to draw attention to a main character or an important element, brightness and contrast are useful tools that can help.

Adding street lamps, godrays, or other light sources that shine on the character, or in other ways guide the viewer’s eyes towards the character, is very effective. You can use light to make [leading lines](https://www.adobe.com/uk/creativecloud/photography/discover/leading-lines-photography.html?ref=keheka.com), for example.

As a very basic method, you can often even just grade up the area where the main character is located using a large, soft matte, and/or darken down surrounding areas in the same way.

> [!important]
> 
> Keep the visual continuity of the sequence in mind when making grading changes like this.

Playing with the exposure in isolated areas of the frame to highlight important elements is very effective. And often, you may get client notes asking to increase or decrease certain areas by for example “half a stop”.

If you’re not familiar with cameras, that could be confusing. So let’s take a look at what they’re referring to:

### F-Stops

In VFX, we quite naturally will often communicate in photographic terms.

When it comes to brightness, we talk about it in terms of exposure that’s measured in (f-)**stops**.

However, in Nuke we mostly work with **gain**/**multiply** in relation to brightness. (There _is_ also the _Exposure_ node, but the Grade node is far more popular). To convert between the two we need to do a bit of maths:

To convert from Stops (f) to Gain: **Gain = 2**f

And to convert from Gain to Stops (f): **f = log**2 **Gain**

This means:

- Increasing the brightness by 1 stop is equal to doubling the brightness – a gain or multiply of 2.

- Increasing the brightness by 1/2 stop equals a gain or multiply of ~1.41.

- Increasing the brightness by 1/4 stop equals a gain or multiply of ~1.19.

And increasing the brightness by 2 stops is equal to quadrupling the brightness – a gain or multiply of 4. And so on.

And the other way:

- Decreasing the brightness by 1 stop is equal to halving the brightness – a gain or multiply of 0.5.

- Decreasing the brightness by 1/2 stop equals a gain or multiply of ~0.71.

- Decreasing the brightness by 1/4 stop equals a gain or multiply of ~0.84.

And decreasing the brightness by 2 stops is equal to quartering the brightness – a gain or multiply of 0.25. And so on.

> [!important]
> 
> You could also think of it as every half stop up/down equals a multiplication/division of the gain by the square root of 2. (Which is what the Viewer Gain controls in Nuke do).

Okay, that wraps up exposure and dynamic range.

Next, let's look at hues:

### Colour Temperature

Colour temperature is a way to describe the colour of visible light by comparing it to the colour of the light emitted by a [black body](https://en.m.wikipedia.org/wiki/Black_body?ref=keheka.com).

When you heat up a black body it starts to emit light. As the temperature rises, the colour of the light goes from red → orange → yellow → white → bluish white.

> [!important]
> 
> Counter-intuitively, (relatively) cooler temperatures produce what we classically think of as warmer colours (reds/oranges/yellows), while hotter temperatures produce cooler colours.

[![](https://www.keheka.com/content/images/2024/09/ColourTemp.png)](https://www.keheka.com/content/images/2024/09/ColourTemp.png)

_The colours of the light emitted by a black body at various temperatures._

Temperature is the sole factor that’s affecting the colour of the emitted light from a black body. So we can use the equivalent black body temperature to define the colour of a light.

- At a temperature of 1700 [Kelvin](https://en.m.wikipedia.org/wiki/Kelvin?ref=keheka.com) (K), the colour of the light emitted from a black body will match the colour of the light that's emitted from a [low pressure sodium-vapour lamp](https://en.m.wikipedia.org/wiki/Sodium-vapor_lamp?ref=keheka.com#Low-pressure_sodium).

- At a temperature of 1850 K, the colour of the emitted light will look like a candle flame or a sunset.

- At 2400 K, it will look like a standard incandescent lamp.

- At 2700 K, it'll appear like a "Soft white" compact fluorescent or LED lamp.

- At 3200 K, it'll look like photographic studio lamps.

- At 5000 K, it's like horizon daylight.

- At 6500 K, it's like overcast daylight and shade.

- And at 6500-9500 K, it looks like a LCD screen.

> [!important]
> 
> This is why you'll see light bulbs classified by colour temperature.

And so depending on the type of lighting in the scene, and the [white balance](https://photographylife.com/definition/white-balance?ref=keheka.com) set in the camera or in post, the colours in your shot may be tinted warm or cool.

When we composite things into the plate, we have to match this tint. So, make sure to pay attention to the colours in the plate – do they lean towards **warm** or **cool**? And how **saturated** are the colours?

[![](https://www.keheka.com/content/images/2024/09/Sodium-Vapour.jpg)](https://www.keheka.com/content/images/2024/09/Sodium-Vapour.jpg)

_Sodium-vapour street lamps tinting the scene orange._

[![](https://www.keheka.com/content/images/2024/09/Fluorescent.jpg)](https://www.keheka.com/content/images/2024/09/Fluorescent.jpg)

_Fluorescent lights typically provide a much cooler light._

[![](https://www.keheka.com/content/images/2024/09/Neon.jpg)](https://www.keheka.com/content/images/2024/09/Neon.jpg)

_Neon lights colouring the surroundings pink._

[![](https://www.keheka.com/content/images/2024/09/Blue_LED.png)](https://www.keheka.com/content/images/2024/09/Blue_LED.png)

_A white sheet of paper under blue LED lighting._

[![](https://www.keheka.com/content/images/2024/09/Dune.jpg)](https://www.keheka.com/content/images/2024/09/Dune.jpg)

_Heavily orange-tinted scene from Dune. Even a cool blue object in the scene would barely look blue at all due to the heavy tint._

[![](https://www.keheka.com/content/images/2024/09/WB.jpg)](https://www.keheka.com/content/images/2024/09/WB.jpg)

_A landscape captured with different white balance settings. Anything composited into these two images would need a very different grade applied for each scenario._

The objects in your CG render will have a colour, a hue, which may be tinted by the lighting in the plate.

> [!important]
> 
> Different areas in the plate can be lit by different types of lighting (for example indoor and outdoor lighting), and will be tinted differently. Make sure to adjust and/or mask the grading of your CG and elements accordingly.

### Adjusting The White Balance

You can use the **whitepoint** and **gain** (_white_) knobs in a Grade node to make changes to the white balance of your CG.

If there are objects in your plate and your CG render which you know are white, like a white wall or a white piece of paper, you can use them to semi-automatically set the correct white point in your CG:

(The white objects may be tinted in the plate or in the CG, but we’re talking about the true colour of the object here).

1. Connect a Grade node to your CG render and view it. (Make sure to un-/premultiply the grade).

1. Colour pick a white object in your CG render using the **whitepoint** knob in the Grade node. This will neutralise any tint and change the colour of the object to be true white.

1. View the plate, and colour pick a white object in your plate using the **gain** (_white_) knob in the same Grade node. This will shift the true white colour (that we set before) to match the tint of the plate.

1. The Grade will now shift the white balance of the CG to match up with the plate.

💡

When you colour pick white objects, ideally find flatly lit, white areas of the objects. Don’t colour pick specular highlights – they may look white but their values are much higher than the true, plain white colour of the object, and can mess up your grading.

If there aren’t any clearly white objects in the plate or CG, you can match the white balance by eye, just by adjusting the aforementioned **whitepoint** knob in a Grade node.

Note that when you drag the colour wheel on the _whitepoint_ knob around, it sort of has the opposite effect to what you might expect.

By dragging the colour wheel towards a warmer colour, you’re grading the CG cooler, and vice versa. The _whitepoint_ knob turns the colour you set into white, so if you set the white point to be yellow, then everything will look more blue as a result.

### Darker Areas

Shadows will often have a differently coloured tint than highlights.  
(Typically, they will contain more visible blue fill light from the sky).

The whitepoint knob in the Grade node tints the  
whole image. But we can use different knobs to affect the areas in  
shade, i.e. the darks and midtones.

Adjust the **blackpoint**, **lift** (_black_), or **gamma** knobs in the Grade node to introduce a coloured tint to the areas in shade.

This is also related to the black levels in the image:

### Black Levels

Black levels are the colour values of the darkest parts of the image.

Adjusting the black levels is typically purely a **technical grade** where we match the CG to the scan.

It’s quite important to match the black levels, because otherwise the audience may consciously or subconsciously feel that the various elements in the shot are not connected. It’s one of those things that can contribute to the audience losing their suspension of disbelief.

[![](https://www.keheka.com/content/images/2024/09/Black_Levels_Mismatch.jpg)](https://www.keheka.com/content/images/2024/09/Black_Levels_Mismatch.jpg)

_Mismatching black levels between the CG glove and the real hand. (The image was exposed up in order to reveal this)._

There are a vast number of different types of monitors, projectors, screens, and viewing setups in the world. Most people will watch your shot under very different conditions to what you did on your work machine with a professional grade monitor.

So when we make technical adjustments like setting the black levels, it’s crucial to **increase and decrease both the Viewer gain and gamma** in Nuke to make sure our grade holds up under scrutiny.

The black levels can vary across the frame in a shot, especially if there is a lens flare or any haze/atmos that lifts up the blacks in certain parts of the frame.

And the black levels tend to become more lifted with increased distance from the camera. In fact, they are one of the key elements for [depth cueing](https://www.keheka.com/how-to-create-a-sense-of-depth-in-your-composite/).

[![](https://www.keheka.com/content/images/2024/09/Depth_Cueing.jpg)](https://www.keheka.com/content/images/2024/09/Depth_Cueing.jpg)

_The further away a mountain is from the camera, the more lifted it becomes due to the larger amount of atmosphere in between them. This is also known as aerial perspective._

Grading the black levels should ideally be **the last grading operation** that you perform on your CG render. Since setting the black levels correctly is a technical grade, any creative grading should be done upstream in order to avoid breaking the black levels.

### Adjusting The Black Levels

Very similar to adjusting the white point, you can also match the  
black levels in your CG render to your plate semi-automatically:

1. Connect a Grade node to your CG render and view it. (Make sure to un-/premultiply the grade).

1. Colour pick the darkest part of your CG render using the **blackpoint** knob in the Grade node. This will set that colour to be true black.

1. View your plate, and colour pick the darkest part of the plate using the **lift** (_black_) knob in the same Grade node. This will shift the true black colour (that we set before) to match the black levels of the plate.

1. The Grade will now shift the black levels of the CG to match up with the plate.

  

> [!important]
> 
> Make sure to check that the black levels match up in each of the R, G, and B channels, individually.

❗

The _lift_ knob pivots around the value 1. Which means, when lifting your footage, values above 1 are darkened down. You may not want this. Instead, you can use the _offset_ knob, or a different approach to adjusting the black levels entirely, like the [BlacksMatch](https://youtu.be/Kw3bcsmkGuk?feature=shared&ref=keheka.com) tool.

Some shots will be slightly clamped and have areas that ramp off more quickly into darkness. In those cases, the _lift_ or _offset_ in a Grade node may not give you a completely matching result, because too much detail will be kept in the darker areas of the CG.

Instead, you can use a **Toe** node to lift your black levels. This will softly clamp the black levels to match what’s happening in the plate.

### Saturation

Saturation is sort of the odd one out of the colour grading operations.

Vibrant colours, or a **high saturation** value, means there is a **strong disparity** in the colour values of the R, G, and B channels.

Dull or grey colours, or a **low or zero saturation** value, means there is **little disparity** in the colour values of the R, G, and B channels – they are all similar or identical to each other, respectively.

> [!important]
> 
> This article explains in-depth [how saturation works](https://guillermoalgora.com/understanding-hsvl.html?ref=keheka.com).

So by increasing the saturation, you’re pushing the colour channels further apart in value, and by decreasing the saturation you’re pushing the colour channels closer together in value.

For example, an RGB value of **1, 0, 0** equals pure, vibrant red. There is a great disparity between the channels.

An RGB value of **1, 0.25, 0.25** has still got red in there, but a more washed out red. It’s not as vibrant anymore – it’s less saturated. There is less disparity between the channels.

An RGB value of **0.25, 0.25, 0.25** equals a dark grey. There is no saturation at all, and no disparity between the channels – they’re all the same value.

[![](https://www.keheka.com/content/images/2024/09/Saturation_Red.jpg)](https://www.keheka.com/content/images/2024/09/Saturation_Red.jpg)

[![](https://www.keheka.com/content/images/2024/09/Saturation_Washed_Out_Red.jpg)](https://www.keheka.com/content/images/2024/09/Saturation_Washed_Out_Red.jpg)

[![](https://www.keheka.com/content/images/2024/09/Saturation_Grey.jpg)](https://www.keheka.com/content/images/2024/09/Saturation_Grey.jpg)

RGB values of **1, 0, 0** (left), **1, 0.25, 0.25** (middle), and **0.25, 0.25, 0.25** (right).

Saturation issues can sometimes be tricky to spot.

It’s a good idea to increase the saturation of your composite, either using the Saturation slider in the Viewer or by connecting a Saturation node at the end and bumping up the saturation. By exaggerating the saturation, it’s easier to spot areas (CG or patches, for example) where the saturation doesn’t match up to the plate.

### Problematic Values

We previously saw in the _Artefacts_ section that bad pixel values can ‘infect’ the rest of the image when you apply various filtering operations to them in comp.

(And you can use tools such as [FireflyKiller](https://stefanmuller.com/wp-content/uploads/FireflyKiller.gizmo?ref=keheka.com), [mtPixelFixer](https://www.nukepedia.com/gizmos/draw/mtpixelfixer/?ref=keheka.com), _Soft Rolloff_ (see the code in the _Artefacts_ section), or the Grade/Clamp node to kill the unwanted values in your image).

However, it can sometimes be difficult to spot problematic pixel values in your renders. Particularly when they only show up in seemingly random, single pixels here and there, and you’re working with 4K images, for example.

An effective method for spotting these values before they become a further problem is to isolate the troublesome pixels and then dilate them out, making them much larger and more noticeable against a black frame.

You can do that in several ways, for example by using an Expression node to isolate the pixels and an Erode (filter) node to dilate them out.

To identify NaN values, you can use the **isnan()** expression, and combine it with an _if statement_ to isolate the corrupted pixels:

**isnan(r) ? 1 : 0**

The expression above means: if a pixel in the red channel has a NaN value, turn its value into 1. Otherwise, turn it into 0.

Apply the same expression to the other channels in the Expression node, as well – only swapping _r_ for _g_, _b_, or _a_, respectively. :

**isnan(g) ? 1 : 0**

**isnan(b) ? 1 : 0**

**isnan(a) ? 1 : 0**

[![](https://www.keheka.com/content/images/2024/09/Check_If_NaN.png)](https://www.keheka.com/content/images/2024/09/Check_If_NaN.png)

_The Expression node for isolating NaN pixels._

That will give you a black and white image where the white pixels indicate where there are NaN values in the image.

Then, add an Erode (filter) node which is set to affect the _rgba_ channels, and change the _size_ to for example -100.

By connecting the setup above to an image, any NaN pixels will now ‘explode’ out into a large white cluster on a black background – very easy to spot.

You can do the same with _inf_ pixel values by changing to the **isinf()** expression in the Expression node:

**isinf(r) ? 1 : 0**

**isinf(g) ? 1 : 0**

**isinf(b) ? 1 : 0**

**isinf(a) ? 1 : 0**

[![](https://www.keheka.com/content/images/2024/09/Check_If_Inf.png)](https://www.keheka.com/content/images/2024/09/Check_If_Inf.png)

_The Expression node for isolating inf pixels._

To isolate very high values, you can change the expressions to:

**r > maxValue ? 1 : 0**

**g > maxValue ? 1 : 0**

**b > maxValue ? 1 : 0**

**a > maxValue ? 1 : 0**

Then, make a variable called _maxValue_ at the top and assign it whichever numerical value that you don’t want to go above.

The expressions above mean: if a pixel's value is greater than the max value that you set, turn its value into 1. Otherwise, turn it into 0.

[![](https://www.keheka.com/content/images/2024/09/Check_If_High.png)](https://www.keheka.com/content/images/2024/09/Check_If_High.png)

_The Expression node for isolating high value pixels. (Here, with the maxValue arbitrarily set to 50)._

Or finally, to isolate negative values, you can change the expressions to:

**r < 0 ? 1 : 0**

**g < 0 ? 1 : 0**

**b < 0 ? 1 : 0**

**a < 0 ? 1 : 0**

The expressions above mean: if a pixel's value is less than 0 (i.e. negative), turn its value into 1. Otherwise, turn it into 0.

[![](https://www.keheka.com/content/images/2024/09/Check_If_Negative.png)](https://www.keheka.com/content/images/2024/09/Check_If_Negative.png)

_The Expression node for isolating negative value pixels._

By using the different Expression nodes above in conjunction with the Erode (filter) node, you can first spot – and then, by reducing/disabling the dilation, pinpoint exactly where there are – any  
problematic pixels in your image.

Sometimes when you render on a render farm, random pixels on random frames can become corrupt. If you're having that problem, this is a great way to weed them out.

Next, let’s take a look at how CG is usually rendered, and how it might affect us in comp:

### Physical Accuracy

CG scenes are typically rendered with [physically based rendering](https://en.m.wikipedia.org/wiki/Physically_based_rendering?ref=keheka.com) nowadays.

That means the lights and surfaces in the 3D software behave closely to what their physical counterparts would in the real world.

And so to be physically accurate when compositing these CG renders, you should _technically_ only adjust the raw lighting passes using a multiply or gain operation in Nuke, and merge them with a Merge (plus) operation. Because, in real life, you can only increase or decrease the strength of the lights in the scene, or change their colours – and light is additive.

If you adjust the gamma and contrast, or merge the lighting using other operations than plus, on the other hand, you technically venture outside of physical realism because you're altering the behaviour of the lights.

However, **don't feel limited by this**. Use whichever operations that give you the best visual result.

### Make It Look Good

VFX is a creative, visual artform, and there is plenty of room for creative interpretation and stylistic choices.

Our job is ultimately all about making beautiful imagery. And so it doesn’t necessarily matter if a shot is 100% technically and physically correct. Very often, reality has to be pushed a little bit in order to tell a better story. (And vice versa – sometimes, you'll find that something which was actually shot in camera will look fake and need changing).

While the technical grading of your CG is important, so is the creative grading. If a major plot point (or even a minor one) is completely lost in darkness – leaving the audience confused and taken out of the story – then it doesn't really matter if that's how the shot technically would look if filmed in real life.

> [!important]
> 
> Many people will remember the numerous scenes in Game of Thrones which were so dark, we just physically couldn't see anything. While watching these scenes, we became aware of the fact that we couldn't see what's going on, which pulled us out of the story – and out of the suspension of disbelief. Losing the audience's immersion like that is a preventable mistake.

[![](https://www.keheka.com/content/images/2024/09/GoT_Dark_Scene.jpg)](https://www.keheka.com/content/images/2024/09/GoT_Dark_Scene.jpg)

_It was hard to tell what was going on in some scenes in Game of Thrones._

As a starting point, match your CG to what's going on in the live-action plate (if there is one). But then from there, make sure that you tell the story clearly by focusing the viewer's attention where it should be.

Highlight the key elements in the shot, and neutralise unimportant areas of the frame. And when the shot looks great creatively, try to nudge it as close as you can to physical realism without losing too much of what makes the shot work from a creative point of view.

> [!important]
> 
> Often, even a subtle grade to accentuate an area will do the trick.

To end the _Colour Grading_ chapter, let's look at some tools and tutorials which help make the CG grading process easier:

### Grading Tools And Tutorials

Various tools have been built to help you grade your work, for example:

- [Light Mixer](https://youtu.be/V3b_dY52wjk?feature=shared&ref=keheka.com)

- [GradeAOVS](https://github.com/vfxwiki/gradeAOVS?ref=keheka.com)

- [LUE Grading For Nuke](https://www.nukepedia.com/gizmos/colour/lue-grading-for-nuke?ref=keheka.com)

Here is a [step-by-step grading tutorial](http://admvfx.com/nuke-course/color-correction/integration-cc/?ref=keheka.com) for integrating an element with live-action footage.

And here, you can learn more about [how the Grade node works under the hood](https://www.chrisbturner.com/blog/nukes-grade-node-demystified?ref=keheka.com).

Next, let's look at another very important part of compositing your CG:

# Depth Cueing

Creating a sense of depth and dimension to your CG renders is a major aspect of CG compositing.

This is a large topic which I’ve written an in-depth guide about here:

[How To Create A Sense Of Depth In Your Composite](https://www.keheka.com/how-to-create-a-sense-of-depth-in-your-composite/)

Two effective ways of introducing depth to your CG are using depth hazing and defocusing.

> [!important]
> 
> We’ll take a look at defocusing later on, in the _Defocus_ section.

### Depth Hazing

Like we saw in the _Black Levels_ section, black levels tend to become more lifted with increased distance from the camera.

That’s because the further away from the camera an object gets, the more atmosphere (which scatters light) there is in between the object and the camera.

You can emulate this in comp by using the normalised Z Depth pass:

1. Shuffle out the Z Depth pass from the CG render and add a Grade node.

1. Colour pick the values **furthest** away from the camera in the Z Depth pass with the _whitepoint_ knob in the Grade node.

1. Colour pick the values **closest** to the camera in the Z Depth pass with the _blackpoint_ knob in the Grade node.

1. Now you’ll have a 0-1 mask which gets brighter further away from the camera.

1. Add a Grade node to your CG and connect the mask to its _mask_ input, then increase the _offset_ value to add depth hazing to your CG.

Another part of depth cueing is masking objects correctly in relation to each other.

### Masking

Masking subjects and objects is a fundamental part of compositing.

And when it comes to CG compositing, there are plenty of options for making procedural masks, saving yourself a ton of rotoscoping work:

### Position Masking

With a position matte tool such as [aPMatte](https://www.nukepedia.com/blink/keyer/apmatte?ref=keheka.com), you can use the PWorld, PRef, PObject, or PCam AOVs to generate mattes, ramps, or noise patterns which will track with the camera move or with an object's animation.

  

> [!important]
> 
> This tutorial shows you [how to build a position matte tool from the ground up](https://www.nukepedia.com/written-tutorials/expressions-101?ref=keheka.com).

This is extremely useful for creating procedural mattes to grade or hold out specific areas of your CG, or for [breaking up clean edges and surfaces](https://www.keheka.com/breaking-up-edges-and-surfaces-in-nuke/).

> [!important]
> 
> When you grade on the unpremultiplied CG, make sure that your position data  
> is unpremultiplied, as well, so that the edges will line up correctly.

You could for instance create a PMatte from the PWorld AOV for grading only a certain section of a building facade, for example.

Or, a PMatte from the PObject AOV for grading a specific part of the face of an animated character.

You could create a PNoise from the PWorld AOV to create dappled lighting in a scene.

Or, a PRamp from the PWorld AOV for grading in some low lying mist/fog, or to darken down an object’s connection point with the ground.

You could also create directional PMattes using the NWorld pass, by setting the size to a very low number like 1, which could be used for relighting a scene.

There are really so many use cases that a good PMatte gizmo should be an essential part of your toolkit.

### Cryptomatte

As we saw in the _Format_ section of this guide, cryptomattes rely on **exact, unfiltered values**  
in each of its channels. Any operation that mixes the pixel values with  
neighbouring pixel values will damage the cryptomatte information.

So make sure that there are no Reformat, LensDistortion, or similar nodes applied to your cryptomatte AOVs before keying your mattes. Instead, these operations should either be applied to the extracted mattes, or the filter for the operations should be set to **impulse**.

Likewise, avoid using proxy mode while colour picking your cryptomattes, as it may produce poor or unexpected results.

And when using the colour picker in the Cryptomatte node, make sure to press the **Ctrl/Cmd** key only. Avoid using Ctrl/Cmd + Alt, which will colour pick values from upstream of the node. (Many of us are in the habit of pressing Ctrl/Cmd + Alt when colour picking).

Note that cryptomatte selection works with all colours, so you can Ctrl/Cmd-click on the black background to get a matte of the background, should you wish.

You can also marquee select multiple mattes by holding **Ctrl/Cmd** + **Shift** and dragging in the Viewer. This is useful when there are many small cryptomattes next to each other and you want to select a whole bunch of them.

You don't need to be viewing the Cryptomatte node to select and deselect mattes.

The Cryptomatte node will by default output premultiplied mattes which match the alpha edges of the beauty. When you're using the cryptomattes for masking Grade nodes which are grading the unpremultiplied CG, make sure to tick the **Unpremultiply** option in the Cryptomatte node. That way, it’ll output unpremultiplied mattes which match up along the edges of the CG, and you can grade your CG correctly.

> [!important]
> 
> Combine cryptomattes and position mattes to gain even more control over your masks.

### ID Passes

While cryptomattes have taken the VFX world by storm in the past few  
years, there are some software which have not implemented them yet.

And, they might not work when piped through complex and specific builds, for instance in FX, where things deform and dissipate.

So there may still be times where you'll receive the trusty ol’ RGBA mattes; an AOV where each channel is a matte for something specific.

These AOVs are pretty straightforward – just shuffle out the matte you need, and use it as a mask for whichever operation you need it for.

### Deep Holdouts

Like we saw in the _Deep_ section, the deep AOV is really a sophisticated layering assistant.

You can use deep to perform complex, multi-layered merges and [holdouts](https://learn.foundry.com/nuke/content/comp_environment/deep/creating_holdouts.html?ref=keheka.com) with your CG.

Because of the multiple depth samples per pixel, several layers of objects that overlap from the camera's perspective can accurately be interwoven, combining what traditionally would need to be a number of separate, potentially complicated holdouts into a single one.

And by using simple (or complex) geometry, you can [make custom deep holdouts](https://www.keheka.com/deep-holdouts-using-geometry-in-nuke/) with much more control than the standard DeepCrop node.

For instance, if you have the roto for a subject in a shot, you can live-project that roto onto a Card which is positioned correctly in 3D space. And then, use that to apply a deep holdout to the CG scene to accurately hold the subject out amongst the objects in the scene.

> [!important]
> 
> You can use the DeepToPoints or PositionToPoints node in Nuke to create a point cloud to use as a reference for positioning your Card in 3D space.

[![](https://www.keheka.com/content/images/2024/09/Live-Project_Roto.png)](https://www.keheka.com/content/images/2024/09/Live-Project_Roto.png)

_Setup for live-projecting roto onto a Card._

As an example, I made a particle rain system in Nuke to create heavy rainfall for a project. In one of the shots, a person was walking from the camera towards a mansion, effectively moving deeper into the rain.

I live-projected the roto of the person onto a Card, and animated the Card to move roughly the same distance in 3D as the person did on the set. And then I used the ScanlineRender output of the Card projection (A) to hold out the ScanlineRender output of the particle system (B) with a DeepMerge (holdout).

  

> [!important]
> 
> The ScanlineRender node natively outputs deep, but it only becomes visible/accessible when you connect Deep nodes to the output of the ScanlineRender node.

When compositing the (held out) rain with the plate, the person now realistically appeared to walk deeper into the rain. There wasn't just one background layer of rain and one foreground layer of rain with the person constantly staying in between them – the person actually moved through the depth of the rainfall.

> [!important]
> 
> In the ScanlineRender node, you can go to the **MultiSample** tab and increase the **samples**. This will increase the number of deep samples per pixel.

An important part of integrating your CG renders is getting the shadows right.

# Shadows

Shadows are simply the **absence of light**.

However, just like how we for practical reasons separate all of the scene lighting into its constituent parts, we can also isolate the absence of light as AOVs.

And like with the other AOVs, we can also split shadows into two categories: **direct shadows** and **indirect shadows**.

– More commonly referred to as **cast shadows** and **ambient occlusion**, respectively.

In the real world, both direct and indirect shadows are technically cast shadows. But, because indirect shadows are heavy to calculate in CG, several methods have been developed for faking them, e.g. ambient occlusion.

Visually, direct shadows are typically **harder and more directional** shadows – cast by objects lit by **direct lighting**. And indirect shadows are typically **softer and less (visibly) directional** shadows – cast by objects lit by **indirect lighting**.

Let's take a closer look at each.

### Cast Shadows

A cast shadow is the silhouetted projection of an object which is illuminated by a source of light.

[![](https://www.keheka.com/content/images/2024/09/Cast_Shadows.jpg)](https://www.keheka.com/content/images/2024/09/Cast_Shadows.jpg)

_The silhouetted shape of the cue ball is projected onto the floor as a cast shadow._

It's a shadow that is cast directly from one object onto another object. (Or onto the opposite side of itself, in relation to the light source).

Cast shadows happen where there is a directional light like the sun, a lamp, or another light source shining directly onto an object in the scene.

Although they fade and become softer and lighter over distance (as more and more ambient lighting or other lights in the scene fill in the missing light), cast shadows are typically quite sharp and defined overall. And more so the closer they are to their source – i.e. the object which is casting the shadow.

> [!important]
> 
> Multiple shadows will be cast if there is more than one light, and the projections will also appear out of focus (softer shadow) if the light is larger than the object receiving the light.

[![](https://www.keheka.com/content/images/2024/09/Shadow_Umbra_Penumbra.jpg)](https://www.keheka.com/content/images/2024/09/Shadow_Umbra_Penumbra.jpg)

_Shadow softness in relation to the size of, and distance to, the light source._

Cast shadows are made up of two components: the **umbra** and the **penumbra**. (U and P in the picture above, respectively).

- The umbra is the primary portion of the shadow – the deep, dark centre of it, where the object fully occludes the light.

- The penumbra is the secondary portion of the shadow – the fuzzy perimeter  
    created by oblique rays of light coming around the edges of the  
    subject.

[![](https://www.keheka.com/content/images/2024/09/Shadows_More_Umbra.jpg)](https://www.keheka.com/content/images/2024/09/Shadows_More_Umbra.jpg)

[![](https://www.keheka.com/content/images/2024/09/Shadows_More_Penumbra.jpg)](https://www.keheka.com/content/images/2024/09/Shadows_More_Penumbra.jpg)

_Hard light from distant, small, and/or less diffused sources create more umbra (left), while soft light from nearer, larger, and/or more diffused sources create more penumbra (right)._

### Ambient Occlusion

Ambient occlusion is typically a much softer shadow than the direct shadows, and is darkest in the gaps and crevices in and between objects – where less ambient light (i.e. soft and indirect light) from the scene is able to reach.

[![](https://www.keheka.com/content/images/2024/09/AO.jpg)](https://www.keheka.com/content/images/2024/09/AO.jpg)

_Ambient occlusion._

As mentioned, ambient occlusion is a shading and rendering technique used to fake indirect shadows – by calculating how exposed each point in a scene is to ambient lighting.

It does this by casting rays out from each surface of the geometry. If the rays come into contact with another surface, that area will become darker. And if they don’t find another surface, the area will stay brighter.

The resulting render pass is a grayscale image which can be multiplied with your beauty rebuild to darken down occluded areas. (The effect itself looks similar to the way an object might appear on an [](https://en.m.wikipedia.org/wiki/Overcast?ref=keheka.com)overcast day).

While the cast shadows (direct shadows) are often visually more dominant in an image, ambient occlusion is still very important.

When something looks too ‘CG’, one of the biggest telltale signs is usually that objects and surfaces don't make contact with each other in a realistic way.

Where two objects connect – for example a ball and the ground – we normally get a contact shadow. That's because one object, in this case the ball, occludes (blocks) some of the ambient  
lighting in the scene from reaching the other object, i.e. the ground.

[![](https://www.keheka.com/content/images/2024/09/Tennis_Ball_AO.jpg)](https://www.keheka.com/content/images/2024/09/Tennis_Ball_AO.jpg)

_Contact shadow between a tennis ball and the ground._

  

> [!important]
> 
> The bare ground is pretty much completely exposed to the ambient lighting in the scene. But with a ball on the ground, the part of the ground underneath it no longer receives (as much) ambient lighting and looks shadowed.

An object can also occlude ambient lighting from itself. For example, the interior of a tube is typically more occluded (and therefore darker) than the exposed outer surfaces. And it becomes darker deeper inside the tube. Similarly, creases or joins in an object will also occlude the ambient lighting in the scene.

The result is a diffuse, less directional shading effect that doesn't cast as clearly defined shadows, but rather, softly darkens down contacting surfaces and enclosed and sheltered areas.

> [!important]
> 
> Ambient occlusion can be rendered as omnidirectional shadows, shadowing in all directions, or unidirectional shadows, taking into account which way the lights in the scene are shining. Depending on the lighting in the scene, the former can make the shadows look unrealistic.

[![](https://www.keheka.com/content/images/2024/09/Directional_Occlusion.jpg)](https://www.keheka.com/content/images/2024/09/Directional_Occlusion.jpg)

_Omnidirectional vs. unidirectional occlusion._

  

### Matching Up Shadows

Getting the shadows right is crucial for integrating CG renders with a live-action plate, or for sitting CG objects in with the rest of a CG scene.

It’s not always as simple as multiplying the shadow AOVs with the plate (or using them as a mask for grading the plate).

Shadows are cast when light is blocked, and we have to take into consideration what happens when an area becomes shadowed. For example, the specular highlights go away.

So, just grading down the shadow area overall might not work – it might end up just looking like an artificially graded down area, and not an area that’s actually in shadow.

Instead, it’s usually better to create a shadowed version of the plate and reveal it using the shadow matte.

This tutorial explains the technique really well:

> [!info] Compositing Complex Shadows in Nuke [Advanced]  
> A nuke compositing tutorial about how to composite shadows in Nuke.  
> [https://youtu.be/Yb3Cn3JnkUI](https://youtu.be/Yb3Cn3JnkUI)  

_Compositing Complex Shadows in Nuke._

You may end up creating your own shadow pass. Here are two tutorials for how to project shadows:

> [!info] Creating Shadows with Projections in Nuke  
> In this workshop Eyal will guide us through a process of creation shadow maps in nuke without using Lights.  
> [https://youtu.be/VhQgo1Qku9U](https://youtu.be/VhQgo1Qku9U)  

_Creating Shadows with Projections in Nuke._

> [!info] Compositing about Nuke's shadow  
> How does depth map shadow work in Nuke?  
> [https://youtu.be/NIrlInSJ2SQ](https://youtu.be/NIrlInSJ2SQ)  

_Compositing about Nuke's shadow._

And here is another tutorial which shows how to create contact shadows:

> [!info] Projections And Displacements in Nuke  
> And yet another technical and exciting workshop from Eyal, we will learn how to create contact shadows, fix holes and geometries and even how to create a contact displacement when two geometries and touching each other, all that within our Nuke!  
> [https://youtu.be/JxgoqjGsq68](https://youtu.be/JxgoqjGsq68)  

_Projections And Displacements in Nuke._

  

> [!important]
> 
> You can also use simple PMattes to create masks for contact shadows.

Lastly, here is how to create ambient occlusion pass in Nuke:

[Create your own Ambient Occlusion in Nuke, using RayRender](https://benmcewan.com/blog/2019/10/07/create-your-own-ambient-occlusion-in-nuke-using-rayrender/?ref=keheka.com)

Like we saw in the _Adjusting The Black Levels_ section, you can use a **Toe** node to lift your shadow areas in a way that will softly clamp the shadows to match what’s happening in the plate.

### Isolating And Removing Shadows

We saw in the _Auxiliary AOVs_ section – under the _Shadow Matte_ and _Ambient Occlusion_ points – that if you have **occluded** and **unoccluded** AOVs, you can isolate the shadows simply by dividing the occluded by the unoccluded AOV:

**Occluded direct lighting / Unoccluded direct lighting = Shadow matte**

**Occluded indirect lighting / Unoccluded indirect lighting = Ambient occlusion**

If you already have the shadow AOVs, but not the unoccluded AOVs, you can rearrange the equations to get those:

**Occluded direct lighting / Shadow matte = Unoccluded direct lighting**

**Occluded indirect lighting / Ambient occlusion = Unoccluded indirect lighting**

So by dividing the occluded lighting by the relevant shadow AOV, you can remove the shadows from the CG.

This is great when the CG render has baked in shadows that don’t quite work with the plate. Divide them out, change them, and multiply them back in – or leave them out, whatever works best for your shot.

# Reflections

Like mentioned previously, reflections are a special case because the AOV contains every shadingcomponent in the scene baked together. It’s like a mini-beauty render, just for the mirror reflections.

It is, however, possible to split out each shading component from the reflection pass by using Light Path Expressions:

[Arnold Reflection AOVs using Light Path Expressions (Maya/Arnold/Python)](https://bhavesh7393.artstation.com/pages/reflection-aovs?ref=keheka.com)

But otherwise, we'll have to do some tricks in comp.

Trixter and Tony Lyons have great workshops on dealing with reflections:

> [!info] Compositing of Reflections  
> 00:00 Introduction  
> [https://youtu.be/P3lwZg5CNJ8](https://youtu.be/P3lwZg5CNJ8)  

_Compositing of Reflections._

> [!info] CG Compositing Series - 2.5 Material AOVs - Refractions & Reflections  
> In the first half of the video we are understanding the problem with refraction (transmission) and reflections (indirect specular) and the last half of the video we explore potential solutions.  
> [https://youtu.be/FbxuJePSyPU](https://youtu.be/FbxuJePSyPU)  

_CG Compositing Series - 2.5 Material AOVs - Refractions & Reflections._

And here is a quick tip for making reflections in Nuke:

> [!info] How to Make Reflections in Nuke | 3D | Make Reflections | Nuke Compositing \#nuke \#compositing  
> Hello everyone welcome back to another nuke compositing tutorial in this one I'm going to show you how you can make reflections pass in nuke using ray render and scanline render i hope you find this useful.  
> [https://youtu.be/Zdf1MhaoOE4](https://youtu.be/Zdf1MhaoOE4)  

_How to Make Reflections in Nuke | 3D | Make Reflections | Nuke Compositing._

# Refractions

Similar to reflections, refractions are also a special case where the  
AOV contains every shading component in the scene baked together.

And  
just like with reflections, it's possible to split out each shading  
component from the refraction pass by using Light Path Expressions.

If you don’t have that available, however, there are some tricks in comp for dealing with refractions:

  

> [!important]
> 
> The [video](https://youtu.be/FbxuJePSyPU?feature=shared&ref=keheka.com) by Tony Lyons linked above in the _Reflections_ section also covers refractions.

You can fake refractions using normals:

> [!info] Death Fall - Render Layers: Fake Refraction  
> In this tutorial I'm going to show you haw to create a Normal shader from scratch inside Maya and how to use it in Nuke to simulate refraction.  
> [https://vimeo.com/30058027](https://vimeo.com/30058027)  

_Death Fall - Render Layers: Fake Refraction._

You can also create ripple distortions to simulate a refracting water surface:

> [!info] Nuke Distortion Workflow Guide \#nuke \#compositing \#keying  
> Hello guys welcome to another nuke guide tutorial in this one i'm going to show you how to setup distortion in nuke how to make distortion map in nuke and different ways to do distortion and gizmo overview as well i hope this will be helpful for you guys.  
> [https://youtu.be/cg-ImGA7qdw](https://youtu.be/cg-ImGA7qdw)  

_Nuke Distortion Workflow Guide._

And here is a nice tool for creating refractions in Nuke:

[https://www.nukepedia.com/gizmos/filter/aerefracthor](https://www.nukepedia.com/gizmos/filter/aerefracthor?ref=keheka.com)

  

# Geometry

Having access to the geometry of the 3D scene that was used to create the CG render can be very useful.

With geometry, you can do a lot of things, such as:

- Create accurate projections – and when the geometry is animated, you can [stick projections to the animated geometry using this technique](https://www.keheka.com/sticking-a-projection-onto-animated-geometry/).

- Use the geometry as reference for where to place things like smoke elements on Cards in 3D space in Nuke.

- Select vertices on the geometry and use the snap to geo features in Nuke. (In Cards or Axis nodes, for example). And use [AnimatedSnap3D](https://www.nukepedia.com/python/3d/animatedsnap3d/?ref=keheka.com) to follow animated geometry. This is great for placing things like Cards in 3D space, or for [creating 2D tracks from 3D objects](https://www.keheka.com/3d-to-2d-tracking-in-nuke/).

- Output various AOVs using the ScanlineRender or RayRender nodes with your camera.

- Create deep holdouts by outputting the geometry through a ScanlineRender node  
    with your camera. (Remember to connect a Constant node with an alpha of 1 as the texture input for the geometry).

By using the **ReadGeo** node, you can import 3D geometry into your Nuke script.

[![](https://www.keheka.com/content/images/2024/09/Geo_Nuke.jpg)](https://www.keheka.com/content/images/2024/09/Geo_Nuke.jpg)

_3D geometry imported into Nuke._

The node currently supports the following file formats:

- Alembic (**.abc**)

- FBX (**.fbx**)

- OBJ (**.obj**)

- USD (**.usd**/**.usda**)

> [!important]
> 
> Note that when importing any of these formats, materials/textures are not read in. You'll have to import the textures manually, and recreate the materials in Nuke.

### LOD

There can be different versions of the geometry for the same scene or object, each with a different level of detail (LOD).

The [LIDAR](https://en.m.wikipedia.org/wiki/Lidar?ref=keheka.com) scan from the set will be very dense and detailed, and have lots of polygons, for example. Which makes it quite accurate but heavy to work with. (And it may have holes in it, in the areas where the laser didn't reach).

Based on the LIDAR, the modeller may create a more optimised, but still detailed model of the scene, and potentially one or more lower resolution versions.

So you could have multiple LODs available – from LIDAR and all the way down to low resolution proxy geometry.

What to use in comp depends on your needs. For some projections, proxy geometry might suffice, but for holdouts you may have to use higher resolution geometry, or even the LIDAR.

If you don't have geometry available for your shot, or the geometry is too dense and heavy to work with, you can use the **PositionToPoints** or **DeepToPoints** node and create a point cloud to use as reference when positioning Cards etc. in 3D space.

> [!important]
> 
> You can also [accurately place Cards using the position pass](https://www.keheka.com/snapping-cards-to-a-position-pass-in-nuke/), for example when adding a large number of fire and smoke elements into a CG environment.

# Relighting The CG

Relighting CG renders is very useful for integrating the CG with light sources that’s been added in comp, like a fire.

Or for look development where you don’t want to wait for new CG renders to try out different lighting scenarios.

I’ve written a detailed guide on how to relight CG renders here:

[Relighting CG Renders In Nuke](https://www.keheka.com/relighting-cg-renders-in-nuke/)

Keep in mind that the ReLight node creates raw lighting, which has to be multiplied with the albedo to create the proper primary AOV:

[![](https://www.keheka.com/content/images/2024/09/ReLight_Green_Raw.jpg)](https://www.keheka.com/content/images/2024/09/ReLight_Green_Raw.jpg)

[![](https://www.keheka.com/content/images/2024/09/ReLight_Green_Raw_Plus.jpg)](https://www.keheka.com/content/images/2024/09/ReLight_Green_Raw_Plus.jpg)

[![](https://www.keheka.com/content/images/2024/09/ReLight_Green_Multiplied_With_Albedo.jpg)](https://www.keheka.com/content/images/2024/09/ReLight_Green_Multiplied_With_Albedo.jpg)

[![](https://www.keheka.com/content/images/2024/09/ReLight_Green_Raw_Mult_Plus.jpg)](https://www.keheka.com/content/images/2024/09/ReLight_Green_Raw_Mult_Plus.jpg)

_The raw relighting (1) plus’ed directly with the beauty does not give the correct result (2). Multiplying the raw relighting with the albedo first (3) creates the correct primary AOV, which when plus’ed with the beauty gives the correct result (4)._

Like we saw in the _Position Masking_ section, another useful trick for relighting a scene is to create directional PMattes using the NWorld pass, by setting the size to a very low number like 1 and colour picking a point on your CG which is facing in the direction that you want your matte to cover.

> [!important]
> 
> When relighting in Nuke, think about how you should [light CG objects with faraway light sources](https://www.keheka.com/the-lunar-terminator-illusion/).

You can also relight elements without using the usual AOVs, by using 2D relighting tools like the [aeRelight2D](https://www.nukepedia.com/gizmos/colour/aerelight2d?ref=keheka.com).

# Projections

Projecting images is very cost-effective.

Instead of rendering full-frame range renders, you can in many cases render a  
single frame and project it using projection geometry and your camera.

Or, you could project DMP patches to touch up your CG or fill in a background, for example.

To get accurate projections you need to get four main things right:

1. **Accurate render (matchmove) camera**. If the render camera is slipping, so will the projection. The better the camera, the better the projection.

1. **Accurate projection camera**. I’ll go into more detail below in the next section, but essentially,  
    you have to use the right projection camera to project your CG/DMP/patch – which might _not_ be the current shot’s matchmove camera. The  
    correct projection camera also has to be frameheld (or have its  
    animation killed) on the right frame, i.e. the same frame that the CG  
    was rendered on, or that the DMP was painted on.

1. **Accurately positioned projection geometry**. You have to position your Card or other projection geometry correctly  
    in 3D space – where the object you're projecting actually is located in  
    real life in relation to the camera. Otherwise, the [parallax](https://en.wikipedia.org/wiki/Parallax?ref=keheka.com) will be all wrong. Use the geometry or create a [point](https://learn.foundry.com/nuke/content/reference_guide/3d_nodes/positiontopoints.html?ref=keheka.com) [cloud](https://learn.foundry.com/nuke/content/reference_guide/deep_nodes/deeptopoints.html?ref=keheka.com) to use as reference for where things are in 3D space.

1. **Accurate lens (un-)distortion**. If you're trying to match up a projection to a plate without applying  
    the proper lens distortion, it will slide near the edges of the frame.  
    And if you’re projecting a plate from a different shot, you’ll need to  
    undistort it with that shot’s camera’s undistortion before projecting  
    it, for it to line up properly. (And then redistort after the projection with the current shot’s camera’s lens distortion).

Like we saw in the _Coverage_ and _Plate Alignment And Tracking_ sections, there is a lot we can do in comp to counter sliding projections in Nuke. But the better the source material is, the better your projections will be right off the bat.

Here are some tips for projecting:

- [Projecting Overscan With No Cropping In Nuke](https://www.keheka.com/projecting-overscan-with-no-cropping/)

- [Combining Projections In Nuke](https://www.keheka.com/combining-projections-in-nuke/)

- [Sticking A Projection Onto Animated Geometry In Nuke](https://www.keheka.com/sticking-a-projection-onto-animated-geometry/)

You can also use the position AOVs for projecting images.

This is great for when you don't have geometry but want to project a patch onto your CG – which may have a complex surface with parallax.

There are several gizmos and toolsets to help with this, such as:

- [Pos Toolkit](https://www.nukepedia.com/toolsets/3d/pos-toolkit?ref=keheka.com)

- [ProjectionBuddy](https://www.hiramgifford.com/buddy-system/projectionbuddy?ref=keheka.com)

- [RefPosProject](https://www.nukepedia.com/gizmos/3d/refposproject?ref=keheka.com)

- [World Position Tool Kit](https://www.nukepedia.com/toolsets/3d/wptk?ref=keheka.com)

> [!important]
> 
> You could even use the aforementioned ST map technique to stick images onto your animated geometry.

  

### Re-Using CG Renders

It's very common to re-use CG renders from one or more shots across a whole sequence on a project.

To line up the CG correctly from shot to shot, you can make use of projections. I've written a guide about how to do that here:

[Re-use CG renders across a sequence](https://www.keheka.com/re-using-cg-renders-across-a-sequence/).

[![](https://www.keheka.com/content/images/2024/09/Setup_For_Re-Using_CG.png)](https://www.keheka.com/content/images/2024/09/Setup_For_Re-Using_CG.png)

_Setup for re-using CG renders from a hero shot._

When projecting, it's important to separate the **projection camera** from the **render camera**. While the two _can_ be the one and same camera sometimes, you have to understand that these are two separate concepts.

For example, if you're re-using a CG render (or DMP) from a different shot in your shot:

You have to project the CG from the other shot onto the geometry using the **other shot’s camera** as the **projection camera** (frameheld on the projection frame: the frame the CG was rendered on) – otherwise, it won't line up correctly.

I.e. you have to use the camera which the CG was **rendered with** to align the CG to the geometry correctly.

And then, use your **current shot's camera** as the **render camera** so that the matchmove sticks.

A lot of people use the current shot's camera for both – which **only** works if the CG was rendered using the current shot's camera.

# Deep Compositing

Like we saw in the _Deep_ section, Deep data is a separate data set to normal 2D renders.

Deep renders are read into, and written out from Nuke using the [DeepRead](https://learn.foundry.com/nuke/content/reference_guide/deep_nodes/deepread.html?ref=keheka.com) and [DeepWrite](https://learn.foundry.com/nuke/content/reference_guide/deep_nodes/deepwrite.html?ref=keheka.com) nodes, respectively.

Typically, deep renders will only contain RGBA and deep data. And so to work with the AOVs that you're used to – and do a beauty rebuild, for example – you may have to do a normal 2D composite with the normal 2D CG renders _alongside_ the deep composite with the deep renders,and combine the two using the [DeepRecolor](https://learn.foundry.com/nuke/content/reference_guide/deep_nodes/deeprecolor.html?ref=keheka.com) node.

So you'll have the 2D beauty/AOVs and 2D nodes in one pipe, and the deep data and deep nodes (like DeepReformat, DeepMerge for deep holdouts, etc.) in another pipe. And then you connect both of these pipes into the DeepRecolor node to recolour the deep data with the adjusted 2D pixel values.

Do this for all the renders, and then DeepMerge them all together.

And finally, to flatten the deep composite into a regular 2D image, connect the [DeepToImage](https://learn.foundry.com/nuke/content/reference_guide/deep_nodes/deeptoimage.html?ref=keheka.com) node.

Here’s the basic setup:

[![](https://www.keheka.com/content/images/2024/09/FG_MG_BG_Deep_Renders.png)](https://www.keheka.com/content/images/2024/09/FG_MG_BG_Deep_Renders.png)

_Basic deep composite, here using the subtractive method with the light group 01 AOV._

However, deep can be very heavy to work with in Nuke, and so it's beneficial to get out of deep and into regular 2D compositing as quickly as possible.

To do that, you'll want to use the deep data to create accurate holdouts for each 2D layer, and output perfect ‘puzzle piece’ mattes for each layer for further 2D compositing. And then precomposite all the mattes to speed up your script:

[Deep Compositing Using A 2D Workflow In Nuke](https://www.keheka.com/deep-compositing-using-a-2d-workflow-in-nuke/)

Tony Lyons has taken this one step further and simplified the process by making a single deep composite (which can also be used as a ground truth for the layering), containing all of the mattes:

> [!info] STAMPS & DEEP Workflow | Script Refactoring  
> This is a Script Refactoring video focusing on Stamps and Deep Combine Workflow.  
> [https://youtu.be/3tujOTTcw4w](https://youtu.be/3tujOTTcw4w)  

_STAMPS & DEEP Workflow | Script Refactoring._

💡

When you have mattes where all the other layers are held out from each matte, you'll have to merge each layer using the **disjoint-over** operation in the Merge node to avoid faint gaps in the alpha. And you **can’t make any changes to the mattes**  
because they're already perfect puzzle pieces which fit together  
exactly. (So any blurring etc. will have to happen before the premult,  
or after the merge).

Deep compositing is all about correct layering, and deep holdouts play a big part in this. You can [create deep holdouts using geometry in Nuke](https://www.keheka.com/deep-holdouts-using-geometry-in-nuke/), taking advantage of the fact that Nuke's ScanlineRender node natively outputs deep data.

You can also use deep for other interesting things by [Manipulating Deep Data In Nuke](https://www.keheka.com/manipulating-deep-data/).

# Defocus

Because of the way cameras and optics work, an image is only fully  
sharp at the focal plane of the camera: a thin slice across the camera's  
frustum.

(And even then it's not 100% sharp across the whole frame, because curved lenses turn the focal plane to a curved focal _surface_, softening the edges).

There will be an area of acceptable focus spanning from a certain distance in front of this focal plane to a certain distance behind it, where the image will be acceptably sharp. And outside of this, the focus falls off more and more with distance.

> [!important]
> 
> In the [Compositing Photorealistic Lens Effects In Nuke](https://www.keheka.com/compositing-photorealistic-lens-effects-in-nuke/) guide, I gave an in-depth explanation of the circle of confusion, focal plane, depth of field, and bokeh.

CG cameras are not affected by real world optics, but are instead mathematically perfect cameras.

And so CG renders, unless rendered with depth of field enabled, will be completely sharp across the entire depth and width of the image.

This is, of course, not realistic. We have to add proper defocus (matching to the camera that filmed the plate, if there is a plate – or matching to what the real life equivalent of the CG camera would do, if there isn't) in order to give the CG that photorealistic quality.

However, CG depth of field is almost never rendered in VFX. You'll typically always receive a depth.z AOV and do your own defocusing in Nuke.

While it’s not as accurate as rendering true depth of field, it’s completely acceptable in most cases. And the benefits are that it’s much faster and much more flexible than baking the defocus into the CG renders.

Here is a great video that dives into the topic:

> [!info] Simulating Physically Accurate Depth of Field in Nuke  
> A discussion of lenses and optics, and how they affect depth of field behavior in an image.  
> [https://youtu.be/Rv7L6M8f2lk](https://youtu.be/Rv7L6M8f2lk)  

_Simulating Physically Accurate Depth of Field in Nuke._

Xavier Bourque has taken this a step further, and developed the excellent [PxF_ZDefocus](https://youtu.be/526Qi-NMwUo?feature=shared&ref=keheka.com) and [PxF_DeepDefocus](https://youtu.be/x4ZaklAKGlc?feature=shared&ref=keheka.com) gizmos. He describes the differences between the normal ZDefocus and  
Bokeh nodes in Nuke, and how they stack up against each other and to his  
own tools, here:

> [!info] Nuke - Realistic Depth of Field Defocus. Complete Guide for CLEAN edges!  
> Here's a complete guide to setting up ZDefocus and Bokeh to achieve realistic 'depth of field' (DOF) defocus effects for VFX and animation in Nuke.  
> [https://youtu.be/HTM59OFuQfQ](https://youtu.be/HTM59OFuQfQ)  

_Nuke - Realistic Depth of Field Defocus. Complete Guide for CLEAN edges!_

Focus isn’t always a static property of the image. There can be focus breathing in the plate, or a rack focus, which we have to match. Here is a useful technique for [matching rack focus in Nuke](https://www.keheka.com/matching-rack-focus-in-nuke/). – Make sure to increase the number of _depth layers_ in the ZDefocus node if your rack focus looks steppy.

It’s also extremely rare for there to be pixel pin sharp edges in a plate. There will usually be a very small softening, even on the sharpest of details. So make sure to zoom in real close and compare your CG with the plate. Add a gentle defocus as needed. Sometimes, a small diffusion is  
all that it takes – you can do that with a Blur node set to a _size_ of ~3 and a _mix_ of ~0.5.

When there are objects at different depths in your CG render, the various defocusing nodes may run into issues along the edges between the objects, where two very different depth values are right next to each other in the depth.z pass.

To help alleviate that, you can dilate the depth.z pass by 1 pixel, or edge extend it. But it may be better to do a deep defocus using the PxF_DeepDefocus gizmo if you have deep renders. That’s because deep contains information about what’s behind a pixel, which gets pulled in around the edges when defocusing. (This is what happens in real life, defocused edges go transparent and  
we can see a little of what’s behind).

> [!important]
> 
> If you’re getting defocus issues along the edges of objects, also try unpremultiplying the depth.z pass. A premultiplied depth.z pass may have incorrect depth values along the edges.

If you have multiple renders, you have two options for defocusing them: individually, or in one go at the end, after merging them together. The latter is more technically correct but can be troublesome to get to work correctly like described above. So if the former method gives you a  
visually good result, feel free to do that.

Sometimes, you may be getting super bright values in the depth.z pass where there is just an empty black background (i.e. where there are no objects in the CG render). This should ideally be fixed on the 3D side, but you can also find the highest natural value in the depth.z pass by sampling the  
furthest away distance, and then clamping the depth.z pass to that maximum value.

# Motion Blur

As mentioned in the _Motion Vectors_ chapter, motion blur is a large topic and I have written an extensive guide about it here:

[Motion Blur Masterclass For Compositors](https://www.keheka.com/motion-blur-masterclass-for-compositors/).

Ideally, motion blur should be applied _after_ the defocus in your comp. Because in real life, motion blur will smear the bokeh:

[![](https://www.keheka.com/content/images/2024/09/Smeared_Bokeh.png)](https://www.keheka.com/content/images/2024/09/Smeared_Bokeh.png)

_Smeared bokeh due to motion blur from long exposure photography. Notice the static red lights bottom left showing up as bokeh, while the moving red lights smear the bokeh along their path._

> [!important]
> 
> Motion blur does not smear any lens contaminations stuck on the lens, because they follow the motion of the lens. So only apply motion blur to your CG, and not to any lens dirt. Exceptions include rain drops or splatter running down the lens – i.e. lens contaminations moving in relation to  
> the lens – they would receive motion blur.

This happens because defocus is a lens effect, while motion blur is a sensor effect. So defocus physically occurs before motion blur does.

However, this ordering of the defocus and the motion blur can sometimes cause issues in comp if the depth of field effect is quite strong. So feel free to break this ‘rule’ if reversing the order works better for your particular shot.

# Edges

Precise edge work is important, especially for the later stages of compositing and for getting a shot tech finalled.

There are four main areas where you have to check your edges when compositing CG:

1. The edges of the CG objects.

1. The edges of mattes holding out the CG.

1. The edges of the plate being put back on top of the CG.

1. The edges of the frame.

Like we saw in the _Defocus_ section, it’s not common for edges to be pin sharp. And so make sure to soften the edges of the first three things above to match up with the plate.

The edges of both CG objects and roto shapes can also often be too ‘perfect’. Perfectly straight and even, lacking real life detail. It’s a good idea to roughen up such edges to make them less perfect. Here is a nice technique:

[Breaking Up Edges And Surfaces In Nuke](https://www.keheka.com/breaking-up-edges-and-surfaces-in-nuke/)

If you’re compositing CG objects in front of something bright in the plate, or putting a part of the plate back over bright CG objects, make sure to add lightwrap as needed – see the _Lightwrap_ section later on in the guide.

When adding a part of the plate back on top of a CG render, you may run into issues along the edges of the patch. Especially in defocused and motion blurred edges, where you can see details from the original plate through the soft edge – breaking the integration with the CG.

For example, there may be some bright or dark edges and texture where the plate originally has a brighter or darker area which is not fully being covered by the CG.

To fix this, you can gently erode in and extend the edges outwards to kill the unwanted detail – while keeping the object’s colours in the soft edges.

> [!important]
> 
> Always mask your edge extensions by the matte of the CG that the plate patch is going on top of – to avoid unnecessary plate-on-plate edge extensions.

This video explains the technique well:

> [!info] The Secret to Perfectly Merge CGI with Live Action | (Edge Extending)  
> This tutorial covers a technique using Nuke Edge Extending, and goes over the fundamental compositing concept.  
> [https://youtu.be/Ub0MmjYy0b0](https://youtu.be/Ub0MmjYy0b0)  

_The Secret to Perfectly Merge CGI with Live Action | (Edge Extending)._

> [!important]
> 
> Around the 7:02 mark in the video above, to fix the edge problem you can use a **Merge (atop)** instead of eroding the mask. The atop operation puts the foreground over the background, but only within the background’s alpha.

In addition to the native EdgeExtend node in Nuke, there are many great edge extension tools on Nukepedia. For example, this useful gizmo, which pushes pixels outwards based on vectors: [VectorExtendEdge](https://www.nukepedia.com/gizmos/filter/vectorextendedge?ref=keheka.com).

It’s also important to check the edges of the frame. There, you’ll find most of the various crop issues that often happen due to lack of overscan.  
For example, the lens distortion could be pulling the CG (which doesn’t  
have enough overscan) too far in, and you get stretched or black pixels  
at the edges.

Or, without enough overscan, glows tend to flicker at the edges of the frame when bright objects go out of frame.

And, if you’re not careful and mask your edge extensions correctly, you may also see problematic pixels along the edges of the frame.

If your plate has a letterbox (black bars top and bottom) baked into it, make sure to crop out any CG composited into the plate from the letterbox. And, if you’re getting edge extension issues where the black pixels of the letterbox are being pulled into the frame, make sure to add a Crop node first. Set it to crop out the letterbox with the _black outside_ checkbox **unticked** – so that the last row of colour pixels top and bottom stretch outward and fill the letterbox.

[![](https://www.keheka.com/content/images/2024/09/Lettebox_Baked_In.jpg)](https://www.keheka.com/content/images/2024/09/Lettebox_Baked_In.jpg)

[![](https://www.keheka.com/content/images/2024/09/Lettebox_Filled_In.jpg)](https://www.keheka.com/content/images/2024/09/Lettebox_Filled_In.jpg)

_Image with a letterbox (left), and the same image with the letterbox filled in by stretching the top and bottom rows of colour pixels outward with a Crop node._

# Real Life Is Not Perfect

A problem we often face with CG renders is that they tend to be too "perfect".

By that, I mean the edges are perfectly straight, the lighting is perfectly even, the surfaces are perfectly shiny and clean, and so on - the imperfections of real life are missing.

In reality, even a smooth table edge will have micro-scratches, dents, fingerprints, or dirt that break up the specular reflection.

There will be small creases between the joins catching highlights, and occluding light. There may be dust, uneven surfaces, and other small imperfections that we only notice whenever they are missing.

Almost like how we notice that a fan has been constantly running in the background only when it suddenly stops.

So it's important to introduce these imperfections to your CG renders to increase the realism of your composite.

Like mentioned previously, I've written a guide on [Breaking Up Edges And Surfaces In Nuke](https://www.keheka.com/breaking-up-edges-and-surfaces-in-nuke/), which covers this topic.

### Lens Effects

A number of the imperfections of real life are byproducts of the camera and the lens.

Despite using extremely precise machinery, lens manufacturers are subject to real world physical limitations. They cannot create a perfect lens like in a CG camera.

In the real world, the different wavelengths of the light refracting through the glass elements of a lens will not always perfectly converge. Lightrays split up slightly as they refract and hit the sensor in different places, causing chromatic aberration, appearing as colour fringing.

Or they might scatter and reflect as they hit the sensor, causing halation, a blurred glow effect around highlights in the image.

Glass lenses will have microscopic imperfections, especially after some use. There  
may also be lens dust, dirt, water spots, and scratches.

In CG, cameras and lenses are mathematically perfect, and so an important part of CG compositing is "dirtying up" the renders in order to mimic how a real lens would capture something.

That is a large topic in itself, and I have written a massive guide (100+ pages) covering every aspect, here:

[Compositing Photorealistic Lens Effects In Nuke](https://www.keheka.com/compositing-photorealistic-lens-effects-in-nuke/).

A fundamental lens effect which you’ll be using for every CG composite is lens distortion:

# Lens Distortion Workflow

It's important to know the correct workflow for applying lens distortion.

In the vast majority of cases, we want to keep our scan as pristine as possible. That means we don't want to apply any sort of unnecessary filtering to it which softens the pixels and causes a loss of detail.

Which means, we do NOT typically Undistort our scan, merge our CG with it, and then Redistort everything.

Instead, the correct procedure is to leave the scan as-is, and then ONLY apply the lens distortion to the CG, and merge that over the scan.

That way, only the CG takes a filter hit. Which is inevitable and absolutely fine.

> [!important]
> 
> The exception to this is when you need to reproject your scan, for example to change the camera move or use a part of the scan somewhere else in the frame. In that case, you would undistort the scan, project it, and redistort the projection.

Another effect that can happen in the lens is glow:

# Glow

Glow is a scattering of light around a light source (or a reflected light source).

It's usually a soft bloom with an exponential falloff (meaning its  
brightness decays more and more with increasing distance from the  
source) – like the flaring that happens around a bright flame.

[![](https://www.keheka.com/content/images/2024/09/Bonfire.jpg)](https://www.keheka.com/content/images/2024/09/Bonfire.jpg)

_Glow from a bonfire flame._

Glow can happen when there are particles in the air surrounding a local light source, like smoke or fog, which scatter the light. This is what we saw earlier in the _Volume_ section, as volumetric light.

[![](https://www.keheka.com/content/images/2024/09/Volume_Light.jpg)](https://www.keheka.com/content/images/2024/09/Volume_Light.jpg)

_Local glow from volumetric lighting._

Or, it can happen in the camera itself, when the light reflects multiple times inside of the lens and scatters across the sensor. This will typically appear as diffusion or a lens flare.

[![](https://www.keheka.com/content/images/2024/09/Diffusion.jpg)](https://www.keheka.com/content/images/2024/09/Diffusion.jpg)

_Global glow from lens reflections._

And it can often be a combination of these two scenarios.

Which means, glow can light up the immediate surroundings near a light source, and it can also bloom out across the frame to other areas where the light wouldn't normally reach.

With local glow (i.e. light scattering in the surrounding atmos – a.k.a. volumetric light), you'll typically want to break it up with either a noise or a smoke element so that it feels like it is interacting with the environment.  
Otherwise, it can look too smooth and perfect.

And, for the same reason, with global glow (i.e. light scattering in the camera), you'll typically want to break it up with a lens dirt element so that it feels like it's interacting with the lens.

In both cases you would multiply the glow (effect only) with the relevant element using a Merge (multiply), before merging it with the comp using a Merge (plus).

> [!important]
> 
> You would merge any local glow in the relevant part of your script, just like with volumetric lighting. But any global glow should be merged towards the end of your script, _before_ applying the grain, so that it can properly bloom out across the frame.

Without any glow or diffusion, CG renders can sometimes feel too clean and ‘CG’ – like they're not ‘sitting in’, or properly interacting, with the surrounding atmosphere.

Just make sure not to overdo it – it's very easy to go overboard and make the glow too strong and  
cheesy-looking. But most of the time, a subtle effect is all that's needed.

### Creating Glow

At its most basic, a glow effect is simply a three-step process in Nuke:

1. Blur the image.

1. Add the blurred image back over the original image with a Merge (plus).

1. Divide the result by 2 (or multiply by 0.5) to take the average, and avoid  
    doubling up the image in brightness. (You could even skip this step, and instead set the operation in the Merge node in point 2 above to _average_).

From this basic method, we can make the glow effect more realistic by elaborating on the setup.

For example, by making the blur exponential in order to simulate real light falloff. That is, by merging multiple Blur nodes together with increasing _size_ values in the Blur nodes and decreasing _mix_ values in the Merge (plus) nodes.

Chris Fryer explains the technique here:

> [!info] TrueExponentialBlur | Chris Fryer  
> You can download the tool and see the rest of the content on my blog at chrisfryer.  
> [https://youtu.be/lELzmACwwE0](https://youtu.be/lELzmACwwE0)  

_TrueExponentialBlur | Chris Fryer._

> [!important]
> 
> You can experiment with different combinations of _size_ and _mix_ values. For example, doubling the _size_ and halving the _mix_ in each successive Blur/Merge node. Or, using the [Fibonacci sequence](https://en.m.wikipedia.org/wiki/Fibonacci_sequence?ref=keheka.com).

There are many exponential glow gizmos on Nukepedia that you can use, for example [apGlow](https://www.nukepedia.com/gizmos/filter/apglow?ref=keheka.com), [bm_OpticalGlow](https://www.nukepedia.com/gizmos/filter/bm_opticalglow?ref=keheka.com), or [L_ExponBlur](https://www.nukepedia.com/gizmos/filter/l_exponblur?ref=keheka.com).

# Lightwrap

Lightwrap is a physical effect where the light from a light source  
behind an object will bleed around the edges and towards the front of  
the object.

[![](https://www.keheka.com/content/images/2024/09/lightwrap.jpg)](https://www.keheka.com/content/images/2024/09/lightwrap.jpg)

_The sunlight ‘wraps around’ the edges of the man_.

[![](https://www.keheka.com/content/images/2024/09/Bonfire_Glow.jpg)](https://www.keheka.com/content/images/2024/09/Bonfire_Glow.jpg)

_The light from a bonfire wraps around the edges of the people standing in front of it._

It's essentially a glow effect around the edges of subjects or objects in front of bright light sources.

From the camera's point of view, the lightwrap effect is stronger where the  
angle between the light source and the surface of the object is more  
obtuse (wider). And the effect fades off towards the camera side of the  
object, where the object blocks the light.

  

> [!important]
> 
> This is because of the [Fresnel effect](https://en.wikipedia.org/wiki/Fresnel_equations?ref=keheka.com), where more light is reflected at near-grazing incidence.

### Creating Lightwrap

To replicate the lightwrap effect in comp, we can use the Lightwrap  
node in Nuke, or better yet, more advanced tools from Nukepedia or  
elsewhere – such as [LightWrapPro](https://github.com/CreativeLyons/Lyons_Tools_Public/tree/master/06_Filter?ref=keheka.com) by Tony Lyons.

A benefit of compositing CG is that we can use an object's normals and the camera to calculate the so-called **facing ratio matte**. Like we saw in the _Camera Facing Ratio_ section, this matte essentially describes the angles of the object’s surfaces in relation to the camera.

And using that matte, we can improve the physical accuracy of our lightwraps:

[Upgrade Your Light Wraps Using Facing Ratios In Nuke](https://www.keheka.com/upgrade-your-light-wraps-using-facing-ratios/)

Another technique for making the facing ratio pass an be found here:

[Fresnel Reflections In Nuke](https://www.keheka.com/fresnel-reflections-in-nuke/)

It’s very common to overdo the lightwrap effect. 99 times out of a 100, only  
a very subtle effect is all that’s needed. Any more, and it will take  
away from the photorealism of the shot.

> [!important]
> 
> A good rule of thumb is to dial in exactly the perfect amount of lightwrap, and then halve it.

Next, let’s look at breathing a bit of life into your composite:

# Bring It All To Life

CG renders, particularly CG environments, can often look a bit static and lifeless.

Where suitable, add a bit of life to them with various signs of human and  
animal activity, or other dynamic environmental effects, such as:

- [Smoke](https://www.keheka.com/compositing-smoke-in-nuke/)

- Steam

- Crowd elements

- [Fire](https://youtu.be/oYVyEDo9gu0?feature=shared&ref=keheka.com)

- [Heat distortion](https://www.keheka.com/compositing-realistic-heat-distortion-in-nuke/)

- Bird elements

- Rain, wind, or other weather effects

- Traffic

- [Godrays](https://youtu.be/PqbqxnBFOHg?feature=shared&ref=keheka.com)

- [Flickering lights](https://www.keheka.com/add-or-remove-flickering-in-nuke/)

- Billboards

- Lens flares

> [!important]
> 
> This technique for [positioning Cards using the position pass](https://www.keheka.com/snapping-cards-to-a-position-pass-in-nuke/) can really speed things up if you're placing many fire/smoke elements around in a scene.

If you're tracking 2D fire elements onto torches or other moving objects,  
this is a great technique for making the fire appear to be affected by  
the movement:

> [!info] ST_Map fire  
> A quick tutor showing how to use ST_Map to create a realistic looking wavy fire effect  
> [https://youtu.be/L92yF3H6j0U](https://youtu.be/L92yF3H6j0U)  

_ST_Map fire._

One of the final things to apply to your composite is grain:

# Grain

The importance of matching the grain is in a funny state nowadays, where two things are simultaneously true:

1. For much of our work, matching the grain is _technically_ the least important it has been since back in the Standard Definition days of VFX.

1. There is no excuse for less than perfect grain anymore.

It's **obviously** still important to match the grain for work that will be displayed in the cinema.

However, the various streaming platforms obliterate the grain with their  
compression, and all the work we do fine-tuning the grain just won't  
reach the audience's eyeballs there.

But no matter – I'd say that matching the grain should be a matter of principle. It's part  
of crossing the Ts and dotting the Is, and taking pride in our work.

And it has basically never been easier to pretty much automatically get a  
perfect grain match than it is nowadays with the common [DasGrain](https://www.nukepedia.com/gizmos/other/dasgrain?ref=keheka.com) + [Neat Video ReduceNoise](https://www.neatvideo.com/?ref=keheka.com) combo in Nuke.

> [!important]
> 
> You can also learn the inner mechanics of [building your own grain tool](https://www.keheka.com/how-to-build-a-show-specific-grain-tool/), using grain plates from the film shoot.

The combo mentioned above should give you perfect grain in the vast majority of cases, if you set it up correctly.

And there _are_ some things you can do to help to get the best grain possible:

### Denoise Only Positive Pixel Values

Neat Video ReduceNoise doesn't handle negative pixel values too well.

So before connecting up the node, add the Expression node that displays negative values, which we made in the _Problematic Values_ section, to your plate – in a separate branch.

In between the Expression node and the plate, connect an Add node, and increase its _value_ until no more negative pixels are showing.

When there are only positive pixel values, connect the Neat Video ReduceNoise node to the Add node and do your denoising.

Afterwards, to return to the original pixel brightness, copy the Add node and place  
it after the ReduceNoise. Then, add a minus sign in front of the _value_ in the Add node to reverse the lifting.

### Transfer A Denoise

If there are no flat and featureless areas in your plate to sample  
for the denoise, you can often use a different scan from a similar shot in the sequence.

The denoise from one shot can usually be transferred to another shot which was filmed with the same equipment/settings and under the same conditions – which shots in the  
same sequence often tend to be.

Work up your denoise on the other plate, and then copy the ReduceNoise node over to your own plate. (Make sure to do the _add → denoise → reverse add_ trick for both plates, individually).

If the two plates are similar enough, you should get an excellent denoise.

### Plate Ghosts

The original plate grain, particularly where the plate is very  
bright, can cause a ghosting effect when added back on top of the CG.

You can use the _Scatter_ function in the DasGrain node to create a new grain pattern, but make  
sure that you're masking the scattered grain to the areas of the comp with your CG, only.

Keep the original, unscattered grain  
over the original plate where no changes have been made. Otherwise,  
you'll replace all of the grain – degrading the plate by a small amount.

> [!important]
> 
> If you copy the alpha for your CG into the B-pipe of your script, adding  
> to it and holding it out as you go along down the comp, you'll have a  
> matte at the end which is perfect to use as a mask when Keymixing a  
> scattered-grain DasGrain node with an unscattered-grain DasGrain node.

### Match Each Channel Separately

When matching your grain to the plate grain, look at each R, G, and B channel individually.

It’s much easier to spot where the grain isn’t matching in size or intensity than when looking at all three channels combined.

When you’ve matched the grain properly, you’ll be getting close to finalising your composite.

# DI Mattes

After you have carefully balanced everything including the colours in  
your shot, and the composite is approved by the client, your work will  
usually go through a final colour grading process in the Digital  
Intermediate (DI).

Historically, DI was a finishing process for digitising a motion picture and for manipulating the colours and other image characteristics such as sharpness and grain. Nowadays  
most cameras are digital, skipping that first step.

In terms of colour, in DI they will:

- Apply the final creative look to the shots.

- Bring all the shots in a sequence in line, ironing out any colour discrepancies.

- Fix any last minute small issues that do not require the shot to be sent back to the VFX vendor.

Be aware that the client and the colourist will in their efforts above  
invariably push the contrast and saturation in your shot much further  
than you probably would like. And, from a compositor's perspective,  
sometimes 'screw up' the beautiful balanced grade you have spent time  
fine tuning.

DI will often ask for mattes for the separate elements in your shot, to have finer control over the grading process.  
Unfortunately, this often means that they can further break apart your  
composite by pushing these elements of the image around with heavy  
grades. There is not much you can do about this, as it is a creative  
decision by the client about the look of the final images.

However, it _is_ your responsibility to make sure that the composite you deliver to the client does not break apart by pushing the grade _overall_.

This is why we grade the shot two stops up and down, and increase the gamma  
and saturation during tech checking. Your composite should survive such  
'abuse' if you have balanced it correctly.

A seamless composite will allow for some creative freedom in DI, and equally  
important, it will allow for differences in screen/TV/monitor brightness  
and saturation on the viewer's end.

But anyway, back to DI mattes:

Collect the alphas for all the various bits and pieces in your composite that the client has requested a DI matte for, and **make sure to hold them out correctly**.

When connecting any of the mattes to the _mask_ input of a Grade node, **only the requested area should be affected when adjusting the grade**.

For example, let’s say that you’re making three DI mattes:

1. Sky

1. BG Mountains

1. FG City

– Each matte should exclusively affect its specific area when grading.

So the Sky matte should be held out by both the BG Mountain and the FG  
City. And the BG Mountains matte should be held out by the FG City.  
Grading the FG City should not affect anything else.

In essence, DI mattes should fit together like perfect puzzle pieces (much like cryptomattes).

  

> [!important]
> 
> Clamp all your DI mattes to guarantee that their values stay in the 0-1  
> range. That way, you won’t cause issues for the colour grading tools in DI.

DI usually wants their mattes delivered in a certain way, e.g. with a specific naming and render settings. Copy the mattes into the correctly named channels, and make sure you adhere to  
the specifications.

Note that you’ll have to set the Write node to render _all_ channels in order to include custom DI matte channels. So make sure that you remove every layer/channel that you _don’t_ want to include with the render at the end of your comp.

Another thing to watch out for is the bounding box. Check to see that your DI  
mattes have been cropped the same way as your composite. This also includes any baked-in letterbox (black bars at the top and bottom of the frame) in the composite – make sure to crop that as needed in the DI mattes, as well.

# Tech Check

Finally, when you have completed your composite and gotten it  
creatively approved by the client, it’s time to fix any remaining  
technical issues.

> [!important]
> 
> It’s good practice to fix technical issues along the way as you comp a shot,  
> to avoid having a ton of issues to fix at the very end.

I have made a handy checklist for things to look out for when tech checking your shot:

[Tech Check Checklist For Compositors](https://www.keheka.com/tech-check-checklist/)

### The Plate Is ‘Holy’

A rule of thumb in VFX is to avoid changing the plate (or changing it as little as possible) in the areas of the frame where you’re not directly adding/removing things to/from.

> [!important]
> 
> This includes even small things like grain. Typically, you should keep all  
> the original plate grain where you haven’t changed anything. Denoising  
> and regraining can introduce a small amount of softness to the plate.

And anything that you composite into the plate – for example a CG object – should match to the plate and not the other way around.

For example, typically, you would warp the CG to match to the plate if there is a misalignment, and not the plate to the CG.

The Director Of Photography carefully had the set lit a certain way, and framed and shot the scene to get a certain look. And our work should stay true to that vision.

So when you do your tech checking, compare the comp and the original plate with a difference key to make sure you are only affecting the areas of the plate that you have to.

In addition to the CG you’ve inserted into the plate, there may be other things affecting the plate:

- Interactive lighting from a fire.

- Heat distortion from a fighter jet.

- Environmental damage by a creature.

- Specific shadows or reflections being cast.

– In which case, it’s okay to alter the plate. But try to keep any changes motivated. There is no reason to change the grain over an area of the plate which has had no changes made to it in the comp.

In other cases, the comp may be so different to the original plate that it doesn’t make sense to keep the plate exactly as it was. You may have to do a so-called ‘sympathy grade’ to align the lighting in your plate closer with the CG and the direction the composite is taking.

But generally, try to keep the plate part of your composite pristine.

And that wraps up this guide about CG compositing!