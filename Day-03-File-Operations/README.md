# Module Overview: Day 03 - File Operations & Text Viewing

## Module Summary
This module covers fundamental file operations (`touch`, `cp`, `mv`, `rm`) and text manipulation utilities (`cat`, `less`, `head`, `tail`, `nano`). It details Virtual File System (VFS) Inode mapping, atomic file movement, unlinking mechanics, and real-time security log streaming.

## Files in this Directory
- **Day-03-File-Operations.md**: Detailed notes, command cheatsheet, and practical task reflections.
- **README.md**: Overview and summary of Day 03 learning objectives.

## Key Takeaways
- **Inode Pointer Stability**: `mv` updates directory entry mappings without altering Inode numbers, whereas `cp` allocates entirely new Inodes and data blocks.
- **Unlinking vs Sanitization**: `rm` triggers `unlinkat()`. Data blocks are freed only after the hard link count drops to zero and all open File Descriptors close.
- **Real-Time Log Analysis**: `tail -f` utilizes kernel `inotify` subsystems to inspect expanding log files dynamically.
