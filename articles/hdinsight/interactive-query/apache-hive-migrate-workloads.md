---
title: Migrieren von Azure HDInsight 3.6-Hive-Workloads zu HDInsight 4.0
description: Erfahren Sie, wie Sie Apache Hive-Workloads in HDInsight 3.6 zu HDInsight 4.0 migrieren.
author: msft-tacox
ms.author: tacox
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 11/13/2019
ms.openlocfilehash: 9f49a9224ed123b76f4d300c27a8dd5822e50ea3
ms.sourcegitcommit: c69c8c5c783db26c19e885f10b94d77ad625d8b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74706023"
---
# <a name="migrate-azure-hdinsight-36-hive-workloads-to-hdinsight-40"></a>Migrieren von Azure HDInsight 3.6-Hive-Workloads zu HDInsight 4.0

In diesem Dokument erfahren Sie, wie Sie Apache Hive- und LLAP-Workloads in HDInsight 3.6 zu HDInsight 4.0 migrieren. HDInsight 4.0 bietet neuere Hive- und LLAP-Features, beispielsweise materialisierte Sichten und die Zwischenspeicherung von Abfrageergebnissen. Wenn Sie Ihre Workloads zu HDInsight 4.0 migrieren, können Sie viele neuere Funktionen von Hive 3 nutzen, die nicht in HDInsight 3.6 verfügbar sind.

In diesem Artikel werden die folgenden Themen behandelt:

* Migration von Hive-Metadaten zu HDInsight 4.0
* Sichere Migration von ACID- und Nicht-ACID-Tabellen
* Beibehaltung von Hive-Sicherheitsrichtlinien in verschiedenen HDInsight-Versionen
* Abfrageausführung und Debuggen von HDInsight 3.6 in HDInsight 4.0

Ein Vorteil von Hive ist die Möglichkeit, Metadaten in eine externe Datenbank zu exportieren (bezeichnet als Hive-Metastore). Der **Hive-Metastore** ist für das Speichern von Tabellenstatistiken zuständig, einschließlich des Tabellenspeicherorts, der Spaltennamen und der Indexinformationen der Tabelle. Das Datenbankschema des Metastores unterscheidet sich je nach Hive-Version. Die empfohlene Vorgehensweise zum sicheren Aktualisieren des Hive-Metastores besteht darin, eine Kopie zu erstellen und die Kopie anstelle der aktuellen Produktionsumgebung zu aktualisieren.

## <a name="copy-metastore"></a>Kopieren des Metastores

HDInsight 3.6 und HDInsight 4.0 erfordern unterschiedliche Schemas für den Metastore. Die gemeinsame Nutzung eines einzelnen Metastores ist daher nicht möglich.

### <a name="external-metastore"></a>Externer Metastore

Erstellen Sie eine neue Kopie Ihres externen Metastores. Wenn Sie einen externen Metastore verwenden, besteht eine sichere und einfache Methode zum Erstellen einer Kopie des Metastores in der [Wiederherstellung der Datenbank](../../sql-database/sql-database-recovery-using-backups.md#point-in-time-restore) mit einem anderen Namen unter Verwendung der Wiederherstellungsfunktion von SQL-Datenbank.  Weitere Informationen zum Anfügen eines externen Metastores an einen HDInsight-Cluster finden Sie unter [Verwenden von externen Metadatenspeichern in Azure HDInsight](../hdinsight-use-external-metadata-stores.md).

### <a name="internal-metastore"></a>Interner Metastore

Wenn Sie einen internen Metastore verwenden, können Sie Objektdefinitionen mithilfe von Abfragen in den Hive-Metastore exportieren und dann in eine neue Datenbank importieren.

1. Stellen Sie mit einem [Secure Shell-Client (SSH)](../hdinsight-hadoop-linux-use-ssh-unix.md) eine Verbindung mit dem HDInsight-Cluster her.

1. Stellen Sie mithilfe des folgenden Befehls mit Ihrem [Beeline-Client](../hadoop/apache-hadoop-use-hive-beeline.md) eine Verbindung mit HiveServer2 aus Ihrer Open SSH-Sitzung her:

    ```hiveql
    for d in `beeline -u "jdbc:hive2://localhost:10001/;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "show databases;"`; do echo "create database $d; use $d;" >> alltables.sql; for t in `beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "show tables;"` ; do ddl=`beeline -u "jdbc:hive2://localhost:10001/$d;transportMode=http" --showHeader=false --silent=true --outputformat=tsv2 -e "show create table $t;"`; echo "$ddl ;" >> alltables.sql ; echo "$ddl" | grep -q "PARTITIONED\s*BY" && echo "MSCK REPAIR TABLE $t ;" >> alltables.sql ; done; done
    ```

    Mit diesem Befehl wird die Datei **alltables.sql** generiert. Da die Standarddatenbank nicht gelöscht oder neu erstellt werden kann, entfernen Sie die Anweisung `create database default;` aus der Datei **alltables.sql**.

1. Beenden Sie Ihre SSH-Sitzung. Geben Sie dann einen SCP-Befehl ein, um **alltables.sql** lokal herunterzuladen.

    ```bash
    scp sshuser@CLUSTERNAME-ssh.azurehdinsight.net:alltables.sql c:/hdi
    ```

1. Laden Sie **alltables.sql** in den *neuen* HDInsight-Cluster hoch.

    ```bash
    scp c:/hdi/alltables.sql sshuser@CLUSTERNAME-ssh.azurehdinsight.net:/home/sshuser/
    ```

1. Stellen Sie dann mithilfe von SSH eine Verbindung mit dem *neuen* HDInsight-Cluster her. Führen Sie aus der SSH-Sitzung den folgenden Code aus:

    ```bash
    beeline -u "jdbc:hive2://localhost:10001/;transportMode=http" -i alltables.sql
    ```

## <a name="upgrade-metastore"></a>Aktualisieren des Metastores

Nachdem das **Kopieren** des Metastores abgeschlossen ist, führen Sie ein Schemaskript für die Aktualisierung unter [Skriptaktion](../hdinsight-hadoop-customize-cluster-linux.md) auf dem vorhandenen HDInsight 3.6-Cluster aus, um den neuen Metastore auf das Hive 3-Schema zu aktualisieren. Dadurch kann die Datenbank als HDInsight 4.0-Metastore angefügt werden.

Verwenden Sie die Werte in der Tabelle weiter unten. Ersetzen Sie `SQLSERVERNAME DATABASENAME USERNAME PASSWORD` durch die entsprechenden Werte für den **kopierten** Hive-Metastore, getrennt durch Leerzeichen. Lassen Sie bei der Angabe des SQL-Servernamens „.database.windows.net“ weg.

|Eigenschaft | Wert |
|---|---|
|Skripttyp|--Benutzerdefiniert|
|NAME|Hive-Upgrade|
|Bash-Skript-URI|`https://hdiconfigactions.blob.core.windows.net/hivemetastoreschemaupgrade/launch-schema-upgrade.sh`|
|Knotentyp(en)|Hauptknoten|
|Parameter|SQLSERVERNAME DATABASENAME USERNAME PASSWORD|

> [!Warning]  
> Das Upgrade zum Konvertieren des HDInsight 3.6-Metadatenschemas in das HDInsight 4.0-Schema kann nicht rückgängig gemacht.

Sie können das Upgrade überprüfen, indem Sie die folgende SQL-Abfrage für die Datenbank ausführen:

```sql
select * from dbo.version
```

## <a name="migrate-hive-tables-to-hdinsight-40"></a>Migrieren von Hive-Tabellen zu HDInsight 4.0

Nachdem Sie die obigen Schritte zum Migrieren des Hive-Metastores zu HDInsight 4.0 durchgeführt haben, können Sie die im Metastore gespeicherten Tabellen und Datenbanken im HDInsight 4.0-Cluster anzeigen, indem Sie `show tables` oder `show databases` im Cluster ausführen. Informationen zur Abfrageausführung in HDInsight 4.0-Clustern finden Sie unter [Abfrageausführung in verschiedenen HDInsight-Versionen](#query-execution-across-hdinsight-versions).

Auf die eigentlichen Daten in den Tabellen kann jedoch erst zugegriffen werden, wenn der Cluster Zugriff auf die erforderlichen Speicherkonten hat. Führen Sie die folgenden Schritte aus, um sicherzustellen, dass Ihr HDInsight 4.0-Cluster auf die gleichen Daten zugreifen kann wie der alte HDInsight 3.6-Cluster:

1. Ermitteln Sie das Azure-Speicherkonto der Tabelle bzw. Datenbank.

1. Wenn Ihr HDInsight 4.0-Cluster bereits ausgeführt wird, fügen Sie das Azure-Speicherkonto über Ambari an den Cluster an. Falls Sie den HDInsight 4.0-Cluster noch nicht erstellt haben, vergewissern Sie sich, dass das Azure-Speicherkonto als primäres oder sekundäres Clusterspeicherkonto angegeben ist. Weitere Informationen zum Hinzufügen von Speicherkonten zu HDInsight-Clustern finden Sie unter [Hinzufügen zusätzlicher Speicherkonten zu HDInsight](../hdinsight-hadoop-add-storage.md).

## <a name="deploy-new-hdinsight-40-and-connect-to-the-new-metastore"></a>Bereitstellen einer neuen HDInsight 4.0-Instanz und Herstellen der Verbindung mit dem neuen Metastore

Nachdem das Schema aktualisiert wurde, stellen Sie einen neuen HDInsight 4.0-Cluster bereit, und verbinden Sie den aktualisierten Metastore. Wenn Sie 4.0 bereits bereitgestellt haben, richten Sie es so ein, dass Sie über Ambari eine Verbindung mit dem Metastore herstellen können.

## <a name="run-schema-migration-script-from-hdinsight-40"></a>Ausführen des Schemaskripts für die Migration aus HDInsight 4.0

Tabellen werden in HDInsight 3.6 und HDInsight 4.0 unterschiedlich behandelt. Aus diesem Grund ist es nicht möglich, die gleichen Tabellen für Cluster verschiedener Versionen gemeinsam zu nutzen. Wenn Sie HDInsight 3.6 und HDInsight 4.0 gleichzeitig verwenden möchten, benötigen Sie für jede Version separate Kopien der Daten.

Ihre Hive-Workload kann eine Mischung von ACID- und Nicht-ACID-Tabellen (Atomicity, Consistency, Isolation, Durability = Atomarität, Konsistenz, Isolation, Dauerhaftigkeit) enthalten. Ein wesentlicher Unterschied zwischen Hive in HDInsight 3.6 (Hive 2) und Hive in HDInsight 4.0 (Hive 3) ist die ACID-Konformität für Tabellen. In HDInsight 3.6 erfordert Hive-ACID-Konformität eine zusätzliche Konfiguration, in HDInsight 4.0 sind Tabellen jedoch standardmäßig ACID-konform. Die einzige erforderliche Aktion vor der Migration ist das Durchführen einer größeren Komprimierung für die ACID-Tabelle im 3.6-Cluster. Führen Sie in der Hive-Ansicht oder in Beeline die folgende Abfrage aus:

```sql
alter table myacidtable compact 'major';
```

Diese Komprimierung ist erforderlich, weil ACID-Deltas von ACID-Tabellen in HDInsight 3.6 und HDInsight 4.0 unterschiedlich interpretiert werden. Die Komprimierung erzwingt einen kompletten Neuanfang, der Konsistenz garantiert. Abschnitt 4 der [Dokumentation zur Hive-Migration](https://docs.hortonworks.com/HDPDocuments/Ambari-2.7.3.0/bk_ambari-upgrade-major/content/prepare_hive_for_upgrade.html) enthält eine Anleitung für die Massenkomprimierung von ACID-Tabellen in HDInsight 3.6.

Nachdem Sie die Schritte zur Migration des Metastores und Komprimierung durchgeführt haben, können Sie das eigentliche Warehouse migrieren. Nach der Migration des Hive-Warehouse weist das HDInsight 4.0-Warehouse die folgenden Eigenschaften auf:

|3.6 |4,0 |
|---|---|
|Externe Tabellen|Externe Tabellen|
|Nicht transaktionale verwaltete Tabellen|Externe Tabellen|
|Transaktionale verwaltete Tabellen|Verwaltete Tabellen|

Möglicherweise müssen Sie die Eigenschaften Ihres Warehouse anpassen, bevor Sie die Migration durchführen. Wenn Sie beispielsweise davon ausgehen, dass Drittanwendungen oder -dienste (z. B. ein HDInsight 3.6-Cluster) auf eine Tabelle zugreifen, muss diese Tabelle nach Abschluss der Migration extern sein. In HDInsight 4.0 sind alle verwalteten Tabellen transaktional. Daher sollte nur von HDInsight 4.0-Clustern auf verwaltete Tabellen in HDInsight 4.0 zugegriffen werden.

Nachdem Sie Ihre Tabelleneigenschaften korrekt festgelegt haben, führen Sie das Migrationstool für Hive-Warehouses auf einem der Clusterhauptknoten über die SSH-Shell aus:

1. Stellen Sie über SSH eine Verbindung mit Ihrem Clusterhauptknoten her. Anweisungen hierzu finden Sie unter [Herstellen einer Verbindung mit HDInsight per SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).
1. Öffnen Sie als Hive-Benutzer eine Anmelde-Shell, indem Sie `sudo su - hive` ausführen.
1. Führen Sie `ls /usr/hdp` aus, um die Version des Datenplattformstapels zu ermitteln. In der Ausgabe wird eine Versionszeichenfolge angezeigt, die Sie im nächsten Befehl verwenden.
1. Führen Sie den folgenden Befehl über die Shell aus. Ersetzen Sie `STACK_VERSION` durch die Versionszeichenfolge aus dem vorherigen Schritt:

```bash
/usr/hdp/STACK_VERSION/hive/bin/hive --config /etc/hive/conf --service  strictmanagedmigration --hiveconf hive.strict.managed.tables=true -m automatic --modifyManagedTables
```

Nach Abschluss der Ausführung des Migrationstools ist Ihr Hive-Warehouse bereit für HDInsight 4.0.

> [!Important]  
> Auf verwaltete Tabellen in HDInsight 4.0 (einschließlich Tabellen, die von 3.6 migriert wurden) sollten keine anderen Dienste oder Anwendungen zugreifen, auch keine HDInsight 3.6-Cluster.

## <a name="secure-hive-across-hdinsight-versions"></a>Sichern von Hive in verschiedenen HDInsight-Versionen

Seit HDInsight 3.6 ist HDInsight in Azure Active Directory und das HDInsight-Enterprise-Sicherheitspaket (ESP) integriert. Das Enterprise-Sicherheitspaket verwendet Kerberos und Apache Ranger, um die Berechtigungen bestimmter Ressourcen im Cluster zu verwalten. Für Hive in HDInsight 3.6 bereitgestellte Ranger-Richtlinien können wie folgt zu HDInsight 4.0 migriert werden:

1. Navigieren Sie in Ihrem HDInsight 3.6-Cluster zum Bereich „Ranger Service Manager“.
2. Navigieren Sie zur Richtlinie namens **HIVE**, und exportieren Sie die Richtlinie in eine JSON-Datei.
3. Stellen Sie sicher, dass alle Benutzer, auf die in der JSON-Datei mit der exportierten Richtlinie verwiesen wird, im neuen Cluster vorhanden sind. Wenn in der JSON-Datei mit der Richtlinie auf einen Benutzer verwiesen wird, der nicht im neuen Cluster vorhanden ist, fügen Sie den Benutzer dem neuen Cluster hinzu, oder entfernen Sie den Verweis aus der Richtlinie.
4. Navigieren Sie in Ihrem HDInsight 4.0-Cluster zum Bereich **Ranger Service Manager**.
5. Navigieren Sie zur Richtlinie namens **HIVE**, und importieren Sie die in Schritt 2 erstellte JSON-Datei mit der Ranger-Richtlinie.

## <a name="check-compatibility-and-modify-codes-as-needed-in-test-app"></a>Überprüfen der Kompatibilität und Ändern von Codes nach Bedarf in der Test-App

Wenn Sie Workloads wie z.B. vorhandene Programme und Abfragen migrieren, überprüfen Sie die Versionshinweise und die Dokumentation auf Änderungen, und übernehmen Sie die Änderungen bei Bedarf. Wenn Ihr HDInsight 3.6-Cluster einen freigegebenen Spark- und Hive-Metastore verwendet, ist eine [zusätzliche Konfiguration mit Hive Warehouse Connector](./apache-hive-warehouse-connector.md) erforderlich.

## <a name="deploy-new-app-for-production"></a>Bereitstellen der neuen App für die Produktion

Um zum neuen Cluster zu wechseln, können Sie z.B. eine neue Clientanwendung installieren und als neue Produktionsumgebung verwenden oder ein Upgrade Ihrer vorhandenen Clientanwendung durchführen und zu HDInsight 4.0 wechseln.

## <a name="switch-hdinsight-40-to-the-production"></a>Wechseln von HDInsight 4.0 zur Produktionsversion

Wenn beim Testen im Metastore Unterschiede aufgetreten sind, müssen Sie die Änderungen unmittelbar vor der Umstellung aktualisieren. In diesem Fall können Sie den Metastore exportieren und importieren und dann das Upgrade erneut ausführen.

## <a name="remove-the-old-production"></a>Entfernen der alten Produktionsversion

Nachdem Sie sich vergewissert haben, dass das Release vollständig und betriebsbereit ist, können Sie Version 3.6 und den vorherigen Metastore entfernen. Stellen Sie sicher, dass alles migriert wurde, bevor Sie die Umgebung löschen.

## <a name="query-execution-across-hdinsight-versions"></a>Abfrageausführung in verschiedenen HDInsight-Versionen

Zum Ausführen und Debuggen von Hive-/LLAP-Abfragen in einem HDInsight 3.6-Cluster stehen zwei Möglichkeiten zur Verfügung. HiveCLI bietet eine Befehlszeilenumgebung, und die Tez-Ansicht/Hive-Ansicht stellt einen Workflow mit grafischer Benutzeroberfläche (Graphical User Interface, GUI) bereit.

In HDInsight 4.0 wurde HiveCLI durch Beeline ersetzt. HiveCLI ist ein Thrift-Client für Hive-Server 1, und Beeline ist ein JDBC-Client, der Zugriff auf Hive-Server 2 bietet. Beeline kann auch zum Herstellen einer Verbindung mit einem beliebigen anderen JDBC-kompatiblen Datenbankendpunkt verwendet werden. Beeline ist in HDInsight 4.0 standardmäßig verfügbar und erfordert keinerlei Installation.

In HDInsight 3.6 ist die Ambari-Hive-Ansicht der GUI-Client für die Interaktion mit dem Hive-Server. HDInsight 4.0 wird nicht mit Ambari View ausgeliefert. Wir haben unseren Kunden eine Möglichkeit zur Verfügung gestellt, Data Analytics Studio (DAS) zu verwenden, bei dem es sich nicht um einen HDInsight-Kerndienst handelt. DAS ist nicht standardmäßig in HDInsight-Clustern enthalten und kein offiziell unterstütztes Paket. DAS kann jedoch mit einer [Skriptaktion](../hdinsight-hadoop-customize-cluster-linux.md) wie folgt auf dem Cluster installiert werden:

|Eigenschaft | Wert |
|---|---|
|Skripttyp|--Benutzerdefiniert|
|NAME|DAS|
|Bash-Skript-URI|`https://hdiconfigactions.blob.core.windows.net/dasinstaller/LaunchDASInstaller.sh`|
|Knotentyp(en)|Hauptknoten|

Warten Sie 5 bis 10 Minuten, und starten Sie dann Data Analytics Studio mit der folgenden URL: `https://CLUSTERNAME.azurehdinsight.net/das/`.

Falls die von Ihnen ausgeführten Abfragen nach der Installation von DAS nicht in der Abfrageanzeige angezeigt werden, führen Sie die folgenden Schritte aus:

1. Legen Sie die Konfigurationen für Hive, Tez und DAS wie in diesem [Leitfaden zur Problembehandlung für die DAS-Installation](https://docs.hortonworks.com/HDPDocuments/DAS/DAS-1.2.0/troubleshooting/content/das_queries_not_appearing.html) beschrieben fest.
2. Stellen Sie sicher, dass die folgenden Azure-Speicherverzeichniskonfigurationen Seitenblobs und unter `fs.azure.page.blob.dirs` aufgelistet sind:
    * `hive.hook.proto.base-directory`
    * `tez.history.logging.proto-base-dir`
3. Starten Sie auf beiden Hauptknoten Hadoop Distributed File System, Hive, Tez und DAS neu.

## <a name="next-steps"></a>Nächste Schritte

* [Ankündigung von HDInsight 4.0](../hdinsight-version-release.md)
* [HDInsight 4.0 deep dive](https://azure.microsoft.com/blog/deep-dive-into-azure-hdinsight-4-0/) (Ausführliche Informationen zu HDInsight 4.0)
* [Hive 3-ACID-Tabellen](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.1.0/using-hiveql/content/hive_3_internals.html)
