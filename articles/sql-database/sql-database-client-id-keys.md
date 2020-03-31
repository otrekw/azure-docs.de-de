---
title: Abrufen von Werten zum Authentifizieren einer App
description: Erstellen Sie einen Dienstprinzipalnamen für den Zugriff auf SQL-Datenbank aus dem Code.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/12/2019
ms.openlocfilehash: 5b2c64660f37745f5b13d53559037e84ca20c47b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79476965"
---
# <a name="get-the-required-values-for-authenticating-an-application-to-access-sql-database-from-code"></a>Abrufen der erforderlichen Werte aus dem Code zum Authentifizieren einer Anwendung für den Zugriff auf die SQL-Datenbank

Zum Erstellen und Verwalten der SQL-Datenbank über den Code müssen Sie Ihre App in der Azure Active Directory-Domäne (AAD) in dem Abonnement registrieren, in dem die Azure-Ressourcen erstellt wurden.

## <a name="create-a-service-principal-to-access-resources-from-an-application"></a>Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen aus einer Anwendung

In den folgenden Beispielen werden die Active Directory-Anwendung (AD) und der Dienstprinzipal erstellt, den wir zum Authentifizieren unserer C#-App benötigen. Das Skript gibt Werte aus, die für das vorhergehende C#-Beispiel erforderlich sind. Ausführliche Informationen finden Sie unter [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe von Azure PowerShell](../active-directory/develop/howto-authenticate-service-principal-powershell.md).

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

> [!IMPORTANT]
> Das Azure Resource Manager-Modul von PowerShell wird von Azure SQL-Datenbank weiterhin unterstützt, aber alle zukünftigen Entwicklungen erfolgen für das Az.Sql-Modul. Das AzureRM-Modul erhält mindestens bis Dezember 2020 weiterhin Fehlerbehebungen.  Die Argumente für die Befehle im Az-Modul und den AzureRm-Modulen sind im Wesentlichen identisch. Weitere Informationen zur Kompatibilität finden Sie in der [Einführung in das neue Azure PowerShell Az-Modul](/powershell/azure/new-azureps-module-az).

```powershell
# sign in to Azure
Connect-AzAccount

# for multiple subscriptions, uncomment and set to the subscription you want to work with
#$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
#Set-AzContext -SubscriptionId $subscriptionId

$appName = "{app-name}" # display name for your app, must be unique in your directory
$uri = "http://{app-name}" # does not need to be a real uri
$secret = "{app-password}"

# create an AAD app
$azureAdApplication = New-AzADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret

# create a Service Principal for the app
$svcprincipal = New-AzADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

Start-Sleep -s 15 # to avoid a PrincipalNotFound error, pause here for 15 seconds

# if you still get a PrincipalNotFound error, then rerun the following until successful.
$roleassignment = New-AzRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

# output the values we need for our C# application to successfully authenticate
Write-Output "Copy these values into the C# sample app"

Write-Output "_subscriptionId:" (Get-AzContext).Subscription.SubscriptionId
Write-Output "_tenantId:" (Get-AzContext).Tenant.TenantId
Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
Write-Output "_applicationSecret:" $secret
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
# sign in to Azure
az login

# for multiple subscriptions, uncomment and set to the subscription you want to work with
#$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
#az account set --subscription $subscriptionId

$appName = "{app-name}" # display name for your app, must be unique in your directory
$uri = "http://{app-name}" # does not need to be a real uri
$secret = "{app-password}"

# create an AAD app
$azureAdApplication = az ad app create --display-name $appName --homepage $Uri --identifier-uris $Uri --password $secret

# create a Service Principal for the app
$svcprincipal = az ad sp create --id $azureAdApplication.ApplicationId

Start-Sleep -s 15 # to avoid a PrincipalNotFound error, pause for 15 seconds

# if you still get a PrincipalNotFound error, then rerun the following until successful.
$roleassignment = az role assignment create --role "Contributor" --assignee $azureAdApplication.ApplicationId.Guid

# output the values we need for our C# application to successfully authenticate
Write-Output "Copy these values into the C# sample app"

Write-Output "_subscriptionId:" (az account show --query "id")
Write-Output "_tenantId:" (az account show --query "tenantId")
Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
Write-Output "_applicationSecret:" $secret
```

* * *

## <a name="see-also"></a>Weitere Informationen

[Erstellen einer SQL-Datenbank mit C#](sql-database-get-started-csharp.md)  
[Herstellen einer Verbindung mit SQL-Datenbank unter Verwendung der Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md)
