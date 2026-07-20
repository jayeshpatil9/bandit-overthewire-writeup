
# Bandit Levels 00 to 05 Write-ups

> Technical documentation and command methodology for OverTheWire Bandit levels 0 through 5.

---

## Level 00 → Level 01

### Objective
Establish an initial remote SSH connection to the game server and retrieve the first password stored in the `readme` file.

### Commands Used
```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
ls
cat readme
```

### Technical Explanation
* SSH Remote Access: `ssh` allows encrypted terminal communication with a remote system. The -p 2220 flag specifies the non-standard SSH port used by OverTheWire.

* File Inspection: `ls` lists files in the current working directory, and cat displays the plain-text content of the readme file.

---

## Level 01 → Level 02

### Objective
Retrieve the password from a file named `-` located in the home directory.

### Commands Used
```bash
ls
cat ./-
```

### Technical Explanation
* Standard Input Handling: Running cat - fails because Linux shells interpret a standalone - as standard input (stdin) rather than a filename.

* Relative Pathing: Using ./- explicitly directs cat to treat - as a filename in the current directory (./), bypassing flag/option parsing.

---

## Level 02 → Level 03

### Objective
Retrieve the password from a file containing spaces in its filename (--spaces in this filename--).

### Commands Used
```bash
ls
cat "./--spaces in this filename--"
```

### Technical Explanation
* Space Escaping: Spaces in filenames act as argument separators in Bash. Enclosing the filename in double quotes ("") forces the shell to parse the entire string as a single argument.

* Option Flag Prevention: Prefixing with ./ prevents tools from confusing leading double dashes (--) with command-line flags.

---

## Level 03 → Level 04

### Objective
Find and read the hidden file located inside the inhere directory.

### Commands Used
```bash
cd inhere
ls -la
cat ./.hidden-file-name
```

### Technical Explanation
* Hidden Files: Files starting with a dot (.) are hidden by default in Linux filesystems and are omitted from standard ls output.

* Extended Directory Listing: The -a (all) flag in ls -la forces the terminal to display all hidden files and directories.

---

## Level 04 → Level 05

### Objective
Identify and read the only human-readable ASCII text file among multiple files inside the inhere directory.

### Commands Used
```bash
cd inhere
ls -la
file ./*
cat ./-file07
```

### Technical Explanation
* MIME/Type Inspection: The file command inspects file headers/magic numbers to determine actual file types regardless of extensions.

* Wildcard Inspection: file ./* inspects every file in the directory. Checking the output identifies the single file classified as ASCII text, which can then be displayed using cat.

---

