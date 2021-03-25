---
title: 'Best Practices für die App-Entwicklung: Azure Database for PostgreSQL'
description: Erfahren Sie mehr über bewährte Methoden für die Entwicklung einer App mit Azure Database for PostgreSQL.
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.topic: conceptual
ms.date: 12/10/2020
ms.openlocfilehash: 6463f30bc79d937bd5a51a5c8c78fbdd72954b1e
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "97364537"
---
# <a name="best-practices-for-building-an-application-with-azure-database-for-postgresql"></a>Bewährte Methoden für die Entwicklung von Anwendungen mit Azure Database for PostgreSQL

Im Folgenden finden Sie einige bewährte Methoden, die Ihnen beim Erstellen einer cloudfähigen Anwendung mithilfe von Azure Database for PostgreSQL helfen sollen. Diese bewährten Methoden können die Entwicklungszeit für Ihre App verkürzen.

## <a name="configuration-of-application-and-database-resources"></a>Konfiguration von Anwendungs- und Datenbankressourcen

### <a name="keep-the-application-and-database-in-the-same-region"></a>Anwendung und Datenbank in derselben Region
Stellen Sie sicher, dass sich sämtliche Abhängigkeiten in derselben Region befinden, wenn Sie Ihre Anwendung in Azure bereitstellen. Wenn Sie die Instanzen auf verschiedene Regionen oder Verfügbarkeitszonen verteilen, führt dies zu Netzwerklatenzen, die sich auf die Gesamtleistung Ihrer Anwendung auswirken können.

### <a name="keep-your-postgresql-server-secure"></a>Gewährleisten der Sicherheit Ihres PostgreSQL-Servers
Ihr PostgreSQL-Server sollte [sicher](./concepts-security.md) konfiguriert und nicht öffentlich zugänglich sein. Verwenden Sie eine der folgenden Optionen, um Ihren Server zu schützen:
- [Firewallregeln](./concepts-firewall-rules.md)
- [Virtuelle Netzwerke](./concepts-data-access-and-security-vnet.md)
- [Azure Private Link](./concepts-data-access-and-security-private-link.md)

Aus Sicherheitsgründen müssen Sie Verbindungen mit Ihrem PostgreSQL-Server immer über SSL herstellen und den PostgreSQL-Server und Ihre Anwendung für die Verwendung von TLS 1.2 konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren von SSL/TLS](./concepts-ssl-connection-security.md).

### <a name="tune-your-server-parameters"></a>Optimieren der Serverparameter
Bei Workloads mit sehr vielen Lesevorgängen kann das Optimieren der Serverparameter `tmp_table_size` und `max_heap_table_size` zu einer besseren Leistung beitragen. Um die für diese Variablen erforderlichen Werte zu berechnen, betrachten Sie die Werte für den Gesamtarbeitsspeicher pro Verbindung und den Basisarbeitsspeicher. Die Summe der Parameter für den Arbeitsspeicher pro Verbindung ohne `tmp_table_size` plus Basisarbeitsspeicher ergibt den Gesamtarbeitsspeicher des Servers.

### <a name="use-environment-variables-for-connection-information"></a>Verwenden von Umgebungsvariablen für Verbindungsinformationen
Speichern Sie Datenbank-Anmeldeinformationen nicht in Ihrem Anwendungscode. Befolgen Sie abhängig von der Front-End-Anwendung die Anweisungen zum Einrichten von Umgebungsvariablen. Informationen zur Verwendung von App Service finden Sie unter [Konfigurieren von App-Einstellungen](../app-service/configure-common.md#configure-app-settings), und Informationen für den Azure Kubernetes-Dienst finden Sie unter [Secrets](https://kubernetes.io/docs/concepts/configuration/secret/) (Geheimnisse).

## <a name="performance-and-resiliency"></a>Leistung und Resilienz
Im Folgenden finden Sie einige Tools und Verfahren, mit denen Sie Leistungsprobleme Ihrer Anwendung debuggen können.

### <a name="use-connection-pooling"></a>Verwenden des Verbindungspoolings
Beim Verbindungspooling werden zum Startzeitpunkt mehrere Verbindungen eingerichtet und dann verwaltet. Dadurch wird auch die Speicherfragmentierung auf dem Server verringert, die durch die auf dem Datenbankserver eingerichteten dynamischen neuen Verbindungen verursacht wird. Das Verbindungspooling kann auf der Anwendungsseite konfiguriert werden, wenn es vom App-Framework oder Datenbanktreiber unterstützt wird. Wenn dies nicht unterstützt wird, sollte ein Proxyverbindungs-Poolerdienst wie [PgBouncer](https://pgbouncer.github.io/) oder [Pgpool](https://pgpool.net/mediawiki/index.php/Main_Page) verwendet werden, der außerhalb der Anwendung ausgeführt wird und eine Verbindung mit dem Datenbankserver herstellt. PgBouncer und Pgpool sind Tools auf Communitybasis, die mit Azure Database for PostgreSQL eingesetzt werden können.

### <a name="retry-logic-to-handle-transient-errors"></a>Wiederholungslogik zum Behandeln vorübergehender Fehler
Bei Ihrer Anwendung können vorübergehende Fehler auftreten, die dazu führen, dass Verbindungen mit der Datenbank vorübergehend gelöscht oder getrennt werden. In solchen Situationen ist der Server nach ein oder zwei Wiederholungen innerhalb von 5–10 Sekunden wieder betriebsbereit. Es wird empfohlen, vor dem ersten Wiederholungsversuch fünf Sekunden zu warten. Erhöhen Sie dann nach jeder Wiederholung die Wartezeit nach und nach auf bis zu 60 Sekunden. Legen Sie eine maximale Anzahl von Wiederholungen fest, nach denen Ihre Anwendung den Vorgang als fehlerhaft ansieht, damit Sie weitere Untersuchungen anstellen können. Weitere Informationen finden Sie unter [Beheben von Verbindungsproblemen](./concepts-connectivity.md).

### <a name="enable-read-replication-to-mitigate-failovers"></a>Aktivieren von Lesereplikaten zum Minimieren von Failovervorgängen
Für Failoverszenarien können Sie die [Datenreplikation](./concepts-read-replicas.md) verwenden. Wenn Sie Lesereplikate verwenden, erfolgt kein automatisiertes Failover zwischen Quell- und Replikatserver. Sie bemerken eine Verzögerung zwischen der Quelle und dem Replikat, da die Replikation asynchron ist. Die Netzwerkverzögerung kann durch viele Faktoren beeinflusst werden, z. B. durch die Größe der Workload auf dem Quellserver und die Latenz zwischen Rechenzentren. In den meisten Fällen beträgt die Replikatverzögerung einige Sekunden bis zu einigen Minuten.


## <a name="database-deployment"></a>Datenbankbereitstellung

### <a name="configure-cicd-deployment-pipeline"></a>Konfigurieren der CI/CD-Bereitstellungspipeline
Gelegentlich werden Sie Änderungen in der Datenbank bereitstellen müssen. In solchen Fällen können Sie Continuous Integration (CI) über [GitHub-Aktionen](https://github.com/Azure/postgresql/blob/master/README.md) für Ihren PostgreSQL-Server verwenden, um die Datenbank durch Ausführung eines benutzerdefinierten Skripts zu aktualisieren.

### <a name="define-manual-database-deployment-process"></a>Definieren des manuellen Vorgangs zur Datenbankbereitstellung
Im Folgenden finden Sie Schritte für die manuelle Datenbankbereitstellung, mit denen Sie Ausfallzeiten sowie das Risiko von Bereitstellungsfehlern minimieren können:

- Erstellen Sie mit „pg_dump“ eine Kopie einer Produktionsdatenbank in einer neuen Datenbank.
- Aktualisieren Sie die neue Datenbank mit den erforderlichen Schemaänderungen oder Aktualisierungen.
- Versetzen Sie die Produktionsdatenbank in den schreibgeschützten Zustand. Bis die Bereitstellung vollständig abgeschlossen ist, dürfen in der Produktionsdatenbank keine Schreibvorgänge erfolgen.
- Testen Sie die Anwendung mit der frisch aktualisierten Datenbank aus Schritt 1.
- Stellen Sie die Anwendungsänderungen bereit, und stellen Sie sicher, dass die Anwendung jetzt die neue Datenbank verwendet, die über die neuesten Updates verfügt.
- Behalten Sie die alte Produktionsdatenbank eine Zeit lang, um ggf. ein Rollback der Änderungen ausführen zu können. Danach können Sie die alte Produktionsdatenbank löschen oder bei Bedarf nach Azure Storage exportieren.

>  [!NOTE]
> Wenn Ihre Anwendung beispielsweise eine E-Commerce-App ist, die nicht in den schreibgeschützten Zustand versetzt werden kann, stellen Sie die Änderungen direkt in der Produktionsdatenbank bereit, nachdem Sie eine Sicherung erstellt haben. Diese Änderung sollte zu einer Zeit mit wenig Datenverkehr für die App erfolgen, um mögliche Auswirkungen zu minimieren, da es bei einigen Benutzern zu Anforderungsfehlern kommen könnte. Stellen Sie sicher, dass Ihr Anwendungscode auch Anforderungsfehler behandelt.

## <a name="database-schema-and-queries"></a>Datenbankschema und -abfragen
Im Folgenden finden Sie einige Tipps für die Erstellung des Datenbankschemas und der Abfragen.

### <a name="use-bigint-or-uuid-for-primary-keys"></a>Verwenden von BIGINT oder UUID für Primärschlüssel
Beim Entwickeln einer benutzerdefinierten Anwendung oder einiger Frameworks verwenden Sie möglicherweise `INT` anstelle von `BIGINT` für Primärschlüssel. Wenn Sie ```INT``` verwenden, besteht das Risiko, dass der Wert in der Datenbank die Speicherkapazität des ```INT```-Datentyps überschreitet. Diese Änderung an einer vorhandenen Produktionsanwendung kann zeitaufwändig sein und damit zusätzliche Entwicklungszeit kosten. Eine andere Möglichkeit ist die Verwendung von [UUID](https://www.postgresql.org/docs/current/datatype-uuid.html) für Primärschlüssel. Dieser Bezeichner verwendet eine automatisch generierte 128-Bit-Zeichenfolge, z. B. ```a0eebc99-9c0b-4ef8-bb6d-6bb9bd380a11```. Erfahren Sie mehr über [PostgreSQL-Datentypen](https://www.postgresql.org/docs/8.1/datatype.html).

### <a name="use-indexes"></a>Verwenden von Indizes

Es gibt viele Typen von [Indizes](https://www.postgresql.org/docs/9.1/indexes.html) in Postgres, die auf unterschiedliche Weise verwendet werden können. Mithilfe eines Indexes kann der Server bestimmte Zeilen viel schneller finden und abrufen. Indizes sind jedoch auch mit einem Mehraufwand für den Datenbankserver verbunden. Daher sollten zu viele Indizes vermieden werden.

### <a name="use-autovacuum"></a>Verwenden von Autovacuum
Mit Autovacuum können Sie Ihren Azure Database for PostgreSQL-Server optimieren. PostgreSQL ermöglicht eine größere Datenbankparallelität mit allen Updateergebnissen von Einfüge- und Löschvorgängen. Zum Löschen werden die Datensätze vorläufig gekennzeichnet, die später endgültig gelöscht werden. Um diese Aufgaben auszuführen, führt PostgreSQL einen Vacuum-Auftrag aus. Wenn Sie Vacuum nicht hin und wieder ausführen, können sich inaktive Tupel ansammeln und zu Problemen führen:

- Datenüberfrachtung – größere Datenbanken und Tabellen
- Größere suboptimale Indizes
- Steigende E/A

Erfahren Sie mehr über das [Optimieren von Autovacuum auf einem Azure Database for PostgreSQL-Einzelserver](howto-optimize-autovacuum.md).

### <a name="use-pg_stats_statements"></a>Verwenden von „pg_stats_statements“
Pg_stat_statements ist eine PostgreSQL-Erweiterung, die für Azure Database for PostgreSQL standardmäßig aktiviert ist. Die Erweiterung ermöglicht das Nachverfolgen von Ausführungsstatistiken zu allen SQL-Anweisungen, die auf einem Server ausgeführt werden. Weitere Informationen finden Sie unter [Optimieren der Sammlung mit Abfragestatistiken auf einem Azure Database for PostgreSQL-Einzelserver](howto-optimize-query-stats-collection.md).


### <a name="use-the-query-store"></a>Verwenden des Abfragedatenspeichers
Das Feature [Abfragespeicher](./concepts-query-store.md) in Azure Database for PostgreSQL bietet eine effektivere Methode zur Nachverfolgung von Abfragestatistiken. Dieses Feature wird als Alternative zur Verwendung von pg_stats_statements empfohlen.

### <a name="optimize-bulk-inserts-and-use-transient-data"></a>Optimieren von Masseneinfügungen und Verwenden transienter Daten
Erwägen Sie die Verwendung von nicht protokollierten Tabellen für Workloadvorgänge, die kurzlebige Daten oder Masseneinfügungen von großen Datasets nutzen. Es bietet standardmäßig Unteilbarkeit und Dauerhaftigkeit. Unteilbarkeit, Konsistenz, Isolation und Dauerhaftigkeit stellen die ACID-Eigenschaften dar. Weitere Informationen finden Sie unter [Optimieren von Masseneinfügungen und Verwendung kurzlebiger Daten auf einem Azure Database for PostgreSQL-Einzelserver](howto-optimize-bulk-inserts.md).

## <a name="next-steps"></a>Nächste Schritte
[Postgres-Handbuch](http://postgresguide.com/)
