# xurbanai.com

Experimental platform for urban AI models, agents, benchmarks, and prototypes. Static site served by GitHub Pages at <https://xurbanai.com>. No build step is needed to serve it.

## Site map

| Page | Role |
| --- | --- |
| `index.html` | Entry point: the three registries, the common fields, principles, release process |
| `models.html` | Registry 01: released models and the model card fields |
| `agents.html` | Registry 02: released agents and the agent card fields |
| `benchmarks.html` | Registry 03: released benchmarks and the benchmark card fields |
| `entry.html` | One entry, rendered from the data: `entry.html?registry=<key>&id=<id>` |
| `standards.html` | The full entry standard: fields, release checklist, status and versioning, out of scope |
| `404.html` | Not-found page |

Shared assets: `assets/site.css`, `assets/site.js`, and `assets/og.png` (social preview card).

## How the standard is kept in one place

`data/schema.json` is the single source of truth. Everything else follows from it.

```
data/schema.json ──► scripts/build.py ──► field lists in index, standards and registry pages
       │                              └─► .github/ISSUE_TEMPLATE/<registry>-entry.yml
       └──────────► scripts/validate.py ──► checks data/registry.json and data/examples/
                    assets/site.js ───────► reads both files to render lists and entry pages
```

- **Change a field**: edit `data/schema.json`, then run `python scripts/build.py`. It rewrites the content between `<!-- build:... -->` markers in the pages and regenerates the issue forms. Do not edit those blocks by hand.
- **Add an entry**: add an object to the right array in `data/registry.json`, then run `python scripts/validate.py`. Entries with `status: "draft"` are kept but not shown.
- **CI**: `.github/workflows/validate.yml` runs both scripts with `--check` on every push and pull request.

### Entry fields

| Field | Required when | Notes |
| --- | --- | --- |
| `id`, `title`, `summary`, `status` | always | `id` is a lowercase slug used in the URL; `status` is `draft`, `released` or `deprecated` |
| `version`, `released` | released, deprecated | semantic version; ISO date |
| `authors`, `license`, `links`, `citation`, `card` | released | `links` is a list of `{label, href}`; the first link is the primary button |
| `card` fields | released | every field defined for the registry; placeholders such as "TBD" or "to follow" fail validation |
| `updated`, `tags`, `url`, `replaced_by` | optional | `url` points to an external card instead of `entry.html`; `replaced_by` names the id that supersedes a deprecated entry |

### Preview and examples

`data/examples/registry.json` holds one complete example per registry. It is never listed on the site, but it renders on the entry page for previewing the layout:

- `entry.html?registry=models&id=example-model&src=example`
- `entry.html?registry=benchmarks&id=example-benchmark&src=example`

Local preview needs a static server, because the pages fetch JSON:

```
python -m http.server 8000
```

## Proposals and corrections

Issue forms are generated from the schema, one per registry, plus a correction form. Blank issues are disabled; general questions go to the City Computing Collective discussions.

## Editing

Pages share the same header, footer, and `<head>` block. When changing them, change every page. The footer lists the four sites of the network in the same order on every site: Urban Science AI Lab, UrbanProbe, XUrbanAI, City Computing Collective.
