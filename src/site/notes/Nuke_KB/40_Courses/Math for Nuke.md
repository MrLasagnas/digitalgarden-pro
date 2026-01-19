---
{"dg-publish":true,"permalink":"/nuke-kb/40-courses/math-for-nuke/"}
---




---

## Merge Modes

#### Plus

> Plus = A + B

- to merge light sources, glows. 
- Additive keying.

Beware of negative values.

#### Subtraction

Input sensitive, so we have two inputs.

> Minus : A - B
> From : B - A 

- for differences. Like, remove rain from a still plate.
- to isolate light sources before "plus" them back.
- to find negative values by minus black constant.
- to find details by substracting slightly blurred plate, or edges by substracting slightly transformed plate -> ==Frequency separation==
-  to mitigate lens effects with a single multiply for a big stack of effects 
-![Pasted image 20251119130427.png](/img/user/0_Assets/attachments/Pasted%20image%2020251119130427.png)

> Difference = abs(A-B)

Keeps only positive numbers.
- To find differences between elements. ex: to target where to regrain or keep plate grain.

> Invert = 1 - A




#### Multiplication


> Multiply = A * B

- For masking or filtering
- To bring shadows/occlusion
- To multiply RAW light passes with texture/color


BE CAREFUL : Multiplication doesnt work as intended with two negative values.

> Mask = B * a (alpha of a)

So you can directly multiply B with a roto for example.

> In = A * b (alpha of b)

Invert inputs of mask merge mode.

#### Division

> Divide = A / B

- Can be used for normalization. Like normalize a flat vignette to then remove it from a plate.
- Can do white balance with the same principle.
- To normalize a depth pass ( to use as a mask for eg.)
- To extract raw light from a lit object (with albedo)
- To treat details separatly from color.
- To remove some flicker.
![Pasted image 20251119153159.png](/img/user/0_Assets/attachments/Pasted%20image%2020251119153159.png)


### Over

> Over = A + B * (1-a)

Punches a hole in B with A inverted alpha channel, then 'plus' the two inputs.

### Under

> Under = A * (1-b) + B

Invert operation as Over.

### Matte

> Matte = A * a + B * (1 - a)

Same as Over but with a premult applied to A input with its alpha before the over operation itself. Be carefull as to not double premult by using it. Might be better to use a premult node and then a Over operation.

### Atop

> Atop = A * b + B * (1-a)

Can be useful to merge plate back on top of CG for example.
![Pasted image 20251120094348.png](/img/user/0_Assets/attachments/Pasted%20image%2020251120094348.png)


### Xor

> Xor = A * (1-b) + B * (1-a)

Punches a hole at the intersection of both inputs alpha channels.
- Can be used for motion  graphics.
- Can be used for edge detections if input A = input B, and you can then edit inner and outer edge separatly on each branch.

### Disjoint-Over

> Disjoint-over = A + B * (1-a) / b

Prevent alpha problems (dark edge) where A and B are overlapping by normalizing B's contribution base on how opaque it is. B's color contribute only fully where B is opaque.
> But, if a + b < 1, it performs a simple plus operation : A + B

### Conjoint-Over

> Conjoint-over = A + B * (1-a) / b

Same as disjoint-over, but this time the condition is different :
> if a > b performs  = A

Good with semi-transparent elements to avoid alpha getting too dense.

### Stencil

> Stencil = B * (1-a)

Simply punches a hole into B with A alpha channel.
Inverse operation of mask.

### Out

> Out = A * (1-b)

Like a stencil but with inputs inverted. Useful to keep B pipe in order.

### Screen

> Screen = A + B - A * B

> if A and B between 0-1
> else A if A > B,
> else B

Ignore alpha channel.
Intended to work with values between 0-1 and keep them under 1.

- to add windows or screen reflection.

### Hypot


> Hypot = $\sqrt{A*A+B*B}$

Kind of like a screen but darkers values contribute less than bright values.

### Exclusion

> Exclusion = A + B - 2 * A * B

Like a difference but more pleasing to the eye.
White inverts but black doesnt. 


### Average

> Average = ( A + B ) / 2

Can be done with more inputs, and will divide the sum by the number of inputs.
- Can be used to average grain
- Can be used to detect details with a difference  = ==High pass filter==

-![Pasted image 20251121093643.png](/img/user/0_Assets/attachments/Pasted%20image%2020251121093643.png)

### Geometric

> Geometric = (2 * A * B) / (A+B)

Favors dark values

### Overlay

> Overlay = multiply if  B < 0.5 // screen if B > 0.5

- Can add some constrast if used with the high pass filter made with the average method.

### Min / Max

> Min = min (A, B)
> Max = max (A, B)

- Can help with dark/bright edges. To mix extended edges back only in concerned pixels (and with a mask to target correct area of course)
- Can be used (min) to add shadows (be sure to multiply with plate details extracted with a divide)
- Can be used to merge mattes
- Can be used (max) to merge elements shot on black background.
- Can be used with temporal inputs to remove things like rain for example.

### Hard-Light

>  Hard-light = multiply if A < 0.5 // screen if A > 0.5

Same as overlay but with input A
- Can be used to preserve B pipe

### Soft-light

> Soft-light = B * (2 * A + (B * (1 - A * B )))

A FAIRE

### Color-dodge

> color-dodge = B / (1 - A)

Brightens B towards A. Name herited from the film photographic process.

### Color-brun

> Color-burn = 1 - 
> (1 - B ) / A

Darkens B towards A. Also herited from photographic process.





## Difference Keying

Isolate fine dark/bright details to add them to the BG.


# Normalization

Normalization is a process to change the range of values by shifting and rescaling them so that they end up ranging between 0 and 1.

Formula to normalize values :

(x-min)/(max-min)

![Pasted image 20260105094123.png](/img/user/0_Assets/attachments/Pasted%20image%2020260105094123.png)

If the minimum value is already 0, the formule becomes :

x/max

Can be used to :
- Create STMAP
- Normalize depth values to use it as mask
- Create relations between two sets of values (Blur + grain for exemple)
- Create fake reflections by normalizing normal pass to STMAP
- Convert an input range to a new output range via normalization
![Pasted image 20260105095726.png](/img/user/0_Assets/attachments/Pasted%20image%2020260105095726.png)

# LERP

![Pasted image 20260107094318.png](/img/user/0_Assets/attachments/Pasted%20image%2020260107094318.png)

- Useful to blend values (like transforms, cameras, or multiple stacks of effects)
- Create controller for particules templates (independant from keyframes actual location)

- Can be used to normalize values

# Inverse

1 - x
Pivots around 0.5
Intended to work with normalized values between 0 and 1.

- Invert masks or depth pass
- Invert normal pass channel (for rim lighting for exemple)
- Used in particles expressions
- To invert boolean operations
- Create fake reflections by mirroring camera translate/rotate (also add 180° to invert rotation)

### Additive Inversion

x * - 1
Pivots around 0

- Can invert curves without offsetting
- Can be used to find negative values

### Multiplicative Inversion

![Pasted image 20260107101858.png](/img/user/0_Assets/attachments/Pasted%20image%2020260107101858.png)


# Percentage / Ratio

![Pasted image 20260107103453.png](/img/user/0_Assets/attachments/Pasted%20image%2020260107103453.png)

- To modify values with percentages according to sup demand (be careful with values that originate from "1", like Gain for exemple)


![Pasted image 20260107104624.png](/img/user/0_Assets/attachments/Pasted%20image%2020260107104624.png)

- To calculate how more pixels we need if render is not covering the work resolution.
- Can be used to automatically correct motion passes if resolution is changing, and thus keeking motion vector correct.


![Pasted image 20260107110417.png](/img/user/0_Assets/attachments/Pasted%20image%2020260107110417.png)


## Rounding

![Pasted image 20260115143353.png](/img/user/0_Assets/attachments/Pasted%20image%2020260115143353.png)
![Pasted image 20260115143445.png](/img/user/0_Assets/attachments/Pasted%20image%2020260115143445.png)
![Pasted image 20260115143559.png](/img/user/0_Assets/attachments/Pasted%20image%2020260115143559.png)
![Pasted image 20260115143653.png](/img/user/0_Assets/attachments/Pasted%20image%2020260115143653.png)

Can be usefull to get rid of retime artifact, with two linked FrameHold, one in floor and one in ceil :
![Pasted image 20260115144804.png](/img/user/0_Assets/attachments/Pasted%20image%2020260115144804.png)




## Modulo

![Pasted image 20260115145427.png](/img/user/0_Assets/attachments/Pasted%20image%2020260115145427.png)
![Pasted image 20260115145600.png](/img/user/0_Assets/attachments/Pasted%20image%2020260115145600.png)


## Average


![Pasted image 20260116162022.png](/img/user/0_Assets/attachments/Pasted%20image%2020260116162022.png)

![Pasted image 20260116162107.png](/img/user/0_Assets/attachments/Pasted%20image%2020260116162107.png)

Can be used to calculate pivot point of a render  with bbox average.
Can make a nan killer.
Can be used to make despill.

![Pasted image 20260116163953.png](/img/user/0_Assets/attachments/Pasted%20image%2020260116163953.png)

![Pasted image 20260116164140.png](/img/user/0_Assets/attachments/Pasted%20image%2020260116164140.png)



![Pasted image 20260116164240.png](/img/user/0_Assets/attachments/Pasted%20image%2020260116164240.png)

![Pasted image 20260116164345.png](/img/user/0_Assets/attachments/Pasted%20image%2020260116164345.png)

![Pasted image 20260116164908.png](/img/user/0_Assets/attachments/Pasted%20image%2020260116164908.png)

![Pasted image 20260116164933.png](/img/user/0_Assets/attachments/Pasted%20image%2020260116164933.png)


> [!NOTE] ATTENTION
> To use 'mode' in python, you need to import it from statistics module
> ![Pasted image 20260116165135.png](/img/user/0_Assets/attachments/Pasted%20image%2020260116165135.png)


![Pasted image 20260116165425.png](/img/user/0_Assets/attachments/Pasted%20image%2020260116165425.png)

![Pasted image 20260116170007.png](/img/user/0_Assets/attachments/Pasted%20image%2020260116170007.png)

## Grade Node

![Pasted image 20260116172450.png](/img/user/0_Assets/attachments/Pasted%20image%2020260116172450.png)

