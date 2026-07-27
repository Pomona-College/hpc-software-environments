---
title: 'Managing Conda Packages'
teaching: 10
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- How do I export my environment for sharing?
- How do I recreate an environment from a file?
- How do I manage disk space used by environments?

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Export environments to YAML files for reproducibility
- Recreate environments from exported files
- Understand platform-specific vs. cross-platform exports
- Monitor and manage disk usage from conda environments

::::::::::::::::::::::::::::::::::::::::::::::

## Exporting Your Environment

When your analysis works perfectly, save the exact configuration:

```bash
(myproject) $ conda env export > environment.yml
```

This creates an `environment.yml` file with every package and version:

```yaml
name: myproject
channels:
  - defaults
dependencies:
  - numpy=1.24.3
  - scipy=1.10.1
  - matplotlib=3.7.1
  - python=3.11.8
  - ... (many more packages and versions)
```

## Recreating from a File

Share `environment.yml` with collaborators. They can recreate the exact environment:

```bash
$ conda env create -f environment.yml
```

Or with a different name:

```bash
$ conda env create -n collab_version -f environment.yml
```

This guarantees the same versions and reproducibility.

### Platform-specific exports

The standard export includes platform-specific details. For sharing across different operating systems, use:

```bash
(myproject) $ conda env export --from-history > environment.yml
```

This exports only the packages you explicitly installed (not their dependencies), allowing conda to resolve them on different systems.

## Checking Disk Usage

::::::::::::::::::::::::::::::::::::: callout

### Watch Your Disk Quota

Conda environments can use hundreds of MB to several GB each. On shared systems with disk quotas, this matters. Regularly check your usage and clean up old environments to stay under your limit. On Sagehen, /rhome and /bigdata share a 1 TB lab quota.

::::::::::::::::::::::::::::::::::::::::::::::

Conda environments live in `~/.conda/envs/`:

```bash
$ du -sh ~/.conda/envs/
2.3G    /home/username/.conda/envs/

$ du -sh ~/.conda/envs/*
2.1G    /home/username/.conda/envs/deeplearning_2024
1.3G    /home/username/.conda/envs/genomics_2024
500M    /home/username/.conda/envs/myproject
```

### Removing old environments

```bash
$ conda env remove -n old_project
```

### Cleaning conda cache

Conda keeps package caches that can be large:

```bash
$ conda clean --all
```

This removes unused packages and cached install files, potentially freeing 500 MB to 2 GB.

::::::::::::::::::::::::::::::::::::: challenge

### Challenge 1: Export and Recreate

1. Activate your data_sci environment
2. Export it to a YAML file
3. Create a copy from the exported file
4. List all your environments

```bash
$ conda activate data_sci
(data_sci) $ conda env export > my_environment.yml
(data_sci) $ head -20 my_environment.yml
(data_sci) $ conda deactivate
$ conda env create -n data_sci_copy -f my_environment.yml
$ conda env list
```

::::::::::::::: solution

## Solution

The YAML file contains:
```yaml
name: data_sci
channels:
  - defaults
dependencies:
  - numpy=...
  - scipy=...
  - pandas=...
  - python=3.10.X
  ...
```

`conda env list` shows both environments:
```
data_sci          /home/username/.conda/envs/data_sci
data_sci_copy     /home/username/.conda/envs/data_sci_copy
```

This demonstrates reproducibility; you can reliably recreate your environment.

:::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Export with `conda env export > environment.yml` for reproducibility
- Recreate with `conda env create -f environment.yml`
- Use `--from-history` for cross-platform environment files
- Monitor disk usage with `du -sh ~/.conda/envs/*`
- Remove old environments with `conda env remove -n name`
- Clean conda cache with `conda clean --all` to free space

::::::::::::::::::::::::::::::::::::::::::::::
