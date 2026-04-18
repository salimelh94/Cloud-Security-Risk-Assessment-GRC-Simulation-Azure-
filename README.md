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



## Step 2: Assign Risk Scores (Quantifying Risk)

Risk scoring translates technical weaknesses into a format that leadership can prioritize. Instead of subjective labels, we use a repeatable mathematical model.

### Risk Scoring Method
*   **Likelihood (1-5):** How probable is it that the threat will occur?
*   **Impact (1-5):** The severity of damage if the threat is realized.
*   **Risk Score:** $Likelihood \times Impact$

---

### A. How to Assign Likelihood
Consider these factors:
*   **Exposure:** Is it accessible from the internet (e.g., public IP)?
*   **Threat Prevalence:** Is this a commonly observed attack technique?
*   **Ease of Exploitation:** Can it be automated or does it require advanced skills?
*   **Existing Controls:** Are there any defenses already in place?

### B. How to Assign Impact
Ask the following:
*   Would this result in data loss or exposure?
*   Could it lead to full system or domain compromise?
*   Would it cause service downtime?
*   What is the impact on compliance and reputation?

### Risk Register (Core GRC Artifact)
| Asset | Threat | Likelihood | Impact | Risk Score | Justification |
| :--- | :--- | :---: | :---: | :---: | :--- |
| **RDP** | Brute-force attack | 4 | 4 | **16** | Public Exposure |
| **Admin** | Credential compromise | 3 | 5 | **15** | Full System Control |
| **OS** | Unpatched vulnerabilities | 3 | 4 | **12** | Common Windows Attack |
| **Azure** | Privilege misuse | 2 | 5 | **10** | High Impact |
| **Logs** | Undetected attack | 3 | 3 | **9** | Delayed Responses |

---

## Step 3: Map Controls to Policies & Frameworks

This step ensures every security action is justified and traceable to industry standards like **NIST CSF** or **CIS Controls**.

### Control Types
1.  **Preventive:** Stop attacks before they happen (e.g., MFA).
2.  **Detective:** Identify and alert on malicious activity (e.g., Logging).
3.  **Corrective:** Mitigate impact after an incident (e.g., Patching).

### Control Mapping Table
| Risk | Security Control | Control Type | Framework |
| :--- | :--- | :--- | :--- |
| **RDP attacks** | Restrict IP access | Preventive | CIS 4 |
| **Credential theft** | MFA enforcement | Preventive | NIST PR.AC |
| **OS exploits** | Patch management | Corrective | CIS 7 |
| **No detection** | Centralized logging | Detective | NIST DE.CM |
| **Privilege misuse** | RBAC | Preventive | CIS 5 |

> [!TIP]
> In GRC, **traceability** matters more than the specific tool. Whether you use Azure Native tools or third-party software, the control must fulfill the framework's intent.

---

## Step 4: Produce a GRC Risk Assessment Report

The final stage is consolidating findings into a professional report. This document bridges the gap between technical teams and stakeholders.

### Report Objectives:
*   **Clarity:** Readable by both technical and non-technical staff.
*   **Structure:** Logical flow from Asset identification to Mitigation.
*   **Accountability:** Provides a clear record of security decisions for auditors.


# Risk Assessment Report

## 1. Executive Summary
This report summarizes the findings of a cloud security risk assessment conducted on a single Azure-hosted virtual machine from a **Governance, Risk, and Compliance (GRC)** perspective.

The objective was to identify critical assets, evaluate potential threat scenarios, and quantify risks to recommend controls aligned with industry frameworks. The assessment identified several **medium-to-high risks**, primarily due to:
*   Exposed remote access (RDP).
*   Absence of Multi-Factor Authentication (MFA).
*   Inadequate OS hardening.
*   Limited centralized monitoring.

---

## 2. Scope and Environment
The assessment was performed from a GRC (non-exploitative) perspective, focusing strictly on the following environment:

| Item | Description |
| :--- | :--- |
| **Cloud Provider** | Microsoft Azure |
| **Resource Group** | GRC-Lab |
| **Virtual Machine Name** | GRC-WIN-VM01 |
| **Operating System** | Windows Server 2022 Datacenter |
| **Access Method** | Remote Desktop Protocol (RDP) |
| **Public Exposure** | Yes (Public IP enabled) |
| **Administrative Account** | `azureadmin` |

---

## 3. Asset, Threat, and Vulnerability Summary

| Asset | Threat | Vulnerability | Why it Matters |
| :--- | :--- | :--- | :--- |
| **Azure VM** | Brute-force attack | RDP exposed to the internet | Easy entry point for attackers |
| **Admin Account** | Credential theft | No MFA | Full system compromise possible |
| **OS** | Exploitation | Default hardening | Common Windows attack vectors |
| **Logs** | Undetected attacks | No centralized logging | Delayed detection of incidents |
| **Azure Sub** | Misconfiguration | Single admin | High-impact if misused |

---

## 4. Risk Prioritization
Risks were scored using the **Likelihood × Impact** model ($1 = Low$, $5 = High$).

| Asset | Threat | Likelihood | Impact | Risk Score | Justification |
| :--- | :--- | :---: | :---: | :---: | :--- |
| **RDP** | Brute-force attack | 4 | 4 | **16** | Public exposure |
| **Admin** | Credential compromise | 3 | 5 | **15** | Full system control |
| **OS** | Unpatched vulnerabilities | 3 | 4 | **12** | Common Windows attack |
| **Azure** | Privilege misuse | 2 | 5 | **10** | High-impact resource |
| **Logs** | Undetected attack | 3 | 3 | **9** | Delayed response |

---

## 5. Security Controls & Compliance Mapping

| Risk | Control | Control Type | Framework |
| :--- | :--- | :--- | :--- |
| **RDP attacks** | Restrict inbound IP access | Preventive | CIS 4 |
| **Credential theft** | Enforce MFA | Preventive | NIST CSF PR.AC |
| **OS exploits** | Patch management | Corrective | CIS 7 |
| **Lack of logging** | Centralized logging | Detective | NIST CSF DE.CM |
| **Privilege misuse** | RBAC enforcement | Preventive | CIS 5 |

---

## 6. Recommended Mitigation Actions

> [!IMPORTANT]
> The following actions should be prioritized to reduce the current risk posture:

1.  **Restrict RDP access:** Use Network Security Groups (NSGs) to limit access only to trusted IP ranges.
2.  **Enable MFA:** Enforce Multi-Factor Authentication for all administrative accounts.
3.  **Patch Management:** Apply regular operating system patches and updates.
4.  **Centralized Logging:** Enable **Azure Monitor** or **Microsoft Defender for Cloud** for logging and alerts.
5.  **RBAC:** Implement Role-Based Access Control to ensure the principle of least privilege.



# Template
## Cloud Security Risk Assessment Report
**Governance, Risk, and Compliance (GRC)**

### 1. Executive Summary

This report documents the results of a cloud security risk assessment conducted on a single Azure-hosted virtual machine from a Governance, Risk, and Compliance (GRC) perspective. The purpose of the assessment is to identify critical assets, evaluate realistic threat scenarios, assess vulnerabilities, quantify risk, and recommend security controls aligned with recognized industry frameworks.

The assessment identified multiple medium to high risks, primarily related to exposed remote access, lack of multi-factor authentication, default operating system hardening, and limited centralized monitoring. While no active exploitation was observed during the assessment, these findings reflect common cloud misconfigurations that could be leveraged by attackers if left unaddressed.

### 2. Scope and Environment

**Assessment Scope**
* Single Azure Virtual Machine
* No additional cloud services or applications included
* Assessment performed from a GRC (non-exploitative) perspective

**Environment Details**
| Items | Description |
| :--- | :--- |
| Cloud Provider | Microsoft Azure |
| Resource Group | GRC-Lab |

*Note: Define what you are assessing. Include the type of VM, OS, access method, and whether it’s publicly exposed. Think about what is in and out of scope.*

### 3. Asset Identification

The following assets were identified as part of the assessment. Assets include not only infrastructure, but also identities, access paths, and cloud control mechanisms.

| Asset Category | Asset | Why it Matters |
| :--- | :--- | :--- |
| Compute | Azure Virtual Machine | Host's operating system and services |
| Identity | Local Administrator Account | Grants full system control |

*Note: List all valuable resources — not just hardware. Include accounts, network access points, cloud permissions, and anything attackers could abuse.*

### 4. Threat and Vulnerability Summary

This section links what could go wrong (threats) with why it could succeed (vulnerabilities).

| Asset | Threat | Vulnerability | Why it Matters |
| :--- | :--- | :--- | :--- |
| Azure VM | Brute-force attack | RDP exposed to the internet | Common automated attacks |
| Admin Account | Credential theft | No MFA | Full system compromise |

*Note: For each asset, think about realistic threats (what could go wrong) and the vulnerabilities that make it possible (why it could succeed). Focus on common cloud attack scenarios.*

### 5. Risk Assessment and Prioritization

Risks were scored using a standard likelihood × impact model to enable consistent prioritization.

**Risk Scoring Model**
* Likelihood: 1 (Low) → 5 (High)
* Impact: 1 (Low) → 5 (High)
* Risk Score = Likelihood × Impact

**Risk Register Table**
| Asset | Threat | Likelihood | Impact | Risk Score | Justification |
| :--- | :--- | :---: | :---: | :---: | :--- |
| RDP | Brute-force attack | 4 | 4 | 16 | Public exposure |
| Admin Account | Credential compromise | 3 | 5 | 15 | Full system access |

**How to Fill Out the Risk Register**
This section explains how to fill out the risk register table. The risk register takes the assets, threats, and vulnerabilities we identify and turns them into measurable risks. This helps management prioritize and address these risks effectively.

**A. Asset**
List the system, account, service, or access path being assessed. If it provides access, stores data, or grants control, it should be included as an asset.
Examples:
* RDP (3389)
* Local Administrator Account
* Azure Virtual Machine
* Azure Subscription
* Windows Event Logs

**B. Threat**
Describe a realistic threat scenario that could impact the asset. Threats should reflect common, real-world attack techniques seen in enterprise environments.
Examples:
* Brute-force attack
* Credential compromise
* Unauthorized configuration changes
* Exploitation of known vulnerabilities
* Undetected malicious activity

**C. Likelihood**
Assess the likelihood of the threat occurring in the current environment by considering factors such as internet exposure, frequency of this attack type, ease of exploitation, and existing security controls.
| Score | Description |
| :---: | :--- |
| 1 | Rare |
| 2 | Unlikely |
| 3 | Possible |
| 4 | Likely |
| 5 | Almost Certain |

**D. Impact**
Evaluate the severity of impact if the threat is successfully exploited by considering the following factors: loss of confidentiality, integrity, or availability; the level of access gained (user vs. administrator); operational disruption; and potential compliance or reputational damage.
| Score | Description |
| :---: | :--- |
| 1 | Minimal Impact |
| 2 | Minor Impact |
| 3 | Moderate Impact |
| 4 | High Impact |
| 5 | Critical Impact |

**E. Risk Score**
Calculate the risk score by multiplying likelihood by impact.
Example: Likelihood 4 × Impact 4 = 16

**F. Justification**
Provide a brief explanation to support the likelihood and impact scores. This field is essential for auditability. Justifications for scores must be provided, not assumed.
Examples:
* “RDP is publicly exposed and commonly targeted by automated brute-force attacks.”
* “Compromise of an administrator account would grant full system control.”
* “Lack of centralized logging increases time to detect malicious activity.”

### 6. Security Controls and Compliance Mapping

Identified risks were mapped to preventive, detective, and corrective controls aligned with recognized frameworks.

| Risk | Security Control | Control Type | Framework |
| :--- | :--- | :--- | :--- |
| RDP attacks | Restrict inbound IP access | Preventive | CIS Control 4 |
| Credential theft | Enforce MFA | Preventive | NIST CSF PR.AC |

*Note: For each identified risk, select controls that reduce it. Categorize them as Preventive (stop it), Detective (detect it), or Corrective (fix it after it happens). Then map each control to a recognized framework (e.g., NIST CSF or CIS Controls) to ensure auditability and governance alignment. Focus on whether the control meets the framework’s intent, not the specific tool.*

### 7. Recommended Mitigation Actions

The following actions are recommended to reduce overall risk:
* Restrict RDP access via Network Security Groups to trusted IP ranges
* Enable multi-factor authentication for all administrative accounts
* Apply regular operating system patches and updates
* Enable Azure Monitor or Defender for centralized logging and alerts
* Implement role-based access control (RBAC) for Azure resources

*Note: Suggest clear, actionable steps to reduce each risk. Make them measurable and specific, e.g., enable MFA, restrict RDP access, apply patches, enable logging, and enforce RBAC.*

### 8. Overall Risk Posture
Based on the findings, the overall risk posture of the assessed environment is Medium to High. While the environment is small, the presence of exposed access paths and limited monitoring significantly increases risk.



## Lessons Learned: 
This project demonstrated that cloud security is as much about governance as it is about technical configuration. I learned that identifying a vulnerability is only the first step; the real value lies in quantifying its business impact and mapping mitigations to industry frameworks like NIST and CIS.
This simulation reinforced the importance of continuous monitoring and the principle of least privilege, proving that a structured GRC approach is essential for transforming technical findings into actionable, defensible security strategies in a real-world enterprise.




