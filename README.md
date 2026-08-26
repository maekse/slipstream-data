# slipstream-data

Baked season snapshots for Slipstream. **Data only** — no application code lives
here.

`season-2026.json` is a single-file summary of the Formula 1 season: the
calendar with per-session times, results, qualifying, and the championship
standings after each round. It is rebuilt by a scheduled job and served over
GitHub Pages at:

```
https://maekse.github.io/slipstream-data/season-2026.json
```

## Freshness

The file carries its own `generatedAt`. If that timestamp stops advancing, the
publishing job has stopped — the file will still be served, and it will still be
internally consistent, so the timestamp is the only signal.

## Attribution

Race data is derived from the [Jolpica-F1 API](https://github.com/jolpica/jolpica-f1),
the community successor to Ergast. Circuit time zones are hand-authored rather
than derived, and audited against `timezonefinder`.

This project is not associated with, endorsed by, or connected to Formula 1,
Formula One Licensing BV, the FIA, or any Formula 1 team. "F1" and "Formula 1"
are trademarks of Formula One Licensing BV.
