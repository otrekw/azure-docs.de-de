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
ms.date: 11/08/2019
ms.custom: seodec18
ms.openlocfilehash: 722e329b1a2d069b614a301a5eb2f8642b697295
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/28/2019
ms.locfileid: "75534762"
---
# <a name="use-secrets-in-training-runs"></a>Verwenden von Geheimnissen in Trainingsausführungen
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie Geheimnisse sicher in Trainingsausführungen verwenden. Beispielsweise müssen Sie, wenn Sie eine Verbindung mit einer externen Datenbank herstellen möchten, um Trainingsdaten abzufragen, Ihren Benutzernamen und Ihr Kennwort an den Remoteausführungskontext übergeben. Das Codieren solcher Werte in Trainingsskripts in Klartext ist unsicher, da so das Geheimnis verfügbar gemacht wird. 

Stattdessen hat Ihr Azure Machine Learning-Arbeitsbereich [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview) als zugehörige Ressource. Diese Key Vault-Instanz kann verwendet werden, um Geheimnisse über APIs, die zum Azure Machine Learning Python SDK gehören, sicher an Remoteausführungen zu übergeben.

Der grundlegende Ablauf für das Verwenden von Geheimnissen sieht wie folgt aus:
 1. Melden Sie sich auf einem lokalen Computer bei Azure an, und stellen Sie eine Verbindung mit Ihrem Arbeitsbereich her.
 2. Legen Sie auf dem lokalen Computer ein Geheimnis in der Key Vault-Instanz des Arbeitsbereichs fest.
 3. Starten Sie eine Remoteausführung.
 4. Rufen Sie in der Remoteausführung das Geheimnis aus dem Schlüsselwert ab, und verwenden Sie es.

## <a name="set-secrets"></a>Festlegen von Geheimnissen

Im Azure Machine Learning Python SDK enthält die [Keyvault](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py)-Klasse Methoden zum Festlegen von Geheimnissen. Rufen Sie in Ihrer lokalen Python-Sitzung zuerst einen Verweis auf Azure Key Vault ab, und verwenden Sie dann die [set_secret](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#set-secret-name--value-)-Methode, um ein Geheimnis mit Name und Wert festzulegen.

```python
from azureml.core import Workspace
import os

ws = Workspace.from_config()
my_secret = os.environ.get("MY_SECRET")
keyvault = ws.get_default_keyvault()
keyvault.set_secret(name="mysecret", value = my_secret)
```

Schreiben Sie den Wert des Geheimnisses nicht in Python-Code, weil es unsicher ist, den Wert als Klartext in einer Datei zu speichern. Rufen Sie den Wert des Geheimnisses stattdessen aus einer Umgebungsvariablen, z. B. Azure DevOps-Buildgeheimnis, oder aus einer interaktiven Benutzereingabe ab.

Sie können Geheimnisnamen mit der [list_secrets](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#list-secrets--)-Methode auflisten. Die __set_secret__-Methode aktualisiert den Geheimniswert, wenn der Name bereits vorhanden ist.

## <a name="get-secrets"></a>Abrufen geheimer Schlüssel

Im lokalen Code können Sie die [Keyvault.get_secret](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#get-secret-name-)-Methode verwenden, um den Geheimniswert nach Namen abzurufen.

In Ausführungen, die mit [Experiment.submit](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py#submit-config--tags-none----kwargs-) übermittelt (gestartet) wurden, verwenden Sie die [Run.get_secret](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#get-secret-name-)-Methode. Da die übermittelte Ausführung ihren Arbeitsbereich kennt, umgeht diese Methode die Arbeitsbereichsinstanziierung und gibt den Geheimniswert direkt zurück.

```python
# Code in submitted run
from azureml.core import Run

run = Run.get_context()
secret_value = run.get_secret(name="mysecret")
```

Achten Sie darauf, dass Sie den Geheimniswert nicht dadurch verfügbar machen, dass Sie ihn schreiben oder drucken.

Für die set- und die get-Methode gibt es auch die Batchversionen [set_secrets](https://docs.microsoft.com/python/api/azureml-core/azureml.core.keyvault.keyvault?view=azure-ml-py#set-secrets-secrets-batch-) und [get_secrets](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#get-secrets-secrets-), mit denen gleichzeitig auf mehrere Geheimnisse zugegriffen werden kann.

## <a name="next-steps"></a>Nächste Schritte

 * [Beispielnotebook anzeigen](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/manage-azureml-service/authentication-in-azureml/authentication-in-azureml.ipynb)
 * [Informationen über Unternehmenssicherheit mit Azure Machine Learning](concept-enterprise-security.md)
