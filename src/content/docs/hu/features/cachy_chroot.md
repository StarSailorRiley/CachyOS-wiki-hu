---
title: CachyOS chroot segéd
description: Segédeszköz a rendszerekbe való chrootolás megkönnyítéséhez
---

A [**`cachy-chroot`**](https://github.com/CachyOS/cachy-chroot) egy egyszerű segítőprogram, amely megkönnyíti a meglévő CachyOS vagy bármely Arch-alapú telepítésbe való chrootolás folyamatát. Felsorolja a gépen észlelt összes partíciót, és támogatja a BTRFS alkötetek listázását is. Végül, de nem utolsósorban, a `cachy-chroot` támogatja a LUKS-on keresztül titkosított rendszereket is. Minden `fstab` bejegyzést leképez a kijelölt `crypttab` bejegyzésekhez, és szabályosan bezárja az összes LUKS kötetet a chrootból való kilépéskor.

## Használat

A chrootolás folyamatát **kötelező** live ISO-n elvégezni. Az alábbiakban egy példa látható a `cachy-chroot` használatára egy CachyOS BTRFS telepítésben.

```sh title="chrootolás cachy-chroot-al"
❯ sudo su # Lépjen be a root felhasználóba a live ISO-n belül
❯ pacman -Sy cachy-chroot # Győződjön meg arról, hogy a cachy-chroot a legújabb verzión van
❯ cachy-chroot
Info: Found 3 block devices
Info: Found partition: Partition: /dev/nvme0n1p1: FS: vfat UUID: EDA6-ED98
Info: Found partition: Partition: /dev/nvme0n1p2: FS: btrfs UUID: b09a027e-a61d-424f-858f-2e02be61b342
Info: Found partition: Partition: /dev/nvme0n1p4: FS: btrfs UUID: 66e84339-8c77-4131-afce-50ec2cf67a80
? Select the block device for the root partition (use arrow keys):  › # Válassza ki a root partíció blokkeszközét (használja a nyilakat)
  Partition: /dev/nvme0n1p1: FS: vfat UUID: EDA6-ED98
❯ Partition: /dev/nvme0n1p2: FS: btrfs UUID: b09a027e-a61d-424f-858f-2e02be61b342
  Partition: /dev/nvme0n1p4: FS: btrfs UUID: 66e84339-8c77-4131-afce-50ec2cf67a80
✔ Select the block device for the root partition (use arrow keys):  · Partition: /dev/nvme0n1p2: FS: btrfs UUID: b09a027e-a61d-424f-858f-2e02be61b342
Info: Selected BTRFS partition, mounting and listing subvolumes...
Info: Mounting partition /dev/nvme0n1p2 at /tmp/cachyos-chroot-temp-mount-b09a027e-a61d-424f-858f-2e02be61b342-hwAeIm with options: []
Info: Unmounting partition at /tmp/cachyos-chroot-temp-mount-b09a027e-a61d-424f-858f-2e02be61b342-hwAeIm
? Do you want to use CachyOS BTRFS preset to auto mount root subvolume? (y/n) › # Szeretné hogy a CachyOS BTRFS előbeállítását a root automatikus mountolásához? Írjon be egy y betűt ha CachyOS-t használ
```

A root partíció kiválasztása után a program további partíciók, pl. a `/boot` partíció mountolását kéri.

```sh title="További partíciók mountolása"
✔ Do you want to mount additional partitions? · yes
? Enter the mount point for additional partition (e.g. /boot) type 'skip' to cancel:  › # /boot a systemd-boot-on, /boot/efi a GRUB-on és rEFInd-on
```

Ha kész, lépjen ki a chroot környezetből az `exit` parancs megadásával a promptban, vagy a `CTRL+D` billentyűkombináció megnyomásával.

```sh title="Kilépés a chrootból"
exit
```

## További információk

- [Arch Wiki - chroot](https://wiki.archlinux.org/title/Chroot)

