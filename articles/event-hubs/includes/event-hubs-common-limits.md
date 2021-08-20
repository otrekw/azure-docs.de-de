---
title: include file description: include file services: event-hubs author: spelluru ms.service: event-hubs ms.topic: include ms.date: 05/10/2021 ms.author: spelluru ms.custom: "include file","fasttrack-edit","iot","event-hubs"

---

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

