---
{"dg-publish":true,"permalink":"/nuke-kb/40-courses/keheka-courses/automating-tedious-tasks-with-python/"}
---

A good way to start is to dive into how we can use Python to both  
automate and speed up the daily tasks that we don't really want to do,  
so that we can focus more on the fun and creative aspects of  
compositing!

> [!important]
> 
> I’ve found that when tackling a large and complex subject such as Python,  
> it's more enjoyable when you’re actually building something useful.  
> Instead of studying all of the nuts and bolts, and then only using  
> 10-20% of that knowledge to actually make something, let’s focus on the  
> end goal which is building several different practical tools (see list  
> below). We’ll break that goal down into smaller objectives, and then  
> learn the relevant things we need to know in order to accomplish those  
> objectives along the way. And finally, we'll flesh out that knowledge  
> with different scenarios and approaches, reinforcing the principles for  
> making your own Python tools.

# Automate The Boring Tasks

At some point while compositing, you’ll no doubt have encountered  
tedious and mundane tasks which purely require repetitive, manual  
input.

Tasks which are not at all creative in themselves – only frustrating and time consuming.

For example:

- Changing the same setting over and over in many nodes.

- Creating a large number of the same nodes with different values applied to each of them.

- Shuffling out and labelling all of the render passes in your CG render and  
    removing the superfluous layers/channels from each stream.

It can be very annoying to have to manually create hundreds of nodes, open up their properties, and individually change the parameters over and over.

But the good news is that this is **completely avoidable**.

In this large guide, we’ll walk through how to write three very useful and adaptable Python scripts – easy, medium, and a little bit more complex – plus variations of them. These scripts will take advantage of the **for-loop** in order to automate the repetitive tasks described above.

> [!important]
> 
> You don't need any prior Python knowledge to follow along with this guide.

# What’s A For-loop Anyway?

A for-loop is a way to iterate over a given sequence.

What that means, is that you can use a for-loop to go through a collection of things **sequentially** and perform a specific task (or tasks) for each thing in the collection.

There are different types of sequences which you can iterate through using a for-loop, each with their own special qualities:

- [List](https://www.w3schools.com/python/python_lists.asp?ref=keheka.com)

- [Tuple](https://www.w3schools.com/python/python_tuples.asp?ref=keheka.com)

- [Dictionary](https://www.w3schools.com/python/python_dictionaries.asp?ref=keheka.com)

- [Set](https://www.w3schools.com/python/python_sets.asp?ref=keheka.com)

- [String](https://www.w3schools.com/python/python_strings.asp?ref=keheka.com)

This is great because it means that you can iterate through a lot of useful things, like for example the nodes which you currently have selected in Nuke, or the characters in the filename of a Read node.

### For-Loop Flowchart

Here’s a basic overview of how the for-loop works:

[![](https://www.keheka.com/content/images/2024/02/Flowchart_For-Loop.jpg)](https://www.keheka.com/content/images/2024/02/Flowchart_For-Loop.jpg)

The flowchart of a for-loop.

The loop starts at the beginning of the sequence, for example with the first node in a list of the currently selected nodes in Nuke.

Then, it will perform the task(s) that you have specified in the body of the for-loop. For example, setting the mix value of that node to 0.5, or creating a different node connected to it.

After that, it loops back and does the same task again for the next item in the sequence. E.g. the next node in the selection of nodes.

It continues doing this until there are no more items remaining in the sequence, and then it stops.

### The Structure Of A For-Loop

Let’s look at a generalised for-loop so that we can unpick its structure:

for **iteration_variable** in **sequence**:

**statement(s)**

The **iteration_variable** is a variable that is used to keep track of the progress of a loop. It can be named anything, and you’ll often see it called either the generic ‘i’ or something more descriptive like ‘layer’ or ‘frame’, depending on what’s being iterated through.

> [!important]
> 
> A variable is a container for storing data values.

Each time the body of a loop is executed is called an iteration, and the iteration variable takes on a new value with each iteration of the loop.

As mentioned before, the **sequence** is a collection of items. If the sequence would be a list of the currently selected nodes in Nuke, for example, then at first the iteration variable would get the first node in the list. Then, after the first loop, it would get the second node in the list. After the second  
loop it would get the third node in the list. And so on.

Notice the words **for** and **in**, and the **colon (:)** at the end of the first line. They specify that it’s a for-loop we’re making, and their order is important.

The body of the loop consists of one or more **statements** – or tasks, if you will. Each time the loop runs, those tasks are performed for the current item in the sequence.

Notice that the body of the for-loop is **indented**. I have used **four spaces** here, but you can use fewer (minimum of one space) or more. Pressing the Tab key is also allowed for making indentations. Whichever you choose, you have to **stick to it** throughout the entire Python script, or else you’ll get errors.

The indentation is important in order to indicate that this is the body of the for-loop in the Python script. Anything indented by the same amount belongs to the same block of code: a grouping of statements for a specific purpose.

You can visualise it as a hierarchy: the first line in the generalised for-loop above is like a supervisor who’s overseeing and guiding the work, and the second line is like an artist who’s doing the actual tasks.

Okay, let’s build some useful Python scripts with the use of the for-loop.

# Changing The Same Setting In Many Nodes

When delivering shots to the client, as a Supervisor I have to check both the EXRs and the QuickTimes beforehand, and make sure that everything is in order before we send the files.

When importing the delivery QTs into Nuke for review, the Read nodes all have to be set to Raw Data for them to display correctly in our pipeline.

Opening up the properties of dozens and dozens of Read nodes and manually ticking the Raw Data checkbox for each of them is very tedious and time consuming. – And completely unnecessary.

Instead, let's make Python do all the work for us in a blink of an eye, with a simple for-loop.

### The Python Script

Let’s break down what we want to do:

For each of the currently selected Read nodes in Nuke, we want to tick the Raw Data checkbox in the Read node’s properties.

Below is a Python script which accomplishes just that:

```Plain
import nuke

for reads in nuke.selectedNodes('Read'):
    reads['raw'].setValue(True)
```

Let’s go through each part of the code step by step.

**import nuke**

This first line simply gives us access to the Nuke Python module, so that we can use Nuke-specific code in our Python script.

This lets Python understand how to interact with Nuke.

Next, I left a blank line for some breathing room and increased readability.

The third and fourth lines contain the for-loop and its body.

Let’s look at the third line first:

**for reads in nuke.selectedNodes('Read'):**

This line sets up the for-loop structure with the **iteration variable** and the **sequence**. Let’s break it down:

**for in :**

Like in the generalised for-loop we saw earlier, we have the words **for** and **in**, and the **colon (:)** at the end.

**reads**

This is what I named the **iteration variable**.

> [!important]
> 
> We will be iterating through Read nodes, and so ‘**reads**_’_ seemed both appropriate and descriptive to use as the name for the iteration variable.

As mentioned before, this name could be anything – ‘**i**’, for example – and the code would still work.

**nuke.selectedNodes('Read')**

This is the **sequence** we want to iterate through.

We’ll be iterating through all of the Read nodes in our current node selection.

Let’s break down the code for this sequence into each individual part:

To get to the currently selected nodes in the Node Graph in Nuke, we have to use Nuke-specific Python code. So first, we access the Nuke module (that we imported earlier) with:

**nuke**

Then, we choose the relevant Nuke Python code for accessing the currently selected nodes:

**.selectedNodes()**

> [!important]
> 
> The dot (**.**) signifies that we are accessing something within what comes before it: we’re accessing the Nuke specific **selectedNodes()** code inside the **nuke** module.

And finally, we specify that we only want the Read class of nodes in that selection, with:

**‘Read’**

‘Read’ goes inside of the parentheses of selectedNodes(). That's because selectedNodes() is a function.

Information can be passed into a function as so-called arguments. So to tell the function to only get the Read nodes in the selection, we put ‘Read’ as an argument in the function.

> [!important]
> 
> Using the correct capitalisation and punctuation are very important for the code to work. A Python script is just a set of very specific instructions for the machine to follow. If we get it wrong, it won't know what to do and will throw errors.

Next, let’s look at the final line of our code, which is the body of the for-loop.

In this case, it only contains one line of code:

**reads['raw'].setValue(True)**

This is the **statement** that we want to execute for each item in the sequence.

We want to enable the Raw Data checkbox in the Read node.

First, we make an **indentation** (four spaces) to indicate that this is the body of the for-loop.

Next, we tell the loop to access the current Read node in the sequence, with our iteration variable:

**reads**

Then, we tell it to access the **knob** called _raw_ in the that node, with:

**[‘raw’]**

Notice that ‘Raw Data’ is just the **label** of the knob, i.e. what we can read in the properties. The actual **name** of the knob is _raw_.

> [!important]
> 
> To find out what a particular knob’s name is, you can hover your mouse over the knob and the tooltip will show its name in **bold font** at the top. Often, the knob’s name will be the same (or very similar) to the knob’s label, but not always, so it's best to check each time.

[![](https://www.keheka.com/content/images/2024/02/Raw_Data.png)](https://www.keheka.com/content/images/2024/02/Raw_Data.png)

Hovering the mouse over the Raw Data checkbox, we get to see the true name of the knob: **raw**.

> [!important]
> 
> If you wanted to access another knob in the Read node, for example the first Frame Range number field (knob name: **first**), you would just swap that in like this: **[‘first’]**

Finally, we tell the loop to enable the _raw_ checkbox by setting it to a value of True, with:

**.setValue(True)**

> [!important]
> 
> Checkboxes will either have a value of True (enabled) or False (disabled).

### Reusing The Same Structure

The same code structure that we just built works for other nodes and settings, too.

If you instead wanted to change for example the _size_ of all the selected Blur nodes to 20, you would just swap out the relevant bits:

import nuke

for **blurs** in nuke.selectedNodes('**Blur**'):

**blurs**['**size**'].setValue(**20**)

Or, if you wanted to change the _channels_ of all the selected Grade nodes to alpha, you would write:

import nuke

for **grades** in nuke.selectedNodes('**Grade**'):

**grades**['**channels**’].setValue(**‘alpha’**)

> [!important]
> 
> Note that ‘alpha’ has quotation marks around it while 20 did not. Words/characters (with the exception of the boolean values True and False, or the special value None) need quotation marks around them to indicate that they are strings as opposed to numerical values or other types of sequences, such as lists or tuples. Otherwise, you’ll get errors when running the code.

These were just a couple of examples, but you can swap in any nodes you want. To find the  
class name for a specific node, select the node in the Node Graph and hit “i” on the keyboard.

[![](https://www.keheka.com/content/images/2024/02/Node_Class_Info.png)](https://www.keheka.com/content/images/2024/02/Node_Class_Info.png)

Finding the class name of a node.

> [!important]
> 
> Sometimes there are multiple versions of a node, so pay attention to any **number** at the end of a class name – it has to be included to get the right version of the node. For example, Tracker4 or Shuffle2.

If you just wanted to get _all_ the selected nodes in the Nuke script (i.e. not specifying any particular class of node), you would just leave the parentheses empty, like this:

import nuke

for nodes in nuke.selectedNodes**()**:

nodes['channels’].setValue(‘alpha’)

However, not all nodes have the _channels_ drop-down menu, the _size_ knob, or other settings that you specify.

In those cases, when the for-loop reaches a node in your selection which doesn't have that specific setting available, it will throw an error.

You can avoid that by automatically skipping over these nodes with some additional code:

import nuke

for nodes in nuke.selectedNodes():

**try:**

nodes['channels’].setValue(‘alpha’)

**except:**

**pass**

The **try** block lets you test a block of code for errors. Note that the original code is **indented** below the **try:** part – that's because it's now an ‘artist’ under the ‘supervision’ of the try block, and not directly under the for-loop block.

The **except** block lets you handle the error. When the try block errors out (in this case, when a node in the selection doesn't have the _channels_ drop-down menu), the code in the **except** block will run.

When the **pass** statement is executed, nothing happens, but you avoid getting an error. It will simply skip over the error and continue with the loop.

Lastly, if you just wanted to get _all_ the nodes in the whole Nuke script, you would change the code like this:

import nuke

for nodes in nuke.**all**Nodes():

try:

nodes['channels’].setValue(‘alpha’)

except:

pass

Note that you can also specify node classes here, to get all the nodes of a specific class in yourNuke script:

import nuke

for nodes in nuke.allNodes(**‘Grade’**):

nodes['channels’].setValue(‘alpha’)

That would get all the Grade nodes in the entire Nuke script and change their channels setting to alpha.

– Okay, that was hopefully a useful detour there. Let's get back on track.

How do we actually use the script we made before that sets the selected Read nodes to Raw Data?

### Using The Script In Practice

You _could_ copy and paste the Python code into the Script Editor (Window → Script Editor) and [run it from there](https://learn.foundry.com/nuke/content/comp_environment/script_editor/using_script_editor.html?ref=keheka.com).

Or, you _could_ save it as a Python script and [load it up through the menu.py](https://youtu.be/yLUdKEhz4fY?feature=shared&ref=keheka.com) with [its own hotkey](https://benmcewan.com/blog/2018/01/14/whats-a-menu-py-and-why-should-i-have-one/?ref=keheka.com).

However, I prefer to make scripts like these more mobile – easy to save and bring along across computers, users, projects, or companies. A great way of doing that is to put the script into a node that you can easily copy around and save as a ToolSet:

First, create a NoOp node and name it ReadToRaw (or any other name you prefer).

In an empty part of its properties, right-click and select Manage User Knobs…

Then, choose Add → Python Script Button.

Name the button ‘readToRaw’ and label it ‘Set selected read nodes to raw’.

Then, in the Script section, paste the Python code we wrote earlier and hit OK.

[![](https://www.keheka.com/content/images/2024/02/Python_Script_Button.png)](https://www.keheka.com/content/images/2024/02/Python_Script_Button.png)

Creating a Python Script Button.

Now, you can open up the NoOp (ReadToRaw) node's properties in a floating window by Ctrl + double-clicking the node, and then select all the Read nodes you want to set to Raw Data beforehitting the Python Script Button.

In a flash, all of your selected Read nodes will be set to Raw Data, and you've saved yourself a bunch of tedious clicking. 😊

  

> [!info] Running The Read To Raw Python Script  
>  
> [https://youtu.be/7ZioOLXArJI](https://youtu.be/7ZioOLXArJI)  

Running the Python script.

Here is the entire tool which you can copy and paste into your Node Graph:

```Plain
set cut_paste_input [stack 0]
version 14.0 v2
push $cut_paste_input
NoOp {
 name ReadToRaw
 selected true
 xpos -76
 ypos -135
 hide_input true
 addUserKnob {20 User l "Read to raw"}
 addUserKnob {22 readRaw l "Set selected read nodes to raw" T "import nuke\n\nfor reads in nuke.selectedNodes('Read'):\n    reads\['raw'].setValue(True)" +STARTLINE}
}
```

Let's move on to the next example.

# Creating Many Identical Nodes With Different Settings

A useful method for removing rain or snow from a shot that has a  
static camera, is to create a bunch of FrameHolds (one for every frame  
of the shot), and then Merge(min) them all together.

Rain and snow are typically slightly brighter than the background, so this will effectively pick the parts of the frames where there is no rain or snow (i.e. lower brightness) and combine them into a clean image.

However, making something like a hundred FrameHold nodes and changing the held  
frame in each of them is tedious, so let’s again have Python make quick work of this task.

### User Controls

First, make a NoOp node again to hold the Python script, and let’s name it FrameHoldMultiple.

In this node, right-click on the properties and select Manage User Knobs…

Then, click Add → Python Script Button.

Name it ‘generateFrameHolds’ and label it ‘Generate FrameHolds’.

Leave the Script section blank, we’ll paste our Python code in here later.

This time around, it's also useful to add some extra user controls in the NoOp node, in order to dynamically change the code.

Specifically, it would be nice to be able to manually choose the frame range which is used to create the FrameHolds.

So, right-click on the properties of the NoOp node and select Manage User Knobs…

Then, click Add → Integer Knob.

> [!important]
> 
> We’re using an Integer Knob (instead of a Floating Point Slider, for example) because we can only FrameHold whole number frames.

Name the Integer Knob ‘firstFr’ and label it ‘first frame’.

Next, make another Integer Knob, name it ‘lastFr’, and label it ‘last frame’.

To make our tool even more useful, let’s add another knob for controlling the frame incrementation, so that we can frame hold for example every tenth frame in a given frame range.

Make yet another Integer Knob, name it ‘frameInc’, and label it ‘frame increment’.

Now we have three number fields in our node where we can input our desired frame range and frame incrementation.

[![](https://www.keheka.com/content/images/2024/02/FrameHoldMultiple.png)](https://www.keheka.com/content/images/2024/02/FrameHoldMultiple.png)

The user controls.

And we can use them in our Python code.

### The Python Script

Let’s break down what we want to do:

For every (nth) frame in our chosen frame range, we want to create a FrameHold node, attach it to our selected node, and then set the FrameHold node’s frame value to that frame number.

Below is a Python script which accomplishes just that:

```Plain
import nuke

thisNode = nuke.thisNode()
selNode = nuke.selectedNode()
firstFr = int(thisNode.knob('firstFr').value())
lastFr = int(thisNode.knob('lastFr').value())
frameInc = int(thisNode.knob('frameInc').value())

for frame in range(firstFr, (lastFr+1), frameInc):
    frameHold = nuke.createNode('FrameHold')
    frameHold.setInput(0, selNode)
    frameHold['firstFrame'].setValue(frame)
```

This script is a little bit bigger than the last one, but it can all easily be broken down into its individual parts.

Let’s start at the top:

**import nuke**

Same as before, this line imports the Nuke module so that Python can interact with Nuke.

Next, since we want to use the values of the User Controls that we specified in our NoOp (FrameHoldMultiple) node, we need to be able to access this node.

We do that in this line:

**thisNode = nuke.thisNode()**

This line creates a **variable** (a container for storing data values) that is used to access the node in which our Python script resides. Let’s break it down:

Since we’ll be accessing the node multiple times – and to make our Python script easier to read – we first set up a variable which we can assign the necessary code to:

**thisNode**

Then, we write the Nuke-specific code for accessing the node in which our Python script resides:

**nuke.thisNode()**

To assign the code to the variable, we just put an equal sign between them:

**thisNode = nuke.thisNode()**

So now, anytime we type **thisNode**, Python understands that we really mean **nuke.thisNode()**.

Next, since we want to attach our FrameHolds to the currently selected node, we also need to access that node.

**selNode = nuke.selectedNode()**

This line creates a variable that is used to access the currently selected node in our Node Graph. Let’s break it down.

Again, we’ll create a variable which we can assign the necessary code to, and refer to again later:

**selNode**

To get just a single selected node – as opposed to the previous selected group of node**s** – we drop the ‘**s**’ from the nuke.selectedNode**s**() code that we wrote earlier:

**nuke.selectedNode()**

> [!important]
> 
> If you have multiple nodes selected, this code will only get the bottom-most node in that selection (the output node).

Just like how we had to define those two nodes to use them in our code, we also have to define the values of our User Controls. Let’s start with the firstFr knob:

**firstFr = int(thisNode.knob('firstFr').value())**

This line creates a variable that is used to retrieve the value of our custom firstFr knob. Let’s break it down.

Again, we’ll make a variable to assign the necessary code to,

**firstFr**

Next, we need to tell Python to make sure that we get an integer value (whole number), with:

**int()**

> [!important]
> 
> The **int()** function converts the specified value into an integer.

That’s because we’ll be using a function later which only accepts integer values, and it will error out if it receives for example floating point values (decimal numbers).

So any value we put inside the parentheses will be turned into an integer. The first part of what we’ve put inside is:

**thisNode**

Like we defined earlier, that’s referring to this NoOp (FrameHoldMultiple) node. – We’re accessing that node.

The next part,

**.knob('firstFr')**

means that we want to access the knob called firstFr in that node. (The knob we previously made to hold the first frame of our chosen frame range).

And the last part:

**.value()**

means that we want to access the value of that knob.

So, the whole line: **int(thisNode.knob('firstFr').value())** means: “get the value of the knob called firstFr in this node, and make sure that the value is (converted to) a whole number”.

Let’s move to the next line of the script:

**lastFr = int(thisNode.knob('lastFr').value())**

This is exactly the same as the above, just with the lastFr knob instead of the firstFr knob.

And same with the next line:

**frameInc = int(thisNode.knob('frameInc').value())**

This is exactly the same as the above, just with the frameInc knob instead.

Now, we have defined everything we need to build the for-loop.

**for frame in range(firstFr, (lastFr+1), frameInc):**

This line sets up the loop structure with the **iteration variable** and the **sequence**. Let’s break it down:

After setting up the usual **for  in  :**  part, we add our **iteration variable**:

**frame**

Next, let's add the **sequence**.

To define a frame range, we can use the **range()** function.

This function returns a sequence of numbers which starts from 0 by default, increments by 1 by default, and stops **before** a specified number. It's used like this:

**range(start, stop, step)**

**start**

Optional. An integer number specifying at which position to start. The default is 0.

**stop**

Required. An integer number specifying at which position to stop (**not included**).

**step**

Optional. An integer number specifying the incrementation. The default is 1.

We have already defined both the start frame and the end frame – **firstFr** and **lastFr**, respectively. And we have also defined the frame incrementation: **frameInc**.

So let's put these into the range() function.

The only thing we have to be wary of is the last frame. We want to **include** the last frame, but the range() function stops when it reaches the stop value.

So, we can simply add 1 to correct for that – i.e. the function will stop on the frame after the last frame, instead:

**(lastFr+1)**

The **sequence** of the for-loop then becomes:

**range(firstFr, (lastFr+1), frameInc)**

Let's move on to the body of the for-loop.

Remember, for every (nth) frame in our chosen frame range, we want to create a FrameHold node, attach it to our selected node, and then set the FrameHold node’s frame value to that frame number.

**frameHold = nuke.createNode('FrameHold')**

This line creates a **variable** named frameHold which is used to create the FrameHold node. Let’s break it down.

First, we add the **indentation** (four spaces) to indicate that this is a new code block: the body of the for-loop.

Then, we define a variable to hold the snippet of code for creating the FrameHold node, so that we can easily reuse it later:

**frameHold**

And then, we add the Nuke-specific code for creating the FrameHold node:

**nuke.createNode('FrameHold')**

Next, we need to attach the FrameHold node to our selected node.

**frameHold.setInput(0, selNode)**

This line connects the input of the current FrameHold node to the selected node. Let’s break it down.

The first part,

**frameHold**

refers to the variable that we just created earlier – which creates the FrameHold node.

The last part,

**.setInput(0, selNode)**

controls the connection of the node’s inputs.

Inside the parentheses, the two values **0** and **selNode** refer to which input of the node to use for connecting, and which node to connect that input to, respectively.

So, **0**  
refers to the FrameHold node’s main input pipe. (In this case,  
FrameHold only has one input). You can read more about inputs and how  
they work in this article: [The Hidden Number In Nuke](https://www.keheka.com/the-hidden-number-in-nuke/)

And, **selNode** is the variable we made earlier, which refers to the selected node.

**frameHold.setInput(0, selNode)** therefore means: connect the main input of the FrameHold node to the selected node.

Finally, let’s set the frame numbers we want to hold on.

**frameHold['firstFrame'].setValue(frame)**

This line accesses the current FrameHold node’s firstFrame knob, and sets its value to the current **iteration variable**. Let’s break it down:

The first part,

**frameHold**

again refers to the variable that we made earlier for our FrameHold node.

The next part,

**['firstFrame']**

means: access the knob called firstFrame of this node.

And the last part,

**.setValue(frame)**

means, set the value of that knob to the value of the current iteration  
variable, i.e. the current frame the loop is iterating on.

Now, we have a Python script that will do everything we want it to do, provided the user doesn’t make any errors.

However, if someone would set the frame incrementation to 0 in the User Controls, the code would throw an error. The frame incrementation has to be 1 or higher for the range() function to work properly.

And, what if someone would set the last frame to be a number lower than the first frame? Again, the range() function wouldn’t know what to do, because it expects the range to go from a lower number to a higher (or same) number.

Let’s make some additions to our code to handle these potential problems, and guide the user to making the right choices.

### Error Handling

The new, upgraded code looks like this:

```Plain
import nuke

thisNode = nuke.thisNode()
selNode = nuke.selectedNode()
firstFr = int(thisNode.knob('firstFr').value())
lastFr = int(thisNode.knob('lastFr').value())
frameInc = int(thisNode.knob('frameInc').value())

if frameInc < 1:
    nuke.message("The frame increment must be 1 or higher.")
elif firstFr > lastFr:
    nuke.message("The last frame must be greater than or equal to the first frame.")
else:
    for frame in range(firstFr, (lastFr+1), frameInc):
        frameHold = nuke.createNode('FrameHold')
        frameHold.setInput(0, selNode)
        frameHold['firstFrame'].setValue(frame)
```

Let’s break it down:

First, we import the Nuke module and define our variables, just like before.

But then, before we move on to the for-loop, we do some tests to check that everything is in order.

To do that, we use the if-else statement, here generalised:

if **condition**:

**statement(s)**

else:

**statement(s)**

This means: check **if** a certain **condition** is true. For example, check if the current frame number is greater than 1025.

If it is true, execute the code block of **statements** below the if-line.

If it is not true (**else**), execute the code block of **statements** below the else-line.

If you want to check for multiple different things, you can add the else-if statement, which in Python is called **elif**.

if **condition**:

**statement(s)**

elif **condition**:

**statement(s)**

else:

**statement(s)**

After the first **if condition** has been checked AND it turns out to be **false**, then the **elif condition** will be checked. If it is **true**, the code block of statements below the elif-line will be executed. If it's false, the **else** block kicks in.

> [!important]
> 
> You can add as many **elif**s as you like.

In our upgraded Python code, we’ve added the following if-elif-else statement:

**if frameInc < 1:**

This line checks if the frame incrementation is less than 1.

If it is, then execute the following statement:

**nuke.message("The frame increment must be 1 or higher.")**

This code will pop up a message box in Nuke to warn the user that the frame increment must be 1 or higher. Remember the **indentation**.

If the frame increment is 1 or higher, then the script will move on to the next check:

**elif firstFr > lastFr:**

This line checks if the firstFr knob value is greater than the lastFr knob value.

If it is, then execute the following statement:

**nuke.message("The last frame must be greater than or equal to the first frame.")**

This code will pop up a message box in Nuke to warn the user that the last frame must be greater than or equal to the first frame. Remember the **indentation**.

Finally, if both checks turn out to be false, then the **else** statement kicks in - which is our for-loop:

**else:**

**for frame in range(firstFr, (lastFr+1), frameInc):**

**frameHold = nuke.createNode('FrameHold')**

**frameHold.setInput(0, selNode)**

**frameHold['firstFrame'].setValue(frame)**

Again, remember the **indentation**. All of the for-loop has to be indented below the else-line. (What were previously no spaces become 4 spaces, and what were previously 4 spaces  
now become 8 spaces).

Now, our tool should work the way we expect it to, and handle user errors – happy days!

Here is the entire tool which you can copy and paste into your Node Graph:

```Plain
set cut_paste_input [stack 0]
version 14.0 v2
push $cut_paste_input
NoOp {
 name FrameHoldMultiple
 selected true
 xpos 40
 ypos -156
 hide_input true
 addUserKnob {20 User l "Generate FrameHolds"}
 addUserKnob {3 firstFr l "first frame"}
 firstFr 1
 addUserKnob {3 lastFr l "last frame"}
 lastFr 5
 addUserKnob {3 frameInc l "frame increment"}
 frameInc 1
 addUserKnob {26 ""}
 addUserKnob {22 generateFrameHolds l "Generate FrameHolds" T "import nuke\n\nthisNode = nuke.thisNode()\nselNode = nuke.selectedNode()\nfirstFr = int(thisNode.knob('firstFr').value())\nlastFr = int(thisNode.knob('lastFr').value())\nframeInc = int(thisNode.knob('frameInc').value())\n\nif frameInc < 1:\n    nuke.message(\"The frame increment must be 1 or higher.\")\nelif firstFr > lastFr:\n    nuke.message(\"The last frame must be greater than or equal to the first frame.\")\nelse:\n    for frame in range(firstFr, (lastFr+1), frameInc): \n        frameHold = nuke.createNode('FrameHold')\n        frameHold.setInput(0, selNode)\n        frameHold\['firstFrame'].setValue(frame)" +STARTLINE}
}
```

### A Useful Variation

Another scenario where it’s great to automate the creation of many  
nodes with different values applied to them, is when you want to make  
symmetrical patterns.

You can for example make 60 Transform nodes with a six degree rotation increment in order to rotate an element around into a full circle. For instance, to build a watch face or a HUD element.

Let’s adapt the previous Python script to make a tool which can do this.

First, let’s break down what we want the tool to do.

We want to create a specified number of Transform nodes, each one with a specified rotational offset from the previous one, all attached to the selected node.

To start off, let’s create a NoOp node again to hold our Python script and User Controls. Name it TransformMultiple.

Then, make an Integer Knob, name it ‘numberOfNodes’ and label it ‘number of nodes’.

Next, make a Floating Point Slider, name it ‘rotationIncrement’ and label it ‘rotation increment’. Set the Minimum value to 0 and the Maximum value to 360. That way, the default slider can go up to one full rotation.

> [!important]
> 
> A Floating Point Slider (as opposed to an Integer Knob) allows for decimal values, which we’ll need for the rotational offset values.

Finally, make a Python Script Button, name it ‘generateTransforms’ and label it ‘Generate Transforms’.

[![](https://www.keheka.com/content/images/2024/02/TransformMultiple.png)](https://www.keheka.com/content/images/2024/02/TransformMultiple.png)

The user controls.

Below is the Python script which we will be pasting into the Script section of the Python Script Button:

```Plain
import nuke

thisNode = nuke.thisNode()
selNode = nuke.selectedNode()
numberOfNodes = int(thisNode.knob('numberOfNodes').value())
rotationIncrement = float(thisNode.knob('rotationIncrement').value())

if numberOfNodes < 1:
    nuke.message("The number of nodes must be 1 or higher.")
else:
    for i in range(1, (numberOfNodes+1)):
        transform = nuke.createNode('Transform')
        transform.setInput(0, selNode)
        transform['rotate'].setValue(i * rotationIncrement)
```

Let’s break it down.

Exactly like before, we first import the Nuke module and make variables for retrieving this node and the selected node:

**import nuke**

**thisNode = nuke.thisNode()**

**selNode = nuke.selectedNode()**

Then, we make two more variables. One for retrieving the value of the numberOfNodes knob, and one for retrieving the value of the rotationIncrement knob in our NoOp (TransformMultiple) node.

**numberOfNodes = int(thisNode.knob('numberOfNodes').value())**

This is exactly like how we did it before, we have just swapped out the name of the variable and the knob.

**rotationIncrement = float(thisNode.knob('rotationIncrement').value())**

This is also like how we did it before, except instead of setting the value to be an integer with the **int()** function, we set it to be a floating point value with the **float()** function.

> [!important]
> 
> As opposed to the number of nodes, which should be a whole number (integer), the rotational offset can be a number with a decimal value. So for that, we have to specify it to be a floating point value instead of an integer value, using the **float()** function.

Next, let’s do a quick check to cover the error handling:

**if numberOfNodes < 1:**

This line checks if the value in the numberOfNodes knob is less than 1. (Obviously, we want to make at least one node).

If it is, Nuke will pop up a message saying that the number of nodes must be 1 or higher, with this bit of code:

**nuke.message("The number of nodes must be 1 or higher.")**

Next, if the number of nodes is 1 or higher, we run a for-loop with the else statement:

**else:**

The for-loop looks like this:

**for i in range(1, (numberOfNodes+1)):**

Here, I just named the **iteration variable** the generic ‘**i**’ and set the **sequence** to be a range from 1 to the number of nodes we specified in our NoOp (TransformMultiple) node plus one.

> [!important]
> 
> Remember that the stop value in the range() function is not inclusive, so we need to add 1 to include the last value.

Remember the **indentation**. This for-loop is the body of the **else** statement.

Next, let’s look at the body of the for-loop.

The first line creates a **variable** called transform which is used to create a Transform node:

**transform = nuke.createNode('Transform')**

The second line attaches the main input of the Transform node to the selected node:

**transform.setInput(0, selNode)**

And the third line sets the rotation in the Transform node to be our iteration variable times the rotation increment:

**transform['rotate'].setValue(i * rotationIncrement)**

That means, in the first loop the rotation value will be:

**1 * rotationIncrement**

In the second loop it will be:

**2 * rotationIncrement**

In the third loop it will be:

**3 * rotationIncrement**

And so on.

So we’re using the iteration variable to increase the rotation that we apply to the Transform nodes by one rotation increment for each round of the loop.

This is actually a very useful trick for when you’re making other Python scripts where you need to increment a value like this – Use the iteration variable to your advantage.

Paste the Python script into the Script section of the Python Script Button in your NoOp (TransformMultiple) node, and you’re good to go.

Here’s a quick example of the tool in action:

> [!info] Running the TransformMultiple script to create symmetry  
>  
> [https://youtu.be/9bSybgswCRc](https://youtu.be/9bSybgswCRc)  

One roto shape rotated around several times to create a symmetrical pattern.

You could make way more complex and interesting patterns than this, just experiment a bit 🙂

Here is the entire tool which you can copy and paste into your Node Graph:

```Plain
set cut_paste_input [stack 0]
version 14.0 v2
push $cut_paste_input
NoOp {
 name TransformMultiple
 selected true
 xpos -95
 ypos -124
 hide_input true
 addUserKnob {20 User l "Generate Transforms"}
 addUserKnob {3 numberOfNodes l "number of nodes"}
 numberOfNodes 8
 addUserKnob {7 rotationIncrement l "rotation increment" R 0 360}
 rotationIncrement 45
 addUserKnob {26 ""}
 addUserKnob {22 generateTransforms l "Generate Transforms" T "import nuke\n\nthisNode = nuke.thisNode()\nselNode = nuke.selectedNode()\nnumberOfNodes = int(thisNode.knob('numberOfNodes').value())\nrotationIncrement = float(thisNode.knob('rotationIncrement').value())\n\nif numberOfNodes < 1:\n    nuke.message(\"The number of nodes must be 1 or higher.\")\nelse:\n    for i in range(1, (numberOfNodes+1)): \n        transform = nuke.createNode('Transform')\n        transform.setInput(0, selNode)\n        transform\['rotate'].setValue(i * rotationIncrement)\n" +STARTLINE}
}
```

Let's move on to the final example.

# Shuffling Out All Of The Render Passes

When you’re compositing CG renders using the additive method, you’ll  
need to shuffle out all of the render passes to be able to work on them  
individually.

Depending on how many passes you get in  
your render, this can be quite tedious. But don’t worry – once again,  
Python will come to the rescue 😊

Let’s quickly break down what we want to do.

We want to shuffle out all of the render passes in our selected CG renders, label the Shuffle nodes based on the name of the render passes, and then remove the superfluous layers/channels from each stream. – So that each pipe only contains the specific layer and channels from that render pass, shuffled into rgba.

Let’s dive in.

### User Controls

For this tool, we really only need the single Python Script Button to activate the chain of events.

So, create a NoOp node and name it ‘ShuffleOutRenderPasses’.

In the node, create a Python Script Button, name it ‘shuffleOutRenderPasses’ and label it ‘Shuffle Out Render Passes’.

Here’s what we’re going to put in the Script section of the Python Script Button:

### The Python Script

> [!important]
> 
> This time, we’ll use a few different approaches to show a bit more of Python and how you can do things in more than one way by altering the code.

```Plain
import nuke

nodes = nuke.selectedNodes()

for node in nodes:
    if node.Class() == 'Read':
        channels = node.channels()
        layers = list(set([channel.split('.')[0] for channel in channels]))
        layers.sort()
        if 'rgba' in layers:
            layers.remove('rgba')
        for layer in layers:
            shuffleNode = nuke.nodes.Shuffle2(label=layer, inputs=[node])
            shuffleNode['in1'].setValue(layer)
            removeNode = nuke.nodes.Remove()
            removeNode['operation'].setValue('keep')
            removeNode['channels'].setValue('rgba')
            removeNode.setInput(0, shuffleNode)
    else:
        pass
```

If you think this code looks a little bit more daunting than the previous ones, don’t worry. Let’s go through it step by step.

**import nuke**

Just like before, we import the Nuke module.

**nodes = nuke.selectedNodes()**

Here we create a **variable** called **nodes** to retrieve a list of the currently selected nodes.

**for node in nodes:**

Here we start the for-loop. The iteration variable is called **node**, and the **sequence** that’s being iterated through is the list of our selected nodes (which we defined as **nodes** just above).

Next, let’s move to the body of the for-loop (remember to add **indentation**):

**if node.Class() == 'Read':**

Instead of putting **‘Read’** inside the parentheses of the **selectedNodes()** like we did earlier, another way of getting only the Read nodes in the selected nodes is to do an if-else check.

In the line above, we’re saying: if the class of the current node is Read, then execute the below statements. Notice that we use **double equal signs** when checking if something is equal to something else in an if-else statement. (Single equal signs are used for assigning values).

Next, let’s move on to the body of the if statement (remember to add **indentation**):

**channels = node.channels()**

First, we define a **variable** called **channels** which we use to retrieve a list of the channels in the current Read node in the iteration (which we’ve already defined as **node**) using the method **.channels()**.

> [!important]
> 
> A **method** is like a function, but while functions are independent blocks of code which can be called from anywhere, methods are associated to an object. So, we could use the **list()** function anywhere in our code, but the **.channels()** method has to be connected to something, like a Read node.Hence, the **node.channels()**.

**layers = list(set([channel.split('.')[0] for channel in channels]))**

This line is a little bit more complex. Let’s break it down.

First, we create a **variable**, called:

**layers**

This will contain a list of all the layer names from our set of channels.

Remember that Nuke names layers and channels like this:

**layer.channel**

So for example, the layer called **forward** that’s created by a ScanlineRender node has two channels, **u** and **v**:

**forward.u**

**forward.v**

When we’ll be shuffling out the render passes later, we only want the **layer** name as the label for our Shuffle node. So we would for example want **forward** as the label, and not **forward.u** and/or **forward.v** – because the Shuffle node will contain both.

Let’s quickly jump one step ahead:

If you studied the linked pages at the very beginning of the guide, you may remember that the **set()** function stores multiple items, and cannot contain duplicates. So we can use it to remove any duplicate layer names. (**forward** would otherwise show up twice in a list, for example, because it has two channels).

However, a **set** is unordered and cannot be changed, meaning we can’t sort the **layer** names alphabetically (which is something we want to do to make our Shuffle nodes all nice and neatly ordered).

Which is why we turn the **set** into a **list** with the **list()** function:

**list(set())**

Inside the **set()** function, we add another for-loop, just written a bit differently:

**[channel.split('.')[0] for channel in channels]**

Let’s break down how it works.

You can write a for-loop on one line, by putting the body first, and then the for-loop structure at the end. The above is called [list comprehension](http://docs.python.org/tutorial/datastructures.html?ref=keheka.com#list-comprehensions) and the code is slightly more efficient/faster than spreading it over multiple lines. It also means we can put it neatly inside a function,  
like our **set()** function.

The second part of the code,

**for channel in channels**

should look familiar to you by now. It’s the for-loop, only placed at the end of the line of code instead of on its own line.

**channel** is the **iteration variable** and **channels** is the **sequence** to iterate through, which we previously defined as a list of all the channels in the current Read node.

The first part of the code,

**channel.split('.')[0]**

means: take the current channel and split its name into two at the dot (**.**). Then, only keep the first part of it. **[0]** refers to the first item in a collection. (And [1] would be the second item, and so on).

So for example, **forward.u** would become **forward** and **u** (separated), and we would only keep **forward**.

Now, we have all the **layer** names in a **set** without any duplicates, and we have turned that set into a **list**.

Let’s tidy up the list by sorting it alphabetically.

**layers.sort()**

Here, **layers** is the variable we created earlier (which now contains our list of unsorted, but unique layer names). And **.sort()** simply sorts the list alphabetically.

> [!important]
> 
> By writing **.sort(reverse=True)** you could sort by reverse alphabetical order.

So now we have a nice, sorted list of all the layer names.

Typically, when shuffling out render passes, we don’t need the rgba layer, because we can just look at the Read node directly.

So let’s remove that layer from the list.

**if 'rgba' in layers:**

This line checks if there is an rgba layer in our list with an if-check. (Remember, the variable called **layers** contains our list of sorted layers).

**layers.remove('rgba')**

If there is a layer called rgba, this line removes it from the list.

Notice that we don’t have an **else** statement above. When the **if** check is false, it will simply skip over the body. So if you require code to run only when the statement returns true (and do nothing else if false) then an else statement is not needed.

Finally, we have a tidy list of all of the layers that we want to shuffle out.

Let’s make a for-loop to shuffle them out:

**for layer in layers:**

In this line, **layer** is our **iteration variable** and **layers** is the **sequence** to iterate through: our sorted list of layer names.

**shuffleNode = nuke.nodes.Shuffle2(label=layer, inputs=[node])**

Here, we create a variable called **shuffleNode** and use it to create a Shuffle node, with:

**nuke.nodes.Shuffle2()**

Notice that we are using the newer version – version two – of the Shuffle node, with the **Shuffle2** class. Also notice that we are using a different code for creating the node than before. You can explore the differences between the two [here](https://learn.foundry.com/nuke/developers/140/pythondevguide/basics.html?ref=keheka.com).

As we are creating the Shuffle node, we can simultaneously set some of its parameters.

Inside the parentheses of **.Shuffle2()** above, we add:

**label=layer, inputs=[node]**

This will set the **label** of the current Shuffle node to be the current **layer** name, and it will connect the Shuffle node’s **input** to the current **Read node**.

> [!important]
> 
> If you wanted to connect both of the Shuffle node’s inputs to different nodes, which you had assigned to the variables **a** and **b**, then you could change the end of the line above to **inputs=[a, b]**.

The above is just an example of a way to shorten down the script, saving a couple lines of code.

However, let’s continue writing the next changes we make individually, for clarity:

**shuffleNode['in1'].setValue(layer)**

This line will set the current Shuffle node’s **in1** knob to the current **layer**.

Now that we have shuffled out the layer into rgba, let’s remove all the other layers from this stream using a Remove node:

**removeNode = nuke.nodes.Remove()**

This line defines a **variable** called **removeNode** which is used for creating a Remove node.

**removeNode['operation'].setValue('keep')**

This line sets the **operation** knob of the Remove node to **keep**.

**removeNode['channels'].setValue('rgba')**

This line sets the **channels** knob of the Remove node to **rgba**.

**removeNode.setInput(0, shuffleNode)**

And this line connects the main input of the Remove node to the current Shuffle node.

Finally, let’s close out the if-else statement:

**else:**

**pass**

And there you go! When run, the script will get all the layers from all the selected Read nodes, shuffle them out and label the Shuffle nodes neatly, and then remove the superfluous channels from each stream.

A super useful script, in my opinion!

Here is the entire tool which you can copy and paste into your Node Graph:

```Plain
set cut_paste_input [stack 0]
version 14.0 v2
push $cut_paste_input
NoOp {
 name ShuffleOutRenderPasses
 selected true
 xpos -92
 ypos -127
 hide_input true
 addUserKnob {20 User l "Shuffle Out Render Passes"}
 addUserKnob {22 shuffleOutRenderPasses l "Shuffle Out Render Passes" T "import nuke\n\nnodes = nuke.selectedNodes() \n\nfor node in nodes: \n    if node.Class() == 'Read': \n        channels = node.channels() \n        layers = list(set(\[channel.split('.')\[0] for channel in channels])) \n        layers.sort() \n        if 'rgba' in layers: \n            layers.remove('rgba') \n        for layer in layers: \n            shuffleNode = nuke.nodes.Shuffle2(label=layer,inputs=\[node]) \n            shuffleNode\['in1'].setValue(layer) \n            removeNode = nuke.nodes.Remove() \n            removeNode\['operation'].setValue('keep')\n            removeNode\['channels'].setValue('rgba')\n            removeNode.setInput(0, shuffleNode)\n    else: \n        pass" +STARTLINE}
}
```

# Final Thoughts And Resources

Hopefully, this guide has helped you better understand the basics of how to make your own Python tool. There is a **ton** more to explore for those interested. – And very few limitations on what’s possible to create.

And you can also combine what you have learned in this guide in different ways. You could for example make nested for-loops, for changing multiple parameters of multiple nodes.

As an example, if you take a look at the Python script in the [tool I linked](https://www.nukepedia.com/gizmos/3d/mixture_cam/?ref=keheka.com) in this tutorial: [Blending Between Cameras In Nuke](https://www.keheka.com/blending-between-cameras-in-nuke/), it uses a nested for-loop.

If you want to dive deeper into Python, here are some great resources:

- [https://www.w3schools.com/python/](https://www.w3schools.com/python/?ref=keheka.com)

- [https://www.python.org/about/gettingstarted/](https://www.python.org/about/gettingstarted/?ref=keheka.com)

- [https://www.learnpython.org/](https://www.learnpython.org/?ref=keheka.com)

And the link below is very useful for learning more about the **nuke** module. For example to find out which parameters each function takes:

- [https://learn.foundry.com/nuke/developers/140/pythondevguide/index.html#](https://learn.foundry.com/nuke/developers/140/pythondevguide/index.html?ref=keheka.com#)

> [!important]
> 
> For instance, [here](https://learn.foundry.com/nuke/developers/140/pythondevguide/_autosummary/nuke.selectedNodes.html?ref=keheka.com) we can see that the selectedNodes() function takes one optional parameter, the node class.

I’ll leave you with one last little tip:

Since I explained all the bits of code in this guide, I didn’t touch on comments.

Comments can be used to explain your Python code and make it more readable, or to prevent a part of the code from executing when testing things out.

You can add comments to your script with the **#** key. Use it to describe what’s happening in your code, just like you would label a Backdrop around a group of nodes in Nuke to explain what’s happening in that section of the script. Especially when your Python script gets larger and more complex.

Take a look [here](https://www.w3schools.com/python/python_comments.asp?ref=keheka.com) for examples of how to comment.

Anyway, that's it! Hopefully, these scripts and any variations that you make  
from them will save you a bunch of time and headache in your day-to-day  
comping 😊