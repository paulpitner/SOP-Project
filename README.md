# Linux Sysadmin SOPs

Standard operating procedures (SOPs) for common Linux server administration tasks: disks, backups, users, networking, packages, services, monitoring, and troubleshooting. Originally written as coursework, then cleaned up and corrected for use as a reference.

> [!WARNING]
> Several procedures here are destructive (partitioning, formatting, `dd`, forced user deletion). Practice in a VM or on a spare disk first, and always confirm device names with `lsblk` before running anything.

## Contents

| # | SOP | What it covers |
|---|-----|----------------|
| 01 | [Partitions](docs/01-partitioning.md) | Create, format, and mount a partition with `fdisk` and `mkfs` |
| 02 | [Backups](docs/02-backups.md) | Weekly full + daily incremental backups with GNU `tar` |
| 03 | [Dual-boot](docs/03-dual-boot.md) | Install a second OS alongside Linux and repair GRUB2 |
| 04 | [Print server](docs/04-print-server.md) | Set up and configure CUPS |
| 05 | [Users and passwords](docs/05-users-and-passwords.md) | `useradd`, `usermod`, `userdel`, `passwd`, password policy |
| 06 | [Groups](docs/06-groups.md) | `/etc/group`, `groupadd`, `groupmod`, membership |
| 07 | [Networking basics](docs/07-networking.md) | Static vs DHCP, viewing/assigning IPs, `ping` |
| 08 | [Firewall](docs/08-firewall.md) | `firewall-cmd` basics |
| 09 | [Hardware identification](docs/09-hardware-info.md) | `/proc`, `lsusb`, `lspci`, `hwinfo` |
| 10 | [Package management](docs/10-package-management.md) | `dnf` (Red Hat family) and `apt` (Debian family) |
| 11 | [Services and processes](docs/11-services-and-processes.md) | Background jobs, `ps`, `kill`, `systemctl` |
| 12 | [Resource monitoring](docs/12-resource-monitoring.md) | `top`, `/proc/meminfo`, `free`, `df` |
| 13 | [Basic troubleshooting](docs/13-troubleshooting.md) | `ping`, `netstat`/`ss`, `nslookup`/`dig`, `traceroute`, `top`, `free` |

## Conventions

- Commands are written for **Debian/Ubuntu** and **RHEL/Fedora** families; differences are called out where they matter.
- Example device names (`/dev/sdc`), paths, and usernames are placeholders. Substitute your own.
- `<angle brackets>` mark values you must replace.
- Commands that need elevated privileges are prefixed with `sudo`.
- Example output in some sections was captured on a Raspberry Pi (Raspberry Pi OS), so hardware details will differ on your machine.
