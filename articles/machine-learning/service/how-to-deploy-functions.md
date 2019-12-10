---
title: Bereitstellen von ML-Modellen in Azure Functions-Apps (Vorschauversion)
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie mithilfe von Azure Machine Learning ein Modell in einer Azure Functions-App bereitstellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: vaidyas
author: vaidyas
ms.reviewer: larryfr
ms.date: 11/22/2019
ms.openlocfilehash: 9fba3221656405f2bf2b1654b43d687f1915cca6
ms.sourcegitcommit: 36eb583994af0f25a04df29573ee44fbe13bd06e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2019
ms.locfileid: "74542289"
---
# <a name="deploy-a-machine-learning-model-to-azure-functions-preview"></a>Bereitstellen eines Machine Learning-Modells in Azure Functions (Vorschauversion)
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

Hier erfahren Sie, wie Sie ein Modell in Azure Machine Learning als Funktions-App in Azure Functions bereitstellen.

> [!IMPORTANT]
> Azure Machine Learning und Azure Functions sind zwar allgemein verfügbar, die Funktion zum Bereitstellen eines Modells aus dem Machine Learning-Dienst für Functions befindet sich jedoch in der Vorschauphase.

Mit Azure Machine Learning können Sie Docker-Images auf der Grundlage trainierter Machine Learning-Modelle erstellen. Azure Machine Learning bietet nun die Vorschaufunktion zum Einbinden dieser Machine Learning-Modelle in Funktions-Apps, die [in Azure Functions bereitgestellt](https://docs.microsoft.com/azure/azure-functions/functions-deployment-technologies#docker-container) werden können.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure Machine Learning-Arbeitsbereich. Weitere Informationen finden Sie im Artikel [Erstellen eines Arbeitsbereichs](how-to-manage-workspace.md).
* Die [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
* Ein in Ihrem Arbeitsbereich registriertes trainiertes Machine Learning-Modell. Falls Sie kein Modell besitzen, können Sie anhand der Informationen unter [Tutorial: Trainieren von Bildklassifikationsmodellen mit MNIST-Daten und Scikit-learn mithilfe von Azure Machine Learning](tutorial-train-models-with-aml.md) ein Modell trainieren und registrieren.

    > [!IMPORTANT]
    > Bei den in diesem Artikel verwendeten Codeausschnitten wird davon ausgegangen, dass Sie die folgenden Variablen festgelegt haben:
    >
    > * `ws`: Ihr Azure Machine Learning-Arbeitsbereich
    > * `model`: das registrierte Modell, das bereitgestellt wird
    > * `inference_config`: die Rückschlusskonfiguration für das Modell
    >
    > Weitere Informationen zum Festlegen dieser Variablen finden Sie unter [Bereitstellen von Modellen mit Azure Machine Learning](how-to-deploy-and-where.md).

## <a name="prepare-for-deployment"></a>Vorbereiten der Bereitstellung

Vor der Bereitstellung müssen Sie die Elemente definieren, die zum Ausführen des Modells als Webdienst erforderlich sind. Im Folgenden werden die allgemeinen für eine Bereitstellung erforderlichen Elemente beschrieben:

* Ein __Eingabeskript__. Dieses Skript akzeptiert Anforderungen, bewertet die Anforderung mithilfe des Modells und gibt die Ergebnisse zurück.

    > [!IMPORTANT]
    > Das Eingabeskript ist spezifisch für Ihr Modell. Es muss das Format der Daten der eingehenden Anforderung, das Format der im Modell erwarteten Daten und das Format der an Clients zurückgegebenen Daten erkennen.
    >
    > Wenn die Anforderungsdaten in einem Format vorliegen, das in Ihrem Modell nicht verwendet werden kann, können sie im Skript in ein akzeptables Format umgewandelt werden. Außerdem kann die Antwort umgewandelt werden, bevor sie an den Client zurückgegeben wird.
    >
    > Beim Packen für Functions wird die Eingabe standardmäßig als Text behandelt. Wenn Sie die Rohbytes der Eingabe verarbeiten möchten (z. B. für Blob-Trigger), müssen Sie [AMLRequest zum Akzeptieren von Rohdaten](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where#binary-data) verwenden.


* **Abhängigkeiten**, z. B. Hilfsprogramme oder Python/Conda-Pakete, die zum Ausführen des Eingabeskripts oder Modells erforderlich sind

Diese Entitäten werden in einer __Rückschlusskonfiguration__ gekapselt. Die Rückschlusskonfiguration verweist auf das Eingabeskript und andere Abhängigkeiten.

> [!IMPORTANT]
> Beim Erstellen einer Rückschlusskonfiguration zur Verwendung mit Azure Functions müssen Sie ein [Environment](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment%28class%29?view=azure-ml-py)-Objekt verwenden. Im folgenden Beispiel wird veranschaulicht, wie ein Environment-Objekt erstellt und mit einer Rückschlusskonfiguration verwendet wird:
>
> ```python
> from azureml.core import Environment
> from azureml.core.environment import CondaDependencies
>
> # Create an environment and add conda dependencies to it
> myenv = Environment(name="myenv")
> # Enable Docker based environment
> myenv.docker.enabled = True
> # Build conda dependencies
> myenv.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])
> ```

Weitere Informationen zu Umgebungen finden Sie unter [Erstellen und Verwalten von Umgebungen für Training und Bereitstellung](how-to-use-environments.md).

Weitere Informationen zur Rückschlusskonfiguration finden Sie unter [Bereitstellen von Modellen mit Azure Machine Learning](how-to-deploy-and-where.md).

> [!IMPORTANT]
> Bei der Bereitstellung in Functions müssen Sie keine __Bereitstellungskonfiguration__ erstellen.

## <a name="install-the-sdk-preview-package-for-functions-support"></a>Installieren des SDK-Vorschaupakets für Functions-Unterstützung

Zum Erstellen von Paketen für Azure Functions müssen Sie das SDK-Vorschaupaket installieren.

```bash
pip install azureml-contrib-functions
```

## <a name="create-the-image"></a>Erstellen des Images

Verwenden Sie zum Erstellen des in Azure Functions bereitgestellten Docker-Images [azureml.contrib.functions.package](https://docs.microsoft.com/python/api/azureml-contrib-functions/azureml.contrib.functions?view=azure-ml-py) oder die spezifische Paketfunktion für den Trigger, den Sie verwenden möchten. Der folgende Codeausschnitt veranschaulicht, wie ein neues Paket mit einem Blob-Trigger aus der Modell- und Rückschlusskonfiguration erstellt wird:

> [!NOTE]
> Im Codeausschnitt wird davon ausgegangen, dass `model` ein registriertes Modell und `inference_config` die Konfiguration für die Rückschlussumgebung enthält. Weitere Informationen finden Sie unter [Bereitstellen von Modellen mit Azure Machine Learning](how-to-deploy-and-where.md).

```python
from azureml.contrib.functions import package
from azureml.contrib.functions import BLOB_TRIGGER
blob = package(ws, [model], inference_config, functions_enabled=True, trigger=BLOB_TRIGGER, input_path="input/{blobname}.json", output_path="output/{blobname}_out.json")
blob.wait_for_creation(show_output=True)
# Display the package location/ACR path
print(blob.location)
```

Bei `show_output=True` wird die Ausgabe des Docker-Buildprozesses angezeigt. Nach Abschluss des Prozesses wurde das Image in der Azure Container Registry-Instanz für Ihren Arbeitsbereich erstellt. Nachdem das Image erstellt wurde, wird der Speicherort in Ihrer Instanz von Azure Container Registry angezeigt. Der zurückgegebene Speicherort weist das Format `<acrinstance>.azurecr.io/package@sha256:<hash>` auf.

> [!NOTE]
> Pakete für Functions unterstützen derzeit HTTP-Trigger, Blob-Trigger und Service Bus-Trigger. Weitere Informationen zu Triggern finden Sie unter [Azure Functions-Bindungen](https://docs.microsoft.com/azure/azure-functions/functions-bindings-storage-blob?tabs=csharp#trigger---blob-name-patterns).

> [!IMPORTANT]
> Speichern Sie die Speicherortinformationen, da sie beim Bereitstellen des Images verwendet werden.

## <a name="deploy-image-as-a-web-app"></a>Bereitstellen eines Images als Web-App

1. Verwenden Sie den folgenden Befehl, um die Anmeldeinformationen für die Azure Container Registry-Instanz zu erhalten, die das Image enthält. Ersetzen Sie `<acrinstance>` durch den zuvor von `package.location` zurückgegebenen Wert: 

    ```azurecli-interactive
    az acr credential show --name <myacr>
    ```

    Die Ausgabe dieses Befehls ähnelt dem folgenden JSON-Dokument:

    ```json
    {
    "passwords": [
        {
        "name": "password",
        "value": "Iv0lRZQ9762LUJrFiffo3P4sWgk4q+nW"
        },
        {
        "name": "password2",
        "value": "=pKCxHatX96jeoYBWZLsPR6opszr==mg"
        }
    ],
    "username": "myml08024f78fd10"
    }
    ```

    Speichern Sie die Werte für __Benutzername__ und eines der __Kennwörter__.

1. Wenn Sie noch nicht über eine Ressourcengruppe oder einen App Service-Plan für die Bereitstellung des Diensts verfügen, veranschaulichen die folgenden Befehle das Erstellen dieser beiden Elemente:

    ```azurecli-interactive
    az group create --name myresourcegroup --location "West Europe"
    az appservice plan create --name myplanname --resource-group myresourcegroup --sku EP1 --is-linux
    ```

    In diesem Beispiel wird ein _Linux Premium_-Tarif (`--sku EP1`) verwendet.

    > [!IMPORTANT]
    > Die von Azure Machine Learning erstellten Images verwenden Linux, daher müssen Sie den Parameter `--is-linux` verwenden.

1. Verwenden Sie den folgenden Befehl, um die Funktions-App zu erstellen. Ersetzen Sie `<app-name>` durch den Namen, den Sie verwenden möchten. Ersetzen Sie `<acrinstance>` und `<imagename>` durch die zuvor von `package.location` zurückgegebenen Werte:

    ```azurecli-interactive
    az storage account create --name 
    az functionapp create --resource-group myresourcegroup --plan myplanname --name <app-name> --deployment-container-image-name <acrinstance>.azurecr.io/package:<imagename>
    ```

    > [!IMPORTANT]
    > An diesem Punkt wurde die Funktions-App erstellt. Da Sie jedoch nicht die Verbindungszeichenfolge für den Blob-Trigger oder Anmeldeinformationen für die Azure Container Registry-Instanz mit dem Image angegeben haben, ist die Funktions-App nicht aktiv. Im nächsten Schritt geben Sie die Verbindungszeichenfolge und die Authentifizierungsinformationen für die Containerregistrierung an. 

1. Erstellen Sie das Speicherkonto, das als Trigger verwendet werden soll, und rufen Sie die zugehörige Verbindungszeichenfolge ab.

    ```azurecli-interactive
    az storage account create --name triggerStorage --location westeurope --resource-group myresourcegroup --sku Standard_LRS
    ```
    ```azurecli-interactive
    az storage account show-connection-string --resource-group myresourcegroup --name triggerStorage --query connectionString --output tsv
    ```
    Notieren Sie sich diese Verbindungszeichenfolge, die für die Funktions-App angegeben werden soll. Sie wird später benötigt, wenn `<triggerConnectionString>` abgefragt wird.

1. Erstellen Sie die Container für Eingabe und Ausgabe im Speicherkonto. 

    ```azurecli-interactive
    az storage container create -n input --connection-string <triggerConnectionString>
    ```
    ```azurecli-interactive
    az storage container create -n output --connection-string <triggerConnectionString>
    ```

1. Sie müssen das Tag für den erstellten Container mit dem folgenden Befehl abrufen:

    ```azurecli-interactive
    az acr repository show-tags --repository package --name <username> --output tsv
    ```
    Das angezeigte neueste Tag ist unten `imagetag`.

1. Verwenden Sie den folgenden Befehl, um die für den Zugriff auf die Containerregistrierung erforderlichen Anmeldeinformationen für die Funktions-App bereitzustellen. Ersetzen Sie `<app-name>` durch den Namen, den Sie verwenden möchten. Ersetzen Sie `<acrinstance>` und `<imagetag>` durch die Werte aus dem AZ CLI-Aufruf im vorherigen Schritt. Ersetzen Sie `<username>` und `<password>` durch die zuvor abgerufenen ACR-Anmeldeinformationen:

    ```azurecli-interactive
    az functionapp config container set --name <app-name> --resource-group myresourcegroup --docker-custom-image-name <acrinstance>.azurecr.io/package:<imagetag> --docker-registry-server-url https://<acrinstance>.azurecr.io --docker-registry-server-user <username> --docker-registry-server-password <password>
    ```

    Dieser Befehl gibt Informationen ähnlich dem folgenden JSON-Dokument zurück:

    ```json
    [
    {
        "name": "WEBSITES_ENABLE_APP_SERVICE_STORAGE",
        "slotSetting": false,
        "value": "false"
    },
    {
        "name": "DOCKER_REGISTRY_SERVER_URL",
        "slotSetting": false,
        "value": "https://myml08024f78fd10.azurecr.io"
    },
    {
        "name": "DOCKER_REGISTRY_SERVER_USERNAME",
        "slotSetting": false,
        "value": "myml08024f78fd10"
    },
    {
        "name": "DOCKER_REGISTRY_SERVER_PASSWORD",
        "slotSetting": false,
        "value": null
    },
    {
        "name": "DOCKER_CUSTOM_IMAGE_NAME",
        "value": "DOCKER|myml08024f78fd10.azurecr.io/package:20190827195524"
    }
    ]
    ```

Zu diesem Zeitpunkt beginnt die Funktions-App mit dem Laden des Images.

> [!IMPORTANT]
> Es kann einige Minuten dauern, bis das Image geladen wurde. Sie können den Fortschritt im Azure-Portal überwachen.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie in der Dokumentation zu [Functions](https://docs.microsoft.com/azure/azure-functions/functions-create-function-linux-custom-imag), wie Sie Ihre Funktions-App konfigurieren.
* Weitere Informationen zu Blob Storage-Triggern finden Sie unter [Azure Blob Storage-Bindungen](https://docs.microsoft.com/azure/azure-functions/functions-bindings-storage-blob).
* [Bereitstellen Ihres Modells in Azure App Service](how-to-deploy-app-service.md).
* [Consume a ML Model deployed as a web service (Nutzen eines als Webdienst bereitgestellten Azure Machine Learning-Modells)](how-to-consume-web-service.md).
* [API-Referenz](https://docs.microsoft.com/python/api/azureml-contrib-functions/azureml.contrib.functions?view=azure-ml-py)