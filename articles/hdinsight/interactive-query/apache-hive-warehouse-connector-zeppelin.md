---
title: Hive Warehouse Connector-Integration mit Apache Zeppelin mithilfe von Livy in Azure HDInsight
description: Hier erfahren Sie, wie Sie Hive Warehouse Connector mit Apache Zeppelin in Azure HDInsight integrieren.
author: nis-goel
ms.author: nisgoel
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/22/2020
ms.openlocfilehash: 1f9d2d9bd2a58fa4c6f14db8ffd067bb39fc1553
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2020
ms.locfileid: "83853712"
---
# <a name="integrate-apache-zeppelin-with-hive-warehouse-connector-in-azure-hdinsight"></a>Integrieren von Apache Zeppelin mit Hive Warehouse Connector in Azure HDInsight

HDInsight Spark-Cluster enthalten Apache Zeppelin-Notebooks mit verschiedenen Interpretern. In diesem Artikel wird überwiegend der Livy-Interpreter für den Zugriff auf Hive-Tabellen über Spark mit Hive Warehouse Connector behandelt.

## <a name="prerequisite"></a>Voraussetzung

Schließen Sie die Schritte für das [Hive Warehouse Connector-Setup](apache-hive-warehouse-connector.md#hive-warehouse-connector-setup) ab.

## <a name="getting-started"></a>Erste Schritte

1. Verwenden Sie den Befehl [ssh](../hdinsight-hadoop-linux-use-ssh-unix.md), um eine Verbindung mit Ihrem Apache Spark-Cluster herzustellen. Bearbeiten Sie den folgenden Befehl, indem Sie CLUSTERNAME durch den Namen Ihres Clusters ersetzen, und geben Sie den Befehl dann ein:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Führen Sie in Ihrer SSH-Sitzung den folgenden Befehl aus, und notieren Sie sich die Versionen von `hive-warehouse-connector-assembly` und `pyspark_hwc`:

    ```bash
    ls /usr/hdp/current/hive_warehouse_connector
    ```

    Speichern Sie die Ausgabe zur späteren Verwendung für die Konfiguration von Apache Zeppelin.

## <a name="configure-livy"></a>Konfigurieren von Livy

Die folgenden Konfigurationen sind für den Zugriff auf Hive-Tabellen über Zeppelin mit dem Livy-Interpreter erforderlich.

### <a name="interactive-query-cluster"></a>Interactive Query-Cluster

1. Öffnen Sie `https://LLAPCLUSTERNAME.azurehdinsight.net/#/main/services/HDFS/configs` in einer Webbrowser. Ersetzen Sie LLAPCLUSTERNAME dabei durch den Namen Ihres Interactive Query-Clusters.

1. Navigieren Sie zu **Advanced** > **Custom core-site** (Erweitert > Benutzerdefinierte Hauptwebsite). Klicken Sie auf **Add Property...** (Eigenschaft hinzufügen), um die folgenden Konfigurationen hinzuzufügen:

    | Konfiguration                 | Wert |
    | ----------------------------- |-------|
    | hadoop.proxyuser.livy.groups  | *     |
    | hadoop.proxyuser.livy.hosts   | *     |

1. Speichern Sie die Änderungen, und starten Sie alle betroffenen Komponenten neu.

### <a name="spark-cluster"></a>Spark-Cluster

1. Öffnen Sie `https://CLUSTERNAME.azurehdinsight.net/#/main/services/SPARK2/configs` in einem Webbrowser. Ersetzen Sie dabei CLUSTERNAME durch den Namen Ihres Apache Spark-Clusters.

1. Erweitern Sie **Custom livy2-conf**. Klicken Sie auf **Add Property...** (Eigenschaft hinzufügen), um die folgende Konfiguration hinzuzufügen:

    | Konfiguration                 | Wert                                      |
    | ----------------------------- |------------------------------------------  |
    | livy.file.local-dir-whitelist | /usr/hdp/current/hive_warehouse_connector/ |

1. Speichern Sie die Änderungen, und starten Sie alle betroffenen Komponenten neu.

### <a name="configure-livy-interpreter-in-zeppelin-ui-spark-cluster"></a>Konfigurieren des Livy-Interpreters über die Zeppelin-Benutzeroberfläche (Spark-Cluster)

1. Öffnen Sie `https://CLUSTERNAME.azurehdinsight.net/zeppelin/#/interpreter` in einem Webbrowser. Ersetzen Sie dabei `CLUSTERNAME` durch den Namen Ihres Apache Spark-Clusters.

1. Navigieren Sie zu **livy2**.

1. Fügen Sie die folgenden Konfigurationen hinzu:

    | Konfiguration                 | Wert                                      |
    | ----------------------------- |:------------------------------------------:|
    | livy.spark.hadoop.hive.llap.daemon.service.hosts | @llap0 |
    | livy.spark.security.credentials.hiveserver2.enabled | true |
    | livy.spark.sql.hive.llap | true |
    | livy.spark.yarn.security.credentials.hiveserver2.enabled | true |
    | livy.superusers | livy,zeppelin |
    | livy.spark.jars | `file:///usr/hdp/current/hive_warehouse_connector/hive-warehouse-connector-assembly-VERSION.jar`.<br>Ersetzen Sie VERSION durch den Wert, den Sie zuvor im Abschnitt [Erste Schritte](#getting-started) abgerufen haben. |
    | livy.spark.submit.pyFiles | `file:///usr/hdp/current/hive_warehouse_connector/pyspark_hwc-VERSION.zip`.<br>Ersetzen Sie VERSION durch den Wert, den Sie zuvor im Abschnitt [Erste Schritte](#getting-started) abgerufen haben. |
    | livy.spark.sql.hive.hiveserver2.jdbc.url | Legen Sie für diese Konfiguration die HiveServer2 Interactive JDBC-URL des Interactive Query-Clusters fest. |
    | spark.security.credentials.hiveserver2.enabled | true |

1. Fügen Sie nur für ESP-Cluster die folgende Konfiguration hinzu:

    | Konfiguration| Wert|
    |---|---|
    | livy.spark.sql.hive.hiveserver2.jdbc.url.principal | `hive/<headnode-FQDN>@<AAD-Domain>` |

    Ersetzen Sie `<headnode-FQDN>` durch den vollqualifizierten Domänennamen des Hauptknotens des Interactive Query-Clusters.
    Ersetzen Sie `<AAD-DOMAIN>` durch den Namen der Azure Active Directory-Instanz (AAD), der der Cluster angehört. Verwenden Sie eine Zeichenfolge aus Großbuchstaben für den Wert `<AAD-DOMAIN>`, da die Anmeldeinformationen sonst nicht gefunden werden. Überprüfen Sie `/etc/krb5.conf` bei Bedarf auf die Bereichsnamen.

1. Speichern Sie die Änderungen, und starten Sie den Livy-Interpreter neu.

Wenn der Livy-Interpreter nicht verfügbar ist, passen Sie die `shiro.ini`-Datei an, die in der Zeppelin-Komponente in Ambari vorhanden ist. Weitere Informationen finden Sie unter [Konfigurieren der Apache Zeppelin-Sicherheit](https://docs.cloudera.com/HDPDocuments/HDP3/HDP-3.0.1/configuring-zeppelin-security/content/enabling_access_control_for_interpreter__configuration__and_credential_settings.html).  


## <a name="running-queries-in-zeppelin"></a>Ausführen von Abfragen in Zeppelin 

Starten Sie ein Zeppelin-Notebook mithilfe des Livy-Interpreters, und führen Sie Folgendes aus:

```python
%livy2

import com.hortonworks.hwc.HiveWarehouseSession
import com.hortonworks.hwc.HiveWarehouseSession._
import org.apache.spark.sql.SaveMode

# Initialize the hive context
val hive = HiveWarehouseSession.session(spark).build()

# Create a database
hive.createDatabase("hwc_db",true)
hive.setDatabase("hwc_db")

# Create a Hive table
hive.createTable("testers").ifNotExists().column("id", "bigint").column("name", "string").create()

val dataDF = Seq( (1, "foo"), (2, "bar"), (8, "john")).toDF("id", "name")

# Validate writes to the table
dataDF.write.format("com.hortonworks.spark.sql.hive.llap.HiveWarehouseConnector").mode("append").option("table", "hwc_db.testers").save()

# Validate reads
hive.executeQuery("select * from testers").show()

```

## <a name="next-steps"></a>Nächste Schritte

* [HWC und Apache Spark-Vorgänge](./apache-hive-warehouse-connector-operations.md)
* [HWC-Integration mit Apache Spark und Apache Hive](./apache-hive-warehouse-connector.md)
* [Verwenden von Interactive Query mit HDInsight](./apache-interactive-query-get-started.md)