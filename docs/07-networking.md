# SOP 07: Networking Basics (IP Addressing and ping)

## Purpose

Set up and test basic network connectivity on a Linux system.

## Static vs DHCP

An IP address can be assigned automatically by a DHCP server or set manually on the machine.

| Method | Characteristics |
|--------|-----------------|
| **Static** | More likely to cause configuration errors. Ideal for servers whose address must not change. |
| **DHCP** | Less error-prone because assignment is handled by the DHCP server. Ideal for workstations that connect and disconnect regularly. Frees unused addresses for other machines. |

## View interfaces

```bash
ip -br addr          # modern
ifconfig -a          # net-tools (may need installing)
```

Both list every network interface, including the loopback adapter.

## Assign an address manually

Before assigning:

- Check your router/DHCP configuration so the address isn't already in use or reserved.
- Confirm the correct **default gateway** and **subnet mask**.

`ifconfig` options:

| Parameter | Purpose |
|-----------|---------|
| `<address>` | IP address to set on the interface |
| `netmask` | Subnet mask |
| `broadcast` | Broadcast address |
| `up` / `down` | Activate / deactivate the interface |

Usage:

```bash
sudo ifconfig ens192 192.168.1.8 netmask 255.255.255.0
```

Equivalent with `ip`:

```bash
sudo ip addr add 192.168.1.8/24 dev ens192
sudo ip link set ens192 up
```

Afterwards, verify with `ifconfig -a` or `ip -br addr`.

> [!NOTE]
> Addresses set this way are **not persistent** and are lost on reboot. Make them permanent through your distribution's network configuration (Netplan, NetworkManager/`nmcli`, or `/etc/sysconfig/network-scripts`).

## Test with `ping`

```bash
ping <host>
```

| Result | Meaning |
|--------|---------|
| Reply | The destination answered with an echo |
| Request Timed Out | No echo reply was received |
| Destination Network Unreachable | There is no route to the destination |

More tools: [SOP 13: Troubleshooting](13-troubleshooting.md).
