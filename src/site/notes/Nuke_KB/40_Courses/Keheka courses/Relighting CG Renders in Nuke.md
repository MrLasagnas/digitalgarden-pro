---
{"dg-publish":true,"permalink":"/nuke-kb/40-courses/keheka-courses/relighting-cg-renders-in-nuke/"}
---

# **Relighting Or Just Adding Light?**

The tutorials I’ve found online for relighting CG renders in Nuke mainly show how to add  
additional lighting from another light source to a CG render.

This can be very useful for integrating 2D elements with your CG, or for adding extra _umph_ to  
a shot with some rim light, for example. And so we’ll take a look at methods for doing this in the first part of the article.

However, I haven’t really seen tutorials for _replacing_ the lighting in a CG render, i.e. changing the lighting to suit your needs. This can be great for look development and experimenting, or for last  
minute changes when there is no time to re-render the CG.

And so the second part of this article will cover how to do that.

Let’s start with a quick and easy technique for adding additional lighting to CG renders. For that, we can use some familiar render passes and tools, but in a different way than we normally would:

# **PMattes With A Twist**

To understand how the technique works, let's first look at the differences between a position pass and a normal pass.

  

> [!important]
> 
> In this article, 'position' and 'normal' passes specifically refer to **world** position and normal passes.

  

A **position** pass contains the XYZ **coordinates** of the surfaces of objects visible to the camera. Each pixel in the pass can essentially have any **unique** rgb values, depending on where the object is located in 3D space at that pixel. And coordinates nearby each other will have **similar** (but not the same) position values.

A **normal** pass contains the XYZ **orientation** values of the surfaces visible to the camera. Each pixel in the pass has RGB values between -1 and 1, representing how the object is oriented in 3D space at that point.

For instance, if a surface is perpendicular to the X-axis and faces its positive direction, the red channel of the normal pass will have a value of 1 in that area. Conversely, if the surface faces the negative direction of the X-axis, the red channel will have a value of -1.

When a surface is exactly parallel to the X-axis, it will have a value of 0. Any angles in between will have values ramping up or down towards 1 or -1.

The same principle applies to the Y and Z axes, corresponding to the green and blue channels, respectively.

Unlike the position pass, multiple pixels in the normal pass can have identical values—this occurs when surfaces face the same direction.

  

> [!important]
> 
> Surfaces facing in the same direction will all have **unique** _position_ values, but the **same** _normal_ values.

  

Knowing this, we can manipulate familiar tools to do relighting.

Position matte gizmos such as [aPMatte](https://www.keheka.com/r/18fa7892?m=4f37be22-d28d-40d8-aafb-990730b69481) are normally used to create an alpha matte **based on similar values** in the position pass (i.e. an area surrounding a certain coordinate).

  

> [!important]
> 
> You can see this in action yourself by color-picking a green screen instead of a position pass using the gizmo and setting the radius value very low (e.g., 0.1-0.5). The resulting alpha matte will consist of areas with similar green screen values. (However, this isn't really suitable as a keyer due to its limited functionality.) Position matte gizmos essentially turn pixels with RGB values similar to the one you pick into an alpha matte.

  

However, if we connect a **normal** pass instead of a position pass to the position matte gizmo, we can create an alpha matte for **all** the surfaces facing a specific direction.

This works because the gizmo creates a matte based on similar values—and surfaces facing the same direction will have the same or similar values in the normal pass.

To illustrate this, I've stacked some cubes on top of each other in Nuke:

[![](https://ci3.googleusercontent.com/meips/ADKq_Nav2Wlh5p1iD1erBqe1l2gpZXuJ6ABn45mQRnBj5xB-X-ojb2MVy7XZgFwZInsQ33dmyZyXXXyiRCvW6nnhxXoReIsKSFnpotqDlzWd=s0-d-e1-ft#https://www.keheka.com/content/images/2024/06/Boxes.jpg)](https://ci3.googleusercontent.com/meips/ADKq_Nav2Wlh5p1iD1erBqe1l2gpZXuJ6ABn45mQRnBj5xB-X-ojb2MVy7XZgFwZInsQ33dmyZyXXXyiRCvW6nnhxXoReIsKSFnpotqDlzWd=s0-d-e1-ft#https://www.keheka.com/content/images/2024/06/Boxes.jpg)

_Stacked Cubes._

[![](https://ci3.googleusercontent.com/meips/ADKq_NZXoYUDNaDPLtYaHMgvTj83BzsORA3Ly44a_u3HtAHJeNXxBTLT8VqPyPGYR9egNqk7AJlTCr-nmYzwlcZ2-HFIFYvpRxAf0S_Q1_n5Cg=s0-d-e1-ft#https://www.keheka.com/content/images/2024/06/PMatte.jpg)](https://ci3.googleusercontent.com/meips/ADKq_NZXoYUDNaDPLtYaHMgvTj83BzsORA3Ly44a_u3HtAHJeNXxBTLT8VqPyPGYR9egNqk7AJlTCr-nmYzwlcZ2-HFIFYvpRxAf0S_Q1_n5Cg=s0-d-e1-ft#https://www.keheka.com/content/images/2024/06/PMatte.jpg)

[![](https://ci3.googleusercontent.com/meips/ADKq_Nam2tiliV1jSgK5skGEWk0519j6LTVzSm09sjrkfnM5wGPcbByal_SwNmBX7HuSKIwCVRAl6YNRFatDHn6E6mfg6z8AJz1YrIO2eBnAmA=s0-d-e1-ft#https://www.keheka.com/content/images/2024/06/NMatte.jpg)](https://ci3.googleusercontent.com/meips/ADKq_Nam2tiliV1jSgK5skGEWk0519j6LTVzSm09sjrkfnM5wGPcbByal_SwNmBX7HuSKIwCVRAl6YNRFatDHn6E6mfg6z8AJz1YrIO2eBnAmA=s0-d-e1-ft#https://www.keheka.com/content/images/2024/06/NMatte.jpg)

_PMatte (left) vs. NMatte (right)._

In the images above, I color-picked the same exact spot in the frame (the center of the bottom cube) but used different settings in the position matte gizmo:

On the left, I color-picked from the **position** pass and set the radius to 21.

On the right, I color-picked the same spot but from the **normal** pass and set the radius to 1.8.

> [!important]
> 
> When connecting a **normal** pass instead of a position pass to a position matte gizmo, **the radius setting becomes quite sensitive**. High radius values can excessively increase the surface angle included in the matte, resulting in unwanted areas being added. (For example, the default radius of 5 in the aPMatte is usually too high for this method.) With a **position** pass, the radius value depends on the object's size (i.e., it can be quite high sometimes). However, with a **normal** pass, the value is independent of object size (as it's purely based on orientation). Typically, setting the radius to **a value between 1 and 2** is a good starting point.

As evident above, the NMatte is much more defined and directional than the PMatte—a beneficial characteristic for relighting CG renders.

# **Limiting The NMatte**

As mentioned earlier, when color-picking the normal pass this way, you'll get an alpha matte containing **every** surface visible to the camera that's facing that particular direction. However, you might want to create a matte for just a section of a building's facade, for example. In such cases, you can mask the NMatte with a localized PMatte, including only the areas you want to keep in your matte.

For a more [organic matte](https://www.keheka.com/r/49cf2501?m=4f37be22-d28d-40d8-aafb-990730b69481), you can break up the NMatte by masking it with a **PNoise**. Alternatively, you could mask the NMatte with a **PRamp** to smoothly fade it off in a particular direction.

The PRamp technique is particularly useful if you want the light to fall off over a certain distance. (However, you can also automate this with the next relighting technique we'll discuss below.)

> [!important]
> 
> You can create both PNoise and PRamps using the aforementioned [aPMatte](https://www.keheka.com/r/07213832?m=4f37be22-d28d-40d8-aafb-990730b69481) gizmo.

# **Using The NMatte**

Now, how can we effectively use our NMatte to relight the CG render?

The simplest method for adding light to your render (or reducing existing light) is to use the NMatte directly as a mask for a Grade node. This allows you to adjust the CG's brightness up or down in specific areas.

Let's consider an example where you want to add a flash of light from a particular direction:

[![](https://ci3.googleusercontent.com/meips/ADKq_NYzgz69FCdxNB-dnwSi5USSPRGuU7iNTgiyC5sUzxv0MU0TnyZejBFPVoHAUyj0DNNWGSEXmJ38HChUhhUJExzTJkes30IR3jJ-LaH3PhI=s0-d-e1-ft#https://www.keheka.com/content/images/2024/06/Boxes-1.jpg)](https://ci3.googleusercontent.com/meips/ADKq_NYzgz69FCdxNB-dnwSi5USSPRGuU7iNTgiyC5sUzxv0MU0TnyZejBFPVoHAUyj0DNNWGSEXmJ38HChUhhUJExzTJkes30IR3jJ-LaH3PhI=s0-d-e1-ft#https://www.keheka.com/content/images/2024/06/Boxes-1.jpg)

[![](https://ci3.googleusercontent.com/meips/ADKq_NYEY8-a1N2hqoHHVNVJeMeONzmFkLriVSeatom3k5Eraq00Juwf4nwYghxDDgbd9hmhJD85CmxjaGO1dkPUDk8073HRtqn6hBz1wKqjS6Th=s0-d-e1-ft#https://www.keheka.com/content/images/2024/06/NMatte-1.jpg)](https://ci3.googleusercontent.com/meips/ADKq_NYEY8-a1N2hqoHHVNVJeMeONzmFkLriVSeatom3k5Eraq00Juwf4nwYghxDDgbd9hmhJD85CmxjaGO1dkPUDk8073HRtqn6hBz1wKqjS6Th=s0-d-e1-ft#https://www.keheka.com/content/images/2024/06/NMatte-1.jpg)

[![](https://ci3.googleusercontent.com/meips/ADKq_NZSbDhbiMv18ulblVa0L3341IvcjesnlA_fQeL5hCEQOHKQCV-zhAX8USk3ljT_zi-V2ge94CkhznRdx30rK2lQf-EfExlZ30XJr29S2X-98qmZDVY=s0-d-e1-ft#https://www.keheka.com/content/images/2024/06/Boxes_Relight.jpg)](https://ci3.googleusercontent.com/meips/ADKq_NZSbDhbiMv18ulblVa0L3341IvcjesnlA_fQeL5hCEQOHKQCV-zhAX8USk3ljT_zi-V2ge94CkhznRdx30rK2lQf-EfExlZ30XJr29S2X-98qmZDVY=s0-d-e1-ft#https://www.keheka.com/content/images/2024/06/Boxes_Relight.jpg)

_The CG render (left), the NMatte (middle), and the relighting, i.e. graded result (right)._

[![](https://ci3.googleusercontent.com/meips/ADKq_NbUNnSUORJRMAx1HemYS6Hhz1n-JMW_pWR6Ql7U8U4RB124woqqdg00gnKOW347Vcx7UpvPby5QLNSLAQtWaLCoANvgcltBikvZlaTWxKYGOPJpGzPc6wl8Ww=s0-d-e1-ft#https://www.keheka.com/content/images/2024/06/Grade_using_NMatte.png)](https://ci3.googleusercontent.com/meips/ADKq_NbUNnSUORJRMAx1HemYS6Hhz1n-JMW_pWR6Ql7U8U4RB124woqqdg00gnKOW347Vcx7UpvPby5QLNSLAQtWaLCoANvgcltBikvZlaTWxKYGOPJpGzPc6wl8Ww=s0-d-e1-ft#https://www.keheka.com/content/images/2024/06/Grade_using_NMatte.png)

_Grading the CG using the NMatte as a mask._

This method of using the NMatte to grade the CG render is great for when you’re adding more light to your CG from **another light source which has been added in comp**. For example, a 2D fire or explosion element. – Just gain up and add some colour as needed in the Grade node.

If you’re adding a 2D element **placed on a Card** to your composite, you can also use another method for relighting your CG – which is more interactive in Nuke’s 3D space.

# **The ReLight Node**

To illustrate this method, let's use an example with a CG render of a large marble sphere and a 2D fire element on a Card. We'll composite the fire element off to the side and behind the sphere, with the flames lighting up the sphere realistically.

Let's start with a basic overview before diving into the details. First, here's our CG render of the sphere:

  

[![](https://ci3.googleusercontent.com/meips/ADKq_NYgSSwGfKoHM4Unsvm2QUO6JIAvJ5E5qmPsMiZLomIDzQaqv6uuza6cP-CmaUwWpGj8W5P-sUqh1NU_NuHUjoCpu1-6oG4ueF6sa2CTAwjEQ8Q5PWpnV2HeJ3Zd=s0-d-e1-ft#https://www.keheka.com/content/images/size/w1600/2024/06/CG_render.jpg)](https://ci3.googleusercontent.com/meips/ADKq_NYgSSwGfKoHM4Unsvm2QUO6JIAvJ5E5qmPsMiZLomIDzQaqv6uuza6cP-CmaUwWpGj8W5P-sUqh1NU_NuHUjoCpu1-6oG4ueF6sa2CTAwjEQ8Q5PWpnV2HeJ3Zd=s0-d-e1-ft#https://www.keheka.com/content/images/size/w1600/2024/06/CG_render.jpg)

_A CG render of a large marble sphere._

We'll place a fire element on a Card behind the sphere in 3D space, using a point cloud of the sphere as a reference for positioning. Then we'll render the fire using a ScanlineRender node.

> [!important]
> 
> To create a point cloud of the geometry, you can use either a **PositionToPoints** or **DeepToPoints** node. This makes it easier to position your Cards and Lights accurately.

Here's how the initial composite of the sphere and fire looks:

[![](https://ci3.googleusercontent.com/meips/ADKq_NYyFAPDTlFBxqyyF6Ufn2ALyJ6IecF-lbzUgTO8Cb7pjdOO0NdNzdiMITs3pih5Segn0vcuBOGHpcQiq-nWpxTUao8tMPm7tk-E7yM5cOhHoylQb9FqVVLRGQVODNlnl-Qk3_mNLdvNEQ=s0-d-e1-ft#https://www.keheka.com/content/images/size/w1600/2024/06/CG_render_Fire_on_Card.jpg)](https://ci3.googleusercontent.com/meips/ADKq_NYyFAPDTlFBxqyyF6Ufn2ALyJ6IecF-lbzUgTO8Cb7pjdOO0NdNzdiMITs3pih5Segn0vcuBOGHpcQiq-nWpxTUao8tMPm7tk-E7yM5cOhHoylQb9FqVVLRGQVODNlnl-Qk3_mNLdvNEQ=s0-d-e1-ft#https://www.keheka.com/content/images/size/w1600/2024/06/CG_render_Fire_on_Card.jpg)

_The initial composite: A 2D fire element placed on a Card slightly behind and to the right of the CG sphere. Note the absence of interactive lighting on the CG render at this stage._

Next, we’ll place a Light where the core of the fire is located on the Card in 3D space:

[![](https://ci3.googleusercontent.com/meips/ADKq_NZKVgOckl-z1J2ZcU80LWi4Ak0WOQmpl_wkeX2WfOY8CZDHRO1TWaRPnF7MC71kpfz5lqts3fqZMNhQOthlipmqis2okwc9uNEtu24wsXP88S2Vih-efxt0Xtk25ulZRw=s0-d-e1-ft#https://www.keheka.com/content/images/size/w1600/2024/06/ReLight_Scene.png)](https://ci3.googleusercontent.com/meips/ADKq_NZKVgOckl-z1J2ZcU80LWi4Ak0WOQmpl_wkeX2WfOY8CZDHRO1TWaRPnF7MC71kpfz5lqts3fqZMNhQOthlipmqis2okwc9uNEtu24wsXP88S2Vih-efxt0Xtk25ulZRw=s0-d-e1-ft#https://www.keheka.com/content/images/size/w1600/2024/06/ReLight_Scene.png)

_A light matching the fire element's color and intensity is positioned at the Card's location in 3D space. A point cloud of the CG render serves as a reference for accurately placing both the Card and the Light._

When we feed the CG render and our Light (along with other components detailed in the step-by-step guide below) into a ReLight node, we achieve this result:

[![](https://ci3.googleusercontent.com/meips/ADKq_NaIzbM4ClxKAFKVvz_W9_o2VfKFhNtrt0G2pqA1veeKAx-Of4z0UMzdJ4ZYBB-hd79sEBQS5g_9S11ZjvL753PePKq7XLg5RKEaylUmqQ6JNfroo_dr7Az4y_4I7xuG6Y4=s0-d-e1-ft#https://www.keheka.com/content/images/size/w1600/2024/06/ReLight_Output.jpg)](https://ci3.googleusercontent.com/meips/ADKq_NaIzbM4ClxKAFKVvz_W9_o2VfKFhNtrt0G2pqA1veeKAx-Of4z0UMzdJ4ZYBB-hd79sEBQS5g_9S11ZjvL753PePKq7XLg5RKEaylUmqQ6JNfroo_dr7Az4y_4I7xuG6Y4=s0-d-e1-ft#https://www.keheka.com/content/images/size/w1600/2024/06/ReLight_Output.jpg)

_The output of the ReLight node._

And compositing that on top of our initial comp, we get this:

[![](https://ci3.googleusercontent.com/meips/ADKq_NbipkY8CbbaqJYfKYOLdhcqECrZ37tF8HCXFKZYjuHkdFWy-SeGtLOQE8L7gWDJh1SGEXE3n1ilu-d8z8Acx_dJ4LNxZMI2mvgvjTbdgo55LNv5nws6KCfcOT0oCeD7RXlfmz65P19c2NvdwyJ7PG4_Veld=s0-d-e1-ft#https://www.keheka.com/content/images/size/w1600/2024/06/CG_render_Fire_on_Card_Relighting.jpg)](https://ci3.googleusercontent.com/meips/ADKq_NbipkY8CbbaqJYfKYOLdhcqECrZ37tF8HCXFKZYjuHkdFWy-SeGtLOQE8L7gWDJh1SGEXE3n1ilu-d8z8Acx_dJ4LNxZMI2mvgvjTbdgo55LNv5nws6KCfcOT0oCeD7RXlfmz65P19c2NvdwyJ7PG4_Veld=s0-d-e1-ft#https://www.keheka.com/content/images/size/w1600/2024/06/CG_render_Fire_on_Card_Relighting.jpg)

_Adding the output of the ReLight node to the initial composite._

The sphere now appears to be illuminated by the fire element. While this composite isn't the most visually stunning, it effectively demonstrates the concept. Importantly, this additional lighting is interactive—it will update accurately if the camera moves.

Let's break down the process step by step:

1. Import your **CG render** into Nuke, ensuring it includes the **normal** and **position** passes as layers. Copy these into the stream if necessary.

1. Create a **ReLight** node and connect its **color** input to your CG render.

1. In the **properties** of the ReLight node, select your normal and position passes from their respective drop-down menus. To retain the original alpha from the **color** input, tick the **use alpha** checkbox. This is generally recommended, as it prevents the relighting from introducing harsh, aliased edges.

1. Connect the **lights** input of the ReLight node to a **Light** node, or to multiple Light nodes via a **Scene** node.

1. After connecting the **lights** input, a third input—**cam**—will appear on the left side of the ReLight node. Connect this to your **Camera**.

1. Once the camera is connected, the final **material** input will appear on the left side of the ReLight node. Connect this to a shader of your choice, such as the **Phong** shader.

1. **Remove all channels except RGB** from the ReLight node using a Remove (keep) node. Ensure there's **no alpha**, as we'll be adding the output to our CG render and don't want to alter its alpha.

1. With the main ReLight setup complete, you can now adjust the Light to relight your CG. To simulate the fire illuminating the sphere, **position your Light near the core of the fire on the Card in 3D space**.

1. In the **Light** node's properties, **select a color** matching your fire element. Here, you can also modify other settings, such as adding light **falloff**.

1. [Add flickering](https://www.keheka.com/r/c282c94c?m=4f37be22-d28d-40d8-aafb-990730b69481) to the Light to mimic your fire element's behavior.

1. Merge (plus) the ReLight node's output (A) with your original CG render (B) to incorporate the additional lighting.

1. Finally, composite this result with your fire element.

[![](https://ci3.googleusercontent.com/meips/ADKq_Nbd7nzFz9nSrx9i1zwOA60-scHPzsxNMe_z2ITUGrbUdD4Cm1BM0l5hDAfPLeblZ4TDDPV_0J11FNfPMwxHihG9Tm4VWw0ZBWH9-sHwUtLmKdyRiG61Gbg8nyruZSeIoA=s0-d-e1-ft#https://www.keheka.com/content/images/size/w1600/2024/06/ReLight_Setup.png)](https://ci3.googleusercontent.com/meips/ADKq_Nbd7nzFz9nSrx9i1zwOA60-scHPzsxNMe_z2ITUGrbUdD4Cm1BM0l5hDAfPLeblZ4TDDPV_0J11FNfPMwxHihG9Tm4VWw0ZBWH9-sHwUtLmKdyRiG61Gbg8nyruZSeIoA=s0-d-e1-ft#https://www.keheka.com/content/images/size/w1600/2024/06/ReLight_Setup.png)

_The ReLight setup._

If you want to reduce the lighting in your CG render, you can also shuffle the ReLight node's output into the alpha channel, clamp it, and use it as a mask for grading your CG render.

With these two techniques, you can both enhance and diminish the lighting in your CG renders. At the end of the article, I'll share some tutorials featuring additional methods you can employ.

# **Replacing The Lighting In A CG Render**

So far, we've focused on adding extra light to our CG render or using it to reduce existing light. However, there are times when you might want to alter the position or direction of a light in your CG render—for example, when developing the look for a shot or sequence.

In these cases, it's beneficial to be able to **replace** the light in the render, rather than simply adding more light on top of it.

Let's explore how to achieve this, starting with the NMatte method:

Begin by shuffling out the **light group** or render pass (e.g., **diffuse**) you wish to modify. Then, **subtract** it (A) from the beauty pass (B) using a Merge (from) node.

[![](https://ci3.googleusercontent.com/meips/ADKq_NZPyOiIME-hO605aJ1USfw4fJAuE39w6UEZ9_QbeBZB7b_dbglSDWx5m8qt9AcUoqxYZwHa9HNjeUA1VbZNBeSDtxFGUOKhRPVAPIADX2B2h7-rSBPTUds=s0-d-e1-ft#https://www.keheka.com/content/images/2024/06/Subtracting_LG01.png)](https://ci3.googleusercontent.com/meips/ADKq_NZPyOiIME-hO605aJ1USfw4fJAuE39w6UEZ9_QbeBZB7b_dbglSDWx5m8qt9AcUoqxYZwHa9HNjeUA1VbZNBeSDtxFGUOKhRPVAPIADX2B2h7-rSBPTUds=s0-d-e1-ft#https://www.keheka.com/content/images/2024/06/Subtracting_LG01.png)

_Subtracting a light group from the beauty._

Now, you'll have the beauty minus that specific light group/render pass in your main pipe.

Next, let's separate the raw lighting from the texture so we can adjust just the lighting.

Shuffle out the texture pass (also known as the albedo) from the CG render. Then, divide the shuffled-out light group/render pass (A) by the albedo (B) using a Merge (divide) node.

[![](https://ci3.googleusercontent.com/meips/ADKq_NbycxloOBNAR0dZMgWuiwxXBJSA3-L_ljiks9Ce8ZD9crz0p6vdfny-iD9IpHaSyosCVLs_KKZPWIMsXH1A5XDA5s7y7486Aw7NU5UizAtEGRBpQpS4iIA5JA1DqUcuMoRhIxMg8N4=s0-d-e1-ft#https://www.keheka.com/content/images/2024/06/Separating_Lighting_And_Texture.png)](https://ci3.googleusercontent.com/meips/ADKq_NbycxloOBNAR0dZMgWuiwxXBJSA3-L_ljiks9Ce8ZD9crz0p6vdfny-iD9IpHaSyosCVLs_KKZPWIMsXH1A5XDA5s7y7486Aw7NU5UizAtEGRBpQpS4iIA5JA1DqUcuMoRhIxMg8N4=s0-d-e1-ft#https://www.keheka.com/content/images/2024/06/Separating_Lighting_And_Texture.png)

_Separating the lighting from the texture._

> [!important]
> 
> Depending on the renderer used, you may already have the raw lighting pass available. If so, you can skip the step of dividing out the texture.

At this point, we have both the original raw lighting and the texture separated.

To replace the original raw lighting, create a Constant node and color-pick its value from the original raw lighting. This ensures you match the color and intensity of the original light as a starting point—you can adjust this later if needed.

> [!important]
> 
> If your CG render includes overscan, add an AdjBBox node to the Constant to match the overscan area.

Now, mask this Constant (B) with your NMatte (A) using a Merge (mask) node. This creates your new raw lighting pass.

[![](https://ci3.googleusercontent.com/meips/ADKq_NYWtJn5wCiVYdf8qgNWEEY3KHYyME3FJzAsz0tlHhSndV6h6DniIopSsbfZpcgTqQh1BNfZs-BlDp63fbHPFRCrQ35mlUa4L5C1G3QzCy19ByzOd9ruaxdOxEUO0zOVNxPUxhD2Q4r39nTrkT2OFW0=s0-d-e1-ft#https://www.keheka.com/content/images/size/w1600/2024/06/Creating_The_New_Raw_Lighting.png)](https://ci3.googleusercontent.com/meips/ADKq_NYWtJn5wCiVYdf8qgNWEEY3KHYyME3FJzAsz0tlHhSndV6h6DniIopSsbfZpcgTqQh1BNfZs-BlDp63fbHPFRCrQ35mlUa4L5C1G3QzCy19ByzOd9ruaxdOxEUO0zOVNxPUxhD2Q4r39nTrkT2OFW0=s0-d-e1-ft#https://www.keheka.com/content/images/size/w1600/2024/06/Creating_The_New_Raw_Lighting.png)

_Creating the new raw lighting pass._

Multiply this new pass (A) with the albedo pass (B) using a Merge (multiply) in  
order to create the new light group/render pass.

[![](https://ci3.googleusercontent.com/meips/ADKq_NZyTxDsP-olnM9bz3f035kxLxiajH5bl277CIEx-NDGIcfe2MHz3gBfagJNKRZmvzBC4DPJmCxx3dR1tsyVg2GGA11_RLofdLAi8R_CCnX7LjROiPtlsPC6Ky1U-uf8qh7UIvRpJQ=s0-d-e1-ft#https://www.keheka.com/content/images/size/w1600/2024/06/Creating_The_New_LG.png)](https://ci3.googleusercontent.com/meips/ADKq_NZyTxDsP-olnM9bz3f035kxLxiajH5bl277CIEx-NDGIcfe2MHz3gBfagJNKRZmvzBC4DPJmCxx3dR1tsyVg2GGA11_RLofdLAi8R_CCnX7LjROiPtlsPC6Ky1U-uf8qh7UIvRpJQ=s0-d-e1-ft#https://www.keheka.com/content/images/size/w1600/2024/06/Creating_The_New_LG.png)

_Creating the new light group._

Finally, add this new light group (A) back into the main pipe (B) using a Merge (plus).

[![](https://ci3.googleusercontent.com/meips/ADKq_Na4WyeLOc05LuLPAg8OI8YCJHIhojZq2C8rbilPOdb0J3LGNkAqIYuHS5clweRM_gy1zrUQPasIMxrF3LGOi1UteNuTBqbyEqakoJmt2esdIBgzNJIJ9_phUkHOmqCeWmOddgDZ9__ohPxk_A=s0-d-e1-ft#https://www.keheka.com/content/images/size/w1600/2024/06/NMatte_Setup_Replace_LG01.png)](https://ci3.googleusercontent.com/meips/ADKq_Na4WyeLOc05LuLPAg8OI8YCJHIhojZq2C8rbilPOdb0J3LGNkAqIYuHS5clweRM_gy1zrUQPasIMxrF3LGOi1UteNuTBqbyEqakoJmt2esdIBgzNJIJ9_phUkHOmqCeWmOddgDZ9__ohPxk_A=s0-d-e1-ft#https://www.keheka.com/content/images/size/w1600/2024/06/NMatte_Setup_Replace_LG01.png)

_Replacing a light group using the NMatte setup._

And that’s the NMatte relighting setup complete.

To use the ReLight node setup instead, follow all of the above steps, except:

1. Colour-pick the **Color** value of the Light node in the ReLight setup (instead of colour-picking the value of a Constant node) from the raw lighting.

1. Instead of using the Constant node masked by the NMatte, just multiply the  
    output of the ReLight node setup directly with the texture pass.

[![](https://ci3.googleusercontent.com/meips/ADKq_Nah5zKsaulQXoy_2VWaBgAgySZKh5u6XvCp0WJ7RGpCsYy5OIZV1ocXQzCr8H3s_4FT2T5QM_TppDXnIUNYveNwEeZ9KgIn8ScA5c0A9qEsPANvRQY5pYV-ujmeIiZjQBMWyF0AQoWPQ1SRQfU=s0-d-e1-ft#https://www.keheka.com/content/images/size/w1600/2024/06/ReLight_Setup_Replace_LG01.png)](https://ci3.googleusercontent.com/meips/ADKq_Nah5zKsaulQXoy_2VWaBgAgySZKh5u6XvCp0WJ7RGpCsYy5OIZV1ocXQzCr8H3s_4FT2T5QM_TppDXnIUNYveNwEeZ9KgIn8ScA5c0A9qEsPANvRQY5pYV-ujmeIiZjQBMWyF0AQoWPQ1SRQfU=s0-d-e1-ft#https://www.keheka.com/content/images/size/w1600/2024/06/ReLight_Setup_Replace_LG01.png)

_Replacing a light group using the ReLight node setup._

Remember to adjust the shader in the ReLight node setup based on the render pass you're replacing:

- For the diffuse pass, use the **Diffuse** shader

- For the specular pass, use the **Specular** shader

- For the emission pass, use the **Emission** shader

- For the transmission pass, use the **Transmission** shader

- For a light group, use either a **combination of the above** (daisy-chained) or a **Phong** shader

> [!important]
> 
> To achieve a more accurate recreation of the shading, import the various maps (diffuse map, specular map, etc.) used in the 3D software to produce the CG render, and connect them to your shaders in Nuke.

The ReLight node setup offers more versatility than the NMatte setup, as it allows you to separate lighting into individual components. For instance, you can replace a problematic specular reflection pass—something the NMatte setup struggles to create.

However, if you don't have access to these maps, keep in mind that the ReLight node and NMatte setups can produce different looks. It's best to experiment with both approaches to determine which works best for your particular shot.

# **Other Relighting Methods**

Below are some other methods for relighting CG renders using the normal pass:

  

[https://www.youtube.com/watch?v=VZljzMID4So](https://www.youtube.com/watch?v=VZljzMID4So)

_Relighting using Normal Pass._

  

[https://www.youtube.com/watch?v=7un3iJC0FBg](https://www.youtube.com/watch?v=7un3iJC0FBg)

_Make Your Own Relight tool In Nuke._

V-Ray for Nuke also has a [Mesh based light](https://www.keheka.com/r/4b25f722?m=4f37be22-d28d-40d8-aafb-990730b69481) which you can use for relighting your CG. And Chris Fryer has made the [SoftBoxRelight](https://www.keheka.com/r/17606e35?m=4f37be22-d28d-40d8-aafb-990730b69481) tool, which does a similar thing with a native Blinkscript solution.

And, if you want to relight 2D elements which _don’t_ have a normal or position pass, such as a blood splatter or smoke element, the end of this video shows some great techniques:

  

[https://www.youtube.com/watch?v=eFarvkyJ22I](https://www.youtube.com/watch?v=eFarvkyJ22I)

_2D Relighting Hacks._