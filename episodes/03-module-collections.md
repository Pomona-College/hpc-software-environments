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
$ module load openmpi gaussian miniconda3 ncbi-blast
$ module list
Currently Loaded Modules:
  1) openmpi/4.1.5_ucx-1.14.0    2) gaussian/16c01_avx2    3) miniconda3/py313_26.3.2-2    4) ncbi-blast/2.13.0+
```

## Swapping Module Versions

To switch between software versions, use `module swap`:

```bash
$ module load cuda/11.8.0
$ module swap cuda cuda/12.2.1
$ module list
Currently Loaded Modules:
  1) cuda/12.2.1
```

The `swap` command unloads the old version and loads the new one in a single operation, which is cleaner than separate unload/load commands.

## Resetting Everything: `module purge`

If you load many modules and need to start fresh:

```bash
$ module load r miniconda3 ncbi-blast gaussian
$ module purge
$ module list
No modules loaded
```

Use `module purge` to start a fresh shell environment, avoid conflicts when switching projects, or test if your code really needs all those modules.

::::::::::::::::::::::::::::::::::::: callout

## Default Modules at Login

When you first log into Sagehen HPC, certain modules might be automatically loaded based on system configuration or your `.bashrc` file. Use `module purge` to clear them, then load only what you need for your current task.

::::::::::::::::::::::::::::::::::::::::::::::

## Saving Module Collections

::::::::::::::::::::::::::::::::::::: callout

### Save Time with Collections

Manually loading the same modules every time gets tedious and error-prone. Module collections let you save a set of modules once and restore them with a single command.

::::::::::::::::::::::::::::::::::::::::::::::

Save your current module configuration as a named collection:

```bash
$ module load openblas/0.3.25 r/4.5.1
$ module save biostats
```

Lmod stores it in `~/.lmod.d/biostats`.

### Restoring a collection

```bash
$ module restore biostats
$ module list
Currently Loaded Modules:
  1) openblas/0.3.25    2) r/4.5.1
```

### Listing your collections

```bash
$ module --list-collections
Named collections for /rhome/<myusername>:
  1) biostats
  2) python_analysis
  3) quantum_chem
```

## Setting a Default Collection

Make a collection load automatically when you log in:

```bash
$ module load openblas/0.3.25 r/4.5.1 miniconda3
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
module load openblas/0.3.25
module load r/4.5.1
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

1. Load at least three different modules (e.g., openblas, miniconda3, r)
2. Save them as a collection called "myworkflow"
3. Purge all modules
4. Restore the collection
5. Verify all modules are reloaded

```bash
$ module load openblas miniconda3 r
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
  1) openblas/0.3.25    2) miniconda3/py313_26.3.2-2    3) r/4.5.1
```

The collection saves the exact versions you had loaded and restores them perfectly.

:::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

### Challenge 2: Write a Job Script with Modules

Create a file called `test_job.sh` with:
1. SLURM header requesting 2 CPUs, 4GB RAM, 10 minutes
2. Load the openblas and miniconda3 modules
3. Print "Modules loaded:" followed by `module list`
4. Echo "Job completed"

```bash
$ cat > test_job.sh << 'EOF'
#!/bin/bash
#SBATCH --job-name=test_modules
#SBATCH --cpus-per-task=2
#SBATCH --mem=4G
#SBATCH --time=00:10:00

module load openblas miniconda3
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

<!-- highlight <labname>/<myusername> placeholders in code blocks; remove if the varnish theme handles this natively -->
<script>(function(){var CSS='.sh-placeholder{color:#c2410c;font-weight:700}[data-bs-theme="dark"] .sh-placeholder,html.dark .sh-placeholder{color:#fdba74}@media (prefers-color-scheme: dark){[data-bs-theme="auto"] .sh-placeholder{color:#fdba74}}';var RX=/<labname>|<myusername>/g;function firstMatch(el){var w=document.createTreeWalker(el,NodeFilter.SHOW_TEXT,null),nodes=[],full='';while(w.nextNode()){nodes.push({n:w.currentNode,s:full.length});full+=w.currentNode.nodeValue;}RX.lastIndex=0;var m;while((m=RX.exec(full))){var s=m.index,e=s+m[0].length,inSpan=false,parts=[];for(var j=0;j<nodes.length;j++){var ns=nodes[j].s,ne=ns+nodes[j].n.nodeValue.length;if(ne<=s||ns>=e)continue;parts.push({node:nodes[j].n,a:Math.max(s-ns,0),b:Math.min(e-ns,nodes[j].n.nodeValue.length)});var p=nodes[j].n.parentNode;while(p&&p!==el){if(p.classList&&p.classList.contains('sh-placeholder')){inSpan=true;break;}p=p.parentNode;}}if(!inSpan&&parts.length)return parts;}return null;}function wrapParts(parts){for(var i=parts.length-1;i>=0;i--){var t=parts[i].node,txt=t.nodeValue,a=parts[i].a,b=parts[i].b;var span=document.createElement('span');span.className='sh-placeholder';span.textContent=txt.slice(a,b);var f=document.createDocumentFragment();if(a>0)f.appendChild(document.createTextNode(txt.slice(0,a)));f.appendChild(span);if(b<txt.length)f.appendChild(document.createTextNode(txt.slice(b)));t.parentNode.replaceChild(f,t);}}function run(){var st=document.createElement('style');st.textContent=CSS;document.head.appendChild(st);document.querySelectorAll('pre,code').forEach(function(el){var guard=0,parts;while((parts=firstMatch(el))&&guard++<500){wrapParts(parts);}});}if(document.readyState==='loading'){document.addEventListener('DOMContentLoaded',run);}else{run();}})();</script>
