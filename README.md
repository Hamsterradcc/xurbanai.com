# xurbanai.com

Experimental platform for urban AI models, agents, benchmarks, and prototypes. Static site served by GitHub Pages at <https://xurbanai.com>.

## Site map

| Page | Role |
| --- | --- |
| `index.html` | Entry point: the three registries, the common entry standard, principles, release process |
| `models.html` | Registry 01 — released models, plus the model card fields |
| `agents.html` | Registry 02 — released agents, plus the agent card fields |
| `benchmarks.html` | Registry 03 — released benchmarks, plus the benchmark card fields |
| `standards.html` | The full entry standard: common fields, per-registry fields, release checklist, status and versioning, out of scope |
| `404.html` | Not-found page (served by GitHub Pages) |

Shared assets: `assets/site.css` (one stylesheet for every page) and `assets/site.js` (mobile navigation and registry rendering). No build step and no external dependencies.

## Registry data

`data/registry.json` is the single source of truth for entries. Each registry page renders its list from this file; the index shows entry counts. With no entries the pages show their built-in empty state, so the site works without the file or without JavaScript.

```json
{
  "models": [],
  "agents": [],
  "benchmarks": [
    {
      "id": "example-task",
      "title": "Example task",
      "summary": "One sentence on what the task measures.",
      "status": "released",
      "version": "1.0.0",
      "released": "2026-01-01",
      "url": "entries/benchmarks/example-task.html",
      "tags": ["city", "modality"]
    }
  ]
}
```

- `status` is one of `draft`, `released`, `deprecated`. Drafts are kept in the file but not shown.
- `url` points at the entry card. Reserve `entries/<registry>/<id>.html` for cards hosted here; external cards may link out.
- Fields the card must contain are listed on `standards.html`.

## Editing

Pages share the same header, footer, and `<head>` block. When changing them, change every page. Navigation marks the current page with `aria-current="page"`.
