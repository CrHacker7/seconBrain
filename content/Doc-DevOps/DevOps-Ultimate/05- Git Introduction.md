#### Práctica
Let's install GIT. First, identify the operating system this lab environment is built on.
`cat /etc/*release*`
Install GIT on the system.
`sudo apt update and then sudo apt install git -y`
We will learn more about working with different git commands throughout this course. But let's see if you can figure some stuff out using the git help command.
What's the git command used to Show various types of objects?
`git help`
```
examine the history and state (see also: git help revisions)
   bisect     Use binary search to find the commit that introduced a bug
   grep       Print lines matching a pattern
   log        Show commit logs
   show       Show various types of objects
```
What's the git command used to List, create, or delete branches?
`git help`
```grow, mark and tweak your common history
   branch     List, create, or delete branches
   checkout   Switch branches or restore working tree files
   commit     Record changes to the repository
   diff       Show changes between commits, commit and working tree, etc
   merge      Join two or more development histories together
   rebase     Reapply commits on top of another base tip
   tag        Create, list, delete or verify a tag object signed with GPG
   ```
What's the git command used to Download objects from another repository?
```
git help
collaborate (see also: git help workflows)
   fetch      Download objects and refs from another repository
   pull       Fetch from and integrate with another repository or a local branch
   push       Update remote refs along with associated objects
   ```
   What's the git command used to start a working area?
   ```start a working area (see also: git help tutorial)
   clone      Clone a repository into a new directory
   init       Create an empty Git repository or reinitialize an existing one
   ```
   You may view additional help on each command following the syntax git help <command>. For this you must first install git man pages using the command ==sudo apt-get install git-man==
Once done use the help of the init command with the command git help init and identify the option used to create a bare repository.
We will learn about initializing a repository later in this course.
```
-sudo apt-get install git-man
-git help init
SYNOPSIS
       git init [-q | --quiet] [--bare] [--template=<template_directory>]
                 [--separate-git-dir <git dir>]
                 [--shared[=<permissions>]] [directory]
```
##### Initialize a GIT Repository
**Práctica**
Let’s add a file to our project inside /home/sarah/story-blog
File name: lion-and-mouse.txt
File content: A Lion lay asleep in the forest
```
sarah $ touch lion-and-mouse.txt
sarah $ A Lion lay asleep in the forest > lion-and-mouse.txt
bash: A: command not found
sarah $ echo "A Lion lay asleep in the forest" > lion-and-mouse.txt
sarah $ cat lion-and-mouse.txt 
A Lion lay asleep in the forest
sarah $ 

**best option**
touch lion-and-mouse.txt; echo "A Lion lay asleep in the forest" >> lion-and-mouse.txt
```
It is good that the file is untracked. But it is still under GIT's radar. If you run the "git add ." command accidentally git will start to track this file.
Let's configure git to ignore this file permanently.
`echo notes.txt >> .gitignore`

You are asked to commit the README.md file with the commit message Add instructions for verification and the js/theme.js file with the message Increase time from 400 to 500
Note that the README.md file is already staged. So you just have to commit it. The file js/theme.js is to be committed as part of another commit.
```
Since README.md is already staged, commit it using the command git commit -m "Add instructions for verification". Then add and commit the js/theme.js file using git commit -am "Increase time from 400 to 500"
```
Sarah has written a story lion-and-mouse.txt under /home/sarah/story-blog/. Please commit it to local git repo
Add commit message: Added the lion and mouse story
`git add .;git commit -m "Added the lion and mouse story"`
You can list the changed files as well using the --name-only option with the git log command
Run the command git log --name-only
```
sarah (master)$ git log --name-only
commit 41e5b89962e16fc802b440a4d2b7856d99dcaae2 (HEAD -> master)
Author: sarah <sarah@example.com>
Date:   Sat Dec 21 20:21:21 2024 +0000

    Added the lion and mouse story

lion-and-mouse.txt
```
Another user has committed a new file to the repository now. Identify the user and the new file that was added.
Commit message: Added a new story.
Use the --name-only option to view the files as well
```
sarah (master)$ git log --name-only
commit 23885600418819a7f457c8621444f42e2ef3e942 (HEAD -> master)
Author: tom <tom@example.com>
Date:   Sat Dec 21 20:23:57 2024 +0000

    Added a new story

frogs-and-ox.txt

commit 41e5b89962e16fc802b440a4d2b7856d99dcaae2
Author: sarah <sarah@example.com>
Date:   Sat Dec 21 20:21:21 2024 +0000

    Added the lion and mouse story

lion-and-mouse.txt
```
Who made lasta commit
```
git log --max-count=3
commit 8ed6ea2dab16b3e065fc653a2b6cb86b62c19f1b (HEAD -> master)
Author: tej <tej@example.com>
Date:   Sat Dec 21 20:18:32 2024 +0000

    Update color from red to green

commit a4103aa6c0d7044ddb0f71a2d1007cbe6af8a893
Author: sarah <sarah@example.com>
Date:   Sat Dec 21 20:18:32 2024 +0000

    Add instructions to verify application

commit c77ff5b1b66f2e25e01aef1148277a3dd3d340da
Author: max <max@example.com>
Date:   Sat Dec 21 20:18:32 2024 +0000

    Increase interval time to 500
sarah (master)$ git log --max-count=1
commit 8ed6ea2dab16b3e065fc653a2b6cb86b62c19f1b (HEAD -> master)
Author: tej <tej@example.com>
Date:   Sat Dec 21 20:18:32 2024 +0000

    Update color from red to green
sarah (master)$ git log -n 1
commit 8ed6ea2dab16b3e065fc653a2b6cb86b62c19f1b (HEAD -> master)
Author: tej <tej@example.com>
Date:   Sat Dec 21 20:18:32 2024 +0000

    Update color from red to green
```
Judging by their actions, can you guess who may be the javascript developer in the team?
Look at the logs and identify the person who made changes to the .js file recently. You have already learned the option to display files associated with a commit
`git log --name-only`
```
sarah (master)$ git log --name-only 
commit 8ed6ea2dab16b3e065fc653a2b6cb86b62c19f1b (HEAD -> master)
Author: tej <tej@example.com>
Date:   Sat Dec 21 20:18:32 2024 +0000

    Update color from red to green

css/style.css

commit c77ff5b1b66f2e25e01aef1148277a3dd3d340da
Author: max <max@example.com>
Date:   Sat Dec 21 20:18:32 2024 +0000

    Increase interval time to 500

js/theme.js
```
