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