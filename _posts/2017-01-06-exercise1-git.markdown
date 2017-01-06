---
layout: post
title: Week 1- Exercise 1 (Version control with git and github)
date:   2017-01-06
author: Gaurav Kandlikar
---

In this exercise, you will go through the "`git add`, `git commit`, `git push`" workflow. In Section 2 of this exercise, you will explore the use of branches and reverts to previous commits.

Begin this (and all subsequent exercises in this course) by opening up a terminal window and navigating to your `homework` folder. 

### Section 1

0. Ensure that you have set up a git repository to track your `homework` folder and that this repository is connected to your github repository ([instructions]()).   
1. Create a new subfolder called `exercise_1` and add this to the repositories tracked by git. Hint: use `mkdir` to make a new folder; use `git add <filename>` to start tracking it with git.  
2. Navigate into `exercise_1` and create a readme file by executing `touch README.txt` (the command `touch` simply creates an empty file with a given name). Open `README.txt` and write in some information about this exercise (e.g. when you are completing this exercise, what are the goals of this exercise). Hint: you can use `sudo gedit <filename>` to open text files from the terminal.   
3. Navigate back up to the `homework` directory and add the newly created `exercise_1/README.txt` file to the git repository.  
4. View the status of your git repository using `git status`. You should see something that looks like this: 

![]({{ site.url }}/images/exercise1-gitstatus.png)

5. Now, a snapshot of `exercise-1/README.txt` is present with the so-called staging-area of the git repository. Use `git commit` to commit this snapshot. Make sure that your commit includes a meaningful commit message! Use `git status` once again to verify that your commit was successful.    
6. Push your current commits to your online github repository using `git push`. Visit your github repository through your browser and verify that your commits have gone through. Hint: this assumes that you have successfully completed the git and github setup - please see Gaurav if you have trouble with this part!  

7. Return to the terminal, and within the `exercise-1` folder, create a file called `one-liner.txt` and write your favorite one-liner into this document. Follow the workflow above to add, commit, and push this to your github repo. Hint: Gaurav appreciates a good pun, especially if they relate to computing, ecology, or evolution.  

