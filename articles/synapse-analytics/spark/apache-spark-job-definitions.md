---
title: 'Tutorial – Apache Spark für Azure Synapse Analytics: Apache Spark-Auftragsdefinition für Synapse'
description: 'Tutorial: Erstellen Sie Spark-Auftragsdefinitionen mithilfe von Azure Synapse Analytics, und übermitteln Sie sie an einen Pool für Apache Spark für Azure Synapse Analytics.'
author: hrasheed-msft
ms.author: jejiang
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 04/15/2020
ms.openlocfilehash: 5fc9dffaa73d195c842381b6682a00e9834c0fe7
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83587934"
---
# <a name="tutorial-use-azure-synapse-analytics-to-create-apache-spark-job-definitions-for-synapse-spark-pools"></a>Tutorial: Erstellen von Apache Spark-Auftragsdefinitionen für Synapse Spark-Pools mithilfe von Azure Synapse Analytics

In diesem Tutorial erfahren Sie, wie Sie Spark-Auftragsdefinitionen mithilfe von Azure Synapse Analytics erstellen und anschließend an einen Synapse Spark-Pool übermitteln. Sie können das Plug-In auf mehrere Arten verwenden:

* Entwickeln und Übermitteln einer Spark-Auftragsdefinition für einen Synapse Spark-Pool
* Anzeigen von Auftragsdetails nach der Übermittlung

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Entwickeln und Übermitteln einer Spark-Auftragsdefinition für einen Synapse Spark-Pool
> * Anzeigen von Auftragsdetails nach der Übermittlung

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure Synapse Analytics-Arbeitsbereich. Entsprechende Anweisungen finden Sie unter [Erstellen eines Arbeitsbereichs](../../machine-learning/how-to-manage-workspace.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#create-a-workspace).

## <a name="get-started"></a>Erste Schritte

Wenn Sie eine Spark-Auftragsdefinition übermitteln möchten, müssen Sie der Besitzer der Speicherblobdaten des ADLS Gen2-Dateisystems sein, das Sie verwenden möchten. Andernfalls müssen Sie die Berechtigung manuell hinzufügen.

### <a name="scenario-1-add-permission"></a>Szenario 1: Hinzufügen der Berechtigung

1. Öffnen Sie in [Microsoft Azure](https://ms.portal.azure.com) die Option „Speicherkonto“.

2. Klicken Sie auf **Container**, und erstellen Sie ein **Dateisystem**. In diesem Tutorial wird `sparkjob` verwendet.

    ![Klicken Sie auf die Schaltfläche „Übermitteln“, um die Spark-Auftragsdefinition zu übermitteln.](./media/apache-spark-job-definitions/open-azure-container.png)

    ![Dialogfeld für die Spark-Übermittlung](./media/apache-spark-job-definitions/create-new-filesystem.png)

3. Öffnen Sie `sparkjob`, klicken Sie auf **Zugriffssteuerung (IAM)** , klicken Sie auf **Hinzufügen**, und wählen Sie **Rollenzuweisung hinzufügen** aus.

    ![Klicken Sie auf die Schaltfläche „Übermitteln“, um die Spark-Auftragsdefinition zu übermitteln.](./media/apache-spark-job-definitions/add-role-assignment-01.png)

    ![Klicken Sie auf die Schaltfläche „Übermitteln“, um die Spark-Auftragsdefinition zu übermitteln.](./media/apache-spark-job-definitions/add-role-assignment-02.png)

4. Klicken Sie auf **Rollenzuweisungen**, geben Sie den Benutzernamen ein, und überprüfen Sie die Benutzerrolle.

    ![Klicken Sie auf die Schaltfläche „Übermitteln“, um die Spark-Auftragsdefinition zu übermitteln.](./media/apache-spark-job-definitions/verify-user-role.png)

### <a name="scenario-2-prepare-folder-structure"></a>Szenario 2: Vorbereiten der Ordnerstruktur

Vor dem Übermitteln einer Spark-Auftragsdefinition müssen Sie zunächst Dateien in ADLS Gen2 hochladen und dort die Ordnerstruktur vorbereiten. Wir verwenden zum Speichern von Dateien den Storage-Knoten in Synapse Studio.

1. Öffnen Sie [Azure Synapse Analytics](https://web.azuresynapse.net/).

2. Klicken Sie auf **Daten**, wählen Sie **Speicherkonten** aus, und laden Sie die relevanten Dateien in Ihr ADLS Gen2-Dateisystem hoch. Unterstützt werden Scala, Java, .NET und Python. Zur Veranschaulichung wird in diesem Tutorial das auf der Abbildung gezeigte Beispiel verwendet. Sie können die Projektstruktur jedoch nach Belieben ändern.

    ![Festlegen des Werts der Spark-Auftragsdefinition](./media/apache-spark-job-definitions/prepare-project-structure.png)

## <a name="create-a-spark-job-definition"></a>Erstellen einer Spark-Auftragsdefinition

1. Öffnen Sie [Azure Synapse Analytics](https://web.azuresynapse.net/), und wählen Sie **Entwickeln** aus.

2. Wählen Sie im linken Bereich die Option **Spark job definitions** (Spark-Auftragsdefinitionen) aus.

3. Klicken Sie rechts neben „Spark job definitions“ (Spark-Auftragsdefinitionen) auf den Knoten **Aktionen**.

     ![Erstellen einer neuen Spark-Auftragsdefinition](./media/apache-spark-job-definitions/create-new-definition-01.png)

4. Wählen Sie in der Dropdownliste **Aktionen** die Option **New Spark job definition** (Neue Spark-Auftragsdefinition) aus.

     ![Erstellen einer neuen Spark-Auftragsdefinition](./media/apache-spark-job-definitions/create-new-definition-02.png)

5. Wählen Sie im Fenster „New Spark job definition“ (Neue Spark-Auftragsdefinition) die Sprache aus, und geben Sie anschließend folgende Informationen an:  

   * Wählen Sie unter **Sprache** die Option **Spark (Scala)** aus.

    |  Eigenschaft   | BESCHREIBUNG   |  
    | ----- | ----- |  
    |Auftragsdefinitionsname| Geben Sie einen Namen für Ihre Spark-Auftragsdefinition ein.  In diesem Tutorial wird `job definition sample` verwendet. Dieser Name kann bis zur Veröffentlichung jederzeit aktualisiert werden.|  
    |„Main definition file“ (Hauptdefinitionsdatei)| Die für den Auftrag verwendete Hauptdatei. Wählen Sie eine JAR-Datei aus Ihrem Speicher aus. Sie können **Datei hochladen** auswählen, um die Datei in ein Speicherkonto hochzuladen. |
    |„Main class name“ (Name der Hauptklasse)| Der vollqualifizierte Bezeichner oder die Hauptklasse in der Hauptdefinitionsdatei.|
    |Befehlszeilenargumente| Optionale Argumente für den Auftrag.|
    |„Reference files“ (Referenzdateien)| Zusätzliche Dateien, die zu Referenzzwecken in der Hauptdefinitionsdatei verwendet werden. Sie können **Datei hochladen** auswählen, um die Datei in ein Speicherkonto hochzuladen.|
    |Spark-Pool| Der Auftrag wird an den ausgewählten Spark-Pool übermittelt.|
    |Spark-Version| Die vom Spark-Pool verwendete Version von Spark.|
    |Ausführer| Die gewünschte Anzahl von Executors im angegebenen Spark-Pool für den Auftrag.|
    |„Executor size“ (Executor-Größe)| Die Anzahl von Kernen und die Menge an Arbeitsspeicher, die für Executors im angegebenen Spark-Pool für den Auftrag verwendet werden sollen.|  
    |„Driver size“ (Treibergröße)| Die Anzahl von Kernen und die Menge an Arbeitsspeicher, die für Treiber im angegebenen Spark-Pool für den Auftrag verwendet werden sollen.|

    ![Festlegen des Werts der Spark-Auftragsdefinition](./media/apache-spark-job-definitions/create-scala-definition.png)

   * Wählen Sie unter **Sprache** die Option **PySpark (Python)** aus.

    |  Eigenschaft   | BESCHREIBUNG   |  
    | ----- | ----- |  
    |Auftragsdefinitionsname| Geben Sie einen Namen für Ihre Spark-Auftragsdefinition ein.  In diesem Tutorial wird `job definition sample` verwendet. Dieser Name kann bis zur Veröffentlichung jederzeit aktualisiert werden.|  
    |„Main definition file“ (Hauptdefinitionsdatei)| Die für den Auftrag verwendete Hauptdatei. Wählen Sie eine PY-Datei aus Ihrem Speicher aus. Sie können **Datei hochladen** auswählen, um die Datei in ein Speicherkonto hochzuladen.|
    |Befehlszeilenargumente| Optionale Argumente für den Auftrag.|
    |„Reference files“ (Referenzdateien)| Zusätzliche Dateien, die zu Referenzzwecken in der Hauptdefinitionsdatei verwendet werden. Sie können **Datei hochladen** auswählen, um die Datei in ein Speicherkonto hochzuladen.|
    |Spark-Pool| Der Auftrag wird an den ausgewählten Spark-Pool übermittelt.|
    |Spark-Version| Die vom Spark-Pool verwendete Version von Spark.|
    |Ausführer| Die gewünschte Anzahl von Executors im angegebenen Spark-Pool für den Auftrag.|
    |„Executor size“ (Executor-Größe)| Die Anzahl von Kernen und die Menge an Arbeitsspeicher, die für Executors im angegebenen Spark-Pool für den Auftrag verwendet werden sollen.|  
    |„Driver size“ (Treibergröße)| Die Anzahl von Kernen und die Menge an Arbeitsspeicher, die für Treiber im angegebenen Spark-Pool für den Auftrag verwendet werden sollen.|

    ![Festlegen des Werts der Spark-Auftragsdefinition](./media/apache-spark-job-definitions/create-py-definition.png)

   * Wählen Sie unter **Sprache** die Option **.NET Spark (C#/F#)** aus.

    |  Eigenschaft   | BESCHREIBUNG   |  
    | ----- | ----- |  
    |Auftragsdefinitionsname| Geben Sie einen Namen für Ihre Spark-Auftragsdefinition ein.  In diesem Tutorial wird `job definition sample` verwendet. Dieser Name kann bis zur Veröffentlichung jederzeit aktualisiert werden.|  
    |„Main definition file“ (Hauptdefinitionsdatei)| Die für den Auftrag verwendete Hauptdatei. Wählen Sie in Ihrem Speicher eine ZIP-Datei aus, die Ihre Anwendung vom Typ „.NET für Spark“ enthält (ausführbare Hauptdatei, DLLs mit benutzerdefinierten Funktionen und andere erforderliche Dateien). Sie können **Datei hochladen** auswählen, um die Datei in ein Speicherkonto hochzuladen.|
    |„Main executable file“ (Ausführbare Hauptdatei)| Die ausführbare Hauptdatei in der ZIP-Datei der Hauptdefinition.|
    |Befehlszeilenargumente| Optionale Argumente für den Auftrag.|
    |„Reference files“ (Referenzdateien)| Zusätzliche Dateien, die von den Workerknoten zum Ausführen der Anwendung vom Typ „.NET für Spark“ benötigt werden und nicht in der ZIP-Datei der Hauptdefinition enthalten sind (also abhängige JAR-Dateien, zusätzliche DLLs mit benutzerdefinierten Funktionen sowie andere Konfigurationsdateien). Sie können **Datei hochladen** auswählen, um die Datei in ein Speicherkonto hochzuladen.|
    |Spark-Pool| Der Auftrag wird an den ausgewählten Spark-Pool übermittelt.|
    |Spark-Version| Die vom Spark-Pool verwendete Version von Spark.|
    |Ausführer| Die gewünschte Anzahl von Executors im angegebenen Spark-Pool für den Auftrag.|
    |„Executor size“ (Executor-Größe)| Die Anzahl von Kernen und die Menge an Arbeitsspeicher, die für Executors im angegebenen Spark-Pool für den Auftrag verwendet werden sollen.|  
    |„Driver size“ (Treibergröße)| Die Anzahl von Kernen und die Menge an Arbeitsspeicher, die für Treiber im angegebenen Spark-Pool für den Auftrag verwendet werden sollen.|

    ![Festlegen des Werts der Spark-Auftragsdefinition](./media/apache-spark-job-definitions/create-net-definition.png)

6. Wählen Sie **Veröffentlichen** aus, um die Spark-Auftragsdefinition zu speichern.

    ![Veröffentlichen der Spark-Auftragsdefinition](./media/apache-spark-job-definitions/publish-net-definition.png)

## <a name="submit-a-spark-job-definition"></a>Übermitteln einer Spark-Auftragsdefinition

Nachdem Sie eine Spark-Auftragsdefinition erstellt haben, können Sie sie an einen Synapse Spark-Pool übermitteln. Stellen Sie sicher, dass Sie die **ersten Schritte** absolviert haben, bevor Sie die Beispiele in diesem Teil ausprobieren.

### <a name="scenario-1-submit-spark-job-definition"></a>Szenario 1: Übermitteln einer Spark-Auftragsdefinition

1. Öffnen Sie ein Spark-Auftragsdefinitionsfenster, indem Sie darauf klicken.

      ![Öffnen der zu übermittelnden Spark-Auftragsdefinition ](./media/apache-spark-job-definitions/open-spark-definition.png)

2. Klicken Sie auf **Übermitteln**, um Ihr Projekt an den ausgewählten Spark-Pool zu übermitteln. Sie können auf die Registerkarte **Spark monitoring URL** (Spark-Überwachungs-URL) klicken, um die Protokollabfrage der Spark-Anwendung anzuzeigen.

    ![Klicken Sie auf die Schaltfläche „Übermitteln“, um die Spark-Auftragsdefinition zu übermitteln.](./media/apache-spark-job-definitions/submit-spark-definition.png)

    ![Dialogfeld für die Spark-Übermittlung](./media/apache-spark-job-definitions/submit-definition-result.png)

### <a name="scenario-2-view-spark-job-running-progress"></a>Szenario 2: Anzeigen des Status der Spark-Auftragsausführung

1. Klicken Sie auf **Überwachen**, und wählen Sie dann die Option **Spark-Anwendungen** aus. Navigieren Sie zur übermittelten Spark-Anwendung.

    ![Anzeigen der Spark-Anwendung](./media/apache-spark-job-definitions/view-spark-application.png)

2. Klicken Sie auf die Spark-Anwendung. Daraufhin wird das Fenster **Log Query** (Protokollabfrage) angezeigt. Sie können den Status der Auftragsausführung anhand der **Protokollabfrage** ermitteln.

    ![Anzeigen der Protokollabfrage für die Spark-Anwendung](./media/apache-spark-job-definitions/view-job-log-query.png)

### <a name="scenario-3-check-output-file"></a>Szenario 3: Überprüfen der Ausgabedatei

 1. Klicken Sie auf **Daten**, und wählen Sie **Speicherkonten** aus. Nach erfolgreicher Ausführung können Sie zum ADLS Gen2-Speicher navigieren und sich vergewissern, dass Ausgaben generiert wurden.

    ![Anzeigen der Ausgabedatei](./media/apache-spark-job-definitions/view-output-file.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie Spark-Auftragsdefinitionen mithilfe von Azure Synapse Analytics erstellen und anschließend an einen Synapse Spark-Pool übermitteln. Als Nächstes können Sie Azure Synapse Analytics verwenden, um Power BI-Datasets zu erstellen und Power BI-Daten zu verwalten. 

- [Schnellstart: Verbinden mit Daten in Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-quickstart-connect-to-data)
- [Visualisieren mit Power BI](../sql-data-warehouse/sql-data-warehouse-get-started-visualize-with-power-bi.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
