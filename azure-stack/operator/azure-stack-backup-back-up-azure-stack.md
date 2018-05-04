---
title: Back up Azure Stack | Microsoft Docs
description: Perform an on-demand backup on Azure Stack with backup in place.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''

ms.assetid: 9565DDFB-2CDB-40CD-8964-697DA2FFF70A
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg

---
# Back up Azure Stack

*Applies to: Azure Stack integrated systems and Azure Stack Development Kit*

Perform an on-demand backup on Azure Stack with backup in place. If you need to enable the Infrastructure Backup Service, see [Enable Backup for Azure Stack from the administration portal](azure-stack-backup-enable-backup-console.md).

> [!Note]  
>  Azure Stack Tools contain the **Start-AzSBackup** cmdlet. For instructions on installing the tools, see [Get up and running with PowerShell in Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-configure-quickstart).

##  Load the Connect and Infrastructure modules

Open Windows PowerShell with an elevated prompt, and run the following commands:

   ```powershell
    cd C:\tools\AzureStack-Tools-master\Connect
    Import-Module .\AzureStack.Connect.psm1
    
    cd C:\tools\AzureStack-Tools-master\Infrastructure
    Import-Module .\AzureStack.Infra.psm1 
    
   ```
> [!NOTE]
> AzureStack.Connect.psm1 is not needed for this as the AzsBackup functions are in AzureStack.Infra.psm1 but you might need the connect if you are using VPN.

##  Setup Rm environment and log into the operator management endpoint

In the same PowerShell session, Edit the following PowerShell script by adding the variables for your environment. Run the updated script to set up the RM environment and log into the operator management endpoint.

| Variable    | Description |
|---          |---          |
| $TenantName | Azure Active Directory tenant name. |
| Operator account name        | Your Azure Stack operator account name. |
| Azure Resource Manager Endpoint | URL to the Azure Resource Manager. |

   ```powershell
   # Specify Azure Active Directory tenant name
    $TenantName = "contoso.onmicrosoft.com"
    
    # Set the module repository and the execution policy
    Set-PSRepository `
      -Name "PSGallery" `
      -InstallationPolicy Trusted
    
    Set-ExecutionPolicy RemoteSigned `
      -force
    
    # Configure the Azure Stack operator’s PowerShell environment.
    Add-AzureRMEnvironment `
      -Name "AzureStackAdmin" `
      -ArmEndpoint "https://adminmanagement.seattle.contoso.com"
    
    Set-AzureRmEnvironment `
      -Name "AzureStackAdmin" `
      -GraphAudience "https://graph.windows.net/"
    
    $TenantID = Get-AzsDirectoryTenantId `
      -AADTenantName $TenantName `
      -EnvironmentName AzureStackAdmin
    
    # Sign-in to the operator's console.
    Add-AzureRmAccount -EnvironmentName "AzureStackAdmin" -TenantId $TenantID
    
   ```


## Start Azure Stack backup

```powershell
    $location = Get-AzsLocation
    Start-AzSBackup -Location $location.Name
```


## Confirm backup completed via PowerShell

```powershell
    Get-AzsBackup -Location $location.Name | Select-Object -ExpandProperty BackupInfo
```

The result should look like the following output:

```powershell
    backupDataVersion :
    backupId          : xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx
    roleStatus        : {@{roleName=NRP; status=Succeeded}, @{roleName=SRP; status=Succeeded}, @{roleName=CRP; status=Succeeded}, @{roleName=KeyVaultInternalControlPlane; status=Succeeded}...}
    status            : Succeeded
    createdDateTime   : 2018-05-03T12:16:50.3876124Z
    timeTakenToCreate : PT22M54.1714666S
    stampVersion      :
    oemVersion        :
    deploymentID      :
```

## Confirm backup completed in the administration portal

1. Open the Azure Stack administration portal at [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external).
2. Select **More services** > **Infrastructure backup**. Choose **Configuration** in the **Infrastructure backup** blade.
3. Find the **Name** and **Date Completed** of the backup in **Available backups** list.
4. Verify the **State** is **Succeeded**.

You can also confirm the backup completed from the administration portal. Navigate to `\MASBackup\<datetime>\<backupid>\BackupInfo.xml`

## Next steps

- Learn more about the workflow for recovering from a data loss event. See [Recover from catastrophic data loss](azure-stack-backup-recover-data.md).
