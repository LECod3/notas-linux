# Help and Manuals

Knowing how to discover command documentation is more valuable than memorizing every option.

## `help`

Shows help for shell built-ins (commands internal to Bash).

```bash
help cd
help
```

## `man` — manual pages

Displays the system manual for a command or topic.

```bash
man ls
man 5 passwd    # section 5: file formats
```

Common sections:

| Section | Content |
|---|---|
| 1 | User commands |
| 2 | System calls |
| 3 | Library functions |
| 4 | Special files |
| 5 | File formats |
| 8 | Administration commands |

Navigation in `man` is usually the same as `less`.

## `whatis`

Prints a short one-line description of a command.

```bash
whatis ls
```

## `tldr` — simplified examples

Community-maintained practical examples. Not installed by default.

```bash
tldr tar
```

Install on Ubuntu:

```bash
sudo apt install tldr
```

## `alias`

Creates shortcuts for commands.

```bash
alias ll='ls -l'
alias ..='cd ..'
```

Make aliases persistent by adding them to `~/.bashrc` or `~/.bash_aliases`.

### Common pitfalls

- Aliases do not work in non-interactive scripts.
- Use functions for anything with logic or arguments.
- Avoid aliasing destructive commands to something harmless without understanding side effects.

## A practical lookup workflow

1. `whatis command` for a quick reminder.
2. `command --help` for common options.
3. `man command` for full behavior.
4. `tldr command` for examples.

## See also

- [Navigation basics](./navigating.md)
- [Files and directories](./files-and-dirs.md)