# SOP 04: Set Up a Network Print Server (CUPS)

## Purpose

Centralize printing for an organization with several high-volume printers, reducing headaches for administrators and users. This SOP assumes CUPS is being set up on an existing Linux server.

## Reference

### `systemctl` for CUPS

| Command | Purpose |
|---------|---------|
| `systemctl status cups` | Show the status of the CUPS daemon |
| `systemctl start cups` | Start the daemon |
| `systemctl enable cups` | Start the daemon at boot |

### `/etc/cups/cupsd.conf` options

| Option | Purpose |
|--------|---------|
| `ServerName` | Name the server broadcasts on the network |
| `ServerAdmin` | Email address of the CUPS administrator |
| `LogLevel` | Level of detail in the CUPS log |
| `MaxCopies` | Maximum copies for a single job |
| `MaxJobsPerUser` | Maximum jobs for one user |
| `MaxClients` | Maximum concurrent client connections |
| `Browsing` | Whether the server is visible on the network |

Other directives:

| Option | Notes |
|--------|-------|
| `User`, `Group` | Linux account/group the daemon runs as. In CUPS 2.x these live in `/etc/cups/cups-files.conf` |
| `BrowseAddress`, `BrowseAllow`, `BrowseDeny` | Legacy directives (CUPS before 1.6). Modern versions use `Browsing` and `Allow`/`Deny` rules inside `<Location>` blocks |

### Web interface

`http://localhost:631`: add, remove, and manage printers and queues.

## 1. Install CUPS

1. Most distributions ship with CUPS. If yours doesn't, install it with the package manager:
   ```bash
   sudo apt install cups     # Debian/Ubuntu
   sudo dnf install cups     # RHEL/Fedora
   ```
2. Check that the daemon is running:
   ```bash
   systemctl status cups
   ```
3. If it isn't, start it and enable it at boot:
   ```bash
   sudo systemctl start cups
   sudo systemctl enable cups
   ```

## 2. Configure CUPS

1. Review `/etc/cups/cupsd.conf` against the options above and set the values you want. Test the file, then restart:
   ```bash
   sudo cupsd -t
   sudo systemctl restart cups
   ```
2. Open the web interface at `http://localhost:631`.
3. Add each network printer by its IP address and give it a descriptive name. Have the make, model, IP address, and location of each printer ready.
4. Set default printing options per printer.

## 3. Allow access from other machines

By default CUPS listens on the loopback interface only, and the web interface is restricted to local access.

1. Allow remote administration (or edit `Listen` and the `<Location>` blocks in `cupsd.conf` by hand):
   ```bash
   sudo cupsctl --remote-admin
   ```
2. Open **port 631** on the firewall (see [SOP 08](08-firewall.md)).
3. From another machine, browse to `http://<print-server-ip>:631`.
