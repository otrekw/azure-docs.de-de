---
title: Authentifizierungsgeheimnisse im Training
titleSuffix: Azure Machine Learning
description: Übergeben Sie Geheimnisse an Trainingsausführungen auf sichere Weise mit dem Schlüsseltresor des Arbeitsbereichs.
services: machine-learning
author: rastala
ms.author: roastala
ms.reviewer: larryfr
ms.service: machine-learning
ms.subservice: core
ms.date: 03/09/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: e984c0c43dcc47c3e11a36f3d5c32bf2ddb9973a
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90902336"
---
# <a name="use-authentication-credential-secrets-in-azure-machine-learning-training-runs"></a>Verwenden von Geheimnissen als Anmeldeinformationen für die Authentifizierung in Azure Machine Learning-Trainingsausführungen


In diesem Artikel erfahren Sie, wie Sie Geheimnisse sicher in Trainingsausführungen verwenden. Authentifizierungsinformationen (wie etwa Ihr Benutzername und Ihr Kennwort) sind Geheimnisse. Wenn Sie beispielsweise eine Verbindung mit einer externen Datenbank herstellen möchten, um Trainingsdaten abzufragen, müssen Sie Ihren Benutzernamen und Ihr Kennwort an den Remoteausführungskontext übergeben. Das Codieren solcher Werte in Trainingsskripts in Klartext ist unsicher, da so das Geheimnis verfügbar gemacht wird. 

Daher ist Ihrem Azure Machine Learning-Arbeitsbereich eine Ressource namens [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview) zugeordnet. Verwenden Sie diese Key Vault-Instanz, um Geheimnisse über eine Gruppe von APIs aus dem Azure Machine Learning Python SDK sicher an Remoteausführungen zu übergeben.

Der Standardablauf für das Verwenden von Geheimnissen sieht wie folgt aus:
 1. Melden Sie sich auf einem lokalen Computer bei Azure an, und stellen Sie eine Verbindung mit Ihrem Arbeitsbereich her.
 2. Legen Sie auf dem lokalen Computer ein Geheimnis in der Key Vault-Instanz des Arbeitsbereichs fest.
 3. Starten Sie eine Remoteausführung.
 4. Rufen Sie innerhalb der Remoteausführung das Geheimnis aus der Key Vault-Instanz ab, und verwenden Sie es.

## <a name="set-secrets"></a>Festlegen von Geheimnissen

In Azure Machine Learning enthält die Klasse [Keyvault](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py&preserve-view=true) Methoden zum Festlegen von Geheimnissen. Rufen Sie in Ihrer lokalen Python-Sitzung zunächst einen Verweis auf die Key Vault-Instanz Ihres Arbeitsbereichs ab, und verwenden Sie dann die Methode [`set_secret()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#&preserve-view=trueset-secret-name--value-), um ein Geheimnis mit Name und Wert festzulegen. Die __set_secret__-Methode aktualisiert den Geheimniswert, wenn der Name bereits vorhanden ist.

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

Geheimnisnamen können mithilfe der Methode [`list_secrets()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#&preserve-view=truelist-secrets--) aufgelistet werden. Mit [set_secrets()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#&preserve-view=trueset-secrets-secrets-batch-) steht außerdem eine Batchversion zur Verfügung, mit der Sie mehrere Geheimnisse gleichzeitig festlegen können.

## <a name="get-secrets"></a>Abrufen geheimer Schlüssel

In Ihrem lokalen Code können Sie die Methode [`get_secret()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#&preserve-view=trueget-secret-name-) verwenden, um den Geheimniswert anhand des Namens abzurufen.

Verwenden Sie für Ausführungen mit Übermittlung von [`Experiment.submit`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py#&preserve-view=truesubmit-config--tags-none----kwargs-) die Methode [`get_secret()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#&preserve-view=trueget-secret-name-) mit der Klasse [`Run`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py&preserve-view=true). Da die übermittelte Ausführung ihren Arbeitsbereich kennt, umgeht diese Methode die Arbeitsbereichsinstanziierung und gibt den Geheimniswert direkt zurück.

```python
# Code in submitted run
from azureml.core import Experiment, Run

run = Run.get_context()
secret_value = run.get_secret(name="mysecret")
```

Achten Sie darauf, dass Sie den Geheimniswert nicht dadurch verfügbar machen, dass Sie ihn schreiben oder drucken.

Mit [get_secrets()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#&preserve-view=trueget-secrets-secrets-) steht auch eine Batchversion zur Verfügung, mit der Sie gleichzeitig auf mehrere Geheimnisse zugreifen können.

## <a name="next-steps"></a>Nächste Schritte

 * [Beispielnotebook anzeigen](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/manage-azureml-service/authentication-in-azureml/authentication-in-azureml.ipynb)
 * [Informationen über Unternehmenssicherheit mit Azure Machine Learning](concept-enterprise-security.md)
