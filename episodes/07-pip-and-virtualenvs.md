---
title: 'pip and Virtual Environments'
teaching: 10
exercises: 5
---

:::::::::::::::::::::::::::::::::::::: questions

- When should I use pip instead of conda?
- What is a virtual environment and how is it different from conda?
- How do I manage dependencies with requirements.txt?

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Understand when pip is preferable to conda
- Create and use Python virtual environments with venv
- Install packages from pip and PyPI
- Manage dependencies with requirements.txt

::::::::::::::::::::::::::::::::::::::::::::::

## Choosing the Right Tool

Three main Python package management tools exist on Sagehen:

| Tool | Best For | Pros | Cons |
|------|----------|------|------|
| **conda** | Scientific computing, complex dependencies | Binary packages, system libs, cross-platform | Slower, larger environments |
| **pip** | Any Python package, rapid development | Lightweight, simple, universal | Dependencies on C libraries trickier |
| **venv** | Lightweight isolation without conda | Fast, minimal overhead, part of Python | Manual dependency management |

**General guidance:**
- For data science: Use **conda** (it handles Fortran/C libraries well)
- For Python development: Use **pip** inside a conda environment
- For minimal isolation: Use **venv**

## Pip Inside a Conda Environment

The easiest approach: use conda for the base Python environment, then pip for additional packages.

```bash
$ module load miniconda3
$ conda create -n webdev python=3.11
$ conda activate webdev
(webdev) $ pip install flask requests
```

This works because pip installs pure Python packages that don't need system libraries.

### Pip directly (not recommended on shared systems)

Avoid using pip with `--user` on Sagehen:

```bash
# NOT RECOMMENDED
$ pip install --user numpy
```

The `--user` flag installs to `~/.local`, creating a global Python environment that's easy to mess up. Conda environments are cleaner and more reproducible.

## Virtual Environments with venv

::::::::::::::::::::::::::::::::::::: callout

### When to Use Venv

Use venv for lightweight Python projects that don't need system-level dependencies (like C libraries or FORTRAN bindings). It's faster and simpler than conda for pure Python work.

::::::::::::::::::::::::::::::::::::::::::::::

Python's built-in `venv` module creates lightweight virtual environments:

```bash
$ module load miniconda3
$ python -m venv myproject
$ source myproject/bin/activate
(myproject) $ pip install numpy scipy
(myproject) $ deactivate
```

### Venv vs. Conda

**Venv advantages:**
- Faster to create (seconds vs. minutes)
- Smaller disk footprint
- No external tool needed beyond Python

**Venv disadvantages:**
- Harder to manage system-level dependencies
- No automatic binary package compilation
- Less suitable for complex scientific packages

## Managing Dependencies with requirements.txt

### Creating a requirements.txt

```bash
(webdev) $ pip freeze > requirements.txt
```

This creates:
```
flask==2.3.2
requests==2.31.0
werkzeug==2.3.6
jinja2==3.1.2
...
```

### Installing from requirements.txt

```bash
$ python -m venv newproject
$ source newproject/bin/activate
(newproject) $ pip install -r requirements.txt
```

All packages are installed at the same versions.

::::::::::::::::::::::::::::::::::::: challenge

### Challenge 1: Create and Use a Virtual Environment

1. Load miniconda3
2. Create a venv called "myapp"
3. Activate it
4. Install requests and beautifulsoup4 with pip
5. List installed packages
6. Deactivate

```bash
$ module load miniconda3
$ python -m venv myapp
$ source myapp/bin/activate
(myapp) $ pip install requests beautifulsoup4
(myapp) $ pip list
(myapp) $ deactivate
```

::::::::::::::: solution

## Solution

After installing, `pip list` shows:
```
beautifulsoup4           4.12.2
requests                 2.31.0
(other dependencies)
```

Deactivation removes the venv from your PATH. The directory `myapp/` remains and can be reactivated later with `source myapp/bin/activate`.

:::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Use conda for scientific computing with complex dependencies
- Use pip for pure Python packages or when adding to conda environments
- Use venv for lightweight, fast Python isolation
- Export with `pip freeze > requirements.txt` for pip-based reproducibility
- For shared systems, avoid `pip install --user` and use conda or venv instead

::::::::::::::::::::::::::::::::::::::::::::::
