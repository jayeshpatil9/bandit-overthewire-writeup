# Bandit Levels 11 to 15 Write-ups

> Technical documentation and command methodology for OverTheWire Bandit levels 11 through 15.

---

## Level 10 → Level 11

### Objective
Decode the Base64-encoded credentials stored inside `data.txt`.

### Commands Used
```bash

base64 -d data.txt

```
### Technical Explanation
* Base64 Decoding: The -d (decode) flag tells the base64 utility to decode ASCII armor data back into its original plain-text representation.

---

## Level 11 → Level 12

### Objective
Decrypt the password stored in data.txt that has been rotated by 13 positions using the ROT13 cipher.

### Commands Used
```bash

cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'

```
### Technical Explanation
*Character Translation: tr replaces characters from a source set with the corresponding characters of a target set. Shifting the alphabet mapping forward by 13 positions (A-Z to N-ZA-M) reverses the symmetric ROT13 cipher.

---

## Level 12 → Level 13

### Objective
Perform reverse hex dump translation and iteratively decompress nested archive formats (gzip, bzip2, tar) to retrieve the password.

### Commands Used
```bash

# Setup a writable workspace
mkdir -p /tmp/workspace && cp data.txt /tmp/workspace/ && cd /tmp/workspace

# Reverse hex dump
xxd -r data.txt > data_stage1

# Inspect file type and iteratively decompress
file data_stage1
mv data_stage1 data_stage1.gz && gzip -d data_stage1.gz

file data_stage1
mv data_stage1 data_stage1.bz2 && bzip2 -d data_stage1.bz2

file data_stage1
mv data_stage1 data_stage1.gz && gzip -d data_stage1.gz

file data_stage1
tar -xf data_stage1

file data5.bin
tar -xf data5.bin

file data6.bin
mv data6.bin data6.bin.bz2 && bzip2 -d data6.bin.bz2

file data6.bin
tar -xf data6.bin

file data8.bin
mv data8.bin data8.bin.gz && gzip -d data8.bin.gz

file data8.bin
cat data8.bin

```
### Technical Explanation
* Hex Reversal: xxd -r converts a plain-text hexadecimal dump back into its raw binary format.
  
* Format-Specific Decompression: Utilities like gzip and bzip2 require standard file extensions (.gz, .bz2) to decompress files. Using file at each step identifies the container type regardless of the current extension.
  
---

## Level 13 → Level 14

### Objective
Authenticate as bandit14 using the private SSH key stored on the server (sshkey.private).

### Commands Used
```bash

# Option 1: Direct localhost connection
ssh -i sshkey.private bandit14@localhost -p 2220

# Option 2: Local machine execution
chmod 600 local_bandit_key
ssh -i local_bandit_key bandit14@bandit.labs.overthewire.org -p 2220

# Read credentials
cat /etc/bandit_pass/bandit14

```
### Technical Explanation
* Key-Based Authentication: The -i flag supplies an RSA private key file for asymmetric cryptographic authentication instead of a standard password.

* Strict Key Permissions: OpenSSH mandates that private key files are readable solely by the owner (chmod 600); otherwise, the client rejects the key due to insecurity.

---

## Level 14 → Level 15

### Objective
Transmit the current bandit14 password to a listening service on network port 30000 to retrieve the bandit15 password.

### Commands Used
```bash

nc localhost 30000
# Submit current bandit14 password into the active socket

```
### Technical Explanation
* Network Socket Interaction: nc (Netcat) opens a raw TCP socket connection to a specified host and port (localhost:30000).

*Interactive Standard Input: Once the TCP handshake is complete, entering text directly into the open terminal session sends the payload over the network stream to the remote daemon.

---
