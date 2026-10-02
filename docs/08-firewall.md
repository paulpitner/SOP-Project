# SOP 08: Firewall Basics (firewalld)

## Purpose

Protect the system from outside threats. Linux firewalls are built on the kernel packet filter (iptables/nftables) with front-end tools such as `firewalld` (Red Hat family) and `ufw` (Ubuntu) to manage them.

`firewalld` has both a command-line tool, `firewall-cmd`, and a GUI.

## `firewall-cmd` options

| Option | Purpose |
|--------|---------|
| `--state` | Check whether the daemon is running |
| `--reload` | Reload the firewall rules |
| `--permanent` | Apply the change permanently (takes effect after `--reload`) |
| `--get-default-zone` | Show the default zone |
| `--set-default-zone=<zone>` | Set the default zone |
| `--list-all` | Show the active configuration |

## Common tasks

Check the daemon:

```bash
sudo firewall-cmd --state
```

Open a port permanently, then apply it (CUPS web interface, as an example from [SOP 04](04-print-server.md)):

```bash
sudo firewall-cmd --permanent --add-port=631/tcp
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```

Ubuntu equivalent:

```bash
sudo ufw allow 631/tcp
```
