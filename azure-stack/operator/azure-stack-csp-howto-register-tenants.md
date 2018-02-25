---
title: Add tenants for usage and billing to Azure Stack | Microsoft Docs
description: Type the description in Azure Stack.
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
ms.date: 02/22/2018
ms.author: mabrigg
ms.reviewer: alfredo

---

# Add tenants for usage and billing to Azure Stack

*Applies to: Azure Stack integrated systems*

This article describes the step required to configure Azure Stack so that when a new tenant uses resources their usage will be reported to their Cloud Service Provider (CSP) subscription. 

CSP often provides service to multiple customers (tenants) on their Azure Stack deployment. Registering tenants ensures that each tenant’s usage will be reported and billed to that tenant’s CSP subscription. If you do not complete the steps in this article, tenant usage is charged to the subscription used in the initial registration of Azure Stack.

## Create a new customer in Partner Center

Add the customer in Partner Center and creates an Azure subscription. For instructions, see [Add a new customer](https://msdn.microsoft.com/en-us/partner-center/add-a-new-customer).

## Configure usage reporting by adding a new tenant to your registration

Update your registration with the new customer’s subscription. Azure reports the customer's usage using the customer identity from Partner Central. This step ensures that each customer’s usage is reported under that customer’s individual CSP subscription. This makes tracking user usage and billing much easier.

> [!Note]  
> To carry out this step, you must have [registered Azure Stack](azure-stack-register.md).

1. Open Windows PowerShell with an elevated prompt, and run:  
    `Login-AzureRmAccount`
2. Type your Azure credentials.
3. In the PowerShell session, run:

```powershell
    New-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01 -Properties
```
### New-AzureRmResource PowerShell parameters
| Parameter | Description |
| --- | --- | 
|registrationSubscriptionID | The Azure subscription that was used for the initial registration. |
| customerSubscriptionID | The Azure subscription (not Azure Stack) belonging to the customer to be registered. Must be created in the CSP offer. In practice, this means through Partner Center. If a customer has more than one tenant, this subscription must be created in the tenant that will be used to log into Azure Stack.
| resourceGroup | The resource group in Azure in which your registration is stored. 
| registrationName | The name of the registration of your Azure Stack. It is an object stored in Azure. The name is usually in the form | 
| azurestack-CloudID | The Cloud ID of your Azure Stack deployment.

> [!Note]  
> Tenants need to be registered with each Azure Stack they use. If a tenant uses more than one Azure Stack, you need to update the initial registrations of each deployment with the tenant subscription.

## Add the new tenant to Azure Stack

You can configure Azure Stack to support users from multiple Azure AD tenants to use services in Azure Stack. To provide services to an end user, you either:

 - Manage the end customer account and their Azure AD tenant for them. In this case, yo will add their new customer tenant to Azure Stack. For instructions, see [Enable multi-tenancy in Azure Stack](azure-stack-enable-multitenancy.md).
 - Manage the end customer tenant from a guest account with owner privileges for the Azure AD tenant. Create the guest account, and send the information to your end customer. The end customer than add **owner** privileges to the guest account.

## Next steps

 - To review the error messages if they are triggered in your registration process, see [Tenant registration error messages](/azure-stack-csp-ref-error-codes.md).
 - To learn more about how to retrieve resource usage information from Azure Stack, see [Usage and billing in Azure Stack](/azure-stack-billing-and-chargeback.md).
 - To review how an end customer may add you, as the CSP, as the manager for their Azure Stack, tenant, see [Enable a Cloud Service Provider to manage your Azure Stack subscription](user\azure-stack-csp-enable-billing-usage-tracking.md).
