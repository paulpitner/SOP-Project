# SOP 12: Resource Monitoring

## Purpose

Monitor system resources in detail to avoid overtaxing hardware and to keep enough free for essential services. A slow system also generates more complaints.

## Live CPU and memory: `top`

`top` shows what the system is doing right now, much like the Windows task manager. It lists processes with their CPU use, memory percentage, user, and priority.

```bash
top
```

<!-- TODO: add a screenshot of `top` output (docs/images/top.png) -->

| Key | Action |
|-----|--------|
| `q` | Quit |
| `P` | Sort by CPU |
| `M` | Sort by memory |

## Memory snapshot: `/proc/meminfo`

```bash
cat /proc/meminfo
```

```
MemTotal:         931588 kB
MemFree:           86580 kB
MemAvailable:     731460 kB
Buffers:           73960 kB
Cached:           597080 kB
SwapCached:          244 kB
Active:           369776 kB
Inactive:         377308 kB
```

Values are reported in **kilobytes** (`kB`). For a friendlier summary, use `free -h`, or `free -b` for bytes. See [SOP 13](13-troubleshooting.md#free).

## Disk usage: `df`

See how close each file system is to full:

```bash
df -h
```

```
Filesystem      Size  Used Avail Use% Mounted on
/dev/root        15G  5.6G  8.2G  41% /
devtmpfs        326M     0  326M   0% /dev
tmpfs           455M  2.8M  453M   1% /dev/shm
tmpfs           182M  764K  182M   1% /run
tmpfs           5.0M  4.0K  5.0M   1% /run/lock
/dev/mmcblk0p1  253M   31M  222M  12% /boot
tmpfs            91M   12K   91M   1% /run/user/999
tmpfs            91M   16K   91M   1% /run/user/1000
```

`-h` prints human-readable sizes. Without it, `df` reports 1 KiB blocks; use `df -B1` for bytes.
