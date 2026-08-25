A cloud security case study designing a segmented Azure environment for a fictional city's digital services.
# Riverside Municipal Pilot 🏙️

## Azure Cloud Security Architecture Case Study

A practical Azure project that simulates a fictional city's first digital services environment.

The goal was to design a small cloud environment where a public facing citizen portal and an internal records system have different trust levels and are not exposed in the same way.

---

## 🎯 The Problem

Riverside is a fictional city launching digital services for citizens.

The environment needs:

• A public facing citizen portal

• An internal records system

• Separation between public and internal workloads

• Governance over resource creation

• Cost visibility

• Basic operational monitoring

The key question behind this project was:

> How can a public facing service be made accessible without turning it into a bridge to internal government records?

---

# 🏗️ Architecture

The project was deployed inside:

`rg-riverside-pilot`

The environment contains an Azure Virtual Network with two separate subnets.

```text
                         Internet
                            │
                            ▼
                    Citizen Portal
                    Public Facing VM
                            │
                            ▼
┌──────────────────────────────────────────────┐
│                Azure VNet                    │
│                                              │
│  subnet-public       10.0.1.0/24            │
│                                              │
│  Citizen Portal VM                           │
│  Public facing workload                      │
│                                              │
│  ───────── Trust Boundary ─────────          │
│                                              │
│  subnet-admin        10.0.2.0/24            │
│                                              │
│  Internal Records VM                         │
│  No public IP                                │
│  No direct inbound internet access           │
│                                              │
└──────────────────────────────────────────────┘


🌐 Network Segmentation

The Azure Virtual Network contains two subnets.

subnet-public

Address range:

10.0.1.0/24

This subnet hosts the citizen portal virtual machine.

The public workload is designed to accept only the required web traffic
on ports:

80 HTTP

443 HTTPS

subnet-admin

Address range:

10.0.2.0/24

This subnet hosts the internal records virtual machine.

The internal records system has:

• No public IP

• No direct inbound internet exposure

• Internal network isolation from the public facing workload

Why this matters

A public facing application and an internal records system should not
have the same trust level.

The network segmentation helps reduce the risk that a compromise of the
citizen portal becomes a direct path into internal government records.

📸 Virtual Network Evidence



🖥️ Citizen Portal VM

A virtual machine was deployed to represent a public facing citizen
portal.

The workload is placed in:

subnet-public

This represents services such as:

• Permit applications

• Citizen information

• Public transportation information

• Other digital public services



🔐 Internal Records VM

A second virtual machine was deployed to represent the city's internal
records system.

The workload is placed in:

subnet-admin

The goal was to ensure that this workload does not require direct public
internet exposure.

This separation creates a basic trust boundary between public services
and internal government records.



💾 Azure Storage and Data Tiers

Azure Storage was used to demonstrate how different types of municipal
data can have different access requirements.

Hot Tier

Used for:

• Active permit applications

• Documents currently being processed

• Frequently accessed records

Cool Tier

Used for:

• Archived records

• Historical documents

• Data accessed less frequently

Why this matters

Storage decisions are also cost decisions.

Frequently accessed data may require faster access, while older records
do not necessarily need the same storage tier.

This allows cloud infrastructure to be designed around realistic access
patterns.



🛡️ Governance with Azure Policy

A built in Azure Policy was assigned to:

rg-riverside-pilot

The policy requires resources to include the following tag:

department

The policy was configured with a:

Deny

effect.

When a test resource was created without the required tag, Azure denied
the deployment.

This demonstrates a preventive governance control.

Create Resource
       │
       ▼
Check for department tag
       │
   ┌───┴───┐
   │       │
  YES      NO
   │       │
   ▼       ▼
Allowed  Denied

Key lesson

Instead of relying on users to remember governance requirements, Azure
Policy can automatically prevent noncompliant resources from being
created.

Add your Azure Policy denial screenshot here.

💰 Cost Management

A monthly budget was configured for the project.

Budget: $10

Alert threshold: 80%

This means an alert is configured when project spending approaches the
defined budget threshold.

At the time of the screenshot:

Evaluated spend: $0.00



Important

The budget provides cost visibility and alerts.

It does not automatically shut down resources.

📊 Monitoring and Audit Trail

Basic Azure Monitor metrics were reviewed for both virtual machines.

Metrics included:

• CPU activity

• Network activity

The Azure Activity Log was also reviewed to provide an administrative
audit trail of:

• Resource deployments

• Configuration changes

• Resource creation

• Other control plane operations

Add your Activity Log screenshot here.

⚠️ Challenges Encountered

vCPU Quota Limitation

The project encountered a regional vCPU quota limitation when deploying
the second virtual machine.

The subscription had insufficient available vCPUs.

To resolve this, an earlier lab virtual machine was deallocated to free
compute capacity.

Lesson

Cloud architecture also has to work within:

• Subscription quotas

• Regional availability

• Resource limits

Network Configuration

A deployment issue initially caused a virtual machine to risk being
associated with the wrong automatically generated virtual network.

This had to be corrected because the network segmentation was central to
the security design.

Lesson

Designing an architecture is not enough.

Resources must be validated to confirm they were deployed where
intended.

🌍 SDG 9 and SDG 11

SDG 9: Resilient Infrastructure

Network segmentation improves resilience by reducing the potential for a
compromise to spread from a public facing workload into internal
systems.

SDG 11: Sustainable Cities

Tiered storage and cost monitoring support sustainable digital
infrastructure by improving resource efficiency and financial
visibility.

This project connects cloud architecture to realistic public
infrastructure challenges.

🔮 Future Improvements

A production municipal environment would require additional security
controls.

Future improvements include:

• Microsoft Entra ID

• Role Based Access Control

• Conditional Access

• Defender for Cloud

• Centralized security monitoring

• Backup and disaster recovery

• Multi region resilience

• Stronger identity based access controls

🧠 Key Lessons

This project helped me move beyond learning individual Azure services.

Instead of asking:

What does this Azure service do?

I focused on:

What problem does this control solve?

The Riverside Municipal Pilot combines:

• Network segmentation

• Internal workload isolation

• Storage tiering

• Azure Policy governance

• Cost monitoring

• Operational visibility

👨‍💻 Author

Augustine Alfred

Aspiring Microsoft Cloud and AI Security Engineer.

Documenting a 90 day learning journey through practical Azure projects,
cloud security concepts, and digital infrastructure case studies.

📌 Project Status

Completed

The next phase of learning will focus on:

Microsoft Entra ID
Identity and Access Management
Conditional Access
Defender for Cloud
Cloud security posture management
