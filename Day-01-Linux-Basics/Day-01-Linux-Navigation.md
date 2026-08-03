# Day 01: Linux File System & Basic CLI Navigation

## Overview
Understanding the Linux file system hierarchy and basic Command Line Interface (CLI) navigation is the foundational step in system administration and penetration testing. Linux utilizes a single inverted tree structure originating from the root directory (`/`), unlike drive-letter based systems like Windows.

## Key Concepts Learned
- **Root Directory (`/`) vs Home Directory (`~`)**: The root directory is the topmost level of the entire file system, whereas `~` represents the logged-in user's personal home folder (e.g., `/home/kali`).
- **Hidden Files in Linux**: Files preceded by a dot (`.filename`) are hidden by default in the file system to prevent clutter and accidental modifications.
- **Directory Traversal**: Using relative paths (`..`) allows navigation one level up in the directory tree.
- **Content Inspection & Searching**: Basic awareness of viewing files (`cat`) and searching specific text strings (`grep`).

## Commands Cheatsheet (With Flags Breakdown)

| Base Command | Flags / Options | Combined Command | Description | Example Usage |
| :--- | :--- | :--- | :--- | :--- |
| `pwd` | N/A | `pwd` | Displays current working directory path | `pwd` |
| `whoami` | N/A | `whoami` | Displays current active username | `whoami` |
| `ls` | `-a` (all including hidden)<br>`-l` (long format) | `ls -la` | Lists all files including hidden ones with metadata | `ls -la` |
| `cd` | `..` (parent dir)<br>`~` (home dir)<br>`/` (root dir) | `cd ..`<br>`cd ~`<br>`cd /` | Navigates between directory paths | `cd /home/kali` |
| `cat` | N/A | `cat` | Displays file contents to stdout | `cat access.log` |
| `grep` | N/A | `grep` | Searches for specific text inside files | `grep "THM" access.log` |

## TryHackMe Learning Takeaways
- Successfully completed **Tasks 1 to 4** in the **Linux Fundamentals Part 1** room.
- Hands-on practice extracting hidden flags using `grep "THM"` inside server log files (`access.log`).
- Practiced fluid navigation between terminal paths without relying on GUI mouse interactions.

## What I Messed Up / Got Wrong Today & How I Fixed It
- **Initial Confusion:** Understanding the structural difference between system root (`/`) and user home (`~`).
- **Resolution:** Verified using `pwd` after executing `cd /` and `cd ~` to observe the absolute paths (`/` vs `/home/kali`).
- **Additional Tools Encountered:** Introduced to `grep` and `cat` earlier than planned via THM tasks; reinforced that `cat` reads full files while `grep` filters specific patterns.
