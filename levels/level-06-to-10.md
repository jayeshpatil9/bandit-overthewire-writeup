# Bandit Levels 06 to 10 Write-ups

> Technical documentation and command methodology for OverTheWire Bandit levels 6 through 10.

---

## Level 05 → Level 06

### Objective
Find a file inside the `inhere` directory that matches specific properties: human-readable, exactly 1033 bytes in size, and non-executable.

### Commands Used
```bash

find inhere -type f -size 1033c ! -executable

cat inhere/maybehere07/.file2
```
### Technical Explanation
* Filtered Search: The find command locates files matching precise constraints: -type f limits the search to regular files, -size 1033c searches for an exact byte count (c stands for bytes), and ! -executable excludes any file with execution permissions.

---

## Level 06 → Level 07

### Objective
Locate a file somewhere on the entire server owned by user bandit7, group bandit6, and exactly 33 bytes in size.

### Commands Used
```bash

find / -user bandit7 -group bandit6 -size 33c 2>/dev/null

cat /var/lib/dpkg/info/bandit7.password

```

### Technical Explanation
* Root Filesystem Searching: Searching from the root directory (/) traverses all system mounts and directories for specific user/group metadata.

* Error Redirection: Standard searches across / produce numerous "Permission denied" errors. Appending 2>/dev/null redirects stderr (file descriptor 2) to the null device, keeping the terminal output clean and showing only readable results.
  
---

## Level 07 → Level 08

### Objective
Extract the password stored in data.txt next to the keyword millionth.

### Commands Used
```bash

grep "millionth" data.txt

```

### Technical Explanation
* Pattern Matching with grep: grep (Global Regular Expression Print) searches plain-text datasets for lines matching a specified regular expression or string, avoiding the need to manually read massive text files.
  
---

## Level 08 → Level 09

### Objective

Find the only line of text inside data.txt that occurs exactly once among hundreds of duplicate lines.

### Commands Used
```bash

sort data.txt | uniq -u

```

### Technical Explanation
* Command Pipelining (|): The pipe operator redirects the standard output (stdout) of the sort command directly into the standard input (stdin) of uniq.

* Deduplication Requirement: The uniq tool only detects adjacent duplicate lines. Running sort first arranges duplicate entries sequentially, allowing uniq -u to filter out all duplicates and print only the unique line.
  
---

## Level 09 → Level 10

### Objective
Extract human-readable ASCII text from the binary file data.txt preceded by several = characters.

### Commands Used
```bash

strings data.txt | grep "==="

```

### Technical Explanation
* Binary String Extraction: The strings utility parses non-text/binary files and returns sequences of printable characters of at least 4 characters in length.

* Piped Pattern Filter: Piping the output to grep "===" isolates the specific password pattern from the rest of the extracted text.
* 
---
