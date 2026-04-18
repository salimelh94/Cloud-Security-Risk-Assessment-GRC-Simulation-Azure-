# Cloud Security Risk Assessment & GRC Simulation (Azure)
Real-world cloud security risk assessment from a Governance, Risk, and Compliance (GRC) perspective using a simple Azure-based lab environment simulation


![images alt](https://github.com/salimelh94/Cloud-Security-Risk-Assessment-GRC-Simulation-Azure-/blob/71de4489a2a2239daaf376572fa8f623f53948b9/images/O5ROW.jpg)


# Required Tools / Prerequisites

To complete this project, learners will need:

* Microsoft Azure account (free tier)
* One Azure virtual machine (Windows Server or Windows 10/11)
* Excel or Google Sheets
* Any text editor (Word, Google Docs, etc.)
* Basic understanding of computers and the internet
* Basic knowledge of cybersecurity concepts (accounts, passwords, updates)
* No prior cloud or GRC experience required

## Setting Up the Environment

Before starting this lab, you must create an Azure free account.

1. Go to https://azure.microsoft.com/free
2. Click Try Azure for Free

![images alt](https://github.com/salimelh94/Cloud-Security-Risk-Assessment-GRC-Simulation-Azure-/blob/71de4489a2a2239daaf376572fa8f623f53948b9/images/1.png)

   
3. Sign in with a Microsoft account (create one if needed)
4. Complete identity verification:
    * Supported credit or debit card (no charges unless you upgrade)
    * (Visa, Mastercard, American Express, JCB).
    * Phone number (SMS verification)
5. After signing up, access the Azure Portal at https://portal.azure.com

> Note: Azure’s free tier provides enough credits to complete this lab. If you encounter region availability or VM quota issues, choose a nearby region or a smaller VM size.

## Lab Environment Setup

### 1. Search for and Create a Resource Group

  ![images alt](https://github.com/salimelh94/Cloud-Security-Risk-Assessment-GRC-Simulation-Azure-/blob/71de4489a2a2239daaf376572fa8f623f53948b9/images/2.png)


Create a dedicated container for all lab resources.

**Azure Portal → Resource groups → + Create**

* Subscription: Default
* Resource group name: GRC-Lab
* Region: Nearest region (e.g., Southeast Asia, East US)

Click Review + Create → Create

![images alt](https://github.com/salimelh94/Cloud-Security-Risk-Assessment-GRC-Simulation-Azure-/blob/71de4489a2a2239daaf376572fa8f623f53948b9/images/3.png)


### 2. Create the Virtual Machine

**Azure Portal → Create a Resource → Virtual Machines**

  ![images alt](https://github.com/salimelh94/Cloud-Security-Risk-Assessment-GRC-Simulation-Azure-/blob/71de4489a2a2239daaf376572fa8f623f53948b9/images/4.png)


* Subscription: Default
* Resource group: GRC-Lab
* VM name: GRC-WIN-VM01
* Region: your nearest region (e.g., Southeast Asia)
* Image: Windows Server 2022 Datacenter
* Size: B1ms or B1s (low-cost, sufficient for lab use)

**Administrator Account**

* Username: azureadmin
* Password: Strong password (save it)

**Inbound Ports**

* Public inbound ports: Allow selected ports
* Allowed ports: RDP (3389)

Note: This intentional exposure will be documented later as a security risk.

Click Next: Disks

### 3. Disks Setup

* OS disk: Standard SSD

  ![images alt](https://github.com/salimelh94/Cloud-Security-Risk-Assessment-GRC-Simulation-Azure-/blob/71de4489a2a2239daaf376572fa8f623f53948b9/images/5.png)

Leave the setup to defaults → Next: Networking

### 4. Networking Setup

Azure will auto-create:

* Virtual Network (VNet)
* Subnet
* Network Security Group (NSG)
* Public IP

Leave defaults, but note:

* Public IP is enabled
* RDP is open to the internet

Click Review + Create → Create

  ![images alt](https://github.com/salimelh94/Cloud-Security-Risk-Assessment-GRC-Simulation-Azure-/blob/71de4489a2a2239daaf376572fa8f623f53948b9/images/6.png)

### 5. Connect to the Virtual Machine (VM)

**Virtual machines → GRC-WIN-VM01 → Connect → RDP**

![images alt](https://github.com/salimelh94/Cloud-Security-Risk-Assessment-GRC-Simulation-Azure-/blob/71de4489a2a2239daaf376572fa8f623f53948b9/images/7.png)

* Download RDP file
* Log in with your created account:
    * Username: azureadmin
    * Password: (password)

In GRC, you define the scope first. This VM is now your entire assessment scope, just as a real risk assessment for a small cloud workload would be.


# Instructions

## Step 1: Identify Assets, Threats, and Vulnerabilities (Core GRC Skill)

This is the most important step in GRC. If Step 1 is weak, everything else falls apart. We’ll do this in three layers: Assets, Threats, and Vulnerabilities.

This step answers three critical questions:

* What do we care about?
* What could harm it?
* Why could that harm succeed?

### 1. Identify Assets (What Are We Protecting?)

Think: “What would hurt if compromised?”

#### How to Do This

1. **Navigate to:**
    * Virtual Machines → GRC-WIN-VM01

2. **Look at:**

    **A. Settings → Operating System**

      ![images alt](https://github.com/salimelh94/Cloud-Security-Risk-Assessment-GRC-Simulation-Azure-/blob/00ceb8ea2ba7259b75659f8c6f66ead676400784/images/2-1.png)
   
    Confirm the operating system running on the VM for asset inventory and risk classification.

    **B. Networking → Network Setting → Rules → Open ports**

     ![images alt](https://github.com/salimelh94/Cloud-Security-Risk-Assessment-GRC-Simulation-Azure-/blob/00ceb8ea2ba7259b75659f8c6f66ead676400784/images/2-2.png)

    Identify externally accessible services that increase the attack surface.

    **C. Admin Accounts: Operations → Run Command → Choose RunPowerShellScript**
    Type `Get-LocalUser` and click **Run**.
   
     ![images alt](https://github.com/salimelh94/Cloud-Security-Risk-Assessment-GRC-Simulation-Azure-/blob/00ceb8ea2ba7259b75659f8c6f66ead676400784/images/2-3.png)

     Identify privileged accounts with elevated access to the system.

    **D. Public Exposure**

     ![images alt](https://github.com/salimelh94/Cloud-Security-Risk-Assessment-GRC-Simulation-Azure-/blob/00ceb8ea2ba7259b75659f8c6f66ead676400784/images/2-4.png)

    Determine whether the VM is accessible from the internet.

Think beyond hardware. **Assets include anything attackers want or can abuse.**

### Identified Assets

| Asset Category | Asset | Why It Matters |
| :--- | :--- | :--- |
| Compute | Azure VM (Windows Server 2022) | Hosts data & services |
| Identity | Local Admin Account | Full system control |
| Network | Public IP & RDP Port | Entry point for attackers |
| Cloud Control | Azure Subscription | Management & billing access |
| OS | Windows Services | Vulnerable if unpatched |

If it holds value, grants access, or provides privilege, it’s an asset. Assets are not limited to servers; accounts, access pathways, and cloud permissions are also considered assets.























