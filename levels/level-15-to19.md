# Bandit Levels 15 to 19 Write-ups

> Technical documentation and command methodology for OverTheWire Bandit levels 15 through 19.

---

## Level 15 → Level 16

### Objective
Transmit the current `bandit15` password to a listening service on port 30001 using SSL/TLS encryption.

### Commands Used
```bash

# Option 1: OpenSSL Client
openssl s_client -connect localhost:30001 -quiet

# Option 2: Ncat with SSL
ncat --ssl localhost 30001

```
### Technical Explanation
* Encrypted Network Sockets: Unlike nc (Netcat), which transmits plain text over raw TCP sockets, openssl s_client and ncat --ssl negotiate a TLS/SSL handshake to establish an encrypted tunnel before data transmission.

* Quiet Mode: The -quiet flag in openssl suppresses certificate metadata and handshake details, presenting a clean interactive session for submitting the password.

---

## Level 16 → Level 17

### Objective
Scan ports 31000 through 32000 to identify which service speaks SSL/TLS, submit the bandit16 password, and capture the returned private SSH key.

### Commands Used
```bash

# Scan port range to locate SSL/TLS listening services
nmap -sV -p 31000-32000 localhost

# Connect to the identified SSL service (port 31790)
openssl s_client -connect localhost:31790 -quiet -ign_eof

# Save the returned RSA private key on local machine
cat << 'EOF' > bandit17.key
-----BEGIN RSA PRIVATE KEY-----
[Extracted Private Key Content]
-----END RSA PRIVATE KEY-----
EOF

# Set strict permissions and authenticate to Level 17
chmod 600 bandit17.key
ssh -i bandit17.key bandit17@bandit.labs.overthewire.org -p 2220

```
### Technical Explanation
* Service Enumeration: nmap -sV performs service and version detection across the specified port range (-p 31000-32000) to differentiate between generic echo daemons and valid TLS credentials listeners.
  
* Connection Persistence: The -ign_eof flag prevents openssl s_client from closing the connection when standard input reaches end-of-file, ensuring the remote daemon has enough time to return the private key payload.
  
---

## Level 17 → Level 18

### Objective
Identify the only modified line of text between passwords.old and passwords.new in the home directory.

### Commands Used
```bash

diff passwords.old passwords.new

```
### Technical Explanation  
* File Comparison: The diff utility performs line-by-line comparison between two text files. Lines prefixed with > represent insertions/changes found in the new file, quickly revealing the updated password without manual inspection.
  
---

## Level 18 → Level 19

### Objective
Log into bandit18 and read the readme file while bypassing a modified .bashrc file configured to log the user out immediately upon login.

### Commands Used
```bash

ssh bandit18@bandit.labs.overthewire.org -p 2220 'cat readme'

```
### Technical Explanation  
* Direct SSH Command Execution: Appending a command (e.g., 'cat readme') to the ssh invocation instructs the server to execute that specific non-interactive command directly instead of spawning an interactive login shell. This completely bypasses profile scripts like .bashrc and .bash_profile.   

---

## Level 19 → Level 20

### Objective
Exploit a SetUID binary (bandit20-do) to execute commands with elevated bandit20 privileges and retrieve the password file.

### Commands Used
```bash

./bandit20-do cat /etc/bandit_pass/bandit20

```
### Technical Explanation  
* SetUID (SUID) Privilege Execution: SUID permissions allow an executable to run with the effective permissions of the file owner rather than the user executing it. Because bandit20-do is owned by bandit20 with the SUID bit enabled, passing cat /etc/bandit_pass/bandit20 as an argument reads the protected password file directly.

---
