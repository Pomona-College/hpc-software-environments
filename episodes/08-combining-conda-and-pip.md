---
title: 'Combining Conda and pip'
teaching: 10
exercises: 5
---

:::::::::::::::::::::::::::::::::::::: questions

- How do I use pip inside a conda environment?
- How do I export an environment that uses both conda and pip packages?
- How do I use these combined environments in job scripts?

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Combine conda and pip appropriately in one environment
- Export combined environments for reproducibility
- Use combined environments in SLURM job scripts

::::::::::::::::::::::::::::::::::::::::::::::

## Why Combine Conda and pip?

- Conda handles complex binary dependencies (MKL, BLAS, etc.)
- Pip handles packages not on conda (newer packages, smaller libraries)
- A single `environment.yml` file captures both for full reproducibility

## Recommended Approach

1. Create a conda environment with major scientific packages:

```bash
$ conda create -n bioinformatics python=3.10 numpy scipy pandas
```

2. Activate and add pip packages:

```bash
$ conda activate bioinformatics
(bioinformatics) $ pip install biopython pysam
```

3. Export for reproducibility:

```bash
(bioinformatics) $ conda env export > environment.yml
```

The environment.yml includes both conda and pip packages:

```yaml
name: bioinformatics
channels:
  - defaults
dependencies:
  - numpy=1.24.3
  - scipy=1.10.1
  - pandas=1.5.3
  - pip
  - pip:
    - biopython==1.81
    - pysam==0.21.0
```

## Practical Examples

### Web development project

Using venv and pip:

```bash
$ module load miniconda3
$ python -m venv flask_app
$ source flask_app/bin/activate
(flask_app) $ pip install flask flask-sqlalchemy flask-login
(flask_app) $ pip freeze > requirements.txt
```

Collaborator sets up:

```bash
$ python -m venv flask_app
$ source flask_app/bin/activate
(flask_app) $ pip install -r requirements.txt
```

### Machine learning project

Using conda + pip:

```bash
$ conda create -n ml_project python=3.10 numpy pandas scikit-learn pytorch torchvision -c pytorch
$ conda activate ml_project
(ml_project) $ pip install tensorboard wandb
(ml_project) $ conda env export > environment.yml
```

Collaborator sets up:

```bash
$ conda env create -f environment.yml
$ conda activate ml_project
```

## In Job Scripts

### Using venv

```bash
#!/bin/bash
#SBATCH --job-name=web_task
#SBATCH --time=00:30:00

module load miniconda3
source myproject/bin/activate
python app.py
deactivate
```

### Using conda with pip packages

```bash
#!/bin/bash
#SBATCH --job-name=ml_inference
#SBATCH --time=01:00:00
#SBATCH --gpus=1
#SBATCH --mem=32G

module load miniconda3
conda activate ml_project
python inference.py
```

Both work; choose based on your project needs.

::::::::::::::::::::::::::::::::::::: challenge

### Challenge 1: Export and Recreate with requirements.txt

1. Create a venv and install some packages
2. Generate requirements.txt
3. Create a new venv and install from the requirements file
4. Verify the same packages are installed

```bash
$ module load miniconda3
$ python -m venv testapp
$ source testapp/bin/activate
(testapp) $ pip install requests beautifulsoup4
(testapp) $ pip freeze > requirements.txt
(testapp) $ deactivate
$ python -m venv testapp_copy
$ source testapp_copy/bin/activate
(testapp_copy) $ pip install -r requirements.txt
(testapp_copy) $ pip list
```

::::::::::::::: solution

## Solution

The requirements.txt file contains:
```
beautifulsoup4==4.12.2
certifi==2023.X.X
requests==2.31.0
...
```

After installing in the new environment, `pip list` shows identical packages and versions. This demonstrates reproducibility with pure pip and venv.

:::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Combine conda (for scientific packages) and pip (for pure Python packages) in one environment
- The exported environment.yml captures both conda and pip packages
- Use conda for the base, then pip for packages not available in conda
- Both venv/pip and conda approaches work in SLURM job scripts
- Always export your environment for reproducibility regardless of which tools you use

::::::::::::::::::::::::::::::::::::::::::::::
