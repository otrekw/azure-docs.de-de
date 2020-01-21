---
title: Inkrementelle Anreicherung (Vorschau)
titleSuffix: Azure Cognitive Search
description: Sie können Zwischeninhalte und inkrementelle Änderungen aus der KI-Anreicherungspipeline in Azure Storage zwischenspeichern, um Investitionen in vorhandene verarbeitete Dokumente zu erhalten. Dieses Feature ist zurzeit als öffentliche Preview verfügbar.
manager: nitinme
author: Vkurpad
ms.author: vikurpad
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/09/2020
ms.openlocfilehash: 285b3608bc57d88ca2e81ed14355923436ed9d8d
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "76028512"
---
# <a name="introduction-to-incremental-enrichment-and-caching-in-azure-cognitive-search"></a>Einführung in inkrementelle Anreicherung und das Zwischenspeichern in Azure Cognitive Search

> [!IMPORTANT] 
> Die inkrementelle Anreicherung ist derzeit als öffentliche Vorschauversion verfügbar. Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Dieses Feature wird durch die [REST-API-Version 2019-05-06-Preview](search-api-preview.md) bereitgestellt. Derzeit werden weder das Portal noch das .NET SDK unterstützt.

Durch die inkrementelle Anreicherung werden Zwischenspeicherung und Statusbehaftung zu einer Anreicherungspipeline hinzugefügt, sodass Ihre Investition in vorhandene Ausgaben erhalten bleibt, während nur die Dokumente geändert werden, die von bestimmten Änderungen betroffen sind. Dies sorgt nicht nur dafür, dass ihre monetären Investitionen in die Verarbeitung (insbesondere OCR und Bildverarbeitung) gesichert werden, sondern schafft auch ein effizienteres System. Wenn Strukturen und Inhalt zwischengespeichert werden, kann ein Indexer nun ermitteln, welche Qualifikationen sich geändert haben, und nur die geänderten Qualifikationen und alle abhängigen Downstreamqualifikationen ausführen. 

## <a name="indexer-cache"></a>Indexercache

Die inkrementelle Anreicherung ergänzt die Anreicherungspipeline um einen Zwischenspeicher. Der Indexer speichert die Ergebnisse der Dokumententschlüsselung sowie die Ausgaben der einzelnen Qualifikationen für jedes Dokument zwischen. Wenn ein Skillset aktualisiert wird, werden lediglich die geänderten Qualifikationen (oder die Downstreamqualifikationen) erneut ausgeführt. Die aktualisierten Ergebnisse werden in den Zwischenspeicher geschrieben, und das Dokument wird im Suchindex oder im Wissensspeicher aktualisiert.

Der Zwischenspeicher wird physisch in einem Blobcontainer in Ihrem Azure Storage-Konto gespeichert. Alle Indizes in einem Suchdienst können für den Indexercache das gleiche Speicherkonto verwenden. Jedem Indexer wird ein eindeutiger und unveränderlicher Cachebezeichner zum Container zugewiesen, der ihn verwendet.

## <a name="cache-configuration"></a>Cachekonfiguration

Um die inkrementelle Anreicherung nutzen zu können, muss die `cache`-Eigenschaft für den Indexer festgelegt werden. Das folgende Beispiel veranschaulicht einen Indexer, bei dem die Zwischenspeicherung aktiviert ist. Bestimmte Teile dieser Konfiguration werden in den folgenden Abschnitten beschrieben. Weitere Informationen finden Sie unter [Einrichten der inkrementellen Anreicherung](search-howto-incremental-index.md).

```json
{
    "name": "myIndexerName",
    "targetIndexName": "myIndex",
    "dataSourceName": "myDatasource",
    "skillsetName": "mySkillset",
    "cache" : {
        "storageConnectionString" : "Your storage account connection string",
        "enableReprocessing": true
    },
    "fieldMappings" : [],
    "outputFieldMappings": [],
    "parameters": []
}
```

Wenn Sie diese Eigenschaft für einen vorhandenen Indexer festlegen, müssen Sie sie zurücksetzen und den Indexer erneut ausführen, was dazu führt, dass alle Dokumente in der Datenquelle erneut verarbeitet werden. Dieser Schritt ist erforderlich, um alle Dokumente zu eliminieren, die durch frühere Versionen des Skillsets angereichert wurden. 

## <a name="cache-management"></a>Cacheverwaltung

Der Lebenszyklus des Caches wird vom Indexer verwaltet. Wird die `cache`-Eigenschaft im Indexer auf „Null“ festgelegt oder die Verbindungszeichenfolge geändert, wird der vorhandene Cache gelöscht. Der Cachelebenszyklus ist auch an den Lebenszyklus des Indexers gebunden. Wird ein Indexer gelöscht, wird auch der zugehörige Cache gelöscht.

Die inkrementelle Anreicherung ist zwar so konzipiert, dass Änderungen erkannt werden und darauf reagiert wird, ohne dass Sie selbst eingreifen müssen. Es gibt aber Parameter, die Sie verwenden können, um Standardverhalten zu überschreiben:

+ Priorisieren neuer Dokumente
+ Umgeben von Skillsetüberprüfungen
+ Umgeben von Datenquellenüberprüfungen
+ Erzwingen der Skillsetauswertung

### <a name="prioritize-new-documents"></a>Priorisieren neuer Dokumente

Legen Sie die Eigenschaft `enableReprocessing` fest, um die Verarbeitung eingehender Dokumente, die bereits im Cache vorhanden sind, zu steuern. Ist diese Option auf `true` festgelegt (Standard), werden Dokumente, die bereits im Cache vorhanden sind, bei der erneuten Ausführung des Indexers erneut verarbeitet, vorausgesetzt, dass sich Ihre Qualifikationsaktualisierung auf dieses Dokument auswirkt. 

Ist diese Option auf `false` festgelegt, werden bestehende Dokumente nicht erneut verarbeitet, wodurch neuen, eingehenden Inhalten effektiv eine höhere Priorität eingeräumt wird als bestehenden Inhalten. Sie sollten `enableReprocessing` nur temporär auf `false` festlegen. Um die Konsistenz im Korpus sicherzustellen, sollte `enableReprocessing` in den meisten Fällen auf `true` festgelegt werden, um sicherzustellen, dass alle Dokumente, sowohl neue als auch vorhandene, gemäß der aktuellen Skillsetdefinition gültig sind.

### <a name="bypass-skillset-evaluation"></a>Umgehen der Skillsetauswertung

Das Ändern eines Skillsets und das erneute Verarbeiten dieses Skillsets gehen in der Regel Hand in Hand. Einige Änderungen an einem Skillset sollten jedoch nicht zur erneuten Verarbeitung führen (z. B. das Bereitstellen einer benutzerdefinierten Qualifikation an einem neuen Ort oder mit einem neuen Zugriffsschlüssel). Höchstwahrscheinlich handelt es sich hierbei um periphere Änderungen, die keine echte Auswirkung auf des Inhalt des Skillsets haben. 

Wenn Sie wissen, dass eine Änderung des Skillsets tatsächlich oberflächlich ist, sollten Sie die Skillsetauswertung außer Kraft setzen, indem Sie den Parameter `disableCacheReprocessingChangeDetection` auf `true` festlegen:

1. Rufen Sie „Update Skillset“ auf, und ändern Sie die Skillsetdefinition.
1. Hängen Sie den Parameter `disableCacheReprocessingChangeDetection=true` an die Anforderung an.
1. Übermitteln Sie die Änderung.

Indem Sie diesen Parameter festlegen, wird sichergestellt, dass nur Aktualisierungen der Skillsetdefinition committet werden. Die Auswirkungen der Änderung auf den vorhandenen Korpus werden dagegen nicht ausgewertet.

Das folgende Beispiel zeigt eine Update Skillset-Anforderung mit dem Parameter:

```http
PUT https://customerdemos.search.windows.net/skillsets/callcenter-text-skillset?api-version=2019-05-06-Preview&disableCacheReprocessingChangeDetection=true
```

### <a name="bypass-data-source-validation-checks"></a>Umgehen von Datenquellen-Validierungsüberprüfungen

Die meisten Änderungen an einer Datenquellendefinition führen dazu, dass der Cache ungültig wird. In Szenarien, bei denen eine Änderung den Cache nicht ungültig machen sollte (wie z. B. das Ändern einer Verbindungszeichenfolge oder das Rotieren des Schlüssels im Speicherkonto) fügen Sie jedoch den Parameter`ignoreResetRequirement` an die Datenquellenaktualisierung an. Wird dieser Parameter auf `true` festgelegt, kann der Commit durchlaufen werden, ohne eine Bedingung „Zurücksetzen“ auszulösen, die dazu führen würde, dass alle Objekte von Grund auf neu erstellt und gefüllt werden.

```http
PUT https://customerdemos.search.windows.net/datasources/callcenter-ds?api-version=2019-05-06-Preview&ignoreResetRequirement=true
```

### <a name="force-skillset-evaluation"></a>Erzwingen der Skillsetauswertung

Der Zweck des Caches ist es, unnötige Verarbeitung zu vermeiden, aber angenommen, Sie nehmen eine Änderung an einer Qualifikation vor, die der Indexer nicht erkennt (etwa eine Änderung an Elementen in externem Code, wie z. B. einer benutzerdefinierten Qualifikation).

In diesem Fall können Sie [Reset Skills](preview-api-resetskills.md) verwenden, um die erneute Verarbeitung einer bestimmten Qualifikation zu erzwingen, einschließlich aller Downstreamqualifikationen, die von der Ausgabe dieser Qualifikation abhängig sind. Diese API akzeptiert eine POST-Anforderung mit einer Liste von Qualifikationen, die ungültig gemacht und zur erneuten Verarbeitung markiert werden sollten. Führen Sie nach „Reset Skills“ den Indexer aus, um die Pipeline aufzurufen.

## <a name="change-detection"></a>Änderungserkennung

Sobald Sie einen Cache aktivieren, wertet der Indexer Änderungen in Ihrer Pipelinekomposition aus, um zu bestimmen, welche Inhalte wiederverwendet werden können und welche neu verarbeitet werden müssen. In diesem Abschnitt werden Änderungen aufgezählt, die den Cache vollständig ungültig machen, gefolgt von Änderungen, die eine inkrementelle Verarbeitung auslösen. 

### <a name="changes-that-invalidate-the-cache"></a>Änderungen, die den Cache ungültig machen

Durch eine solche Änderung wird der gesamte Cache ungültig. Ein Beispiel für eine solche Änderung wäre die Aktualisierung Ihrer Datenquelle. Im Anschluss folgt eine vollständige Liste der Änderungen, die dazu führen, dass Ihr Cache ungültig wird:

* Änderung des Datenquellentyps
* Änderung des Datenquellencontainers
* Anmeldeinformationen für die Datenquelle
* Richtlinie zur Erkennung von Änderungen für die Datenquelle
* Richtlinie zur Erkennung von Löschungen für die Datenquelle
* Indexerfeldzuordnungen
* Indexerparameter
    * Analysemodus
    * Ausgeschlossene Dateierweiterungen
    * Indizierter Dateierweiterungen
    * Indexspeicher-Metadaten (nur für übergroße Dokumente)
    * Textheader mit Trennzeichen
    * Trennzeichen für Text mit Trennzeichen
    * Dokumentstamm
    * Bildaktion (Änderung an der Art der Bildextraktion)

### <a name="changes-that-trigger-incremental-processing"></a>Änderungen, die eine inkrementelle Verarbeitung auslösen

Bei der inkrementellen Verarbeitung wird Ihre Skillsetdefinition ausgewertet, und es wird ermittelt, welche Qualifikationen erneut ausgeführt werden sollen, wobei die betroffenen Teile der Dokumentstruktur selektiv aktualisiert werden. Hier finden Sie die vollständige Liste der Änderungen, die zu einer inkrementellen Anreicherung führen:

* Anderer Qualifikationstyp im Skillset Aktualisierung des OData-Typs der Qualifikation
* Aktualisierung qualifikationsspezifischer Parameter wie URL, Standardwerte oder andere Parameter
* Änderung der Qualifikationsausgaben (Rückgabe zusätzlicher oder anderer Ausgaben durch die Qualifikation)
* Qualifikationsaktualisierungen, durch die sich eine andere Herkunft ergibt (Änderung der Qualifikationsverkettung, also Qualifikationseingaben)
* Upstream-Qualifikationsinvalidierung (im Falle der Aktualisierung einer Qualifikation, die eine Eingabe für diese Qualifikation bereitstellt)
* Aktualisierung des Projektionsspeicherorts für den Wissensspeicher, wodurch Dokumente neu projiziert werden müssen
* Änderung der Wissensspeicherprojektionen, wodurch Dokumente neu projiziert werden müssen
* Änderung der Ausgabefeldzuordnungen für einen Indexer, wodurch Dokumente erneut im Index projiziert werden müssen

## <a name="api-reference-content-for-incremental-enrichment"></a>API-Referenzinhalt für inkrementelle Anreicherung

REST `api-version=2019-05-06-Preview` stellt die APIs für die inkrementelle Anreicherung mit Ergänzungen für Indexer, Skillsets und Datenquellen bereit. Die [offizielle Referenzdokumentation](https://docs.microsoft.com/rest/api/searchservice/) bezieht sich auf allgemein verfügbare APIs und deckt keine Previewfunktionen ab. Der folgende Abschnitt enthält den Referenzinhalt für betroffene APIs.

Verwendungsinformationen und Beispiele finden Sie unter [Konfigurieren der Zwischenspeicherung für inkrementelle Anreicherung](search-howto-incremental-index.md).

### <a name="indexers"></a>Indexer

[Indexer erstellen](https://docs.microsoft.com/rest/api/searchservice/create-indexer) und [Indexer aktualisieren](https://docs.microsoft.com/rest/api/searchservice/update-indexer) machen nun neue Eigenschaften im Zusammenhang mit dem Cache verfügbar:

+ `StorageAccountConnectionString`: Die Verbindungszeichenfolge für das Speicherkonto, das zum Zwischenspeichern der Zwischenergebnisse verwendet wird.

+ `EnableReprocessing`: Ist standardmäßig auf `true` festgelegt. Wird die Eigenschaft auf `false` festgelegt, werden zwar weiterhin Dokumente in den Cache geschrieben, vorhandene Dokumente werden jedoch nicht auf der Grundlage der zwischengespeicherten Daten erneut verarbeitet.

+ `ID` (schreibgeschützt): Die Cache-ID (`ID`) ist der Bezeichner des Containers innerhalb des `annotationCache`-Speicherkontos, das als Cache für diesen Indexer verwendet wird. Hierbei handelt es sich um einen eindeutigen Cache für den Indexer. Wird der Indexer gelöscht und dann mit dem gleichen Namen erneut erstellt, wird auch `ID` erneut generiert. `ID` kann nicht festgelegt werden, sondern wird durch den Dienst generiert.

### <a name="skillsets"></a>Qualifikationsgruppen

+ [Update Skillset](https://docs.microsoft.com/rest/api/searchservice/update-skillset) unterstützt einen neuen Parameter in der Anforderung: `disableCacheReprocessingChangeDetection`. Dieser sollte auf `true` festgelegt werden, wenn Sie keine Aktualisierungen bestehender Dokumente auf der Grundlage der aktuellen Aktion wünschen.

+ [Reset Skills](preview-api-resetskills.md) ist eine neue Operation, mit der ein Skillset ungültig gemacht wird.

### <a name="datasources"></a>Datenquellen

+ Einige Indexer rufen Daten über Abfragen ab. Für Abfragen, die Daten abrufen, unterstützt [Update Data Source](https://docs.microsoft.com/rest/api/searchservice/update-data-source) einen neuen Parameter in einer Anforderung: `ignoreResetRequirement`, der auf `true` festgelegt werden sollte, wenn Ihre Aktualisierungsaktion den Cache nicht ungültig machen soll.

Setzen Sie `ignoreResetRequirement` mit Bedacht ein, da diese Option zu unerwünschten Inkonsistenzen in Ihren Daten führen kann, die nicht ohne Weiteres erkennbar sind.

## <a name="next-steps"></a>Nächste Schritte

Inkrementelle Anreicherung ist ein leistungsstarkes Feature, das die Änderungsverfolgung auf Skillsets und KI-Anreicherung ausdehnt. Während sich Skillsets weiterentwickeln, sorgt die Anreicherung für letztliche Konsistenz bei Ihren Dokumenten und hält dabei den Aufwand so gering wie möglich.

Beginnen Sie damit, dem einem vorhandenen Indexer einen Cache hinzuzufügen, oder fügen Sie den Cache beim Definieren eines neuen Indexers hinzu.

> [!div class="nextstepaction"]
> [Konfigurieren der Zwischenspeicherung für die inkrementelle Anreicherung](search-howto-incremental-index.md)
