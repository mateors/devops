# Git practice

## Branch Create, Merge and Resolve Conflicts
> `mkdir webapp`\
> `git init`

> `cd webapp`\
> `touch main.go`\
> `touch test.go`\

> `touch config.ini`


> `git add .`\
> `git commit -m "initial commit"`

> `git branch branch1`\
> `git branch`\
> `git checkout branch1`\
> `ls -ltr`\
> `git branch`\
> `touch about.txt`\
> `tohch contact.txt`

> `git add .`\
> `git commit -m "initial commit in branch1"`\

> `ls -ltr`\
> `pwd`

> `git checkout master`\
> `ls -ltr`

> `git checkout -b branch2`\
> `git branch`

> `git checkout master`\
> `ls -ltr`

> `git branch`\
> `git merge branch1`

> `git branch -d branch1`\
> `git branch`

## Unmerged branch deletion
> `git branch -D branch2`

## Merge Conflicts
> `nano main.go`\
> add some content and save

> `git add .`\
> `git commit -m "code added"`

> `git branch branch1`\
> `git checkout branch1`

> `ls -ltr`

> `nano main.go`\
> add one line

> `git add .`\
> `git commit -m "change done in branch1"`

> `git checkout master`
 
> `nano main.go`\
> modify last edited text

> `git add .`\
> `git commit -m "code modified"`

> `git merge branch1`

> `git config --global merge.tool kdiff3`

> `git mergetool`

## Show the merged branches
> `git branch --merged`