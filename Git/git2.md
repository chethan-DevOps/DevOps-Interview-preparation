
Git reset is a powerful command used to move the current branch (HEAD) to a specific commit, and optionally update the staging area (index) and working directory.

To understand the flags --soft, --mixed, and --hard, you need to know the three layers Git manages:

HEAD → the last commit (current branch pointer)
Index (staging area) → what will go into the next commit
Working directory → your actual files on disk
🔹 git reset --soft <commit>

What it does:

Moves HEAD to the given commit
Keeps index and working directory unchanged

Effect:

All changes from the reset commits stay staged

Use case:
You want to undo commits but keep everything ready to recommit

Example:

git reset --soft HEAD~1

Undo last commit, but keep changes staged.

🔹 git reset --mixed <commit> (default)

What it does:

Moves HEAD
Resets index (unstages changes)
Keeps working directory unchanged

Effect:

Changes become unstaged, but still exist in files

Use case:
You want to undo commits and rework changes before staging again

Example:

git reset HEAD~1

(same as --mixed)
Undo last commit and unstage changes.

🔹 git reset --hard <commit>

What it does:

Moves HEAD
Resets index
Resets working directory

Effect:

Everything after that commit is deleted (staged + unstaged changes lost)

Use case:
You want to completely discard changes

Example:

git reset --hard HEAD~1

Undo last commit and delete all associated changes permanently.

⚠️ Important Warning
--hard is destructive — changes are difficult (sometimes impossible) to recover.
Avoid using it on shared branches unless you know what you're doing.

| Flag      | HEAD | Index (Staging) | Working Directory |
| --------- | ---- | --------------- | ----------------- |
| `--soft`  | ✅    | ❌ unchanged     | ❌ unchanged       |
| `--mixed` | ✅    | 🔄 reset        | ❌ unchanged       |
| `--hard`  | ✅    | 🔄 reset        | 🔄 reset          |


🧩 Simple Mental Model
--soft → “Undo commit, keep everything staged”
--mixed → “Undo commit, keep changes but unstaged”
--hard → “Undo everything and wipe it out”


**--->** if we want to commit the new changes to the latest commit(HEAD) without creating the new commit then enter the command - "git commit --amend" , it will open the commit details of latest commit so modify the commit message accordingly and save. This will make the new changes commiting into the latest commit without the need of creating new commit.

**--->** Let's say we have 6 features committed already. If we want to squash the 5th and 6th feature commits into one commit then we need to enter the command - "git rebase -i commitid(previous commit of feature 5&6- means we need to give 4th feature commit id here)". It will open the text file with features 5&6 with pick flag so change the feature 6 with squash flag and modify the commit message accordingly So it will squash the 5th and 6th feature by rewriting the history.

**--->** To reorder the commits in the history, we can use the same command - git rebase -i commitid(just the before commit present to the 2 commits for which we want to reorder them). It will open the text file with commit messages(with pick flag) where we can just reorder the messages accordingly. 

**--->** To drop or delete the commit, we can use the same command - git rebase -i commitid(just the before commit present to the particular commit for which we want to drop/delete). It will open the text file with commit messages(with pick flag) where we have to change pick to drop or d to delete that commit by rewriting the history.

**--->** To execute some shell commands in between the commit, we can use the same command - git rebase -i commitid(just the before commit present to the particular commit where we want to execute shell commands). It will open the text file with commit messages(with pick flag) where we have to enter to new line and use exec flag for the shell commands to be executed.

git bisect : git bisect is a debugging tool that helps you find the exact commit that introduced a bug by using a binary search over your commit history.

Instead of checking commits one by one, it quickly narrows it down in O(log n) steps.

🧠 Core Idea

You tell Git:

a bad commit (where the bug exists)
a good commit (where the bug did not exist)

Git then:

Picks a commit in the middle
Asks you: is this good or bad?
Repeats until it finds the exact commit that introduced the issue

🔹 Basic Workflow
1. Start bisect
git bisect start
2. Mark the current (buggy) commit
git bisect bad
3. Mark a known good commit
git bisect good <commit-hash>

Now Git checks out a commit halfway between them.

4. Test and mark

At each step:

If the bug is present:

git bisect bad

If the bug is NOT present:

git bisect good

Git keeps narrowing down.

5. Result

Eventually, Git outputs something like:
coomit-hash is the first bad commit

6. Exit bisect
git bisect reset

Returns you to your original branch.

⚡ Example Scenario
HEAD → app is broken
Commit abc123 → app worked fine
git bisect start
git bisect bad
git bisect good abc123

Git picks a middle commit → you test → mark good/bad → repeat.

🔹 Automating with a Script

If you can test automatically (e.g., via tests), you can speed things up:

git bisect run ./test.sh
Script returns 0 → good
Non-zero → bad

Git will automatically run the binary search.

🔹 Useful Commands

Skip a commit (if it can’t be tested):

git bisect skip

See progress:

git bisect log
🧩 Mental Model

Think of it like guessing a number between 1 and 100:

Instead of checking each number,
You always pick the middle → cut the search space in half each time.
⚠️ Tips
Choose a reliable good commit — otherwise results may be wrong
Make sure your test for “good vs bad” is consistent
Avoid changing files manually during bisect
🆚 When to Use It

Use git bisect when:

A bug appeared somewhere in history
You don’t know which commit caused it
There are too many commits to check manually
