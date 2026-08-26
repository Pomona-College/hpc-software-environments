---
title: Setup
---

## Prerequisites

To participate in this workshop, you need:

1. **An active Sagehen HPC account** with SSH access
2. **SSH client** installed on your computer
3. **A text editor** (vi, nano, or VS Code with remote SSH)
4. **Basic command-line familiarity**

## Connecting to Sagehen HPC

### macOS and Linux

Open Terminal and connect:

```bash
$ ssh <myusername>@sagehen.hpc.pomona.edu
```

Replace `username` with your Pomona account username.

### Windows

Use one of these tools:

**PowerShell** (Windows 10+): Same command as above

**PuTTY**: Download from https://www.putty.org/

**Windows Subsystem for Linux (WSL)**: Install and use native SSH

Connect with PowerShell:

```powershell
PS> ssh <myusername>@sagehen.hpc.pomona.edu
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
/rhome/<myusername>
$ ls -la
```

You should see your home directory with standard directories like `.bashrc`, `.bash_profile`, etc.

## Creating a Workshop Directory

Create a space for this workshop:

```bash
$ mkdir -p workshop/software-environments
$ cd workshop/software-environments
$ pwd
/rhome/<myusername>/workshop/software-environments
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

<!-- highlight <labname>/<myusername> placeholders in code blocks; remove if the varnish theme handles this natively -->
<script>(function(){var CSS='.sh-placeholder{color:#c2410c;font-weight:700}[data-bs-theme="dark"] .sh-placeholder,html.dark .sh-placeholder{color:#fdba74}@media (prefers-color-scheme: dark){[data-bs-theme="auto"] .sh-placeholder{color:#fdba74}}';var RX=/<labname>|<myusername>/g;function firstMatch(el){var w=document.createTreeWalker(el,NodeFilter.SHOW_TEXT,null),nodes=[],full='';while(w.nextNode()){nodes.push({n:w.currentNode,s:full.length});full+=w.currentNode.nodeValue;}RX.lastIndex=0;var m;while((m=RX.exec(full))){var s=m.index,e=s+m[0].length,inSpan=false,parts=[];for(var j=0;j<nodes.length;j++){var ns=nodes[j].s,ne=ns+nodes[j].n.nodeValue.length;if(ne<=s||ns>=e)continue;parts.push({node:nodes[j].n,a:Math.max(s-ns,0),b:Math.min(e-ns,nodes[j].n.nodeValue.length)});var p=nodes[j].n.parentNode;while(p&&p!==el){if(p.classList&&p.classList.contains('sh-placeholder')){inSpan=true;break;}p=p.parentNode;}}if(!inSpan&&parts.length)return parts;}return null;}function wrapParts(parts){for(var i=parts.length-1;i>=0;i--){var t=parts[i].node,txt=t.nodeValue,a=parts[i].a,b=parts[i].b;var span=document.createElement('span');span.className='sh-placeholder';span.textContent=txt.slice(a,b);var f=document.createDocumentFragment();if(a>0)f.appendChild(document.createTextNode(txt.slice(0,a)));f.appendChild(span);if(b<txt.length)f.appendChild(document.createTextNode(txt.slice(b)));t.parentNode.replaceChild(f,t);}}function run(){var st=document.createElement('style');st.textContent=CSS;document.head.appendChild(st);document.querySelectorAll('pre,code').forEach(function(el){var guard=0,parts;while((parts=firstMatch(el))&&guard++<500){wrapParts(parts);}});}if(document.readyState==='loading'){document.addEventListener('DOMContentLoaded',run);}else{run();}})();</script>
