# SOP 06: Manage Groups

## Purpose

Inspect, create, rename, and populate groups.

## Inspect existing groups

```bash
cat /etc/group
```

Example output:

```
pulse-access:x:120:
scanner:x:121:saned
saned:x:122:
pihole:x:995:www-data
```

Each line has four colon-separated fields:

```
groupname:password:groupID:members
```

An `x` in the password field means no group password is set. The file lists every system and user group and who belongs to each.

### Which groups does a user belong to?

```bash
groups <username>
# or
id <username>
```

### Which directories can a group access?

```bash
find / -type d -group <groupname> -print 2>/dev/null | less
```

Prints the directories owned by that group.

## Add a group: `groupadd`

| Option | Purpose |
|--------|---------|
| `-g` | Specify a GID |
| `-r` | Create a system group |

```bash
sudo groupadd sales
```

Creates `sales` and assigns the next available group ID.

## Naming standard

In this organization, groups are named after departments as listed in the company directory: **all lowercase, no spaces or special characters**.

| Good | Bad |
|------|-----|
| `sales` | `Sales` |
| `researchanddevelopment` | `research_and_development` |

Check `/etc/group` for existing examples.

## Rename a group: `groupmod`

```bash
sudo groupmod -n <newname> <oldname>
```

The group ID and members are preserved.

## Add and remove members

Membership is changed with `usermod`.

| Option | Purpose |
|--------|---------|
| `-G` | Set the user's supplementary groups |
| `-g` | Set the user's primary group |
| `-a` | Append to the supplementary list instead of overwriting it (use with `-G`) |

Add `smithj` to `it` and `hr` without losing existing groups:

```bash
sudo usermod -aG it,hr smithj
```

> [!WARNING]
> `usermod -G` **without** `-a` replaces the user's entire supplementary group list.

Remove a user from a group:

```bash
sudo gpasswd -d smithj hr
```

Run `groups smithj` before and after any change to verify it.
