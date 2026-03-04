# FIE463 – GitHub Copilot Hands-On Session

This repository contains materials for a hands-on session exploring how to use **GitHub Copilot** in VS Code and on the command line. Students will use AI-assisted coding to solve problems in quantitative macroeconomics and finance.

## Prerequisites

- VS Code with the GitHub Copilot extension installed
- GitHub Copilot CLI installed and accessible in your `PATH`
- Anaconda environment `FIE463` (all required Python packages are pre-installed)

---

## Folder Overview

### `OLG/`

Materials for an Overlapping Generations (OLG) macroeconomic model.

- **`lecture/`** – Reference implementation from Lecture 7:
  - `lecture07_olg.py` — Complete Python solution for the baseline OLG model (log utility, permanent TFP shock).
  - `lecture07.md` / `lecture07.ipynb` — Lecture notes and notebook covering the model setup and solution.
- **`workshop/`** – Hands-on exercises where students extend the lecture code using Copilot:
  - **Exercise 1** (`workshop07_ex01.md`, `instructions_ex01.md`) — Simulate transition dynamics under a transitory vs. a persistent TFP shock, assuming log utility.
  - **Exercise 2** (`workshop07_ex02.md`, `instructions_ex02.md`) — Extend the simulation to general CRRA preferences by implementing a root-finding algorithm to solve the Euler equation each period.

### `assignment/`

Mandatory assignment on optimal savings and portfolio choice.

- Students solve a two-period consumption-savings problem with a risky and a risk-free asset under CARA utility.
- Tasks include computing expected utilities, plotting utility functions and lifetime utility, and implementing a grid search to find optimal savings and risky-asset share.
- Available as both `assignment.md` and `assignment.pdf`.

### `company-summaries/`

A CLI pipeline demo that combines financial data with AI-generated text.

- `specification.md` — Project specification written as a prompt for Copilot to generate the implementation ("vibe coding" example).
- Demonstrates API integration, subprocess management, LLM prompting, and specification-driven development.

### `.github/`

Repository-level Copilot configuration.

- `copilot-instructions.md` — Custom instructions automatically injected into every Copilot prompt: coding conventions (PEP 8, NumPy docstrings, single quotes), Matplotlib output settings (PDF figures), and the Anaconda environment to use (`FIE463`).

---

## How to add GitHub Copilot instructions

- Create or modify the file `copilot-instructions.md` in the folder `.github/`.
- The file should contain instructions for GitHub Copilot that will be automatically added to the context of any prompt sent to GitHub Copilot.

Things you might want to include in the instructions:

- Matplotlib output file format (PDF)
- Which Anaconda environment to use (FIE463)
- Any specific coding style or conventions to follow (e.g., PEP 8, docstrings, etc.)
- Any specific libraries or functions that should be used (e.g., NumPy, SciPy, etc.)