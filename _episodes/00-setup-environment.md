---
title: "Setup"
teaching: 10
exercises: 0
questions:
    - "How do I setup my computing environment for the Daresbury workshop tutorials?"
objectives:
    - "Access virtual environment for the tutorials"
keypoints:
    - "STFC cloud environment"
    - "There are several useful tools installed"
    - "You need to activate the ase-tutorial environment"    

---

### We will use the STFC cloud training platform

- We will use STFC cloud environment https://training.analysis.stfc.ac.uk
- You should have received sign-up information if you are attending the course
- It runs a customised Linux environment


#### Browsers

**Mozilla Firefox** is available. This may be a convenient way to view the tutorial, copy/paste, and access external files.

#### Molecular Graphics Packages

**VESTA** is available for visualisation of structure data.

#### Editors

There are several editors available. You should choose whichever you are confortable with.

* **vi** the venerable UNIX screen mode editor.
* **vim** the improved venerable UNIX screen mode editor.
* **emacs** probably the commonest full-screen UNIX editor.
* **gedit** gui editor
* **vscode**

#### Terminals

You will need to open a terminal from the application menu. The one
named "Terminal" is simplest; you can customise this with
preferred fonts, colours etc.

#### Jupyter

For this tutorial you should _not_ use Jupyter from the application menu: see the next section for details.

### ase-tutorial environment

A Python virtual environment is set up in the ~/opt/ase-tutorial
folder. When you open a new terminal/shell, you need to activate this environment with

```bash
source ~/opt/ase-tutorial/bin/activate
```

If the environment is active, "(ase-tutorial)" will appear in the prompt.
You can deactivate the environment with `deactivate`.

From this environment, you can get an interactive python terminal with
`python` or `ipython` or launch a Jupyter environment with `jupyter
lab`. Unlike the Jupyter in the application menu, this will connect
properly to the tutorial environment (with pre-installed extensions).
If you are prompted to choose a kernel, choose "Python3".
