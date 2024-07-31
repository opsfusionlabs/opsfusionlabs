### Local Repo 

- Setting up a local repository
  
	```
	mkdir opsfusionlabs
	cd opsfusionlabs  
	```

	```
	git init
	```

Output: 

```
root@OpsFusionLabs-Git-Server:~/opsfusionlabs# git init
hint: Using 'master' as the name for the initial branch. This default branch name
hint: is subject to change. To configure the initial branch name to use in all
hint: of your new repositories, which will suppress this warning, call:
hint:
hint:   git config --global init.defaultBranch <name>
hint:
hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
hint: 'development'. The just-created branch can be renamed via this command:
hint:
hint:   git branch -m <name>
Initialized empty Git repository in /root/opsfusionlabs/.git/

```

### Git Status 

- Git-status is used to understand what stage the files in a repository are at.

	```
	git status 
	```

Output: 

```
root@OpsFusionLabs-Git-Server:~/opsfusionlabs# git status
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)

```

-  Create new file `app.py`

	```
	touch app.py 
	```
	
Output : 

```
root@OpsFusionLabs-Git-Server:~/opsfusionlabs# touch app.py
root@OpsFusionLabs-Git-Server:~/opsfusionlabs#
root@OpsFusionLabs-Git-Server:~/opsfusionlabs# ll
total 12
drwxr-xr-x 3 root root 4096 Jul 30 23:03 ./
drwx------ 6 root root 4096 Jul 30 22:59 ../
drwxr-xr-x 7 root root 4096 Jul 30 23:02 .git/
-rw-r--r-- 1 root root    0 Jul 30 23:03 app.py
root@OpsFusionLabs-Git-Server:~/opsfusionlabs# git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        app.py

nothing added to commit but untracked files present (use "git add" to track)

```

### Git add 

- The `git add` command adds a change in the working directory to the staging area

	```
	git add app.py 
	```

Output :

```
root@OpsFusionLabs-Git-Server:~/opsfusionlabs# git add app.py
root@OpsFusionLabs-Git-Server:~/opsfusionlabs# git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   app.py

root@OpsFusionLabs-Git-Server:~/opsfusionlabs#

```
### Git commit 

- `git commit` creates a commit, which is like a snapshot of your repository. These commits are snapshots of your entire repository at specific times.
  
	```
	git commit -m "commit message" <file name>
	```

Output: 

```
root@OpsFusionLabs-Git-Server:~/opsfusionlabs# git commit -m "app.py file added" .
[master (root-commit) e473881] app.py file added
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 app.py

```

### Git log
* Check the Commit messages 
  
	```
	git log 
	```

Output: 

```
root@OpsFusionLabs-Git-Server:~/opsfusionlabs# git log
commit e4738815ddb62b57d65e0bf560894f401416dd4e (HEAD -> master)
Author: opsfusionlabs <opsfusionlabs@gmail.com>
Date:   Tue Jul 30 23:12:23 2024 +0530

    app.py file added
root@OpsFusionLabs-Git-Server:~/opsfusionlabs#

```



### 

Un stage a file while retaining the changes in working directory

```
git reset [file]
```

