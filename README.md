# Gitails-DRAWINGS — drawing repository (source of truth: DXF)

Layout maintained by `arcdiff extract` from Gitails-Tools:

```
drawings/D-101.dxf
drawings/test-01.dxf
drawings/test-02.dxf
state/D-101.jsonl       # canonical state, committed
state/D-101.idmap.json  # persistent element IDs, committed — never regenerate
```

## Drafter workflow (single command, runs locally)

```bash
arcdiff extract drawings/D-101.dxf --state-dir state --config-dir ../Gitails-Tools/config
git add drawings state
git commit -m "Rev C: glazing 3mm -> 2mm"
```

* DWG→DXF export to ASCII DXF R2018+ before commit (`.dwg` may sit beside but is never parsed).
* `state/*.jsonl` + `state/*.idmap.json` are committed. `index.sqlite` (Part 4) is derived, never committed.
* If CI reports `state/*.jsonl` out of sync, you forgot to run extract.
