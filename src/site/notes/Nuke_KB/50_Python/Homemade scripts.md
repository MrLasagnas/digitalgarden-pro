---
{"dg-publish":true,"permalink":"/nuke-kb/50-python/homemade-scripts/"}
---



  

- Pour pouvoir labeler les nodes avec un Shift +N


    ```JavaScript
    import nuke
    
    def quick_label():
        """Prompts the user for a label and applies it in bold font while keeping it centered."""
        selected_node = nuke.selectedNode()
    
        if not selected_node:
            nuke.message("No node selected!")
            return
    
        label_text = nuke.getInput("Enter node label:", selected_node['label'].value())
    
        if label_text is not None:  # Ensure the user didn't cancel
            selected_node['label'].setValue(label_text)  # Apply label
            selected_node['note_font'].setValue("Bold")  # Set bold font
    
    # Add the function to the menu
    nuke.menu("Nuke").addCommand("Edit/Quick Label", quick_label, "shift+n")
    ```