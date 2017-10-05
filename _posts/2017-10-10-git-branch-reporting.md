---
layout: post
title:  ""
date: 2017-01-26 22:40:56
categories: tests coding
published: true
---


Sometimes our GitHub might get messy if we do not maintain it properly. 
The problem I had was that many branches that has been merged were not closed properly and have been getting older and staler everyday.
So I wrote this program to identify which branches are behind the master and can be removed.

```
#!/bin/sh
  
branches=$(git branch -r)
log_file="path/to/log/file" 

for bName in $branches:
do
  echo "$bName"
  echo "$(git rev-parse remotes/$bName)"
#  echo "$(git diff origin/master...$bName)"
if [[ -z "$(git diff origin/master...$bName)" ]]
then
  echo "to be deleted"
else
#  echo "$(git diff origin/master...$bName)"
  echo "changes not merged"
fi
  echo "**************************"
done&>$log_file
  
```

What this program does is:
1. Lists all branches in the repo
2. Gets each branch hash (for the future if we need to open the branch back)
3. Compares each branch with master
4. Prints if the the branch can be deleted or if something was not merged yet. 
5. Log of the activity is written to a file (file creation was not included in this program)


This can be easily enhanced with adding deletion functionality.
```
git branch -d origin/master...$bName
```
