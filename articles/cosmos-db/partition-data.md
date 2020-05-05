---
title: Partitionierung und horizontale Skalierung in Azure Cosmos DB
description: Erfahren Sie, wie die Partitionierung in Azure Cosmos DB funktioniert, wie Partitionierung und Partitionsschlüssel konfiguriert werden und wie Sie den richtigen Partitionsschlüssel für Ihre Anwendung wählen.
author: deborahc
ms.author: dech
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/28/2020
ms.openlocfilehash: 19e4c61ba930bb9b127e2401174bcea3fd240dce
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82234209"
---
# <a name="partitioning-and-horizontal-scaling-in-azure-cosmos-db"></a>Partitionierung und horizontale Skalierung in Azure Cosmos DB

In diesem Artikel wird die Beziehung zwischen logischen und physischen Partitionen erörtert. Außerdem werden bewährte Methoden für die Partitionierung erläutert. Und Sie erhalten eine detaillierte Übersicht über die Funktionsweise der horizontalen Skalierung in Azure Cosmos DB. Sie müssen diese internen Details nicht unbedingt verstehen, um [Ihren Partitionsschlüssel auswählen](partitioning-overview.md#choose-partitionkey) zu können, aber wir haben sie angeführt, damit Sie Klarheit über die Funktionsweise von Azure Cosmos DB haben.

## <a name="logical-partitions"></a>Logische Partitionen

Eine logische Partition besteht aus einer Gruppe von Elementen mit demselben Partitionsschlüssel. Beispielsweise enthalten alle Elemente in einem Container mit Daten über Nahrungsmittel eine `foodGroup`-Eigenschaft. Sie können `foodGroup` als Partitionsschlüssel für den Container verwenden. Elementgruppen mit bestimmten Werten für `foodGroup` (z. B. `Beef Products`, `Baked Products` und `Sausages and Luncheon Meats`) bilden eindeutige logische Partitionen. Sie müssen sich keine Gedanken über das Löschen einer logischen Partition machen, wenn die zugrunde liegenden Daten gelöscht werden.

Eine logische Partition definiert auch den Bereich für Datenbanktransaktionen. Sie können Elemente in einer logischen Partition mithilfe einer [Transaktion mit Momentaufnahmeisolation](database-transactions-optimistic-concurrency.md) aktualisieren. Wenn einem Container neue Elemente hinzugefügt werden, werden neue logische Partitionen transparent vom System erstellt.

Es besteht keine Beschränkung für die Anzahl von logischen Partitionen in Ihrem Container. Jede logische Partition kann bis zu 20 GB Daten speichern. Durch eine sinnvolle Auswahl des Partitionsschlüssels ergeben sich viele mögliche Werte. Beispielsweise können in einem Container, in dem alle Elemente eine `foodGroup`-Eigenschaft aufweisen, die Daten innerhalb der logischen Partition `Beef Products` auf bis zu 20 GB anwachsen. Durch [Auswählen eines Partitionsschlüssels](partitioning-overview.md#choose-partitionkey) mit einer Vielzahl möglicher Werte können Sie sicherstellen, dass der Container skaliert werden kann.

## <a name="physical-partitions"></a>Physische Partitionen

Ein Azure Cosmos-Container wird skaliert, indem Daten und Durchsatz auf physische Partitionen verteilt werden. Intern wird mindestens eine logische Partition einer einzelnen physischen Partition zugeordnet. Die meisten kleinen Cosmos-Container verfügen über viele logische Partitionen, benötigen aber nur eine einzige physische Partition. Im Gegensatz zu logischen Partitionen sind physische Partitionen eine interne Implementierung des Systems, und sie werden vollständig von Azure Cosmos DB verwaltet.

Die Anzahl der physischen Partitionen in Ihrem Cosmos-Container hängt von folgenden Faktoren ab:

- Der Menge des bereitgestellten Durchsatzes (jede einzelne physische Partition kann einen Durchsatz von bis zu 10.000 Anforderungseinheiten pro Sekunde bereitstellen)
- Dem gesamten Datenspeicher (jede einzelne physische Partition kann bis zu 50 GB speichern)

Es besteht keine Beschränkung für die Gesamtanzahl von physischen Partitionen in Ihrem Container. Wenn der bereitgestellte Durchsatz oder die Datengröße zunimmt, erstellt Azure Cosmos DB automatisch neue physische Partitionen, indem die vorhandenen aufgeteilt werden. Die Aufteilung physischer Partitionen hat keine Auswirkungen auf die Verfügbarkeit Ihrer Anwendung. Nach der Aufteilung einer physischen Partition bleiben alle Daten innerhalb einer einzelnen logischen Partition weiterhin auf derselben physischen Partition gespeichert. Bei der Aufteilung physischer Partitionen wird einfach eine neue Zuordnung der logischen Partitionen zu physischen Partitionen erstellt.

Der für einen Container bereitgestellte Durchsatz wird gleichmäßig auf physische Partitionen aufgeteilt. Ein Partitionsschlüsselentwurf, der die Durchsatzanforderungen nicht gleichmäßig verteilt, kann „heiße“ Partitionen verursachen. „Heiße“ Partitionen können zu einer Ratenbegrenzung und einer ineffizienten Nutzung des bereitgestellten Durchsatzes sowie zu höheren Kosten führen.

Sie können die physischen Partitionen Ihres Containers im Azure-Portal auf dem Blatt **Metriken** im Abschnitt **Speicher** anzeigen:

[![Anzeigen der Anzahl von physischen Partitionen](./media/partition-data/view-partitions-zoomed-out.png) ](./media/partition-data/view-partitions-zoomed-in.png#lightbox)

In diesem Beispielcontainer, für den wir `/foodGroup` als Partitionsschlüssel ausgewählt haben, stellt jedes der drei Rechtecke eine physische Partition dar. In der Abbildung entspricht der **Partitionsschlüsselbereich** einer physischen Partition. Die ausgewählte physische Partition enthält drei logische Partitionen: `Beef Products`, `Vegetable and Vegetable Products` und `Soups, Sauces, and Gravies`.

Wenn wir einen Durchsatz von 18.000 Anforderungseinheiten pro Sekunde (RU/s) bereitstellen, kann jede der drei physischen Partitionen 1/3 des insgesamt bereitgestellten Durchsatzes nutzen. Innerhalb der ausgewählten physischen Partition können die logischen Partitionsschlüssel `Beef Products`, `Vegetable and Vegetable Products` und `Soups, Sauces, and Gravies` die 6.000 bereitgestellten RU/s der physischen Partition gemeinsam nutzen. Weil der bereitgestellte Durchsatz gleichmäßig auf die physischen Partitionen Ihres Containers aufgeteilt ist, müssen Sie unbedingt einen Partitionsschlüssel auswählen, der den verbrauchten Durchsatz gleichmäßig aufteilt. Informationen hierzu finden Sie unter [Auswählen eines Partitionsschlüssels](partitioning-overview.md#choose-partitionkey). Wenn Sie einen Partitionsschlüssel auswählen, der den verbrauchten Durchsatz gleichmäßig auf logische Partitionen verteilt, stellen Sie sicher, dass der Durchsatzverbrauch auf den physischen Partitionen ausgeglichen ist.

## <a name="replica-sets"></a>Replikatgruppen

Jede physische Partition besteht aus [*Replikatgruppen*](global-dist-under-the-hood.md). Jede Replikatgruppe hostet eine Instanz der Azure Cosmos-Datenbank-Engine. Eine Replikatgruppe macht die in der physischen Partition gespeicherten Daten dauerhaft, hochverfügbar und konsistent. Jedes Replikat, aus dem die physische Partition besteht, erbt das Speicherkontingent der Partition. Alle Replikate einer physischen Partition unterstützen gemeinsam den Durchsatz, der der physischen Partition zugeordnet wurde. Replikatgruppen werden von Azure Cosmos DB automatisch verwaltet.

Die meisten kleinen Cosmos-Container benötigen nur eine einzige physische Partition, sie verfügen jedoch trotzdem über mindestens 4 Replikate.

Die folgende Abbildung zeigt, wie logische Partitionen physischen Partitionen zugeordnet werden, die global verteilt sind:

![Abbildung zur Veranschaulichung der Azure Cosmos DB-Partitionierung](./media/partition-data/logical-partitions.png)

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über das [Wählen eines Partitionsschlüssels](partitioning-overview.md#choose-partitionkey).
* Erfahren Sie mehr über den [bereitgestellten Durchsatz in Azure Cosmos DB](request-units.md).
* Erfahren Sie mehr über die [globale Verteilung in Azure Cosmos DB](distribute-data-globally.md).
* Erfahren Sie mehr über das [Bereitstellen von Durchsatz für einen Azure Cosmos-Container](how-to-provision-container-throughput.md).
* Erfahren Sie mehr über das [Bereitstellen des Durchsatzes für eine Datenbank in Azure Cosmos](how-to-provision-database-throughput.md).
