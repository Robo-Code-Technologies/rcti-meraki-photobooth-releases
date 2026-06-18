# Changelog

All notable changes to the RCTI Meraki Photobooth booth runtime are recorded here.
Versions are the Velopack/nbgv release versions published to the
`rcti-meraki-photobooth-releases` feed. Booths receive a version when an operator runs
**System tab → Check for updates**.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

> **Mirror each release entry into the PUBLIC releases repo too.** This file lives in the private
> source repo; operators and the public only see the **`rcti-meraki-photobooth-releases`** repo. When you cut a
> release, copy that version's section into the **GitHub Release notes** for its `v<version>` tag
> (and keep a copy of this file in the releases repo). See `deploy/RELEASING.md` → "Changelog".

## [1.0.0] — 2026-06-18

**First official release.** The booth graduates from the beta train to a stable `1.0.0`. This
release consolidates everything since `beta.120`: a redesigned operator console, a deterministic
print path, a polished customer flow, and real cloud upload. Operators on a beta build get this the
next time they run **System tab → Check for updates**.

### Added
- **Deterministic print profiles (M23).** Instead of trusting the driver to report what paper is
  loaded (which the DNP does unreliably), the operator now *declares* the booth's printer, the loaded
  paper, and the products it can make — each with a fixed print recipe. Print time becomes a simple
  lookup, so a 2×6 or 4×6 always comes out the size you set. A sensible default profile is
  auto-created on a fresh booth, and **Printing settings** (simple + Advanced) open from both the
  System tab and the Run hub.
- **Cloud upload, for real (M9).** Finished sessions upload to the cloud gallery over the live Worker
  contract, and the delivery/print screen now shows a **cloud-gallery download QR** alongside the
  local-hotspot one so guests can grab their photos from anywhere.
- **Operator dev-mode & cloud controls (M24).** A System-tab card exposes developer mode (windowed
  kiosk, allow-print, mock camera) and a switch to pause cloud auto-upload — all driven from booth
  settings, no launch flags.
- **Cloud-gallery links in session detail (M11).** Each session shows a cloud gallery link and a QR
  dialog for re-sharing.
- **Per-slot retake (M21).** Customers can retake a single shot instead of redoing the whole strip,
  on a touch-friendly picker.

### Changed
- **Operator console redesigned for release (M11).** A plain-language pass across the System tab,
  modern Fluent input controls and toggle switches, regrouped settings (Hardware exploded out, Audit
  Log removed), a redesigned session detail (preview hero + select-to-preview gallery + per-output
  delete), a restyled Sessions browse, an inline-editable booth address, and a de-staled shell
  header/footer. Booth Defaults and Event Settings share one restyled panel.
- **Customer flow polished end-to-end (M21).** The Print screen is now interactive (Print / Skip with
  a staged roll-off animation), delivery is merged into it (the separate Transfer screen is retired),
  a new ProductPreview gallery and post-capture review let customers pick their output, screen
  transitions are real cross-fades, and tap-to-shoot is the default shutter.
- **Capture options split (M21):** the operator card now separates **Capture modes** from
  **Outputs**, and the customer welcome/instruction screens were rebuilt on the shared red sunburst
  ground with a prominent call-to-action.
- **Token tickets redesigned (M13):** one-time PINs print as wide tickets with a cloud-gallery QR,
  two-up on a cut strip, with a printed-status batch run.
- **Session folders are sortable (M22):** named `yyyy-MM-dd_NNN_code`.
- **Camera selection is plain-language (M2):** a Camera type picker with a mirror-camera option,
  replacing the old checkbox.

### Fixed
- **Kiosk lockdown is gated on operator presence (M19).** The fullscreen lockdown engages only when
  the operator is actually running, and the escape-chord close no longer lets the operator resurrect
  the kiosk.
- **Loading screens stay responsive (M21):** heavy renders run off the UI thread, the FILTER Continue
  is no longer laggy, and a white flash between screens is gone.
- **Run hub printer media refreshes (M23)** after Print Settings closes, and the printer buttons
  collapse into one overflow menu.
- **Find-a-strip and Run-hub token entry** work against the current token format.

### Removed
- **Session-long video recording (M7).** The booth captures stills only; the motion keepsakes are the
  Animated Strip and Shot GIF, both built from preview frames.

## [1.0.0-beta.120] — 2026-06-13

### Changed
- **Capture viewfinder matches the printed photo (M21).** The live capture preview is now cropped to
  the photo slot's aspect ratio, so what the customer frames on screen is what comes out on the strip
  instead of a wider view that gets trimmed at print time.

## [1.0.0-beta.118] — 2026-06-12

### Added
- **Built-in default templates on a fresh booth (M18).** A newly installed booth now ships with the
  default Meraki strips and the Shot GIF frame already set up, so an operator can run sessions out of
  the box without authoring a template first.

### Changed
- **Default themes and templates now update with the app (M15).** Updating an already-installed booth
  refreshes the built-in Meraki theme and the default templates whenever a release ships changed
  versions — previously only a brand-new install received the latest defaults. Themes and templates
  you authored yourself are left untouched.

### Fixed
- **DNP 2×6 strips print upright and cut (M12).** Corrected the 90° rotation and restored the 2-inch
  cut, so a 2×6 comes out the right way up and separated.
- **DNP 4×6 prints edge-to-edge (M12).** The print is scaled to full bleed so there is no white border.

## [1.0.0-beta.104] — 2026-06-12

### Added
- **Customer capture modes (M21).** Customers can now pick a shot technique before capturing,
  and operators choose which modes are offered (Booth Defaults + per-event). Includes Boomerang,
  an **Animated Strip** (straight or boomerang, chosen on a new post-capture review screen), a
  **Shot GIF** framed by an operator template, manual tap-to-shoot, and a live template strip.
- **Customer photo filters (M21).** A FILTER screen lets the customer apply one of several looks
  (SkiaSharp) to the printed and on-screen strip before printing; previews are pre-rendered so
  switching is instant.
- **Test Print dialog (M12)** with live preview over the active template — rotation, scaling, and
  DNP cut controls — plus a **Printer settings** button in the Run hub to open the driver dialog.
- **Token cards (M13):** print one-time PINs as ~2cm cards in a PDF preview with the Meraki logo.
- **Operator-triggered software updates (M18):** the System tab checks the public release feed,
  shows the available version, and updates + restarts on demand (blocked while a session is live).
- **Diagnostics log export (M19):** System tab → Diagnostics exports a zip of logs to the Desktop.
- **Identify displays (M18):** flashes a number on each monitor so the operator can confirm which
  screen the customer kiosk will use.
- **Branded installer (M20):** app icon and an animated install splash.

### Changed
- **The customer app launches on operator command, not automatically** — press **Launch** in the
  Run hub. A never-launched or stopped kiosk is no longer revived by a stray disconnect.
- **Customer screens restyled gold-free (M15)** and scaled to fill the kiosk (no red side bars).
- **Print path is printer-agnostic (M12):** DNP cut is used on DNP drivers and falls back to a
  custom paper size on ordinary printers.

### Fixed
- **Kiosk lockdown no longer disturbs the operator's screen (M18)** — the primary taskbar is hidden
  only when the customer is actually on the primary monitor.
- **In-slot live preview now matches the printed photo (M21)** — the viewfinder is center-cropped
  the same way the engine composites, so what you see is what prints.
- **DNP 2×6 cut (M12):** composed as a two-up 4×6 cut sheet with a per-job 2-inch-cut DEVMODE.
- **GIF / animated-strip playback (M21):** looping media no longer renders blank or upside-down.
- **A deployed (Release) booth can no longer be put into developer mode (M18).**
- **Hard crashes are now captured (M19):** crash markers and non-UI-thread crash handlers in both
  apps, with a pre-release preflight gate guarding every published build.

[1.0.0-beta.104]: https://github.com/Robo-Code-Technologies/rcti-meraki-photobooth-releases/releases/tag/v1.0.0-beta.104

## [1.0.0-beta.17] — 2026-06-08

### Added
- **Identify displays** button (System tab → Display): flashes a large number on every
  connected monitor, mirroring Windows' own "Identify", so the operator can confirm which
  physical screen each *Customer display* index maps to before selecting it. The overlay uses
  the same placement code as the customer kiosk, so the number shown on a screen is exactly
  where the customer app will launch.

### Changed
- **The customer app no longer auto-launches when the operator starts.** It now launches only
  when the operator presses **Launch** in the Run hub. A customer that was launched and then
  crashes is still auto-restarted, but a customer that was never launched (or was stopped) is
  not revived by a stray disconnect.
- Customer startup now logs the monitor it placed itself on (resolution + which display),
  to aid diagnosing wrong-screen / sizing reports.

### Fixed
- **Kiosk lockdown no longer disturbs the operator's screen.** Hiding the taskbar for the
  customer kiosk used to hide the *primary* taskbar unconditionally — a global operation that
  reflowed/​resized windows across the whole desktop (including the operator laptop) and often
  didn't restore them, the "messed up resolution/windows" symptom. The primary taskbar is now
  hidden only when the customer is actually on the primary monitor; in the normal setup
  (customer on the secondary booth touchscreen) the operator's taskbar is left untouched.
- **A deployed (Release) booth can no longer be put into developer mode.** Dev-mode launch
  flags (`--dev`, `--print`, `--skip-composite`, `--auto-token`) are honored only in DEBUG
  builds, so a stale `customer_dev_mode` setting in a booth's database is ignored on a packaged
  release — the kiosk always stays locked down and printing always enabled. The
  "Developer mode" toggle still works in local DEBUG dev builds.

[1.0.0]: https://github.com/Robo-Code-Technologies/rcti-meraki-photobooth-releases/releases/tag/v1.0.0
[1.0.0-beta.120]: https://github.com/Robo-Code-Technologies/rcti-meraki-photobooth-releases/releases/tag/v1.0.0-beta.120
[1.0.0-beta.118]: https://github.com/Robo-Code-Technologies/rcti-meraki-photobooth-releases/releases/tag/v1.0.0-beta.118
[1.0.0-beta.17]: https://github.com/Robo-Code-Technologies/rcti-meraki-photobooth-releases/releases/tag/v1.0.0-beta.17
