# RCTI Meraki Photobooth — Releases

Public **release feed** for the RCTI Meraki Photobooth booth runtime.

This repository contains **only the packaged application binaries** ([Velopack](https://github.com/velopack/velopack)
packages) published as GitHub Releases. There is **no source code here** — the source lives in a
separate private repository. This split is intentional: booths read this feed **anonymously, with no
token**, so a public binaries-only repo is all they need.

> Nothing is committed to this repo by hand. Every release is produced and uploaded by the build
> pipeline (`deploy\build.ps1 -Publish`) in the source repo. The `main` branch stays essentially
> empty — the payload is on the [Releases](../../releases) page.

## Install

1. Download `MerakiPhotobooth-win-Setup.exe` from the [latest release](../../releases/latest).
2. Run it. It installs per-user to `%LocalAppData%` (no admin needed).
3. Local transfer (guests downloading over the booth hotspot) needs a one-time admin step on each
   booth — see the provisioning runbook in the source repo.

> Releases are currently **unsigned**, so Windows SmartScreen will warn on first install. Choose
> "More info → Run anyway".

## Updates

Updates are **operator-triggered**, not automatic. In the booth's Operator console, the **System**
tab has a **Check for updates** card that reads this feed, shows the available version, and — on
demand — downloads and restarts onto the new version. Applying an update is blocked while a customer
session is live, so an update can never interrupt a guest mid-session.

## Versioning

Versions are derived from git via [Nerdbank.GitVersioning](https://github.com/dotnet/Nerdbank.GitVersioning)
and currently ship as `1.0.0-beta.N` pre-releases. The installer, the executables, and this feed all
carry the same version number.
