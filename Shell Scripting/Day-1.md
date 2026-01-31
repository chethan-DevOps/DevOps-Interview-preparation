Basic version
#!/bin/bash

file="path/to/your/file"

if [ -e "$file" ]; then
    echo "File exists"
    echo "Size: $(stat -c %s "$file") bytes"
    echo "Permissions: $(stat -c %A "$file")"
else
    echo "File does not exist"
fi

Explanation

-e "$file" → checks if the file exists

stat -c %s → prints file size in bytes

stat -c %A → prints permissions (e.g. -rw-r--r--)

One-liner version
[ -e "$file" ] && stat -c "Size: %s bytes | Permissions: %A" "$file" || echo "File does not exist"

If you only want regular files (not directories)
if [ -f "$file" ]; then
    stat -c "Size: %s bytes\nPermissions: %A" "$file"
fi

macOS note (BSD stat)

If you’re on macOS, stat flags are different:

stat -f "Size: %z bytes\nPermissions: %Sp" "$file"


If you want:

human-readable size (KB/MB)

octal permissions (644)

recursive directory handling

tell me your OS and use case 👍

give the script for multiple files with same scenario

Here’s a solid bash script that handles multiple files, checks existence, and prints size + permissions for each.

Bash script (Linux)
#!/bin/bash

files=(
    "file1.txt"
    "file2.log"
    "/path/to/file3"
)

for file in "${files[@]}"; do
    echo "Checking: $file"

    if [ -e "$file" ]; then
        size=$(stat -c %s "$file")
        perms=$(stat -c %A "$file")
        echo "  Exists"
        echo "  Size: $size bytes"
        echo "  Permissions: $perms"
    else
        echo "  File does not exist"
    fi

    echo "-------------------------"
done

Pass files as command-line arguments (more flexible)
#!/bin/bash

if [ "$#" -eq 0 ]; then
    echo "Usage: $0 file1 file2 ..."
    exit 1
fi

for file in "$@"; do
    if [ -e "$file" ]; then
        stat -c "File: %n\nSize: %s bytes\nPermissions: %A\n" "$file"
    else
        echo "File: $file"
        echo "Does not exist"
        echo
    fi
done


Run it like:

./check_files.sh a.txt b.txt /etc/passwd

macOS version (BSD stat)
for file in "$@"; do
    if [ -e "$file" ]; then
        stat -f "File: %N\nSize: %z bytes\nPermissions: %Sp\n" "$file"
    else
        echo "File: $file does not exist"
    fi
done

Want extras?

I can add:

human-readable sizes (ls -lh style)

only regular files

colorized output

directory recursion

Just say the word 😄

yeah give the script to get it in human readable sizes style

Nice, that’s a useful upgrade 👍
Below are clean scripts that show size in human-readable format (K/M/G) for multiple files.

Linux (GNU tools)
Script (command-line arguments)
#!/bin/bash

if [ "$#" -eq 0 ]; then
    echo "Usage: $0 file1 file2 ..."
    exit 1
fi

for file in "$@"; do
    echo "Checking: $file"

    if [ -e "$file" ]; then
        size=$(du -h "$file" | cut -f1)
        perms=$(stat -c %A "$file")
        echo "  Exists"
        echo "  Size: $size"
        echo "  Permissions: $perms"
    else
        echo "  File does not exist"
    fi

    echo "----------------------------"
done

Why du -h?

stat doesn’t natively support human-readable size

du -h gives K/M/G format automatically

Note: du shows disk usage, not exact file size.
If you want exact logical size, use the alternative below.

Linux (exact size, still human-readable)
#!/bin/bash

for file in "$@"; do
    if [ -e "$file" ]; then
        size=$(stat -c %s "$file" | numfmt --to=iec)
        perms=$(stat -c %A "$file")
        echo -e "File: $file\nSize: $size\nPermissions: $perms\n"
    else
        echo "File: $file does not exist"
        echo
    fi
done


✔ Uses:

numfmt --to=iec → converts bytes → K/M/G

macOS version
#!/bin/bash

for file in "$@"; do
    if [ -e "$file" ]; then
        size=$(stat -f %z "$file" | awk '
            function human(x) {
                s="BKMGT"; while (x>=1024 && length(s)>1)
                {x/=1024; s=substr(s,2)}
                return int(x+0.5) substr(s,1,1)
            }
            {print human($1)}
        ')
        perms=$(stat -f %Sp "$file")
        echo -e "File: $file\nSize: $size\nPermissions: $perms\n"
    else
        echo "File: $file does not exist"
        echo
    fi
done

Example output
Checking: /etc/passwd
  Exists
  Size: 4.0K
  Permissions: -rw-r--r--


If you want:

directories handled recursively

colored output

sorted by size

skip missing files silently

just tell me what flavor you want 😄

directories handled recursively

yes

Perfect 👍
Here’s a bash script that recursively handles directories, prints human-readable sizes, and shows permissions for every file inside.

This works on Linux (GNU tools).

Recursive directory + files (human-readable size)
#!/bin/bash

if [ "$#" -eq 0 ]; then
    echo "Usage: $0 file_or_directory ..."
    exit 1
fi

process_item() {
    local item="$1"

    if [ -e "$item" ]; then
        size=$(stat -c %s "$item" | numfmt --to=iec)
        perms=$(stat -c %A "$item")
        type=$(stat -c %F "$item")

        printf "%-10s | %-8s | %-12s | %s\n" "$type" "$size" "$perms" "$item"
    fi
}

export -f process_item

for path in "$@"; do
    if [ -d "$path" ]; then
        echo "Scanning directory: $path"
        echo "Type       | Size     | Permissions | Path"
        echo "-------------------------------------------------------------"
        find "$path" -print0 | while IFS= read -r -d '' item; do
            process_item "$item"
        done
        echo
    elif [ -e "$path" ]; then
        echo "Type       | Size     | Permissions | Path"
        echo "-------------------------------------------------------------"
        process_item "$path"
        echo
    else
        echo "Path does not exist: $path"
    fi
done

Example usage
chmod +x check_recursive.sh
./check_recursive.sh /var/log myfile.txt

Example output
Type       | Size     | Permissions | Path
-------------------------------------------------------------
directory  | 4.0K     | drwxr-xr-x   | /var/log
regular    | 12K      | -rw-r--r--   | /var/log/syslog
regular    | 1.1M     | -rw-------   | /var/log/auth.log

Notes

✅ Exact size, not disk usage

✅ Handles files + directories

✅ Fully recursive

✅ Clean tabular output

If you want, I can:

add color output

skip directories and show files only

sort by size (largest first)

limit recursion depth

Just say what you want next 😄

skip directories and show **files only**

yes

Got it 👍 — files only, skip directories, still human-readable sizes, recursive.

Here’s a clean Linux bash script that does exactly that.

Recursive → files only → human-readable size
#!/bin/bash

if [ "$#" -eq 0 ]; then
    echo "Usage: $0 file_or_directory ..."
    exit 1
fi

echo "Size     | Permissions | File"
echo "-----------------------------------------------"

for path in "$@"; do
    if [ -d "$path" ]; then
        find "$path" -type f -print0 | while IFS= read -r -d '' file; do
            size=$(stat -c %s "$file" | numfmt --to=iec)
            perms=$(stat -c %A "$file")
            printf "%-8s | %-11s | %s\n" "$size" "$perms" "$file"
        done
    elif [ -f "$path" ]; then
        size=$(stat -c %s "$path" | numfmt --to=iec)
        perms=$(stat -c %A "$path")
        printf "%-8s | %-11s | %s\n" "$size" "$perms" "$path"
    else
        echo "Skipping (not found): $path"
    fi
done

Example usage
chmod +x files_only.sh
./files_only.sh /var/log myfile.txt

Example output
Size     | Permissions | File
-----------------------------------------------
12K      | -rw-r--r--  | /var/log/syslog
1.1M     | -rw-------  | /var/log/auth.log
4.0K     | -rw-r--r--  | myfile.txt

Why this works well

find -type f → files only

numfmt --to=iec → exact size → K/M/G

Safe for filenames with spaces

Works for files and directories passed as input
