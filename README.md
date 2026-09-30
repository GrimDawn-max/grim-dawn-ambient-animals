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

### On Linux or macOS, under Wine or CrossOver

Wine prefers its own built-in `winmm`, so it has to be told otherwise. In the
bottle's Wine configuration, add a library override for **winmm**, set to
**Native (Windows), then Builtin**.

Scope it to `Grim Dawn.exe` rather than the whole bottle — a bottle-wide winmm
override breaks other applications in it. Shut the bottle down before editing
the registry by hand: Wine keeps it in memory and rewrites it on exit, so
changes made while it is running are lost.

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
