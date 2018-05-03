---
title: Add a Kubernetes Cluster to the Azure Stack Marketplace | Microsoft Docs
description: Learn how to add a Kubernetes Cluster to the Azure Stack Marketplace.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''

ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: mabrigg
ms.reviewer: waltero

---

# Add a Kubernetes Cluster to the Azure Stack Marketplace

*Applies to: Azure Stack integrated systems and Azure Stack Development Kit*

You can offer a Kubernetes Cluster as a Marketplace item to your users. Your users can deploy Kubernetes in a single, coordinated operation.

The following article look at using an Azure Resource Manager template to deploy and provision the resources for a standalone Kubernetes cluster. Before you start, check your Azure Stack and global Azure tenant settings. Collect the required information about your Azure Stack. Add necessary resources to your tenant and to the Azure Stack Marketplace.

## Prerequisites 

To get started, make sure you have the right permissions and that your Azure Stack is ready.

1. Verify that you can create applications in your Azure Active Directory (Azure AD) tenant. You need these permissions for the Kubernetes deployment.

    For instructions on checking your permissions, see [Check Azure Active Directory permissions](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#check-azure-active-directory-permissions).

2. Generate an SSH public and private key pair to sign in to the Linux VM on Azure Stack. You will need the public key when creating the cluster.

    For instructions on generating a key, see [SSH Key Generation](https://github.com/msazurestackworkloads/acs-engine/blob/master/docs/ssh.md#ssh-key-generation).

3. Check that you have a valid subscription in your Azure Stack tenant portal, and that you have enough public IP addresses available to add new applications.

## Create a service principal in Azure AD

1. Sign in to the global [Azure portal](http://www.poartal.azure.com).
2. Check that you signed in using the Azure AD tenant associated with Azure Stack.
3. Create an Azure AD application.

    a. Select **Azure Active Directory** > **+ App Registrations** > **New Application Registration**.

    b. Enter a **Name** of the application.

    c. Select **Web app / API**

    d. Enter `http://localhost` for the **Sign-on URL**.

    c. Click **Create**

4. Make note of the **Application ID**. You will need the ID when creating the cluster. The ID is referenced as **Service Principal Client ID**.

5. Select **Settings** < **Keys**.

    a. Enter the **Description**.

    b. Select **Never expires** for **Expires**.

    c. Select **Save**. Make note the key string. You will need the key string when creating the cluster. The key is references as the **Service Principal Client Secret**.

## Create a plan, an offer, and a subscription

Create a plan, an offer, and a subscription for the Kubernetes Cluster Marketplace item. You can also use an existing plan and offer.

1. Sign in to the [Administration portal.](https://adminportal.local.azurestack.external).

2. Create a plan as the base plan. For instructions, see [Create a plan in Azure Stack](azure-stack-create-plan.md).

3. Create an offer. For instructions, see [Create an offer in Azure Stack](azure-stack-create-offer.md).

4. Select **Offers**, and find the offer you created.

5. Select **Overview** in the Offer blade.

6. Select **Change state**. Select **Public**.

7. Select **+ New** > **Offers and Plans** > **Subscription** to create a new subscription.

    a. Enter a **Display Name**.

    b. Enter a **User**. Use the Azure AD account associated with your tenant.

    c. **Provider Description**

    d. Set the **Directory tenant** to the Azure AD tenant for your Azure Stack. 

    e. Select **Offer**. Select the name of the offer that you created. Make note of the Subscription ID.

## Give the service principal access

Give the service principal access to your subscription so that the principal can create resources.

1.  Sign in to the [Administration portal](https://adminportal.local.azurestack.external).

2. Select **More services** > **User subscriptions** > **+ Add**.

3. Select the subscription that you created.

4. Select **Access control (IAM)** > Select ** + Add**.

5. Select the **Owner** role.

6. Select the application name created for your service principal. You may have to type the name in the search box.

7. Click **Save**.

## Add an Ubuntu server image

Add the following Ubuntu Server image from the Marketplace:

1. Sign in to the [Administration portal](https://adminportal.local.azurestack.external).

2. Select **More services** > **Marketplace Management**.

3. Select **+ Add from Azure**.

4. Enter `UbuntuServer`.

5. Select the server with the following profile:
    - **Publisher**: Canonical
    - **Offer**: UbuntuServer
    - **SKU**: 16.04-LTS
    - **Version**: 16.04.201802220

6. Select **Download.**

## Add a custom script for Linux

Add the Kubernetes Cluster to the Marketplace:

1. Open the [Administration portal](https://adminportal.local.azurestack.external).

2. Select **More services** > **Marketplace Management**.

3. Select **+ Add from Azure**.

4. Enter `Custom Script for Linux`.

5. Select the script with the following profile:
    - **Offer**: Custom Script for Linux 2.0
    - **Version**: 2.0.3
    - **Publisher**: Microsoft Corp

6. Select **Download.**

## Add the Kubernetes Cluster

Add the Kubernetes Cluster to the marketplace.

1. Open the [Administration portal](https://adminportal.local.azurestack.external).

2. Select **More services** > **Marketplace Management**.

3. Select **+ Add from Azure**.

4. Enter `Kubernetes Cluster`.

5. Select `Kubernetes Cluster`.

6. Select **Download.**

> [!note]  
> It may take five minutes for the marketplace item to appear in the Marketplace.

## Deploy Kubernetes Cluster to the Marketplace

1. Open the [Administration portal](https://adminportal.local.azurestack.external).

2. Select **+New** > **Compute** > **Kubernetes Cluster**.

    ![Deploy Solution Template](media/azure-stack-solution-template-kubernetes-cluster-add/azure-stack-kubernetes-cluster-solution-template.png)

3. Select **Parameters** in the Deploy Solution Template.

    ![Deploy Solution Template](media/azure-stack-solution-template-kubernetes-cluster-add/azure-stack-kubernetes-cluster-solution-template-parameters.png)

2. Enter the **Linux administrative user name**. User name for the Linux Virtual Machines that are part of the Kubernetes cluster and DVM.

3. Enter the **SSH public key** used for authorization to all Linux machines created as part of the Kubernetes cluster and DVM.

4. Enter the **tenant endpoint**. This is the Azure Resource Manager endpoint to connect to create the resource group for the Kubernetes cluster. Use  https://management.redmond.ext-n42r0703.masd.stbtest.microsoft.com (multi-node) or https://management.local.azurestack.external (one-node).

5. Enter the **tenant ID** for the tenant. If you need help finding this value, see [Get tenant ID](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#get-tenant-id). 

6. Enter the **master profile DNS prefix** that is unique to the region. This must be a region-unique name, such as `k8s-12345`. Try to chose it same as the resource group name as best practice.

7. Enter the number of agents in the cluster. This value is referred to as the **Agent Pool Profile Count**. There can be from 1 to 32

8. Enter the **service principal application ID** (used by the Kubernetes Azure cloud provider).

9. Enter the **service principal client secret** that you created when creating service principal application.

10. Enter the **Kubernetes Azure Cloud Provider Version**. This is the version for the Kubernetes Azure provider. Azure Stack releases a custom Kubernetes build for each Azure Stack version.

11. Leave the **artifact location** as is. The template requires artifacts retrieved from a base URI. When you deploy the template using the accompanying scripts, a private location in the subscription is used. The value in this box is automatically generated.

12. Select **OK**.

### Specify the solution values

1. Select the **Subscription**.

2. Enter the name of a new resource group or select an existing resource group. The resource name needs to be alphanumeric and lowercase.

3. Enter the location of the resource group, such as **local**. 

4. Select **Create.**

## Next steps

[Overview of offering services in Azure Stack](azure-stack-offer-services-overview.md)

[Deploy a Kubernetes Cluster to Azure Stack](/user/azure-stack-solution-template-kubernetes-deploy.md)