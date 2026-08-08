# Day 06: Storage, Disks & File System Management

## Overview
Day 06 marks the entry into Level 2 (Storage & Disk Management). Today's hands-on lab focused on inspecting physical and virtual block devices, evaluating partition structures, auditing disk space usage at system and directory levels, creating raw virtual block storage, formatting filesystems (`ext4`), and executing manual mount operations onto target filesystem trees.

## Key Concepts Learned
- **Block Device Enumeration**: `lsblk` displays high-level storage hierarchies, while `fdisk -l` provides detailed sector-level metadata (MBR vs GPT partition labels).
- **Disk Space Auditing (`df` vs `du`)**:
  - `df -h`: Evaluates overall file system capacity, usage percentage, and active mount points.
  - `du -sh`: Calculates actual disk space consumed by specific individual files or directories.
- **Virtual Block Creation (`dd`)**: Raw dummy block images can be provisioned using stream generators like `/dev/zero`.
- **Filesystem Provisioning & Mounting**: Formatting raw block devices with `mkfs.ext4` initializes journal systems and inode tables. Using `mount` attaches the raw block device to a directory (e.g., `/mnt/test-disk`), exposed via Linux loop devices (`/dev/loopX`).

## Commands Cheatsheet (With Flags Breakdown)

| Base Command | Flags / Options | Combined Command | Description | Example Usage |
| :--- | :--- | :--- | :--- | :--- |
| `lsblk` | None | `lsblk` | Lists information about all available block storage devices. | `lsblk` |
| `fdisk` | `-l` | `sudo fdisk -l /dev/sda` | Lists sector details, partition sizes, and partition table types. | `sudo fdisk -l /dev/sda` |
| `df` | `-h` | `df -h` | Displays filesystem disk space usage in human-readable units (GB/MB). | `df -h` |
| `du` | `-sh` | `sudo du -sh /var` | Summarizes total disk usage for a specific folder in human-readable format. | `sudo du -sh /opt/project_alpha` |
| `du` | `-h --max-depth=1` | `sudo du -h --max-depth=1 /var` | Shows top-level folder usage up to a depth of 1 directory down. | `sudo du -h --max-depth=1 /var 2>/dev/null` |
| `dd` | `if, of, bs, count` | `dd if=/dev/zero of=...` | Creates raw block files using byte stream copies. | `sudo dd if=/dev/zero of=/tmp/disk1.img bs=1M count=100` |
| `mkfs.ext4` | None | `sudo mkfs.ext4 <file>` | Constructs an ext4 filesystem structure (inodes, journal, superblocks). | `sudo mkfs.ext4 /tmp/disk1.img` |
| `mount` | None | `sudo mount <device> <target>` | Attaches a block device / image to an active directory tree point. | `sudo mount /tmp/disk1.img /mnt/test-disk` |

## Practical Learning Takeaways
- Checked primary drive `/dev/sda1` layout using `lsblk` and verified MBR (`dos`) labeling via `fdisk -l`.
- Calculated `/var` directory storage metrics using `du -h --max-depth=1 /var 2>/dev/null`, suppressing standard error output via `2>/dev/null`.
- Provisioned a 100MB dummy storage image at `/tmp/disk1.img` using `dd`.
- Formatted the block image to `ext4` via `mkfs.ext4` and mounted it to `/mnt/test-disk`, successfully establishing a virtual loop storage device (`/dev/loop0`).

## What I Messed Up / Got Wrong Today & How I Fixed It
- **Running `fdisk` Without Block Target**: Executed bare `fdisk` which returned `fdisk: bad usage`.
  - *Resolution*: Learned that `fdisk` requires an explicit device path argument alongside administrative privileges (e.g., `sudo fdisk -l /dev/sda`).
- **Path Naming Discrepancies**: Accidentally typed `/mnt/test-disk` instead of `/mnt/test_disk`.
  - *Resolution*: Recognized that Linux directory paths are strict; creating `/mnt/test-disk` via `mkdir` ensures clean mounting without path mismatch errors.
