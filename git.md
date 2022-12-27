# Hints for GIT

This command will sync the local repository with the remote repository getting rid of every change you have made on your local\
`git reset --hard origin/<branch>`

Check outcoming commits (that haven't been pushed) at your local branch\
`git log @{u}..`


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


First `push` from a local branch to a remote branch that has not been set up\
`` git push --set-upstream origin `git branch --show-current` ``

