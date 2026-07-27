---
title: 'Why Software Modules?'
teaching: 10
exercises: 5
---

:::::::::::::::::::::::::::::::::::::: questions

- Why do we need environment modules on shared computing systems?
- What is an environment module and how does it work?
- How does Lmod modify your shell environment?

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Understand the problem that environment modules solve
- Learn how Lmod modifies your shell environment
- Understand the difference between loading and installing software

::::::::::::::::::::::::::::::::::::::::::::::

## The Problem: Conflicting Software Versions

::::::::::::::::::::::::::::::::::::: callout

### The Version Conflict Problem

Imagine you're working on Sagehen with two research groups. One group needs R version 3.6.0 for legacy analysis scripts that haven't been updated in five years. Another group requires R version 4.2.3 for their latest genomics pipeline. Your colleague needs Gaussian 16 to run quantum chemistry simulations, but you need Gaussian 09 for continuity with your previous research.

Without a way to manage these different versions, installing them all in the same locations (like `/usr/bin` or `/usr/lib`) creates conflicts:
- Programs and libraries overwrite each other
- Running `R` might load the wrong version
- Dependencies break unpredictably
- Users can't work on different projects simultaneously

On a shared system with hundreds of users, this would be chaos.

::::::::::::::::::::::::::::::::::::::::::::::

## The Solution: Environment Modules

Environment modules (managed by **Lmod** on Sagehen) solve this problem by dynamically modifying your shell environment. A module is essentially a collection of instructions that:

1. Adds software locations to your `PATH` (where the system looks for executable programs)
2. Sets `LD_LIBRARY_PATH` (where the system looks for shared libraries)
3. Sets other environment variables needed by the software
4. Possibly loads prerequisite modules

When you load a module, it's like installing software temporarily in your current shell session without actually changing the filesystem.

## How Lmod Modifies Your Environment

Let's look at a practical example. When you load the R module:

```bash
$ module load r
```

Lmod runs a script that:
- Adds `/opt/lmod/r/4.2.3/bin` to your `PATH`
- Sets `LD_LIBRARY_PATH` to include R's libraries
- Sets `R_HOME=/opt/lmod/r/4.2.3`
- Sets other R-specific variables

Now when you type `R` at the command line, the shell finds the R executable in the path that was just added.

If you then load a different version:

```bash
$ module swap r r/3.6.0
```

Lmod removes the 4.2.3 entries and adds the 3.6.0 entries instead. No files are actually installed or removed; just your shell environment changes.

## Module Naming Conventions

Modules follow the pattern `software/version`:

- `r/4.2.3`: R version 4.2.3
- `r/3.6.0`: R version 3.6.0
- `r`: Same as the default, usually `r/4.2.3` (marked with D)
- `miniconda3`: The default version of miniconda3
- `miniconda3/22.11`: Specific version
- `gcc/11.2.0`: GCC compiler version 11.2.0

When you run `module load r`, you get the default. To load a specific version, be explicit.

::::::::::::::::::::::::::::::::::::: challenge

### Challenge 1: Explore Available Modules

Connect to Sagehen and run `module avail`. Scroll through the output and identify:
1. How many versions of R are available?
2. What compilers (gcc) are available?
3. Which modules have a default version marked with (D)?

```bash
$ ssh username@sagehen.hpc.pomona.edu
$ module avail
```

::::::::::::::: solution

## Solution

Output will vary, but a typical response shows:

```
R versions:
- r/3.6.0
- r/4.2.3 (D)

Compilers:
- gcc/9.3.0
- gcc/11.2.0

Modules with defaults:
- miniconda3/22.11 (D)
- openmpi/4.1.1 (D)
- r/4.2.3 (D)
```

The important point is that you can see all available options before loading.

:::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Environment modules solve the problem of multiple software versions on shared systems
- Modules temporarily modify your shell environment (PATH, LD_LIBRARY_PATH, etc.)
- No files are installed or removed; only your environment changes
- Modules follow the naming convention software/version
- Modules with **(D)** are defaults; specify versions explicitly to get other versions

::::::::::::::::::::::::::::::::::::::::::::::
