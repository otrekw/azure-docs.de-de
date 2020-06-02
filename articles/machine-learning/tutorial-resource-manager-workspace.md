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
ms.openlocfilehash: b6eac711ed6c420330b036cb6094c841508cee9f
ms.sourcegitcommit: a9784a3fd208f19c8814fe22da9e70fcf1da9c93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/22/2020
ms.locfileid: "83790715"
---
# <a name="tutorial-deploy-an-azure-machine-learning-workspace-using-a-resource-manager-template"></a>Tutorial: Bereitstellen eines Azure Machine Learning-Arbeitsbereichs mithilfe einer Resource Manager-Vorlage
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Tutorial erfahren Sie, wie ein Azure Machine Learning-Arbeitsbereich mithilfe einer Azure Resource Manager-Vorlage erstellt wird. Azure Machine Learning-Arbeitsbereiche organisieren alle Ihre Ressourcen für Machine Learning von Baselinedatasets bis hin zu bereitgestellten Modellen. Arbeitsbereiche stellen einen einzelnen Ort für die Zusammenarbeit mit Kollegen beim Erstellen, Ausführen und Überprüfen von Experimenten, das Verwalten Ihrer Computeressourcen für Training und Rückschluss und das Überwachen und die Versionsverwaltung bereitgestellter Modelle dar.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://aka.ms/AMLFree), bevor Sie beginnen

* Um die CLI-Befehle in diesem Dokument in Ihrer **lokalen Umgebung** zu verwenden, benötigen Sie die [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)

## <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs

### <a name="review-the-template"></a>Überprüfen der Vorlage

Die in dieser Schnellstartanleitung verwendete Vorlage stammt von der Seite mit den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/101-machine-learning-create/).

:::code language="json" source="~/quickstart-templates/101-machine-learning-create/azuredeploy.json":::

Die folgenden Ressourcen sind in der Vorlage definiert:

* [Microsoft.MachineLearningServices/workspaces](/azure/templates/microsoft.machinelearningservices/workspaces): Erstellen eines Azure ML-Arbeitsbereichs In dieser Vorlage sind Speicherort und Name Parameter, die der Benutzer einlesen oder interaktiv eingeben kann.

### <a name="deploy-the-template"></a>Bereitstellen der Vorlage 

Um die Vorlage an der Azure-Befehlszeilenschnittstelle zu verwenden, melden Sie sich an, und wählen Sie Ihr Abonnement aus (Siehe dazu [Anmelden bei Azure CLI](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest)). Führen Sie dann Folgendes aus:

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

1. Einen Projektnamen, der die Grundlage der Namen der erstellten Ressourcengruppe und des Azure ML-Arbeitsbereichs bildet
1. Den Azure-Standort, an dem die Bereitstellung erfolgen soll

## <a name="review-deployed-resources"></a>Überprüfen der bereitgestellten Ressourcen

So zeigen Sie Ihren Azure ML-Arbeitsbereich an:

1. Besuchen Sie https://portal.azure.com. 
1. Anmelden 
1. Wählen Sie den soeben erstellten Arbeitsbereich aus

Die Azure Machine Learning-Startseite wird angezeigt: 

:::image type="content" source="media/tutorial-resource-manager-workspace/workspace-home.png" alt-text="Screenshot des Azure ML-Arbeitsbereichs":::

Um alle der Bereitstellung zugeordneten Ressourcen anzuzeigen, klicken Sie oben links auf den Link mit dem Arbeitsbereichsnamen (im Screenshot `my_templated_ws`). Mit diesem Link gelangen Sie zur Ressourcengruppe im Azure-Portal. Der Name der Ressourcengruppe ist `{projectName}rg`, und der Name des Arbeitsbereichs `{projectName}ws`.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie diesen Arbeitsbereich nicht verwenden möchten, löschen Sie ihn. Da der Arbeitsbereich weiteren Ressourcen, wie etwa einem Speicherkonto, zugeordnet ist, ist es wohl sinnvoll, die gesamte erstellte Ressourcengruppe zu löschen. Sie können die Ressourcengruppe im Portal löschen, indem Sie auf die Schaltfläche „Löschen“ klicken und den Befehl bestätigen. Alternativ können Sie die Ressourcengruppe an der Befehlszeilenschnittstelle löschen, mit diesem Befehl: 

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen Azure Machine Learning-Arbeitsbereich aus einer Azure Resource Manager-Vorlage erstellt. Wenn Sie Azure Machine Learning erkunden möchten, fahren Sie mit dem Tutorial fort. 

> [!div class="nextstepaction"]
> [Tutorial: Erste Schritte beim Erstellen Ihres ersten ML-Experiments mit dem Python SDK](tutorial-1st-experiment-sdk-setup.md)
