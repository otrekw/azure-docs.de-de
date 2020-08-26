---
title: 'Best Practices für die App-Entwicklung: Azure Database for MySQL'
description: Lernen Sie Best Practices für die Entwicklung einer App mit Azure Database for MySQL kennen.
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: conceptual
ms.date: 08/11/2020
ms.openlocfilehash: 36f9cfa2369032351f6ed2948fc0c98c1648bcb6
ms.sourcegitcommit: ef055468d1cb0de4433e1403d6617fede7f5d00e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2020
ms.locfileid: "88259234"
---
# <a name="best-practices-for-building-applications-with-azure-database-for-mysql"></a>Best Practices für die Entwicklung von Anwendungen mit Azure Database for MySQL 

Im Folgenden finden Sie einige Best Practices, die Sie beim Erstellen von cloudfähigen Anwendungen mit Azure Database for MySQL unterstützen und die Entwicklungszeit für Ihre Anwendung verkürzen können. 

## <a name="application-and-database-resource-configuration"></a>Konfiguration von Anwendungs- und Datenbankressourcen

### <a name="application-and-database-in-the-same-region"></a>Anwendung und Datenbank in derselben Region
Stellen Sie sicher, dass sich **sämtliche Abhängigkeiten in derselben Region befinden**, wenn Sie Ihre Anwendung in Azure bereitstellen. Wenn Sie die Instanzen auf verschiedene Regionen oder Verfügbarkeitszonen verteilen, führt dies zu Netzwerklatenzen, die sich auf die Gesamtleistung Ihrer Anwendung auswirken können. 

### <a name="keep-your-mysql-server-secure"></a>Sichern des MySQL-Servers
Ihr MySQL-Server sollte [sicher](https://docs.microsoft.com/azure/mysql/concepts-security) konfiguriert und nicht öffentlich zugänglich sein. Verwenden Sie eine der folgenden Optionen, um Ihren Server zu sichern: 
- [Firewallregeln](https://docs.microsoft.com/azure/mysql/concepts-firewall-rules)
- [Virtuelle Netzwerke](https://docs.microsoft.com/azure/mysql/concepts-data-access-and-security-vnet) 
- [Private Link](https://docs.microsoft.com/azure/mysql/concepts-data-access-security-private-link)

Aus Gründen der Sicherheit müssen Sie Verbindungen mit Ihrem MySQL-Server immer über **SSL** herstellen und den Server und Ihre Anwendung für die Verwendung von **TLS1.2** konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren von SSL/TLS](https://docs.microsoft.com/azure/mysql/concepts-ssl-connection-security). 

### <a name="tune-your-server-parameters"></a>Optimieren der Serverparameter
Bei Workloads mit vielen Lesevorgängen kann eine Feinabstimmung der Parameter „tmp_table_size“ und „max_heap_table_size“ zur Optimierung Ihrer Lösung beitragen. Um die für „tmp_table_size“ und „max_heap_table_size“ erforderlichen Werte zu berechnen, betrachten Sie die Werte für den Gesamtarbeitsspeicher pro Verbindung und den Basisarbeitsspeicher. Die Summe der Parameter für den Arbeitsspeicher pro Verbindung ohne „tmp_table_size“ plus Basisarbeitsspeicher ergibt den Gesamtarbeitsspeicher des Servers.

Verwenden Sie die folgende Formel, um die größtmöglichen Werte für „tmp_table_size“ und „max_heap_table_size“ zu berechnen:

```(total memory - (base memory + (sum of per-connection memory * # of connections)) / # of connections```

>[!NOTE]
> Der Gesamtarbeitsspeicher gibt die Gesamtmenge an Arbeitsspeicher an, die dem Server für alle bereitgestellten virtuellen Kerne zur Verfügung steht.  Bei einem universellen Azure Database for MySQL-Server mit 2 virtuellen Kernen beispielsweise beträgt der Gesamtarbeitsspeicher 5 GB × 2.  Weitere Informationen zum Arbeitsspeicher für jede Ebene finden Sie in der Dokumentation zu den [Tarifen](https://docs.microsoft.com/azure/mysql/concepts-pricing-tiers).
> Der Basisarbeitsspeicher gibt die Arbeitsspeichervariablen wie „query_cache_size“ und „innodb_buffer_pool_size“ an, die MySQL beim Start des Servers initialisiert und zuweist.  Arbeitsspeicher pro Verbindung wie „sort_buffer_size“ und „join_buffer_size“ wird nur dann zugewiesen, wenn eine Abfrage dies erfordert.

### <a name="create-a-non-admin-user"></a>Erstellen eines Benutzers ohne Administratorrechte 
[Erstellen Sie Benutzer ohne Administratorrechte](https://docs.microsoft.com/azure/mysql/howto-create-users) für jede der Datenbanken. In der Regel entsprechen die Benutzernamen den Namen der Datenbanken.

### <a name="resetting-your-password"></a>Zurücksetzen Ihres Kennworts
Im Azure-Portal können Sie [das Kennwort für Ihren MySQL-Server zurücksetzen](https://docs.microsoft.com/azure/mysql/howto-create-manage-server-portal#update-admin-password). 

Das Zurücksetzen des Serverkennworts für eine Produktionsdatenbank kann dazu führen, dass Ihre Anwendung nicht mehr verfügbar ist. Sie sollten das Kennwort für Produktionsworkloads daher außerhalb der Spitzennutzungszeiten zurücksetzen, um die Auswirkungen auf die Endbenutzer Ihrer Anwendung zu minimieren.

## <a name="performance-and-resiliency"></a>Leistung und Resilienz 
Im Folgenden finden Sie einige Tools und Muster, mit denen Sie Leistungsprobleme Ihrer Anwendung debuggen können.

### <a name="enable-slow-query-logs-identify-performance-issues"></a>Aktivieren von Protokollen zu langsamen Abfragen zum Ermitteln von Leistungsproblemen
Sie können [Protokolle zu langsamen Abfragen](https://docs.microsoft.com/azure/mysql/concepts-server-logs) und [ Überwachungsprotokolle](https://docs.microsoft.com/azure/mysql/concepts-audit-logs) auf Ihrem Server aktivieren. Durch eine Analyse des Protokolls für langsame Abfragen können Sie Leistungsengpässe erkennen. 

Überwachungsprotokolle sind auch über Azure-Diagnoseprotokolle in Azure Monitor-Protokollen, Event Hubs und im Speicherkonto verfügbar. Weitere Informationen finden Sie unter [Problembehandlung der Abfrageleistung](https://docs.microsoft.com/azure/mysql/howto-troubleshoot-query-performance).

### <a name="use-connection-pooling"></a>Verbindungspooling verwenden
Das Verwalten von Datenbankverbindungen kann sich auf die Leistung der gesamten Anwendung erheblich auswirken. Um die Leistung zu optimieren, müssen Sie die Häufigkeit reduzieren, mit der Verbindungen hergestellt werden, und die Zeit für das Herstellen von Verbindungen in wichtigen Codepfaden verkürzen.  Verwenden Sie das [Verbindungspooling](https://docs.microsoft.com/azure/mysql/concepts-connectivity#access-databases-by-using-connection-pooling-recommended) für die Verbindung mit Azure Database for MySQL, um die Resilienz und Leistung zu verbessern. 

Mit [ProxySQL](https://proxysql.com/), einem Tool für Verbindungspools, können Sie Verbindungen effizient verwalten. Ein solches Tool kann Verbindungen im Leerlauf reduzieren und vorhandene Verbindungen wiederverwenden, um Probleme zu vermeiden. Weitere Informationen finden Sie unter [Einrichten von ProxySQL](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/connecting-efficiently-to-azure-database-for-mysql-with-proxysql/ba-p/1279842). 

### <a name="retry-logic-to-handle-transient-errors"></a>Wiederholungslogik zum Behandeln vorübergehender Fehler
Bei Ihrer Anwendung können [vorübergehende Fehler](https://docs.microsoft.com/azure/mysql/concepts-connectivity#handling-transient-errors) auftreten, die dazu führen, dass Verbindungen mit der Datenbank vorübergehend gelöscht oder getrennt werden. In solchen Situationen ist der Server nach ein oder zwei Wiederholungen innerhalb von 5–10 Sekunden wieder betriebsbereit. 

Bei Wiederholungsversuchen empfiehlt es sich, vor dem ersten Versuch 5 Sekunden lang zu warten und die Wartezeit bei jedem weiteren Versuch allmählich auf bis zu 60 Sekunden zu erhöhen. Legen Sie eine maximale Anzahl von Wiederholungen fest, nach denen Ihre Anwendung den Vorgang als fehlerhaft ansieht, damit Sie weitere Untersuchungen anstellen können. Weitere Informationen finden Sie unter [Beheben von Verbindungsproblemen](https://docs.microsoft.com/azure/mysql/howto-troubleshoot-common-connection-issues). 

### <a name="enable-read-replication-to-mitigate-failovers"></a>Aktivieren von Lesereplikaten zum Minimieren von Failovervorgängen
Für Failoverszenarien können Sie die [Replikation eingehender Daten](https://docs.microsoft.com/azure/mysql/howto-data-in-replication) verwenden. Bei Lesereplikaten erfolgt zwischen Master- und Replikatservern kein automatisiertes Failover. Sie werden eine Verzögerung zwischen Master und Replikat feststellen, da die Replikation asynchron verläuft. Die Netzwerkverzögerung kann durch viele Faktoren beeinflusst werden, z. B. durch den Umfang der Workload auf dem Masterserver und die Latenz zwischen Rechenzentren. In den meisten Fällen beträgt die Replikatverzögerung einige Sekunden bis zu einigen Minuten.

## <a name="database-deployment"></a>Datenbankbereitstellung 

### <a name="configure-azure-database-for-mysql-task-in-your-cicd-deployment-pipeline"></a>Konfigurieren einer Azure Database for MySQL-Aufgabe in der CI/CD-Bereitstellungspipeline
Gelegentlich werden Sie Änderungen in der Datenbank bereitstellen müssen. In solchen Fällen können Sie Continuous Integration (CI) und Continuous Delivery (CD) über [Azure-Pipelines](https://azure.microsoft.com/services/devops/pipelines/) einrichten und eine Aufgabe für [Ihren MySQL-Server](https://docs.microsoft.com/azure/devops/pipelines/tasks/deploy/azure-mysql-deployment?view=azure-devops) verwenden, um die Datenbank durch Ausführung eines benutzerdefinierten Skripts zu aktualisieren.

### <a name="manual-database-deployment"></a>Manuelle Datenbankbereitstellung 
Im Folgenden finden Sie ein empfehlenswertes Muster für die manuelle Datenbankbereitstellung, mit dem Sie Ausfallzeiten sowie das Risiko von Bereitstellungsfehlern minimieren können: 

1. Erstellen Sie mit [mysqldump](https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html) oder [MySQL Workbench](https://dev.mysql.com/doc/workbench/en/wb-admin-export-import-management.html) eine Kopie der Produktionsdatenbank in einer neuen Datenbank. 
2. Aktualisieren Sie die neue Datenbank mit den erforderlichen Schemaänderungen oder Aktualisierungen. 
3. Versetzen Sie die Produktionsdatenbank in den schreibgeschützten Zustand. Bis die Bereitstellung vollständig abgeschlossen ist, dürfen in der Produktionsdatenbank keine Schreibvorgänge erfolgen. 
4. Testen Sie die Anwendung mit der frisch aktualisierten Datenbank aus Schritt 1.
5. Stellen Sie die Anwendungsänderungen bereit, und stellen Sie sicher, dass die Anwendung jetzt die neue Datenbank verwendet, die über die neuesten Updates verfügt. 
6. Behalten Sie die alte Produktionsdatenbank eine Zeit lang, um ggf. ein Rollback der Änderungen ausführen zu können. Danach können Sie die alte Produktionsdatenbank löschen oder bei Bedarf in einen Azure-Speicher exportieren. 

>[!NOTE]
>  - Wenn Sie Ihre Anwendung nicht in den schreibgeschützten Zustand versetzen können (beispielsweise bei einer E-Commerce-App), und stellen Sie die Änderungen direkt in der Produktionsdatenbank bereit, nachdem Sie eine Sicherungskopie erstellt haben.  Diese Änderung sollte zu einer Zeit mit wenig Datenverkehr für die App erfolgen, um mögliche Auswirkungen zu minimieren, da es bei einigen Benutzern zu Anforderungsfehlern kommen könnte. 
>  - Stellen Sie sicher, dass Ihr Anwendungscode auch Anforderungsfehler behandelt.

### <a name="use-mysql-native-metrics-to-see-if-your-workload-is-exceeding-in-memory-temporary-table-sizes"></a>Verwenden von nativen MySQL-Metriken, um zu ermitteln, ob die Workload die Größen arbeitsspeicherinterner temporärer Tabellen überschreitet
Bei Workloads mit vielen Lesevorgängen können Abfragen des MySQL-Servers die Größen arbeitsspeicherinterner temporärer Tabellen überschreiten. Das kann dazu führen, dass der Server temporäre Tabellen auf einen Datenträger schreibt. Dies wiederum beeinträchtigt die Leistung Ihrer Anwendung. Um zu ermitteln, ob Ihr Server aufgrund einer Größenüberschreitung der temporären Tabelle auf einen Datenträger schreibt, sehen Sie sich die folgenden Metriken an:

```
show global status like 'created_tmp_disk_tables';
show global status like 'created_tmp_tables';
```
Die Metrik „created_tmp_disk_tables“ gibt an, wie viele Tabellen auf dem Datenträger erstellt wurden, „created_tmp_table“ informiert darüber, wie viele temporäre Tabellen im Arbeitsspeicher für Ihre Workload erstellt werden müssen. Um zu ermitteln, ob für die Ausführung einer bestimmten Abfrage temporäre Tabellen verwendet werden, führen Sie eine EXPLAIN-Anweisung für die Abfrage aus. Die Details in der Spalte „extra“ geben „Using temporary“ an, wenn bei Ausführung der Abfrage temporäre Tabellen verwendet werden.

Um zu berechnen, für welchen Prozentsatz Ihrer Workload die Abfragen auf Datenträgern geschrieben werden, verwenden Sie die folgende Formel mit Ihren Metrikwerten:

```(created_tmp_disk_tables / (created_tmp_disk_tables + created_tmp_tables)) * 100```

Im Idealfall sollte das Ergebnis unter 25 % liegen. Wenn der Wert 25 % oder höher ist, empfiehlt es sich, Änderungen an zwei Serverparametern vorzunehmen: „tmp_table_size“ und „max_heap_table_size“.


## <a name="database-schema-and-queries"></a>Datenbankschema und -abfragen

Im Folgenden finden Sie einige Tipps und Tricks für die Erstellung des Datenbankschemas und der Abfragen.

### <a name="always-use-the-right-datatype-for-your--table-columns"></a>Verwenden des richtigen Datentyps für Ihre Tabellenspalten
Verwenden Sie immer die geeigneten Datentypen, je nachdem, welche Arten von Daten Sie speichern möchten. So können Sie den Speicher optimieren und Fehler aufgrund falscher Datentypen reduzieren.

### <a name="use-indexes"></a>Verwenden von Indizes
Sie können Indizes verwenden, um langsame Abfragen zu vermeiden. Mithilfe von Indizes lassen sich Zeilen mit bestimmten Spalten schnell finden. Weitere Informationen finden Sie unter [How MySQL Uses Indexes](https://dev.mysql.com/doc/refman/8.0/en/mysql-indexes.html) (Verwenden von Indizes in MySQL).

### <a name="explain-your-select-queries"></a>Ausführen von EXPLAIN-Anweisungen für SELECT-Abfragen
Verwenden Sie eine [EXPLAIN](https://dev.mysql.com/doc/refman/8.0/en/explain.html)-Anweisung, um zu erfahren, auf welche Weise MySQL Ihre Abfrage ausführt. So können Sie mögliche Engpässe oder Probleme bei Ihrer Abfrage erkennen. Weitere Informationen finden Sie unter [Verwenden von EXPLAIN zum Analysieren der Abfrageleistung in Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/howto-troubleshoot-query-performance).


