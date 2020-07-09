---
title: Dienstgrenzwerte für Ebenen und SKUs
titleSuffix: Azure Cognitive Search
description: Grenzwerte für den Dienst, die bei der Kapazitätsplanung verwendet werden, sowie Höchstwerte für Anforderungen und Antworten für die kognitive Azure-Suche.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 05/11/2020
ms.openlocfilehash: 35cac2c05a8603313bb2bbe1bde3817dc88c6ed2
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83682635"
---
# <a name="service-limits-in-azure-cognitive-search"></a>Dienstgrenzwerte in der kognitiven Azure-Suche

Die Grenzwerte für Speicher, Workloads und Mengen von Indizes und anderen Objekten hängen davon ab, ob die [Bereitstellung von Azure Cognitive Search](search-create-service-portal.md) im Tarif **Free**, **Basic**, **Standard** oder **Storage Optimized** erfolgt.

+ **Free** ist ein gemeinsamer mehrinstanzfähiger Dienst, der Teil Ihres Azure-Abonnements ist. 

+ **Basic** bietet dedizierte Computeressourcen für Produktionsworkloads mit geringerem Umfang, teilen sich jedoch bestimmte Netzwerkinfrastruktur mit anderen Mandanten.

+ **Standard** wird auf dedizierten Computern ausgeführt. Sie bieten höhere Speicher- und Verarbeitungskapazität auf jeder Ebene. Standard ist in vier Ebenen verfügbar: S1, S2, S3 und S3 HD. S3 High Density (S3 HD) ist für [Mehrinstanzenfähigkeit](search-modeling-multitenant-saas-applications.md) und große Mengen von kleinen Indizes (3.000 Indizes pro Dienst) konzipiert. S3 HD bietet kein [Indexerfeature](search-indexer-overview.md), und die Datenerfassung muss APIs nutzen, die Daten aus der Quelle an den Index übermitteln. 

+ **Storage Optimized** wird auf dedizierten Computern mit mehr Gesamtspeicher, Speicherbandbreite und Arbeitsspeicher als **Standard** ausgeführt. Dieser Tarif ist für große Indizes konzipiert, die nur langsam geändert werden. „Storage Optimized“ gibt es auf zwei Ebenen: L1 und L2.

## <a name="subscription-limits"></a>Grenzwerte für Abonnements
[!INCLUDE [azure-search-limits-per-subscription](../../includes/azure-search-limits-per-subscription.md)]

## <a name="storage-limits"></a>Speichergrenzwerte
[!INCLUDE [azure-search-limits-per-service](../../includes/azure-search-limits-per-service.md)]

<a name="index-limits"></a>

## <a name="index-limits"></a>Indexgrenzwerte

| Resource | Kostenlos | Basic&nbsp;<sup>1</sup>  | S1 | S2 | S3 | S3&nbsp;HD | L1 | L2 |
| -------- | ---- | ------------------- | --- | --- | --- | --- | --- | --- |
| Maximale Anzahl von Indizes |3 |5 oder 15 |50 |200 |200 |1000 pro Partition oder 3000 pro Dienst |10 |10 |
| Maximale Anzahl der einfachen Felder pro Index |1000 |100 |1000 |1000 |1000 |1000 |1000 |1000 |
| Maximale Anzahl der komplexen Sammlungsfelder pro Index |40 |40 |40 |40 |40 |40 |40 |40 |
| Maximale Anzahl von Elementen in allen komplexen Sammlungen pro Dokument&nbsp;<sup>2</sup> |3000 |3000 |3000 |3000 |3000 |3000 |3000 |3000 |
| Maximale Tiefe der komplexen Felder |10 |10 |10 |10 |10 |10 |10 |10 |
| Maximale Anzahl von [Vorschlägen](https://docs.microsoft.com/rest/api/searchservice/suggesters) pro Index |1 |1 |1 |1 |1 |1 |1 |1 |
| Maximale Anzahl von [Bewertungsprofilen](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) pro Index |100 |100 |100 |100 |100 |100 |100 |100 |
| Maximale Anzahl von Funktionen pro Profil |8 |8 |8 |8 |8 |8 |8 |8 |

<sup>1</sup> Basic-Dienste, die vor Dezember 2017 erstellt wurden, haben niedrigere Grenzwerte (5 statt 15) für Indizes. Der Basic-Tarif ist die einzige SKU mit einem unteren Grenzwert von 100 Feldern pro Index.

<sup>2</sup> Die Verwendung einer sehr großen Zahl von Elementen in komplexen Sammlungen pro Dokument führt derzeit zu einer hohen Speicherauslastung. Dies ist ein bekanntes Problem. Bis das Problem behoben wird, ist die Zahl 3.000 eine sichere Obergrenze für alle Dienstebenen. Dieser Grenzwert wird nur für Indizierungsvorgänge erzwungen, für die die früheste allgemein verfügbare API-Version mit Unterstützung von Feldern komplexen Typs (`2019-05-06`) verwendet wird (oder eine höhere Version). Damit es nicht zu Beeinträchtigungen für Clients kommt, für die ggf. ältere API-Vorschauversionen (mit Unterstützung von Feldern komplexen Typs) genutzt werden, wird dieser Grenzwert für Indizierungsvorgänge mit diesen API-Vorschauversionen nicht erzwungen. Beachten Sie, dass API-Vorschauversionen nicht für die Verwendung für Produktionsszenarien bestimmt sind und dass wir Kunden dringend empfehlen, die Umstellung auf die neueste allgemein verfügbare API-Version durchzuführen.

<a name="document-limits"></a>

## <a name="document-limits"></a>Dokumentgrenzwerte 

Seit Oktober 2018 gelten für neue Dienste, die in einem kostenpflichtigen Tarif (Basic, S1, S2, S3, S3 HD) in einer beliebigen Region erstellt werden, keine Grenzwerte für die Anzahl von Dokumenten mehr. Ältere Dienste, die vor Oktober 2018 erstellt wurden, unterliegen möglicherweise weiterhin den Grenzwerten für die Dokumentanzahl.

Um zu ermitteln, ob Ihr Dienst über Dokumentgrenzwerte verfügt, verwenden Sie die [GET Service Statistics-REST-API](https://docs.microsoft.com/rest/api/searchservice/get-service-statistics). Dokumentgrenzwerte werden in der Antwort angezeigt, wobei `null` für keine Grenzwerte steht.

> [!NOTE]
> Obwohl der Dienst selbst keine Dokumentgrenzwerte festlegt, gibt es einen Shardgrenzwert von etwa 24 Milliarden Dokumenten pro Index für Basic-, S1-, S2 und S3-Suchdienste. Für S3 HD liegt der Shardgrenzwert bei 2 Milliarden Dokumenten pro Index. Jedes Element einer komplexen Sammlung zählt für den Shardgrenzwert als separates Dokument.

### <a name="document-size-limits-per-api-call"></a>Dokumentgrößenbeschränkungen pro API-Aufruf

Die maximale Dokumentgröße beim Aufrufen einer Index-API beträgt etwa 16 Megabytes.

Die Dokumentgröße ist tatsächlich eine Begrenzung der Größe des Anforderungstexts der Index-API. Da Sie einen Batch von mehreren Dokumenten auf einmal an die Index-API übergeben können, hängt die realistische maximale Größe davon ab, wie viele Dokumente im Batch vorhanden sind. Für einen Batch mit einem einzelnen Dokument beträgt die maximale Dokumentgröße 16 MB von JSON.

Denken Sie beim Schätzen der Dokumentgröße daran, nur die Felder zu berücksichtigen, die von einem Suchdienst verwendet werden können. Sämtliche Binär- oder Bilddaten in den Quelldokumenten sollten aus der Berechnungen ausgeschlossen werden.  

## <a name="indexer-limits"></a>Indexergrenzwerte

Es gibt eine maximale Ausführungsdauer, um den Dienst als Ganzes ausgewogen und stabil zu gestalten, aber größere Datensätze benötigen möglicherweise mehr Indizierungszeit, als das Maximum zulässt. Wenn ein Indizierungsauftrag nicht innerhalb der maximal zulässigen Zeit abgeschlossen werden kann, versuchen Sie, den Auftrag nach einem Zeitplan auszuführen. Der Planer verfolgt den Indizierungsstatus. Wenn ein geplanter Indizierungsauftrag aus irgendeinem Grund unterbrochen wird, kann der Indexer den Auftrag bei der nächsten geplanten Ausführung an der Stelle fortsetzen, an der er unterbrochen wurde.


| Resource | Free&nbsp;<sup>1</sup> | Basic&nbsp;<sup>2</sup>| S1 | S2 | S3 | S3&nbsp;HD&nbsp;<sup>3</sup>|L1 |L2 |
| -------- | ----------------- | ----------------- | --- | --- | --- | --- | --- | --- |
| Maximale Anzahl von Indexern |3 |5 oder 15|50 |200 |200 |– |10 |10 |
| Maximale Datenquellen |3 |5 oder 15 |50 |200 |200 |– |10 |10 |
| Maximale Qualifikationsgruppen <sup>4</sup> |3 |5 oder 15 |50 |200 |200 |– |10 |10 |
| Maximale Indizierungslast pro Aufruf |10.000 Dokumente |Nur durch maximale Dokumentanzahl beschränkt |Nur durch maximale Dokumentanzahl beschränkt |Nur durch maximale Dokumentanzahl beschränkt |Nur durch maximale Dokumentanzahl beschränkt |– |Keine Begrenzung |Keine Begrenzung |
| Minimaler Zeitplan | 5 Minuten |5 Minuten |5 Minuten |5 Minuten |5 Minuten |5 Minuten |5 Minuten | 5 Minuten |
| Maximale Ausführungsdauer <sup>5</sup> | 1–3 Minuten |24 Stunden |24 Stunden |24 Stunden |24 Stunden |–  |24 Stunden |24 Stunden |
| Maximale Ausführungsdauer für Qualifikationsgruppen der kognitiven Suche oder für die BLOB-Indizierung bei Bildanalysen <sup>5</sup> | 3 bis 10 Minuten |2 Stunden |2 Stunden |2 Stunden |2 Stunden |–  |2 Stunden |2 Stunden |
| Blobindexer: maximale Blobgröße, MB |16 |16 |128 |256 |256 |–  |256 |256 |
| Blobindexer: maximale Anzahl der Zeichen des aus einem Blob extrahierten Inhalts |32.000 |64.000 |4&nbsp;Millionen |8&nbsp;Mio. |16&nbsp;Mio. |– |4&nbsp;Millionen |4&nbsp;Millionen |

<sup>1</sup> Die maximale Indexerausführungszeit bei Diensten im Free-Tarif beträgt drei Minuten für Blobquellen und eine Minute für alle anderen Datenquellen. Für eine KI-Indizierung, die Aufrufe in Cognitive Services ausführt, gilt bei den kostenlosen Diensten ein Limit von 20 kostenlosen Transaktionen pro Tag. Dabei ist eine Transaktion als ein Dokument definiert, das die Anreicherungspipeline erfolgreich durchläuft.

<sup>2</sup> Basic-Dienste, die vor Dezember 2017 erstellt wurden, haben niedrigere Grenzwerte (5 statt 15) für Indexer, Datenquellen und Qualifikationsgruppen.

<sup>3</sup> S3 HD-Dienste beinhalten keine Indexerunterstützung.

<sup>4</sup> Maximal 30 Fähigkeiten pro Qualifikationsgruppe.

<sup>5</sup> Die KI-Anreicherung und die Bildanalyse sind rechenintensive Vorgänge, die unverhältnismäßig große Mengen der verfügbaren Verarbeitungskapazität verbrauchen. Die Laufzeit für diese Workloads wurde verkürzt, damit andere Aufträge in der Warteschlange bessere Chancen haben, ausgeführt zu werden.  

> [!NOTE]
> Wie unter [Indexgrenzwerte](#index-limits) beschrieben, erzwingen Indexer die Obergrenze von 3.000 Elementen auch für alle komplexen Sammlungen pro Dokument – ab der neuesten allgemein verfügbaren API-Version, die komplexe Typen (`2019-05-06`) unterstützt. Dies bedeutet, dass dieser Grenzwert für Sie nicht gilt, wenn Sie Ihren Indexer mit einer früheren API-Version erstellt haben. Zur Sicherstellung der maximalen Kompatibilität wird ein Indexer, der mit einer früheren API-Version erstellt und dann mit API-Version `2019-05-06` oder höher aktualisiert wurde, trotzdem von der Begrenzung **ausgenommen**. Kunden sollten sich dieser negativen Auswirkungen, die wie oben erwähnt mit der Verwendung sehr komplexer Sammlungen verbunden sind, bewusst sein. Wir empfehlen Ihnen dringend, für die Erstellung aller neuen Indexer die neueste allgemein verfügbare API-Version zu nutzen.

## <a name="synonym-limits"></a>Synonymlimits

Die maximale Anzahl der Synonymzuordnungen variiert je nach Tarif. Jede Regel kann bis zu 20 Erweiterungen aufweisen. Als Erweiterungen werden gleichwertige Begriffe bezeichnet. Beispielsweise würde für „Katze“, die Zuordnung von „Kätzchen“, „Kater“ und „Felis“ (die Gattung der Katzen) als drei Erweiterungen gezählt werden.

| Resource | Kostenlos | Basic | S1 | S2 | S3 | S3-HD |L1 | L2 |
| -------- | -----|------ |----|----|----|-------|---|----|
| Maximale Synonymzuordnungen |3 |3|5 |10 |20 |20 | 10 | 10 |
| Maximale Anzahl von Regeln pro Zuordnung |5\.000 |20000|20000 |20000 |20000 |20000 | 20000 | 20000  |

## <a name="queries-per-second-qps"></a>Abfragen pro Sekunde (QPS)

QPS-Schätzungen müssen unabhängig von jedem Kunde erstellt werden. Indexgröße und Komplexität, Abfragegröße und Komplexität sowie der Umfang des Datenverkehrs sind Hauptentscheidungskriterium für den QPS-Wert. Es gibt keine Möglichkeit, sinnvolle Schätzungen abzugeben, wenn diese Faktoren unbekannt sind.

Schätzungen sind besser vorhersagbar, wenn sie für Dienste berechnet werden, die auf dedizierten Ressourcen ausgeführt werden (Basic- und Standard-Tarife). Sie können den QPS-Wert genauer schätzen, da Sie die Kontrolle über mehr Parameter haben. Anleitungen zur Herangehensweise für Schätzungen finden Sie unter [Leistung und Optimierung der kognitiven Azure-Suche](search-performance-optimization.md).

Für die Tarife vom Typ „Storage Optimized“ (L1 und L2) sollten Sie einen geringeren Abfragedurchsatz und eine höhere Latenz als für die Tarife vom Typ „Standard“ erwarten. 

## <a name="data-limits-ai-enrichment"></a>Datengrenzwerte (KI-Anreicherung)

Für eine [KI-Anreicherungspipeline](cognitive-search-concept-intro.md), die Aufrufe zur [Entitätserkennung](cognitive-search-skill-entity-recognition.md), [Schlüsselbegriffserkennung](cognitive-search-skill-keyphrases.md), [Stimmungsanalyse](cognitive-search-skill-sentiment.md), [Sprachenerkennung](cognitive-search-skill-language-detection.md) und [Erkennung personenbezogener Daten](cognitive-search-skill-pii-detection.md) an eine Textanalyseressource sendet, gelten Datengrenzwerte. Die maximale Größe eines Datensatzes beträgt 50.000 Zeichen (gemessen durch [`String.Length`](https://docs.microsoft.com/dotnet/api/system.string.length)). Wenn Sie Ihre Daten teilen müssen, bevor Sie sie an das Stimmungsanalysetool senden, verwenden Sie den [Skill „Text teilen“](cognitive-search-skill-textsplit.md).

## <a name="throttling-limits"></a>Drosselungslimits

Suchabfrage- und Indizierungsanforderungen werden gedrosselt, wenn das System sich der Spitzenkapazität nähert. Die Drosselung verhält sich für verschiedene APIs unterschiedlich. Abfrage-APIs (Suchen/Vorschlagen/AutoVervollständigen) und Indizierungs-APIs drosseln dynamisch basierend auf der Last des Diensts. Index-APIs verfügen über statische Grenzwerte für Anforderungsraten. 

Statische Grenzwerte für Anforderungsraten für Indexvorgänge:

+ Indizes auflisten (GET /indexes): 5 pro Sekunde pro Sucheinheit
+ Index abrufen (GET /indexes/myindex): 10 pro Sekunde pro Sucheinheit
+ Index erstellen (POST /indexes): 12 pro Minute pro Sucheinheit
+ Index erstellen oder aktualisieren (PUT /indexes/myindex): 6 pro Sekunde pro Sucheinheit
+ Index löschen (DELETE /indexes/myindex): 12 pro Minute pro Sucheinheit 

## <a name="api-request-limits"></a>API-Anforderungsgrenzwerte
* Maximal 16 MB pro Anforderung <sup>1</sup>
* Maximale URL-Länge von 8 KB
* Maximal 1000 Dokumente pro Batch von Hochlade-, Zusammenführungs- oder Löschvorgängen für Indizes
* Maximal 32 Felder in $orderby-Klausel
* Maximale Suchbegriffgröße ist 32.766 Byte (32 KB minus 2 Bytes) von UTF-8-codiertem Text

<sup>1</sup> In der kognitiven Azure-Suche darf der Inhalt einer Anforderung nicht größer als 16 MB sein. Dies beschränkt möglicherweise den Inhalt einzelner Felder oder Sammlungen, für die keine anderen theoretischen Beschränkungen gelten. (Weitere Informationen zur Feldzusammensetzung und den Beschränkungen finden Sie unter [Unterstützte Datentypen](https://docs.microsoft.com/rest/api/searchservice/supported-data-types).)

## <a name="api-response-limits"></a>API-Antwortengrenzwerte
* Maximale Rückgabe von 1000 Dokumenten pro Seite mit Suchergebnissen
* Maximale Rückgabe von 100 Vorschlägen pro Anforderung der Vorschlags-API

## <a name="api-key-limits"></a>API-Schlüsselgrenzwerte
API-Schlüssel werden für die Dienstauthentifizierung verwendet. Es gibt zwei Arten. Administratorschlüssel werden im Anforderungsheader angegeben und gewähren vollständigen Lese-und Schreibzugriff für den Dienst. Abfrageschlüssel sind schreibgeschützt, die in der URL angegeben und normalerweise an Clientanwendungen verteilt werden.

* Maximal 2 Administratorschlüssel pro Dienst
* Maximal 50 Abfrageschlüssel pro Dienst