---
title: Behandeln häufiger Fehler in der Cassandra-API von Azure Cosmos DB
description: In diesem Dokument erfahren Sie, wie Sie häufige Probleme in der Cassandra-API von Azure Cosmos DB behandeln.
author: TheovanKraay
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: troubleshooting
ms.date: 12/01/2020
ms.author: thvankra
ms.openlocfilehash: f5f2cb5ac8c354df38310cdcb47b98e1da5b6cfa
ms.sourcegitcommit: 66479d7e55449b78ee587df14babb6321f7d1757
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/15/2020
ms.locfileid: "97517698"
---
# <a name="troubleshoot-common-issues-in-azure-cosmos-db-cassandra-api"></a>Behandeln häufiger Probleme in der Cassandra-API von Azure Cosmos DB
[!INCLUDE[appliesto-cassandra-api](includes/appliesto-cassandra-api.md)]

Die Cassandra-API in Azure Cosmos DB ist eine Kompatibilitätsebene, die [Wire Protocol-Unterstützung](cassandra-support.md) für die beliebte Open-Source-Datenbank Apache Cassandra bereitstellt und von [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction) unterstützt wird. Als vollständig verwalteter cloudnativer Dienst bietet Azure Cosmos DB [Garantien für Verfügbarkeit, Durchsatz und Konsistenz](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_3/) für die Cassandra-API. Diese Garantien sind für Legacyimplementierungen von Apache Cassandra nicht möglich. Außerdem ermöglicht die Cassandra-API die Ausführung von Plattformvorgängen ohne Wartung und das Patchen ohne Ausfallzeiten. Daher unterscheiden sich viele Back-End-Vorgänge von Apache Cassandra. Aus diesem Grund werden bestimmte Einstellungen und Vorgehensweisen empfohlen, um häufige Fehler zu vermeiden. 

In diesem Artikel werden häufige Fehler und deren Lösungen für Anwendungen beschrieben, die die Cassandra-API von Azure Cosmos DB nutzen.

## <a name="common-errors-and-solutions"></a>Häufige Fehler und Lösungen

| Fehler               |  BESCHREIBUNG             | Lösung  |
|---------------------|--------------------------|-----------|
| OverloadedException (Java) | Die Gesamtanzahl der verbrauchten Anforderungseinheiten übersteigt die bereitgestellten Anforderungseinheiten für den Keyspace oder die Tabelle. Daher werden die Anforderungen gedrosselt. | Skalieren Sie den Durchsatz, der einem Keyspace oder einer Tabelle zugewiesen ist, im Azure-Portal ([hier](manage-scale-cassandra.md) finden Sie weitere Informationen zu Skalierungsvorgängen in der Cassandra-API), oder implementieren Sie eine Wiederholungsrichtlinie. Informationen zu Java finden Sie unter den Beispielen für Wiederholungslogik für den [v3.x-Treiber](https://github.com/Azure-Samples/azure-cosmos-cassandra-java-retry-sample) und den [v4.x-Treiber](https://github.com/Azure-Samples/azure-cosmos-cassandra-java-retry-sample-v4). Weitere Informationen finden Sie auch unter [Azure Cosmos Cassandra Extensions for Java](https://github.com/Azure/azure-cosmos-cassandra-extensions) (Cassandra-Erweiterungen für Java in Azure Cosmos DB). |
| OverloadedException (Java) auch mit ausreichendem Durchsatz | Das System scheint Anforderungen zu drosseln, obwohl ausreichend Durchsatz für das Anforderungsvolumen und/oder die verbrauchten Anforderungseinheiten bereitgestellt wurde.  | Die Cassandra-API implementiert ein Budget für den Systemdurchsatz bei Vorgängen auf Schemaebene (CREATE TABLE, ALTER TABLE, DROP TABLE). Dieses Budget sollte für Schemavorgänge in einem Produktionssystem ausreichen. Bei einer sehr großen Anzahl von Vorgängen auf Schemaebene ist es jedoch möglich, dass dieser Grenzwert überschritten wird. Da dieses Budget nicht vom Benutzer gesteuert wird, müssen Sie eventuell die Anzahl der ausgeführten Schemavorgänge verringern. Wenn das Problem dadurch nicht behoben werden kann oder diese Aktion für Ihre Workload nicht möglich ist, [erstellen Sie eine Azure-Supportanfrage](../azure-portal/supportability/how-to-create-azure-support-request.md).|
| ClosedConnectionException (Java) | Nach dem erfolgreichen Herstellen einer Verbindungen kann die Anwendung nach einer Leerlaufzeit keine Verbindung mehr herstellen.| Dieser Fehler kann auf dem Leerlauftimeout von Azure Load Balancer zurückzuführen sein, das vier Minuten beträgt. Legen Sie die Keepalive-Einstellung im Treiber (siehe unten) fest, und erhöhen Sie die Keepalive-Einstellungen im Betriebssystem. Sie können auch das [Leerlauftimeout in Azure Load Balancer](../load-balancer/load-balancer-tcp-idle-timeout.md?tabs=tcp-reset-idle-portal) anpassen. |
| Andere zeitweilige Verbindungsfehler (Java) | Die Verbindung wird unerwartet getrennt oder läuft aus | Die Apache Cassandra-Treiber für Java bieten zwei native Richtlinien für die erneute Verbindungsherstellung: `ExponentialReconnectionPolicy` und `ConstantReconnectionPolicy`. Der Standardwert ist `ExponentialReconnectionPolicy`. Für die Cassandra-API von Azure Cosmos DB wird jedoch `ConstantReconnectionPolicy` mit einer Verzögerung von 2 Sekunden empfohlen. Weitere Informationen zum Java v4.x-Treiber finden Sie in der [Dokumentation des Treibers](https://docs.datastax.com/developer/java-driver/4.9/manual/core/reconnection/) und zu Java 3.x in [diesem Leitfaden](https://docs.datastax.com/developer/java-driver/3.7/manual/reconnection/) (sowie in den Beispielen unten).|

Wenn der Fehler oben nicht aufgeführt ist und beim Ausführen eines [unterstützten Vorgangs in der Cassandra-API](cassandra-support.md) ein Fehler auftritt, der bei der Verwendung von *nativem Apache Cassandra nicht auftritt*, [erstellen Sie eine Azure-Supportanfrage](../azure-portal/supportability/how-to-create-azure-support-request.md).

## <a name="configuring-reconnectionpolicy-for-java-driver"></a>Konfigurieren einer Richtlinie für Verbindungswiederholungen für Java-Treiber

### <a name="version-3x"></a>Version 3.x

Bei Version 3.x des Java-Treibers konfigurieren Sie die Richtlinie für Verbindungswiederholungen bei der Erstellung eines Clusterobjekts:

```java
import com.datastax.driver.core.policies.ConstantReconnectionPolicy;

Cluster.builder()
  .withReconnectionPolicy(new ConstantReconnectionPolicy(2000))
  .build();
```

### <a name="version-4x"></a>Version 4.x

Bei Version 4.x des Java-Treibers konfigurieren Sie die Richtlinie für Verbindungswiederholungen durch Überschreiben der Einstellungen in der Datei `reference.conf`:

```xml
datastax-java-driver {
  advanced {
    reconnection-policy{
      # The driver provides two implementations out of the box: ExponentialReconnectionPolicy and
      # ConstantReconnectionPolicy. We recommend ConstantReconnectionPolicy for Cassandra API, with 
      # base-delay of 2 seconds.
      class = ConstantReconnectionPolicy
      base-delay = 2 second
    }
}
```

## <a name="enable-keep-alive-for-java-driver"></a>Aktivieren der Keepalive-Einstellung für Java-Treiber

### <a name="version-3x"></a>Version 3.x

Bei Version 3.x des Java-Treibers legen Sie die Keepalive-Einstellung beim Erstellen eines Clusterobjekts fest. Sie müssen darüber hinaus auch sicherstellen, dass Keepalive [im Betriebssystem aktiviert](https://knowledgebase.progress.com/articles/Article/configure-OS-TCP-KEEPALIVE-000080089) ist:

```java
import java.net.SocketOptions;
    
SocketOptions options = new SocketOptions();
options.setKeepAlive(true);
cluster = Cluster.builder().addContactPoints(contactPoints).withPort(port)
  .withCredentials(cassandraUsername, cassandraPassword)
  .withSocketOptions(options)
  .build();
```

### <a name="version-4x"></a>Version 4.x

Bei Version 4.x des Java-Treibers legen Sie die Keepalive-Einstellung durch Überschreiben der Einstellungen in `reference.conf` fest. Sie müssen darüber hinaus auch sicherstellen, dass Keepalive [im Betriebssystem aktiviert](https://knowledgebase.progress.com/articles/Article/configure-OS-TCP-KEEPALIVE-000080089) ist:

```xml
datastax-java-driver {
  advanced {
    socket{
      keep-alive = true
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über die [unterstützten Features](cassandra-support.md) der Cassandra-API von Azure Cosmos DB.
- Erfahren Sie, wie Sie [vom nativen Apache Cassandra zur Cassandra-API von Azure Cosmos DB migrieren](cassandra-migrate-cosmos-db-databricks.md).

