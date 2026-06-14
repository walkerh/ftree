# rtree

A lightweight directory tree utility that displays file permissions, sizes, and ownership in the terminal.

## Features

- **Tree view** with recursive directory traversal
- **File permissions** (Unix mode) and file sizes in human-readable format
- **Optional user/group columns** to show file ownership
- **Symlink support** with target display
- **Smart directory stubbing** to avoid verbose output (.git, bare repos, .venv)
- **Customizable ignore patterns** to hide files and directories
- **Built-in filters** for common clutter (.DS_Store, __pycache__, caches, checkpoints, emacs backups)
- **Automatic paging** when output exceeds terminal height

## Installation

This is a uv script. You'll need [uv](https://docs.astral.sh/uv/) installed.

```bash
uv run rtree --help
```

Or add the script to your PATH for direct execution:

```bash
chmod +x rtree
export PATH="$PATH:$(pwd)"
```

## Usage

### Basic usage

```bash
rtree          # Show tree of current directory
rtree /path    # Show tree of specific directory
```

### Options

- `-u, --user-group` — Include user and group columns
- `-I, --ignore GLOB` — Add additional glob pattern to hide (repeatable)
- `--no-pager` — Disable automatic paging
- `-h, --help` — Show help message

### Examples

```bash
# Show current directory with permissions and sizes
rtree

# Show specific directory with user/group info
rtree -u /tmp

# Hide a specific directory
rtree -I "*.egg-info" -I "build"

# Combine options
rtree -u -I "*.log" /path/to/project
```

## Default ignore patterns

The following are hidden entirely from output:

| Pattern | What it hides |
|---|---|
| `.DS_Store` | macOS metadata files |
| `__pycache__` | Python bytecode cache directories |
| `*.egg-info` | Python packaging metadata |
| `*.pyc`, `*.pyo` | Legacy Python bytecode files |
| `.*_cache` | Tool caches (`.mypy_cache`, `.ruff_cache`, `.pytest_cache`, …) |
| `.cache` | Generic hidden cache directory |
| `.*_checkpoints` | Notebook and model checkpoints (`.ipynb_checkpoints`, …) |
| `*~` | Emacs backup files |
| `.tmp*` | Temporary hidden files and directories |
| `Icon` (0 bytes) | macOS custom folder icon resource file (`Icon\r`) |

## Stubbed directories

Stubbed directories appear in the tree but are not expanded. This confirms they
exist without cluttering the output.

| Entry | Type | What it stubs |
|---|---|---|
| `.venv` | exact name | Python virtual environment |
| `trash`, `.trash` | exact name | Trash directories |
| `*.git` | glob | Git metadata (`.git`) and bare repos (`project.git`) |
| `*tmp` | glob | Any directory whose name ends in `tmp` |

The `*.git` glob stubs both the `.git` directory inside a working tree and
bare-clone repositories, which are conventionally named `repo.git`.

## Dependencies

- `rich` — Terminal formatting and colors

## Output format

Columns displayed:

1. **Permissions** — Unix file mode (e.g., `-rw-r--r--`, `drwxr-xr-x`)
2. **User/Group** (optional, with `-u`) — File owner and group
3. **Size** — Human-readable file size (— for directories)
4. **Name** — File or directory name (directories shown with trailing `/`)
5. **Symlink target** (if applicable) — What the symlink points to

## Colors and styles

- **Blue bold** — Directories
- **Cyan** — Sizes
- **Green** (with `-u`) — User and group names
- **Dim blue bold** — Stubbed directories
- **Italic** — Symlinks
- **Dim** — Permissions
