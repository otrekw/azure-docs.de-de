---
title: API-Versionen
titleSuffix: Azure Cognitive Search
description: Versionsrichtlinie für REST-APIs für die kognitive Azure-Suche und die Clientbibliothek im .NET SDK.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 08/26/2020
ms.openlocfilehash: 73037ac3725ad1e7e9dd84597936c47406244630
ms.sourcegitcommit: e69bb334ea7e81d49530ebd6c2d3a3a8fa9775c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "88949880"
---
# <a name="api-versions-in-azure-cognitive-search"></a>API-Versionen in der kognitiven Azure-Suche

Für die kognitive Azure-Suche sind regelmäßig Featureupdates verfügbar. Manchmal, aber nicht immer, erfordern diese Updates eine neue Version der API, um die Abwärtskompatibilität zu gewährleisten. Die Veröffentlichung einer neuen Version ermöglicht Ihnen, zu steuern, wann und wie Sie Suchdienstupdates in Ihren Code integrieren.

In der Regel versucht das Team für die kognitive Azure-Suche, neue Versionen nur im Bedarfsfall zu veröffentlichen, da es einen gewissen Aufwand erfordern kann, wenn Sie Ihren Code aktualisieren, um eine neue API-Version zu verwenden. Eine neue Version ist nur dann erforderlich, wenn sich ein Aspekt der API in einer Weise geändert hat, die die Abwärtskompatibilität beeinträchtigt. Ursache für solche Änderungen können Fehlerbehebungen bei vorhandenen Funktionen sein oder neue Funktionen, die vorhandene API-Oberflächenbereiche ändern.

Die gleiche Regel gilt bei SDK-Updates. Das SDK für die kognitive Azure-Suche folgt den Regeln der [semantischen Versionierung](https://semver.org/), d.h., dass seine Version aus drei Teilen besteht: Haupt-, Neben- und Buildnummer (z.B. 1.1.0). Eine neue Hauptversion des SDK wird nur dann veröffentlicht, wenn Änderungen die Abwärtskompatibilität beeinträchtigen. Bei Funktionsupdates ohne Beeinträchtigung der Abwärtskompatibilität wird die Nebenversion, bei Fehlerbehebungen nur die Buildversion heraufgesetzt.

> [!Important]
> Die Azure-SDKs für .NET, Java, Python und JavaScript führen neue Clientbibliotheken für Azure Cognitive Search ein. Zurzeit unterstützt keine der Azure-SDK-Bibliotheken die neuesten REST-APIs für die Suche (2020-06-30) oder die Verwaltung (2020-03-13) vollständig, dies ändert sich jedoch im Laufe der Zeit. Sie können diese Seite oder die [Neuerungen](whats-new.md) in regelmäßigen Abständen überprüfen, um Ankündigungen zu funktionalen Erweiterungen zu erhalten.

<a name="unsupported-versions"></a>

## <a name="unsupported-versions"></a>Nicht unterstützte Versionen

Führen Sie bis zum 15. Oktober 2020 ein Upgrade der vorhandenen Suchlösungen auf die aktuelle Version der REST-API aus. Zu diesem Zeitpunkt werden die folgenden Versionen der Azure Cognitive Search-REST-API und deren Support eingestellt:

+ **2015-02-28**
+ **2015-02-28-Preview**
+ **2014-07-31-Preview**
+ **2014-10-20-Preview**

Darüber hinaus werden Versionen des Azure Cognitive Search .NET SDK vor [**3.0.0-rc**](https://www.nuget.org/packages/Microsoft.Azure.Search/3.0.0-rc), ebenfalls eingestellt, da sie auf eine dieser REST-API-Versionen ausgerichtet sind. 

Nach diesem Datum können Anwendungen, die eine der veralteten REST-API- oder SDK-Versionen verwenden, nicht mehr verwendet werden und müssen aktualisiert werden. Wie bei jeder Änderung dieser Art kündigen wir diese 12 Monate im Voraus an, sodass Sie ausreichend Zeit haben, sich darauf einzustellen.

Für die Weiterverwendung von Azure Cognitive Search muss vorhandener Code, der auf die [REST-API](search-api-migration.md) ausgerichtet ist, bis zum 15. Oktober 2020 zur [REST-API-Version 2020-06-30](https://docs.microsoft.com/rest/api/searchservice/) oder zu einem neueren SDK migriert werden.  Wenn Sie Fragen zur Aktualisierung auf die neueste Version haben, senden Sie bitte bis zum 15. Mai 2020 eine E-Mail an azuresearch_contact@microsoft.com, um sicherzustellen, dass genügend Zeit für die Aktualisierung Ihres Codes bleibt.

## <a name="rest-apis"></a>REST-APIs

Eine Instanz des Azure Cognitive Search-Diensts unterstützt mehrere REST-API-Versionen, einschließlich der neuesten. Sie können auch ältere Versionen weiterhin verwenden, aber es wird empfohlen, den Code zur neuesten Version zu [migrieren](search-api-migration.md). Wenn Sie die REST-API verwenden, müssen Sie die API-Version bei jeder Anforderung über den api-version-Parameter angeben. Wenn Sie das .NET SDK verwenden, legt die Version des verwendeten SDK die Version der REST-API fest. Wenn Sie ein älteres SDK verwenden, können Sie diesen Code weiterhin ohne Änderungen ausführen, selbst wenn der Dienst aktualisiert wird, um eine neuere Version der API zu unterstützen.

Die folgende Tabelle zeigt die Versionsgeschichte der aktuellen und zuvor veröffentlichter Versionen der REST-API für den Suchdienst. Die Dokumentation wird nur für die aktuellste stabile Version und die Vorschauversionen veröffentlicht.

### <a name="search-service-apis"></a>Suchdienst-APIs

Erstellen und verwalten Sie Inhalte in einem Suchdienst.

| Version&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   | Status | Problem mit der Abwärtskompatibilität |
|-------------------------|--------|------------------------------|
| [Search 2020-06-30](/rest/api/searchservice/index)| Stable | Das neueste stabile Release der Such-REST-APIs mit Verbesserungen bei der Relevanzbewertung und der allgemeinen Verfügbarkeit des Wissensspeichers.|
| [Search 2020-06-30-Preview](/rest/api/searchservice/index-preview)| Vorschau | Mit der stabilen Version verknüpfte Vorschauversion. Umfasst mehrere [Previewfunktionen](search-api-preview.md). |
| Search 2019-05-06 | Stable  | Fügt [komplexe Typen](search-howto-complex-data-types.md) hinzu. |
| Search 2019-05-06-Preview | Vorschau | Mit der stabilen Version verknüpfte Vorschauversion. |
| Search 2017-11-11 | Stable | Fügt Skillsets und [KI-Anreicherung](cognitive-search-concept-intro.md) hinzu. |
| Search 2017-11-11-Preview | Vorschau | Mit der stabilen Version verknüpfte Vorschauversion. |
| Search 2016-09-01 |Stable | Fügt [Indexer](search-indexer-overview.md) hinzu. |
| Search 2016-09-01-Preview | Vorschau | Mit der stabilen Version verknüpfte Vorschauversion.|
| Search 2015-02-28 | Nicht mehr unterstützt ab dem 10. 10. 2020   | Erstes allgemein verfügbares Release.  |
| Search 2015-02-28-Preview | Nicht mehr unterstützt ab dem 10. 10. 2020  | Mit der stabilen Version verknüpfte Vorschauversion. |
| Search 2014-10-20-Preview | Nicht mehr unterstützt ab dem 10. 10. 2020 | Zweite öffentliche Vorschauversion. |
| Search 2014-07-31-Preview | Nicht mehr unterstützt ab dem 10. 10. 2020  | Erste öffentliche Vorschauversion. |

### <a name="management-rest-apis"></a>REST-APIs für die Verwaltung

Erstellen und verwalten Sie einen Suchdienst, und verwalten Sie API-Schlüssel.

| Version&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   | Status | Problem mit der Abwärtskompatibilität |
|-------------------------|--------|------------------------------|
| [Management 2020-03-13](/rest/api/searchmanagement/) | Stable | Das neueste stabile Release der Management-REST-APIs mit Verbesserungen beim Endpunktschutz. Fügt einen [privaten Endpunkt](service-create-private-endpoint.md) über Private Link sowie [Netzwerk-IP-Regeln](service-configure-firewall.md) für neue Dienste hinzu. |
| [Management 2019-10-01-Preview](/rest/api/searchmanagement/index-2019-10-01-preview) | Vorschau  | Trotz der Versionsnummer ist dies weiterhin die aktuelle Vorschauversion der Management-REST-APIs. Zurzeit sind keine Vorschaufeatures vorhanden. Alle Vorschaufeatures wurden vor Kurzem in die allgemeine Verfügbarkeit überführt. |
| Management 2015-08-19  | Stable | Die erste allgemein verfügbare Version der Management-REST-APIs. Bietet Dienstbereitstellung, Hochskalierung und API-Schlüsselverwaltung. |
| Management 2015-08-19-Preview | Vorschau | Die erste Vorschauversion der Management-REST-APIs. |

## <a name="azure-sdk-for-net"></a>Azure SDK für .NET

In der folgenden Tabelle finden Sie Links zu neueren SDK-Versionen. 

| SDK-Version | Status | BESCHREIBUNG |
|-------------|--------|------------------------------|
| [Azure.Search.Documents 11.0](https://www.nuget.org/packages/Azure.Search.Documents/1.0.0-preview.4) | Stable | Neue Clientbibliothek aus dem Azure .NET SDK, veröffentlicht im Juli 2020. Ist für die REST-API für die Suche (Version 2020-06-30) vorgesehen, unterstützt jedoch noch keine Geofilter oder [FieldBuilder](/dotnet/api/microsoft.azure.search.fieldbuilder?view=azure-dotnet). |
| [Microsoft.Azure.Search 10.0](https://www.nuget.org/packages/Microsoft.Azure.Search/) | Stable | Veröffentlichung: Mai 2019 Gilt für die Such-REST-API, api-version=2019-05-06.|
| [Microsoft.Azure.Search 8.0-preview](https://www.nuget.org/packages/Microsoft.Azure.Search/8.0.0-preview) | Vorschau | Veröffentlichung: April 2019 Gilt für die Such-REST-API, api-version=2019-05-06-Preview.|
| [Microsoft.Azure.Management.Search 3.0.0](/dotnet/api/overview/azure/search/management?view=azure-dotnet) | Stable | Gilt für die Management REST-API, api-version=2015-08-19.  |

Weitere Informationen zu früheren Releases von Microsoft.Azure.Search finden Sie auf der Seite mit dem [NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.Search/) für alle Releases. Scrollen Sie nach unten zu **Versionsverlauf**, und erweitern Sie den Abschnitt, um die vollständige Liste der Releases anzuzeigen.

## <a name="azure-sdk-for-java"></a>Azure SDK für Java

| SDK-Version | Status | BESCHREIBUNG  |
|-------------|--------|------------------------------|
| [Java azure-search-documents 11](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-search-documents/11.0.0/index.html) | Stable | Neue Clientbibliothek aus dem Azure .NET SDK, veröffentlicht im Juli 2020. Gilt für die Such-REST-API, api-version=2019-05-06. |
| [Java Management Client 1.35.0](/java/api/overview/azure/search/management?view=azure-java-stable) | Stable | Gilt für die Management REST-API, api-version=2015-08-19. |

## <a name="azure-sdk-for-javascript"></a>Azure SDK für JavaScript

| SDK-Version | Status | BESCHREIBUNG  |
|-------------|--------|------------------------------|
| [JavaScript azure-search 11.0](https://azure.github.io/azure-sdk-for-node/azure-search/latest/) | Stable | Neue Clientbibliothek aus dem Azure .NET SDK, veröffentlicht im Juli 2020. Gilt für die Such-REST-API, api-version=2016-09-01. |
| [JavaScript azure-arm-search](https://azure.github.io/azure-sdk-for-node/azure-arm-search/latest/) | Stable | Gilt für die Management REST-API, api-version=2015-08-19. |

## <a name="azure-sdk-for-python"></a>Azure SDK für Python

| SDK-Version | Status | BESCHREIBUNG  |
|-------------|--------|------------------------------|
| [Python azure-search-documents 11.0](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-search-documents/11.0.0/index.html) | Stable | Neue Clientbibliothek aus dem Azure .NET SDK, veröffentlicht im Juli 2020. Gilt für die Such-REST-API, api-version=2019-05-06. |
| [Python azure-mgmt-search 1.0](/python/api/overview/azure/search?view=azure-python) | Stable | Gilt für die Management REST-API, api-version=2015-08-19. |