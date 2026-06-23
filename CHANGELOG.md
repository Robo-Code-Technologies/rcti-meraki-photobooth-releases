# Changelog

All notable changes to the RCTI Meraki Photobooth booth runtime are recorded here.
Versions are the Velopack/nbgv release versions published to the
`rcti-meraki-photobooth-releases` feed. Booths receive a version when an operator runs
**System tab → Check for updates**.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

> **The changelog is mirrored automatically on publish.** `deploy/build.ps1 -Publish` (1) pulls this
> version's section and sets it as the **GitHub Release notes** for the `v<version>` tag, and (2) pushes
> this whole file into the PUBLIC **`rcti-meraki-photobooth-releases`** repo (operators/public see only
> that repo). So just keep this file current — add the new version's section *before* publishing and both
> land automatically. This file lives in the private source repo. See `deploy/RELEASING.md`.

## [1.5.0] — 2026-06-23

**Real print tracking, a faster path to the camera, and a cleaner developer mode.** The booth now
records whether prints actually came out, shows live printer status while you run, and gets guests to
the camera in fewer steps. Developer mode no longer steps on your real settings, and booths can now
deliver photos by cloud only.

### Added
- **The booth now knows whether a print actually succeeded.** Print results are recorded from the
  printer's own confirmation (not just the button press): the Run hub shows today's tally as
  **"prints: N printed / M failed"**, and each session's detail shows an **Outcomes** row (red when a
  print failed). Operator reprints and test prints now wait for the printer and record the real
  result, so a failed print no longer counts as a successful copy.
- **Live printer status on the Run hub.** The media card now shows a status pill that updates as the
  printer works — **Printing…**, **Ready**, **Out of paper**, **Paper jam**, **Cover open**, **Ribbon
  low**, **Offline**, **Paused** — including during a reprint or test print with no customer on screen.
- **Cloud-only delivery (turn off on-site downloads).** The System tab's **Local transfer** card has a
  new **Offer local download** switch. With it off, guests receive their photos through the cloud
  gallery only — the customer screen drops the local "download now" QR. New and updated booths now
  default to cloud-only; an operator can switch local downloads back on at any time, and a guest is
  never left with no way to get their photos (the local QR still appears as a fallback if cloud upload
  isn't set up).

### Changed
- **A faster path to the camera.** After a guest enters a valid code, the booth now goes straight to
  choosing a template — the separate "here's how it works" screen is skipped. The template picker was
  rebuilt into a large preview of the selected strip alongside a scrollable grid of options, with a
  clear **Continue** button, and the timeout for picking was lengthened from 30 to 90 seconds so guests
  aren't rushed.
- **A shorter, cleaner ending.** The standalone "Thank you" screen is gone — the thank-you now lives on
  the delivery screen, which returns straight to the start. On Preview & Print, the low-key "no thanks
  — just send my photos" option moved to a subtle top-corner link and the "change filter" link was
  removed, leaving **Print** as the single obvious action.
- **Developer mode is now a clean override layer that never touches your real settings.** Dev-mode's
  "disable cloud upload", "disable printing", and "mock camera" are now overrides shown as **DEV
  OVERRIDE** on the affected cards, and your real production values are kept and restored the moment dev
  mode is turned off. The dev dialog now labels which toggles apply on the next session versus which
  need a Customer App restart, and a **Restart Customer App** indicator (Run hub Restart button glow +
  System card pill) lights up only when a restart-only change is actually waiting to be applied.
- **The Run hub now recognizes a customer app started outside the operator.** A kiosk launched
  independently reads **Running (external)** instead of wrongly showing "Stopped".

### Fixed
- **Turning off "Allow printing" now actually stops prints on a running booth.** The setting used to be
  read only at customer launch, so toggling it off mid-event was ignored; it's now checked live at
  print time.
- **A skipped print no longer counts as a print in the session history.** Choosing "no thanks" now
  records zero copies instead of one.
- **Restart no longer leaves a second kiosk running.** Pressing Restart on a customer the operator
  didn't launch itself now tears the old one down first instead of stacking a second kiosk on top.

## [1.4.0] — 2026-06-20

**A countdown guests can follow — and operator settings that apply without restarting the booth.**
The capture screen now shows how long until the photo is taken, gets a cleaner look, and several
settings that used to need a booth restart now take effect on the next session.

### Added
- **A countdown progress bar on the capture screen.** During the Auto Countdown, a slim bar fills
  across the whole lead-in — the "Get ready" beat plus the 3·2·1 — so guests can see exactly when the
  photo will be taken instead of being surprised by it. It eases to nearly full right as the shutter
  fires.

### Changed
- **A cleaner capture screen.** The "Get ready" moment and the "Start Countdown" button were restyled
  and right-sized — the button no longer covers the live view — and the countdown now reads
  "Smile in… 3 · 2 · 1".
- **Simpler Preview & Print actions.** "Change filter" is now a quiet link next to "No thanks", leaving
  **Print my strip** as the single clear action.
- **Auto Countdown timing now applies without a restart.** Changing the get-ready length, the countdown
  length, or the tap-to-start option in Booth Defaults now takes effect on the very next session — no
  need to restart the booth.
- **Customer theme changes now apply without a restart.** Switching the booth's customer theme takes
  effect the next time the booth returns to the welcome (attract) screen.
- **"Mirror camera" now applies without a restart.** Turning mirroring on or off takes effect on the
  next photo session.

## [1.3.0] — 2026-06-20

**Operators can now mix and match how the shutter fires.** Tap to Capture used to be permanently on
and couldn't be turned off. Now the capture modes are a free choice — turn on whichever the booth (or
a specific event) should offer, in any combination, as long as at least one stays on.

### Changed
- **Capture modes are now a free multi-select.** In Booth Defaults and per-event settings, both **Tap
  to Capture** and **Auto Countdown** can be switched on or off independently. The only rule is that at
  least one must stay on — trying to turn off the last remaining mode snaps it back on. This makes
  **Auto-Countdown-only** booths possible for the first time; when both modes are on, guests still pick
  before capture, exactly as before.

## [1.2.1] — 2026-06-20

**Polish and fixes for the new Preview & Print screen.** This refines the reveal, changes how
walk-aways are handled, makes the post-capture wait dramatically faster, and fixes session media
deletion in the operator console.

### Changed
- **A more cinematic Preview & Print reveal.** The keepsakes now fly up and fan out, hold for a beat,
  then deal out one at a time, with a bolder headline and a longer, easier-to-read guided tour that
  names each keepsake.
- **Walk-aways now go to the phone, not the printer.** If a guest leaves the Preview & Print screen,
  the booth no longer auto-prints; after five minutes of no activity it delivers the photos digitally
  instead. Pressing **Print** still prints as before.
- **The post-capture wait is much faster.** "Developing your strip" now takes roughly two to three
  seconds instead of about seventeen — the animated strip and filters are prepared far more
  efficiently (the look is applied while the photos are drawn, frames are lighter, and the strips
  render in parallel).

### Fixed
- **Deleting a session's media fully clears it.** Deleting the photos for a session used to leave the
  session behind as an empty shell full of "missing on disk" tiles that could never be cleared. Media
  now deletes cleanly, leaving only the intended record stub for the audit trail. Sessions already
  left in the broken state are repaired automatically on update.
- **Re-picking a filter updates everything.** Changing the look after it was first applied now updates
  the GIF and live video strip too, not just the printed strip.
- **Removed a leftover "Ready to print?" step** that could briefly appear before the new Preview &
  Print screen.

## [1.2.0] — 2026-06-20

**One "Preview & Print" screen — and a much faster filter step.** After capturing, guests now see
all their photos on a single screen that introduces each keepsake, lets them pick a look once, and
prints — with no separate loading screens in between.

### Added
- **Preview & Print, all in one place.** The old separate preview, processing, and print screens are
  now a single screen. When it opens it gives guests a short guided tour of everything they got — the
  printed strip, the looping GIF, and the live video strip — sliding to each with a big label, then
  settles on the strip. Guests can swipe between the keepsakes (the others peek in from the sides), and
  a caption always names what they're looking at and whether it prints or goes to their phone.
- **Print is instant.** The printable strip is now prepared in the background during the loading beat,
  so pressing **Print** prints right away instead of waiting on a "compositing" screen.
- **A clear way to skip the print (for testing/demos).** A deliberately low-key link sends the photos
  to the phone without printing; guests who simply walk away still get their strip printed automatically.

### Changed
- **Pick a filter once, for everything.** The chosen look now applies to the printed strip, the GIF,
  and the live strip together — the separate "also apply to my GIF / live strip" checkboxes are gone.
- **A shorter, smoother flow.** The standalone "compositing" loading screen is retired (there's nothing
  left to wait for after the background prep), and choosing to print now happens right on the preview
  with a pop-out hand-off into the printing animation.

### Fixed
- **The "mixing your looks" wait is gone.** Building the eight filter previews used to take around 30
  seconds because it re-processed the full-resolution photos for every look, one at a time. It now
  shrinks the photos once and builds all the looks in parallel — the wait drops to roughly a second or
  two. The post-capture "developing your strip" beat got the same speed-up.

## [1.1.0] — 2026-06-18

**Guided first-run setup.** A freshly installed booth now walks the operator through everything it
needs before an event, instead of expecting them to find each setting on their own. Plus tools to
hand the booth to a new client and a tidy uninstall.

### Added
- **First-run setup guide (M20).** The first time a new booth opens, a friendly full-screen guide
  walks through naming the booth, choosing where photos are saved, picking the screen guests use, and
  setting up the camera and printer — with optional steps for an online gallery and phone downloads. A
  table-of-contents sidebar shows progress, the wording explains what each thing is (written for
  someone new to the booth), and you can re-open the guide any time from **System → Re-run setup**.
- **Phone downloads, made obvious (M20).** The booth now shows whether guest phone-downloads are
  already set up, so you never repeat the one-time permission step. Turning it on is one click, and the
  booth restarts itself to start serving — no more “remember to restart”.
- **Reset booth for a new client (M20).** **System tab → Reset booth** clears all photo sessions,
  events, and tickets and returns the booth to a fresh setup, while **keeping your templates and
  design packs** and leaving saved photos on disk untouched.

### Changed
- **A clean uninstall (M20).** Uninstalling the booth now removes its data instead of leaving it
  behind. The built-in templates and design packs come back automatically on the next install.

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

[1.5.0]: https://github.com/Robo-Code-Technologies/rcti-meraki-photobooth-releases/releases/tag/v1.5.0
[1.4.0]: https://github.com/Robo-Code-Technologies/rcti-meraki-photobooth-releases/releases/tag/v1.4.0
[1.3.0]: https://github.com/Robo-Code-Technologies/rcti-meraki-photobooth-releases/releases/tag/v1.3.0
[1.2.1]: https://github.com/Robo-Code-Technologies/rcti-meraki-photobooth-releases/releases/tag/v1.2.1
[1.2.0]: https://github.com/Robo-Code-Technologies/rcti-meraki-photobooth-releases/releases/tag/v1.2.0
[1.1.0]: https://github.com/Robo-Code-Technologies/rcti-meraki-photobooth-releases/releases/tag/v1.1.0
[1.0.0]: https://github.com/Robo-Code-Technologies/rcti-meraki-photobooth-releases/releases/tag/v1.0.0
[1.0.0-beta.120]: https://github.com/Robo-Code-Technologies/rcti-meraki-photobooth-releases/releases/tag/v1.0.0-beta.120
[1.0.0-beta.118]: https://github.com/Robo-Code-Technologies/rcti-meraki-photobooth-releases/releases/tag/v1.0.0-beta.118
[1.0.0-beta.17]: https://github.com/Robo-Code-Technologies/rcti-meraki-photobooth-releases/releases/tag/v1.0.0-beta.17
