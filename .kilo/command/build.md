# Build Commands

## Build main document

```
make
```

## Build expose

```
make expose
```

## Clean build artifacts

```
make clean
```

## Build with VS Code

Open the repository in VS Code and use the LaTeX Workshop extension (Ctrl+Shift+P → LaTeX Workshop: Build with recipe). The `.vscode/settings.json` is already configured for `pdflatex -shell-escape`.

## Prerequisites

- TeX Live (with `minted`, `svg`, and `biblatex` packages)
- Python (for `pygmentize`, required by `minted`)
- Inkscape (for SVG support)
- Make (or use VS Code LaTeX Workshop)

On Windows, use WSL2 with TeX Live installed. See the [README](../README.md) for more details.
