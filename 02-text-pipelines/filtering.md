# Filtering Text

Filtering means keeping only the lines that matter. `grep` is the classic tool; `rg` (ripgrep) is a faster, modern alternative that respects `.gitignore` by default.

## `grep` — search text

```bash
grep pattern file.txt          # lines containing pattern
grep -i error /var/log/syslog  # ignore case
grep -n TODO notes.md          # show line numbers
grep -v "^#" config.conf       # invert match: lines NOT starting with #
grep -c error app.log          # count matching lines
grep -r pattern src/           # recurse into directories
grep -w todo notes.md          # match whole words only
grep -E "error|warn" app.log   # extended regex (alternation)
```

Real examples:

```bash
# Failed SSH logins
grep "Failed password" /var/log/auth.log

# Lines matching error or warn in any .log file under /var/log
grep -rE "error|warn" /var/log --include="*.log"

# Show matching file names without the content
grep -rl "TODO" src/

# Show 3 lines of context around matches
grep -n -C 3 "panic" system.log
```

Useful options:

| Option | Meaning |
|---|---|
| `-i` | Ignore case |
| `-n` | Show line numbers |
| `-v` | Invert match |
| `-c` | Count matching lines |
| `-r` | Recursive |
| `-l` | Print only filenames |
| `-w` | Whole words |
| `-C N` | Context lines around matches |
| `-E` | Extended regular expressions |

### Common pitfalls

- Forgetting quotes: `grep "foo bar" file` is not the same as `grep foo bar file`.
- `grep pattern *` can search huge sets; use `-r` with a path.
- A pattern starting with `-` is read as a flag; use `grep -- -pattern file`.
- Regex flavors differ: basic vs extended (`-E`) vs PCRE (`-P`).

## `rg` — ripgrep

Faster than `grep`, recursive by default, and skips files ignored by git.

Install on Ubuntu:

```bash
sudo apt install ripgrep
```

```bash
rg pattern                 # search current directory recursively
rg -i pattern              # ignore case
rg -n pattern              # with line numbers
rg -t py "def main"        # only Python files
rg -g "*.md" pattern       # only matching glob
rg -v pattern file         # invert match
rg --hidden pattern        # include hidden files
rg -l pattern              # only file names
```

Real examples:

```bash
# Find TODO comments in a project
rg "TODO|FIXME"

# Search only Markdown files
rg -g "*.md" "permissions"

# Search including git-ignored files
rg -uu "password"

# Show context around matches
rg -C 2 "timeout"
```

### Common pitfalls

- `rg` is not always installed; scripts should fall back to `grep`.
- By default it ignores `.gitignore`; use `-uu` when you really need everything.
- Pattern is a regex; escape special characters such as `.`, `*`, `[`.

## `grep` vs `rg` quick comparison

| Task | `grep` | `rg` |
|---|---|---|
| Single file | `grep pat file` | `rg pat file` |
| Recursive | `grep -r pat dir` | `rg pat` |
| Ignore case | `grep -i pat` | `rg -i pat` |
| File type filter | `--include="*.py"` | `-t py` |
| Hidden/ignored files | included | excluded by default |

## Counting and validating

```bash
# How many errors today?
grep -c error app.log

# Are there any non-empty lines?
grep -c . file.txt

# Count occurrences of a word (notes: -o prints each match)
grep -o "linux" notes.md | wc -l
```

## See also

- [Viewing files](./viewing.md)
- [Transforming text](./transforming.md) (planned)
- [Redirections and pipes](./redirections-pipes.md) (planned)