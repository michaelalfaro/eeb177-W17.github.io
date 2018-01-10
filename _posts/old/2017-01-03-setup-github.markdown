---
layout: post
title: Week 0- Getting Started with Git and Github
date:   2017-01-03
author: Gaurav Kandlikar
---

**Note**: all instructions in this document are derived from [this guide](https://help.github.com/articles/set-up-git/). Your virtualbox already has the latest version of Git installed, so you can skip step 1 of the "Setting up Git" section.

### Setting up git locally

Boot up your virtualbox and launch a terminal. You should be in the home directory- verify this by using `pwd` (i.e. "*P*ath of current *W*orking *D*irectory).

Before we use Git for the first time, we need to tell it our name and email address. This allows your commits to have an author and a contact- for example, here is a screenshot of one of Gaurav's repositories:

![]({{ site.url }}/images/git-screenshot.png)


You can set up your name and email address with the following commands in the terminal:  

`git config --global user.name "YOUR NAME"`  
`git config --global user.email "YOUR EMAIL ADDRESS"`


You can now use Git to do version control locally through your command line- hurray! To start using Git, use your terminal to navigate to the Desktop using the `cd` (i.e. "change directory") command and create a new `eeb-177` folder using `mkdir` (i.e. make directory). **Note**: some of you may have done this step already if you were following along in lecture on Tuesday:  

`cd Desktop`   
`mkdir eeb-177`   
Then, you can switch into the newly made folder by using `cd eeb-177`. 

If you had already made the `eeb-177` folder during lecture, simply navigate to it (i.e. `cd Desktop`; then `cd eeb-177`).   

Within this folder, you will now make a "lab-work" folder that will include exercise you do during lab:  

`mkdir lab-work`  


Navigate into this new folder using `cd lab-work`, and confirm that you made it in by entering the command `pwd` (The output of `pwd` should be  `home/eeb177-student/Desktop/eeb-177/lab-work`). We want this folder to be tracked using git: for each weeks assignment, you are to make a new subfolder within this folder and use Git+Github to have the weekly subfolders pushed through to github. 

To start tracking this folder using git, enter the following command:

`git init`

*Note*: this command only needs to be run when first setting up a new git repository.  

It is good practice to initialize each git folder with a README file that explains the purpose of the repository. Create a README file in the homework folder with the command `touch README.txt`- this just creates an empty file. You can open this empty file using gedit (`gedit README.txt`). Edit this file to include your **name, year in college, computing experience, and lab section** for this course.   

Next, we need to ask git to keep track of this file:  
`git add README.txt`

Next we "commit" this file as a version we want saved in our history:

`git commit -m "initial commit: adding README file"`

*Note*: `-m` is a *flag* that says that this particular commit should be made with the message "initial commit: adding a barebones README file". 


You can look over the (brief) history of your git repository by asking to see its log:

`git log`


-----

### Getting Github to talk to your computer 

You now have a functioning Git repository on your computer- hurray! Now, we need to connect it to your Github account so that the files and commit history on your computer can be "pushed" through to be available online. 

Log into your account on github, and click "New repository" on the right hand column. Give it an appropriate name (it will contain assignments for this course), and keep it public. On the "Quick setup" page, make sure that "HTTPS" is selected- *not* "SSH". Copy the code under **"…or push an existing repository from the command line"**, return to the terminal, and paste in the two lines. You will be prompted for a username and password; these are your github credentials. 

*Note*: As it stands, you will need to enter your username and password every time you wish to use `git push`. Run the following two commands in your terminal to store your credentials for one hour (i.e. you will have to enter your username/pwd once every hour):

Set git to use the credential memory cache:      
`git config --global credential.helper cache`    

Set the cache to timeout after 1 hour (setting is in seconds):  
`git config --global credential.helper 'cache --timeout=3600'`    

If you now navigate to your new repository on github, you should see your lab-work folder with the README file that you wrote. Congrats!
