# SOP 02: Full System Backup (Weekly Full + Daily Incremental)

## Purpose

Preserve data in case of failure. A good backup is an essential part of a disaster recovery plan.

## Scenario

| Item | Value |
|------|-------|
| Data to protect | `/data` |
| Backup target | Partition `/dev/sdc1`, mounted at `/mnt/backup` |
| Full backup | Weekly (Sunday) |
| Incremental backup | Daily |

Back up to a **separate disk** so the data survives a failure of the system disk.

```bash
sudo mkdir -p /mnt/backup
sudo mount /dev/sdc1 /mnt/backup
```

> [!NOTE]
> A device node such as `/dev/sdc1` is not a directory. Mount the partition and write to the mount point.

## `tar` options used

| Option | Long form | Purpose |
|--------|-----------|---------|
| `-c` | `--create` | Create a new archive |
| `-z` | `--gzip` | Compress through gzip |
| `-f` | `--file` | Archive file name (must be followed by the name) |
| `-g` | `--listed-incremental=FILE` | GNU incremental mode. `FILE` is a **snapshot file** where tar records what it has already dumped, so later runs only archive changed files |
| `-v` | `--verbose` | List files as they are processed |

## Weekly full backup

A full backup takes a lot of time and system resources. Schedule it for a period when users do not need peak performance.

Delete the old snapshot file so tar starts a fresh level-0 dump, then create the archive:

```bash
sudo rm -f /mnt/backup/data.snar
sudo tar --create --gzip --verbose \
  --listed-incremental=/mnt/backup/data.snar \
  --file=/mnt/backup/full-$(date +%F).tar.gz \
  /data
```

Using `--listed-incremental` on the full backup is what creates the snapshot file the daily incrementals depend on.

## Daily incremental backup

An incremental backup only archives files that were changed or created since the previous run:

```bash
sudo tar --create --gzip --verbose \
  --listed-incremental=/mnt/backup/data.snar \
  --file=/mnt/backup/incr-$(date +%F).tar.gz \
  /data
```

tar updates `data.snar` on every run, so each incremental covers changes since the **previous** backup.

## Scheduling

Put each command in a script (for example `/usr/local/sbin/backup-full.sh` and `backup-incr.sh`) and schedule them with cron or a systemd timer. Example crontab entries:

```cron
0 2 * * 0    /usr/local/sbin/backup-full.sh
0 2 * * 1-6  /usr/local/sbin/backup-incr.sh
```

## Restoring

Extract the most recent full backup first, then each incremental **in order**. Use `/dev/null` as the snapshot file so tar applies the recorded deletions and changes:

```bash
sudo tar --extract --gzip --listed-incremental=/dev/null \
  --file=/mnt/backup/full-<date>.tar.gz -C /restore
sudo tar --extract --gzip --listed-incremental=/dev/null \
  --file=/mnt/backup/incr-<date>.tar.gz -C /restore
```

## Why this strategy

**Daily incremental + weekly full** means no more than 24 hours of work is lost if the system goes down, and it balances ease of backup against ease of restore.

A **differential** backup (everything changed since the last full) takes longer each night but is easier to restore: you need only the last full backup and the latest differential, not up to six incrementals.

Trade-off accepted here: a full restore late in the week is slower (full + up to 6 incrementals), but nightly backups use minimal resources and cause less downtime. Because the system is used far more often than it is restored, this trade-off is justified.
