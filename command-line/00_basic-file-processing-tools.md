# Basic File-Processing Tools — Linux Cheat Sheet

A concise reference for everyday file/text processing on the command line.

**Viewing & combining files**
- `cat`: concatenate and print files. Example: `cat file1 file2`.
- `paste`: merge lines from files side-by-side (columns). Example: `paste a.txt b.txt`.
- `od`: dump file contents in octal/other formats. Example: `od -c file` shows printable characters.
- `split`: split a file into pieces. Examples: `split -b 10M big.bin prefix.` or `split -l 1000 large.txt part_`.
- `diff`: show differences between files. Example: `diff -u old new` (unified diff for patches).
- `sort`: sort lines. Example: `sort input.txt -o sorted.txt`.
- `nl`: number lines. Example: `nl -ba file` (number all lines).
- `head` / `tail`: show start/end of files. Examples: `head -n 20 file`, `tail -n 100 file`, `tail -f logfile`.
- `more` / `less`: paginate large output. Prefer `less` (`less file`) for backwards scrolling and search.
- `wc`: word/line/byte counts. Example: `wc -l file` (lines), `wc -w` (words), `wc -c` (bytes).
- `cut`: extract columns/fields. Examples: `cut -c1-10 file` (chars), `cut -d',' -f1-3 file` (CSV fields).
- `uniq`: filter adjacent duplicate lines. Common with `sort`. Example: `sort file | uniq -c` (counts per unique line).

Common pipeline: count and rank values
```
sort items.txt | uniq -c | sort -nr   # frequency sorted descending
```

**Secure / file integrity checksums**
- `md5sum FILE` — compute MD5 hash (fast; not collision‑resistant; useful for quick checks).
- `sha256sum FILE` — compute SHA‑256 (recommended for integrity checks today).
- `sha512sum FILE` — compute SHA‑512 (stronger when desired).

Create checksum file for many files and verify:
```
sha256sum * > SHA256SUMS
sha256sum -c SHA256SUMS   # verifies each file against recorded hash
```

Notes: keep checksum files safe; use signatures (GPG) if you need tamper-proof verification.

**Redirecting output, errors, and pipes**
- `>` : redirect STDOUT to a file (overwrite). Example: `ls > files.txt`.
- `>>`: append STDOUT to a file. Example: `echo x >> out.txt`.
- `2>`: redirect STDERR. Example: `cmd 2>err.log`.
- `2>&1`: send STDERR to where STDOUT is going. Example: `cmd >out.log 2>&1`.
- `&>` (bash): redirect both STDOUT and STDERR: `cmd &>all.log` (portable approach is `>file 2>&1`).
- Pipes: `|` pass STDOUT of one command to STDIN of another. Example: `grep foo file | less`.
- `tee`: duplicate a stream to file and stdout. Example: `cmd | tee out.txt`; append with `tee -a out.txt`.
- Here-documents: feed inline text to stdin.
```
cat <<'EOF' >file.txt
line1
line2
EOF
```
- Process substitution (bash): `<(cmd)` or `>(cmd)` for commands that accept filenames. Example: `diff <(sort a) <(sort b)`.

**Searching & simple editing: `grep` and `sed`**
- `grep`: search lines matching a pattern.
  - `grep 'pattern' file` — basic.
  - Useful flags: `-i` (ignore case), `-r` (recursive), `-n` (show line number), `-E` (extended regex), `-P` (Perl regex).
  - Example: `grep -nE 'error|fail' /var/log/syslog`
- `sed`: stream editor for non-interactive substitutions and transforms.
  - Basic replacement: `sed 's/old/new/g' file` (prints to stdout).
  - Inline edit (with backup): `sed -i.bak 's/old/new/g' file` (creates `file.bak`).
  - Print only matched lines: `sed -n '/pattern/p' file`.

Small examples
- Replace and save:
```
sed -i 's/localhost/127.0.0.1/g' /etc/myapp/config
```
- Find lines and show context with `grep`:
```
grep -nC3 'timeout' /var/log/app.log   # 3 lines context
```

Quick tips
- Prefer `less` over `more` for interactive viewing.
- Use `sort | uniq -c | sort -nr` to get frequency counts.
- Always verify the running kernel with `uname -r` before removing kernel packages.
- When using `sed -i`, test the command without `-i` first.
- Use checksum + GPG signatures for secure release verification.

Further reading
- `man` pages: `man grep`, `man sed`, `man coreutils`.
