---
title: "Tips and Tricks"
date: 2022-09-18T08:06:25+06:00
description: "A collection of useful tips and tricks."
theme: Toha
menu:
  sidebar:
    name: Tips and Tricks
    identifier: tips_and_tricks
    weight: 6
---
# Tips and Tricks

## [Git+latexdiff](https://twiki.cern.ch/twiki/bin/viewauth/AtlasProtected/PubComLaTeXDiff#git_latexdiff_command)
Generate a latexdiff between two commits
```bash
git latexdiff 0db4b84289f7554810cfdaacf7bafdbab23466b2 -- --main ANA-JETM-2023-07-PUB.tex
```