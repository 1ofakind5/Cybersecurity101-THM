# 🐧 Linux System Administration & Defense Fundamentals

A technical reference guide for navigating the Linux operating system, managing files, and auditing system configurations from a defensive security perspective.

---

## 1. Directory Navigation & System Awareness
As a defender, quickly orienting yourself within a compromised or audited system is critical.

* `pwd` - **Print Working Directory:** Displays the absolute path of the current directory.
* `ls` - **List:** Lists directory contents.
  * `ls -la` - Reveals hidden files (files starting with a `.`) and displays detailed file permissions, owners, and sizes.
* `cd <dir>` - **Change Directory:** Moves to a specified folder.
* `whoami` - Displays the current logged-in user identity (essential for verifying privilege levels).

---

## 2. File & Directory Management
Commands used to inspect, modify, and manage configuration files or potential artifacts.

* `cat <file>` - Concatenates and displays the entire content of a file.
* `less <file>` - Displays file content one page at a time (excellent for scanning massive log files).
* `touch <file>` - Creates an empty file or updates the timestamp of an existing file.
* `mkdir <name>` - Creates a new directory.
* `cp <source> <dest>` - Copies files or directories.
* `mv <source> <dest>` - Moves or renames files or directories.
* `rm <file>` - Removes a file. (`rm -r` recursively deletes a directory).

---

## 3. Searching & Filtering Data (Log Auditing)
Defenders spend hours searching through system logs (`/var/log`). These commands isolate specific malicious patterns or indicators of compromise (IoCs).

* `grep "<pattern>" <file>` - Searches for a specific string within a file.
  * *Example:* `grep "Failed password" /var/log/auth.log` (Finds brute-force login attempts).
* `find <path> -name <filename>` - Searches the filesystem for specific files or extensions.
* `wc -l <file>` - Counts the total number of lines in a file (useful for tracking log volume anomalies).

---

## 4. File Permissions & Security Ownership
Linux security relies heavily on file permissions. Understanding who can read, write, or execute a file prevents unauthorized access.

### Reading Permissions (`ls -l` output):
Permissions are broken down into three blocks: **User (Owner)**, **Group**, and **Others** (e.g., `-rwxr-xr--`).
* `r` (Read) = 4
* `w` (Write) = 2
* `x` (Execute) = 1

### Management Commands:
* `chmod <permissions> <file>` - Changes file read/write/execute permissions (e.g., `chmod 755 script.sh`).
* `chown <user>:<group> <file>` - Changes the owner and group ownership of a system file.
