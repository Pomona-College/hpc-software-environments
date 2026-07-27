---
title: 'Best Practices and Troubleshooting'
teaching: 10
exercises: 5
---

:::::::::::::::::::::::::::::::::::::: questions

- How do I avoid breaking my analysis when modules are updated?
- How do I clean up and avoid filling my disk quota?
- How should I test before submitting large jobs?
- How should I ask for help from the HPC team?

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Manage disk usage efficiently
- Test scripts locally before submitting large jobs
- Know when and how to contact the HPC support team
- Follow a checklist for setting up new projects

::::::::::::::::::::::::::::::::::::::::::::::

## Monitor and Manage Disk Usage

::::::::::::::::::::::::::::::::::::: callout

### Prevent Quota Issues

Conda environments can consume substantial disk space. Regular maintenance prevents quota problems and keeps your cluster account healthy. On Sagehen, /rhome and /bigdata share a 1 TB lab quota.

::::::::::::::::::::::::::::::::::::::::::::::

### Check your usage

Note: Bare `quota` is unreliable on BeeGFS; use Pomona's `quota_check.sh` wrapper which handles BeeGFS quota correctly.

```bash
$ quota_check.sh
$ du -sh --apparent-size ~/.conda/envs/*
2.1G    /home/username/.conda/envs/deeplearning_2024
1.3G    /home/username/.conda/envs/genomics_2024
500M    /home/username/.conda/envs/myproject
```

### Clean up unused environments

```bash
$ conda env remove -n old_project
```

### Clean conda cache

```bash
$ conda clean --all
```

This removes unused packages and cached install files, potentially freeing 500 MB to 2 GB.

### Check for large hidden directories

```bash
$ du -sh ~/* | sort -rh | head -10
```

## Test Before Submitting Large Jobs

### Interactive testing

Before running a 24-hour job, test locally:

```bash
$ module load gcc r
$ Rscript analysis.R < small_test_data.csv > output.txt
```

### Test with a small job first

```bash
#!/bin/bash
#SBATCH --job-name=test
#SBATCH --time=00:05:00
#SBATCH --cpus-per-task=1
#SBATCH --mem=2G

module load gcc r
Rscript analysis.R < small_test_data.csv > output.txt
```

Submit and check:

```bash
$ sbatch test_job.sh
$ squeue -u $USER
$ tail slurm-12345.out
```

Once you know it works, submit the full job.

## Getting Help

### Common issues

1. **Module not found:**
   - Check spelling: `module avail -t | grep name`
   - Contact its-hpc@pomona.edu
2. **Code worked locally but fails on cluster:**
   - Environment mismatch: verify modules in job script
   - Data path issues: use absolute paths, not relative
   - Resource limits: check memory and time requirements
3. **Out of disk quota:**
   - Remove old conda environments: `conda env remove -n oldenv`
   - Clean conda cache: `conda clean --all`
   - Archive old data to external storage

### Contacting support

Email its-hpc@pomona.edu with:
- Job ID (from `squeue` or log files)
- Full error message
- What you've already tried
- Your account name

## Checklist: Setting Up a New Project

- [ ] Create project directory with version control (git)
- [ ] Create conda environment with specific Python version
- [ ] Install required packages and export `environment.yml`
- [ ] Create template job script with `module load` commands
- [ ] Test locally with small data
- [ ] Test on cluster with small job (short time limit)
- [ ] Document in README what software is needed
- [ ] Add environment files to git repository
- [ ] Run full analysis
- [ ] Commit final results

## Complete Project Setup Example

```bash
# Create project
$ mkdir genomics_analysis_2024
$ cd genomics_analysis_2024
$ git init

# Create conda environment
$ conda create -n genomics python=3.10
$ conda activate genomics
(genomics) $ conda install numpy pandas biopython samtools -c conda-forge
(genomics) $ pip install pysam biotools
(genomics) $ conda env export > environment.yml
(genomics) $ git add environment.yml

# Create job script
$ cat > job_script.sh << 'EOF'
#!/bin/bash
#SBATCH --job-name=genomics
#SBATCH --time=04:00:00
#SBATCH --cpus-per-task=8
#SBATCH --mem=32G

module purge
module load gcc/11.2.0
module load miniconda3

conda activate genomics
python analyze_genomes.py
EOF

$ chmod +x job_script.sh
$ sbatch job_script.sh
$ git add analyze_genomes.py job_script.sh
$ git commit -m "Initial genomics analysis setup"
```

::::::::::::::::::::::::::::::::::::: challenge

### Challenge 1: Check and Manage Disk Usage

1. Check your total disk quota: `quota_check.sh`
2. See what's using space: `du -sh --apparent-size ~/.conda/envs/*`
3. List all your conda environments: `conda env list`
4. If you have test environments, remove one: `conda env remove -n test_env`
5. Clean up conda cache: `conda clean --all`

```bash
$ quota_check.sh
$ du -sh --apparent-size ~/.conda/envs/*
$ conda env list
$ conda clean --all
```

::::::::::::::: solution

## Solution

Expected output from quota_check.sh shows your current usage and limit. `du -sh --apparent-size` shows sizes like:
```
1.2G    /home/username/.conda/envs/bigproject
500M    /home/username/.conda/envs/test_env
```

After cleanup, total usage drops and you stay under quota. This regular maintenance prevents quota issues.

:::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Monitor disk usage regularly and clean up old conda environments
- Test scripts locally and with small jobs before submitting large ones
- Contact its-hpc@pomona.edu with job ID, error message, and what you've tried
- Follow the new project checklist for reproducible research setups
- Use `module purge` at the start of job scripts for a clean environment
- Start with `module purge`, then load specific module versions explicitly

::::::::::::::::::::::::::::::::::::::::::::::
