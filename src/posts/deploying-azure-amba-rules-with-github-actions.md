---
title: Deploying Azure AMBA rules with GitHub Actions
date: 2026-03-09
description: Prerequisites for deploying Azure Monitor Baseline Alerts (AMBA) at management-group scope with GitHub Actions — management groups, an Entra ID app, RBAC, and repository secrets.
tags:
  - azure
  - github-actions
  - amba
  - monitoring
image: /assets/images/deploying-azure-amba-rules-with-github-actions/Screenshot-2026-02-17-at-21.38.28.png
---
In preparation for a workshop, I am creating prerequisites for participants to prepare before startup. The setup is based on [azure-monitor-baseline-alerts](https://github.com/Azure/azure-monitor-baseline-alerts) on GitHub. This is the template supplied from Microsoft and all you need to set up AMBA rules can be found there. 

In this workshop we will use two GitHub Actions workflows: 

-   Deploy-AMBA 

-   Monitor-Baseline-Alerts-(AMBA)-Plan 

These workflows either deploy, or plan Azure Monitor Baseline Alerts (AMBA) into your Azure tenant. To make them work safely and consistently, you must prepare: 

1.  Azure management‑group and subscription structure 

2.  An Entra ID application (service principal) for automation 

3.  Correct role assignments (RBAC) in Azure 

4.  GitHub repository access and secrets for the workflows 

## Azure Management Groups and Subscriptions

For the workshop a management group structure with the following structure is required: 

-   **Tenant Root Group**  
    -   **Corporate** (management group)
        -   **PaygoDev** (subscription) 
        -   **PayGoProd** (subscription) 

It works fine with only have 1 subscription. 

Why this matters for the workflows 

-   Deploy-AMBA.yml and Monitor-Baseline-Alerts-(AMBA)-Plan.yml are designed to work at **management group scope**. 

-   Having subscriptions under a management group allows you to deploy AMBA at scale. Any new subscriptions added later will get the same rules. 

![](/assets/images/deploying-azure-amba-rules-with-github-actions/Screenshot-2026-02-17-at-21.38.28.png)

## Entra ID Application (Service Principal)

The workflows authenticate to Azure using an Entra ID app registration with a service principal. 

The following will need to be created: 

In Entra ID: 

1.  **App registration**  

[https://learn.microsoft.com/en-us/entra/identity-platform/quickstart-register-app](https://learn.microsoft.com/en-us/entra/identity-platform/quickstart-register-app)

2.  **Service principal** for the app in the tenant (created automatically when you register the app using the portal). 

3.  Configure a federated identity credential on an app: [GitHub actions deploying Azure resources](https://learn.microsoft.com/en-us/entra/workload-id/workload-identity-federation-create-trust?pivots=identity-wif-apps-methods-azp#github-actions). 

![](/assets/images/deploying-azure-amba-rules-with-github-actions/Screenshot-2026-02-17-at-21.40.49.png)

What you need to write down 

-   Tenant ID 

-   Subscription ID (Prod subscription) 

-   Application (Client) ID 

These values go into **GitHub Secrets** (see section 4), **not** in the Bicep files or the workflows. 

## Role Assignments (Azure RBAC)

The service principal must have permissions Owner on management group level. This may seem excessive, but the policies add role permissions. This makes Owner permission a requirement.  

Recommend you can set a condition that excludes roles: 

Owner, User Access Administrator, Role Based Access Control Administrator. 

I tried including only the needed roles, but there are many rules in use, and this will most likely be high maintenance as more roles might be included later. 

## GitHub Repository and Secrets

Fork [https://github.com/Eide-consulting/AMBA-workshop](https://github.com/Eide-consulting/AMBA-workshop) to your personal GitHub account  

Notice the workflows located in: 

-   .github/workflows/Deploy-AMBA.yml 

-   .github/workflows/Monitor-Baseline-Alerts-(AMBA)-Plan.yml 

Secrets required for the workflows 

In the GitHub repository: 

1.  Go to **Settings → Secrets and variables → Actions**. 

2.  Press the Manage environment secrets 

3.  Create a new environment called “production” 

4.  Create the following **environment secrets:** 

-   AZURE\_TENANT\_ID 

Entra tenant ID. 

-   AZURE\_SUBSCRIPTION\_ID 

The dev/test subscription used for the workshop. 

-   AZURE\_CLIENT\_ID 

The app registration’s Application (client) ID. 

**You must never commit these values to the code in the repo.** 

Secrets are only available to workflows; they are masked in logs. 

![](/assets/images/deploying-azure-amba-rules-with-github-actions/Screenshot-2026-02-14-at-20.37.09.png)

![](/assets/images/deploying-azure-amba-rules-with-github-actions/Screenshot-2026-02-14-at-20.38.12.png)

With this setup you should be able to run the workflows needed to plan or deploy Azure policies that deploy AMBA rules.
