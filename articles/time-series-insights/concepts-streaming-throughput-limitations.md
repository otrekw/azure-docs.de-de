---
title: 'Einschränkungen bei Datenstromerfassungs-Durchsatz: Azure Time Series Insights | Microsoft-Dokumentation'
description: Erfahren Sie mehr über Eingangsdurchsatz-Grenzwerte in Azure Time Series Insights.
author: lyrana
ms.author: lyhughes
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 06/03/2020
ms.custom: seodec18
ms.openlocfilehash: baa132589f5242ab480c9caa55ca793e0652ad6e
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86135010"
---
# <a name="streaming-ingestion-throughput-limits"></a>Durchsatzlimits für Datenstromerfassung

Unten sind die Einschränkungen für die Datenstromerfassung in Azure Time Series Insights beschrieben.

> [!TIP]
> Eine umfassende Liste mit allen Einschränkungen der Vorschauversion finden Sie unter [Planen Ihrer Azure Time Series Insights Preview-Umgebung](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-plan#review-preview-limits).

## <a name="per-environment-limitations"></a>Einschränkungen pro Umgebung

Im Allgemeinen werden Eingangsraten als Faktor der Anzahl von Geräten in Ihrer Organisation, der Häufigkeit der Ereignisausgabe und der Größe der einzelnen Ereignisse angesehen.

*  **Anzahl der Geräte** × **Häufigkeit der Ereignisausgabe** × **Größe jedes Ereignisses**.

Standardmäßig kann Time Series Insights eingehende Daten mit einer Rate von **bis zu 1 MB pro Sekunde (MBit/s) pro Time Series Insights-Umgebung** erfassen. Es gelten zusätzliche Einschränkungen [pro Hubpartition](concepts-streaming-throughput-limitations.md#hub-partitions-and-per-partition-limits).

> [!TIP]
>
> * Die Umgebungsunterstützung für Erfassungsgeschwindigkeiten von bis zu 16 MBit/s kann auf Anfrage bereitgestellt werden.
> * Falls Sie einen höheren Durchsatz benötigen, können Sie sich an uns wenden, indem Sie über das Azure-Portal ein Supportticket senden.
 
* **Beispiel 1:**

    Contoso Shipping verfügt über 100.000 Geräte, die dreimal pro Minute ein Ereignis ausgeben. Die Größe eines Ereignisses beträgt 200 Byte. Hierfür wird eine IoT Hub-Instanz mit vier Partitionen als Time Series Insights-Ereignisquelle genutzt.

    * Die Erfassungsrate für die Time Series Insights-Umgebung lautet: **100.000 Geräte · 200 Byte/Ereignis · (3/60 Ereignis/s) = 1 MBit/s**.
    * Die Erfassungsrate pro Partition wäre 0,25 Mbit/s.
    * Die Erfassungsrate von Contoso Shipping läge damit innerhalb der Skalierungseinschränkungen.

* **Beispiel 2:**

    Contoso Fleet Analytics verfügt über 60.000 Geräte, die jede Sekunde ein Ereignis ausgeben. Hierfür wird ein Event Hub mit vier Partitionen als Time Series Insights-Ereignisquelle genutzt. Die Größe eines Ereignisses beträgt 200 Byte.

    * Die Erfassungsrate der Umgebung wäre wie folgt: **60.000 Geräte x 200 Byte/Ereignis x 1 Ereignis/s = 12 MBit/s**.
    * Die Rate pro Partition wäre dann 3 MBit/s.
    * Die Erfassungsrate von Contoso Fleet Analytics liegt über den Umgebungs- und Partitionsgrenzwerten. Sie können über das Azure-Portal eine Anforderung an Time Series Insights senden, um die Erfassungsrate für ihre Umgebung zu erhöhen, und einen Event Hub mit mehr Partitionen erstellen, um die Grenzwerte der Vorschau nicht zu übersteigen.

## <a name="hub-partitions-and-per-partition-limits"></a>Hub-Partitionen und Limits pro Partition

Bei der Planung Ihrer Time Series Insights-Umgebung ist es wichtig, die Konfiguration der Ereignisquellen zu berücksichtigen, die Sie mit Time Series Insights verbinden möchten. Sowohl Azure IoT Hub als auch Event Hubs verwenden Partitionen, um die horizontale Skalierung für die Ereignisverarbeitung zu ermöglichen. 

Eine *Partition* ist eine geordnete Sequenz von Ereignissen, die in einem Event Hub besteht. Die Partitionsanzahl wird beim Erstellen des Hubs festgelegt und kann nicht geändert werden.

Informationen zu den bewährten Methoden für die Event Hubs-Partitionierung finden Sie unter [Wie viele Partitionen benötige ich?](https://docs.microsoft.com/azure/event-hubs/event-hubs-faq#how-many-partitions-do-i-need).

> [!NOTE]
> Für die meisten IoT Hub-Instanzen, die mit Azure Time Series Insights genutzt werden, sind nur vier Partitionen erforderlich.

Unabhängig davon, ob Sie einen neuen Hub für Ihre Time Series Insights-Umgebung erstellen oder einen vorhandenen Hub verwenden, müssen Sie die Erfassungsrate pro Partition berechnen, um festzustellen, ob diese innerhalb der Limits liegt. 

Für Azure Time Series Insights Preview gilt derzeit ein allgemeiner **Grenzwert von 0,5 MBit/s pro Partition**.

### <a name="iot-hub-specific-considerations"></a>IoT Hub-spezifische Überlegungen

Wenn ein Gerät in IoT Hub erstellt wird, wird es einer Partition dauerhaft zugewiesen. Auf diese Weise kann von IoT Hub die Ereignisreihenfolge sichergestellt werden (da sich die Zuweisung niemals ändert).

Eine feste Partitionszuweisung wirkt sich auch auf Time Series Insights-Instanzen aus, mit denen Daten erfasst werden, die von IoT Hub nachfolgend gesendet werden. Wenn Nachrichten von mehreren Geräten mit derselben Gatewaygeräte-ID an den Hub weitergeleitet werden, kommen sie ggf. gleichzeitig in derselben Partition an, und dies kann unter Umständen dazu führen, dass die Skalierungslimits pro Partition überschritten werden.

**Auswirkung**:

* Wenn es für eine einzelne Partition zu einer dauerhaften Erfassungsrate oberhalb des Limits der Vorschauversion kommt, werden von Time Series Insights ggf. nicht alle Gerätetelemetriedaten synchronisiert, bevor der Zeitraum für die IoT Hub-Datenaufbewahrung überschritten wurde. Aus diesem Grund können gesendete Daten verloren gehen, falls die Erfassungslimits dauerhaft überschritten werden.

Wir empfehlen Ihnen die folgenden bewährten Methoden, um dieses Problem zu umgehen:

* Berechnen Sie Ihre Erfassungsraten pro Umgebung und pro Partition, bevor Sie Ihre Lösung bereitstellen.
* Stellen Sie sicher, dass für Ihre IoT Hub-Geräte ein maximaler Lastenausgleich erfolgt.

> [!IMPORTANT]
> In Umgebungen, die IoT Hub als Ereignisquelle verwenden, berechnen Sie die Erfassungsrate anhand der Anzahl der verwendeten Hubgeräte, um sicherzugehen, dass die Rate unter 0,5 MBit/s pro Partitionseinschränkung fällt.
>
> * Auch wenn mehrere Ereignisse gleichzeitig eintreffen, wird das Limit der Vorschauversion nicht überschritten.

  ![IoT Hub-Partitionsdiagramm](media/concepts-ingress-overview/iot-hub-partiton-diagram.png)

Unter den folgenden Ressourcen finden Sie weitere Informationen zur Optimierung des Hub-Durchsatzes und der zugehörigen Partitionen:

* [Skalieren von Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-scaling)
* [Skalierbarkeit – Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/event-hubs-scalability#throughput-units)
* [Event Hub-Partitionen](https://docs.microsoft.com/azure/event-hubs/event-hubs-features#partitions)

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur [Datenspeicherung](concepts-storage.md).