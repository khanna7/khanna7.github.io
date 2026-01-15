---
layout: post
title: "VS Code, renv, and R Versions on OSCAR (Brown's HPC)"
tags: [R, renv, VSCode, HPC]
---

### Problem s
OSCAR recently upgraded to RHEL 9.6.

I had to reinstall the r environment.

I thought it might be a good idea to additionally upgrade to r/4.5.1 since that is the default r on the system. And keeping pace with later R sometimes has downstream benefits (sometimes not).


### Solution

To do this correctly, remember to update the `launch_R.sh` ececutable file at `/oscar/home/akhann16/code/net-ergm-v4plus/launch_R.sh`.

WHen upgrading the environment, the userterms packages may need to be installed locally ()

```
install.packages("/oscar/home/akhann16/code/net-ergm-v4plus/ergm.userterms.hepcep", type="source", repos=NULL)
```

and

```
install.packages("/oscar/home/akhann16/code/net-ergm-v4plus/ergm.userterms", type="source", repos=NULL)
```

Since the first is purely local, and the second is currently off of CRAN

Note the updated launch_R.sh:

```
#!/bin/bash
# Wrapper to launch R in VS Code with correct shared libraries

# Make 'module' available in non-interactive shells (VS Code often runs this way)
source /etc/profile.d/modules.sh

module purge 

# Load the R module
 #OLD R MODULE module load r/4.4.0-yycctsj
 module load r/4.5.1-iikl
 

# Optional: activate renv if needed here instead of .Rprofile
# source ./renv/activate.R

# Launch R
# exec /oscar/rt/9.2/software/0.20-generic/0.20.1/opt/spack/linux-rhel9-x86_64_v3/gcc-11.3.1/r-4.4.0-yycctsjvszuj5o2q4gfbaehsq7rkl4bz/bin/R --no-save --no-restore "$@"

# Launch R from PATH (provided by the module)
exec R --no-save --no-restore "$@"
```



