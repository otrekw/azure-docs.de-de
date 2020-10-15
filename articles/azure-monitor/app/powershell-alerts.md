---
title: Einrichten von Warnungen in Application Insights mithilfe von PowerShell | Microsoft Docs
description: Automatisieren Sie die Konfiguration von Application Insights so, dass Sie bei Metrikänderungen E-Mails erhalten.
ms.topic: conceptual
ms.date: 07/23/2016
ms.openlocfilehash: 74d477b6660c0f7ec2ee32b34169bb85886936e5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87322464"
---
# <a name="use-powershell-to-set-alerts-in-application-insights"></a>Einrichten von Warnungen in Application Insights mithilfe von PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Sie können die Konfiguration von [Warnungen](../platform/alerts-log.md) in [Application Insights](./app-insights-overview.md) automatisieren.

Darüber hinaus können Sie [Webhooks zum Automatisieren Ihrer Reaktion auf eine Warnung festlegen](../platform/alerts-webhooks.md).

> [!NOTE]
> Wenn Sie gleichzeitig Ressourcen und Warnungen erstellen möchten, sollten Sie [eine Azure Resource Manager-Vorlage](powershell.md) verwenden.

## <a name="one-time-setup"></a>Einmalige Konfiguration
Wenn Sie PowerShell noch nicht mit Ihrem Azure-Abonnement verwendet haben:

Installieren Sie das Azure-PowerShell-Modul auf dem Computer, auf dem die Skripts ausgeführt werden sollen.

* Installieren Sie [Microsoft-Webplattform-Installer (Version 5 oder höher)](https://www.microsoft.com/web/downloads/platform.aspx).
* Installieren Sie hiermit Microsoft Azure PowerShell.

## <a name="connect-to-azure"></a>Herstellen einer Verbindung mit Azure
Starten Sie Azure PowerShell, und [stellen Sie eine Verbindung mit Ihrem Abonnement her](/powershell/azure/):

```azurepowershell
Add-AzAccount
```


## <a name="get-alerts"></a>Abrufen von Warnungen

```azurepowershell
Get-AzAlertRule -ResourceGroup "Fabrikam" `
  [-Name "My rule"] `
  [-DetailedOutput]
```

## <a name="add-alert"></a>Warnung hinzufügen

```azurepowershell
Add-AzMetricAlertRule -Name "{ALERT NAME}" `
  -Description "{TEXT}" `
  -ResourceGroup "{GROUP NAME}" `
  -TargetResourceId "/subscriptions/{SUBSCRIPTION ID}/resourcegroups/{GROUP NAME}/providers/microsoft.insights/components/{APP RESOURCE NAME}" `
  -MetricName "{METRIC NAME}" `
  -Operator GreaterThan `
  -Threshold {NUMBER}  `
  -WindowSize {HH:MM:SS} `
  [-SendEmailToServiceOwners] `
  [-CustomEmails "EMAIL1@X.COM","EMAIL2@Y.COM"] `
  -Location "East US" // must be East US at present `
  -RuleType Metric
```


## <a name="example-1"></a>Beispiel 1
E-Mail-Nachricht senden, wenn die Antwort des Servers auf HTTP-Anforderungen, gemittelt über 5 Minuten, langsamer als 1 Sekunde ist. Der Name meiner Application Insights-Ressource lautet IceCreamWebApp, und sie befindet sich in der Ressourcengruppe "Fabrikam". Ich bin Besitzer des Azure-Abonnements.

Die GUID ist die Abonnement-ID (nicht der Instrumentierungsschlüssel der Anwendung).

```azurepowershell
Add-AzMetricAlertRule -Name "slow responses" `
  -ResourceGroup "Fabrikam" `
  -TargetResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
  -MetricName "request.duration" `
  -Operator GreaterThan `
  -Threshold 1 `
  -WindowSize 00:05:00 `
  -SendEmailToServiceOwners `
  -Location "East US" `
  -RuleType Metric
```

## <a name="example-2"></a>Beispiel 2
Ich habe eine Anwendung, in der ich mit [TrackMetric()](./api-custom-events-metrics.md#trackmetric) eine Metrik namens „salesPerHour“ melde. Eine E-Mail an meine Kollegen senden, wenn "salesPerHour" gemittelt über 24 Stunden unter 100 fällt.

```azurepowershell
Add-AzMetricAlertRule -Name "poor sales" `
  -Description "slow sales alert" `
  -ResourceGroup "Fabrikam" `
  -TargetResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
  -MetricName "salesPerHour" `
  -Operator LessThan `
  -Threshold 100 `
  -WindowSize 24:00:00 `
  -CustomEmails "satish@fabrikam.com","lei@fabrikam.com" `
  -Location "East US" `
  -RuleType Metric
```

Dieselbe Regel kann für die Metrik verwendet werden, die mit dem [Messparameter](./api-custom-events-metrics.md#properties) eines anderen Trackingaufrufs gemeldet wird, z.B. „TrackEvent“ oder „trackPageView“.

## <a name="metric-names"></a>Metriknamen
| Metrikname | Anzeigename | BESCHREIBUNG |
| --- | --- | --- |
| `basicExceptionBrowser.count` |Browserausnahmen |Anzahl nicht erfasster Ausnahmen, die im Browser ausgelöst wurden. |
| `basicExceptionServer.count` |Serverausnahmen |Anzahl der nicht behandelten Ausnahmen, die von der Anwendung ausgelöst wurden |
| `clientPerformance.clientProcess.value` |Clientverarbeitungszeit |Zeit zwischen dem Empfang des letzten Byte eines Dokuments und dem Laden des DOM. Asynchrone Anforderungen werden möglicherweise immer noch verarbeitet. |
| `clientPerformance.networkConnection.value` |Netzwerkverbindungszeit zum Laden der Seite |Zeit, die der Browser für die Verbindung mit dem Netzwerk benötigt. Kann bei Zwischenspeicherung 0 sein. |
| `clientPerformance.receiveRequest.value` |Empfängt Antwortzeit |Zeit zwischen dem Senden der Anforderung durch den Browser und dem Empfangen der Antwort. |
| `clientPerformance.sendRequest.value` |Anforderungszeit senden |Vom Browser benötigte Zeit zum Senden der Anforderung. |
| `clientPerformance.total.value` |Browser-Seitenladezeit |Zeit ab der Benutzeranforderung, bis DOM, Stylesheets, Skripts und Bilder geladen werden. |
| `performanceCounter.available_bytes.value` |Verfügbarer Arbeitsspeicher |Physischer Arbeitsspeicher, der sofort für einen Prozess oder für die Nutzung durch das System verfügbar ist. |
| `performanceCounter.io_data_bytes_per_sec.value` |E/A-Rate für Prozess |Gesamtanzahl von pro Sekunde in Dateien, im Netzwerk und auf Geräten gelesenen und geschriebenen Bytes. |
| `performanceCounter.number_of_exceps_thrown_per_sec.value` |Ausnahmerate |Pro Sekunde ausgelöste Ausnahmen. |
| `performanceCounter.percentage_processor_time.value` |Prozess-CPU |Der Prozentsatz der verstrichenen Zeit für alle Prozessthreads, die vom Prozessor zur Ausführung von Anweisungen für den Anwendungsprozess verwendet wird. |
| `performanceCounter.percentage_processor_total.value` |Prozessorzeit |Der Prozentsatz der Zeit, die der Prozessor nicht im Leerlauf in Threads verbringt. |
| `performanceCounter.process_private_bytes.value` |Private Bytes für Prozess |Speicher, der exklusiv den überwachten Anwendungsprozessen zugewiesen ist. |
| `performanceCounter.request_execution_time.value` |Ausführungsdauer für ASP.NET-Anforderungen |Ausführungszeit der aktuellen Anforderung. |
| `performanceCounter.requests_in_application_queue.value` |ASP.NET-Anforderungen in Ausführungswarteschlange |Länge der Anwendungsanforderungswarteschleife. |
| `performanceCounter.requests_per_sec.value` |ASP.NET-Anforderungsrate |Rate aller Anforderungen an die Anwendung pro Sekunde von ASP.NET. |
| `remoteDependencyFailed.durationMetric.count` |Abhängigkeitsfehler |Anzahl der Aufrufe mit Fehlern von der Serveranwendung an externe Ressourcen. |
| `request.duration` |Serverantwortzeit |Zeit zwischen dem Empfang einer HTTP-Anforderung und dem Abschluss des Sendevorgangs der Antwort. |
| `request.rate` |Anforderungsrate |Rate aller Anforderungen an die Anwendung pro Sekunde. |
| `requestFailed.count` |Anforderungsfehler |Anzahl von HTTP-Anforderungen, die als Ergebnis Antwortcode >= 400 hatten. |
| `view.count` |Seitenaufrufe |Anzahl der Clientbenutzeranforderungen für eine Webseite. Synthetischer Datenverkehr wird herausgefiltert. |
| {benutzerdefinierter Metrikname} |{Ihr Metrikname} |Der von [TrackMetric](./api-custom-events-metrics.md#trackmetric) oder im [Messparameter eines Trackingaufrufs](./api-custom-events-metrics.md#properties) gemeldete Metrikwert. |

Die Metriken werden von verschiedenen Telemetriemodulen gesendet:

| Metrikgruppe | Erfassungsmodul |
| --- | --- |
| basicExceptionBrowser,<br/>clientPerformance,<br/>Ansicht |[Browser JavaScript](./javascript.md) |
| performanceCounter |[Leistung](./configuration-with-applicationinsights-config.md) |
| remoteDependencyFailed |[Abhängigkeit](./configuration-with-applicationinsights-config.md) |
| request,<br/>requestFailed |[Serveranfrage](./configuration-with-applicationinsights-config.md) |

## <a name="webhooks"></a>webhooks
Sie können [Ihre Reaktion auf eine Warnung automatisieren](../platform/alerts-webhooks.md). Azure ruft eine Webadresse Ihrer Wahl auf, wenn eine Warnung ausgelöst wird.

## <a name="see-also"></a>Weitere Informationen
* [Skript zum Konfigurieren von Application Insights](./create-new-resource.md#creating-a-resource-automatically)
* [Erstellen von Application Insights- und Webtestressourcen aus Vorlagen](powershell.md)
* [Automatisieren der Kopplung der Microsoft Azure-Diagnose mit Application Insights](powershell-azure-diagnostics.md)
* [Automatisieren Ihrer Reaktion auf eine Warnung](../platform/alerts-webhooks.md)

