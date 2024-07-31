# Git Branching

Git branches are effectively a pointer to a snapshot of your changes. When you want to add a new feature or fix a bug—no matter how big or how small—you spawn a new branch to encapsulate your changes. This makes it harder for unstable code to get merged into the main code base, and it gives you the chance to clean up your future's history before merging it into the main branch.

### Git Branch 

1. List all of the branches in your repository.
	```
	git branch --list 
	```

2. Create a new branch 
   
	```
	git branch ofl-dev
	```

3. Delete the specified branch
   
	```
	git branch -d ofl-dev
	```

4. Force delete the specified branch, even if it has unmerged changes.
   
	```
	git branch -D ofl-dev
	```

5. List all remote branches.

	```
	git branch -a 
	```


6. List all remote branches 
   
	```
	git branch -r 
	```
## Git Checkout 

7. Create and Switching to new branches
   
	```
	git checkout -b ofl-test
	```

8. Switching branches

	```
	git checkout master
	```

9. Checkout a remote branch
	```
	git fetch --all
	```

	```
	git checkout ＜remotebranch＞
	```

