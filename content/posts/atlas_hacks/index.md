---
title: "ATLAS hacks"
date: 2022-11-16T21:16:00+06:00
# hero: /images/posts/writing-posts/analytics.svg
description: "Several useful ATLAS tricks to make your life easier"
theme: Toha
menu:
  sidebar:
    name: ATLAS hacks
    identifier: atlas_hacks
    weight: 995
hero: ATLAS_logo.png
---

## Preparing plots for ATLAS publication
All ATLAS plots used for the circulation inside the collaboration must have the "ATLAS Internal" label if you use data or "ATLAS Simulation Internal" if you use only simulation.
However, when preparing plots for publication outside the collaboration, the label "Internal" must be removed for papers, replaced with "Preliminary" for CONF or PUB notes, or replaced with "Work in progress" for thesis/regional conferences.
Instead of manually recreating all PDF plots, where you only change the label, you can use the following code to automate the process for an existing PDF plot.
```bash
pip install -U pymupdf
```

```python
import pymupdf

file_path = "figures/extracted_gluon.pdf" 
save_path = "tmp/extracted_gluon_redacted.pdf"

doc = pymupdf.open(file_path)

needle = "Internal"

for page in doc:
    res = page.search_for(needle)
    
    for r in res:
        page.add_redact_annot(r, text=None)
        page.apply_redactions()
        
doc.save(save_path)
```