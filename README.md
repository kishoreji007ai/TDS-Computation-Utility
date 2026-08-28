# TDS Audit Utility — Desktop (.exe) Build Kit

This folder turns the TDS Automated Computation & Audit Utility into a **standalone
Windows desktop app** with its own window and icon — no browser, no install of
anything on your side to *use* it once built.

It uses [Tauri](https://tauri.app), so the finished installer is small (~3–5 MB)
and the app runs on the Windows system WebView (Edge) that's already on every
Windows 10/11 machine.

You have two ways to get the `.exe`. **Option A needs no software on your computer.**

---

## Option A — Let GitHub build the .exe for you (recommended, no tools needed)

You'll upload this folder to a free GitHub account. GitHub's Windows machines
compile the app and hand you back the installer. It takes about 10 minutes, most
of which is waiting.

1. **Make a free GitHub account** at https://github.com if you don't have one.

2. **Create a new repository:**
   - Click the **+** (top-right) → **New repository**.
   - Give it any name, e.g. `tds-audit-utility`.
   - Choose **Public** (free build minutes) or Private (also fine).
   - Click **Create repository**.

3. **Upload this folder's contents:**
   - On the new repo page, click **uploading an existing file**
     (the link in "…or upload an existing file").
   - Drag in **everything inside this `tds-desktop` folder** — the `src`,
     `src-tauri`, and `.github` folders and the `package.json`, `.gitignore`,
     and this `README.md`. (Drag the *contents*, not the outer folder.)
   - Click **Commit changes**.

4. **The build starts automatically.** Click the **Actions** tab at the top of
   the repo. You'll see a run called **Build Windows App** with a spinning icon.
   Wait for the green check (~8–10 minutes the first time).

   > If it doesn't start on its own, open **Actions**, pick **Build Windows App**
   > on the left, and click **Run workflow**.

5. **Download your installer.** Click the finished (green) run, scroll to
   **Artifacts** at the bottom, and download **TDS-Audit-Utility-Windows**.
   Inside the zip you'll find:
   - a **`.exe` setup installer** (NSIS) — the usual double-click-to-install file, and
   - a **`.msi`** — same app, for environments that prefer MSI.

   Run either one; it installs "TDS Audit Utility" with its icon into the Start
   menu like any normal program.

That's it. To rebuild after I send you an updated `index.html`, just replace
`src/index.html` in the repo (Add file → Upload files) and the build re-runs.

---

## Option B — Build it yourself on a Windows PC

Only if you'd rather build locally. On a Windows 10/11 machine:

1. Install **Node.js LTS**: https://nodejs.org
2. Install the **Rust toolchain**: https://www.rust-lang.org/tools/install
3. Install the **Microsoft C++ Build Tools** (Tauri needs them):
   https://visualstudio.microsoft.com/visual-cpp-build-tools/
   — during setup, tick **"Desktop development with C++"**.
4. Open a terminal in this folder and run:
   ```
   npm install
   npm run build
   ```
5. Your installer appears in:
   ```
   src-tauri\target\release\bundle\nsis\   (the .exe setup)
   src-tauri\target\release\bundle\msi\    (the .msi)
   ```

---

## What's in here

| Path | What it is |
|------|-----------|
| `src/index.html` | The entire utility — this is the app's UI. Replace this file to update the app. |
| `src-tauri/tauri.conf.json` | Window size, app name, icon, and installer settings. |
| `src-tauri/icons/` | The app icon in all required formats. |
| `src-tauri/src/main.rs` | Tiny Rust launcher that opens the window. |
| `src-tauri/Cargo.toml`, `build.rs` | Rust build configuration. |
| `package.json` | Pulls in the Tauri build tool. |
| `.github/workflows/build-windows.yml` | The recipe GitHub follows to build the .exe. |

---

## A note on the security warning

The installer is **unsigned** (code-signing certificates cost money and must be
bought in your own name). The first time you run it, Windows SmartScreen may show
**"Windows protected your PC"** → click **More info** → **Run anyway**. This is
normal for any unsigned app; it isn't a sign of a problem with the file. If you
plan to distribute this widely, look into an **OV/EV code-signing certificate**
to remove that prompt.

## Updating the tax logic

All the TDS rates, payment codes, and report logic live inside
`src/index.html`. When the rules change (or I send you a new build), swap that one
file and rebuild — nothing else needs to change.
