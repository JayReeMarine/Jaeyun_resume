# Jay Ree – Resume (LaTeX)

Managing a resume in `.tex` instead of Word.
Edit the file, save, and a PDF is generated automatically.

---

## Setup (one-time)

### 1. Check required software

| Tool | How to verify | If missing |
|---|---|---|
| TeX Live | `pdflatex --version` in terminal | Install from [tug.org/texlive](https://tug.org/texlive/) |
| VSCode | — | [code.visualstudio.com](https://code.visualstudio.com/) |
| LaTeX Workshop | Search `james-yu.latex-workshop` in Extensions tab | Install from VSCode Marketplace |

### 2. Configure LaTeX Workshop

Open `Cmd + ,` → click the **"Open Settings (JSON)"** icon (top-right) → add the following:

```jsonc
{
  // Auto-compile on save
  "latex-workshop.latex.autoBuild.run": "onSave",

  // View PDF inside a VSCode tab (use the shortcut, not the file explorer)
  "latex-workshop.view.pdf.viewer": "tab",

  // Use latexmk — handles re-runs automatically
  "latex-workshop.latex.tools": [
    {
      "name": "latexmk",
      "command": "latexmk",
      "args": ["-pdf", "-synctex=1", "-interaction=nonstopmode", "%DOC%"]
    }
  ],
  "latex-workshop.latex.recipes": [
    {
      "name": "latexmk",
      "tools": ["latexmk"]
    }
  ]
}
```

> Restart VSCode after saving the settings.

---

## Compilation

To compile the resume to PDF:

```bash
cd Jaeyun_resume
latexmk -pdf resume.tex
```

The compiled PDF will be generated as `resume.pdf`.

> **Note:** If you see `"All targets are up-to-date"`, the PDF already exists and is current — this is not an error.

To force a full recompile from scratch:

```bash
latexmk -pdf -g resume.tex
```

To clean up build artifacts:

```bash
latexmk -c   # removes .aux .log .out etc., keeps resume.pdf
latexmk -C   # removes everything including resume.pdf
```

---

## Daily workflow

### Edit & preview

```
1. Open  resume.tex  in VSCode
2. Make your changes
3. Cmd + S  →  latexmk runs automatically  →  resume.pdf is updated
4. View PDF:  Cmd + Option + V
```

> **Important:** Always open the PDF via `Cmd + Option + V` while `resume.tex` is active.
> Do NOT click `resume.pdf` directly in the file explorer — VSCode will show a binary error.

---

## File structure

```
Jaeyun_resume/
├── resume.tex      ← only file you need to edit
├── resume.pdf      ← auto-generated (safe to exclude from Git)
├── resume.aux      ← build cache        ┐
├── resume.fls      ← file list          │ all auto-generated,
├── resume.log      ← compile log        │ excluded via .gitignore
├── resume.out      ← hyperlink cache    ┘
└── README.md
```

---

## Editing guide

### Add a new section

```latex
\resumesection{Section Name}   % renders a full-width coloured header bar
```

### Add a job entry

```latex
\noindent\textbf{Role – Company}\hfill\textbf{Date range}

\begin{itemize}[leftmargin=1.5em, itemsep=3pt, topsep=2pt, parsep=0pt]
  \item Bullet point one
  \item Bullet point two
\end{itemize}
```

### Add a hyperlink

```latex
\href{https://example.com}{Text to display}
```

### Special character escaping

| Character | LaTeX |
|---|---|
| `%` | `\%` |
| `$` | `\$` |
| `&` | `\&` |
| `#` | `\#` |
| `~` | `\textasciitilde` |

---

## TODO: URLs to fill in

Search (`Cmd + F`) for the placeholders below in `resume.tex` and replace with real URLs:

| Placeholder | Replace with |
|---|---|
| `YOURPROFILE` | LinkedIn profile URL |
| `YOURUSERNAME` | GitHub profile URL |
| `YOURPROJECTLINK` | Research project link |
| `YOURCAFEPROJECTLINK` | Cafe management project link |
| `GLORYAUTOURL` | Glory Auto website URL |
| `YOURBADGEID` | AWS Credly badge URL |

---

## Troubleshooting

### "Binary file" error when opening resume.pdf
This happens when you click `resume.pdf` directly in the file explorer.
**Fix:** Open `resume.tex` first, then press `Cmd + Option + V`.

### PDF not updating after save
- Check the LaTeX Workshop status bar at the bottom of VSCode
- Or run manually: `latexmk -pdf resume.tex`

### Compilation errors
- `Cmd + Shift + P` → `LaTeX Workshop: Show Compilation Log`
- Or in the terminal: `grep "!" resume.log`

### Missing fontawesome5 package
```bash
sudo tlmgr install fontawesome5
```
