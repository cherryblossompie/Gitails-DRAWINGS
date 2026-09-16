# Gitails-DRAWINGS — drawing repository (source of truth: DXF)

Layout maintained by `gitail` from Gitails-Tools:

```
drawings/D-101.dxf            # or drawings/<project>/D-102.dxf
drawings/StageC/D-102.dxf
state/D-101.jsonl             # canonical state, committed (mirrors drawings/)
state/D-101.idmap.json        # persistent element IDs, committed — never regenerate
pdf/D-101.pdf                 # rendered preview, committed (mirrors drawings/)
pdf/StageC/D-102.pdf
images/StageC/site.png        # view-only references (png/jpg), committed, never parsed
```

Inputs: `.dxf` (parsed), `.dwg` (companion — export to DXF first, never parsed),
`.pdf` alone (view-only link, not searchable), `.png`/`.jpg` (view-only thumbnails,
never parsed). Search-result links always open the **PDF**; the DXF text source
is one extra click away.

## Drafter workflow (runs locally)

```bash
# .dwg? export to ASCII DXF R2018+ first (ODA File Converter or AutoCAD).
gitail extract drawings/StageC/D-102.dxf --state-dir state --drawings-dir drawings --config-dir ../Gitails-Tools/config
gitail render --drawings-dir drawings --pdf-dir pdf
git add drawings state pdf
git commit -m "Rev C: glazing 3mm -> 2mm"
```

* `state/*.jsonl` + `state/*.idmap.json` + `pdf/**/*.pdf` are committed. `index.sqlite` is derived, never committed.
* If CI reports `state/*.jsonl` out of sync, you forgot extract; if PDFs are stale, you forgot render.

## Search (needs Gitails-Tools installed)

```bash
pip install "git+https://github.com/cherryblossompie/Gitails-Tools.git"
gitail index --repo . --db index.sqlite
# brief query: every detail ever glazed 3mm + which revision changed it:
gitail find --db index.sqlite --material glass --value 3 --ever
gitail find --db index.sqlite --project StageC --material concrete
gitail history <element_id> --db index.sqlite
# static page — open in browser, no server (autocomplete + PDF links):
gitail report --db index.sqlite --html report.html --pdf-dir pdf
```

On pull requests, CI posts before/after values per drawing, flags fuzzy
matches for review, and fails when state or PDFs are out of sync.
