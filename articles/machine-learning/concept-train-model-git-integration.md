---
title: Git-Integration für Azure Machine Learning
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Azure Machine Learning in ein lokales Git-Repository integriert werden kann. Beim Übermitteln einer Trainingsausführung aus einem lokalen Verzeichnis, bei dem es sich um ein Git-Repository handelt, werden im Rahmen der Ausführung Informationen zum Repository, zum Branch und zum aktuellen Commit nachverfolgt.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.date: 03/05/2020
ms.openlocfilehash: 154d28e4df9bad68f3c5e93208ccf62ba2721663
ms.sourcegitcommit: d7352c07708180a9293e8a0e7020b9dd3dd153ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2020
ms.locfileid: "89144820"
---
# <a name="git-integration-for-azure-machine-learning"></a>Git-Integration für Azure Machine Learning

[Git](https://git-scm.com/) ist ein beliebtes Versionskontrollsystem, mit dem Sie Projekte freigeben und gemeinsam mit anderen an ihnen arbeiten können. 

Azure Machine Learning unterstützt Git-Repositorys zur Nachverfolgung der Arbeit – Sie können Repositorys direkt für Ihr freigegebenes Arbeitsbereichsdateisystem klonen, Git auf Ihrer lokalen Arbeitsstation verwenden oder Git über eine CI/CD-Pipeline verwenden.

Wenn beim Übermitteln eines Auftrags an Azure Machine Learning die Quelldateien in einem lokalen Git-Repository gespeichert sind, werden Informationen über das Repository als Teil des Trainingsprozesses nachverfolgt.

Da Azure Machine Learning Informationen aus einem lokalen Git-Repository nachverfolgt, ist es nicht an ein spezifisches zentrales Repository gebunden. Ihr Repository kann von GitHub, GitLab, Bitbucket, Azure DevOps oder einem beliebigen anderen, mit Git kompatiblen Dienst geklont sein.

## <a name="clone-git-repositories-into-your-workspace-file-system"></a>Klonen von Git-Respositorys für das Arbeitsbereichsdateisystem
Azure Machine Learning bietet ein gemeinsames Dateisystem für alle Benutzer im Arbeitsbereich.
Um ein Git-Repository für diese Dateifreigabe zu klonen, empfehlen wir Ihnen, eine Compute-Instanz zu erstellen und ein Terminal zu öffnen.
Nachdem das Terminal geöffnet ist, haben Sie Zugriff auf einen vollständigen Git-Client und können über die Git-CLI-Oberfläche entsprechend Git klonen und mit Git arbeiten.

Es wird empfohlen, dass Sie das Repository in das Verzeichnis Ihrer Benutzer klonen, damit andere keine Konflikte direkt in Ihrem Arbeitsbranch verursachen.

Sie können ein beliebiges Git-Repository klonen, bei dem Sie sich authentifizieren können (GitHub, Azure Repos, BitBucket, usw.)

Einen Leitfaden zur Verwendung der Git-CLI finden Sie [hier](https://guides.github.com/introduction/git-handbook/).

## <a name="track-code-that-comes-from-git-repositories"></a>Nachverfolgen von Code, der aus Git-Repositorys stammt

Wenn Sie einen Trainingslauf vom Python SDK oder der Machine Learning-CLI übermitteln, werden die zum Trainieren des Modells erforderlichen Dateien in Ihren Arbeitsbereich hochgeladen. Wenn der `git`-Befehl in Ihrer Entwicklungsumgebung verfügbar ist, wird er vom Uploadvorgang verwendet, um zu überprüfen, ob die Dateien in einem Git-Repository gespeichert sind. Ist dies der Fall, werden die Informationen aus Ihrem Git-Repository ebenfalls als Bestandteil des Trainingslaufs hochgeladen. Diese Informationen sind in den folgenden Eigenschaften für den Trainingslauf gespeichert:

| Eigenschaft | Git-Befehl zum Abrufen des Werts | BESCHREIBUNG |
| ----- | ----- | ----- |
| `azureml.git.repository_uri` | `git ls-remote --get-url` | Der URI, von dem Ihr Repository geklont wurde. |
| `mlflow.source.git.repoURL` | `git ls-remote --get-url` | Der URI, von dem Ihr Repository geklont wurde. |
| `azureml.git.branch` | `git symbolic-ref --short HEAD` | Der aktive Branch bei der Übermittlung des Laufs. |
| `mlflow.source.git.branch` | `git symbolic-ref --short HEAD` | Der aktive Branch bei der Übermittlung des Laufs. |
| `azureml.git.commit` | `git rev-parse HEAD` | Der Commithash des Codes, der für den Lauf übermittelt wurde. |
| `mlflow.source.git.commit` | `git rev-parse HEAD` | Der Commithash des Codes, der für den Lauf übermittelt wurde. |
| `azureml.git.dirty` | `git status --porcelain .` | `True`, wenn der Branch/Commit geändert wurde („dirty“), andernfalls `false`. |

Diese Informationen werden für Läufe gesendet, die einen Estimator, eine Machine Learning-Pipeline oder eine Skriptausführung verwenden.

Wenn Ihre Trainingsdateien nicht in einem Git-Repository in Ihrer Entwicklungsumgebung gespeichert sind oder der `git`-Befehl nicht verfügbar ist, werden keine Git-bezogenen Informationen nachverfolgt.

> [!TIP]
> Um zu überprüfen, ob der Git-Befehl in Ihrer Entwicklungsumgebung verfügbar ist, öffnen Sie eine Shellsitzung, eine Eingabeaufforderung, eine PowerShell oder eine andere Befehlszeilenschnittstelle, und geben Sie den folgenden Befehl ein:
>
> ```
> git --version
> ```
>
> Sofern installiert und im Pfad enthalten, erhalten Sie eine Antwort wie `git version 2.4.1`. Weitere Informationen zur Installation von Git in der Entwicklungsumgebung finden Sie auf der [Git-Website](https://git-scm.com/).

## <a name="view-the-logged-information"></a>Anzeigen der protokollierten Informationen

Die Git-Informationen sind in den Eigenschaften für einen Trainingslauf gespeichert. Sie können diese Informationen mithilfe von Azure-Portal, Python SDK und CLI anzeigen. 

### <a name="azure-portal"></a>Azure-Portal

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) Ihren Arbeitsbereich aus.
1. Wählen Sie __Experimente__ und dann eins Ihrer Experimente aus.
1. Wählen Sie einen der Läufe in der Spalte __Ausführungsanzahl__ aus.
1. Wählen Sie __Protokolle__ aus, und erweitern Sie dann die Einträge __logs__ und __azureml__. Wählen Sie den Link aus, der mit __###\_azure__ beginnt.

    ![Eintrag „###_azure“ im Portal](./media/concept-train-model-git-integration/azure-machine-learning-logs.png)

Die protokollierten Informationen enthalten Text, der so ähnlich aussieht wie der folgende JSON-Code:

```json
"properties": {
    "_azureml.ComputeTargetType": "batchai",
    "ContentSnapshotId": "5ca66406-cbac-4d7d-bc95-f5a51dd3e57e",
    "azureml.git.repository_uri": "git@github.com:azure/machinelearningnotebooks",
    "mlflow.source.git.repoURL": "git@github.com:azure/machinelearningnotebooks",
    "azureml.git.branch": "master",
    "mlflow.source.git.branch": "master",
    "azureml.git.commit": "4d2b93784676893f8e346d5f0b9fb894a9cf0742",
    "mlflow.source.git.commit": "4d2b93784676893f8e346d5f0b9fb894a9cf0742",
    "azureml.git.dirty": "True",
    "AzureML.DerivedImageName": "azureml/azureml_9d3568242c6bfef9631879915768deaf",
    "ProcessInfoFile": "azureml-logs/process_info.json",
    "ProcessStatusFile": "azureml-logs/process_status.json"
}
```

### <a name="python-sdk"></a>Python SDK

Nach dem Übermitteln eines Trainingslaufs wird ein [Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py)-Objekt zurückgegeben. Das `properties`-Attribut dieses Objekts enthält die protokollierten Git-Informationen. Beispielsweise ruft der folgende Code den Commithash ab:

```python
run.properties['azureml.git.commit']
```

### <a name="cli"></a>Befehlszeilenschnittstelle (CLI)

Der CLI-Befehl `az ml run` kann verwendet werden, um die Eigenschaften eines Laufs abzurufen. Beispielsweise gibt der folgende Befehl die Eigenschaften für den letzten Lauf im Experiment mit dem Namen `train-on-amlcompute` zurück:

```azurecli-interactive
az ml run list -e train-on-amlcompute --last 1 -w myworkspace -g myresourcegroup --query '[].properties'
```

Weitere Informationen finden Sie in der Referenzdokumentation zu [az ml run](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest).

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden von Computeziele für das Modelltraining](how-to-set-up-training-targets.md)
