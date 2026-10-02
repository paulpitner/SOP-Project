# SOP 01: Create, Format, and Mount a Partition

## Purpose

Create a new partition on a disk, put an ext4 file system on it, and mount it.

## Example values

| Item | Value |
|------|-------|
| Disk | `/dev/sdc` |
| New partition | `/dev/sdc1` (first partition on the disk) |
| Size | 1 TB |
| File system | ext4 |
| Mount point | `/sysadmin` |

> [!CAUTION]
> Partitioning the wrong disk destroys data. Confirm the target with `lsblk` before continuing.

## 1. Create the partition

1. Open a terminal and list disks and partitions:
   ```bash
   sudo fdisk -l
   lsblk
   ```
2. Open `fdisk` on the target disk:
   ```bash
   sudo fdisk /dev/sdc
   ```
3. Press `n` to create a new partition and enter the partition number (`1` for the first).
4. At the **first sector** prompt, accept the default. This starts the partition at the beginning of the disk, or right after the previous partition.
5. At the **last sector** prompt, enter the size: `+1T`.
6. Press `p` to review the partition table.
7. Press `w` to write the changes and exit. **Nothing is saved until you write.** Press `q` to quit without saving.

| fdisk key | Action |
|-----------|--------|
| `n` | New partition |
| `p` | Print the partition table |
| `w` | Write changes and exit |
| `q` | Quit without saving |

## 2. Format the partition

1. Confirm the new partition appears:
   ```bash
   lsblk
   ```
2. Create an ext4 file system:
   ```bash
   sudo mkfs -t ext4 /dev/sdc1
   ```

## 3. Mount the partition

```bash
sudo mkdir -p /sysadmin
sudo mount /dev/sdc1 /sysadmin
```

## Verify

```bash
lsblk -f
df -h /sysadmin
```

## Optional: mount at boot

1. Find the partition's UUID:
   ```bash
   sudo blkid /dev/sdc1
   ```
2. Add a line to `/etc/fstab`:
   ```
   UUID=<uuid>  /sysadmin  ext4  defaults  0  2
   ```
3. Test the entry before rebooting:
   ```bash
   sudo mount -a
   ```
