---
title: Installieren, Einrichten und Verwenden der 2.0 CLI
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie die Azure CLI-Erweiterung für Machine Learning installieren, einrichten und verwenden.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
author: lostmygithubaccount
ms.author: copeters
ms.date: 05/25/2021
ms.reviewer: laobri
ms.openlocfilehash: 19534a5b55ea666233bb1a9b66916791e934908a
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110476611"
---
# <a name="install-set-up-and-use-the-20-cli-preview"></a>Installieren, Einrichten und Verwenden der 2.0 CLI (Vorschau)

Die Erweiterung `ml` (Vorschau) für die [Azure CLI](/cli/azure/) ist die Schnittstelle der nächsten Generation für Azure Machine Learning. Hiermit können Sie Modelle über die Befehlszeile trainieren und bereitstellen und Features nutzen, mit denen beim Nachverfolgen des Modelllebenszyklus das Hoch- und Aufskalieren von Data Science-Einheiten beschleunigt werden kann.

[!INCLUDE [preview disclaimer](../../includes/machine-learning-preview-generic-disclaimer.md)]

## <a name="prerequisites"></a>Voraussetzungen

- Für die Verwendung der CLI benötigen Sie ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) noch heute aus.
- Um die CLI-Befehle in diesem Dokument aus Ihrer **lokalen Umgebung** zu verwenden, benötigen Sie die [Azure CLI](/cli/azure/install-azure-cli).

    > [!TIP]
    > Wenn Sie die [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/) verwenden, befindet sich die CLI in der Cloud, und der Zugriff erfolgt über den Browser.

## <a name="installation"></a>Installation

Für die neue Machine Learning-Erweiterung **ist die Azure CLI-Version `>=2.15.0` erforderlich**. Stellen Sie sicher, dass diese Anforderung erfüllt ist:

```azurecli
az version
```

Ist dies nicht der Fall, [aktualisieren Sie Ihre Azure CLI](/cli/azure/update-azure-cli).

Überprüfen Sie die installierten Azure CLI-Erweiterungen:

:::code language="azurecli" source="~/azureml-examples-main/cli/how-to-configure-cli.sh" id="az_extension_list":::

Stellen Sie sicher, dass keine in Konflikt stehende Erweiterung mit dem Namespace `ml` installiert ist, einschließlich der Erweiterung `azure-cli-ml`:

:::code language="azurecli" source="~/azureml-examples-main/cli/how-to-configure-cli.sh" id="az_extension_remove":::

Installieren Sie nun die Erweiterung `ml`:

:::code language="azurecli" source="~/azureml-examples-main/cli/setup.sh" id="az_ml_install":::

Führen Sie den Hilfebefehl aus, um Ihre Installation zu überprüfen und verfügbare Unterbefehle anzuzeigen:

:::code language="azurecli" source="~/azureml-examples-main/cli/how-to-configure-cli.sh" id="az_ml_verify":::

Sie können die Erweiterung auf die neueste Version aktualisieren:

:::code language="azurecli" source="~/azureml-examples-main/cli/how-to-configure-cli.sh" id="az_ml_update":::

## <a name="set-up"></a>Einrichten

Anmeldung:

```azurecli
az login
```

Wenn Sie Zugriff auf mehrere Azure-Abonnements haben, können Sie Ihr aktives Abonnement festlegen:

:::code language="azurecli" source="~/azureml-examples-main/cli/how-to-configure-cli.sh" id="az_account_set":::

Ist noch keine Azure-Ressourcengruppe vorhanden, können Sie sie erstellen:

:::code language="azurecli" source="~/azureml-examples-main/cli/setup.sh" id="az_group_create":::

Für Machine Learning-Unterbefehle sind die Parameter `--workspace/-w` und `--resource-group/-g` erforderlich. Konfigurieren Sie Standardwerte, um diese nicht wiederholt eingeben zu müssen:

:::code language="azurecli" source="~/azureml-examples-main/cli/setup.sh" id="az_configure_defaults":::

> [!TIP]
> Bei den meisten Codebeispielen wird davon ausgegangen, dass Sie einen Standardarbeitsbereich und eine Standardressourcengruppe festgelegt haben. Sie können diese über die Befehlszeile überschreiben.

Erstellen Sie nun den Arbeitsbereich für maschinelles Lernen:

:::code language="azurecli" source="~/azureml-examples-main/cli/setup.sh" id="az_ml_workspace_create":::

## <a name="hello-world"></a>Hello World

Klonen Sie zum Ausführen der Schritte das Repository mit den Beispielen, und wechseln Sie in das Unterverzeichnis `cli`:

```azurecli-interactive
git clone https://github.com/Azure/azureml-examples --depth 1
cd azureml-examples/cli
```

Informationen zum lokalen Ausführen von „Hallo Welt“ über Python finden Sie im Beispiel im Unterverzeichnis `jobs`:

:::code language="yaml" source="~/azureml-examples-main/cli/jobs/hello-world.yml":::

> [!IMPORTANT]
> [Docker](https://docker.io) muss lokal installiert und ausgeführt werden.

Übermitteln Sie den Auftrag. Dabei werden die Protokolle an die Konsolenausgabe gestreamt und die Ausführung in Azure Machine Learning Studio geöffnet:

:::code language="azurecli" source="~/azureml-examples-main/cli/how-to-configure-cli.sh" id="hello_world":::

> [!IMPORTANT]
> Die erste Ausführung kann einige Minuten dauern, da das Docker-Image lokal gepullt und der Azure ML-Auftrag ausgeführt wird. Bei nachfolgenden Ausführungen wird das Image lokal zwischengespeichert und schneller abgeschlossen.

## <a name="next-steps"></a>Nächste Schritte

- [Einrichten der Azure Machine Learning-Erweiterung für Visual Studio Code](how-to-setup-vs-code.md)
- [Trainieren von Modellen mithilfe der Machine Learning-CLI-Erweiterung (Vorschau)](how-to-train-cli.md)
- [Trainieren eines TensorFlow-Modells für die Bildklassifizierung mit der Azure Machine Learning-Erweiterung für Visual Studio Code](tutorial-train-deploy-image-classification-model-vscode.md)
