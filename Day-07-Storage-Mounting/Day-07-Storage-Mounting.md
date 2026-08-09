# Day 07: Mount Operations & Persistent Storage (/etc/fstab)

## Overview
Day 07 focused on temporary vs. persistent storage mounting mechanisms in Linux environments. Hands-on labs covered manual mounting/unmounting workflows, analyzing `/etc/fstab` configuration structures, provisioning persistent loop mounts, and using validation tools (`mount -a`, `systemctl daemon-reload`) to avoid boot-time storage failures.

## Key Concepts Learned
- **Mount Dynamics**: Volatile mounts configured via CLI do not persist across system reboots.
- **Persistence Engineering (`/etc/fstab`)**: The Linux kernel reads `/etc/fstab` during initialization to attach persistent block storage.
- **Safety Validations**: Executing `sudo mount -a` dry-runs `/etc/fstab` configurations without triggering system reboots, shielding against syntax errors that cause boot crashes.

## Commands Cheatsheet

| Base Command | Description | Example Usage |
| :--- | :--- | :--- |
| `mount` | Connects a formatted device or image to a filesystem directory. | `sudo mount /tmp/disk1.img /mnt/test-disk` |
| `umount` | Safely detaches a device from a mount target. | `sudo umount /mnt/test-disk` |
| `mount -a` | Mounts all filesystems listed in `/etc/fstab`. Used as a safety validation tool. | `sudo mount -a` |
| `/etc/fstab` | File System Table config file defining boot-time mount targets. | `cat /etc/fstab` |
| `systemctl daemon-reload` | Notifies systemd to refresh internal configuration trees after editing fstab. | `sudo systemctl daemon-reload` |

## Practical Learning Takeaways
- Remounted virtual storage `/tmp/disk1.img` onto `/mnt/test-disk`.
- Configured a persistent entry inside `/etc/fstab`:
  `/tmp/disk1.img /mnt/test-disk ext4 defaults 0 0`
- Handled syntax errors in `/etc/fstab` using `sudo nano /etc/fstab` and verified stability using `sudo systemctl daemon-reload` and `sudo mount -a`.

## Troubleshooting Experience
- **Issue**: `mount: <dump>: unknown filesystem type` error when executing `mount -a`.
- **Root Cause**: Uncommented non-standard metadata text inside `/etc/fstab`.
- **Resolution**: Removed broken line using `nano` (`Ctrl + K`), reloaded systemd daemon (`sudo systemctl daemon-reload`), and verified clean execution via `sudo mount -a`.
