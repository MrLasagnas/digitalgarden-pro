---
{"dg-publish":true,"permalink":"/nuke-kb/50-python/basic-py-script/"}
---

  

  

We can check the major and minor version of our nuke :

```Python
nuke.NUKE_VERSION_MAJOR
nuke.NUKE_VERSION_MINOR
```

We can check if our code is using the user interface (is it run by an artist or by the farm) :

```Python
nuke.GUI
```

You can undo (or redo) the last change :

```Python
nuke.undo()
nuke.redo()
```

  

  

  

We can return the node currently selected :

```Python
nuke.selectedNode()
nuke.selectedNodes()

\#we can also give a node class argument.
nuke.selectedNodes("Grade")
```

  

Or we can return a list of all the nodes, that we can then iterate through a loop for example.  
This method also take a node class as an argument to return only the node of the defined class.

```Python
nuke.allNodes()
nuke.allNodes("Blur")
```

  

  

We can directly create a node of a certain class via python :

```Python
\#Using this method, you simulate the user creating the node
\#so when created, the node connects to last selected node, and its properties are opened
nuke.createNode("Blur")

\#This methods just creates the node
nuke.nodes.Blur()
```

  

We can verify if a node exists with the following method. It returns a boolean.

```Python
nuke.exists("Grade3")
```

  

This code can access a specific node by giving its name as an argument. With this method, it can be store in a variable.

```Python
my_blur = nuke.toNode("Blur1")
```

  

We can get/modify the position of the node :

```Python
\#returns the XY values of the node
my_blur.xpos()
my_blur.ypos()

\#modify th XY values of the node
my_blur.setXpos(100)
my_blur.setYpos(-50)
```

  

We can set the input of the node :

```Python
my_blur.setInput(0,nuke.toNode("Grade1"))
```

We can get the name of our node :

```Python
my_blur.name()
\#or
my_blur["name"].value()

\#Result: Blur1
```

We can select or deselect the node:

```Python
\#Select the node
my_blur.setSelected(True)

\#Deselect the node
my_blur.setSelected(False)
```

  

We can delete the node :

```Python
nuke.delete(my_blur)
```

  

Then, we can access a specific knob of our node with this method :

```Python
b_size = my_blur.knob("size")

\#or
b_size = my_blur["size"]
```

  

From there we can store the value of the knob :

```Python
b_size.getValue()

\#or
b_size.value()

\#theses two methods can return the value in a different way. Test it in your code.
```

  

Or we can set the value of the knob :

```Python
b_size.setValue(25)
```