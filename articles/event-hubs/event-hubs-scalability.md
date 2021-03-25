---
title: Skalierbarkeit – Azure Event Hubs | Microsoft-Dokumentation
description: Dieser Artikel enthält Informationen zum Skalieren von Azure Event Hubs mithilfe von Partitionen und Durchsatzeinheiten.
ms.topic: article
ms.date: 03/16/2021
ms.openlocfilehash: f258ee2a3b4162dabf7a8e615db82b9b889d628b
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "103601276"
---
# <a name="scaling-with-event-hubs"></a>Skalierung mit Event Hubs

Es gibt zwei Faktoren, die die Skalierung mit Event Hubs beeinflussen.
*   Durchsatzeinheiten
*   Partitionen

## <a name="throughput-units"></a>Durchsatzeinheiten

Die Durchsatzkapazität von Event Hubs wird durch *Durchsatzeinheiten* gesteuert. Durchsatzeinheiten werden vorab als Kapazitätseinheiten erworben. Ein einzelner Durchsatz ermöglicht Folgendes:

* Eingehende Daten: bis zu 1 MB pro Sekunde oder 1.000 Ereignisse pro Sekunde, je nachdem, was zuerst eintritt
* Ausgehende Daten: bis zu 2 MB pro Sekunde oder 4.096 Ereignisse pro Sekunde

Bei Überschreitung der Kapazität der erworbenen Durchsatzeinheiten wird der Eingang eingeschränkt und [ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) zurückgegeben. Der Ausgang erstellt zwar keine Drosselungsausnahmen, die Kapazität der erworbenen Durchsatzeinheiten ist allerdings dennoch beschränkt. Wenn Sie Ausnahmen für die Veröffentlichungsrate erhalten oder einen größeren Ausgang erwarten, überprüfen Sie, wie viele Durchsatzeinheiten Sie für den Namespace erworben haben. Sie können Durchsatzeinheiten auf dem Blatt **Skalierung** der Namespaces im [Azure-Portal](https://portal.azure.com) verwalten. Mithilfe der [Event Hubs-APIs](./event-hubs-samples.md) können Durchsatzeinheiten auch programmgesteuert verwaltet werden.

Durchsatzeinheiten werden im Voraus erworben und auf Stundenbasis abgerechnet. Nach dem Erwerb werden Durchsatzeinheiten für mit einem Minimum von einer Stunde in Rechnung gestellt. Bis zu 20 Durchsatzeinheiten können für einen Event Hubs-Namespace erworben und in allen Event Hubs dieses Namespace gemeinsam verwendet werden.

Das Feature **Automatische Vergrößerung** von Event Hubs wird automatisch durch Erhöhung der Anzahl von Durchsatzeinheiten zentral hochskaliert, um den Nutzungsanforderungen gerecht zu werden. Zusätzliche Durchsatzeinheiten verhindern Drosselungsszenarien, bei denen Folgendes vorliegt:

- Die Datenerfassungsrate überschreitet die festgelegten Durchsatzeinheiten.
- Die Raten für ausgehende Datenanforderungen überschreiten die festgelegten Durchsatzeinheiten.

Der Event Hubs-Dienst erhöht den Durchsatz, wenn die Last den minimalen Schwellenwert überschreitet, ohne dass Anforderungen mit ServerBusy-Fehlern auftreten. 

Weitere Informationen zum Feature für die automatische Vergrößerung finden Sie unter [Automatisches Skalieren von Durchsatzeinheiten](event-hubs-auto-inflate.md).

## <a name="partitions"></a>Partitionen
[!INCLUDE [event-hubs-partitions](../../includes/event-hubs-partitions.md)]




## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Event Hubs finden Sie unter den folgenden Links:

- [Automatisches Skalieren von Durchsatzeinheiten](event-hubs-auto-inflate.md)
- [Event Hubs-Dienst: Übersicht](./event-hubs-about.md)
