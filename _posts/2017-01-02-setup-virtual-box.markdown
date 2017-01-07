---
layout: post
title: Week 0- Setting up the EEB177 Virtual Box
date:   2017-01-02
author: Gaurav Kandlikar
---

To minimize platform-related problems during this quarter, everyone in this class we will  a standardized "guest" computer that essentially runs as a separate computer within your "host" operating system (e.g. Mac OSX, Windows 7, etc.). To do this, we will use the VirtualBox software, which lets us visualize virtual machines images, and a Linux virtual machine image we have created for the course that includes core Unix functionality as well as a set of important computing packages (e.g. `R`, `python`.) 

### Download the course virtualbox image and VirtualBox software

Download the EEB177 virtualbox image from **INSERT DROPBOX LINK**. *Note: this is a large file (~2gb), so prepare to be patient!* For now, place this file on your Desktop or at another easily-accessible location.

Download and install the appropriate version of VirtualBox for your machine from this [link](https://www.virtualbox.org/wiki/Downloads). 

### Importing the virtualbox
Follow these steps to get your virtualbox going:  

1) Launch the VirtualBox software    
2) Click "File" in the upper menu, and select "Import Appliance"    
3) Click on the Folder icon in the Import dialog box and navigate to the folder containing the Virtualbox image that you downloaded from Dropbox   
4) Double-click on the image file "eeb177-lubuntu-box.ova"   
5) Click next; this will take you to the Appliance Settings menu. Feel free to read through the appliance settings, and click "Next".   
6) Importing the image will take a few minutes.   
7) Once importing is complete, launch the virtualbox by double clicking on the "EEB177-Lubuntu" icon. **NOTE**: You will likey have an error the first time you try to launch your box, with the error message `A new node couldn't be inserted because one with the same name exists. (VERR_CFGM_NODE_EXISTS)`. Complete Step 8 below to work around this error.   
8) Right-click on the "EEB177-Lubuntu" icon and select "Settings". Select "USB" in the settings menu. Un-check the "Enable USB controller" checkbox, and recheck the same box. Click OK to apply the settings. In Macs, this the "USB" settings are located within the "Ports" tab. 
9) Double click on the icon to launch the virtualbox. You should see a series of black screens scroll by with some text; finally, you should see a dialog box with username "EEB-177-student". The password for this account is `yakyak`.   

You should see a home screen that looks like this:

![]({{ site.url }}/images/virtualbox_screenshot.png)

If you're here, congrats! You have a functioning virtualbox! Complete the next few steps to add some more functionality to your environment: 

- Launch the Firefox browser and ensure that you can connect to the internet.   
- You can make files that are housed on your "host" computer available to the "guest" computer (i.e. the virtual computer) by setting up "Shared Folders". To set up a shared folder, navigate to "Devices" and select "Shared Folder Settings". Click on the "Add" icon (a blue folder with a green "+"). Browse to the folder you wish to share across the host and guest in the "Folder Path". 
