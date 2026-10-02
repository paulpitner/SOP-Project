# SOP 03: Create a Dual-Boot System

## Purpose

Install two operating systems directly on the same hardware and choose between them at boot. Virtualization is often the better choice, but dual-boot is sometimes required.

## Scenario

A Linux server is already installed. Windows Server is being added on a second disk, `/dev/sdb`.

## Reference

| Tool | Option / file | Purpose |
|------|---------------|---------|
| `dd` | `if=` | File to read from |
| | `of=` | Device to write to |
| GRUB2 | `/etc/default/grub` | Editable configuration file |
| | `GRUB_TIMEOUT` | Seconds to wait for input before booting the default OS |
| | `GRUB_DEFAULT` | Which entry boots by default |
| | `update-grub` | Regenerate the GRUB config after editing (Debian/Ubuntu) |

## 1. Install the second OS

1. **Choose an install location.** Use a separate disk or another partition. Here: the first partition on a second disk, `/dev/sdb1`.
2. **Download** the installer ISO from the vendor (for Windows Server, Microsoft's website).
3. **Create bootable media.** Insert a blank USB drive and identify it:
   ```bash
   lsblk
   ```
   For this example the USB drive is `/dev/sdc`. Write the ISO to it:
   ```bash
   sudo dd if=/path/to/file.iso of=/dev/sdc bs=4M status=progress conv=fsync
   ```
   > [!WARNING]
   > `dd` overwrites the target with no confirmation. A wrong `of=` destroys that disk.

   > [!NOTE]
   > `dd` works for hybrid ISOs, which includes most Linux distributions. Windows ISOs are generally **not** hybrid, so a `dd`-written USB often won't boot. For Windows media use a purpose-built tool such as WoeUSB-ng or Ventoy on Linux, or Rufus on Windows.
4. **Boot from the USB drive.** The key that opens the boot menu varies by motherboard manufacturer.
5. Follow the installer prompts and install to the second disk.

## 2. Repair GRUB2 (you'll probably need to)

Installing Windows alongside Linux commonly overwrites the bootloader. To restore it (BIOS/MBR systems):

1. Boot from a live Linux USB.
2. Identify the disks and the installed Linux root partition:
   ```bash
   lsblk
   ```
3. Mount the installed system's root partition (and `/boot` if it is separate), for example:
   ```bash
   sudo mount /dev/sda2 /mnt
   ```
4. Reinstall GRUB to the primary disk, pointing at the installed system's `/boot`:
   ```bash
   sudo grub-install --boot-directory=/mnt/boot /dev/sda
   ```
5. Reboot into the installed Linux system.

> [!NOTE]
> On **UEFI** systems Windows usually leaves GRUB's files alone but changes the firmware boot order. Fix it from the firmware boot menu or with `efibootmgr`.

## 3. Test the system

1. On power-up, the boot menu should list both operating systems.
2. If Windows is missing, boot into Linux and regenerate the menu:
   ```bash
   sudo update-grub          # Debian/Ubuntu
   sudo grub2-mkconfig -o /boot/grub2/grub.cfg   # RHEL/Fedora (BIOS path shown; UEFI path differs)
   ```
   This runs `os-prober`, which detects other installed operating systems. In newer GRUB releases `os-prober` is disabled by default. If Windows is still missing, set `GRUB_DISABLE_OS_PROBER=false` in `/etc/default/grub` and rerun the command.
3. Edit `/etc/default/grub` so there is time to choose and the default entry is the one you want (`GRUB_TIMEOUT`, `GRUB_DEFAULT`), then run `update-grub` again.
4. Boot into Windows Server and confirm it works.
5. Reboot into Linux and confirm it works.
