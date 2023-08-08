# OMV Startup Sequence Fixer

A fix for OpenMediaVault (OMV) boot sequence issues, particularly concerning SMB and other service disruptions post-reboot.

## Problem Statement

Upon booting OpenMediaVault (OMV), you might encounter issues with SMB and other services not functioning as expected. File systems might not mount properly, and services like Docker, SMB, NFS might experience disruptions. Permission issues might also arise.

## Solution

This script executes a series of commands after every boot to ensure a smoother startup for the server, addressing the aforementioned problems.

## How to Implement

1. **OpenMediaVault Control Panel**:
    - Navigate to the **"Scheduled Jobs"** section.
    - Create a new job.
    - Set the **Time of execution** to **"At reboot"**.
    - Set **User** to **"root"**.
    - In the **Command** field, paste the following:

        ```bash
        mount -o remount, rw /; touch /forcefsck; sudo journalctl; sudo service smbd stop; sudo service smbd restart; sudo systemctl start systemd-resolved; sudo systemctl enable systemd-resolved; systemctl stop containerd; systemctl start containerd; systemctl start docker.service; sudo systemctl enable collectd.service; sudo systemctl start collectd.service; sudo systemctl restart nfs-kernel-server
        ```

    - Ensure the job is **enabled** and set to run.

2. **Via Terminal**:
    - If you can't access the OMV control panel but have terminal access as root, you can manually type in the aforementioned command.

## Caution

Always ensure you understand the commands you're executing, especially as the root user. This solution is provided as-is, and it's crucial to use it at your own discretion and risk. Ensure you have backups and understand the implications of each command.

