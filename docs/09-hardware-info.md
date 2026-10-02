# SOP 09: Identify Server Hardware

## Purpose

Learn what hardware a server contains without opening the case.

Example output below was captured on a Raspberry Pi.

## The `/proc` directory

Linux exposes system information under `/proc`. View files with `cat`, `head`, `tail`, `more`, or `less`.

| File | Contents |
|------|----------|
| `cpuinfo` | CPU information |
| `meminfo` | Installed and used memory |
| `version` | Linux kernel version |
| `devices` | Registered character and block device drivers (by major number) |
| `bus/` | Directories and files for buses, including USB devices |

### `version`

```
$ cat /proc/version
Linux version 5.15.32-v8+ (dom@buildbot) (aarch64-linux-gnu-gcc-8 (Ubuntu/Linaro 8.4.0-3ubuntu1) 8.4.0, GNU ld (GNU Binutils for Ubuntu) 2.34) #1538 SMP PREEMPT Thu Mar 31 19:40:39 BST 2022
```

### `meminfo`

```
$ head /proc/meminfo
MemTotal:         931588 kB
MemFree:           87276 kB
MemAvailable:     718708 kB
Buffers:           89636 kB
Cached:           566268 kB
SwapCached:            8 kB
Active:           355848 kB
Inactive:         383828 kB
Active(anon):       1096 kB
Inactive(anon):    90148 kB
```

## Other commands

### `lsusb`: USB devices

| Option | Purpose |
|--------|---------|
| `-v` | Exhaustive detail |
| `-s <bus>` | Show a specific bus |
| `-t` | Tree diagram |

```
$ lsusb -t
/:  Bus 01.Port 1: Dev 1, Class=root_hub, Driver=dwc_otg/1p, 480M
    |__ Port 1: Dev 2, If 0, Class=Hub, Driver=hub/5p, 480M
        |__ Port 1: Dev 3, If 0, Class=Vendor Specific Class, Driver=smsc95xx, 480M
```

### `hwinfo`: detailed hardware report

Prints a large amount of detail, pulling from much of `/proc`. Excerpt:

```
>> block.4: partition
----- /proc/partitions -----
   1        0       4096 ram0
   1        1       4096 ram1
 ...
 179        0   15558144 mmcblk0
 179        1     262144 mmcblk0p1
 179        2   15291904 mmcblk0p2
```

Use `hwinfo --short` for a one-line-per-device summary.

### `lspci`: PCI devices

| Option | Purpose |
|--------|---------|
| `-k` | Show the kernel module supporting each device |
| `-t` | Tree diagram |

The output is blank on a Raspberry Pi because it is a single-board computer with no PCI slots:

```
$ lspci -k
$
```

### See also

`lscpu` (CPU summary) and `lsblk` (disks and partitions).
