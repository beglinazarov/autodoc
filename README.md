# autodoc

**Documentation that keeps itself current.** `autodoc` is a Claude Code plugin that maintains a local documentation site
for your project and updates it automatically when a task is finished or the architecture changes, including schemas
and architecture diagrams.

> **Status: early development. Nothing in this repository is installable yet.** This README describes the design and the
> roadmap so you can follow the project, give feedback or contribute. Items are marked **planned** until they ship. See
> [Roadmap](#roadmap).

## Why

Project documentation rots because updating it is a separate chore that happens after the work, if at all. Coding agents
already know what just changed, so they can do the chore at the moment it matters: when a task is finalised, a schema
moves or a component is added. `autodoc` turns that into a repeatable, reviewable step instead of a one-off prompt.

## What it does (planned)

| Capability | What you get |
|---|---|
| **Local doc project** | A Next.js static-export site scaffolded in your repo (default `docs-site/`), with navigation by area, search and Mermaid rendering. Markdown is the source of truth; the site is a view of it. |
| **Automatic triggers** | A Claude Code hook runs when a task is finalised (and on architecture-relevant changes such as new modules, services, routes or schema files). It decides which pages are affected and updates only those. |
| **Auto schema** | Extracts schemas from what your project already has (OpenAPI / JSON Schema first; SQL migrations and ORM models after) and writes reference pages that cannot drift from the source. |
| **Architecture diagrams** | Generates and updates Mermaid diagrams (context, containers, data model, key flows) from the code structure and your notes, stored next to the pages that use them. |
| **Doc coverage and freshness** | A percentage of your project's endpoints, schema items, modules and config keys that a page documents, plus the pages whose source files changed since they were written. Shown in a status line and panel, and as JSON for CI. |
| **Reviewable by design** | Changes land on a branch or in a drafts folder as a normal diff. `autodoc` never silently rewrites published pages and never invents facts it cannot trace to the code or to a task. |
| **Private by default** | Everything runs locally. The generated site is static, so you can host it anywhere (for example behind your own access control). Nothing is sent anywhere except through your normal Claude Code session. |

## How it works (planned design)

1. **Skill (`autodoc`)**: the instructions Claude follows to read a change, map it to documentation pages and write the update, with a style guide and a "cite the source file" rule for every non-trivial claim.
2. **Hooks**: a task-finalised hook and an architecture-change hook call the skill with the diff and the task summary.
3. **Docs project**: the scaffold (`docs-site/` by default) with a page tree, front matter conventions and a build script. Pages are plain Markdown plus Mermaid.
4. **Extractors**: small, replaceable scripts that turn a schema source into a reference page. New sources are plugins to the plugin.
5. **Config (`autodoc.config.json`)**: where the docs live, which paths count as architecture-relevant, which schema sources to read, and whether to write to a branch or a drafts folder.

## Installation

Not available yet. When the first release is tagged, the intended flow is the standard Claude Code plugin flow:

```text
/plugin marketplace add beglinazarov/autodoc
/plugin install autodoc@autodoc
```

Requirements (planned): Claude Code with plugin support, Node.js 20 or newer, and git. The repository will document the
exact commands here once they work. Until then, watch or star the repository to follow progress.

## Roadmap

- [ ] **0.1**: skill + docs-project scaffold + manual `/autodoc update`
- [ ] **0.2**: task-finalised hook, branch / drafts output, config file
- [ ] **0.3**: OpenAPI / JSON Schema reference pages
- [ ] **0.4**: doc coverage and freshness: an inventory of what your project exposes (endpoints, schema, modules, config keys), the percentage a page documents, and which pages are stale; shown in a Claude Code status line and panel and as `/autodoc coverage --json` for CI. It measures whether something is documented and cited, not whether the text is correct
- [ ] **0.5**: Mermaid architecture diagrams (context, containers, data model), plus an optional [draw.io MCP](https://github.com/jgraph/drawio-mcp) for hand-polished diagrams: the `.drawio` file and an exported SVG are committed next to the page, and automatic updates never overwrite a diagram a person edited. Off by default; the local server is the default, the hosted endpoint sends diagram content to a third party
- [ ] **0.6**: SQL migration and ORM schema extractors
- [ ] **1.0**: stable config, docs, examples and a test suite

## Contributing

Ideas, design feedback and issues are welcome while the design is still moving. Open an issue describing your project
type and what you would want kept up to date automatically. Code contributions: please open an issue first so the design
stays coherent.

## License

[MIT](LICENSE)
