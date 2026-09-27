# What PersonalConsole does to your machine

PersonalConsole is closed source. That means you cannot read the code, so nothing on this page asks
you to take its word for anything.

Every claim below comes with **a way to check it yourself**, using tools that are already on your
computer. Where something cannot be checked from the outside, this page says so instead of pretending
otherwise — see [What this page cannot prove](#what-this-page-cannot-prove) at the end.

If a claim here turns out to be false, that is a bug report worth making.

---

## The short version

- It **runs as administrator**, and it needs to. This is the single most important thing on this page.
- It **hides the Windows taskbar and desktop** while the Console Desktop is open, and puts them back.
- It **turns controller input into keyboard and mouse events** — that is the product.
- It talks to **one** address on the internet: once when it starts, and again if you press the
  update button. Nothing else, and nothing about you is sent.
- Your settings live in **your Documents folder**; your saved passwords do not, and never leave the
  machine they were entered on.

---

## Administrator rights

**What it does.** The application requests administrator rights at launch. You will see the Windows
UAC prompt every time you start it.

**Why.** Windows silently discards synthetic input sent from an ordinary program into a window that
belongs to a program running as administrator. Without administrator rights, your controller would
stop working the moment you focused such a window — with no error and no explanation. Driving other
applications is the entire point of this one.

**How you check it.** The UAC prompt *is* the check, and it is not optional: press **No** and the
application does not start at all. Nothing about this can be hidden from you, because Windows itself
asks the question every single time.

> ### Say the uncomfortable part plainly
>
> Running as administrator means the ceiling on what this program *could* do to your machine is very
> high. That is true of every program you grant those rights to, and it is true here. The rest of this
> page exists so you can watch what it actually does rather than trust what it says it does.

**And a consequence you should know about:** applications you launch **from the Console Desktop
inherit those administrator rights**. If you start a game or a browser from the tile desktop, it runs
with administrator rights too. If that matters to you, start those applications the normal Windows way
instead.

**How you check that.** This one is a comparison, which makes it stronger than any single reading:

1. Open **Command Prompt** the normal way, from the Start menu. Run:
   ```
   whoami /groups | findstr /i mandatory
   ```
   It prints `Mandatory Label\Medium Mandatory Level` — ordinary rights.
2. Now add Command Prompt to the Console Desktop and launch it **from there**. Its title bar reads
   **`Administrator: Command Prompt`**, and the same command prints
   `Mandatory Label\High Mandatory Level`.

That difference is the inheritance, shown by Windows rather than claimed by this page. It applies to
anything you start from the tile desktop, not just Command Prompt.

---

## The Windows shell

**What it does.** While the Console Desktop is open it hides the taskbar and the desktop icons so its
own full-screen interface can take their place. Closing it puts them back.

**Nothing is deleted or replaced.** The taskbar is hidden and disabled, not destroyed, and the
registry key Windows uses to decide which program is the shell (`Winlogon\Shell`) is **not touched**.
This is a deliberate choice: a program that rewrites that key and then fails to undo it leaves you
with no desktop at all.

**How you check it.** Open the Console Desktop, then close it, and confirm the taskbar and icons come
back. For the registry key: `regedit` →
`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon` → the `Shell` value should
still read `explorer.exe`, before and after.

**The honest limit.** If the application is killed while the Console Desktop is open — Task Manager's
*End task*, a power cut, a crash — the taskbar stays hidden until you **start the application again**,
which repairs it. If you would rather fix it by hand: press `Ctrl+Shift+Esc`, *Run new task*, type
`explorer.exe`, tick *Create this task with administrative privileges*.

---

## Controller input

**What it does.** It reads your controller and generates keyboard and mouse events from it, according
to the profile you configured. Those events go to whichever window is in the foreground — the same
place your real keyboard's events go.

**What it does not do.** It does not record what you type. There is no keystroke log, and no file
that accumulates the text you enter.

**How you check it.** Two ways, and the second is the stronger one:

1. Resource Monitor (`resmon`) → **Network** tab. Type with the on-screen keyboard, use the
   controller, play for a while. `PersonalConsole.exe` should not appear as sending data. If it
   sends bytes while you type, this page is lying to you.
2. **Cut it off entirely.** Windows Defender Firewall → *Advanced settings* → *Outbound Rules* →
   *New Rule* → Program → point it at `PersonalConsole.exe` → Block. Everything except the update
   check keeps working — the program starts and runs normally, it simply never finds out whether a
   newer version exists. A program that needed to phone home would not survive that.

**The word-prediction dictionaries are local files** that grow as you type, and you can read them.
They live next to your other settings (see below), one `.json` file per language, and they open in
Notepad. If you want to see exactly what the keyboard has learned, open them. If you want it to
forget, delete the words.

> ⚠️ While you are entering a password into the password manager, learning and prediction are
> switched off for that entry, so the password cannot end up in a dictionary file. You can verify
> this the same way: type a password, then open the dictionary file and search for it.

---

## Network

**One address, two moments.** The application contacts `api.github.com` to ask which releases exist.
It does this **once when it starts**, and again **when you press the update button**. That is the
whole of its network activity.

**What the request carries.** A single web request asking for the list of releases. It sends a name
and version so the server will answer at all (`PersonalConsole/0.2.0` — this is required; GitHub
refuses requests that do not identify themselves) and nothing else: no account, no licence key, no
identifier, and nothing about you, your machine or what you do with the program. If it cannot
reach the internet it gives up quietly — no error, no retry loop, no dialog in your way.

> **The startup check is new in v0.2.0.** Earlier versions only checked when you pressed the button.
> It was added so that you find out a fix exists without having to go looking; it does not download
> or install anything, and the paragraph below still holds.

**There is no automatic update.** It never downloads or runs anything by itself. When a newer version
exists it opens the download page in your browser, and installing it is your decision and your
double-click. This is deliberate: a program running as administrator that downloads and executes
files is a different trust question entirely, and this one does not ask you to answer it.

**No telemetry, no analytics, no crash reporting, no licence check.**

**How you check it.** Open Resource Monitor (`resmon`) → **Network** tab *before* you start the
application, and watch the row for `PersonalConsole.exe`:

- **At launch** you should see **one** connection, to `api.github.com`. One, and then nothing.
- **While you use it** — playing, typing on the on-screen keyboard, opening the Console Desktop for
  as long as you like — the row should stay silent. If bytes keep moving, this page is lying to you.
- **Press the update button** and a second connection appears, to the same address.

Stronger still: block it in the firewall as described above, then start it. It runs normally and
the Update page simply reports that it could not check. Nothing else changes — which is what
"the network is not load-bearing here" means, and you can see it rather than take our word for it.

**The donation link** opens your browser at a Patreon page when you click it. That is a normal link;
the application sends nothing.

---

## Where your files go

| What | Where | Notes |
|---|---|---|
| Profiles, mappings, themes, radial menus, keyboard dictionaries | `Documents\PersonalConsole\Shared\` | Plain XML and text. Copy the folder to another PC and it works there. |
| Desktop layout, tabs, pinned items, remembered program locations | `Documents\PersonalConsole\<COMPUTER-NAME>\` | Per machine, because it describes shortcuts and paths that exist on *this* machine. The remembered locations are only used to draw a profile's icon. |
| Diagnostic logs | `Documents\PersonalConsole\Logs\` | Deleted automatically after 7 days. |
| Saved sign-ins | `%LOCALAPPDATA%\PersonalConsole\credentials.dat` | Encrypted, and deliberately **not** in Documents. |
| Settings you choose to share with other Windows accounts | `%ProgramData%\PersonalConsole\` | **Empty unless you turn sharing on.** See below. |

**Sharing with other accounts is off until you turn it on.** Preferences → System → *Share Settings
With Other Accounts* copies the categories you tick — profiles, appearance, radial menus, keyboard —
into `%ProgramData%\PersonalConsole\`, which is the one folder on this computer every account can
read. Three things about it are worth stating plainly, because they are the difference between a
convenience and a surprise:

- **Nothing goes there until you ask.** With the switch off, that folder is never created and the
  application behaves exactly as it did before this feature existed.
- **Anyone who signs in to this computer can read what you put there.** That is the entire point of
  the folder, and it is why saved sign-ins can never be shared — `credentials.dat` stays in your own
  account and is not one of the categories.
- **Turning it back off returns to your own copy**, which was left in place rather than moved.
  Nothing is deleted in either direction.

**How you check it.** Open those folders. Everything except the credentials file is human-readable —
open the XML in Notepad and read your own settings. That table is the whole list. The one thing that
can move it: if Windows cannot tell the application where your Documents folder is, everything except
the credentials file falls back to `%LOCALAPPDATA%\PersonalConsole` instead. Nothing is written
anywhere else.

---

## Saved passwords

**What it does.** The password manager stores sign-in details using **DPAPI**, the encryption service
built into Windows, tied to **your Windows user account on that machine**.

**What that means in practice.** The file cannot be decrypted by another user account, and it cannot
be decrypted on another computer — not even by you. Copying it somewhere else produces an unreadable
blob. This is why that one file is kept out of `Documents`: that folder is often synced to OneDrive,
and a password blob has no business being uploaded anywhere.

**The logs never contain a password.** The application writes diagnostic logs, and they record only
the profile name, whether an entry exists, and **how many characters** it had — never the value.

**How you check it.** Open `%LOCALAPPDATA%\PersonalConsole\credentials.dat` in Notepad: unreadable.
Then open any log file in `Documents\PersonalConsole\Logs\` and search for your password: it is not
there. Search for `chars` instead and you will see the character count that gets logged in its place.

---

## Starting with Windows

**Off by default.** If you turn on *Launch on Startup* in Preferences, the application creates a
**Scheduled Task**. It does not write to the registry Run keys.

**How you check it.** Task Scheduler (`taskschd.msc`) → Task Scheduler Library → look for the
PersonalConsole task. Turning the setting off deletes it; check it disappears.

**The honest limit.** **Uninstalling does not remove that task.** If you enabled startup and then
uninstall, a task pointing at an executable that no longer exists is left behind. It cannot start
anything, but it is clutter. To remove it: turn *Launch on Startup* off **before** uninstalling, or
delete the task in Task Scheduler afterwards.

---

## The system helper service

**Setup installs a second program and registers it as a Windows service: `PersonalConsoleSys`,
shown as *PersonalConsole SYSTEM Helper*.** This is the one part of the installation that runs
outside your own account, so it gets its own section.

**What it is for.** A few tray icons are drawn by programs Windows runs as the system account —
Apollo and the NVIDIA settings icon are the common ones. Windows does not let an ordinary program,
even one running as administrator, open those icons' menus. The helper is allowed to, because it
runs at the same level they do, and it exists only to read such a menu and carry out the entry you
choose.

**When it runs.** Never on its own. It is registered **demand start**, which means Windows does not
run it at boot or at login; the application starts it the first time you hold **A** on one of those
icons, and it stops again with the machine. If you never touch such an icon it never runs.

**What talks to it.** One named pipe that only the system account, Administrators and the user
signed in at the keyboard may open — and it additionally refuses a caller that is not running
elevated. It carries two requests: *read this icon's menu* and *choose this entry of it*.

**How you check it.** Open **Services** (`services.msc`) and look for *PersonalConsole SYSTEM
Helper*: its startup type reads **Manual** and its status is blank until you use the feature. On a
command line, `sc qc PersonalConsoleSys` prints the same thing, including the program it runs
(`PersonalConsoleSys.exe --service`, in the installation folder) and the account (`LocalSystem`).

**Removing it.** Uninstalling PersonalConsole stops and removes the service — that is part of the
package, not something you have to do by hand. You can check afterwards with `sc query
PersonalConsoleSys`, which should report that the service does not exist. If you would rather keep
the application without this ability, set *Enable System Tray Menus* to off in the Console Mode
settings; the service is then never started.

**The honest limit.** A service running as the system account is the most powerful thing this
installation puts on your machine. Its own code only reads and clicks tray menus, and this page
cannot prove that to you by itself — see *What this page cannot prove* at the end.

---

## Installing and removing

**Installing** copies the program into `Program Files` and adds Start-menu shortcuts. The program
never writes into its own installation folder afterwards — your settings go to Documents, which is
why a reinstall or an upgrade cannot wipe them.

**Two drivers come with it.** Setup installs **ViGEmBus** and **HidHide**, the drivers this
application needs to present a controller of its own and to hide your physical one from a game while
you type. Both are the drivers' own signed packages by Nefarius Software Solutions, included
unchanged, and both are kernel-mode: that is what a virtual controller requires. Earlier versions sent
you to their download pages instead; nothing else about what they do has changed.

**They are not removed when you uninstall PersonalConsole.** Other controller software on the machine
may be using them, so taking them away would break it. **Apps & Features** lists ViGEmBus and HidHide
separately and either can be removed there. Their licences ship with the application as
`THIRD-PARTY-NOTICES.txt`.

**How you check it.** Before installing, the two packages are inside the installer; after installing,
**Apps & Features** shows three entries — PersonalConsole, ViGEmBus Driver and HidHide — each with its
own publisher, and Windows shows Nefarius Software Solutions as the signer of the two drivers. The
build that produced this release verifies both downloads against a published hash and refuses to
package if either signature does not match.

**Removing it** uninstalls the program and leaves your settings folder alone, on purpose: uninstalling
to fix a problem should not also destroy your mappings. If you want them gone, delete
`Documents\PersonalConsole\` and `%LOCALAPPDATA%\PersonalConsole\` yourself.

**How you check it.** Install, note the folders, uninstall, look again. What remains is the settings
folder, the credentials file and — if you enabled it — the startup task.

**SmartScreen.** The installer is not code-signed, so Windows will warn you the first time. A signing
certificate is a recurring cost this project has not taken on. Verify the download instead — in
PowerShell, in the folder you downloaded it to:

```
Get-FileHash .\PersonalConsole-Setup-*.exe
```

Compare the result with the hash in `SHA256SUMS.txt`, published beside the download and repeated in
the release notes. Anyone who is uncomfortable with an unsigned installer is being reasonable, and the
portable ZIP is there if you prefer to unpack and inspect before running anything.

**Antivirus.** Some engines flag this kind of program. That is not an accusation to argue with: an
application that requests administrator rights, replaces the shell and synthesises keyboard and mouse
input matches the behavioural pattern of software that does those things for bad reasons. Scan it
yourself, and read the result knowing what it is looking at.

---

## What this page cannot prove

Being honest about the ceiling is the point of this section.

- You can observe **behaviour**. You cannot audit **intent**. Everything above is checkable from
  outside the program; none of it proves what the code was written to do.
- There is no way for you to confirm that the published installer was built from code that behaves as
  described. That is what source access would give you, and this project does not provide it.
- The checks above prove things about **the version you are running, while you are watching**. They
  are not a guarantee about a future version.
- This page is not a substitute for the source code. It is a list of the things an outsider *can*
  verify, offered because "trust us" is not an answer.

If you find a discrepancy between this page and what the program does, that is worth reporting — and
the report is more useful than the page.
