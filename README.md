# QuickSnap

Screenshots in Minecraft are annoying to share. You take one, then you're tabbing out, hunting down the screenshots folder, dragging the file into Discord or a browser, waiting for it to upload. QuickSnap cuts all of that out. Press 1 and you get a link, press 2 and the image is straight in your clipboard.

![Minecraft](https://img.shields.io/badge/Minecraft-26.1-brightgreen?style=flat-square)
![Fabric](https://img.shields.io/badge/Fabric-0.18.0+-blue?style=flat-square)
![Version](https://img.shields.io/badge/1.0.0--beta.1-yellow?style=flat-square)

---

## Requirements

- Minecraft `~26.1`
- Fabric Loader `>=0.18.0`
- Fabric API
- Java 25+

Client-side only, works on any server.

---

## Installation

Drop the jar in your mods folder. Make sure Fabric API is installed too.

ModMenu is optional but if you have it, QuickSnap will show up there.

---

## Basic usage

Press F2 like normal. A popup appears in the corner giving you options for what to do with the screenshot. Press 1 to upload, 2 to copy to clipboard, G to open the gallery, or Esc to ignore it and move on.

That's really it for day to day use.

### Gallery

Press G at any time to open the gallery. Your screenshots are grouped by world and server name so they're actually organized instead of just a flat list of filenames.

From there you can upload, copy, delete, open the folder, or edit the overlay effects on a specific shot before uploading it.

One thing worth knowing: on first launch QuickSnap unbinds the vanilla Quick Actions key to free up G for the gallery. It only does this once. If you rebind it yourself afterwards it won't touch it again.

---

## Overlay effects

You can have QuickSnap stamp info onto your screenshots at capture time. Coords, biome, game mode, FPS, shader pack name, timestamp, whatever you want. It uses the Monocraft pixel font so it actually looks decent.

Two styles: **Tooltip** (dark panel in the corner) and **F3 Debug** (plain text like the F3 screen). Position is configurable too, anywhere from top-left to bottom-right.

Everything is off by default. Turn on what you actually want.

### What's available

**Player info:** Player name, health and hunger, XP level, game mode, held item.

**World info:** Coordinates, biome, dimension, weather, in-game time, difficulty, world or server name, moon phase. Coords have an obfuscate option that rounds X and Z to the nearest 50 if you don't want your exact location visible.

**Setup:** MC version, mod count, shader pack (needs Iris installed), resource pack, resolution.

**Performance:** FPS, GPU name.

**Other:** Timestamp, a small watermark with customizable text.

### Per-shot editing

Every screenshot gets a `.json` sidecar file saved next to it. It stores the game data from when the shot was taken and which effects were active. You'll never really notice it's there.

What it enables is editing effects per-shot from the gallery. If you want to add coords to a specific screenshot before uploading it, you can do that without it affecting your global settings. The original file doesn't get touched, the overlay is applied fresh when you upload.

---

## HUD hiding

If you want clean screenshots without the hotbar and crosshair in them, QuickSnap can handle that automatically.

| Option | What it does |
|---|---|
| Auto-Hide HUD | Hides everything, same as F1 |
| Hide Hotbar | Just the hotbar |
| Hide Chat | Just the chat |
| Hide Crosshair | Just the crosshair |
| Hide Hand | Hides your held item in first person |

These can be combined. When any of them are on, QuickSnap waits two frames before actually capturing so the HUD has time to disappear. The quick-action popup is always hidden from screenshots automatically so you don't have to worry about that.

---

## Upload providers

### Catbox (default)

No setup at all. Uploads to catbox.moe and gives you a permanent direct link.

### Litterbox

Same thing but links expire. Duration is configurable: `1h`, `12h`, `24h`, `72h`.

### Imgur

Needs a Client ID. It's a bit annoying to get but it's free.

1. Go to [api.imgur.com/oauth2/addclient](https://api.imgur.com/oauth2/addclient)
2. Register an app, pick "OAuth 2 authorization without a callback URL"
3. Copy the Client ID (not the secret)
4. Paste it into the Imgur Client-ID field in settings

### Custom

If you're running a server and want everyone's screenshots going to your own backend, this is what you want. Point it at any HTTP endpoint that accepts a multipart file upload.

| Setting | Description |
|---|---|
| Custom URL | The URL to POST to |
| Custom Field Name | The form field name for the file, defaults to `file` |
| Custom Headers | Extra headers in `Key: Value` format, separate multiple with `;` or newlines |
| Custom Response Regex | Regex to pull the URL out of the response. Group 1 is used if there is one, otherwise the full match. Leave blank to use the whole response body. |

For example if your server responds with `{"url":"https://example.com/abc.png"}` you'd set the regex to `"url":"([^"]+)"` and it would extract the URL from group 1.

---

## Config file

Saved at `.minecraft/config/quicksnap.json`. Gets written on startup and whenever you change a setting. You can edit it by hand if you want, changes take effect on next launch.

```json
{
  "showSmartSwitch": true,
  "autoHideHud": false,
  "overlayStyle": "Tooltip",
  "overlayPosition": "BottomLeft",
  "gallerySortMode": "Newest",
  "uploadProvider": "Catbox",
  "imgurClientId": "",
  "litterboxDuration": "1h",
  "customUploadUrl": "",
  "customUploadFieldName": "file",
  "customUploadHeaders": "",
  "customUploadResponseRegex": ""
}
```

---

## Compatibility

Uses mixins on three vanilla classes: the screenshot handler, the GUI renderer, and the item-in-hand renderer. Should be fine with most mods. If there's a conflict it'll crash on startup so it's not something that'll sneak up on you.

Shader pack detection only works if Iris is installed. There's no hard dependency on it, it just won't show shader info if it's not there.

No Forge or NeoForge support.

---

## Bugs

This is a beta so there are probably some. If you find one open an issue with your QuickSnap version, Minecraft and Fabric Loader version, mod list if it crashed, and the relevant part of `.minecraft/logs/latest.log`.

---

MIT license.
