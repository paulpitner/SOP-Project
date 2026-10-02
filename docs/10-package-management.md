# SOP 10: Package Management

## Purpose

Install, remove, and update software. A Linux **package** is the set of files a program needs to run; installing it places those files in the right directories. Red Hat-family and Debian-family systems use different package formats and managers.

## Red Hat family: `dnf`

Red Hat, CentOS, and Fedora use `.rpm` packages, managed with `dnf`.

| Command | Purpose |
|---------|---------|
| `dnf install <package>` | Install a package |
| `dnf list --installed` | List installed packages |
| `dnf list --available` | List packages available in the repositories |
| `dnf remove <package>` | Uninstall a package |
| `dnf info <package>` | Show package details |
| `dnf upgrade` | Upgrade every package. Name a package to upgrade only that one |

## Debian family: `apt`

Debian-based systems such as Ubuntu and Linux Mint use `.deb` packages, managed with `apt`.

| Command | Purpose |
|---------|---------|
| `apt update` | Refresh the package index from the repositories |
| `apt install <package>` | Install a package |
| `apt upgrade` | Upgrade every package. Name a package to upgrade only that one |
| `apt full-upgrade` | Upgrade all packages, adding or removing dependencies as needed |
| `apt remove <package>` | Remove a package |
| `apt search <term>` | Search the repositories |

All of these require `sudo`. Run `apt update` before `apt upgrade`.
