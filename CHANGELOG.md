# Changelog

Release notes for the RCTI Meraki Photobooth booth runtime. The packaged installers for each
version are on the [Releases](../../releases) page; this file is the human-readable summary of
what changed. Versions follow [Nerdbank.GitVersioning](https://github.com/dotnet/Nerdbank.GitVersioning)
(`1.0.0-beta.N`) and the format loosely follows [Keep a Changelog](https://keepachangelog.com).

## [1.0.0-beta.13] — 2026-06-08

### Fixed

- **DNP 2×6 strips now print correctly.** The booth previously duplicated the strip itself and sent
  a doubled 4×6 sheet, which double-cut on printers whose driver already produces 2×6 via its own
  cut form. The booth now sends a **single clean 2×6 strip** and selects the driver's 2×6 cut form,
  letting the printer lay it two-up on a 4×6 and cut it — the correct DNP behavior. One session
  still yields the two identical strips, from the driver's cut rather than an app-side copy.

### Changed

- **Removed the "Cut queue" setting** from the Operator System tab. It configured a second Windows
  print queue for cut products, which is no longer needed now that the driver's 2×6 cut form does
  the cut. All jobs print to the one configured printer. Existing booths need no action — the field
  simply disappears and any previously saved value is ignored.

### Operator notes

- For 2×6 strips, ensure the **2-inch cut form is enabled in the DNP driver** (Printing Preferences).
  If the driver isn't exposing a 2×6 cut form, a print attempt now reports that directly instead of
  asking you to change the media roll.

---

> Releases before `1.0.0-beta.13` predate this changelog and were build-only iterations; see the
> [Releases](../../releases) page for their installers.
