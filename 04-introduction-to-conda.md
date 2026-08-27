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
conda 26.3.2
```

![The miniconda3 module comes in three versions on Sagehen HPC; `py313_26.3.2-2` is the default `(D)` — and often already loaded `(L)`.](fig/04-module-avail-miniconda3.png){alt='A page of module avail output highlighting three miniconda3 modules: py311, py312, and py313 versions, with the py313 version marked L and D, meaning loaded and default. Surrounding modules include matlab, mathematica, and openmpi.'}

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
/rhome/<myusername>/.conda/envs/myproject/bin/python
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
base                  /opt/.../miniconda3/py313_26.3.2-2
myproject             /rhome/<myusername>/.conda/envs/myproject
analysis_2024         /rhome/<myusername>/.conda/envs/analysis_2024
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
which python outputs: /rhome/<myusername>/.conda/envs/data_sci/bin/python
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

<!-- highlight <labname>/<myusername> placeholders in code blocks; remove if the varnish theme handles this natively -->
<script>(function(){var CSS='.sh-placeholder{color:#c2410c;font-weight:700}[data-bs-theme="dark"] .sh-placeholder,html.dark .sh-placeholder{color:#fdba74}@media (prefers-color-scheme: dark){[data-bs-theme="auto"] .sh-placeholder{color:#fdba74}}';var RX=/<labname>|<myusername>/g;function firstMatch(el){var w=document.createTreeWalker(el,NodeFilter.SHOW_TEXT,null),nodes=[],full='';while(w.nextNode()){nodes.push({n:w.currentNode,s:full.length});full+=w.currentNode.nodeValue;}RX.lastIndex=0;var m;while((m=RX.exec(full))){var s=m.index,e=s+m[0].length,inSpan=false,parts=[];for(var j=0;j<nodes.length;j++){var ns=nodes[j].s,ne=ns+nodes[j].n.nodeValue.length;if(ne<=s||ns>=e)continue;parts.push({node:nodes[j].n,a:Math.max(s-ns,0),b:Math.min(e-ns,nodes[j].n.nodeValue.length)});var p=nodes[j].n.parentNode;while(p&&p!==el){if(p.classList&&p.classList.contains('sh-placeholder')){inSpan=true;break;}p=p.parentNode;}}if(!inSpan&&parts.length)return parts;}return null;}function wrapParts(parts){for(var i=parts.length-1;i>=0;i--){var t=parts[i].node,txt=t.nodeValue,a=parts[i].a,b=parts[i].b;var span=document.createElement('span');span.className='sh-placeholder';span.textContent=txt.slice(a,b);var f=document.createDocumentFragment();if(a>0)f.appendChild(document.createTextNode(txt.slice(0,a)));f.appendChild(span);if(b<txt.length)f.appendChild(document.createTextNode(txt.slice(b)));t.parentNode.replaceChild(f,t);}}function run(){var st=document.createElement('style');st.textContent=CSS;document.head.appendChild(st);document.querySelectorAll('pre,code').forEach(function(el){var guard=0,parts;while((parts=firstMatch(el))&&guard++<500){wrapParts(parts);}});}if(document.readyState==='loading'){document.addEventListener('DOMContentLoaded',run);}else{run();}})();</script>
