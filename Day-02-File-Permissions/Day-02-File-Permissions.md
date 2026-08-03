# Day 02: Linux File Permissions & Access Control

## Overview
Linux is a multi-user operating system that enforces strict file permissions to protect system files and user data. Understanding how to inspect, modify, and audit these permissions is critical for both system hardening and identifying privilege escalation vulnerabilities during security audits.

## Key Concepts Learned
- **Permission Triad**: Access rights are defined for three entity levels: **Owner (u)**, **Group (g)**, and **Others (o)**.
- **Permission Types**:
  - **Read (`r` / Value: 4)**: Permission to view contents.
  - **Write (`w` / Value: 2)**: Permission to modify contents.
  - **Execute (`x` / Value: 1)**: Permission to execute a file/script.
- **Octal (Numeric) Notation**: Combining numeric values to set permissions efficiently (e.g., $4+2+1 = 7$).
- **Redirection Operators**:
  - `>` Overwrites destination file content.
  - `>>` Appends content to the end of the destination file.

## Commands Cheatsheet (With Flags Breakdown)

| Base Command | Options / Arguments | Example Usage | Description |
| :--- | :--- | :--- | :--- |
| `ls` | `-l` | `ls -l script.sh` | Displays long listing including file permissions, owner, and group |
| `chmod` | `755` | `chmod 755 script.sh` | Grants `rwx` (7) to owner and `r-x` (5) to group/others |
| `chmod` | `u+x` | `chmod u+x tool.py` | Adds execute permission specifically for the file owner |
| `chown` | `user:group` | `sudo chown root:root file.txt` | Changes ownership of a file to root user and group |

## TryHackMe Learning Takeaways
- Completed **Linux Fundamentals Part 1 (Task 5 - Shell Operators)** and started **Linux Fundamentals Part 2**.
- Learned how background processes (`&`), command chaining (`&&`), and output redirections (`>`, `>>`) operate within shell sessions.

## What I Messed Up / Got Wrong Today & How I Fixed It
- **Mistake 1 (Quiz):** Misinterpreted `-r-x` permission segment for Group as write/execute instead of read/execute.
  - **Fix:** Remembered that position 1 in the triplet is Read (`r`), position 2 is Write (`w`), and position 3 is Execute (`x`). Hyphens (`-`) represent missing permissions.
- **Mistake 2 (THM Task):** Selected `>` instead of `>>` when asked for the non-overwriting redirector.
  - **Fix:** Reinforced that single arrow `>` wipes and overwrites, while double arrow `>>` appends to existing log or text files.
