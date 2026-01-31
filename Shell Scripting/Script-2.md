**Here are simple, correct bash scripts to count files and directories in a given path.**

**Count files and directories (non-recursive)**

#!/bin/bash

path="$1"

if [ -z "$path" ]; then
    echo "Usage: $0 /path/to/directory"
    exit 1
fi

if [ ! -d "$path" ]; then
    echo "Not a directory: $path"
    exit 1
fi

files=$(find "$path" -maxdepth 1 -type f | wc -l)
dirs=$(find "$path" -maxdepth 1 -type d | wc -l)

# subtract 1 to exclude the directory itself
dirs=$((dirs - 1))

echo "Path: $path"
echo "Files: $files"
echo "Directories: $dirs"

**Count files and directories (recursive)**

#!/bin/bash

path="$1"

if [ -z "$path" ]; then
    echo "Usage: $0 /path/to/directory"
    exit 1
fi

files=$(find "$path" -type f | wc -l)
dirs=$(find "$path" -type d | wc -l)

# exclude root directory itself
dirs=$((dirs - 1))

echo "Path: $path"
echo "Total files: $files"
echo "Total directories: $dirs"


**Here’s a clear, line-by-line explanation of the scripts I gave you for counting files and directories.**
**1️⃣ Non-recursive count (only the given directory)
Script**
path="$1"

Takes the first command-line argument as the directory path.

if [ -z "$path" ]; then
    echo "Usage: $0 /path/to/directory"
    exit 1
fi

Checks if no path was provided.
-z → string is empty.
Exits with error if missing.


if [ ! -d "$path" ]; then
    echo "Not a directory: $path"
    exit 1
fi

Ensures the given path exists and is a directory.
-d → directory test.

files=$(find "$path" -maxdepth 1 -type f | wc -l)

find "$path" → searches inside the directory
-maxdepth 1 → do not go into subdirectories
-type f → only regular files
wc -l → counts the number of lines (files found)

dirs=$(find "$path" -maxdepth 1 -type d | wc -l)

Same as above, but:
-type d → directories
This includes the directory itself

dirs=$((dirs - 1))

Subtracts 1 to remove the root directory from the count.

echo "Files: $files"
echo "Directories: $dirs"

Prints final counts.


**2️⃣ Recursive count (directory + all subdirectories)
Script**

files=$(find "$path" -type f | wc -l)

Searches recursively
Counts all files under the path.

dirs=$(find "$path" -type d | wc -l)
dirs=$((dirs - 1))

Counts all directories recursively.
Subtracts 1 to exclude the top-level directory.

echo "Total files: $files"
echo "Total directories: $dirs"

Displays recursive totals.


| Option        | Meaning                    |
| ------------- | -------------------------- |
| `find`        | Searches filesystem        |
| `-type f`     | Regular files              |
| `-type d`     | Directories                |
| `-maxdepth 1` | Limit to current directory |
| `wc -l`       | Count lines (items found)  |


Why find is preferred
Handles spaces in filenames
Works recursively
More reliable than ls
