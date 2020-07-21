---
title: Verwenden von Azure Data Lake Storage Gen2 mit Azure HDInsight-Clustern
description: Erfahren Sie, wie Sie Azure Data Lake Storage Gen2 mit Azure HDInsight-Clustern verwenden können.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive,seoapr2020
ms.date: 04/24/2020
ms.openlocfilehash: 2a534bd0cb89e837ff2315cb3fb9cfe70ad01f5f
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86078985"
---
# <a name="use-azure-data-lake-storage-gen2-with-azure-hdinsight-clusters"></a>Verwenden von Azure Data Lake Storage Gen2 mit Azure HDInsight-Clustern

Azure Data Lake Storage Gen2 ist ein Cloudspeicherdienst für Big Data-Analysen, der auf Azure Blob Storage basiert. Data Lake Storage Gen2 vereint die Funktionen von Azure Blob Storage und Azure Data Lake Storage Gen1. Der daraus resultierende Dienst bietet Features aus Azure Data Lake Storage Gen1. Zu diesen Features gehören: Dateisystemsemantik, Sicherheit auf Verzeichnisebene und auf Dateiebene sowie Flexibilität. Sie werden ergänzt durch die geringen Kosten, den mehrstufigen Speicher, die Hochverfügbarkeit und die Notfallwiederherstellung von Azure Blob Storage.

## <a name="data-lake-storage-gen2-availability"></a>Verfügbarkeit von Data Lake Storage Gen2

Data Lake Storage Gen2 ist als Speicheroption für fast alle Typen von Azure HDInsight-Clustern sowohl als Standard- als auch zusätzliches Speicherkonto verfügbar. HBase kann jedoch nur ein Data Lake Storage Gen2-Konto verwenden.

Einen vollständigen Vergleich der Optionen für die Clustererstellung mit Azure Data Lake Storage Gen2 finden Sie unter [Vergleich der Speicheroptionen für die Verwendung mit Azure HDInsight-Clustern](hdinsight-hadoop-compare-storage-options.md).

> [!Note]  
> Nachdem Sie Data Lake Storage Gen2 als **primären Speichertyp** ausgewählt haben, können Sie kein Data Lake Storage Gen1-Konto als zusätzlichen Speicher auswählen.

## <a name="create-a-cluster-with-data-lake-storage-gen2-through-the-azure-portal"></a>Erstellen eines Clusters mit Data Lake Storage Gen2 über das Azure-Portal

Konfigurieren Sie mithilfe der folgenden Schritte ein Data Lake Storage Gen2-Konto, um einen HDInsight-Cluster zu erstellen, der Data Lake Storage Gen2 als Speicher verwendet.

### <a name="create-a-user-assigned-managed-identity"></a>Erstellen einer benutzerseitig zugewiesenen verwalteten Identität

Erstellen Sie eine benutzerseitig zugewiesene verwaltete Identität, falls noch keine vorhanden ist.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie links oben **Ressource erstellen** aus.
1. Geben Sie im Suchfeld den Suchbegriff **Vom Benutzer zugewiesen** ein, und klicken Sie auf **Benutzerseitig zugewiesene verwaltete Identität**.
1. Klicken Sie auf **Erstellen**.
1. Geben Sie einen Namen für Ihre verwaltete Identität ein, und wählen Sie das richtige Abonnement, die Ressourcengruppe und den Speicherort aus.
1. Klicken Sie auf **Erstellen**.

Weitere Informationen zur Funktionsweise verwalteter Identitäten in Azure HDInsight finden Sie unter [Verwaltete Identitäten in Azure HDInsight](hdinsight-managed-identities.md).

![Erstellen einer benutzerseitig zugewiesenen verwalteten Identität](./media/hdinsight-hadoop-use-data-lake-storage-gen2/create-user-assigned-managed-identity-portal.png)

### <a name="create-a-data-lake-storage-gen2-account"></a>Erstellen eines Data Lake Storage Gen2-Kontos

Erstellen Sie ein Azure Data Lake Storage Gen2-Speicherkonto.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie links oben **Ressource erstellen** aus.
1. Geben Sie im Suchfeld den Suchbegriff **Speicher** ein, und klicken Sie auf **Speicherkonto**.
1. Klicken Sie auf **Erstellen**.
1. Auf dem Bildschirm **Speicherkonto erstellen**:
    1. Wählen Sie das richtige Abonnement und die Ressourcengruppe aus.
    1. Geben Sie einen Namen für Ihr Data Lake Storage Gen2-Konto ein.
    1. Klicken Sie auf die Registerkarte **Erweitert**.
    1. Klicken Sie auf **Aktiviert** neben **Hierarchischer Namespace** unter **Data Lake Storage Gen2**.
    1. Klicken Sie auf **Überprüfen + erstellen**.
    1. Klicken Sie auf **Erstellen**

Weitere Informationen zu anderen Optionen während der Erstellung des Speicherkontos finden Sie unter [Schnellstart: Erstellen eines Azure Data Lake Storage Gen2-Speicherkontos](../storage/blobs/data-lake-storage-quickstart-create-account.md).

![Screenshot zur Speicherkontoerstellung im Azure-Portal](./media/hdinsight-hadoop-use-data-lake-storage-gen2/azure-data-lake-storage-account-create-advanced.png)

### <a name="set-up-permissions-for-the-managed-identity-on-the-data-lake-storage-gen2-account"></a>Festlegen von Berechtigungen für verwaltete Identitäten im Data Lake Storage Gen2-Konto

Weisen Sie die verwaltete Identität der Rolle **Besitzer von Speicherblobdaten** im Speicherkonto zu.

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Speicherkonto.
1. Wählen Sie Ihr Speicherkonto und dann **Zugriffssteuerung (IAM)** aus, um die Zugriffssteuerungseinstellungen für das Konto anzuzeigen. Wählen Sie die Registerkarte **Rollenzuweisungen** aus, um die Liste mit den Rollenzuweisungen anzuzeigen.

    ![Screenshot mit Anzeige der Zugriffssteuerungseinstellungen für Speicher](./media/hdinsight-hadoop-use-data-lake-storage-gen2/portal-access-control.png)

1. Klicken Sie auf die Schaltfläche **+ Rollenzuweisung hinzufügen**, um eine neue Rolle hinzuzufügen.
1. Wählen Sie im Fenster **Rollenzuweisung hinzufügen** die Rolle **Besitzer von Speicherblobdaten** aus. Wählen Sie dann das Abonnement aus, das über die verwaltete Identität und das Speicherkonto verfügt. Suchen Sie als Nächstes die vom Benutzer zugewiesene verwaltete Identität, die Sie zuvor erstellt haben. Wählen Sie abschließend die verwaltete Identität aus, sodass sie unter **Ausgewählte Mitglieder** aufgelistet wird.

    ![Screenshot: Zuweisen einer RBAC-Rolle](./media/hdinsight-hadoop-use-data-lake-storage-gen2/add-rbac-role3-window.png)

1. Wählen Sie **Speichern** aus. Die vom Benutzer zugewiesene Identität, die Sie ausgewählt haben, wird jetzt unter der ausgewählten Rolle aufgelistet.
1. Nachdem das anfängliche Setup abgeschlossen ist, können Sie einen Cluster über das Portal erstellen. Der Cluster muss sich in der gleichen Azure-Region befinden wie das Speicherkonto. Wählen Sie auf der Registerkarte **Speicher** des Menüs zur Clustererstellung die folgenden Optionen aus:

    * Wählen Sie für **Primärer Speichertyp** **Azure Data Lake Storage Gen2** aus.
    * Suchen Sie unter **Primäres Speicherkonto** das neu erstellte Data Lake Storage Gen2-Speicherkonto, und wählen Sie es aus.

    * Wählen Sie unter **Identität** die neu erstellte, vom Benutzer zugewiesene verwaltete Identität aus.

        ![Speichereinstellungen für das Verwenden von Data Lake Storage Gen2 mit Azure HDInsight](./media/hdinsight-hadoop-use-data-lake-storage-gen2/azure-portal-cluster-storage-gentwo.png)

    > [!NOTE]
    > * Zum Hinzufügen eines sekundären Data Lake Storage Gen2-Kontos auf der Speicherkontoebene müssen Sie lediglich die zuvor erstellte verwaltete Identität zum neuen Data Lake Storage Gen2-Speicherkonto hinzufügen, das Sie hinzufügen möchten. Beachten Sie, dass das Hinzufügen eines sekundären Data Lake Storage Gen2-Kontos über das Blatt „Zusätzliche Speicherkonten“ in HDInsight nicht unterstützt wird.
    > * Sie können RA-GRS oder RA-ZRS (georedundanter Speicher mit Lesezugriff oder geo- und zonenredundanter Speicher) für das von HDInsight verwendete Azure-Speicherkonto aktivieren. Jedoch wird das Erstellen eines Clusters für den sekundären RA-GRS- oder RA-ZRS-Endpunkt nicht unterstützt.


## <a name="create-a-cluster-with-data-lake-storage-gen2-through-the-azure-cli"></a>Erstellen eines Clusters mit Data Lake Storage Gen2 über die Azure CLI

Sie können [hier eine Beispielvorlagendatei](https://github.com/Azure-Samples/hdinsight-data-lake-storage-gen2-templates/blob/master/hdinsight-adls-gen2-template.json) und [hier eine Beispieldatei für Parameter](https://github.com/Azure-Samples/hdinsight-data-lake-storage-gen2-templates/blob/master/parameters.json) herunterladen. Ersetzen Sie vor der Verwendung der Vorlage und des Azure CLI-Codeausschnitts unten die folgenden Platzhalter durch die korrekten Werte:

| Platzhalter | BESCHREIBUNG |
|---|---|
| `<SUBSCRIPTION_ID>` | Die ID Ihres Azure-Abonnements |
| `<RESOURCEGROUPNAME>` | Die Ressourcengruppe, in der der neue Cluster und das Speicherkonto erstellt werden sollen |
| `<MANAGEDIDENTITYNAME>` | Der Name der verwalteten Identität, der die Berechtigungen für Ihr Azure Data Lake Storage Gen2-Konto gewährt werden |
| `<STORAGEACCOUNTNAME>` | Das zu erstellende Azure Data Lake Storage Gen2-Konto |
| `<CLUSTERNAME>` | Der Name des HDInsight-Clusters. |
| `<PASSWORD>` | Das gewählte Kennwort für die Anmeldung im Cluster über SSH und das Ambari-Dashboard |

Der Codeausschnitt führt die folgenden ersten Schritte durch:

1. Anmeldung bei Ihrem Azure-Konto
1. Festlegen des aktiven Abonnements, in dem die Erstellung durchgeführt wird
1. Erstellen einer neuen Ressourcengruppe für die neuen Bereitstellungsaktivitäten
1. Erstellen einer benutzerseitig zugewiesenen verwalteten Identität
1. Hinzufügen einer Erweiterung zur Azure CLI, um Features für Data Lake Storage Gen2 zu nutzen
1. Erstellen Sie ein neues Data Lake Storage Gen2-Konto mit dem Flag `--hierarchical-namespace true`.

```azurecli
az login
az account set --subscription <SUBSCRIPTION_ID>

# Create resource group
az group create --name <RESOURCEGROUPNAME> --location eastus

# Create managed identity
az identity create -g <RESOURCEGROUPNAME> -n <MANAGEDIDENTITYNAME>

az extension add --name storage-preview

az storage account create --name <STORAGEACCOUNTNAME> \
    --resource-group <RESOURCEGROUPNAME> \
    --location eastus --sku Standard_LRS \
    --kind StorageV2 --hierarchical-namespace true
```

Melden Sie sich als Nächstes beim Portal an. Fügen Sie die neue benutzerseitig zugewiesene verwaltete Identität zu der Rolle **Mitwirkender an Storage-Blobdaten** im Speicherkonto zu. Dieser Schritt wird in Schritt 3 unter [Verwenden des Azure-Portals](hdinsight-hadoop-use-data-lake-storage-gen2.md) beschrieben.

Stellen Sie die Vorlage mit dem folgenden Codeausschnitt bereit, nachdem Sie die Rolle für die benutzerseitig zugewiesene verwaltete Identität zugewiesen haben.

```azurecli
az group deployment create --name HDInsightADLSGen2Deployment \
    --resource-group <RESOURCEGROUPNAME> \
    --template-file hdinsight-adls-gen2-template.json \
    --parameters parameters.json
```

## <a name="create-a-cluster-with-data-lake-storage-gen2-through-azure-powershell"></a>Erstellen eines Clusters mit Data Lake Storage Gen2 über Azure PowerShell

PowerShell wird zum Erstellen eines HDInsight-Clusters mit Azure Data Lake Storage Gen2 derzeit nicht unterstützt.

## <a name="access-control-for-data-lake-storage-gen2-in-hdinsight"></a>Zugriffssteuerung für Data Lake Storage Gen2 in HDInsight

### <a name="what-kinds-of-permissions-does-data-lake-storage-gen2-support"></a>Welche Arten von Berechtigungen unterstützt Data Lake Storage Gen2?

Data Lake Storage Gen2 verwendet ein Zugriffssteuerungsmodell, das sowohl die rollenbasierte Zugriffssteuerung (Role Based Access Control, RBAC) als auch POSIX-Zugriffssteuerungslisten (Access Control Lists, ACLs) unterstützt. Data Lake Storage Gen1 unterstützt Zugriffssteuerungslisten nur zum Steuern des Zugriffs auf Daten.

Für die RBAC in Azure werden Rollenzuweisungen verwendet, um Benutzern, Gruppen und Dienstprinzipalen Berechtigungen für Azure-Ressourcen effektiv zuzuweisen. In der Regel sind diese Azure-Ressourcen auf Ressourcen auf oberster Ebene (z.B. Azure Storage-Konten) beschränkt. Für Azure Storage und damit auch Data Lake Storage Gen2 wurde dieser Mechanismus auf die Dateisystemressource ausgeweitet.

 Weitere Informationen zu Dateiberechtigungen mit rollenbasierter Zugriffssteuerung finden Sie im Abschnitt „Rollenbasierte Zugriffssteuerung in Azure (RBAC)“ unter [Zugriffssteuerung in Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-access-control.md#azure-role-based-access-control-rbac).

Weitere Informationen zu Dateiberechtigungen mit ACLs finden Sie im Abschnitt „Zugriffssteuerungslisten für Dateien und Verzeichnisse“ unter [Zugriffssteuerung in Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-access-control.md#access-control-lists-on-files-and-directories).

### <a name="how-do-i-control-access-to-my-data-in-data-lake-storage-gen2"></a>Wie steuere ich in Data Lake Storage Gen2 den Zugriff auf meine Daten?

In Data Lake Storage Gen2 steuern verwaltete Identitäten den Zugriff auf Dateien durch Ihren HDInsight-Cluster. Eine verwaltete Identität ist eine in Azure Active Directory (Azure AD) registrierte Identität, deren Anmeldeinformationen von Azure verwaltet werden. Bei Verwendung von verwalteten Identitäten müssen Sie in Azure AD keine Dienstprinzipale registrieren. Sie müssen auch keine Anmeldeinformationen, z. B. Zertifikate, verwalten.

Es gibt zwei Arten verwalteter Identitäten für Azure-Dienste: durch das System zugewiesene und durch den Benutzer zugewiesene Identitäten. HDInsight verwendet durch den Benutzer zugewiesene verwaltete Identitäten, um auf Data Lake Storage Gen2 zuzugreifen. Benutzerseitig zugewiesene verwaltete Identitäten werden als eigenständige Azure-Ressourcen erstellt. Azure erstellt eine Identität in dem Azure AD-Mandanten, der vom verwendeten Abonnement als vertrauenswürdig eingestuft wird. Nachdem die Identität erstellt wurde, kann sie einer oder mehreren Azure-Dienstinstanzen zugewiesen werden.

Der Lebenszyklus einer vom Benutzer zugewiesenen Identität wird getrennt vom Lebenszyklus der Azure-Dienstinstanzen verwaltet, denen sie zugewiesen ist. Weitere Informationen zu verwalteten Identitäten finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../active-directory/managed-identities-azure-resources/overview.md).

### <a name="how-do-i-set-permissions-for-azure-ad-users-to-query-data-in-data-lake-storage-gen2-by-using-hive-or-other-services"></a>Wie richte ich Berechtigungen für Azure AD-Benutzer zum Abfragen von Daten in Data Lake Storage Gen2 mit Hive oder anderen Diensten ein?

Verwenden Sie Azure AD-Sicherheitsgruppen als zugewiesenen Prinzipal in ACLs, um Benutzern Berechtigungen zum Abfragen von Daten zu erteilen. Weisen Sie einzelnen Benutzern oder Dienstprinzipalen nicht direkt Dateizugriffsberechtigungen zu. Wenn Sie Azure AD-Sicherheitsgruppen zum Steuern des Flusses von Berechtigungen verwenden, können Sie Benutzer oder Dienstprinzipale hinzufügen und entfernen, ohne erneut ACLs auf eine gesamte Verzeichnisstruktur anzuwenden. Sie müssen die Benutzer einfach der entsprechenden Azure AD-Sicherheitsgruppe hinzufügen oder sie aus ihr entfernen. ACLs werden nicht vererbt, und die erneute Anwendung von ACLs erfordert daher eine Aktualisierung der ACL für jede Datei und jedes Unterverzeichnis.

## <a name="access-files-from-the-cluster"></a>Zugreifen auf Dateien aus dem Cluster

Es gibt mehrere Möglichkeiten, wie Sie auf die Dateien in Data Lake Storage Gen2 über einen HDInsight-Cluster zugreifen können.

* **Verwenden des vollqualifizierten Namens** Bei diesem Ansatz geben Sie den vollständigen Pfad zu der Datei an, auf die Sie zugreifen möchten.

    ```
    abfs://<containername>@<accountname>.dfs.core.windows.net/<file.path>/
    ```

* **Verwenden des verkürzten Pfadformats** Bei diesem Ansatz ersetzen Sie den Pfad bis zum Clusterstamm durch:

    ```
    abfs:///<file.path>/
    ```

* **Verwenden des relativen Pfads** Bei diesem Ansatz geben Sie nur den relativen Pfad zu der Datei an, auf die Sie zugreifen möchten.

    ```
    /<file.path>/
    ```

### <a name="data-access-examples"></a>Beispiele für Datenzugriff

Die Beispiele basieren auf einer [ssh-Verbindung](./hdinsight-hadoop-linux-use-ssh-unix.md) mit dem Hauptknoten des Clusters. In den Beispielen werden alle drei URI-Schemas verwendet. Ersetzen Sie `CONTAINERNAME` und `STORAGEACCOUNT` durch die entsprechenden Werte.

#### <a name="a-few-hdfs-commands"></a>Ein paar HDFS-Befehle

1. Erstellen von Dateien im lokalen Speicher.

    ```bash
    touch testFile.txt
    ```

1. Erstellen von Verzeichnissen im Clusterspeicher.

    ```bash
    hdfs dfs -mkdir abfs://CONTAINERNAME@STORAGEACCOUNT.dfs.core.windows.net/sampledata1/
    hdfs dfs -mkdir abfs:///sampledata2/
    hdfs dfs -mkdir /sampledata3/
    ```

1. Kopieren von Daten aus dem lokalen Speicher in den Clusterspeicher.

    ```bash
    hdfs dfs -copyFromLocal testFile.txt  abfs://CONTAINERNAME@STORAGEACCOUNT.dfs.core.windows.net/sampledata1/
    hdfs dfs -copyFromLocal testFile.txt  abfs:///sampledata2/
    hdfs dfs -copyFromLocal testFile.txt  /sampledata3/
    ```

1. Auflisten von Verzeichnisinhalten im Clusterspeicher.

    ```bash
    hdfs dfs -ls abfs://CONTAINERNAME@STORAGEACCOUNT.dfs.core.windows.net/sampledata1/
    hdfs dfs -ls abfs:///sampledata2/
    hdfs dfs -ls /sampledata3/
    ```

#### <a name="creating-a-hive-table"></a>Erstellen einer Hive-Tabelle

Zu Zwecken der Veranschaulichung werden drei Speicherorte gezeigt. Verwenden Sie für die tatsächliche Ausführung nur einen der `LOCATION`-Einträge.

```hql
DROP TABLE myTable;
CREATE EXTERNAL TABLE myTable (
    t1 string,
    t2 string,
    t3 string,
    t4 string,
    t5 string,
    t6 string,
    t7 string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
STORED AS TEXTFILE
LOCATION 'abfs://CONTAINERNAME@STORAGEACCOUNT.dfs.core.windows.net/example/data/';
LOCATION 'abfs:///example/data/';
LOCATION '/example/data/';
```

## <a name="next-steps"></a>Nächste Schritte

* [Azure HDInsight integration with Data Lake Storage Gen2 preview – ACL and security update (Integration von Azure HDInsight in Data Lake Storage Gen2 (Vorschau) – ACL und Sicherheitsupdate)](https://azure.microsoft.com/blog/azure-hdinsight-integration-with-data-lake-storage-gen-2-preview-acl-and-security-update/)
* [Einführung in Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md)
* [Tutorial: Extrahieren, Transformieren und Laden von Daten mithilfe von Interactive Query in Azure HDInsight](./interactive-query/interactive-query-tutorial-analyze-flight-data.md)
