# Hints for GIT

This command will sync the local repository with the remote repository getting rid of every change you have made on your local\
`git reset --hard origin/<branch>`

Check outcoming commits (that haven't been pushed) at your local branch\
`git log @{u}..`


### rebase

Whenever 'rebase' doesn't go smooth\
`git rebase --abort`\
`rm -fr .git/rebase-merge`

Ignore changes on local files\
`git update-index --assume-unchanged ../custom/bestsecretcore/buildPrototype.sh`\
`git update-index --assume-unchanged ../idea-module-files/e2e-tests.iml`

Rebase feature branch (get the latest updates from master and get feature branch commits on top of master's head)\
`git checkout feature`\
`git rebase master`

This will apply the commits from the master branch onto your feature branch, updating your feature branch to reflect the changes from the master branch.

Once you have completed the rebase operation, you can push the updated feature branch to the remote repository using the git push command. This will update the feature branch on the remote repository with the changes you have made locally.
If 'push' is not working (eg. "Updates were rejected because the tip of your current branch is behind") [then](https://sakhawat-ali.medium.com/git-resolving-conflict-while-git-rebase-33b70ddb528e)
`git push -f origin feature` or `git push --force-with-lease`


First `push` from a local branch to a remote branch that has not been set up\
`` git push --set-upstream origin `git branch --show-current` ``

### undo file change
`git checkout -- <path_to_file>`

### log of local changes to be pushed
shows commits
```
git log origin/master..HEAD
```
shows file changes
```
git diff origin/master..HEAD
```

Show all commits that you have locally but not upstream with:
```
git log @{u}..
```

Undo the recent commit
```
git reset --soft HEAD^
```

### forget about a file
* remove file from tracking history. To stop tracking a file, we must remove it from the index
  
`git rm --cached <file>`  
`git rm -r --cached <folder>`  

* make git forgetting about the file changes  
`git update-index --assume-unchanged <file>`

### change the commit author for the last commit

```
git config --local user.name "Mama Papa"

git config --local user.email mamapapa@email.com

git commit --amend --reset-author --no-edit
```




