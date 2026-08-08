# Module Overview: Day 06 - Storage & Disk Management

## Module Summary
This module introduces core concepts in Linux Storage Engineering. It covers disk enumeration (`lsblk`, `fdisk`), disk space diagnostic operations (`df`, `du`), virtual block creation (`dd`), filesystem initialization (`mkfs.ext4`), and mount point mapping.

## Files in this Directory
- **Day-06-Storage-Management.md**: Comprehensive notes, storage commands, flag breakdowns, and practical disk provisioning steps.
- **README.md**: Overview of Day 06 storage tasks.

## Key Takeaways
- **Diagnostics First**: Use `df -h` to assess overall filesystem capacity and `du -sh` to isolate heavy consumer folders.
- **Block Formatting**: Raw disks require an explicit filesystem (`ext4`, `xfs`) before the OS kernel can perform file read/write operations via mount points.
