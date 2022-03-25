---
title: 'GIT:delete history commit.'
date: 2022-03-25 23:40:23
tags: 'git skills'
---

Follow the below steps to complete this task.
[Warning]: This will remove your old commit history completely, You can’t recover it again.

1，Create Orphan Branch – Create a new orphan branch in git repository. The newly created branch will not show in ‘git branch’ command.
git checkout --orphan temp_branch

2，Add Files to Branch – Now add all files to newly created branch and commit them using following commands.
git add -A
git commit -am "the first commit"

3，Delete master Branch – Now you can delete the master branch from your git repository.
git branch -D master

4，Rename Current Branch – After deleting the master branch, let’s rename newly created branch name to master.
git branch -m master

5，Push Changes – You have completed the changes to your local git repository. Finally, push your changes to remote (Github) repository forcefully.
git push -f origin master
