# Day 05: Level 1 Boss Practical Challenge (System Audit & Security Hardening)

## Overview
Day 05 focused on applying foundational concepts to a real-world enterprise Incident Response scenario. Acting as a Systems Security Engineer auditing a compromised host, this hands-on lab covered auditing active/system users, analyzing privilege delegation via groups (`sudo`/`wheel`), inspecting high-risk shared spaces (`/tmp`), and hardening sensitive log file permissions using Discretionary Access Control (DAC) models.

## Key Concepts Learned
- **System Account Partitioning**: Linux assigns ranges based on UID structures:
  - `UID 0`: Root / Superuser with unrestricted kernel access.
  - `UID 1-999`: System service accounts (e.g., `redis`, `mysql`) provisioned with non-interactive shells (`/usr/sbin/nologin`).
  - `UID 1000+`: Human/Interactive users (e.g., `kali`, `hassan-dev`).
- **Extended Regular Expression Auditing**: Utilizing `grep -E` allows multi-pattern evaluation (e.g., searching for `sudo` OR `wheel` within `/etc/group` in a single command execution).
- **Sticky Bit Dynamics**: The `/tmp` directory permissions (`drwxrwxrwt`) enforce the Sticky Bit (`t`), ensuring users can create files within the directory but cannot remove or overwrite files owned by other users.
- **Access Isolation Testing**: Switching session context using `su - <user>` validates DAC policies, confirming that unauthorized users receive `Permission denied` when trying to read or modify permissions (`chmod`) on restricted files (`600`).

## Commands Cheatsheet (With Flags Breakdown)

| Base Command | Flags / Options | Combined Command | Description | Example Usage |
| :--- | :--- | :--- | :--- | :--- |
| `tail` | `-n 10` | `tail -n 10 /etc/passwd` | Displays the final 10 lines of the system password database. | `tail -n 10 /etc/passwd` |
| `grep` | `-E` | `grep -E 'sudo\|wheel' /etc/group` | Executes extended regex matching multiple administrative target groups. | `grep -E 'sudo\|wheel' /etc/group` |
| `ls` | `-la` | `ls -la /tmp` | Lists all files (including hidden) with full permission bit attributes. | `ls -la /tmp` |
| `chmod` | Absolute (Octal) | `chmod 600 <file>` | Hardens target file so only the owner possesses read/write privileges. | `chmod 600 /tmp/security_audit.log` |
| `su` | `-` | `su - hassan-dev` | Switches user context and completely initializes the target user's shell environment. | `su - hassan-dev` |
| `whoami` | None | `whoami` | Prints the effective user identity associated with the active shell session. | `whoami` |

## Practical Learning Takeaways
- Audited `/etc/passwd` to classify system service UIDs vs human accounts (`UID 1000+`).
- Checked `/etc/group` with `grep -E` to confirm that privileged escalation pathways were limited strictly to authorized users (`kali`).
- Created a simulated sensitive security audit log (`/tmp/security_audit.log`) and applied `chmod 600` to prevent unauthorized inspection.
- Switched session context to `hassan-dev` via `su -` and verified that non-owners are blocked from both reading (`cat`) and modifying ACLs (`chmod`) on hardened files.

## What I Messed Up / Got Wrong Today & How I Fixed It
- **Conflating UIDs and GIDs**: Mixed up user IDs with group IDs when reviewing human account ranges (`1000+`).
  - *Resolution*: Clarified that numeric `UID 1000+` strictly identifies human user accounts, while `GIDs` are separate numbers mapping group access levels inside `/etc/group`.
- **Misunderstanding `grep -E` Utility**: Questioned why normal `grep` wasn't used for checking multiple group names.
  - *Resolution*: Learned that `-E` enables Extended Regular Expressions, allowing the logical OR (`|`) pipe operator to match multiple strings in one go (`sudo|wheel`).
