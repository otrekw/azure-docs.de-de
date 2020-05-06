---
title: Häufig gestellte Fragen zu Event Hubs für Apache Kafka
description: In diesem Artikel wird erläutert, wie Consumer und Producer, die unterschiedliche Protokolle verwenden (AMQP, Apache Kafka und HTTPS), über Azure Event Hubs Ereignisse austauschen können.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/01/2020
ms.author: shvija
ms.openlocfilehash: 0186b90e1d75c5dba6e1ca26e4ba079a3456cea4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81606685"
---
# <a name="frequently-asked-questions---event-hubs-for-apache-kafka"></a>Häufig gestellte Fragen zu Event Hubs für Apache Kafka 
Dieser Artikel enthält Antworten auf einige häufig gestellte Fragen zum Migrieren zu Event Hubs für Apache Kafka.

## <a name="do-you-run-apache-kafka"></a>Führen Sie Apache Kafka aus?

Nein.  Wir wenden Kafka-API-Vorgänge auf Event Hubs-Infrastruktur an.  Da es eine enge Korrelation zwischen Apache Kafka- und AMQP-Funktionalität von Event Hubs gibt (d. h. „produce“, „receive“, „management“ usw.), können wir die bekannte Zuverlässigkeit von Event Hubs in den PAAS-Bereich von Kafka einbringen.

## <a name="event-hubs-consumer-group-vs-kafka-consumer-group"></a>Event Hubs-Consumergruppe im Vergleich zu Kafka-Consumergruppe
Worin besteht der Unterschied zwischen einer Event Hub-Consumergruppe und einer Kafka-Consumergruppe in Event Hubs? Kafka-Consumergruppen in Event Hubs unterscheiden sich grundlegend von standardmäßigen Event Hubs-Consumergruppen.

**Event Hubs-Consumergruppen**

- Diese Gruppen werden über das Portal, SDK oder Azure Resource Manager-Vorlagen erstellt, abgerufen, aktualisiert und gelöscht. Event Hubs-Consumergruppen lassen sich nicht automatisch erstellen.
- Es handelt sich um untergeordnete Entitäten einer Event Hub-Instanz. Dies bedeutet, dass derselbe Consumergruppenname unter Event Hubs im gleichen Namespace wiederverwendet werden kann, da es sich um separate Entitäten handelt.
- Sie werden nicht zum Speichern von Offsets verwendet. Die orchestrierte AMQP-Nutzung erfolgt über externen Offsetspeicher, z. B. Azure Storage.

**Kafka-Consumergruppen**

- Werden automatisch erstellt.  Kafka-Gruppen können über die Kafka-Consumergruppen-APIs verwaltet werden.
- Sie können Offsets im Event Hubs-Dienst speichern.
- Sie dienen als Schlüssel in einer Art Schlüssel-Wertspeicher für Offsets. Für ein eindeutiges Paar von `group.id` und `topic-partition` speichern wir einen Offset in Azure Storage (Dreifachreplikation). Event Hubs-Benutzern entstehen durch die Speicherung von Kafka-Offsets keine zusätzlichen Speicherkosten. Offsets sind über die APIs der Kafka-Consumergruppe bearbeitbar, aber die Offsetspeicher*konten* sind für Event Hub-Benutzer nicht direkt sichtbar oder bearbeitbar.  
- Sie umfassen einen Namespace. Das Verwenden desselben Kafka-Gruppennamens für mehrere Anwendungen zu mehreren Themen bedeutet, dass alle Anwendungen und ihre Kafka-Clients immer dann neu ausgeglichen werden, wenn nur eine einzige Anwendung neu ausgeglichen werden muss.  Wählen Sie Ihre Gruppennamen wohlüberlegt.
- Sie unterscheiden sich gänzlich von Event Hubs Consumergruppen. Sie **müssen weder**  '$Default' verwenden, noch müssen Sie sich Sorgen machen, dass Kafka-Clients AMQP-Workloads stören.
- Sie sind im Azure-Portal nicht einsehbar. Informationen zu Consumergruppen sind über Kafka-APIs verfügbar.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Event Hubs und Event Hubs für Kafka finden Sie in folgenden Artikeln:  

- [Apache Kafka-Entwicklerleitfaden für Event Hubs](apache-kafka-developer-guide.md)
- [Apache Kafka-Migrationsleitfaden für Event Hubs](apache-kafka-migration-guide.md)
- [Apache Kafka-Leitfaden zur Problembehandlung für Event Hubs](apache-kafka-troubleshooting-guide.md)
- [Empfohlene Konfigurationen](https://github.com/Azure/azure-event-hubs-for-kafka/blob/master/CONFIGURATION.md)

