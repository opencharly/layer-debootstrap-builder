# debootstrap-builder

Privileged Debian-family bootstrap toolchain for OpenCharly images.

The `debootstrap-builder` candy installs the tools needed to bootstrap a
Debian-family rootfs from scratch via `debootstrap`, partition a VM disk, and
install `grub-efi` for a bootable Debian/Ubuntu system. It is the layer
composition for the `debian-debootstrap-builder` / `ubuntu-debootstrap-builder`
images, which run as privileged containers under `charly box build` and
`charly vm build` for `kind: bootstrap` source kinds.

It is the counterpart of `layer-pacstrap-builder`; both compose the same
disk-build toolchain (`qemu-img` + `parted` + `mkfs` + `grub-efi`) plus their
family-specific bootstrap tool.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `debootstrap-builder` |
| Bootstrap tool | `/usr/sbin/debootstrap` (`debootstrap` package) |
| Disk tooling | `qemu-img`, `parted`, `mkfs.*` (`e2fsprogs`/`xfsprogs`/`btrfs-progs`), `grub-install`, `efibootmgr` |
| Packages | `debootstrap`, `debian-archive-keyring`, `ubuntu-keyring`, `qemu-utils`, `dosfstools`, `e2fsprogs`, `xfsprogs`, `btrfs-progs`, `parted`, `util-linux`, `grub-efi-amd64-bin`, `grub-common`, `grub2-common`, `efibootmgr`, `mount`, `ca-certificates` |
| Service / port | none |

## How to use it

This candy is the composition of a builder image; it is normally reached by
building the `debian-debootstrap-builder` / `ubuntu-debootstrap-builder` image
that composes it, then using that builder from a `kind: bootstrap` VM:

```bash
charly --repo opencharly/distro-debian box build debian-debootstrap-builder
charly --repo opencharly/distro-ubuntu box build ubuntu-debootstrap-builder
```

To compose it directly, pin this repo in a box's `candy:` list:

```yaml
my-bootstrap-builder:
  candy:
    base: debian:13
    build: [deb]
    candy:
      - '@github.com/opencharly/layer-debootstrap-builder:v2026.239.1637'
```

The candy's `plan:` asserts each tool at a fixed path and each package registered
in the dpkg database (`debootstrap`, `qemu-img`, `parted`, `grub-install`,
`efibootmgr`, `e2fsprogs`, `dosfstools`), so a missing tool fails the checks.

## Layout

- `charly.yml` — the `debootstrap-builder:` candy entity (the `package:` list and
  the `check:` assertions).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: none — this candy declares no `skill:` entity; the gap is tracked
  in [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).
  Closest family skills: `/charly-distros:debian-debootstrap-builder`,
  `/charly-distros:ubuntu-debootstrap-builder` (owned by their distro repos).
- Counterpart: `layer-pacstrap-builder` (Arch/CachyOS bootstrap toolchain)
- Consumers: `/charly-distros:debian-debootstrap`, `/charly-distros:ubuntu-debootstrap`
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
