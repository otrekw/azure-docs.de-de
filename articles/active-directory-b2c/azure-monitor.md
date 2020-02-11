---
title: Überwachen von Azure AD B2C mit Azure Monitor
titleSuffix: Azure AD B2C
description: Es wird beschrieben, wie Sie Azure AD B2C-Ereignisse mit Azure Monitor protokollieren, indem Sie die delegierte Ressourcenverwaltung verwenden.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.author: marsma
ms.subservice: B2C
ms.date: 02/03/2020
ms.openlocfilehash: 108c9c1112327a3fcadeff4c4074f31f976a4e3d
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77026410"
---
# <a name="monitor-azure-ad-b2c-with-azure-monitor"></a>Überwachen von Azure AD B2C mit Azure Monitor

Verwenden Sie Azure Monitor, um Aktivitätsereignisse der Azure AD B2C-Nutzung (Azure Active Directory B2C) an verschiedene Überwachungslösungen zu leiten. Sie können die Protokolle entweder zur langfristigen Verwendung speichern oder in SIEM-Drittanbietertools (Security Information & Event Management) integrieren, um Einblicke in Ihre Umgebung zu gewinnen.

Sie können Protokollereignisse an folgende Komponenten weiterleiten:

* Ein Azure-Speicherkonto.
* Einen Azure Event Hub (mit Integration in Ihre Splunk- und Sumo Logic-Instanzen).
* Einen Azure Log Analytics-Arbeitsbereich (zum Analysieren von Daten, Erstellen von Dashboards und Warnen vor bestimmten Ereignissen).

![Azure Monitor](./media/azure-monitor/azure-monitor-flow.png)

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen der Schritte in diesem Artikel stellen Sie eine Azure Resource Manager-Vorlage bereit, indem Sie das Azure PowerShell-Modul verwenden.

* [Azure PowerShell-Modul](https://docs.microsoft.com/powershell/azure/install-az-ps), Version 6.13.1 oder höher

Sie können auch die [Azure Cloud Shell](https://shell.azure.com) nutzen, die die aktuelle Version des Azure PowerShell-Moduls enthält.

## <a name="delegated-resource-management"></a>Delegierte Ressourcenverwaltung

Für Azure AD B2C wird die [Azure Active Directory-Überwachung](../active-directory/reports-monitoring/overview-monitoring.md) genutzt. Zum Aktivieren der *Diagnoseeinstellungen* in Azure Active Directory auf Ihrem Azure AD B2C-Mandanten nutzen Sie die [delegierte Ressourcenverwaltung](../lighthouse/concepts/azure-delegated-resource-management.md).

Sie autorisieren einen Benutzer in Ihrem Azure AD B2C-Verzeichnis (**Dienstanbieter**), um die Azure Monitor-Instanz auf dem Mandanten zu konfigurieren, der Ihr Azure-Abonnement (**Kunde**) enthält. Zum Erstellen der Autorisierung stellen Sie eine [Azure Resource Manager](../azure-resource-manager/index.yml)-Vorlage auf Ihrem Azure AD-Mandanten bereit, der das Abonnement enthält. In den folgenden Abschnitten wird der Prozess Schritt für Schritt beschrieben.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Führen Sie auf dem Azure AD-Mandanten (Azure Active Directory), der Ihr Azure-Abonnement enthält (*nicht* das Verzeichnis mit Ihrem Azure AD B2C-Mandanten), die [Erstellung einer Ressourcengruppe](../azure-resource-manager/management/manage-resource-groups-portal.md#create-resource-groups) durch. Verwenden Sie die folgenden Werte:

* **Abonnement**: Wählen Sie Ihr Azure-Abonnement.
* **Ressourcengruppe**: Geben Sie einen Namen für die Ressourcengruppe ein. Beispiel: *azure-ad-b2c-monitor*.
* **Region**: Wählen Sie einen Azure-Standort aus. Beispiel: *USA, Mitte*.

## <a name="delegate-resource-management"></a>Delegieren der Ressourcenverwaltung

Sammeln Sie als Nächstes die folgenden Informationen:

**Verzeichnis-ID** Ihres Azure AD B2C-Verzeichnisses (auch als Mandanten-ID bezeichnet).

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com/) als Benutzer mit der Rolle *Benutzeradministrator* (oder höher) an.
1. Wählen Sie auf der Symbolleiste des Portals das Symbol **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Wählen Sie **Azure Active Directory** und dann **Eigenschaften** aus.
1. Notieren Sie sich die **Verzeichnis-ID**.

**Objekt-ID** der Azure AD B2C-Gruppe bzw. des -Benutzers, der bzw. dem Sie die Berechtigung *Mitwirkender* für die Ressourcengruppe gewähren möchten, die Sie zuvor im Verzeichnis mit Ihrem Abonnement erstellt haben.

Um die Verwaltung zu vereinfachen, empfiehlt es sich, für jede Rolle Azure AD-*Benutzergruppen* zu verwenden. Dies ermöglicht es Ihnen, der Gruppe einzelne Benutzer hinzuzufügen oder daraus zu entfernen, anstatt einem Benutzer Berechtigungen direkt zuzuweisen. In dieser exemplarischen Vorgehensweise fügen Sie einen Benutzer hinzu.

1. Wählen Sie – bei weiterhin bestehender Auswahl von **Azure Active Directory** im Azure-Portal – die Option **Benutzer** und dann einen Benutzer aus.
1. Notieren Sie sich die **Objekt-ID** des Benutzers.

### <a name="create-an-azure-resource-manager-template"></a>Erstellen einer Azure Resource Manager-Vorlage

Erstellen Sie für das Onboarding Ihres Azure AD-Mandanten (**Kunde**) eine [Azure Resource Manager-Vorlage](../lighthouse/how-to/onboard-customer.md) mit den folgenden Informationen für Ihr Angebot. Die Werte `mspOfferName` und `mspOfferDescription` werden eingeblendet, wenn Sie im Azure-Portal auf der [Seite „Dienstanbieter“](../lighthouse/how-to/view-manage-service-providers.md) die Angebotsdetails anzeigen.

| Feld   | Definition |
|---------|------------|
| `mspOfferName`                     | Ein Name, der diese Definition beschreibt. Beispiel: *Azure AD B2C Managed Services*. Dieser Wert wird für den Kunden als Titel des Angebots angezeigt. |
| `mspOfferDescription`              | Eine kurze Beschreibung Ihres Angebots. Beispiel: *Ermöglicht Azure Monitor in Azure AD B2C*.|
| `rgName`                           | Der Name der Ressourcengruppe, die Sie zuvor auf Ihrem Azure AD-Mandanten erstellt haben. Beispiel: *azure-ad-b2c-monitor*. |
| `managedByTenantId`                | Die **Verzeichnis-ID** Ihres Azure AD B2C-Mandanten (auch als Mandanten-ID bezeichnet). |
| `authorizations.value.principalId` | Die **Objekt-ID** der B2C-Gruppe oder des -Benutzers, die bzw. der Zugriff auf die Ressourcen unter diesem Azure-Abonnement haben soll. Geben Sie für diese exemplarische Vorgehensweise die Objekt-ID des Benutzers an, die Sie zuvor notiert haben. |

Laden Sie die Azure Resource Manager-Vorlage und die Parameterdateien herunter:

- [rgDelegatedResourceManagement.json](https://raw.githubusercontent.com/Azure/Azure-Lighthouse-samples/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management/rgDelegatedResourceManagement.json)
- [rgDelegatedResourceManagement.parameters.json](https://raw.githubusercontent.com/Azure/Azure-Lighthouse-samples/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management/rgDelegatedResourceManagement.parameters.json)

Aktualisieren Sie anschließend die Parameterdatei mit den Werten, die Sie zuvor notiert haben. Der folgende JSON-Codeausschnitt enthält ein Beispiel für eine Parameterdatei der Azure Resource Manager-Vorlage. Verwenden Sie für `authorizations.value.roleDefinitionId` den Wert der [integrierten Rolle](../role-based-access-control/built-in-roles.md) für die *Rolle „Mitwirkender“* : `b24988ac-6180-42a0-ab88-20f7382dd24c`.

```JSON
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "mspOfferName": {
            "value": "Azure AD B2C Managed Services"
        },
        "mspOfferDescription": {
            "value": "Enables Azure Monitor in Azure AD B2C"
        },
        "rgName": {
            "value": "azure-ad-b2c-monitor"
        },
        "managedByTenantId": {
            "value": "<Replace with DIRECTORY ID of Azure AD B2C tenant (tenant ID)>"
        },
        "authorizations": {
            "value": [
                {
                    "principalId": "<Replace with user's OBJECT ID>",
                    "principalIdDisplayName": "Azure AD B2C tenant administrator",
                    "roleDefinitionId": "b24988ac-6180-42a0-ab88-20f7382dd24c"
                }
            ]
        }
    }
}
```

### <a name="deploy-the-azure-resource-manager-templates"></a>Bereitstellen der Azure Resource Manager-Vorlagen

Stellen Sie nach der Aktualisierung Ihrer Parameterdatei die Azure Resource Manager-Vorlage auf dem Azure-Mandanten auf Abonnementebene bereit. Da es sich hierbei um eine Bereitstellung auf Abonnementebene handelt, kann sie nicht im Azure-Portal initiiert werden. Sie können für die Bereitstellung das Azure PowerShell-Modul oder die Azure CLI verwenden. Die Azure PowerShell-Methode ist unten dargestellt.

Melden Sie sich mit [Connect-AzAccount](/powershell/azure/authenticate-azureps) an dem Verzeichnis an, das Ihr Abonnement enthält. Verwenden Sie das Flag `-tenant`, um die Authentifizierung im richtigen Verzeichnis zu erzwingen.

```PowerShell
Connect-AzAccount -tenant contoso.onmicrosoft.com
```

Verwenden Sie das Cmdlet [Get-AzSubscription](/powershell/module/az.accounts/get-azsubscription), um die Abonnements, auf die mit dem aktuellen Konto zugegriffen werden kann, unter dem Azure AD-Mandanten aufzulisten. Notieren Sie sich die ID des Abonnements, das Sie für Ihren Azure AD B2C-Mandanten projizieren möchten.

```PowerShell
Get-AzSubscription
```

Wechseln Sie als Nächstes zu dem Abonnement, das Sie für den Azure AD B2C-Mandanten projizieren möchten:

``` PowerShell
Select-AzSubscription <subscription ID>
```

Stellen Sie abschließend die Azure Resource Manager-Vorlage und die Parameterdateien bereit, die Sie zuvor heruntergeladen und aktualisiert haben. Ersetzen Sie die Werte `Location`, `TemplateFile` und `TemplateParameterFile` entsprechend.

```PowerShell
New-AzDeployment -Name "AzureADB2C" `
                 -Location "centralus" `
                 -TemplateFile "C:\Users\azureuser\Documents\rgDelegatedResourceManagement.json" `
                 -TemplateParameterFile "C:\Users\azureuser\Documents\rgDelegatedResourceManagement.parameters.json" `
                 -Verbose
```

Bei der erfolgreichen Bereitstellung der Vorlage wird in etwa die folgende Ausgabe erzeugt (hier gekürzt angegeben):

```Console
PS /usr/csuser/clouddrive> New-AzDeployment -Name "AzureADB2C" `
>>                  -Location "centralus" `
>>                  -TemplateFile "rgDelegatedResourceManagement.json" `
>>                  -TemplateParameterFile "rgDelegatedResourceManagement.parameters.json" `
>>                  -Verbose
WARNING: Breaking changes in the cmdlet 'New-AzDeployment' :
WARNING:  - The cmdlet 'New-AzSubscriptionDeployment' is replacing this cmdlet.


WARNING: NOTE : Go to https://aka.ms/azps-changewarnings for steps to suppress this breaking change warning, and other information on breaking changes in Azure PowerShell.
VERBOSE: 7:25:14 PM - Template is valid.
VERBOSE: 7:25:15 PM - Create template deployment 'AzureADB2C'
VERBOSE: 7:25:15 PM - Checking deployment status in 5 seconds
VERBOSE: 7:25:42 PM - Resource Microsoft.ManagedServices/registrationDefinitions '44444444-4444-4444-4444-444444444444' provisioning status is succeeded
VERBOSE: 7:25:48 PM - Checking deployment status in 5 seconds
VERBOSE: 7:25:53 PM - Resource Microsoft.Resources/deployments 'rgAssignment' provisioning status is running
VERBOSE: 7:25:53 PM - Checking deployment status in 5 seconds
VERBOSE: 7:25:59 PM - Resource Microsoft.ManagedServices/registrationAssignments '11111111-1111-1111-1111-111111111111' provisioning status is running
VERBOSE: 7:26:17 PM - Checking deployment status in 5 seconds
VERBOSE: 7:26:23 PM - Resource Microsoft.ManagedServices/registrationAssignments '11111111-1111-1111-1111-111111111111' provisioning status is succeeded
VERBOSE: 7:26:23 PM - Checking deployment status in 5 seconds
VERBOSE: 7:26:29 PM - Resource Microsoft.Resources/deployments 'rgAssignment' provisioning status is succeeded

DeploymentName          : AzureADB2C
Location                : centralus
ProvisioningState       : Succeeded
Timestamp               : 1/31/20 7:26:24 PM
Mode                    : Incremental
TemplateLink            :
Parameters              :
                          Name                   Type                       Value
                          =====================  =========================  ==========
                          mspOfferName           String                     Azure AD B2C Managed Services
                          mspOfferDescription    String                     Enables Azure Monitor in Azure AD B2C
...
```

Nach der Bereitstellung der Vorlage kann es einige Minuten dauern, bis die Ressourcenprojektion abgeschlossen ist. Unter Umständen müssen Sie einige Minuten warten (normalerweise nicht mehr als fünf), bevor Sie mit dem nächsten Abschnitt fortfahren, um das Abonnement auszuwählen.

## <a name="select-your-subscription"></a>Wählen Sie Ihr Abonnement aus.

Nachdem Sie die Vorlage bereitgestellt und einige Minuten auf den Abschluss der Ressourcenprojektion gewartet haben, können Sie Ihr Abonnement mit den folgenden Schritten Ihrem Azure AD B2C-Verzeichnis hinzufügen.

1. Führen Sie die **Abmeldung** vom Azure-Portal durch, falls Sie gerade angemeldet sind. Dieser und die folgenden Schritte werden ausgeführt, um Ihre Anmeldeinformationen für die Portalsitzung zu aktualisieren.
1. Melden Sie sich mit Ihrem Azure AD B2C-Administratorkonto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie auf der Symbolleiste im Portal das Symbol **Verzeichnis + Abonnement** aus.
1. Wählen Sie das Verzeichnis aus, das Ihr Abonnement enthält.

    ![Wechseln des Verzeichnisses](./media/azure-monitor/azure-monitor-portal-03-select-subscription.png)
1. Vergewissern Sie sich, dass Sie das richtige Verzeichnis und Abonnement ausgewählt haben. In diesem Beispiel werden alle Verzeichnisse und Abonnements ausgewählt.

    ![Auswahl aller Verzeichnisse im Verzeichnis- und Abonnementfilter](./media/azure-monitor/azure-monitor-portal-04-subscriptions-selected.png)

## <a name="configure-diagnostic-settings"></a>Konfigurieren von Diagnoseeinstellungen

Nachdem Sie die Ressourcenverwaltung delegiert und Ihr Abonnement ausgewählt haben, können Sie im Azure-Portal die [Diagnoseeinstellungen erstellen](../active-directory/reports-monitoring/overview-monitoring.md).

Konfigurieren Sie die Überwachungseinstellungen für Azure AD B2C-Aktivitätsprotokolle wie folgt:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
1. Wählen Sie auf der Symbolleiste des Portals das Symbol **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Wählen Sie **Azure Active Directory** aus.
1. Wählen Sie unter **Überwachung** die Option **Diagnoseeinstellungen** aus.
1. Wählen Sie **+Diagnoseeinstellung hinzufügen**  aus.

    ![Bereich „Diagnoseeinstellungen“ im Azure-Portal](./media/azure-monitor/azure-monitor-portal-05-diagnostic-settings-pane-enabled.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Hinzufügen und Konfigurieren von Diagnoseeinstellungen in Azure Monitor finden Sie im folgenden Tutorial in der Azure Monitor-Dokumentation:

[Tutorial: Sammeln und Analysieren von Ressourcenprotokollen von einer Azure-Ressource](/azure-monitor/learn/tutorial-resource-logs.md)
