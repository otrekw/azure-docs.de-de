---
title: Überwachen von Linux-Clusterereignissen in Azure Service Fabric
description: Erfahren Sie, wie Sie Linux-Clusterereignisse in Service Fabric überwachen, indem Sie Service Fabric-Plattformereignisse in Syslog schreiben.
author: srrengar
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: srrengar
ms.openlocfilehash: fe31c6fdca3651bfe56e798b30d50c9f047c680b
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2020
ms.locfileid: "86258639"
---
# <a name="service-fabric-linux-cluster-events-in-syslog"></a>Service Fabric-Linux-Clusterereignisse in Syslog

Service Fabric stellt eine Reihe von Plattformereignissen zur Verfügung, um Sie über wichtige Aktivitäten in Ihrem Cluster zu informieren. Die vollständige Liste der Ereignisse, die bereitgestellt werden, finden Sie [hier](service-fabric-diagnostics-event-generation-operational.md). Es gibt verschiedene Möglichkeiten, diese Ereignisse zu nutzen. In diesem Artikel werden wir erläutern, wie Sie Service Fabric so konfigurieren, dass diese Ereignisse in Syslog geschrieben werden.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="introduction"></a>Einführung

In Release 6.4 wurde SyslogConsumer eingeführt, um die Ereignisse der Service Fabric-Plattform für Linux-Cluster an Syslog zu senden. Nach der Aktivierung werden die Ereignisse automatisch an Syslog weitergeleitet, die vom Log Analytics-Agent erfasst und gesendet werden können.

Jedes Syslog-Ereignis besteht aus 4 Komponenten
* Facility
* Identity
* `Message`
* severity

SyslogConsumer schreibt alle Plattformereignisse mithilfe von Facility `Local0`. Sie können ein Update auf jede gültige Facility ausführen, indem Sie die Konfiguration ändern. Die verwendete Identität ist `ServiceFabric`. Das Feld „Message“ enthält das gesamte Ereignis (im JSON-Format serialisiert), sodass es abgefragt und von einer Vielzahl von Tools genutzt werden kann. 

## <a name="enable-syslogconsumer"></a>Aktivieren von SyslogConsumer

Um SyslogConsumer zu aktivieren, müssen Sie ein Upgrade Ihres Clusters ausführen. Der Abschnitt `fabricSettings` muss mit dem folgenden Code aktualisiert werden. Beachten Sie, dass dieser Code nur Abschnitte umfasst, die im Zusammenhang mit SyslogConsumer stehen.

```json
    "fabricSettings": [
        {
            "name": "Diagnostics",
            "parameters": [
            {
                "name": "ConsumerInstances",
                "value": "AzureWinFabCsv, AzureWinFabCrashDump, AzureTableWinFabEtwQueryable, SyslogConsumer"
            }
            ]
        },
        {
            "name": "SyslogConsumer",
            "parameters": [
            {
                "name": "ProducerInstance",
                "value": "WinFabLttProducer"
            },
            {
            "name": "ConsumerType",
            "value": "SyslogConsumer"
            },
            {
                "name": "IsEnabled",
                "value": "true"
            }
            ]
        },
        {
            "name": "Common",
            "parameters": [
            {
                "name": "LinuxStructuredTracesEnabled",
                "value": "true"
            }
            ]
        }
    ],
```

Die folgenden Änderungen sind zu beachten:
1. Im Abschnitt „Common“ gibt es einen neuen Parameter namens `LinuxStructuredTracesEnabled`. **Dieser ist erforderlich, damit Linux-Ereignisse strukturiert und serialisiert vorliegen, wenn sie an Syslog gesendet werden.**
2. Im Abschnitt „Diagnostics“ gibt es eine neue ConsumerInstance: SyslogConsumer wurde hinzugefügt. Dadurch wird die Plattform informiert, dass ein anderer Consumer der Ereignisse vorhanden ist. 
3. Der neue Abschnitt „SyslogConsumer“ muss `IsEnabled` mit dem Wert `true` enthalten. Die Konfiguration sieht vor, dass die Local0-Facility automatisch verwendet wird. Sie können dies außer Kraft setzen, indem Sie einen anderen Parameter hinzufügen.

```json
    {
        "name": "New LogFacility",
        "value": "<Valid Syslog Facility>"
    }
```

## <a name="azure-monitor-logs-integration"></a>Integration von Azure Monitor-Protokollen
Sie können diese Syslog-Ereignisse in einem Überwachungstool wie Azure Monitor-Protokolle lesen. Sie können einen Log Analytics-Arbeitsbereich erstellen, indem Sie Azure Marketplace mit den folgenden [instructions].(../azure-monitor/learn/quick-create-workspace.md) verwenden. Außerdem müssen Sie Ihrem Cluster den Log Analytics-Agent hinzufügen, um diese Daten zu erfassen und an den Arbeitsbereich zu senden. Dies ist der gleiche Agent, der verwendet wird, um Leistungsindikatoren zu erfassen. 

1. Navigieren Sie zum Blatt `Advanced Settings`.

    ![Arbeitsbereichseinstellungen](media/service-fabric-diagnostics-oms-syslog/workspace-settings.png)

2. Klicken Sie auf `Data`.
3. Klicken Sie auf `Syslog`.
4. Konfigurieren Sie Local0 als die nachzuverfolgende Facility. Sie können eine andere Facility hinzufügen, wenn Sie diese in fabricSettings geändert haben.

    ![Konfigurieren von Syslog](media/service-fabric-diagnostics-oms-syslog/syslog-configure.png)
5. Wechseln Sie zum Abfrage-Explorer, indem Sie im Menü der Arbeitsbereichsressource auf `Logs` klicken, um die Abfrage zu starten.

    ![Arbeitsbereichsprotokolle](media/service-fabric-diagnostics-oms-syslog/workspace-logs.png)
6. Sie können die Tabelle `Syslog` abfragen, um nach `ServiceFabric` als Prozessname zu suchen. Die folgende Abfrage ist ein Beispiel dafür, wie Sie den JSON-Code im Ereignis analysieren und seinen Inhalt anzeigen können.

```kusto
    Syslog | where ProcessName == "ServiceFabric" | extend $payload = parse_json(SyslogMessage) | project $payload
```

![Syslog-Abfrage](media/service-fabric-diagnostics-oms-syslog/syslog-query.png)

Das Beispiel oben ist ein NodeDown-Ereignis. Die vollständige Liste der Ereignisse können Sie [hier](service-fabric-diagnostics-event-generation-operational.md) anzeigen.

## <a name="next-steps"></a>Nächste Schritte
* [Stellen Sie den Log Analytics-Agent auf Ihren Knoten bereit](service-fabric-diagnostics-oms-agent.md), um Leistungsindikatoren zu erfassen und Docker-Statistiken und -Protokolle für Ihre Container zu sammeln.
* Machen Sie sich mit den Funktionen zur [Protokollsuche und -abfrage](../azure-monitor/log-query/log-query-overview.md) in Azure Monitor-Protokolle vertraut.
* [Erstellen benutzerdefinierter Ansichten in Azure Monitor-Protokollen mithilfe des Ansicht-Designers](../azure-monitor/platform/view-designer.md)
* Referenz zur Vorgehensweise bei der [Integration von Azure Monitor-Protokollen in Syslog](../azure-monitor/platform/data-sources-syslog.md).
