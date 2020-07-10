---
title: Neuerungen in der kognitiven Azure-Suche
description: Ankündigungen neuer und erweiterter Features, einschließlich der Umbenennung des Diensts von Azure Search in kognitive Azure-Suche.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: overview
ms.date: 06/30/2020
ms.openlocfilehash: 078892691bfaec62f71f9d601a42de3f80221149
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2020
ms.locfileid: "85958156"
---
# <a name="whats-new-in-azure-cognitive-search"></a>Neuerungen in der kognitiven Azure-Suche

Informieren Sie sich über die Neuerungen im Dienst. Legen Sie ein Lesezeichen für diese Seite an, um über den Dienst auf dem Laufenden zu bleiben.

## <a name="feature-announcements"></a>Featureankündigungen

### <a name="june-2020"></a>Juni 2020

+ Der [Wissensspeicher](knowledge-store-concept-intro.md) ist jetzt allgemein verfügbar.

+ Die [Suchdienst-REST-API 2020-06-30](https://docs.microsoft.com/rest/api/searchservice/) ist die neue stabile Version der REST-APIs. Zusätzlich zum Wissensspeicher enthält diese allgemein verfügbare Version Verbesserungen in Bezug auf die Suchrelevanz und Bewertung.

+ Der neue Rangfolgenalgorithmus in Bezug auf die Relevanz ist für alle neuen Dienste, die Sie erstellen, jetzt [BM25](https://en.wikipedia.org/wiki/Okapi_BM25). Für vorhandene Dienste können Sie dies aktivieren, indem Sie die Eigenschaft `similarity` für Indexfelder festlegen. Diese Eigenschaft ist allgemein verfügbar.

+ Der neue Indexer `executionEnvironment` kann explizit auf `private` festgelegt werden. Diese Funktion unterstützt den Indexerzugriff auf externe Daten über private Endpunkte und ist allgemein verfügbar.

+ [Azure Machine Learning (AML)](cognitive-search-aml-skill.md) ist ein neuer Skilltyp zum Integrieren eines Rückschlussendpunkts aus Azure Machine Learning. Das Portal bietet Unterstützung für die Ermittlung und Integration Ihres Azure Machine Learning-Endpunkts innerhalb eines Cognitive Search-Skillsets. Die Ermittlung erfordert es, dass Ihre Cognitive Search- und AML-Dienste in demselben Abonnement bereitgestellt werden. Dieser Skill ist allgemein verfügbar. Erste Schritte mit [diesem Tutorial](cognitive-search-tutorial-aml-custom-skill.md).

### <a name="may-2020-microsoft-build"></a>Mai 2020 (Microsoft Build)

+ Features für [Debugsitzungen](cognitive-search-debug-session.md) befinden sich jetzt in der Vorschauphase. Debugsitzungen verfügen über eine portalbasierte Schnittstelle zur Untersuchung und Lösung von Problemen mit einem Skillset. In der Debugsitzung erstellte Korrekturen können in Skillsets der Produktionsumgebungen gespeichert werden. Erste Schritte mit [diesem Tutorial](cognitive-search-tutorial-debug-sessions.md).

+ Schirmen Sie einen Suchdienstendpunkt gegenüber dem öffentlichen Internet ab, indem Sie [IP-Regeln für die Unterstützung eingehender Firewalls konfigurieren](service-configure-firewall.md) oder [Azure Private Link für einen privaten Suchendpunkt](service-create-private-endpoint.md) nutzen. Beide Funktionen sind allgemein verfügbar.

+ Verwenden Sie eine [vom System verwaltete Identität (Vorschau)](search-howto-managed-identities-data-sources.md), um eine Verbindung zu einer Azure-Datenquelle für die Indexierung einzurichten. Gilt für [Indexer](search-indexer-overview.md), die Inhalte von Azure-Datenquellen wie Azure SQL Database, Azure Cosmos DB und Azure Storage erfassen.

+ Ändern Sie die Grundlage für die Berechnung der Suchbewertungen von „pro Shard“ in „alle Shards“, indem Sie die Abfrageparameter [sessionId](index-similarity-and-scoring.md) und [scoringStatistics=global](index-similarity-and-scoring.md#scoring-statistics) verwenden. Diese Parameter sind allgemein verfügbar.

+ Fügen Sie einen Abfrageparameter vom Typ [featuresMode (Vorschau)](index-similarity-and-scoring.md#featuresMode-param) hinzu, um eine Relevanzbewertung zu erweitern, damit mehr Details angezeigt werden: Ähnlichkeitsergebnis pro Feld, Ausdruckshäufigkeit pro Feld und Anzahl von übereinstimmenden eindeutigen Token pro Feld. Sie können diese Datenpunkte in benutzerdefinierten Bewertungsalgorithmen verwenden. Ein Beispiel, in dem diese Funktion veranschaulicht wird, finden Sie unter [Hinzufügen von maschinellem Lernen (LearnToRank) zur Suchrelevanz](https://github.com/Azure-Samples/search-ranking-tutorial).

### <a name="march-2020"></a>März 2020

+ [Natives vorläufiges Löschen von Blobs (Vorschau)](search-howto-indexing-azure-blob-storage.md#incremental-indexing-and-deletion-detection) bedeutet, dass der Azure Blob Storage-Indexer in Azure Cognitive Search Blobs erkennt, die sich im vorläufig gelöschten Zustand befinden. Das entsprechende Suchdokument wird während der Indizierung entfernt.

+ Die neue stabile [Verwaltungs-REST-API (2020-03-13)](https://docs.microsoft.com/rest/api/searchmanagement/management-api-versions) ist jetzt allgemein verfügbar. 

### <a name="february-2020"></a>Februar 2020

+ [PII-Erkennung (Vorschau)](cognitive-search-skill-pii-detection.md) ist eine Cognitive Search-Qualifikation und wird bei der Indizierung eingesetzt. Sie extrahiert personenbezogene Informationen aus einem Eingabetext und bietet verschiedene Maskierungsmöglichkeiten für diese Informationen.

+ [Benutzerdefinierte Entitätssuche (Vorschau)](cognitive-search-skill-custom-entity-lookup.md ) sucht nach Text aus einer benutzerdefinierten Liste von Wörtern und Ausdrücken. Mithilfe dieser Liste werden alle Dokumente mit übereinstimmenden Entitäten mit einer Bezeichnung markiert. Die Qualifikation unterstützt auch einen gewissen Grad an Fuzzyübereinstimmung, der für die Suche nach ähnlichen, aber nicht exakten Übereinstimmungen verwendet werden kann. 

### <a name="january-2020"></a>Januar 2020

+ [Vom Kunden verwaltete Verschlüsselungsschlüssel](search-security-manage-encryption-keys.md) jetzt allgemein verfügbar. Wenn Sie REST verwenden, können Sie über `api-version=2019-05-06` oder höher auf das Feature zugreifen. Bei verwaltetem Code heißt das richtige Paket noch [.NET SDK Version 8.0-preview](search-dotnet-sdk-migration-version-9.md), auch wenn das Feature sich nicht mehr in der Vorschau befindet. 

+ Privater Zugriff auf einen Suchdienst ist über zwei Mechanismen verfügbar (beider zurzeit in der Vorschau):

  + Sie können den Zugriff auf bestimmte IP-Adressen einschränken, indem Sie den Dienst mit der Verwaltungs-REST-API `api-version=2019-10-01-Preview` erstellen. Die Vorschau-API verfügt über neue **IpRule**- und **NetworkRuleSet**-Eigenschaften in der [CreateOrUpdate-API](https://docs.microsoft.com/rest/api/searchmanagement/2019-10-01-preview/createorupdate-service). Diese Previewfunktion ist in den ausgewählten Regionen verfügbar. Weitere Informationen finden Sie unter [Verwenden der Verwaltungs-REST-API](https://docs.microsoft.com/rest/api/searchmanagement/search-howto-management-rest-api).

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