---
title: Erstellen von Apache Hadoop-Clustern mit einem Webbrowser – Azure HDInsight
description: Erfahren Sie, wie Sie mit einem Webbrowser und dem Azure-Portal Apache Hadoop-, Apache HBase-, Apache Storm- oder Apache Spark-Cluster unter Linux für HDInsight erstellen.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/13/2019
ms.openlocfilehash: 820ddb8d06cfd2aac2b053305f23ad330e4fd7c3
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74215901"
---
# <a name="create-linux-based-clusters-in-hdinsight-by-using-the-azure-portal"></a>Erstellen von Linux-basierten Clustern in HDInsight mit dem Azure-Portal

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Das Azure-Portal ist ein webbasiertes Verwaltungstool für Dienste und Ressourcen, die in der Microsoft Azure-Cloud gehostet werden. In diesem Artikel wird beschrieben, wie Sie mit dem Portal Linux-basierte Azure HDInsight-Cluster erstellen.

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Voraussetzungen

* **Ein Azure-Abonnement**. Weitere Informationen finden Sie unter [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) (Gewusst wie: Verwenden einer kostenlosen Azure-Testversion für Hadoop in HDInsight).
* **Ein zeitgemäßer Webbrowser**. Im Azure-Portal werden HTML5 und JavaScript verwendet. In älteren Webbrowsern funktioniert dies unter Umständen nicht richtig.

## <a name="create-clusters"></a>Erstellen von Clustern

Das Azure-Portal macht die meisten Clustereigenschaften verfügbar. Mithilfe von Azure Resource Manager-Vorlagen können Sie viele Details ausblenden. Weitere Informationen finden Sie unter [Erstellen von Apache Hadoop-Clustern in HDInsight mit Resource Manager-Vorlagen](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Navigieren Sie im Menü auf der linken Seite zu **+ Ressource erstellen** >  **Analytics** > **Azure HDInsight**.

    ![Erstellen eines neuen Clusters im Azure-Portal](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster.png "Erstellen eines neuen Clusters im Azure-Portal")

1. Wählen Sie auf der Seite **HDInsight-Cluster erstellen** die Option **Zur klassischen Benutzeroberfläche für die Erstellung wechseln** aus.

    ![Zur klassischen Benutzeroberfläche für die Erstellung wechseln](./media/hdinsight-hadoop-create-linux-clusters-portal/azure-portal-cluster-create-classic.png)

1. Wählen Sie auf der Seite **HDInsight** die Option **Benutzerdefiniert (Größe, Einstellungen, Apps)** .

1. Wählen Sie **1 Grundlagen**. Geben Sie anschließend die folgenden Informationen ein:

    |Eigenschaft |BESCHREIBUNG |
    |---|---|
    |Clustername|Dieser Name muss global eindeutig sein.|
    |Subscription|Wählen Sie in der Dropdownliste das Azure-Abonnement aus, das für den Cluster verwendet wird.|
    |Clustertyp|Wählen Sie den Typ von Cluster aus, den Sie erstellen möchten. Beispiele hierfür sind Hadoop und Apache Spark. Das **Betriebssystem** ist **Linux**. Wählen Sie als Nächstes eine Version für den Clustertyp aus. Verwenden Sie die Standardversion, wenn Sie nicht wissen, was Sie auswählen sollen. Weitere Informationen finden Sie unter [HDInsight-Clusterversionen](hdinsight-component-versioning.md).|
    |Benutzername für Clusteranmeldung|Geben Sie den Benutzernamen an, der Standardwert ist **admin**.|
    |Kennwort für Clusteranmeldung|Geben Sie das Kennwort an.|
    |SSH-Benutzername (Secure Shell)|Der Standardwert ist **sshuser**. Aktivieren Sie das Kontrollkästchen **Clusteranmeldekennwort für SSH verwenden**, wenn Sie möchten, dass das SSH-Kennwort mit dem zuvor angegebenen Administratorkennwort identisch ist. Falls nicht, müssen Sie entweder ein **KENNWORT** oder einen **ÖFFENTLICHEN SCHLÜSSEL** zum Authentifizieren des SSH-Benutzers eingeben. Ein öffentlicher Schlüssel ist der von uns empfohlene Ansatz. Wählen Sie unten die Option **Auswählen**, um die Konfiguration der Anmeldeinformationen zu speichern.  Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit HDInsight (Hadoop) per SSH](hdinsight-hadoop-linux-use-ssh-unix.md).|
    |Resource group|Geben Sie an, ob Sie eine neue Ressourcengruppe erstellen oder eine vorhandene Ressourcengruppe verwenden möchten.|
    |Location|Geben Sie ein Rechenzentrum an, in dem der Cluster erstellt wird.|

    ![HDInsight: Erstellen von Clustergrundlagen](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-basics.png "Erstellen eines neuen Clusters im Azure-Portal")

    > [!IMPORTANT]  
    > Es gibt unterschiedliche Typen von HDInsight-Clustern. Diese entsprechen der Workload oder Technologie, für die der Cluster optimiert wurde. Es gibt keine unterstützte Methode zum Erstellen eines Clusters, in dem mehrere Typen kombiniert sind. Ein Beispiel hierfür ist die gemeinsame Verwendung von Storm und HBase in einem Cluster.

    Klicken Sie auf **Weiter**, um zur nächsten Seite zu wechseln.

1. Unter **2 Sicherheit + Netzwerkbetrieb** können Sie Ihren Cluster über das Dropdownmenü mit einem virtuellen Netzwerk verbinden. Wählen Sie ein virtuelles Azure-Netzwerk und das Subnetz aus, wenn Sie den Cluster in einem virtuellen Netzwerk platzieren möchten. Weitere Informationen zur Verwendung von HDInsight mit einem virtuellen Netzwerk finden Sie unter [Plan a virtual network for Azure HDInsight (Planen eines virtuellen Netzwerks für Azure HDInsight)](hdinsight-plan-virtual-network-deployment.md). Der Artikel enthält die spezifischen Konfigurationsanforderungen für das virtuelle Netzwerk.

    Befolgen Sie diese Anleitung, wenn Sie das **Enterprise-Sicherheitspaket** verwenden möchten: [Konfigurieren eines HDInsight-Clusters mit Enterprise-Sicherheitspaket (Enterprise Security Package, ESP) mithilfe von Azure Active Directory Domain Services](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds).

    Klicken Sie auf **Weiter**, um zur nächsten Seite zu wechseln.

1. Geben Sie unter **3 Speicher** für **Speicherkontoeinstellungen** an, ob Azure Storage oder Azure Data Lake Storage als Standardspeicher verwendet werden soll. Ausführlichere Informationen finden Sie in der unten stehenden Tabelle.

    | Primärer Speichertyp | BESCHREIBUNG |
    |------------------|-------------|
    | Azure Storage   |  * Wählen Sie unter **Auswahlmethode** die Option **Meine Abonnements**, wenn Sie ein Speicherkonto angeben möchten, das Teil Ihres Azure-Abonnements ist. Wählen Sie anschließend das Speicherkonto aus. Wählen Sie andernfalls die Option **Zugriffsschlüssel**. Geben Sie dann die Informationen für das nicht in Ihrem Azure-Abonnement enthaltene Speicherkonto an, das Sie auswählen möchten.</br></br> * Wählen Sie unter **Standardcontainer** den vom Portal vorgeschlagenen Standardcontainernamen oder einen eigenen Namen.</br></br> * Wenn Sie Azure-Blobspeicher als Standardspeicher verwenden, können Sie auch **Zusätzliche Speicherkonten** auswählen, um dem Cluster weitere Speicherkonten zuzuordnen. Wählen Sie unter **Azure-Speicherschlüssel** die Option**Speicherschlüssel hinzufügen**. Anschließend können Sie ein Speicherkonto aus Ihren Azure-Abonnements oder anderen Abonnements bereitstellen. Geben Sie den Zugriffsschlüssel für das Speicherkonto an.</br></br> * Wenn Sie Blobspeicher als Standardspeicher verwenden, können Sie auch die Option **Data Lake Storage-Zugriff** wählen, um Azure Data Lake Storage als zusätzlichen Speicher anzugeben. Weitere Informationen finden Sie unter [Quickstart: Einrichten von Clustern in HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).</li></ul> |
    | Azure Data Lake Store | Wählen Sie **Azure Data Lake Storage Gen1** oder **Azure Data Lake Storage Gen2** aus. Lesen Sie anschließend den Artikel [Schnellstart: Einrichten von Clustern in HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md), um entsprechende Anweisungen zu erhalten. |

    **Metastore-Einstellungen (optional)**

    Optional können Sie eine SQL-Datenbank angeben, die zum Speichern von Apache Hive- und Apache Oozie-Metadaten für den Cluster verwendet werden soll. Wählen Sie unter **SQL-Datenbank für Hive auswählen** eine SQL-Datenbank aus. Geben Sie anschließend den Benutzernamen und das Kennwort für die Datenbank an. Wiederholen Sie diese Schritte für Oozie-Metadaten.

    Hier sind einige zu berücksichtigende Punkte zur Verwendung von Azure SQL-Datenbank für Metastores angegeben:
    * Die für den Metastore verwendete Azure SQL-Datenbank muss für die Konnektivität mit anderen Azure-Diensten, z.B. Azure HDInsight, konfiguriert sein. Klicken Sie im Dashboard der Azure SQL-Datenbank auf der rechten Seite auf den Servernamen. Auf diesem Server wird die SQL-Datenbank-Instanz ausgeführt. Wählen Sie in der Serveransicht die Option **Konfigurieren**. Wählen Sie unter **Azure-Dienste** die Option **Ja**. Klicken Sie dann auf **Speichern**.
    * Verwenden Sie beim Erstellen eines Metastore für die Namen einer Datenbank keine Binde- oder Gedankenstriche. Diese Zeichen können dazu führen, dass der Prozess der Clustererstellung fehlschlägt.

    ![HDInsight: Erstellen von Clusterspeicher](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-storage.png "Erstellen eines neuen Clusters im Azure-Portal")

    > [!WARNING]  
    > Die Verwendung eines zusätzlichen Speicherkontos an einem anderen Ort als im HDInsight-Cluster wird nicht unterstützt.

    Klicken Sie auf **Weiter**, um zur nächsten Seite zu wechseln.

1. Wählen Sie unter **4 Anwendungen (optional)** alle gewünschten Anwendungen aus. Diese Anwendungen können von Microsoft, unabhängigen Softwareherstellern (Independent Software Vendors, ISVs) oder Ihnen selbst entwickelt werden. Weitere Informationen finden Sie unter [Installieren von Anwendungen während der Clustererstellung](hdinsight-apps-install-applications.md#install-applications-during-cluster-creation).

    Klicken Sie auf **Weiter**, um zur nächsten Seite zu wechseln.

1. Unter **5 Clustergröße** finden Sie Informationen zu den Knoten, die für diesen Cluster verwendet werden. Legen Sie die Anzahl von Workerknoten fest, die Sie für den Cluster benötigen. Die geschätzten Kosten der Clusterausführung werden ebenfalls angezeigt.

    ![HDInsight: Erstellen von Clusterknoten](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-nodes.png "Angeben der Anzahl der Clusterknoten")

   > [!IMPORTANT]  
   > Wenn Sie mehr als 32 Workerknoten planen, sollten Sie eine Hauptknotengröße mit mindestens 8 Kernen und 14 GB Arbeitsspeicher (RAM) auswählen. Planen Sie die Knoten entweder während der Clustererstellung oder durch das Skalieren des Clusters nach der Erstellung.
   >
   > Weitere Informationen zu Knotengrößen und den damit verbundenen Kosten finden Sie unter [Azure HDInsight – Preise](https://azure.microsoft.com/pricing/details/hdinsight/).

    Klicken Sie auf **Weiter**, um zur nächsten Seite zu wechseln.

1. Unter **6 Skriptaktionen** können Sie einen Cluster für die Installation von benutzerdefinierten Komponenten anpassen. Diese Option ist geeignet, wenn Sie einen Cluster bei seiner Erstellung mit einem benutzerdefinierten Skript anpassen möchten. Weitere Informationen zu Skriptaktionen finden Sie unter [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).

   Klicken Sie auf **Weiter**, um zur nächsten Seite zu wechseln.

1. Überprüfen Sie unter **7 Zusammenfassung** die Informationen, die Sie zuvor eingegeben haben. Klicken Sie anschließend auf **Erstellen**.

    ![HDInsight: Zusammenfassung der Clustererstellung](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-summary.png "Angeben der Anzahl der Clusterknoten")

    > [!NOTE]  
    > Die Erstellung des Clusters dauert in der Regel ca. 20 Minuten. Unter **Benachrichtigungen** können Sie den Bereitstellungsprozess überprüfen.

1. Wählen Sie nach Abschluss des Erstellungsprozesses in der Benachrichtigung **Bereitstellung erfolgreich** die Option **Zu Ressource wechseln**. Im Clusterfenster werden die folgenden Informationen angezeigt.

    ![HDI Azure-Portal: Clusterübersicht](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-completed.png "Clustereigenschaften")

    Einige der Symbole im Fenster stehen für Folgendes:

    |Eigenschaft | BESCHREIBUNG |
    |---|---|
    |Übersicht|Enthält alle grundlegenden Informationen zum Cluster. Beispiele hierfür sind der Name, die zugehörige Ressourcengruppe, der Standort, das Betriebssystem und die URL für das Cluster-Dashboard.|
    |Cluster-Dashboards|Führt Sie zu dem mit dem Cluster verbundenen Ambari-Portal.|
    |SSH + Clusteranmeldung|Enthält die Informationen zum Zugreifen auf den Cluster per SSH.|
    |Löschen|Löscht den HDInsight-Cluster.|

## <a name="customize-clusters"></a>Anpassen von Clustern

* [Anpassen von HDInsight-Clustern mithilfe von Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md)
* [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md)

## <a name="delete-the-cluster"></a>Löschen des Clusters

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Problembehandlung

Falls beim Erstellen von HDInsight-Clustern Probleme auftreten, sehen Sie sich die [Voraussetzungen für die Zugriffssteuerung](hdinsight-hadoop-create-linux-clusters-portal.md) an.

## <a name="next-steps"></a>Nächste Schritte

Sie haben die Erstellung eines HDInsight-Clusters erfolgreich abgeschlossen. Als Nächstes wird beschrieben, wie Sie mit Ihrem Cluster arbeiten.

### <a name="apache-hadoop-clusters"></a>Apache Hadoop-Cluster

* [Verwenden von Apache Hive mit HDInsight](hadoop/hdinsight-use-hive.md)
* [Verwenden von MapReduce mit HDInsight](hadoop/hdinsight-use-mapreduce.md)

### <a name="apache-hbase-clusters"></a>Apache HBase Cluster

* [Erste Schritte mit Apache HBase in HDInsight](hbase/apache-hbase-tutorial-get-started-linux.md)
* [Entwickeln von Java-Anwendungen für Apache HBase in HDInsight](hbase/apache-hbase-build-java-maven-linux.md)

### <a name="apache-storm-clusters"></a>Apache Storm-Cluster

* [Entwickeln von Java-Topologien für Apache Storm in HDInsight](storm/apache-storm-develop-java-topology.md)
* [Verwenden von Python-Komponenten in Apache Storm in HDInsight](storm/apache-storm-develop-python-topology.md)
* [Bereitstellen und Überwachen von Topologien mit Apache Storm in HDInsight](storm/apache-storm-deploy-monitor-topology-linux.md)

### <a name="apache-spark-clusters"></a>Apache Spark-Cluster

* [Erstellen einer eigenständigen Anwendung mit Scala](spark/apache-spark-create-standalone-application.md)
* [Ausführen von Remoteaufträgen in einem Apache Spark-Cluster mithilfe von Apache Livy](spark/apache-spark-livy-rest-interface.md)
* [Apache Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](spark/apache-spark-use-bi-tools.md)
* [Apache Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](spark/apache-spark-machine-learning-mllib-ipython.md)
