---
title: Wählen zwischen bereitgestelltem Durchsatz und serverlos in Azure Cosmos DB
description: Erfahren Sie, wie Sie zwischen bereitgestelltem Durchsatz und serverlos für ihre Workload wählen.
author: ThomasWeiss
ms.author: thweiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 01/08/2021
ms.openlocfilehash: 3f5c3400f319a3f9d5f1544457b009f90d479634
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98049829"
---
# <a name="how-to-choose-between-provisioned-throughput-and-serverless"></a>Wählen zwischen bereitgestelltem Durchsatz und serverlos
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

Azure Cosmos DB ist in zwei verschiedenen Kapazitätsmodi verfügbar: [bereitgestellter Durchsatz](set-throughput.md) und [serverlos](serverless.md). In beiden Modi können Sie genau die gleichen Datenbankvorgänge ausführen, aber die Abrechnung für diese Vorgänge unterscheidet sich radikal. Im folgenden Video werden die wichtigsten Unterschiede zwischen diesen Modi und deren Eignung für verschiedene Arten von Workloads erläutert:

> [!VIDEO https://www.youtube.com/embed/CgYQo6uHyt0]

## <a name="detailed-comparison"></a>Ausführlicher Vergleich

| Kriterien | Bereitgestellter Durchsatz | Serverlos |
| --- | --- | --- |
| Status | Allgemein verfügbar | In der Vorschau |
| Am besten geeignet für | Workloads mit anhaltendem Datenverkehr, die vorhersagbare Leistung erfordern | Workloads mit zeitweiligem oder unvorhersehbarem Datenverkehr und einem geringen Verhältnis zwischen durchschnittlichem und hohem Datenverkehrsaufkommen |
| Funktionsweise | Für jeden Ihrer Container stellen Sie eine gewisse Durchsatzmenge bereit, ausgedrückt in [Anforderungseinheiten](request-units.md) pro Sekunde. Jede Sekunde steht diese Menge von Anforderungseinheiten für Ihre Datenbankvorgänge zur Verfügung. Bereitgestellter Durchsatz kann manuell aktualisiert oder automatisch mit [Autoskalierung](provision-throughput-autoscale.md) angepasst werden. | Sie führen die Datenbankvorgänge für Ihre Container aus, ohne Kapazität bereitstellen zu müssen. |
| Geografische Verteilung | Verfügbar (unbegrenzte Anzahl von Azure-Regionen) | Nicht verfügbar (serverlose Konten können nur in einer einzigen Azure-Region ausgeführt werden) |
| Maximale Speicherkapazität pro Container | Unbegrenzt | 50 GB |
| Leistung | < 10 ms Latenz für Punktlese- und Schreibvorgänge abgedeckt durch SLA | < 10 ms Latenz für Punktlese- und < 30 ms für Schreibvorgänge abgedeckt durch SLA |
| Abrechnungsmodell | Die Abrechnung erfolgt auf Stundenbasis für die bereitgestellten RU/s und unabhängig davon, wie viele RUs verbraucht wurden. | Die Abrechnung erfolgt auf Stundenbasis für die Menge an RUs, die von den Datenbankvorgängen beansprucht werden. |

> [!IMPORTANT]
> Einige dieser serverlosen Einschränkungen können gelockert oder aufgehoben werden, wenn serverlos allgemein verfügbar wird, und **Ihr Feedback** wird uns bei der Entscheidung unterstützen! Nehmen Sie Kontakt mit uns auf, und erzählen Sie uns mehr über Ihre Erfahrungen mit serverlos: [azurecosmosdbserverless@service.microsoft.com](mailto:azurecosmosdbserverless@service.microsoft.com).

## <a name="estimating-your-expected-consumption"></a>Schätzen der erwarteten Nutzung

In manchen Situationen kann es unklar sein, ob für eine bestimmte Workload bereitgestellter Durchsatz oder serverlos gewählt werden sollte. Um diese Entscheidung treffen zu können, können Sie Ihre gesamte **erwartete Nutzung** schätzen, d. h. die Gesamtzahl der RUs, die Sie im Laufe eines Monats wahrscheinlich nutzen (zur Schätzung können Sie die [hier](plan-manage-costs.md#estimating-serverless-costs) gezeigte Tabelle zurate ziehen).

**Beispiel 1**: Es wird davon ausgegangen, dass eine Workload auf maximal 500 RU/s anwächst und im Lauf eines Monats insgesamt 20 Millionen RUs beansprucht.

- Im Modus „bereitgestellter Durchsatz“ würden Sie einen Container mit 500 RU/s mit den folgenden monatlichen Kosten bereitstellen: 0,008 USD * 5 * 730 = **29,20 USD**
- Im serverlosen Modus zahlen Sie folgendermaßen für die verbrauchten RUs: 0,25 USD * 20 = **5,00 USD**

**Beispiel 2**: Es wird davon ausgegangen, dass eine Workload auf maximal 500 RU/s anwächst und im Laufe eines Monats insgesamt 250 Millionen RUs beansprucht.

- Im Modus „bereitgestellter Durchsatz“ würden Sie einen Container mit 500 RU/s mit den folgenden monatlichen Kosten bereitstellen: 0,008 USD * 5 * 730 = **29,20 USD**
- Im serverlosen Modus zahlen Sie folgendermaßen für die verbrauchten RUs: 0,25 USD * 250 = **62,50 USD**

(diese Beispiele berücksichtigen nicht die Speicherkosten, die für beide Modi gleich hoch sind)

> [!NOTE]
> Die im vorherigen Beispiel gezeigten Kosten dienen nur zu Demonstrationszwecken. Aktuelle Preisinformationen finden Sie auf der [Preisseite](https://azure.microsoft.com/pricing/details/cosmos-db/).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum [Bereitstellen von Durchsatz in Azure Cosmos DB](set-throughput.md)
- Weitere Informationen zu [Azure Cosmos DB serverlos](serverless.md)
- Einführung in das Konzept von [Anforderungseinheiten](request-units.md)
