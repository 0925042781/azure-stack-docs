---
title: Register tenants for utilization tracking in Azure Stack | Microsoft Docs
description: Type the description in Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''

ms.assetid: 2A35D8ED-0059-424C-BA23-586F488569DF
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/01/2018
ms.author: mabrigg

---

# Register tenants for utilization tracking in Azure Stack

*Applies to: Azure Stack integrated systems*

## Add tenant to registration

You can add or update the registration resource. If you want to change the subscription associated with a tenant, you can call PUT/New-AzureRMResource again. The old mapping will be overwritten.

| Parameter                  | Description |
|---                         | --- |
| registrationSubscriptionID | The Azure subscription that was used for the initial registration. |
| customerSubscriptionID     | The  Azure subscription (not Azure Stack) belonging to the customer to be registered. Must be created in the CSP offer. In practice, this means through Partner Center. If a customer has more than one tenant, this subscription must be created in the tenant that will be used to log into Azure Stack. |
| resourceGroup              | The resource group in Azure in which your registration is stored. |
| registrationName           | The name of the registration of your Azure Stack. It is an object stored in Azure. The name is usually in the form azurestack-CloudID, where CloudID is the Cloud ID of your Azure Stack deployment. |

> [!Note]  
> Tenants need to be registered with each Azure Stack they use. If a tenant uses more than one Azure Stack, you need to update the initial registrations of each deployment with the tenant subscription.

### PowerShell

Use the New-AzureRmResource cmdlet to update the registration resource. Log in to Azure (Login-AzureRMAccount) using the account you used for the initial registration. Here is an example of how to add a tenant:

    ```powershell
    Add new tenant mapping
    New-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01 -Properties
    ```

### API call

**Operation**: PUT  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}?api-version=2017-06-01 HTTP/1.1`  
**Response**: 201 Created  
**Response Body**: Empty  

## List all registered tenants

Get a list of all tenants that have been added to a registration.

 > [!Note]  
 > If no tenants have been registered, you will get no response.

### Parameters

| Parameter                  | Description          |
|---                         | ---                  |
| registrationSubscriptionId | The Azure subscription that was used for the initial registration.   |
| resourceGroup              | The resource group in Azure in which your registration is stored.    |
| registrationName           | The name of the registration of your Azure Stack. It is an object stored in Azure. The name is usually in the form azurestack-CloudID, where CloudID is the Cloud ID of your Azure Stack deployment.   |

### PowerShell

<Summary>

    Get-AzureRmResovurce -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions" -ApiVersion 2017-06-01
    ```

### API Call

You can get a list of all tenant mappings using the GET operation

**Operation**: GET  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions?api-version=2017-06-01 HTTP/1.1`  
**Response**: 200  
**Response Body**: 

```JSON  
{
    "value": [{
            "id": " subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{ cspSubscriptionId 1}”,
            "name": " cspSubscriptionId 1",
            "type": “Microsoft.AzureStack\customerSubscriptions”,
            "properties": { "tenantId": "tId1" }
        },
        {
            "id": " subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{ cspSubscriptionId 2}”,
            "name": " cspSubscriptionId2 ",
            "type": “Microsoft.AzureStack\customerSubscriptions”,
            "properties": { "tenantId": "tId2" }
        }
    ],
    "nextLink": "{originalRequestUrl}?$skipToken={opaqueString}"
}
```


## Remove a tenant mapping

You can remove a tenant that has been added to a registration

### Parameters

| Parameter                  | Description          |
|---                         | ---                  |
| registrationSubscriptionId | Yadda yadda yadda.   |
| resourceGroup              | Yadda yadda yadda.   |
| registrationName           | Yadda yadda yadda.   |
| customerSubscriptionId     | Yadda yadda yadda.   |

### PowerShell

<Summary>

    ```powershell
    Remove-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01
    ```

### API Call

<Summary>

**Operation**: DELETE  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}?api-version=2017-06-01 HTTP/1.1`  
**Response**: 204 No Content  
**Response Body**: Empty

## Next steps

To learn about X, see [X]().
