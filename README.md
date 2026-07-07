<img src="./screenshot.jpg" />

# FAVpage

A personal start page / homelab dashboard in the spirit of Homarr or Heimdall — except the whole app is **a single HTML file with zero dependencies**. No server, no build step, no framework. Open it in a browser and it just works.

## Features

- **Free-form layout** — drag & drop tiles and groups anywhere on a 20 px grid. Elements never overlap: dropping onto an occupied spot pushes neighbours down ("make room"), and when a group shrinks, the elements below slide back up.
- **Groups** — resizable containers with custom titles and colors. Tiles can live inside a group or float freely on the board.
- **Lasso multi-select** — in edit mode, drag across empty space to select multiple tiles, then move the whole selection at once (onto the board or into a group), keeping relative spacing.
- **Smart icons** — icon resolution cascade: custom value (a [dashboard-icons](https://github.com/homarr-labs/dashboard-icons) name, an image URL, or a `data:image/...` URI) → site favicon → colored letter avatar. One click downloads all icons and embeds them as base64 for fully offline use. Downloaded content is validated (MIME type + magic bytes), so an HTML page served instead of a favicon never gets embedded.
- **Hover effect** — a rotating gradient border in the icon's dominant color (extracted at runtime, cached).
- **Animated background** — a subtle particle network (30 % alpha) drawn on a canvas behind the content; pauses automatically when the tab is hidden.
- **Instant search** — press `/` or click the magnifier to roll out the search box; `Esc` clears and collapses it.
- **No accounts, no cloud** — your data is one readable JavaScript file next to the HTML.

## Getting started

1. Grab `index.html` and `links.js` and put them in one folder.
2. Open `index.html` in **Chrome, Edge, or Brave** (saving uses the File System Access API; other browsers can view the page but not save).
3. Click the **paperclip button** and pick your `links.js` — from now on every change is saved automatically.
4. Click the **pencil button** to enter edit mode:
   - drag groups by the `⠿` handle, resize them by the bottom-right corner,
   - drag tiles anywhere — within a group, into another group, or onto the open board,
   - use the `+` buttons to add links and groups.

## Data, sync between computers

All data lives in `links.js` (`window.FAV = {...}`) — groups, links, positions, icons — with a content timestamp (`meta.updatedAt`) bumped on every change.

- Every change is also cached in `localStorage`, so unsaved work survives a refresh or a crash.
- **On start** the newer of file vs. cache wins; adopted unsaved work is flagged so you remember to save it.
- **In the background** (on window focus and once a minute) the connected file's timestamp is compared — when another computer wrote a newer version, it is loaded automatically. Local unsaved changes are never silently overwritten; conflicts only produce a warning.

Put the folder in any synced location (Nextcloud, Dropbox, Syncthing, a network share…) and every computer sees the same board. Any machine can add links; the timestamps sort it out.

As a safety net, every save sanitizes icon data: any `data:` icon that is not a real image is dropped and the link falls back to the automatic icon chain.

## Keyboard shortcuts

| Key | Action |
|---|---|
| `/` | open & focus search |
| `Esc` | clear search / cancel selection / close dialog |
| `Enter` | confirm dialog |

## Tech notes

- Single file: ~180 lines of CSS and ~1000 lines of vanilla ES5-style JavaScript, no dependencies.
- Collision layout is a simple axis-aligned "pack down" algorithm — the dropped element stays, overlapping neighbours shift down just enough to clear; it always terminates because y only ever increases.
- Local preview during development: any static server, e.g. `python -m http.server`.

## Browser support

Full functionality (including saving) requires a Chromium-based browser with the File System Access API — Chrome, Edge, Brave, Opera. Firefox and Safari render the page fine but cannot write to `links.js`.
