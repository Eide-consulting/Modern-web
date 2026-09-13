---
title: Tags on Hybrid machines in Azure GUI error
date: 2025-12-07
description: Why an Azure Arc machine tag can look correct in the portal but still trigger alerts — and how to set it reliably with PowerShell.
tags:
  - azure
  - azure-arc
  - powershell
  - monitoring
image: /assets/images/tags-on-hybrid-machines-in-azure-gui-error/Screenshot-2025-12-07-at-20.38.03.png
---
We use **Azure Monitor** to track metrics on servers in the local data center that are enrolled in **Azure Arc**. Servers that should not be monitored can be excluded by setting the tag **`MonitorDisable`** to `true`. For a single server, this is usually easiest to do in the **Azure portal**.  
However, we have observed cases where the tag appears correctly in the portal, but alerts are still triggered.  
Running this query in Azure Resource graph explorer reveals that the tag is not set as expected:  

```kql
resources
| where type =~ "Microsoft.HybridCompute/machines"
//if the problem is Azure native VMs:
//| where type =~ "Microsoft.compute/virtualmachines"
| where tags.["MonitorDisable"] !in~ ("true", "Test", "Dev", "Sandbox")
| project name, resourceTags = tags
```

Here is a example shown where tag is set ok, note that _**`in~`**_ is used instead of _**!`in~`**_. Changing this can confirm whether the tag is set or not

![](/assets/images/tags-on-hybrid-machines-in-azure-gui-error/Screenshot-2025-12-07-at-20.38.03.png)

If it does not show up in the list, the tag is not set correctly, even if the GUI says otherwise.

We have found that setting the tag via **PowerShell** works best. In the portal, you may need to apply the tag multiple times before it takes effect.

```powershell
# Description: This script adds or updates a tag on an Azure Arc connected VM using PowerShell.
$vmName = "YourVMName"
$tagName = "TagName"
$tagValue = "TagValue"
$resourceGroup = "ResourceGroup"
# Get the VM
$vmResource = Get-AzConnectedMachine -ResourceGroupName $resourceGroup -Name $vmName
# Add or update the tag using Update-AzTag
Update-AzTag -ResourceId $vmResource.Id -Tag @{ $tagName = $tagValue } -Operation Merge
Write-Output "Tag '$tagName' with value '$tagValue' has been added/updated on VM '$vmName'."
# Operation "Merge" ensures that existing tags are preserved and only the specified tag is added or updated.
# To delete tag use -Operation Delete instead of Merge
# To replace all tags use -Operation Replace instead of Merge
# Note: Make sure you have the Az.ConnectedMachine module installed and imported in your PowerShell session.
# You can install it using: Install-Module -Name Az.ConnectedMachine
# Also, ensure you are logged in to your Azure account using Connect-AzAccount before running this script.
# Replace "TagName", "TagValue", "ResourceGroup", and "$vmName" with your desired tag name, tag value, resource group name, and VM name respectively.
```

Once the script is run, the tag should show up i both the GUI and in Resource graph explorer, and the alert should resolve.
