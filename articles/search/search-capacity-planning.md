---
title: Anpassen der Kapazität für Abfrage- und Indexworkloads
titleSuffix: Azure Cognitive Search
description: Passen Sie Partitions- und Replikatcomputerressourcen in der kognitiven Azure-Suche an, wobei jede Ressource in abrechenbaren Sucheinheiten abgerechnet wird.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/14/2020
ms.openlocfilehash: e2ba5301b81b1a6f5de696ab4587cd8ff43e3c68
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "77462563"
---
# <a name="adjust-capacity-in-azure-cognitive-search"></a>Anpassen der Kapazität in Azure Cognitive Search

Nehmen Sie sich vor dem [Bereitstellen eines Suchdiensts](search-create-service-portal.md) und dem Einschließen einer bestimmten Preisstufe ein paar Minuten Zeit, um sich mit den Rollen von Replikaten und Partitionen in einem Dienst vertraut zu machen und zu verstehen, wie Sie einen Dienst anpassen können, um Spitzen und Einbrüche in der Ressourcennachfrage auszugleichen.

Die Kapazität ist vom [ausgewählten Tarif](search-sku-tier.md) (Tarife bestimmen die Hardwareeigenschaften) und der für geplante Workloads erforderlichen Kombination aus Replikat und Partition abhängig. Je nach Tarif und Umfang der Anpassung kann das Hinzufügen oder Reduzieren von Kapazität zwischen 15 Minuten und mehreren Stunden dauern. 

Wir empfehlen, die Replikat- und Partitionszuordnung über das Azure-Portal zu ändern. Das Portal erzwingt Grenzwerte für zulässige Kombinationen, damit die Obergrenzen eines Tarifs nicht überschritten werden. Wenn die Bereitstellung jedoch skript- oder codebasiert erfolgen soll, sind die [Azure PowerShell](search-manage-powershell.md) oder die [Verwaltungs-REST-API](https://docs.microsoft.com/rest/api/searchmanagement/services) alternative Lösungen.

## <a name="terminology-replicas-and-partitions"></a>Terminologie: Replikate und Partitionen

|||
|-|-|
|*Partitionen* | Partitionen stellen Indexspeicher und E/A für Lese-/Schreibvorgänge (beispielsweise bei der Neuerstellung oder Aktualisierung eines Index) bereit. Jede Partition hat einen Anteil am Gesamtindex. Wenn Sie drei Partitionen zuordnen, wird Ihr Index in Drittel aufgeteilt. |
|*Replikate* | Replikate sind Instanzen des Suchdiensts und dienen in erster Linie zum Lastenausgleich bei Abfragevorgängen. Jedes Replikat ist eine Kopie eines Indexes. Wenn Sie drei Replikate zuordnen, stehen Ihnen drei Kopien eines Indexes für die Bearbeitung von Abfrageanforderungen zur Verfügung.|

## <a name="when-to-add-nodes"></a>Zeitpunkt zum Hinzufügen von Knoten

Zunächst wird einem Dienst eine Mindestmenge von Ressourcen (bestehend aus einer Partition und einem Replikat) zugeordnet. 

Ein einzelner Dienst muss über genügend Ressourcen verfügen, um sämtliche Workloads (Indizierung und Abfragen) bewältigen zu können. Beide Workloads laufen nicht im Hintergrund. Sie können die Indizierung für Zeiten planen, in denen Abfrageanforderungen naturgemäß weniger häufig sind, aber der Dienst priorisiert ansonsten keine Aufgabe gegenüber einer anderen. Zusätzlich gleicht ein gewisses Maß an Redundanz die Abfrageleistung aus, wenn Dienste oder Knoten intern aktualisiert werden.

Allgemein gilt: Suchanwendungen benötigen in der Regel mehr Replikate als Partitionen – insbesondere, wenn die Dienstvorgänge auf Abfrageworkloads ausgerichtet sind. Warum das so ist, erfahren Sie im [Abschnitt zu Hochverfügbarkeit](#HA).

Das Hinzufügen weiterer Replikate oder Partitionen erhöht Ihre Kosten für den Betrieb des Diensts. Sie sollten daher den [Preisrechner](https://azure.microsoft.com/pricing/calculator/) verwenden, um die Auswirkungen des Hinzufügens weiterer Knoten auf die Abrechnung zu verstehen. Das [Diagramm unten ](#chart) kann Ihnen helfen, die Anzahl der für eine bestimmte Konfiguration erforderlichen Sucheinheiten zu ermitteln.

## <a name="how-to-allocate-replicas-and-partitions"></a>Gewusst wie: Zuordnen von Replikaten und Partitionen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, und wählen Sie den Suchdienst aus.

1. Öffnen Sie in **Einstellungen** die Seite **Skalierung**, um Replikate und Partitionen zu ändern. 

   Der folgende Screenshot zeigt einen Standarddienst, der mit einem Replikat und einer Partition bereitgestellt wurde. Die Formel im unteren Bereich gibt an, wie viele Sucheinheiten verwendet werden (1). Wenn der Preis pro Einheit 100 US-Dollar wäre (kein echter Preis), würden die monatlichen Kosten für die Ausführung dieses Diensts durchschnittlich 100 US-Dollar betragen.

   ![Seite „Skalierung“ mit aktuellen Werten](media/search-capacity-planning/1-initial-values.png "Seite „Skalierung“ mit aktuellen Werten")

1. Mit dem Schieberegler können Sie die Anzahl der Partitionen erhöhen oder verringern. Die Formel im unteren Bereich gibt an, wie viele Sucheinheiten verwendet werden.

   In diesem Beispiel wird die Kapazität verdoppelt – mit jeweils zwei Replikaten und Partitionen. Beachten Sie die Anzahl der Sucheinheiten. Sie beträgt jetzt vier, weil die Formel für die Abrechnung lautet: Replikate, multipliziert mit Partitionen (2 x 2). Bei einer Verdopplung der Kapazität fallen mehr als doppelt so hohe Kosten für die Ausführung des Diensts an. Wenn die Kosten für die Sucheinheit 100 US-Dollar wären, würde die neue Monatsrechnung jetzt 400 US-Dollar betragen.

   Wenn Sie die aktuellen Kosten pro Einheit für die einzelnen Tarife erfahren möchten, besuchen Sie die [Seite mit der Preisübersicht](https://azure.microsoft.com/pricing/details/search/).

   ![Hinzufügen von Replikaten und Partitionen](media/search-capacity-planning/2-add-2-each.png "Hinzufügen von Replikaten und Partitionen")

1. Klicken Sie auf **Speichern**, um die Änderungen zu bestätigen.

   ![Bestätigen von Änderungen an Skalierung und Abrechnung](media/search-capacity-planning/3-save-confirm.png "Bestätigen von Änderungen an Skalierung und Abrechnung")

   Kapazitätsänderungen dauern mehrere Stunden. Sie können den Prozess nach seinem Start nicht abbrechen, und es gibt keine Echtzeitüberwachung für Replikat- und Partitionsanpassungen. Allerdings bleibt die folgende Meldung sichtbar, während Änderungen vorgenommen werden.

   ![Statusmeldung im Portal](media/search-capacity-planning/4-updating.png "Statusmeldung im Portal")

> [!NOTE]
> Sobald ein Dienst bereitgestellt ist, kann kein direktes Upgrade auf einen höheren Tarif erfolgen. In diesem Fall müssen Sie einen Suchdienst unter dem neuen Tarif erstellen und Ihre Indizes neu laden. Informationen zur Dienstbereitstellung finden Sie unter [Erstellen eines Diensts für die kognitive Azure-Suche im Portal](search-create-service-portal.md).
>
> Darüber hinaus werden die Partitionen und Replikate ausschließlich und intern vom Dienst verwaltet. Es gibt kein Konzept der Prozessoraffinität oder der Zuweisung einer Workload auf einen bestimmten Knoten.
>

<a id="chart"></a>

## <a name="partition-and-replica-combinations"></a>Partitions- und Replikatskombinationen

Ein Basic-Dienst kann genau eine Partition und bis zu drei Replikate besitzen. Die Obergrenze liegt bei drei SUs. Nur die Replikate können angepasst werden. Für Hochverfügbarkeit bei Abfragen sind mindestens zwei Replikate erforderlich.

Alle Suchdienste vom Typ „Standard“ oder „Storage Optimized“ können die folgenden Kombinationen von Replikaten und Partitionen annehmen, vorbehaltlich dem Limit 36 SU. 

|   | **1 Partition** | **2 Partitionen** | **3 Partitionen** | **4 Partitionen** | **6 Partitionen** | **12 Partitionen** |
| --- | --- | --- | --- | --- | --- | --- |
| **1 Replikat:** |1 SU |2 SU |3 SU |4 SU |6 SU |12 SU |
| **2 Replikate** |2 SU |4 SU |6 SU |8 SU |12 SU |24 SU |
| **3 Replikate** |3 SU |6 SU |9 SU |12 SU |18 SU |36 SU |
| **4 Replikate** |4 SU |8 SU |12 SU |16 SU |24 SU |– |
| **5 Replikate** |5 SU |10 SU |15 SU |20 SU |30 SU |– |
| **6 Replikate** |6 SU |12 SU |18 SU |24 SU |36 SU |– |
| **12 Replikate** |12 SU |24 SU |36 SU |– |– |– |

SUs, Preise und Kapazität werden auf der Azure-Website ausführlich erläutert. Weitere Informationen finden Sie unter [Preise](https://azure.microsoft.com/pricing/details/search/).

> [!NOTE]
> Die Anzahl der Replikate und Partitionen ist ein ganzzahliger Teiler von 12 (d.h. 1, 2, 3, 4, 6, 12). Der Grund: Die kognitive Azure-Suche unterteilt jeden Index vorab in 12 Shards, damit er gleichmäßig auf alle Partitionen verteilt werden kann. Wenn Ihr Dienst z.B. drei Partitionen aufweist und Sie einen Index erstellen, enthält jede Partition 4 Shards des Indexes. Die kognitive Azure-Suche erstellt Shards eines Index in Form von Implementierungsdetails, die sich bei zukünftigen Versionen ändern können. Auch wenn die Anzahl heute 12 beträgt, sollten Sie nicht davon ausgehen, das dies auch in Zukunft immer so ist.
>

<a id="HA"></a>

## <a name="high-availability"></a>Hochverfügbarkeit

Da ein Hochskalieren einfach und relativ schnell durchzuführen ist, wird im Allgemeinen empfohlen, mit einer Partition und einem oder zwei Replikaten zu beginnen und dann bei steigenden Abfragevolumen hochzuskalieren. Abfrageworkloads werden in erster Linie auf Replikaten ausgeführt. Wenn Sie einen höheren Durchsatz oder Hochverfügbarkeit benötigen, sind wahrscheinlich zusätzliche Replikate erforderlich.

Allgemeine Empfehlungen für Hochverfügbarkeit sind:

* Zwei Replikate für Hochverfügbarkeit von schreibgeschützten Workloads (Abfragen)

* Drei oder mehr Replikate für Hochverfügbarkeit von Lese-/Schreibworkloads (Abfragen und Indizierung, wenn einzelne Dokumente hinzugefügt, aktualisiert oder gelöscht werden)

Vereinbarungen zum Servicelevel (Service Level Agreements, SLAs) für die kognitive Azure-Suche sind auf Abfragevorgänge und auf Indexupdates (Hinzufügen, Aktualisieren oder Löschen von Dokumenten) ausgerichtet.

Der Tarif „Basic“ erreicht den Höchstwert bei einer Partition und drei Replikaten. Wenn Sie die Flexibilität benötigen, auf Schwankungen beim Bedarf an Indizierungen und Abfragedurchsatz sofort zu reagieren, ziehen Sie einen der Standard-Tarife in Betracht.  Wenn Sie feststellen, dass Ihre Speicheranforderungen viel schneller wachsen als Ihr Abfragedurchsatz, sollten Sie einen der Tarife vom Typ „Storage Optimized“ in Betracht ziehen.

## <a name="disaster-recovery"></a>Notfallwiederherstellung

Derzeit steht kein integrierter Mechanismus für die Notfallwiederherstellung bereit. Das Hinzufügen von Partitionen oder Replikaten wäre die falsche Strategie, um die Zielsetzungen für eine Notfallwiederherstellung zu erfüllen. Der gängigste Ansatz ist, Redundanz auf Dienstebene durch Bereitstellung eines zweiten Suchdiensts in einer anderen Region hinzuzufügen. Die Umleitungs- oder Failoverlogik muss genau wie bei der Verfügbarkeit während der Indexneuerstellung in Ihrem Code bereitgestellt werden.

## <a name="estimate-replicas"></a>Schätzen der Replikate

Für Produktionsdienste sollten Sie aus SLA-Gründen drei Replikate zuordnen. Wenn Sie eine langsame Abfrageleistung feststellen, können Sie Replikate hinzufügen, damit zusätzliche Kopien des Index online geschaltet werden, um größere Abfrageworkloads zu unterstützen und die Anforderungslast gleichmäßig auf mehrere Replikate zu verteilen.

Wir stellen keine Richtlinien zur Verfügung, wie viele Replikate für bestimmte Abfragelasten benötigt werden. Die Abfrageleistung hängt von der Komplexität der Abfrage und den konkurrierenden Workloads ab. Obwohl das Hinzufügen von Replikaten die Leistung deutlich erhöht, ist das Endergebnis aber nicht streng linear: Das Hinzufügen von 3 Replikaten garantiert keinen dreifachen Durchsatz.

Leitlinien zur Schätzung der QPS für Ihre Lösung finden Sie unter [Skalieren zur Verbesserung der Leistung](search-performance-optimization.md) und [Überwachen von Abfragen](search-monitor-queries.md).

## <a name="estimate-partitions"></a>Schätzen der Partitionen

Der [ausgewählte Tarif](search-sku-tier.md) bestimmt die Größe und Geschwindigkeit der Partition, und jeder Tarif ist um eine Reihe von Eigenschaften optimiert, die auf bestimmte Szenarien ausgerichtet sind. Wenn Sie sich für einen höherwertigen Tarif entscheiden, benötigen Sie möglicherweise weniger Partitionen als bei S1. Eine der Fragen, die Sie durch selbstgesteuerte Tests beantworten müssen, lautet: Bringt eine größere und teurere Partition eine bessere Leistung als zwei billigere Partitionen bei einem Dienst, der in einem niedrigeren Tarif bereitgestellt wird?

Suchanwendungen, die Datenaktualisierung nahezu in Echtzeit erfordern, benötigen proportional mehr Partitionen als Replikate. Das Hinzufügen von Partitionen verteilt Lese-/ Schreibvorgänge über eine größere Anzahl von Computerressourcen. Außerdem erhalten Sie mehr Speicherplatz auf dem Datenträger zum Speichern zusätzlicher Indizes und Dokumente.

Größere Indizes erfordern eine längere Abfragezeit. Daher werden Sie feststellen, dass jede inkrementelle Zunahme an Partitionen einen kleineren, aber proportionalen Anstieg der Replikate erforderlich macht. Die Komplexität Ihrer Abfragen und das Abfragevolumen haben darauf Einfluss, wie schnell die Abfrage ausgeführt wird.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Auswählen eines Tarifs für die kognitive Azure-Suche](search-sku-tier.md)