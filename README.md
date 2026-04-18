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


## 2. Identify Threats (What Could Go Wrong?)

Threats answer the fundamental question: **“What could realistically go wrong?”**

### How to Think About Threats
To identify threats effectively, ask yourself:
* **Who** would attack this?
* **How** would they access it?
* **What** attacks are common in the wild?

> [!IMPORTANT]
> Use real SOC (Security Operations Center) threat patterns, not "movie hackers." These threats are realistic and encountered daily by security professionals.

### Identified Threats
| Asset | Threat Scenario |
| :--- | :--- |
| **Public RDP** | Internet brute-force attacks |
| **Admin account** | Password compromise |
| **OS** | Exploitation of known vulnerabilities |
| **Azure account** | Unauthorized configuration changes |
| **Logs** | Undetected malicious activity |

*Note: Threats can be both external and internal. GRC addresses both aspects.*

---

## 3. Identify Vulnerabilities (Why the Threat Could Work)

Vulnerabilities are **weaknesses**, not attacks themselves. If something can be abused, it represents a vulnerability—you don't need to exploit it to prove it exists.

### How to Identify Vulnerabilities

#### A. Review Azure NSGs (Network Security Groups)
1. Navigate to **GRC-WIN-VM01** → **Networking** → **Network Settings**.
2. Verify if an NSG is attached to the network interface or subnet.
3. Check for **Inbound Port Rules**:
    * **Port:** RDP (TCP 3389)
    * **Source:** `Any` or `Internet`
    * **Action:** `Allow`

> [!WARNING]
> Allowing RDP access from the internet significantly increases the risk of brute-force attacks and must be documented as a high security risk.

   ![images alt](https://github.com/salimelh94/Cloud-Security-Risk-Assessment-GRC-Simulation-Azure-/blob/ae86bde367943c3ec3d454c5a4c37dbbf3ad6487/images/3-1.png)

#### B. Check the Authentication Method
## (OS-Level)
Inside the VM, open **Computer Management** → **Local Users and Groups** → **Users**:

* **Confirm:** `azureadmin` exists.
* **Confirm:** It is a member of the **Administrators** group.

 ![images alt](https://github.com/salimelh94/Cloud-Security-Risk-Assessment-GRC-Simulation-Azure-/blob/ae86bde367943c3ec3d454c5a4c37dbbf3ad6487/images/3-2.png)

#### C. Confirm Monitoring is Enabled

* **Azure Monitoring:** Go to **Monitoring** → **Insights**. If basic performance data isn't visible, Azure will prompt to enable it.

  ![images alt](https://github.com/salimelh94/Cloud-Security-Risk-Assessment-GRC-Simulation-Azure-/blob/ae86bde367943c3ec3d454c5a4c37dbbf3ad6487/images/3-3.png)
  
* **OS-Level Logging:**
   Open **Event Viewer** → **Windows Logs** → **Security**.
  
   Confirm that events are being generated.
   ![images alt](https://github.com/salimelh94/Cloud-Security-Risk-Assessment-GRC-Simulation-Azure-/blob/ae86bde367943c3ec3d454c5a4c37dbbf3ad6487/images/3-4.png)

#### D. Default OS Configuration
This assessment assumes a standard **Windows Server 2022** setup with no additional hardening:
* No custom Group Policies or CIS benchmarks.
* Windows Defender and Firewall are on **default settings**.
* No host-based security measures beyond the OS defaults.

### Identified Vulnerabilities
| Asset | Vulnerability | Why It Matters |
| :--- | :--- | :--- |
| **Virtual Machine (VM)** | RDP exposed to the internet | Increases attack surface |
| **Authentication** | No MFA (Multi-Factor) | High risk of password compromise |
| **Operating System** | Default hardening | Common target for automated exploits |
| **Monitoring** | No SIEM integration | Attacks may go completely unnoticed |
| **Identity** | Single admin account | No accountability or redundancy |




















