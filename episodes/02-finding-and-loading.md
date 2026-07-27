---
title: 'Finding and Loading Modules'
teaching: 15
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- How do I find specific software on Sagehen?
- How do I load and unload modules?
- How do I learn what a module does before loading it?
- What if I need software that isn't available?

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Use `module avail` with search patterns to find software
- Load and unload modules with `module load` and `module unload`
- Use `module help` and `module show` to inspect modules
- Know what to do if software isn't available

::::::::::::::::::::::::::::::::::::::::::::::

## Searching for Software

On a cluster with hundreds of modules, finding specific software efficiently is essential.

### Basic filtering

```bash
$ module avail r
```

This shows all modules with "r" in their name:

```
   r/3.6.0                       rclone/1.59.1
   r/4.2.3 (D)
```

### Advanced search with grep

Combine `module avail -t` with `grep` to find modules by pattern:

```bash
$ module avail -t | grep python
$ module avail -t | grep -E '^gaussian'
$ module avail -t | grep -i gamess
```

The `-t` flag gives a table-style listing (one per line), which is easier to grep. The `-i` flag makes grep case-insensitive.

## Loading and Unloading Modules

### Your first module load

```bash
$ module load miniconda3
```

Now check what modules you have loaded:

```bash
$ module list
Currently Loaded Modules:
  1) miniconda3/22.11
```

To see what this module actually changed:

```bash
$ which python
/opt/lmod/miniconda3/22.11/bin/python
```

### Unloading modules

When you're done with software, unload it:

```bash
$ module unload miniconda3
$ which python
/usr/bin/python
```

`python` now refers to the system Python (if it exists) or isn't available.

::::::::::::::::::::::::::::::::::::: callout

### Key Commands Summary

| Command | Purpose |
|---------|---------|
| `module avail` | List all available modules |
| `module load <name>` | Load a module |
| `module list` | Show currently loaded modules |
| `module unload <name>` | Unload a module |
| `which <program>` | Show the full path to a program |

::::::::::::::::::::::::::::::::::::::::::::::

## Learning What a Module Does

::::::::::::::::::::::::::::::::::::: callout

### Check Before You Load

**Always use `module help` before loading an unfamiliar module.** This prevents surprises and shows you what dependencies will be automatically loaded.

::::::::::::::::::::::::::::::::::::::::::::::

### Using `module help`

```bash
$ module help r
```

Output:
```
----------- Module Specific Help for "r/4.2.3" ----------

This module loads R version 4.2.3 and its associated libraries.

Prerequisite modules: gcc/11.2.0

Usage:
  R          - start R interactively
  Rscript    - run R scripts

Documentation: https://www.r-project.org/
Maintainer: Andrew Wilson (awilson@pomona.edu)
```

### Understanding module dependencies

Notice the output mentions "Prerequisite modules: gcc/11.2.0". Loading R automatically loads gcc:

```bash
$ module load r
$ module list
Currently Loaded Modules:
  1) gcc/11.2.0    2) r/4.2.3
```

### Using `module show`

To see exactly what a module does to your environment:

```bash
$ module show r/4.2.3
```

Output (simplified):
```
load("gcc/11.2.0")
prepend_path("PATH","/opt/lmod/r/4.2.3/bin")
setenv("R_HOME","/opt/lmod/r/4.2.3")
setenv("R_LIBS_SITE","/opt/lmod/r/4.2.3/lib/R/library")
```

## When Software Isn't Available

If you can't find the software you need:

1. **Double-check your search** with `module avail -t | grep -i name`
2. **Check the Sagehen documentation** for software inventory
3. **Request installation** by emailing its-hpc@pomona.edu with:
   - Software name and version
   - Research purpose
   - License information (open-source, commercial, etc.)
   - Links to documentation

::::::::::::::::::::::::::::::::::::: challenge

### Challenge 1: Load, Check, and Unload

1. Load the `gcc` module (the default version)
2. Use `module list` to confirm it's loaded
3. Use `which gcc` to find its location
4. Unload it
5. Try `which gcc` again; notice the difference

```bash
$ module load gcc
$ module list
$ which gcc
$ module unload gcc
$ which gcc
```

::::::::::::::: solution

## Solution

```bash
$ module load gcc
$ module list
Currently Loaded Modules:
  1) gcc/11.2.0

$ which gcc
/opt/lmod/gcc/11.2.0/bin/gcc

$ module unload gcc
$ which gcc
/usr/bin/gcc
```

The key insight: the module version is gone after unload, and you fall back to the system version (or nothing).

:::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

### Challenge 2: Understand Module Dependencies

1. Check which modules are currently loaded
2. Load the R module
3. Check again: what was automatically loaded?
4. Unload only R (not its dependencies)

```bash
$ module list
$ module load r
$ module list
$ module unload r
$ module list
```

::::::::::::::: solution

## Solution

After `module load r`:
```
Currently Loaded Modules:
  1) gcc/11.2.0    2) r/4.2.3
```

After `module unload r`:
```
Currently Loaded Modules:
  1) gcc/11.2.0
```

GCC remains loaded! Unloading R doesn't remove its dependencies. You must explicitly unload gcc if you don't need it: `module unload gcc`. This is a safety feature; other modules might also depend on gcc.

:::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Use `module avail` with search terms or `grep` to find software
- `module load` activates software; `module unload` deactivates it
- Use `module help` to understand what a module does before loading
- Use `module show` to see exact environment modifications
- Modules can automatically load prerequisites
- If software isn't available, contact its-hpc@pomona.edu to request it

::::::::::::::::::::::::::::::::::::::::::::::
