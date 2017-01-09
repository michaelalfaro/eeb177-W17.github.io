---
layout: post
title: Week 1- Exercise 1 (Version control with git and github)
date:   2017-01-06
author: Gaurav Kandlikar
---

In this exercise, you will go through the "`git add`, `git commit`, `git push`" workflow. In Section 2 of this exercise, you will explore the use of branches and reverts to previous commits.

Begin this (and all subsequent exercises in this course) by opening up a terminal window and navigating to your `homework` folder. 

### Section 1- adding, commiting, and pushing to github

Before you begin, please read through [this helpful summary](https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control#_getting_started)<sup>0</sup> on the basics of version control.

0. Ensure that you have set up a git repository to track your `homework` folder and that this repository is connected to your github repository ([instructions](http://gaurav.kandlikar.com/eeb177-W17/2017/01/03/setup-github.html))<sup>1</sup>.   
1. Create a new subfolder called `exercise_1` and add this to the repositories tracked by git. Hint: use `mkdir` to make a new folder; use `git add <filename>` to start tracking it with git.  
2. Navigate into `exercise_1` and create a readme file by executing `touch README.txt` (the command `touch` simply creates an empty file with a given name). Open `README.txt` and write in some information about this exercise (e.g. when you are completing this exercise, what are the goals of this exercise). Hint: you can use `sudo gedit <filename>` to open text files from the terminal.   
3. Navigate back up to the `homework` directory and add the newly created `exercise_1/README.txt` file to the git repository.  
4. View the status of your git repository using `git status`. You should see something that looks like this: 
![]({{ site.url }}/images/exercise1-gitstatus.png)

5. Now, a snapshot of `exercise-1/README.txt` is present with the so-called staging-area of the git repository. Use `git commit` to commit this snapshot. Make sure that your commit includes a meaningful commit message! Use `git status` once again to verify that your commit was successful.    
6. Push your current commits to your online github repository using `git push`. Visit your github repository through your browser and verify that your commits have gone through. Hint: this assumes that you have successfully completed the git and github setup - please see Gaurav if you have trouble with this part!  

7. Return to the terminal, and within the `exercise-1` folder, create a file called `one-liner.txt` and write your favorite one-liner into this document. Follow the workflow to add, commit, and push this to your github repo. Hint: Gaurav appreciates a good pun, especially if they relate to computing, ecology, or evolution.  

8. Go back to `exercise-1/README.txt` and in this document add some text to this document. This can be anything you like- if you're out of ideas, tell me about your favorite restaurant in Westwood. Once this is saved, go through the same workflow to add, commit, and push your new changes to this file.

Verify that you have completed three commits in this section by looking at the output of `git log`.

### Section 2- Branching, merging, removing files, and undoing commits

In this section, you will learn:
- how (and when) to create new *branches* in a git repository     
- how to merge two branches of a repository     
- how to remove a file from a git repository     
- how to revert to a previous commit    

Note: You probably won't need to be doing most of these during this course, but understanding these commands will help you better appreciate the power of version control.  

Before you begin, please read through [this helpful page](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell)<sup>2</sup> on branching in git repositories.

1. Navigate to the `homework` folder and run `git branch` to check which branch you are currently on. Unless you have already made a new branch, you should see that your repository only has the `master` branch, and the asterisk indicates that you are currently on that branch.  
2. Use `git checkout -b dummy-branch` to make a new branch called `dummy-branch` within the repository.
3. Check that you are on the `dummy-branch` (how would you do this?).  
4. Navigate into the `exercise-1` folder and run `touch dummy-file`. Navigate back to the `homework` folder, and go through the "add, commit, push" workflow to add this new `dummy-file` to your git repository. 
5. Check your git log using `git log --decorate` to make sure that the `dummy-file` was added to the new dummy branch. Your output shoud look something like this:  
![]({{ site.url }}/images/exercise1-gitbranch.png)
6. In the `exercise-1` folder, check the list of files using `ls`. You should see that your empty file `dummy-file` is included in the output. 
7. Switch back to the Master branch by issuing `git checkout master`, and again check the contents of the `exercise-1` folder. You should **not** see the `dummy-file` in this list- can you explain why? 

At this point, you have two distinct branches of your repository- essentially, two distinct versions! You can imagine a workflow in which you have a stable program in your `master` branch, and splitting off into new branches to try building new features while making sure that there exists an untarnished copy of the core program. 

8. To "import" the changes made in `dummy-branch` into `master`, make sure that you are currently on the `master` branch and run `git merge dummy-branch`. Check the file list in `exercise-1` again- you should see `dummy-file` in this list!

9. Since this is a dummy file, let's get rid of it! To remove a file from a git repository, use `git rm <filename>`. This step is basically the opposite of `git add <filename>`. Commit the removal of the dummy file and push your changes. 

10. Oops! The dummy file was actually of some importance, and we want to undo our deletion! Well, we can "revert" a commit that was made in error by issuing `git revert <commit number>`, where `<commit number>` is the long code associated with each commit (this is available in your git log- see screenshot below- commit number is highlighted in orange). In this case, we want to rever the commit in which we deleted `dummy-file`. 
![]({{ site.url }}/images/exercise1-commitnumber.png)

11. Examine the effects of `git revert` by checking the git log. How has git effectively undone our deletion? Does our old commit, in which we deleted `dummy-file`, simply vanish?

12. Run `git push` to push all of your work to Github, where I can view your commits and grade your work!  

If you'd like some more guidance on branches and reverting in git, check out [this handy guide](https://www.atlassian.com/git/tutorials/undoing-changes/)<sup>3</sup>.

### Section 3- Viewing your git repository from Github

1. Log into your github account and navigate to your EEB177 repository.   
2. By default, github will show your the `master` repository when you log in. If you got through exercise 12 in Section 2, you should see `dummy-file` as one of the files in this master branch (remember, we deleted `dummy-file` in one commit, and reverted the deletion in the next commit!).  
3. You can look at the changes made in your repository by clicking on the commit history option in the top panel of your repository (See "25 Commits" highlighted in green at the top-left of the screenshot below):
![]({{ site.url }}/images/exercise1-githubrepo.png). Click on this link and explore your repository's commit history, making note of branch creations, etc.   
4. Save a screenshot of your own repository's commit history to your `exercise-1` folder- make sure that you give the screenshot a useful name! Use git to add, commit, and push this new image.   
5. Return to your repository home screen, and find how to switch to `dummy-branch` from `master`. Take a look at the commit history of this branch.

<sup>0</sup> https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control#_getting_started   
<sup>1</sup> http://gaurav.kandlikar.com/eeb177-W17/2017/01/03/setup-github.html   
<sup>2</sup> https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell   
<sup>3</sup> https://www.atlassian.com/git/tutorials/undoing-changes/    
