---
title: 'Tutorial: Extrahieren von Formulardaten per Massenvorgang mithilfe der Formularerkennung von Azure Data Factory'
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie Azure Data Factory-Aktivitäten einrichten, um das Trainieren und Ausführen von Formularerkennungsmodellen auszulösen und einen umfangreichen Backlog von Dokumenten zu digitalisieren.
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: tutorial
ms.date: 01/04/2021
ms.author: lajanuar
ms.openlocfilehash: 0c009a87a5834997cdc489efc75ebb16f9459754
ms.sourcegitcommit: 3ea12ce4f6c142c5a1a2f04d6e329e3456d2bda5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2021
ms.locfileid: "103467101"
---
# <a name="tutorial-extract-form-data-in-bulk-by-using-azure-data-factory"></a>Tutorial: Extrahieren von Formulardaten per Massenvorgang mithilfe von Azure Data Factory

In diesem Tutorial erfahren Sie, wie Sie mithilfe von Azure-Diensten eine große Anzahl von Formularen erfassen und in digitale Medien umwandeln. Das Tutorial zeigt, wie Sie die Erfassung von Daten aus einer Azure Data Lake Storage-Instanz mit Dokumenten in einer Azure SQL-Datenbank-Instanz automatisieren. Mit nur wenigen Klicks können Sie Modelle trainieren und neue Dokumente verarbeiten.

## <a name="business-need"></a>Geschäftliche Anforderung

Die meisten Organisationen sind sich inzwischen bewusst, wie wertvoll die Daten sind, die ihnen in verschiedenen Formaten (PDF, Bilder, Videos) zur Verfügung stehen. Sie suchen nach den besten und kostengünstigsten Methoden, um diese Ressourcen zu digitalisieren.

Außerdem erhalten unsere Kunden häufig verschiedene Arten von Formularen von ihren zahlreichen Clients und Kunden. Im Gegensatz zu den [Schnellstartanleitungen](./quickstarts/client-library.md) erfahren Sie in diesem Tutorial, wie Sie ein Modell unter Verwendung eines metadatenbasierten Ansatzes automatisch mit neuen und verschiedenen Arten von Formularen trainieren. Falls für einen Formulartyp noch kein Modell vorhanden ist, wird vom System eines erstellt, und Sie erhalten die Modell-ID. 

Durch die Extraktion von Formulardaten und die Kombinierung dieser Daten mit vorhandenen betrieblichen Systemen und Data Warehouses können Unternehmen Erkenntnisse gewinnen und einen Mehrwert für ihre Kunden und Geschäftsbenutzer generieren.

Mit der Azure-Formularerkennung können Organisationen ihre Daten nutzbar machen, Prozesse wie Rechnungszahlungen und die steuerliche Bearbeitung automatisieren, Zeit und Geld sparen und eine höhere Datengenauigkeit erzielen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Einrichten von Azure Data Lake Storage zum Speichern Ihrer Formulare
> * Erstellen einer Parametrisierungstabelle mithilfe einer Azure-Datenbank
> * Speichern vertraulicher Anmeldeinformationen mithilfe von Azure Key Vault
> * Trainieren Ihres Formularerkennungsmodells in einem Azure Databricks-Notebook
> * Extrahieren Ihrer Formulardaten mithilfe eines Databricks-Notebooks
> * Automatisieren von Formulartraining und -extraktion mithilfe von Azure Data Factory

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. [Erstellen Sie ein kostenloses Konto.](https://azure.microsoft.com/free/cognitive-services/)
* Wenn Sie über Ihr Azure-Abonnement verfügen, können Sie im Azure-Portal <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer"  title="Erstellen einer Formularerkennungsressource"  target="_blank">eine Formularerkennungsressource erstellen</a>, um Ihren Schlüssel und Endpunkt zu erhalten. Nachdem die Ressource bereitgestellt wurde, klicken Sie auf **Zu Ressource wechseln**.
    * Sie benötigen den Schlüssel und Endpunkt der von Ihnen erstellten Ressource, um Ihre Anwendung mit der Formularerkennungs-API zu verbinden. Der Schlüssel und der Endpunkt werden weiter unten in dieser Schnellstartanleitung in den Code eingefügt.
    * Zum Testen des Diensts können Sie den kostenlosen Tarif (F0) verwenden. Für die Produktion können Sie dann später ein Upgrade auf einen kostenpflichtigen Tarif durchführen.
* Einen Satz von mindestens fünf Formularen des gleichen Typs. Im Idealfall sollte dieser Workflow große Mengen von Dokumenten unterstützen. Tipps und Optionen für die Zusammenstellung eines Trainingsdatasets finden Sie unter [Erstellen eines Trainingsdatasets für ein benutzerdefiniertes Modell](./build-training-data-set.md). In diesem Tutorial können Sie die Dateien im Ordner „Train“ des [Beispieldatasets](https://go.microsoft.com/fwlink/?linkid=2128080) verwenden.


## <a name="project-architecture"></a>Projektarchitektur 

Dieses Projekt stellt eine Reihe von Azure Data Factory-Pipelines bereit, um Python-Notebooks auszulösen, die Trainings-, Analyse- und Extraktionsschritte für Dokumentdaten in einem Azure Data Lake-Speicherkonto durchführen.

Die REST-API für die Formularerkennung benötigt einige Parameter als Eingabe. Aus Sicherheitsgründen werden einige dieser Parameter in einem Azure-Schlüsseltresor gespeichert. Weniger sensible Parameter wie etwa der Name des Speicherblob-Ordners werden dagegen in einer Parametrisierungstabelle in einer Azure SQL-Datenbank-Instanz gespeichert.

Für jeden zu analysierenden Formulartyp wird von Data Engineers oder Data Scientists eine Zeile der Parametertabelle aufgefüllt. Anschließend verwenden sie Azure Data Factory, um die Liste der erkannten Formulartypen zu durchlaufen und die relevanten Parameter an ein Databricks-Notebook zu übergeben, damit die Formularerkennungsmodelle trainiert (oder neu trainiert) werden können. Hier kann auch eine Azure-Funktion verwendet werden.

Das Azure Databricks-Notebook verwendet nun die trainierten Modelle, um Formulardaten zu extrahieren. Diese Daten werden dann in eine Azure SQL-Datenbank-Instanz exportiert.

:::image type="content" source="./media/tutorial-bulk-processing/architecture.png" alt-text="Diagramm: Architektur des Projekts":::


## <a name="set-up-azure-data-lake"></a>Einrichten von Azure Data Lake

Ihr Backlog an Formularen kann sich beispielsweise in Ihrer lokalen Umgebung oder auf einem (s)FTP-Server befinden. In diesem Tutorial werden Formulare in einem Azure Data Lake Storage Gen2-Speicherkonto verwendet. Dorthin können Sie Ihre Dateien mithilfe von Azure Data Factory, Azure Storage-Explorer oder AzCopy übertragen. Die Trainings- und Bewertungsdatasets können sich in unterschiedlichen Containern befinden. Die Trainingsdatasets für alle Formulartypen müssen jedoch im gleichen Container enthalten sein. (Sie können sich allerdings in verschiedenen Ordnern befinden.)

Gehen Sie zum Erstellen eines neuen Data Lake wie unter [Erstellen eines Speicherkontos für die Verwendung mit Azure Data Lake Storage Gen2](../../storage/blobs/create-data-lake-storage-account.md) beschrieben vor.

## <a name="create-a-parameterization-table"></a>Erstellen einer Parametrisierungstabelle

Als Nächstes erstellen wir eine Metadatentabelle in einer Azure SQL-Datenbank-Instanz. Diese Tabelle enthält die nicht vertraulichen Daten, die von der REST-API für die Formularerkennung benötigt werden. Sobald im Dataset ein neuer Formulartyp auftaucht, fügen wir in diese Tabelle einen neuen Datensatz ein und lösen die Trainings- und Bewertungspipelines aus. (Diese Pipelines werden später noch implementiert.)

In der Tabelle werden folgende Felder verwendet:

* **form_description**: Im Rahmen des Trainings nicht erforderlich. Dieses Feld enthält eine Beschreibung des Formulartyps, für den das Modell trainiert wird (also beispielsweise „Formulare für Client A“ oder „Formulare für Hotel B“).
* **training_container_name**: Der Name des Speicherkontocontainers, in dem das Trainingsdataset gespeichert ist. Dabei kann es sich um den gleichen Container handeln wie in **scoring_container_name**.
* **training_blob_root_folder**: Der Ordner im Speicherkonto, in dem die Dateien für das Modelltraining gespeichert werden.
* **scoring_container_name**: Der Name des Speicherkontocontainers, in dem die Dateien gespeichert sind, aus denen die Schlüssel-Wert-Paare extrahiert werden sollen. Dabei kann es sich um den gleichen Container handeln wie in **training_container_name**.
* **scoring_input_blob_folder**: Der Ordner in dem Speicherkonto, in dem die Dateien für die Datenextraktion gespeichert werden.
* **model_id**: Die ID des Modells, das neu trainiert werden soll. Bei der ersten Ausführung muss der Wert auf „-1“ festgelegt sein, damit vom Trainingsnotebook ein neues benutzerdefiniertes Modell zum Trainieren erstellt wird. Vom Trainingsnotebook wird die neu erstellte Modell-ID an die Azure Data Factory-Instanz zurückgegeben. Dieser Wert wird von uns mithilfe einer Aktivität einer gespeicherten Prozedur in der Azure SQL-Datenbank-Instanz aktualisiert.

  Wenn Sie einen neuen Formulartyp erfassen möchten, müssen Sie die Modell-ID manuell auf „-1“ zurücksetzen, bevor Sie das Modell trainieren.

* **file_type**: Folgende Formulartypen werden unterstützt: `application/pdf`, `image/jpeg`, `image/png` und `image/tif`.

  Wenn Sie über Formulare mit anderen Dateitypen verfügen, müssen Sie beim Trainieren eines neuen Formulartyps diesen Wert sowie **model_id** ändern.
* **form_batch_group_id**: Im Laufe der Zeit verfügen Sie möglicherweise über mehrere Formulartypen, die Sie mit dem gleichen Modell trainieren. Das Feld **form_batch_group_id** ermöglicht die Angabe aller Formulartypen, die mit einem bestimmten Modell trainiert wurden.

### <a name="create-the-table"></a>Erstellen der Tabelle


[Erstellen Sie eine Azure SQL-Datenbank-Instanz](https://ms.portal.azure.com/#create/Microsoft.SQLDatabase), und führen Sie anschließend im [Abfrage-Editor](../../azure-sql/database/connect-query-portal.md) das folgende SQL-Skript aus, um die erforderliche Tabelle zu erstellen:


```sql
CREATE TABLE dbo.ParamFormRecogniser(
    form_description varchar(50) NULL,
  training_container_name varchar(50) NOT NULL,
    training_blob_root_folder varchar(50) NULL,
    scoring_container_name varchar(50) NOT NULL,
    scoring_input_blob_folder varchar(50) NOT NULL,
    scoring_output_blob_folder varchar(50) NOT NULL,
    model_id varchar(50) NULL,
    file_type varchar(50) NULL
) ON PRIMARY
GO
```

Führen Sie das folgende Skript aus, um die Prozedur zu erstellen, durch die **model_id** nach dem Trainieren automatisch aktualisiert wird:

```SQL
CREATE PROCEDURE [dbo].[update_model_id] ( @form_batch_group_id  varchar(50),@model_id varchar(50))
AS
BEGIN 
    UPDATE [dbo].[ParamFormRecogniser]   
        SET [model_id] = @model_id  
    WHERE form_batch_group_id =@form_batch_group_id
END
```

## <a name="use-azure-key-vault-to-store-sensitive-credentials"></a>Speichern vertraulicher Anmeldeinformationen mithilfe von Azure Key Vault

Aus Sicherheitsgründen sollen bestimmte vertrauliche Informationen nicht in der Parametrisierungstabelle in der Azure SQL-Datenbank-Instanz gespeichert werden. Daher speichern wir vertrauliche Parameter als Azure Key Vault-Geheimnisse.

### <a name="create-an-azure-key-vault"></a>Erstellen einer Azure Key Vault-Instanz

[Erstellen Sie eine Key Vault-Ressource.](https://ms.portal.azure.com/#create/Microsoft.KeyVault) Navigieren Sie anschließend zu der erstellten Key Vault-Ressource, und wählen Sie im Bereich **Einstellungen** die Option **Geheimnisse** aus, um die Parameter hinzuzufügen.

Ein neues Fenster wird angezeigt. Wählen Sie die Option **Generieren/importieren** aus. Geben Sie Name und Wert des Parameters ein, und wählen Sie anschließend **Erstellen** aus. Führen Sie diese Schritte für die folgenden Parameter aus:

* **CognitiveServiceEndpoint**: Die Endpunkt-URL Ihrer API für die Formularerkennung.
* **CognitiveServiceSubscriptionKey**: Der Zugriffsschlüssel für Ihren Formularerkennungsdienst. 
* **StorageAccountName**: Das Speicherkonto, in dem das Trainingsdataset und die Formulare gespeichert sind, aus denen die Schlüssel-Wert-Paare extrahiert werden sollen. Wenn sich diese Elemente in unterschiedlichen Konten befinden, geben Sie jeden Kontonamen als separates Geheimnis ein. Zur Erinnerung: Die Trainingsdatasets müssen für alle Formulartypen im gleichen Container enthalten sein. Sie können sich aber in unterschiedlichen Ordnern befinden.
* **StorageAccountSasKey**: Die Shared Access Signature (SAS) des Speicherkontos. Navigieren Sie zum Abrufen der SAS-URL zu Ihrer Speicherressource. Navigieren Sie auf der Registerkarte **Storage-Explorer** zu Ihrem Container, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Shared Access Signature abrufen** aus. Achten Sie darauf, die SAS für den Container abzurufen, nicht für das Speicherkonto. Stellen Sie sicher, dass die Berechtigungen **Lesen** und **Auflisten**  ausgewählt sind, und wählen Sie anschließend **Erstellen** aus. Kopieren Sie den Wert im **URL**-Abschnitt. Das Format sollte wie folgt aussehen: `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>`.

## <a name="train-your-form-recognizer-model-in-a-databricks-notebook"></a>Trainieren Ihres Formularerkennungsmodells in einem Databricks-Notebook

Zum Speichern und Ausführen des Python-Codes, der mit dem Formularerkennungsdienst interagiert, wird Azure Databricks verwendet.

### <a name="create-a-notebook-in-databricks"></a>Erstellen eines Notebooks in Databricks

[Erstellen Sie eine Azure Databricks-Ressource](https://ms.portal.azure.com/#create/Microsoft.Databricks) im Azure-Portal. Navigieren Sie zu der erstellten Ressource, und öffnen Sie den Arbeitsbereich.

### <a name="create-a-secret-scope-backed-by-azure-key-vault"></a>Erstellen eines von Azure Key Vault unterstützten Geheimnisbereichs


Um auf die Geheimnisse im weiter oben erstellten Azure-Schlüsseltresor verweisen zu können, muss in Databricks ein Geheimnisbereich erstellt werden. Gehen Sie dazu wie unter [Erstellen eines von Azure Key Vault unterstützten Geheimnisbereichs](/azure/databricks/security/secrets/secret-scopes#--create-an-azure-key-vault-backed-secret-scope) beschrieben vor.

### <a name="create-a-databricks-cluster"></a>Erstellen eines Databricks-Clusters

Bei einem Cluster handelt es sich um eine Sammlung von Databricks-Berechnungsressourcen. Erstellen Sie wie folgt einen Cluster:

1. Wählen Sie im linken Bereich die Schaltfläche **Cluster** aus.
1. Wählen Sie auf der Seite **Cluster** die Option **Cluster erstellen** aus.
1. Geben Sie auf der Seite **Cluster erstellen** einen Clusternamen an, und wählen Sie in der Liste für die **Databricks-Runtimeversion** die Option **7.2 (Scala 2.12, Spark 3.0.0)** aus.
1. Klicken Sie auf **Cluster erstellen**.

### <a name="write-a-settings-notebook"></a>Erstellen eines Einstellungsnotebooks

Nun können Sie Python-Notebooks hinzufügen. Erstellen Sie zunächst ein Notebook mit dem Namen **Settings**. Durch dieses Notebook werden die Werte aus Ihrer Parametrisierungstabelle den Variablen im Skript zugewiesen. Von Azure Data Factory werden die Werte später als Parameter übergeben. Außerdem weisen wir Variablen Werte aus den Geheimnissen im Schlüsseltresor zu. 

1. Wählen Sie zum Erstellen des Notebooks **Settings** die Schaltfläche **Arbeitsbereich** aus. Wählen Sie auf der neuen Registerkarte die Dropdownliste und anschließend **Erstellen** > **Notebook** aus.
1. Geben Sie im daraufhin angezeigten Popupfenster einen Namen für das Notebook ein, und wählen Sie **Python** als Standardsprache aus. Wählen Sie Ihren Databricks-Cluster und anschließend **Erstellen** aus.
1. In der ersten Notebookzelle werden die von Azure Data Factory übergebenen Parameter abgerufen:

    ```python 
    dbutils.widgets.text("form_batch_group_id", "","")
    dbutils.widgets.get("form_batch_group_id")
    form_batch_group_id = getArgument("form_batch_group_id")
    
    dbutils.widgets.text("model_id", "","")
    dbutils.widgets.get("model_id")
    model_id = getArgument("model_id")
    
    dbutils.widgets.text("training_container_name", "","")
    dbutils.widgets.get("training_container_name")
    training_container_name = getArgument("training_container_name")
    
    dbutils.widgets.text("training_blob_root_folder", "","")
    dbutils.widgets.get("training_blob_root_folder")
    training_blob_root_folder=  getArgument("training_blob_root_folder")
    
    dbutils.widgets.text("scoring_container_name", "","")
    dbutils.widgets.get("scoring_container_name")
    scoring_container_name=  getArgument("scoring_container_name")
    
    dbutils.widgets.text("scoring_input_blob_folder", "","")
    dbutils.widgets.get("scoring_input_blob_folder")
    scoring_input_blob_folder=  getArgument("scoring_input_blob_folder")
    
    
    dbutils.widgets.text("file_type", "","")
    dbutils.widgets.get("file_type")
    file_type = getArgument("file_type")
    
    dbutils.widgets.text("file_to_score_name", "","")
    dbutils.widgets.get("file_to_score_name")
    file_to_score_name=  getArgument("file_to_score_name")
    ```

1. In der zweiten Zelle werden Geheimnisse aus Key Vault abgerufen und Variablen zugewiesen:

    ```python 
    cognitive_service_subscription_key = dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "CognitiveserviceSubscriptionKey")
    cognitive_service_endpoint = dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "CognitiveServiceEndpoint")
    
    training_storage_account_name = dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "StorageAccountName")
    storage_account_sas_key= dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "StorageAccountSasKey")  
    
    ScoredFile = file_to_score_name+ "_output.json"
    training_storage_account_url="https://"+training_storage_account_name+".blob.core.windows.net/"+training_container_name+storage_account_sas_key 
    ```

### <a name="write-a-training-notebook"></a>Erstellen eines Trainingsnotebooks

Nach Abschluss des Notebooks **Settings** können wir ein Notebook zum Trainieren des Modells erstellen. Wie bereits erwähnt, verwenden wir hier Dateien, die in einem Ordner in einem Azure Data Lake Storage Gen2-Konto (**training_blob_root_folder**) gespeichert sind. Der Ordnername wurde als Variable übergeben. Jede Gruppe von Formulartypen wird im gleichen Ordner gespeichert. Beim Durchlaufen der Parametertabelle wird das Modell unter Verwendung aller Formulartypen trainiert. 

1. Erstellen Sie ein Notebook mit dem Namen **TrainFormRecognizer**. 
1. Führen Sie in der ersten Zelle das Notebook **Settings** aus:

    ```python
    %run "./Settings"
    ```

1. Weisen Sie in der nächsten Zelle Variablen aus der Datei Settings zu, und trainieren Sie das Modell dynamisch für jeden Formulartyp. Wenden Sie dazu den Code aus der [REST-Schnellstartanleitung](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/FormRecognizer/rest/python-train-extract.md#get-training-results%20) an.

    ```python
    import json
    import time
    from requests import get, post
    
    post_url = cognitive_service_endpoint + r"/formrecognizer/v2.0/custom/models"
    source = training_storage_account_url
    prefix= training_blob_root_folder
    
    includeSubFolders=True
    useLabelFile=False
    headers = {
        # Request headers.
        'Content-Type': file_type,
        'Ocp-Apim-Subscription-Key': cognitive_service_subscription_key,
    }
    body =     {
        "source": source
        ,"sourceFilter": {
            "prefix": prefix,
            "includeSubFolders": includeSubFolders
       },
    }
    if model_id=="-1": # If you don't already have a model you want to retrain. In this case, we create a model and use it to extract the key/value pairs.
      try:
          resp = post(url = post_url, json = body, headers = headers)
          if resp.status_code != 201:
              print("POST model failed (%s):\n%s" % (resp.status_code, json.dumps(resp.json())))
              quit()
          print("POST model succeeded:\n%s" % resp.headers)
          get_url = resp.headers["location"]
          model_id=get_url[get_url.index('models/')+len('models/'):]     
          
      except Exception as e:
          print("POST model failed:\n%s" % str(e))
          quit()
    else :# If you already have a model you want to retrain, we reuse it and (re)train with the new form types.  
      try:
        get_url =post_url+r"/"+model_id
          
      except Exception as e:
          print("POST model failed:\n%s" % str(e))
          quit()
    ```

1. Im letzten Schritt des Trainingsprozesses wird das Trainingsergebnis in einem JSON-Format abgerufen:

    ```python
    n_tries = 10
    n_try = 0
    wait_sec = 5
    max_wait_sec = 5
    while n_try < n_tries:
        try:
            resp = get(url = get_url, headers = headers)
            resp_json = resp.json()
            print (resp.status_code)
            if resp.status_code != 200:
                print("GET model failed (%s):\n%s" % (resp.status_code, json.dumps(resp_json)))
                n_try += 1
                quit()
            model_status = resp_json["modelInfo"]["status"]
            print (model_status)
            if model_status == "ready":
                print("Training succeeded:\n%s" % json.dumps(resp_json))
                n_try += 1
                quit()
            if model_status == "invalid":
                print("Training failed. Model is invalid:\n%s" % json.dumps(resp_json))
                n_try += 1
                quit()
            # Training still running. Wait and retry.
            time.sleep(wait_sec)
            n_try += 1
            wait_sec = min(2*wait_sec, max_wait_sec)     
            print (n_try)
        except Exception as e:
            msg = "GET model failed:\n%s" % str(e)
            print(msg)
            quit()
    print("Train operation did not complete within the allocated time.")
    ```

## <a name="extract-form-data-by-using-a-notebook"></a>Extrahieren von Formulardaten mithilfe eines Notebooks

### <a name="mount-the-azure-data-lake-storage"></a>Einbinden der Azure Data Lake Storage-Instanz

Der nächste Schritt besteht darin, die verschiedenen vorhandenen Formulare mithilfe des trainierten Modells zu bewerten. Dazu binden wir das Azure Data Lake Storage-Konto in Databricks ein und verweisen während des Erfassungsprozesses auf die Einbindung.

Genau wie in der Trainingsphase verwenden wir Azure Data Factory, um die Extraktion der Schlüssel-Wert-Paare aus den Formularen auszulösen. Wir durchlaufen die Formulare in den in der Parametertabelle angegebenen Ordnern.

1. Erstellen Sie das Notebook für die Einbindung des Speicherkontos in Databricks. Nennen Sie es **MountDataLake**. 
1. Zuerst muss das Notebook **Settings** aufgerufen werden:

    ```python
    %run "./Settings"
    ```

1. Definieren Sie in der zweiten Zelle Variablen für die vertraulichen Parameter, die wir aus unseren Key Vault-Geheimnissen abrufen:

    ```python
    cognitive_service_subscription_key = dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "CognitiveserviceSubscriptionKey")
    cognitive_service_endpoint = dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "CognitiveServiceEndpoint")
    
    scoring_storage_account_name = dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "StorageAccountName")
    scoring_storage_account_sas_key= dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "StorageAccountSasKey")  
    
    scoring_mount_point = "/mnt/"+scoring_container_name 
    scoring_source_str = "wasbs://{container}@{storage_acct}.blob.core.windows.net/".format(container=scoring_container_name, storage_acct=scoring_storage_account_name) 
    scoring_conf_key = "fs.azure.sas.{container}.{storage_acct}.blob.core.windows.net".format(container=scoring_container_name, storage_acct=scoring_storage_account_name)
    
    ```

1. Versuchen Sie, die Einbindung des Speicherkontos aufzuheben, falls es bereits eingebunden wurde:

    ```python
    try:
      dbutils.fs.unmount(scoring_mount_point) # Use this to unmount as needed
    except:
      print("{} already unmounted".format(scoring_mount_point))
    
    ```

1. Binden Sie das Speicherkonto ein:


    ```python
    try: 
      dbutils.fs.mount( 
        source = scoring_source_str, 
        mount_point = scoring_mount_point, 
        extra_configs = {scoring_conf_key: scoring_storage_account_sas_key} 
      ) 
    except Exception as e: 
      print("ERROR: {} already mounted. Run previous cells to unmount first".format(scoring_mount_point))
    
    ```

    > [!NOTE]
    > Wir haben nur das Trainingsspeicherkonto eingebunden. In diesem Fall befinden sich die Trainingsdateien und die Dateien, aus denen Schlüssel-Wert-Paare extrahiert werden sollen, im gleichen Speicherkonto. Wenn Sie zum Bewerten und Trainieren unterschiedliche Speicherkonten verwenden, müssen beide Speicherkonten eingebunden werden. 

### <a name="write-the-scoring-notebook"></a>Erstellen des Bewertungsnotebooks

Nun können wir ein Bewertungsnotebook erstellen. Dazu gehen wir ähnlich vor wie beim Trainingsnotebook: Wir verwenden Dateien, die in Ordnern in dem soeben eingebundenen Azure Data Lake Storage-Konto gespeichert sind. Der Ordnername wird als Variable übergeben. Wir durchlaufen alle Formulare im angegebenen Ordner und extrahieren die Schlüssel-Wert-Paare. 

1. Erstellen Sie ein Notebook, und nennen Sie es **ScoreFormRecognizer**. 
1. Führen Sie die Notebooks **Settings** und **MountDataLake** aus:

    ```python
    %run "./Settings"
    %run "./MountDataLake"
    ```

1. Fügen Sie den folgenden Code hinzu, um die [Analyse-API](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeWithCustomForm) aufzurufen:

    ```python
    ########### Python Form Recognizer Async Analyze #############
    import json
    import time
    from requests import get, post 
    
    
    #prefix= TrainingBlobFolder
    post_url = cognitive_service_endpoint + "/formrecognizer/v2.0/custom/models/%s/analyze" % model_id
    source = r"/dbfs/mnt/"+scoring_container_name+"/"+scoring_input_blob_folder+"/"+file_to_score_name
    output = r"/dbfs/mnt/"+scoring_container_name+"/scoringforms/ExtractionResult/"+os.path.splitext(os.path.basename(source))[0]+"_output.json"
    
    params = {
        "includeTextDetails": True
    }
    
    headers = {
        # Request headers.
        'Content-Type': file_type,
        'Ocp-Apim-Subscription-Key': cognitive_service_subscription_key,
    }
    
    with open(source, "rb") as f:
        data_bytes = f.read()
    
    try:
        resp = post(url = post_url, data = data_bytes, headers = headers, params = params)
        if resp.status_code != 202:
            print("POST analyze failed:\n%s" % json.dumps(resp.json()))
            quit()
        print("POST analyze succeeded:\n%s" % resp.headers)
        get_url = resp.headers["operation-location"]
    except Exception as e:
        print("POST analyze failed:\n%s" % str(e))
        quit() 
    ```

1. In der nächsten Zelle erhalten wir die Ergebnisse der Extraktion der Schlüssel-Wert-Paare. In dieser Zelle wird das Ergebnis ausgegeben. Wir benötigen das Ergebnis im JSON-Format, um es in Azure SQL-Datenbank oder in Azure Cosmos DB weiterverarbeiten zu können. Daher schreiben wir das Ergebnis in eine JSON-Datei. Zur Benennung der Ausgabedatei wird der Name der bewerteten Datei mit „_output.json“ verkettet. Die Datei wird in dem Ordner gespeichert, in dem sich auch die Quelldatei befindet.

    ```python
    n_tries = 10
    n_try = 0
    wait_sec = 5
    max_wait_sec = 5
    while n_try < n_tries:
       try:
           resp = get(url = get_url, headers = {"Ocp-Apim-Subscription-Key": cognitive_service_subscription_key})
           resp_json = resp.json()
           if resp.status_code != 200:
               print("GET analyze results failed:\n%s" % json.dumps(resp_json))
               n_try += 1
               quit()
           status = resp_json["status"]
           if status == "succeeded":
               print("Analysis succeeded:\n%s" % json.dumps(resp_json))
               n_try += 1
               quit()
           if status == "failed":
               print("Analysis failed:\n%s" % json.dumps(resp_json))
               n_try += 1
               quit()
           # Analysis still running. Wait and retry.
           time.sleep(wait_sec)
           n_try += 1
           wait_sec = min(2*wait_sec, max_wait_sec)     
       except Exception as e:
           msg = "GET analyze results failed:\n%s" % str(e)
           print(msg)
           n_try += 1
           print("Analyze operation did not complete within the allocated time.")
           quit()
    
    ```
1. Verwenden Sie für die Prozedur zum Schreiben der Datei eine neue Zelle:

    ```python
    import requests
    file = open(output, "w")
    file.write(str(resp_json))
    file.close()
    ```

## <a name="automate-training-and-scoring-by-using-azure-data-factory"></a>Automatisieren des Trainierens und Bewertens mithilfe von Azure Data Factory

Der letzte Schritt besteht darin, den Azure Data Factory-Dienst einzurichten, um die Trainings- und Bewertungsprozesse zu automatisieren. Führen Sie zuerst die Schritte unter [Erstellen einer Data Factory](../../data-factory/quickstart-create-data-factory-portal.md#create-a-data-factory) aus. Nach der Erstellung der Azure Data Factory-Ressource müssen drei Pipelines erstellt werden: eine für das Training und zwei für die Bewertung. (Dies wird später noch näher erläutert.)

### <a name="training-pipeline"></a>Trainingspipeline

Die erste Aktivität in der Trainingspipeline ist ein Lookup-Vorgang, um die Werte aus der Parametrisierungstabelle in der Azure SQL-Datenbank-Instanz zu lesen und zurückzugeben. Alle Trainingsdatasets befinden sich im gleichen Speicherkonto und Container (aber möglicherweise in unterschiedlichen Ordnern). Daher behalten wir in den Einstellungen der Lookup-Aktivität den Standardattributwert **First row only** (Nur erste Zeile) bei. Für jeden Formulartyp, mit dem das Modell trainiert werden soll, wird das Modell mit allen Dateien in **training_blob_root_folder** trainiert.

:::image type="content" source="./media/tutorial-bulk-processing/training-pipeline.png" alt-text="Screenshot: Trainingspipeline in Data Factory":::

Von der gespeicherten Prozedur werden zwei Parameter akzeptiert: **model_id** und **form_batch_group_id**. Der Code zum Zurückgeben der Modell-ID aus dem Databricks-Notebook lautet `dbutils.notebook.exit(model_id)`. Der Code zum Lesen des Codes in der Aktivität der gespeicherten Prozedur in Data Factory lautet `@activity('GetParametersFromMetadaTable').output.firstRow.form_batch_group_id`.

### <a name="scoring-pipelines"></a>Bewertungspipelines

Um die Schlüssel-Wert-Paare zu extrahieren, werden alle Ordner in der Parametrisierungstabelle gescannt. Bei jedem Ordner werden die Schlüssel-Wert-Paare aller enthaltenen Dateien extrahiert. Von Azure Data Factory werden keine geschachtelten ForEach-Schleifen unterstützt. Stattdessen erstellen wir zwei Pipelines. Die erste Pipeline führt den Lookup-Vorgang in der Parametrisierungstabelle durch und übergibt die Ordnerliste als Parameter an die zweite Pipeline.

:::image type="content" source="./media/tutorial-bulk-processing/scoring-pipeline-1a.png" alt-text="Screenshot: Erste Bewertungspipeline in Data Factory":::

:::image type="content" source="./media/tutorial-bulk-processing/scoring-pipeline-1b.png" alt-text="Screenshot: Details der ersten Bewertungspipeline in Data Factory":::

In der zweiten Pipeline wird mithilfe einer GetMeta-Aktivität die Liste der Dateien im Ordner abgerufen und als Parameter an das Databricks-Bewertungsnotebook übergeben.

:::image type="content" source="./media/tutorial-bulk-processing/scoring-pipeline-2a.png" alt-text="Screenshot: Zweite Bewertungspipeline in Data Factory":::

:::image type="content" source="./media/tutorial-bulk-processing/scoring-pipeline-2b.png" alt-text="Screenshot: Details der zweiten Bewertungspipeline in Data Factory":::

### <a name="specify-a-degree-of-parallelism"></a>Angeben eines Parallelitätsgrads

Sowohl in der Trainingspipeline als auch in den Bewertungspipelines können Sie den Grad der Parallelität angeben, um die gleichzeitige Verarbeitung mehrerer Formulare zu ermöglichen.

So legen Sie den Grad der Parallelität in der Azure Data Factory-Pipeline fest:

1. Wählen Sie die Aktivität **ForEach** aus.
1. Deaktivieren Sie das Kontrollkästchen **Sequential** (Sequenziell).
1. Legen Sie im Feld **Batch count** (Batchanzahl) den Grad der Parallelität fest. Für die Bewertung sollte die Batchanzahl maximal auf „15“ festgelegt werden.

:::image type="content" source="./media/tutorial-bulk-processing/parallelism.png" alt-text="Screenshot: Konfiguration der Parallelität für die Bewertungsaktivität in Azure Data Factory":::

## <a name="how-to-use-the-pipeline"></a>Verwenden der Pipeline

Sie verfügen nun über eine automatisierte Pipeline, mit der Sie Ihr Backlog an Formularen digitalisieren und einige Analysen dafür durchführen können. Wenn Sie einem vorhandenen Speicherordner neue Formulare eines bekannten Typs hinzufügen, können Sie die Bewertungspipelines einfach erneut ausführen. Dadurch werden alle Ihre Ausgabedateien aktualisiert (einschließlich Ausgabedateien für die neuen Formulare). 

Wenn Sie neue Formulare eines neuen Typs hinzufügen, müssen Sie auch ein Trainingsdataset in den entsprechenden Container hochladen. Fügen Sie als Nächstes der Parametrisierungstabelle eine neue Zeile hinzu, und geben Sie die Speicherorte der neuen Dokumente und des zugehörigen Trainingsdatasets ein. Geben Sie für **model_ID** den Wert „-1“ ein, um anzugeben, dass für die Formulare ein neues Modell trainiert werden muss. Führen Sie dann die Trainingspipeline in Azure Data Factory aus. Die Pipeline liest daraufhin aus der Tabelle, trainiert ein Modell und überschreibt die Modell-ID in der Tabelle. Anschließend können Sie die Bewertungspipelines aufrufen, um die Ausgabedateien zu schreiben.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Azure Data Factory-Pipelines eingerichtet, um das Trainieren und Ausführen von Formularerkennungsmodellen auszulösen und einen umfangreichen Backlog von Dateien zu digitalisieren. Im nächsten Artikel finden Sie Informationen zur API für die Formularerkennung sowie zu weiteren Verwendungsmöglichkeiten.

* [REST-API für die Formularerkennung](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-3/operations/AnalyzeBusinessCardAsync)
