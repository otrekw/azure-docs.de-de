---
title: Fortlaufendes Bereitstellen von Azure Machine Learning-Modellen
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie Modelle mit der Azure Machine Learning-DevOps-Erweiterung fortlaufend bereitstellen. Sie können automatisch auf neue Modellversionen prüfen und diese bereitstellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: gopalv
author: gvashishtha
ms.date: 08/03/2020
ms.topic: conceptual
ms.reviewer: larryfr
ms.custom: how-to, tracking-python, deploy
ms.openlocfilehash: 9de971639e22f9656ea75dc64993ac5881efbffb
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102609412"
---
# <a name="continuously-deploy-models"></a>Fortlaufendes Bereitstellen von Modellen

In diesem Artikel erfahren Sie, wie Sie mithilfe von Continuous Deployment (CD) in Azure DevOps automatisch nach neuen Versionen registrierter Modelle suchen und diese neuen Modelle in die Produktion verschieben.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie bereits ein Modell in Ihrem Azure Machine Learning-Arbeitsbereich registriert haben. In [diesem Tutorial](how-to-train-scikit-learn.md) finden Sie ein Beispiel für das Trainieren und Registrieren eines Scikit-learn-Modells.

## <a name="continuously-deploy-models"></a>Fortlaufendes Bereitstellen von Modellen

Sie können Modelle fortlaufend bereitstellen, indem Sie die Machine Learning-Erweiterung für [Azure DevOps](https://azure.microsoft.com/services/devops/) verwenden. Sie können die Machine Learning-Erweiterung für Azure DevOps verwenden, um eine Bereitstellungspipeline auszulösen, wenn ein neues Machine Learning-Modell in einem Azure Machine Learning-Arbeitsbereich registriert wurde.

1. Registrieren Sie sich für [Azure Pipelines](/azure/devops/pipelines/get-started/pipelines-sign-up), was eine kontinuierliche Integration und Bereitstellung Ihrer Anwendung auf jeder Plattform oder in jeder Cloud ermöglicht. (Beachten Sie, dass Azure Pipelines nicht mit [Machine Learning-Pipelines](concept-ml-pipelines.md#compare) identisch ist.)

1. [Erstellen eines Azure DevOps-Projekts.](/azure/devops/organizations/projects/create-project)

1. Installieren Sie die [Machine Learning-Erweiterung für Azure Pipelines](https://marketplace.visualstudio.com/items?itemName=ms-air-aiagility.vss-services-azureml&targetId=6756afbe-7032-4a36-9cb6-2771710cadc2&utm_source=vstsproduct&utm_medium=ExtHubManageList).

1. Richten Sie mit Dienstverbindungen eine Dienstprinzipalverbindung mit Ihrem Azure Machine Learning-Arbeitsbereich ein, damit Sie auf Ihre Artefakte zugreifen können. Wechseln Sie zu „Projekteinstellungen“, wählen Sie **Dienstverbindungen** aus, und wählen Sie dann **Azure Resource Manager** aus:

    [![Azure Resource Manager auswählen](media/how-to-deploy-and-where/view-service-connection.png)](media/how-to-deploy-and-where/view-service-connection-expanded.png)

1. Wählen Sie in der Liste **Bereichsebene** die Option **AzureMLWorkspace** aus, und geben Sie dann die restlichen Werte ein:

    ![AzureMLWorkspace auswählen](media/how-to-deploy-and-where/resource-manager-connection.png)

1. Um Ihr Machine Learning-Modell kontinuierlich über Azure Pipelines bereitzustellen, wählen Sie unter „Pipelines“ die Option **Freigabe** (release) aus. Fügen Sie ein neues Artefakt hinzu, und wählen Sie dann das **AzureML Model**-Artefakt und die Dienstverbindung aus, die Sie zuvor erstellt haben. Wählen Sie das Modell und die Version aus, um eine Bereitstellung auszulösen:

    [![AzureML Model auswählen](media/how-to-deploy-and-where/enable-modeltrigger-artifact.png)](media/how-to-deploy-and-where/enable-modeltrigger-artifact-expanded.png)

1. Aktivieren Sie den Modelltrigger auf Ihrem Modellartefakt. Wenn Sie den Trigger aktivieren, wird jedes Mal, wenn die angegebene Version (dies ist die neueste Version) dieses Modells in Ihrem Arbeitsbereich registriert wird, eine Azure DevOps-Releasepipeline ausgelöst.

    [![Modelltrigger aktivieren](media/how-to-deploy-and-where/set-modeltrigger.png)](media/how-to-deploy-and-where/set-modeltrigger-expanded.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele für Continuous Deployment für ML-Modelle finden Sie in den folgenden Projekten auf GitHub.

* [Microsoft/MLOps](https://github.com/Microsoft/MLOps)
* [Microsoft/MLOpsPython](https://github.com/microsoft/MLOpsPython)