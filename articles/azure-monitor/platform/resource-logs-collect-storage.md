---
title: Archivieren von Azure-Ressourcenprotokollen in einem Speicherkonto | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihre Azure-Ressourcenprotokolle zur langfristigen Aufbewahrung in einem Speicherkonto archivieren.
author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 12/15/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: 843c179826b2064a1be24d3cee84b398987b4aed
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2020
ms.locfileid: "77658970"
---
# <a name="archive-azure-resource-logs-to-storage-account"></a>Archivieren von Azure-Ressourcenprotokollen in einem Speicherkonto
[Plattformprotokolle](platform-logs-overview.md) in Azure, wie Azure-Aktivitätsprotokolle und Ressourcenprotokolle, liefern detaillierte Diagnose- und Überwachungsinformationen für Azure-Ressourcen und die Azure-Plattform, von der sie abhängen.  In diesem Artikel wird beschrieben, wie Plattformprotokolle in einem Azure-Speicherkonto erfasst werden, um Daten für die Archivierung beizubehalten.

## <a name="prerequisites"></a>Voraussetzungen
Falls Sie noch nicht über ein Konto verfügen, müssen Sie [ein Azure-Speicherkonto erstellen](../../storage/common/storage-account-create.md). Das Speicherkonto muss sich nicht unter demselben Abonnement befinden wie die Ressource, die Protokolle sendet, sofern der Benutzer, der die Einstellung konfiguriert, den entsprechenden RBAC-Zugriff auf beide Abonnements besitzt.


> [!IMPORTANT]
> Azure Data Lake Storage Gen2-Konten werden derzeit nicht als Ziel für Diagnoseeinstellungen unterstützt, auch wenn sie möglicherweise als gültige Option im Azure-Portal aufgeführt werden.


Um den Zugriff auf die Daten besser steuern zu können, sollten Sie kein bereits vorhandenes Speicherkonto mit anderen, nicht überwachungsbezogenen Daten verwenden. Wenn Sie das Aktivitätsprotokoll und Ressourcenprotokollen zusammen archivieren, können Sie aber dasselbe Speicherkonto verwenden, damit sich alle Überwachungsdaten an einem zentralen Ort befinden.

## <a name="create-a-diagnostic-setting"></a>Erstellen einer Diagnoseeinstellung
Senden Sie Plattformprotokolle an den Speicher und andere Ziele, indem Sie eine Diagnoseeinstellung für eine Azure-Ressource erstellen. Weitere Informationen finden Sie unter [Erstellen einer Diagnoseeinstellung zum Erfassen von Protokollen und Metriken in Azure](diagnostic-settings.md).


## <a name="collect-data-from-compute-resources"></a>Erfassen von Daten von Computeressourcen
Mit den Diagnoseeinstellungen werden Ressourcenprotokolle für Azure-Computeressourcen wie alle anderen Ressourcen erfasst, jedoch nicht deren Gastbetriebssystem oder die Workloads. Um diese Daten zu erfassen, installieren Sie den [Windows Azure-Diagnoseagenten](diagnostics-extension-overview.md). 


## <a name="schema-of-platform-logs-in-storage-account"></a>Schema der Plattformprotokolle im Speicherkonto

Nach dem Einrichten der Diagnoseeinstellung wird im Speicherkonto ein Speichercontainer erstellt, sobald ein Ereignis in einer der aktivierten Protokollkategorien auftritt. Die Blobs im Container verwenden die folgende Benennungskonvention:

```
insights-logs-{log category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/RESOURCEGROUPS/{resource group name}/PROVIDERS/{resource provider name}/{resource type}/{resource name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Beispielsweise könnte das Blob für eine Netzwerksicherheitsgruppe einen Namen aufweisen, der dem folgenden Beispiel ähnelt:

```
insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

Jedes Blob vom Typ „PT1H.json“ enthält ein JSON-Blob mit Ereignissen, die innerhalb der in der Blob-URL angegebenen Stunde (Beispiel: h=12) aufgetreten sind. Während der aktuellen Stunde werden Ereignisse an die Datei „PT1H.json“ angefügt, sobald sie auftreten. Der Minutenwert (m=00) ist immer „00“, da Ressourcenprotokollereignisse stundenweise in einzelne Blobs unterteilt werden.

Innerhalb der Datei „PT1H.json“ wird jedes Ereignis im folgenden Format gespeichert. Hierbei wird ein gemeinsames Schema der obersten Ebene verwendet. Es ist aber für jeden Azure-Dienst eindeutig, wie unter [Ressourcenprotokollschema](diagnostic-logs-schema.md) und [Aktivitätsprotokollschema](activity-log-schema.md) beschrieben.

``` JSON
{"time": "2016-07-01T00:00:37.2040000Z","systemId": "46cdbb41-cb9c-4f3d-a5b4-1d458d827ff1","category": "NetworkSecurityGroupRuleCounter","resourceId": "/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/TESTNSG","operationName": "NetworkSecurityGroupCounters","properties": {"vnetResourceGuid": "{12345678-9012-3456-7890-123456789012}","subnetPrefix": "10.3.0.0/24","macAddress": "000123456789","ruleName": "/subscriptions/ s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/default-allow-rdp","direction": "In","type": "allow","matchedConnections": 1988}}
```

> [!NOTE]
> Plattformprotokolle werden mit [JSON Lines](http://jsonlines.org/) in den Blobspeicher geschrieben. Dies bedeutet, dass jedes Ereignis in einer Zeile enthalten ist und mit dem Zeilenumbruchzeichen ein neues Ereignis angegeben wird. Dieses Format wurde im November 2018 implementiert. Vor diesem Datum wurden Protokolle in den Blobspeicher als JSON-Array von Datensätzen geschrieben, wie unter [Vorbereiten auf die Formatänderung für Azure Monitor-Plattformprotokolle, die in einem Speicherkonto archiviert werden](resource-logs-blob-format.md) beschrieben.

## <a name="next-steps"></a>Nächste Schritte

* [Weitere Informationen zu Ressourcenprotokollen](platform-logs-overview.md).
* [Erstellen einer Diagnoseeinstellung zum Erfassen von Protokollen und Metriken in Azure](diagnostic-settings.md).
* [Herunterladen von Blobs für die Analyse](../../storage/blobs/storage-quickstart-blobs-dotnet.md).
* [Archivieren von Azure Active Directory-Protokollen mit Azure Monitor](../../active-directory/reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md).
