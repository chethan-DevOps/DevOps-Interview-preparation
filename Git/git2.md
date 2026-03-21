
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

