# AGENTS.md — layer-sway-desktop-vnc

Standalone candy repo for the `sway-desktop-vnc` layer — a headless Sway Wayland
desktop with Chrome, served over VNC on `:5900`. The candy lives in `charly.yml`
at the repo root: the `require:` list of the four check-verb plugins, the `candy:`
composition, the `env:` pin, the `check:` probes, and the embedded `skill:` entity
projected into the marketplace corpus as `/charly-selkies:sway-desktop-vnc`.

Canonical files:

- `charly.yml` — the `sway-desktop-vnc:` candy entity and the
  `sway-desktop-vnc-skill:` skill entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-selkies:sway-desktop-vnc` — the owning skill. The composition, the
  `WLR_RENDERER=pixman` software-rendering pin, and the NVIDIA/wayvnc notes. Load
  before editing or troubleshooting the candy.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, the `require:`/`candy:` composition lists).
- `/charly-check:check` — the disposable bed and probe-verb reference for the
  `cdp:` / `vnc:` / `wl:` / `dbus:` runtime checks this candy authors.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- `charly box validate` at the repo root checks the manifest parses and
  validates.
- The candy's `plan:` `check:` steps split into build-context (the three desktop
  binaries, the `WLR_RENDERER` pin) and runtime probes (supervisord RUNNING for
  `sway` + `wayvnc`, the VNC port reachable, the `cdp:`/`vnc:`/`wl:`/`dbus:`
  verbs, and a VNC framebuffer screenshot). The runtime probes are driven by the
  `check-sway-browser-vnc` bed.
- The `require:` list of `plugin-cdp`, `plugin-vnc`, `plugin-dbus`, and
  `plugin-wl` is load-bearing: each serves a check verb this candy authors, so
  the plugin must be in the composing image's scanned candy set or the verb
  `SKIP`s with `unknown verb`.

## Modify this repo

- Edit the `sway-desktop-vnc:` candy entity AND the `sway-desktop-vnc-skill:`
  skill entity in `charly.yml` together. The skill is the projected usage source,
  so a composition or probe change not mirrored in the skill leaves the corpus
  stale.
- Keep the `WLR_RENDERER=pixman` pin: it is what makes the desktop render without
  a GPU and keeps VNC screenshots reliable.
- Any new authored check verb needs its serving plugin added to `require:`.
- New behaviour claims belong in the `plan:` as an observable `check:` step, and
  in the skill body.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
