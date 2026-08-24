---
title: Instructor Notes
---

## Workshop Overview

**Total Time:** 2.5 hours (90 min teaching + 60 min exercises)
**Target Audience:** Researchers and students using Sagehen HPC cluster
**Difficulty:** Intermediate (assumes basic Linux command-line knowledge)
**Format:** Hands-on with live coding and exercises

## Learning Progression

The workshop builds from simple (loading modules) to complex (managing project-wide reproducibility):

1. **Lmod Basics**:Foundation: what modules are, why they exist
2. **Finding Software**:Practical: how to discover and learn about modules
3. **Managing Modules**:Complex workflows: multiple modules, saved collections
4. **Conda Environments**:Shift to package management for Python/R
5. **Pip and Venv**:Alternative approaches and when to use them
6. **Best Practices**:Reproducibility and professional workflows

Each episode builds on previous knowledge without being strictly prerequisite; instructors can skip episodes or reorder based on audience needs.

## Pre-Workshop Checklist

**1 week before:**
- Confirm all learners have Sagehen accounts and SSH access
- Test SSH connections from your teaching location (account for network restrictions)
- Verify modules listed in setup guide are current:
  - `module avail r`:should show r/4.2.2, r/4.2.3, r/4.4.1, r/4.5.1 (D)
  - `module avail miniconda3`:should show py311, py312, py313 versions (py313_26.3.2-2 D)
  - `module avail cuda`:should show cuda/11.8.0, 12.0.0, 12.2.1 (D)
- Test conda environment creation: `conda create -n testenv python=3.10` works
- Prepare a demo conda environment for live examples

**1 day before:**
- Test all example commands on a live Sagehen account
- Create sample environments and collections
- Verify `module save` and `module restore` work as documented
- Confirm disk quota is not an issue on your demo account

## Episode 1: Lmod Basics (20 min)

### Key Points to Emphasize

1. **Why modules matter on shared systems**:Concrete example: "Without modules, if you install R 4.2 globally, anyone needing R 3.6 is stuck. Modules solve this."

2. **Environment vs. Installation**:"Loading a module doesn't install software. It modifies your environment so you can use pre-installed software."

3. **Clarity on PATH**:Learners struggle with abstract "PATH" concept. Show it:
   ```bash
   $ echo $PATH
   /opt/.../r/4.5.1/bin:...
   ```

### Teaching Tips

- **Live Demo First:** Spend 5 minutes demonstrating `module avail`, `module load`, `module list`
- **Have them repeat:** Have each learner run `module load r` and `module list` in their terminal
- **Show what changed:** Before and after `which R` is powerful visual proof
- **Avoid theory:** Keep explanation of LD_LIBRARY_PATH brief; most learners don't need details

### Common Misconceptions

- "Loading a module installs it":No, it's already installed
- "Unloading removes the software":No, it just modifies your environment
- "My modules persist to next login":No, each shell starts fresh (unless you set default)

### Exercise Troubleshooting

**Problem:** "module: command not found"
- **Solution:** Module system isn't initialized. Check `/etc/bashrc` loading. Usually works after fresh SSH.

**Problem:** `which` shows system executable, not module version
- **Solution:** Module wasn't loaded. Verify with `module list`.

## Episode 2: Finding Software (20 min)

### Key Points to Emphasize

1. **Filtering with patterns**:`module avail r` is much faster than scrolling full list
2. **module help is crucial**:Always check before loading unfamiliar modules
3. **Defaults matter**:Why r/4.2.3 loads when you just say "r"

### Teaching Tips

- **Demo grep with module avail:**
  ```bash
  $ module avail cuda          # built-in substring filter (there is no gcc module)
  cuda/11.8.0
  cuda/12.0.0
  cuda/12.2.1
  ```
  This is powerful; it shows programmatic discovery without needing GUI tools.

- **Show module help output**:Live example is better than reading documentation
- **Emphasize contacting support**:If software isn't there, they should ask, not work around it

### Exercise Troubleshooting

**Problem:** "grep: command not found" on some systems
- **Solution:** Usually a PATH issue. Have learner do `module list` to check what's loaded
- **Workaround:** Use `module avail <name>` (built-in filter) or `module spider <name>`

## Episode 3: Managing Modules (25 min)

### Key Points to Emphasize

1. **module swap is cleaner than unload+load**:Safety and explicit intent
2. **Collections save time and prevent errors**:Show the time savings
3. **Job scripts require explicit loads**:This is critical for reproducibility

### Teaching Tips

- **Live demo of saving/restoring:**
  ```bash
  $ module load r openblas miniconda3
  $ module save myworkflow
  $ module purge
  $ module list  # Verify empty
  $ module restore myworkflow
  $ module list  # Verify restored
  ```
  This is faster and more convincing than explanation.

- **Show job script in detail:**
  - Explain why `module purge` comes first
  - Show that the exact module loading matters
  - Run a real example: `sbatch minimal_job.sh` and show output

- **Walk through a complex workflow:** Quantum chemistry pipeline might need gcc → gamess → R. Show dependency ordering.

### Exercise Troubleshooting

**Problem:** `module save` fails with "permission denied"
- **Solution:** Check if `.lmod.d` directory exists: `ls -la ~/.lmod.d/`
- **Workaround:** Manually create: `mkdir -p ~/.lmod.d`

**Problem:** `module restore` doesn't load anything
- **Solution:** Verify collection was saved: `module --list-collections`
- **Common cause:** Saved as different name than trying to restore

## Episode 4: Conda Environments (35 min)

### Key Points to Emphasize

1. **Conda is for packages, modules are for applications**:Clear separation
2. **Never install in base on shared systems**:This is a hard rule for HPC
3. **Reproducibility through export**:environment.yml is not optional

### Teaching Tips

- **Live environment creation:**
  ```bash
  $ module load miniconda3
  $ conda create -n demo python=3.10
  $ conda activate demo
  (demo) $ which python  # Show it's the conda one
  (demo) $ python -c "import numpy"  # Will fail
  (demo) $ conda install numpy
  (demo) $ python -c "import numpy; print(numpy.__version__)"
  ```

- **Show the disk space issue:**
  ```bash
  $ du -sh ~/.conda/envs/*
  ```
  Real numbers are more persuasive than warnings about disk space.

- **Export and recreate locally:**
  ```bash
  (demo) $ conda env export > environment.yml
  $ conda env create -n demo2 -f environment.yml
  $ conda env list  # Show both exist with identical packages
  ```

### Exercise Troubleshooting

**Problem:** `conda: command not found` after loading miniconda3
- **Solution:** Module didn't load correctly. Try: `module load miniconda3` again
- **Check:** `which conda` should show `/opt/lmod/miniconda3/...`

**Problem:** `conda create` fails: "Collecting package metadata"
- **Solution:** This is normal; wait for it or check internet connection
- **Note:** Creating large environments (many packages) can take 5+ minutes

**Problem:** Package installation is slow
- **Solution:** Normal on first install with many dependencies. Be patient. Can take 10+ minutes for scipy, pandas, etc.

### Common Mistakes

- **Installing numpy into base:** Remind them: "You're sharing base with the system. Don't install into it."
- **Forgetting `conda activate`:** They load an environment but don't activate it. Show how `which python` changes after activation.
- **Not exporting:** "If you don't export, you can't reproduce this 6 months from now."

## Episode 5: Pip and Venv (15 min)

### Key Points to Emphasize

1. **Conda vs. pip trade-offs**:Conda better for science, pip for web dev
2. **Both are valid**:Choose based on your project, not religion
3. **Combining them works**:Conda for base environment, pip for additional packages

### Teaching Tips

- **Quick venv demo:**
  ```bash
  $ python -m venv quicktest
  $ source quicktest/bin/activate
  (quicktest) $ pip install requests
  (quicktest) $ pip freeze > requirements.txt
  (quicktest) $ deactivate
  ```
  Fast and shows the lightweight nature.

- **Emphasize the choice:**
  - Use conda if: complex dependencies, conda-forge packages needed, shared system
  - Use venv if: pure Python packages only, minimal overhead needed, web development
  - Use both: conda base, pip for additional packages

### Exercise Troubleshooting

**Problem:** `python: command not found` when creating venv
- **Solution:** Need to load Python via miniconda3 first
- **Correct:** `module load miniconda3` then `python -m venv`

## Episode 6: Best Practices (15 min)

### Key Points to Emphasize

1. **Reproducibility requires explicit configuration**:No magic, no assumptions
2. **Disk management prevents quota disasters**:Regular cleanup is essential
3. **Documentation saves future you**:Comments in job scripts matter

### Teaching Tips

- **Show a bad job script, then good:**

  **Bad:**
  ```bash
  #!/bin/bash
  Rscript analysis.R
  python analysis.py
  ```

  **Good:**
  ```bash
  #!/bin/bash
  #SBATCH --job-name=analysis
  #SBATCH --time=01:00:00
  #SBATCH --cpus-per-task=4
  #SBATCH --mem=16G

  # Clean state and load required modules
  module purge
  module load openblas/0.3.25
  module load r/4.5.1
  module load miniconda3

  # Activate data analysis environment
  conda activate myanalysis

  # Run analysis
  Rscript analysis.R
  python analysis.py
  ```

- **Disk quota demo:**
  ```bash
  $ quota_check.sh
  $ du -sh --apparent-size ~/.conda/envs/*
  $ conda clean --all  # Show cleanup in action
  ```
  Note: Bare `quota` is unreliable on BeeGFS; always use `quota_check.sh`.

- **Project structure example:** Show them a real repository with environment.yml, job scripts, README

## Timing and Pacing

| Episode | Teaching | Exercise | Total |
|---------|----------|----------|-------|
| 1. Lmod Basics | 15 min | 5 min | 20 min |
| 2. Finding Software | 10 min | 10 min | 20 min |
| 3. Managing Modules | 15 min | 10 min | 25 min |
| 4. Conda Environments | 20 min | 15 min | 35 min |
| 5. Pip/Venv | 10 min | 5 min | 15 min |
| 6. Best Practices | 10 min | 5 min | 15 min |
| **TOTAL** | **80 min** | **50 min** | **130 min** |

**Note:** Timings are targets. Adjust based on:
- Audience background (more time for beginners)
- System responsiveness (conda operations take time)
- Interest level (best practices can expand for engaged group)

## Technical Considerations

### Network Issues

If participants are off-site and VPN is slow:
- Pre-create conda environments if possible
- Avoid large package installations during exercises
- Have backup examples pre-recorded to show if live demo fails

### Cluster Load

During exercises, many conda create operations might slow the system:
- Stagger participants' environment creation
- Have 2-3 pre-made environments for learners to copy/restore
- Use smaller environments (Python only, not full scipy stack) for exercises

### SSH Access Issues

- Confirm all accounts are active 48 hours before
- Have fallback: pair programming if one person's SSH fails
- Provide backup instructions for Windows users (PuTTY, WSL, etc.)

## Engagement Strategies

1. **Pair programming**:Learners work in pairs, one codes, one watches and comments
2. **Challenges with increasing difficulty**:Episode 1 is very basic; Episode 6 is complex
3. **Real-world examples**:Reference actual research workflows: genomics, ML, quantum chemistry
4. **Frequent breaks**:Every 30 minutes, give people a 2-minute break
5. **Ask questions**:"What would happen if we loaded this module twice?" engages critical thinking

## Facilitating Challenges

Each episode has multiple challenges. Recommended approach:

1. **Read the challenge aloud**:Ensure everyone understands
2. **Give time to work**:5-10 minutes of silence while they work
3. **Circulate**:Check if anyone is stuck
4. **Show solutions**:But have people explain their approach first
5. **Discuss variations**:"What if we changed this parameter?"

## Assessment

No formal exam, but check understanding through:

- **Challenges:** Can they complete tasks independently?
- **Questions:** Do they ask good diagnostic questions?
- **Troubleshooting:** Can they identify and fix problems?

By end of workshop, learners should:
- [ ] Confidently load and unload modules
- [ ] Create conda environments and export them
- [ ] Write reproducible job scripts with modules
- [ ] Understand when to use conda vs. pip vs. venv
- [ ] Know how to get help when things break

## Post-Workshop Resources

Point learners to:
- **Sagehen user guide:** https://pomona-college-hpc.github.io/
- **Lmod docs:** https://lmod.readthedocs.io/
- **Conda docs:** https://docs.conda.io/
- **Support email:** its-hpc@pomona.edu
- **This workshop:** Can be repeated, materials available online

## Troubleshooting Common Workshop Issues

### Few people show up
- Still run it; provide individual attention
- Offer to schedule follow-up 1:1 sessions
- Record it for asynchronous learning

### Many people have connection issues
- Switch to live demonstration only (no hands-on)
- Offer follow-up session for individual troubleshooting
- Provide pre-recorded demos

### Someone finds a module isn't available
- Check on the spot: `module avail modulename`
- If not there, explain: "We can request it from its-hpc@pomona.edu"
- Move on; don't get stuck on one person's missing software

### Conda environment creation takes too long
- Totally normal for large environments (scipy takes 10+ minutes)
- Have people continue with slides/discussion while waiting
- Have backup pre-made environments to use instead

## Improving for Next Time

After the workshop:
- **Collect feedback**:Ask what was most useful, what was confusing
- **Note timing**:Did you finish on time? Which episodes rushed?
- **Module updates**:Check if any modules have changed; update slides
- **Success stories**:Ask participants follow-up: "Did this help your research?"

Use feedback to refine the workshop for next iteration.
