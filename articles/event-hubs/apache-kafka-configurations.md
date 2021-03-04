---
title: 'Empfohlene Konfigurationen für Apache Kafka-Clients: Azure Event Hubs'
description: Dieser Artikel enthält empfohlene Apache Kafka-Konfigurationen für Clients, die mit Azure Event Hubs für Apache Kafka interagieren.
ms.topic: reference
ms.date: 03/03/2021
ms.openlocfilehash: be009aae41b2cb26ab02fdbe14bc4e18311ad235
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102042350"
---
# <a name="recommended-configurations-for-apache-kafka-clients"></a>Empfohlene Konfigurationen für Apache Kafka-Clients
Hier finden Sie die empfohlenen Konfigurationen für die Verwendung von Azure Event Hubs aus Apache Kafka-Clientanwendungen. 

## <a name="java-client-configuration-properties"></a>Java-Clientkonfigurationseigenschaften

### <a name="producer-and-consumer-configurations"></a>Producer- und Consumerkonfigurationen

Eigenschaft | Empfohlene Werte | Zulässiger Bereich | Notizen
---|---:|-----:|---
`metadata.max.age.ms` | 180.000 (ungefähr) | < 240.000 | Kann verringert werden, um Metadatenänderungen früher zu übernehmen.
`connections.max.idle.ms`   | 180.000 | < 240.000 | Azure schließt eingehende TCP-Verbindungen, die sich länger als 240.000 ms im Leerlauf befinden, was zum Senden über stillgelegte Verbindungen führen kann (wird aufgrund des Sendetimeouts als abgelaufene Batches angezeigt).

### <a name="producer-configurations-only"></a>Nur Producerkonfigurationen
Producerkonfigurationen finden Sie [hier](https://kafka.apache.org/documentation/#producerconfigs).

Eigenschaft | Empfohlene Werte | Zulässiger Bereich | Notizen
---|---:|---:|---
`max.request.size` | 1000000 | < 1.046.528 | Der Dienst schließt Verbindungen, wenn Anforderungen gesendet werden, die größer als 1.046.528 Bytes sind.  *Dieser Wert **muss** geändert werden und führt zu Problemen bei Szenarien mit hohem Durchsatz.*
`retries` | > 0 | | Erfordert möglicherweise eine Erhöhung des delivery.timeout.ms-Werts. Weitere Informationen finden Sie in der Dokumentation.
`request.timeout.ms` | 30.000 60000 | > 20.000| EH wird intern standardmäßig auf mindestens 20.000 ms festgelegt.  *Obwohl Anforderungen mit niedrigeren Timeoutwerten akzeptiert werden, wird das Clientverhalten nicht garantiert.*
`metadata.max.idle.ms` | 180.000 | > 5.000 | Steuert, wie lange der Producer Metadaten für ein Thema zwischenspeichert, das sich im Leerlauf befindet. Wenn die Zeit, die seit der letzten Erstellung eines Themas verstrichen ist, die Leerlaufzeit der Metadaten überschreitet, werden die Metadaten des Themas verworfen, und der nächste Zugriff darauf erzwingt eine Anforderung zum Abrufen der Metadaten.
`linger.ms` | > 0 | | Bei Szenarien mit hohem Durchsatz sollte der Verweilwert gleich dem höchsten tolerablen Wert sein, um Batchverarbeitung zu nutzen.
`delivery.timeout.ms` | | | Wird gemäß der Formel (`request.timeout.ms` + `linger.ms`) * `retries` festgelegt.
`compression.type` | `none` | | Komprimierung wird derzeit nicht unterstützt.

### <a name="consumer-configurations-only"></a>Nur Consumerkonfigurationen
Consumerkonfigurationen finden Sie [hier](https://kafka.apache.org/documentation/#consumerconfigs).

Eigenschaft | Empfohlene Werte | Zulässiger Bereich | Notizen
---|---:|-----:|---
`heartbeat.interval.ms` | 3000 | | 3\.000 ist der Standardwert und sollte nicht geändert werden.
`session.timeout.ms` | 30.000 |6\.000 300000| Beginnen Sie mit 30.000, und vergrößern Sie den Wert bei einem häufigen Ausgleich aufgrund von verpassten Takten.


## <a name="librdkafka-configuration-properties"></a>librdkafka-Konfigurationseigenschaften
Die `librdkafka`-Hauptkonfigurationsdatei ([Link](https://github.com/edenhill/librdkafka/blob/master/CONFIGURATION.md)) enthält erweiterte Beschreibungen für die unten aufgeführten Eigenschaften.

### <a name="producer-and-consumer-configurations"></a>Producer- und Consumerkonfigurationen

Eigenschaft | Empfohlene Werte | Zulässiger Bereich | Notizen
---|---:|-----:|---
`socket.keepalive.enable` | true | | Erforderlich, wenn erwartet wird, dass die Verbindung im Leerlauf ist.  Azure schließt eingehende TCP-Verbindungen,die länger als 240.000 ms im Leerlauf sind.
`metadata.max.age.ms` | ~ 180.000| < 240.000 | Kann verringert werden, um Metadatenänderungen früher zu übernehmen.

### <a name="producer-configurations-only"></a>Nur Producerkonfigurationen

Eigenschaft | Empfohlene Werte | Zulässiger Bereich | Notizen
---|---:|-----:|---
`retries` | > 0 | | Standard ist 2. Es wird empfohlen, diesen Wert beizubehalten. 
`request.timeout.ms` | 30.000 60000 | > 20.000| EH wird intern standardmäßig auf mindestens 20.000 ms festgelegt.  Der `librdkafka`-Standardwert ist 5.000. Dies kann problematisch sein. *Obwohl Anforderungen mit niedrigeren Timeoutwerten akzeptiert werden, wird das Clientverhalten nicht garantiert.*
`partitioner` | `consistent_random` | Weitere Informationen finden Sie in der librdkafka-Dokumentation. | `consistent_random` ist Standard und am besten geeignet.  Leere und NULL-Schlüssel werden in den meisten Fällen ideal behandelt.
`compression.codec` | `none` || Komprimierung wird derzeit nicht unterstützt.

### <a name="consumer-configurations-only"></a>Nur Consumerkonfigurationen

Eigenschaft | Empfohlene Werte | Zulässiger Bereich | Notizen
---|---:|-----:|---
`heartbeat.interval.ms` | 3000 || 3\.000 ist der Standardwert und sollte nicht geändert werden.
`session.timeout.ms` | 30.000 |6\.000 300000| Beginnen Sie mit 30.000, und vergrößern Sie den Wert bei einem häufigen Ausgleich aufgrund von verpassten Takten.


## <a name="further-notes"></a>Weitere Hinweise

Überprüfen Sie die folgende Tabelle mit allgemeinen konfigurationsbezogenen Fehlerszenarien.

Symptome | Problem | Lösung
----|---|-----
Offsetcommitfehler aufgrund eines erneuten Ausgleichs | Der Consumer wartet zu lange zwischen Aufrufen von poll(), und der Dienst entfernt den Consumer aus der Gruppe. | Sie haben mehrere Möglichkeiten: <ul><li>Erhöhen des Timeouts für die Abrufverarbeitung (`max.poll.interval.ms`)</li><li>Verkleinern der Nachrichtenbatchgröße, um die Verarbeitung zu beschleunigen</li><li>Verbessern der Verarbeitungsparallelisierung, um das Blockieren von consumer.poll() zu vermeiden</li></ul> Das Anwenden einer Kombination dieser drei Optionen ist wahrscheinlich am sinnvollsten.
Netzwerkausnahmen bei hohem Producerdurchsatz | Verwenden Sie einen Java-Client und den Standardwert von max.request.size?  Ihre Anforderungen sind möglicherweise zu groß. | Weitere Informationen finden Sie oben unter den Java-Konfigurationen.

## <a name="next-steps"></a>Nächste Schritte
Informationen zu Kontingenten und Einschränkungen finden Sie unter [Einschränkungen für Azure-Abonnements und -Dienste, Kontingente und Einschränkungen](..//azure-resource-manager/management/azure-subscription-service-limits.md). 
