# Basic File-Processing — Extras Cheat Sheet

Additional, commonly useful file and text processing utilities (compact reference).

**Field & column processing**
- `awk`: field-oriented processing and light scripting. Default field separator is whitespace.
  - Example: `awk -F',' '{print $1, $3}' file.csv` — print columns 1 and 3 from CSV.
  - Sum a column: `awk '{sum+=$2} END{print sum}' file`.
- `cut`: extract byte/char/fields. Example: `cut -d':' -f1 /etc/passwd`.

**Character and encoding tools**
- `tr`: translate or delete characters. Example: `tr -d '\r' < windows.txt > unix.txt`.
- `iconv`: convert character encodings. Example: `iconv -f ISO-8859-1 -t UTF-8 old.txt > new.txt`.
- `dos2unix` / `unix2dos`: fix CRLF vs LF line endings.

**Finding and acting on files**
- `find`: search by name, type, time, size and run actions.
  - Example: `find . -type f -name '*.log' -mtime +30 -exec rm {} +`.
- `locate` / `updatedb`: very fast name searches using a database. Update DB with `sudo updatedb`.
- `xargs`: build command lines from stdin (use `-0` with NULs). Example:
  - `find . -name '*.tmp' -print0 | xargs -0 rm`.

**File metadata, links, and timestamps**
- `file`: identify file type. Example: `file archive.gz`.
- `stat`: detailed file metadata (size, timestamps, inode).
- `touch`: create empty file or update mtime. Example: `touch -c file`.
- `ln` / `ln -s`: create hard/soft links.

**Copying, syncing, imaging**
- `rsync`: fast, resumable sync and copy. Example: `rsync -av --progress src/ dest/`.
- `dd`: low-level copy (use with care). Examples:
  - Create image: `dd if=/dev/sda of=disk.img bs=4M status=progress`.
  - Write image: `dd if=disk.img of=/dev/sdb bs=4M status=progress`.

**Binary inspection**
- `hexdump` / `xxd`: view binary as hex. Example: `xxd file | less`.
- `strings`: extract printable strings from binaries: `strings a.out | less`.

**Progress & pipeline tools**
- `pv`: monitor progress through a pipe. Example: `pv big.iso | dd of=/dev/sdb`.
- `tee`: already in main sheet — duplicate stream to file and stdout.

**Comparing / merging**
- `comm`: compare two sorted files line-by-line (three-column output).
- `join`: join two files on a common field (like SQL JOIN).

**Splitting & context-based splitting**
- `split`: split by size or lines (see main sheet).
- `csplit`: split files by context/pattern. Example: `csplit file /pattern/ {*}`.
- `tac`: print file in reverse line order; `rev` reverses characters on each line.

**Text reformatting**
- `fmt` / `fold`: reflow or wrap text to widths. Example: `fmt -w 72 README.md`.

Quick examples
- Count distinct words in a file (ignore case):
```
tr -cs '[:alnum:]' '[\n*]' < file | tr 'A-Z' 'a-z' | sort | uniq -c | sort -nr
```
- Find files changed in last 7 days and archive them:
```
find /var/log -type f -mtime -7 -print0 | tar --null -T - -czf recent-logs.tar.gz
```
