# Module Overview: Day 05 - Level 1 Boss Challenge

## Module Summary
This module simulates a live security audit and system hardening operation. It covers identity auditing (`/etc/passwd`), checking privileged groups (`sudo`/`wheel`), inspecting temporary directories (`/tmp` Sticky Bit mechanics), applying explicit file-level hardening (`chmod 600`), and validating multi-user isolation using `su`.

## Files in this Directory
- **Day-05-Boss-Challenge.md**: Comprehensive notes, auditing command breakdown, and practical incident reflection.
- **README.md**: Overview and summary of Day 05 objectives.

## Key Takeaways
- **Least Privilege Access**: Files containing audit or system logs must be restricted (`chmod 600`) to prevent data leakage across local user accounts.
- **Environment Context Switching**: Using `su - <user>` provides a safe, realistic method to verify user permission boundaries without logging off the system.
