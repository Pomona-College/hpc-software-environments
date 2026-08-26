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

Imagine you're working on Sagehen HPC with two research groups. One group needs R version 3.6.0 for legacy analysis scripts that haven't been updated in five years. Another group requires R version 4.2.3 for their latest genomics pipeline. Your colleague needs Gaussian 16 to run quantum chemistry simulations, but you need Gaussian 09 for continuity with your previous research.

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
- Adds the R installation's `bin` directory (e.g. `/opt/.../r/4.5.1/bin`) to your `PATH`
- Sets `LD_LIBRARY_PATH` to include R's libraries
- Sets `R_HOME` to the installation directory
- Sets other R-specific variables

Now when you type `R` at the command line, the shell finds the R executable in the path that was just added.

If you then load a different version:

```bash
$ module swap r r/4.2.3
```

Lmod removes the 4.5.1 entries and adds the 4.2.3 entries instead. No files are actually installed or removed; just your shell environment changes.

## Module Naming Conventions

Modules follow the pattern `software/version`:

- `r/4.5.1`: R version 4.5.1
- `r/4.2.3`: R version 4.2.3
- `r`: Same as the default, currently `r/4.5.1` (marked with D)
- `miniconda3`: The default version of miniconda3
- `miniconda3/py313_26.3.2-2`: Specific version
- `cuda/12.2.1`: NVIDIA CUDA toolkit version 12.2.1

When you run `module load r`, you get the default. To load a specific version, be explicit.

::::::::::::::::::::::::::::::::::::: challenge

### Challenge 1: Explore Available Modules

Connect to Sagehen and run `module avail`. Scroll through the output and identify:

1. How many versions of R are available?
2. What CUDA toolkit versions are available?
3. Which modules have a default version marked with (D)?

```bash
$ ssh <myusername>@sagehen.hpc.pomona.edu
$ module avail
```

::::::::::::::: solution

## Solution

Output will vary, but a typical response shows:

```
R versions:
- r/4.2.2
- r/4.2.3
- r/4.4.1
- r/4.5.1 (D)

CUDA toolkit versions:
- cuda/11.8.0
- cuda/12.0.0
- cuda/12.2.1 (D)

Modules with defaults:
- miniconda3/py313_26.3.2-2 (D)
- r/4.5.1 (D)
- go/1.23.1 (D)
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

<!-- highlight <labname>/<myusername> placeholders in code blocks; remove if the varnish theme handles this natively -->
<script>(function(){var CSS='.sh-placeholder{color:#c2410c;font-weight:700}[data-bs-theme="dark"] .sh-placeholder,html.dark .sh-placeholder{color:#fdba74}@media (prefers-color-scheme: dark){[data-bs-theme="auto"] .sh-placeholder{color:#fdba74}}';var RX=/<labname>|<myusername>/g;function firstMatch(el){var w=document.createTreeWalker(el,NodeFilter.SHOW_TEXT,null),nodes=[],full='';while(w.nextNode()){nodes.push({n:w.currentNode,s:full.length});full+=w.currentNode.nodeValue;}RX.lastIndex=0;var m;while((m=RX.exec(full))){var s=m.index,e=s+m[0].length,inSpan=false,parts=[];for(var j=0;j<nodes.length;j++){var ns=nodes[j].s,ne=ns+nodes[j].n.nodeValue.length;if(ne<=s||ns>=e)continue;parts.push({node:nodes[j].n,a:Math.max(s-ns,0),b:Math.min(e-ns,nodes[j].n.nodeValue.length)});var p=nodes[j].n.parentNode;while(p&&p!==el){if(p.classList&&p.classList.contains('sh-placeholder')){inSpan=true;break;}p=p.parentNode;}}if(!inSpan&&parts.length)return parts;}return null;}function wrapParts(parts){for(var i=parts.length-1;i>=0;i--){var t=parts[i].node,txt=t.nodeValue,a=parts[i].a,b=parts[i].b;var span=document.createElement('span');span.className='sh-placeholder';span.textContent=txt.slice(a,b);var f=document.createDocumentFragment();if(a>0)f.appendChild(document.createTextNode(txt.slice(0,a)));f.appendChild(span);if(b<txt.length)f.appendChild(document.createTextNode(txt.slice(b)));t.parentNode.replaceChild(f,t);}}function run(){var st=document.createElement('style');st.textContent=CSS;document.head.appendChild(st);document.querySelectorAll('pre,code').forEach(function(el){var guard=0,parts;while((parts=firstMatch(el))&&guard++<500){wrapParts(parts);}});}if(document.readyState==='loading'){document.addEventListener('DOMContentLoaded',run);}else{run();}})();</script>
