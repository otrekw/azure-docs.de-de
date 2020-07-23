---
title: Konfigurieren von BYOS (Bring Your Own Storage) für Profiler und Momentaufnahmedebugger
description: Konfigurieren von BYOS (Bring Your Own Storage) für Profiler und Momentaufnahmedebugger
ms.topic: conceptual
author: renatosalas
ms.author: regutier
ms.date: 04/14/2020
ms.reviewer: mbullwin
ms.openlocfilehash: 50dcd3f438645c99e0ed3cfdded7a101ee5f1852
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86539855"
---
# <a name="configure-bring-your-own-storage-byos-for-application-insights-profiler-and-snapshot-debugger"></a>Konfigurieren von BYOS (Bring Your Own Storage) für Application Insights Profiler und Momentaufnahmedebugger

## <a name="what-is-bring-your-own-storage-byos-and-why-might-i-need-it"></a>Was ist BYOS (Bring Your Own Storage) und wozu benötige ich es möglicherweise? 
Wenn Sie Application Insights Profiler oder Momentaufnahmedebugger verwenden, werden von Ihrer Anwendung generierte Artefakte über das öffentliche Internet in Azure Storage-Konten hochgeladen. Diese Konten werden zur Verarbeitung und Analyse von Microsoft bezahlt und festgelegt. Microsoft bestimmt die Richtlinien zur Verschlüsselung ruhender Daten und zur Verwaltung der Lebensdauer für diese Artefakte.

Mit Bring Your Own Storage werden diese Artefakte in ein Speicherkonto hochgeladen, das Ihrer Kontrolle unterliegt. Das bedeutet, dass Sie die Richtlinie zur Verschlüsselung ruhender Daten, die Richtlinie zur Verwaltung der Lebensdauer und den Netzwerkzugriff festlegen. Sie sind jedoch auch für die mit diesem Speicherkonto verbundenen Kosten verantwortlich.

> [!NOTE]
> Wenn Sie Private Link aktivieren, ist Bring Your Own Storage eine Voraussetzung. Weitere Informationen zu Private Link für Application Insights finden Sie in der entsprechenden [Dokumentation](../platform/private-link-security.md).
>
> Wenn Sie kundenseitig verwaltete Schlüssel aktivieren, ist Bring Your Own Storage eine Voraussetzung. Weitere Informationen zu kundenseitig verwalteten Schlüsseln für Application Insights finden Sie in der entsprechenden [Dokumentation](../platform/customer-managed-keys.md).

## <a name="how-will-my-storage-account-be-accessed"></a>Wie erfolgt der Zugriff auf mein Speicherkonto?
1. Agents, die in Virtual Machines oder App Service ausgeführt werden, laden Artefakte (Profile, Momentaufnahmen und Symbole) in Blobcontainer in Ihrem Konto hoch. Dabei wird vom Application Insights Profiler- oder Momentaufnahmedebuggerdienst ein SAS-Token (Shared Access Signature) für ein neues Blob in Ihrem Speicherkonto abgerufen.
1. Der Application Insights Profiler- oder Momentaufnahmedebuggerdienst analysiert das eingehende Blob und schreibt die Analyseergebnisse und Protokolldateien in den Blobspeicher zurück. Abhängig von der verfügbaren Computekapazität kann dieser Vorgang jederzeit nach dem Hochladen erfolgen.
1. Wenn Sie die Profiler-Ablaufverfolgungen oder Momentaufnahmedebuggeranalysen aufrufen, ruft der Dienst die Analyseergebnisse aus dem Blobspeicher ab.

## <a name="prerequisites"></a>Voraussetzungen
* Erstellen Sie Ihr Speicherkonto an dem Ort, an dem sich Ihre Application Insights-Ressource befindet. Ex. Wenn sich Ihre Application Insights-Ressource in der Region „USA, Westen 2“ befindet, muss sich Ihr Speicherkonto ebenfalls in der Region „USA, Westen 2“ befinden. 
* Weisen Sie der AAD-Anwendung „Diagnostic Services Trusted Storage Access“ (Diagnosedienste für vertrauenswürdigen Speicherzugriff) in Ihrem Speicherkonto die Rolle „Mitwirkender an Storage-Blobdaten“ über das Benutzeroberflächenelement „Zugriffssteuerung (IAM)“ zu.
* Wenn Private Link aktiviert ist, konfigurieren Sie die zusätzliche Einstellung, um eine Verbindung mit unserem vertrauenswürdigen Microsoft-Dienst über Ihr virtuelles Netzwerk zuzulassen. 

## <a name="how-to-enable-byos"></a>Aktivieren von BYOS

### <a name="create-storage-account"></a>Erstellen eines Speicherkontos
Erstellen Sie an dem Ort, an dem sich Ihre Application Insights-Ressource befindet, ein vollkommen neues Speicherkonto (sofern nicht bereits geschehen).
Wenn sich Ihre Application Insights-Ressource in der Region `West US 2` befindet, muss sich Ihr Speicherkonto ebenfalls in der Region `West US 2` befinden.

### <a name="grant-access-to-diagnostic-services-to-your-storage-account"></a>Gewähren des Zugriffs auf Diagnosedienste für Ihr Speicherkonto
Ein BYOS-Speicherkonto wird mit einer Application Insights Ressource verknüpft. Es darf nur ein Speicherkonto pro Application Insights Ressource vorhanden sein, und das Speicherkonto und die Ressource müssen sich am selben Ort befinden. Sie können ein Speicherkonto für mehrere Application Insights-Ressourcen verwenden.

Zunächst muss dem Application Insights Profiler- und dem Momentaufnahmedebuggerdienst Zugriff auf das Speicherkonto gewährt werden. Fügen Sie zum Gewähren des Zugriffs der AAD-Anwendung mit dem Namen `Diagnostic Services Trusted Storage Access` die Rolle `Storage Blob Data Contributor` über die Seite „Zugriffssteuerung (IAM)“ in Ihrem Speicherkonto wie in Abbildung 1.0 dargestellt zu. 

Schritte: 
1. Klicken Sie im Abschnitt „Rollenzuweisung hinzufügen“ auf die Schaltfläche „Hinzufügen“. 
1. Auswählen der Rolle „Mitwirkender an Storage-Blobdaten“ 
1. Wählen Sie im Abschnitt „Zugriff zuweisen zu“ die Option „Azure AD-Benutzer, -Gruppe oder -Dienstprinzipal“ aus. 
1. Suchen Sie nach der App „Diagnostic Services Trusted Storage Access“ (Diagnosedienste für vertrauenswürdigen Speicherzugriff), und wählen Sie sie aus. 
1. Speichern der Änderungen

_![Abbildung 1.0](media/profiler-bring-your-own-storage/figure-10.png)_
_Abbildung 1.0_ 

Nachdem Sie die Rolle hinzugefügt haben, wird sie wie in Abbildung 1.1 im Abschnitt „Rollenzuweisungen“ angezeigt. 
_![Abbildung 1.1](media/profiler-bring-your-own-storage/figure-11.png)_
_Abbildung 1.1_ 

Wenn Sie auch Private Link verwenden, ist ein zusätzlicher Konfigurationsschritt erforderlich, um eine Verbindung mit unserem vertrauenswürdigen Microsoft-Dienst über Ihr virtuelles Netzwerk zuzulassen. Informationen hierzu finden Sie in der [Dokumentation zur Sicherheit des Speichernetzwerks](../../storage/common/storage-network-security.md#trusted-microsoft-services).

### <a name="link-your-storage-account-with-your-application-insights-resource"></a>Verknüpfen des Speicherkontos mit der Application Insights-Ressource
Es gibt zwei Möglichkeiten, BYOS für die Diagnose auf Codeebene (Profiler/Debugger) zu konfigurieren:

* Verwenden von Azure PowerShell-Cmdlets
* Verwenden der Azure-Befehlszeilenschnittstelle (Command Line Interface, CLI)
* Verwenden von Azure Resource Manager-Vorlagen

#### <a name="configure-using-azure-powershell-cmdlets"></a>Konfigurieren mit Azure PowerShell-Cmdlets

1. Stellen Sie sicher, dass Sie Az PowerShell 4.2.0 installiert haben.

    Informationen zum Installieren von Azure PowerShell finden Sie in der [offiziellen Dokumentation zu Azure PowerShell](/powershell/azure/install-az-ps).

1. Installieren Sie die PowerShell-Erweiterung Application Insights.
    ```powershell
    Install-Module -Name Az.ApplicationInsights -Force
    ```

1. Melden Sie sich mit Ihrem Azure-Konto an.
    ```powershell
    Connect-AzAccount -Subscription "{subscription_id}"
    ```

    Weitere Informationen zur Anmeldung finden Sie in der [Dokumentation zu Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount).

1. Entfernen Sie das vorherige Speicherkonto, das mit Ihrer Application Insights-Ressource verknüpft ist.

    Muster:
    ```powershell
    $appInsights = Get-AzApplicationInsights -ResourceGroupName "{resource_group_name}" -Name "{storage_account_name}"
    Remove-AzApplicationInsightsLinkedStorageAccount -ResourceId $appInsights.Id
    ```

    Beispiel:
    ```powershell
    $appInsights = Get-AzApplicationInsights -ResourceGroupName "byos-test" -Name "byos-test-westus2-ai"
    Remove-AzApplicationInsightsLinkedStorageAccount -ResourceId $appInsights.Id
    ```

1. Verbinden Sie Ihr Speicherkonto mit Ihrer Application Insights-Ressource.
    
    Muster:
    ```powershell
    $storageAccount = Get-AzStorageAccount -ResourceGroupName "{resource_group_name}" -Name "{storage_account_name}"
    $appInsights = Get-AzApplicationInsights -ResourceGroupName "{resource_group_name}" -Name "{application_insights_name}"
    New-AzApplicationInsightsLinkedStorageAccount -ResourceId $appInsights.Id -LinkedStorageAccountResourceId $storageAccount.Id
    ```

    Beispiel:
    ```powershell
    $storageAccount = Get-AzStorageAccount -ResourceGroupName "byos-test" -Name "byosteststoragewestus2"
    $appInsights = Get-AzApplicationInsights -ResourceGroupName "byos-test" -Name "byos-test-westus2-ai"
    New-AzApplicationInsightsLinkedStorageAccount -ResourceId $appInsights.Id -LinkedStorageAccountResourceId $storageAccount.Id
    ```

#### <a name="configure-using-azure-cli"></a>Konfigurieren über die Azure-Befehlszeilenschnittstelle

1. Stellen Sie sicher, dass die Azure-Befehlszeilenschnittstelle installiert ist.

    Informationen zum Installieren der Azure-Befehlszeilenschnittstelle finden Sie in der [offiziellen Dokumentation zur Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli).

1. Installieren Sie die Application Insights-CLI-Erweiterung.
    ```powershell
    az extension add -n application-insights
    ```

1. Verbinden Sie Ihr Speicherkonto mit Ihrer Application Insights-Ressource.

    Muster:
    ```powershell
    az monitor app-insights component linked-storage link --resource-group "{resource_group_name}" --app "{application_insights_name}" --storage-account "{storage_account_name}"
    ```
    
    Beispiel:
    ```powershell
    az monitor app-insights component linked-storage link --resource-group "byos-test" --app "byos-test-westus2-ai" --storage-account "byosteststoragewestus2"
    ```
    
    Erwartete Ausgabe:
    ```powershell
    {
      "id": "/subscriptions/{subscription}/resourcegroups/byos-test/providers/microsoft.insights/components/byos-test-westus2-ai/linkedstorageaccounts/serviceprofiler",
      "linkedStorageAccount": "/subscriptions/{subscription}/resourceGroups/byos-test/providers/Microsoft.Storage/storageAccounts/byosteststoragewestus2",
      "name": "serviceprofiler",
      "resourceGroup": "byos-test",
      "type": "microsoft.insights/components/linkedstorageaccounts"
    }
    ```

    > [!NOTE]
    > Informationen zum Durchführen von Aktualisierungen für die verknüpften Speicherkonten auf die Application Insights-Ressource finden Sie in der [Dokumentation zur Application Insights-CLI](/cli/azure/ext/application-insights/monitor/app-insights/component/linked-storage).

#### <a name="configure-using-azure-resource-manager-template"></a>Konfigurieren mit der Azure Resource Manager-Vorlage

1. Erstellen Sie eine Azure Resource Manager-Vorlagendatei mit folgendem Inhalt (byos.template.json).
    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "applicationinsights_name": {
          "type": "String"
        },
        "storageaccount_name": {
          "type": "String"
        }
      },
      "variables": {},
      "resources": [
        {
          "name": "[concat(parameters('applicationinsights_name'), '/serviceprofiler')]",
          "type": "Microsoft.Insights/components/linkedStorageAccounts",
          "apiVersion": "2020-03-01-preview",
          "properties": {
            "linkedStorageAccount": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageaccount_name'))]"
          }
        }
      ],
      "outputs": {}
    }
    ```

1. Führen Sie den folgenden PowerShell-Befehl aus, um die vorherige Vorlage (verknüpftes Speicherkonto erstellen) bereitzustellen.

    Muster:
    ```powershell
    New-AzResourceGroupDeployment -ResourceGroupName "{your_resource_name}" -TemplateFile "{local_path_to_arm_template}"
    ```

    Beispiel:
    ```powershell
    New-AzResourceGroupDeployment -ResourceGroupName "byos-test" -TemplateFile "D:\Docs\byos.template.json"
    ```

1. Geben Sie die folgenden Parameter an, wenn Sie in der PowerShell-Konsole dazu aufgefordert werden:
    
    |           Parameter           |                                BESCHREIBUNG                               |
    |-------------------------------|--------------------------------------------------------------------------|
    | application_insights_name     | Der Name der Application Insights-Ressource zum Aktivieren von BYOS.            |
    | storage_account_name          | Der Name der Speicherkontoressource, die Sie als BYOS verwenden. |
    
    Erwartete Ausgabe:
    ```powershell
    Supply values for the following parameters:
    (Type !? for Help.)
    application_insights_name: byos-test-westus2-ai
    storage_account_name: byosteststoragewestus2
    
    DeploymentName          : byos.template
    ResourceGroupName       : byos-test
    ProvisioningState       : Succeeded
    Timestamp               : 4/16/2020 1:24:57 AM
    Mode                    : Incremental
    TemplateLink            :
    Parameters              :
                              Name                            Type                       Value
                              ==============================  =========================  ==========
                              application_insights_name        String                     byos-test-westus2-ai
                              storage_account_name             String                     byosteststoragewestus2
                              
    Outputs                 :
    DeploymentDebugLogLevel :
    ```

1. Aktivieren Sie die Diagnose auf Codeebene (Profiler/Debugger) für die entsprechende Workload über das Azure-Portal. (App Service > Application Insights) _![Abbildung 2.0](media/profiler-bring-your-own-storage/figure-20.png)_
_Abbildung 2.0_

## <a name="troubleshooting"></a>Problembehandlung
### <a name="template-schema-schema_uri-isnt-supported"></a>Das Vorlagenschema '{schema_uri}' wird nicht unterstützt.
* Stellen Sie sicher, dass die Eigenschaft `$schema` der Vorlage gültig ist. Sie muss das folgende Muster aufweisen: `https://schema.management.azure.com/schemas/{schema_version}/deploymentTemplate.json#`
* Vergewissern Sie sich, dass sich die `schema_version` der Vorlage innerhalb der gültigen Werte befindet: `2014-04-01-preview, 2015-01-01, 2018-05-01, 2019-04-01, 2019-08-01`.
    Fehlermeldung:
    ```powershell
    New-AzResourceGroupDeployment : 11:53:49 AM - Error: Code=InvalidTemplate; Message=Deployment template validation failed: 'Template schema
    'https://schema.management.azure.com/schemas/2020-01-01/deploymentTemplate.json#' is not supported. Supported versions are
    '2014-04-01-preview,2015-01-01,2018-05-01,2019-04-01,2019-08-01'. Please see https://aka.ms/arm-template for usage details.'.
    ```

### <a name="no-registered-resource-provider-found-for-location-location"></a>Für den Ort '{location}' wurde kein registrierter Ressourcenanbieter gefunden.
* Vergewissern Sie sich, dass die `apiVersion` der Ressource `microsoft.insights/components` den Wert `2015-05-01` aufweist.
* Vergewissern Sie sich, dass die `apiVersion` der Ressource `linkedStorageAccount` den Wert `2020-03-01-preview` aufweist.
    Fehlermeldung:
    ```powershell
    New-AzResourceGroupDeployment : 6:18:03 PM - Resource microsoft.insights/components 'byos-test-westus2-ai' failed with message '{
      "error": {
        "code": "NoRegisteredProviderFound",
        "message": "No registered resource provider found for location 'westus2' and API version '2020-03-01-preview' for type 'components'. The supported api-versions are '2014-04-01,
    2014-08-01, 2014-12-01-preview, 2015-05-01, 2018-05-01-preview'. The supported locations are ', eastus, southcentralus, northeurope, westeurope, southeastasia, westus2, uksouth,
    canadacentral, centralindia, japaneast, australiaeast, koreacentral, francecentral, centralus, eastus2, eastasia, westus, southafricanorth, northcentralus, brazilsouth, switzerlandnorth,
    australiasoutheast'."
      }
    }'
    ```
### <a name="storage-account-location-should-match-ai-component-location"></a>Der Standort des Speicherkontos muss dem Standort der AI-Komponente entsprechen.
* Vergewissern Sie sich, dass der Standort der Application Insights-Ressource mit dem des Speicherkontos identisch ist.
    Fehlermeldung:
    ```powershell
    New-AzResourceGroupDeployment : 1:01:12 PM - Resource microsoft.insights/components/linkedStorageAccounts 'byos-test-centralus-ai/serviceprofiler' failed with message '{
      "error": {
        "code": "BadRequest",
        "message": "Storage account location should match AI component location",
        "innererror": {
          "trace": [
            "System.ArgumentException"
          ]
        }
      }
    }'
    ```

Allgemeine Informationen zur Problembehandlung bei Profiler finden Sie in der [Dokumentation zur Fehlerbehandlung bei Profiler](profiler-troubleshooting.md).

Allgemeine Informationen zur Problembehandlung beim Momentaufnahmedebugger finden Sie in der [Dokumentation zur Fehlerbehandlung beim Momentaufnahmedebugger](snapshot-debugger-troubleshoot.md). 

## <a name="faqs"></a>Häufig gestellte Fragen
* Werden meine Daten zu meinem Speicherkonto migriert, wenn Profiler oder eine Momentaufnahme aktiviert ist und BYOS aktiviert wird?
    _Nein, das ist nicht der Fall._

* Kann BYOS zusammen mit der Verschlüsselung ruhender Daten und einem kundenseitig verwalteten Schlüssel verwendet werden?
    _Ja, um genau zu sein, ist BYOS erforderlich, um Profiler/Debugger mit kundenseitig verwalteten Schlüsseln zu aktivieren._

* Funktioniert BYOS in einer Umgebung, die vom Internet isoliert ist?
    _Ja. BYOS ist eine Voraussetzung für Szenarios mit isoliertem Netzwerk._

* Funktioniert BYOS, wenn sowohl kundenseitig verwaltete Schlüssel als auch Private Link aktiviert sind? 
    _Ja, das ist möglich._

* Kann ich zum Speichern meiner gesammelten Daten wieder die Speicherkonten der Diagnosedienste verwenden, auch wenn BYOS aktiviert ist? 
    _Ja, das ist möglich. Die Datenmigration von BYOS wird derzeit jedoch nicht unterstützt._

* Übernehme ich nach der Aktivierung von BYOS alle damit verbundenen Kosten, d. h. die Kosten für Speicher und Netzwerk? 
    _Ja_
