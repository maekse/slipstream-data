# slipstream-data

Baked season snapshots for Slipstream. **Data only** — no application code lives
here.

Each `season-YYYY.json` is a single-file summary of one Formula 1 season: the
calendar with per-session times, results, qualifying, the championship standings
after each round, and each circuit's outline. Eleven seasons are published,
2016 through 2026, and they are rebuilt by a scheduled job and served over
GitHub Pages:

```
https://maekse.github.io/slipstream-data/season-2026.json
```

Only the season currently running is rebuilt on a schedule. A completed season
is baked once and then left alone, so its file is effectively immutable and
caches indefinitely.

## Freshness

The file carries its own `generatedAt`. If that timestamp stops advancing on the
live season, the publishing job has stopped — the file will still be served, and
it will still be internally consistent, so the timestamp is the only signal.

## Circuit outlines

Every round carries its circuit's shape under `circuit.outline`:

```json
"outline": { "path": "m12 576c-12 -9 …", "w": 887, "h": 1000 }
```

`path` is SVG path data using only `m`, `l`, `c` and `z` — no arcs, no
quadratics, no smooth curves. Arcs are converted to cubic Béziers when the file
is built, so a consumer needs no ellipse maths.

`w` and `h` are the path's own bounding box, in the path's units, and one of
them is always 1000. The path is already translated to that box's origin, so
fitting the box to a frame is the whole of the drawing code. This matters: the
source drawings are normalised into a square, so without the fit a wide circuit
would render at a fraction of a tall one's size — the calendar's aspect ratios
run from 0.34 (Montreal) to 2.75 (Miami).

**The shape is the layout that season raced, not the circuit's current one.**
Bahrain 2020 is a case worth knowing: rounds 15 and 16 were both held there, on
different configurations — the Grand Prix on the normal lap, the Sakhir Grand
Prix on the outer loop — and the two rounds carry different `path` values.

The field is optional. A circuit with no drawing omits it rather than emitting
null, and consumers should treat its absence as "draw nothing".

## Attribution

Race data is derived from the [Jolpica-F1 API](https://github.com/jolpica/jolpica-f1),
the community successor to Ergast. Circuit time zones are hand-authored rather
than derived, and audited against `timezonefinder`.

Circuit outlines are derived from [F1DB](https://github.com/f1db/f1db) and are
licensed [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). The geometry
is modified from the original: paths are rescaled, translated, converted to
cubics and rounded. **Anything redistributing these files carries the same
attribution requirement.**

This project is not associated with, endorsed by, or connected to Formula 1,
Formula One Licensing BV, the FIA, or any Formula 1 team. "F1" and "Formula 1"
are trademarks of Formula One Licensing BV.
