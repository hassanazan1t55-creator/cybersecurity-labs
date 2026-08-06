# Module Overview: Day 04 - User & Group Administration

## Module Summary
This module covers user and group administration (`useradd`, `usermod`, `groupadd`, `userdel`, `id`, `passwd`). It explores Linux account mechanics, identity databases (`/etc/passwd`, `/etc/shadow`, `/etc/group`), numeric UID/GID mappings, and safe privilege management using group assignments.

## Files in this Directory
- **Day-04-User-Admin.md**: Detailed notes, command cheatsheet, and practical task reflections.
- **README.md**: Overview and summary of Day 04 learning objectives.

## Key Takeaways
- **Numeric Identity Processing**: The kernel evaluates user privileges based on numeric UIDs and GIDs rather than text usernames.
- **Authentication Protection**: `/etc/passwd` stores public account definitions, whereas sensitive password hashes are protected within root-only `/etc/shadow`.
- **Safe Group Modification**: Always apply `usermod -aG` when assigning secondary groups to preserve existing user group memberships.
