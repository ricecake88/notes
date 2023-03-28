[[#Checkout file from another branch while in another branch]]
[[#Squash Commits]]
[[#Revert Commit]]
[[#Revert the last merge commit]]
[[#Deleting Branch]]

## Checkout file from another branch while in another branch
`git checkout <source_branch> <relative_path_to_file>`

### Other Methods:
https://www.freecodecamp.org/news/git-checkout-file-from-another-branch/

## Squash Commits

Select what you want to squash
`git rebase -i master`

Select to squash your last three commits into one
Example:
`git rebase -i HEAD~3`

Examples:
https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/How-to-squash-git-commits-example

## Revert Commit
Reference:
https://dev.to/lofiandcode/git-and-github-how-to-revert-a-single-file-dha

The <commit_id> being the commit that you want to revert.
```bash
git checkout <commit_id> <path_of_file>
```
Make sure to commit the change
```shell
git commit -m "<Enter message>"
```
Or reverting to the last commit:
```shell
git checkout <commit_id>~1 <path_of_file>
```

## Remove the last commit from remote

This is great for my stupid printouts. Note that the change still remains in local but it will be seen as a change locally.

```bash
git reset HEAD^ # remove commit locally
git push origin +HEAD # force-push the new HEAD commit
```
https://stackoverflow.com/questions/8225125/remove-last-commit-from-remote-git-repository

## Revert the last merge commit
Reference to reversing merged commits:
https://brianasz.com/2019/11/26/Reverting_merged_commits.html

```shell
git revert --no-commit <SHA_ID_of_commit> -m 1
```

## Deleting Branch
### remote
`git push -d <remote_name> <branchname>
or 
`git branch -d <branchname>`

where `<remote_name>` usually means `origin`.

### local
`git branch -d <branch_name>` or `git branch -D <branch_name>`

## Removing a Git Commit Which Has Not Yet Been Pushed
Reference:
https://stackoverflow.com/questions/1611215/remove-a-git-commit-which-has-not-been-pushed

`git reset HEAD~1`

## Removing a Git Commit In Which I have pushed to Remote
Reference:
https://stackoverflow.com/questions/1611215/remove-a-git-commit-which-has-not-been-pushed

`git revert HEAD`
