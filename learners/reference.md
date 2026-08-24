---
title: Reference
---

## Quick Reference: Lmod Commands

### Listing and Finding Modules

| Command | Purpose |
|---------|---------|
| `module avail` | List all available modules |
| `module avail r` | List modules matching "r" |
| `module avail name` | Filter the listing (substring match) |
| `module list` | Show currently loaded modules |
| `module --list-collections` | Show your saved collections |

### Loading and Unloading

| Command | Purpose |
|---------|---------|
| `module load name` | Load a module (default version) |
| `module load name/version` | Load specific version |
| `module load a b c` | Load multiple modules |
| `module unload name` | Unload a module |
| `module purge` | Unload all modules |
| `module swap old new` | Replace one module with another |

### Learning About Modules

| Command | Purpose |
|---------|---------|
| `module help name` | Show module documentation |
| `module show name` | Display what module modifies |

### Managing Collections

| Command | Purpose |
|---------|---------|
| `module save mycollection` | Save current modules as "mycollection" |
| `module restore mycollection` | Load a saved collection |
| `module restore default` | Load default collection (auto-loaded on login) |
| `module save default` | Save current modules as auto-load default |

---

## Quick Reference: Conda Commands

### Environment Management

| Command | Purpose |
|---------|---------|
| `conda create -n name python=3.10` | Create new environment |
| `conda activate name` | Activate an environment |
| `conda deactivate` | Exit current environment |
| `conda env list` | List all environments |
| `conda env remove -n name` | Delete an environment |
| `conda env export > env.yml` | Save environment to file |
| `conda env create -f env.yml` | Create environment from file |

### Package Management

| Command | Purpose |
|---------|---------|
| `conda install package` | Install a package |
| `conda install -c conda-forge package` | Install from conda-forge channel |
| `conda list` | List installed packages |
| `conda update package` | Update a package |
| `conda remove package` | Uninstall a package |
| `conda search package` | Search for a package |

### Maintenance

| Command | Purpose |
|---------|---------|
| `conda clean --all` | Remove unused packages and cache |

---

## Quick Reference: Python Virtual Environments

### Creating and Using

| Command | Purpose |
|---------|---------|
| `python -m venv envname` | Create virtual environment |
| `source envname/bin/activate` | Activate (Linux/macOS) |
| `envname\Scripts\activate` | Activate (Windows) |
| `deactivate` | Exit virtual environment |

### Pip Package Management

| Command | Purpose |
|---------|---------|
| `pip install package` | Install a package |
| `pip list` | List installed packages |
| `pip freeze > requirements.txt` | Save dependencies |
| `pip install -r requirements.txt` | Install from file |

---

## Example: Complete Workflow

### 1. Load Modules

```bash
$ module load openblas/0.3.25
$ module load r/4.5.1
$ module load miniconda3
```

### 2. Create Conda Environment

```bash
$ conda create -n myproject python=3.10
$ conda activate myproject
(myproject) $ conda install numpy scipy pandas
```

### 3. Export Environment

```bash
(myproject) $ conda env export > environment.yml
(myproject) $ cat environment.yml
```

### 4. Write Job Script

```bash
#!/bin/bash
#SBATCH --job-name=analysis
#SBATCH --time=01:00:00
#SBATCH --cpus-per-task=4
#SBATCH --mem=16G

module load openblas/0.3.25
module load r/4.5.1
module load miniconda3

conda activate myproject
python analysis.py
```

### 5. Submit and Check

```bash
$ sbatch job_script.sh
Submitted batch job 12345

$ squeue -u $USER
```

---

## Common Patterns

### Load Specific Compiler Version for Compatibility

```bash
module load cuda/11.8.0   # For older code
module load cuda/12.2.1   # For newer code
module swap cuda cuda/12.2.1  # Change between versions
```

### Create Project-Specific Environment

```bash
conda create -n project_name python=3.10
conda activate project_name
conda install package1 package2 package3
conda env export > environment.yml
```

### Use Both Conda and Pip

```bash
conda create -n myenv python=3.10 numpy scipy
conda activate myenv
pip install package_not_on_conda
conda env export > environment.yml
```

### Reproducible Job Script Template

```bash
#!/bin/bash
#SBATCH --job-name=descriptive_name
#SBATCH --time=HH:MM:SS
#SBATCH --cpus-per-task=N
#SBATCH --mem=NGb

# Clean environment
module purge

# Load required modules
# gcc is available system-wide on Sagehen -- no module load needed
module load miniconda3

# Activate environment
conda activate myproject

# Run analysis
python main_script.py
```

---

## Troubleshooting

### Module Not Found

```bash
# Check spelling
$ module avail name          # built-in filter
$ module spider name         # deep search, all versions
$ module -t avail 2>&1 | grep -i name   # grep form (options BEFORE avail)

# Check dependencies
$ module show module_name
```

### Conda: "No such file or directory"

Make sure you loaded miniconda3:

```bash
$ module load miniconda3
```

### Permission Denied in Job

Check file permissions:

```bash
$ chmod +x script.sh
$ ls -la script.sh
# Should show x in permissions
```

### Out of Disk Quota

Check usage and remove unused environments. Note: Bare `quota` is unreliable on BeeGFS; use Pomona's `quota_check.sh` wrapper which handles BeeGFS quota correctly.

```bash
$ quota_check.sh
$ conda env list
$ conda env remove -n unused_env
$ conda clean --all
```

### Wrong Python Version in Job

Verify in job script:

```bash
# Add diagnostic commands
python --version
which python
conda list
```

---

## Contact Information

**Email:** its-hpc@pomona.edu

**When to contact:**
- Module not available
- Software installation request
- Quota issues
- Job failures
- Account issues

**Include in email:**
- Your username
- Job ID (if applicable)
- Error message (copy and paste)
- What you've tried
