---
title: Sichere WebHook-Zustellung mit Azure AD in Azure Event Grid
description: Beschreibt, wie Ereignisse mittels Azure Event Grid an HTTPS-Endpunkte zugestellt werden, die von Azure Active Directory geschützt werden.
ms.topic: how-to
ms.date: 03/20/2021
ms.openlocfilehash: 1298910db78ba468dd9744e84ee4629161e0a776
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106076035"
---
# <a name="publish-events-to-azure-active-directory-protected-endpoints"></a>Veröffentlichen von Ereignissen auf mit Azure Active Directory geschützten Endpunkten
Dieser Artikel beschreibt, wie Sie Azure Active Directory (Azure AD) verwenden, um die Verbindung zwischen Ihrem **Ereignisabonnement** und Ihrem **Webhook-Endpunkt** sichern. Eine Übersicht über Azure AD-Anwendungen und -Dienstprinzipale finden Sie unter [Microsoft Identity Platform (v2.0): Übersicht](../active-directory/develop/v2-overview.md).

In diesem Artikel wird das Azure-Portal zur Demonstration verwendet. Die Funktion kann jedoch auch über mit der CLI, der PowerShell oder den SDKs aktiviert werden.


## <a name="create-an-azure-ad-application"></a>Erstellen einer Azure AD-Anwendung
Registrieren Sie Ihren Webhook bei Azure AD, indem Sie eine Azure AD-Anwendung für Ihren geschützten Endpunkt erstellen. Siehe [Szenario: Geschützte Web-API](https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-overview). Konfigurieren Sie Ihre geschützte API so, dass Sie von einer Daemon-App aufgerufen wird.
    
## <a name="enable-event-grid-to-use-your-azure-ad-application"></a>Aktivieren von Event Grid zur Verwendung Ihrer Azure AD-Anwendung
In diesem Abschnitt erfahren Sie, wie Sie Event Grid für die Verwendung Ihrer Azure AD-Anwendung aktivieren. 

> [!NOTE]
> Sie müssen Mitglied der [Rolle „Azure AD-Anwendungsadministrator“](../active-directory/roles/permissions-reference.md#all-roles) sein, um dieses Skript ausführen zu können.

### <a name="connect-to-your-azure-tenant"></a>Herstellen einer Verbindung mit Ihrem Azure-Mandanten
Stellen Sie zunächst mithilfe des `Connect-AzureAD`-Befehls eine Verbindung mit Ihrem Azure-Mandanten her. 

```PowerShell
$myWebhookAadTenantId = "<Your Webhook's Azure AD tenant id>"

Connect-AzureAD -TenantId $myWebhookAadTenantId
```

### <a name="create-microsofteventgrid-service-principal"></a>Erstellen des Microsoft.EventGrid-Dienstprinzipals
Führen Sie das folgende Skript aus, um den Dienstprinzipal für **Microsoft.EventGrid** zu erstellen, wenn er nicht bereits vorhanden ist. 

```PowerShell
# This is the "Azure Event Grid" Azure Active Directory (AAD) AppId
$eventGridAppId = "4962773b-9cdb-44cf-a8bf-237846a00ab7"

# Create the "Azure Event Grid" AAD Application service principal if it doesn't exist
$eventGridSP = Get-AzureADServicePrincipal -Filter ("appId eq '" + $eventGridAppId + "'")
if ($eventGridSP -match "Microsoft.EventGrid")
{
    Write-Host "The Service principal is already defined.`n"
}
else
{
    # Create a service principal for the "Azure Event Grid" AAD Application and add it to the role
    Write-Host "Creating the Azure Event Grid service principal"
    $eventGridSP = New-AzureADServicePrincipal -AppId $eventGridAppId
}
```

### <a name="create-a-role-for-your-application"></a>Erstellen einer Rolle für Ihre Anwendung   
Führen Sie das folgende Skript aus, um eine Rolle für die Azure AD-Anwendung zu erstellen. In diesem Beispiel lautet der Rollenname wie folgt: **AzureEventGridSecureWebhookSubscriber**. Ändern Sie `$myTenantId` des PowerShell-Skripts so, dass Ihre Azure AD-Mandanten-ID und `$myAzureADApplicationObjectId` mit der Objekt-ID Ihrer Azure AD-Anwendung verwendet wird.

```PowerShell
# This is your Webhook's Azure AD Application's ObjectId. 
$myWebhookAadApplicationObjectId = "<Your webhook's aad application object id>"

# This is the name of the new role we will add to your Azure AD Application
$eventGridRoleName = "AzureEventGridSecureWebhookSubscriber"
       
# Create an application role of given name and description
Function CreateAppRole([string] $Name, [string] $Description)
{
    $appRole = New-Object Microsoft.Open.AzureAD.Model.AppRole
    $appRole.AllowedMemberTypes = New-Object System.Collections.Generic.List[string]
    $appRole.AllowedMemberTypes.Add("Application");
    $appRole.AllowedMemberTypes.Add("User");
    $appRole.DisplayName = $Name
    $appRole.Id = New-Guid
    $appRole.IsEnabled = $true
    $appRole.Description = $Description
    $appRole.Value = $Name;
    return $appRole
}
       
# Get my Azure AD Application, it's roles and service principal
$myApp = Get-AzureADApplication -ObjectId $myWebhookAadApplicationObjectId
$myAppRoles = $myApp.AppRoles

Write-Host "App Roles before addition of new role.."
Write-Host $myAppRoles
       
# Create the role if it doesn't exist
if ($myAppRoles -match $eventGridRoleName)
{
    Write-Host "The Azure Event Grid role is already defined.`n"
}
else
{      
    # Add our new role to the Azure AD Application
    Write-Host "Creating the Azure Event Grid role in Azure Ad Application: " $myWebhookAadApplicationObjectId
    $newRole = CreateAppRole -Name $eventGridRoleName -Description "Azure Event Grid Role"
    $myAppRoles.Add($newRole)
    Set-AzureADApplication -ObjectId $myApp.ObjectId -AppRoles $myAppRoles
}

# print application's roles
Write-Host "My Azure AD Application's Roles: "
Write-Host $myAppRoles

```

### <a name="create-a-role-assignment"></a>Erstellen einer Rollenzuweisung
Die Rollenzuweisung sollte in der Azure AD-Webhook-App für die AAD-App oder den AAD-Benutzer erstellt werden, die bzw. der das Ereignisabonnement erstellt. Verwenden Sie eins der folgenden Skripts, je nachdem, ob das Ereignisabonnement von einer AAD-App oder einem AAD-Benutzer erstellt wird.

#### <a name="option-a-create-a-role-assignment-for-event-subscription-aad-app"></a>Option A. Erstellen einer Rollenzuweisung für die AAD-App des Ereignisabonnements 

```powershell
# This is the app id of the application which will create event subscription. Set to $null if you are not assigning the role to app.
$eventSubscriptionWriterAppId = "<the app id of the application which will create event subscription>"

$myServicePrincipal = Get-AzureADServicePrincipal -Filter ("appId eq '" + $myApp.AppId + "'")

$eventSubscriptionWriterSP = Get-AzureADServicePrincipal -Filter ("appId eq '" + $eventSubscriptionWriterAppId + "'")
if ($eventSubscriptionWriterSP -eq $null)
{
        $eventSubscriptionWriterSP = New-AzureADServicePrincipal -AppId $eventSubscriptionWriterAppId
}

Write-Host "Creating the Azure Ad App Role assignment for application: " $eventSubscriptionWriterAppId
New-AzureADServiceAppRoleAssignment -Id $myApp.AppRoles[0].Id -ResourceId $myServicePrincipal.ObjectId -ObjectId $eventSubscriptionWriterSP.ObjectId -PrincipalId $eventSubscriptionWriterSP.ObjectId
```

#### <a name="option-b-create-a-role-assignment-for-event-subscription-aad-user"></a>Option B. Erstellen einer Rollenzuweisung für den AAD-Benutzer des Ereignisabonnements 

```powershell
# This is the user principal name of the user who will create event subscription. Set to $null if you are not assigning the role to user.
$eventSubscriptionWriterUserPrincipalName = "<the user principal name of the user who will create event subscription>"

$myServicePrincipal = Get-AzureADServicePrincipal -Filter ("appId eq '" + $myApp.AppId + "'")
    
Write-Host "Creating the Azure Ad App Role assignment for user: " $eventSubscriptionWriterUserPrincipalName
$eventSubscriptionWriterUser = Get-AzureAdUser -ObjectId $eventSubscriptionWriterUserPrincipalName
New-AzureADUserAppRoleAssignment -Id $myApp.AppRoles[0].Id -ResourceId $myServicePrincipal.ObjectId -ObjectId $eventSubscriptionWriterUser.ObjectId -PrincipalId $eventSubscriptionWriterUser.ObjectId
```

### <a name="add-event-grid-service-principal-to-the-role"></a>Hinzufügen des Event Grid-Dienstprinzipals zur Rolle
Führen Sie den Befehl „New-AzureADServiceAppRoleAssignment“ aus, um der im vorherigen Schritt erstellten Rolle den Event Grid-Dienstprinzipal zuzuweisen.

```powershell
New-AzureADServiceAppRoleAssignment -Id $myApp.AppRoles[0].Id -ResourceId $myServicePrincipal.ObjectId -ObjectId $eventGridSP.ObjectId -PrincipalId $eventGridSP.ObjectId
```

Führen Sie die folgenden Befehle aus, um Informationen auszugeben, die Sie später verwenden werden.

```powershell
Write-Host "My Webhook's Azure AD Tenant Id:  $myWebhookAadTenantId"
Write-Host "My Webhook's Azure AD Application Id: $($myApp.AppId)"
Write-Host "My Webhook's Azure AD Application ObjectId Id$($myApp.ObjectId)"
```

    
## <a name="configure-the-event-subscription"></a>Konfigurieren des Ereignisabonnements
Führen Sie beim Erstellen eines Ereignisabonnements die folgenden Schritte aus:

1. Wählen Sie den Endpunkttyp als **Webhook** aus. 
1. Geben Sie den **URI** des Endpunkts an.

    ![Auswählen des Endpunkttyps „Webhook“](./media/secure-webhook-delivery/select-webhook.png)
1. Wählen Sie oben auf der Seite **Ereignisabonnements erstellen** die Registerkarte **Zusätzliche Features** aus.
1. Führen Sie auf der Registerkarte **Zusätzliche Features** die folgenden Schritte aus:
    1. Wählen Sie **AAD-Authentifizierung verwenden** aus, und konfigurieren Sie die Mandanten-ID und die Anwendungs-ID:
    1. Kopieren Sie die Azure AD-Mandanten-ID aus der Ausgabe des Skripts, und geben Sie sie in das Feld **AAD-Mandanten-ID** ein.
    1. Kopieren Sie die Azure AD-Anwendungs-ID aus der Ausgabe des Skripts, und geben Sie sie in das Feld **AAD-Anwendungs-ID** ein.

        ![Sichere Webhookaktion](./media/secure-webhook-delivery/aad-configuration.png)



## <a name="next-steps"></a>Nächste Schritte

* Informationen zur Überwachung von Ereignisübermittlungen finden Sie unter [Überwachen der Event Grid-Nachrichtenübermittlung](monitor-event-delivery.md).
* Weitere Informationen zum Authentifizierungsschlüssel finden Sie unter [Event Grid – Sicherheit und Authentifizierung](security-authentication.md).
* Weitere Informationen zum Erstellen eines Azure Event Grid-Abonnements finden Sie unter [Event Grid-Abonnementschema](subscription-creation-schema.md).
