#### Fetching and Pulling
`git fetch origin master`
`git merge origin/master`

**Two commands in one line (fetch and merge)**
- `git pull origin master` 
**Merge conflicts**
- `git add foutth_story.txt`
- `git merge`

## Practice
#### Remote repositories
**No one in Sarah's team has created a remote repository yet for this project. So Sarah decides to create one so everyone in the team can collaborate. Let's now create a remote git repository named story-blog in Gitea using the GUI. Click on Git Portal UI to access the UI, login with the below details and create a new repository.**

*UI login info:*
*Username: sarah*
*Password: Sarah_pass123*
*Repository Name: story-blog*
*Keep other options default when creating repo*

**From within the lab environment (terminal) the remote repository is accessible at the URL http://git.example.com. Let's now configure the remote repository for the local repository at /home/sarah/story-blog**
**Target remote repo URL : http://git.example.com/sarah/story-blog.git**
**remote repo alias : origin**

`cd /home/sarah/story-blog; git remote add origin http://git.example.com/sarah/story-blog.git`

*How many repositories has everyone?*
**In the top menu bar select *Explore* -> *Repositories* and count the number of repositories for each user.**
*How many users there are in the repository*
**In the top menu bar select *Explore* -> *Users* and count the number of repositories for each user.**

**First max needs to download the current code (stories) uploaded by Sarah in remote repository. As user Max, clone the remote repo to his home directory - /home/max**
**Target repo web URL : http://git.example.com/sarah/story-blog**
`cd /home/max; git clone http://git.example.com/sarah/story-blog.git`

**Unlike last time, let's not commit the story to the master branch directly. Instead commit it to a new branch named story/fox-and-grapes with the commit message 'Added fox-and-grapes story'**
`git checkout -b story/fox-and-grapes; git add .; git commit -m 'Added fox-and-grapes story'`

## Pull Request
**Max has pushed his story, but his story is still not in the Master branch. Let's create a Pull Request(PR) to merge Max's story/fox-and-grapes branch into the master branch**
UI login info:
- Username: max
- Password: Max_pass123
**PR title :** Added fox-and-grapes story
**PR pull from branch:** story/fox-and-grapes (source)
**PR merge into branch:** master (destination)
**Follow below steps to create PR**
***In Gitea:***
- Login to Git Portal UI with max user
- Go to the story-blog repository
- Click on Pull requests
- Click on New Pull request
- Put PR pull from branch: story/fox-and-grapes
- Put PR merge into branch: master
- Click on New Pull Request
- Add PR title as Added fox-and-grapes story
- Click on Create Pull request

**Before we can add our story to the master branch, it has to be reviewed first. So lets ask tom to review our PR by assigning him as a reviewer
Add tom as reviewer through the Git Portal UI**
***In Gitea:***
- Go to the newly created PR
- Click on Reviewers on the right
- Add tom as a reviewer to the PR

**Now lets review and approve the PR as user Tom
Login to the portal with the user tom
Logout of Git Portal UI if logged in as max
UI login info:
- Username: tom
- Password: Tom_pass123
PR title : Added fox-and-grapes story
***In Gitea:***
- Sign out of Git Portal UI as max user
- Login as tom user
- Go to *story-blog* repo and click on *Pull Requests*
- Click on the PR - *Added fox-and-grapes story*
- Click on *Files changed* tab and then the green drop down button *Review.* Add any approval message and click on the *Approve* button to approve the PR. You may need to scroll down to see the Approve button.

**Great stuff!! The story has been approved! . It's now time to Merge the Pull Request to make the story available in the master branch.
Login as user sarah and merge the PR.
- Username: sarah
- Password: Sarah_pass123**
***In Gitea:***
- Logout of tom user
- login with the user sarah
- Click on the *sarah/story-blog* repo
- Go to the *Pull Request*
- Select the *PR*
- Click on the green button *Merge Pull Request* and then confirm again by clicking on the green button *Merge Pull Request* to merge the PR
- PR status should be shown as *Merged*

> **To view all the branches- both local and remote ==*git branch -a*== command. The remote branches have the prefix remotes**

Now that we’ve fetched the origin master branch, we can update our local master branch with the latest changes made on origin/master branch.
Merge the remote master branch to local master

 `git merge <other-branch>`
 `git merge origin/master`
 
Max just pushed another story to remote. Let's retrieve that using the second approach. Use ==***git pull origin master***== to pull all remote changes.

## Merge Conflicts
**Let's now stage and commit the story-index.txt file.**
**Use the message - Add index of stories**
`git add story-index.txt; git commit -am 'Add index of stories'`

```groovy
//error sync origin and master
max (master)$ git push origin master
To http://git.example.com/sarah/story-blog.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'http://git.example.com/sarah/story-blog.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
```json
//error merge
max (master)$ git pull origin master
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 306 bytes | 306.00 KiB/s, done.
From http://git.example.com/sarah/story-blog
 * branch            master     -> FETCH_HEAD
   efe5909..f5c658d  master     -> origin/master
hint: Pulling without specifying how to reconcile divergent branches is
hint: discouraged. You can squelch this message by running one of the following
hint: commands sometime before your next pull:
hint: 
hint:   git config pull.rebase false  # merge (the default strategy)
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only
hint: 
hint: You can replace "git config" with "git config --global" to set a default
hint: preference for all repositories. You can also pass --rebase, --no-rebase,
hint: or --ff-only on the command line to override the configured default per
hint: invocation.
CONFLICT (add/add): Merge conflict in story-index.txt
Auto-merging story-index.txt
Automatic merge failed; fix conflicts and then commit the result.
```
**We are now in a merge conflict ! Looks like there was already a file named story-index.txt on remote. Someone beat us to it! Let's find out who.
Check the log of the origin/master branch to see what was the last commit on the repository and identify who added the story-index.txt file.**
`git log origin/master`
>Añade origin/master para más información

**Looks like Sarah already pushed a version of the file. When we pulled latest changes, git tried to merge Max's and Sarah's versions of the story-index.txt file, but was unsuccessful. However the local story-index.txt file is updated with changes from both Max and Sarah to allow you to merge manually.
Inspect the file (use vi editor or just cat story-index.txt) and select the most appropriate statement below. The first section shows Max's data and the second section shows Sarah's data.**
```json
max (master)$ cat story-index.txt
<<<<<<< HEAD .deleteThisLine
1. The Lion and the Mooose
2. The Frogs and the Ox
3. The Fox and the Grapes
4. The Donkey and the Dog
=======  #deleteThisLine
1. The Lion and the Mouse
2. The Frogs and the Ox
3. The Fox and the Grapes
>>>>>>> f5c658dc938446da71db5c7399ac64b95de7302a .deleteThisLine
#dejarLoQueQuieroSubir
```
**Now that we have made a change, we must now commit it.
Commit the current changes with the message - Resolved merge conflicts and merged story index**
```json
max (master)$ git commit -am 'Resolved merge conflicts and merged story index'
[master 968f244] Resolved merge conflicts and merged story index
```
**Now that we have merged the changes, everything's clean ✨ . We can now push the changes to remote.**
`git push origin master`

## Fork
**What are the privileges for user jon to the remote story-blog git repo on Gitea UI?
Login to git UI with sarah user and password: Sarah_pass123 and check jon's privileges.**
==***In Gitea:***==
Click on Git Portal UI to access git UI
Login with sarah user with password: Sarah_pass123
- Go to story-blog repo
- Click on Settings -> Collaborators and check permissions of jon user

**Login to Git Portal UI with the user jon and fork sarah's story-blog repo**
- **Username: jon**
- **Password: Jon_pass123**
**Note: Keep all options default when you fork the repo****
==***In Gitea:***==
Click on Git Portal UI to access git UI
Sign out sarah user and then login with jon user
UI login info:
- Username: jon
- Password: Jon_pass123
Go to story-blog repo and click on fork button.
Keep all options default and click on Fork Repository button.

**Click on the Explore button and view the list of repositories. There are now 2 separate repositories. One each in Sarah's and Jon's accounts
Next to the jon/story-blog repo you will see a small fork icon which indicates that its a forked repo.**


**Push the new changes to the master branch on jon's story-blog repo which was forked in previous step**
**commit message: Added fox-and-grapes story**
- **Username: jon**
- **Password: Jon_pass123**

`cd /home/jon/story-blog/;git add .; git commit -m 'Added fox-and-grapes story'; git push origin master`

**Raise a PR from jon's forked repo to sarah's repo.**
**Login to the Gitea UI as user Jon.**
**Username: jon**
- **Password: Jon_pass123**
**Merge into(destination) branch: sarah:master**
**Pull from(source) branch: jon:master**
**PR name: Added fox-and-grapes story****
==***In Gitea:***==
Go to sarah/story-blog or jon/story-blog repo.
Click on Pull requests -> New Pull Request
Select Merge into(destination) branch: sarah:master and Pull from(source) branch: jon:master
To raise a PR click on New Pull Request-> Create Pull Request while keeping other options default