---
title: Verwenden eines Event Hubs aus der Apache Kafka-App – Azure Event Hubs | Microsoft-Dokumentation
description: Dieser Artikel enthält Informationen zur Azure Event Hubs-Unterstützung für Apache Kafka.
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.custom: seodec18
ms.date: 02/12/2020
ms.author: shvija
ms.openlocfilehash: fc81226e754178ad0edfff96a494dd7522662261
ms.sourcegitcommit: f97f086936f2c53f439e12ccace066fca53e8dc3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/15/2020
ms.locfileid: "77368505"
---
# <a name="use-azure-event-hubs-from-apache-kafka-applications"></a>Verwenden von Azure Event Hubs aus Apache Kafka-Anwendungen
Event Hubs stellt einen Kafka-Endpunkt bereit, der von Ihren vorhandenen Kafka-basierten Anwendungen als Alternative zum Betreiben eines eigenen Kafka-Clusters verwendet werden kann. Event Hubs unterstützt [Apache Kafka-Protokoll 1.0 und höher](https://kafka.apache.org/documentation/) und funktioniert mit Ihren vorhandenen Kafka-Anwendungen, einschließlich MirrorMaker.  

## <a name="what-does-event-hubs-for-kafka-provide"></a>Was bietet Event Hubs für Kafka?

Event Hubs für Kafka bietet einen auf Azure Event Hubs aufbauenden Protokollheader, der binärkompatibel mit Kafka ab Version 1.0 ist – und zwar sowohl zum Lesen aus Kafka-Themen als auch zum Schreiben in diese. Sie können den Kafka-Endpunkt aus Ihrer Anwendung ohne Codeänderung, aber mit einer minimalen Konfigurationsänderung verwenden. Sie aktualisieren die Verbindungszeichenfolge in den Konfigurationen, um auf den Kafka-Endpunkt zu verweisen, der von Ihrem Event Hub bereitgestellt wird, anstatt auf Ihren Kafka-Cluster zu verweisen. Nun können Sie Ereignisse aus Ihren Anwendungen, die das Kafka-Protokoll verwenden, in Event Hubs streamen. Diese Integration unterstützt auch Frameworks wie [Kafka Connect](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/connect), das derzeit als Vorschauversion verfügbar ist. 

Konzeptionell sind Kafka und Event Hubs nahezu identisch: Es handelt sich bei beiden um partitionierte Protokolle für das Streaming von Daten. In der folgenden Tabelle werden die Konzepte von Kafka und Event Hubs einander zugeordnet.

### <a name="kafka-and-event-hub-conceptual-mapping"></a>Konzeptionelle Zuordnung zwischen Kafka und Event Hubs

| Kafka-Konzept | Event Hubs-Konzept|
| --- | --- |
| Cluster | Namespace |
| Thema | Event Hub |
| Partition | Partition|
| Consumergruppe | Consumergruppe |
| Offset | Offset|

### <a name="key-differences-between-kafka-and-event-hubs"></a>Hauptunterschiede zwischen Kafka und Event Hubs

Während es sich bei [Apache Kafka](https://kafka.apache.org/) um Software handelt, die Sie an jedem Speicherort ausführen können, ist Event Hubs ein Clouddienst ähnlich Azure Blob Storage. Es sind keine Server oder Netzwerke zu verwalten und keine Broker zu konfigurieren. Sie erstellen einen Namespace (einen FQDN, in dem sich Ihre Themen befinden) und erstellen Event Hubs oder Themen in diesem Namespace. Weitere Informationen zur Verwendung von Event Hubs und Namespaces finden Sie unter [Event Hubs-Features](event-hubs-features.md#namespace). Als Clouddienst verwendet Event Hubs eine einzelne stabile virtuelle IP-Adresse als Endpunkt, damit Clients keine Informationen über die Broker oder die Computer in einem Cluster benötigen. 

Die Skalierung in Event Hubs wird dadurch gesteuert, wie viele Durchsatzeinheiten Sie erwerben. Dabei berechtigt jede Durchsatzeinheit Sie zu 1 MB pro Sekunde oder 1.000 eingehenden Ereignissen pro Sekunde. Standardmäßig wird Event Hubs mit dem Feature [Automatische Vergrößerung](event-hubs-auto-inflate.md) über Durchsatzeinheiten zentral hochskaliert, wenn Sie Ihr Limit erreichen. Dieses Feature funktioniert auch mit Event Hubs für Kafka. 

### <a name="security-and-authentication"></a>Sicherheit und Authentifizierung
Jedes Mal, wenn Sie Ereignisse/Daten von einem Event Hub für Kafka veröffentlichen oder nutzen, versucht Ihr Client, auf die Event Hubs-Ressourcen zuzugreifen. Sie möchten sicherstellen, dass der Zugriff auf die Ressourcen durch eine autorisierte Entität erfolgt. Wenn Sie das Apache Kafka-Protokoll mit ihren Clients verwenden, können Sie die Konfiguration für Authentifizierung und Verschlüsselung mithilfe der SASL-Mechanismen festlegen. Wenn Sie Event Hubs für Kafka verwenden, ist die TLS-Verschlüsselung erforderlich (da alle Daten während der Übertragung an Event Hubs TLS-verschlüsselt sind). Sie können dies durch Angeben der Option SASL_SSL in Ihrer Konfigurationsdatei erreichen. 

Azure Event Hubs bietet mehrere Optionen, um den Zugriff auf Ihre sicheren Ressourcen zu autorisieren. 

- OAuth
- Shared Access Signature (SAS)

#### <a name="oauth"></a>OAuth
Event Hubs ist in Azure Active Directory (Azure AD) integriert, wodurch ein mit **OAuth** 2.0 kompatibler zentraler Autorisierungsserver bereitsteht. Mit Azure AD können Sie rollenbasierte Zugriffssteuerung (RBAC) verwenden, um Ihren Clientidentitäten fein differenzierte Berechtigungen zu erteilen. Sie können dieses Feature in Kombination mit Ihren Kafka-Clients verwenden, indem Sie **SASL_SSL** als Protokoll und **OAUTHBEARER** als Mechanismus angeben. Details zu den RBAC-Rollen und Ebenen, die den Umfang des Zugriffs definieren, finden Sie unter [Autorisieren des Zugriffs mit Azure AD](authorize-access-azure-active-directory.md).

```xml
bootstrap.servers=NAMESPACENAME.servicebus.windows.net:9093
security.protocol=SASL_SSL
sasl.mechanism=OAUTHBEARER
sasl.jaas.config=org.apache.kafka.common.security.oauthbearer.OAuthBearerLoginModule required;
sasl.login.callback.handler.class=CustomAuthenticateCallbackHandler;
```

#### <a name="shared-access-signature-sas"></a>Shared Access Signature (SAS)
Event Hubs bietet darüber hinaus **Shared Access Signatures (SAS)** für den delegierten Zugriff auf Event Hubs für Kafka-Ressourcen. Die Autorisierung des Zugriffs mithilfe des tokenbasierten OAuth 2.0-Mechanismus sorgt für überlegene Sicherheit und einfache Verwendung im Vergleich mit SAS. Durch die integrierten Rollen kann außerdem die Notwendigkeit einer ACL-basierten Autorisierung entfallen, die vom Benutzer gewartet und verwaltet werden muss. Sie können dieses Feature in Kombination mit Ihren Kafka-Clients verwenden, indem Sie **SASL_SSL** als Protokoll und **PLAIN** als Mechanismus angeben. 

```xml
bootstrap.servers=NAMESPACENAME.servicebus.windows.net:9093
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

#### <a name="samples"></a>Beispiele 
Ein **Tutorial** mit Schritt-für-Schritt-Anleitungen zum Erstellen eines Kafka-fähigen Event Hubs und den Zugriff darauf mithilfe von SAS oder OAuth finden Sie unter [Schnellstart: Datenstreaming mit Event Hubs mithilfe des Kafka-Protokolls](event-hubs-quickstart-kafka-enabled-event-hubs.md).

Weitere **Beispiele**, die die Verwendung von OAuth mit Event Hubs für Kafka veranschaulichen, finden Sie unter [Beispiele auf GitHub](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/oauth).

## <a name="other-event-hubs-features-available-for-kafka"></a>Andere verfügbare Event Hubs-Features für Kafka

Event Hubs für Kafka ermöglicht Ihnen, mit einem Protokoll zu schreiben und mit einem anderen zu lesen, sodass Ihre aktuellen Kafka-Produzenten weiterhin über Kafka veröffentlichen können. Sie können außerdem Leser mit Event Hubs hinzufügen, z.B. Azure Stream Analytics oder Azure Functions. Darüber hinaus funktionieren Event Hubs-Features wie das [Erfassen](event-hubs-capture-overview.md) und die [geografische Notfallwiederherstellung](event-hubs-geo-dr.md) auch mit Event Hubs für Kafka.

## <a name="features-that-are-not-yet-supported"></a>Noch nicht unterstützte Features 

Aufstellung der Kafka-Features, die noch nicht unterstützt werden:

*   Idempotenter Produzent
*   Transaktion
*   Komprimierung
*   Größenbasierte Aufbewahrung
*   Protokollkomprimierung
*   Hinzufügen von Partitionen zu einem vorhandenen Thema
*   Unterstützung der HTTP-Kafka-API
*   Kafka Streams

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde eine Einführung in Event Hubs für Kafka vermittelt. Weitere Informationen finden Sie unter den folgenden Links:

- [Erstellen von Kafka-fähigen Event Hubs](event-hubs-create-kafka-enabled.md)
- [Streamen von Ereignissen in Event Hubs aus Kafka-Anwendungen](event-hubs-quickstart-kafka-enabled-event-hubs.md)
- [Spiegeln eines Kafka-Brokers in einem Kafka-fähigen Event Hub](event-hubs-kafka-mirror-maker-tutorial.md)
- [Verbinden von Apache Spark mit einem Kafka-fähigen Event Hub](event-hubs-kafka-spark-tutorial.md)
- [Verbinden von Apache Flink mit einem Kafka-fähigen Event Hub](event-hubs-kafka-flink-tutorial.md)
- [Integrieren von Kafka Connect in einen Kafka-fähigen Event Hub](event-hubs-kafka-connect-tutorial.md)
- [Verbinden von Akka Streams mit einem Kafka-fähigen Event Hub](event-hubs-kafka-akka-streams-tutorial.md)
- [Erkunden von Beispielen auf GitHub](https://github.com/Azure/azure-event-hubs-for-kafka)


