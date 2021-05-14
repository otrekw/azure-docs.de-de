---
title: Erstellen eines verknüpften Diensts mit Synapse- und Azure Machine Learning-Arbeitsbereichen (Vorschauversion)
titleSuffix: Azure Machine Learning
description: Es wird beschrieben, wie Sie Azure Synapse- und Azure Machine Learning-Arbeitsbereiche verknüpfen, um eine Benutzeroberfläche mit einheitlichem Data Wrangling zu erhalten.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.author: nibaccam
author: nibaccam
ms.reviewer: nibaccam
ms.date: 03/08/2021
ms.custom: devx-track-python, data4ml, synapse-azureml
ms.openlocfilehash: 3ee5986fbe92d7e585a7c9f2f50bdeceb132c268
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108131384"
---
# <a name="link-azure-synapse-analytics-and-azure-machine-learning-workspaces-preview"></a>Verknüpfen von Azure Synapse Analytics- und Azure Machine Learning-Arbeitsbereichen (Vorschauversion)

In diesem Artikel wird beschrieben, wie Sie einen verknüpften Dienst erstellen, mit dem Ihr [Azure Synapse Analytics-Arbeitsbereich](../synapse-analytics/overview-what-is.md) und [Azure Machine Learning-Arbeitsbereich](concept-workspace.md) verknüpft werden.

Nachdem Ihr Azure Machine Learning-Arbeitsbereich mit Ihrem Azure Synapse-Arbeitsbereich verknüpft wurde, können Sie einen Apache Spark-Pool als dedizierte Compute-Umgebung für Data Wrangling im großen Stil anfügen oder das Modelltraining über dasselbe Python-Notebook durchführen.

Sie können Ihren ML-Arbeitsbereich und den Synapse-Arbeitsbereich über das [Python SDK](#link-sdk) oder [Azure Machine Learning Studio](#link-studio) verknüpfen.

Sie können zum Verknüpfen von Arbeitsbereichen und Anfügen eines Synapse Spark-Pools auch eine [ARM-Vorlage (Azure Resource Manager)](https://github.com/Azure/azure-quickstart-templates/blob/master/101-machine-learning-linkedservice-create/azuredeploy.json) verwenden.

>[!IMPORTANT]
> Die Integration von Azure Machine Learning und Azure Synapse befindet sich in der öffentlichen Vorschauphase. Die Funktionen des Pakets `azureml-synapse` sind [experimentelle](/python/api/overview/azure/ml/#stable-vs-experimental) Previewfunktionen, die sich im Laufe der Zeit ändern können.

## <a name="prerequisites"></a>Voraussetzungen

* [Erstellen Sie einen Azure Machine Learning-Arbeitsbereich](how-to-manage-workspace.md?tabs=python).

* [Erstellen eines Synapse-Arbeitsbereichs im Azure-Portal](../synapse-analytics/quickstart-create-workspace.md)

* [Erstellen eines Apache Spark-Pools über das Azure-Portal, Webtools oder Synapse Studio](../synapse-analytics/quickstart-create-apache-spark-pool-studio.md)

* Installieren des [Python SDK für Azure Machine Learning](/python/api/overview/azure/ml/intro)

* Zugriff auf [Azure Machine Learning Studio](https://ml.azure.com/)

<a name="link-sdk"></a>
## <a name="link-workspaces-with-the-python-sdk"></a>Verknüpfen von Arbeitsbereichen mit dem Python SDK

> [!IMPORTANT]
> Um erfolgreich eine Verknüpfung mit dem Synapse-Arbeitsbereich herzustellen, benötigen Sie die Rolle **Besitzer** im Synapse-Arbeitsbereich. Überprüfen Sie Ihren Zugriff im [Azure-Portal](https://ms.portal.azure.com/).
>
> Wenn Sie kein **Besitzer**, sondern nur ein **Mitwirkender** für den Synapse-Arbeitsbereich sind, können Sie nur vorhandene verknüpfte Dienste verwenden. Informieren Sie sich über das [Abrufen und Verwenden eines vorhandenen verknüpften Diensts](how-to-data-prep-synapse-spark-pool.md#get-an-existing-linked-service).

Der folgende Code verwendet die Klassen [`LinkedService`](/python/api/azureml-core/azureml.core.linked_service.linkedservice) und [`SynapseWorkspaceLinkedServiceConfiguration`](/python/api/azureml-core/azureml.core.linked_service.synapseworkspacelinkedserviceconfiguration).

* Verknüpfen Sie Ihren Machine Learning-Arbeitsbereich `ws` mit dem Azure Synapse-Arbeitsbereich.
* Registrieren Sie Ihren Synapse-Arbeitsbereich als verknüpften Dienst bei Azure Machine Learning.

``` python
import datetime  
from azureml.core import Workspace, LinkedService, SynapseWorkspaceLinkedServiceConfiguration

# Azure Machine Learning workspace
ws = Workspace.from_config()

#link configuration 
synapse_link_config = SynapseWorkspaceLinkedServiceConfiguration(
    subscription_id=ws.subscription_id,
    resource_group= 'your resource group',
    name='mySynapseWorkspaceName')

# Link workspaces and register Synapse workspace in Azure Machine Learning
linked_service = LinkedService.register(workspace = ws,              
                                            name = 'synapselink1',    
                                            linked_service_config = synapse_link_config)
```

> [!IMPORTANT] 
> Eine verwaltete Identität (`system_assigned_identity_principal_id`) wird für jeden verknüpften Dienst erstellt. Dieser verwalteten Identität muss die Rolle **Synapse Apache Spark-Administrator** des Synapse-Arbeitsbereichs zugewiesen werden, bevor Sie die Synapse-Sitzung starten. [Weisen Sie die Rolle „Synapse Apache Spark-Administrator“ der verwalteten Identität in Synapse Studio zu.](../synapse-analytics/security/how-to-manage-synapse-rbac-role-assignments.md)
>
> Verwenden Sie `LinkedService.get('<your-mlworkspace-name>', '<linked-service-name>')` zum Ermitteln von `system_assigned_identity_principal_id` eines bestimmten verknüpften Diensts.

### <a name="manage-linked-services"></a>Verwalten von verknüpften Diensten

Zeigen Sie alle verknüpften Dienste an, die Ihrem Machine Learning-Arbeitsbereich zugeordnet sind.

```python
LinkedService.list(ws)
```

Verwenden Sie die Methode `unregister()`, um die Verknüpfung von Arbeitsbereichen aufzuheben.

``` python
linked_service.unregister()
```

<a name="link-studio"></a>
## <a name="link-workspaces-via-studio"></a>Verknüpfen von Arbeitsbereichen über Studio

Verknüpfen Sie mit den folgenden Schritten den Machine Learning-Arbeitsbereich und den Synapse-Arbeitsbereich über Azure Machine Learning Studio: 

1. Melden Sie sich bei [Azure Machine Learning Studio](https://ml.azure.com/) an.
1. Wählen Sie im linken Bereich im Abschnitt **Verwalten** die Option **Verknüpfte Dienste** aus.
1. Wählen Sie **Integration hinzufügen** aus.
1. Füllen Sie die Felder im Formular **Arbeitsbereich verknüpfen** aus.

    |Feld| BESCHREIBUNG    
    |---|---
    |Name| Geben Sie einen Namen für Ihren verknüpften Dienst ein. Dieser Name wird verwendet, um auf diesen bestimmten verknüpften Dienst zu verweisen.
    |Abonnementname | Wählen Sie den Namen Ihres Abonnements aus, das Ihrem Machine Learning-Arbeitsbereich zugeordnet ist. 
    |Synapse-Arbeitsbereich | Wählen Sie den Synapse-Arbeitsbereich aus, mit dem Sie eine Verknüpfung herstellen möchten.
    
1. Wählen Sie **Weiter** aus, um das Formular **Spark-Pools auswählen (optional)** zu öffnen. Wählen Sie in diesem Formular aus, welcher Synapse Spark-Pool an Ihren Arbeitsbereich angefügt werden soll.

1. Wählen Sie **Weiter** aus, um das Formular **Überprüfung** zu öffnen und Ihre Auswahl zu überprüfen.
1. Wählen Sie **Erstellen** aus, um die Erstellung des verknüpften Diensts abzuschließen.

## <a name="next-steps"></a>Nächste Schritte

* [Anfügen von Synapse Spark-Pools für die Datenaufbereitung mit Azure Synapse (Vorschau)](how-to-data-prep-synapse-spark-pool.md)
* [Verwenden von Apache Spark in Ihrer Machine Learning-Pipeline mit Azure Synapse (Vorschau)](how-to-use-synapsesparkstep.md)
* [Trainieren eines Modells](how-to-set-up-training-targets.md).