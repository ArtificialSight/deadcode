# DeadCode

**Detect and remove unused exports, dead routes, orphaned CSS, and unreferenced components in TypeScript/React/Next.js projects.**

[![PyPI](https://img.shields.io/pypi/v/deadcode)](https://pypi.org/project/deadcode/)
[![Python](https://img.shields.io/pypi/pyversions/deadcode)](https://pypi.org/project/deadcode/)
[![License](https://img.shields.io/pypi/l/deadcode)](https://github.com/Coding-Dev-Tools/deadcode/blob/main/LICENSE)
[![CI](https://github.com/Coding-Dev-Tools/deadcode/actions/workflows/test.yml/badge.svg)](https://github.com/Coding-Dev-Tools/deadcode/actions/workflows/test.yml)

**Why DeadCode?** Every TypeScript/React codebase accumulates dead code — exports nobody imports, page components replaced but never deleted, CSS classes refactored out but still sitting in `.module.css` files. ESLint catches unused variables but misses the structural decay: orphaned exports bloat your bundles, stale routes confuse new teammates, and orphaned styles silently accumulate. DeadCode scans your entire project with full TypeScript compiler API analysis and reports exactly what's safe to remove — with a dry-run preview mode so you never delete something you need.

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
- **TypeScript compiler API** — uses the real TS compiler for 100% accurate parsing, not regex heuristics
- **Monorepo support** — handles large projects efficiently with ignore patterns
- **CI integration** — JSON output for automated pipelines and gating

## Ignore Patterns

```bash
deadcode scan -i "generated/" -i "**/*.generated.ts"
```

Default ignores: `node_modules/`, `.git/`, `.next/`, `dist/`, `build/`, `public/`, `static/`

## Pricing

DeadCode is one of eight tools in the Revenue Holdings suite. One license covers all CLI tools.

| Plan | Price | Best For |
|------|-------|----------|
| **Free** | $0 | Individual devs, OSS — CLI only, rate-limited |
| **DeadCode Individual** | **$12/mo** ($10 billed annually) | Professional devs — unlimited scans, auto-removal, CI integration |
| **Suite (all 8 tools)** | **$49/mo** ($39 billed annually) | Full Revenue Holdings toolkit — 40% savings |
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
  <sub>Part of <a href="https://coding-dev-tools.github.io/revenueholdings.dev/">Revenue Holdings</a> — CLI tools built by autonomous AI.</sub>
</p>

## CI/CD Integration

```bash
# Generate report for CI
deadcode scan --json-output > deadcode-report.json

# Fail CI if any dead routes found
deadcode scan -c dead_route && exit 1

# Track dead code trends over time
deadcode scan --json-output > baseline-$(date +%Y-%m-%d).json
```

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
