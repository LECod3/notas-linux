# Viewing Files

Reading file contents is a daily task. The right tool depends on file size, whether you need the whole file or just a part, and whether the file is still changing.

## Quick decision table

| Situation | Tool |
|---|---|
| Small file, full content | `cat` |
| Large file, navigate and search | `less` |
| First lines | `head` |
| Last lines | `tail` |
| Live logs | `tail -f` or `less +F` |
| Count lines, words, bytes | `wc` |

## `cat` — print the whole file

Concatenates files and writes them to standard output. Best for short files or quick pipelines.

```bash
cat notes.md
cat /etc/hostname

# Number every line
cat -n notes.md

# Show tabs, line ends and other non-printing characters
cat -A notes.md

# Concatenate two files into a new one
cat part1.txt part2.txt > full.txt
```

### Common pitfalls

- Using `cat` on a huge file floods the terminal; use `less`.
- `cat` cannot edit or save changes.
- `cat file | grep pattern` works, but `grep pattern file` is shorter and avoids an extra process.

## `less` — paginated viewer

Opens a file without loading everything at once and lets you navigate and search.

```bash
less /var/log/syslog
less +F /var/log/syslog     # follow mode, similar to tail -f
less notes.md
```

Useful keys:

| Key | Action |
|---|---|
| `Space` / `PageDown` | Next page |
| `b` / `PageUp` | Previous page |
| `g` / `G` | Go to start / end |
| `/pattern` | Search forward |
| `?pattern` | Search backward |
| `n` / `N` | Next / previous match |
| `Shift+F` | Toggle follow mode |
| `q` | Quit |

### Common pitfalls

- `less` is a viewer, not an editor. Press `q` to exit before typing commands.
- In follow mode, `Ctrl+C` stops following but keeps the file open.
- Piping into `less` loses the ability to reopen the original file on reload.

## `head` — first lines

Prints the beginning of a file (`10` lines by default).

```bash
head /etc/passwd
head -n 5 /etc/passwd
head -c 100 /etc/passwd      # first 100 bytes

# Show only the header row of a CSV
head -n 1 data.csv
```

## `tail` — last lines

Prints the end of a file (`10` lines by default). Very common for logs.

```bash
tail /var/log/syslog
tail -n 20 /var/log/syslog
tail -f /var/log/syslog       # follow new lines as they are written
tail -F /var/log/syslog       # like -f but survives log rotation
```

Useful combinations:

```bash
# Watch errors appear in real time
tail -f /var/log/syslog | grep -i error

# Skip the first line and show the rest
tail -n +2 data.csv
```

### Common pitfalls

- `tail -f` blocks the terminal; press `Ctrl+C` to stop.
- `-f` follows a file descriptor and breaks after rotation; use `-F` instead.
- A command that never writes new lines makes `tail -f` look frozen.

## `wc` — count lines, words, bytes

Counts content. Handy for quick validation and scripting.

```bash
wc notes.md              # lines, words, bytes
wc -l notes.md           # lines only
wc -w notes.md           # words only
wc -c notes.md           # bytes only

# Count matching lines with grep
grep -i error /var/log/syslog | wc -l

# Count files in a directory
ls -1 | wc -l
```

Output columns for plain `wc file`:

```text
12  45  310 notes.md
```

Read as: lines, words, bytes.

### Common pitfalls

- `wc -c` counts bytes, not characters; multibyte UTF-8 text can differ under `wc -m`.
- `wc -l` counts newline characters, so the last line without a trailing newline is not counted.
- Piping from `grep` counts matching lines, not occurrences.

## See also

- [Filtering](../02-text-pipelines/filtering.md) (planned)
- [Transforming text](../02-text-pipelines/transforming.md) (planned)
- [Redirections and pipes](../02-text-pipelines/redirections-pipes.md) (planned)