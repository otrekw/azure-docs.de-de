---
title: 'Python-Skript ausführen: Modulreferenz'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie das Modul Execute Python Script in Azure Machine Learning zum Ausführen von Python-Code verwendet wird.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 03/10/2020
ms.openlocfilehash: 0f86d1ad03062797764af6a0d49beacaa3458a8f
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "80365564"
---
# <a name="execute-python-script-module"></a>Execute Python Script-Modul

In diesem Artikel wird ein Modul in Azure Machine Learning-Designer (Vorschauversion) beschrieben.

Verwenden Sie dieses Modul zum Ausführen von Python-Code. Weitere Informationen zur Architektur und den Entwurfsprinzipien von Python finden Sie [im folgenden Artikel](https://docs.microsoft.com/azure/machine-learning/machine-learning-execute-python-scripts).

Mit Python können Sie Aufgaben ausführen, die aktuell von vorhandenen Modulen nicht unterstützt werden, beispielsweise:

+ Visualisieren von Daten mit `matplotlib`
+ Verwenden von Python-Bibliotheken zum Auflisten von Datasets und Modellen in Ihrem Arbeitsbereich
+ Lesen, Laden und Bearbeiten von Daten aus Quellen, die vom Modul [Daten importieren](./import-data.md) nicht unterstützt werden
+ Ausführen eigenen Deep Learning-Codes 


Azure Machine Learning verwendet die Anaconda-Distribution von Python, die viele gängige Hilfsprogramme für die Datenverarbeitung enthält. Wir führen Updates der Anaconda-Version automatisch aus. Die aktuelle Version ist:
 -  Anaconda 4.5+-Distribution für Python 3.6 

Dies sind die vorinstallierten Pakete:
-    adal==1.2.2
-    applicationinsights==0.11.9
-    attrs==19.3.0
-    azure-common==1.1.25
-    azure-core==1.3.0
-    azure-graphrbac==0.61.1
-    azure-identity==1.3.0
-    azure-mgmt-authorization==0.60.0
-    azure-mgmt-containerregistry==2.8.0
-    azure-mgmt-keyvault==2.2.0
-    azure-mgmt-resource==8.0.1
-    azure-mgmt-storage==8.0.0
-    azure-storage-blob==1.5.0
-    azure-storage-common==1.4.2
-    azureml-core==1.1.5.5
-    azureml-dataprep-native==14.1.0
-    azureml-dataprep==1.3.5
-    azureml-defaults==1.1.5.1
-    azureml-designer-classic-modules==0.0.118
-    azureml-designer-core==0.0.31
-    azureml-designer-internal==0.0.18
-    azureml-model-management-sdk==1.0.1b6.post1
-    azureml-pipeline-core==1.1.5
-    azureml-telemetry==1.1.5.3
-    backports.tempfile==1.0
-    backports.weakref==1.0.post1
-    boto3==1.12.29
-    botocore==1.15.29
-    cachetools==4.0.0
-    certifi==2019.11.28
-    cffi==1.12.3
-    chardet==3.0.4
-    click==7.1.1
-    cloudpickle==1.3.0
-    configparser==3.7.4
-    contextlib2==0.6.0.post1
-    cryptography==2.8
-    cycler==0.10.0
-    dill==0.3.1.1
-    distro==1.4.0
-    docker==4.2.0
-    docutils==0.15.2
-    dotnetcore2==2.1.13
-    flask==1.0.3
-    fusepy==3.0.1
-    gensim==3.8.1
-    google-api-core==1.16.0
-    google-auth==1.12.0
-    google-cloud-core==1.3.0
-    google-cloud-storage==1.26.0
-    google-resumable-media==0.5.0
-    googleapis-common-protos==1.51.0
-    gunicorn==19.9.0
-    idna==2.9
-    imbalanced-learn==0.4.3
-    isodate==0.6.0
-    itsdangerous==1.1.0
-    jeepney==0.4.3
-    jinja2==2.11.1
-    jmespath==0.9.5
-    joblib==0.14.0
-    json-logging-py==0.2
-    jsonpickle==1.3
-    jsonschema==3.0.1
-    kiwisolver==1.1.0
-    liac-arff==2.4.0
-    lightgbm==2.2.3
-    markupsafe==1.1.1
-    matplotlib==3.1.3
-    more-itertools==6.0.0
-    msal-extensions==0.1.3
-    msal==1.1.0
-    msrest==0.6.11
-    msrestazure==0.6.3
-    ndg-httpsclient==0.5.1
-    nimbusml==1.6.1
-    numpy==1.18.2
-    oauthlib==3.1.0
-    pandas==0.25.3
-    pathspec==0.7.0
-    pip==20.0.2
-    portalocker==1.6.0
-    protobuf==3.11.3
-    pyarrow==0.16.0
-    pyasn1-modules==0.2.8
-    pyasn1==0.4.8
-    pycparser==2.20
-    pycryptodomex==3.7.3
-    pyjwt==1.7.1
-    pyopenssl==19.1.0
-    pyparsing==2.4.6
-    pyrsistent==0.16.0
-    python-dateutil==2.8.1
-    pytz==2019.3
-    requests-oauthlib==1.3.0
-    requests==2.23.0
-    rsa==4.0
-    ruamel.yaml==0.15.89
-    s3transfer==0.3.3
-    scikit-learn==0.22.2
-    scipy==1.4.1
-    secretstorage==3.1.2
-    setuptools==46.1.1.post20200323
-    six==1.14.0
-    smart-open==1.10.0
-    urllib3==1.25.8
-    websocket-client==0.57.0
-    werkzeug==0.16.1
-    wheel==0.34.2

 Um weitere Pakete zu installieren, die nicht in der Liste der vorinstallierten Pakete enthalten sind, beispielsweise *scikit-misc*, fügen Sie Ihrem Skript den folgenden Code hinzu: 

 ```python
import os
os.system(f"pip install scikit-misc")
```

## <a name="upload-files"></a>Hochladen von Dateien
Die Option **Execute Python Script** (Python-Skript ausführen) unterstützt das Hochladen von Dateien mit dem [Python-SDK für Azure Machine Learning](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py#upload-file-name--path-or-stream-).

Das folgende Beispiel zeigt, wie eine Imagedatei im Modul **Execute Python Script** (Python-Skript ausführen) hochgeladen wird:

```Python

# The script MUST contain a function named azureml_main
# which is the entry point for this module.

# imports up here can be used to
import pandas as pd

# The entry point function can contain up to two input arguments:
#   Param<dataframe1>: a pandas.DataFrame
#   Param<dataframe2>: a pandas.DataFrame
def azureml_main(dataframe1 = None, dataframe2 = None):

    # Execution logic goes here
    print(f'Input pandas.DataFrame #1: {dataframe1}')

    from matplotlib import pyplot as plt
    plt.plot([1, 2, 3, 4])
    plt.ylabel('some numbers')
    img_file = "line.png"
    plt.savefig(img_file)

    from azureml.core import Run
    run = Run.get_context(allow_offline=True)
    run.upload_file(f"graphics/{img_file}", img_file)

    # Return value must be of a sequence of pandas.DataFrame
    # E.g.
    #   -  Single return value: return dataframe1,
    #   -  Two return values: return dataframe1, dataframe2
    return dataframe1,
}
```

Nachdem die Pipelineausführung abgeschlossen ist, können Sie eine Vorschau des Images im rechten Bereich des Moduls anzeigen.

> [!div class="mx-imgBorder"]
> ![Uploaded-image](media/module/upload-image-in-python-script.png)

## <a name="how-to-configure-execute-python-script"></a>Konfigurieren von „Execute Python Script“ (Python-Skript ausführen)

Das Modul **Execute Python Script** enthält Python-Beispielcode, den Sie als Ausgangspunkt verwenden können. Um das Modul **Python-Skript ausführen** zu konfigurieren, geben Sie einen Satz von Eingaben und auszuführenden Python-Code im Textfeld **Pytho-Skript** ein.

1. Fügen Sie Ihrer Pipeline das Modul **Python-Skript ausführen** hinzu.

2. Fügen Sie in **Dataset1** alle Datasets aus dem Designer hinzu, die Sie als Eingabe verwenden möchten. Verweisen Sie auf dieses Dataset in Ihrem Python-Skript als **DataFrame1**.

    Die Verwendung eines Datasets ist optional, wenn Sie Daten mithilfe von Python generieren oder Python-Code zum direkten Importieren der Daten in das Modul verwenden möchten.

    Dieses Modul unterstützt das Hinzufügen eines zweiten Datensets in **Dataset2**. Verweisen Sie auf das zweite Dataset in ihrem Python-Skript als „DataFrame2“.

    In Azure Machine Learning gespeicherte Datasets werden automatisch in **Pandas** data.frames konvertiert, wenn sie mit diesem Modul geladen werden.

    ![Eingabezuordnung zum Ausführen von Python-Code](media/module/python-module.png)

4. Um neue Python-Pakete oder Code einzuschließen, fügen Sie die ZIP-Datei, die diese benutzerdefinierten Ressourcen enthält, in **Script bundle** (Skriptbundle) hinzu. Die Eingabe in **Script bundle** muss eine ZIP-Datei sein, die als Dateityp „Dataset“ in Ihren Arbeitsbereich hochgeladen wurde. Sie können das Dataset auf der Ressourcenseite **Datasets** hochladen, und Sie können das Datasetmodul mithilfe von Drag & Drop aus der Liste **My Datasets** (Meine Datasets) in der linken Modulstruktur auf der Designer-Erstellungsseite verschieben. 

    Während der Ausführung der Pipeline kann jede Datei verwendet werden, die im hochgeladenen ZIP-Archiv enthalten ist. Wenn das Archiv eine Verzeichnisstruktur enthält, bleibt die Struktur erhalten, Sie müssen dem Pfad aber ein Verzeichnis mit dem Namen **src** voranstellen.

5. Geben oder fügen Sie in das Textfeld **Python script** (Python-Skript) ein gültiges Python-Skript ein.

    Das Textfeld **Python script** (Python-Skript) ist mit einigen Anweisungen in Kommentaren und Beispielcode für den Datenzugriff und die Datenausgabe voraufgefüllt. Sie müssen diesen Code bearbeiten oder ersetzen. Achten Sie darauf, die Python-Konventionen zu Einzug und Schreibweise zu beachten.

    + Das Skript muss eine Funktion mit dem Namen `azureml_main` als Einstiegspunkt für dieses Modul enthalten.
    + Die Einstiegspunktfunktion kann bis zu zwei Eingabeargumente enthalten: `Param<dataframe1>` und `Param<dataframe2>`
    + ZIP-Dateien, die mit dem dritten Eingabeport verbunden sind, werden entzippt und im Verzeichnis `.\Script Bundle` gespeichert, das dem Python-`sys.path` ebenfalls hinzugefügt wird. 

    Wenn Ihre ZIP-Datei `mymodule.py` enthält, importieren Sie sie daher mit `import mymodule`.

    + Es können zwei Datasets den Designer zurückgegeben werden, bei denen es sich um eine Sequenz vom Typ `pandas.DataFrame` handeln muss. Sie können weitere Ausgaben in Ihrem Python-Code erstellen und sie direkt in den Azure-Speicher schreiben.

6. Übermitteln Sie die Pipeline, oder wählen Sie das Modul aus, und klicken Sie auf **Auswahl ausführen**, um nur das Python-Skript auszuführen.

    Alle Daten und der gesamte Code werden in einen virtuellen Computer geladen und unter Verwendung der angegebenen Python-Umgebung ausgeführt.

## <a name="results"></a>Ergebnisse

Die Ergebnisse aller Berechnungen, die vom eingebetteten Python-Code ausgeführt werden, müssen als ein pandas.DataFrame bereitgestellt werden, der automatisch in das Datasetformat von Azure Machine Learning konvertiert wird, sodass Sie die Ergebnisse zusammen mit anderen Modulen in der Pipeline verwenden können.

Das Modul gibt zwei Datasets zurück:  
  
+ **Results Dataset 1** (Ergebnisdataset 1), durch den ersten zurückgegebenen pandas-Datenrahmen im Python-Skript definiert

+ **Results Dataset 2** (Ergebnisdataset 2), durch den zweiten zurückgegebenen pandas-Datenrahmen im Python-Skript definiert


## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 