---
title: 'Environment Reproducibility'
teaching: 10
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- How do I ensure my research is reproducible?
- What should I document about my software environment?
- How do I pin package versions?

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Write job scripts that explicitly load modules and environments
- Document software choices in version-controlled files
- Pin package versions for long-term reproducibility
- Create a complete, reproducible project structure

::::::::::::::::::::::::::::::::::::::::::::::

## Always Be Explicit in Job Scripts

::::::::::::::::::::::::::::::::::::: callout

### Explicit is Better Than Implicit

**Never rely on your login environment in batch jobs.** Always load modules and activate environments explicitly. This ensures reproducibility and prevents mysterious failures when the default environment changes.

::::::::::::::::::::::::::::::::::::::::::::::

### The solution

```bash
#!/bin/bash
#SBATCH --job-name=myanalysis
#SBATCH --time=02:00:00
#SBATCH --cpus-per-task=8
#SBATCH --mem=32G

# ALWAYS start clean
module purge

# ALWAYS load specific versions
module load r/4.5.1
module load miniconda3/py313_26.3.2-2

# ALWAYS activate environments explicitly
conda activate myanalysis

Rscript analysis.R
python postprocess.py
```

## Pin Versions for Reproducibility

### Dynamic vs. pinned environment.yml

**Bad (unpinned):**
```yaml
name: myanalysis
dependencies:
  - python
  - numpy
  - scipy
```

When you recreate this in 6 months, you get newer versions, possibly breaking your code.

**Good (pinned):**
```yaml
name: myanalysis
channels:
  - defaults
dependencies:
  - python=3.10.8
  - numpy=1.24.3
  - scipy=1.10.1
  - pandas=1.5.3
```

Now `conda env create -f environment.yml` gives the exact same environment regardless of when it's run.

### Creating pinned files

```bash
$ conda create -n myproject python=3.10 numpy scipy pandas
$ conda env export > environment.yml
```

The export includes all exact versions.

### Updating pinned files

When you need to update packages:

```bash
$ conda activate myproject
(myproject) $ conda update numpy scipy
(myproject) $ conda env export > environment.yml
(myproject) $ git commit -m "Update scipy and numpy to latest versions"
```

## Document Your Environment

### In version control

```bash
$ git init myproject
$ conda env export > environment.yml
$ git add environment.yml
$ git commit -m "Initial conda environment specification"
```

### Document module choices in job scripts

```bash
#!/bin/bash
#SBATCH --job-name=quantum_calc

# Gaussian 16 for the quantum chemistry calculation
module load gaussian/16c01_avx2

# R for statistical analysis of the results
module load r/4.5.1

g16 < input.com > output.log
Rscript analyze.R
```

## Recommended Project Structure

```
~/projects/
  genomics_2024/
    environment.yml
    data/
    scripts/
    results/
  deeplearning_2024/
    environment.yml
    notebooks/
    models/
```

Each project has its own `environment.yml` and conda environment.

::::::::::::::::::::::::::::::::::::: challenge

### Challenge 1: Create a Reproducible Job Script

Create a bash script called `final_job.sh` that:

1. Starts with `module purge` for a clean state
2. Loads r/4.5.1 and miniconda3
3. Activates a conda environment
4. Runs `python --version` and `conda env list`
5. Includes comments explaining each step

```bash
$ cat > final_job.sh << 'EOF'
#!/bin/bash
#SBATCH --job-name=reproducible_test
#SBATCH --time=00:10:00

# Clean environment to avoid conflicts
module purge

# Load R at a pinned version
module load r/4.5.1

# Load Python via miniconda3
module load miniconda3

# Activate our conda environment
conda activate myproject

# Verify the environment
echo "Python version:"
python --version
echo "Conda environments available:"
conda env list
EOF

$ chmod +x final_job.sh
```

::::::::::::::: solution

## Solution

The script demonstrates the key reproducibility practices:

- Starting with `module purge` for clean state
- Loading specific module versions (not defaults)
- Explicit conda environment activation
- Verification commands to prove setup worked

This is the minimal reproducible template for any Sagehen job.

:::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

### Challenge 2: Document Your Environment

Create an `environment.yml` file for a hypothetical "climate_analysis" project that needs Python 3.10, numpy, pandas, matplotlib, and netcdf4. Use exact version pinning.

```bash
$ module load miniconda3
$ conda create -n climate_analysis python=3.10 numpy pandas matplotlib netcdf4
$ conda env export > environment.yml
$ cat environment.yml
```

::::::::::::::: solution

## Solution

A typical environment.yml:
```yaml
name: climate_analysis
channels:
  - defaults
dependencies:
  - python=3.10.8
  - numpy=1.24.3
  - pandas=1.5.3
  - matplotlib=3.7.1
  - netcdf4=1.6.2
  - (many transitive dependencies)
```

Commit to git:
```bash
$ git add environment.yml
$ git commit -m "Add climate_analysis conda environment specification"
```

The key is that exact versions and all transitive dependencies are included, making it fully reproducible.

:::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Always load modules and activate environments explicitly in job scripts
- Start job scripts with `module purge` for a clean environment
- Pin package versions in environment.yml for reproducibility
- Document software choices in version control
- Create one environment per project with its own environment.yml
- Update pinned files carefully and commit changes to git

::::::::::::::::::::::::::::::::::::::::::::::
