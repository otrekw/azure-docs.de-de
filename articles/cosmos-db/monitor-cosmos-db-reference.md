---
title: Überwachen von Daten in Azure Cosmos DB – Referenz | Microsoft-Dokumentation
description: Protokoll- und Metrikenreferenz für die Überwachung von Daten in Azure Cosmos DB
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 11/11/2019
ms.author: bwren
ms.custom: subject-monitoring
ms.subservice: logs
ms.openlocfilehash: d243224192b5761af45d387690f5fb41b84481e6
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77588721"
---
# <a name="azure-cosmos-db-monitoring-data-reference"></a>Überwachen von Daten in Azure Cosmos DB – Referenz
Dieser Artikel enthält eine Referenz von Protokoll- und Metrikdaten, die gesammelt werden, um die Leistung und Verfügbarkeit von Azure Cosmos DB zu analysieren. Ausführliche Informationen zum Sammeln und Analysieren von Überwachungsdaten für Azure Cosmos DB finden Sie unter [Überwachen von Cosmos DB](monitor-cosmos-db.md).


## <a name="resource-logs"></a>Ressourcenprotokolle
In der nachstehenden Tabelle sind die Eigenschaften für Azure Cosmos DB-Ressourcenprotokolle aufgelistet, wenn sie in Azure Monitor-Protokollen oder Azure Storage gesammelt werden. In Azure Monitor-Protokolle werden sie in der Tabelle **AzureDiagnostics** gesammelt. Der Wert für **ResourceProvider** lautet *MICROSOFT.DOCUMENTDB*. 

| Azure Storage-Feld oder -Eigenschaft | Eigenschaft von Azure Monitor-Protokolle | BESCHREIBUNG |
| --- | --- | --- |
| **time** | **TimeGenerated** | Datum und Uhrzeit (UTC), zu denen der Vorgang aufgetreten ist. |
| **Ressourcen-ID** | **Ressource** | Das Azure Cosmos DB-Konto, für das Protokolle aktiviert sind.|
| **category** | **Kategorie** | Die verfügbaren Protokolltypen bei Azure Cosmos DB-Protokollen sind: **DataPlaneRequests**, **MongoRequests**, **QueryRuntimeStatistics**, **PartitionKeyStatistics**, **PartitionKeyRUConsumption** und **ControlPlaneRequests**. |
| **operationName** | **OperationName** | Name des Vorgangs. Bei diesem Wert kann es sich um einen der folgenden Vorgänge handeln: Create, Update, Read, ReadFeed, Delete, Replace, Execute, SqlQuery, Query, JSQuery, Head, HeadFeed, Upsert.   |
| **properties** | – | Die Inhalte dieser Felder werden in den folgenden Zeilen beschrieben. |
| **activityId** | **activityId_g** | Die eindeutige GUID für den protokollierten Vorgang. |
| **userAgent** | **userAgent_s** | Eine Zeichenfolge, die den Benutzer-Agent des Clients angibt, der die Anforderung ausführt. Das Format lautet {Name des Benutzer-Agents}/{version}.|
| **requestResourceType** | **requestResourceType_s** | Der Typ der Ressource, auf die zugegriffen wird. Bei diesem Wert kann es sich um einen der folgenden Ressourcentypen handeln: Database, Container, Document, Attachment, User, Permission, StoredProcedure, Trigger, UserDefinedFunction, Offer. |
| **statusCode** | **statusCode_s** | Der Antwortstatus des Vorgangs. |
| **requestResourceId** | **ResourceId** | Die Ressourcen-ID der Anforderung. Der Wert kann je nach ausgeführtem Vorgang auf databaseRid, collectionRid oder documentRid zeigen.|
| **clientIpAddress** | **clientIpAddress_s** | Die IP-Adresse des Clients. |
| **requestCharge** | **requestCharge_s** | Die Anzahl der vom Vorgang verwendeten RUs. |
| **collectionRid** | **collectionId_s** | Die eindeutige ID für die Sammlung.|
| **duration** | **duration_s** | Die Dauer des Vorgangs in Millisekunden. |
| **requestLength** | **requestLength_s** | Die Länge der Anforderung in Bytes. |
| **responseLength** | **responseLength_s** | Die Länge der Antwort in Bytes.|
| **resourceTokenUserRid** | **resourceTokenUserRid_s** | Dieser Wert ist nicht leer, wenn [Ressourcentoken](https://docs.microsoft.com/azure/cosmos-db/secure-access-to-data#resource-tokens) für die Authentifizierung verwendet werden. Der Wert verweist auf die Ressourcen-ID des Benutzers. |

Eine Liste aller Azure Monitor-Protokollkategorien und Links zu zugeordneten Schemas finden Sie unter [Azure Monitor Logs categories and schemas](../azure-monitor/platform/diagnostic-logs-schema.md) (Kategorien und Schemas in Azure Monitor-Protokolle). 

## <a name="metrics"></a>Metriken
In der nachstehenden Tabelle werden die für Azure Cosmos DB gesammelten Plattformmetriken aufgelistet. Alle Metriken werden im Namespace **Cosmos DB-Standardmetriken** gespeichert.

Eine Liste aller Azure Monitor-Unterstützungsmetriken (einschließlich Cosmos DB) finden Sie unter [Unterstützte Metriken von Azure Monitor](../azure-monitor/platform/metrics-supported.md). 

#### <a name="request-metrics"></a>Anforderungsmetriken
            
|Metrik (Metrikanzeigename)|Einheit (Aggregationstyp) |BESCHREIBUNG|Dimensionen| Zeitgranularitäten| Zuordnung von Legacymetriken | Verwendung |
|---|---|---|---| ---| ---| ---|
| TotalRequests (Anforderungen insgesamt) | Count (Anzahl) | Anzahl von gesendeten Anforderungen| DatabaseName, CollectionName, Region, StatusCode| All | TotalRequests, Http 2xx, Http 3xx, Http 400 "," Http 401, Interner Serverfehler, Dienst nicht verfügbar, Anforderungen gedrosselt, Anforderungen pro Sekunde | Wird verwendet, um Anforderungen nach Statuscode zu überwachen, Container mit einer Granularität von einer Minute. Um einen Durchschnittswert für die Anforderungen pro Sekunde zu erhalten, verwenden Sie die Zähl-Aggregation in einer Minute, und teilen Sie sie durch 60. |
| MetadataRequests (Metadatenanforderungen) |Count (Anzahl) | Anzahl der Metadatenanforderungen. Azure Cosmos DB unterhält einen Container mit Systemmetadaten für jedes Konto, wodurch Sie Sammlungen, Datenbanken usw. und deren Konfigurationen ohne anfallende Kosten auflisten können. | DatabaseName, CollectionName, Region, StatusCode| All| |Wird verwendet, um die Drosselung aufgrund von Metadatenanforderungen zu überwachen.|
| MongoRequests (Mongo-Anforderungen) | Count (Anzahl) | Anzahl der ausgegebenen Mongo-Anforderungen | DatabaseName, CollectionName, Region, CommandName, ErrorCode| All |Mongo-Abfrage-Anforderungsrate, Mongo-Aktualisieren-Anforderungsrate, Mongo-Löschen-Anforderungsrate, Mongo-Einfügen-Anforderungsrate, Mongo-Zählen-Anforderungsrate| Wird verwendet, um Mongo-Anforderungsfehler zu überwachen, Verwendungen pro Befehlstyp. |

#### <a name="request-unit-metrics"></a>Metriken für Anforderungseinheiten

|Metrik (Metrikanzeigename)|Einheit (Aggregationstyp)|BESCHREIBUNG|Dimensionen| Zeitgranularitäten| Zuordnung von Legacymetriken | Verwendung |
|---|---|---|---| ---| ---| ---|
| MongoRequestCharge (Kosten der Mongo-Anforderung) | Count (Gesamt) |Verbrauchte Mongo-Anforderungseinheiten| DatabaseName, CollectionName, Region, CommandName, ErrorCode| All |Mongo-Abfrage-Anforderungsgebühr, Mongo-Aktualisieren-Anforderungsgebühr, Mongo-Löschen-Anforderungsgebühr, Mongo-Einfügen-Anforderungsgebühr, Mongo-Zählen-Anforderungsgebühr| Wird verwendet, um die Mongo-Ressourcen-RUs in einer Minute zu überwachen.|
| TotalRequestUnits (Anforderungseinheiten gesamt)| Count (Gesamt) | Verbrauchte Anforderungseinheiten| DatabaseName, CollectionName, Region, StatusCode |All| TotalRequestUnits| Wird verwendet, um die gesamte RU-Nutzung mit einer Granularität von einer Minute zu überwachen. Um einen Durchschnittswert für die verbrauchten RU zu erhalten, verwenden Sie die Gesamtaggregation in einer Minute, und teilen Sie sie durch 60.|
| ProvisionedThroughput (Bereitgestellter Durchsatz)| Count (Maximum) |Bereitgestellter Durchsatz bei Containergranularität| DatabaseName, ContainerName| 5M| | Wird verwendet, um den bereitgestellten Durchsatz pro Container zu überwachen.|

#### <a name="storage-metrics"></a>Speichermetrik

|Metrik (Metrikanzeigename)|Einheit (Aggregationstyp)|BESCHREIBUNG|Dimensionen| Zeitgranularitäten| Zuordnung von Legacymetriken | Verwendung |
|---|---|---|---| ---| ---| ---|
| AvailableStorage (Verfügbarer Speicher) |Bytes (Gesamt) | Gemeldeter Gesamtspeicher mit 5-Minuten-Granularität pro Region| DatabaseName, CollectionName, Region| 5M| Verfügbarer Speicher| Wird verwendet, um die verfügbare Speicherkapazität zu überwachen (gilt nur für feste Speichersammlungen). Die Mindestgranularität sollte 5 Minuten betragen.| 
| DataUsage (Datennutzung) |Bytes (Gesamt) |Gemeldete Gesamtdatennutzung mit 5-Minuten-Granularität pro Region| DatabaseName, CollectionName, Region| 5M |Datengröße | Wird verwendet, um den gesamten Datenverbrauch für Container und Region zu überwachen. Die Mindestgranularität sollte 5 Minuten betragen.|
| IndexUsage (Indexnutzung) | Bytes (Gesamt) |Gemeldete Gesamtindexnutzung mit 5-Minuten-Granularität pro Region| DatabaseName, CollectionName, Region| 5M| Indexgröße| Wird verwendet, um den gesamten Datenverbrauch für Container und Region zu überwachen. Die Mindestgranularität sollte 5 Minuten betragen. |
| DocumentQuota (Dokumentenkontingent) | Bytes (Gesamt) | Gemeldetes Gesamtspeicherkontingent mit 5-Minuten-Granularität pro Region| DatabaseName, CollectionName, Region| 5M |Speicherkapazität| Wird verwendet, um das gesamte Kontingent für Container und Region zu überwachen. Die Mindestgranularität sollte 5 Minuten betragen.|
| DocumentCount (Dokumentanzahl) | Count (Gesamt) |Gemeldete Gesamtdokumentanzahl mit 5-Minuten-Granularität pro Region| DatabaseName, CollectionName, Region| 5M |Dokumentanzahl|Wird verwendet, um das Gesamtanzahl der Dokumente für Container und Region zu überwachen. Die Mindestgranularität sollte 5 Minuten betragen.|

#### <a name="latency-metrics"></a>Latenzmetriken

|Metrik (Metrikanzeigename)|Einheit (Aggregationstyp)|BESCHREIBUNG|Dimensionen| Zeitgranularitäten| Verwendung |
|---|---|---|---| ---| ---|
| ReplicationLatency (Replikationswartezeit)| MilliSeconds (Minimum, Maximum, Durchschnitt) | P99-Replikationswartezeit für Quell- und Zielregionen für geofähiges Konto| SourceRegion, TargetRegion| All | Wird verwendet, um die P99-Replikationswartezeit zwischen zwei beliebigen Regionen für ein georepliziertes Konto zu überwachen. |
| Serverseitige Latenz| Millisekunden (Durchschnitt) | Die Zeit, die der Server für die Verarbeitung der Anforderung benötigt. | CollectionName, ConnectionMode, DatabaseName, OperationType, PublicAPIType, Region | All | Wird verwendet, um die Wartezeit der Anforderungen auf dem Azure Cosmos DB-Server zu überwachen. |



#### <a name="availability-metrics"></a>Verfügbarkeitsmetriken

|Metrik (Metrikanzeigename) |Einheit (Aggregationstyp)|BESCHREIBUNG| Zeitgranularitäten| Zuordnung von Legacymetriken | Verwendung |
|---|---|---|---| ---| ---|
| ServiceAvailability (Dienstverfügbarkeit)| Prozent (Minimum, Maximum) | Verfügbarkeit der Kontoanforderungen mit einer Granularität von einer Stunde| 1H | Dienstverfügbarkeit | Stellt den Prozentsatz der insgesamt übergebenen Anforderungen dar. Eine Anforderung gilt als fehlgeschlagen aufgrund eines Systemfehlers, wenn der Statuscode 410, 500 oder 503 lautet. Wird verwendet, um die Verfügbarkeit des Kontos bei einer Granularität von einer Stunde zu überwachen. |


#### <a name="cassandra-api-metrics"></a>Cassandra-API-Metriken

|Metrik (Metrikanzeigename)|Einheit (Aggregationstyp)|BESCHREIBUNG|Dimensionen| Zeitgranularitäten| Verwendung |
|---|---|---|---| ---| ---|
| CassandraRequests (Cassandra-Anforderungen) | Count (Anzahl) | Anzahl der ausgeführten Cassandra-API-Anforderungen| DatabaseName, CollectionName, ErrorCode, Region, OperationType, ResourceType| All| Wird verwendet, um Cassandra-Anforderungen mit einer Granularität von einer Minute zu überwachen. Um einen Durchschnittswert für die Anforderungen pro Sekunde zu erhalten, verwenden Sie die Zähl-Aggregation in einer Minute, und teilen Sie sie durch 60.|
| CassandraRequestCharges (Gebühren für Cassandra-Anforderungen) | Count (Summe, Minimum, Maximum, Durchschnitt) | Cassandra-API-Anforderungen verbrauchte Anforderungseinheiten| DatabaseName, CollectionName, Region, OperationType, ResourceType| All| Wird verwendet, um die RUs zu überwachen, die pro Minute von einem Cassandra API-Konto genutzt werden.|
| CassandraConnectionClosures (Abschluss von Cassandra-Verbindungen) |Count (Anzahl) |Anzahl der geschlossenen Cassandra-Verbindungen| ClosureReason, Region| All | Wird verwendet, um die Verbindung zwischen Clients und der Cassandra-API von Azure Cosmos DB zu überwachen.|

## <a name="see-also"></a>Weitere Informationen

- Eine Beschreibung der Überwachung von Azure Cosmos DB finden Sie unter [Überwachen von Azure Cosmos DB](monitor-cosmos-db.md).
- Ausführliche Informationen zur Überwachung von Azure-Ressourcen finden Sie unter [Überwachen von Azure-Ressourcen mit Azure Monitor](../azure-monitor/insights/monitor-azure-resource.md).
