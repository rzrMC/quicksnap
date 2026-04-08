# QuickSnap

A Fabric mod that makes sharing Minecraft screenshots actually convenient. Take a screenshot, upload it, get a link — without tabbing out or digging through your files folder.

![Minecraft](https://img.shields.io/badge/Minecraft-26.1-brightgreen?style=flat-square)
![Fabric](https://img.shields.io/badge/Fabric-0.18.0+-blue?style=flat-square)
![Version](https://img.shields.io/badge/1.0.0--beta.1-yellow?style=flat-square)

---

## What it does

- In-game gallery grouped by world and server so your screenshots are actually organized
- One-click upload to Catbox, Litterbox, Imgur, or your own server
- Copy screenshots to clipboard directly from the gallery
- Screenshot overlays — coords, biome, FPS, game mode, held item, timestamp, watermark and more. All individually toggleable
- Two overlay styles: Tooltip and F3 Debug
- Configurable overlay position (bottom left by default)
- Style preview so you can see what your overlay looks like before you commit to it
- Per-shot effect editing from the gallery
- Auto-hide HUD on capture, with independent toggles for hotbar, chat, crosshair, and hand
- Gallery sort order is configurable
- Gallery key is rebindable from the standard controls menu

---

## Installation

Requires Fabric Loader `0.18.0+`, Fabric API, and Java 25+.

Drop the jar in your mods folder. That's it.

ModMenu is optional but supported if you want settings from the mods screen.

---

## Keybinds

The gallery key shows up in the standard Minecraft controls menu under the QuickSnap category and can be rebound to whatever you want. The quick-action popup isn't a keybind — it just appears automatically after you take a screenshot.

---

## Upload providers

Catbox is the default and requires no setup. Litterbox works the same but links expire after a set time. Imgur needs a Client ID which you can get for free from their developer portal. Custom lets you point it at any HTTP endpoint — useful if you're running a server and want screenshots going somewhere central.

Switch between them anytime from the in-game settings screen.

---

## Requirements

- Minecraft `~26.1`
- Fabric Loader `>=0.18.0`
- Fabric API
- Java 25+

Client-side only, works on any server.

---

## Compatibility

Hooks into three vanilla classes via Mixin — the screenshot recorder, the GUI renderer, and the item-in-hand renderer. Should be compatible with most mods. If you're running a heavy HUD or rendering mod and hit a mixin conflict on startup it'll show up immediately in your log, open an issue and include the crash.

No Forge or NeoForge support.

---

## Bugs / issues

Open an issue and include your Fabric version, mod list if it's a crash, and the relevant bit of your latest.log.

---

MIT license.
