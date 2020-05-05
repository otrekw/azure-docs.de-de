---
title: Beheben von Problemen mit Azure Event Hubs für Apache Kafka
description: In diesem Artikel erfahren Sie, wie Sie Probleme mit Azure Event Hubs für Apache Kafka behandeln.
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
ms.openlocfilehash: 12ddc5fa74b7a1b42bbd64fde9ec3410b1c1e425
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81606681"
---
# <a name="apache-kafka-troubleshooting-guide-for-event-hubs"></a>Apache Kafka-Leitfaden zur Problembehandlung für Event Hubs
In diesem Artikel finden Sie Tipps zur Problembehandlung, die bei der Verwendung von Event Hubs für Apache Kafka auftreten können. 

## <a name="server-busy-exception"></a>Ausnahme „Server ausgelastet“
Sie erhalten ggf. die Ausnahme „Server ausgelastet“ aufgrund der Kafka-Drosselung. Bei AMQP-Clients gibt Event Hubs sofort die Ausnahme **Server ausgelastet** zurück, sobald der Dienst gedrosselt wird. Dies entspricht der Meldung „versuchen Sie später erneut“. In Kafka werden Nachrichten verzögert, bevor sie abgeschlossen werden. Die Verzögerungslänge wird in Millisekunden als `throttle_time_ms` in der PRODUCE/FETCH-Antwort zurückgegeben. In den meisten Fällen werden diese verzögerten Anforderungen nicht als ServerBusy-Ausnahmen in den Event Hubs-Dashboards protokolliert. Stattdessen sollte der Wert `throttle_time_ms` der Antwort als Indikator dafür verwendet werden, dass der Durchsatz das vorgesehene Kontingent überschritten hat.

Wenn der Datenverkehr übermäßig hoch ist, weist der Dienst folgendes Verhalten auf:

- Wenn die Verzögerung der PRODUCE-Anforderung das Zeitlimit für Anforderungen überschreitet, gibt Event Hubs den Fehlercode **Richtlinienverletzung** zurück.
- Wenn die Verzögerung der FETCH-Anforderung das Zeitlimit für Anforderungen überschreitet, protokolliert Event Hubs die Anforderung als gedrosselt und antwortet mit einer leeren Gruppe von Datensätzen und ohne Fehlercode.

[Dedizierte Cluster](event-hubs-dedicated-overview.md) weisen keine Drosselungsmechanismen auf. Sie können alle Ihre Clusterressourcen nutzen.

## <a name="no-records-received"></a>Keine Datensätze empfangen
Es kann vorkommen, dass Consumer keine Datensätze empfangen und ständig einen Ausgleich vornehmen. In diesem Szenario empfangen Consumer keine Datensätze und nehmen ständig einen Ausgleich vor. Es tritt keine Ausnahme bzw. kein Fehler auf, wenn dies passiert, aber die Kafka-Protokolle zeigen, dass die Consumer beim Versuch hängen bleiben, der Gruppe wieder beizutreten und Partitionen zuzuweisen. Dafür gibt es verschiedene mögliche Ursachen:

- Stellen Sie sicher, dass Ihr Wert für `request.timeout.ms` mindestens dem empfohlenen Wert 60.000 und Ihr Wert für `session.timeout.ms` mindestens dem empfohlenen Wert 30.000 entspricht. Wenn diese Einstellungen zu niedrig sind, kann dies zu Zeitüberschreitungen von Consumern führen, die dann zu Ausgleichvorgängen führen (was dann zu mehr Zeitüberschreitungen führt, was wiederum zu mehr Ausgleichvorgängen führt usw.). 
- Wenn Ihre Konfiguration mit den empfohlenen Werten übereinstimmt und Sie immer noch ständige Ausgleichvorgänge feststellen, können Sie gerne ein Issue eröffnen (stellen Sie sicher, dass Ihre gesamte Konfiguration in das Issue aufgenommen wird, damit wir beim Debuggen helfen können).

## <a name="compressionmessage-format-version-issue"></a>Problem mit der Komprimierung/Nachrichtenformatversion
Kafka unterstützt die Komprimierung, Event Hubs für Kafka derzeit nicht. Fehler, die eine Nachrichtenformatversion (z. B. `The message format version on the broker does not support the request.`) erwähnen, werden verursacht, wenn ein Client versucht, komprimierte Kafka-Nachrichten an unsere Broker zu senden.

Wenn komprimierte Daten erforderlich sind, ist die Komprimierung Ihrer Daten vor dem Senden an die Broker und die Dekomprimierung nach dem Empfang eine zulässige Problemumgehung. Der Nachrichtentext ist lediglich ein Bytearray für den Dienst, sodass die Komprimierung/Dekomprimierung auf Clientseite keine Probleme verursacht.

## <a name="unknownserverexception"></a>UnknownServerException
Möglicherweise erhalten Sie eine UnknownServerException von Kafka-Clientbibliotheken, die dem folgenden Beispiel ähnelt: 

```
org.apache.kafka.common.errors.UnknownServerException: The server experienced an unexpected error when processing the request
```

Öffnen Sie ein Ticket beim Microsoft-Support.  Protokollierung auf Debugebene und Ausnahmezeitstempel in UTC sind beim Debuggen des Problems hilfreich. 

## <a name="other-issues"></a>Andere Probleme
Überprüfen Sie die folgenden Punkte, wenn Sie Probleme bei der Verwendung von Kafka in Event Hubs feststellen.

- **Durch Firewall blockierter Datenverkehr**: Stellen Sie sicher, dass Port **9093** nicht durch Ihre Firewall blockiert wird.
- **TopicAuthorizationException**: Es folgen die häufigsten Gründe für diese Ausnahme:
    - Ein Tippfehler in der Verbindungszeichenfolge in der Konfigurationsdatei
    - Versuch, Event Hubs für Kafka in einem Namespace im Tarif „Basic“ zu nutzen Event Hubs für Kafka wird [nur von Event Hubs in Namespaces in den Tarifen „Standard“ und „Dedicated“ unterstützt](https://azure.microsoft.com/pricing/details/event-hubs/).
- **Kafka-Versionskonflikt**: Event Hubs für Kafka Ecosystems unterstützt Kafka ab Version 1.0. Einige Anwendungen, die eine Kafka-Version ab 0.10 verwenden, könnten gelegentlich aufgrund der Abwärtskompatibilität des Kafka-Protokolls funktionieren. Wir raten jedoch dringend davon ab, alte API-Versionen zu verwenden. Kafka-Versionen bis 0.9 unterstützen die erforderlichen SASL-Protokolle nicht und können keine Verbindung mit Event Hubs herstellen.
- **Seltsame Codierungen in AMQP-Headern bei Verwendung mit Kafka**: Beim Senden von Ereignissen an eine Event Hub-Instanz über AMQP werden alle AMQP-Nutzlastheader in AMQP-Codierung serialisiert. Kafka-Consumer deserialisieren die Header nicht aus AMQP. Decodieren Sie die AMQP-Header manuell, um Headerwerte zu lesen. Alternativ können Sie AMQP-Header vermeiden, wenn Sie wissen, dass Sie Daten über das Kafka-Protokoll nutzen werden. Weitere Informationen finden Sie in [diesem GitHub-Problem](https://github.com/Azure/azure-event-hubs-for-kafka/issues/56).
- **SASL-Authentifizierung**: Ihr Framework dazu zu bringen, mit dem SASL-Authentifizierungsprotokoll zusammenzuarbeiten, das von Event Hubs benötigt wird, kann schwieriger sein, als es zunächst den Anschein hat. Versuchen Sie, ob Sie die Konfiguration mit den Ressourcen Ihres Frameworks zur SASL-Authentifizierung korrigieren können. 

## <a name="limits"></a>Einschränkungen
Apache Kafka im Vergleich zu Event Hubs für Kafka. Event Hubs für Kafka Ecosystems weist in den meisten Fällen die gleichen Standardeinstellungen, Eigenschaften, Fehlercodes und allgemeinen Verhaltensweisen auf wie Apache Kafka. Die Fälle, in denen sich diese beiden explizit unterscheiden (oder in denen Event Hubs einen Grenzwert vorschreiben, der von Kafka nicht eingehalten wird), sind nachstehend aufgeführt:

- Die maximale Länge der Eigenschaft `group.id` ist 256 Zeichen.
- Die maximale Größe von `offset.metadata.max.bytes` ist 1024 Bytes.
- Offsetcommits werden mit vier Aufrufen pro Sekunde pro Partition bei einer maximalen internen Protokollgröße von 1 MB gedrosselt.


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Event Hubs und Event Hubs für Kafka finden Sie in folgenden Artikeln:  

- [Apache Kafka-Entwicklerleitfaden für Event Hubs](apache-kafka-developer-guide.md)
- [Apache Kafka-Migrationsleitfaden für Event Hubs](apache-kafka-migration-guide.md)
- [Häufig gestellte Fragen zu Event Hubs für Apache Kafka](apache-kafka-frequently-asked-questions.md)
- [Empfohlene Konfigurationen](https://github.com/Azure/azure-event-hubs-for-kafka/blob/master/CONFIGURATION.md)
