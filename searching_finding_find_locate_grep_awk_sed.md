# Searching & Finding: `find`, `locate`, `grep`, `awk`, `sed`

> A practical reference and tutorial for Linux/Unix command-line searching and text-processing tools. Includes syntax, common options, examples, tips, and exercises so anyone can learn and use these tools effectively.

---

## Table of Contents

1. Introduction
2. Filesystem search: `find`
   - What it is
   - Basic syntax
   - Common options and examples
   - Tests, actions, and combining expressions
   - Performance tips and safety notes
3. Fast name search: `locate` (and `updatedb`)
   - How it works
   - Basic usage and examples
   - Limitations and maintenance
4. Text search: `grep`
   - What it is
   - Basic syntax
   - Important options with examples
   - Regular expressions with `grep` (basic vs. extended)
   - Practical examples and tips
5. Field and record processing: `awk`
   - What `awk` is and when to use it
   - Basic syntax and structure
   - Common patterns and examples
   - Advanced usage (variables, functions, scripts)
6. Stream editing: `sed`
   - What `sed` is and when to use it
   - Basic syntax and substitution
   - Addressing, deletion, insertion, and more
   - In-place editing and portability notes
7. Combining tools (pipes, xargs, find -exec)
   - Practical patterns
   - Examples that solve real tasks
8. Regular expressions primer (for grep/awk/sed)
9. Performance and safety considerations
10. Practical exercises (with answers)
11. Cheatsheet: common flags and one-liners
12. Further reading

---

# 1. Introduction

This document covers the most commonly used command-line tools for finding files and searching/manipulating text on Unix-like systems: `find`, `locate`, `grep`, `awk`, and `sed`.

- `find` — search for files and directories in a directory hierarchy using file properties (name, size, time, permissions, owner, etc.).
- `locate` — very fast filename lookup using a periodically updated database (`updatedb`).
- `grep` — search file contents (lines) using patterns/regular expressions.
- `awk` — powerful text processing tool used to parse and transform structured text (records and fields).
- `sed` — stream editor for basic to advanced text transformations on streams or files.

These tools are often combined with pipes (`|`), redirection, and `xargs` to form powerful one-liners.

---

# 2. Filesystem search: `find`

## What it is

`find` walks a directory tree and evaluates expressions (tests and actions) against each file. It's extremely flexible and can search by name, type, modification time, size, ownership, permissions, and more.

## Basic syntax

```sh
find [path...] [expression]
```

- If no path is given, `find` uses the current directory (`.`).
- An expression consists of options, tests, and actions combined with operators like `-and` (implicit), `-or`, `!` (not), and parentheses `(` `)`.

## Common options and examples

Find files by name (case-sensitive):
```sh
find /var/log -name "*.log"
```
Find files by name (case-insensitive):
```shn
find . -iname "readme*"
```
Find directories only:
```sh
find /usr -type d -name "bin"
```
Find regular files only:
```sh
find . -type f -name "*.py"
```
Find files modified in the last 7 days:
```sh
find . -mtime -7
```
Find files accessed more than 30 days ago:
```sh
find /tmp -atime +30
```
Find files larger than 10 megabytes:
```sh
find / -type f -size +10M
```
Find files owned by a user or group:
```sh
find /home -user alice
find /var -group staff
```
Change permissions (dangerous — use with care):
```sh
find /some/path -type f -name "*.sh" -exec chmod +x {} \;
```
Delete files matching pattern (VERY DANGEROUS — double-check first):
```sh
find . -type f -name "*.tmp" -delete
```

## Tests, actions, and combining expressions

- `-name PATTERN` — match filename with shell globbing
- `-iname PATTERN` — case-insensitive name
- `-type [f|d|l|c|b|p|s]` — file type
- `-size N[cwbkMG]` — size (suffix: c=bytes, k=kilobytes, M=megabytes, G=gigabytes)
- `-mtime N`, `-atime N`, `-ctime N` — modification/access/change times (days)
- `-newer file` — newer than file
- `-perm MODE` — match permissions
- `-user` / `-group` — owner and group
- `-exec command {} \;` — run command once per file (safer: `+` to aggregate)
- `-ok command {} \;` — like `-exec` but asks confirmation
- `-prune` — exclude directories from search (useful to skip large trees)

### Example: exclude `.git` and `node_modules`

```sh
find . \( -path ./node_modules -o -path ./.git \) -prune -o -type f -name "*.js" -print
```

### Use `-print0` and `xargs -0` to handle spaces/newlines in filenames

```sh
find . -type f -name "*.jpg" -print0 | xargs -0 -I{} convert {} -resize 800x600 {}
```

### `-exec ... +` vs `-exec ... \;`

- `\;` executes the command once per matched file.
- `+` aggregates many files into a single command invocation (more efficient):

```sh
find . -name "*.log" -exec gzip {} +
```

## Performance tips and safety

- Limit search path when possible (e.g., `find /home` not `/`).
- Use `-prune` to skip directories like backups, `.git`, or `node_modules`.
- Prefer `-exec ... +` or `-print0 | xargs -0` for efficiency.
- Always test with `-print` before running destructive actions like `-delete` or `chmod`.

---

# 3. Fast name search: `locate` and `updatedb`

## How it works

`locate` uses a prebuilt database of filenames (typically created by `updatedb` run via cron). It's very fast because it searches the database, not the filesystem in real time.

## Basic usage

```sh
locate filename
locate -i README  # case-insensitive
```

You can pipe results to `grep` to filter further:

```sh
locate bin | grep -E "/python[0-9.]+$"
```

## Important options

- `-i` — case-insensitive
- `-r` — interpret argument as regular expression (depending on implementation)

## Limitations and maintenance

- The database may be stale until `updatedb` runs. Run `sudo updatedb` to refresh.
- `locate` typically skips files/directories excluded by `updatedb` configuration (e.g., `/tmp`).
- `locate` finds names only — it does not filter by file content or metadata like `find` does.

---

# 4. Text search: `grep`

## What it is

`grep` searches text for lines that match a pattern (string or regular expression). It is extremely common for quick content searches in files and pipelines.

## Basic syntax

```sh
grep [options] PATTERN [file...]
```

- If no file is provided, `grep` reads from standard input.

## Important options and examples

- `-i` — ignore case
- `-v` — invert match (show non-matching lines)
- `-r` or `-R` — recursive search in directories
- `-n` — show line numbers
- `-H` — show filename (enabled by default when searching multiple files)
- `-l` — list filenames only
- `-c` — count matching lines
- `-E` — use extended regular expressions (same as `egrep`)
- `-F` — fixed-string search (faster, like `fgrep`)
- `-o` — show only the matching part of the line
- `-P` — use Perl-compatible regex (if supported)

### Examples

Search recursively for the word "TODO":
```sh
grep -R "TODO" src/
```
Show matching lines with line numbers:
```sh
grep -n "main()" *.c
```
Count matches in a file:
```sh
grep -c "ERROR" /var/log/syslog
```
Show files containing pattern:
```sh
grep -l "password" $(find /etc -type f)
```
Show only matched words:
```sh
echo "one two three" | grep -o "t[a-z]*"
# prints: two three
```
Search for a literal string containing special regex characters using `-F`:
```sh
grep -F "a+b?c" file.txt
```

## Using `ripgrep` or `ag`

There are modern alternatives like `rg` (ripgrep) and `ag` (the_silver_searcher) that are faster for large codebases. Use them when available.

---

# 5. Field and record processing: `awk`

## What `awk` is

`awk` is a domain-specific language for text processing. It reads input line-by-line (records) and splits each record into fields. By default, fields are separated by whitespace; you can change the field separator.

Common uses: summarizing columns, column selection, complex filtering, and generating formatted output.

## Basic syntax

```sh
awk 'pattern { action }' file
```

`awk` programs can be provided inline or in a separate file with `-f script.awk`.

- `$0` — entire line (record)
- `$1`, `$2`, ... — fields
- `NF` — number of fields
- `NR` — current record number (line number)
- `FS` — input field separator (default: space)
- `OFS` — output field separator

### Examples

Print second column of a file:
```sh
awk '{print $2}' file.txt
```
Print lines where 3rd column > 100:
```sh
awk '$3 > 100 {print $0}' data.txt
```
Use a different field separator (CSV with comma):
```sh
awk -F',' '{print $1, $3}' file.csv
```
Sum a column:
```sh
awk '{sum += $2} END {print sum}' sales.txt
```
Print header and formatted output:
```sh
awk 'BEGIN {print "Name,Value"} {printf "%s,%d\n", $1, $2}' data.txt
```
Filter and number lines:
```sh
awk '/ERROR/ {print NR ": " $0}' /var/log/syslog
```

## Advanced features

- Variables (e.g., `threshold=50` and use `$3 > threshold`)
- Functions (user-defined)
- Control structures (`if`, `while`, `for`)
- `split()`, `gsub()` for substitutions
- Associative arrays (very powerful for counting and grouping)

### Example: count occurrences of a word (case-insensitive)

```sh
awk 'BEGIN {IGNORECASE=1} {for(i=1;i<=NF;i++) if ($i == "error") count++} END {print count}' file.log
```

---

# 6. Stream editing: `sed`

## What `sed` is

`sed` is a non-interactive stream editor suitable for simple substitutions and transformations on streams of text. Commonly used for find-and-replace in files or pipelines.

## Basic syntax

```sh
sed [options] 'script' file
# or
sed -f script.sed file
```

A typical substitution:
```sh
sed 's/old/new/' file         # replaces first occurrence on each line
sed 's/old/new/g' file        # replaces all occurrences on each line
```

## Addresses and commands

- `sed '5d' file` — delete the 5th line
- `sed '1,3d' file` — delete lines 1 to 3
- `sed -n '1,5p' file` — print lines 1 to 5
- `sed -n '/pattern/p' file` — print only lines matching pattern

## Insert, append, change

- Insert before a line number or pattern:
```sh
sed '/pattern/i\
Inserted line' file
```
- Append after a line:
```sh
sed '/pattern/a\
Appended line' file
```
- Change entire line matching pattern:
```sh
sed '/pattern/c\
Replacement line' file
```

## In-place editing (with backup)

GNU `sed` supports `-i` for in-place editing (optionally with a backup suffix):

```sh
sed -i.bak 's/foo/bar/g' file.txt
# This edits file.txt and creates file.txt.bak with the original content.
```

Be cautious: BSD/macOS `sed` requires a backup suffix (even if empty):

```sh
# macOS/BSD
sed -i '' 's/foo/bar/g' file.txt
```

## Using `sed` for more than substitution

- Use `sed` for quick line deletions, simple column extraction, and minor edits in pipelines.
- For complex parsing and field-aware transformations, prefer `awk` or use a real scripting language (Python/Perl).

---

# 7. Combining tools (pipes, `xargs`, `find -exec`)

These tools become much more powerful when combined. A few patterns:

### Search files by name and then search inside them

```sh
find . -type f -name "*.py" -print0 | xargs -0 grep -n "TODO"
# or using find -exec:
find . -name "*.py" -exec grep -n "TODO" {} +
```

### Use `grep` to filter `ls` output (not recommended for large directories; prefer `find`)

```sh
ls -l | grep "^d"   # find directories in current dir (but not reliable for edge cases)
```

### Count unique values in a column (awk + sort + uniq)

```sh
awk -F, '{print $3}' data.csv | sort | uniq -c | sort -nr
```

### Use `grep -H` to show filename:lineno and `sed` or `awk` to extract context

```sh
grep -R --line-number "TODO" src/ | sed -n '1,100p'
```

### Safely delete files older than 30 days in a directory

```sh
find /var/log/myapp -type f -mtime +30 -print0 | xargs -0 rm -v
```

---

# 8. Regular expressions primer (for `grep`/`awk`/`sed`)

- `.` — any single character
- `*` — 0 or more of previous atom
- `+` — 1 or more (extended regex; in basic regex use `\+` or `\{1,\}`)
- `?` — 0 or 1 (extended regex)
- `[]` — character class, e.g. `[0-9a-z]`
- `^` — start of line
- `$` — end of line
- `\b` — word boundary (in some regex flavors)
- `\(` `\)` and `\1` — groups and backreferences (escaping differs between BRE and ERE)

Note: Different tools support slightly different regex syntaxes:
- `grep` default (BRE) vs `grep -E` (ERE) vs `grep -P` (PCRE on some systems).
- `awk` uses extended-like regex in patterns.
- `sed` uses basic regex by default; `-E` may enable extended regex in some implementations.

---

# 9. Performance and safety considerations

- Avoid running `find /` without filters — it can be slow and traverse special filesystems.
- Use `-prune` to skip unwanted directories.
- Prefer `-exec ... +` or `-print0 | xargs -0` over `-exec ... \;` when processing many files.
- For searching within codebases, use `rg` (ripgrep) if available — it is faster and respects `.gitignore` by default.
- When editing files in-place, make backups before running destructive commands.
- Be careful with `rm`, `chmod`, `chown` invoked from `find -exec` — always test first.

---

# 10. Practical exercises (with answers)

**Exercise 1:** Find all `.log` files in `/var/log` modified in the last 7 days.

Answer:
```sh
find /var/log -type f -name "*.log" -mtime -7
```

**Exercise 2:** Using `locate`, find all files named `sshd_config`.

Answer:
```sh
locate sshd_config
# If missing recent files, run: sudo updatedb
```

**Exercise 3:** Count how many times the word "ERROR" appears in `app.log` (case-insensitive).

Answer:
```sh
grep -oi "ERROR" app.log | wc -l
# or
grep -i "ERROR" app.log | wc -l   # counts matching lines, not occurrences per line
```

**Exercise 4:** Print the third column from a space-separated file where the value in column 4 is greater than 100.

Answer:
```sh
awk '$4 > 100 {print $3}' file.txt
```

**Exercise 5:** Replace all occurrences of `foo` with `bar` in `file.txt`, keeping a backup.

Answer:
```sh
sed -i.bak 's/foo/bar/g' file.txt
```

---

# 11. Cheatsheet: common flags and one-liners

**find**
- `find . -name "*.conf"`
- `find / -type f -size +100M`
- `find . -type d -name node_modules -prune -o -print`

**locate**
- `locate -i README`
- `sudo updatedb` — refresh DB

**grep**
- `grep -Rni "pattern" dir/` — recursive, ignore case, show line numbers
- `grep -o "pattern" file | wc -l` — count occurrences

**awk**
- `awk '{print $1, $NF}' file` — first and last fields
- `awk -F: '{print $1}' /etc/passwd` — print usernames

**sed**
- `sed -n '1,10p' file` — print first 10 lines
- `sed -i 's/orig/new/g' file` — in-place replace (GNU sed)

---

# 12. Further reading

- `man find`, `man updatedb`, `man locate`, `man grep`, `man awk`, `man sed`
- The GNU `awk` User’s Guide (gawk)
- Regular-expression HOWTO and online regex testers
- Tools: `ripgrep (rg)`, `the_silver_searcher (ag)`

---

_This file was created to be a standalone learning reference. Practice the exercises and adapt the examples to your environment. Always run dangerous commands (delete, chmod, chown) with caution and consider backups._

