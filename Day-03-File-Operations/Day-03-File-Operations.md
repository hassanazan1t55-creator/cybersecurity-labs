# Day 03: File Operations & Text Viewing

## Overview
Day 03 focuses on fundamental file manipulation (`touch`, `cp`, `mv`, `rm`) and text viewing utilities (`cat`, `less`, `head`, `tail`, `nano`). In Linux enterprise administration, these operations interact directly with the Virtual File System (VFS), Inode metadata tracking, system call streams (`openat()`, `read()`, `write()`, `unlinkat()`), and real-time process file descriptors (`stdin`, `stdout`, `stderr`) to maintain system state and audit security logs.

## Key Concepts Learned
- **Inode Allocation & Metadata**: Filenames act merely as human-readable directory entries (dentries) pointing to an Inode number. Creating files (`touch` via `openat`) allocates metadata, whereas copying (`cp`) duplicates raw data blocks onto a newly allocated Inode.
- **Atomic Renaming vs Data Copying (`mv` vs `cp`)**: Moving files across the same filesystem calls `renameat2()`, rewriting directory mapping pointers without touching physical storage blocks or modifying the underlying Inode number.
- **Unlinking & Inode Garbage Collection (`rm`)**: File deletion invokes `unlinkat()`, decrementing the Inode link counter by 1. Disk sectors are marked as free only when the link count reaches zero AND all open process file descriptors referencing it are closed.
- **Kernel-Backed Live Log Streaming**: Utilizing `tail -f` attaches kernel event monitors (`inotify_add_watch`) to track appending log buffers dynamically without locking the underlying file descriptor.

## Commands Cheatsheet (With Flags Breakdown)

| Base Command | Flags / Options | Combined Command | Description | Example Usage |
| :--- | :--- | :--- | :--- | :--- |
| `touch` | `-t` (custom timestamp) | `touch -t 202601011200 file.txt` | Creates an empty file or updates access/modification timestamps (`utimensat`). | `touch system.log` |
| `ls` | `-i` (inode) | `ls -i file.txt` | Prints the unique VFS Inode index number assigned to a file. | `ls -i original_file.txt` |
| `cp` | `-v` (verbose), `-r` (recursive) | `cp -vr source/ dest/` | Duplicates data blocks to a fresh Inode with execution feedback. | `cp original.txt copy.txt` |
| `mv` | `-v` (verbose), `-n` (no-clobber) | `mv -vn old.txt new.txt` | Relocates or renames a directory entry while retaining the original Inode. | `mv copy.txt renamed.txt` |
| `rm` | `-v` (verbose), `-f` (force) | `rm -vf file.txt` | Executes `unlinkat()` to decrement the Inode hard link counter. | `rm -v renamed.txt` |
| `head` | `-n` (line count) | `head -n 5 app.log` | Outputs the top $N$ lines from a stream to standard output. | `head -n 3 app.log` |
| `tail` | `-f` (follow), `-n` (lines) | `tail -fn 20 app.log` | Streams freshly appended data via `inotify` file watching. | `tail -f app.log` |
| `cat` | `-n` (number lines) | `cat -n config.conf` | Reads bytes sequentially into buffer and outputs to stdout. | `cat -n app.log` |
| `nano` | None | `nano config.conf` | Launches a light terminal editor for adjusting configuration files. | `nano server.conf` |

## Practical Learning Takeaways
- Executed `touch` and tracked file creation using `ls -i` to visually confirm that `cp` assigns a completely new Inode, whereas `mv` retains the existing Inode integer.
- Generated sample multi-line log streams using `echo` and filtered top and bottom segments using `head -n` and `tail -n`.
- Initiated a live background log stream via `tail -f &`, appended fresh attack log entries to `app.log`, and observed real-time terminal stdout updates.
- Deleted test files using `rm -v` and verified how kernel unlinking decouples human file paths from underlying disk storage.

## What I Messed Up / Got Wrong Today & How I Fixed It
- **Initial Confusion: How `rm` Handles Data Removal**: Assumed that running `rm` immediately zeroes out raw data blocks from physical disk sectors.
  - *Resolution*: Learned that `rm` only issues `unlinkat()` to reduce the Inode link counter. If a running process holds an active File Descriptor, the data blocks persist and remain inspectable under `/proc/[PID]/fd/`.
- **Initial Confusion: Inode Mechanics During File Rename**: Thought moving a large file physically copies disk blocks to a new sector.
  - *Resolution*: Verified via `ls -i` that moving files across the same disk partition uses `renameat2()` to update dentry pointers only, leaving the Inode integer unchanged and making moves instant.
- **Terminal Lockup During `tail -f`**: Got locked inside the interactive output window after launching `tail -f`.
  - *Resolution*: Used `Ctrl + C` to interrupt the foreground loop, or ran `tail -f app.log &` in the background to keep the active shell prompt usable.
