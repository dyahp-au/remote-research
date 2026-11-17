# Jupyter: quick guide

## How it's set up

- **JupyterLab is installed once, for the whole computer**, as a uv tool.
  It has its own environment, so it never conflicts with other Python packages.
- **Each project has its own packages** in its `.venv` folder.
- **A "kernel" connects the two**: it lets JupyterLab run notebooks with a
  project's packages.

The kernel for this project is called **mcp-project**.

### What is a kernel? (plain English)

JupyterLab is just the **screen** you type into. The kernel is the **engine**
that runs your code: when you run a cell, JupyterLab hands the code to the
kernel, which runs it and sends back the result.

Think of JupyterLab as a **TV remote** and kernels as **TVs**. The remote is
the same everywhere, but what you see depends on which TV it's pointed at:

- **mcp-project** runs code with this project's packages (`mcp`, `arxiv`).
- **Python 3** (the default) runs code with JupyterLab's own Python, which
  doesn't have those packages.

So `ModuleNotFoundError: No module named 'arxiv'` usually just means you're on
the wrong kernel: switch to **mcp-project**.

The kernel also **remembers** everything you've run (variables, imports).
**Restart Kernel** wipes that for a clean start, which is why after reopening a
notebook you need to re-run its cells.

### Taking notes in a notebook

Notes don't depend on the kernel; every notebook can mix notes and code:

- **Code cell**: Python that the kernel runs.
- **Markdown cell**: your notes (headings, bullet points, links). The kernel
  doesn't run these; they're just formatted text.

To turn a cell into a note, click it and press `Esc` then `M`
(or pick "Markdown" in the dropdown at the top). Press `Esc` then `Y` to turn it
back into code. Press `Shift+Enter` to display the formatted note.

---

## Start JupyterLab

1. Open a terminal and go to the folder you want to work in:
   ```
   cd C:\Users\dyah_\Projects\applications\mcp_project
   ```
2. Start it:
   ```
   jupyter-lab
   ```
   - Use a **hyphen**: `jupyter-lab`. `jupyter lab` (with a space) doesn't work here.
   - It opens in your browser (usually http://localhost:8888).
     If it doesn't, copy the link printed in the terminal.
3. Create or open a notebook and choose the **mcp-project** kernel.
   - **mcp-project** has this project's packages (`mcp`, `arxiv`, ...).
   - **Python 3** (the default) has only JupyterLab's own packages.
4. To stop it, press `Ctrl+C` in the terminal (or close the terminal).

---

## Use Jupyter with a NEW project

Run these once, inside the new project folder (one that has a `pyproject.toml`).
Replace `my-project` with a name you'll recognise:

```
uv add --dev ipykernel
uv run python -m ipykernel install --user --name my-project
```

Then pick **my-project** as the kernel in JupyterLab.

> Always add packages with `uv add`, not `uv pip install`.
> Packages installed with `uv pip install` can disappear the next time uv syncs
> the project, and then the kernel stops working.

---

## Handy commands

| What                                 | Command                                   |
|--------------------------------------|-------------------------------------------|
| Start JupyterLab                     | `jupyter-lab`                             |
| List the kernels you've registered   | `jupyter-kernelspec list` (see note below)|
| Remove a kernel                      | delete its folder in `%APPDATA%\jupyter\kernels\` |
| Update JupyterLab                    | `uv tool upgrade jupyterlab`              |
| Add a package to a project           | `uv add <package>` (inside the project)   |

Note: `jupyter-kernelspec` may not be on your PATH. If it isn't, check the folder
`%APPDATA%\jupyter\kernels\` instead: each subfolder is one kernel.

---

## Using VS Code instead

You don't need JupyterLab or a registered kernel. Open the `.ipynb` file in
VS Code and choose the project's `.venv` as the kernel (top right).
The project just needs `ipykernel`: `uv add --dev ipykernel`.

---

## If something goes wrong

- **`jupyter-lab` is not recognized**: open a *new* terminal. If it still fails,
  check that `C:\Users\dyah_\.local\bin` is on your PATH.
- **`ModuleNotFoundError` in a notebook**: you're probably on the wrong kernel.
  Switch to the project's kernel (Kernel menu, then "Change Kernel"), or add the
  package with `uv add <package>`.
- **Install fails on Python 3.14**: reinstall on 3.13 with
  `uv tool install --python 3.13 jupyterlab --force`.
