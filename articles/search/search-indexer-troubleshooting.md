---
title: Beheben von häufigen Problemen bei Suchindexern
titleSuffix: Azure Cognitive Search
description: Beheben von Fehlern und häufigen Problemen bei Indexern in der kognitiven Azure-Suche, einschließlich Datenquellenverbindung, Firewall und fehlender Dokumente.
manager: nitinme
author: mgottein
ms.author: magottei
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 79db94298d190f646393410ec73ba1a25bb48270
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85560394"
---
# <a name="troubleshooting-common-indexer-issues-in-azure-cognitive-search"></a>Beheben von häufigen Problemen bei Suchindexern in der kognitiven Azure-Suche

Indexer können bei der Indizierung von Daten in der kognitiven Azure-Suche auf diverse Probleme stoßen. Die Hauptfehlerkategorien umfassen:

* [Herstellen einer Verbindung mit einer Datenquelle oder anderen Ressourcen](#connection-errors)
* [Verarbeiten von Dokumenten](#document-processing-errors)
* [Erfassen von Dokumenten in einem Index](#index-errors)

## <a name="connection-errors"></a>Verbindungsfehler

> [!NOTE]
> Indexer haben eingeschränkten Zugriff auf Datenquellen und andere Ressourcen, die durch Azure-Netzwerksicherheitsmechanismen geschützt werden. Indexer können derzeit nur über entsprechende Einschränkungsmechanismen für den IP-Adressbereich oder gegebenenfalls über NSG-Regeln auf Datenquellen zugreifen. Details zum Zugriff auf die einzelnen unterstützten Datenquellen finden Sie weiter unten.
>
> Sie können die IP-Adresse Ihres Suchdiensts ermitteln, indem Sie seinen vollqualifizierten Domänennamen pingen (z. B. `<your-search-service-name>.search.windows.net`).
>
> Sie können den IP-Adressbereich des [Diensttags](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#available-service-tags) `AzureCognitiveSearch` ermitteln, indem Sie entweder [herunterladbare JSON-Dateien](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#discover-service-tags-by-using-downloadable-json-files) oder die [Diensttagermittlungs-API](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#use-the-service-tag-discovery-api-public-preview) verwenden. Der IP-Adressbereich wird wöchentlich aktualisiert.

### <a name="configure-firewall-rules"></a>Konfigurieren von Firewallregeln

Azure Storage, Cosmos DB und Azure SQL umfassen eine konfigurierbare Firewall. Sie erhalten keine spezifische Fehlermeldung, wenn die Firewall aktiviert ist. Firewallfehler sind normalerweise generisch und sehen wie folgt aus: `The remote server returned an error: (403) Forbidden` oder `Credentials provided in the connection string are invalid or have expired`.

Es gibt zwei Möglichkeiten, den Zugriff von Indexern auf diese Ressourcen in einem solchen Fall zu ermöglichen:

* Deaktivieren Sie die Firewall, indem Sie den Zugriff über **alle Netzwerke** zulassen (falls möglich).
* Alternativ können Sie den Zugriff für die IP-Adresse des Suchdiensts und den IP-Adressbereich des `AzureCognitiveSearch`-[Diensttags](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#available-service-tags) in den Firewallregeln der Ressource zulassen (Einschränkung des IP-Adressbereichs).

Details zum Konfigurieren der Einschränkungen des IP-Adressbereichs für die einzelnen Datenquellentypen finden Sie unter den folgenden Links:

* [Azure Storage (in englischer Sprache)](https://docs.microsoft.com/azure/storage/common/storage-network-security#grant-access-from-an-internet-ip-range)

* [Cosmos DB](https://docs.microsoft.com/azure/storage/common/storage-network-security#grant-access-from-an-internet-ip-range)

* [Azure SQL](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure#create-and-manage-ip-firewall-rules)

**Einschränkung**: Wie in der Dokumentation zu Azure Storage erläutert, funktionieren Einschränkungen des IP-Adressbereichs nur, wenn sich der Suchdienst und das Speicherkonto in unterschiedlichen Regionen befinden.

In Azure Functions (als [Skill für benutzerdefinierte Web-API](cognitive-search-custom-skill-web-api.md) verwendbar), werden auch [Einschränkungen für IP-Adressen](https://docs.microsoft.com/azure/azure-functions/ip-addresses#ip-address-restrictions) unterstützt. Die Liste der zu konfigurierenden IP-Adressen umfasst die IP-Adresse des Suchdiensts und den IP-Adressbereich des `AzureCognitiveSearch`-Diensttags.

Details zum Zugriff auf Daten in SQL Server auf einem virtuellen Azure-Computer finden Sie [hier](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md).

### <a name="configure-network-security-group-nsg-rules"></a>Konfigurieren von Regeln für Netzwerksicherheitsgruppen (NSG)

Beim Zugriff auf Daten in einer verwalteten SQL-Instanz oder bei Verwendung eines virtuellen Azure-Computers als Webdienst-URI für einen [Skill für benutzerdefinierte Web-API](cognitive-search-custom-skill-web-api.md) müssen sich Kunden nicht um die spezifischen IP-Adressen kümmern.

In diesen Fällen können der virtuelle Azure-Computer oder die verwaltete SQL-Instanz so konfiguriert werden, dass sie sich in einem virtuellen Netzwerk befinden. Dann kann eine Netzwerksicherheitsgruppe konfiguriert werden, um die Art des eingehenden und ausgehenden Netzwerkdatenverkehrs von Subnetzen virtueller Netzwerke und Netzwerkschnittstellen zu filtern.

Das `AzureCognitiveSearch`-Diensttag kann direkt in den [NSG-Regeln](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group#work-with-security-rules) für eingehenden Datenverkehr verwendet werden, ohne den zugehörigen IP-Adressbereich zu suchen.

Weitere Informationen zum Zugreifen auf Daten in einer verwalteten SQL-Instanz finden Sie [hier](search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers.md).

### <a name="cosmosdb-indexing-isnt-enabled"></a>Cosmos DB-Indizierung nicht aktiviert

Die kognitive Azure-Suche steht in impliziter Abhängigkeit zur Indizierung von Cosmos DB. Wenn Sie die automatische Indizierung in Cosmos DB deaktivieren, gibt die kognitive Azure-Suche einen erfolgreichen Status zurück, kann jedoch keine Containerinhalte indizieren. Anweisungen zum Überprüfen der Einstellungen und zum Aktivieren der Indizierung finden Sie unter [Verwalten der Indizierung in Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/how-to-manage-indexing-policy#use-the-azure-portal).

## <a name="document-processing-errors"></a>Fehler bei der Dokumentverarbeitung

### <a name="unprocessable-or-unsupported-documents"></a>Nicht verarbeitbare oder nicht unterstützte Dokumente

Der Blobindexer [ dokumentiert, welche Dokumentformate explizit unterstützt werden](search-howto-indexing-azure-blob-storage.md#SupportedFormats). Gelegentlich enthält ein Blobspeichercontainer nicht unterstützte Dokumente. Manchmal sind die Dokumente fehlerhaft. Sie können verhindern, dass Ihr Indexer für diese Dokumente beendet wird, indem Sie die [Konfigurationsoptionen](search-howto-indexing-azure-blob-storage.md#DealingWithErrors) ändern:

```
PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2020-06-30
Content-Type: application/json
api-key: [admin key]

{
  ... other parts of indexer definition
  "parameters" : { "configuration" : { "failOnUnsupportedContentType" : false, "failOnUnprocessableDocument" : false } }
}
```

### <a name="missing-document-content"></a>Fehlende Dokumentinhalte

Der Blobindexer [ findet und extrahiert Text aus Blobs in einem Container](search-howto-indexing-azure-blob-storage.md#how-azure-search-indexes-blobs). Beim Extrahieren von Text können u.a. diese Probleme auftreten:

* Das Dokument enthält nur gescannte Bilder. PDF-Blobs mit Nichttextinhalten wie gescannten Bildern (JPGs) erzeugen keine Ergebnisse in einer Standard-Blobindizierungspipeline. Wenn Sie Bildinhalte mit Textelementen haben, können Sie mithilfe der [kognitiven Suche](cognitive-search-concept-image-scenarios.md) Text finden und extrahieren.
* Der Blobindexer ist so konfiguriert, dass er nur Metadaten indiziert. Zum Extrahieren von Inhalten muss der Blobindexer so konfiguriert sein, dass er [sowohl Inhalte als auch Metadaten extrahiert](search-howto-indexing-azure-blob-storage.md#controlling-which-parts-of-the-blob-are-indexed):

```
PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2020-06-30
Content-Type: application/json
api-key: [admin key]

{
  ... other parts of indexer definition
  "parameters" : { "configuration" : { "dataToExtract" : "contentAndMetadata" } }
}
```

## <a name="index-errors"></a>Indexfehler

### <a name="missing-documents"></a>Fehlende Dokumente

Indexer finden Dokumente in einer [Datenquelle](https://docs.microsoft.com/rest/api/searchservice/create-data-source). Manchmal fehlt ein Datenquellendokument, das indiziert werden sollte, in einem Index. Es gibt mehrere Gründe für das Auftreten dieser Fehler:

* Das Dokument wurde nicht indiziert. Überprüfen Sie das Portal auf eine erfolgreiche Indexerausführung hin.
* Überprüfen Sie den Wert für die [Änderungsnachverfolgung](https://docs.microsoft.com/rest/api/searchservice/create-data-source#data-change-detection-policies). Wenn der hohe Grenzwert ein Datum ist, das auf einen späteren Zeitpunkt festgelegt ist, werden alle Dokumente mit einem früheren Datum vom Indexer übersprungen. Sie können den Zustand der Änderungsnachverfolgung des Indexers mithilfe der Felder „initialTrackingState“ und „finalTrackingState“ im [Indexerstatus](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status#indexer-execution-result) ermitteln.
* Das Dokument wurde nach der Ausführung des Indexers aktualisiert. Wenn Sie für Ihren Indexer einen [Zeitplan](https://docs.microsoft.com/rest/api/searchservice/create-indexer#indexer-schedule) festgelegt haben, wird er das Dokument schließlich erneut ausführen und abrufen.
* Die in der Datenquelle angegebene [Abfrage](/rest/api/searchservice/create-data-source) schließt das Dokument aus. Indexer können keine Dokumente indizieren, die nicht zur Datenquelle gehören.
* Das Dokument wurde durch [Feldzuordnungen](https://docs.microsoft.com/rest/api/searchservice/create-indexer#fieldmappings) oder [KI-Anreicherung](https://docs.microsoft.com/azure/search/cognitive-search-concept-intro) geändert und sieht anders als erwartet aus.
* Verwenden Sie zur Dokumentensuche die [API zum Suchen von Dokumenten](https://docs.microsoft.com/rest/api/searchservice/lookup-document).
