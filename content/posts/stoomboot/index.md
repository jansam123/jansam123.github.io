---
title: "Nikhef Stoomboot"
date: 2022-09-18T08:06:25+06:00
description: "Tips and tricks for using the Nikhef Stoomboot cluster for ATLAS work"
theme: Toha
menu:
  sidebar:
    name: Nikhef Stoomboot
    identifier: stoomboot
    weight: 994
hero: stoomboot.jpeg  
---
# Nikhef Stoomboot tips and tricks

## Copying from `/eos`
Use the following to copy from `/eos` in interactive session (good for small data):
```bash
setupATLAS
lsetup xrootd
lsetup xcache
voms-proxy-info -all
xrdcp --streams=4 --parallel=8 -r root://eosatlas.cern.ch//eos/atlas/atlaslocalgroupdisk/higgs/HHbbtautau/Run2Run3/EasyJetNTuples/Prod_v7_merged/HadHad /dcache/atlas/sjankovy/HHbbtautau/Prod_v7_merged
```

### Setup for submitting jobs to copy from `/eos`
1. Prerequisites. Run these in terminal:
```bash
setupATLAS
lsetup rucio
voms-proxy-init -voms atlas -valid 192:00 -out ~/.globus/proxies/x509proxy
```
Be aware that the proxy is valid only for limited time (192 hours in this case), but the jobs can't run longer than that anyway. You might need to resubmit the jobs after the jobs reach maximum runtime.
1. Create `copy_job.sh` with the following content. `/eos/atlas/atlaslocalgroupdisk/xxx` is the source path and `/dcache/atlas/${USER}/xxx` is the destination path - make sure to put you big data to `/dcache`. If you can't access the folder, write to `stbc-admin@nikhef.nl` to get access.

```bash
#!/bin/bash
export ATLAS_LOCAL_ROOT_BASE=/cvmfs/atlas.cern.ch/repo/ATLASLocalRootBase
source ${ATLAS_LOCAL_ROOT_BASE}/user/atlasLocalSetup.sh
# run this before submitting the job: `voms-proxy-init -voms atlas -valid 192:00 -out ~/.globus/proxies/x509proxy
export X509_USER_PROXY=/user/sjankovy/.globus/proxies/x509proxy
lsetup xrootd
lsetup xcache
voms-proxy-info -all
xrdcp --streams=4 --parallel=8 -r root://eosatlas.cern.ch//eos/atlas/atlaslocalgroupdisk/xxx /dcache/atlas/${USER}/xxx

```

3. Create `copy_sub.sh` with the following content:

```bash
#!/bin/bash
# HTCondor Submit File for xrdcp Transfer

# Executable script to run
executable = copy_job.sh

# Output, error, and log files
output = out/copy_$(Cluster).$(Process).log
error = err/copy_$(Cluster).$(Process).log
log = log/copy_$(Cluster).log

# Enable streaming for real-time output (requires HTCondor 8.9+)
stream_output = True
stream_error = True

# Request resources
request_cpus = 8
request_memory = 16GB

# Requirements and preferences
+JobCategory            = "long"
+UseOS                  = "el9"
# Number of jobs to submit
queue 1
```

4. Create directories for logs:
```bash
mkdir out err log
```

5. Submit the job:
```bash
condor_submit copy_sub.sh
```


