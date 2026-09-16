# Pacific seabed mining map

Interactive map of ISA seabed exploration contracts in the Pacific, with pins for
the five reporting locations and lines from each to the seabed that country holds.

**Live:** https://YOURNAME.github.io/seabed-mining-map/

## Setup — 3 steps

1. **Add your Mapbox token.** Open `config.js` and set `token` to a **public**
   token (starts `pk.`). Never put an `sk.` token here — this file is served to
   every visitor.
2. **Restrict the token.** In account.mapbox.com → Access tokens → your token →
   URL restrictions, add `https://YOURNAME.github.io/*` plus any site that
   iframes the map. Without this, anyone can copy the token and spend your quota.
3. **Turn on Pages.** Settings → Pages → Source: *Deploy from a branch* →
   `main` / `/ (root)`. First build takes a minute or two.

No GitHub Actions needed. This is plain HTML, CSS and JavaScript — Pages serves
the files as they are. Push a change, it's live in about a minute.

## Files

| file | what it is |
|---|---|
| `index.html` | the whole map — layout, legend, story rail, cards |
| `config.js` | token, style URL, opening view. The only file you must edit |
| `data/stories.geojson` | the five stories: reporter, location, headline, dek, url, image |
| `data/leader_lines.geojson` | curved lines from pin to contract |
| `data/leader_offmap.json` | contracts counted in the cards but outside the Pacific frame |
| `.nojekyll` | stops Pages running Jekyll over the files |

Everything else — bathymetry, contract polygons, pins, leader lines, labels —
comes from the hosted Mapbox style and its tileset (`earthjournalism.mua4n1`),
so this repo stays tiny.

## Adding a headline, link or image

Edit `data/stories.geojson`. Each story has:

```json
"headline": "HEADLINE TK",
"dek": "one-line summary shown in the card",
"url": "",       // empty -> the card shows "Story link TK"
"image": ""      // empty -> the card shows a dashed placeholder
```

Fill `url` and `image` and the card wires itself up. `image` can be a full URL or
a path in this repo. No rebuild, no re-upload — just commit.

`TK` is newsroom shorthand for "to come". Search the repo for `TK` before launch.

## Embedding

```html
<iframe src="https://YOURNAME.github.io/seabed-mining-map/"
        title="Pacific seabed mining map" loading="lazy"
        style="width:100%;height:70vh;min-height:420px;max-height:760px;border:0"
        allowfullscreen></iframe>
```

Scroll is not trapped: a plain scroll passes through to the page, and zooming
needs Ctrl (or ⌘) + scroll, two fingers on touch. The page also posts its height
to the parent as `{type:'seabedmap:height'}` if you want to auto-size.

## Sources and permissions

- **Exploration contracts, reserved areas, APEIs** — International Seabed
  Authority, from its ArcGIS feature service, retrieved 16 September 2026.
  ISA's terms permit news use **with credit, and on condition ISA is advised** —
  email news@isa.org.jm before publication.
- **Bathymetry** — GEBCO, via `mapbox.mapbox-bathymetry-v2`.
- **Basemap** — © Mapbox, © OpenStreetMap.

## Rebuilding the data

Source GeoJSONs, the tiling pipeline and `leaders.py` live in the working folder,
not here. This repo holds only what the page needs to run. To change which
contracts a story links to, edit `LINKS` in `leaders.py`, re-run it, re-tile, and
upload a new tileset version.
