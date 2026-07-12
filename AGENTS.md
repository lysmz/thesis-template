# Thesis Template — LLM Context

This file is a context guide for AI coding assistants (LLMs) working on this repository.

## Repository Overview

This is a **LaTeX thesis/expose template** for students at TH Nürnberg Georg Simon Ohm (Faculty of Computer Science). The template produces PDF/A-2b compliant documents.

This is a **submodule** of the parent project `BA-Studyplan/` (Joshua Ohans' bachelor thesis on transforming an AI-generated web prototype into a secure Electron desktop app). The parent `LLM_CONTEXT.md` (`../LLM_CONTEXT.md`) contains the full project context — always consult it when writing thesis content.

- **Main document**: `document.tex` (thesis or report)
- **Expose sub-project**: `expose/expose.tex` (thesis proposal, max 2 pages)
- **Class file**: `ohm.cls`
- **Bibliography**: `refs.bib` (BibTeX, shared between document and expose)
- **Glossary/Acronyms**: `glossary.tex`

## Build System

- **Make** (requires WSL2 on Windows): `wsl make` builds the main document, `wsl make expose` builds the expose, `wsl make clean` removes build artifacts.
- **WSL2 is required** on Windows — TeX Live must be installed inside WSL2. `make` is not available on the Windows host.
- **VS Code** (recommended on Windows): LaTeX Workshop extension with settings in `.vscode/settings.json`. Launch VS Code from WSL2 (`wsl code .`) for full toolchain support.
- The build uses `pdflatex -shell-escape` for `minted` (code listings) and `svg` support.

### Build Verification Workflow

When verifying that `.tex` changes compile correctly:

1. Delete the old PDF: `wsl rm -f document.pdf`
2. Clean auxiliary files: `wsl make clean`
3. Rebuild: `wsl make` (or `wsl make expose` for the expose)
4. Check output for errors — LaTeX stops on error with a `!` prompt, warnings are normal.
5. If the build succeeds, `document.pdf` (or `expose/expose.pdf`) is regenerated.

## Project Structure

```
├── document.tex          # Main entry point
├── ohm.cls               # Document class
├── refs.bib              # Shared bibliography
├── glossary.tex           # Acronyms & glossary entries
├── content/               # Thesis content chapters
│   ├── 1_einleitung.tex
│   ├── 2_hintergrund.tex
│   ├── 3_methodik.tex
│   ├── 4_analyse.tex
│   ├── 5_entwurf.tex
│   ├── 6_evaluation.tex
│   ├── 7_diskussion.tex
│   ├── 8_fazit.tex
│   └── a1_supplemental.tex
├── drafts/                # Working notes and ideation
│   ├── 01_einleitung/
│   ├── 02_hintergrund/
│   ├── 03_methodik/
│   ├── 04_analyse/
│   ├── 05_entwurf/
│   ├── 06_evaluation/
│   ├── 07_diskussion/
│   ├── 08_fazit/
│   └── README.md
├── expose/                # Expose sub-project
│   ├── expose.tex
│   ├── content/
│   └── README.md
├── figures/               # SVG and PNG figures
├── code/                  # Source code for listings
├── format/                # Formatting files (e.g. xmpdata for PDF/A)
└── .github/instructions/  # GitHub Copilot instructions (prompt files)
```

## Formatting Conventions

- **One sentence per line** in `.tex` files — this produces clean git diffs.
- **UTF-8 encoding** for all source files.
- **IEEE capitalization style** for headings.
- Figures should be SVG (preferred) or PNG. Use `.drawio.svg` for editable diagrams.

## LaTeX Conventions

Use the following patterns when writing thesis content.

### Tables

```latex
\begin{table}[ht]
  \centering
  \begin{tabular}{l c r}
    \hline
    \textbf{Header1} & \textbf{Header2} & \textbf{Header3} \\
    \hline
    Value1 & Value2 & Value3 \\
    \hline
  \end{tabular}
  \caption{Caption text}
  \label{tab:label}
\end{table}
```

### Lists (itemize)

```latex
\begin{itemize}
  \item First item
  \item Second item
\end{itemize}
```

### Lists (enumerate)

```latex
\begin{enumerate}
  \item First item
  \item Second item
\end{enumerate}
```

### Code listings (inline)

```latex
\begin{listing}[htbp]
  \caption{Caption}
  \label{lst:label}
  \begin{minted}{typescript}
    const x = 1;
  \end{minted}
\end{listing}
```

### Code listings (from file)

```latex
\begin{listing}[htbp]
  \caption{Caption}
  \label{lst:label}
  \inputminted{typescript}{code/file.ts}
\end{listing}
```

### Figures (SVG)

```latex
\begin{figure}[ht]
  \centering
  \includesvg[width=0.8\textwidth]{figures/name.drawio.svg}
  \caption{Caption}
  \label{fig:label}
\end{figure}
```

### Figures (PNG)

```latex
\begin{figure}[ht]
  \centering
  \includegraphics[width=0.8\textwidth]{figures/name.png}
  \caption{Caption}
  \label{fig:label}
\end{figure}
```

### Citations

```latex
\cite{key}
```

### Footnotes

```latex
\footnote{Footnote text.}
```

### Cross-references

```latex
\ref{ch:label}
\ref{sec:label}
\ref{fig:label}
\ref{tab:label}
\ref{lst:label}
```

### Acronyms / Glossary

```latex
\gls{aci}  % first use: long (short), subsequent: short
\Gls{aci}  % capitalized
```

### Inline formatting

```latex
\textbf{bold}
\texttt{monospace}
\url{https://...}
```

### Language-conditional text

```latex
\ifde German text\else English text\fi
```

## Available Document Class Options (in `document.tex`)

| Option       | Values                        | Description                    |
|-------------|-------------------------------|--------------------------------|
| Bibliography | `ieee`, `wmaainf`            | BibTeX style                   |
| Type         | `master`, `bachelor`, `report`| Document type                  |
| Language     | `german`, `english`           | UI language                    |
| Cover        | `singlepage`, `doublepage`    | Cover page layout              |

## LaTeX Commands Reference

- `\cite{key}` — Cite from `refs.bib`
- `\gls{key}` / `\Gls{key}` / `\glspl{key}` — Glossary/acronym usage
- `\includesvg[width=...]{figures/...}` — Include SVG figures
- `\begin{minted}{lang}...\end{minted}` — Code listings (needs `-shell-escape`)
- `\inputminted{lang}{code/file}` — Include code from file
- `\footnote{text}` — Footnotes
- `\ref{label}` — Cross-references

## Thesis-Specific Context

This thesis repository is a **LaTeX submodule** inside the parent project `BA-Studyplan/`.
When writing thesis content, reference the parent `LLM_CONTEXT.md` (`../LLM_CONTEXT.md`) for complete project context, including:

- **Author**: Joshua Ohans
- **Thesis title**: *Vom KI-Prototypen zur sicheren Desktop-Applikation* — Transforming an AI-generated fullstack web prototype into a secure Electron desktop app
- **Methodology**: Design Science Research (DSR) — every design cycle must be documented with plan, implementation, and reflection
- **Research questions (FF1–FF4)**: Covering IPC transformation, refactoring patterns, SQLite migration, and security hardening
- **Tech stacks**: Current (web prototype: React 19, Express 5, PostgreSQL, Drizzle ORM) vs. Target (Electron, SQLite, IPC, Context Isolation)

### Chapter-to-Source Mapping

| Chapter | File | Primary Sources (parent `BA-Studyplan/`) |
|---|---|---|
| 1 Einleitung | `content/1_einleitung.tex` | `LLM_CONTEXT.md` (Project Identity, Research Questions), expose PDFs in `Organisatorisches/Expose/` |
| 2 Theoretischer Hintergrund | `content/2_hintergrund.tex` | `refs.bib` references, `LLM_CONTEXT.md` Tech Stacks, Architecture Decisions |
| 3 Methodisches Vorgehen | `content/3_methodik.tex` | `LLM_CONTEXT.md` DSR Methodology section |
| 4 Analyse und Problemanalyse | `content/4_analyse.tex` | `.agents/artifacts/` DSR design cycle documentation |
| 5 Entwurf und Entwicklung | `content/5_entwurf.tex` | `.agents/artifacts/` DSR design cycle documentation |
| 6 Evaluation und Demonstration | `content/6_evaluation.tex` | `.agents/artifacts/` DSR design cycle documentation |
| 7 Diskussion | `content/7_diskussion.tex` | `.agents/memory/MEMORY.md`, findings from chapters 4-6 |
| 8 Fazit und Ausblick | `content/8_fazit.tex` | All prior chapters, `LLM_CONTEXT.md` open questions |

### Where to Find Source Material for Thesis Content

| Thesis Section | Source Location |
|---|---|
| Problem statement, motivation | `../LLM_CONTEXT.md` Project Identity, Research Questions |
| Background / Related Work | References in `refs.bib` |
| Methodology (DSR) | `../LLM_CONTEXT.md` DSR Methodology section |
| Transformation approach | `../LLM_CONTEXT.md` Transformation Roadmap, Architecture Decisions |
| Implementation details & code | `.agents/artifacts/` DSR design artifacts with plans, attempts, reflections |
| Technical architecture | `../LLM_CONTEXT.md` Tech Stacks, Architecture Decisions |
| Security hardening | `../LLM_CONTEXT.md` Electron Desktop target (IPC, Context Isolation, CSP) |
| Gotchas / known issues | `.agents/memory/MEMORY.md` |

### Important Paths (relative to parent `BA-Studyplan/`)

| Path | Content |
|---|---|
| `.agents/artifacts/` | DSR design cycle documentation — primary source for chapters 4-5 |
| `.agents/memory/MEMORY.md` | Persistent notes, gotchas, conventions |
| `Organisatorisches/Expose/` | Expose PDFs (authoritative thesis spec) |

## Rules for LLMs

1. **Never modify `ohm.cls`** unless explicitly asked — it's a shared template class.
2. **Never commit directly** — only stage and commit when told to do so.
3. **Always read existing files** before editing them.
4. **Follow the one-sentence-per-line rule** when writing or editing `.tex` files.
5. **Preserve document class options** in `document.tex` unless the user asks to change them.
6. **Acronyms go in `glossary.tex`** using `\newacronym{key}{Short}{Long}`.
7. **Bibliography entries go in `refs.bib`** — the expose shares this file.
8. **Verify builds** when changing `.tex` files — delete the old PDF, clean auxiliary files (`wsl make clean`), rebuild (`wsl make` / `wsl make expose`), and check for build errors.
9. **For the expose**, keep content concise and at most 2 pages.
10. **Use `.drawio.svg` format** for new diagrams so they remain editable.
11. **Do not add trailing whitespace** to any file.
12. **Reference existing conventions** in neighboring files before writing new content.
13. **Drafts go in `drafts/` as Markdown** — only `.tex` files in `content/` are part of the build.
14. **Available convenience commands** defined in `ohm.cls`: `\ua` (und andere), `\zB` (zum Beispiel), `\dahe` (das heißt), `\bzw` (beziehungsweise), `\bzgl` (bezüglich), `\eg` (e.g.), `\ie` (i.e.), `\wrt` (w.r.t.), `\etal` (et al.). Use these in thesis text.
