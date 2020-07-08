---
title: 'Azure Service Bus: Automatisches Aktualisieren von Messagingeinheiten'
description: In diesem Artikel erfahren Sie, wie Sie Messagingeinheiten eines Service Bus-Namespace mithilfe eines Azure Automation-Runbooks automatisch aktualisieren können.
ms.topic: how-to
ms.date: 06/23/2020
ms.openlocfilehash: 52f5b13b482739bfa56ff606f684fd5a9c7d3b6e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85341496"
---
# <a name="automatically-update-messaging-units-of-an-azure-service-bus-namespace"></a>Automatisches Aktualisieren von Messagingeinheiten eines Azure Service Bus-Namespace 
In diesem Artikel erfahren Sie, wie Sie [Messagingeinheiten](service-bus-premium-messaging.md) eines Service Bus-Namespace auf der Grundlage der Ressourcenauslastung (CPU oder Arbeitsspeicher) automatisch aktualisieren können. 

Im Beispiel dieses Artikels wird gezeigt, wie Sie Messagingeinheiten für einen Service Bus-Namespace erhöhen, wenn die CPU-Auslastung des Namespace 75 Prozent übersteigt. Dazu werden die folgenden allgemeinen Schritte ausgeführt:

1. Erstellen eines Azure Automation-Runbooks mit PowerShell-Skript zum Hochskalieren (Erhöhen) der Messagingeinheiten für einen Service Bus-Namespace 
2. Erstellen einer CPU-Auslastungswarnung für den Service Bus-Namespace, durch die das PowerShell-Skript aufgerufen wird, wenn die CPU-Auslastung des Namespace 75 Prozent übersteigt 

> [!IMPORTANT]
> Dieser Artikel gilt nur für den **Premium-Tarif** von Azure Service Bus. 


## <a name="create-a-service-bus-namespace"></a>Erstellen eines Service Bus-Namespace
Erstellen Sie einen Service Bus-Namespace im Premium-Tarif. Führen Sie die unter [Erstellen eines Namespace im Azure-Portal](service-bus-quickstart-portal.md#create-a-namespace-in-the-azure-portal) beschriebenen Schritte aus, um den Namespace zu erstellen. 

## <a name="create-an-azure-automation-account"></a>Erstellen eines Azure Automation-Kontos
Erstellen Sie ein Azure Automation-Konto. Eine entsprechende Anleitung finden Sie im Artikel [Erstellen eines Azure Automation-Kontos](../automation/automation-quickstart-create-account.md). 

## <a name="import-azservice-module-from-gallery"></a>Importieren des Moduls „Az.Service“ aus dem Katalog
Importieren Sie die Module `Az.Accounts` und `Az.ServiceBus` aus dem Katalog in das Automation-Konto. Das Modul `Az.ServiceBus` ist vom Modul `Az.Accounts` abhängig und muss daher zuerst installiert werden. 

Eine ausführliche Anleitung finden Sie unter [Importieren eines Moduls aus dem Modulkatalog im Azure-Portal](../automation/automation-runbook-gallery.md#import-a-module-from-the-module-gallery-with-the-azure-portal).

## <a name="create-and-publish-a-powershell-runbook"></a>Erstellen und Veröffentlichen eines PowerShell-Runbooks

1. Erstellen Sie ein PowerShell-Runbook gemäß der Anleitung im Artikel [Erstellen eines Azure Automation-Runbooks](../automation/automation-quickstart-create-runbook.md). 

    Weiter unten finden Sie ein exemplarisches PowerShell-Skript zum Erhöhen von Messagingeinheiten für einen Service Bus-Namespace. Durch dieses PowerShell-Skript in einem Automation-Runbook werden Messagingeinheiten von 1 auf 2, von 2 auf 4 oder von 4 auf 8 erhöht. Zulässige Werte für diese Eigenschaft: 1, 2, 4 und 8. Sie können ein weiteres Runbook erstellen, um die Messagingeinheiten zu verringern.

    Die Parameter **namespaceName** und **resourceGroupName** werden verwendet, um das Skript unabhängig vom Warnungsszenario zu testen. 
    
    Der Parameter **WebHookData** wird für die Warnung verwendet, um Informationen wie Ressourcengruppenname, Ressourcenname und Ähnliches zur Laufzeit zu übergeben. 

    ```powershell
    [OutputType("PSAzureOperationResponse")]
    param
    (
        [Parameter (Mandatory=$false)]
        [object] $WebhookData,
    
        [Parameter (Mandatory = $false)]
        [String] $namespaceName,
    
        [Parameter (Mandatory = $false)]
        [String] $resourceGroupName
    )
    
    
    if ($WebhookData)
    {
        # Get the data object from WebhookData
        $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)
    
        # Get the alert schema ID
        $schemaId = $WebhookBody.schemaId

        # If it's a metric alert
        if ($schemaId -eq "AzureMonitorMetricAlert") {

            # Get the resource group name from the alert context
            $resourceGroupName = $WebhookBody.resourceGroupName
            
            # Get the namespace name from the alert context
            $namespaceName = $WebhookBody.resourceName
        }
    }
    
    # Connect to Azure account
    $connection = Get-AutomationConnection -Name AzureRunAsConnection
    
    while(!($connectionResult) -And ($logonAttempt -le 10))
    {
        $LogonAttempt++
        # Logging in to Azure...
        $connectionResult =    Connect-AzAccount `
                                    -ServicePrincipal `
                                    -Tenant $connection.TenantID `
                                    -ApplicationId $connection.ApplicationID `
                                    -CertificateThumbprint $connection.CertificateThumbprint
    
        Start-Sleep -Seconds 30
    }
    
    # Get the current capacity (number of messaging units) of the namespace
    $sbusns=Get-AzServiceBusNamespace `
        -Name $namespaceName `
        -ResourceGroupName $resourceGroupName
    
    $currentCapacity = $sbusns.Sku.Capacity
    
    # Increase the capacity
    # Capacity can be one of these values: 1, 2, 4, 8
    if ($currentCapacity -eq 1) {
        $newMU = 2
    }
    elseif ($currentCapacity -eq 2) {
        $newMU = 4
    }
    elseif ($currentCapacity -eq 4) {
        $newMU = 8    
    }
    else {
    
    }
    
    # Update the capacity of the namespace
    Set-AzServiceBusNamespace `
            -Location eastus `
            -SkuName Premium `
            -Name $namespaceName `
            -SkuCapacity $newMU `
            -ResourceGroupName $resourceGroupName

    ```
2. [Testen Sie die Arbeitsmappe](../automation/manage-runbooks.md#test-a-runbook), indem Sie Werte für die Parameter **namespaceName** and **resourceGroupName** angeben. Vergewissern Sie sich, dass die Messagingeinheiten für den Namespace aktualisiert werden. 
3. War der Test erfolgreich, können Sie die [Arbeitsmappe veröffentlichen](..//automation/manage-runbooks.md#publish-a-runbook), damit sie später als Aktion für eine namespacebezogene Warnung hinzugefügt werden kann. 

## <a name="create-an-alert-on-the-namespace-to-trigger-the-runbook"></a>Erstellen einer Warnung für den Namespace, um das Runbook auszulösen
Im Artikel [Verwenden einer Warnung zum Auslösen eines Azure Automation-Runbooks](../automation/automation-create-alert-triggered-runbook.md) erfahren Sie, wie Sie eine Warnung für Ihren Service Bus-Namespace konfigurieren, um das von Ihnen erstellte Automation-Runbook auszulösen. So können Sie beispielsweise eine Warnung für die Metrik **CPU-Nutzung pro Namespace** oder **Arbeitsspeichernutzung nach Namespace** erstellen und eine Aktion hinzufügen, um das von Ihnen erstellte Automation-Runbook auszulösen. Ausführliche Informationen zu diesen Metriken finden Sie unter [Metriken zur Ressourcennutzung](service-bus-metrics-azure-monitor.md#resource-usage-metrics).

Im Anschluss erfahren Sie, wie Sie eine Warnung erstellen, durch die das Automation-Runbook ausgelöst wird, wenn die **CPU-Auslastung** des Namespace **75 Prozent** übersteigt.

1. Wählen Sie auf der Seite **Service Bus-Namespace** für Ihren Namespace im Menü auf der linken Seite die Option **Warnungen** und anschließend auf der Symbolleiste die Option **+ Neue Warnungsregel** aus. 
    
    ![Seite „Warnungen“: Schaltfläche „Neue Warnungsregel“](./media/automate-update-messaging-units/alerts-page.png)
2. Klicken Sie auf der Seite **Warnungsregel erstellen** auf **Bedingung auswählen**. 

    ![Seite „Warnungsregel erstellen“: Option „Bedingung auswählen“](./media/automate-update-messaging-units/alert-rule-select-condition.png) 
3. Wählen Sie auf der Seite **Signallogik konfigurieren** die Option **CPU** als Signal aus. 

    ![Seite „Signallogik konfigurieren“: Auswählen von „CPU“](./media/automate-update-messaging-units/configure-signal-logic.png)
4. Geben Sie einen **Schwellenwert** (in diesem Beispiel **75 %** ) ein, und wählen Sie **Fertig** aus. 

    ![Konfigurieren des CPU-Signals](./media/automate-update-messaging-units/cpu-signal-configuration.png)
5. Klicken Sie auf der Seite **Warnung erstellen** auf **Aktionsgruppe auswählen**.
    
    ![Auswählen einer Aktionsgruppe](./media/automate-update-messaging-units/select-action-group-button.png)
6. Wählen Sie auf der Symbolleiste die Schaltfläche **Aktionsgruppe erstellen** aus. 

    ![Schaltfläche „Aktionsgruppe erstellen“](./media/automate-update-messaging-units/create-action-group-button.png)
7. Führen Sie auf der Seite **Aktionsgruppe hinzufügen** die folgenden Schritte aus:
    1. Geben Sie einen **Namen** für die Aktionsgruppe ein. 
    2. Geben Sie einen **Kurznamen** für die Aktionsgruppe ein.
    3. Wählen Sie das **Abonnement** aus, in dem Sie diese Aktionsgruppe erstellen möchten.
    4. Wählen Sie die **Ressourcengruppe**. 
    5. Geben Sie im Abschnitt **Aktionen** einen **Namen für die Aktion** ein, und wählen Sie unter **Aktionstyp** die Option **Automation-Runbook** aus. 

        ![Seite „Aktionsgruppe hinzufügen“](./media/automate-update-messaging-units/add-action-group-page.png)
8. Führen Sie auf der Seite **Runbook konfigurieren** die folgenden Schritte aus:
    1. Wählen Sie unter **Runbook-Quelle** die Option **Benutzer** aus. 
    2. Wählen Sie unter **Abonnement** das **Azure-Abonnement** mit dem Automation-Konto aus. 
    3. Wählen Sie unter **Automation-Konto** Ihr **Automation-Konto** aus.
    4. Wählen Sie unter **Runbook** Ihr Runbook aus. 
    5. Wählen Sie auf der Seite **Runbook konfigurieren** die Option **OK** aus. 
        ![Seite „Runbook konfigurieren“](./media/automate-update-messaging-units/configure-runbook.png)
9. Wählen Sie im Bereich **Aktionsgruppe hinzufügen** die Option **OK** aus. 
5. Geben Sie als Nächstes auf der Seite **Warnungsregel erstellen** einen **Namen für die Regel** ein, und wählen Sie anschließend **Warnungsregel erstellen** aus. 
    ![Seite „Warnungsregel erstellen“](./media/automate-update-messaging-units/create-alert-rule.png)

    > [!NOTE]
    > Übersteigt nun die CPU-Auslastung des Namespace 75 Prozent, wird durch die Warnung das Automation-Runbook ausgelöst, wodurch wiederum die Messagingeinheiten des Service Bus-Namespace erhöht werden. Analog dazu können Sie eine Warnung für ein anderes Automation-Runbook erstellen, um die Messagingeinheiten zu verringern, wenn die CPU-Auslastung des Namespace unter 25 Prozent sinkt. 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Messagingeinheiten finden Sie unter [Service Bus Premium- und Standard-Preisstufe für Messaging](service-bus-premium-messaging.md).
