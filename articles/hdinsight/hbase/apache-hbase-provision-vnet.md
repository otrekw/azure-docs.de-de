---
title: Erstellen von HBase-Clustern in einem virtuellen Netzwerk – Azure
description: Erste Schritte mit HBase in Azure HDInsight. Erfahren Sie, wie Sie HDInsight HBase-Cluster in Azure Virtual Network erstellen können.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 12/23/2019
ms.openlocfilehash: 82e3374491aa119d9985ea7ef31e180c920511d3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86087740"
---
# <a name="create-apache-hbase-clusters-on-hdinsight-in-azure-virtual-network"></a>Erstellen von Apache HBase-Clustern in HDInsight in Azure Virtual Network

Hier erfahren Sie, wie Sie Apache HBase-Cluster in Azure HDInsight in einem [virtuellen Azure-Netzwerk](https://azure.microsoft.com/services/virtual-network/) erstellen.

Mit der Integration in virtuelle Netzwerke können Apache HBase-Cluster im selben virtuellen Netzwerk bereitgestellt werden wie Ihre Anwendungen, sodass Anwendungen direkt mit HBase kommunizieren können. Es ergeben sich folgende Vorteile:

* Direkte Konnektivität der Webanwendung mit den Knoten des HBase-Clusters, wodurch die Kommunikation über HBase Java-RPC-APIs (Remoteprozeduraufrufe) ermöglicht wird.
* Optimierte Leistung, da Datenverkehr nicht über mehrere Gateways und Lastenausgleichsmodule fließt.
* Möglichkeit einer höheren Sicherheit bei der Verarbeitung sensibler Informationen ohne exponierten öffentlichen Endpunkt.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-apache-hbase-cluster-into-virtual-network"></a>Erstellen von Apache HBase-Clustern in einem virtuellen Netzwerk

In diesem Abschnitt erstellen Sie mit einer [Azure Resource Manager-Vorlage](../../azure-resource-manager/templates/deploy-powershell.md) einen Linux-basierten Apache HBase-Cluster mit dem abhängigen Azure Storage-Konto in einem virtuellen Azure-Netzwerk. Andere Methoden zur Erstellung von Clustern und Informationen zu den Einstellungen finden Sie unter [Erstellen von Linux-basierten Hadoop-Clustern in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md). Weitere Informationen zur Verwendung einer Vorlage zum Erstellen von Apache Hadoop-Clustern in HDInsight finden Sie unter [Erstellen Linux-basierter Apache Hadoop-Cluster in HDInsight mithilfe von Azure Resource Manager-Vorlagen](../hdinsight-hadoop-create-linux-clusters-arm-templates.md).

> [!NOTE]  
> Einige Eigenschaften sind in der Vorlage hartcodiert. Beispiel:
>
> * **Standort**: USA (Ost) 2
> * **Clusterversion:** 3.6
> * **Anzahl der Workerknoten im Cluster:** 2
> * **Standard-Speicherkonto**: eine eindeutige Zeichenfolge
> * **Name des virtuellen Netzwerks:** CLUSTERNAME-vnet
> * **Adressraum des virtuellen Netzwerks:** 10.0.0.0/16
> * **Subnetzname**: subnet1
> * **Subnetzadressbereich:** 10.0.0.0/24
>
> `CLUSTERNAME` wird durch den Clusternamen ersetzt, den Sie bei Verwendung der Vorlage angeben.

1. Wählen Sie die folgende Abbildung aus, um die Vorlage im Azure-Portal zu öffnen. Die Vorlage finden Sie unter [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-linux-vnet/).

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux-vnet%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hbase-provision-vnet/hdi-deploy-to-azure1.png" alt="Deploy to Azure button for new cluster"></a>

1. Wählen Sie im Dialogfeld **Benutzerdefinierte Bereitstellung** die Option **Vorlage bearbeiten** aus.

1. Ändern Sie in Zeile 165 den Wert `Standard_A3` in `Standard_A4_V2`. Klicken Sie dann auf **Speichern**.

1. Füllen Sie die übrige Vorlage mit folgenden Informationen aus:

    |Eigenschaft |Wert |
    |---|---|
    |Subscription|Wählen Sie ein Azure-Abonnement aus, um den HDInsight-Cluster, das abhängige Speicherkonto und das virtuelle Azure-Netzwerk zu erstellen.|
    Resource group|Wählen Sie **Neu erstellen** aus, und geben Sie einen neuen Ressourcengruppennamen ein.|
    |Position|Wählen Sie einen Speicherort für die Ressourcengruppe aus.|
    |Clustername|Geben Sie einen Namen für den zu erstellenden Hadoop-Cluster ein.|
    |Benutzername und Kennwort für Clusteranmeldung|Der Standardbenutzername lautet **admin**. Geben Sie ein Kennwort an.|
    |SSH-Benutzername und -Kennwort|Der Standardbenutzername lautet **sshuser**.  Geben Sie ein Kennwort an.|

    Wählen Sie **Ich stimme den oben genannten Geschäftsbedingungen zu** aus.

1. Wählen Sie die Option **Kaufen**. Das Erstellen eines Clusters dauert ca. 20 Minuten. Sobald der Cluster erstellt wurde, können Sie ihn durch Auswählen im Portal öffnen.

Nach Abschluss des Artikels ist es ratsam, den Cluster zu löschen. Mit HDInsight werden Ihre Daten im Azure-Speicher gespeichert, sodass Sie einen Cluster problemlos löschen können, wenn er nicht verwendet wird. Für einen HDInsight-Cluster fallen auch dann Gebühren an, wenn er nicht verwendet wird. Da die Gebühren für den Cluster erheblich höher sind als die Kosten für den Speicher, ist es sinnvoll, nicht verwendete Cluster zu löschen. Anweisungen zum Löschen eines Clusters finden Sie unter [Verwalten von Windows-basierten Apache Hadoop-Clustern in HDInsight mit dem Azure-Portal](../hdinsight-administer-use-portal-linux.md#delete-clusters).

Führen Sie die Schritte unter [Erste Schritte mit einem Apache HBase-Beispiel in HDInsight](./apache-hbase-tutorial-get-started-linux.md) aus, um mit Ihrem neu erstellten HBase-Cluster zu arbeiten.

## <a name="connect-to-the-apache-hbase-cluster-using-apache-hbase-java-rpc-apis"></a>Herstellen einer Verbindung mit dem Apache HBase-Cluster mithilfe von Apache HBase-Java-RPC-APIs

### <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers

Erstellen Sie einen virtuellen IaaS-Computer (Infrastructure-as-a-Service) im gleichen virtuellen Azure-Netzwerk und im gleichen Subnetz. Anweisungen zum Erstellen eines neuen virtuellen IaaS-Computers finden Sie im Abschnitt zum [Erstellen eines virtuellen Computers unter Windows Server](../../virtual-machines/windows/quick-create-portal.md). Beim Ausführen der in diesem Dokument beschriebenen Schritte müssen Sie die folgenden Werte für die Netzwerkkonfiguration verwenden:

* **Virtuelles Netzwerk:** CLUSTERNAME-vnet
* **Subnetz**: subnet1

> [!IMPORTANT]  
> Ersetzen Sie `CLUSTERNAME` durch den Namen, den Sie beim Erstellen des HDInsight-Clusters in den vorherigen Schritten verwendet haben.

Mit diesen Werten wird der virtuelle Computer im selben virtuellen Netzwerk und Subnetz platziert wie der HDInsight-Cluster. Diese Konfiguration ermöglicht eine direkte Kommunikation zwischen virtuellem Computer und Cluster. Es gibt eine Möglichkeit zum Erstellen eines HDInsight-Clusters mit einem leeren Edgeknoten. Der Edgeknoten kann zum Verwalten des Clusters verwendet werden.  Weitere Informationen finden Sie unter [Use empty edge nodes in HDInsight](../hdinsight-apps-use-edge-node.md)(Verwenden leerer Edgeknoten in HDInsight).

### <a name="obtain-fully-qualified-domain-name"></a>Abrufen des vollqualifizierten Domänennamens

Für Remote-Verbindungen zwischen Java-Anwendungen und HBase müssen Sie den vollqualifizierten Domänennamen (FQDN) verwenden. Rufen Sie hierzu das verbindungsspezifische DNS-Suffix des HBase-Clusters ab. Dazu können Sie eine der folgenden Methoden verwenden:

* Ausführen eines [Apache Ambari](https://ambari.apache.org/)-Aufrufs mithilfe eines Webbrowsers:

    Navigieren Sie zu `https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts?minimal_response=true`. Es wird eine JSON-Datei mit den DNS-Suffixen zurückgegeben.

* Verwenden der Ambari-Website:

    1. Navigieren Sie zu `https://CLUSTERNAME.azurehdinsight.net`.
    2. Wählen Sie auf der oberen Menüleiste die Option **Hosts** aus.

* Durchführen von REST-Aufrufen mithilfe von Curl:

    ```bash
    curl -u <username>:<password> -k https://CLUSTERNAME.azurehdinsight.net/ambari/api/v1/clusters/CLUSTERNAME.azurehdinsight.net/services/hbase/components/hbrest
    ```

Suchen Sie in den zurückgegebenen JSON-Daten (JavaScript Object Notation) den Eintrag "host_name". Er enthält den vollqualifizierten Domänennamen (FQDN) für die Knoten im Cluster. Beispiel:

```
"host_name" : "hn0-hbaseg.hjfrnszlumfuhfk4pi1guh410c.bx.internal.cloudapp.net"
```

Der Teil des Domänennamens, der mit dem Clusternamen beginnt, ist das DNS-Suffix. Beispiel: `hjfrnszlumfuhfk4pi1guh410c.bx.internal.cloudapp.net`.

<!--
3.    Change the primary DNS suffix configuration of the virtual machine. This enables the virtual machine to automatically resolve the host name of the HBase cluster without explicit specification of the suffix. For example, the *workernode0* host name will be correctly resolved to workernode0 of the HBase cluster.

    To make the configuration change:

    1. RDP into the virtual machine.
    2. Open **Local Group Policy Editor**. The executable is gpedit.msc.
    3. Expand **Computer Configuration**, expand **Administrative Templates**, expand **Network**, and then click **DNS Client**.
    - Set **Primary DNS Suffix** to the value obtained in step 2:

        ![hdinsight.hbase.primary.dns.suffix](./media/apache-hbase-provision-vnet/hdi-primary-dns-suffix.png)
    4. Click **OK**.
    5. Reboot the virtual machine.
-->

### <a name="verify-communication-inside-virtual-network"></a>Überprüfen der Kommunikation innerhalb des virtuellen Netzwerks

Führen Sie den Befehl `ping headnode0.<dns suffix>` auf dem virtuellen Computer aus, um zu überprüfen, ob der virtuelle Computer mit dem HBase-Cluster kommunizieren kann. Beispiel: `ping hn0-hbaseg.hjfrnszlumfuhfk4pi1guh410c.bx.internal.cloudapp.net`.

Führen Sie die unter [Erstellen von Java-Anwendungen für die Apache-HBase](./apache-hbase-build-java-maven-linux.md) beschriebenen Schritte aus, um diese Informationen in einer Java-Anwendung zu verwenden. Wenn die Anwendung eine Verbindung mit einem HBase-Remoteserver herstellen soll, müssen Sie den FQDN für Zookeeper in der Datei **hbase-site.xml** eintragen. Beispiel:

```xml
<property>
    <name>hbase.zookeeper.quorum</name>
    <value>zookeeper0.<dns suffix>,zookeeper1.<dns suffix>,zookeeper2.<dns suffix></value>
</property>
```

> [!NOTE]  
> Weitere Informationen zur Namensauflösung in virtuellen Azure-Netzwerken und Verwendung eigener DNS-Server finden Sie unter [Namensauflösung (DNS)](../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie Sie einen Apache HBase-Cluster erstellen. Weitere Informationen finden Sie unter:

* [Erste Schritte mit HDInsight](../hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Verwenden leerer Edgeknoten in HDInsight](../hdinsight-apps-use-edge-node.md)
* [Konfigurieren der Apache HBase-Replikation in HDInsight](apache-hbase-replication.md)
* [Erstellen von Apache Hadoop-Clustern in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md)
* [Erste Schritte mit einem Apache HBase-Beispiel in HDInsight](./apache-hbase-tutorial-get-started-linux.md)
* [Virtuelle Netzwerke im Überblick](../../virtual-network/virtual-networks-overview.md)
