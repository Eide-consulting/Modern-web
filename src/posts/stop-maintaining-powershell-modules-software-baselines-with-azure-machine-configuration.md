---
title: "Stop Maintaining PowerShell Modules: Software Baselines with Azure Machine Configuration"
date: 2026-06-08
description: An open-source framework for maintaining Windows software baselines with Azure Machine Configuration and Azure Policy — no custom PowerShell modules to maintain.
tags:
  - azure
  - azure-arc
  - policy
  - devsecops
  - powershell
---
How do you make sure the _right_ software is installed across hundreds of Windows machines — both in Azure and on-prem — without manual scripts drifting out of control?

I built (and just open-sourced) a framework that solves this with **Azure Machine Configuration + Azure Policy**.

The idea: a single, signed manifest is the source of truth. CI verifies the signature, builds a Machine Configuration package, and generates a ready-to-use Azure Policy definition. Deployment is policy-only, rolled out in rings via assignment scope and machine tags.

The best part? Teams can deploy and maintain software baselines **without writing or maintaining custom PowerShell modules, DSC resources, or bespoke install scripts**. You add an entry to the manifest, sign it, and let policy do the rest — far less code to own and a lot less drift to chase.

Security was front and center — every manifest and allowlist is ECDSA-signed, every SHA256 is treated as a control, not just a checksum.

Check it out and let me know what you think 👇  
🔗 [https://github.com/Eide-consulting/azure-machine-configuration-baselines](https://github.com/Eide-consulting/azure-machine-configuration-baselines)

#Azure #AzureArc #DevSecOps #IaC #PowerShell
