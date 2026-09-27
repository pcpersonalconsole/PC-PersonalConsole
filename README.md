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
  shortcuts or macros, with tap / double-tap / hold / release slots and eight switchable layouts. One
  slot can hold several of these at once — a key and a mouse button go out together on one press.
- **Analog control** — either stick can drive the mouse pointer or synthesise directional keys, with
  adjustable dead zone, sensitivity and acceleration.
- **On-screen keyboard** — two of them. *Realtime* types straight into the focused application;
  *Buffer* collects the text and delivers it when you close the keyboard. Word prediction and
  next-word suggestions, with dictionaries that learn as you type. Multiple languages included, and
  you can add your own.
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
| **Privileges** | **Administrator.** Windows asks for your permission at every launch, and the application will not start without it |
| **Controller** | See the table below — Xbox and compatible pads work as they are |

### Which controllers work

| Controller | Works | What you need to do |
|---|---|---|
| **Xbox** (360, One, Series) and any pad that presents itself as one | **Yes** | Nothing. Plug it in. |
| **Third-party pads with an XInput mode** — 8BitDo, GameSir, PowerA, Nacon and similar | **Yes** | Put the pad in XInput mode, usually a switch on the device or a button combination at power-on. |
| **The same pads in DInput mode** | **Yes** | Nothing — the application reads them directly. Rumble is not available on this path, and rear paddles cannot be told apart from the buttons they copy. |
| **PlayStation** (DualShock 4, DualSense) | **Through a bridge** | Windows does not expose these to XInput at all. Install DS4Windows or use Steam Input, which presents the pad as a virtual Xbox controller; the application then sees an Xbox pad. |
| **Nintendo Switch Pro / Joy-Con** | **Through a bridge** | Same as above. |
| **Generic USB gamepads** | **Usually** | If Windows lists it under *Set up USB game controllers*, the application can read it on the DInput path. Layout quality varies by device. |

📌 **One controller drives the menus.** Extra pads can run their own profiles at the same time, but
interface navigation belongs to the first one.

**About administrator rights:** Windows silently discards simulated input sent from an ordinary
program into a window that belongs to a program running as administrator. Since the whole point is to
drive other applications with a controller, this one has to run as administrator — otherwise its input
would vanish over any such window, with no error to explain why.

**Why DInput is a separate path:** a pad switched to DInput is invisible to XInput, so the application
reads it straight from the device instead. It then behaves like any other controller — the differences
are listed in the table above.

---

## Installation

**Installer (recommended)**

1. Download `PersonalConsole-Setup-vX.Y.Z.exe` from the [Releases](../../releases) page.
2. Run it and accept the Windows permission prompt. The wizard shows the licence, lets you choose the install
   folder, and asks whether you want a desktop shortcut.

**Portable ZIP**

1. Download `PersonalConsole-vX.Y.Z-win-x64.zip`.
2. Extract it to a folder you control. Do not run it from inside the ZIP.
3. Run `PersonalConsole.exe` and accept the Windows permission prompt.

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

### What a fresh install starts with

Nothing here is hidden, so you can decide what to change before you change anything.

| Setting | Default | Why |
|---|---|---|
| **Console desktop** | **Off** | It replaces your desktop. Taking that over on first launch, before you have seen what it is, would be the wrong first impression — you turn it on when you are ready. |
| **Auto hide/show on input** | Off | The console desktop stays where you put it until you ask for this. |
| **Launch on Windows startup** | Off | |
| **Minimise to the system tray** | On | Closing the window leaves the controller engine running; that is the point of the program. |
| **Start minimised** | Off | Only applies to the automatic startup anyway. |
| **Virtual controller output** | Off | It needs a driver installed separately. Off until you install it and choose it. |
| **Diagnostic logging** | **On** | So a problem you hit in this prerelease can actually be explained. Logs stay on your machine and are deleted after 7 days — Preferences → System turns it off. |
| **Built-in file browser** | On | |
| **Show hidden items** | Off | |
| **Profiles** | Desktop, Virtual Keyboard | The two built-in ones. Desktop is what applies when no application-specific profile matches; neither can be deleted. |
| **Templates** | Desktop, Virtual Keyboard | Ready-made copies of the two profiles above, so **Profiles & Templates → Templates** is not empty on a new machine. They are only placed when you have no templates of your own, so an update never overwrites yours. |

More templates are shared at **<https://www.pcpersonalconsole.com/communityfiles>**. Download a `.json`
and load it with **Import Template**; **Export Template** writes one out to share. Four toggles choose
what a template carries, so you can move only the parts you want.

---

## Is this safe to run?

It asks for administrator rights, it hides your taskbar while it runs, and it is closed source. Those
are fair reasons to want more than a reassurance.

- **Administrator:** required, because Windows discards simulated input sent into the windows of
  programs that are themselves running as administrator — without it your controller would stop
  working in some applications, with no error to explain why.
- **Network:** one address, `api.github.com` — once at startup, and again if you press the update
  button. Nothing about you is sent. No telemetry, no automatic downloads.
- **Your data:** `Documents\PersonalConsole`, in plain files you can open. Saved passwords are
  encrypted to your Windows account and cannot be read on another machine.

**[SECURITY.md](SECURITY.md) explains each of these with a way to verify it yourself** — including
the parts that count against the program, and the things this kind of page cannot prove at all.

---

## Updates

**Preferences → Update → Check for updates** compares the running build against the latest release here
and opens this page when a newer one exists.

Since v0.2.0 the same check also runs **once when the application starts**. If a newer release is out,
a small dot appears on the *Preferences* entry and on the *Update* tab — so you find out without going
looking. It downloads and installs nothing; opening the page is still your decision. If there is no
internet the check gives up quietly and nothing is shown.

---

## Known limitations

- **No force feedback on DInput pads.** For this class of controller Windows reports no haptics and no
  force-feedback motors on the raw path; every writable output report the device declares was tried and
  none moved a motor. A pad running in XInput mode rumbles normally.
- **Extra paddles (L4/R4) cannot be bound.** Controllers with rear paddles copy them onto an existing
  button in their own firmware, so nothing distinguishable ever reaches the driver. Map the paddle to a
  spare button in your controller's own software, then bind that button here.
- **Force-closing the application while the Console Desktop is open leaves the desktop broken.**
  Ending the process from Task Manager skips the step that gives Windows its shell back, so the
  desktop icons stay hidden and the taskbar stays unclickable. To recover: start PersonalConsole
  again and then close it normally. This is not fully solved yet. Closing the application normally
  is unaffected.
- **The Console Desktop's shortcut layout stays on the machine it was arranged on.** Profiles, themes
  and radial menus travel with your Documents folder; which programs sit on which tab does not,
  because that list is built from what is installed on the machine you are using. The same applies to
  the folders you pin in File Explorer — as of 0.2.1 those are per machine too, since they point at
  paths that may not exist on another one. Existing pins are kept when you upgrade. The same boundary
  applies to *Share Settings With Other Accounts*: it can share your profiles, appearance, radial
  menus and keyboard with other accounts on this computer, but never the desktop layout or the pins.
- **Tray menus of system-account programs go through a helper service.** Icons drawn by a program
  that Windows runs as the system account — Apollo and the NVIDIA settings icon are the two common
  ones — sit above what any ordinary application may touch, so setup installs a small helper service
  that is allowed to read them. It starts only when you hold **A** on such an icon and is removed
  when you uninstall. Where it cannot start — a policy or security tool blocking it — holding **A**
  says the menu cannot be opened, which is how it behaved everywhere before.
- **In DirectInput mode a game can still read the controller while you type**, unless *Exclusive
  Controller Access* is on. When it is off a game sees two controllers — your own and the
  stand-in this application presents — and which one it uses is the game's decision, not a setting.
  Measured across three games: two ignored the physical controller and stayed quiet while the
  Realtime keyboard was open, one did not and kept moving the character. If a game keeps reacting
  while you type, turn hiding on; that leaves it a single controller to find.

_The full list for the current release, including what was fixed, is in the release notes on the
release page._

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
your system — it runs as administrator and replaces the Windows shell while its console desktop is
enabled.

---

## What Windows asks you during installation

Installing this takes more clicking than most programs, and none of the prompts means something is
wrong with the download. Here is every one of them, in the order you meet it, and why it appears.

| What you see | Why |
|---|---|
| Your browser says the file is not commonly downloaded | The file has no code-signing certificate, so the browser has no publisher to recognise. |
| **Windows protected your PC** (SmartScreen) | Same reason. Choose **More info → Run anyway**. |
| **User Account Control**, showing *Unknown publisher* | The application needs administrator rights, and without a certificate Windows has no name to show. Why it needs them is under *Is this safe to run?* above. |
| A second permission prompt when you install **ViGEmBus** | Optional driver, installed from its own project, and it is signed — you will see its publisher name. Only needed for typing in games without pausing them. |
| A third one when you install **HidHide** | Optional driver, same arrangement. Only needed to hide your controller from games. |

The first three exist because the downloads are **not signed with a code-signing certificate**: a
certificate is a recurring cost that has not been bought for this project. The two drivers are
separate programs by another author, so they ask for permission on their own behalf; the application
never installs them behind your back — it opens their download page and you decide.

**What you can check instead of trusting the prompts.** Every release publishes `SHA256SUMS.txt`
beside the downloads, so you can confirm the file you have is the file that was published:
`Get-FileHash <file>` in PowerShell and compare. **SECURITY.md** describes what the application does
to your machine and how to verify each claim yourself.
