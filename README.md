# sway-desktop-vnc

A headless Sway Wayland desktop with Chrome, served over VNC on `:5900`, for
OpenCharly images.

`sway-desktop-vnc` composes
[`sway-desktop`](https://github.com/opencharly/layer-sway-desktop) (the Sway
compositor + Chrome browser + waybar + the `wl`/desktop tooling) with
[`wayvnc`](https://github.com/opencharly/pod-wayvnc) into ONE running browser-VNC
desktop, and forces `WLR_RENDERER=pixman` so the whole stack renders in software
— no GPU required.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `sway-desktop-vnc` (composition) |
| Composes | `sway-desktop`, `wayvnc` |
| Binaries | `/usr/bin/sway`, `/usr/bin/wayvnc`, `/usr/bin/google-chrome-stable` |
| Env | `WLR_RENDERER=pixman` (software rendering) |
| Port | VNC `5900` |
| Requires | `plugin-cdp`, `plugin-vnc`, `plugin-dbus`, `plugin-wl` (the out-of-tree check verbs it authors) |

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list:

```yaml
sway-browser-vnc:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-sway-desktop-vnc:v2026.243.1835'
```

The composition is observable: all three desktop binaries land in the image, and
at deploy time `sway` + `wayvnc` run together under supervisord with the VNC
server reachable on display `:5900`.

## Layout

- `charly.yml` — the `sway-desktop-vnc:` candy entity (the `require:` list of the
  four check-verb plugins, the `candy:` composition, the `env:` pin, and the
  `check:` probes incl. the `cdp:`/`vnc:`/`wl:`/`dbus:` runtime verbs) and the
  embedded `sway-desktop-vnc-skill:` skill entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-selkies:sway-desktop-vnc`
- Base desktop: `/charly-selkies:sway-desktop`
- VNC server: `/charly-selkies:wayvnc`
- Check beds: `/charly-check:check`, `/charly-check:check-sway-browser-vnc`
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
