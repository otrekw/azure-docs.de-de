---
title: Skalierbarkeit – Azure Event Hubs | Microsoft-Dokumentation
description: Dieser Artikel enthält Informationen zum Skalieren von Azure Event Hubs mithilfe von Partitionen und Durchsatzeinheiten.
ms.topic: article
ms.date: 03/16/2021
ms.openlocfilehash: acfc300ff7e6bd77c570c68bda50a731df10409c
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110367085"
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

## <a name="processing-units"></a>Verarbeitungseinheiten

 [Event Hubs Premium](./event-hubs-premium-overview.md) bietet eine bessere Leistung und bessere Isolation in einer verwalteten mehrinstanzenfähigen PaaS-Umgebung. Die Ressourcen in einem Premium-Tarif werden auf CPU- und Arbeitsspeicherebene isoliert, sodass jede Mandantenworkload isoliert ausgeführt wird. Dieser Ressourcencontainer wird als *Verarbeitungseinheit* (Processing Unit, PU) bezeichnet. Sie können 1, 2, 4, 8 oder 16 Verarbeitungseinheiten für jeden Event Hubs Premium-Namespace erwerben. 

Welche Datenmengen Sie mit einer Verarbeitungseinheit erfassen und streamen können, hängt von verschiedenen Faktoren ab, z. B. von den Producern, den Consumern oder der Rate für die Erfassung und Verarbeitung. Eine Verarbeitungseinheit kann ungefähr eine Kernkapazität von ca. 5 bis 10 MB/s eingehend und 10 bis 20 MB/s ausgehend bieten. Dies setzt voraus, dass genügend Partitionen vorhanden sind, sodass der Speicher keinen Drosselungsfaktor darstellt.  



## <a name="partitions"></a>Partitionen
[!INCLUDE [event-hubs-partitions](../../includes/event-hubs-partitions.md)]




## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Event Hubs finden Sie unter den folgenden Links:

- [Automatisches Skalieren von Durchsatzeinheiten](event-hubs-auto-inflate.md)
- [Event Hubs-Dienst: Übersicht](./event-hubs-about.md)
