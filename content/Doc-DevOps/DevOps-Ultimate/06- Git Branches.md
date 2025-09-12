**Create a new branch**
`git branch sarah`
**Switch to an existing branch**
`git checkout sarah`
**Create a new branch and Switch to it**
`git checkout -b max`
**Delete a branch**
`git branch -d max`
**List all branches**
`git branch`
### Practice
**what is a branch in git?**
pointer to a specific commit in git
**What is the default branch of a git repository?**
master
**Sarah has been working on a git repo at ==/home/sarah/story-blog== and has written a short story. Check git log command output in that directory to see the activity.
What's the name of the file created by Sarah?**
`cd /home/sarah/story-blog; git log --name-only`
```shell
-sarah (master)$ ls
lion-and-mouse.txt
-sarah (master)$ git log
commit 69a1c6ac0dcdd6a384487726e83e708d77d90313 (HEAD -> master)
Author: sarah <sarah@example.com>
Date:   Mon Jan 6 13:02:42 2025 +0000

    Added the lion and mouse story
-sarah (master)$ git log --name-only
commit 69a1c6ac0dcdd6a384487726e83e708d77d90313 (HEAD -> master)
Author: sarah <sarah@example.com>
Date:   Mon Jan 6 13:02:42 2025 +0000

    Added the lion and mouse story

lion-and-mouse.txt
```
**Sarah decides to write a new story . The Frogs and Ox . Let's create and checkout a new branch named story/frogs-and-ox**
`git checkout -b story/frogs-and-ox`
==As you can see the HEAD always points to the last commit on the currently checked-out branch.==
**Max informs Sarah that in her first story there's a typo in the title and needs to be fixed ASAP!
We must go back and fix the story in the master branch. But before we do that, let's commit the new story we have written so far. We don't want to carry our incomplete story to the master branch.
Stage and commit the story with the message Add incomplete frogs-and-ox story**
`git add frogs-and-ox.txt; git commit -am 'Add incomplete frogs-and-ox story'`
**Let's fix the typo in the lion-and-mouse.txt file. LION  is mis-spelt as LIOON. Please fix it and then commit the changes.
Commit message: Fix typo in story title**
Use vi editor to edit the file and fix the typo. Then run the command 
`git commit -am 'Fix typo in story title'` 
vi lion-and-mouse.txt
==**i** ; - text corrected - ; **esc** ; **:wq** -guardar y salir-==

**Looking at the commit history, try to guess what branch was the feature/signout branch created from?
Checkout branch feature/signout and then use the command git log --graph --decorate to see previous commit history along with the branch they were committed on.**
`git checkout feature/signout; git log --graph --decorate`

# GIT Merging branches

**fast-forward** > no crea un commit nuevo con los cambios
**non-fast-forward** > sí crea un commit nuevo con los cambios.
### Practice
**Let's proceed with where we left off in the previous lab. Sarah's local repository should be available at /home/sarah/story-blog**
**How many stories are currently available in the master branch?**
`cd /home/sarah/story-blog; git checkout master` and then list the files `ls`

Correct! First sarah committed the  Lion and Mouse 🐭 story in the master branch and then created a new branch for the Frogs and ox story, then went back and fixed typo in the Lion and Mouse 🐭 story and then went back and finished the Frogs and ox  story.
Next we will merge the new story into the master to have all stories in the master branch.
**While in the master branch merge the story/frogs-and-ox branch. If prompted for a commit message leave it to the default and quit the editor.**
`git checkout master`
`git merge story/frogs-and-ox`
`:wq`
`git log`  it should show commit message as **Merge branch 'story/frogs-and-ox**

**Git merged all the commits we made in the story/frogs-and-ox branch to the master branch. But since we made an additional commit on the master (fixing the typo from LIOON to LION) git created a new merge commit**

