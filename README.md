# DeadCode

CLI tool to detect and auto-remove unused exports, dead routes, orphaned CSS, and unreferenced components in TypeScript/React/Next.js projects.

[![CI](https://github.com/Coding-Dev-Tools/deadcode/actions/workflows/test.yml/badge.svg)](https://github.com/Coding-Dev-Tools/deadcode/actions/workflows/test.yml)
[![PyPI](https://img.shields.io/pypi/v/deadcode)](https://pypi.org/project/deadcode/)

## Features

- **Unused exports** — find functions, classes, constants, types that are exported but never imported
- **Dead routes** — find Next.js pages/api routes with no internal links
- **Orphaned CSS** — find CSS classes defined but never used in JSX
- **Unreferenced components** — find React components defined but never imported
- **Safe auto-removal** — preview with `--dry-run` before making changes
- **Monorepo support** — handles large projects efficiently
- **CI integration** — JSON output for automated pipelines

## Installation

```bash
pip install deadcode
```

## Usage

### Scan for dead code

```bash
deadcode scan
deadcode scan -p /path/to/project
deadcode scan --json-output
deadcode scan -c unused_export   # Filter by category
```

### Preview removal (dry run)

```bash
deadcode remove --dry-run
```

### Remove dead code

```bash
deadcode remove
deadcode remove -c orphaned_css
```

### View stats

```bash
deadcode stats
```

## Categories

| Category | Description |
|----------|-------------|
| `unused_export` | Exported names never imported elsewhere |
| `dead_route` | Next.js routes with no internal links |
| `orphaned_css` | CSS classes not referenced in JSX |
| `unreferenced_component` | React components never imported |

## Ignore Patterns

Use `--ignore` to exclude paths (gitignore-style):

```bash
deadcode scan -i "generated/" -i "**/*.generated.ts"
```

Default ignores: `node_modules/`, `.git/`, `.next/`, `dist/`, `build/`, `public/`, `static/`

## CI Integration

```bash
deadcode scan --json-output > deadcode-report.json
```

Exit with findings:

```bash
deadcode scan -j | python3 -c "import sys,json; d=json.load(sys.stdin); sys.exit(1 if d['findings'] else 0)"
```

## License

MIT
