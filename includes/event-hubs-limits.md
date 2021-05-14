---
title: include file description: include file services: event-hubs author: spelluru ms.service: event-hubs ms.topic: include ms.date: 03/31/2021 ms.author: spelluru ms.custom: "include file","fasttrack-edit","iot","event-hubs"

---

In der folgenden Tabelle finden Sie die Kontingente und Grenzwerte, die für [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) gelten. Informationen zu den Preisen von Event Hubs finden Sie unter [Event Hubs – Preise](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="common-limits-for-all-tiers"></a>Allgemeine Grenzwerte für alle Ebenen
Die folgenden Grenzwerte sind gängig für alle Ebenen: 

| Begrenzung |  Notizen | Wert |
| --- |  --- | --- |
 Größe eines Event Hub-Namens |- | 256 Zeichen |
| Größe eines Consumergruppennamens | Das Kafka-Protokoll erfordert nicht die Erstellung einer Consumergruppe. | <p>Kafka: 256 Zeichen</p><p>AMQP: 50 Zeichen |
| Anzahl nicht epochenbezogener Empfänger pro Consumergruppe |- |5 |
| Anzahl von Autorisierungsregeln pro Namespace | Nachfolgende Anforderungen zur Erstellung von Autorisierungsregeln werden abgelehnt.|12 |
| Die Anzahl der Aufrufe der GetRuntimeInformation-Methode |  - | 50 pro Sekunde | 
| Anzahl von virtuellen Netzwerken (VNETs) | - | 128 | 
| Anzahl von IP-Konfigurationsregeln | - | 128 | 
| Maximale Länge eines Schemagruppennamens | | 50 |  
| Maximale Länge eines Schemanamens | | 100 |    
| Größe in Bytes pro Schema | | 1 MB |   
| Anzahl von Eigenschaften pro Schemagruppe | | 1024 |
| Größe in Bytes pro Schemagruppen-Eigenschaftenschlüssel | | 256 | 
| Größe in Bytes pro Schemagruppen-Eigenschaftenwert | | 1024 | 

### <a name="basic-vs-standard-vs-dedicated-tiers"></a>Vergleich der Tarife Basic, Standard und Dedicated (Dediziert)
Die folgende Tabelle enthält die Grenzwerte, die sich für die Tarife „Basic“, „Standard“ und „Dedicated“ unterscheiden können. In der Tabelle steht CU für [Kapazitätseinheit](../articles/event-hubs/event-hubs-dedicated-overview.md) und TU für [Durchsatzeinheit](../articles/event-hubs/event-hubs-faq.yml#what-are-event-hubs-throughput-units-). 

| Begrenzung | Basic | Standard | Dediziert |
| ----- | ----- | -------- | -------- | 
| Maximale Größe der Event Hubs-Veröffentlichung | 256 KB | 1 MB | 1 MB |
| Anzahl von Consumergruppen pro Event Hub | 1 | 20 | Kein Limit pro CU, 1000 pro Event Hub |
| Anzahl von AMQP-Verbindungen pro Namespace | 100 | 5\.000 | 100.000 enthalten, gleichzeitig Max. |
| Maximale Aufbewahrungsdauer von Ereignisdaten | 1 Tag | 1–7 Tage | 90 Tage, 10 TB enthalten pro CU |
| Maximale Anzahl von TUs oder CUs |20 TUs | 20 TUs | 20 CUs |
| Anzahl von Partitionen pro Event Hub | 32 | 32 | 1\.024 pro Event Hub<br/> 2000 pro CU |
| Anzahl von Namespaces pro Abonnement | 100 | 100 | 100 (50 pro CU) |
| Anzahl von Event Hubs pro Namespace | 10 | 10 | 1000 |
| Eingangsereignisse | | Bezahlung pro Million Ereignisse | Enthalten|
| Erfassung | – | Bezahlung pro Stunde | Enthalten |
| Größe der Schemaregistrierung (Namespace) in Megabytes | – | 25 |  1024 |
| Anzahl von Schemagruppen in einer Schemaregistrierung oder einem Namespace | – | 1 (ohne Standardgruppe) | 1000 |
| Anzahl von Schemaversionen in allen Schemagruppen | – | 25 | 10000 |

> [!NOTE]
> Sie können Ereignisse einzeln oder als Batch veröffentlichen. Der Veröffentlichungsgrenzwert gilt (je nach SKU) unabhängig davon, ob es sich um ein einzelnes Ereignis oder einen Batch handelt. Das Veröffentlichen von Ereignissen, die größer als der maximale Schwellenwert sind, wird abgelehnt.

