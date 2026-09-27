# bedroom-data

Room measurements and candidate furniture for one bedroom, laid out with the
[furniture-layout](../furniture-layout) tool. This project holds only data — no code.

```
data/                                 hand-written input (committed)
  room_layout.json                    room size, openings, surface colors
  material_colors.json                material/finish name -> hex
  furniture/<vendor>/<item>.json      one file per product
  rugs/<size>/<item>.json             one file per rug, e.g. rugs/9x12/
output/                               generated, git-ignored
  furniture_placements.json           where each piece sits (written by the app)
  photos/<product path>/              product photos (written by npm run photos)
```

Files under `furniture/` and `rugs/` are usually *alternatives* for the same spot: each is a
product whose count starts at 0, and the app's −/+ stepper says how many are in the room.

## Running

```
node C:\git\furniture-layout\src\server.js C:\git\bedroom-data\data --open
```

With `DATA_DIR` set to `C:\git\bedroom-data\data`, `npm start` in the furniture-layout repo does
the same thing. Placements are written to `output/` automatically — that folder is ignored, so the
layout isn't versioned here; don't hand-edit `furniture_placements.json`.

Product photos (optional, needs internet):

```
node C:\git\furniture-layout\src\fetch-photos.js --data C:\git\bedroom-data\data
```

## File formats

Documented in [furniture-layout/CLAUDE.md](../furniture-layout/CLAUDE.md) — dimensions, units,
types, materials, and the placement/coordinate conventions.
