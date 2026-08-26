# Software Environments and Module Management

## About This Workshop

This is a Carpentries Workbench workshop teaching software and environment management on Sagehen HPC, Pomona College's HPC cluster. It covers Lmod module system fundamentals, conda environment creation, reproducible Python workflows, and best practices for research computing.

**Workshop Duration:** 2.5 hours (90 min teaching + 60 min exercises)
**Level:** Intermediate
**Prerequisites:** Basic Linux command-line knowledge, SSH access to Sagehen

## Learning Outcomes

By the end of this workshop, you will be able to:

- Use the Lmod module system to load, unload, and manage software
- Find and learn about available software modules
- Create and restore personal module collections
- Build isolated Python environments using conda
- Use virtual environments and pip effectively
- Write reproducible, shareable computational workflows
- Maintain professional practices on shared computing systems

## Contents

### Episodes

1. **Lmod Basics** (~20 min) :  Understanding environment modules and core commands
2. **Finding Software** (~20 min) :  Discovering available modules and learning about them
3. **Managing Modules** (~25 min) :  Complex module workflows and saved collections
4. **Conda Environments** (~35 min) :  Creating isolated Python/R environments
5. **Pip and Virtual Environments** (~15 min) :  Alternative package management approaches
6. **Best Practices** (~15 min) :  Reproducibility, documentation, and professional workflows

### Additional Materials

- **Setup Guide** (`learners/setup.md`) :  Prerequisites and connection instructions
- **Quick Reference** (`learners/reference.md`) :  Cheat sheet of commands and patterns
- **Instructor Notes** (`instructors/instructor-notes.md`) :  Teaching tips, timing, troubleshooting
- **Learner Profiles** (`profiles/learner-profiles.md`) :  Example learners and their needs

## About Sagehen HPC

Sagehen is Pomona College's high-performance computing cluster managed by Information Technology Services (ITS). Key characteristics:

- **Job Scheduler:** SLURM
- **Module System:** Lmod
- **Key Software:** R, Python (via miniconda3), GCC, Gaussian, GAMESS, BLAST, samtools, rclone
- **Contact:** its-hpc@pomona.edu
- **Maintained by:** Andrew Wilson, Director of Research Computing, ITS

## Getting Started

### For Learners

1. **Ensure SSH access** to sagehen.hpc.pomona.edu
2. **Read** `learners/setup.md` for connection instructions
3. **Follow along** with each episode, working through challenges
4. **Bookmark** `learners/reference.md` for future use

### For Instructors

1. **Review** `instructors/instructor-notes.md` for teaching guidance
2. **Verify** all Sagehen modules are current and accessible
3. **Test** conda and module commands on a live account
4. **Prepare** an example conda environment for live demos
5. **Run through** all challenges in advance

## Using This Material

### Online

This workshop is hosted as a Carpentries Workbench site. Browse episodes online, or download the full repository.

### Offline/Self-Paced

Clone the repository and work through episodes locally. All content is in Markdown format and readable as plain text.

### In-Person Workshop

- **Recommended Setup:** Instructor with projection + participants with individual laptops/terminal access
- **Timing:** 2.5 hours total; adjust for your audience's experience level
- **Pacing:** Allow breaks every 30 minutes
- **Hands-on:** Participants should code along with each example

## Key Resources

- [Sagehen User Guide](https://pomona-college.github.io/)
- [Lmod Documentation](https://lmod.readthedocs.io/)
- [Conda User Guide](https://docs.conda.io/projects/conda/en/latest/user-guide.html)
- [SLURM Documentation](https://slurm.schedmd.com/)

## License

This workshop is licensed under CC-BY 4.0. You are free to use, modify, and distribute this material with attribution.

## Contributing

Found an error? Want to improve the content? Contributions are welcome.

Contact: its-hpc@pomona.edu

## Workshop Development

**Created:** March 5, 2026
**Authors:** Pomona College ITS Research Computing Team
**Format:** Carpentries Workbench
**Version:** 1.0 (Pre-alpha)
**Life Cycle:** Pre-alpha :  actively being tested and refined

## Feedback

We'd love to hear from workshop participants! After completing this workshop:

- What was most helpful?
- What was confusing?
- What topics would you like more depth on?
- Did it help your research computing?

Email feedback to its-hpc@pomona.edu with "Workshop Feedback" in the subject line.

---

**Sagehen HPC Cluster**
Pomona College Information Technology Services

## Acknowledgments

**Andrew Wilson** — Director of Research Computing and Digital Scholarship,
Pomona College. Workshop design and development.

**Andrei Motchenko** — testing, editing, cleanup and screenshots across the
Pomona College HPC Workshop Series.
