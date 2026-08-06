# Day 04: User & Group Administration

## Overview
Day 04 covers identity isolation, access control, and account management using `useradd`, `usermod`, `groupadd`, `userdel`, and inspection tools like `passwd` and `id`. In Linux system engineering, managing user identities relies on numeric UIDs and GIDs processed by the kernel. Administrators configure system security by managing authentication databases (`/etc/passwd`, `/etc/shadow`, and `/etc/group`) to enforce least-privilege access across enterprise environments.

## Key Concepts Learned
- **Kernel User Mapping (UID/GID)**: The Linux kernel identifies entities by numeric User IDs (UID) and Group IDs (GID) rather than human-readable usernames. Root always retains UID 0.
- **Identity & Auth Storage**: `/etc/passwd` maintains world-readable identity metadata (home directory, shell, primary GID), while `/etc/shadow` securely stores cryptographic password hashes (`yescrypt`/`SHA-512`) restricted to root access.
- **Group Access Control**: Primary groups set default file ownership upon creation, while secondary groups grant supplemental access permissions across shared project resources.
- **Safe Group Assignment (`-aG`)**: Appending secondary groups requires `-aG`. Omitting the `-a` flag strips all existing secondary memberships, overwriting user access rights.

## Commands Cheatsheet (With Flags Breakdown)

| Base Command | Flags / Options | Combined Command | Description | Example Usage |
| :--- | :--- | :--- | :--- | :--- |
| `groupadd` | None | `groupadd devops` | Allocates a new group entry and GID in `/etc/group`. | `sudo groupadd devs` |
| `useradd` | `-m`, `-s`, `-g` | `useradd -m -s /bin/bash -g devs user1` | Creates account, generates `/home/user`, sets login shell, and assigns primary GID. | `sudo useradd -m -s /bin/bash -g devs hassan-dev` |
| `passwd` | None | `passwd user1` | Updates password hash inside `/etc/shadow`. | `sudo passwd hassan-dev` |
| `usermod` | `-aG` | `usermod -aG ops user1` | Appends a secondary group without removing existing group memberships. | `sudo usermod -aG ops hassan-dev` |
| `usermod` | `-L` / `-U` | `usermod -L user1` | Locks (`!`) or unlocks user authentication in `/etc/shadow`. | `sudo usermod -L hassan-dev` |
| `userdel` | `-r` | `userdel -r user1` | Removes user account along with home directory and mail spool. | `sudo userdel -r hassan-dev` |
| `id` | None | `id user1` | Prints resolved UID, primary GID, and secondary group lists. | `id hassan-dev` |

## Practical Learning Takeaways
- Created test groups (`devs`, `ops`) and provisioned user `hassan-dev` with a custom home directory and bash shell interface.
- Inspected `/etc/passwd` to verify standard identity format (`hassan-dev:x:1001:1001:...`) and verified hashed secrets in `/etc/shadow` using `grep`.
- Executed `usermod -aG` to append secondary membership (`ops`) and confirmed expanded group privileges via `id hassan-dev`.
- Simulated account cleanup by removing test accounts and directories with `userdel -r`.

## What I Messed Up / Got Wrong Today & How I Fixed It
- **Initial Confusion: String Matching in Inspection Commands**: Executed `grep "hassan_dev" /etc/passwd` and received blank output.
  - *Resolution*: Realized Linux string searching is exact and case-sensitive. Corrected target username from underscore (`hassan_dev`) to hyphen (`hassan-dev`), successfully displaying the entry.
- **Initial Confusion: Lock Errors on System Files**: Attempted `groupdel devops` without elevated privileges and received `Permission denied / cannot lock /etc/group`.
  - *Resolution*: Recognized that system account files are protected root resources. Prefixing execution with `sudo` acquired necessary write locks.
- **Risk of Group Overwrite**: Understood that executing `usermod -G` without `-a` clears all previous secondary group allocations. Used `-aG` to append memberships safely.
