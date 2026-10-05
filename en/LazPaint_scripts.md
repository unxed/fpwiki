# LazPaint scripts

│ **English (en)** │  **[français (fr)](</LazPaint_scripts/fr> "LazPaint scripts/fr")** │    
****

[Go back](<LazPaint.md> "LazPaint")

Scripts are accessible from the **Script** menu. There are in Python language and stored in the _scripts_ folder of the application. 

## Contents

  * 1 Provided scripts
  * 2 How to make a script
  * 3 Run a script
    * 3.1 Run from a user folder
    * 3.2 Run from Script menu
  * 4 Available modules
  * 5 Grouping changes
  * 6 Interact with Pillow
  * 7 Do an action on each layer
  * 8 Color values



## Provided scripts

  * Channels: separate the color channels to RGB or HSL.
  * Layer effects: known layer effects like drop shadow, stroke or color overlay.
  * Mask: function to create a mask layer
  * Render: some example of using the tools from a script
  * Version: show the version of Python



## How to make a script

A script is a text file with the extension .py that contains the code to run. The first line is a comment that contains the title of the script: 
    
    
    # My script
    

Or if you want it to be in a submenu: 
    
    
    # My folder > My script
    

You will need modules to communicate with Lazpaint from the script, so for example to show a message you would write: 
    
    
    from lazpaint import dialog
    

To use a function from the module: 
    
    
    dialog.show_message("hello world")
    

So here is our hello world program: 
    
    
    # Tutorial > Hello world program
    from lazpaint import dialog
    dialog.show_message("hello world")
    

## Run a script

### Run from a user folder

You can save your script in your documents and in LazPaint use Script > Run script to browse the folders. 

### Run from Script menu

Alternatively you can add you script to the menu of LazPaint. To do that, using administrator rights, copy the script file into the _scripts_ folder of LazPaint: 

  * On Windows: _C:\Program Files\LazPaint\scripts_
  * On Linux: _/usr/share/lazpaint/scripts_



Note that the first line must start with a # and have no spacing before. You need to restart LazPaint to update the Script menu. 

## Available modules

  * colors: definitions of colors and color filters
  * command: send raw commands and get version of LazPaint
  * dialog: show messages and dialog boxes
  * filters: apply filters (other than color filters)
  * image: new, load, rotate...
  * imagelist: use the imagelist window (get next image, add files)
  * layer: configure layers, access pixels
  * selection: act on the selection (in particular the blue selection)
  * tools: use the tools by providing mouse positions and key strokes
  * view: zoom and grid



## Grouping changes

Generally you would like the script to be undoable by clicking only once on Undo. To do that you need to group the changes. For example: 
    
    
    from lazpaint import image, layer, colors
    image.do_begin()           # start undo block
    layer.new()                # first action: new layer
    layer.fill(colors.YELLOW)  # second action: fill with yellow
    image.do_end()             # end undo block
    

## Interact with Pillow

If you want to modify the image using a module like [Pillow](<https://python-pillow.org/>), you can transfer the image to the module using a temporary file: 
    
    
    from lazpaint import image, layer
    from PIL import Image
    temp_name = image.get_temporary_name()
    temp_name = layer.save_as(temp_name) # export to disk
    im = Image.open(temp_name) # load into Pillow
    

Then you can transfer back the image to LazPaint: 
    
    
    im.save(temp_name, "PNG") # export to disk
    layer.add_from_file(temp_name) # import from disk
    

## Do an action on each layer

The module image contains the function _iterate_layers_ which selects each layer one after another. 
    
    
    from lazpaint import image, colors
    image.do_begin()
    for cur_layer_id in image.iterate_layers():
      colors.complementary()
    image.do_end()
    

## Color values

Colors are stored in an instance of the class colors.RGBA. It has 4 fields: red, green, blue and alpha which are between 0 and 255. Those are in [sRGB](<https://en.wikipedia.org/wiki/SRGB>) colorspace. 

The functions colors.to_linear and colors.to_std transforms a channel value from sRGB and from linear RGB. Linear values are floating point values between 0 and 1. 
    
    
    from lazpaint import colors, dialog, tools
    dodger_blue = colors.RGB(30, 144, 255)
    half_transparent_black = colors.RGBA(0, 0, 0, 128)
    dialog.show_message(dodger_blue.blue) # displays 255
    tools.set_fore_color(dodger_blue) # select dodger blue as pen color

---

_Source: [https://wiki.freepascal.org/LazPaint_scripts](https://web.archive.org/web/20250401024027/https://wiki.freepascal.org/LazPaint_scripts)_
