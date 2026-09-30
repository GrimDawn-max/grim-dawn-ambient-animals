# Grim Dawn Ambient Animals

Brings the ambient animals back to the Grim Dawn main menu and to the world,
on the current game version.

A parrot, a cat or an axe beak appears at the campfire on the login screen, and
a creature appears when you enter certain areas — a seagull, a squirrel, a
sitting bird, a rooster, a wandering turtle, a crocodile, a hippo or a
basilisk. One animal per area, chosen at random, so they come and go.

## Installing

Copy two folders into your Grim Dawn directory, alongside `Grim Dawn.exe`:

    x64/winmm.dll
    x64/GIA64-world.txt
    _GIAnimalData/database/GIA_Animals.arz
    _GIAnimalData/resources/creatures.arc

Nothing is renamed or replaced. `x64/` already exists; `_GIAnimalData/` is new.

### On Windows

That is the whole install. The game loads `winmm.dll` from its own folder
because Windows searches the executable's directory first and `winmm` is not a
protected system library.

### On macOS or Linux, under CrossOver, Wine or Proton

One extra step is needed. Wine ships its own `winmm` and prefers it, so it has
to be told to use the one you just installed. Without this the files sit there
and nothing loads.

**CrossOver.** In the bottle's settings, open Wine Configuration, and under
Libraries add an override for **winmm**, set to **Native (Windows), then
Builtin**. Scope it to `Grim Dawn.exe` rather than the whole bottle — a
bottle-wide winmm override breaks other applications in it. Shut the bottle
down before editing the registry by hand, because Wine keeps it in memory and
rewrites it on exit, so changes made while it is running are lost.

**Steam with Proton.** Set the launch options for Grim Dawn to:

    WINEDLLOVERRIDES="winmm=n,b" %command%

**Plain Wine or Lutris.** Either run `winecfg` and add the same override under
Libraries, or set the environment variable before launching:

    WINEDLLOVERRIDES="winmm=n,b"

`n,b` means native first, then builtin — the same thing the dialog does.

To check it worked, look for `x64/GIA64-WINMM.log` after launching. If that
file does not appear, the override has not taken effect.

## Compatibility with other tools

This installs as `x64/winmm.dll`. Some other Grim Dawn tools do the same —
**dpYes!** (the player and pet DPS meter) is one — and only one file of that
name can exist in the folder. Installing this over another tool's `winmm.dll`
will stop that tool working, and vice versa.

There is no way round it at present. `winmm` is the obvious library to use
because the game imports it, does not ship it, and Windows is willing to load
it from the game's own folder — so independent tools keep arriving at the same
answer.

If you want both, keep a copy of each `winmm.dll` and swap them. Renaming this
one does not work; the game has to load it under that name.


## What this has been tested on

Grim Dawn **1.3.0.8, GOG standalone**, on:

- Windows
- macOS under CrossOver

It has **not** been tested on the Steam version, on Proton, or on Linux. The
mechanism is the same everywhere — the game is a Windows program in all cases,
and the loading behaviour it relies on is standard — but untested is untested,
and the Steam install layout and Proton's override mechanism both differ from
what was actually verified.

## Moving the animals

`x64/GIA64-world.txt` lists where each creature goes, one per line:

    region | record | x | y | z | yaw | scale | snap

It is read every time an animal is placed, so changes take effect without
rebuilding anything — though you will need to leave the area and come back,
since only one animal is placed per area.

- **yaw** is degrees. Each model has its own built-in forward direction, so
  some need turning.
- **scale** is optional, default 1.0.
- **snap** set to 1 finds the ground and ignores the height given. Use it when
  you move something somewhere new; leave it off for anything perched.

Lines beginning with `#` are comments. Adding a second creature to a region
widens the roll rather than placing both.

## Credits

The animals, and the idea of putting them in the game, are **GlockenGerda's**,
from Grim Internals. Grim Internals no longer runs on current versions of Grim
Dawn, and its other features have either been absorbed into the game or are
served by other tools — but the animals went with it, and nothing replaced
them.

The placements here are hers: the coordinates were recovered from Grim
Internals itself, so the seagull sits where she put it. Two things differ —
the seagull is scaled down, and the rooster stands at the foot of the post
rather than on top of it.

The creature models and records are Crate Entertainment's, as shipped with
Grim Dawn.

This is a separate implementation. It shares no code with Grim Internals,
which was never published in source form.
