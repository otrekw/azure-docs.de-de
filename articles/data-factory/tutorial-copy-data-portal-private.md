---
title: Erstellen einer Azure Data Factory-Pipeline mithilfe von privaten Endpunkten
description: Dieses Tutorial enthält detaillierte Anweisungen zur Erstellung einer Data Factory mit Pipeline mithilfe des Azure-Portals. Die Pipeline verwendet die Kopieraktivität, um Daten aus Azure Blob Storage in eine Azure SQL-Datenbank zu kopieren.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: tutorial
ms.custom: seo-lt-2019
ms.date: 05/15/2020
ms.author: jingwang
ms.openlocfilehash: 9458c89922fab8450b7cb8e202491d4c47e250e8
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86540525"
---
# <a name="copy-data-securely-from-azure-blob-storage-to-a-sql-database-by-using-private-endpoints"></a>Sicheres Kopieren von Daten aus Azure Blob Storage in eine SQL-Datenbank mithilfe von privaten Endpunkten

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

In diesem Tutorial erstellen Sie eine Data Factory über die Azure Data Factory-Benutzeroberfläche (User Interface, UI). **Die Pipeline in dieser Data Factory kopiert Daten sicher aus Azure Blob Storage in eine Azure SQL-Datenbank (beide lassen nur den Zugriff auf ausgewählte Netzwerke zu). Dazu werden private Endpunkte in einem [verwalteten virtuellen Azure Data Factory-Netzwerk](managed-virtual-network-private-endpoint.md) verwendet.** Das Konfigurationsmuster in diesem Tutorial gilt für Kopiervorgänge aus einem dateibasierten Datenspeicher in einen relationalen Datenspeicher. Eine Liste der Datenspeicher, die als Quellen und Senken unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](https://docs.microsoft.com/azure/data-factory/copy-activity-overview).

> [!NOTE]
>
> - Falls Sie noch nicht mit Data Factory vertraut sind, ist es ratsam, den Artikel [Einführung in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction) zu lesen.

In diesem Tutorial führen Sie die folgenden Schritte aus:

> * Erstellen einer Data Factory
> * Erstellen einer Pipeline mit einer Kopieraktivität


## <a name="prerequisites"></a>Voraussetzungen
* **Azure-Abonnement**. Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* **Azure-Speicherkonto**. Sie verwenden Blob Storage als *Quelldatenspeicher*. Wenn Sie kein Speicherkonto besitzen, finden Sie unter [Informationen zu Azure-Speicherkonten](https://docs.microsoft.com/azure/storage/common/storage-account-create?tabs=azure-portal) Schritte zum Erstellen eines solchen Kontos. **Stellen Sie sicher, dass das Storage-Konto nur den Zugriff über ausgewählte Netzwerke zulässt.** 
* **Azure SQL-Datenbank**. Sie verwenden die Datenbank als *Senkendatenspeicher*. Wenn Sie keine Azure SQL-Datenbank besitzen, finden Sie Schritte zum Erstellen einer solchen Datenbank unter [Erstellen einer SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal). **Stellen Sie sicher, dass das Azure SQL-Datenbank-Konto nur den Zugriff über ausgewählte Netzwerke zulässt.** 

### <a name="create-a-blob-and-a-sql-table"></a>Erstellen eines Blobs und einer SQL-Tabelle

Nun bereiten Sie Ihre Blob Storage-Instanz und SQL-Datenbank durch Ausführen der folgenden Schritte auf das Tutorial vor:

#### <a name="create-a-source-blob"></a>Erstellen eines Quellblobs

1. Starten Sie den Editor. Kopieren Sie den folgenden Text, und speichern Sie ihn als **emp.txt**-Datei auf einem Datenträger:

    ```
    FirstName,LastName
    John,Doe
    Jane,Doe
    ```

1. Erstellen Sie in Blob Storage einen Container mit dem Namen **adftutorial**. Erstellen Sie einen Ordner namens **input** in diesem Container. Laden Sie anschließend die Datei **emp.txt** in den Ordner **input** hoch. Verwenden Sie für diese Aufgaben das Azure-Portal oder Tools wie [Azure Storage-Explorer](https://storageexplorer.com/).

#### <a name="create-a-sink-sql-table"></a>Erstellen einer SQL-Senkentabelle

1. Verwenden Sie das folgende SQL-Skript, um die Tabelle **dbo.emp** in Ihrer SQL-Datenbank zu erstellen:

    ```sql
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50)
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);
    ```

1. Gewähren Sie Azure-Diensten den Zugriff auf SQL Server. Stellen Sie sicher, dass die Einstellung **Zugriff auf Azure-Dienste erlauben** für Ihre SQL Server-Instanz **aktiviert** ist, damit Data Factory Daten in diese SQL Server-Instanz schreiben kann. Um diese Einstellung zu überprüfen und zu aktivieren, navigieren Sie auf dem Azure SQL-Datenbank-Server zu „Übersicht“ > „Serverfirewall festlegen“, und legen Sie die Option **Zugriff auf Azure-Dienste zulassen** auf **EIN** fest.

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory
In diesem Schritt erstellen Sie eine Data Factory und starten die Data Factory-Benutzeroberfläche, um eine Pipeline in der Data Factory zu erstellen.

1. Öffnen Sie **Microsoft Edge** oder **Google Chrome**. Die Data Factory-Benutzeroberfläche wird zurzeit nur in den Webbrowsern Microsoft Edge und Google Chrome unterstützt.


2. Klicken Sie im Menü auf der linken Seite auf **Ressource erstellen** > **Analytics** > **Data Factory**.

3. Geben Sie auf der Seite **Neue Data Factory** unter **Name** den Namen **ADFTutorialDataFactory** ein.

   Der Name der Azure Data Factory muss *global eindeutig*sein. Wenn eine Fehlermeldung zum Namenswert angezeigt wird, geben Sie einen anderen Namen für die Data Factory ein. (Verwenden Sie beispielsweise „IhrNameADFTutorialDataFactory“.) Benennungsregeln für Data Factory-Artefakte finden Sie im Thema [Azure Data Factory – Benennungsregeln](https://docs.microsoft.com/azure/data-factory/naming-rules).

4. Wählen Sie das **Azure-Abonnement** aus, in dem die Data Factory erstellt werden soll.

5. Führen Sie unter **Ressourcengruppe** einen der folgenden Schritte aus:

    a. Wählen Sie die Option **Use existing**(Vorhandene verwenden) und dann in der Dropdownliste eine vorhandene Ressourcengruppe.

    b. Wählen Sie **Neu erstellen**, und geben Sie den Namen einer Ressourcengruppe ein. 
         
    Weitere Informationen zu Ressourcengruppen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](https://docs.microsoft.com/azure/azure-resource-manager/management/overview). 

6. Wählen Sie unter **Version** die Option **V2**.

7. Wählen Sie unter **Standort** einen Standort für die Data Factory aus. In der Dropdownliste werden nur unterstützte Standorte angezeigt. Die Datenspeicher (etwa Azure Storage und SQL-Datenbank) und Computeeinheiten (etwa Azure HDInsight), die von der Data Factory genutzt werden, können sich in anderen Regionen befinden.


8. Klicken Sie auf **Erstellen**.


9. Nach Abschluss der Erstellung wird der Hinweis im Benachrichtigungscenter angezeigt. Wählen Sie **Zu Ressource wechseln** aus, um zur Data Factory-Seite zu navigieren.

10. Klicken Sie auf **Erstellen und überwachen**, um die Data Factory-Benutzeroberfläche auf einer separaten Registerkarte zu starten.

## <a name="create-an-azure-integration-runtime-in-adf-managed-virtual-network"></a>Erstellen einer Azure Integration Runtime-Instanz in einem verwalteten virtuellen ADF-Netzwerk
In diesem Schritt erstellen Sie eine Azure Integration Runtime-Instanz und aktivieren ein verwaltetes virtuelles Netzwerk.

1. Navigieren Sie im ADF-Portal zu **Manage Hub** (Hub verwalten), und klicken Sie auf **Neu**, um eine neue Azure Integration Runtime-Instanz zu erstellen.
   ![Erstellen einer neuen Azure Integration Runtime-Instanz](./media/tutorial-copy-data-portal-private/create-new-azure-ir.png)
2. Wählen Sie die Erstellung einer Azure Integration Runtime-Instanz.
   ![Neue Azure Integration Runtime-Instanz](./media/tutorial-copy-data-portal-private/azure-ir.png)
3. Aktivieren Sie **Virtuelles Netzwerk**.
   ![Neue Azure Integration Runtime-Instanz](./media/tutorial-copy-data-portal-private/enable-managed-vnet.png)
4. Klicken Sie auf **Erstellen**.

## <a name="create-a-pipeline"></a>Erstellen einer Pipeline
In diesem Schritt erstellen Sie eine Pipeline mit einer Copy-Aktivität in der Data Factory. Die Copy-Aktivität kopiert Daten aus Blob Storage in SQL-Datenbank. Im [Schnellstarttutorial](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal) haben Sie anhand der folgenden Schritte eine Pipeline erstellt:

1. 1\. Erstellen des verknüpften Diensts
1. Erstellen von Eingabe- und Ausgabedatasets
1. Erstellen einer Pipeline.

In diesem Tutorial beginnen Sie mit dem Erstellen der Pipeline. Verknüpfte Dienste und Datasets erstellen Sie, wenn Sie sie zum Konfigurieren der Pipeline benötigen.

1. Wählen Sie auf der Seite **Erste Schritte** die Option **Pipeline erstellen** aus.

   ![Erstellen der Pipeline](./media/doc-common-process/get-started-page.png)
1. Geben Sie im Bereich **Eigenschaften** der Pipeline als **Name** der Pipeline **CopyPipeline** ein.

1. Erweitern Sie in der Toolbox **Aktivitäten** die Kategorie **Move and Transform** (Verschieben und transformieren), und verschieben Sie die Aktivität **Daten kopieren** aus der Toolbox auf die Oberfläche des Pipeline-Designers. Geben Sie unter **Name** den Namen **CopyFromBlobToSql** ein.

    ![Copy-Aktivität](./media/tutorial-copy-data-portal-private/drag-drop-copy-activity.png)

### <a name="configure-source"></a>Konfigurieren der Quelle

>[!TIP]
>In diesem Tutorial verwenden Sie *Kontoschlüssel* als Authentifizierungstyp für Ihren Quelldatenspeicher, Sie können bei Bedarf aber auch andere unterstützte Authentifizierungsmethoden auswählen: *SAS-URI*, *Dienstprinzipal* und *Verwaltete Identität*. Ausführlichere Informationen finden Sie in den entsprechenden Abschnitten [dieses Artikels](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#linked-service-properties).
>Zum sicheren Speichern von Geheimnissen für Datenspeicher empfiehlt sich darüber hinaus die Verwendung einer Azure Key Vault-Instanz. Eine ausführliche Erläuterung finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault).

#### <a name="create-source-dataset-and-linked-service"></a>Erstellen eines Quelldataset und eines verknüpften Diensts

1. Wechseln Sie zur Registerkarte **Quelle**. Klicken Sie auf **+ Neu**, um ein Quelldataset zu erstellen.

1. Wählen Sie im Dialogfeld **Neues Dataset** die Option **Azure Blob Storage** und dann **Weiter** aus. Da sich die Quelldaten in Blob Storage befinden, wählen Sie **Azure Blob Storage** als Quelldataset aus.

1. Wählen Sie im Dialogfeld **Format auswählen** den Formattyp Ihrer Daten und dann **Weiter** aus.

1. Geben Sie im Dialogfeld **Eigenschaften festlegen** als Name **SourceBlobDataset** ein. Aktivieren Sie das Kontrollkästchen **Erste Zeile als Kopfzeile**. Klicken Sie unter dem Textfeld **Verknüpfter Dienst** auf **+ Neu**.

1. Geben Sie im Dialogfeld **New Linked Service (Azure Blob Storage)** (Neuer verknüpfter Dienst (Azure Blob Storage)) als Name **AzureStorageLinkedService** ein, und wählen Sie in der Liste **Speicherkontoname** Ihr Speicherkonto aus. 

1. Aktivieren Sie unbedingt **Interactive Authoring** (Interaktive Dokumenterstellung). Die Aktivierung kann ungefähr 1 Minute dauern.

    ![Interaktive Erstellung](./media/tutorial-copy-data-portal-private/interactive-authoring.png)

1. Wählen Sie **Verbindung testen** aus. Dabei tritt ein Fehler auf, wenn das Storage-Konto ausschließlich Zugriff über ein ausgewähltes Netzwerk zulässt und Azure Data Factory zur Erstellung eines privaten Endpunkts erforderlich ist, der vor der Verwendung genehmigt werden muss. In der Fehlermeldung sollte ein Link zum Erstellen eines **privaten Endpunkts** angezeigt werden, dem Sie folgen können, um einen verwalteten privaten Endpunkt zu erstellen. *Alternativ dazu können Sie direkt zur Registerkarte „Verwalten“ navigieren und die Anweisungen im [nächsten Abschnitt](#create-a-managed-private-endpoint) befolgen, um einen verwalteten privaten Endpunkt zu erstellen.*
> [!NOTE]
> Die Registerkarte „Verwalten“ ist möglicherweise nicht für alle Data Factory-Instanzen verfügbar. Wird sie nicht angezeigt, können Sie wie folgt auf private Endpunkte zugreifen: Registerkarte **Ersteller** > **Verbindungen** > **Privater Endpunkt**.
1. Lassen Sie das Dialogfeld geöffnet, und navigieren Sie dann zu dem Storage-Konto, das Sie oben ausgewählt haben.

1. Befolgen Sie die Anweisungen in [diesem Abschnitt](#approval-of-a-private-link-in-storage-account), um die private Verbindung zu genehmigen.

1. Gehen Sie zurück zum Dialogfeld. Wählen Sie erneut **Verbindung testen** und anschließend **Erstellen** aus, um den verknüpften Dienst bereitzustellen.

1. Nach der Erstellung des verknüpften Diensts wird wieder die Seite **Eigenschaften festlegen** angezeigt. Klicken Sie neben **Dateipfad** auf **Durchsuchen**.

1. Navigieren Sie zum Ordner **adftutorial/input**, wählen Sie die Datei **emp.txt** aus, und klicken Sie auf **OK**.

1. Klicken Sie auf **OK**. Die Pipelineseite wird automatisch aufgerufen. Vergewissern Sie sich, dass auf der Registerkarte **Quelle** die Option **SourceBlobDataset** ausgewählt ist. Wenn Sie auf dieser Seite eine Vorschau der Daten anzeigen möchten, klicken Sie auf **Datenvorschau**.

    ![Quelldataset](./media/tutorial-copy-data-portal-private/source-dataset-selected.png)

#### <a name="create-a-managed-private-endpoint"></a>Erstellen eines verwalteten privaten Endpunkts

Sollten Sie beim Testen der oben genannten Verbindung nicht auf den Link geklickt haben, folgen Sie dem nachstehenden Pfad. Jetzt müssen Sie einen verwalteten privaten Endpunkt erstellen, den Sie mit dem zuvor erstellten verknüpften Dienst verbinden werden.

1. Navigieren Sie zur Registerkarte „Verwalten“.
> [!NOTE]
> Die Registerkarte „Verwalten“ ist möglicherweise nicht für alle Data Factory-Instanzen verfügbar. Wird sie nicht angezeigt, können Sie wie folgt auf private Endpunkte zugreifen: Registerkarte **Ersteller** > **Verbindungen** > **Privater Endpunkt**.

1. Navigieren Sie zum Abschnitt „Verwaltete private Endpunkte“.

1. Wählen Sie unter „Verwaltete private Endpunkte“ die Option **+ Neu** aus.

    ![Neuer verwalteter privater Endpunkt](./media/tutorial-copy-data-portal-private/new-managed-private-endpoint.png) 

1. Wählen Sie in der Liste die Kachel „Azure Blob Storage“ und dann **Weiter** aus.

1. Geben Sie den Namen des zuvor erstellten Speicherkontos ein.

1. Klicken Sie auf **Erstellen**.

1. Nach einigen Sekunden Wartezeit sollten Sie informiert werden, dass für den erstellten privaten Link eine Genehmigung erforderlich ist.

1. Wählen Sie den zuvor erstellten privaten Endpunkt aus. Ein Link wird angezeigt, über den Sie den privaten Endpunkt auf Storage-Kontoebene genehmigen können.

    ![Verwalten eines privaten Endpunkts](./media/tutorial-copy-data-portal-private/manage-private-endpoint.png) 

#### <a name="approval-of-a-private-link-in-storage-account"></a>Genehmigung einer privaten Verbindung im Speicherkonto
1. Navigieren Sie im Storage-Konto im Abschnitt „Einstellungen“ zu **Verbindungen mit privatem Endpunkt**.

1. Aktivieren Sie den privaten Endpunkt, den Sie oben erstellt haben, und wählen Sie **Genehmigen** aus.

    ![Genehmigen des privaten Endpunkts](./media/tutorial-copy-data-portal-private/approve-private-endpoint.png)

1. Fügen Sie eine Beschreibung hinzu, und klicken Sie auf **ja**.
1. Gehen Sie zurück zum Abschnitt **Verwaltete private Endpunkte** der Registerkarte **Verwalten** in Azure Data Factory.
1. Es sollte ungefähr ein bis zwei Minuten dauern, bis die Genehmigung für Ihren privaten Endpunkt auf der Azure Data Factory-Benutzeroberfläche angezeigt wird.


### <a name="configure-sink"></a>Konfigurieren der Senke
>[!TIP]
>In diesem Tutorial verwenden Sie *SQL-Authentifizierung* als Authentifizierungstyp für Ihren Senkendatenspeicher, Sie können bei Bedarf aber auch andere unterstützte Authentifizierungsmethoden auswählen: *Dienstprinzipal* und *Verwaltete Identität*. Ausführlichere Informationen finden Sie in den entsprechenden Abschnitten [dieses Artikels](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-database#linked-service-properties).
>Zum sicheren Speichern von Geheimnissen für Datenspeicher empfiehlt sich darüber hinaus die Verwendung einer Azure Key Vault-Instanz. Eine ausführliche Erläuterung finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault).

#### <a name="create-sink-dataset-and-linked-service"></a>Erstellen eines Senkendataset und eines verknüpften Diensts
1. Wechseln Sie zur Registerkarte **Senke**, und klicken Sie auf **+ Neu**, um ein Senkendataset zu erstellen.

1. Geben Sie im Dialogfeld **Neues Dataset** in das Suchfeld „SQL“ ein, um die Connectors zu filtern. Wählen Sie anschließend **Azure SQL-Datenbank** und dann **Weiter** aus. In diesem Tutorial kopieren Sie Daten in eine SQL-Datenbank.

1. Geben Sie im Dialogfeld **Eigenschaften festlegen** als Name **OutputSqlDataset** ein. Klicken Sie in der Dropdownliste **Verknüpfter Dienst** auf **+ Neu**. Einem verknüpften Dienst muss ein Dataset zugewiesen werden. Der verknüpfte Dienst enthält die Verbindungszeichenfolge, die Data Factory zum Herstellen einer Verbindung mit der SQL-Datenbank zur Laufzeit verwendet. Das Dataset gibt den Container, den Ordner und (optional) die Datei an, in die die Quelldaten kopiert werden.

1. Führen Sie im Dialogfeld **New Linked Service (Azure SQL Database)** (Neuer verknüpfter Dienst (Azure SQL-Datenbank)) die folgenden Schritte aus:

    1. Geben Sie unter **Name** den Namen **AzureSqlDatabaseLinkedService** ein.
    1. Wählen Sie unter **Servername** Ihre SQL Server-Instanz aus.
    1. Aktivieren Sie unbedingt **Interactive Authoring** (Interaktive Dokumenterstellung).
    1. Wählen Sie unter **Datenbankname** Ihre SQL-Datenbank aus.
    1. Geben Sie unter **Benutzername** den Namen des Benutzers ein.
    1. Geben Sie unter **Kennwort** das Kennwort für den Benutzer ein.
    1. Klicken Sie auf **Verbindung testen**. Ein Fehler tritt auf, weil der SQL-Server nur Zugriff über ausgewählte Netzwerke zulässt und Azure Data Factory für die Erstellung eines privaten Endpunkts erforderlich ist, der vor der Verwendung genehmigt werden muss. In der Fehlermeldung sollte ein Link zum Erstellen eines **privaten Endpunkts** angezeigt werden, dem Sie folgen können, um einen verwalteten privaten Endpunkt zu erstellen. *Alternativ dazu können Sie direkt zur Registerkarte „Verwalten“ navigieren und die Anweisungen im nächsten Abschnitt befolgen, um einen verwalteten privaten Endpunkt zu erstellen.*
    1. Lassen Sie das Dialogfeld geöffnet, und navigieren Sie dann zum oben ausgewählten SQL-Server.    
    1. Befolgen Sie die Anweisungen in [diesem Abschnitt](#approval-of-a-private-link-in-sql-server), um die private Verbindung zu genehmigen.
    1. Gehen Sie zurück zum Dialogfeld. Wählen Sie erneut **Verbindung testen** und anschließend **Erstellen** aus, um den verknüpften Dienst bereitzustellen.

1. Das Dialogfeld **Eigenschaften festlegen** wird automatisch geöffnet. Wählen Sie unter **Tabelle** die Option **[dbo].[emp]** aus. Klicken Sie anschließend auf **OK**.

1. Wechseln Sie zur Registerkarte mit der Pipeline, und überprüfen Sie, ob für **Senkendataset** die Option **OutputSqlDataset** ausgewählt ist.

    ![Pipelineregisterkarte](./media/tutorial-copy-data-portal-private/pipeline-tab-2.png)       

Optional können Sie das Schema der Quelle dem entsprechenden Zielschema zuordnen. Befolgen Sie dazu die Anweisungen unter [Schemazuordnung in Kopieraktivität](https://docs.microsoft.com/azure/data-factory/copy-activity-schema-and-type-mapping).

#### <a name="create-a-managed-private-endpoint"></a>Erstellen eines verwalteten privaten Endpunkts

Sollten Sie beim Testen der oben genannten Verbindung nicht auf den Link geklickt haben, folgen Sie dem nachstehenden Pfad. Jetzt müssen Sie einen verwalteten privaten Endpunkt erstellen, den Sie mit dem zuvor erstellten verknüpften Dienst verbinden werden.

1. Navigieren Sie zur Registerkarte „Verwalten“.
1. Navigieren Sie zum Abschnitt „Verwaltete private Endpunkte“.
1. Wählen Sie unter „Verwaltete private Endpunkte“ die Option **+ Neu** aus.

    ![Neuer verwalteter privater Endpunkt](./media/tutorial-copy-data-portal-private/new-managed-private-endpoint.png) 

1. Wählen Sie in der Liste die Kachel „Azure SQL-Datenbank“ und dann **Weiter** aus.
1. Geben Sie den Namen des SQL-Servers ein, den Sie oben ausgewählt haben.
1. Klicken Sie auf **Erstellen**.
1. Nach einigen Sekunden Wartezeit sollten Sie informiert werden, dass für den erstellten privaten Link eine Genehmigung erforderlich ist.
1. Wählen Sie den zuvor erstellten privaten Endpunkt aus. Es wird ein Link angezeigt, über den Sie den privaten Endpunkt auf der Ebene des SQL-Servers genehmigen können.


#### <a name="approval-of-a-private-link-in-sql-server"></a>Genehmigung einer privaten Verbindung in der SQL Server-Instanz
1. Navigieren Sie auf dem SQL-Server im Abschnitt „Einstellungen“ zu **Verbindungen mit privatem Endpunkt**.
1. Aktivieren Sie den privaten Endpunkt, den Sie oben erstellt haben, und wählen Sie **Genehmigen** aus.
1. Fügen Sie eine Beschreibung hinzu, und klicken Sie auf **ja**.
1. Gehen Sie zurück zum Abschnitt **Verwaltete private Endpunkte** der Registerkarte **Verwalten** in Azure Data Factory.
1. Es sollte ungefähr ein bis zwei Minuten dauern, bis die Genehmigung für Ihren privaten Endpunkt angezeigt wird.

#### <a name="debug-and-publish-the-pipeline"></a>Debuggen und Veröffentlichen der Pipeline

Sie können vor dem Veröffentlichen von Artefakten (verknüpfte Dienste, Datasets und Pipeline) in Data Factory oder Ihrem eigenen Azure DevOps-Git-Repository eine Pipeline debuggen.

1. Klicken Sie auf der Symbolleiste auf **Debuggen**, um die Pipeline zu debuggen. Der Status der Pipelineausführung wird unten im Fenster auf der Registerkarte **Ausgabe** angezeigt.
2. Sobald die Pipeline erfolgreich ausgeführt werden kann, klicken Sie in der oberen Symbolleiste auf **Alle veröffentlichen**. Mit dieser Aktion werden erstellte Entitäten (Datasets und Pipelines) in Data Factory veröffentlicht.
3. Warten Sie, bis die Meldung **Erfolgreich veröffentlicht** angezeigt wird. Damit Benachrichtigungsmeldungen angezeigt werden, klicken Sie oben rechts auf **Benachrichtigungen anzeigen** (Schaltfläche mit Glocke).


#### <a name="summary"></a>Zusammenfassung
Die Pipeline in diesem Beispiel kopiert mithilfe eines privaten Endpunkts in einem verwalteten virtuellen Netzwerk Daten aus Blobspeicher in Azure SQL-Datenbank. Sie haben Folgendes gelernt:

> * Erstellen einer Data Factory
> * Erstellen einer Pipeline mit einer Kopieraktivität

