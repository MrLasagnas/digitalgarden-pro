---
{"dg-publish":true,"permalink":"/nuke-kb/50-python/user-interaction/"}
---






We can interact with the user in various ways.

  

We can just display a message :

```Python
nuke.message("Hi user!")
```

  

![message.gif](/img/user/0_Assets/attachments/message.gif)

  

We can ask a yes or no question that returns a boolean :

```Python
nuke.ask("Are you ok?")
```

  

![ask.gif](/img/user/0_Assets/attachments/ask.gif)

Let the user choose between multiples options in a dropdown :

```Python
nuke.choice("Name of the window","Choose an option",["A","B","C","D"])
```

![choice.gif](/img/user/0_Assets/attachments/choice.gif)

  

Let the user type he’s own response :

```Python
nuke.getInput("What is your name ?")
```

![GetInput.gif](/img/user/0_Assets/attachments/GetInput.gif)

  

Let the user choose a file in the system :

```Python
nuke.getFilename("Choose a file...")
```

![filename.gif](/img/user/0_Assets/attachments/filename.gif)

  

Let the user choose a color :

```Python
nuke.getColor()
```

![getColor.gif](/img/user/0_Assets/attachments/getColor.gif)