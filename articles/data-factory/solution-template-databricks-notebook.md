---
title: Transformation mit Azure Databricks
description: Sie erfahren, wie in Azure Data Factory eine Lösungsvorlage zum Transformieren von Daten mithilfe eines Databricks-Notebooks verwendet wird.
services: data-factory
ms.author: abnarain
author: nabhishek
ms.reviewer: douglasl
manager: anandsub
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 03/03/2020
ms.openlocfilehash: e771bc152ab50f907a8f2ad384e887c00d3f627a
ms.sourcegitcommit: e6bce4b30486cb19a6b415e8b8442dd688ad4f92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2020
ms.locfileid: "78933950"
---
# <a name="transformation-with-azure-databricks"></a>Transformation mit Azure Databricks

In diesem Tutorial erstellen Sie in Data Factory eine End-to-End-Pipeline, die die Aktivitäten **Validierung**, **Kopieren** und **Notebook** enthält.

-   Die **Validierungsaktivität** wird verwendet, um sicherzustellen, dass das Quelldataset für die nachgeschaltete Nutzung bereit ist, bevor der Kopier- und Analyseauftrag ausgelöst wird.

-   Mit der **Kopieraktivität** wird die Quelldatei/das Quelldataset in den Senkenspeicher kopiert. Der Senkenspeicher wird als DBFS im Databricks-Notebook bereitgestellt, damit das Dataset direkt von Spark genutzt werden kann.

-   Die **Databricks-Notebook-Aktivität** löst das Databricks-Notebook für die Transformation des Datasets aus und fügt es einem verarbeiteten Ordner/SQL-DW hinzu.

Um diese Vorlage einfach zu halten, wird damit kein geplanter Trigger erstellt. Sie können diese Funktion bei Bedarf hinzufügen.

![1](media/solution-template-Databricks-notebook/pipeline-example.png)

## <a name="prerequisites"></a>Voraussetzungen

1. Erstellen Sie ein **Blobspeicherkonto** und einen Container namens `sinkdata`, der als **Senke** verwendet wird. Notieren Sie den **Namen des Speicherkontos**, den **Containernamen** und den **Zugriffsschlüssel**, da später in der Vorlage darauf verwiesen wird.

2. Vergewissern Sie sich, dass Sie einen **Azure Databricks-Arbeitsbereich** haben, oder erstellen Sie einen neuen.

3. **Importieren Sie das Notebook für die Transformation**. 
    1. Die folgenden Screenshots dienen als Referenz, wie Sie in Ihrer Azure Databricks-Instanz ein **Transformations**-Notebook in den Databricks-Arbeitsbereich importieren. Es muss sich nicht am gleichen Speicherort wie unten befinden, aber merken Sie sich den ausgewählten Pfad zur späteren Verwendung.
   
       ![2](media/solution-template-Databricks-notebook/import-notebook.png)    
    
    1. Wählen Sie „Importieren aus: **URL**“ aus, und geben Sie im Textfeld die folgende URL ein:
    
       * `https://adflabstaging1.blob.core.windows.net/share/Transformations.html`
        
       ![3](media/solution-template-Databricks-notebook/import-from-url.png)    

4. Jetzt aktualisieren wir das **Transformations**-Notebook mit den Speicherverbindungsinformationen. Wechseln Sie im oben importierten Notebook zu **Befehl 5** (wie im folgenden Codeausschnitt gezeigt), und ersetzen Sie `<storage name>` und `<access key>` durch Ihre eigenen Speicherverbindungsinformationen. Stellen Sie sicher, dass es sich bei diesem Konto um das zuvor erstellte Speicherkonto handelt und dass es den Container `sinkdata` enthält.

    ```python
    # Supply storageName and accessKey values  
    storageName = "<storage name>"  
    accessKey = "<access key>"  

    try:  
      dbutils.fs.mount(  
        source = "wasbs://sinkdata\@"+storageName+".blob.core.windows.net/",  
        mount_point = "/mnt/Data Factorydata",  
        extra_configs = {"fs.azure.account.key."+storageName+".blob.core.windows.net": accessKey})  

    except Exception as e:  
      # The error message has a long stack track. This code tries to print just the relevant line indicating what failed.

    import re
    result = re.findall(r"\^\s\*Caused by:\s*\S+:\s\*(.*)\$", e.message, flags=re.MULTILINE)
    if result:
      print result[-1] \# Print only the relevant error message
    else:  
      print e \# Otherwise print the whole stack trace.  
    ```

5.  Generieren Sie ein **Databricks Zugriffstoken**, damit Data Factory auf Databricks zugreifen kann. **Speichern Sie das Zugriffstoken** für die spätere Verwendung beim Erstellen eines mit Databricks verknüpften Diensts. Das sieht in etwa wie folgt aus: „dapi32db32cbb4w6eee18b7d87e45exxxxxx“.

    ![4](media/solution-template-Databricks-notebook/user-setting.png)

    ![5](media/solution-template-Databricks-notebook/generate-new-token.png)

## <a name="how-to-use-this-template"></a>So verwenden Sie diese Vorlage

1.  Wechseln Sie zur Vorlage **Transformation mit Azure Databricks**. Erstellen Sie neue verknüpfte Dienste für die folgenden Verbindungen. 
    
    ![Einstellung für Verbindungen](media/solution-template-Databricks-notebook/connections-preview.png)

    1.  **Quellblobverbindung**: Für den Zugriff auf Quelldaten. 
        
        Für dieses Beispiel können Sie den öffentlichen Blobspeicher mit den Quelldateien verwenden. Der folgende Screenshot dient als Referenz für die Konfiguration. Verwenden Sie die folgende **SAS-URL** zum Herstellen einer Verbindung mit dem Quellspeicher (schreibgeschützter Zugriff): 
        * `https://storagewithdata.blob.core.windows.net/data?sv=2018-03-28&si=read%20and%20list&sr=c&sig=PuyyS6%2FKdB2JxcZN0kPlmHSBlD8uIKyzhBWmWzznkBw%3D`

        ![6](media/solution-template-Databricks-notebook/source-blob-connection.png)

    1.  **Zielblobverbindung**: Für das Kopieren von Daten in das Ziel. 
        
        Wählen Sie im verknüpften Senkendienst ein Speicherkonto aus, das in **Voraussetzung** 1 erstellt wurde.

        ![7](media/solution-template-Databricks-notebook/destination-blob-connection.png)

    1.  **Azure Databricks**: Für das Herstellen einer Verbindung mit dem Databricks-Cluster.

        Erstellen Sie mithilfe des in **Voraussetzung** 2.c generierten Zugriffsschlüssels einen verknüpften Databricks-Dienst. Wenn Sie einen *interaktiven Cluster* haben, können Sie diesen auswählen. (In diesem Beispiel wird die Option *Neuer Auftragscluster* verwendet.)

        ![8](media/solution-template-Databricks-notebook/databricks-connection.png)

1. Wählen Sie **Diese Vorlage verwenden** aus. Dann wird eine Pipeline (wie unten dargestellt) erstellt:
    
    ![Erstellen einer Pipeline](media/solution-template-Databricks-notebook/new-pipeline.png)   

## <a name="pipeline-introduction-and-configuration"></a>Einführung und Konfiguration der Pipeline

In der neu erstellten Pipeline sind die meisten Einstellungen automatisch mit den jeweiligen Standardwerten konfiguriert. Sehen Sie sich die Konfigurationen an, und nehmen Sie ggf. Aktualisierungen gemäß Ihren eigenen Einstellungen vor. Sehen Sie sich die nachstehenden Anweisungen und Screenshots als Referenz an, um weitere Informationen zu erhalten.

1.  Die Validierungsaktivität **Availability flag** wird zum Durchführen einer Überprüfung der Quellverfügbarkeit erstellt. Als Dataset wird das im vorherigen Schritt erstellte *SourceAvailabilityDataset* ausgewählt.

    ![12](media/solution-template-Databricks-notebook/validation-settings.png)

1.  Die Kopieraktivität **file-to-blob** wird erstellt, um das Dataset aus der Quelle in die Senke zu kopieren. Auf den folgenden Screenshots sehen Sie Quell- und Senkenkonfigurationen in der Kopieraktivität.

    ![13](media/solution-template-Databricks-notebook/copy-source-settings.png)

    ![14](media/solution-template-Databricks-notebook/copy-sink-settings.png)

1.  Die Notebook-Aktivität **Transformation** wird erstellt, und der im vorherigen Schritt erstellte verknüpfte Dienst wird ausgewählt.
    ![16](media/solution-template-Databricks-notebook/notebook-activity.png)

     1. Wählen Sie die Registerkarte **Einstellungen** aus. Für den *Notebook-Pfad* wird von der Vorlage standardmäßig ein Pfad definiert. Möglicherweise müssen Sie auf „Durchsuchen“ klicken und den in **Voraussetzung** 2 hochgeladenen richtigen Notebook-Pfad auswählen. 

         ![17](media/solution-template-Databricks-notebook/notebook-settings.png)
    
     1. Erweitern Sie die erstellten *Basisparameter*, wie im Screenshot gezeigt. Sie müssen von Data Factory an das Databricks-Notebook übergeben werden. 

         ![Basisparameter](media/solution-template-Databricks-notebook/base-parameters.png)

1.  Die **Pipelineparameter** werden wie unten dargestellt definiert.

    ![15](media/solution-template-Databricks-notebook/pipeline-parameters.png)

1. Die Datasets werden eingerichtet.
    1.  Das **SourceAvailabilityDataset** wird erstellt, um zu überprüfen, ob Quelldaten verfügbar sind.

        ![9](media/solution-template-Databricks-notebook/source-availability-dataset.png)

    1.  **SourceFilesDataset**: Zum Kopieren der Quelldaten.

        ![10](media/solution-template-Databricks-notebook/source-file-dataset.png)

    1.  **DestinationFilesDataset**: Zum Kopieren in die Senke/an den Zielspeicherort.

        1.  Verknüpfter Dienst: Der im vorherigen Schritt erstellte Dienst *sinkBlob_LS*.

        2.  Dateipfad: *sinkdata/staged_sink*.

            ![11](media/solution-template-Databricks-notebook/destination-dataset.png)


1.  Wählen Sie **Debuggen** aus, um die Pipeline auszuführen. Sie finden einen Link zu Databricks-Protokollen, um ausführlichere Spark-Protokolle zu erhalten.

    ![18](media/solution-template-Databricks-notebook/pipeline-run-output.png)

    Sie können die Datendatei auch mit dem Speicher-Explorer überprüfen. (Für die Korrelation mit Data Factory-Pipelineausführungen, wird in diesem Beispiel die Pipelineausführungs-ID aus Data Factory dem Ausgabeordner angefügt. So können Sie die bei den einzelnen Ausführungen generierten Dateien nachverfolgen.)

    ![19](media/solution-template-Databricks-notebook/verify-data-files.png)

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in den Azure Data Factory-Dienst](introduction.md)
