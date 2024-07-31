###  Repository

#### Initializing a Repository in an Existing Directory

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

#### Cloning an Existing Repository

* You clone a repository with `git clone <url>`  
  
	```
	git clone <repository url> 
	```

## Tracking New Files
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

	```
	// stage all files in current directory
	git add .
	
	// stage single file
	git add <file>
	
	// stage multiple files
	git add <file1> <file2> <file3> ...
	```

Output: 

```
root@OpsFusionLabs-Git-Server:~/opsfusionlabs# git commit -m "app.py file added" .
[master (root-commit) e473881] app.py file added
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 app.py

```

### Git diff

1. Show difference between working directory and last commit.
	```
	git diff HEAD
	```

2. Show difference between staged changes and last commit
	```
	git diff --cached
	```
### Git log
1. Check the Commit messages 
  
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

- Limit number of commits by . E.g. ”git log -5” will limit to 5 commits.

	```
	git log -5
	```

- Condense each commit to a single line

	```
	git log --oneline
	```

- Display the full diff of each commit.
  
	```
	git log -p
	```

- Include which files were altered and the relative number of lines that were added or deleted from each of them

	```
	git log --stat
	```

- Search for commits by a particular author.

	```
	git log --author= "opsfusionlabs"
	```

- Search for commits with a commit message that matches `pattern` 

	```
	git log --grep="<pattern">
	```

- Only display commits that have the specified file.  

	```
	git log -- app.py
	```

- graph flag draws a text based graph of commits on left side of commit msgs. --decorate adds names of branches or tags of commits shown  
  
	```
	git log --graph --decorate
	```


### Undoing Changes

1. Create new commit that undoes all of the changes made in , then apply it to the current branch.

	```
	git revert <commit id>
	```

2. Remove from the staging area, but leave the working directory unchanged. This unstages a file without overwriting any changes.

	```
	git reset <file> 
	```

3. Shows which files would be removed from working directory. Use the -f flag in place of the -n flag to execute the clean.

	```
	git clean -n
	```

### Git reset 

1. Reset staging area to match most recent commit, but leave the working directory unchanged.

	```
	git reset
	```

2. Reset staging area and working directory to match most recent commit and overwrites all changes in the working directory.

	```
	git reset --hard
	```

3. Move the current branch tip backward to , reset the staging area to match, but leave the working directory alone.

	```
	git reset <commitId>
	```

4. Same as previous, but resets both the staging area & working directory to match. Deletes uncommitted changes, and all commits after.

	```
	git reset --hard <commitid>
	```

### Rewriting git history

1. Replace the last commit with the staged changes and last commit combined. Use with nothing staged to edit the last commit’s message.

	```
	git commit --amend <commit id> 
	```


2. Rebase the current branch onto . can be a commit ID, branch name, a tag, or a relative reference to HEAD.

	```
	git rebase <rebase>
	```


3. Show a log of changes to the local repository’s HEAD. Add --relative-date flag to show date info or --all to show all refs.

	```
	git reflog 
	```