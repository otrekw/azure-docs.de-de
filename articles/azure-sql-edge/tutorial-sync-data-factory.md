---
title: Synchronisieren von Daten aus Azure SQL Edge mithilfe von Azure Data Factory
description: Hier erfahren Sie, wie Sie Daten zwischen Azure SQL Edge und Azure Blob Storage synchronisieren.
keywords: SQL Edge, Daten aus SQL Edge synchronisieren, SQL Edge Data Factory
services: sql-edge
ms.service: sql-edge
ms.topic: tutorial
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: b83201ae864d1f1eb9124af5268360bb1748f6c8
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97507607"
---
# <a name="tutorial-sync-data-from-sql-edge-to-azure-blob-storage-by-using-azure-data-factory"></a>Tutorial: Synchronisieren von Daten aus SQL Edge mit Azure Blob Storage unter Verwendung von Azure Data Factory

In diesem Tutorial verwenden Sie Azure Data Factory, um Daten aus einer Tabelle in einer Instanz von Azure SQL Edge inkrementell mit Azure Blob Storage zu synchronisieren.

## <a name="before-you-begin"></a>Voraussetzungen

Falls Sie in Ihrer Azure SQL Edge-Bereitstellung noch keine Datenbank oder Tabelle erstellt haben, verwenden Sie eine der folgenden Erstellungsmethoden:

* Verwenden Sie [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms/) oder [Azure Data Studio](/sql/azure-data-studio/download/) zum Herstellen einer Verbindung mit SQL Edge. Führen Sie ein SQL-Skript zum Erstellen der Datenbank und der Tabelle aus.
* Erstellen Sie eine Datenbank und Tabelle mithilfe von [SQLCMD](/sql/tools/sqlcmd-utility/), indem Sie eine direkte Verbindung mit dem SQL Edge-Modul herstellen. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit der Datenbank-Engine mithilfe von „sqlcmd“](/sql/ssms/scripting/sqlcmd-connect-to-the-database-engine/).
* Verwenden Sie „SQLPackage.exe“, um eine DAC-Paketdatei im SQL Edge-Container bereitzustellen. Sie können diesen Prozess automatisieren, indem Sie den URI der SqlPackage-Datei als Teil der gewünschten Konfiguration des Moduls angeben. Sie können auch direkt das Clienttool „SqlPackage.exe“ verwenden, um ein DAC-Paket in SQL Edge bereitzustellen.

    Weitere Informationen zum Herunterladen von „SqlPackage.exe“ finden Sie unter [Herunterladen und Installieren von „sqlpackage“](/sql/tools/sqlpackage-download/). Im folgenden finden Sie einige Beispielbefehle für „SqlPackage.exe“. Weitere Informationen finden Sie in der Dokumentation zu „SqlPackage.exe“.

    **Erstellen eines DAC-Pakets**

    ```cmd
    sqlpackage /Action:Extract /SourceConnectionString:"Data Source=<Server_Name>,<port>;Initial Catalog=<DB_name>;User ID=<user>;Password=<password>" /TargetFile:<dacpac_file_name>
    ```

    **Anwenden eines DAC-Pakets**

    ```cmd
    sqlpackage /Action:Publish /Sourcefile:<dacpac_file_name> /TargetServerName:<Server_Name>,<port> /TargetDatabaseName:<DB_Name> /TargetUser:<user> /TargetPassword:<password>
    ```

## <a name="create-a-sql-table-and-procedure-to-store-and-update-the-watermark-levels"></a>Erstellen einer SQL-Tabelle und einer Prozedur zum Speichern und Aktualisieren der Grenzwertstufen

Eine Grenzwerttabelle dient zum Speichern des letzten Zeitstempels, bis zu dem die Daten bereits mit Azure Storage synchronisiert wurden. Eine gespeicherte T-SQL-Prozedur (Transact-SQL) dient dazu, die Grenzwerttabelle nach jeder Synchronisierung zu aktualisieren.

Führen Sie für die SQL Edge-Instanz die folgenden Befehle aus:

```sql
    Create table [dbo].[watermarktable]
    (
    TableName varchar(255),
    WatermarkValue datetime,
    )
    GO

    CREATE PROCEDURE usp_write_watermark @timestamp datetime, @TableName varchar(50)  
    AS  
    BEGIN
    
    UPDATE [dbo].[watermarktable]
    SET [WatermarkValue] = @timestamp
    WHERE [TableName] = @TableName

    END
    Go
```

## <a name="create-a-data-factory-pipeline"></a>Erstellen einer Data Factory-Pipeline

In diesem Abschnitt erstellen Sie eine Azure Data Factory-Pipeline, um Daten aus einer Tabelle in Azure SQL Edge mit Azure Blob Storage zu synchronisieren.

### <a name="create-a-data-factory-by-using-the-data-factory-ui"></a>Erstellen einer Data Factory über die Data Factory-Benutzeroberfläche

Erstellen Sie eine Data Factory gemäß der Anleitung in [diesem Tutorial](../data-factory/quickstart-create-data-factory-portal.md#create-a-data-factory).

### <a name="create-a-data-factory-pipeline"></a>Erstellen einer Data Factory-Pipeline

1. Wählen Sie auf der Seite **Erste Schritte** der Data Factory-Benutzeroberfläche die Kachel **Pipeline erstellen** aus.

    ![Erstellen einer Data Factory-Pipeline](media/tutorial-sync-data-factory/data-factory-get-started.png)

2. Geben Sie im Fenster **Eigenschaften** der Pipeline auf der Seite **Allgemein** den Namen **PeriodicSync** ein.

3. Fügen Sie die Lookup-Aktivität hinzu, um den alten Grenzwert abzurufen. Erweitern Sie im Bereich **Aktivitäten** die Option **Allgemein**, und ziehen Sie die **Lookup**-Aktivität auf die Oberfläche des Pipeline-Designers. Ändern Sie den Namen der Aktivität in **OldWatermark**.

    ![Hinzufügen der Suche nach dem altem Grenzwert](media/tutorial-sync-data-factory/create-old-watermark-lookup.png)

4. Wechseln Sie zur Registerkarte **Einstellungen**, und wählen Sie unter **Source Dataset** (Quelldataset) die Option **Neu** aus. Sie erstellen nun ein Dataset für Daten in der Grenzwerttabelle. Diese Tabelle enthält den alten Grenzwert, der für den vorherigen Kopiervorgang verwendet wurde.

5. Wählen Sie im Fenster **Neues Dataset** die Option **Azure SQL Server** und anschließend **Weiter** aus.  

6. Geben Sie im Fenster **Eigenschaften festlegen** für das Dataset unter **Name** den Namen **WatermarkDataset** ein.

7. Wählen Sie unter **Verknüpfter Dienst** die Option **Neu** aus, und führen Sie dann die folgenden Schritte aus:

    1. Geben Sie unter **Name** die Zeichenfolge **SQLDBEdgeLinkedService** ein.

    2. Geben Sie unter **Servername** die Details Ihres SQL Edge-Servers ein.

    3. Wählen Sie in der Liste Ihren **Datenbanknamen** aus.

    4. Geben Sie unter **Benutzername** Ihren Benutzernamen und unter **Kennwort** Ihr Kennwort ein.

    5. Wählen Sie **Verbindung testen** aus, um die Verbindung mit der SQL Edge-Instanz zu testen.

    6. Klicken Sie auf **Erstellen**.

    ![Erstellen eines verknüpften Diensts](media/tutorial-sync-data-factory/create-linked-service.png)

    7. Klicken Sie auf **OK**.

8. Wählen Sie auf der Registerkarte **Einstellungen** die Option **Bearbeiten** aus.

9. Wählen Sie auf der Registerkarte **Verbindung** unter **Tabelle** die Option **[dbo].[watermarktable]** aus. Wählen Sie **Datenvorschau** aus, falls Sie eine Vorschau für die Daten in der Tabelle anzeigen möchten.

10. Wechseln Sie zum Pipeline-Editor, indem Sie oben die Registerkarte „Pipeline“ oder in der Strukturansicht auf der linken Seite den Namen der Pipeline auswählen. Vergewissern Sie sich im Eigenschaftenfenster für die Lookup-Aktivität, dass in der Liste **Source Dataset** (Quelldataset) die Option **WatermarkDataset** ausgewählt ist.

11. Erweitern Sie im Bereich **Aktivitäten** die Option **Allgemein**, und ziehen Sie eine weitere **Lookup**-Aktivität auf die Oberfläche des Pipeline-Designers. Legen Sie im Eigenschaftenfenster auf der Registerkarte **Allgemein** als Namen **NewWatermark** fest. Mit dieser Lookup-Aktivität wird der neue Grenzwert aus der Tabelle mit den Quelldaten abgerufen, damit er in das Ziel kopiert werden kann.

12. Wechseln Sie im Eigenschaftenfenster für die zweite Lookup-Aktivität zur Registerkarte **Einstellungen**, und wählen Sie **Neu** aus, um ein Dataset zu erstellen, das auf die Quelltabelle mit dem neuen Grenzwert verweist.

13. Wählen Sie im Fenster **Neues Dataset** die **SQL Edge-Instanz** und anschließend **Weiter** aus.

    1. Geben Sie im Fenster **Eigenschaften festlegen** unter **Name** den Namen **SourceDataset** ein. Wählen Sie unter **Verknüpfter Dienst** die Option **SQLDBEdgeLinkedService** aus.

    2. Wählen Sie unter **Tabelle** die zu synchronisierende Tabelle aus. Sie können auch eine Abfrage für dieses Dataset angeben, wie weiter unten in diesem Tutorial beschrieben. Die Abfrage hat Vorrang vor der Tabelle, die Sie in diesem Schritt angeben.

    3. Klicken Sie auf **OK**.

14. Wechseln Sie zum Pipeline-Editor, indem Sie oben die Registerkarte „Pipeline“ oder in der Strukturansicht auf der linken Seite den Namen der Pipeline auswählen. Vergewissern Sie sich im Eigenschaftenfenster für die Lookup-Aktivität, dass im Feld **Source Dataset** (Quelldataset) die Option **SourceDataset** ausgewählt ist.

15. Wählen Sie unter **Abfrage verwenden** die Option **Abfrage** aus. Aktualisieren Sie den Tabellennamen in der folgenden Abfrage, und geben Sie die Abfrage ein. Sie wählen nur den maximalen Wert von `timestamp` aus der Tabelle aus. Achten Sie darauf, **First row only** (Nur erste Zeile) auszuwählen.

    ```sql
    select MAX(timestamp) as NewWatermarkvalue from [TableName]
    ```

    ![Auswählen der Abfrage](media/tutorial-sync-data-factory/select-query-data-factory.png)

16. Erweitern Sie im Bereich **Aktivitäten** den Eintrag **Move & Transform** (Verschieben und transformieren), und ziehen Sie die **Copy-Aktivität** aus dem Bereich **Aktivitäten** auf die Designer-Oberfläche. Legen Sie als Namen der Aktivität **IncrementalCopy** fest.

17. Verbinden Sie beide Lookup-Aktivitäten mit der Copy-Aktivität, indem Sie die grüne Schaltfläche, die den Lookup-Aktivitäten zugeordnet ist, zur Copy-Aktivität ziehen. Lassen Sie die Maustaste los, wenn die Rahmenfarbe der Kopieraktivität blau wird.

18. Wählen Sie die Copy-Aktivität aus, und vergewissern Sie sich, dass die Eigenschaften für die Aktivität im Fenster **Eigenschaften** angezeigt werden.

19. Wechseln Sie im **Eigenschaftenfenster** zur Registerkarte **Quelle**, und führen Sie die folgenden Schritte aus:

    1. Wählen Sie im Feld **Source Dataset** (Quelldataset) die Option **SourceDataset** aus.

    2. Wählen Sie unter **Abfrage verwenden** die Option **Abfrage** aus.

    3. Geben Sie im Feld **Abfrage** die SQL-Abfrage ein. Dies ist eine Beispielabfrage:

    ```sql
    select * from TemperatureSensor where timestamp > '@{activity('OldWaterMark').output.firstRow.WatermarkValue}' and timestamp <= '@{activity('NewWaterMark').output.firstRow.NewWatermarkvalue}'
    ```

20. Wählen Sie auf der Registerkarte **Senke** unter **Sink Dataset** (Senkendataset) die Option **Neu** aus.

21. In diesem Tutorial ist der Senkendatenspeicher ein Azure Blob Storage-Datenspeicher. Wählen Sie **Azure Blob Storage** und anschließend im Fenster **Neues Dataset** die Option **Weiter** aus.

22. Wählen Sie im Fenster **Format auswählen** das Format Ihrer Daten und anschließend **Weiter** aus.

23. Geben Sie im Fenster **Eigenschaften festlegen** unter **Name** den Namen **SinkDataset** ein. Wählen Sie unter **Verknüpfter Dienst** die Option **Neu** aus. Sie erstellen nun eine Verbindung (einen verknüpften Dienst) mit Ihrer Azure Blob Storage-Instanz.

24. Führen Sie im Fenster **New Linked Service (Azure Blob Storage)** (Neuer verknüpfter Dienst (Azure Blob Storage)) die folgenden Schritte aus:

    1. Geben Sie unter **Name**  den Namen **AzureStorageLinkedService** ein.

    2. Wählen Sie unter **Speicherkontoname** das Azure Storage-Konto für Ihr Azure-Abonnement aus.

    3. Testen Sie die Verbindung, und wählen Sie **Fertig stellen** aus.

25. Vergewissern Sie sich, dass im Fenster **Eigenschaften festlegen** unter **Verknüpfter Dienst** die Option **AzureStorageLinkedService** ausgewählt ist. Wählen Sie **Erstellen** und **OK** aus.

26. Wählen Sie auf der Registerkarte **Senke** die Option **Bearbeiten** aus.

27. Navigieren Sie zur Registerkarte **Verbindung** von SinkDataset, und führen Sie die folgenden Schritte aus:

    1. Geben Sie unter **Dateipfad** den Pfad *asdedatasync/incrementalcopy* ein. *asdedatasync* ist hierbei der Blobcontainername und *incrementalcopy* der Ordnername. Erstellen Sie den Container, wenn er noch nicht vorhanden ist, oder verwenden Sie den Namen eines bereits vorhandenen. Azure Data Factory erstellt den Ausgabeordner *incrementalcopy* automatisch, falls er nicht vorhanden ist. Sie können auch die Schaltfläche **Durchsuchen** für den **Dateipfad** verwenden, um zu einem Ordner in einem Blobcontainer zu navigieren.

    2. Wählen Sie für den Teil **Datei** des Felds **Dateipfad** die Option **Dynamischen Inhalt hinzufügen [ALT+P]** aus, und geben Sie dann **@CONCAT('Incremental-', pipeline().RunId, '.txt')** in das geöffnete Fenster ein. Wählen Sie **Fertig stellen** aus. Der Dateiname wird mit dem Ausdruck dynamisch generiert. Jede Pipelineausführung verfügt über eine eindeutige ID. Die Copy-Aktivität nutzt die Ausführungs-ID zum Generieren des Dateinamens.

28. Wechseln Sie zum Pipeline-Editor, indem Sie oben die Registerkarte „Pipeline“ oder in der Strukturansicht auf der linken Seite den Namen der Pipeline auswählen.

29. Erweitern Sie im Bereich **Aktivitäten** die Option **Allgemein**, und ziehen Sie die **Stored Procedure**-Aktivität aus dem Bereich **Aktivitäten** auf die Oberfläche des Pipeline-Designers. Verbinden Sie die grüne Ausgabe (Erfolgreich) der Copy-Aktivität mit der Stored Procedure-Aktivität.

30. Wählen Sie im Pipeline-Designer die Option **Stored Procedure-Aktivität** aus, und ändern Sie den Namen in **SPtoUpdateWatermarkActivity**.

31. Wechseln Sie zur Registerkarte **SQL-Konto**, und wählen Sie unter **Verknüpfter Dienst** die Option **_SQLDBEdgeLinkedService_* aus.

32. Wechseln Sie zur Registerkarte **Gespeicherte Prozedur**, und führen Sie die folgenden Schritte aus:

    1. Wählen Sie unter **Name der gespeicherten Prozedur** die Option **[dbo].[usp_write_watermark]** aus.

    2. Wählen Sie zum Angeben von Werten für die Parameter der gespeicherten Prozedur die Option **Import parameter** (Importparameter) aus, und geben Sie für die Parameter die folgenden Werte ein:

    |Name|type|Wert|
    |-----|----|-----|
    |LastModifiedtime|Datetime|@{activity('NewWaterMark').output.firstRow.NewWatermarkvalue}|
    |TableName|String|@{activity('OldWaterMark').output.firstRow.TableName}|

33. Wählen Sie zum Überprüfen der Pipelineeinstellungen auf der Symbolleiste die Option **Überprüfen** aus. Vergewissern Sie sich, dass keine Validierungsfehler vorliegen. Wählen Sie **>>** aus, um das Fenster **Pipeline Validation Report** (Pipelineüberprüfungsbericht) zu schließen.

34. Veröffentlichen Sie die Entitäten (verknüpfte Dienste, Datasets und Pipelines) im Azure Data Factory-Dienst, indem Sie die Schaltfläche **Alle veröffentlichen** auswählen. Warten Sie, bis eine Erfolgsmeldung für den Veröffentlichungsvorgang angezeigt wird.

## <a name="trigger-a-pipeline-based-on-a-schedule"></a>Auslösen einer Pipeline nach einem Zeitplan

1. Wählen Sie auf der Symbolleiste für die Pipeline nacheinander **Trigger hinzufügen**, **Neu/Bearbeiten** und dann **Neu** aus.

2. Benennen Sie den Trigger mit **HourlySync**. Wählen Sie unter **Typ** die Option **Zeitplan** aus. Legen Sie die **Wiederholung** auf „1 Stunde“ fest.

3. Klicken Sie auf **OK**.

4. Wählen Sie **Alle veröffentlichen**.

5. Wählen Sie **Trigger Now** (Jetzt auslösen) aus.

6. Wechseln Sie im linken Bereich zur Registerkarte **Überwachen**. Sie können den Status der Pipelineausführung anzeigen, die vom manuellen Trigger ausgelöst wird. Klicken Sie zum Aktualisieren der Liste auf **Aktualisieren**.

## <a name="next-steps"></a>Nächste Schritte

Die Azure Data Factory-Pipeline in diesem Tutorial kopiert einmal pro Stunde Daten aus einer Tabelle in der SQL Edge-Instanz an einen Speicherort in Azure Blob Storage. Informationen zu weiteren Data Factory-Verwendungsszenarien finden Sie in [diesen Tutorials](../data-factory/tutorial-copy-data-portal.md).
