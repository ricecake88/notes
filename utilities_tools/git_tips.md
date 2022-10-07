# Checkout file from another branch while in another branch
`git checkout <source_branch> <relative_path_to_file>`


# Squash Commits

Select what you want to squash
`git rebase -i master`

Select to squash your last three commits into one
Example:
`git rebase -i HEAD~3`