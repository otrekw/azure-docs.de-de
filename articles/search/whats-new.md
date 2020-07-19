---
title: Neuerungen in der kognitiven Azure-Suche
description: Ankündigungen neuer und erweiterter Features, einschließlich der Umbenennung des Diensts von Azure Search in kognitive Azure-Suche.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: overview
ms.date: 07/10/2020
ms.openlocfilehash: d2567a5a0374fd05f17e1153c3b2d7f3df50e0d9
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2020
ms.locfileid: "86246995"
---
# <a name="whats-new-in-azure-cognitive-search"></a>Neuerungen in der kognitiven Azure-Suche

Informieren Sie sich über die Neuerungen im Dienst. Legen Sie ein Lesezeichen für diese Seite an, um über den Dienst auf dem Laufenden zu bleiben.

## <a name="feature-announcements"></a>Featureankündigungen

### <a name="june-2020"></a>Juni 2020

|Funktion&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  | Category | BESCHREIBUNG | Verfügbarkeit  |
|---------|------------------|-------------|---------------|
[**Wissensspeicher**](knowledge-store-concept-intro.md) | KI-Anreicherung | Ausgabe eines durch KI angereicherten Indexers, der Inhalte in Azure Storage für die Verwendung in anderen Apps und Prozessen speichert. | Allgemein verfügbar. </br> Verwenden Sie [REST-API 2020-06-30](https://docs.microsoft.com/rest/api/searchservice/) oder höher suchen oder das Portal. |
| [**REST-API 2020-06-30 suchen**](https://docs.microsoft.com/rest/api/searchservice/) | REST | Eine neue stabile Version der REST-APIs. Zusätzlich zum Wissensspeicher enthält diese Version Verbesserungen in Bezug auf die Suchrelevanz und Bewertung. | Allgemein verfügbar. |
| [**Okapi BM25-Relevanzalgorithmus**](https://en.wikipedia.org/wiki/Okapi_BM25) | Abfrage | Der neue Algorithmus für die Relevanzrangfolge wird automatisch für alle neuen Suchdienste verwendet, die nach dem 15 Juli erstellt wurden. Für früher erstellte Dienste können Sie dies aktivieren, indem Sie die `similarity`-Eigenschaft für Indexfelder festlegen. | Allgemein verfügbar. </br> Verwenden Sie [REST-API 2020-06-30](https://docs.microsoft.com/rest/api/searchservice/) oder höher suchen oder REST-API 2019-05-06. |
| **executionEnvironment** | Sicherheit (Indexer) | Legen Sie diese Indexerkonfigurationseigenschaft explizit auf `private` fest, um alle Verbindungen mit externen Datenquellen über einen privaten Endpunkt zu erzwingen. Gilt nur für Suchdienste, die Azure Private Link nutzen. | Allgemein verfügbar. </br> Verwenden Sie [REST-API 2020-06-30 suchen](https://docs.microsoft.com/rest/api/searchservice/), um diesen allgemeinen Konfigurationsparameter festzulegen. |

### <a name="may-2020-microsoft-build"></a>Mai 2020 (Microsoft Build)

|Funktion&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  | Category | BESCHREIBUNG | Verfügbarkeit  |
|---------|------------------|-------------|---------------|
| [**Debugsitzungen**](cognitive-search-debug-session.md) | KI-Anreicherung | Debugsitzungen bieten eine portalbasierte Schnittstelle zur Untersuchung und Lösung von Problemen mit einem vorhandenen Skillset. In der Debugsitzung erstellte Korrekturen können in Skillsets der Produktionsumgebungen gespeichert werden. Erste Schritte mit [diesem Tutorial](cognitive-search-tutorial-debug-sessions.md). | Öffentliche Vorschau im Portal. |
| [**IP-Regeln für eingehende Firewallunterstützung**](service-configure-firewall.md) | Sicherheit | Beschränken Sie den Zugriff auf einen Suchdienstendpunkt auf bestimmte IP-Adressen. | Allgemein verfügbar. </br> Verwenden Sie [Verwaltungs-REST-API 2020-03-13](https://docs.microsoft.com/rest/api/searchmanagement/) oder höher oder das Portal. |
| [**Azure Private Link für einen privaten Suchendpunkt**](service-create-private-endpoint.md) | Sicherheit| Sie können einen Suchdienst vor dem öffentlichen Internet abschirmen, indem Sie ihn als Private Link-Ressource ausführen, auf die nur Client-Apps und andere Azure-Dienste im gleichen virtuellen Netzwerk zugreifen können. | Allgemein verfügbar. </br> Verwenden Sie [Verwaltungs-REST-API 2020-03-13](https://docs.microsoft.com/rest/api/searchmanagement/) oder höher oder das Portal. |
| [**Vom System verwaltete Identität (Vorschau)** ](search-howto-managed-identities-data-sources.md) | Sicherheit (Indexer) | Registrieren Sie einen Suchdienst als vertrauenswürdigen Dienst mit Azure Active Directory, um Verbindungen mit einer unterstützten Azure-Datenquelle für die Indizierung einzurichten. Gilt für [Indexer](search-indexer-overview.md), die Inhalte von Azure-Datenquellen wie Azure SQL Database, Azure Cosmos DB und Azure Storage erfassen. | Öffentliche Vorschau. </br> Verwenden Sie das Portal, um den Suchdienst zu registrieren. |
| [**sessionId-Abfrageparameter**](index-similarity-and-scoring.md), [scoringStatistics=global parameter](index-similarity-and-scoring.md#scoring-statistics) | Abfrage (Relevanz) | Fügen Sie einer Abfrage sessionID hinzu, um eine Sitzung für die Berechnung von Suchbewertungen einzurichten, wobei scoringStatistics=global zum Erfassen von Bewertungen aus allen Shards für konsistentere Suchsbewertungsberechnungen verwendet wird. | Allgemein verfügbar. </br> Verwenden Sie [REST-API 2020-06-30](https://docs.microsoft.com/rest/api/searchservice/) oder höher suchen oder REST-API 2019-05-06. |
| [**featuresMode (Vorschau)** ](index-similarity-and-scoring.md#featuresMode-param) | Abfrage | Fügen Sie diesen Abfrageparameter hinzu, um eine Relevanzbewertung zu erweitern, damit mehr Details angezeigt werden: Ähnlichkeitsergebnis pro Feld, Ausdruckshäufigkeit pro Feld und Anzahl von übereinstimmenden eindeutigen Token pro Feld. Sie können diese Datenpunkte in benutzerdefinierten Bewertungsalgorithmen verwenden. Ein Beispiel, in dem diese Funktion veranschaulicht wird, finden Sie unter [Hinzufügen von maschinellem Lernen (LearnToRank) zur Suchrelevanz](https://github.com/Azure-Samples/search-ranking-tutorial). | Öffentliche Vorschau. </br> Verwenden Sie [REST-API 2020-06-30-Preview suchen](https://docs.microsoft.com/rest/api/searchservice/index-preview) oder Rest-API 2019-05-06-Preview. |

### <a name="march-2020"></a>März 2020

|Funktion&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  | Category | BESCHREIBUNG | Verfügbarkeit  |
|---------|------------------|-------------|---------------|
| [**Natives vorläufiges Löschen von Blobs (Vorschau)** ](search-howto-indexing-azure-blob-storage.md#incremental-indexing-and-deletion-detection) | Indexer | Ein Azure Blob Storage-Indexer in Azure Cognitive Search erkennt Blobs, die sich im vorläufig gelöschten Zustand befinden. Das entsprechende Suchdokument wird während der Indizierung entfernt. | Öffentliche Vorschau. </br> Verwenden Sie [REST-API 2020-06-30-Preview suchen](https://docs.microsoft.com/rest/api/searchservice/index-preview) und REST-API 2019-05-06-Preview, wobei der Indexer für eine Azure-Blobdatenquelle ausgeführt wird, für die natives „vorläufiges Löschen“ aktiviert ist. |
| [**Verwaltungs-REST-API (2020-03-13)** ](https://docs.microsoft.com/rest/api/searchmanagement/management-api-versions) | REST | Neue stabile Rest-API zum Erstellen und Verwalten eines Suchdiensts. Fügt Unterstützung für IP-Firewall und Private Link hinzu. | Allgemein verfügbar. |

### <a name="february-2020"></a>Februar 2020

|Funktion&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  | Category | BESCHREIBUNG | Verfügbarkeit  |
|---------|------------------|-------------|---------------|
| [**PII-Erkennung (Vorschau)** ](cognitive-search-skill-pii-detection.md) | KI-Anreicherung | Ein neuer kognitiver Skill, der bei der Indizierung verwendet wird. Er extrahiert personenbezogene Informationen aus einem Eingabetext und bietet verschiedene Maskierungsmöglichkeiten für diese Informationen. | Öffentliche Vorschau. </br> Verwenden Sie das Portal oder [REST-API 2020-06-30-Preview suchen](https://docs.microsoft.com/rest/api/searchservice/index-preview) oder Rest-API 2019-05-06-Preview. |
| [**Benutzerdefinierte Entitätssuche (Vorschau)** ](cognitive-search-skill-custom-entity-lookup.md )| KI-Anreicherung | Ein neuer kognitiver Skill, der nach Text aus einer benutzerdefinierten Liste von Wörtern und Ausdrücken sucht. Mithilfe dieser Liste werden alle Dokumente mit übereinstimmenden Entitäten mit einer Bezeichnung markiert. Der Skill unterstützt auch einen gewissen Grad an Fuzzyübereinstimmung, der für die Suche nach ähnlichen, aber nicht genauen Übereinstimmungen verwendet werden kann. | Öffentliche Vorschau. </br> Verwenden Sie das Portal oder [REST-API 2020-06-30-Preview suchen](https://docs.microsoft.com/rest/api/searchservice/index-preview) oder Rest-API 2019-05-06-Preview. |

### <a name="january-2020"></a>Januar 2020

|Funktion&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  | Category | BESCHREIBUNG | Verfügbarkeit  |
|---------|------------------|-------------|---------------|
| [**Von Kunden verwaltete Verschlüsselungsschlüssel**](search-security-manage-encryption-keys.md) |Sicherheit | Zusätzlich zur integrierten Verschlüsselung der Plattform wird eine zusätzliche Verschlüsselungsebene hinzugefügt. Mithilfe eines Verschlüsselungsschlüssels, den Sie erstellen und verwalten, können Sie Indexinhalt und Synonymzuordnungen verschlüsseln, bevor die Nutzlast einen Suchdienst erreicht. | Allgemein verfügbar. </br> Verwenden Sie REST-API 2019-05-06 oder höher suchen. Bei verwaltetem Code heißt das richtige Paket noch [.NET SDK Version 8.0-preview](search-dotnet-sdk-migration-version-9.md), auch wenn das Feature sich nicht mehr in der Vorschau befindet. |
| [**IP-Regeln für eingehende Firewallunterstützung(Vorschau)** ](service-configure-firewall.md) | Sicherheit | Beschränken Sie den Zugriff auf einen Suchdienstendpunkt auf bestimmte IP-Adressen. Die Vorschau-API verfügt über neue **IpRule**- und **NetworkRuleSet**-Eigenschaften in der [CreateOrUpdate-API](https://docs.microsoft.com/rest/api/searchmanagement/2019-10-01-preview/createorupdate-service). Diese Previewfunktion ist in den ausgewählten Regionen verfügbar. |  Öffentliche Vorschau mit api-version=2019-10-01-Preview.  |
| [**Azure Private Link für einen privaten Suchendpunkt (Vorschau)** ](service-create-private-endpoint.md) | Sicherheit| Sie können einen Suchdienst vor dem öffentlichen Internet abschirmen, indem Sie ihn als Private Link-Ressource ausführen, auf die nur Client-Apps und andere Azure-Dienste im gleichen virtuellen Netzwerk zugreifen können. | Öffentliche Vorschau mit api-version=2019-10-01-Preview.  |

## <a name="feature-announcements-in-2019"></a>Featureankündigungen 2019

### <a name="december-2019"></a>Dezember 2019

+ [Demo-App erstellen (Vorschau)](search-create-app-portal.md) ist ein neuer Assistent im Portal, mit dem eine herunterladbare HTML-Datei mit Abfragezugriff (schreibgeschützt) auf einen Index generiert wird. Die Datei enthält ein eingebettetes Skript zum Rendern einer funktionierenden, Localhost-ähnlichen Web-App, die an einen Index Ihres Suchdiensts gebunden ist. Seiten können im Assistenten konfiguriert werden und eine Suchleiste, einen Ergebnisbereich, Randleistennavigation sowie Unterstützung von Eingabevorschlägen für Abfragen umfassen. Der HTML-Code kann offline geändert werden, um den Workflow oder die Darstellung zu erweitern oder anzupassen. Eine Demo-App kann nicht ohne Weiteres so erweitert werden, dass sie über Ebenen für Sicherheit und Hosting verfügt, die in Produktionsszenarien normalerweise benötigt werden. Sie sollten sie nicht als Vorstufe einer vollständigen Client-App ansehen, sondern als Überprüfungs- und Testtool.

+ Unter [Erstellen eines privaten Endpunkts für sichere Verbindungen (Vorschau)](service-create-private-endpoint.md) wird erläutert, wie Sie einen Azure Private Link für sichere Verbindungen mit Ihrem Suchdienst einrichten. Diese Previewfunktion ist auf Anforderung verfügbar und verwendet [Azure Private Link](../private-link/private-link-overview.md) und [Azure Virtual Network-](../virtual-network/virtual-networks-overview.md) als Teil der Lösung.

### <a name="november-2019---ignite-conference"></a>November 2019 – Ignite-Konferenz

+ Durch [Inkrementelle Anreicherung (Vorschau)](cognitive-search-incremental-indexing-conceptual.md) wird Zwischenspeicherung und Statusbehaftung zu einer Anreicherungspipeline hinzugefügt, sodass Sie an bestimmten Schritten oder Phasen arbeiten können, ohne bereits verarbeitete Inhalte zu verlieren. Bisher erforderte jede Änderung an einer Anreicherungspipeline eine vollständige Neuerstellung. Bei der inkrementellen Anreicherung wird die Ausgabe kostspieliger Analysen, insbesondere der Bildanalyse, beibehalten.

<!-- 
+ Custom Entity Lookup is a cognitive skill used during indexing that allows you to provide a list of custom entities (such as part numbers, diseases, or names of locations you care about) that should be found within the text. It supports fuzzy matching, case-insensitive matching, and entity synonyms. -->

+ Die [Dokumentextrahierung (Vorschauversion)](cognitive-search-skill-document-extraction.md) ist eine kognitive Qualifikation, die während der Indizierung verwendet wird und es Ihnen ermöglicht, den Inhalt einer Datei aus einem Skillset zu extrahieren. Bisher fand die Dokumententschlüsselung nur vor der Ausführung von Qualifikationsgruppen statt. Durch diese zusätzliche Qualifikation können Sie diesen Vorgang auch bei der Ausführung von Qualifikationsgruppen durchführen.

+ Die [Textübersetzung](cognitive-search-skill-text-translation.md) ist eine kognitive Qualifikation, die während der Indizierung zum Auswerten von Text verwendet wird und für jeden Datensatz Text zurückgibt, der in die angegebene Zielsprache übersetzt wurde.

+ [Power BI-Vorlagen](https://github.com/Azure-Samples/cognitive-search-templates/blob/master/README.md) können Ihren Visualisierungen und Analysen von angereicherten Inhalten in einem Wissensspeicher in Power BI Desktop neue Impulse verleihen. Diese Vorlage ist für Azure-Tabellenprojektionen konzipiert, die mithilfe des [Assistenten zum Importieren von Daten](knowledge-store-create-portal.md) erstellt wurden.

+ [Azure Data Lake Storage Gen2 (Vorschauversion)](search-howto-index-azure-data-lake-storage.md), die [Cosmos DB-Gremlin-API (Vorschauversion)](search-howto-index-cosmosdb.md) und die [Cosmos DB-Cassandra-API (Vorschauversion)](search-howto-index-cosmosdb.md) werden nun in Indexern unterstützt. Füllen Sie [dieses Formular](https://aka.ms/azure-cognitive-search/indexer-preview) aus, um sich zu registrieren. Sie erhalten eine Bestätigungs-E-Mail, sobald Sie in das Vorschauprogramm aufgenommen wurden.

### <a name="july-2019"></a>Juli 2019

+ In der [Azure Government-Cloud](../azure-government/documentation-government-services-webandmobile.md#azure-cognitive-search) allgemein verfügbar.

<a name="new-service-name"></a>

## <a name="new-service-name"></a>Neuer Dienstname

Azure Search wurde in **Azure Cognitive Search** umbenannt, um die erweiterte (optionale) Verwendung von kognitiven Skills und KI-Verarbeitung bei wichtigen Vorgängen widerzuspiegeln. API-Versionen, NuGet-Pakete, Namespaces und Endpunkte bleiben unverändert. Neue und bereits vorhandene Suchlösungen sind von der Änderung des Dienstnamens nicht betroffen.

## <a name="service-updates"></a>Dienstupdates

[Ankündigungen von Dienstupdates](https://azure.microsoft.com/updates/?product=search&status=all) für die kognitive Azure-Suche finden Sie auf der Azure-Website.