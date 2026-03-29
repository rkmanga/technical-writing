<section class="section" markdown="1">

<h2 class="section-title"><span class="icon-holder"><i class="fas fa-question-circle"></i> FAQ / Troubleshooting Guide</span></h2>

<div class="annotation-block" style="background: #11-0602; border: 0px solid #c3ddd5; border-radius: 4px; padding: 18px 22px; margin-bottom: 32px;">
  <h4 style="margin-top: 0; color: #02ffff; font-size: .75rem;">About This Sample</h4>
  <table style="width: 100%; font-size: 0.88rem; border-collapse: collapse;">
    <tr>
      <td style="width: 130px; font-weight: 500; padding: 4px 10px 4px 8px; vertical-align: top; color: #11-0602;">Product</td>
      <td style="padding: 4px 0; color: #11-0602;">Docker Desktop — a real, widely used application that provides a GUI and CLI for building, running, and managing containers on Windows and macOS development machines.</td>
    </tr>
    <tr>
      <td style="font-weight: 500; padding: 4px 10px 4px 8px; vertical-align: top; color: #11-0602;">Audience</td>
      <td style="padding: 4px 0; color: #11-0602;">Developers who are new to containerization and have just installed Docker Desktop (or are trying to), primarily on Windows 10/11 and macOS (Intel or Apple Silicon). Assumes basic command-line comfort but not Docker expertise.</td>
    </tr>
    <tr>
      <td style="font-weight: 500; padding: 4px 10px 4px 8px; vertical-align: top; color: #11-0602;">Tools Used</td>
      <td style="padding: 4px 0; color: #11-0602;">MS Word, Docker CLI (to validate all commands), Windows Subsystem for Linux 2 (WSL2) documentation review.</td>
    </tr>
    <tr>
      <td style="font-weight: 500; padding: 4px 10px 4px 8px; vertical-align: top; color: #11-0602;">Challenge Solved</td>
      <td style="padding: 4px 0; color: #11-0602;">Docker Desktop's official troubleshooting page lists many issues but doesn't prioritize by frequency or explain root causes in plain language. Support tickets from new users clustered around six recurring problems. The challenge was writing a focused, scannable guide that addressed the most common failure scenarios — without assuming users understood virtualization concepts — and reduced the need for escalation.</td>
    </tr>
  </table>
</div>

---

## Docker Desktop Troubleshooting Guide

Use this guide to resolve the most common issues encountered when installing and running Docker Desktop on Windows and macOS.

---

### Contents

1. [Docker Desktop fails to start on Windows](#1-docker-desktop-fails-to-start-on-windows)
2. [WSL2 installation or kernel update error (Windows)](#2-wsl2-installation-or-kernel-update-error-windows)
3. [Cannot connect to the Docker daemon](#3-cannot-connect-to-the-docker-daemon)
4. [Port is already in use / address already in use](#4-port-is-already-in-use--address-already-in-use)
5. [Container exits immediately after starting](#5-container-exits-immediately-after-starting)
6. [Docker Desktop is running slowly or using too much disk](#6-docker-desktop-is-running-slowly-or-using-too-much-disk)
7. [Volume mounts not working on macOS](#7-volume-mounts-not-working-on-macos)
8. [No internet access inside a container](#8-no-internet-access-inside-a-container)

---

### 1. Docker Desktop fails to start on Windows

#### **Symptom:** Docker Desktop opens briefly and then closes, or shows "Docker Desktop starting…" indefinitely.

##### **Common causes and fixes:**

###### A. **Virtualization is not enabled in BIOS/UEFI.**

Docker requires hardware virtualization. To check:
  
  1. Open Task Manager > **Performance** tab > **CPU**.
  2. Look for **Virtualization: Enabled** at the bottom.
  3. If it shows Disabled, restart your computer, enter BIOS/UEFI setup (typically by pressing F2, F12, or Del during boot), and enable **Intel VT-x** or **AMD-V**.

###### B. **Hyper-V or WSL2 is not installed.**

Docker on Windows requires either Hyper-V (Windows Pro/Enterprise) or WSL2 (Windows Home, Pro, or Enterprise). If you are on Windows Home, see [WSL2 Installation](#2-wsl2-installation-or-kernel-update-error-windows).

To enable Hyper-V:
```powershell
# Run PowerShell as Administrator
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
```
Restart your computer when prompted.

###### C. **Docker Desktop installation is corrupted.**

Uninstall Docker Desktop completely from **Settings > Apps**, restart, and download a fresh installer from [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop/).

---

### 2. WSL2 Installation or Kernel Update Error (Windows)

#### **Symptom:** Docker Desktop prompts you to install or update the WSL2 Linux kernel, or shows an error like: `WSL 2 requires an update to its kernel component.`

**Fix:**

1. Open your browser and download the WSL2 kernel update package from Microsoft: [aka.ms/wsl2kernel](https://aka.ms/wsl2kernel)
2. Run the downloaded `.msi` installer.
3. After installation, open PowerShell as Administrator and run:
   ```powershell
     wsl --set-default-version 2
   ```
1. Restart Docker Desktop.

**If WSL is not installed at all:**
   ```powershell
      # Run PowerShell as Administrator
      wsl --install
   ```

This installs WSL2 and Ubuntu by default. Restart when prompted, then restart Docker Desktop.

---

### 3. Cannot Connect to the Docker Daemon

#### **Symptom:** Running `docker ps` or any Docker CLI command returns:

  ```
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
  ```

**Fix — Check that Docker Desktop is running:**
The Docker daemon only runs when Docker Desktop is open. Look for the Docker whale icon in your system tray (Windows) or menu bar (macOS). If it's not there, launch Docker Desktop from your Applications folder or Start Menu and wait until the icon stops animating.

**Fix — Restart the Docker service:**

If Docker Desktop is open but the error persists:

1. Right-click the Docker icon in your system tray or menu bar.
2. Select **Restart Docker Desktop**.
3. Wait 30–60 seconds for the daemon to fully restart, then retry your command.

**On macOS, if the icon is missing:**
```bash
open /Applications/Docker.app
```

---

### 4. Port Is Already in Use / Address Already Bound

#### **Symptom:** Starting a container with `-p` flag fails:

  ```
Error starting userland proxy: listen tcp 0.0.0.0:8080: bind: address already in use
  ```

**Cause:** Another process on your machine is already using that port.

**Fix — Find and stop the conflicting process:**

On **macOS/Linux:**
```bash
lsof -i :8080
# Shows the process using port 8080
kill -9 <PID>
```

On **Windows (PowerShell):**
```powershell
netstat -ano | findstr :8080
# Note the PID in the last column
Stop-Process -Id <PID> -Force
```

**Alternative — Use a different port:**
Map the container to a different host port:
```bash
docker run -p 9090:8080 my-image
```
This maps port 9090 on your machine to port 8080 inside the container.

---

### 5. Container Exits Immediately After Starting

#### **Symptom:** You run `docker run my-image` and the container exits within seconds. `docker ps` shows nothing running; `docker ps -a` shows the container with an `Exited (1)` or `Exited (0)` status.

**Cause:** The container's main process finished or crashed. Containers only run as long as their primary process is running.

**Fix — Check the container logs:**
```bash
docker logs <container-id-or-name>
```
This shows the stdout/stderr output from inside the container, which usually explains the error.

**Common sub-causes:**

- **Missing entry point or command:** The Dockerfile's `CMD` or `ENTRYPOINT` points to a script that doesn't exist in the image. Verify the path is correct.
- **Application crash on startup:** A missing environment variable or misconfigured setting causes the app to exit. Check the logs for error messages and ensure required environment variables are passed with `-e` or `--env-file`.
- **Interactive container without a terminal:** If you built an image that expects an interactive shell, start it with `-it`:
```bash
docker run -it my-image /bin/bash
```

---

### 6. Docker Desktop Is Running Slowly or Using Too Much Disk

#### **Symptom:** Your computer runs slowly while Docker is open, or you see a notification that the Docker disk image has grown very large.

**Fix — Reclaim disk space:**

Old images, stopped containers, and unused volumes accumulate over time. Remove them with:

```bash
# Remove all stopped containers, unused networks, dangling images, and build cache
docker system prune

# Also remove unused volumes (adds -v flag)
docker system prune -a --volumes
```

!!! warning
    `--volumes` permanently deletes data in unused volumes. Only use this if you are sure you don't need that data.

**Fix — Limit Docker's resource usage:**

1. Open Docker Desktop and go to **Settings > Resources**.
2. Reduce **CPUs** and **Memory** to values appropriate for your machine (e.g., 2 CPUs, 4 GB RAM for a typical development machine).
3. Click **Apply & Restart**.

---

### 7. Volume Mounts Not Working on macOS

#### **Symptom:** A container starts but the mounted directory appears empty, even though files exist at that path on your Mac.

**Cause:** Docker Desktop on macOS uses a Linux VM; file sharing must be explicitly configured.

**Fix:**
1. Open Docker Desktop > **Settings > Resources > File Sharing**.
2. Click **+** and add the folder path you are trying to mount (e.g., `/Users/yourname/projects`).
3. Click **Apply & Restart**.
4. Retry your `docker run` command with the `-v` flag.

!!! tip
    On Docker Desktop 4.x and later with VirtioFS enabled, `/Users`, `/Volumes`, `/private`, and `/tmp` are shared by default. If you are mounting a path outside those directories, you must add it manually.

---

### 8. No Internet Access Inside a Container

#### **Symptom:** Commands inside the container that require internet access fail (`curl`, `apt-get update`, `pip install`, etc.).

**Fix — Check Docker's DNS settings:**
```bash
# Inside the container, test DNS resolution
docker run --rm alpine nslookup google.com
```
If this fails, Docker may have a DNS conflict with your host machine's VPN or corporate firewall.

**Override DNS for a container:**
```bash
docker run --dns 8.8.8.8 my-image
```

**Global DNS override (Docker Desktop):**

1. Go to **Settings > Docker Engine**.
2. Add `"dns": ["8.8.8.8", "8.8.4.4"]` to the JSON configuration:
```json
{
  "dns": ["8.8.8.8", "8.8.4.4"]
}
```
1. Click **Apply & Restart**.

If you are on a corporate VPN, contact your network administrator — VPN clients often redirect DNS in ways that conflict with Docker's virtual network.

---

*All commands in this guide were verified on Docker Desktop 4.28 (Windows 11 and macOS 14 Sonoma).*
*For issues not covered here, visit the [Docker Community Forums](https://forums.docker.com) or submit a bug report via Docker Desktop's **Help > Report an issue** menu.*

</section>
