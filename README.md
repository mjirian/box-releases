# box

box turns an ordinary x86-64 machine into a home server for self-hosted
apps. Its disk is encrypted and it unlocks without you typing a passphrase,
all of its internet traffic leaves through a VPS you control, and you
manage it from a web page on your LAN.

**[Download the latest release](https://github.com/mjirian/box-releases/releases/latest)**:
the release page has the files and the install steps.

## What you get

- **An encrypted disk that unlocks on its own.** At boot, the box fetches
  its key from your hub (and from your other boxes) over WireGuard, using
  Clevis and Tang, plus the TPM if the machine has one. If it boots from a
  network it hasn't run from before, it waits for your approval on the hub
  first. A recovery passphrase, shown once at install time, is the fallback.
- **Its internet traffic goes through the hub.** The box's WAN traffic
  leaves through a VPS you rent. If the tunnel is down, nothing goes out
  (a kill switch). The web UI and SSH answer only on your LAN.
- **Apps from a catalog:** Jellyfin, Vaultwarden and Nextcloud, installed
  and kept up to date from the web UI. Each app's data is on its own btrfs
  volume, with hourly, daily and weekly snapshots and nightly encrypted
  backups to the hub.
- **Upkeep included.** Debian and app updates install every night. An
  update that breaks the box is rolled back, whether it's a failed boot or
  an app that doesn't come back up. There are also disk health checks, extra
  disks as one pool (mirrored once there are two), and email alerts from
  the hub.

## What you need

- **A VPS** running Debian 13 with a public IPv4 address. This is the hub.
  A small one is enough, but all of your boxes' internet traffic goes
  through it.
- **A machine for each box:** x86-64 with UEFI, and an internal disk of
  12 GiB or more. **The installer erases that disk.** A TPM 2.0 is used if
  present but not required.
- **A USB stick** of 2 GB or more for the installer.

## Install, in short

1. Set up the hub on the VPS with `bootstrap.sh` and `box-hub_*.deb`, and
   create an enrollment code for each box.
2. Write `box-installer-*.raw.xz` to the USB stick.
3. Boot the box from the stick and answer its questions.

Each release's notes have the exact commands. Check your downloads
against its `SHA256SUMS` first.

## Upgrading

New releases ship the box packages as `.deb`s. Publish them on your hub
(`box-hub repo publish box-*.deb`), and your boxes and the hub install
them in their next nightly upgrade. You only need the installer for new
boxes.
