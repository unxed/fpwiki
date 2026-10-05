# SVG Image List

Hi, this is a simple tutorial on how to use SVG Images in our Lazarus applications. 

## Contents

  * 1 Requirements
  * 2 Tutorial
    * 2.1 Create elements
    * 2.2 Create SVG Image List
    * 2.3 Optional parameter



## Requirements

1) Install [BGRABitmap](<BGRABitmap.md> "BGRABitmap")

2) Install [BGRAControls](<BGRAControls.md> "BGRAControls")

(Hint: Use online package manager) 

## Tutorial

In this tutorial we will add an SVG Icon to a TMainMenu. 

### Create elements

1) Drop a TMainMenu, add some entries like 'File' and a sub entry 'Open'. 

2) Drop a regular TImageList, assign it to the MainMenu1 we previously created. 

3) Assign the ImageIndex in the entries, for example in the File > Open add index 0. 

That is not different as you need to do it anyways with a regular image list, so nothing changes here. The next step shows the SVG stuff. 

### Create SVG Image List

1) Add a TBGRASVGImageList from BGRA Themes pallete. 

2) Double click the SVG Image List component and add some SVG's, just load them from file. 

3) Add this code, the glue between our two image lists: 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      BGRASVGImageList1.PopulateImageList(ImageList1, [24]);
    end;
    

**Done!**

Now you have SVG Icons in your application, repeat for buttons and any component that supports a regular image list. 

### Optional parameter

The second parameter is array of resolutions, 24px is the default on Windows. You can add more like 48px for retina on macOS or High DPI on Windows. 
    
    
    BGRASVGImageList1.PopulateImageList(ImageList1, [24, 48]);
    

**Important:** You need to enable 'Scaled' property on the regular image list in order to the extra resolutions work fine. 

Enjoy!

---

_Source: [https://wiki.freepascal.org/SVG_Image_List](https://web.archive.org/web/20220930213033/https://wiki.freepascal.org/SVG_Image_List)_
