# Navigation Basics

Navigation is the first skill in any terminal workflow. Mastering these commands makes every other task faster.

## `pwd` — print working directory

Shows the absolute path of your current directory.

```bash
pwd
```

Example output:

```text
/home/luciano/proyectos/notas-linux
```

## `cd` — change directory

Moves you to another directory.

```bash
cd /etc
cd ..        # parent directory
cd .          # current directory
cd ~          # home directory
cd -          # previous directory
```

### Path types

| Prefix | Meaning |
|---|---|
| `/` | Absolute path from the root |
| `.` | Current directory |
| `..` | Parent directory |
| `~` | Your home directory |
| `-` | Previous directory |

### Common pitfalls

- Forgetting that `cd` without arguments goes home (`cd` == `cd ~`).
- Using relative paths when you meant absolute paths.
- Paths with spaces require quotes: `cd "My Folder"`.

## `ls` — list directory contents

Lists files and directories.

```bash
ls
ls -a        # include hidden files (starting with .)
ls -l        # long format: permissions, owner, size, date
ls -la       # combine flags
ls -al       # same result in most cases
```

Long format example:

```text
drwxr-x--- 7 pete penguingroup 4096 Nov 20 16:37 Desktop
```

Breakdown (from left to right):

- permissions
- link count
- owner
- group
- size
- modification date/time
- name

Timestamp example:

```text
Nov 20 16:37   # recent file: month day hour:minute
Nov 20  2024   # older file: month day  year
```

In most GNU `ls` outputs, files modified within the last six months show `Mon DD HH:MM`; older files show `Mon DD  YYYY`.

### Decoding a real `ls -l` line

```text
drwxr-x--- 7 pete penguingroup 4096 Nov 20 16:37 Desktop
```

Breakdown:

| Field | Value | Meaning |
|---|---|---|
| Type | `d` | Directory |
| Owner permissions | `rwx` | Read, write, execute/enter |
| Group permissions | `r-x` | Read and execute/enter only |
| Other permissions | `---` | No access |
| Numeric mode | `750` | Common shorthand for the permission set |
| Hard links | `7` | Directory entries including `.` and `..` |
| Owner | `pete` | User that owns the directory |
| Group | `penguingroup` | Group that owns the directory |
| Size | `4096` | Directory metadata size, not total content size |
| Timestamp | `Nov 20 16:37` | Last modification time |
| Name | `Desktop` | Directory name |

Practical reading:

- `pete` can read, modify, and enter the directory.
- Members of `penguingroup` can enter and list it, but not create or delete files.
- Everyone else has no access at all.
- If `pete` needs full collaboration, a common choice is `chmod 770 Desktop` (owner and group full access, others none), but only after deciding group membership carefully.

Another example:

```text
-rw-r--r-- 1 pete dev 1234 Nov 19 09:12 notes.md
```

Breakdown:

| Field | Value | Meaning |
|---|---|---|
| Type | `-` | Regular file |
| Owner permissions | `rw-` | Read and write |
| Group permissions | `r--` | Read only |
| Other permissions | `r--` | Read only |
| Numeric mode | `644` | Common mode for readable text files |
| Hard links | `1` | One file name points to this inode |
| Owner | `pete` | File owner |
| Group | `dev` | File group |
| Size | `1234` | File size in bytes |
| Timestamp | `Nov 19 09:12` | Last modification time |
| Name | `notes.md` | File name |

### Common pitfalls

- Hidden files are invisible unless you use `-a`.
- `ls` order and flags may differ between systems; check `man ls`.
- Aliased `ls` (common in Ubuntu) can hide defaults; use `\ls` to bypass aliases.

## `clear`

Clears the terminal screen. History is still available.

```bash
clear
```

Shortcut: `Ctrl + L`.

## `history`

Shows previously executed commands.

```bash
history       # list history
history -c    # clear in-memory history
history -w    # write history to file (when supported)
```

### Common pitfalls

- `history -c` does not always remove the on-disk history file.
- Sensitive commands stay in history; use `HISTCONTROL=ignorespace` and prefix with a space to skip saving.

## Related files

- [Files and directories](./files-and-dirs.md)
- [Permissions](./permissions.md)
- [Help and manuals](./help-man.md)
```

## Navigation tips

- Use `Tab` for path completion and to avoid typos.
- Use `pwd` when scripts depend on the current working directory.
- Prefer absolute paths in scripts; relative paths depend on where the script runs.