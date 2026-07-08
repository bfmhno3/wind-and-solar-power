# Agent Instructions: Wind and Solar Power

This file defines the environment, architecture, coding style, and operational constraints for AI agents working on the Wind and Solar Power project.

## 1. Project Structure & Naming

```bash
.
├── .gitignore / .latexmkrc / LICENSE / refs.bib
├── main.tex           # Master file (structural inclusions \include/\input only)
├── contents/          # Modular chapter files (e.g., chap_01.tex)
├── docs/              # Documentation and reference materials
├── data/              # Raw data files (.csv format)
├── figs/              # Generated vector graphics (.pdf, .eps)
├── scripts/           # Python calculation and plotting scripts
└── settings/          # format.tex (layout/macros) & packages.tex (imports)
```

- **Strict Naming (`snake_case`):** All filenames repository-wide (including `.tex`, `.py`, `.csv`, `.pdf`) must use strictly lowercase letters and underscores. No uppercase letters, spaces, or hyphens.
  - *Correct:* `solar_profile.csv`, `chap_01.tex`
  - *Incorrect:* `solar-profile.CSV`, `Chap_01.tex`

## 2. Environment Configuration

- **LaTeX Stack:** TeXLive 2026 | Build Tool: `latexmk` (configured via `.latexmkrc`). Zero-error tolerance policy; warnings are acceptable.
- **Python Stack:** Package Manager: `uv` | Execution: `uv run <script.py>` | Dependencies: `uv add <package>`

## 3. Code Style & Formatting Constraints

### Spacing & Punctuation

- **CJK/English Space:** Insert exactly one space between Chinese characters and English words/numbers (e.g., `共有 6 个`).
- **Cross-References:** Insert exactly one space *after* the `\ref{}` command, none before (e.g., `如\ref{fig:test} 所示`).
- **Quotes:** Use full-width Chinese quotation marks (`“` and `”`) inside Chinese text. Never use English straight quotes or standard LaTeX grave/acute accents in regular prose.

### Math & Units

- **Inline Math**: Only use `\(\)`，NEVER use `$$`, e.g., `\(f(X)\)`.

- **Units:** Use `siunitx` syntax with raw letters (e.g., `\qty{3}{m^3/s}`, `\qtyrange{3}{4}{\degreeCelsius}`, `\unit{MHz}`).
- **Chinese in Math:** Wrap Chinese text inside math environments using `\text{}` (e.g., `\(h_{\text{实}}\)`).

### Layout & Tables

- **Tables:** Use `tblr` (standard `table` floats) or `talltblr` (multi-page tables) from `tabularray`. Do not indent inside `\begin{}`/`\end{}` blocks. Do not manually align cells with `&`.
- **Equations:** Use `equation` (numbered single-line), `equation*` (unnumbered), or `aligned` inside `equation` (multi-line).

## 4. ElegantBook Style Guide

### Class Options Quick Reference

| **Option**   | **Values**                      | **Purpose**                                                  |
| ------------ | ------------------------------- | ------------------------------------------------------------ |
| `lang`       | `cn` (default) / `en`           | Standardizes structural names (Figures, Theorems, etc.) to target language. |
| `mode`       | `fancy` (default) / `simple`    | `fancy` uses colored `tcolorbox` for theorems; `simple` resembles plain `amsthm`. |
| `thmcnt`     | `chapter` (default) / `section` | Theorem numbering scope.                                     |
| `usesamecnt` | *(Boolean flag)*                | Unifies counters across definitions, theorems, and propositions. |
| `result`     | `answer` (default) / `noanswer` | `noanswer` globally hides `proof` and `solution` environments. |
| `toc`        | `twocol`                        | Enables two-column Table of Contents.                        |
| `marginpar`  | `margintrue`                    | Enables marginal notes. (No standard floats allowed here; use `\captionof`). |

### Environment Selection Rules

Always select environments based on **semantic meaning**, never visual layout.

- **Theory & Logic:**
  - `theorem`: Core, formal conclusions requiring external cross-referencing.
  - `lemma` / `corollary`: Auxiliary supporting lemmas or direct structural consequences.
  - `proposition`: Tool-like or localized propositions (e.g., algorithmic properties).
  - `axiom` / `postulate`: Foundational axioms. Do not use for ordinary modeling assumptions.
  - `proof`: Proof steps. Follows `theorem`/`lemma`/`proposition`. Hidable via `result=noanswer`.
- **Setup & Traits:**
  - `definition`: New mathematical concepts, terms, or objects.
  - `assumption`: Model assumptions or prerequisites (modeling/algorithm analysis).
  - `property`: Specific traits of an object (e.g., properties of norms).
- **Exposition & Commentary:**
  - `note` / `remark`: `note` handles critical alerts/pitfalls; `remark` is for light, casual commentary.
  - `conclusion`: Summary text trailing a major section or derivation.
- **Pedagogy & Exercises:**
  - `example` / `problem`: `example` for textbook exposition; `problem` for formal, worked walkthroughs.
  - `solution`: Procedural answers for problems/exercises. Never use `proof` for exercise answers.
  - `exercise` / `problemset`: `exercise` for standalone reader tasks; `problemset` for end-of-chapter itemized lists.

### Formatting & Syntax

- **Theorems:** Use native syntax: `\begin{theorem}[Name]\label{thm:key} ... \end{theorem}`
- **Structure:** Use `introduction` with `\item` for chapter abstracts. Use `lstlisting` with language flags (e.g., `language=Python`) for code.
- **Labels:** Required for referenced items only. Starred environments (e.g., `theorem*`) are unnumbered and must not have labels.
- **Standard Label Prefixes:** `def:`, `thm:`, `lem:`, `cor:`, `axi:`, `pos:`, `pro:`, `fig:`, `tab:`, `eq:`, `exer:`, `sec:`.

## 5. Computation & Plotting Workflow

### Data & Graphics Integrity

- All data must be programmatically processed via Python in `scripts/` using `.csv` inputs from `data/`. No manual math.
- Scripts must print clean numerical results to `stdout`.
- Plots must be saved directly to `figs/` in vector format (`.pdf` or `.eps`). No raster formats (`.png`, `.jpg`). Match academic font scaling and line weights.

### Engineering Calculation Layout Order (Strict)

1. **Principle Statement:** Narrative explanation of theory/principles.
2. **Variable Definitions:** "式中" (where...) details placed directly under the formula.
3. **Data Provenance:** Use explicit `\ref{}` tokens immediately after definitions to declare sources.
4. **Formula Re-statement:** Symbolic math inside an `equation` or `equation*` block.
5. **Substitution:** Next line/block showing literal numbers substituted into the formula.
6. **Final Evaluation:** Final evaluated numerical result.

## 6. Agent Behavior & Automation Rules

### Output Protocol

- **Code First:** Output file modifications or script blocks first. Follow with minimal, essential explanations only.
- **No Prose:** Zero conversational filler, greetings, or inline commentary.
- **Plain Syntax:** Use standard hyphens and straight quotes (`'` or `"`) only. No smart quotes, em-dashes, or decorative Unicode.

### Review & Debugging Protocol

- **Review:** State the exact bug $\rightarrow$ Show the explicit fix $\rightarrow$ Stop. No architectural suggestions or compliments.
- **Debugging:** Read logs/source files first before drawing conclusions. State: what was found, where it is, and the exact fix in one pass.

### Git & Search Protocol

- **Git Workflow:** Make atomic, single-purpose commits. Use Conventional Commits formatting (e.g., `feat(contents): add solar model`, `fix(scripts): resolve division error`). Summaries must be precise.
- **Infrastructure:** Use `ripgrep` (`rg`) and `ast-grep`. If missing from the host environment, halt immediately and prompt the user to install them.

### Content Safety

- **Additive Bias:** Prioritize adding over deleting. Never silently delete/override functional code or prose.
- **Destructive Changes:** If an error is spotted in existing logic, pause and request explicit user confirmation.
- **Accountability:** Post-deletion/modification, clearly summarize exactly what was changed and the outcome.
