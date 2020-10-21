---
title: 'Behandeln von Problemen mit der Replikationswartezeit: Azure Database for MySQL'
description: Erfahren Sie, wie Sie Probleme mit der Replikationswartezeit durch Azure Database for MySQL-Lesereplikate beheben.
keywords: mysql, Problembehandlung, Replikationswartezeit in Sekunden
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: troubleshooting
ms.date: 10/08/2020
ms.openlocfilehash: 16a502a53b4441faf68ea342e0bc865731d38b1a
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91876839"
---
# <a name="troubleshoot-replication-latency-in-azure-database-for-mysql"></a>Behandeln von Problemen mit der Replikationswartezeit in Azure Database for MySQL

Mithilfe des [Lesereplikat](concepts-read-replicas.md)-Features können Sie Daten von einem Azure Database for MySQL-Server auf einem schreibgeschützten Replikatserver replizieren. Lesereplikate werden verwendet, um Workload aufzuskalieren, indem Lese-/Berichterstellungsabfragen von der Anwendung an Replikatsever weitergeleitet werden. Dadurch werden die Belastung des primären Servers verringert und die Gesamtleistung und Wartezeit der Anwendung bei deren Skalierung verbessert. Replikate werden asynchron mithilfe des auf der Position der nativen, binären Protokolldatei (binlog) basierenden Replikationsverfahrens der MySQL-Engine aktualisiert. Weitere Informationen zur binlog-Replikation finden Sie unter [Binary Log File Position Based Replication Configuration Overview](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html) (Konfiguration der auf der Position der binären Protokolldatei basierenden Replikation – Übersicht). 

Die Replikationsverzögerung für die sekundären Lesereplikatserver hängt von einer Reihe von Faktoren ab, zu denen die folgenden gehören: 

- Netzwerklatenz
- Transaktionsvolumen auf dem Quellserver
- Computeebene des Quell- und des sekundären Lesereplikatservers
- Abfragen, die auf dem primären und sekundären Server ausgeführt werden 

In diesem Dokument erfahren Sie, wie Sie Probleme mit der Replikationswartezeit in Azure Database for MySQL behandeln. Außerdem werden einige der üblichen Gründe für eine größere Replikationswartezeit auf Replikatservern erläutert.

## <a name="replication-concepts"></a>Replikationskonzepte

Wenn binäre Protokollierung aktiviert ist, schreibt der Quellserver jede committete Transaktion in das binäre Protokoll, das für Replikation verwendet wird. Die binäre Protokollierung ist standardmäßig für alle neu bereitgestellten Server aktiviert, die bis zu 16 TB Speicher unterstützen. Auf einem Replikatserver gibt es zwei Threads, die ausgeführt werden: einer wird als E/A-Thread und der andere als SQL-Thread bezeichnet.

- Der **E/A-Thread** stellt eine Verbindung mit dem Quellserver her und fordert aktualisierte binäre Protokolle (Transaktionsprotokolle) an. Nachdem dieser Thread die Aktualisierungen der binären Protokolle empfangen hat, werden die Aktualisierungen auf einem Replikatserver in einem lokalen Protokoll gespeichert, das als Relaisprotokoll bezeichnet wird.
- Der **SQL-Thread** liest das Relaisprotokoll und wendet die Datenänderungen auf Replikatservern an.

## <a name="monitoring-replication-latency"></a>Überwachen der Replikationswartezeit

Azure Database for MySQL stellt die Metrik „Replication lag in seconds“ (Replikationsverzögerung in Sekunden) in [Azure Monitor](concepts-monitoring.md) bereit. Diese Metrik ist nur auf Lesereplikatservern verfügbar. Sie wird mit der „seconds_behind_master“-Metrik berechnet, die in MySQL verfügbar ist. Um die Hauptursache der vergrößerten Replikationswartezeit zu verstehen, stellen Sie über [MySQL Workbench](connect-workbench.md) oder [Azure Cloud Shell](https://shell.azure.com) eine Verbindung mit dem Replikatserver her, und führen Sie folgenden Befehl aus:

 Ersetzen Sie die Werte durch den tatsächlichen Replikatservernamen und den Anmeldenamen des Administratorbenutzers. Für den Namen des Administratorbenutzers ist „@\<servername>“ für Azure Database for MySQL erforderlich:

  ```azurecli-interactive
  mysql --host=myreplicademoserver.mysql.database.azure.com --user=myadmin@mydemoserver -p 
  ```

  So sieht die Anzeige im Cloud Shell-Terminal aus:
  ```
  Requesting a Cloud Shell.Succeeded.
  Connecting terminal...

  Welcome to Azure Cloud Shell

  Type "az" to use Azure CLI
  Type "help" to learn about Cloud Shell

  user@Azure:~$mysql -h myreplicademoserver.mysql.database.azure.com -u myadmin@mydemoserver -p
  Enter password:
  Welcome to the MySQL monitor.  Commands end with ; or \g.
  Your MySQL connection id is 64796
  Server version: 5.6.42.0 Source distribution

  Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

  Oracle is a registered trademark of Oracle Corporation and/or its
  affiliates. Other names may be trademarks of their respective
  owners.

  Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
  mysql>
  ```
  Führen Sie im selben Azure Cloud Shell-Terminal den folgenden Befehl aus:

  ```
  mysql> SHOW SLAVE STATUS;
  ```

  Eine typische Ausgabe sieht wie folgt aus:
  
>[!div class="mx-imgBorder"]
> :::image type="content" source="./media/howto-troubleshoot-replication-latency/show-status.png" alt-text="Überwachen der Replikationswartezeit":::


Die Ausgabe enthält viele Informationen, normalerweise genügt es aber, sich auf die folgenden Spalten zu konzentrieren:

|Metrik|Beschreibung|
|---|---|
|Slave_IO_State| Der aktuelle Status des E/A-Threads. Normalerweise ist der Status gleich „Waiting for master to send event“, wenn über den Thread synchronisiert wird. Wird jedoch „Connecting to master“ für den Status angezeigt, ist die Verbindung des Replikats mit dem Masterserver verloren gegangen. Überprüfen Sie, ob der Masterserver ausgeführt wird oder ob eine Firewall die Verbindung blockiert.|
|Master_Log_File| Die binäre Protokolldatei, in die der Masterserver schreibt.|
|Read_Master_Log_Pos| Gibt die Position in der obigen binären Protokolldatei an, in die der Masterserver schreibt.|
|Relay_Master_Log_File| Gibt die binäre Protokolldatei an, die der Replikatserver vom Masterserver liest.|
|Slave_IO_Running| Kennzeichnet, ob der E/A-Thread ausgeführt wird. Der Wert sollte gleich „Yes“ sein. Ist er gleich „No“, ist die Replikation höchstwahrscheinlich nicht funktionsfähig.|
|Slave_SQL_Running| Kennzeichnet, ob der SQL-Thread ausgeführt wird. Der Wert sollte gleich „Yes“ sein. Ist er gleich „No“, ist die Replikation höchstwahrscheinlich nicht funktionsfähig.|
|Exec_Master_Log_Pos| Zeigt die Position aus Relay_Master_Log_File an, die im Replikat angewendet wird. Gibt es eine Wartezeit, sollte diese Positionssequenz kleiner sein als Read_Master_Log_Pos.|
|Relay_Log_Space|Zeigt die Obergrenze der Größe des Relaisprotokolls an. Sie können die Größe überprüfen, indem Sie globale Variablen wie „relay_log_space_limit“ anzeigen.|
|Seconds_Behind_Master| Zeigt die Replikationswartezeit in Sekunden an.|
|Last_IO_Errno|Zeigt ggf. den Fehlercode des E/A-Threads an. Weitere Informationen zu diesen Codes finden Sie in der [MySQL-Dokumentation](https://dev.mysql.com/doc/refman/5.7/en/server-error-reference.html).|
|Last_IO_Error| Zeigt ggf. die Fehlermeldung des E/A-Threads an.|
|Last_SQL_Errno|Zeigt ggf. den Fehlercode des SQL-Threads an. Weitere Informationen zu diesen Codes finden Sie in der [MySQL-Dokumentation](https://dev.mysql.com/doc/refman/5.7/en/server-error-reference.html).|
|Last_SQL_Error|Zeigt ggf. die Fehlermeldung des SQL-Threads an.|
|Slave_SQL_Running_State| Kennzeichnet den aktuellen Status des SQL-Threads. Beachten Sie, dass die Anzeige von „System lock“ in diesem Status ein normales Verhalten ist. Normalerweise wird der Status als „Waiting for dependent transaction to commit“ angezeigt. Dies kennzeichnet, dass der Replikatserver darauf wartet, dass der Masterserver committete Transaktionen aktualisiert.|

Ist Slave_IO_Running gleich „Yes“ und Slave_SQL_Running gleich „Yes“, wird die Replikation ordnungsgemäß ausgeführt. 

Im nächsten Schritt müssen Sie Last_IO_Errno, Last_IO_Error, Last_SQL_Errno und Last_SQL_Error überprüfen.  Diese Felder enthalten jeweils die Fehlernummer und die Fehlermeldung des letzten Fehlers, der dazu geführt hat, dass der SQL-Thread angehalten wurde. Eine Fehlernummer 0 und eine leere Meldung bedeuten, dass kein Fehler vorliegt. Ist die Fehlernummer ungleich 0, muss weiter untersucht werden, indem in der [MySQL-Dokumentation](https://dev.mysql.com/doc/refman/5.7/en/server-error-reference.html) nach dem Fehlercode gesucht wird.

## <a name="common-scenarios-for-high-replication-latency"></a>Übliche Szenarien für hohe Replikationswartezeit

### <a name="network-latency-or-high-cpu-on-source-server"></a>Netzwerkwartezeit oder hohe CPU-Auslastung auf dem Quellserver

Wenn Sie die folgenden Werte feststellen, ist die häufigste Ursache für die Replikationswartezeit eine hohe Netzwerkwartezeit oder eine hohe CPU-Auslastung auf dem Quellserver. In diesem Fall wartet der aktive E/A-Thread auf den Masterserver. Der Masterserver (Quellserver) hat bereits in eine binäre Protokolldatei 20 geschrieben, während das Replikat nur bis zur Datei 10 empfangen hat. Die Hauptfaktoren für eine hohe Replikationswartezeit in diesem Szenario sind die Netzwerkgeschwindigkeit oder eine hohe CPU-Auslastung auf dem Quellserver.  In Azure liegt die Netzwerkwartezeit innerhalb einer Region normalerweise im Millisekundenbereich, kann bei Regionsüberschreitung aber auf Sekunden anwachsen. In den meisten Fällen wird die Verzögerung im E/A-Thread bei der Verbindung zum Quellserver durch eine hohe CPU-Auslastung auf dem Quellserver verursacht, wodurch die E/A-Threadverarbeitung langsam ist. Dies kann festgestellt werden, indem mit Azure Monitor für den Quellserver die CPU-Auslastung überwacht und die Anzahl der gleichzeitigen Verbindungen ermittelt wird.

Stellen Sie keine hohe CPU-Auslastung auf dem Quellserver fest, kann die Netzwerkwartezeit die mögliche Ursache sein. Stellen Sie plötzlich eine außergewöhnlich hohe Netzwerkwartezeit fest, sollten Sie die [Azure-Statusseite](https://status.azure.com/status) überprüfen, um sicherzustellen, dass keine bekannten Probleme oder Ausfälle vorliegen. 

```
Slave_IO_State: Waiting for master to send event
Master_Log_File: the binary file sequence is larger then Relay_Master_Log_File, e.g. mysql-bin.00020
Relay_Master_Log_File: the file sequence is smaller than Master_Log_File, e.g. mysql-bin.00010
```

### <a name="heavy-burst-of-transactions-on-source-server"></a>Starke Zunahme von Transaktionen auf dem Quellserver

Wenn Sie die folgenden Werte feststellen, ist die häufigste Ursache für die Replikationswartezeit eine starke Zunahme von Transaktionen auf dem Quellserver. In der folgenden Ausgabe zeigt der Replikat-E/A-Thread, obwohl der Replikatserver das binäre Protokoll hinter dem Masterserver abrufen kann, dass der Protokollspeicherplatz des Relaisservers voll ist. Somit wird die Verzögerung nicht durch die Netzwerkgeschwindigkeit verursacht, da der Replikatserver bereits versucht hat, so schnell wie möglich abzurufen. Stattdessen überschreitet die aktualisierte Größe des binären Protokolls die Obergrenze des Relaisprotokollspeicherplatzes. Um dieses Problem zu beheben, sollte [slow query log](concepts-server-logs.md) (Protokoll für langsame Abfrage) auf dem Masterserver aktiviert werden. Langsame Abfrageprotokolle ermöglichen es Ihnen, Transaktionen mit langer Laufzeit auf dem Quellserver zu erkennen. Die erkannten Abfragen müssen optimiert werden, um die Wartezeit auf dem Server zu verringern. 

```
Slave_IO_State: Waiting for the slave SQL thread to free enough relay log space
Master_Log_File: the binary file sequence is larger then Relay_Master_Log_File, e.g. mysql-bin.00020
Relay_Master_Log_File: the file sequence is smaller then Master_Log_File, e.g. mysql-bin.00010
```

Nachstehend sind die häufigsten Ursachen für die Wartezeit in dieser Kategorie aufgeführt:

#### <a name="replication-latency-due-to-data-load-on-source-server"></a>Replikationswartezeit aufgrund von Datenlast auf dem Quellserver
In einigen Fällen gibt es wöchentliche oder monatliche Datenlast auf Quellservern. Leider ist die Replikationswartezeit in diesem Fall unvermeidlich. In diesem Szenario ziehen die Replikatserver schließlich gleich, nachdem die Datenlast auf dem Quellserver abgeschlossen ist.


### <a name="slowness-on-the-replica-server"></a>Langsamkeit auf dem Replikatserver

Wenn Sie die folgenden Werte feststellen, ist die häufigste Ursache ein Problem auf dem Replikatserver sein, das weiter untersucht werden muss. In diesem Szenario werden, wie in der Ausgabe zu sehen ist, sowohl der E/A- als auch der SQL-Thread ordnungsgemäß ausgeführt, und der Replikatserver liest genau die binäre Protokolldatei, in die der Masterserver schreibt. Auf dem Replikatserver tritt jedoch eine gewisse Wartezeit auf, bis die jeweilige Transaktion des Quellservers eingetragen ist. 

```
Slave_IO_State: Waiting for master to send event
Master_Log_File: The binary log file sequence equals to Relay_Master_Log_File, e.g. mysql-bin.000191
Read_Master_Log_Pos: The position of master server written to the above file is larger than Relay_Log_Pos, e.g. 103978138
Relay_Master_Log_File: mysql-bin.000191
Slave_IO_Running: Yes
Slave_SQL_Running: Yes
Exec_Master_Log_Pos: The position of slave reads from master binary log file is smaller than Read_Master_Log_Pos, e.g. 13468882
Seconds_Behind_Master: There is latency and the value here is greater than 0
```

Nachstehend sind die häufigsten Ursachen für die Wartezeit in dieser Kategorie aufgeführt:

#### <a name="no-primary-or-unique-key-on-a-table"></a>Kein Primärschlüssel oder eindeutiger Schlüssel für eine Tabelle

Für Azure Database for MySQL wird zeilenbasierte Replikation verwendet. Bei zeilenbasierter Replikation schreibt der Masterserver Ereignisse zu einzelnen Tabellenzeilenänderungen in das binäre Protokoll. Der SQL-Thread wiederum schreibt diese Änderungen in die entsprechenden Tabellenzeilen auf dem Replikatserver. Kein Primär- oder eindeutiger Schlüssel für eine Tabelle ist eine der häufigsten Ursachen für Replikationswartezeit. Liegt kein Primär- oder eindeutiger Schlüssel vor, muss der SQL-Thread alle Zeilen der Zieltabelle überprüfen, um die Änderungen anwenden zu können.

In MySQL ist der Primärschlüssel ein zugeordneter Index, der eine hohe Abfrageleistung sicherstellt, da er keine NULL-Werte enthalten darf. Mit der InnoDB-Speicher-Engine werden die Tabellendaten physisch angeordnet, um extrem schnelle Such- und Sortierungvorgänge auf Basis des Primärschlüssels durchzuführen. Es empfiehlt sich daher, vor dem Erstellen des Replikatservers auf dem Quellserver einen Primärschlüssel für Tabellen hinzuzufügen. In diesem Szenario müssen Sie Primärschlüssel auf dem Quellserver hinzufügen und Lesereplikate neu erstellen, um die Replikationswartezeit zu verkürzen.

Mit der folgenden Abfrage können Sie die Tabellen auf dem Quellserver ermitteln, die keinen Primärschlüssel haben:

```sql 
select tab.table_schema as database_name, tab.table_name 
from information_schema.tables tab left join 
information_schema.table_constraints tco 
on tab.table_schema = tco.table_schema 
and tab.table_name = tco.table_name 
and tco.constraint_type = 'PRIMARY KEY' 
where tco.constraint_type is null 
and tab.table_schema not in('mysql', 'information_schema', 'performance_schema', 'sys') 
and tab.table_type = 'BASE TABLE' 
order by tab.table_schema, tab.table_name;

```

#### <a name="replication-latency-due-to-long-running-queries-on-replica-server"></a>Replikationswartezeit wegen zeitintensiver Abfragen auf dem Replikatserver

Es kann vorkommen, dass die Workload auf dem Replikatserver verhindert, dass der SQL-Thread mit dem E/A-Thread Schritt hält. Eine der häufigsten Ursachen für eine lange Replikationswartezeit ist eine zeitintensive Abfrage auf dem Replikatserver. In diesem Fall sollte [slow query log](concepts-server-logs.md) (Protokoll für langsame Abfrage) auf dem Replikatserver aktiviert werden, um das Problem zu beheben. Langsame Abfragen können die Ressourcennutzung erhöhen oder den Server verlangsamen, sodass der Replikatserver nicht mit dem Masterserver Schritt halten kann. In diesem Szenario müssen Sie langsame Abfragen verbessern. Schnellere Abfragen verhindern das Blockieren von SQL-Threads und verkürzen die Replikationswartezeit erheblich.


#### <a name="replication-latency-due-to-ddl-queries-on-source-server"></a>Replikationswartezeit aufgrund von DDL-Abfragen auf dem Quellserver
Nehmen Sie an, es gibt einen zeitintensiven DDL-Befehl, etwa [ALTER TABLE](https://dev.mysql.com/doc/refman/5.7/en/alter-table.html), der auf dem Quellserver ausgeführt wird, und das Ausführen des Befehls hat eine Stunde gedauert. Während dieser Zeit können Tausende von anderen Abfragen parallel auf dem Quellserver ausgeführt werden. Wenn der DDL-Befehl auf dem Replikatserver repliziert wird, muss der DDL-Befehl von der MySQL-Engine in einem einzigen Replikationsthread ausgeführt werden, damit die Konsistenz der Datenbank sichergestellt ist. Daher werden alle anderen replizierten Abfragen blockiert, und sie müssen eine Stunde oder länger warten, bis der DDL-Vorgang auf dem Replikatserver abgeschlossen ist. Dies gilt unabhängig davon, ob der DDL-Vorgang ein Onlinevorgang ist oder nicht. Bei DDL-Vorgängen ist zu erwarten, dass die Replikation eine längere Replikationswartezeit hat.

Wenn Sie [slow query log](concepts-server-logs.md) (Protokoll für langsame Abfrage) auf dem Quellserver aktiviert haben, kann dieses Szenario erkannt werden, indem Sie die Protokolle für langsame Abfragen auswerten, um festzustellen, ob auf dem Quellserver ein DDL-Befehl ausgeführt wurde. Obwohl für Indexlöschung, -umbenennung und -erstellung der INPLACE-Algorithmus im ALTER TABLE-Befehl verwendet werden sollte, kann es dafür erforderlich sein, Tabellendaten zu kopieren und die Tabelle neu zu erstellen. Für den INPLACE-Algorithmus werden üblicherweise gleichzeitige DML-Vorgänge unterstützt, aber eine exklusive Metadatensperre für die Tabelle wird möglicherweise während der Vorbereitungs- und der Ausführungsphase des Vorgangs durchgesetzt. Daher können für die CREATE INDEX-Anweisung die Klauseln ALGORITHM und LOCK verwendet werden, um die Tabellenkopiermethode und den Parallelitätsgrad für Lese- und Schreibvorgänge zu beeinflussen. Dennoch führt Hinzufügen eines FULLTEXT- oder SPATIAL-Indexes dazu, dass DML-Vorgänge weiterhin verhindert werden. Nachstehend finden Sie ein Beispiel für ein Erstellen eines Indexes mit den Klauseln ALGORITHM und LOCK:

```sql
ALTER TABLE table_name ADD INDEX index_name (column), ALGORITHM=INPLACE, LOCK=NONE;
```

Für eine DDL-Anweisung, die eine Sperre erfordert, kann Replikationswartezeit nicht vermieden werden. Deshalb sollten diese Arten von DDL-Vorgängen außerhalb der Spitzenzeiten ausgeführt werden, beispielsweise nachts, um mögliche Auswirkungen zu verringern.

#### <a name="replication-latency-due-to-replica-server-lower-sku"></a>Replikationswartezeit aufgrund eines niedrigeren Tarifs des Replikatservers

In Azure Database for MySQL werden Lesereplikate mit derselben Serverkonfiguration wie der Masterserver erstellt. Die Replikatserverkonfiguration kann nach der Erstellung geändert werden. Wird der Replikatserver jedoch herabgestuft, kann die Workload zu einer höheren Ressourcenauslastung führen, die wiederum zu einer Replikationswartezeit führen kann. Dies lässt sich beobachten, indem die CPU- und Arbeitsspeicherauslastung des Replikatservers mit Azure Monitor überwacht werden. In diesem Szenario sollten für die Konfiguration des Replikatservers mindestens die gleichen Werte verwendet werden wie für den Quellserver, damit das Replikat über genügend Kapazität verfügt.

#### <a name="improving-replication-latency-using-server-parameter-tuning-on-source-server"></a>Verkürzen der Replikationswartezeit über Optimieren von Serverparametern auf dem Quellserver

In Azure Database for MySQL ist Replikation optimiert, sodass sie standardmäßig mit parallelen Threads für Replikate ausgeführt wird. Für Workloads mit großer Parallelität auf dem Quellserver, bei denen der Replikatserver nicht Schritt halten kann, lässt sich die Replikationswartezeit verkürzen, indem der Parameter binlog_group_commit_sync_delay auf dem Quellserver konfiguriert wird. Mit diesem Parameter wird gesteuert, wie viele Mikrosekunden für das Commit des binären Protokolls gewartet wird, bevor die binäre Protokolldatei synchronisiert wird. Der Vorteil besteht darin, dass der Masterserver die Aktualisierungen des binären Protokolls in einem Block sendet, anstatt jede committete Transaktion sofort anzuwenden. Dadurch werden die E/A-Vorgänge auf dem Replikatserver verringert, wodurch die Leistung verbessert wird. In diesem Szenario kann es nützlich sein, binlog_group_commit_sync_delay auf 1000 oder einen ähnlichen Wert festzulegen und die Replikationswartezeit zu überwachen. Dieser Parameter sollte vorsichtig festgelegt und nur für hohe gleichzeitige Workloads genutzt werden. Für ein Szenario mit geringer Parallelität und vielen Einzeltransaktionen kann das Festlegen von binlog_group_commit_sync_delay die Wartezeit verlängern, weil der E/A-Thread auf blockweise Aktualisierungen des binären Protokolls wartet, während möglicherweise nur wenige Transaktionen committet werden. 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter [Binary Log File Position Based Replication Configuration Overview](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html) (Übersicht über binärprotokollgesteuerte Replikation in MySQL).
