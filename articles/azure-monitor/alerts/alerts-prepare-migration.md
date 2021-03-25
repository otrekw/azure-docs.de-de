---
title: Aktualisieren von Logik-Apps und Runbooks für die Migration von Warnungen
description: Erfahren Sie, wie Sie Ihre Webhooks, Logik-Apps und Runbooks zur Vorbereitung der freiwilligen Migration ändern.
author: yanivlavi
ms.author: yalavi
ms.topic: conceptual
ms.date: 02/14/2021
ms.openlocfilehash: ce61c3539a4ea29cbeb48c379ed143363500701e
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102038015"
---
# <a name="prepare-your-logic-apps-and-runbooks-for-migration-of-classic-alert-rules"></a>Vorbereiten Ihrer Logik-Apps und Runbooks für die Migration von klassischen Warnungsregeln

> [!NOTE]
> Wie [zuvor angekündigt](monitoring-classic-retirement.md) werden klassische Warnungen in Azure Monitor für Benutzer der öffentlichen Cloud eingestellt. Bis zum **31. Mai 2021** ist die Verwendung noch eingeschränkt möglich. Klassische Warnungen für Azure Government Cloud und Azure China 21ViaNet werden am **29. Februar 2024** eingestellt.
>

Wenn Sie sich entscheiden, Ihre klassischen Warnungsregeln zu neuen Warnungsregeln zu migrieren, beachten Sie, dass zwischen den beiden Systemen einige Unterschiede bestehen. In diesem Artikel werden die Unterschiede erläutert, und Sie erfahren, wie Sie sich auf die Änderung vorbereiten können.

## <a name="api-changes"></a>API-Änderungen

Die APIs zum Erstellen und Verwalten klassischer Warnungsregeln (`microsoft.insights/alertrules`) unterscheiden sich von den APIs zum Erstellen und Verwalten der neuen Metrikwarnungen (`microsoft.insights/metricalerts`). Wenn Sie klassische Warnungsregeln derzeit programmgesteuert erstellen und verwalten, aktualisieren Sie Ihre Bereitstellungsskripts, damit sie mit den neuen APIs funktionieren.

In der folgenden Tabelle sind befehlsorientierte Benutzerschnittstellen für klassische und neue Warnungen aufgeführt:

| Typ des Bereitstellungsskripts | Klassische Warnungen | Neue Metrikwarnungen |
| ---------------------- | -------------- | ----------------- |
|REST-API     | [microsoft.insights/alertrules](/rest/api/monitor/alertrules)         | [microsoft.insights/metricalerts](/rest/api/monitor/metricalerts)       |
|Azure-Befehlszeilenschnittstelle     | [az monitor alert](/cli/azure/monitor/alert)        | [az monitor metrics alert](/cli/azure/monitor/metrics/alert)        |
|PowerShell      | [Referenz](/powershell/module/az.monitor/add-azmetricalertrule)       |  [Referenz](/powershell/module/az.monitor/add-azmetricalertrulev2)    |
| Azure Resource Manager-Vorlage | [Für klassische Warnungen](./alerts-enable-template.md)|[Für neue Metrikwarnungen](./alerts-metric-create-templates.md)|

## <a name="notification-payload-changes"></a>Änderungen der Benachrichtigungsnutzlast

Das Format der Benachrichtigungsnutzlast für [klassische Warnungsregeln](alerts-webhooks.md) und [neue Metrikwarnungen](alerts-metric-near-real-time.md#payload-schema) unterscheidet sich geringfügig. Wenn Sie über klassische Warnungsregeln mit Webhook-, Logik-App- oder Runbookaktionen verfügen, müssen Sie die Ziele so aktualisieren, dass sie das neue Nutzdatenformat akzeptieren.

Mithilfe der folgenden Tabelle können Sie die Felder für die Webhooknutzlast im klassischen Format dem neuen Format zuordnen:

| Typ des Benachrichtigungsendpunkts | Klassische Warnungen | Neue Metrikwarnungen |
| -------------------------- | -------------- | ----------------- |
|Wurde die Warnung aktiviert oder behoben?    | **status**       | **data.status** |
|Kontextinformationen zur Warnung     | **context**        | **data.context**        |
|Zeitstempel der Aktivierung oder Auflösung der Warnung     | **context.timestamp**       | **data.context.timestamp**        |
| Warnungsregel-ID | **context.id** | **data.context.id** |
| Name der Warnungsregel | **context.name** | **data.context.name** |
| Beschreibung der Warnungsregel | **context.description** | **data.context.description** |
| Warnungsregelbedingung | **context.condition** | **data.context.condition** |
| Metrikname | **context.condition.metricName** | **data.context.condition.allOf[0].metricName** |
| Zeitaggregation (wie die Metrik im Auswertungsfenster aggregiert wird)| **context.condition.timeAggregation** | **context.condition.timeAggregation** |
| Auswertungszeitraum | **context.condition.windowSize** | **data.context.condition.windowSize** |
| Operator (Vergleich des aggregierten Metrikwerts mit dem Schwellenwert) | **context.condition.operator** | **data.context.condition.operator** |
| Schwellenwert | **context.condition.threshold** | **data.context.condition.allOf[0].threshold** |
| Metrikwert | **context.condition.metricValue** | **data.context.condition.allOf[0].metricValue** |
| Abonnement-ID | **context.subscriptionId** | **data.context.subscriptionId** |
| Ressourcengruppe der betroffenen Ressource | **context.resourceGroup** | **data.context.resourceGroup** |
| Name der betroffenen Ressource | **context.resourceName** | **data.context.resourceName** |
| Typ der betroffenen Ressource | **context.resourceType** | **data.context.resourceType** |
| Ressourcen-ID der betroffenen Ressource | **context.resourceId** | **data.context.resourceId** |
| Direkter Link zur Ressourcenzusammenfassungsseite des Portals | **context.portalLink** | **data.context.portalLink** |
| Benutzerdefinierte Nutzlastfelder, die an den Webhook oder die Logik-App übergeben werden sollen | **properties** | **data.properties** |

Wie Sie sehen, sind die Nutzlasten ähnlich. Der nächste Abschnitt enthält Folgendes:

- Ausführliche Informationen zum Ändern von Logik-Apps zur Verwendung mit dem neuen Format
- Ein Runbook-Beispiel, in dem die Benachrichtigungsnutzlast für neue Warnungen analysiert wird

## <a name="modify-a-logic-app-to-receive-a-metric-alert-notification"></a>Ändern einer Logik-App zum Empfangen einer Benachrichtigung über eine Metrikwarnung

Wenn Sie Logik-Apps mit klassischen Warnungen verwenden, müssen Sie den Logik-App-Code ändern, damit die neue Metrikwarnungsnutzlast analysiert wird. Folgen Sie diesen Schritten:

1. Erstellen Sie eine neue Logik-App.

1. Verwenden Sie die Vorlage „Azure Monitor – Metrics Alert Handler“ (Azure Monitor – Metrikwarnungshandler). Diese Vorlage enthält einen Auslöser **HTTP-Anforderung**, für den das entsprechende Schema definiert ist.

    ![Screenshot: Zwei Schaltflächen: „Leere Logik-App“ und „Azure Monitor – Metrics Alert Handler“ (Azure Monitor – Metrikwarnungshandler)](media/alerts-prepare-migration/logic-app-template.png "Vorlage für Metrikwarnung")

1. Fügen Sie eine Aktion zum Hosten Ihrer Verarbeitungslogik hinzu.

## <a name="use-an-automation-runbook-that-receives-a-metric-alert-notification"></a>Verwenden eines Automation-Runbooks, das eine Benachrichtigung über eine Metrikwarnung empfängt

Das folgende Beispiel enthält PowerShell-Code zur Verwendung in Ihrem Runbook. Mit diesem Code können die Nutzlasten sowohl für klassische Metrikwarnungsregeln als auch neue Metrikwarnungsregeln analysiert werden.

```PowerShell
## Example PowerShell code to use in a runbook to handle parsing of both classic and new metric alerts.

[OutputType("PSAzureOperationResponse")]

param
(
    [Parameter (Mandatory=$false)]
    [object] $WebhookData
)

$ErrorActionPreference = "stop"

if ($WebhookData)
{
    # Get the data object from WebhookData.
    $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Determine whether the alert triggering the runbook is a classic metric alert or a new metric alert (depends on the payload schema).
    $schemaId = $WebhookBody.schemaId
    Write-Verbose "schemaId: $schemaId" -Verbose
    if ($schemaId -eq "AzureMonitorMetricAlert") {

        # This is the new metric alert schema.
        $AlertContext = [object] ($WebhookBody.data).context
        $status = ($WebhookBody.data).status

        # Parse fields related to alert rule condition.
        $metricName = $AlertContext.condition.allOf[0].metricName
        $metricValue = $AlertContext.condition.allOf[0].metricValue
        $threshold = $AlertContext.condition.allOf[0].threshold
        $timeAggregation = $AlertContext.condition.allOf[0].timeAggregation
    }
    elseif ($schemaId -eq $null) {
        # This is the classic metric alert schema.
        $AlertContext = [object] $WebhookBody.context
        $status = $WebhookBody.status

        # Parse fields related to alert rule condition.
        $metricName = $AlertContext.condition.metricName
        $metricValue = $AlertContext.condition.metricValue
        $threshold = $AlertContext.condition.threshold
        $timeAggregation = $AlertContext.condition.timeAggregation
    }
    else {
        # The schema is neither a classic metric alert nor a new metric alert.
        Write-Error "The alert data schema - $schemaId - is not supported."
    }

    # Parse fields related to resource affected.
    $ResourceName = $AlertContext.resourceName
    $ResourceType = $AlertContext.resourceType
    $ResourceGroupName = $AlertContext.resourceGroupName
    $ResourceId = $AlertContext.resourceId
    $SubId = $AlertContext.subscriptionId

    ## Your logic to handle the alert here.
}
else {
    # Error
    Write-Error "This runbook is meant to be started from an Azure alert webhook only."
}

```

Ein vollständiges Beispiel für ein Runbook, das bei Auslösung einer Warnung einen virtuellen Computer beendet, finden Sie in der [Dokumentation zu Azure Automation](../../automation/automation-create-alert-triggered-runbook.md).

## <a name="partner-integration-via-webhooks"></a>Partnerintegration über Webhooks

Die meisten [unserer Partner, die integrierte Lösungen für klassische Warnungen anbieten](../partners.md), unterstützen neuere Metrikwarnungen bereits über ihre Integrationen. Bekannte Integrationen, die bereits mit neuen Metrikwarnungen funktionieren:

- [PagerDuty](https://www.pagerduty.com/docs/guides/azure-integration-guide/)
- [OpsGenie](https://docs.opsgenie.com/docs/microsoft-azure-integration)
- [Signl4](https://www.signl4.com/blog/mobile-alert-notifications-azure-monitor/)

Wenn Sie eine Partnerintegration verwenden, die hier nicht aufgeführt ist, erkundigen Sie sich beim Anbieter, ob diese mit neuen Metrikwarnungen funktioniert.

## <a name="next-steps"></a>Nächste Schritte

- [How to use the migration tool](alerts-using-migration-tool.md) (Verwenden des Migrationstools)
- [Funktionsweise des Migrationstools](alerts-understand-migration.md)