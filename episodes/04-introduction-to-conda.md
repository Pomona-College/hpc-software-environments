---
title: 'Introduction to Conda'
teaching: 15
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- Why do I need Conda when I already have modules?
- How do I create an isolated Python environment?
- How do I activate and deactivate environments?

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Understand the difference between modules and conda environments
- Create and activate conda environments
- Verify which Python is being used
- Deactivate environments when done

::::::::::::::::::::::::::::::::::::::::::::::

## Modules vs. Conda: When to Use Each

**Modules** load pre-installed software managed by system administrators:
- Good for: Large applications, compiled binaries, cluster-wide tools
- Examples: R, GAMESS, Gaussian, GCC

**Conda** manages Python/R packages in isolated environments:
- Good for: Python libraries, package dependencies, project-specific stacks
- Examples: numpy, scipy, matplotlib, scikit-learn, TensorFlow

Most workflows use both: modules for system software, conda for language-specific packages.

## Your First Conda Environment

### Step 1: Load Miniconda

Miniconda is a minimal conda distribution available as a module:

```bash
$ module load miniconda3
$ conda --version
conda 22.11.1
```

### Step 2: Create an environment

Create a new conda environment for a specific project:

```bash
$ conda create -n myproject python=3.11
```

This creates an environment called "myproject" with Python 3.11. Conda asks for confirmation:

```
The following packages will be installed:
  ...

Proceed? [y/N] y
```

Press `y` to proceed.

### Step 3: Activate the environment

```bash
$ conda activate myproject
(myproject) $
```

Notice the prompt changes to show `(myproject)`. You're now inside the environment.

### Step 4: Verify Python

```bash
(myproject) $ python --version
Python 3.11.8

(myproject) $ which python
/opt/lmod/miniconda3/22.11/envs/myproject/bin/python
```

Python is from your environment, not the system Python.

### Step 5: Deactivate when done

```bash
(myproject) $ conda deactivate
$
```

The prompt returns to normal.

## Listing Environments

See all conda environments on your account:

```bash
$ conda env list
# conda environments:
#
base                  /opt/lmod/miniconda3/22.11
myproject             /home/username/.conda/envs/myproject
analysis_2024         /home/username/.conda/envs/analysis_2024
```

The `base` environment is miniconda3 itself; other environments are yours.

::::::::::::::::::::::::::::::::::::: callout

## WARNING: Never Install into Base

**DO NOT** run `conda install` in the base environment without specifying `-n envname`.

```bash
# BAD - never do this on shared systems
$ conda install tensorflow
```

Instead, always create a named environment:

```bash
# GOOD
$ conda create -n ml_project tensorflow
$ conda activate ml_project
```

On shared clusters, installing in base can consume your disk quota and interfere with other users' work.

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

### Challenge 1: Create and Activate an Environment

1. Load miniconda3
2. Create an environment called "data_sci" with Python 3.10
3. Activate it
4. Check which Python is being used
5. Deactivate

```bash
$ module load miniconda3
$ conda create -n data_sci python=3.10
$ conda activate data_sci
(data_sci) $ which python
(data_sci) $ python --version
(data_sci) $ conda deactivate
```

::::::::::::::: solution

## Solution

After activation:
```
which python outputs: /home/username/.conda/envs/data_sci/bin/python
python --version outputs: Python 3.10.X
```

After deactivation, `which python` points to system Python or shows not found. The key point: conda activation changes your PATH to prioritize the environment's Python.

:::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Modules load system software; conda manages Python packages in isolated environments
- Create environments with `conda create -n name python=version`
- Use `conda activate` to enter an environment; `conda deactivate` to exit
- Most workflows use both modules and conda together
- Never install into the base environment on shared systems

::::::::::::::::::::::::::::::::::::::::::::::
