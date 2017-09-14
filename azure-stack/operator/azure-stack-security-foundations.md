---
title: Understand Azure Stack security controls | Microsoft Docs
description: As a service administrator learn about the security controls applied to Azure Stack
services: azure-stack
documentationcenter: ''
author: Heathl17
manager: byronr
editor: ''

ms.assetid: cccac19a-e1bf-4e36-8ac8-2228e8487646
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw

---
# Understand Azure Stack security foundations

*Applies to: Azure Stack integrated systems*

Security considerations and compliance regulations are among the main drivers for using hybrid clouds. Azure Stack was designed for these scenarios, and it is important to understand the controls already in place when adopting Azure Stack.

It is important to keep in mind that in Azure Stack there are two security posture “layers” that coexist. The first layer comprises the Azure Stack infrastructure, which goes from the hardware components all the way up to the Azure Resource Manager, and includes the Admin and the Tenant portals. The other layer consists of the workloads that tenants create, deploy, and manage and includes things like virtual machines or App Services web sites.  

As in Azure, the security posture of the Azure Stack infrastructure is defined by Microsoft, while the security of the workloads running on top of it is tenants’ responsibility. This document provides foundational knowledge on the security posture of the Azure Stack infrastructure.

Azure Stack was designed with a security posture to defend against modern threats, and was built to meet the requirements from the major compliance standards. As a result, the security posture of the Azure Stack infrastructure is built on two pillars:

 - Assume Breach, where starting from the assumption that the system has already been breached, we focus on *detecting and limiting the impact of breaches* versus only trying to prevent attacks. 
 - Hardened by Default, where since the infrastructure runs on well-defined hardware and software, we *enable, configure, and validate security features* that are usually left to customers to implement.

## Data at rest encryption
All Azure Stack infrastructure and tenant data is encrypted at rest using Bitlocker. This encryption protects against physical loss or theft of Azure Stack storage components. 

## Data in transit encryption
The Azure Stack infrastructure components communicate using channels encrypted with TLS 1.2. Encryption certificates are self-managed by the infrastructure. 
All external infrastructure endpoints, such as the REST endpoints or the Azure Stack portal, support TLS 1.2 for secure communications. Encryption certificates, either from a 3rd party or your enterprise Certificate Authority, must be provided for those endpoints. 
While self-signed certificates can be used for these external endpoints, Microsoft strongly advises against using them. 

## Secret management
Azure Stack infrastructure uses a multitude of secrets, like passwords, to function. Most of them are autonomously rotated at a very high frequency because they are Group Managed Service accounts, and rotate every 24 hours.
The remaining secrets that are not autonomously managed can be rotated manually with a script in the Privileged Endpoint.

## Code Integrity
Azure Stack makes use of the latest Windows Server 2016 security features. One of them is Windows Defender Device Guard, which provides application whitelisting, and ensures that only authorized code runs within the Azure Stack infrastructure. 

Authorized code is signed by either Microsoft or the OEM partner, and it is included in the list of allowed software that is specified in a policy defined by Microsoft. In other words, only software that has been approved to run in the Azure Stack infrastructure can be executed. Any attempt to execute unauthorized code will be blocked and an audit will be generated.

The Device Guard policy also prevents third party agents or software from running in the Azure Stack infrastructure.

## Credential Guard
Another Windows Server 2016 security feature in Azure Stack is Windows Defender Credential Guard, which is used to protect Azure Stack infrastructure credentials from Pass-the-Hash and Pass-the-ticket attacks.

## Antimalware
Every component in Azure Stack (both Hyper-V hosts and Virtual Machines) is protected with Windows Defender Antivirus.
Privileged operations management
Administration in Azure Stack is a well-defined process. Azure Stack has three entry points for management: 
1. The Admin Portal provides a point-and-click experience for daily management operations. For information on the Admin Portal, please see here: <LINK> 
2. The Azure Resource Manager exposes all the management operations of the Admin Portal via a REST/Powershell API. For information on Azure Resource Manager, please see here: <LINK> 
3. For specific low-level operations, e.g. data center integration or support scenarios, Azure Stack exposes a PowerShell endpoint called Privileged Endpoint. This endpoint exposes only a whitelisted set of cmdlets and it is heavily audited. For information on the Privileged Endpoint, please see here: <LINK> 

## Network controls
Azure Stack infrastructure comes with multiple layers of network Access Control List(ACL).  The ACLs     prevent unauthorized access to the infrastructure components and limit infrastructure communications to only the paths that are required for its functioning. 

Network ACLs are enforced in three layers:
1.  Top of Rack switches
2.  Software Defined Network
3.  Host and VM operating system firewalls 


