# How to use this repository template

This ZIP contains a starter layout for the public GitHub repository (extension X1).
This instruction file sits **outside** the `project-name/` folder on purpose: do not copy it into your repository.

## Steps

1. Rename `project-name/` to a descriptive name, e.g. `bus-halt-queue-simulator`, and rename `notebook/project-name.ipynb` to match.
2. Create a **public** repository on GitHub with the same name and push the contents of the folder.
3. Replace every `[placeholder in square brackets]` in the files.
4. Replace the example simulation in `docs/index.html` (a Monte Carlo estimate of pi) with your own simulation.
5. Turn on GitHub Pages: *Settings → Pages → Deploy from a branch → main → /docs*.
6. In `README.md`, update the live-demo link and the Open in Colab badge with your user and repository names.
7. Python users keep `requirements.txt`. Julia users delete it and add a `Project.toml`.
8. Commit regularly across your working days with meaningful messages. Before the deadline, tag the final state: `git tag v1.0 && git push --tags`.

## Rules to remember

- The repository is a public demo for the world. **Do not mention the module code or name, the university, "assignment", "coursework", your lecturer or your student ID anywhere**, including commit messages, the web page and notebook outputs.
- Do include the user guide, the mathematics, the references, and the "Concepts applied" section that connects your work to the concepts you studied (generic wording only).
- No API keys or passwords; only small datasets, with their source and licence in `data/README.md`.
- Your detailed AI log stays private in OneDrive. The README carries only a short AI-assistance note.
