---
title: 'Module Collections and Defaults'
teaching: 15
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- How do I load multiple modules together?
- How do I switch between different versions of the same software?
- How can I save my module configuration and reload it later?
- How do I use modules in batch job scripts?

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Load multiple modules for complex workflows
- Use `module swap` to switch between versions
- Use `module purge` to reset to a clean state
- Create and manage personal module collections
- Write module commands in SLURM job scripts

::::::::::::::::::::::::::::::::::::::::::::::

## Loading Multiple Modules

Many research workflows require multiple pieces of software:

```bash
$ module load gcc/11.2.0 gamess miniconda3 blast
$ module list
Currently Loaded Modules:
  1) gcc/11.2.0    2) gamess/2020    3) miniconda3/22.11    4) blast/2.13
```

## Swapping Module Versions

To switch between software versions, use `module swap`:

```bash
$ module load gcc/9.3.0
$ module swap gcc gcc/11.2.0
$ module list
Currently Loaded Modules:
  1) gcc/11.2.0
```

The `swap` command unloads the old version and loads the new one in a single operation, which is cleaner than separate unload/load commands.

## Resetting Everything: `module purge`

If you load many modules and need to start fresh:

```bash
$ module load r miniconda3 blast gamess
$ module purge
$ module list
No modules loaded
```

Use `module purge` to start a fresh shell environment, avoid conflicts when switching projects, or test if your code really needs all those modules.

::::::::::::::::::::::::::::::::::::: callout

## Default Modules at Login

When you first log into Sagehen, certain modules might be automatically loaded based on system configuration or your `.bashrc` file. Use `module purge` to clear them, then load only what you need for your current task.

::::::::::::::::::::::::::::::::::::::::::::::

## Saving Module Collections

::::::::::::::::::::::::::::::::::::: callout

### Save Time with Collections

Manually loading the same modules every time gets tedious and error-prone. Module collections let you save a set of modules once and restore them with a single command.

::::::::::::::::::::::::::::::::::::::::::::::

Save your current module configuration as a named collection:

```bash
$ module load gcc/11.2.0 r/4.2.3
$ module save biostats
```

Lmod stores it in `~/.lmod.d/biostats`.

### Restoring a collection

```bash
$ module restore biostats
$ module list
Currently Loaded Modules:
  1) gcc/11.2.0    2) r/4.2.3
```

### Listing your collections

```bash
$ module --list-collections
Named collections for /home/username:
  1) biostats
  2) python_analysis
  3) quantum_chem
```

## Setting a Default Collection

Make a collection load automatically when you log in:

```bash
$ module load gcc/11.2.0 r/4.2.3 miniconda3
$ module save default
```

Now every time you SSH to Sagehen, those modules are loaded automatically. To remove the automatic loading:

```bash
$ module purge
$ module save default
```

## Using Modules in Job Scripts

::::::::::::::::::::::::::::::::::::: callout

### Critical for Reproducibility

When you submit a batch job to SLURM, your current module configuration is **not** automatically passed to the job. Always load modules in your job script.

::::::::::::::::::::::::::::::::::::::::::::::

### Example job script

```bash
#!/bin/bash
#SBATCH --job-name=ranalysis
#SBATCH --time=00:30:00
#SBATCH --cpus-per-task=4
#SBATCH --mem=16G

# Load required modules
module load gcc/11.2.0
module load r/4.2.3
module load miniconda3

# Run your analysis
Rscript analysis.R
```

You can also restore a collection in a job script:

```bash
#!/bin/bash
#SBATCH --job-name=genomics
#SBATCH --time=04:00:00
#SBATCH --cpus-per-task=8
#SBATCH --mem=32G

module restore genomics_pipeline
./run_analysis.sh
```

::::::::::::::::::::::::::::::::::::: challenge

### Challenge 1: Create and Restore a Collection

1. Load at least three different modules (e.g., gcc, miniconda3, r)
2. Save them as a collection called "myworkflow"
3. Purge all modules
4. Restore the collection
5. Verify all modules are reloaded

```bash
$ module load gcc miniconda3 r
$ module save myworkflow
$ module purge
$ module restore myworkflow
$ module list
```

::::::::::::::: solution

## Solution

After restore:
```
Currently Loaded Modules:
  1) gcc/11.2.0    2) miniconda3/22.11    3) r/4.2.3
```

The collection saves the exact versions you had loaded and restores them perfectly.

:::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

### Challenge 2: Write a Job Script with Modules

Create a file called `test_job.sh` with:
1. SLURM header requesting 2 CPUs, 4GB RAM, 10 minutes
2. Load gcc and miniconda3 modules
3. Print "Modules loaded:" followed by `module list`
4. Echo "Job completed"

```bash
$ cat > test_job.sh << 'EOF'
#!/bin/bash
#SBATCH --job-name=test_modules
#SBATCH --cpus-per-task=2
#SBATCH --mem=4G
#SBATCH --time=00:10:00

module load gcc miniconda3
echo "Modules loaded:"
module list
echo "Job completed"
EOF

$ chmod +x test_job.sh
$ sbatch test_job.sh
```

::::::::::::::: solution

## Solution

When submitted with `sbatch test_job.sh`, it creates a job that explicitly loads modules before running commands. Check the output with `squeue` or look in the log file (slurm-JOBID.out). The job script explicitly loads modules rather than relying on your login environment.

:::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Load multiple modules with `module load module1 module2 module3`
- Use `module swap` to switch versions cleanly
- Use `module purge` to start fresh
- Save configurations with `module save <name>` and restore with `module restore <name>`
- Set `module save default` to auto-load modules on login
- Always load modules in job scripts; don't rely on your login environment

::::::::::::::::::::::::::::::::::::::::::::::
