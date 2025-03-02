---
title: 'GRUB Bootloader reparieren'
slug: grub-bootloader-reparieren
excerpt: 'Anleitung zur Reparatur des GRUB Bootloaders einer Instanz'
section: Tutorials
updated: 2020-11-23
---

**Letzte Aktualisierung am 22.11.2020**

## Ziel

Es kann sein, dass Sie den GRUB Bootloader reparieren müssen. In dieser Anleitung erfahren Sie, wie Sie den Bootloader reparieren und den Betrieb Ihrer Instanz wiederherstellen können.

## Voraussetzungen

- Die Instanz muss sich im Rescue-Modus befinden (siehe Anleitung zum [Rescue-Modus](../umstellung_einer_instanz_auf_den_rescue-modus/)).

## In der praktischen Anwendung

Verbinden Sie sich mit der Instanz entweder über VNC im [OVHcloud Kundencenter](https://www.ovh.com/auth/?action=gotomanager&from=https://www.ovh.de/&ovhSubsidiary=de) oder über SSH.

Geben Sie folgende Befehle ein, um das Dateisystem zu mounten und die Reparatur von GRUB zu starten:

```sh
mount /dev/sdb1 /mnt
mount -o bind /proc /mnt/proc
mount -o bind /sys /mnt/sys
mount -o bind /dev /mnt/dev
chroot /mnt /bin/bash
```

### GRUB (oder GRUB2) aktualisieren

GRUB aktualisieren:

```sh
grub-install /dev/sdb
update-grub
```

GRUB2 aktualisieren:

```sh
grub2-install /dev/sdb
grub2-mkconfig -o /boot/grub2/grub.cfg
```

Sie können die Instanz nun aus dem Rescue-Modus entfernen (siehe Anleitung zum [Rescue-Modus](../umstellung_einer_instanz_auf_den_rescue-modus/)).

## Weiterführende Informationen

Für den Austausch mit unserer User Community gehen Sie auf <https://community.ovh.com/en/>.