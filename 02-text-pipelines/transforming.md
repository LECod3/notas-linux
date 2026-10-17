# Transforming Text

Filtering keeps lines; transforming rewrites them. These tools turn raw text (logs, CSVs, command output) into the exact shape you need.

## `sed` — stream editor

Edits text while it flows through a pipe, or in place with `-i`.

```bash
# Replace the first occurrence per line
sed 's/old/new/' file.txt

# Replace every occurrence per line (g = global)
sed 's/old/new/g' file.txt

# Case-insensitive replace
sed 's/error/ERROR/gi' app.log

# Print only matching lines (like grep)
sed -n '/ERROR/p' app.log

# Delete matching lines
sed '/^#/d' config.conf

# Delete empty lines
sed '/^$/d' file.txt

# Work on a range of lines
sed -n '10,20p' file.txt
```

In-place editing (modifies the file):

```bash
sed -i 's/foo/bar/g' file.txt
sed -i.bak 's/foo/bar/g' file.txt   # keep file.txt.bak as backup
```

Real examples:

```bash
# Normalize Windows line endings (CRLF -> LF)
sed -i 's/\r$//' file.txt

# Strip leading whitespace
sed 's/^[[:space:]]*//' file.txt

# Add a prefix to every non-empty line
sed '/^$/! s/^/> /' notes.md
```

### Common pitfalls

- `sed -i` has no undo; use `-i.bak` when unsure.
- `s/old/new/` replaces only the first match per line; add `g` for all.
- Delimiters are flexible (`s|a|b|`), handy when the pattern contains `/`.
- Basic regex is default; use `-E` for `+`, `?`, `|` without escaping.

## `awk` — column and field processing

Great for structured text: split each line into fields and act on them.

```bash
# Print the first and third fields (default separator: whitespace)
awk '{print $1, $3}' file.txt

# Use a custom separator, e.g. comma for CSV
awk -F, '{print $1, $2}' data.csv

# Print lines where the second field is greater than 100
awk '$2 > 100' file.txt

# Number lines
awk '{print NR": "$0}' file.txt

# Prepend a header and a footer
awk 'BEGIN {print "START"} {print} END {print "END"}' file.txt

# Sum a numeric column
awk '{sum += $3} END {print sum}' file.txt
```

Real examples:

```bash
# Top 5 IPs from an access log (field 1)
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head -n 5

# Average of the third column
awk '{sum += $3; n++} END {if (n) print sum/n}' file.txt

# Print only lines with 5 or more fields
awk 'NF >= 5' file.txt
```

### Common pitfalls

- `$0` is the whole line, `$1` is the first field, `NF` is the field count.
- Mixed separators need a regex: `awk -F'[,;]'`.
- Quoting: single quotes protect `$1` from the shell; double quotes let the shell expand it.
- Logic belongs in the pattern/action blocks, not the shell.

## `cut` — extract columns or characters

Simpler than `awk` when you only need fixed positions.

```bash
# First field, colon-separated
cut -d: -f1 /etc/passwd

# Fields 1 and 7
cut -d: -f1,7 /etc/passwd

# A range of fields
cut -d, -f2-4 data.csv

# Characters 1 to 10
cut -c1-10 file.txt
```

### Common pitfalls

- `cut` works with a single delimiter character; multi-char separators fail.
- It cannot handle quoted CSV fields that contain the delimiter (use `awk` or a CSV tool).
- Default delimiter is `Tab`, not space; set `-d' '` explicitly for spaces.

## `sort` — order lines

```bash
sort file.txt
sort -n numbers.txt        # numeric order
sort -r file.txt           # reverse
sort -u file.txt           # sort and remove duplicates
sort -k2 file.txt          # sort by the second field
sort -t: -k3 -n /etc/passwd  # colon-separated, numeric third field
sort -h sizes.txt          # human-readable sizes (1K, 2M, 3G)
```

## `uniq` — collapse adjacent duplicates

`uniq` only compares neighboring lines, so sort first.

```bash
sort file.txt | uniq
sort file.txt | uniq -c     # prefix with the count
sort file.txt | uniq -d     # only duplicated lines
sort file.txt | uniq -u     # only unique lines
```

Real example: count occurrences and show the most frequent first.

```bash
sort ips.txt | uniq -c | sort -rn | head
```

### Common pitfalls

- Forgetting `sort` before `uniq` gives wrong results.
- Leading whitespace in counts: use `awk '{print $2}'` if you need clean values.

## `tr` — translate or delete characters

Operates on single characters, not strings.

```bash
echo "Hello" | tr 'a-z' 'A-Z'    # uppercase
tr -d '\r' < file.txt            # delete carriage returns
tr -s ' ' < file.txt             # squeeze repeated spaces
tr ',' '\t' < data.csv           # comma to tab
```

### Common pitfalls

- `tr` cannot replace words; use `sed` for multi-character strings.
- Character ranges are locale-dependent (`[:upper:]`, `[:lower:]`, `[:digit:]` are safer).
- `tr` reads from stdin; redirect with `<` or pipe.

## Putting it together

```bash
# Top 10 most frequent words in a file (lowercased, one per line)
tr -cs '[:alnum:]' '\n' < notes.md \
  | tr '[:upper:]' '[:lower:]' \
  | sort | uniq -c | sort -rn | head -n 10

# Convert a CSV to a sorted, unique list of the second column
awk -F, '{print $2}' data.csv | sort -u
```

## See also

- [Viewing files](./viewing.md)
- [Filtering text](./filtering.md)
- [Redirections and pipes](./redirections-pipes.md) (planned)