# rtree

A lightweight directory tree utility that displays file permissions, sizes, and ownership in the terminal.

## Features

- **Tree view** with recursive directory traversal
- **File permissions** (Unix mode) and file sizes in human-readable format
- **Optional user/group columns** to show file ownership
- **Symlink support** with target display
- **Smart directory stubbing** to avoid verbose output (.git, .venv)
- **Customizable ignore patterns** to hide files and directories
- **Built-in filters** for common clutter (.DS_Store, __pycache__, *.egg-info)

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

The following patterns are hidden by default:

- `.DS_Store` (macOS metadata)
- `__pycache__` (Python cache)
- `*.egg-info` (Python packaging)

## Stubbed directories

The following directories are shown but not expanded (useful to avoid clutter):

- `.git` — Version control
- `.venv` — Python virtual environment

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
