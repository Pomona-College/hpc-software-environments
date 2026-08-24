---
title: Learner Profiles
---

## Profile 1: Alex, Graduate Student in Molecular Biology

**Background:** 2 years of research experience, basic Linux skills, uses R frequently

**Current Challenge:**
Alex needs to run genomics workflows on Sagehen that require multiple software versions:
- R 4.2.3 for statistical analysis (newer packages needed)
- BLAST 2.13 for sequence searching
- samtools for manipulating sequencing data
- Python for preprocessing scripts

Every time Alex logs into the cluster, they manually load modules and install Python packages, hoping the setup is the same each time. Recently, a collaborator tried to run Alex's analysis script but got different results because conda packages were installed in a different order.

**Why This Workshop:**
- Wants to save time with module collections
- Needs to share reproducible environments with lab members
- Confused about whether to use R packages vs. conda vs. pip

**What They'll Get:**
- Ability to save `module restore genomics` instead of manually reloading
- Knowledge of `environment.yml` for sharing Python environments
- Understanding of job script best practices to ensure reproducibility

---

## Profile 2: Jordan, Postdoc in Physics

**Background:** Computational physicist, very strong Linux/HPC skills, uses quantum chemistry software

**Current Challenge:**
Jordan runs quantum chemistry simulations with GAMESS and Gaussian, followed by Python analysis. They've written sophisticated job scripts but still experience occasional issues:
- Sometimes GAMESS fails with cryptic errors about library versions
- Different job results depending on which module versions load (by default vs. explicitly)
- Wants to create reusable, shareable analysis pipelines that colleagues can run identically

**Why This Workshop:**
- Has intermediate HPC knowledge but wants to optimize workflows
- Frustrated by environment-related debugging
- Wants to understand best practices for complex multi-software pipelines

**What They'll Get:**
- Deep understanding of module dependencies and version pinning
- Knowledge of conda for reproducible Python analysis stacks
- Job script patterns that work across different users and times

---

## Profile 3: Sam, Undergraduate in Data Science

**Background:** One semester of Python experience, no HPC experience, never used Linux before workshop

**Current Challenge:**
Sam needs to use machine learning libraries (TensorFlow, scikit-learn) on Sagehen for a research project but:
- Doesn't understand what "modules" are or why they're necessary
- Installed packages with `pip install --user` globally and now confused about versions
- Script works on their laptop but fails on Sagehen

**Why This Workshop:**
- Complete beginner who needs foundational understanding
- Wants to create a single reproducible environment for their project
- Needs confidence that their setup is correct

**What They'll Get:**
- Clear explanation of why environment management is important
- Step-by-step conda environment setup for machine learning
- Templates for reproducible job scripts
- Understanding of when to ask for help (its-hpc@pomona.edu)

---

## Accommodating Different Profiles

This workshop is designed to serve all three profiles simultaneously:

**For Alex (intermediate):**
- Episodes 1-3 review and solidify existing knowledge
- Episodes 4-6 address specific gaps (conda, best practices)
- Challenge exercises have increasing difficulty

**For Jordan (advanced):**
- Episodes 1-2 are familiar; can be quickly reviewed
- Episodes 3-6 provide structure and best practices for complex workflows
- Instructor notes and references serve as documentation
- Challenge exercises allow deeper exploration

**For Sam (beginner):**
- Episodes 1-2 provide essential foundation
- Episode 4 (conda) is most relevant for their Python work
- Ample time for questions and clarification
- Reference card serves as long-term resource

## Timing Flexibilities

**For pure beginners (Sam):**
- Spend extra time on episodes 1-2
- Skip episode 5 (pip/venv) if time is short
- Focus on episode 4 (conda) and episode 6 (best practices)
- Allow 3 hours instead of 2.5 hours

**For intermediate (Alex):**
- Episodes 1-2 quick review (15 min instead of 40 min)
- Full depth on episodes 3-6 (120 min)
- Discuss real-world genomics workflows
- Allow 2.5 hours as planned

**For advanced (Jordan):**
- Episodes 1-3 brief review (20 min)
- Episodes 4-6 with depth and discussion of edge cases (60 min)
- Time for discussion about their specific use cases
- Use full 2.5 hours for in-depth problem-solving

## Pre-Workshop Assessment

To gauge learner profiles, send a survey before the workshop:

1. How would you describe your Linux experience?
   - Never used it / Some experience / Comfortable / Advanced

2. Have you used HPC clusters before?
   - No / Yes, but briefly / Yes, regularly / Yes, extensively

3. Which software do you primarily use?
   - Python / R / Other compiled software / Multiple languages

4. What's your main goal for this workshop?
   - Understand modules / Manage Python environments / Reproducibility / Advanced optimization

Based on responses, you can:
- Assign reading materials pre-workshop
- Create advanced/beginner breakout groups
- Prepare specific examples relevant to audience
- Adjust pacing and focus
