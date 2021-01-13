---
title: Schätzen der Kapazität und Kosten
titleSuffix: Azure Cognitive Search
description: Lesen Sie den Leitfaden zum Schätzen der Kapazität und zum Verwalten der Kosten für Suchdienste, einschließlich der Infrastruktur und Tools in Azure, und informieren Sie sich über bewährte Methoden für die effektivere Verwendung von Ressourcen.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/15/2020
ms.openlocfilehash: d48ae71a979a2d0f1457b0cefa8a98a02710dd96
ms.sourcegitcommit: 77ab078e255034bd1a8db499eec6fe9b093a8e4f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/16/2020
ms.locfileid: "97577558"
---
# <a name="how-to-estimate-capacity-and-costs-of-an-azure-cognitive-search-service"></a>Schätzen der Kapazität und Kosten eines Azure Cognitive Search-Diensts

Im Rahmen der Kapazitätsplanung für Azure Cognitive Search können Sie mithilfe der folgenden Anleitungen Kosten senken oder effektiver verwalten:

+ Erstellen Sie alle Ressourcen in derselben Region oder in möglichst wenigen Regionen, um Bandbreitengebühren zu minimieren oder auszuschließen.

+ Führen Sie alle Dienste in einer Ressourcengruppe zusammen, z.B. kognitive Azure-Suche, Cognitive Services und alle anderen in Ihrer Lösung verwendeten Azure-Dienste. Navigieren Sie im Azure-Portal zu der Ressourcengruppe, und verwenden Sie die Befehle für **Kostenverwaltung**, um Einblicke in die tatsächlichen und projizierten Ausgaben zu erhalten.

+ Sie können Azure App Service für Ihre Front-End-Anwendung verwenden, sodass Anforderungen und Antworten innerhalb der Grenzen des Rechenzentrums bleiben.

+ Führen Sie die Hochskalierung für ressourcenintensive Vorgänge wie die Indizierung durch, und passen Sie die Skalierung anschließend für reguläre Abfrageworkloads wieder an. Beginnen Sie mit der Mindestkonfiguration für die kognitive Azure-Suche (eine SU bestehend aus einer Partition und einem Replikat), und überwachen Sie dann die Benutzeraktivität, um Verwendungsmuster zu identifizieren, die auf die Notwendigkeit einer höheren Kapazität hindeuten. Wenn ein vorhersagbares Muster vorliegt, können Sie möglicherweise die Skalierung mit der Aktivität synchronisieren (um dies zu automatisieren, müssen Sie Code schreiben).

Informationen zu abrechenbaren Ereignissen, zur Formel für die Abrechnung und zur abrechenbaren Rate finden Sie unter [Auswählen eines Tarifs](search-sku-tier.md). Außerdem können Sie sich unter [Abrechnung und Kostenverwaltung](../cost-management-billing/cost-management-billing-overview.md) über integrierte Tools und Features im Zusammenhang mit Ausgaben informieren.

Das temporäre Herunterfahren eines Suchdiensts ist nicht möglich. Dedizierte Ressourcen werden für eine exklusive Nutzung während der gesamten Lebensdauer des Diensts ständig betrieben. Das Löschen eines Diensts ist endgültig und löscht auch die zugehörigen Daten.

Im Hinblick auf den Dienst selbst besteht die einzige Möglichkeit zur Kostenreduzierung darin, Replikate und Partitionen auf ein Niveau zu reduzieren, das immer noch eine akzeptable Leistung und [SLA-Konformität](https://azure.microsoft.com/support/legal/sla/search/v1_0/) bietet, oder einen Dienst mit einem niedrigeren Tarif zu erstellen. (Die Stundensätze von S1 sind niedriger als die von S2 oder S3.) Angenommen, Sie stellen einen Dienst am unteren Ende Ihrer voraussichtlichen Auslastungen bereit. Wenn dieser Dienst Ihre Anforderungen nicht mehr erfüllt, können Sie einen zweiten Dienst in einem höheren Tarif erstellen, Ihre Indizes für den zweiten Dienst neu erstellen und dann den ersten Dienst löschen.

## <a name="how-to-evaluate-capacity-requirements"></a>Auswerten von Kapazitätsanforderungen

In der kognitiven Azure-Suche wird Kapazität in *Replikate* und *Partitionen* strukturiert.

+ Replikate sind Instanzen des Suchdiensts. Jedes Replikat hostet die Kopie eines Index mit Lastenausgleich. Beispielsweise verfügt ein Dienst mit sechs Replikaten über sechs Kopien jedes in den Dienst geladenen Index.

+ Partitionen speichern Indizes und teilen automatisch durchsuchbare Daten. Zwei Partitionen teilen den Index in zwei Teile, drei Partitionen in drei Teile und so weiter. Im Hinblick auf die Kapazität ist die *Partitionsgröße* das primäre Unterscheidungsmerkmal für Tarife.

> [!NOTE]
> Alle Tarife vom Typ „Standard“ und „Speicheroptimiert“ unterstützen [flexible Partitions- und Replikatskombinationen](search-capacity-planning.md#chart), sodass Sie durch Ändern des Verhältnisses [Ihr System in Hinblick auf Geschwindigkeit oder Speicher optimieren](search-performance-optimization.md) können. Der Tarif „Basic“ bietet bis zu drei Replikate für Hochverfügbarkeit, enthält jedoch nur eine Partition. Die Tarife vom Typ „Free“ bieten keine dedizierten Ressourcen: Computeressourcen werden von mehreren Abonnenten gemeinsam genutzt.

### <a name="evaluating-capacity"></a>Abschätzen der Kapazität

Zwischen der Kapazität und den Kosten für die Ausführung des Diensts besteht ein direkter Zusammenhang. Tarife sind mit Grenzwerten auf zwei Ebenen verbunden: Speicher und Inhalt (z. B. Anzahl der Indizes). Der zuerst erreichte Grenzwert gilt – daher sollten Sie sich über beide Aspekte Gedanken machen.

In der Regel diktieren Geschäftsanforderungen die erforderliche Anzahl der Indizes. Für ein großes Repository von Dokumenten benötigen Sie beispielsweise einen globalen Index. Oder Sie benötigen möglicherweise je nach Region, Anwendung oder Geschäftsnische mehrere Indizes.

Um die Größe eines Indexes zu bestimmen, müssen Sie [einen erstellen](search-what-is-an-index.md). Die Größe basiert auf den importierten Daten und der Indexkonfiguration, also beispielsweise, ob Sie Vorschlagsfunktionen, die Filterung und Sortierung aktivieren.

Bei der Volltextsuche entspricht die primäre Datenstruktur der Struktur eines [invertierten Indexes](https://en.wikipedia.org/wiki/Inverted_index), die über andere Eigenschaften als die Quelldaten verfügt. Bei einem invertierten Index werden Größe und Komplexität vom Inhalt bestimmt, nicht notwendigerweise von der Menge der Daten, die Sie eingeben. Eine große Datenquelle mit hoher Redundanz könnte einen kleineren Index ergeben als ein kleineres Dataset mit stark variierendem Inhalt. Daher ist es kaum möglich, die Indexgröße aus der Größe des ursprünglichen Datasets abzuleiten.

> [!NOTE] 
> Auch wenn sich das Schätzen der künftig benötigten Indizes und des erforderlichen Speichers wie bloßes Mutmaßen anfühlt, lohnt sich der Aufwand. Stellt sich die Kapazität eines Tarifs als zu gering heraus, müssen Sie einen neuen Dienst in einem höheren Tarif bereitstellen und anschließend die [Indizes neu laden](search-howto-reindex.md). Für einen Dienst kann kein direktes Upgrade von einem Tarif auf einen anderen durchgeführt werden.
>

## <a name="estimate-with-the-free-tier"></a>Schätzung mit dem Free-Tarif

Eine Möglichkeit zum Abschätzen der Kapazität besteht darin, mit dem Tarif „Free“ zu beginnen. Bedenken Sie, dass der Dienst vom Typ „Free“ bis zu drei Indizes, 50 MB Speicherplatz und 2 Minuten Indizierungszeit bietet. Es kann schwierig sein, mit diesen Einschränkungen eine voraussichtliche Indexgröße zu schätzen. Sie können es dennoch wie folgt versuchen:

+ [Erstellen Sie einen kostenlosen Dienst](search-create-service-portal.md).
+ Bereiten Sie ein kleines repräsentatives Dataset vor.
+ [Erstellen Sie einen anfänglichen Index im Portal](search-get-started-portal.md), und notieren Sie sich die Größe. Funktionen und Attribute haben Auswirkungen auf den Speicher. Durch das Hinzufügen von Vorschlagsfunktionen (Search-as-you-Type-Abfragen) erhöhen sich beispielsweise die Speicheranforderungen. Sie können mehrere Versionen eines Index unter Verwendung desselben Datasets erstellen und dabei verschiedene Attribute für die einzelnen Felder verwenden, um zu ermitteln, wie sich die Speicheranforderungen ändern. Weitere Informationen finden Sie [im Abschnitt „Hinweise zum Speicher“ unter „Erstellen eines einfachen Index“](search-what-is-an-index.md#index-size).

Mit einer groben Schätzung können Sie die betreffende Größe für zwei Indizes verdoppeln (Entwicklung und Produktion) und dann Ihren Tarif entsprechend auswählen.

## <a name="estimate-with-a-billable-tier"></a>Schätzung mit einem abrechenbaren Tarif

Dedizierte Ressourcen ermöglichen längere Sampling- und Verarbeitungszeiten und eignen sich besser für realistische Schätzungen bezüglich Indexmenge, Größe und Abfragevolumen während der Entwicklung. Einige Kunden beginnen direkt mit einem abrechenbaren Tarif und werten ihn im Lauf des Entwicklungsprojekts erneut aus.

1. [Überprüfen Sie die Dienstgrenzwerte in jedem Tarif](./search-limits-quotas-capacity.md#index-limits), um festzustellen, ob niedrigere Tarife die Anzahl der benötigten Indizes unterstützen können. In den Tarifen „Basic“, „S1“ und „S2“ betragen die Grenzwerte für Indizes 15, 50 bzw. 200. Der Tarif „Speicheroptimiert“ ist auf zehn Indizes beschränkt, da er für eine geringe Anzahl sehr großer Indizes ausgelegt ist.

1. [Erstellen Sie einen Dienst unter einem abzurechnenden Tarif](search-create-service-portal.md):

    + Beginnen Sie mit einem niedrigen Tarif, z. B. „Basic“ oder „S1“, wenn Sie sich über die projizierte Last nicht sicher sind.
    + Sind umfangreiche Indizierungs- und Abfragelasten vorgesehen, starten Sie mit einem höheren Tarif, wie „S2“ oder sogar „S3“.
    + Beginnen Sie mit einem Tarif vom Typ „Speicheroptimiert“ („L1“ oder „L2“), wenn Sie sehr viele Daten indizieren möchten und die Abfragelast relativ gering ist (etwa im Fall einer internen Geschäftsanwendung).

1. [Erstellen Sie einen anfänglichen Index](search-what-is-an-index.md), um zu bestimmen, wie Quelldaten in einen Index übersetzt werden. Dies ist die einzige Möglichkeit, die Größe des Indexes zu schätzen.

1. [Überwachen Sie Speicher, Dienstgrenzwerte, Abfragevolumen und Latenz](search-monitor-usage.md) im Portal. Im Portal werden die Abfragen pro Sekunde, gedrosselte Abfragen und die Wartezeit bei Suchvorgängen angezeigt. Diese Werte können Ihnen dabei helfen, den richtigen Tarif auszuwählen. 

Anzahl und Größe von Indizes sind für die Analyse gleichermaßen wichtig. Die maximalen Grenzwerte werden durch vollständige Auslastung des Speichers (Partitionen) oder über die maximalen Grenzwerte von Ressourcen (Indizes, Indexer usw.) erreicht, je nachdem, was zuerst eintritt. Im Portal können Sie beides verfolgen, da die aktuelle Verwendung und die maximalen Grenzwerte nebeneinander auf der Seite „Übersicht“ angezeigt werden.

> [!NOTE]
> Der Speicherbedarf kann zunehmen, wenn Dokumente irrelevante Daten enthalten. Im Idealfall enthalten Dokumente nur die Daten, die Sie für die Suche benötigen. Binäre Daten sind nicht durchsuchbar und sollten separat gespeichert werden (beispielsweise in einer Azure-Tabelle oder in Azure Blob Storage). In diesem Fall sollte dem Index ein Feld mit einem URL-Verweis auf die externen Daten hinzugefügt werden. Die maximale Größe eines einzelnen Dokuments beträgt 16 MB (oder weniger, wenn Sie im Rahmen einer einzelnen Anforderung mehrere Dokumente gleichzeitig hochladen). Weitere Informationen finden Sie unter [Grenzwerte für den Dienst für die kognitive Azure-Suche](search-limits-quotas-capacity.md).
>

**Überlegungen zum Abfragevolumen**

„Abfragen pro Sekunde“ (Queries per Second, QPS) ist eine wichtige Metrik bei der Leistungsoptimierung, spielt aber im Allgemeinen bei der Tarifauswahl nur dann eine Rolle, wenn Sie zu Beginn ein hohes Abfragevolumen erwarten.

Die Tarife vom Typ „Standard“ bieten ein ausgewogenes Verhältnis von Replikaten und Partitionen. Durch Hinzufügen von Replikaten für den Lastenausgleich oder durch Hinzufügen von Partitionen für eine parallele Verarbeitung können Sie kürzere Abfrageverarbeitungszeiten erzielen. Nach der Bereitstellung des Diensts können Sie die Leistung optimieren.

Ist von Anfang an ein hohes durchgängiges Abfragevolumen zu erwarten, sollten Sie höhere Tarife vom Typ „Standard“ in Verbindung mit einer leistungsfähigeren Hardware in Erwägung ziehen. Bleiben diese Abfragevolumen aus, können Sie Partitionen und Replikate offline schalten oder auch zu einem Dienst mit niedrigerem Tarif wechseln. Weitere Informationen zum Berechnen des Abfragedurchsatzes finden Sie unter [Überlegungen zur Leistung und Optimierung der kognitiven Azure-Suche](search-performance-optimization.md).

Die Tarife vom Typ „Speicheroptimiert“ eigenen sich für Workloads mit großen Datenmengen und unterstützen insgesamt mehr verfügbaren Indexspeicher bei niedrigeren Anforderungen an die Abfragewartezeit. Dabei sollten Sie weiterhin zusätzliche Replikate für den Lastenausgleich und zusätzliche Partitionen für die parallele Verarbeitung verwenden. Nach der Bereitstellung des Diensts können Sie die Leistung optimieren.

**Vereinbarungen zum Servicelevel**

Die Funktionen des Tarifs „Free“ und der Vorschauversion bieten keine [Vereinbarungen zum Servicelevel (Service Level Agreements, SLA)](https://azure.microsoft.com/support/legal/sla/search/v1_0/). Für alle abrechenbaren Tarife gelten SLAs, wenn Sie genügend Redundanz für Ihren Dienst bereitstellen. Für Abfrage-SLAs (Lesezugriff) benötigen Sie zwei oder mehr Replikate. Für Abfrage- und Indizierungs-SLAs (Lese-/Schreibzugriff) sind drei oder mehr Replikate erforderlich. Die Anzahl der Partitionen hat keine Auswirkungen auf die SLAs.

## <a name="tips-for-tier-evaluation"></a>Tipps zur Tarifbewertung

+ Erstellen Sie Metriken zu Abfragen, und sammeln Sie Daten zu Verwendungsmustern (Abfragen während der Geschäftszeiten, Indizierung außerhalb der Spitzenzeiten). Treffen Sie anhand dieser Daten Entscheidungen zur Dienstbereitstellung. Da ein stündliches oder tägliches Intervall ungeeignet ist, können Sie Partitionen und Ressourcen dynamisch so anpassen, dass geplante Änderungen von Abfragevolumen ermöglicht werden. Auch ungeplante, aber anhaltende Änderungen können berücksichtigt werden, wenn Ebenen beständig genug sind, um die Durchführung von Aktionen zu garantieren.

+ Beachten Sie, dass der einzige Nachteil einer unterdimensionierten Bereitstellung darin besteht, dass Sie ggf. einen Dienst entfernen müssen, wenn die tatsächlichen Anforderungen Ihre Schätzungen überschreiten. Um Dienstunterbrechungen zu vermeiden, sollten Sie einen neuen Dienst in einem höheren Tarif erstellen und parallel ausführen, bis alle Apps und Anforderungen den neuen Endpunkt zum Ziel haben.

## <a name="next-steps"></a>Nächste Schritte

Beginnen Sie mit einem Tarif vom Typ „Free“, und erstellen Sie einen anfänglichen Index mit einer Teilmenge Ihrer Daten, um deren Eigenschaften zu verstehen. Die Datenstruktur in der kognitiven Azure-Suche entspricht einem invertierten Index. Größe und Komplexität eines invertierten Index werden vom Inhalt bestimmt. Denken Sie daran, dass hoch redundanter Inhalt eher zu einem kleineren Index tendiert als sehr unregelmäßiger Inhalt. Daher werden die Speicheranforderungen für den Index durch die Inhaltseigenschaften und nicht durch die Größe des Datasets bestimmt.

Stellen Sie nach einer ersten Schätzung der Indexgröße [einen abrechenbaren Dienst](search-create-service-portal.md) in einem der folgenden Tarife bereit, die in diesem Artikel beschrieben werden: „Basic“, „Standard“ oder „Speicheroptimiert“. Lockern Sie künstliche Einschränkungen im Zusammenhang mit der Dimensionierung von Daten, und [erstellen Sie den Index neu](search-howto-reindex.md), um alle Daten einzuschließen, die durchsuchbar sein sollen.

[Ordnen Sie Partitionen und Replikate zu](search-capacity-planning.md), wie es erforderlich ist, um die gewünschte Leistung und Skalierung zu erzielen.

Wenn Sie mit der Leistung und Kapazität zufrieden sind, dann sind Sie fertig. Erstellen Sie andernfalls erneut einen Suchdienst in einem anderen Tarif, der Ihren Anforderungen eher entspricht.

> [!NOTE]
> Wenn Sie Fragen haben, posten Sie in [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search), oder wenden Sie sich an den [Azure-Support](https://azure.microsoft.com/support/options/).