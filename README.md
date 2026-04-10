# QuickSnap

Screenshots in Minecraft are annoying to share. You take one, then you're tabbing out, hunting down the screenshots folder, dragging the file into Discord or a browser, waiting for it to upload. QuickSnap cuts all of that out. Press 1 and you get a link, press 2 and the image is straight in your clipboard.

![Minecraft](https://img.shields.io/badge/Minecraft-26.1-brightgreen?style=flat-square)
![Fabric](https://img.shields.io/badge/Fabric-0.18.0+-blue?style=flat-square)
![Version](https://img.shields.io/badge/1.0.0--beta.2-yellow?style=flat-square)
[![Modrinth](https://img.shields.io/badge/Modrinth-Download-1bd96a?style=flat-square&logo=modrinth)](https://modrinth.com/mod/quicksnap)
[![Website](https://img.shields.io/badge/Website-quicksnap.pics-97c459?style=flat-square)](https://quicksnap.pics)

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

ModMenu is optional but if you have it, QuickSnap shows up there with links to the website and issue tracker.

---

## Basic usage

Press F2 like normal. A popup slides in from the top-right corner with options for what to do with the screenshot. Press 1 to upload, 2 to copy to clipboard, G to open the gallery, or Esc to dismiss it. Your game keeps running the whole time.

The popup has a timer bar at the bottom showing how long until it auto-dismisses, and a sub-panel that slides out during uploads showing progress and percentage.

That's really it for day to day use.

---

## Gallery

Press G at any time to open the gallery. Screenshots are grouped by world and server name into collapsible sections so they're actually organized instead of just a flat list of filenames. When you open the gallery while in a world, everything except that world's section is collapsed by default.

Thumbnails are loaded lazily and scaled with bilinear filtering so they look sharp on HiDPI displays. Arrow keys navigate between shots, double-click opens the full viewer.

Each screenshot shows its filename, file size, and timestamp in the footer. The footer action row gives you:

- **Upload** -- uploads the selected screenshot using your configured provider
- **Copy** -- copies the image file to your clipboard
- **Delete** -- deletes the file and its sidecar
- **Folder** -- opens the containing folder in your file manager
- **Effects** -- opens the overlay effects editor for that shot
- **Settings** -- opens QuickSnap settings

Sort modes at the top: **Newest**, **Oldest**, **Favorites**.

### Favorites

Click the star in the top-left corner of any thumbnail to favorite it. Favorited screenshots get a dedicated section pinned at the top of the gallery regardless of sort mode. Single-clicking a favorite opens it directly in the viewer.

### One thing worth knowing

On first launch QuickSnap unbinds the vanilla Quick Actions key to free up G for the gallery. It only does this once. If you rebind it yourself afterwards it won't touch it again.

---

## Screenshot viewer

Double-clicking a thumbnail (or pressing Enter or E) opens the full viewer. It shows a live preview with your overlay effects applied and a panel on the right where you can edit those effects per-shot before uploading or copying.

The viewer supports left/right keyboard navigation between all screenshots in the same gallery section so you can flip through shots without going back to the gallery.

Changes made in the viewer are saved to the screenshot's sidecar and don't affect your global settings.

---

## Overlay effects

QuickSnap can stamp info onto your screenshots at capture time using the Monocraft pixel font. Two display styles:

- **Tooltip** -- a dark panel in the corner
- **F3 Debug** -- plain text like the F3 screen

Position is configurable from top-left to bottom-right. Font scale has four presets: Small, Normal, Large, XL.

Everything is on by default on first launch so you can see what it looks like. Turn off whatever you don't want from the Effects screen or per-shot from the viewer.

### What's available

**Player:** Player name, health and hunger, XP level, game mode, held item.

**World:** Coordinates, biome, dimension, weather, in-game time, moon phase, difficulty, world or server name. Coords have an obfuscate option that rounds X and Z to the nearest 50 if you don't want your exact location visible.

**Setup:** MC version, mod count, shader pack (needs Iris), resource pack, resolution.

**Performance:** FPS, GPU name.

**Other:** Timestamp, watermark with customizable text.

### Per-shot editing

Every screenshot gets a `.json` sidecar saved in a `.meta` subfolder next to it. It stores the game data captured at screenshot time and which effects were active. You'll never notice it's there.

What it enables is editing effects per-shot from the viewer or gallery without touching your global settings. The original file is never modified, the overlay is applied fresh when you upload or copy.

---

## HUD hiding

If you want clean screenshots without UI elements in them, QuickSnap can handle that automatically. When any of these are enabled it waits two frames before capturing so everything has time to disappear. The QuickSnap popup is always excluded from screenshots automatically.

| Option | What it does |
|---|---|
| Auto-Hide HUD | Hides everything, same as F1 |
| Hide Hotbar | Just the hotbar |
| Hide Chat | Just the chat |
| Hide Crosshair | Just the crosshair |
| Hide Hand | Hides your held item in first person |

---

## Upload providers

### screenshot.rest (default)

QuickSnap's own upload service at [screenshot.rest](https://screenshot.rest). No setup, no account needed. Screenshots are compressed to JPEG at 85% quality before uploading so they're a fraction of the raw PNG size with no visible difference.

Every upload gets a viewer page with Discord embed support so the link shows a full preview when you paste it anywhere. The viewer has download, raw image, and copy link buttons. Links are permanent.

Uploads use a per-request HMAC challenge/response system. The mod fetches a single-use token from the server, signs it, and sends both with the upload. The server deletes the token on use so it can't be replayed. Even if someone extracts the signing key from the jar, they can't pre-generate valid tokens.

### Litterbox

No setup. Links expire after a configurable duration: `1h`, `12h`, `24h`, or `72h`.

### Imgur

Needs a Client ID. It's free.

1. Go to [api.imgur.com/oauth2/addclient](https://api.imgur.com/oauth2/addclient)
2. Register an app, pick "OAuth 2 authorization without a callback URL"
3. Copy the Client ID (not the secret)
4. Paste it into the Imgur Client-ID field in QuickSnap settings

### Custom

Point it at any HTTP endpoint that accepts a multipart file upload. Useful if you're running your own image host or want everyone on a server going to the same place.

| Setting | Description |
|---|---|
| Custom URL | The URL to POST to |
| Custom Field Name | Form field name for the file, defaults to `file` |
| Custom Headers | Extra headers as `Key: Value`, separate multiple with `;` or newlines |
| Custom Response Regex | Regex to extract the URL from the response. Group 1 is used if present, otherwise the full match. Leave blank to use the whole body. |

---

## Analytics

On first launch a consent screen appears explaining what's collected and gives you the option to opt out before anything is sent. You can also toggle it off at any time in settings.

When enabled, after each upload or clipboard copy the mod sends the name of the provider you used (e.g. `screenshot.rest`, `imgur`) to screenshot.rest. That's it. No usernames, no IPs, no image data, nothing identifiable. The counts show up as aggregate stats on [quicksnap.pics](https://quicksnap.pics).

---

## Config file

Saved at `.minecraft/config/quicksnap.json`. Written on startup and whenever you change a setting. You can edit it by hand, changes take effect on next launch.

```json
{
  "showSmartSwitch": true,
  "autoHideHud": false,
  "overlayStyle": "Tooltip",
  "overlayPosition": "BottomLeft",
  "overlayFontScale": 1.0,
  "gallerySortMode": "Newest",
  "analyticsEnabled": true,
  "uploadProvider": "screenshot.rest",
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

This is a beta so there are probably some. If you find one open an issue with your QuickSnap version, Minecraft and Fabric Loader version, your mod list if it crashed, and the relevant part of `.minecraft/logs/latest.log`.

---

All Rights Reserved. See [LICENSE](LICENSE) for full terms including data collection disclosure.
