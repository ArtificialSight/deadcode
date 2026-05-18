# DeadCode

[![GitHub stars](https://img.shields.io/github/stars/Coding-Dev-Tools/deadcode?style=social)](https://github.com/Coding-Dev-Tools/deadcode/stargazers)
[![Awesome DevOps](https://img.shields.io/badge/Awesome_DevOps-Submitted-grey?logo=github)](https://github.com/wmariuss/awesome-devops)<!-- PR #433 -->

**Detect and remove unused exports, dead routes, orphaned CSS, and unreferenced components in TypeScript/React/Next.js projects.**

> ⭐ **Star this repo** if you care about bundle size — it helps other devs find DeadCode!

[![PyPI](https://img.shields.io/pypi/v/deadcode)](https://pypi.org/project/deadcode/)
[![Python](https://img.shields.io/pypi/pyversions/deadcode)](https://pypi.org/project/deadcode/)
[![License](https://img.shields.io/pypi/l/deadcode)](https://github.com/Coding-Dev-Tools/deadcode/blob/main/LICENSE)
[![CI](https://github.com/Coding-Dev-Tools/deadcode/actions/workflows/ci.yml/badge.svg)](https://github.com/Coding-Dev-Tools/deadcode/actions/workflows/ci.yml)
[![Open Source Alternative](https://img.shields.io/badge/Open_Source_Alternative-%E2%87%92-blue?logo=opensourceinitiative)](https://www.opensourcealternative.to/project/deadcode)
[![LibHunt](https://img.shields.io/badge/LibHunt-%E2%87%92-blue?logo=codeigniter)](https://www.libhunt.com/r/Coding-Dev-Tools/deadcode)
[![Awesome Python](https://img.shields.io/badge/Awesome_Python-%E2%87%92-blue?logo=python)](https://github.com/uhub/awesome-python)

**Why DeadCode?** Dead code silently bloats your bundle, slows your CI, and confuses new developers. Linters catch unused variables — but they miss orphaned React components, CSS classes nobody references, API routes with zero traffic, and exported functions called from nowhere. DeadCode finds all of it. It maps your entire dependency graph, marks what's reachable, and flags everything else for removal. Run it in CI to prevent dead code from shipping in the first place.

## Quick Start

```bash
pip install deadcode

# Scan current project
deadcode scan

# Scan a specific project
deadcode scan -p /path/to/project

# Preview removable dead code
deadcode remove --dry-run

# Remove confirmed dead code
deadcode remove
```

## Commands

### `deadcode scan`

Scan a TypeScript/React/Next.js project for all categories of dead code.

```bash
deadcode scan                          # Scan current directory
deadcode scan -p /path/to/project      # Scan specific project
deadcode scan --json-output            # Machine-readable JSON
deadcode scan -c unused_export         # Filter by category
deadcode scan -i "generated/"          # Ignore paths
```

### `deadcode remove`

Remove dead code after previewing. Always preview with `--dry-run` first.

```bash
deadcode remove --dry-run              # Preview changes (no writes)
deadcode remove                        # Apply removals
deadcode remove -c orphaned_css        # Remove only orphaned CSS
```

### `deadcode stats`

Quick overview of dead code in your project.

```bash
deadcode stats
```

## Categories

| Category | Description | Example |
|----------|-------------|---------|
| `unused_export` | Exported names never imported elsewhere | `export function oldHelper()` with zero consumers |
| `dead_route` | Next.js routes with no internal links | `app/legacy/page.tsx` no longer linked from navigation |
| `orphaned_css` | CSS classes not referenced in JSX | `.oldClass` in `styles.module.css` with zero usages |
| `unreferenced_component` | React components defined but never imported | A `<LegacyWidget>` component with no import sites |

## Features

- **Unused export detection** — finds functions, types, classes, interfaces, enums, and consts that are exported but never imported within your project
- **Dead route detection** — detects unreachable page components in Next.js App Router projects
- **Orphaned CSS detection** — finds CSS module classes that are defined but never referenced in TSX/JSX files
- **Safe auto-removal** — `--dry-run` preview mode shows exactly what will be deleted before making changes
- **Full-project AST analysis** — regex-based scanning covers export/import patterns, route detection, CSS class usage, and component references across your entire codebase
- **Monorepo support** — handles large projects efficiently with ignore patterns
- **CI integration** — JSON output for automated pipelines and gating

## Ignore Patterns

```bash
deadcode scan -i "generated/" -i "**/*.generated.ts"
```

Default ignores: `node_modules/`, `.git/`, `.next/`, `dist/`, `build/`, `public/`, `static/`

## Pricing

DeadCode is one of eight tools in the DevForge suite. One license covers all CLI tools.

| Plan | Price | Best For |
|------|-------|----------|
| **Free** | $0 | Individual devs, OSS — CLI only, rate-limited |
| **DeadCode Individual** | **$12/mo** ($10 billed annually) | Professional devs — unlimited scans, auto-removal, CI integration |
| **Suite (all 8 tools)** | **$49/mo** ($39 billed annually) | Full DevForge toolkit — 40% savings |
| **Team** | **$79/mo** ($63 billed annually) | Up to 5 devs — trend analytics, shared baselines, alerts |
| **Enterprise** | Custom | SSO, RBAC, compliance reports, dedicated support |

🔹 **No lock-in**: CLI works fully offline on the free tier — no telemetry, no phone-home.
🔹 **Annual billing**: Save 20%.

### Per-Tier Features

| Feature | Free | Individual | Suite | Team | Enterprise |
|---------|:----:|:----------:|:-----:|:----:|:----------:|
| CLI: scan, stats | ✓ | ✓ | ✓ | ✓ | ✓ |
| All 3 scanner categories | — | ✓ | ✓ | ✓ | ✓ |
| Auto-removal (`deadcode remove`) | — | ✓ | ✓ | ✓ | ✓ |
| Unlimited file scanning | — | ✓ | ✓ | ✓ | ✓ |
| CI/CD integration (JSON output) | — | ✓ | ✓ | ✓ | ✓ |
| Project trend baselines | — | — | — | ✓ | ✓ |
| Dashboard & analytics | — | — | — | ✓ | ✓ |
| Compliance reports | — | — | — | — | ✓ |
| RBAC / SSO / SAML / OIDC | — | — | — | — | ✓ |
| Priority support | Community | 24h | 24h | 8h | Dedicated |

---

<p align="center">
  <sub>Part of <a href="https://coding-dev-tools.github.io/devforge.dev/">DevForge</a> — CLI tools built by autonomous AI.</sub>
</p>

## CI/CD Integration

```bash
# Generate report for CI
deadcode scan --json-output > deadcode-report.json

# Fail CI if any dead routes found
deadcode scan -c dead_route --fail 1

# Fail CI if total findings exceed threshold
deadcode scan --fail 10

# Track dead code trends over time
deadcode scan --json-output > baseline-$(date +%Y-%m-%d).json
```

## Configuration (.deadcode.yml)

Create a `.deadcode.yml` file in your project root:

```yaml
# .deadcode.yml
ignore:
  - "generated/"
  - "**/*.generated.ts"
  - "src/legacy/"

categories:
  - unused_export
  - dead_route
  - orphaned_css
  - unreferenced_component

# Exit with code 1 if findings >= this number (for CI gating)
fail_threshold: 10
```

CLI flags override config file settings.

## Storage

- `.deadcode.yml` — project configuration (ignore patterns, categories)
- `deadcode-baseline.json` — saved scan results for trend tracking

## Roadmap

- [ ] VS Code extension with inline decorations showing dead code
- [ ] ESLint plugin integration with auto-fix
- [ ] Webpack/Rollup bundle analysis hooks
- [ ] MCP server for AI-assisted cleanup
- [ ] Incremental scanning with cache
- [ ] GitHub Actions annotator for PR comments

## License

MIT — see [LICENSE](LICENSE)
