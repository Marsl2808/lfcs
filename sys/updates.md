# 🐧 Ubuntu Updates Cheat-Sheet
A practical reference for keeping an Ubuntu system fully up to date: packages, kernel, and OS releases.


## 🔍 System information
- `lsb_release -a` — Ubuntu version
- `uname -r` — running kernel
- `uptime` — quick sanity check


## 🔄 Standard package updates
Safe, everyday updates (apps, libraries, security fixes):

```bash
sudo apt update
sudo apt upgrade

apt list --upgradable # show available updates
# 🧹 cleanup old packages
apt autoremove # todo:  difference?
apt autoclean

```

## 🚀 Full system upgrade (recommended)
Allows dependency changes and installs new kernels:

```bash
sudo apt full-upgrade
sudo reboot
```

## 🧠 Kernel management
- Check running kernel: `uname -r`
- List installed kernels:

```bash
dpkg -l | grep linux-image
```

- Install latest HWE kernel (recommended on LTS desktops for newer hardware):

```bash
sudo apt install --install-recommends linux-generic-hwe-$(lsb_release -rs)
```

Kernel changes take effect only after reboot.

## 🔔 Check if reboot is required

```bash
[ -f /var/run/reboot-required ] && echo "Reboot required"
```

## 🔐 Security updates (dry run)

```bash
sudo unattended-upgrade --dry-run
```

## ⬆️ Minor Ubuntu release updates

Example: 22.04 → 22.04.4

Handled automatically via normal updates — no special action needed.

## ⬆️ Major Ubuntu version upgrade

Example: 22.04 → 24.04

### Preparation

```bash
sudo apt update
sudo apt full-upgrade
sudo apt autoremove
```

### Start upgrade

```bash
sudo do-release-upgrade
```

Force upgrade check (LTS → next LTS):

```bash
sudo do-release-upgrade -d
```

Before upgrading:
- Back up important data
- Disable third-party PPAs
- Ensure stable internet and power

## 🛑 Fix broken package state

```bash
sudo apt --fix-broken install
sudo dpkg --configure -a
```

## 🧠 Best practices
- Always reboot after kernel updates
- Keep at least one older kernel installed
- Avoid major upgrades on production systems without backups
- Prefer LTS releases for long-term stability

## dpkg vs. apt

- dpkg: Low-level tool that installs and manages local .deb packages without resolving dependencies.
- apt: High-level package manager that downloads packages and automatically resolves dependencies using dpkg.

Even shorter:
- dpkg: Handles .deb files directly.
- apt: Fetches, resolves, and installs packages for you.

### .deb
- .deb file: A packaged software archive for Debian-based Linux systems containing binaries, metadata, and install scripts.
- .deb: The standard package format for Debian/Ubuntu software.