# Thesis Template — LLM Context

This is a **LaTeX thesis/expose template** for students at TH Nürnberg Georg Simon Ohm (Faculty of Computer Science). The template produces PDF/A-2b compliant documents.

This is a **submodule** of the parent project `BA-Studyplan/`. The `thesis-context-loader` skill (`.agents/skills/thesis-context-loader/SKILL.md`) loads thesis-relevant context from the parent repo (project identity, RQs, methodology).

- **Main document**: `document.tex` (thesis or report)
- **Expose sub-project**: `expose/expose.tex` (thesis proposal, max 2 pages)
- **Class file**: `ohm.cls` — do not modify unless explicitly asked
- **Bibliography**: `refs.bib` (BibTeX, shared between document and expose)
- **Glossary/Acronyms**: `glossary.tex`
- **Build**: `wsl make` (requires WSL2 with TeX Live)

## Build & Verification

- `wsl make` builds main document, `wsl make expose`, `wsl make clean` removes artifacts
- Verification: delete old PDF, `wsl make clean`, `wsl make`, check output for errors
- LaTeX stops on error with a `!` prompt; warnings are normal
- VS Code (recommended): LaTeX Workshop extension, launch from WSL2 (`wsl code .`)

## Project Structure

```
├── document.tex          # Main entry point
├── ohm.cls               # Document class
├── refs.bib              # Shared bibliography
├── glossary.tex           # Acronyms & glossary entries
├── content/               # Thesis content chapters (built)
│   ├── 1_einleitung.tex
│   ├── 2_hintergrund.tex
│   ├── 3_methodik.tex
│   ├── 4_analyse.tex
│   ├── 5_entwurf.tex
│   ├── 6_evaluation.tex
│   ├── 7_diskussion.tex
│   ├── 8_fazit.tex
│   └── a1_supplemental.tex
├── drafts/                # Markdown drafts (NOT built)
│   ├── 01_einleitung/
│   ├── 02_hintergrund/
│   └── ... (03-08)
├── expose/                # Expose sub-project
├── figures/               # SVG and PNG figures
├── code/                  # Source code for listings
└── format/                # Formatting files (e.g. xmpdata for PDF/A)
```

## Rules for LLMs

1. **Never modify `ohm.cls`** unless explicitly asked — shared template class.
2. **Never commit directly** — only stage and commit when told to do so.
3. **Always read existing files** before editing them.
4. **Follow the one-sentence-per-line rule** when writing or editing `.tex` files.
5. **Preserve document class options** in `document.tex` unless the user asks to change them.
6. **Acronyms go in `glossary.tex`** using `\newacronym{key}{Short}{Long}`.
7. **Bibliography entries go in `refs.bib`** — the expose shares this file.
8. **Verify builds** when changing `.tex` — delete old PDF, `wsl make clean`, `wsl make`, check errors.
9. **For the expose**, keep content concise and at most 2 pages.
10. **Use `.drawio.svg` format** for new diagrams so they remain editable.
11. **Do not add trailing whitespace** to any file.
12. **Reference existing conventions** in `context/thesis-writing.md` before writing new content.
13. **Drafts go in `drafts/` as Markdown** — only `.tex` files in `content/` are part of the build.
14. **Convenience commands** defined in `ohm.cls`: `\ua`, `\zB`, `\dahe`, `\bzw`, `\bzgl`, `\eg`, `\ie`, `\wrt`, `\etal`. Use these in thesis text.

## LaTeX Templates (Quick Reference)

See `context/thesis-writing.md` for full conventions and command reference.

### Tables

```latex
\begin{table}[ht]
  \centering
  \begin{tabular}{l c r}
    \hline
    \textbf{Header} & \textbf{Header} & \textbf{Header} \\
    \hline
    Value & Value & Value \\
    \hline
  \end{tabular}
  \caption{}
  \label{tab:label}
\end{table}
```

### Lists

```latex
\begin{itemize}
  \item First item
\end{itemize}

\begin{enumerate}
  \item First item
\end{enumerate}
```

### Code Listings

```latex
\begin{listing}[htbp]
  \caption{}
  \label{lst:label}
  \begin{minted}{lang}
    code here
  \end{minted}
\end{listing}
\inputminted{lang}{code/file}
```

### Figures

```latex
\begin{figure}[ht]
  \centering
  \includesvg[width=0.8\textwidth]{figures/name.drawio.svg}
  \caption{}
  \label{fig:label}
\end{figure}
\includegraphics[width=0.8\textwidth]{figures/name.png}
```

### Cross-References & Formatting

```latex
\cite{key}    \footnote{text}    \ref{label}
\gls{key}     \textbf{bold}     \texttt{monospace}
\Gls{Key}     \ifde German\else English\fi
```

## Academic Writing & Style Guidelines

When drafting .tex content from drafts/, act as an **expert academic author** (*wissenschaftlicher Fließtext*).

1. **Inspiration, not a checklist** — treat bullets as guardrails, not sentence instructions. Do not translate bullets 1:1 into robotic mini-paragraphs.
2. **Synthesize and categorize** — group into paradigms, themes, or categories. Build a cohesive narrative (*roter Faden*) with logical transitions.
3. **Continuous prose over lists** — do NOT use bullets in final .tex output unless strictly necessary. Translate list-based ideas into comprehensive paragraphs.
4. **Academic register** — objective, precise, formal German. Avoid fluff and recurring filler words.
5. **Depth over breadth** — expand on the *why* and *how*, not just mentions that concepts exist.