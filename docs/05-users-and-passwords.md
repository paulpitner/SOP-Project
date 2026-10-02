# SOP 05: Manage Users and Passwords

## Purpose

Create, modify, and delete user accounts, and enforce sound password practice. Securing users and their rights is one of the first things to do on a fresh installation.

Examples use a user named Jane Smith with the login `smithj` (naming convention: last name + first initial).

## Naming standards

- Logins are all lowercase and follow one pattern across the organization.
- Use the comment (GECOS) field for the user's full name.

## Add a user: `useradd`

```
useradd [options] LOGIN
```

| Option | Purpose |
|--------|---------|
| `-m` | Create the home directory |
| `-c` | Comment field, often a full name, phone number, or extension |
| `-d` | Absolute path of the home directory |
| `-g` | Primary group |
| `-G` | Supplementary groups |
| `-s` | Login shell |

Options you leave out fall back to the defaults in `/etc/default/useradd`.

```bash
sudo useradd -m -c "Jane Smith" -d /home/smithj -s /bin/bash smithj
```

> [!NOTE]
> On Debian-based systems `useradd` does not create a home directory unless you pass `-m`.

## Modify a user: `usermod`

```
usermod [options] LOGIN
```

| Option | Purpose |
|--------|---------|
| `-l` | Change the login name |
| `-c` | Edit the comment field |
| `-d` | Change the home directory |
| `-m` | Move the user's files to the new home directory (use with `-d`) |

## Delete a user: `userdel`

```
userdel [options] LOGIN
```

| Option | Purpose |
|--------|---------|
| `-r` | Remove the home directory and mail spool |
| `-f` | Force removal, even if the user is logged in |

## Set, reset, and lock passwords: `passwd`

| Command | Purpose |
|---------|---------|
| `passwd` | Change your own password |
| `passwd <username>` | Reset the specified user's password |
| `passwd -S <username>` | Show the account's password status |
| `passwd -l <username>` | Lock the account |
| `passwd -u <username>` | Unlock the account |
| `passwd --expire <username>` | Force a password change at next login |

## Force a password change at next login

An administrator should not know any user's password. After setting a temporary password, expire it:

```bash
sudo passwd smithj
sudo passwd --expire smithj
```

At next login the user sees:

```
WARNING: Your password has expired.
You must change your password now and login again!
Changing password for smithj.
(current) UNIX password:
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

## Enforce password complexity

Educate users on creating strong passwords, and let the system enforce a minimum. The file depends on the distribution:

| Distribution | File |
|--------------|------|
| Debian/Ubuntu | `/etc/pam.d/common-password` |
| Red Hat family | `/etc/security/pwquality.conf` |

**Debian/Ubuntu minimum length.** Append `minlen=8` to the `pam_unix.so` line:

```
password [success=2 default=ignore] pam_unix.so obscure sha512 minlen=8
```

**Parameters** (pwquality):

| Parameter | Purpose |
|-----------|---------|
| `minlen` | Minimum password length |
| `minclass` | Minimum number of character classes (uppercase, lowercase, digits, other) |
| `enforcing` | If `1`, reject passwords that don't comply |

To require 8+ characters with upper- and lowercase letters, numbers, and symbols:

```
minlen = 8
minclass = 4
enforcing = 1
```

> [!NOTE]
> Exact parameter support varies by distribution and `libpwquality` version. Confirm with `man pwquality.conf` and `man pam_pwquality` on your system.

See also: [SOP 06: Groups](06-groups.md).
