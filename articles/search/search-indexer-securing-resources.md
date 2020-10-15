---
title: Sicheres Zugreifen auf Indexerressourcen
titleSuffix: Azure Cognitive Search
description: Konzeptionelle Übersicht über die Sicherheitsoptionen auf Netzwerkebene für den Azure-Datenzugriff durch Indexer in Azure Cognitive Search.
manager: nitinme
author: arv100kri
ms.author: arjagann
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/07/2020
ms.openlocfilehash: 85446847e8ad77bc83eea657ab17268839e0b231
ms.sourcegitcommit: a2d8acc1b0bf4fba90bfed9241b299dc35753ee6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2020
ms.locfileid: "91949818"
---
# <a name="indexer-access-to-data-sources-using-azure-network-security-features"></a>Indexerzugriff auf Datenquellen mit Azure-Netzwerksicherheitsfeatures

Azure Cognitive Search-Indexer können während der Ausführung ausgehende Aufrufe an verschiedene Azure-Ressourcen richten. In diesem Artikel werden die Konzepte hinter dem Indexerzugriff auf Ressourcen erläutert, wenn diese Ressourcen durch IP-Firewalls, private Endpunkte und andere Sicherheitsmechanismen auf Netzwerkebene geschützt werden. Die möglichen Ressourcentypen, auf die ein Indexer bei normaler Ausführung zugreifen könnte, sind in der folgenden Tabelle aufgeführt.

| Resource | Zweck innerhalb der Indexerausführung |
| --- | --- |
| Azure Storage (Blobs, Tabellen, ADLS Gen 2) | Datenquelle |
| Azure Storage (Blobs, Tabellen) | Skillsets (Zwischenspeichern von angereicherten Dokumenten und Speichern von Wissensspeicherprojektionen) |
| Azure Cosmos DB (verschiedene APIs) | Datenquelle |
| Azure SQL-Datenbank | Datenquelle |
| SQL Server auf Azure-VMs (IaaS) | Datenquelle |
| Verwaltete SQL-Instanzen | Datenquelle |
| Azure-Funktionen | Host für „Benutzerdefinierte Web-API“-Qualifikationen |
| Cognitive Services | Dem Skillset angefügt, das zum Abrechnen der Anreicherung über das Limit der 20 kostenlosen Dokumente hinaus verwendet wird |

> [!NOTE]
> Die einem Skillset angefügte Cognitive Service-Ressource wird für die Abrechnung auf Basis der im Suchindex ausgeführten und dort hinein geschriebenen Anreicherungen verwendet. Sie wird nicht für den Zugriff auf die Cognitive Services-APIs verwendet. Der Zugriff eines Indexers aus der Anreicherungspipeline auf Cognitive Services-APIs erfolgt über einen sicheren Kommunikationskanal, bei dem Daten während der Übertragung stark verschlüsselt und nie im Ruhezustand gespeichert werden.

Kunden können diese Ressourcen über verschiedene von Azure angebotene Netzwerkisolations-Mechanismen sichern. Mit Ausnahme der Cognitive Service-Ressource haben Indexer auch dann eingeschränkten Zugriff auf alle anderen Ressourcen, wenn sie wie in der folgenden Tabelle aufgeführt netzwerkisoliert sind.

| Resource | IP-Einschränkung | Privater Endpunkt |
| --- | --- | ---- |
| Azure Storage (Blobs, Tabellen, ADLS Gen 2) | Wird nur unterstützt, wenn sich das Speicherkonto und der Suchdienst in unterschiedlichen Regionen befinden | Unterstützt |
| Azure Cosmos DB – SQL-API | Unterstützt | Unterstützt |
| Azure Cosmos DB – Cassandra-, Mongo- und Gremlin-API | Unterstützt | Nicht unterstützt |
| Azure SQL-Datenbank | Unterstützt | Unterstützt |
| SQL Server auf Azure-VMs (IaaS) | Unterstützt | – |
| Verwaltete SQL-Instanzen | Unterstützt | – |
| Azure-Funktionen | Unterstützt | Nur für bestimmte SKUs von Azure-Funktionen unterstützt |

> [!NOTE]
> Zusätzlich zu den oben aufgeführten Optionen können Kunden für netzwerkgesicherte Azure-Speicherkonten die Tatsache nutzen, dass Azure Cognitive Search ein [vertrauenswürdiger Microsoft-Dienst](../storage/common/storage-network-security.md#trusted-microsoft-services) ist. Dies bedeutet, dass ein bestimmter Suchdienst das virtuelle Netzwerk oder IP-Einschränkungen für das Speicherkonto umgehen und auf Daten im Speicherkonto zugreifen kann, wenn die entsprechende rollenbasierte Zugriffssteuerung für das Speicherkonto aktiviert ist. Ausführliche Informationen finden Sie unter [Zugriff auf Daten in Speicherkonten über „Vertrauenswürdiger Dienst“-Ausnahme](search-indexer-howto-access-trusted-service-exception.md). Diese Option kann anstelle der IP-Einschränkungsroute verwendet werden, wenn das Speicherkonto oder der Suchdienst nicht in eine andere Region verschoben werden kann.

Beachten Sie bei der Auswahl des sicheren Zugriffsmechanismus, den ein Indexer verwenden sollte, die folgenden Einschränkungen:

- [Dienstendpunkte](../virtual-network/virtual-network-service-endpoints-overview.md) werden für keine Azure-Ressource unterstützt.
- Ein Suchdienst kann nicht in einem bestimmten virtuellen Netzwerk bereitgestellt werden – diese Funktion bietet Azure Cognitive Search nicht.
- Wenn Indexer (ausgehende) private Endpunkte für den Zugriff auf Ressourcen verwenden, fallen möglicherweise zusätzliche [Private Link-Gebühren](https://azure.microsoft.com/pricing/details/search/) an.

## <a name="indexer-execution-environment"></a>Indexerausführungsumgebung

Azure Cognitive Search-Indexer können effizient Inhalt aus Datenquellen extrahieren, dem extrahierten Inhalt Anreicherungen hinzufügen und optional Projektionen erzeugen, bevor die Ergebnisse in den Suchindex geschrieben werden. Abhängig von der Anzahl der Zuständigkeiten, die einem Indexer zugewiesen sind, kann er in einer von zwei Umgebungen ausgeführt werden:

- Eine Umgebung, die für einen bestimmten Suchdienst privat ist. In solchen Umgebungen ausgeführte Indexer teilen Ressourcen mit anderen Workloads (z. B. anderen vom Kunden initiierten Indizierungen oder Abfragen der Arbeitsauslastung). In der Regel werden nur Indexer, die nicht viele Ressourcen benötigen (z. B. kein Skillset verwenden), in dieser Umgebung ausgeführt.
- Eine mehrinstanzenfähige Umgebung, die ressourcenintensive Indexer hostet, z. B. solche mit einem Skillset. Ihrerseits ressourcenintensive Ressourcen werden in dieser Umgebung ausgeführt, um eine optimale Leistung zu erzielen und gleichzeitig sicherzustellen, dass die Suchdienstressourcen für andere Workloads verfügbar sind. Diese mehrinstanzenfähige Umgebung wird von Azure Cognitive Search ohne zusätzliche Kosten für den Kunden verwaltet und gesichert.

Für eine bestimmte Indexerausführung bestimmt Azure Cognitive Search die beste Umgebung.

## <a name="granting-access-to-indexer-ip-ranges"></a>Gewähren von Zugriff auf die Indexer-IP-Adressbereiche

Wenn die Ressource, auf die Ihr Indexer zugreifen möchte, auf eine bestimmte Gruppe von IP-Adressbereichen beschränkt ist, müssen Sie die Gruppe so erweitern, dass sie die möglichen IP-Adressbereiche einschließt, aus denen eine Indexeranforderung kommen kann. Wie bereits erwähnt, gibt es zwei mögliche Umgebungen, in denen Indexer ausgeführt werden und aus denen Zugriffsanforderungen kommen können. Sie müssen die IP-Adressen __beider__ Umgebungen hinzufügen, damit der Indexerzugriff funktioniert.

- Zum Abrufen der IP-Adresse der jeweiligen privaten Umgebung des Suchdiensts führen Sie ein `nslookup` (oder `ping`) für den vollqualifizierten Domänennamen (FQDN) des Suchdiensts durch. Der FQDN eines Suchdiensts in der öffentlichen Cloud wäre z. B. `<service-name>.search.windows.net`. Diese Informationen finden Sie im Azure-Portal.
- Die IP-Adressen der mehrinstanzenfähigen Umgebungen sind über das `AzureCognitiveSearch`-Diensttag verfügbar. Für [Azure-Diensttags](../virtual-network/service-tags-overview.md) ist für jeden Dienst ein veröffentlichter Bereich von IP-Adressen verfügbar. Sie sind über [Verwendung der Diensttagermittlungs-API (öffentliche Vorschau)](../virtual-network/service-tags-overview.md#use-the-service-tag-discovery-api-public-preview) oder [Ermitteln von Diensttags mithilfe von herunterladbaren JSON-Dateien](../virtual-network/service-tags-overview.md#discover-service-tags-by-using-downloadable-json-files) verfügbar. In beiden Fällen sind die IP-Adressbereiche nach Regionen untergliedert. Sie können nur die IP-Adressbereiche auswählen, die der Region zugewiesen sind, in der Ihr Suchdienst bereitgestellt wird.

Für bestimmte Datenquellen kann das Diensttag selbst direkt verwendet werden, anstatt die Liste der IP-Adressbereiche aufzulisten (die IP-Adresse des Suchdiensts muss weiterhin explizit verwendet werden). Diese Datenquellen schränken den Zugriff durch das Einrichten einer [Netzwerksicherheitsgruppen-Regel](../virtual-network/network-security-groups-overview.md) ein, die native Unterstützung für das Hinzufügen eines Diensttags bietet, im Gegensatz zu IP-Regeln wie den von Azure Storage, Cosmos DB, Azure SQL usw. angebotenen. Folgende Datenquellen unterstützen die direkte Verwendung des `AzureCognitiveSearch`-Diensttags zusätzlich zur IP-Adresse des Suchdiensts:

- [SQL Server unter IaaS](./search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md#restrict-access-to-the-azure-cognitive-search)

- [SQL Managed Instances](./search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers.md#verify-nsg-rules)

Ausführliche Informationen finden Sie unter [Einrichten von IP-Firewallregeln zum Aktivieren des Indexerzugriffs](search-indexer-howto-access-ip-restricted.md).

## <a name="granting-access-via-private-endpoints"></a>Gewähren des Zugriffs über private Endpunkte

Indexer können [private Endpunkte](../private-link/private-endpoint-overview.md) für den Zugriff auf Ressourcen verwenden, die entweder zum Auswählen virtueller Netzwerke gesperrt sind, oder für die kein öffentlicher Zugriff aktiviert ist.
Diese Funktion ist nur für kostenpflichtige Dienste verfügbar, mit Beschränkungen hinsichtlich der Anzahl der privaten Endpunkte, die erstellt werden. Details zu den Grenzwerten werden auf der Seite [Dienstgrenzwerte in der kognitiven Azure-Suche](search-limits-quotas-capacity.md) beschrieben.

### <a name="step-1-create-a-private-endpoint-to-the-secure-resource"></a>Schritt 1: Erstellen eines privaten Endpunkts für die sichere Ressource

Kunden sollten den Suchverwaltungsvorgang aufrufen, die [API zum Erstellen oder Aktualisieren einer *gemeinsam genutzten Private Link-Ressource*](/rest/api/searchmanagement/sharedprivatelinkresources/createorupdate), um eine Verbindung über einen privaten Endpunkt mit ihrer sicheren Ressource (z. B. einem Speicherkonto) herzustellen. Datenverkehr, der über diese (ausgehende) Verbindung über einen privaten Endpunkt geleitet wird, stammt nur aus dem virtuellen Netzwerk, das sich in der suchdienstspezifischen „privaten“ Indexerausführungsumgebung befindet.

Azure Cognitive Search überprüft, ob Aufrufer dieser API über Berechtigungen zum Genehmigen von Anforderungen von Verbindungen über private Endpunkte an die sichere Ressource verfügen. Wenn Sie z. B. eine Verbindung über einen privaten Endpunkt mit einem Speicherkonto anfordern, auf das Sie keinen Zugriff haben, wird dieser Aufruf abgelehnt.

### <a name="step-2-approve-the-private-endpoint-connection"></a>Schritt 2: Genehmigen der Verbindung mit einem privaten Endpunkt

Wenn der (asynchrone) Vorgang abgeschlossen wird, der eine freigegebene Private Link-Ressource erstellt, wird eine Verbindung über einen privaten Endpunkt mit dem Status „Ausstehend“ erstellt. Es wird noch kein Datenverkehr über die Verbindung übertragen.
Es wird dann erwartet, dass der Kunde diese Anforderung auf seiner sicheren Ressource findet und „genehmigt“. In der Regel kann dies entweder über das Portal oder die [REST-API](/rest/api/virtualnetwork/privatelinkservices/updateprivateendpointconnection) erfolgen.

### <a name="step-3-force-indexers-to-run-in-the-private-environment"></a>Schritt 3: Erzwingen der Indexerausführung in der „privaten“ Umgebung

Ein genehmigter privater Endpunkt ermöglicht erfolgreiche vom Suchdienst ausgehende Aufrufe an eine Ressource, die über eine bestimmte Form von Zugriffsbeschränkungen auf Netzwerkebene verfügt (z. B. eine Speicherkonto-Datenquelle, die so konfiguriert ist, dass nur von bestimmten virtuellen Netzwerken darauf zugegriffen werden kann).
Dies bedeutet, dass jeder Indexer, der über den privaten Endpunkt eine solche Datenquelle erreichen kann, erfolgreich ausgeführt wird.
Wenn der private Endpunkt nicht genehmigt wird oder der Indexer die Verbindung mit dem privaten Endpunkt nicht nutzt, endet die Indexerausführung mit `transientFailure`.

Damit Indexer über Verbindungen über private Endpunkte auf Ressourcen zugreifen können, ist es obligatorisch, die `executionEnvironment` des Indexers auf `"Private"` festzulegen, um sicherzustellen, dass alle Indexerausführungen den privaten Endpunkt verwenden können. Dies liegt daran, dass private Endpunkte innerhalb der privaten suchdienstspezifischen Umgebung bereitgestellt werden.

```json
    {
      "name" : "myindexer",
      ... other indexer properties
      "parameters" : {
          ... other parameters
          "configuration" : {
            ... other configuration properties
            "executionEnvironment": "Private"
          }
        }
    }
```

Diese Schritte werden unter [Zugriff auf sichere Ressourcen über private Endpunkte](search-indexer-howto-access-private.md) ausführlicher beschrieben.
Wenn Sie über einen genehmigten privaten Endpunkt für eine Ressource verfügen, werden als *privat* festgelegte Indexer versuchen, über die Verbindung über den privaten Endpunkt Zugriff zu erhalten.

### <a name="limits"></a>Grenzwerte

Um eine optimale Leistung und Stabilität des Suchdiensts zu gewährleisten, werden Einschränkungen (von der Suchdienst-SKU) in den folgenden Dimensionen auferlegt:

- Die Arten von Indexern, die als *privat* festgelegt werden können.
- Die Anzahl der freigegebenen Private Link-Ressourcen, die erstellt werden können.
- Die Anzahl der unterschiedlichen Ressourcentypen, für die freigegebene Private Link-Ressourcen erstellt werden können.

Diese Grenzwerte werden in [Dienstgrenzwerte in der kognitiven Azure-Suche](search-limits-quotas-capacity.md) dokumentiert.