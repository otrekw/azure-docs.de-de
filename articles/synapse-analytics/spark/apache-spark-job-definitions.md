---
title: 'Tutorial: Erstellen einer Apache Spark-Auftragsdefinition in Synapse Studio'
description: 'Tutorial: Erstellen Sie Spark-Auftragsdefinitionen mithilfe von Azure Synapse Analytics, und übermitteln Sie sie an einen Pool für Apache Spark für Azure Synapse Analytics.'
author: hrasheed-msft
ms.author: jejiang
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 04/15/2020
ms.openlocfilehash: a40099c06946830808a235fce2915d28b7647fa5
ms.sourcegitcommit: c4ad4ba9c9aaed81dfab9ca2cc744930abd91298
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/12/2020
ms.locfileid: "84735802"
---
# <a name="tutorial-create-apache-spark-job-definition-in-synapse-studio"></a>Tutorial: Erstellen einer Apache Spark-Auftragsdefinition in Synapse Studio

In diesem Tutorial erfahren Sie, wie Sie Apache Spark-Auftragsdefinitionen mithilfe von Azure Synapse Studio erstellen und anschließend an einen Apache Spark-Pool übermitteln.

Dieses Tutorial enthält die folgenden Aufgaben:

* Erstellen einer Apache Spark-Auftragsdefinition für PySpark (Python)
* Erstellen einer Apache Spark-Auftragsdefinition für Spark (Scala)
* Erstellen einer Apache Spark-Auftragsdefinition für .NET Spark (C#)
* Übermitteln einer Apache Spark-Auftragsdefinition als Batchauftrag
* Hinzufügen einer Apache Spark-Auftragsdefinition in einer Pipeline

## <a name="prerequisites"></a>Voraussetzungen

Vergewissern Sie sich zunächst, dass die folgenden Anforderungen erfüllt sind bzw. dass Folgendes vorhanden ist:

* Ein Azure Synapse Analytics-Arbeitsbereich. Entsprechende Anweisungen finden Sie unter [Erstellen eines Arbeitsbereichs](../../machine-learning/how-to-manage-workspace.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#create-a-workspace).
* Ein Apache Spark-Pool
* Ein ADLS Gen2-Speicherkonto. Sie müssen der Besitzer der Speicherblobdaten des ADLS Gen2-Dateisystems sein, das Sie verwenden möchten. Andernfalls müssen Sie die Berechtigung manuell hinzufügen.

## <a name="create-an-apache-spark-job-definition-for-pyspark-python"></a>Erstellen einer Apache Spark-Auftragsdefinition für PySpark (Python)

In diesem Abschnitt erstellen Sie eine Apache Spark-Auftragsdefinition für PySpark (Python).

1. Öffnen Sie [Azure Synapse Studio](https://web.azuresynapse.net/).

2. Sie können zu [Beispieldateien zum Erstellen von Apache Spark-Auftragsdefinitionen](https://github.com/Azure-Samples/Synapse/tree/master/Spark/Python) wechseln, um **wordcount.jar** und **shakespear.txt** herunterzuladen. Laden Sie dann diese Dateien in Azure Storage hoch: Klicken Sie auf **Daten**, wählen Sie **Speicherkonten** aus, und laden Sie die relevanten Dateien in Ihr ADLS Gen2-Dateisystem hoch. Überspringen Sie diesen Schritt, wenn sich Ihre Dateien bereits in Azure Storage befinden. 

     ![Python-Datei hochladen](./media/apache-spark-job-definitions/upload-python-file.png)

3. Klicken Sie auf den Hub **Entwickeln**, wählen Sie im linken Bereich **Spark-Auftragsdefinitionen** aus, klicken Sie auf den Aktionsknoten „...“ neben der **Spark-Auftragsdefinition**, und wählen Sie dann im Kontextmenü **Neue Spark-Auftragsdefinition** aus.

     ![Neue Definition für Python erstellen](./media/apache-spark-job-definitions/create-new-definition.png)

4. Wählen Sie in der Dropdownliste „Sprache“ im Hauptfenster der Apache Spark-Auftragsdefinition **PySpark (Python)** aus.

5. Tragen Sie Informationen für die Apache Spark-Auftragsdefinition ein. Sie können die Beispielinformationen kopieren.

     |  Eigenschaft   | BESCHREIBUNG   |  
     | ----- | ----- |  
     |Auftragsdefinitionsname| Geben Sie einen Namen für Ihre Apache Spark-Auftragsdefinition ein. Dieser Name kann bis zur Veröffentlichung jederzeit aktualisiert werden. Beispiel: `job definition sample`|
     |„Main definition file“ (Hauptdefinitionsdatei)| Die für den Auftrag verwendete Hauptdatei. Wählen Sie eine PY-Datei aus Ihrem Speicher aus. Sie können **Datei hochladen** auswählen, um die Datei in ein Speicherkonto hochzuladen. Beispiel: `abfss://azureportaldeploy@storageaccountname.dfs.core.windows.net/synapse/workspaces/workspace name/batchjobs/python/fileexists.py`|
     |Befehlszeilenargumente| Optionale Argumente für den Auftrag. Beispiel: `shakespeare.txt`|
     |„Reference files“ (Referenzdateien)| Zusätzliche Dateien, die zu Referenzzwecken in der Hauptdefinitionsdatei verwendet werden. Sie können **Datei hochladen** auswählen, um die Datei in ein Speicherkonto hochzuladen. Beispiel: `abfss://azureportaldeploy@storageaccountname.dfs.core.windows.net/synapse/workspaces/workspace name/batchjobs/python/shakespeare.txt`|
     |Spark-Pool| Der Auftrag wird an den ausgewählten Apache Spark-Pool übermittelt.|
     |Spark-Version| Die vom Apache Spark-Pool ausgeführte Version von Apache Spark.|
     |Ausführer| Die gewünschte Anzahl von Executors im angegebenen Apache Spark-Pool für den Auftrag.|
     |„Executor size“ (Executorgröße)| Die Anzahl von Kernen und die Menge an Arbeitsspeicher, die für Executors im angegebenen Apache Spark-Pool für den Auftrag verwendet werden sollen.|  
     |„Driver size“ (Treibergröße)| Die Anzahl von Kernen und die Menge an Arbeitsspeicher, die für Treiber im angegebenen Apache Spark-Pool für den Auftrag verwendet werden sollen.|

     ![Festlegen des Werts der Spark-Auftragsdefinition für Python](./media/apache-spark-job-definitions/create-py-definition.png)

6. Wählen Sie **Veröffentlichen** aus, um die Apache Spark-Auftragsdefinition zu speichern.

     ![Py-Definition veröffentlichen](./media/apache-spark-job-definitions/publish-py-definition.png)

## <a name="create-an-apache-spark-job-definition-for-apache-sparkscala"></a>Erstellen einer Apache Spark-Auftragsdefinition für Apache Spark (Scala)

In diesem Abschnitt erstellen Sie eine Apache Spark-Auftragsdefinition für Apache Spark (Scala).

 1. Öffnen Sie [Azure Synapse Studio](https://web.azuresynapse.net/).

 2. Sie können zu [Beispieldateien zum Erstellen von Apache Spark-Auftragsdefinitionen](https://github.com/Azure-Samples/Synapse/tree/master/Spark/Scala) wechseln, um **wordcount.jar** und **shakespear.txt** herunterzuladen. Laden Sie dann diese Dateien in Azure Storage hoch: Klicken Sie auf **Daten**, wählen Sie **Speicherkonten** aus, und laden Sie die relevanten Dateien in Ihr ADLS Gen2-Dateisystem hoch. Überspringen Sie diesen Schritt, wenn sich Ihre Dateien bereits in Azure Storage befinden. 
 
     ![Scala-Struktur vorbereiten](./media/apache-spark-job-definitions/prepare-scala-structure.png)

 3. Klicken Sie auf den Hub **Entwickeln**, wählen Sie im linken Bereich **Spark-Auftragsdefinitionen** aus, klicken Sie auf den Aktionsknoten „...“ neben der **Spark-Auftragsdefinition**, und wählen Sie dann im Kontextmenü **Neue Spark-Auftragsdefinition** aus.
     ![Neue Definition für Scala erstellen](./media/apache-spark-job-definitions/create-new-definition.png)

 4. Wählen Sie in der Dropdownliste „Sprache“ im Hauptfenster der Apache Spark-Auftragsdefinition **Spark (Scala)** aus.

 5. Tragen Sie Informationen für die Apache Spark-Auftragsdefinition ein. Sie können die Beispielinformationen kopieren.

     |  Eigenschaft   | BESCHREIBUNG   |  
     | ----- | ----- |  
     |Auftragsdefinitionsname| Geben Sie einen Namen für Ihre Apache Spark-Auftragsdefinition ein. Dieser Name kann bis zur Veröffentlichung jederzeit aktualisiert werden. Beispiel: `job definition sample`|
     |„Main definition file“ (Hauptdefinitionsdatei)| Die für den Auftrag verwendete Hauptdatei. Wählen Sie eine JAR-Datei aus Ihrem Speicher aus. Sie können **Datei hochladen** auswählen, um die Datei in ein Speicherkonto hochzuladen. Beispiel: `abfss://sparkjob@storageaccountname.dfs.core.windows.net/scala/wordcount/wordcount.jar`|
     |„Main class name“ (Name der Hauptklasse)| Der vollqualifizierte Bezeichner oder die Hauptklasse in der Hauptdefinitionsdatei. Beispiel: `WordCount`|
     |Befehlszeilenargumente| Optionale Argumente für den Auftrag. Beispiel: `abfss://sparkjob@storageaccountname.dfs.core.windows.net/scala/wordcount/shakespeare.txt abfss://sparkjob@storageaccountname.dfs.core.windows.net/scala/wordcount/result`|
     |„Reference files“ (Referenzdateien)| Zusätzliche Dateien, die zu Referenzzwecken in der Hauptdefinitionsdatei verwendet werden. Sie können **Datei hochladen** auswählen, um die Datei in ein Speicherkonto hochzuladen.|
     |Spark-Pool| Der Auftrag wird an den ausgewählten Apache Spark-Pool übermittelt.|
     |Spark-Version| Die vom Apache Spark-Pool ausgeführte Version von Apache Spark.|
     |Ausführer| Die gewünschte Anzahl von Executors im angegebenen Apache Spark-Pool für den Auftrag.|  
     |„Executor size“ (Executorgröße)| Die Anzahl von Kernen und die Menge an Arbeitsspeicher, die für Executors im angegebenen Apache Spark-Pool für den Auftrag verwendet werden sollen.|
     |„Driver size“ (Treibergröße)| Die Anzahl von Kernen und die Menge an Arbeitsspeicher, die für Treiber im angegebenen Apache Spark-Pool für den Auftrag verwendet werden sollen.|

     ![Festlegen des Werts der Spark-Auftragsdefinition für Scala](./media/apache-spark-job-definitions/create-scala-definition.png)

 6. Wählen Sie **Veröffentlichen** aus, um die Apache Spark-Auftragsdefinition zu speichern.

     ![Scala-Definition veröffentlichen](./media/apache-spark-job-definitions/publish-scala-definition.png)


## <a name="create-an-apache-spark-job-definition-for-net-sparkc"></a>Erstellen einer Apache Spark-Auftragsdefinition für .NET Spark (C#)

In diesem Abschnitt erstellen Sie eine Apache Spark-Auftragsdefinition für .Net Spark (C#).
 1. Öffnen Sie [Azure Synapse Studio](https://web.azuresynapse.net/).

 2. Sie können zu [Beispieldateien zum Erstellen von Apache Spark-Auftragsdefinitionen](https://github.com/Azure-Samples/Synapse/tree/master/Spark/DotNET) wechseln, um **wordcount.zip** und **shakespear.txt** herunterzuladen. Laden Sie dann diese Dateien in Azure Storage hoch: Klicken Sie auf **Daten**, wählen Sie **Speicherkonten** aus, und laden Sie die relevanten Dateien in Ihr ADLS Gen2-Dateisystem hoch. Überspringen Sie diesen Schritt, wenn sich Ihre Dateien bereits in Azure Storage befinden. 

     ![DotNet-Struktur vorbereiten](./media/apache-spark-job-definitions/prepare-scala-structure.png)

 3. Klicken Sie auf den Hub **Entwickeln**, wählen Sie im linken Bereich **Spark-Auftragsdefinitionen** aus, klicken Sie auf den Aktionsknoten „...“ neben der **Spark-Auftragsdefinition**, und wählen Sie dann im Kontextmenü **Neue Spark-Auftragsdefinition** aus.

     ![Neue Definition für DotNet erstellen](./media/apache-spark-job-definitions/create-new-definition.png)

 4. Wählen Sie in der Dropdownliste „Sprache“ im Hauptfenster der Apache Spark-Auftragsdefinition **.NET Spark (C#/F#)** aus.

 5. Tragen Sie Informationen für die Apache Spark-Auftragsdefinition ein. Sie können die Beispielinformationen kopieren.
     |  Eigenschaft   | BESCHREIBUNG   |  
     | ----- | ----- |  
     |Auftragsdefinitionsname| Geben Sie einen Namen für Ihre Apache Spark-Auftragsdefinition ein. Dieser Name kann bis zur Veröffentlichung jederzeit aktualisiert werden. Beispiel: `job definition sample`|
     |„Main definition file“ (Hauptdefinitionsdatei)| Die für den Auftrag verwendete Hauptdatei. Wählen Sie in Ihrem Speicher eine ZIP-Datei aus, die Ihre Anwendung vom Typ „.NET für Apache Spark“ enthält (ausführbare Hauptdatei, DLLs mit benutzerdefinierten Funktionen und andere erforderliche Dateien). Sie können **Datei hochladen** auswählen, um die Datei in ein Speicherkonto hochzuladen. Beispiel: `abfss://sparkjob@storageaccountname.dfs.core.windows.net/dotnet/wordcount/wordcount.zip`|
     |„Main executable file“ (Ausführbare Hauptdatei)| Die ausführbare Hauptdatei in der ZIP-Datei der Hauptdefinition. Beispiel: `WordCount`|
     |Befehlszeilenargumente| Optionale Argumente für den Auftrag. Beispiel: `abfss://sparkjob@storageaccountname.dfs.core.windows.net/dotnet/wordcount/shakespeare.txt abfss://sparkjob@storageaccountname.dfs.core.windows.net/dotnet/wordcount/result`|
     |„Reference files“ (Referenzdateien)| Zusätzliche Dateien, die von den Workerknoten zum Ausführen der Anwendung vom Typ „.NET für Apache Spark“ benötigt werden und nicht in der ZIP-Datei der Hauptdefinition enthalten sind (also abhängige JAR-Dateien, zusätzliche DLLs mit benutzerdefinierten Funktionen sowie andere Konfigurationsdateien). Sie können **Datei hochladen** auswählen, um die Datei in ein Speicherkonto hochzuladen.|
     |Spark-Pool| Der Auftrag wird an den ausgewählten Apache Spark-Pool übermittelt.|
     |Spark-Version| Die vom Apache Spark-Pool ausgeführte Version von Apache Spark.|
     |Ausführer| Die gewünschte Anzahl von Executors im angegebenen Apache Spark-Pool für den Auftrag.|  
     |„Executor size“ (Executorgröße)| Die Anzahl von Kernen und die Menge an Arbeitsspeicher, die für Executors im angegebenen Apache Spark-Pool für den Auftrag verwendet werden sollen.|
     |„Driver size“ (Treibergröße)| Die Anzahl von Kernen und die Menge an Arbeitsspeicher, die für Treiber im angegebenen Apache Spark-Pool für den Auftrag verwendet werden sollen.|

     ![Festlegen des Werts der Spark-Auftragsdefinition für DotNet](./media/apache-spark-job-definitions/create-net-definition.png)

 6. Wählen Sie **Veröffentlichen** aus, um die Apache Spark-Auftragsdefinition zu speichern.

      ![DotNet-Definition veröffentlichen](./media/apache-spark-job-definitions/publish-net-definition.png)

## <a name="submit-an-apache-spark-job-definition-as-a-batch-job"></a>Übermitteln einer Apache Spark-Auftragsdefinition als Batchauftrag

Nachdem Sie eine Apache Spark-Auftragsdefinition erstellt haben, können Sie sie an einen Apache Spark-Pool übermitteln. Stellen Sie sicher, dass Sie der Besitzer der Speicherblobdaten des ADLS Gen2-Dateisystems sind, das Sie verwenden möchten. Andernfalls müssen Sie die Berechtigung manuell hinzufügen.

### <a name="scenario-1-submit-apache-spark-job-definition"></a>Szenario 1: Übermitteln einer Apache Spark-Auftragsdefinition
 1. Öffnen Sie ein Apache Spark-Auftragsdefinitionsfenster, indem Sie darauf klicken.

      ![Öffnen der zu übermittelnden Spark-Auftragsdefinition ](./media/apache-spark-job-definitions/open-spark-definition.png)

 2. Klicken Sie auf **Übermitteln**, um Ihr Projekt an den ausgewählten Apache Spark-Pool zu übermitteln. Sie können auf die Registerkarte **Spark monitoring URL** (Spark-Überwachungs-URL) klicken, um die Protokollabfrage der Apache Spark-Anwendung anzuzeigen.

    ![Klicken Sie auf die Schaltfläche „Übermitteln“, um die Spark-Auftragsdefinition zu übermitteln.](./media/apache-spark-job-definitions/submit-spark-definition.png)

    ![Dialogfeld für die Spark-Übermittlung](./media/apache-spark-job-definitions/submit-definition-result.png)

### <a name="scenario-2-view-apache-spark-job-running-progress"></a>Szenario 2: Anzeigen des Status der Apache Spark-Auftragsausführung

 1. Klicken Sie auf **Überwachen**, und wählen Sie dann die Option **Spark-Anwendungen** aus. Navigieren Sie zur übermittelten Apache Spark-Anwendung.

     ![Anzeigen der Spark-Anwendung](./media/apache-spark-job-definitions/view-spark-application.png)

 2. Klicken Sie dann auf die Apache Spark-Anwendung. Daraufhin wird das Fenster **Log Query** (Protokollabfrage) angezeigt. Sie können den Status der Auftragsausführung anhand der **Protokollabfrage** ermitteln.
     
     ![Anzeigen der Protokollabfrage für die Spark-Anwendung](./media/apache-spark-job-definitions/view-job-log-query.png)

### <a name="scenario-3-check-output-file"></a>Szenario 3: Überprüfen der Ausgabedatei

 1. Klicken Sie auf **Daten**, und wählen Sie **Speicherkonten** aus. Nach erfolgreicher Ausführung können Sie zum ADLS Gen2-Speicher navigieren und sich vergewissern, dass Ausgaben generiert wurden.

     ![Anzeigen der Ausgabedatei](./media/apache-spark-job-definitions/view-output-file.png)

## <a name="add-an-apache-spark-job-definition-into-pipeline"></a>Hinzufügen einer Apache Spark-Auftragsdefinition in einer Pipeline

In diesem Abschnitt fügen Sie eine Apache Spark-Auftragsdefinition in einer Pipeline hinzu.

 1. Öffnen Sie eine vorhandene Apache Spark-Auftragsdefinition.

 2. Klicken Sie oben rechts in der Apache Spark-Auftragsdefinition auf das Symbol, wählen Sie **Vorhandene Pipeline** oder **Neue Pipeline** aus. Weitere Informationen finden Sie auf der Seite „Pipeline“.

     ![Zu Pipeline hinzufügen](./media/apache-spark-job-definitions/add-to-pipeline01.png)

     ![Zu Pipeline hinzufügen](./media/apache-spark-job-definitions/add-to-pipeline02.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial erfahren Sie, wie Sie Apache Spark-Auftragsdefinitionen mithilfe von Azure Synapse Studio erstellen und anschließend an einen Apache Spark-Pool übermitteln. Als Nächstes können Sie Azure Synapse Studio verwenden, um Power BI-Datasets zu erstellen und Power BI-Daten zu verwalten.

