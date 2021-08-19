## Git and Github crash course


### What is Git?

Git is the most widely used modern version control system. Git is an actively maintained open source project originally developed in 2005 by Linus Torvalds, the famous creator of the Linux operating system kernel. A staggering number of software projects rely on Git for version control, including commercial projects as well as open-source.

### What is Github?

**GitHub** is a code hosting platform for version control and collaboration. It lets you and others work together on projects from anywhere.

## Git Installation

Go to [git](https://git-scm.com/downloads) and download git for your OS.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627311857542/McrDp-wyY.png)

## Git Setup

Run these commands and your git will be ready to use the useful commands.

```
git config --global user.name "John Doe" # your name
git config --global user.email johndoe@example.com # your email
```


## Important Git commands

### Git init

This initializes an empty repository locally on your computer.

```
git init
```


### Git add

This commands adds the files to the staging area and you can, later on, commit it with a message.

```
git add . # to add all files
git add filename # add a specific file
```


### Git status

This command shows which files have been added to the staging area and which files are left to be added.

```
git status
```


### Git commit

This command commits the file changes and helps you keep track of your code. Whenever you add a new feature you can commit it with a message.

```
git commit -m "this is the message"
```


### Git push

This command is used to push your code to a git provider like github, bitbucket.

```
git push
```


### **Git clone**

If you want to copy a whole repository on your machine do the following -

Go to the repo you want to copy then click on the code button and it will show you a dropdown.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627311859216/oXwBhsKls.png)

Just copy this and run the command

```
git clone [https://github.com/avneesh0612/Firebase-auth-demo.git](https://github.com/avneesh0612/Firebase-auth-demo.git)     # enter the url you have got
```


## Git branch

***Firstly, what is a git branch?***

A git branch represents an independent line of development. Branches serve as an abstraction for the edit/stage/commit process. You can think of them as a way to request a brand new working directory, staging area, and project history.

**Create a branch**

```
git branch branchname
```


**Check all branches**

```
git branch
```


**Switch branches**

```
git checkout branchname
```


**Merge a branch**

```
git merge branchname
```


## How to connect a repository to Github

Go to [Github](https://github.com/) and signup for an account.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627311860984/OYTpKg6Df.png)

Click on new and it will redirect you to this page.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627311862849/RnfZNhoPY.png)

Name your repository anything you like then if you want to have a description you can add it. It is not compulsory.

You can choose it to be private or public and leave the rest as default. When you click on create repository. You will reach a page similar to this-

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627311864658/nh51c3bLq.png)

Now just copy the commands and paste them into the terminal.

After doing this you can simply add, commit and push the files.

```
git add .
git commit -m "completed"
git push
```


Congratulations, you have completed this crash course on git and GitHub 🥳.

If you want to contribute to an open-source project check [this](https://medium.com/weekly-webtips/how-to-contribute-to-an-open-source-project-and-make-a-pr-cc92f6c9831d).

Social links

[Instagram](http://instagram.com/avneesh__agarwal)

[LinkedIn](https://www.linkedin.com/in/avneesh-agarwal-78312b20a/)

[Github](https://github.com/avneesh0612)

[Linktree](https://linktr.ee/avneesh_agarwal)