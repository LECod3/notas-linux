# Permissions

Linux protects files using permissions and ownership. Understanding them is essential for security and troubleshooting.

## Reading permission output

```bash
ls -l
```

Example:

```text
-rwxr-x--- 1 luciano dev 4096 Nov 20 16:37 script.sh
```

First character:

- `-`: regular file
- `d`: directory
- `l`: symbolic link

The next nine characters are three permission groups:

- user (owner)
- group
- others

Each group uses `r` (read), `w` (write), `x` (execute).

## `chmod` — change mode

Symbolic form is readable and safer for incremental changes.

```bash
chmod u+x script.sh
chmod g-w report.txt
chmod o=r file.txt
chmod a+r file.txt
```

Numeric (octal) form is common for full permission sets.

| Octal | Permission |
|---|---|
| 0 | --- |
| 1 | --x |
| 2 | -w- |
| 3 | -wx |
| 4 | r-- |
| 5 | r-x |
| 6 | rw- |
| 7 | rwx |

Examples:

```bash
chmod 755 script.sh
chmod 644 notes.txt
chmod 600 secret.txt
```

## Special permissions

| Bit | Name | Octal prefix | Typical use |
|---|---|---|---|
| Setuid | user executes as owner | `4` | `/usr/bin/passwd` |
| Setgid | group inheritance | `2` | shared project directories |
| Sticky bit | only owner can delete | `1` | `/tmp` |

Example:

```bash
chmod 2775 shared-dir
chmod +t shared-dir
```

## `chown` — change owner/group

```bash
sudo chown luciano file.txt
sudo chown luciano:dev file.txt
sudo chown -R luciano:dev project/
```

## `umask` — default permission mask

`umask` removes permissions from new files. Common values:

- `022`: files `644`, directories `755`
- `027`: files `640`, directories `750`
- `077`: files `600`, directories `700`

```bash
umask
umask 027
```

## Common pitfalls

- Treating `777` as a quick fix; it is a security risk.
- Forgetting execute permission on scripts, especially when running `./script.sh`.
- Navigating into a directory requires execute permission.
- Changing system file ownership can lock you out of services; use `sudo` carefully.

## Related files

- [Files and directories](./files-and-dirs.md)
- [Users and groups](../05-users-security/users-groups.md) (planned)