# WHIP: txAdmin recipe

A complete base for a FiveM server. Players can make a character, carry and stash things, keep money in the bank, buy,
fuel and tune a car, own a home and furnish it, and be hurt and revived. You run it from inside the game: `/whip`
opens an admin panel for players, items, settings, banks, garages, shops and homes.

This repository holds the txAdmin recipes that install WHIP. The framework itself lives in the resource repositories
listed [below](#whats-in-it).

## Install

WHIP is installed as a **new server**, by txAdmin: the recipe makes the whole server (resources, database,
`server.cfg`). It is not added to a server you already have.

**You need:**

- FiveM's server with txAdmin: **Legacy** (`FXServer`) or **GTA V Enhanced** (`cfx-server`). WHIP runs on both.
- MariaDB 10.2.3+ or MySQL 8.0.16+, and a user that may create a database.
- About 1 GB of disk, and at least 10 Mbit/s to GitHub: txAdmin gives each download 150 seconds, and the largest is
  about 145 MB.

**Then:**

1. Start the server and open txAdmin. On its setup page choose **Remote URL Template** and paste the recipe for your game:

   | Game | Recipe URL |
   | --- | --- |
   | Legacy | `https://raw.githubusercontent.com/WHIPFX/txAdmin-recipe/main/whip-legacy.yaml` |
   | Enhanced | `https://raw.githubusercontent.com/WHIPFX/txAdmin-recipe/main/whip-enhanced.yaml` |

2. Give it your server key (from [portal.cfx.re](https://portal.cfx.re)) and your database login. txAdmin downloads
   WHIP and oxmysql, makes an empty database (WHIP builds every table itself, so there is no SQL file), turns OneSync
   on, writes `server.cfg` and makes your txAdmin account an admin.
3. Start the server.

### Check it works

1. A few seconds after `Framework loaded successfully!` the console prints a **WHIP setup** block: anything marked FIX
   says what to change. Type `whipsetup` in the console to see it again.
2. Join. Make a character, and the appearance creator opens.
3. **F2** opens the inventory, **/w** has the quick tools and **/whip** the admin panel. Another admin: they type `/w`,
   and the console prints the `add_ace` line to add for them.

Next: open `resources/whip_core/docs.html` in a browser. It works offline.

## Updating

Each resource is released on its own GitHub page. Read `whip_core/CHANGELOG.md` first, then stop the server and replace
the resource's folder with the new release's (**Source code (zip)** on its releases page). The database carries over:
whip_core changes its tables to match on the next start.

What the server wrote into the old folders is yours to carry over: Director packs you exported
(`whip_core/pack_exports`), pictures you took in game (`whip_media_props/img/props`, `whip_media_previews/img/vehicles`)
and the engine list if you ran `/whiplashengines` (`whip_autos/engines.json`).

## What's in it

In `resources`, each started by its own `ensure` line in `server.cfg`:

| Resource | What it is |
| --- | --- |
| [whip_core](https://github.com/WHIPFX/whip_core) | Players, characters, inventory, money and banks, jobs, vehicles, garages, properties, the UI, the Director, the admin panel |
| [whip_gunny](https://github.com/WHIPFX/whip_gunny) | Weapons: magazines, calibers, melee, throwables, attachments |
| [whip_fuel](https://github.com/WHIPFX/whip_fuel) | Fuel tanks, pumps with a hose, the jerry can |
| [whip_rope](https://github.com/WHIPFX/whip_rope) | A physics rope: carry it, tie it, tow with it |
| [whip_phone](https://github.com/WHIPFX/whip_phone) | The Duo phone, whose apps are modules |
| [whip_autos](https://github.com/WHIPFX/whip_autos), [whip_autos_shop](https://github.com/WHIPFX/whip_autos_shop) | Whiplash: customisation, turbos and nitrous, and the shop in Burton |
| [whip_voice](https://github.com/WHIPFX/whip_voice) | Voice chat on your own voice server, in 3D |
| [whip_chat](https://github.com/WHIPFX/whip_chat), [whip_loadingscreen](https://github.com/WHIPFX/whip_loadingscreen) | Chat and the loading screen |

In `resources/[assets]/[media]`, kept out of whip_core so its updates stay small (`ensure [assets]` starts everything
in `[assets]`):

| Resource | What it is |
| --- | --- |
| [whip_media_clothing_male](https://github.com/WHIPFX/whip_media_clothing_male), [whip_media_clothing_female](https://github.com/WHIPFX/whip_media_clothing_female) | The character editor's clothing pictures |
| [whip_media_props](https://github.com/WHIPFX/whip_media_props) | The furniture catalog's pictures |
| [whip_media_previews](https://github.com/WHIPFX/whip_media_previews) | Vehicle, NPC and tattoo pictures, and the emote menu's preview videos |
| [whip_media_map](https://github.com/WHIPFX/whip_media_map) | The pause menu's 3D map, GTA's blip icons and the HUD radar's tiles |

The two map packs are in `resources/[assets]`:

| Resource | What it is |
| --- | --- |
| [whip_homes_mirrorpark](https://github.com/WHIPFX/whip_homes_mirrorpark) | Every Mirror Park house as a walk-in home, and 25 garages |
| [whip_graphics](https://github.com/WHIPFX/whip_graphics) | LED streetlights and WHIP's look for the city's light and sky |

oxmysql, which WHIP needs for its database, is in `resources/[ox]`. It is [WHIPFX's fork](https://github.com/WHIPFX/oxmysql):
oxmysql with one fix, so that a query fails at once when the database is down instead of waiting for ever.

Every resource runs on both games. Those with models carry them twice, `stream/` for Legacy and `stream_enhanced/` for
Enhanced (which reads that instead), and an Enhanced server says so in `server.cfg` with `setr whip_platform enhanced`.
That line (and Legacy's `sv_enforceGameBuild`) is the only difference between the two recipes.

## Releasing

A WHIP release is the list of tags in the recipes. To release a resource:

1. Bump `version` in its `fxmanifest.lua`, commit, and release that commit as `v<version>`:
   `gh release create v1.0.1 --repo WHIPFX/whip_core --target main --generate-notes`
2. Change its `ref` in **both** recipes, `whip-legacy.yaml` and `whip-enhanced.yaml` (they list the same
   resources), and bump the recipes' `version`.
3. Push to `main`. txAdmin reads the recipes from there, so the next new server gets the new release.

The recipes follow txAdmin's [recipe format](https://github.com/citizenfx/txAdmin/blob/master/docs/recipe.md).

## Licence

WHIP's resources are under the GNU LGPL 3.0; see each repository's `LICENSE` and `NOTICE.md`.
