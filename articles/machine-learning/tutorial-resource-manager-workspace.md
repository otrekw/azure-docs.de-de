---
title: 'Tutorial: Erstellen eines Azure ML-Arbeitsbereichs mithilfe einer Resource Manager-Vorlage'
description: In diesem Tutorial verwenden Sie eine Azure Resource Manager-Vorlage, um schnell einen Azure-Arbeitsbereich für Machine Learning bereitzustellen
services: machine-learning
author: lobrien
ms.author: laobri
ms.custom: subject-armqs
ms.date: 05/26/2020
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.openlocfilehash: 760406b738d2aa95ef086941850814f4bf39fbb4
ms.sourcegitcommit: 628be49d29421a638c8a479452d78ba1c9f7c8e4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88639867"
---
# <a name="tutorial-deploy-an-azure-machine-learning-workspace-using-an-arm-template"></a>Tutorial: Bereitstellen eines Azure Machine Learning-Arbeitsbereichs mithilfe einer ARM-Vorlage

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Tutorial erfahren Sie, wie ein Azure Machine Learning-Arbeitsbereich mithilfe einer ARM-Vorlage (Azure Resource Manager) erstellt wird. Azure Machine Learning-Arbeitsbereiche organisieren alle Ihre Ressourcen für Machine Learning von Baselinedatasets bis hin zu bereitgestellten Modellen. Arbeitsbereiche stellen einen einzelnen Ort für die Zusammenarbeit mit Kollegen beim Erstellen, Ausführen und Überprüfen von Experimenten, das Verwalten Ihrer Computeressourcen für Training und Rückschluss und das Überwachen und die Versionsverwaltung bereitgestellter Modelle dar.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Wenn Ihre Umgebung die Voraussetzungen erfüllt und Sie mit der Verwendung von ARM-Vorlagen vertraut sind, klicken Sie auf die Schaltfläche **In Azure bereitstellen**. Die Vorlage wird im Azure-Portal geöffnet.

[![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-machine-learning-create%2Fazuredeploy.json)

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/services/machine-learning/) erstellen, bevor Sie beginnen.

* Um die CLI-Befehle in diesem Dokument aus Ihrer **lokalen Umgebung** zu verwenden, benötigen Sie die [Azure CLI](/cli/azure/install-azure-cli).

## <a name="review-the-template"></a>Überprüfen der Vorlage

Die in dieser Schnellstartanleitung verwendete Vorlage stammt von der Seite mit den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/101-machine-learning-create/).

:::code language="json" source="~/quickstart-templates/101-machine-learning-create/azuredeploy.json":::

Die folgenden Ressourcen sind in der Vorlage definiert:

* [Microsoft.MachineLearningServices/workspaces](/azure/templates/microsoft.machinelearningservices/workspaces): Erstellen eines Azure ML-Arbeitsbereichs In dieser Vorlage sind Speicherort und Name Parameter, die der Benutzer einlesen oder interaktiv eingeben kann.

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Um die Vorlage an der Azure-Befehlszeilenschnittstelle zu verwenden, melden Sie sich an, und wählen Sie Ihr Abonnement aus (Siehe dazu [Anmelden bei Azure CLI](/cli/azure/authenticate-azure-cli)). Führen Sie dann Folgendes aus:

```azurecli-interactive
read -p "Enter a project name that is used for generating resource names:" projectName &&
read -p "Enter the location (i.e. centralus):" location &&
templateUri="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-machine-learning-create/azuredeploy.json" &&
resourceGroupName="${projectName}rg" &&
workspaceName="${projectName}ws" &&
az group create --name $resourceGroupName --location "$location" &&
az deployment group create --resource-group $resourceGroupName --template-uri $templateUri --parameters workspaceName=$workspaceName location=$location &&
echo "Press [ENTER] to continue ..." &&
read
```

Wenn Sie den Befehl oben ausführen, geben Sie dies ein:

1. Einen Projektnamen, der die Grundlage der Namen der erstellten Ressourcengruppe und des Azure ML-Arbeitsbereichs bildet
1. Den Azure-Standort, an dem die Bereitstellung erfolgen soll

## <a name="review-deployed-resources"></a>Überprüfen der bereitgestellten Ressourcen

So zeigen Sie Ihren Azure ML-Arbeitsbereich an:

1. Gehe zu https://portal.azure.com.
1. Melden Sie sich an.
1. Wählen Sie den soeben erstellten Arbeitsbereich aus.

Die Azure Machine Learning-Startseite wird angezeigt:

:::image type="content" source="media/tutorial-resource-manager-workspace/workspace-home.png" alt-text="Screenshot des Azure ML-Arbeitsbereichs":::

Um alle der Bereitstellung zugeordneten Ressourcen anzuzeigen, klicken Sie oben links auf den Link mit dem Arbeitsbereichsnamen (im Screenshot `my_templated_ws`). Mit diesem Link gelangen Sie zur Ressourcengruppe im Azure-Portal. Der Name der Ressourcengruppe ist `{projectName}rg`, und der Name des Arbeitsbereichs `{projectName}ws`.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie diesen Arbeitsbereich nicht verwenden möchten, löschen Sie ihn. Da der Arbeitsbereich weiteren Ressourcen, wie etwa einem Speicherkonto, zugeordnet ist, ist es wohl sinnvoll, die gesamte erstellte Ressourcengruppe zu löschen. Sie können die Ressourcengruppe im Portal löschen, indem Sie auf die Schaltfläche **Löschen** klicken und den Vorgang bestätigen. Alternativ können Sie die Ressourcengruppe an der Befehlszeilenschnittstelle löschen, mit diesem Befehl:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen Azure Machine Learning-Arbeitsbereich auf der Grundlage einer ARM-Vorlage erstellt. Wenn Sie Azure Machine Learning erkunden möchten, fahren Sie mit dem Tutorial fort.

> [!div class="nextstepaction"]
> [Tutorial: Erste Schritte beim Erstellen Ihres ersten ML-Experiments mit dem Python SDK](tutorial-1st-experiment-sdk-setup.md)
