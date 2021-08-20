---
title: Referenz zur Überwachung von Azure Container Registry-Daten
description: In diesem Artikel finden Sie wichtiges Referenzmaterial für die Überwachung Ihrer Azure Container Registry-Instanz. Hier finden Sie Details zu Metriken, Ressourcenprotokollen und Protokollschemas.
author: dlepow
ms.author: danlep
ms.topic: reference
ms.custom: subject-monitoring
ms.service: container-registry
ms.date: 03/19/2021
ms.openlocfilehash: 99b9206e305e57898a2eb4bc6d0de7dd6fe94473
ms.sourcegitcommit: 8b7d16fefcf3d024a72119b233733cb3e962d6d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2021
ms.locfileid: "114290415"
---
# <a name="monitoring-azure-container-registry-data-reference"></a>Referenz zur Überwachung von Azure Container Registry-Daten

Unter [Überwachen von Azure Container Registry](monitor-service.md) finden Sie Details zur Erfassung und Analyse von Überwachungsdaten für Azure Container Registry.

## <a name="metrics"></a>Metriken

### <a name="container-registry-metrics"></a>Container Registry-Metriken

Ressourcenanbieter und -typ: [Microsoft.ContainerRegistry/registries](../azure-monitor/essentials/metrics-supported.md#microsoftcontainerregistryregistries)

| Metrik | Über Diagnoseeinstellungen exportierbar? | Einheit | Aggregationstyp | BESCHREIBUNG | Dimensionen  |  
|:-------|:-----|:-----|:------------|:------------------|:----- |
|     AgentPoolCPUTime   | Ja |   Sekunden   | Gesamt |   Von [ACR-Aufgaben](container-registry-tasks-overview.md) verwendete CPU-Zeit, die in dedizierten [Agentpools](tasks-agent-pools.md) ausgeführt werden             | Keine | 
|     RunDuration   | Ja |  Millisekunden   |  Gesamt |  Dauer der Ausführungen von [ACR-Aufgaben](container-registry-tasks-overview.md)       | Keine | 
|     StorageUsed   |  Nein | Byte   |   Average | Von der Containerregistrierung verwendeter Speicher<br/><br/>Zusammenfassung des Speichers für einzelne und freigegebene Ebenen, Manifestdateien und Replikatkopien in allen Repositorys<sup>1</sup>            | Geolocation | 
|     SuccessfulPullCount | Ja  |   Anzahl   | Gesamt | Erfolgreiche Pulls von Containerimages und anderen Artefakten aus der Registrierung              | Keine | 
|     SuccessfulPushCount   | Ja |   Anzahl   | Gesamt | Erfolgreiche Pushes von Containerimages und anderen Artefakten an die Registrierung          | Keine | 
|     TotalPullCount   |   Ja | Anzahl   |     Gesamt |  Gesamtanzahl an Pulls von Containerimages und anderen Artefakten aus der Registrierung         | Keine | 
|     TotalPushCount   | Ja |  Anzahl   |   Gesamt |  Gesamtanzahl an Pushes von Containerimages und anderen Artefakten an die Registrierung           | Keine | 

<sup>1</sup> Aufgrund der gemeinsamen Nutzung der Ebene fällt der verwendete Registrierungsspeicher möglicherweise geringer aus als die Summe des für die einzelnen Repositorys genutzten Speichers. Wenn Sie ein Repository oder ein Tag [löschen](container-registry-delete.md), stellen Sie nur den Speicher wieder her, der von Manifestdateien und den einzelnen Ebenen verwendet wird, auf die verwiesen wird.

Weitere Informationen finden Sie in der Liste [aller in Azure Monitor unterstützten Plattformmetriken](../azure-monitor/essentials/metrics-supported.md).

## <a name="metric-dimensions"></a>Metrikdimensionen

Weitere Informationen zu Metrikdimensionen finden Sie unter [Mehrdimensionale Metriken](../azure-monitor/essentials/data-platform-metrics.md#multi-dimensional-metrics).

Den Metriken von Azure Container Registry sind die folgenden Dimensionen zugeordnet.

| Dimensionsname | BESCHREIBUNG |
| ------------------- | ----------------- |
| **Geolocation** | Die Azure-Region für eine Registrierung oder [Georeplikation](container-registry-geo-replication.md) |


## <a name="resource-logs"></a>Ressourcenprotokolle

In diesem Abschnitt werden die Typen der Ressourcenprotokolle aufgeführt, die für Azure Container Registry erfasst werden können. 

Eine Referenz finden Sie in der Liste [aller Typen von Ressourcenprotokollkategorien, die in Azure Monitor unterstützt werden](../azure-monitor/essentials/resource-logs-schema.md).

### <a name="container-registries"></a>Containerregistrierungen

Ressourcenanbieter und -typ: [Microsoft.ContainerRegistry/registries](../azure-monitor/essentials/resource-logs-categories.md#microsoftcontainerregistryregistries)

| Kategorie | Anzeigename | Details  |
|:---------|:-------------|------------------|
| ContainerRegistryLoginEvents  | Anmeldeereignisse | Hier werden Authentifizierungsereignisse und der zugehörige Status für Registrierungen gespeichert, z. B. eingehende Identität und IP-Adresse. |
| ContainerRegistryRepositoryEvents | Repositoryereignisse           | Vorgänge für Images und andere Artefakte in Registrierungsrepositorys<br/><br/> Die folgenden Vorgänge werden protokolliert: Push, Pull, Tag aufheben, löschen (einschließlich Repository löschen), Tag endgültig löschen und Manifest endgültig löschen<sup>1</sup>. |

<sup>1</sup> Ereignisse zum endgültigen Löschen werden nur protokolliert, wenn eine [Aufbewahrungsrichtlinie](container-registry-retention-policy.md) für eine Registrierung konfiguriert ist.

## <a name="azure-monitor-logs-tables"></a>Tabellen in Azure Monitor-Protokollen

Dieser Abschnitt bezieht sich auf alle Kusto-Tabellen in Azure Monitor-Protokollen, die für Azure Container Registry relevant und für die Abfrage durch Log Analytics verfügbar sind. 

### <a name="container-registry"></a>Container Registry

| Tabelle |  Beschreibung | 
|:---------|:-------------|
| [AzureActivity](/azure/azure-monitor/reference/tables/azureactivity)   | Einträge aus dem Azure-Aktivitätsprotokoll, das Erkenntnisse zu Ereignissen auf Abonnement- oder Verwaltungsgruppenebene bietet, die in Azure aufgetreten sind | 
| [AzureMetrics](/azure/azure-monitor/reference/tables/azuremetrics) | Von Azure-Diensten ausgegebene Metrikdaten zur Messung von Integrität und Leistung.    |  
|  [ContainerRegistryLoginEvents](/azure/azure-monitor/reference/tables/containerregistryloginevents)               | Überwachungsprotokolle für Anmeldungen in Azure Container Registry                             |                                                     
|  [ContainerRegistryRepositoryEvents](/azure/azure-monitor/reference/tables/containerregistryrepositoryevents)               | Überwachungsprotokolle für Repositorys in Azure Container Registry                         |                                                    

Eine Referenz zu allen Azure Monitor-Protokollen und Log Analytics-Tabellen finden Sie in der [Referenz zu Tabellen in Azure Monitor-Protokollen](/azure/azure-monitor/reference/tables/tables-resourcetype).

## <a name="activity-log"></a>Aktivitätsprotokoll

In der folgenden Tabelle finden Sie die Vorgänge in Zusammenhang mit Azure Container Registry, die im [Aktivitätsprotokoll](../azure-monitor/essentials/activity-log.md) erstellt werden können. Diese Liste ist nicht vollständig.

| Vorgang | BESCHREIBUNG |
|:---|:---|
| Containerregistrierung erstellen oder aktualisieren | Mit diesem Vorgang wird eine Containerregistrierung erstellt oder aktualisiert. |
| Containerregistrierung löschen | Mit diesem Vorgang wird eine Containerregistrierung gelöscht. |
| Anmeldeinformationen für Containerregistrierung auflisten | Mit diesem Vorgang werden die Anmeldeinformationen für das Administratorkonto der Registrierung angezeigt. |
| Abbild importieren | Mit diesem Vorgang wird ein Image oder ein anderes Artefakt in eine Registrierung importiert. |
| Rollenzuweisung erstellen | Bei diesem Vorgang wird einer Identität eine RBAC-Rolle zugewiesen, um auf eine Ressource zuzugreifen.  |


## <a name="schemas"></a>Schemas

Die folgenden Schemas werden von den Ressourcenprotokollen in Azure Container Registry verwendet.

| Schema | BESCHREIBUNG |
|:--- |:---|
| [ContainerRegistryLoginEvents](/azure/azure-monitor/reference/tables/ContainerRegistryLoginEvents)  | Schema für Authentifizierungsereignisse und den zugehörigen Status, z. B. eingehende Identität und IP-Adresse. |
| [ContainerRegistryRepositoryEvents](/azure/azure-monitor/reference/tables/ContainerRegistryRepositoryEvents) | Schema für Vorgänge für Images und andere Artefakte in Registrierungsrepositorys |
## <a name="next-steps"></a>Nächste Schritte

- Unter [Überwachen von Azure Container Registry](monitor-service.md) finden Sie eine Beschreibung der Überwachung einer Azure Container Registry-Instanz.
- Ausführliche Informationen zur Überwachung von Azure-Ressourcen finden Sie unter [Überwachen von Azure-Ressourcen mit Azure Monitor](../azure-monitor/overview.md).