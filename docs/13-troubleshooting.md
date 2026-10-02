# SOP 13: Basic Troubleshooting

## Purpose

Troubleshooting means finding the cause of a problem, designing a fix, implementing it, and verifying it worked. It takes years to get good at; this SOP covers a few core tools.

## Network troubleshooting

> "What we've got here is a failure to communicate."

### `ping`

Sends ICMP echo requests to a host by IP address or hostname. On Linux it pings continuously until `Ctrl+C`, then prints summary statistics.

| Option | Purpose |
|--------|---------|
| `-4` | IPv4 only |
| `-6` | IPv6 only |
| `-c <n>` | Send `n` packets, then show statistics |

> [!NOTE]
> A host may have ping replies disabled, so a failed ping is not a foolproof sign of no connectivity.

### `netstat`

Shows connections to and from the machine: network connections, routing tables, interface statistics, masquerade connections, and multicast memberships. See `man netstat` for everything it can do.

| Option | Purpose |
|--------|---------|
| `-a` | All ports, listening and non-listening |
| `-i` | Table of all network interfaces |
| `-l` | Listening sockets |
| `-s` | Statistics per protocol |
| `-r` | Routing table, including the default gateway |

> [!NOTE]
> TestOut notes that `netstat` is being replaced by `ss`, `ip route` (for `netstat -r`), `ip -s link` (for `netstat -i`), and `ip maddr` (for `netstat -g`).

### `nslookup`

Tests name resolution:

```bash
nslookup <hostname>
```

```
$ nslookup openwireless.org
Server:         192.168.1.2
Address:        192.168.1.2#53

Non-authoritative answer:
Name:   openwireless.org
Address: 173.239.79.206
```

Here the Raspberry Pi is also the DNS server, so the answer came from its own address.

> [!NOTE]
> TestOut notes that `nslookup` is being replaced by `host` and `dig`.

### `traceroute`

Tests connectivity between two devices and shows the path taken. `tracepath` is similar and runs without superuser privileges.

```bash
traceroute <host>
```

Abbreviated output:

```
$ traceroute openwireless.org
traceroute to openwireless.org (173.239.79.206), 30 hops max, 60 byte packets
 1  192.168.1.1 (192.168.1.1)  10.515 ms  10.050 ms  9.813 ms
 2  10.96.248.1 (10.96.248.1)  20.284 ms  20.042 ms  19.970 ms
 3  100.127.78.62 (100.127.78.62)  19.799 ms  24.634 ms  24.466 ms
 ...
13  irc.eff.org (173.239.79.206)  33.663 ms  31.578 ms  44.557 ms
```

Rows of `* * *` are hops that did not respond; the trace can still complete.

## Processes and memory

Looking closer at what a system is doing helps find bottlenecks. For example, when planning an upgrade: is a slowdown caused by too little memory or by disk I/O limits?

### `top`

Lists running processes and the resources each consumes. See [SOP 12](12-resource-monitoring.md).

### `free`

Shows memory and swap usage, including how much is free.

```
$ free
               total        used        free      shared  buff/cache   available
Mem:          931588      137492      195576        7188      598520      721356
Swap:         102396        3840       98556
```

Use `free -h` for human-readable units.
