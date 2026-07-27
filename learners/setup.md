---
title: Setup
---

## Prerequisites

To participate in this workshop, you need:

1. **An active Sagehen account** with SSH access
2. **SSH client** installed on your computer
3. **A text editor** (vi, nano, or VS Code with remote SSH)
4. **Basic command-line familiarity**

## Connecting to Sagehen

### macOS and Linux

Open Terminal and connect:

```bash
$ ssh username@sagehen.hpc.pomona.edu
```

Replace `username` with your Pomona account username.

### Windows

Use one of these tools:
- **PowerShell** (Windows 10+): Same command as above
- **PuTTY**: Download from https://www.putty.org/
- **Windows Subsystem for Linux (WSL)**: Install and use native SSH

Connect with PowerShell:

```powershell
PS> ssh username@sagehen.hpc.pomona.edu
```

## Verifying Your Setup

Once connected to Sagehen, verify the module system works:

```bash
$ module avail
```

You should see a long list of available modules organized by category. If you see an error like "module: command not found", contact its-hpc@pomona.edu.

### Check Your Home Directory

```bash
$ pwd
/home/username
$ ls -la
```

You should see your home directory with standard directories like `.bashrc`, `.bash_profile`, etc.

## Creating a Workshop Directory

Create a space for this workshop:

```bash
$ mkdir -p workshop/software-environments
$ cd workshop/software-environments
$ pwd
/home/username/workshop/software-environments
```

Use this directory to save environment files and test scripts from the workshop.

## No Special Installation Needed

**Important:** You do NOT need to install anything beforehand. All software (R, Python via miniconda3, compilers, etc.) is available as modules on Sagehen. This workshop teaches you how to use the existing software.

## Getting Help

If you have connection issues:
- Check your internet connection
- Verify you're using the correct username
- Confirm you're within Pomona's network or using VPN
- Email its-hpc@pomona.edu with your username and "SSH connection issue"

If you need your password reset:
- Use Pomona's self-service password tool
- Or contact its-hpc@pomona.edu

## Ready to Go!

If you can SSH to sagehen.hpc.pomona.edu and run `module avail`, you're ready to start the workshop.
