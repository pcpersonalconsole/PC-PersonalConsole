# PersonalConsole — User Guide

How to use PersonalConsole, screen by screen.

This guide answers **"what is this screen for, and how do I do X"**. It does not repeat what is
already written elsewhere:

- **Installing it, what it needs, where your files are kept, known limitations** → `RELEASE-README.md`
- **What changed in a version** → `PersonalConsole/docs/RELEASE-NOTES.md`
- **Why it needs administrator, and why you can trust the build** → `SECURITY.md`

Everything below describes the product as it is now — there are no version numbers here on purpose.
If something does not match what you see, the guide is wrong and should be fixed.

Screens referenced as `main/profiles.png` live in `PersonalConsole/screenshots/ui/`.

---

## 1. What this is

PersonalConsole turns a Windows PC into something you can drive from the couch with a game
controller. It does three things:

- **Translates your controller into keyboard and mouse input**, per application, so a program that
  never supported a pad can still be driven by one.
- **Replaces the Windows desktop** with a full-screen, controller-friendly one.
- **Gives you a Virtual Keyboard, radial menus and a file browser** you can use without a mouse.

---

## 2. The main window

The left sidebar is the whole app. Five sections:

| Section | What it is for |
|---|---|
| **Profiles & Templates** | Which controller setup applies to which program |
| **Controller Mapping** | What each button does |
| **Virtual Keyboard** | The Virtual Keyboard's look, languages, dictionaries and saved sign-ins |
| **Console Mode** | The full-screen desktop, its tabs, and the built-in file browser |
| **Preferences** | Startup, controller detection, appearance, themes, updates |

At the bottom: **Minimize** and **Exit**.

**Minimize** puts the window away and leaves the app running — the controller keeps working. Where it
goes depends on one setting:

| **System Tray Behavior** (Preferences → System) | What Minimize does |
|---|---|
| **Off** (default) | An ordinary minimised window on the taskbar |
| **On** | The window is hidden entirely and only the tray icon is left |

Either way the tray icon brings it back.

The top-left corner shows the profile you are editing: its icon and its name (a long name wraps onto
up to four lines; anything longer ends in "…").

**Getting around with the controller:** `LB` / `RB` change section — a thin rail beside the section
list runs from `LB` at the top to `RB` at the bottom, and a lit mark on it sits beside the section you
are in. `LT` / `RT` change the tab inside a section, and they are drawn at the two ends of the tab strip
itself. Both sets of marks appear only while you are using the controller. Opening **Profiles &
Templates** puts you straight on the profile you are editing in the list. Mouse and keyboard work
everywhere too.

---

## 3. Profiles

A **profile** is a full set of button mappings plus the program it belongs to. PersonalConsole
switches profiles by itself: when a program you have a profile for comes to the foreground, its
mappings take over. If a program has no profile, the **Desktop** profile applies.

Every row in the list carries a round icon on its left. **Desktop** and **Virtual Keyboard** have
marks of their own and sit above a divider, so the two built-in profiles read as a group. Every other
profile shows the icon of the program it is for, taken from the first program listed for it.
PersonalConsole looks the program up where Windows records it and among your shortcuts, so it does
not have to be running; a program it cannot place shows a controller mark until it is seen once.

You can also choose the icon yourself. In **Profile Options → Profile Icon**, **Choose Icon** takes
any program, `.ico` or picture and uses it for that profile — useful when two programs share an
executable name, or when you simply prefer a different mark. **Use Automatic** puts it back. Your
choice applies to the built-in profiles too.

📸 `main/profiles.png`

### The two built-in profiles

Two profiles always exist and are pinned to the top of the list. They are not examples you can
replace — the app relies on them, which is why **neither can be renamed or deleted**.

#### Desktop

**The fallback.** It applies whenever nothing more specific does — you are on the Windows desktop, or
in a program that has no profile of its own. This is the profile to set up first, because it is the
one you will spend most of your time in: it is where you want the sticks driving the mouse, a button
opening the Virtual Keyboard, and so on.

It is also the profile the app **forces** in three situations:

- **Emergency Desktop Mode** — the combination from that tab drops you back here from anywhere, which
  is the whole point of it (section 3, *Emergency Desktop Mode*).
- **While a file picker or other system dialog opened by PersonalConsole is on screen** — that dialog
  belongs to the app rather than to your game, so no game profile could sensibly apply. Desktop keeps
  the pad driving the mouse and buttons inside it.
- **When a program's name cannot be read** — an elevated process, for example. Rather than guess, the
  app falls back here and notes it in the log.

#### Virtual Keyboard

**Active whenever a Virtual Keyboard is open, whatever is in the foreground.** It outranks every other
profile, including the one for the game you are playing.

That is deliberate: while you are typing, the pad has to drive the *keyboard*, not the game
underneath. Without it, the same stick would move a cursor and a character at once. The profile is
what makes the keys, the prediction row and the close button reachable.

Map here anything you want available **while typing** — closing the keyboard, switching language,
moving the caret. Bindings you put in a game's own profile are not in effect during that time.

📌 So the order of precedence is: **Virtual Keyboard open → its profile · a system dialog or the
emergency combo → Desktop · otherwise → the profile matching the foreground program · nothing
matches → Desktop.**

### Creating one

1. Open **Profiles & Templates** → **Profile**.
2. Press **+ Add Profile** under the list.
3. Set **Target Executable (.exe)** — **Browse .exe** to pick the program, or **Recent Apps** to
   choose one you have used recently.
4. Optionally set **Controller** if the profile should only apply to one specific pad. The default,
   *Any controller*, applies to whichever pad is connected. 📸 `main/profiles__devices-expanded.png`
   ⚠️ A profile tied to one pad **does not run at all** while that pad is unplugged — no bindings, no
   Realtime keyboard. The row says so when that happens.
   📸 `main/profiles__device-pin-unsatisfied.png`
   🔑 Switching a pad between its **XInput and DInput modes changes the identity Windows reports**, so a
   profile tied to it in one mode will not match in the other. *Any controller* avoids this entirely.

The line under the box — *"Activates automatically while the target window is in the foreground"* —
is the whole rule.

### Editing and deleting

Pick a profile's card: the **Profile Options** panel beside the list follows it, with **Profile Name**,
**Rename Profile** and **Delete Profile**. 📸 `main/profiles__popup-profileoptions.png` (older layout)

⚠️ Built-in profiles (Desktop, Virtual Keyboard) cannot be renamed or deleted. The panel says so.
⚠️ Every profile needs its own name. **Rename Profile** refuses the name of another profile (capital
letters make no difference) and the names **Desktop** and **Virtual Keyboard**, and says why.
**+ Add Profile** picks a free name for you — *New Profile*, *New Profile 2*, and so on.
⚠️ Deleting a profile is permanent, and it is a **hold** button — keep it pressed until the bar
fills. Letting go early cancels, which is the point.

### Emergency Desktop Mode

📸 `main/profiles-emergency.png`

If a game's mapping ever leaves you stuck, this combination force-switches back to the Desktop
profile system-wide. Two dropdowns: the **combination** itself and how long it must be **held**.
Set them to something no game will use by accident.

### Templates

📸 `main/profiles-templates.png`

**Export Template** writes your mappings to a file; **Import Template** loads one. Five toggles pick
what is taken from it — **Target EXEs**, **Button layout**, **Radial menus**, **Combinations** and
**Analog settings** — so you can move only the parts you want to another PC.

Each template is one file named after it. A name that would end up in the same file as an existing
template — one that differs only in capital letters, or only in a character a file name cannot hold,
such as `/` — is refused, and so is a name that cannot be a file name at all (very long, or a name
Windows reserves such as `CON`). Exporting under a template's **exact** name updates that template.

What each toggle does to the profile you are editing:

| Toggle | Effect |
|---|---|
| **Target EXEs** | **Added** to the profile's list; programs already there are kept |
| **Button layout** | ⚠️ **Replaces** every button binding in the profile with the template's — bindings the template does not have are removed |
| **Radial menus** | **Added**; a menu you already have is skipped, and one with the same name comes in under a new name |
| **Combinations** | **Added**; a combination on the same buttons as one you already have is skipped, so yours is kept |
| **Analog settings** | **Replaces** the stick and D-Pad setup — a stick is either Mouse or WASD, so there is nothing to merge |

**Analog settings** covers what each stick and the D-Pad do (Mouse, WASD, Scroll, Arrows) together
with their sensitivity, acceleration and acceleration time, and the deadzone.

⚠️ An import cannot be undone. To keep a profile's own buttons, leave **Button layout** off.

A toggle comes up greyed when the selected template carries nothing of that kind. Templates exported
before a part existed simply have none of it — that is why **Analog settings** is unavailable on an
older template, and why importing one cannot overwrite the analog settings you have now.

A new installation arrives with two templates already here — **Desktop** and **Virtual Keyboard**,
copies of the two built-in profiles — so there is a working arrangement to start from. They are placed
only when there are no templates of your own, so an update never replaces yours.

When OneDrive keeps an extra copy of a template because two computers changed it at the same time
(`Desktop-OTHERPC.json` beside `Desktop.json`), PersonalConsole moves that copy into a **Conflicts**
folder inside the Templates folder, so each template is listed once. Nothing is deleted.

More templates are shared at **<https://www.pcpersonalconsole.com/communityfiles>**. Download a `.json`
from there and load it with **Import Template**.

---

## 4. Mapping a button

Open **Controller Mapping**. The tab strip selects which part of the pad you are working on:
**Device Layout**, **Actions**, **Menu**, **D-Pad**, **Bumpers & Triggers**, **Left Stick**,
**Right Stick**, **Combos**.

**Device Layout** is a picture of your controller with every binding drawn next to the button it
belongs to — the fastest way to see what is already assigned. Assigned buttons stay highlighted.
📸 `main/mapping-device-layout.png`

### The steps

1. Select the button — on **Device Layout**, or from one of the other tabs.
2. The editor opens full screen. Its top-left corner names exactly what you are editing: the
   button's own glyph, the **profile**, and the **layout**. 📸 `mapping/single-tap__fullscreen.png`
3. Pick the slot on the left — **Single Tap**, **Double Tap**, **Hold**, **Release**. One button can
   carry a different action in each of the four.
4. Pick a category, then the action inside it.
5. Press **B** (*Apply & Close*), or `Esc`.

### The categories

| Category | What you get | Screen |
|---|---|---|
| **Keyboard Key** | A full QWERTY picker, plus `Win` / `Alt` modifier keys | `mapping/single-tap__keyboard-key.png` |
| **Mouse Control** | Clicks, scroll and movement | `mapping/single-tap__mouse-control.png` |
| **Virtual Keyboard** | Open the **Realtime** or **Buffer** keyboard, or **Close Keyboard** | `mapping/single-tap__virtual-keyboard.png` |
| **Radial Menu** | Open one of your menus | `mapping/single-tap__radial-menu.png` |
| **Layouts** | Switch to another layout, with **Hold** / **Toggle** / **One-Way** | `mapping/single-tap__layouts.png` |
| **Windows Actions** | **Close App (Alt+F4)**, **Show Desktop (Win+D)**, **Show Console Desktop**, **Task Manager**, **Window Switcher**, plus **Shell** and **Media** groups | `mapping/single-tap__windows-actions.png` |
| **Open File** | Start a program, open a file, or open a folder — whichever you pick | `mapping/hold__open-file.png` |

One slot can hold more than one thing at once — a keyboard key and a mouse click assigned to the
same slot are sent together on the same press.

### Timing the keys on one button

Press a key you have already assigned and a small panel opens for it, with a **note grid** at the
top. Every key on that button gets its own lane, time runs left to right, and each bar shows when
that key goes down and how long it stays down.

Bars that all start at the left go out **at the same moment** — that is a chord like `Ctrl+C`, and
it is what a button does until you change something. Slide a bar to the right and that key now
happens **later**. There is no mode to pick: the bars are the answer.

Because a bar has a length as well as a position, one key can stay down **while** others happen
inside it — hold `Shift` for half a second and tap `A` twice in the middle of it, for example.

- **With a controller:** ◀ ▶ moves the selected bar earlier or later, ▲ ▼ picks a bar, and **A**
  switches ◀ ▶ over to changing the bar's length instead. Pressing ▼ on the last bar leaves the grid.
- **With a mouse:** drag a bar to move it, and use the scroll wheel over a lane to make that bar
  shorter or longer.

**−500 ms / −100 ms / +100 ms / +500 ms** under the grid change how much time it shows — they never
move a key. Widening it is how you make room to push something further out, and because a bar's
width is a real duration, the same key simply draws narrower as the window grows. The ruler along
the top is in milliseconds, so what you see is what will be sent.

The rest of that panel is per key: **Sync with Gamepad** holds the key for exactly as long as you
hold the button, **Fixed Duration** sends it for a set time however briefly you press, and **Remove
Assignment** takes that one key off the button — its lane goes with it.

### Opening a program or file from a button

📸 `mapping/hold__open-file.png`

**Open File** asks you to choose, and then that button opens it. There are three ways to pick:
**Choose a program…** (executables and shortcuts), **Choose any file…** (a document, a picture, a
save file — it opens in whatever program normally handles it), and **Choose a folder…**. **Remove**
takes the assignment off again.

All three open in this app's own File Explorer, which you can drive with the controller, as long as
*Choose Files With The Built-in File Explorer* is on in **Settings → Console Mode → File Explorer**.
Turn it off and they use the Windows dialogs instead. (Its neighbour, *Open Folders In The Built-in
File Explorer*, is a different question: that one decides where a folder **opens** once you already
have it — including from the Console Desktop.) When you are choosing a **file**, **A** picks the one you are on and
anything the button cannot use is drawn faint. When you are choosing a **folder**, **A** still opens
folders so you can find the right one, and **X** chooses whichever folder you are looking at.

The panel shows the full path so you can tell two identically named programs apart, and it tells you
when what you picked is not on the machine right now — an unplugged drive, for example. That is a
notice, not a reset: the assignment is kept, so it works again as soon as the path is back.

🔑 It opens **once per press**. Holding the button down does not open it over and over, and turbo
does not apply to it.

### Slot-specific controls

- **Double Tap** adds a *Double Tap Speed* slider — how fast the second press must come.
  ⚠️ A double tap also sends the **Single Tap** action: the first press has already gone out by the
  time the second one arrives. Leave Single Tap empty if you want the double tap on its own.
  📸 `mapping/double-tap.png`
- **Hold** adds a *Hold Threshold* slider — how long the button must be held.
  📸 `mapping/hold.png`
- **Release** is the plainest slot: it fires when you let go. 📸 `mapping/release.png`

### Clearing

The **Clear** button at the bottom names the slot it will clear — *Clear Single Tap* — so it can
only affect the slot you are looking at.

### Turbo and rumble

On the editor's **Button** tab:
- **Enable Turbo (Repeat)** auto-repeats while held. **Repeat Delay** sets the rate and
  **Enable Acceleration** speeds it up the longer you hold.
- **Enable Rumble** vibrates when the button fires: **Single**, **Ascending** or **Descending**.
  ⚠️ It needs a controller running in its **XInput** mode. A pad read directly (DirectInput) cannot be
  driven at all — Windows reports no motors for it — and **Preferences → Controller** says so while
  such a pad is in use.

### Sticks and the D-Pad

Under the Device Layout picture, three dropdowns set what the sticks and D-Pad do as a whole:
**Mouse**, **Scroll** or **None**. The gear button beside each opens its detailed settings
(sensitivity, deadzone, acceleration).

⚠️ When a stick or the D-Pad is set to anything other than *None*, its four directions are taken
over by that behaviour and show as **locked** in Device Layout — you cannot also bind them
individually. Set it back to *None* and the bindings you had are still there. The stick **click**
(L3 / R3) is not a direction and stays bindable either way.

### Combos

📸 `main/mapping-combos.png` — bind an action to two buttons pressed together. The tab starts empty
until you add one.

---

## 5. Layouts

Each profile has **eight layouts**, numbered 1–8 on the strip at the top of Controller Mapping. A
layout is a complete set of bindings, so one profile can hold eight control schemes.

You switch layouts at run time by binding the **Layouts** action to a button. It works in three ways:

| Mode | Behaviour |
|---|---|
| **Hold** | The layout applies only while the button is held |
| **Toggle** | Press to switch, press again to switch back |
| **One-Way** | Switch and stay |

The editor always names the layout you are editing in its top-left corner. This matters more than it
sounds: **a binding placed on layout 3 does nothing while layout 1 is active**, and that is the most
common reason a new mapping "does not work".

---

## 6. Radial menus

A radial menu is a wheel of actions you open with one button and pick from with the stick.

🔑 **Menus are shared across profiles.** The menu itself belongs to you, not to one program. What
belongs to a profile is the **binding** — which button opens which menu. So a menu you build once is
available everywhere, and you can open it from a different button in each game.

Open the editor with **Open Radial Menu Editor** at the bottom of Controller Mapping.
📸 `radial/empty.png` (nothing selected) · `radial/menu-selected__fullscreen.png` (a menu open)

### Building one

1. Press **+ Add Menu**. It appears in the list on the left with its item count under the name.
2. Set **Menu Name** at the top.
3. Press **+ Add Button** for each entry. Every entry has:
   - **Button Name** — what it is called
   - **Colour** and **Symbol** — how it looks on the wheel
   - **Show as** — symbol only, or symbol and text
   - **Assign Action** — what it does when picked
   - **✕** — remove the entry
4. Use the ▲ / ▼ arrows on a row to change the order entries appear on the wheel.
5. Press **B** (*Apply & Close*).

**Menu Settings ▼** expands in place for the menu's own options.
📸 `radial/menu-selected__settings-expanded.png`

⚠️ **Delete Menu** removes the whole menu and cannot be undone.

### Opening one

Map a button to the **Radial Menu** category (section 4) and pick the menu. A menu entry can itself
open another menu, so menus can nest. The submenu follows its own activation setting (in **Menu
Settings ▼**). With *Gesture Select*, letting go of the button opens the submenu and it waits: hold the
button again, point and let go to pick, or press **A**. **B** closes it.

`LB` / `RB` move between menus while the editor is open.

---

## 7. The Virtual Keyboard

There are exactly **two** Virtual Keyboards, and the difference is where your typing goes:

| | **Realtime Keyboard** | **Buffer Keyboard** |
|---|---|---|
| Keys go | straight to whatever is focused, as you press them | into the keyboard's own text box first |
| The program underneath | keeps running | is held still while you type |
| Text arrives | immediately | when you close the keyboard |
| Use it for | desktop apps, browsers, chat | games that would react to the pad while you type |

You open either one by mapping it to a button (section 4, **Virtual Keyboard** category). The same
category has **Close Keyboard**, so you can bind a dedicated close button.

**Where it opens.** The keyboard takes the top or the bottom of the screen, whichever leaves the place
you are typing visible. It asks the program underneath where its text box is; when the program does not
say — many games draw their own text fields and have nothing to report — it uses where your pointer was
when you asked for the keyboard.

**To decide it yourself**, map the **Change Keyboard Position** action to a button (section 4,
**Virtual Keyboard** category). Each press moves the keyboard to the other half of the screen and
remembers it; pressing it while the keyboard is closed chooses the half the next one opens in. Use it
in a game that draws its own interface: there is nothing there for the automatic placement to read,
and you already know which half your chat box is in.

### The prediction row — four buttons

📸 `mapping/single-tap__virtual-keyboard.png`

Above the keys sit **four** buttons: **three suggestions and one add-to-dictionary slot.**

| Button | What it holds |
|---|---|
| **1st (left)** | Second-best suggestion |
| **2nd (middle)** | 🔑 **The best suggestion** |
| **3rd (right)** | Third-best suggestion |
| **4th** | `+ "word"` — adds what you typed to your dictionary |

🔑 **The best match is the MIDDLE button, not the first.** The three suggestion slots fill
**centre-out** — best to the middle, then left, then right — the way phone keyboards do it, because
the middle is where your thumb already is.

The 4th button is separate on purpose: adding an unknown word used to consume the third suggestion,
so you had to choose between a suggestion and teaching the keyboard. Now you get both. It appears
**only** when what you typed is not already a known word, and it shows the word in quotes so you can
see exactly what will be stored.

**Suggestions keep your capitalisation.** Type `Wed` and pick the suggestion and you get `Wednesday`,
not `wednesday`. Only the part you actually typed is re-cased; the rest stays exactly as the
dictionary has it, so words with capitals inside them — `iPhone`, `eBay` — survive being completed.
Nothing is blanket-uppercased, which is also what keeps alphabets with dotted and dotless letters
correct.

When the line is empty all three suggestion slots offer likely openers, drawn from what you type most
often after nothing.

### Next-word memory

The keyboard also learns **which word you tend to type after which**. Once you have typed
`hello everyone` a few times, typing `hello` pushes `everyone` up the list. This sharpens the
ordering; it never takes a slot away from ordinary matches.

⚠️ **Password fields:** while you type into a field the app knows is a secret, prediction and word
learning are switched **off** for that session, and nothing you type is written to the dictionary or
to the log. The Realtime keyboard has no mask, so the protection is based on the field being a
secret, not on whether dots are drawn.

### Where the files live

Both folders are reachable from **Virtual Keyboard → Language**:

| Button | Folder | Holds |
|---|---|---|
| **Open Language Folder** | `Documents\PersonalConsole\Shared\VirtualKeyboardLanguages` | One `.json` per keyboard layout |
| **Open Dictionary Folder** | `Documents\PersonalConsole\Shared\VirtualKeyboardDictionaries` | One `.json` per language's words |

They open in the built-in file browser (section 9). Both are in the **Shared** folder, so they travel
with the rest of your settings rather than being tied to one machine. *Preferences → System → Share
Settings With Other Accounts* (section 12) moves them instead to a folder every Windows account on
this computer reads.

### Creating a language file

A language file describes **what each key prints**. It is a plain JSON file:

```json
{
  "LanguageCode": "DE",
  "Base":  ["^", "1", "2", "…", "z", "x", "c", "v", "b", "n", "m", ",", ".", "-"],
  "Shift": ["°", "!", "\"", "…", "Z", "X", "C", "V", "B", "N", "M", ";", ":", "_"],
  "Caps":  ["^", "1", "2", "…", "Z", "X", "C", "V", "B", "N", "M", ",", ".", "-"],
  "Sym":   ["`", "1", "2", "…", "z", "x", "c", "v", "b", "n", "m", ",", ".", "/"]
}
```

**The rules:**

1. **`LanguageCode` decides the code, not the file name.** The keyboard shows this value, uppercased.
   Two files with the same `LanguageCode` will collide — one silently replaces the other.
2. **Each layer is 47 entries, in this fixed order** — the standard QWERTY block, left to right, top
   to bottom:

   | Row | Count | Positions |
   |---|---|---|
   | 1 | 13 | `` ` `` `1` `2` `3` `4` `5` `6` `7` `8` `9` `0` `-` `=` |
   | 2 | 13 | `q` `w` `e` `r` `t` `y` `u` `i` `o` `p` `[` `]` `\` |
   | 3 | 11 | `a` `s` `d` `f` `g` `h` `j` `k` `l` `;` `'` |
   | 4 | 10 | `z` `x` `c` `v` `b` `n` `m` `,` `.` `/` |

   Position 1 is the top-left key and position 47 is the bottom-right one. You are replacing **what
   is printed**, not moving keys around — so to build an AZERTY layout you put `a` where `q` is.
3. **Only `Base` is required.** `Shift`, `Caps` and `Sym` are optional, and any layer you leave out
   falls back to `Base`. That makes a minimal language file quite short.
4. **Layers are what the modifier keys show:** `Shift` while Shift is held, `Caps` while Caps Lock is
   on, `Sym` when you press **SYM** (the same key then reads **ABC** to go back).
5. Save the file as UTF-8 so accented and non-Latin characters survive.

**To install it:** drop the `.json` into the language folder and **restart PersonalConsole**. The
folder is read once at startup. Your new code then appears in the **Available** column on
**Virtual Keyboard → Language**; switch it on to add it to the cycle.

⚠️ **A broken file is skipped, not fatal.** If the JSON does not parse, that one language is ignored
and the others still load — so a typo makes a language *disappear* rather than break the keyboard.
If a language you added is missing, that is the first thing to suspect; the log records the file name
and the parse error.

### Creating or editing a dictionary

A dictionary file holds the words for one language, named with the same code — `EN.json`, `TR.json`:

```json
{
  "Version": 1,
  "WordsVersion": 2,
  "Seeded": false,
  "Words":     ["the", "be", "to", "of", "and"],
  "UserWords": ["gamepad", "roguelike"],
  "Pairs":     { "hello": { "everyone": 3 }, "project": { "diablo": 2 } }
}
```

| Field | What it is |
|---|---|
| **`Words`** | The shipped word list for that language |
| **`UserWords`** | Words **you** added with the 4th prediction button |
| **`Pairs`** | Next-word memory: `previous → { next: count }`. The count is how often you did it, and a higher count ranks that word sooner |
| **`Version` / `WordsVersion` / `Seeded`** | Bookkeeping the app uses to decide whether to merge in newer shipped words. Leave them alone |

**To add words in bulk,** open the file and extend **`UserWords`** — that is the list meant for you.
Adding to `Words` works too, but a future update may refresh that list.

**To remove a suggestion you never want,** delete it from `UserWords` (or from `Pairs`). Removals
stick; the app does not put them back.

**To seed next-word behaviour by hand,** add entries to `Pairs`. `"tea": { "kettle": 5 }` makes
`kettle` outrank other matches right after you type `tea`.

⚠️ **Close the keyboard before editing a dictionary by hand.** It writes learned words back as you
type, and a save from the app will overwrite what you edited underneath it.

📌 The dictionary folder is also where the keyboard's learning ends up, so backing up that folder
backs up everything the keyboard has learned about how you write.

### Languages

📸 `main/vk-language.png`

**Virtual Keyboard → Language** has two columns: **Available** on the left, **Enabled — cycled in
this order** on the right. Turning a language on moves it to the right column; turning it off sends
it back.

The numbers on the right are the **cycle order** — the sequence the keyboard steps through when you
switch languages — and the first one is what it opens with.

⚠️ At least one language must stay enabled; the app will not let you switch them all off.

**Open Language Folder** and **Open Dictionary Folder** open those folders in the built-in file
browser, so you can add your own layouts or edit the learned-word lists.

### Appearance

📸 `main/vk-keyboard.png`

- **Size** makes the whole keyboard larger or smaller, from 50% to 200%. It is capped at whatever still
  fits the screen, so a setting that would push the edges out of view is drawn a little smaller than asked
  — the number you chose is kept.
- **Corner Roundness** and **Background Opacity** style the keyboard's panel.
- **Typing Input Lock** holds the foreground while the Realtime keyboard is open, so the program
  underneath stops reacting to the pad. It stands aside only when the stand-in controller is genuinely
  doing the job for it — that is, when DirectInput Compatibility is on **and** the game has no
  controller of its own to read, which is the case with the pad in DInput mode. With the pad in XInput
  mode the game reads it directly, the stand-in shields nothing, and this setting stays in charge.
  ⚠️ **It is not a guarantee once you are inside a game.** Holding the foreground only stops a program
  that checks whether it is the active window. Some games poll the controller regardless of focus, and
  those keep reacting to the pad no matter what — measured in Project Diablo 2, where the main menu
  goes quiet and the game itself does not. For typing inside such a game, use the **Buffer keyboard**:
  it is the one that holds the game still, which is the only thing that always works.
  🔇 **A game that mutes itself in the background goes quiet while the keyboard is open.** The lock
  works by putting the game in the background, so a game's own *mute in background* option switches
  its sound off for that time (Path of Exile 2 has it on by default). The game decides this itself,
  so it cannot be overridden from outside. Two ways keep the sound: turn that option off in the game's
  audio settings, or use the pad in its **DInput mode**, where the lock stands aside and the game stays
  in front. Path of Exile 2 also plays again as soon as its chat is opened from the keyboard.
- **Suggest From All Enabled Languages** widens where suggestions come from. Off, the keyboard offers
  words from the layout you are on. On, it offers words from **every language you enabled**, whichever
  layout is showing — so typing English on the Turkish layout still gets suggestions, and someone who
  mixes languages does not have to teach the same word twice.
  🔑 **What it does not change is where words are saved.** A new word is always learned into the
  language of the layout you were typing on, so your dictionaries stay separate; the setting only
  decides which of them are *read* when the three suggestion slots are filled.

### Saved sign-ins

📸 `main/vk-password.png`

**Virtual Keyboard → Password** is a small password manager for signing in from the couch. Entries
are encrypted and **stay on this machine** — they are deliberately not synced anywhere.

---

## 8. Console Desktop

Console Mode replaces the Windows desktop with a full-screen one built for a controller.

📸 `desktop/tab-all-apps__after-show-desktop.png` · `desktop/tab-desktop.png` · the other `desktop/` frames
in the catalog

### Turning it on

📸 `main/console-shell.png`

**Console Mode → Shell**:

| Setting | What it does |
|---|---|
| **Enable Console Mode Overlay** | Replaces the Windows desktop with the gamepad-driven one |
| **Auto Hide/Show on Input** | Hides it the moment you use the mouse or keyboard, brings it back on gamepad input |
| **Icon Size** | How large the shortcut tiles are |
| **Background Opacity** | How much shows through |
| **Show Shortcut Names** | Item names beneath their icons |
| **Show Hidden Items** | Include items marked hidden |
| **24-Hour Clock** | Off shows AM and PM instead |
| **Show Date With The Clock** | Puts the date above the time in the top bar |

🔑 **Auto Hide/Show is what lets you share the PC with a normal desktop session** — touch the mouse
and the console desktop steps aside; pick the pad back up and it returns.

🔑 **It never steals focus from a window you are using.** It comes forward when everything else is
closed or minimised, not while you are working in another program. That is a deliberate rule: an
earlier version did activate itself aggressively and made ordinary windows unusable.

🎮 **To call it forward on purpose, map the *Show Console Desktop* action** (Mapping window →
Windows Actions) to a button. That is the way back from a game running in windowed fullscreen:
**Win+D** raises the *Windows* desktop, and while Console Mode is on, the desktop you are looking at
is this one — so Windows shows you an empty screen instead. A bound button has no such confusion.
Pressing it minimises every open window — the game, other programs, and this app's own windows — and
the console desktop takes over the screen. A game running in exclusive fullscreen may not minimise.

### What it shows

Your own desktop shortcuts **plus** the ones installed for all users, so programs installed for
everybody appear for everybody. Each Windows account keeps its own arrangement.

**RT / LT** switch between the general view and the open-applications view, remembering where you
were in each.

**Holding Y** on an open application closes it. That is the same request its own ✕ makes, so the
program still gets to ask you about unsaved work; the tile disappears once it really closes, and one
that stays means the program is asking you something. On a shortcut in the general view the same hold
removes the shortcut instead — which is why the hint on the bar reads **Delete / Close**.

The screen is stacked top to bottom — top bar, tabs, your shortcuts, then the open applications —
and **the d-pad walks straight through it**. Down from the top bar lands on your shortcuts, down
again reaches the open applications, and up retraces the same path. Nothing in the middle is skipped,
so RT / LT are a shortcut between the two lists rather than the only way across.

The **clock** in the top bar opens a **calendar** for the month, with today marked. **◀** and **▶**
move between months and **Today** comes back to this one; **B** closes it. How the clock itself
looks — 12 or 24 hours, and whether the date is shown — is set in **Console Mode → Shell**.

The **connection** icon in the top bar shows how this computer reaches the internet: a network-cable
socket when it comes over a cable, the Wi-Fi mark when it comes over Wi-Fi, where more lit arcs mean a
stronger signal (a dot alone is the weakest). When both are connected, the one carrying the internet is
shown. Grey means no connection. Either way it opens the Wi-Fi panel.

Beside the connection and Bluetooth icons, the **speaker** icon opens the **sound** panel. The first row
mutes or unmutes; **−** and **+** lower and raise the volume in 5% steps, and **+** also unmutes. Below
them are the playback devices this computer has — speakers, headphones, a TV — with a **✔** on the one
in use; choosing another switches the sound to it straight away. **B** closes the panel. The icon in
the top bar shows a crossed-out speaker while the sound is muted.

The **system tray** opens from the small **⌄** arrow at the left of the top bar's right-hand icons. It is drawn at a quarter of the icon size and lists the icons Windows is actually
showing - both the ones sitting on the taskbar and the ones behind its chevron. Hovering an icon for
two seconds shows its name; there is no name list underneath and no scrollbar, by design. **A**
activates an icon.

Right after you restart the computer, Windows has not yet built the panel that holds its hidden
icons, and nothing can read them until it has. Until you open a tray menu once, the list is shown
unfiltered and can include programs that are not really in the tray.

PersonalConsole's own icon is not shown here - open its window or quit it from the app itself.

**Holding A** opens that program tray menu - the one a right click gives on the Windows tray - as a
small menu just below the icon, carrying that program own entries. While the menu is being read the
icon spins a small ring on itself. The d-pad moves between them,
**A** carries one out and **B** closes it, leaving the tray panel open. Nothing from Windows appears
while this happens: no taskbar, no hidden-icons flyout and no Windows menu, and the Console Desktop
keeps the controller throughout.

Some programs draw their menu themselves (Epic Games Launcher, Riot Vanguard). Their entries are read
from the menu's picture, so a name can occasionally be misspelled, and choosing one may show the mouse
pointer for a moment where it already is.

Icons belonging to programs Windows runs as the system account — Apollo and the NVIDIA settings icon
are the usual ones — open the same way. A helper installed with the application does the reading for
them; it starts when you hold **A** on such an icon and does nothing the rest of the time. If it
cannot start on your machine, holding **A** says the menu cannot be opened. The switch for it is
*Enable System Tray Menus* in **Console Mode → Shell**.

An entry marked with a **›** opens a further menu. Choose it to go into that submenu; **B** or the
**‹ Back** row at the top returns to the level above.

A few programs answer a right click in their own way or with nothing at all; those open no menu here
either. Some programs are ones Windows will not let PersonalConsole reach at all — Apollo, the NVIDIA
settings icon and Remote Mouse are three; holding **A** on those says *"This program's menu can't be
opened here."*

An icon Windows shows that belongs to no program it can name - "Bluetooth devices" is one - is not
listed here, because there would be nothing to draw and nothing to open. PersonalConsole's own icon is
not listed either.

### System → Tasklist

**System** in the top bar opens the machine's details, the installed applications, and the
**Tasklist**.

The Tasklist shows one row per running program — not one per process — with its icon, what it is
costing in memory and CPU, and how many processes it is running. Programs showing a window are listed
first, then the background ones. **Name**, **CPU** and **Memory** re-order the list within those two
groups; pressing the one already selected reverses the direction. The order is then held still while
the figures refresh, so a row does not move out from under the pad.

**X** ends the program under the cursor. The confirmation names how many processes that is, and
ending it closes all of them; anything unsaved in the program is lost. **B** cancels, and Cancel is
where the cursor starts.

⚠️ **The power menu needs a three-second hold**, with the vibration ramping up as it fills, so it
cannot be triggered by accident.

### Tabs

📸 `main/console-tabs.png` · `main/console-tabs__tab-selected.png`

**Console Mode → Tabs** organises the desktop into tabs. Select a tab to enable **Move Up** and
**Move Down** and reorder it. Items are assigned to tabs from the desktop itself, and the layout is
saved as you go. The first tab is the one the desktop opens on, so it stays first: it cannot be moved,
and no other tab can move above it. That is why both buttons are disabled on the first tab, Move Up is
disabled on the second, and Move Down is disabled on the last.

### The built-in browser setting

📸 `main/console-explorer.png`

**Console Mode → File Explorer** decides when PersonalConsole's own browser is used. There are two
switches because there are two different moments, and they can be set separately:

- **Open Folders In The Built-in File Explorer** — every button that *opens* a folder lands here,
  including the Console Desktop's File Explorer button and any folder shortcut on it. Off means
  Windows Explorer.
- **Choose Files With The Built-in File Explorer** — every button that asks you to *choose* a file
  or folder picks here. Off means the ordinary Windows dialog.
- **Show Hidden and System Files** — off, the browser also leaves out what Windows keeps at the top of
  a drive even though it is not marked hidden: the folders (`Windows.old`, `ESD`, `PerfLogs`, anything
  starting with `$` or `.`) and, at the top of the drive Windows is installed on, loose files such as
  setup logs. Folders there are still listed, and your other drives are untouched. Turn it on to see
  everything
- **Icon Size** and **Background Opacity** for the browser window

---

## 9. File Explorer

A controller-driven file browser. It opens when you open a folder from inside the app — for example
**Open Language Folder** — and from the Console Desktop.

The bar across the bottom is this window's control reference:

| Button | Action |
|---|---|
| **A** | Open the selected item |
| **B** | Back — **hold** to exit the window |
| **X** | Actions menu |
| **Back** | Minimize the window |

The breadcrumb across the top shows where you are, and every part of it is a place you can jump to.
To its right sit **Sort by**, **View** and **✎ PATH** — the last of these lets you type a path directly.

Press **up** from the listing to reach that top row, then **left** and **right** to move along it;
**down** takes you back into the files. Moving sideways stays inside the row, so you cannot fall out
of it by accident.

### The actions menu

📸 `explorer/actions__file-menu__dark-city.png`

**X** opens it, and it acts on the selected item:

Select this item · Select up to here · Clear selection · Copy · Cut · Paste here · Pin to This PC ·
Rename · New folder · Delete · Run as administrator · Send to Desktop · Add to Profile ·
Add to Console Desktop · Properties

Entries that do not apply are dimmed rather than hidden, so the menu keeps the same shape.

### Sorting and view

📸 `explorer/listing__sort-size.png`

**Sort by** and **View** live in the top row, next to the path. Each states the setting currently in
use and moves to the next one every time you press it:

**Sort by**: **Name** → **Size** → **Date modified**
**View**: **Grid** → **List**

Folders always stay above files in every mode, and both choices are remembered between sessions. The
focus stays on the button after each press, so you can cycle straight through the modes.

### Pinning and the Recycle Bin

**Pin to This PC** keeps a folder on the browser's start page. The Recycle Bin appears there too;
inside it, the actions menu offers **Restore** and **Empty Recycle Bin**.

**Delete** asks first, with **Cancel** selected, and sends the item to the Recycle Bin, where
**Restore** brings it back to where it was.

⚠️ **Empty Recycle Bin** cannot be undone.

---

## 10. Appearance and themes

📸 `main/prefs-theme.png` · `main/prefs-theme__scope-strip.png`

**Preferences → Theme** is where the look is chosen.

### Scopes

The strip at the top has three **scopes** — **Main Application**, **Virtual Keyboard** and **Console
Desktop** — and under each it prints the theme that scope currently uses. They are independent: the
keyboard can use a different theme from the main window.

⚠️ If a scope points at a theme that no longer exists, its pill shows **Not found** with the name it
is looking for, instead of quietly falling back to something else.
📸 `main/prefs-theme__scope-notfound.png`

### Applying and editing

**To apply:** pick a card in the grid. The applied theme is marked with a **✓** badge — the badge
follows what is *applied*, not what the cursor is sitting on.
📸 `main/prefs-theme__applied-badge.png`

**To edit:** select a theme and press **Edit** — its colours load into the **Edit Theme** panel:
**Theme Name**, and the **Background**, **Highlight/Accent** and **Text** colours. Each colour row's
swatch **is** the button that opens the colour picker.

The primary button tells you exactly what it will do:

| Caption | What happens |
|---|---|
| **Save changes** | The name is unchanged, so this theme is overwritten |
| **Rename** | You changed the name — the theme is renamed in place, and you still have one theme |
| **Save as copy** | Always creates a second theme and leaves the original alone. If the name is still taken — you kept it, or it is a built-in's — a copy of *Mine* is called *Mine (copy)* (then *Mine (copy 2)*, …); this is also how to make your own version of a built-in theme |

📸 `main/prefs-theme__rename.png`

⚠️ A name another theme already uses is **refused** rather than silently overwriting it.
⚠️ **Delete** removes a custom theme permanently. Built-in themes cannot be deleted.

**Export** / **Import** move themes between PCs; **Open Theme Folder** opens where they are kept.

### The rest of the look

📸 `main/prefs-appearance.png`

**Preferences → Appearance**:

| Setting | What it does |
|---|---|
| **Virtual Keyboard Theme** | Whether the keyboard follows the selected theme |
| **Console Desktop Theme** | Whether the console desktop follows it |
| **Logo and Icon Tint** | Recolours the logo and app icons with the accent colour |
| **Device Layout Button Icons** | Controller glyphs instead of letters in Device Layout |
| **Controller Icons** | Which family's button icons are shown: **Automatic**, **Xbox**, **PlayStation**, **Nintendo** |

---

## 11. Controller reference

The pad hints printed on each screen are always right for that screen. This is the general case.

| Button | In the app's windows | In a radial menu | In File Explorer |
|---|---|---|---|
| **A** | Select / activate | Pick the highlighted entry | Open |
| **B** | Back; closes the innermost thing that is open | Close the menu | Back (**hold** to exit) |
| **X** | — | — | Actions menu |
| **LB / RB** | Change section (change menu in the radial editor) | — | — |
| **LT / RT** | Change tab | — | Switch view on the desktop |
| **D-Pad / Left Stick** | Move the focus | Choose an entry | Move the selection |

**Sliders** are entered with **A**; left/right then changes the value, and **A** or **B** leaves.
Left and right do nothing until you have entered the slider, so a value cannot be nudged by accident
while passing over it.

A single press moves the smallest amount that setting can take — two pixels on an icon size, one
percent on an opacity, ten milliseconds on a timing. **Holding** a direction grows the step from
there and reaches ten percent of the slider's range after about five seconds, then stays there.
Letting go and pressing again starts fine once more, so you can travel a long way and still land on
the exact value. Because the top speed is a share of each slider's own range, every slider feels the
same — a 0–100 one and a 10–1000 one take about the same time to cross.

**Hold-to-confirm buttons** (Delete, Exit, Empty Recycle Bin) fill a bar while held and only fire
when it is full. Letting go early cancels.

Every control in this app can be reached with the controller, the mouse **and** the keyboard. If one
of the three cannot reach something, that is a bug worth reporting.

### Detection

📸 `main/prefs-controller__dark-city.png`

**Preferences → Controller** shows what the app has found and how it is reading it.

Two of the settings below rely on drivers that do not come with Windows — **ViGEmBus** and **HidHide**.
The installer puts both on the machine for you, so there is nothing to download by hand; the panel
still shows whether each one is present, which is where to look if a setting says it is unavailable.
They are not part of this application and **uninstalling it leaves them installed**, because other
controller software may be using them. To remove them, use Apps & Features and look for *ViGEm Bus
Driver* and *HidHide*. Their licences are in `THIRD-PARTY-NOTICES.txt`, next to the application.

- **Input Source** — **Automatic** (XInput when it owns a controller, otherwise reads a DInput pad
  directly), **XInput only**, or **Raw (DInput) only**
- **Active Controller** — the pad in use and which path it is read through
- **Button Layout** — only for a controller read directly (DirectInput). The line says whether the
  layout in use is a known one or a guess; **Map Buttons** opens a step that asks you to press A, B,
  X, Y, the shoulders, the two menu buttons and the two stick clicks, one at a time, and remembers
  what it learns for that model of controller. Press nothing for eight seconds and that control is
  left as it was, so you can map only the buttons that are wrong. It takes effect the next time the
  controller is connected
- **DirectInput Compatibility** — lets games that only read Xbox controllers see yours, and mutes it
  while you type. It needs ViGEmBus, and the panel says whether that is installed.
  ⚠️ This only mutes a game that has **nothing else to read**. In DInput mode a game that reads XInput
  cannot see your pad at all, so the stand-in is its only controller and muting works — but a game that
  reads controllers the DirectInput way can still see your pad, and **which of the two it picks is the
  game's choice, not a setting**. Measured with the pad in DInput mode and hiding off: Project Diablo 2
  and Path of Exile 2 settled on the stand-in and stayed quiet while typing, Elden Ring settled on the
  physical pad and the left stick still moved the character. **Exclusive Controller Access** is the only
  thing that removes that choice — measured, not assumed: closing that route any other way would mean
  locking the controller away from the game, and this application has to keep the controller open itself in
  order to read it at all. In XInput mode the game
  reads your pad directly and the stand-in cannot mute anything — there **Typing Input Lock** is what
  keeps input out of the game, and only for programs that stop reading the pad when they are not the
  active window. Inside a game that polls it anyway, the **Buffer keyboard** is the answer.
- **Exclusive Controller Access** — stops games reading the controller directly, so the stand-in is
  the only pad they can see and muting always works. It needs HidHide, and the panel says whether that
  is installed.
  📌 **Turn it on if a game keeps reacting to the pad while you type in DInput mode.** With hiding off a
  game sees two controllers — yours and the stand-in — and picks one; if it picks yours, nothing can mute
  it. Hiding leaves one controller to find, so the answer no longer depends on the game. (Measured across
  three games: with hiding on, none of them reacted while the Realtime keyboard was open.)
  ⚠️ Turn this on when a tool such as Steam Input presents the controller to games a second time. That
  second controller is a real one as far as the game is concerned, so the stand-in cannot mute it and
  the character keeps moving while you type. Closing that tool has the same effect.
  📌 While it is on, the controller is hidden from ordinary programs — and it becomes visible again as
  soon as the setting is turned off or PersonalConsole closes.
  ⚠️ **What it does not cover, measured rather than assumed.** With the controller in its **XInput mode**
  a game can still read it: that route is not the one this hides, and switching the setting on does not
  close it. Typing without the game reacting works there through **Typing Input Lock** instead (see the
  Virtual Keyboard settings), or by putting the controller in its DirectInput mode. Programs running with
  administrator rights can also still see the controller.
  📌 It stays on and keeps watching. Turning it on with nothing connected is fine: the status line reads
  **Waiting for a controller** and the next controller to appear is hidden on its own. The same applies
  to a controller a tool presents a second time under a different name, which can happen at any point —
  including when a game starts. Nothing has to be turned off and on again.

---

## 12. Sharing settings with the other people who use this computer

Each Windows account that signs in to this computer gets its own settings. One switch changes that:
**Preferences → System → Share Settings With Other Accounts**.

It starts **off**, which is what the app has always done: settings stay in the signed-in account's own
Documents folder, and nothing changes until it is turned on. What it covers:

| Shared when it is on | Never shared |
|---|---|
| Profiles and every button binding in them | Console Desktop icons, tabs and pinned items |
| Themes and which one is in use | File Explorer's pinned and recent items |
| Radial menus and the buttons that open them | |
| Keyboard settings, layouts, and learned words | |

The two on the right describe programs installed on this computer, so another account reading them
would erase the ones it does not have. They have no switch and cannot be reached by this one.

Turning it on copies this account's settings into the computer-wide folder, and every account then
reads them from there. The account's own copy is left where it is, so turning it back off returns to
it. Restart the app for the new location to be read.

⚠️ This is about **accounts on this computer**, not about your own second machine. Settings already
follow you between your machines when your Documents folder syncs, and that is unaffected here.

**If another Windows account has already shared its settings**, theirs are the copies already in the
computer-wide folder, and turning this on simply starts using them. Nothing is copied and nothing is
replaced. Your own settings are untouched and come back the moment it is switched off again.

⛔ Nothing here ever deletes or overwrites a file, in either direction. The consequence worth knowing:
if settings are already shared by someone else, your own version cannot be made the shared one from
inside the app.

---

## 13. If something goes wrong

**The taskbar looks dead after the app was force-closed.**
Console Mode hides the Windows taskbar while it owns the screen and hands it back when the app exits
normally. Killing it from Task Manager skips that hand-back. Start PersonalConsole again and exit it
properly, and the taskbar comes back. This is a known limitation and is listed in
`RELEASE-README.md`.

**A button I just mapped does nothing.**
The editor's top-left corner names the profile **and** the layout you edited. Confirm both are the
ones actually in use — a binding placed on layout 3 does nothing while layout 1 is active. Also check
whether that direction is locked by a stick/D-Pad template (section 4).

**A game does not react to the controller.**
The active profile follows the foreground window; a program with no profile of its own gets the
Desktop profile. Use the Emergency Desktop Mode combination (section 3) to force your way back, then
give the game its own profile.

**Menus work but games do not, or the other way round.**
See **Preferences → Controller**. If a game only reads XInput and your pad is in DInput mode, turn on
**DirectInput Compatibility** so the game sees a stand-in controller.

**A game bought on Steam reports no controller at all, while other games are fine.**
Steam can take Xbox-type controllers into its own layer before a game ever sees them, and the stand-in
controller this application presents is one of the things it takes. The game is then told there is no
controller, no matter what the pad is doing. Measured on Path of Exile 2: with the setting below on, the
game showed **Input Method: WASD** and reacted to nothing; with it off, the same pad appeared instantly
as **Controller Device: Xbox Controller #1**.
Two ways to fix it:
- **Once, for the whole library** — Steam → Settings → **Controller** → Show Advanced Settings → *Enable
  Steam Input for Xbox controllers* → off. Measured: with that one switch off and no per-game setting at
  all, Path of Exile 2 announced *Input method set to Controller* on its own. This is the setting to
  reach for, and it also overrides per-game ones: a game whose own setting says *Enable Steam Input*
  still worked while this was off.
  ⚖️ What it costs: Steam's own layout engine for Xbox pads stops everywhere — community controller
  configs, Steam layouts, and pad navigation in the Steam overlay. Nothing this application does depends
  on any of that, because the mapping happens here instead.
- **For one game only**, if the trade above is unwanted — Library → right-click the game → Properties →
  **Controller** → *Disable Steam Input*. This has to be repeated per game.
📌 **A notice appears over the game.** When a Steam game comes to the front while Steam is set to take Xbox
controllers and your controller is hidden from games, a short message says so and names the setting to
change. It shows once per game, so it never repeats while you play.
📌 **Preferences → Controller says so itself.** While the stand-in is connected and Steam is set to take
Xbox controllers, the line under **DirectInput Compatibility** adds a warning naming this and where to
change it. No warning there means either the setting is off or it could not be read — the app stays quiet
rather than guess.
📌 Another way to tell: Steam → Settings → **Controller** lists the controllers it is holding. If the
stand-in controller is missing from that list while games still report none, this is what is happening.

**In DInput mode a game ignores the controller until the window is clicked.**
A game that finds several controllers at startup picks one of them, and with the pad in DInput mode it
can pick one that is not the stand-in it should be reading. Clicking makes the game look again, which
is why it appears to fix it. The lasting answer is to leave it only one controller to find: turn on
**Exclusive Controller Access** (Preferences → Controller), and connect the pad before starting the
game so the hiding is already in place. The status line reads **Hiding 1 controller from other
programs** once it is.

**The console desktop will not come forward.**
It does not take focus from a window you are using — that is deliberate. Minimise or close what is in
front of it, or pick up the pad if **Auto Hide/Show on Input** is on.

**A language will not turn off.**
At least one keyboard language must stay enabled.

**Text goes to the wrong place when I type in a game.**
Use the **Buffer Keyboard** instead of the Realtime one: it holds the game still and delivers the
text when you close the keyboard.

**I want to report something.**
**Preferences → System → Open log folder** opens the logs. They record what the app did and never
contain passwords or usernames.
