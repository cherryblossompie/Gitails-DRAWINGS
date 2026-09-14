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

## Search (needs Gitails-Tools installed)

```bash
pip install "git+https://github.com/cherryblossompie/Gitails-Tools.git"
arcdiff index --repo . --db index.sqlite
# brief query: every detail ever glazed 3mm + which revision changed it:
arcdiff find --db index.sqlite --material glass --value 3 --ever
arcdiff history <element_id> --db index.sqlite
# static page — open in browser, no server:
arcdiff report --db index.sqlite --html report.html
```

On pull requests, CI posts before/after values per drawing, flags fuzzy
matches for review, and fails when `state/*.jsonl` is out of sync.
