# Files and Directories

Basic file and directory operations are the foundation of system administration and scripting.

## `touch` — create or update files

Creates an empty file or updates its timestamp.

```bash
touch notes.txt
touch a.txt b.txt
```

### Common pitfalls

- If the file exists, `touch` only changes timestamps, it does not erase content.
- Use `>` redirection if you want to create/overwrite content.

## `file` — identify file type

Guesses the type of a file by inspecting content, not just its extension.

```bash
file notes.txt
file /bin/ls
```

## `cat` — concatenate and print files

Prints file content to standard output. Good for small files.

```bash
cat notes.txt
cat a.txt b.txt > combined.txt
```

### Common pitfalls

- Avoid `cat` for very large files; use `less`.
- `cat` does not edit files.

## `less` — paginated viewer

Views large files without loading everything into memory.

```bash
less /var/log/syslog
```

Useful keys:

| Key | Action |
|---|---|
| `Space` | Next page |
| `b` | Previous page |
| `/pattern` | Search forward |
| `?pattern` | Search backward |
| `q` | Quit |

## `cp` — copy

Copies files or directories.

```bash
cp source.txt copy.txt
cp -r src/ backup/
```

Common options:

- `-r`: recursive (directories)
- `-p`: preserve mode, ownership, timestamps
- `-i`: prompt before overwrite

## `mv` — move or rename

Moves or renames files and directories.

```bash
mv old.txt new.txt
mv report.txt ~/Documents/
```

### Common pitfalls

- Moving a file across filesystems copies then deletes; may affect permissions if not root.
- `mv` can overwrite without warning unless `-i` is used.

## `mkdir` — create directories

Creates one or more directories.

```bash
mkdir projects
mkdir -p projects/2026/notes   # create parents as needed
```

## `rmdir` — remove empty directories

Removes only empty directories.

```bash
rmdir empty-dir
```

## `rm` — remove files or directories

Deletes files (and directories with `-r`). There is no recycle bin in the terminal.

```bash
rm temp.txt
rm -f protected.txt      # force, no prompt
rm -r old-project/        # recursive
rm -rf dangerous/          # force + recursive
```

### Safety notes

- `rm -rf` is destructive; double-check the path.
- Prefer `rm -i` in daily use to confirm deletions.
- Never run `rm -rf /` or `rm -rf ~` casually.

## `find` — search files

Searches files by name, type, size, or modification time.

```bash
find . -name "*.md"
find /var/log -type f -name "*.log"
find . -size +100M
find . -mtime -1        # modified in last day
```

Combine with actions:

```bash
find . -name "*.tmp" -delete
find . -name "*.log" -exec gzip {} \;
```

### Common pitfalls

- Forgetting quotes around patterns (shell expands `*` first).
- Running expensive searches on `/` without narrowing the path.

## See also

- [Navigation basics](./navigating.md)
- [Permissions](./permissions.md)