---
title: Datentool zum schrittweisen Kopieren neuer und aktualisierter Dateien
description: Erstellen Sie eine Azure Data Factory, und verwenden Sie dann das Tool zum Kopieren von Daten, um neue Dateien auf Basis von „LastModifiedDate“ inkrementell zu laden.
services: data-factory
author: dearandyxu
ms.author: yexu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.devlang: na
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 3/18/2020
ms.openlocfilehash: 743f9dd8f7998178a75d716f4da22efee2b3bc79
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80130999"
---
# <a name="incrementally-copy-new-and-changed-files-based-on-lastmodifieddate-by-using-the-copy-data-tool"></a>Inkrementelles Kopieren neuer und geänderter Dateien auf Basis von LastModifiedDate und mithilfe des Tools zum Kopieren von Daten

In diesem Tutorial verwenden Sie das Azure-Portal, um eine Data Factory zu erstellen. Anschließend verwenden Sie das Tool zum Kopieren von Daten, um eine Pipeline zu erstellen, die neue und geänderte Dateien nur auf Basis von **LastModifiedDate** inkrementell von einem in den anderen Azure-Blobspeicher kopiert.

Auf diese Weise überprüft ADF alle Dateien aus dem Quellspeicher, wendet den Filter auf deren „LastModifiedDate“ an und kopiert nur die Dateien in den Zielspeicher, die neu sind oder seit dem letzten Mal aktualisiert wurden.  Wenn Sie ADF sehr viele Dateien überprüfen lassen, aber nur wenige Dateien in den Zielspeicherort kopieren, müssen Sie trotzdem mit einer langen Ausführungsdauer rechnen, denn auch das Überprüfen von Dateien ist zeitaufwändig.   

> [!NOTE]
> Falls Sie noch nicht mit Azure Data Factory vertraut sind, ist es ratsam, den Artikel [Einführung in Azure Data Factory](introduction.md) zu lesen.

In diesem Tutorial führen Sie die folgenden Aufgaben aus:

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen einer Pipeline mithilfe des Tools zum Kopieren von Daten
> * Überwachen der Pipeline- und Aktivitätsausführungen.

## <a name="prerequisites"></a>Voraussetzungen

* **Azure-Abonnement**: Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* **Azure-Speicherkonto**: Verwenden Sie den Blobspeicher als _Quelldatenspeicher_ und als _Senkendatenspeicher_. Falls Sie noch nicht über ein Azure-Speicherkonto verfügen, finden Sie Anweisungen dazu unter [Erstellen eines Speicherkontos](../storage/common/storage-account-create.md).

### <a name="create-two-containers-in-blob-storage"></a>Erstellen von zwei Containern im Blobspeicher

Bereiten Sie Ihren Blobspeicher folgendermaßen auf das Tutorial vor.

1. Erstellen Sie einen Container mit dem Namen **source** (Quelle). Sie können für diese Aufgabe verschiedene Tools verwenden, z. B. [Azure Storage-Explorer](https://storageexplorer.com/).

2. Erstellen Sie einen Container mit dem Namen **destination** (Ziel).

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory

1. Wählen Sie im Menü auf der linken Seite **Ressource erstellen** > **Daten + Analysen** > **Data Factory** aus:

   ![Auswählen von „Data Factory“ im Bereich „Neu“](./media/doc-common-process/new-azure-data-factory-menu.png)

2. Geben Sie auf der Seite **Neue Data Factory** unter **Name** den Namen **ADFTutorialDataFactory** ein.

   Der Name der Data Factory muss _global eindeutig_ sein. Sie erhalten unter Umständen die folgende Fehlermeldung:

   ![Fehlermeldung zu neuer Data Factory](./media/doc-common-process/name-not-available-error.png)

   Wenn eine Fehlermeldung zum Namenswert angezeigt wird, geben Sie einen anderen Namen für die Data Factory ein. Verwenden Sie beispielsweise den Namen _**IhrName**_ **ADFTutorialDataFactory**. Benennungsregeln für Data Factory-Artefakte finden Sie im Thema [Data Factory – Benennungsregeln](naming-rules.md).
3. Wählen Sie das **Azure-Abonnement** aus, in dem Sie die neue Data Factory erstellen werden.
4. Führen Sie unter **Ressourcengruppe** einen der folgenden Schritte aus:

    * Wählen Sie die Option **Use existing**(Vorhandene verwenden) und dann in der Dropdownliste eine vorhandene Ressourcengruppe.

    * Wählen Sie **Neu erstellen**, und geben Sie den Namen einer Ressourcengruppe ein. 
         
    Weitere Informationen zu Ressourcengruppen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/management/overview.md).

5. Wählen Sie unter **Version** die Option **V2** aus.
6. Wählen Sie unter **Standort** den Standort für die Data Factory aus. In der Dropdownliste werden nur unterstützte Standorte angezeigt. Die Datenspeicher (z. B. Azure Storage und SQL-Datenbank) und Computeeinheiten (z. B. Azure HDInsight), die von der Data Factory genutzt werden, können sich an anderen Standorten und in anderen Regionen befinden.
8. Klicken Sie auf **Erstellen**.
9. Nach Abschluss der Erstellung wird die Startseite von **Data Factory** angezeigt.
10. Klicken Sie auf die Kachel **Erstellen und überwachen**, um die Azure Data Factory-Benutzeroberfläche (User Interface, UI) auf einer separaten Registerkarte zu öffnen.

    ![Data Factory-Startseite](./media/doc-common-process/data-factory-home-page.png)

## <a name="use-the-copy-data-tool-to-create-a-pipeline"></a>Erstellen einer Pipeline mithilfe des Tools zum Kopieren von Daten

1. Klicken Sie auf der Seite **Let's get started** (Erste Schritte) auf **Copy Data** (Daten kopieren), um das Tool zum Kopieren von Daten zu öffnen.

   ![Kachel für das Tool zum Kopieren von Daten](./media/doc-common-process/get-started-page.png)

2. Gehen Sie auf der Seite **Properties** (Eigenschaften) wie folgt vor:

    a. Geben Sie unter **Task name** (Aufgabenname) **DeltaCopyFromBlobPipeline** ein.

    b. Wählen Sie unter **Task cadence** (Aufgabenhäufigkeit) oder **Task schedule** (Aufgabenzeitplan) die Option **Run regularly on schedule** (Nach Zeitplan regelmäßig ausführen) aus.

    c. Wählen Sie unter **Trigger type** (Triggertyp) die Option **Tumbling Window** (Rollierendes Fenster) aus.

    d. Geben Sie unter **Recurrence** (Wiederholung) **15 Minute(s)** (15 Minuten) ein.

    e. Wählen Sie **Weiter** aus.

    Über die Data Factory-Benutzeroberfläche wird eine Pipeline mit dem angegebenen Tasknamen erstellt.

    ![Eigenschaftenseite](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/copy-data-tool-properties-page.png)

3. Führen Sie auf der Seite **Quelldatenspeicher** die folgenden Schritte aus:

    a. Wählen Sie **+ Create new connection** (+ Neue Verbindung erstellen) aus, um eine Verbindung hinzuzufügen.

    b. Wählen Sie im Katalog **Azure Blob Storage** aus, und klicken Sie dann auf **Fortsetzen**.

    ![Seite „Quelldatenspeicher“](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/source-data-store-page-select-blob.png)

    c. Wählen Sie auf der Seite **New Linked Service (Azure Blob Storage)** (Neuer verknüpfter Dienst (Azure Blob Storage)) in der Liste **Storage account name** (Speicherkontoname) Ihr Speicherkonto aus. Testen Sie die Verbindung, und wählen Sie **Fertig stellen** aus.

    d. Wählen Sie den neu erstellten verknüpften Dienst und dann **Next** (Weiter) aus.

   ![Seite „Quelldatenspeicher“](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/source-data-store-page-select-linkedservice.png)

4. Führen Sie auf der Seite **Choose the input file or folder** (Eingabedatei oder -ordner auswählen) die folgenden Schritte aus:

    a. Suchen Sie nach dem Ordner **source**, wählen Sie ihn aus, und klicken Sie anschließend auf **Choose** (Wählen).

    ![Auswählen der Eingabedatei bzw. des Eingabeordners](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/choose-input-file-folder.png)

    b. Wählen Sie unter **File loading behavior** (Dateiladeverhalten) die Option **Incremental load: LastModifiedDate** (Inkrementelles Laden: LastModifiedDate) aus.

    c. Aktivieren Sie das Kontrollkästchen **Binary copy** (Binärkopie), und wählen Sie **Next** (Weiter) aus.

     ![Auswählen der Eingabedatei bzw. des Eingabeordners](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/check-binary-copy.png)

5. Wählen Sie auf der Seite **Destination data store** (Zieldatenspeicher) den von Ihnen erstellten **AzureBlobStorage** aus. Dies ist dasselbe Speicherkonto wie der Quelldatenspeicher. Wählen Sie **Weiter**aus.

6. Führen Sie auf der Seite **Choose the output file or folder** (Ausgabedatei oder -ordner auswählen) die folgenden Schritte aus:

    a. Suchen Sie nach dem Ordner **destination**, wählen Sie ihn aus, und klicken Sie anschließend auf **Choose** (Wählen).

    ![Auswählen der Ausgabedatei oder des Ausgabeordners](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/choose-output-file-folder.png)

    b. Wählen Sie **Weiter** aus.

7. Klicken Sie auf der Seite **Einstellungen** auf **Weiter**.

8. Überprüfen Sie auf der Seite **Zusammenfassung** die Einstellungen, und klicken Sie anschließend auf **Weiter**.

    ![Seite „Zusammenfassung“](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/summary-page.png)

9. Klicken Sie auf der Seite **Bereitstellung** auf **Überwachen**, um die Pipeline (Task) zu überwachen.

    ![Bereitstellungsseite](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/deployment-page.png)

10. Beachten Sie, dass die Registerkarte **Überwachen** auf der linken Seite automatisch ausgewählt ist. Die Anwendung wechselt zur Registerkarte **Überwachen**. Dort wird der Status der Pipeline angezeigt. Klicken Sie zum Aktualisieren der Liste auf **Aktualisieren**. Klicken Sie auf den Link unter **PIPELINENAME**, um Details zur Aktivitätsausführung anzuzeigen oder die Pipeline erneut auszuführen. 

    ![Aktualisieren der Liste und Auswählen von „View Activity Runs“ (Aktivitätsausführungen anzeigen)](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs1.png)

11. Da die Pipeline nur eine Aktivität (Copy-Aktivität) enthält, wird nur ein Eintrag angezeigt. Wenn Sie Details zum Kopiervorgang anzeigen möchten, wählen Sie in der Spalte **ACTIVTIY NAME** (AKTIVITÄTSNAME) den Link **Details** (das Brillensymbol) aus. Einzelheiten zu den Eigenschaften finden Sie unter [Kopieraktivität in Azure Data Factory](copy-activity-overview.md). 

    ![Copy-Aktivität befindet sich in der Pipeline](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs2.png)

    Da sich im Quellcontainer (**source**) in Ihrem Blobspeicherkonto keine Datei befindet, wird in Ihrem Blobspeicherkonto keine Datei angezeigt, die in den Zielcontainer (**destination**) kopiert wurde.

    ![Keine Datei in Quell- oder Zielcontainer](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs3.png)

12. Erstellen Sie eine leere Textdatei, und nennen Sie sie **file1.txt**. Laden Sie diese Datei in den Quellcontainer (**source**) in Ihrem Speicherkonto hoch. Sie können für diese Aufgaben verschiedene Tools verwenden, etwa [Azure Storage-Explorer](https://storageexplorer.com/).

    ![Erstellen von „file1.txt“ und Hochladen in den Quellcontainer](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs3-1.png)

13. Wenn Sie zur Ansicht **Pipeline Runs** (Pipelineausführungen) zurückkehren möchten, wählen Sie **All Pipelines Runs** (Alle Pipelineausführungen) aus, und warten Sie, bis dieselbe Pipeline erneut automatisch ausgelöst wird.  

    ![Auswählen aller Pipelineausführungen](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs4.png)

14. Wenn die zweite Pipelineausführung abgeschlossen ist, führen Sie dieselben Schritte wie oben aus, um die Details zur Aktivitätsausführung zu überprüfen.  

    Sie werden sehen, dass eine Datei („file1.txt“) aus dem Quellcontainer (**source**) in den Zielcontainer (**destination**) Ihres Blobspeicherkontos kopiert wurde.

    ![File1.txt wurde aus dem Quellcontainer in den Zielcontainer kopiert](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs6.png)

15. Erstellen Sie eine weitere leere Textdatei, und benennen Sie diese **file2.txt**. Laden Sie diese Datei in den Quellcontainer (**source**) in Ihrem Blobspeicherkonto hoch.

16. Wiederholen Sie die Schritte 13 und 14 für diese zweite Textdatei. Sie werden bemerken, dass nur die neue Datei („file2.txt“) bei der nächsten Pipelineausführung vom Quellcontainer (**source**) in den Zielcontainer (**destination**) Ihres Speicherkontos kopiert wurde.  

    Sie können dies auch bestätigen, indem Sie den [Azure Storage-Explorer](https://storageexplorer.com/) verwenden, um die Dateien zu überprüfen.

    ![Überprüfen von Dateien mit Azure Storage-Explorer](./media/tutorial-incremental-copy-lastmodified-copy-data-tool/monitor-pipeline-runs8.png)


## <a name="next-steps"></a>Nächste Schritte
Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie mithilfe eines Apache Spark-Clusters in Azure Daten transformieren:

> [!div class="nextstepaction"]
>[Transformieren von Daten in der Cloud mithilfe von Apache Spark-Clustern](tutorial-transform-data-spark-portal.md)
