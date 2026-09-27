# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Data-only project (no code, no build, no tests): the measurements of one bedroom plus candidate
furniture and rugs, consumed by the separate **furniture-layout** program at `C:\git\furniture-layout`.
The authoritative file-format spec lives in `C:\git\furniture-layout\CLAUDE.md` — read it before
adding or changing fields. Changes to how data is *interpreted* belong in that repo, not here.

## Running the layout app against this data

```
node C:\git\furniture-layout\src\server.js C:\git\bedroom-data\data --open
node C:\git\furniture-layout\src\fetch-photos.js --data C:\git\bedroom-data\data   # optional, needs internet
```

(`npm start` / `npm run photos` in furniture-layout do the same when `DATA_DIR` points here.) The
server watches `data/` and live-reloads the page when any JSON changes, so editing a file is the
way to "test" it. When checking things, pass `-o`/`OUTPUT_DIR` pointing at a scratch folder so the
real `output/furniture_placements.json` isn't overwritten.

## Layout

- `data/room_layout.json` — room size (inches, southwest origin, x = east, y = north), openings
  given as `start`/`end` points on a wall, and named surface colors.
- `data/material_colors.json` — material/finish name → hex (case-insensitive). When a product uses
  a new finish/color name, add it here rather than putting hex values in product files.
- `data/furniture/<vendor>/<item>.json` and `data/rugs/<size>/<item>.json` — one file per product.
  Files are usually *alternatives* for the same spot; each starts at count 0 and the app's stepper
  decides how many are in the room.
- `output/` — generated and git-ignored (`furniture_placements.json`, `photos/`). Never hand-edit.
  Placement ids are the path under `furniture/`/`rugs/` minus `.json`, so renaming a file loses its
  saved placement unless the file name stays the same (a move between folders with the same file
  name is adopted automatically).

## Product file conventions

Copy an existing file of the same kind as a template. Fields the app actually reads:
`units`, `furniture.dimensions` (`width` × `depth` for furniture, `width` × `length` for rugs),
`furniture.type` (drives default placement: beds → west wall, dressers/bookcases → east wall),
`furniture.manufacturer`, optional `furniture.size` / `furniture.shape: "round"` / `selected` /
`quantity`, `metadata.price.configured_price`, `materials` (color lookup for the 3D view), and
`metadata.product_url` / `source_variant_url` (photo scraping). Everything else — construction,
notes, the per-file `coordinate_system` block — is informational.

Existing files follow these habits: snake_case file names including collection, size and finish
(e.g. `hastings_9x12_ivory_taupe_rug.json`); a rug offered in two sizes gets one file per size
folder; `geometry.bounding_box.max` mirrors `dimensions`; prices carry `price_source` and
`price_as_of`, and `metadata.source_retrieved` records the date the specs were gathered.
