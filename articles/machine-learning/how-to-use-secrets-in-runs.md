---
title: Verwenden von Geheimnissen in Trainingsausführungen
titleSuffix: Azure Machine Learning
description: Übergeben Sie Geheimnisse an Trainingsausführungen auf sichere Weise mit dem Schlüsseltresor des Arbeitsbereichs.
services: machine-learning
author: rastala
ms.author: roastala
ms.reviewer: larryfr
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 03/09/2020
ms.openlocfilehash: d877794abf12b8b412cd1ecf4efd72fd1179d768
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78942262"
---
# <a name="use-secrets-in-training-runs"></a>Verwenden von Geheimnissen in Trainingsausführungen
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie Geheimnisse sicher in Trainingsausführungen verwenden. Authentifizierungsinformationen (wie etwa Ihr Benutzername und Ihr Kennwort) sind Geheimnisse. Wenn Sie beispielsweise eine Verbindung mit einer externen Datenbank herstellen möchten, um Trainingsdaten abzufragen, müssen Sie Ihren Benutzernamen und Ihr Kennwort an den Remoteausführungskontext übergeben. Das Codieren solcher Werte in Trainingsskripts in Klartext ist unsicher, da so das Geheimnis verfügbar gemacht wird. 

Daher ist Ihrem Azure Machine Learning-Arbeitsbereich eine Ressource namens [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview) zugeordnet. Verwenden Sie diese Key Vault-Instanz, um Geheimnisse über eine Gruppe von APIs aus dem Azure Machine Learning Python SDK sicher an Remoteausführungen zu übergeben.

Der grundlegende Ablauf für das Verwenden von Geheimnissen sieht wie folgt aus:
 1. Melden Sie sich auf einem lokalen Computer bei Azure an, und stellen Sie eine Verbindung mit Ihrem Arbeitsbereich her.
 2. Legen Sie auf dem lokalen Computer ein Geheimnis in der Key Vault-Instanz des Arbeitsbereichs fest.
 3. Starten Sie eine Remoteausführung.
 4. Rufen Sie innerhalb der Remoteausführung das Geheimnis aus der Key Vault-Instanz ab, und verwenden Sie es.

## <a name="set-secrets"></a>Festlegen von Geheimnissen

In Azure Machine Learning enthält die Klasse [Keyvault](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py) Methoden zum Festlegen von Geheimnissen. Rufen Sie in Ihrer lokalen Python-Sitzung zunächst einen Verweis auf die Key Vault-Instanz Ihres Arbeitsbereichs ab, und verwenden Sie dann die Methode [`set_secret()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#set-secret-name--value-), um ein Geheimnis mit Name und Wert festzulegen. Die __set_secret__-Methode aktualisiert den Geheimniswert, wenn der Name bereits vorhanden ist.

```python
from azureml.core import Workspace
from azureml.core import Keyvault
import os


ws = Workspace.from_config()
my_secret = os.environ.get("MY_SECRET")
keyvault = ws.get_default_keyvault()
keyvault.set_secret(name="mysecret", value = my_secret)
```

Schreiben Sie den Wert des Geheimnisses nicht in Python-Code, weil es unsicher ist, den Wert als Klartext in einer Datei zu speichern. Rufen Sie den Wert des Geheimnisses stattdessen aus einer Umgebungsvariablen, z. B. Azure DevOps-Buildgeheimnis, oder aus einer interaktiven Benutzereingabe ab.

Geheimnisnamen können mithilfe der Methode [`list_secrets()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#list-secrets--) aufgelistet werden. Mit [set_secrets()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#set-secrets-secrets-batch-) steht außerdem eine Batchversion zur Verfügung, mit der Sie mehrere Geheimnisse gleichzeitig festlegen können.

## <a name="get-secrets"></a>Abrufen geheimer Schlüssel

In Ihrem lokalen Code können Sie die Methode [`get_secret()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#get-secret-name-) verwenden, um den Geheimniswert anhand des Namens abzurufen.

Verwenden Sie für Ausführungen mit Übermittlung von [`Experiment.submit`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py#submit-config--tags-none----kwargs-) die Methode [`get_secret()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#get-secret-name-) mit der Klasse [`Run`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py). Da die übermittelte Ausführung ihren Arbeitsbereich kennt, umgeht diese Methode die Arbeitsbereichsinstanziierung und gibt den Geheimniswert direkt zurück.

```python
# Code in submitted run
from azureml.core import Experiment, Run

run = Run.get_context()
secret_value = run.get_secret(name="mysecret")
```

Achten Sie darauf, dass Sie den Geheimniswert nicht dadurch verfügbar machen, dass Sie ihn schreiben oder drucken.

Mit [get_secrets()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#get-secrets-secrets-) steht auch eine Batchversion zur Verfügung, mit der Sie gleichzeitig auf mehrere Geheimnisse zugreifen können.

## <a name="next-steps"></a>Nächste Schritte

 * [Beispielnotebook anzeigen](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/manage-azureml-service/authentication-in-azureml/authentication-in-azureml.ipynb)
 * [Informationen über Unternehmenssicherheit mit Azure Machine Learning](concept-enterprise-security.md)
