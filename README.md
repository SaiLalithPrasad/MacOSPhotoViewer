# Contact Sheet

A single-file photo and video viewer for a local folder. No server, no build step, no dependencies, no network. Open `contact-sheet.html` in a browser, point it at a folder, and browse.

Built for Apple Photos exports, where a folder is rarely just photos: it also holds `.AAE` edit sidecars, duplicate `IMG_E` renders, and `.MOV` clips belonging to Live Photos. Contact Sheet reads those conventions instead of showing you the mess.

## What it does

- **Grid + full-screen viewer** — keyboard, swipe, and slideshow navigation; any aspect ratio fits without cropping.
- **Filters out the noise** — `.AAE`, `._` resource forks, `.DS_Store`, and hidden files never appear as tiles. Subfolders are included.
- **Prefers the edited version** — where Photos wrote `IMG_E1234` next to `IMG_1234`, you see the edited render and the original folds in behind it, so nothing shows up twice.
- **Merges Live Photos** — a still and a same-named `.MOV` become one tile with a `LIVE` badge. Press `l` to play the motion.
- **Reads `.AAE` sidecars** — includes a binary plist parser (Apple moved from XML to `bplist00`), so the info panel shows when a photo was edited, by which app, the format identifier, and the effects detected inside the adjustment blob.
- **Recomputes long exposures** — Apple's long-exposure composite isn't stored anywhere, so the page rebuilds it: it samples frames from the Live Photo `.MOV`, averages them on canvas, and offers a PNG export.

## Keys

| | |
|---|---|
| `←` `→` | previous / next |
| `esc` | close |
| `space` | play or pause video |
| `i` | info panel (file + sidecar data) |
| `l` | play Live Photo motion |
| `s` | slideshow |
| `f` | true full screen |

## Use

Download `contact-sheet.html` and open it. That's the whole install.

Serving it (`python3 -m http.server`, or GitHub Pages) additionally enables Chromium's File System Access picker, which remembers folder permission between visits, and drag-and-drop of a folder onto the page.

Files are read directly from disk by the browser. Nothing is uploaded, copied, or transmitted — there is no backend to send anything to.

## Browser support

**Safari on macOS is the best target.** It decodes `.HEIC` and HEVC `.MOV` natively. Chrome cannot decode HEIC at all and often fails on HEVC video; those tiles show a placeholder saying so. Long-exposure rendering needs the browser to decode the clip, so it is effectively Safari-only for iPhone footage.

## Limits

- **Portrait blur can't be reconstructed.** The depth map is an auxiliary image inside the HEIC, and the blur is already baked into the render you're looking at.
- **Edits can't be applied.** An `.AAE` records a recipe in an undocumented, version-dependent blob. You see *that* a photo was edited and roughly what kind of edit it was, not the edited result — unless Photos also exported the `IMG_E` file.
- **Pairing is name-based.** A photo and video sharing a filename stem are assumed to be a Live Photo. Toggle "Merge Live Photos" off if that's wrong for your folder.
- **No EXIF yet.** Capture data (lens, shutter, ISO, GPS) lives in the image file, not the sidecar.

## License

MIT
