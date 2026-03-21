1. **yum install git -y** ----> To install git in linux machine
2. **git init .** ---->  To initialize empty repo in current directory
3. **git config user.name "username"** ---->  used to configure user name for our git
4. **git config user.email "email"** ---->  used to configure user email for our git
5. **git add filename** ---->  used to track a file
6. **git add *** ---->  used to track all files & folders
7. **git add .** ---->  used to track all files in current directory (including hidden files)
8. **git add -f** ---->  filename to track files forcefully
9. **git rm --cached** ---->  filename used to untack the file
10. **git commit -m "message"** ---->  filename to commit a file
11. **git commit -m "message" .** ---->  used to commit all files which are present in staging area
12. **git log** ---->  used to see the history of the git
13. **git log --oneline** ---->  used to see only commit ID's and messages
14. **git log -1** ---->  used to see the latest commit
15. **git log -3** ----> used to see latest 3 commits
16. **git log --follow --all** ---->  filename used to see the no of commits for a single file
17. **git show commit_id --name-only** ---->  used to see all the commit details along with the file name
18. **git show commit_id --stat** ---->  see the histroy of a file (modifications, data add and deletion)
19. **git commit --amend -m "message"** ---->  used to change the commit message for a latest commit
20. **git commit --amend --author "username <mail>"** ---->  used to change the author of latest commit
21. **git commit --amend --no-edit** ---->  used to commit the changes with previous commit
22. **git reset --hard HEAD~1** ---->  used to delete the latest commit along with the changes
23. **git reset --hard HEAD~3** ----> used to delete the latest 3 commits along with the changes
24. **git resert --soft HEAD~1** ---->  used to delete only commits but not actions/changes
25. **git resert --soft HEAD~3** ---->  used to delete only latest commits but not actions/changes
26. **git revert commit_id** ---->  used to delete a particular commit action and add a new commit for the change
27. **git branch** ---->  used to see the list of branches
28. **git branch branch-name** ---->  to create a branch
29. **git checkout branch-name** ---->  to switch one branch to another
30. **git checkout -b branch-name** ---->  used to create and switch a branch at a time
31. **git branch -m old-branch new-branch** ---->  used to rename a branch
32. **git branch -d branch-name** ---->  to delete a branch
33. **git branch -D branch-name** ---->  to delete a branch forcefully
34. **git merge branch** ---->  copy the all commits from one branch to another
35. **git cherry-pick commit-id** ----> copy the single commits from one branch to another
36. **git merge --abort** ---->  used to cancle the merge when conflicts arise
37. **git rebase branch** ---->  copy the all commits from one branch to another
38. **git diff** ----> to compare the differences present in untracked changes(local) with respect to the changes which are already present in github repo(remote)
39. **git diff --staged** ----> to compare the differences present in tracked changes(local) with respect to the changes which are already present in github repo(remote)
40. **git diff -w** ----> to compare the differences present in untracked changes(local) with respect to the changes which are already present in github repo(remote) by ignoring the white spaces while comparing
41. **git diff --no-index** ----> to compare the differences between 2 different files
42. **git diff commithash-1 commithash2** ----> to compare the differences between 2 commits
43. **git diff --word-diff** ----> to identify the word by word or char by char differences which are modified or changed
44. **git diff --stat** ----> to get overall status of changes/differences of the files
45. **git stash** ---->  to delete the changes permanently
46. **git stash save "message"** ---->  to save the stash along with the message
47. **git stash apply** ---->   to get back the data again ---> it will pop the change back but the status will not be empty
48. **git stash list** ---->  to get the list of stashes
49. **git stash show stash@{2}** ----> to see the files or details that this particular stash contains
50. **git stash clear** ---->  to clear all stashes
51. **git stash pop** ---->  to delete the first stash -->then the stash will become empty if no other stashes present 
52. **git stash drop** ---->  used to delete the latest stash
53. **git stash drop stash@{2}** ---->  used to delete a praticular stash
54. **git remote add origin repo-url** ---->  link local-repo to central-repo
used to get the linked repo in github
55. **git push -u origin branch-name** ---->  push the code from local to central
56. **git push -u origin branch-1 branch-2** ---->  used to push the code to multiple branches
57. **git push -u origin --all** ---->  used to push the code to all branches at a time
58. **git clone repo-url** ---->  used to get the code from central to local
59. **git pull origin branch** ---->  used to get the chanes from central to local
60. **git fetch branch-name** ---->  used to fetch the data from central to local
61. **git fetch --all** ---->  used to fetch the changes from all branches in github
62. **git merge origin/branch** ---->  used to merge the changes from cental to local
63. **git push -u origin --delete branch-name** ---->  used to delete the github branch from local
64. **git remote rm origin** ---->  used to unlink the github-repo
65. **Git remote rename old -link new-link** ---->  used to change the rep
