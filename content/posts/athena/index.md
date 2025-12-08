---
title: "Running DAOD production with Athena"
date: 2022-09-18T08:06:25+06:00
# hero: /images/posts/writing-posts/analytics.svg
description: "Setup a working environment for running DAOD production with Athena"
theme: Toha
menu:
  sidebar:
    name: Athena
    identifier: athena
    weight: 996
hero: daod.png
---

## Prerequisites
1. Get a CERN login.
2. Generate a [Kerberos](https://linux.web.cern.ch/docs/kerberos-access/) authentication ticket using the command and type your password when prompted: 
```bash
kinit
```
Check if the ticket is generated using:
```bash
klist
klist -f
```
3. Create a fork of the [Athena repository](https://gitlab.cern.ch/atlas/athena). THE PROJECT NAME MUST BE AS THE ORIGINAL: `athena`.

## Setup a working environment
4. ssh to lxplus or other CERN machine.
5. Setup the environment:
```bash
setupATLAS 
lsetup git
```
6. Setup the git-atlas (change `sjankovy` to your username):
```bash
git atlas init-config sjankovy --apply
```
Check if the configuration is correct with `git atlas init-config`.

7. Create your own working area:
```bash
mkdir WorkingArea
cd WorkingArea
```

8. Get the parts of repository you need. For example, if you want to work with BoostedJetTaggers, JetMomentTools, JetRecConfig and DerivationFramework:
```bash
git atlas init-workdir https://:@gitlab.cern.ch:8443/atlas/athena.git -p BoostedJetTaggers  JetMomentTools  JetRecConfig DerivationFrameworkPhys DerivationFrameworkJetEtMiss
```

9. Build the code:
```bash
mkdir build
cd build
asetup Athena,main,latest
cmake ../athena/Projects/WorkDir/
make
```

10. If you wish to restore the working environment, use:
```bash
cd build/
setupATLAS
asetup --restore
source x86*/setup.sh
cd ../run/
```
If you change something in the athena code, you need to recompile it. You can do it by running:
```bash
cd build/
make
```

11. Run the derivation code:
```bash
Derivation_tf.py --CA --inputAODFile ../data/mc20_13TeV.364704.Pythia8EvtGen_A14NNPDF23LO_jetjet_JZ4WithSW.recon.AOD.e7142_s3681_r13144.AOD.27464218._000535.pool.root.1 --outputDAODFile output.pool.root --formats JETM2 --maxEvents 10
```
