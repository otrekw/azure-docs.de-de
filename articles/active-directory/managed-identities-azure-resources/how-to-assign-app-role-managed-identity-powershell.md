---
title: 'Zuweisen einer verwalteten Identität zu einer Anwendungsrolle mithilfe von PowerShell: Azure AD'
description: Hier finden Sie Schrittanleitungen für das Zuweisen einer verwalteten Identität zur Rolle einer anderen Anwendung mithilfe von PowerShell.
services: active-directory
documentationcenter: ''
author: johndowns
manager: ''
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/10/2020
ms.author: jodowns
ms.collection: M365-identity-device-management
ms.openlocfilehash: f2d844054e3744aafa01f43ded354a5b5d9f8b4b
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2021
ms.locfileid: "98732064"
---
# <a name="assign-a-managed-identity-access-to-an-application-role-using-powershell"></a>Zuweisen des Zugriffs einer verwalteten Identität auf eine Anwendungsrolle mithilfe von PowerShell

Verwaltete Identitäten für Azure-Ressourcen stellen Azure-Diensten eine Identität in Azure Active Directory bereit. Für die Funktionalität sind keine Anmeldeinformationen in Ihrem Code erforderlich. Azure-Dienste nutzen diese Identitäten, um sich bei Diensten zu authentifizieren, die die Azure AD-Authentifizierung unterstützen. Anwendungsrollen stellen eine Art rollenbasierter Zugriffssteuerung dar und ermöglichen es einem Dienst, Autorisierungsregeln zu implementieren.

In diesem Artikel erfahren Sie, wie Sie einer Anwendungsrolle, die durch eine andere Anwendung verfügbar gemacht wird, über Azure AD PowerShell eine verwaltete Identität zuweisen.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit verwalteten Identitäten für Azure-Ressourcen vertraut sind, helfen Ihnen die Informationen in der [Übersicht](overview.md) weiter. **Machen Sie sich den [Unterschied zwischen einer vom System und einer vom Benutzer zugewiesenen verwalteten Identität](overview.md#managed-identity-types)** bewusst.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.
- Zum Ausführen der Beispielskripts haben Sie zwei Möglichkeiten:
    - Verwenden Sie die [Azure Cloud Shell](../../cloud-shell/overview.md), die Sie über die Schaltfläche **Testen** in der rechten oberen Ecke der Codeblöcke öffnen können.
    - Führen Sie Skripts lokal aus, indem Sie die neueste Version von [Azure AD PowerShell](/powershell/azure/active-directory/install-adv2) installieren.

## <a name="assign-a-managed-identity-access-to-another-applications-app-role"></a>Zuweisen des Zugriffs einer verwalteten Identität auf die App-Rolle einer anderen Anwendung

1. Aktivieren Sie die verwaltete Identität für eine Azure-Ressource, [zum Beispiel einen virtuellen Azure-Computer](qs-configure-powershell-windows-vm.md).

1. Suchen Sie die Objekt-ID des Dienstprinzipals der verwalteten Identität.

   **Bei einer systemseitig zugewiesenen verwalteten Identität** finden Sie die Objekt-ID im Azure-Portal auf der Seite **Identität** der Ressource. Sie können auch das folgende PowerShell-Skript verwenden, um die Objekt-ID zu suchen. Sie benötigen die Ressourcen-ID der in Schritt 1 erstellten Ressource, die Sie im Azure-Portal auf der Seite **Eigenschaften** der Ressource finden.

    ```powershell
    $resourceIdWithManagedIdentity = '/subscriptions/{my subscription ID}/resourceGroups/{my resource group name}/providers/Microsoft.Compute/virtualMachines/{my virtual machine name}'
    (Get-AzResource -ResourceId $resourceIdWithManagedIdentity).Identity.PrincipalId
    ```

    **Bei einer benutzerseitig zugewiesenen verwalteten Identität** finden Sie die Objekt-ID der verwalteten Identität im Azure-Portal auf der Seite **Übersicht** der Ressource. Sie können auch das folgende PowerShell-Skript verwenden, um die Objekt-ID zu suchen. Sie benötigen die Ressourcen-ID der benutzerseitig zugewiesenen verwalteten Identität.

    ```powershell
    $userManagedIdentityResourceId = '/subscriptions/{my subscription ID}/resourceGroups/{my resource group name}/providers/Microsoft.ManagedIdentity/userAssignedIdentities/{my managed identity name}'
    (Get-AzResource -ResourceId $userManagedIdentityResourceId).Properties.PrincipalId
    ```

1. Erstellen Sie eine neue Anwendungsregistrierung, um den Dienst zu repräsentieren, an den Ihre verwaltete Identität eine Anforderung senden soll. Wenn die API oder der Dienst, die bzw. der die Gewährung der App-Rolle für die verwaltete Identität verfügbar macht, bereits über einen Dienstprinzipal in Ihrem Azure AD-Mandanten verfügt, überspringen Sie diesen Schritt. Sie können diesen Schritt beispielsweise dann überspringen, wenn Sie der verwalteten Identität Zugriff auf die Microsoft Graph-API gewähren möchten.

1. Suchen Sie die Objekt-ID des Dienstprinzipals der Dienstanwendung. Sie finden diese im Azure-Portal. Wechseln Sie zu Azure Active Directory, und öffnen Sie die Seite **Unternehmensanwendungen**. Suchen Sie nach der Anwendung und dort nach der **Objekt-ID**. Sie können die Objekt-ID des Dienstprinzipals auch mithilfe des folgenden PowerShell-Skripts anhand ihres Anzeigenamens suchen:

    ```powershell
    $serverServicePrincipalObjectId = (Get-AzureADServicePrincipal -Filter "DisplayName eq '$applicationName'").ObjectId
    ```

    > [!NOTE]
    > Anzeigenamen für Anwendungen sind nicht eindeutig, daher müssen Sie überprüfen, ob Sie den Dienstprinzipal der richtigen Anwendung abrufen.

1. Fügen Sie der in Schritt 3 erstellten Anwendung eine [App-Rolle](../develop/howto-add-app-roles-in-azure-ad-apps.md) hinzu. Sie können die Rolle im Azure-Portal oder mithilfe von Microsoft Graph erstellen. Sie können beispielsweise folgendermaßen eine Rolle hinzufügen:

    ```json
    {
        "allowedMemberTypes": [
            "Application"
        ],
        "displayName": "Read data from MyApi",
        "id": "0566419e-bb95-4d9d-a4f8-ed9a0f147fa6",
        "isEnabled": true,
        "description": "Allow the application to read data as itself.",
        "value": "MyApi.Read.All"
    }
    ```

1. Weisen Sie der verwalteten Identität die App-Rolle zu. Sie benötigen die folgenden Informationen, um die App-Rolle zuzuweisen:
    * `managedIdentityObjectId`: die Objekt-ID des Dienstprinzipals der verwalteten Identität, die Sie in Schritt 2 ermittelt haben
    * `serverServicePrincipalObjectId`: die Objekt-ID des Dienstprinzipals der Serveranwendung, die Sie in Schritt 4 ermittelt haben
    * `appRoleId`: die ID der App-Rolle, die von der Server-App verfügbar gemacht wird und die Sie in Schritt 5 generiert haben; im Beispiel lautet die App-Rollen-ID `0566419e-bb95-4d9d-a4f8-ed9a0f147fa6`
   
   Führen Sie das folgende PowerShell-Skript aus, um die Rollenzuweisung hinzuzufügen:

    ```powershell
    New-AzureADServiceAppRoleAssignment -ObjectId $managedIdentityObjectId -Id $appRoleId -PrincipalId $managedIdentityObjectId -ResourceId $serverServicePrincipalObjectId
    ```

## <a name="complete-script"></a>Vollständiges Skript

Dieses Beispielskript zeigt, wie Sie die verwaltete Identität einer Azure-Web-App einer App-Rolle zuweisen.

```powershell
# Install the module. (You need admin on the machine.)
# Install-Module AzureAD

# Your tenant ID (in the Azure portal, under Azure Active Directory > Overview).
$tenantID = '<tenant-id>'

# The name of your web app, which has a managed identity that should be assigned to the server app's app role.
$webAppName = '<web-app-name>'
$resourceGroupName = '<resource-group-name-containing-web-app>'

# The name of the server app that exposes the app role.
$serverApplicationName = '<server-application-name>' # For example, MyApi

# The name of the app role that the managed identity should be assigned to.
$appRoleName = '<app-role-name>' # For example, MyApi.Read.All

# Look up the web app's managed identity's object ID.
$managedIdentityObjectId = (Get-AzWebApp -ResourceGroupName $resourceGroupName -Name $webAppName).identity.principalid

Connect-AzureAD -TenantId $tenantID

# Look up the details about the server app's service principal and app role.
$serverServicePrincipal = (Get-AzureADServicePrincipal -Filter "DisplayName eq '$serverApplicationName'")
$serverServicePrincipalObjectId = $serverServicePrincipal.ObjectId
$appRoleId = ($serverServicePrincipal.AppRoles | Where-Object {$_.Value -eq $appRoleName }).Id

# Assign the managed identity access to the app role.
New-AzureADServiceAppRoleAssignment `
    -ObjectId $managedIdentityObjectId `
    -Id $appRoleId `
    -PrincipalId $managedIdentityObjectId `
    -ResourceId $serverServicePrincipalObjectId
```

## <a name="next-steps"></a>Nächste Schritte

- [Verwaltete Identitäten für Azure-Ressourcen – Übersicht](overview.md)
- Informationen zum Aktivieren einer verwalteten Identität auf einem virtuellen Azure-Computer finden Sie unter [Konfigurieren von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Azure-Computer mithilfe von PowerShell](qs-configure-powershell-windows-vm.md).