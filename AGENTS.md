# AGENTS.md — layer-debootstrap-builder

Standalone candy repo for the `debootstrap-builder` layer — the privileged
Debian-family bootstrap toolchain. The candy lives in `charly.yml` at the repo
root: the `package:` list and the ordered `plan:` of `check:` assertions over
`debootstrap`, `qemu-img`, `parted`, `grub-install`, `efibootmgr`, `e2fsprogs`
and `dosfstools`. The repo declares **no `skill:` entity**; the owning guidance
is the closest family skill (the gap is tracked in
`opencharly/opencharly#291`).

Canonical files:

- `charly.yml` — the `debootstrap-builder:` candy entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-distros:debian-debootstrap-builder` and
  `/charly-distros:ubuntu-debootstrap-builder` — the closest family skills (the
  builder images that compose this candy; owned by their distro repos). Load
  before editing or troubleshooting the candy.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, per-distro `distro:` arms, package
  sections, service declarations). Load before editing any entity field or plan
  step.
- **Missing owning skill:** this candy has no `skill:` entity of its own, so no
  `/charly-*:*` page is projected for it. The gap is recorded against the named
  batch
  [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps are the functional evidence: each tool at a
  fixed path and each package registered in the dpkg database. They must stay
  valid on every distro arm they run on.

## Modify this repo

- There is no `skill:` entity here to edit; a package change is mirrored only in
  the `charly.yml` entity and its `plan:`.
- New behaviour claims belong in the `plan:` as an observable `check:` step.
- If the missing owning skill is authored, add the `skill:` entity here and
  update this signpost and the README in the same change.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
