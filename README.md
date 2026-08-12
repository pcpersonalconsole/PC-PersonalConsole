<!--
  This is the README for the PUBLIC, RELEASES-ONLY repository - not for this one.
  It lives here so it is versioned and easy to find; copy its contents into the new repository.
  ⛔ It deliberately describes no source layout, no file paths and no internals: that repository
     carries releases only, and the source stays private.
-->

# PersonalConsole

A Windows shell replacement you drive with a game controller.

PersonalConsole replaces the Windows desktop with a full-screen, controller-first console interface:
a tile desktop with tabs, an on-screen keyboard with word prediction, radial menus, a file browser and
a system panel — all navigable without ever reaching for a mouse.

Official website: https://www.patreon.com/cw/personalconsoledev/membership

> Download: **[Releases](../../releases)**



## Requirements

| | |
|---|---|
| **OS** | Windows 10 version 2004 (build 19041) or newer, 64-bit |
| **Privileges** | **Administrator.** The application requests elevation on launch and will not work without it |
| **Controller** | Any XInput controller (Xbox and compatible pads) |

**About administrator rights:** Windows silently discards synthetic input sent to an elevated window
from a non-elevated process. Since the whole point is to drive other applications with a controller,
the application has to run elevated or its input would vanish over any elevated window.

**Controllers in DInput mode:** a pad switched to DInput is invisible to XInput. PersonalConsole reads
such a pad directly and treats it like any other, but note that force feedback is not available on that
path. PlayStation and Switch controllers are not exposed to XInput by Windows at all; use a bridge such
as DS4Windows or Steam Input, which presents them as a virtual Xbox pad.

---

## Installation

**Installer (recommended)**

1. Download `PersonalConsole-Setup-vX.Y.Z.exe` from the [Releases](../../releases) page.
2. Run it and accept the elevation prompt. The wizard shows the licence, lets you choose the install
   folder, and asks whether you want a desktop shortcut.

**Portable ZIP**

1. Download `PersonalConsole-vX.Y.Z-win-x64.zip`.
2. Extract it to a folder you control. Do not run it from inside the ZIP.
3. Run `PersonalConsole.exe` and accept the elevation prompt.

Either way, settings are stored in your Documents folder under `PersonalConsole` — see below.

### Removing it

Close the application — the Windows desktop and taskbar are restored on exit — then uninstall it from
**Settings → Apps**, or delete the folder if you used the portable ZIP. If you enabled "Launch on
Startup", turn it off first so the scheduled task is removed.

**Uninstalling never deletes your settings.** See below for where they are.

## First run

1. Connect a controller before launching, so it is detected at startup.
2. Open **Preferences → Controller** to confirm which input path is reading your pad.
3. Open **Console Mode** and enable the console desktop.
4. Use the mapping page to bind buttons; the Desktop profile is the one that applies when no specific
   application profile matches.

Settings, profiles and themes live in `Documents\PersonalConsole`. Copying that folder to another
machine carries your setup with it, with the exception of saved passwords, which are encrypted to the
machine that stored them and cannot be transferred.

---

## Updates

**Preferences → Update → Check for updates** compares the running build against the latest release here
and opens this page when a newer one exists. The check runs only when that button is pressed.

---

## Known limitations
- **One controller drives the interface.** Additional pads can run their own profiles, but menu
  navigation belongs to the first one.

---

## Support

If PersonalConsole is useful to you, contributions are what keep it being developed:

**[Donate on Patreon](https://www.patreon.com/cw/personalconsoledev)**

Bug reports and feature requests are welcome in [Issues](../../issues). Please include your Windows
version, your controller model, and what you expected to happen.

---

## License

PersonalConsole is proprietary software, free for personal use. It may not be sold,
redistributed, modified or reverse engineered. It is provided with no warranty of any kind.

See [LICENSE.txt](LICENSE.txt) for the full terms, which also describe what the application does to
your system — it runs elevated and replaces the Windows shell while its console desktop is enabled.
