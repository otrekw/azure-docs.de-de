---
title: Massenkopieren von Daten mithilfe des Azure-Portals
description: In diesem Artikel wird erklärt, wie Sie Azure Data Factory und Kopieraktivität zum Kopieren von Daten per Massenvorgang aus einem Quelldatenspeicher in einen Zieldatenspeicher verwenden.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: tutorial
ms.custom: seo-lt-2019; seo-dt-2019
ms.date: 05/28/2020
ms.openlocfilehash: f8b72037046d05b39587c2fd57794b4109a85ae3
ms.sourcegitcommit: 8017209cc9d8a825cc404df852c8dc02f74d584b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2020
ms.locfileid: "84249177"
---
# <a name="copy-multiple-tables-in-bulk-by-using-azure-data-factory"></a>Massenkopieren von mehreren Tabellen mithilfe von Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

In diesem Tutorial wird das **Kopieren von mehreren Tabellen aus einer Azure SQL-Datenbank in Azure Synapse Analytics (ehemals SQL DW)** veranschaulicht. Sie können dieses Muster auch in anderen Kopierszenarios anwenden. So können Sie z. B. Tabellen aus SQL Server/Oracle in Azure SQL-Datenbank/Azure Synapse Analytics (ehemals SQL DW)/Azure Blob kopieren oder verschiedene Pfade aus Blob in Azure SQL-Datenbanktabellen.

> [!NOTE]
> - Falls Sie noch nicht mit Azure Data Factory vertraut sind, ist es ratsam, den Artikel [Einführung in Azure Data Factory](introduction.md) zu lesen.

Das Tutorial umfasst die folgenden Schritte:

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen einer Azure SQL-Datenbank-Instanz, einer Azure Synapse Analytics-Instanz (ehemals SQL DW) und von verknüpften Azure Storage-Diensten.
> * Erstellen von Azure SQL-Datenbank- und Azure Synapse Analytics-Datasets (ehemals SQL DW).
> * Erstellen einer Pipeline zum Abrufen der zu kopierenden Tabellen und einer weiteren Pipeline zur Durchführung des eigentlichen Kopiervorgangs. 
> * Starten einer Pipelineausführung
> * Überwachen der Pipeline- und Aktivitätsausführungen.

In diesem Tutorial wird das Azure-Portal verwendet. Informationen zur Verwendung von anderen Tools/SDKs zum Erstellen einer Data Factory finden Sie unter [Schnellstarts](quickstart-create-data-factory-dot-net.md). 

## <a name="end-to-end-workflow"></a>Kompletter Workflow
In diesem Szenario verfügen Sie über mehrere Tabellen aus der Azure SQL-Datenbank-Instanz, die Sie in Azure Synapse Analytics (ehemals SQL DW) kopieren möchten. Nachfolgend ist der logische Ablauf eines Workflows dargestellt, der in Pipelines ausgeführt wird:

![Workflow](media/tutorial-bulk-copy-portal/tutorial-copy-multiple-tables.png)

* Die erste Pipeline ruft die Liste mit den Tabellen ab, die in die Senkendatenspeicher kopiert werden sollen.  Sie können stattdessen auch eine Metadatentabelle mit den Tabellen verwalten, die in die Senkendatenspeicher kopiert werden sollen. Die Pipeline löst anschließend eine weitere Pipeline aus, die wiederum jede Tabelle in der Datenbank durchläuft und den Datenkopiervorgang ausführt.
* Die zweite Pipeline führt den eigentlichen Kopiervorgang aus. Dazu wird die Liste mit den Tabellen als Parameter verwendet. Kopieren Sie für jede Tabelle in der Liste die jeweilige Tabelle aus der Azure SQL-Datenbank-Instanz in die entsprechende Tabelle in Azure Synapse Analytics (ehemals SQL DW). Verwenden Sie für eine optimale Leistung das [gestaffelte Kopieren über Blob Storage und PolyBase](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse). In diesem Beispiel wird die Liste mit den Tabellen von der ersten Pipeline als Wert für den Parameter übergeben. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen
* **Azure Storage-Konto**. Das Azure Storage-Konto wird im Massenkopiervorgang als Staging-Blobspeicher verwendet. 
* **Azure SQL-Datenbank**. Diese Datenbank enthält die Quelldaten. 
* **Azure Synapse Analytics (ehemals SQL DW)** . Dieses Data Warehouse enthält die Daten, die aus der SQL-Datenbank kopiert werden. 

### <a name="prepare-sql-database-and-azure-synapse-analytics-formerly-sql-dw"></a>Vorbereiten von SQL-Datenbank und Azure Synapse Analytics (ehemals SQL DW)

**Vorbereiten der Azure SQL-Quelldatenbank**:

Erstellen Sie eine Azure SQL-Datenbank mit Adventure Works LT-Beispieldaten, indem Sie den Anweisungen im Artikel [Erstellen einer Azure SQL-Datenbank](../azure-sql/database/single-database-create-quickstart.md) folgen. In diesem Tutorial werden alle Tabellen aus der Beispieldatenbank in Azure Synapse Analytics (ehemals SQL DW) kopiert.

**Vorbereiten der Azure Synapse Analytics-Senke (ehemals SQL DW)** :

1. Wenn Sie noch keine Azure Synapse Analytics-Instanz (ehemals SQL DW) erstellt haben, finden Sie die Anleitung dazu im Artikel [Erstellen eines SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-tutorial.md).

1. Erstellen Sie entsprechende Tabellenschemas in Azure Synapse Analytics (ehemals SQL DW). In einem späteren Schritt können Sie Daten mit Azure Data Factory migrieren/kopieren.

## <a name="azure-services-to-access-sql-server"></a>Azure-Dienste für den Zugriff auf SQL-Server

Gewähren Sie den Azure-Diensten sowohl für SQL-Datenbank als auch für Azure Synapse Analytics (ehemals SQL DW) den Zugriff auf SQL Server. Stellen Sie sicher, dass die Einstellung **Anderen Azure-Diensten und -Ressourcen den Zugriff auf diesen Server gestatten** für Ihren Server auf **EIN** festgelegt ist. Mit dieser Einstellung wird dem Data Factory-Dienst erlaubt, Daten aus Ihrer Azure SQL-Datenbank-Instanz zu lesen und in Azure Synapse Analytics (ehemals SQL DW) zu schreiben. 

Wenn Sie diese Einstellung überprüfen und aktivieren möchten, navigieren Sie zu Ihrem Server >„Sicherheit“ > „Firewalls und virtuelle Netzwerke“, und legen Sie die Option **Anderen Azure-Diensten und -Ressourcen den Zugriff auf diesen Server gestatten** auf **EIN** fest.

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory

1. Starten Sie den Webbrowser **Microsoft Edge** oder **Google Chrome**. Die Data Factory-Benutzeroberfläche wird zurzeit nur in den Webbrowsern Microsoft Edge und Google Chrome unterstützt.
1. Öffnen Sie das [Azure-Portal](https://portal.azure.com). 
1. Klicken Sie links im Azure-Portal-Menü auf **Ressource erstellen** > **Analytics** > **Data Factory**. 
   ![Auswählen von „Data Factory“ im Bereich „Neu“](./media/doc-common-process/new-azure-data-factory-menu.png)
1. Geben Sie auf der Seite **Neue Data Factory** unter **Name** die Zeichenfolge **ADFTutorialBulkCopyDF** ein. 
 
   Der Name der Azure Data Factory muss **global eindeutig**sein. Sollte der unten angegebene Fehler für das Feld „Name“ auftreten, ändern Sie den Namen der Data Factory (beispielsweise in „<IhrName>ADFTutorialBulkCopyDF“). Benennungsregeln für Data Factory-Artefakte finden Sie im Artikel [Azure Data Factory – Benennungsregeln](naming-rules.md).
  
       `Data factory name "ADFTutorialBulkCopyDF" is not available`
1. Wählen Sie Ihr **Azure-Abonnement** aus, in dem die Data Factory erstellt werden soll. 
1. Führen Sie für die **Ressourcengruppe** einen der folgenden Schritte aus:
     
   - Wählen Sie die Option **Use existing**(Vorhandene verwenden) und dann in der Dropdownliste eine vorhandene Ressourcengruppe. 
   - Wählen Sie **Neu erstellen**, und geben Sie den Namen einer Ressourcengruppe ein.   
         
     Weitere Informationen über Ressourcengruppen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/management/overview.md).  
1. Wählen Sie **V2** als **Version** aus.
1. Wählen Sie den **Standort** für die Data Factory aus. Eine Liste der Azure-Regionen, in denen Data Factory derzeit verfügbar ist, finden Sie, indem Sie die für Sie interessanten Regionen auf der folgenden Seite auswählen und dann **Analysen** erweitern, um **Data Factory** zu finden: [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/). Die von der Data Factory verwendeten Datenspeicher (Azure Storage, Azure SQL-Datenbank usw.) und Computedienste (HDInsight usw.) können sich in anderen Regionen befinden.
1. Klicken Sie auf **Erstellen**.
1. Wählen Sie nach der Erstellung **Zu Ressource wechseln** aus, um zur Seite **Data Factory** zu navigieren. 
   
1. Klicken Sie auf die Kachel **Erstellen und überwachen**, um die Anwendung für die Data Factory-Benutzeroberfläche auf einer separaten Registerkarte zu starten.
1. Wechseln Sie auf der Seite **Legen wir los** am linken Rand zur Registerkarte **Erstellen** wie in der folgenden Abbildung gezeigt:

     ![Seite für die ersten Schritte](./media/doc-common-process/get-started-page-author-button.png)

## <a name="create-linked-services"></a>Erstellen von verknüpften Diensten
Sie erstellen verknüpfte Dienste, um Ihre Datenspeicher und Computes mit einer Data Factory zu verknüpfen. Ein verknüpfter Dienst enthält die Verbindungsinformationen, die der Data Factory-Dienst zur Laufzeit zum Herstellen der Verbindung mit dem Datenspeicher verwendet. 

In diesem Tutorial verknüpfen Sie Ihre Azure SQL-Datenbank-, Azure Synapse Analytics- (ehemals SQL DW) und Azure Blob Storage-Datenspeicher mit Ihrer Data Factory. Die Azure SQL-Datenbank ist der Quelldatenspeicher. Azure Synapse Analytics (ehemals SQL DW) ist die Senke bzw. der Zieldatenspeicher. Azure Blob Storage dient zum Bereitstellen der Daten per Staging, bevor sie mit PolyBase in die Azure Synapse Analytics-Instanz (ehemals SQL DW) geladen werden. 

### <a name="create-the-source-azure-sql-database-linked-service"></a>Erstellen des verknüpften Quelldiensts Azure SQL-Datenbank
In diesem Schritt erstellen Sie einen verknüpften Dienst, um Ihre Azure SQL-Datenbank mit der Data Factory zu verbinden. 

1. Klicken Sie unten im Fenster auf **Verbindungen** und anschließend auf der Symbolleiste auf die Schaltfläche **+ Neu**. (Die Schaltfläche **Verbindungen** befindet sich unten in der linken Spalte unter **Factory Resources** (Factory-Ressourcen)). 

1. Wählen Sie im Fenster **New Linked Service** (Neuer verknüpfter Dienst) die Option **Azure SQL-Datenbank**, und klicken Sie auf **Weiter**. 
1. Führen Sie im Fenster **New Linked Service (Azure SQL Database)** (Neuer verknüpfter Dienst (Azure SQL-Datenbank)) die folgenden Schritte aus: 

    a. Geben Sie unter **Name** den Namen **AzureSqlDatabaseLinkedService** ein.
    
    b. Wählen Sie unter **Servername** Ihren Server aus.
    
    c. Wählen Sie unter **Datenbankname** Ihre Azure SQL-Datenbank aus. 
    
    d. Geben Sie den **Namen des Benutzers** ein, um eine Verbindung mit der Azure SQL-Datenbank herzustellen. 
    
    e. Geben Sie das **Kennwort** für den Benutzer ein. 

    f. Klicken Sie auf **Verbindung testen**, um die Verbindung mit der Azure SQL-Datenbank mit den angegebenen Informationen zu testen.
  
    g. Klicken Sie auf **Erstellen**, um den verknüpften Dienst zu speichern.


### <a name="create-the-sink-azure-synapse-analytics-formerly-sql-dw-linked-service"></a>Erstellen des verknüpften Diensts für die Azure Synapse Analytics-Senke (ehemals SQL DW)

1. Klicken Sie auf der Registerkarte **Verbindungen** in der Symbolleiste erneut auf **+ Neu**. 
1. Wählen Sie im Fenster **New Linked Service** (Neuer verknüpfter Dienst) die Option **Azure Synapse Analytics (ehemals SQL DW)** , und klicken Sie auf **Weiter**. 
1. Führen Sie im Fenster **New Linked Service Azure Synapse Analytics (formerly SQL DW)** (Neuer verknüpfter Dienst (Azure Synapse Analytics-Senke (ehemals SQL DW))) die folgenden Schritte aus: 
   
    a. Geben Sie unter **Name** den Namen **AzureSqlDWLinkedService** ein.
     
    b. Wählen Sie unter **Servername** Ihren Server aus.
     
    c. Wählen Sie unter **Datenbankname** Ihre Azure SQL-Datenbank aus. 
     
    d. Geben Sie den **Benutzernamen** ein, um eine Verbindung mit Azure SQL-Datenbank herzustellen. 
     
    e. Geben Sie das **Kennwort** für den Benutzer ein. 
     
    f. Klicken Sie auf **Verbindung testen**, um die Verbindung mit der Azure SQL-Datenbank mit den angegebenen Informationen zu testen.
     
    g. Klicken Sie auf **Erstellen**.

### <a name="create-the-staging-azure-storage-linked-service"></a>Erstellen des verknüpften Stagingdiensts Azure Storage
In diesem Tutorial wird Azure Blob Storage als vorläufiger Stagingbereich zur Aktivierung von PolyBase verwendet, um eine bessere Leistung zu erzielen.

1. Klicken Sie auf der Registerkarte **Verbindungen** in der Symbolleiste erneut auf **+ Neu**. 
1. Wählen Sie im Fenster **New Linked Service** (Neuer verknüpfter Dienst) die Option **Azure Blob Storage**, und klicken Sie dann auf **Weiter**. 
1. Führen Sie im Fenster **New Linked Service (Azure Blob Storage)** (Neuer verknüpfter Dienst (Azure Blob Storage)) die folgenden Schritte aus: 

    a. Geben Sie unter **Name** die Zeichenfolge **AzureStorageLinkedService** ein.                                                 
    b. Wählen Sie unter **Speicherkontoname** Ihr **Azure Storage-Konto** aus.
    
    c. Klicken Sie auf **Erstellen**.


## <a name="create-datasets"></a>Erstellen von Datasets
In diesem Tutorial werden Quell- und Senkendatasets erstellt, die den Speicherort der Daten angeben. 

Das Eingabedataset **AzureSqlDatabaseDataset** verweist auf **AzureSqlDatabaseLinkedService**. Der verknüpfte Dienst gibt die Verbindungszeichenfolge für die Herstellung der Verbindung mit der Datenbank an. Das Dataset gibt den Namen der Datenbank und die Tabelle an, in der die Quelldaten enthalten sind. 

Das Ausgabedataset **AzureSqlDWDataset** verweist auf **AzureSqlDWLinkedService**. Der verknüpfte Dienst gibt die Verbindungszeichenfolge an, die mit Azure Synapse Analytics (ehemals SQL DW) verbunden werden soll. Das Dataset gibt die Datenbank und die Tabelle an, in die die Daten kopiert werden. 

In diesem Tutorial sind die SQL-Quell- und -Zieltabellen in den Datasetdefinitionen nicht hartcodiert. Stattdessen übergibt die ForEach-Aktivität den Namen der Tabelle zur Laufzeit an die Copy-Aktivität. 

### <a name="create-a-dataset-for-source-sql-database"></a>Erstellen eines Datasets für die SQL-Quelldatenbank

1. Klicken Sie im Bereich auf der linken Seite auf **+** (Pluszeichen) und dann auf **Dataset**. 

    ![Menü „Neues Dataset“](./media/tutorial-bulk-copy-portal/new-dataset-menu.png)
1. Wählen Sie im Fenster **Neues Dataset** die Option **Azure SQL-Datenbank** aus, und klicken Sie dann auf **Weiter**. 
    
1. Geben Sie im Fenster **Eigenschaften festlegen** unter **Name** den Namen **AzureSqlDatabaseDataset** ein. Wählen Sie unter **Verknüpfter Dienst** die Option **AzureSqlDatabaseLinkedService** aus. Klicken Sie dann auf **OK**.

1. Wechseln Sie zur Registerkarte **Verbindung**, und wählen Sie unter **Tabelle** eine beliebige Tabelle aus. Diese Tabelle ist eine Dummytabelle. Sie geben beim Erstellen einer Pipeline im Quelldataset eine Abfrage ein. Die Abfrage wird verwendet, um Daten aus der Azure SQL-Datenbank zu extrahieren. Alternativ hierzu können Sie auf das Kontrollkästchen **Bearbeiten** klicken und als Tabellenname **dbo.dummyName** eingeben. 
 

### <a name="create-a-dataset-for-sink-azure-synapse-analytics-formerly-sql-dw"></a>Erstellen des Datasets für die Azure Synapse Analytics-Senke (ehemals SQL DW)

1. Klicken Sie im Bereich auf der linken Seite auf **+** (Pluszeichen) und dann auf **Dataset**. 
1. Wählen Sie im Fenster **Neues Dataset** die Option **Azure Synapse Analytics (ehemals SQL DW)** aus, und klicken Sie auf **Weiter**.
1. Geben Sie im Fenster **Eigenschaften festlegen** unter **Name** den Namen **AzureSqlDWDataset** ein. Wählen Sie unter **Verknüpfter Dienst** die Option **AzureSqlDWLinkedService** aus. Klicken Sie dann auf **OK**.
1. Wechseln Sie zur Registerkarte **Parameter**, klicken Sie auf **+ Neu**, und geben Sie **DWTableName** als Parameternamen ein. Stellen Sie beim Kopieren (und Einfügen) dieses Namens von der Seite sicher, dass am Ende von **DWTableName** keine **nachgestellte Leerstelle** angefügt ist.
1. Wechseln Sie zur Registerkarte **Verbindung**. 

    a. Aktivieren Sie für **Tabelle** die Option **Bearbeiten**. Geben Sie im ersten Eingabefeld für den Tabellennamen **dbo** ein. Wählen Sie dann in das zweite Eingabefeld aus, und klicken Sie unten auf den Link **Dynamischen Inhalt hinzufügen**. 

    ![Tabellenname für die Datasetverbindung](./media/tutorial-bulk-copy-portal/dataset-connection-tablename.png)

    b. Klicken Sie auf der Seite **Dynamischen Inhalt hinzufügen** unter **Parameter** auf **DWTAbleName**. Das oberste Textfeld `@dataset().DWTableName` für Ausdrücke wird automatisch gefüllt. Klicken Sie anschließend auf **Fertig stellen**. Die **tableName**-Eigenschaft des Datasets ist auf den Wert festgelegt, der als Argument für den Parameter **DWTableName** übergeben wird. Die ForEach-Aktivität durchläuft eine Liste mit Tabellen und übergibt diese einzeln an die Copy-Aktivität. 

    ![Dataset-Parameter-Generator](./media/tutorial-bulk-copy-portal/dataset-parameter-builder.png)
 
## <a name="create-pipelines"></a>Erstellen von Pipelines
In diesem Tutorial werden zwei Pipelines erstellt: **IterateAndCopySQLTables** und **GetTableListAndTriggerCopyData**. 

Die Pipeline **GetTableListAndTriggerCopyData** führt zwei Aktionen aus:

* Abrufen der Systemtabelle für die Azure SQL-Datenbank, um die Liste mit den Tabellen abzurufen, die kopiert werden sollen.
* Auslösen der Pipeline **IterateAndCopySQLTables**, um den eigentlichen Kopiervorgang der Daten durchzuführen.

Die Pipeline **IterateAndCopySQLTables** akzeptiert eine Liste von Tabellen als Parameter. Für jede Tabelle in der Liste werden Daten aus der Tabelle in Azure SQL-Datenbank nach Azure Synapse Analytics (ehemals SQL DW) kopiert. Dazu wird das gestaffelte Kopieren und PolyBase verwendet.

### <a name="create-the-pipeline-iterateandcopysqltables"></a>Erstellen der Pipeline „IterateAndCopySQLTables“

1. Klicken Sie im linken Bereich auf **+** (Pluszeichen) und dann auf **Pipeline**.

    ![Menü für neue Pipeline](./media/tutorial-bulk-copy-portal/new-pipeline-menu.png)
 
1. Geben Sie im Bereich „Allgemein“ unter **Eigenschaften** die Eigenschaft **IterateAndCopySQLTables** für **Name** an. Reduzieren Sie dann den Bereich, indem Sie in der oberen rechten Ecke auf das Symbol „Eigenschaften“ klicken.

1. Wechseln Sie zur Registerkarte **Parameter**, und führen Sie die folgenden Aktionen durch: 

    a. Klicken Sie auf **+ NEU**. 
    
    b. Geben Sie **tableList** für den Parameter **Name** ein.
    
    c. Wählen Sie unter **Typ** die Option **Array**.

1. Erweitern Sie in der Toolbox **Aktivitäten** die Option **Iteration & Conditions** (Iteration und Bedingungen), und ziehen Sie die **ForEach**-Aktivität in die Oberfläche zum Entwerfen von Pipelines. Sie können in der Toolbox **Aktivitäten** auch nach Aktivitäten suchen. 

    a. Geben Sie unten auf der Registerkarte **Allgemein** unter **Name** den Namen **IterateSQLTables** ein. 

    b. Wechseln Sie auf die Registerkarte **Einstellungen**, klicken Sie auf das Eingabefeld für **Elemente**, und klicken Sie anschließend unten auf den Link **Dynamischen Inhalt hinzufügen**. 

    c. Reduzieren Sie auf der Seite **Dynamischen Inhalt hinzufügen** die Abschnitte **Systemvariablen** und **Funktionen**, und klicken Sie unter **Parameter** auf **tableList**. Das oberste Textfeld für Ausdrücke wird automatisch mit `@pipeline().parameter.tableList` gefüllt. Klicken Sie auf **Fertig stellen**. 

    ![Foreach-Parameter-Generator](./media/tutorial-bulk-copy-portal/for-each-parameter-builder.png)
    
    d. Wechseln Sie zur Registerkarte **Aktivitäten**, und klicken Sie auf das **Stiftsymbol**, um der Aktivität **ForEach** eine untergeordnete Aktivität hinzuzufügen.
    ![Generator für ForEach-Aktivität](./media/tutorial-bulk-copy-portal/for-each-activity-builder.png)

1. Erweitern Sie in der Toolbox **Aktivitäten** die Option **Move & Transfer** (Verschieben und übertragen), und ziehen Sie die **Copy Data**-Aktivität auf die Oberfläche des Pipeline-Designers. Beachten Sie das Breadcrumb-Menü im oberen Bereich. **IterateAndCopySQLTable** ist der Pipelinename, und **IterateSQLTables** ist der Name der ForEach-Aktivität. Der Designer ist Teil des Aktivitätsbereichs. Sie können im Breadcrumb-Menü auf den Link klicken, um vom ForEach-Editor zurück zum Pipeline-Editor zu wechseln. 

    ![Kopieren in ForEach](./media/tutorial-bulk-copy-portal/copy-in-for-each.png)

1. Wechseln Sie zur Registerkarte **Quelle**, und führen Sie die folgenden Schritte aus:

    1. Wählen Sie unter **Source Dataset** (Quelldataset) die Option **AzureSqlDatabaseDataset**. 
    1. Wählen Sie unter **Abfrage verwenden** die Option **Abfrage** aus. 
    1. Klicken Sie auf das Eingabefeld **Abfrage**, wählen Sie unten die Option **Dynamischen Inhalt hinzufügen**, geben Sie den folgenden Ausdruck für **Abfrage** ein, und wählen Sie **Fertig stellen**.

        ```sql
        SELECT * FROM [@{item().TABLE_SCHEMA}].[@{item().TABLE_NAME}]
        ``` 


1. Wechseln Sie zur Registerkarte **Senke**, und führen Sie die folgenden Schritte aus: 

    1. Wählen Sie unter **Sink Dataset** (Senkendataset) die Option **AzureSqlDWDataset**.
    1. Klicken Sie auf das Eingabefeld für den Wert (VALUE) des Parameters „DWTableName“, wählen Sie unten die Option **Dynamischen Inhalt hinzufügen** aus, geben Sie den Ausdruck `[@{item().TABLE_SCHEMA}].[@{item().TABLE_NAME}]` als Skript ein, und wählen Sie **Fertig stellen** aus.
    1. Wählen Sie als Kopiermethode **PolyBase** aus. 
    1. Deaktivieren Sie die Option **Use Type default** (Typstandard verwenden). 
    1. Klicken Sie auf das Eingabefeld **Pre-copy Script** (Skript für Vorabkopieren), wählen Sie unten die Option **Dynamischen Inhalt hinzufügen**, geben Sie den folgenden Ausdruck als Skript ein, und wählen Sie **Fertig stellen**. 

        ```sql
        TRUNCATE TABLE [@{item().TABLE_SCHEMA}].[@{item().TABLE_NAME}]
        ```

        ![Einstellungen für die Kopiersenke](./media/tutorial-bulk-copy-portal/copy-sink-settings.png)
1. Wechseln Sie zur Registerkarte **Einstellungen**, und führen Sie die folgenden Schritte aus: 

    1. Aktivieren Sie das Kontrollkästchen für **Enable Staging** (Staging aktivieren).
    1. Wählen Sie unter **Store Account Linked Service** (Verknüpfter Dienst des Speicherkontos) die Option **AzureStorageLinkedService**.

1. Klicken Sie zum Überprüfen der Pipelineeinstellungen auf der Symbolleiste für die Pipeline auf **Überprüfen**. Vergewissern Sie sich, dass keine Validierungsfehler vorliegen. Klicken Sie zum Schließen des **Pipeline Validation Report** (Pipelineüberprüfungsbericht) auf **>>** .

### <a name="create-the-pipeline-gettablelistandtriggercopydata"></a>Erstellen der Pipeline „GetTableListAndTriggerCopyData“

Diese Pipeline führt zwei Aktionen aus:

* Abrufen der Systemtabelle für die Azure SQL-Datenbank, um die Liste mit den Tabellen abzurufen, die kopiert werden sollen.
* Auslösen der Pipeline „IterateAndCopySQLTables“, um den eigentlichen Kopiervorgang der Daten auszuführen.

1. Klicken Sie im linken Bereich auf **+** (Pluszeichen) und dann auf **Pipeline**.
1. Ändern Sie auf der Registerkarte **Allgemein** den Namen der Pipeline in **GetTableListAndTriggerCopyData**. 

1. Erweitern Sie in der Toolbox **Aktivitäten** die Option **Allgemein**, und ziehen Sie die **Lookup**-Aktivität auf die Oberfläche des Pipeline-Designers. Führen Sie anschließend die folgenden Schritte aus:

    1. Geben Sie unter **Name** den Namen **LookupTableList** ein. 
    1. Geben Sie unter **Beschreibung** den Text **Retrieve the table list from Azure SQL database** (Tabellenliste aus Azure SQL-Datenbank abrufen) ein.

1. Wechseln Sie zur Registerkarte **Einstellungen**, und führen Sie die folgenden Schritte aus:

    1. Wählen Sie unter **Source Dataset** (Quelldataset) die Option **AzureSqlDatabaseDataset**. 
    1. Wählen Sie unter **Abfrage verwenden** die Option **Abfrage** aus. 
    1. Geben Sie unter **Abfrage** die folgende SQL-Abfrage ein:

        ```sql
        SELECT TABLE_SCHEMA, TABLE_NAME FROM information_schema.TABLES WHERE TABLE_TYPE = 'BASE TABLE' and TABLE_SCHEMA = 'SalesLT' and TABLE_NAME <> 'ProductModel'
        ```
    1. Deaktivieren Sie das Kontrollkästchen für das Feld **First row only** (Nur erste Zeile).

        ![Lookup-Aktivität – Seite „Einstellungen“](./media/tutorial-bulk-copy-portal/lookup-settings-page.png)
1. Ziehen Sie die **Execute Pipeline**-Aktivität aus der Toolbox „Aktivitäten“ auf die Oberfläche des Pipeline-Designers, und legen Sie den Namen auf **TriggerCopy** fest.

1. **Verbinden** Sie die **Lookup**-Aktivität mit der **Execute Pipeline**-Aktivität, indem Sie das **grüne Feld**, das der Lookup-Aktivität zugeordnet ist, in den Bereich links von der Execute Pipeline-Aktivität ziehen.

    ![Verbinden von Lookup- und Execute Pipeline-Aktivitäten](./media/tutorial-bulk-copy-portal/connect-lookup-execute-pipeline.png)

1. Wechseln Sie zur Registerkarte **Einstellungen** der Aktivität **Execute Pipeline** (Pipeline ausführen), und führen Sie die folgenden Schritte aus: 

    1. Wählen Sie unter **Invoked pipeline** (Aufgerufene Pipeline) die Option **IterateAndCopySQLTables**. 
    1. Erweitern Sie den Abschnitt **Erweitert**, und deaktivieren Sie das Kontrollkästchen für **Wait on completion** (Auf Abschluss warten).
    1. Klicken Sie im Abschnitt **Parameter** auf **+ Neu**. 
    1. Geben Sie **tableList** für den Parameter **Name** ein.
    1. Klicken Sie auf das Eingabefeld für den Wert, wählen Sie unten die Option **Dynamischen Inhalt hinzufügen**, geben Sie `@activity('LookupTableList').output.value` als Wert für den Tabellennamen ein, und wählen Sie **Fertig stellen**. Sie legen die Ergebnisliste der Lookup-Aktivität als Eingabe für die zweite Pipeline fest. Die Ergebnisliste enthält die Liste mit den Tabellen, deren Daten auf das Ziel kopiert werden müssen. 

        ![Execute Pipeline-Aktivität – Seite „Einstellungen“](./media/tutorial-bulk-copy-portal/execute-pipeline-settings-page.png)

1. Klicken Sie zum Überprüfen der Pipeline in der Symbolleiste auf **Überprüfen**. Vergewissern Sie sich, dass keine Validierungsfehler vorliegen. Klicken Sie zum Schließen des **Pipeline Validation Report** (Pipelineüberprüfungsbericht) auf **>>** .

1. Klicken Sie zum Veröffentlichen von Entitäten (Datasets, Pipelines usw.) im Data Factory-Dienst oben im Fenster auf **Alle veröffentlichen**. Warten Sie, bis die Veröffentlichung erfolgreich durchgeführt wurde. 

## <a name="trigger-a-pipeline-run"></a>Auslösen einer Pipelineausführung

1. Navigieren Sie zur Pipeline **GetTableListAndTriggerCopyData**, und klicken Sie in der oberen Symbolleiste der Pipeline auf **Add Trigger** (Trigger hinzufügen) und dann auf **Trigger Now** (Jetzt auslösen). 

1. Bestätigen Sie die Ausführung auf der Seite **Pipeline run** (Pipelineausführung), und wählen Sie dann **Finish** (Beenden) aus.

## <a name="monitor-the-pipeline-run"></a>Überwachen der Pipelineausführung

1. Wechseln Sie zur Registerkarte **Überwachen**. Klicken Sie auf **Aktualisieren**, bis Ausführungen für beide Pipelines Ihrer Lösung angezeigt werden. Fahren Sie mit dem Aktualisieren der Liste fort, bis der Status **Erfolgreich** angezeigt wird. 

1. Klicken Sie unter dem Link „Pipelinename“ für diese Pipeline, um die Aktivitätsausführungen anzuzeigen, die der Pipeline **GetTableListAndTriggerCopyData** zugeordnet sind. Es sollten zwei Aktivitätsausführungen für diese Pipelineausführung angezeigt werden. 
    ![Überwachen der Pipelineausführung](./media/tutorial-bulk-copy-portal/monitor-pipeline.png)
1. Wenn Sie die Ausgabe der Aktivität **Lookup** (Suche) anzeigen möchten, klicken Sie auf den Link **Output** (Ausgabe) neben der Aktivität in der Spalte **ACTIVITY NAME** (AKTIVITÄTSNAME). Sie können das Fenster **Ausgabe** maximieren und wiederherstellen. Klicken Sie auf **X**, nachdem Sie die Prüfung durchgeführt haben, um das Fenster **Ausgabe** zu schließen.

    ```json
    {
        "count": 9,
        "value": [
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "Customer"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "ProductDescription"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "Product"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "ProductModelProductDescription"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "ProductCategory"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "Address"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "CustomerAddress"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "SalesOrderDetail"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "SalesOrderHeader"
            }
        ],
        "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (East US)",
        "effectiveIntegrationRuntimes": [
            {
                "name": "DefaultIntegrationRuntime",
                "type": "Managed",
                "location": "East US",
                "billedDuration": 0,
                "nodes": null
            }
        ]
    }
    ```    
1. Durch Wählen des Links **Alle Pipelineausführungen** im oberen Bereich des Breadcrumb-Menüs können Sie zur Ansicht **Pipeline Runs** (Pipelineausführungen) zurückkehren. Klicken Sie auf den Link **IterateAndCopySQLTables** (in der Spalte **PIPELINE NAME** (PIPELINENAME)), um die Aktivitätsausführungen der Pipeline anzuzeigen. Beachten Sie, dass für jede Tabelle in der Ausgabe der **Lookup**-Aktivität eine Ausführung der **Copy**-Aktivität enthalten ist. 

1. Vergewissern Sie sich, dass die Daten in die Azure Synapse Analytics-Zielinstanz (ehemals SQL DW) kopiert wurden, die Sie in diesem Tutorial verwendet haben. 

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie die folgenden Schritte ausgeführt: 

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen einer Azure SQL-Datenbank-Instanz, einer Azure Synapse Analytics-Instanz (ehemals SQL DW) und von verknüpften Azure Storage-Diensten.
> * Erstellen von Azure SQL-Datenbank- und Azure Synapse Analytics-Datasets (ehemals SQL DW).
> * Erstellen einer Pipeline zum Abrufen der zu kopierenden Tabellen und einer weiteren Pipeline zur Durchführung des eigentlichen Kopiervorgangs. 
> * Starten einer Pipelineausführung
> * Überwachen der Pipeline- und Aktivitätsausführungen.

Fahren Sie nun mit dem folgenden Tutorial fort, um mehr über das inkrementelle Kopieren von Daten aus einer Quelle in ein Ziel zu erfahren:
> [!div class="nextstepaction"]
>[Inkrementelles Kopieren von Daten](tutorial-incremental-copy-portal.md)
