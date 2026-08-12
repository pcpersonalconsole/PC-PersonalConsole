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

> Download: **[Releases](../../releases)**

---

## Screenshots

<!-- Add images here. Suggested: console desktop, radial menu, virtual keyboard, mapping page. -->
| Console desktop | Radial menu |
|---|---|
| _screenshot_ | _screenshot_ |

---

## What it does

- **Console desktop** — full-screen tile launcher with tabs, custom ordering, hidden items, a clock and
  a power menu. Replaces the Windows desktop while it is running and hands it back when it closes.
- **Controller mapping engine** — per-application profiles. Any button can send keys, mouse actions,
  shortcuts or macros, with tap / double-tap / hold / release slots and eight switchable layouts.
- **Analog control** — either stick can drive the mouse pointer or synthesise directional keys, with
  adjustable dead zone, sensitivity and acceleration.
- **On-screen keyboard** — two modes: one types straight into the focused application, one buffers the
  text and delivers it when you are done. Word prediction and next-word suggestions, with dictionaries
  that learn as you type. Multiple languages included, and you can add your own.
- **Radial menus** — wheel or hotbar, bound to any button, with per-item colours and symbols and
  nested submenus.
- **File browser and system panel** — a controller-navigable file manager, and a system page for
  display, storage, running applications and installed programs.
- **Password field support** — credentials are stored encrypted with Windows DPAPI and never leave the
  machine they were entered on.
- **Themes** — several built in, plus a custom theme editor.

---

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

---

## Where your data lives

Everything the application saves is kept in one place, split by whether it can be carried to another
computer:

```
Documents\PersonalConsole\
├── Shared\            everything you can copy to another PC:
│                      profiles, themes, keyboard settings, layouts, dictionaries, templates
├── <YOUR-PC-NAME>\    everything tied to this PC:
│                      desktop layout, tabs, pinned folders, recent applications
└── Logs\              diagnostic logs (kept for 7 days)
```

- **Uninstalling leaves this folder untouched.** Reinstalling picks it up again, and so does an update —
  updates replace only the program itself.
- **Moving to a new computer:** copy the `Shared` folder across — that is exactly what it is for. Your
  profiles, themes, keyboard settings and layouts come with it. The folder named after your PC is
  deliberately left behind, because the desktop layout it holds describes the shortcuts installed on
  that particular machine and would be wrong on another one.
- **Saved passwords are the one exception.** They are stored in
  `%LOCALAPPDATA%\PersonalConsole` instead, encrypted and tied to your Windows account on that
  computer. They are deliberately kept out of Documents so they are never uploaded to a cloud folder,
  and they cannot be decrypted on another machine even if copied. Enter them again on the new computer.
- **Starting over:** close the application and delete `Documents\PersonalConsole`. It is recreated with
  defaults on the next launch.

---

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

- **No force feedback on DInput pads.** For this class of controller Windows reports no haptics and no
  force-feedback motors on the raw path; every writable output report the device declares was tried and
  none moved a motor. A pad running in XInput mode rumbles normally.
- **Extra paddles (L4/R4) cannot be bound.** Controllers with rear paddles copy them onto an existing
  button in their own firmware, so nothing distinguishable ever reaches the driver. Map the paddle to a
  spare button in your controller's own software, then bind that button here.
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

---

## A note on SmartScreen

The downloads are not signed with a code-signing certificate, so Windows SmartScreen will warn that
the publisher is unknown the first time you run the installer. Choose **More info → Run anyway** if
you are happy to proceed. A signing certificate is a recurring cost and has not been bought for this
project.
