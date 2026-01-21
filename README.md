Syncthing Podman User Quadlet
=============================

**Disclaimer:** I am not affiliated with, associated, authorized, endorsed by, or in any way officially connected with the Syncthing project.  Back up important data before installing.

This guide demonstrates how to deploy **Syncthing** as a **User (Rootless) Quadlet** on Ubuntu 25.10. Syncthing is a continuous file synchronization program that replaces proprietary sync and cloud services.

Why UserNS=keep-id?
-------------------

**Permissions:** In rootless Podman, containers usually run as a different user ID inside the container. Using `UserNS=keep-id` ensures that the container sees your files as being owned by "you," allowing Syncthing to sync your home directory without "Permission Denied" errors.

System Information
------------------

*   **Tested Environment:** Ubuntu 25.10
*   **Podman Version:** 5.4.x
*   **Type:** User-level Quadlet (Rootless)

1\. The Quadlet File (syncthing.container)
------------------------------------------

This configuration is set to `Network=host`, which is the recommended way to run Syncthing. It allows the service to easily discover other devices on your local network using UPnP/DLNA.

2\. Installation & Setup
------------------------

### Step A: Prepare Directories

Ensure your config and local storage paths exist:

`mkdir -p ~/.config/containers/systemd`
`mkdir -p ~/syncthing/config`

### Step B: Move and Edit

Move the file to the user-level systemd directory and update your external mount paths (e.g., `/mnt/sda1`):

`mv syncthing.container ~/.config/containers/systemd/`
`nano ~/.config/containers/systemd/syncthing.container`

### Step C: Enable Linger

`sudo loginctl enable-linger $USER`

3\. Activation
--------------

Reload the user systemd daemon and start the service:

`systemctl --user daemon-reload`
`systemctl --user start syncthing`
`systemctl --user enable syncthing`

4\. Verification
----------------

### Check Logs

`journalctl --user -u syncthing -f`

### Web Interface

Access the Syncthing GUI at: `http://[Your-IP]:8384`

5\. Maintenance
---------------

Syncthing is set to auto-update via the registry:

`podman auto-update`
