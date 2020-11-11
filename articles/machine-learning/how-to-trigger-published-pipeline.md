---
title: Auslösen einer Machine Learning-Pipeline für neue Daten
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie die Ausführung einer Machine Learning-Pipeline mithilfe von Azure Logic Apps auslösen.
services: machine-learning
author: NilsPohlmann
ms.author: nilsp
ms.service: machine-learning
ms.subservice: core
ms.workload: data-services
ms.date: 02/07/2020
ms.topic: conceptual
ms.custom: how-to, contperfq4
ms.openlocfilehash: 64f1c83a570e936759d674f40db201fb2f2cd0e5
ms.sourcegitcommit: 4b76c284eb3d2b81b103430371a10abb912a83f4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2020
ms.locfileid: "93146261"
---
# <a name="trigger-a-run-of-a-machine-learning-pipeline-from-a-logic-app"></a>Auslösen der Ausführung einer Machine Learning-Pipeline aus einer Logik-App

Lösen Sie die Ausführung Ihrer Azure Machine Learning-Pipeline aus, wenn neue Daten angezeigt werden. Beispielsweise möchten Sie möglicherweise die Pipeline auslösen, damit ein neues Modells trainiert wird, wenn neue Daten im Blob Storage-Konto angezeigt werden. Richten Sie den Auslöser mit [Azure Logic Apps](../logic-apps/logic-apps-overview.md) ein.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure Machine Learning-Arbeitsbereich. Weitere Informationen finden Sie unter [Erstellen eines Azure Machine Learning-Arbeitsbereichs](how-to-manage-workspace.md).

* Der REST-Endpunkt für eine veröffentlichte Machine Learning-Pipeline. [Erstellen und veröffentlichen Sie Ihre Pipeline](how-to-create-your-first-pipeline.md). Suchen Sie anschließend den REST-Endpunkt Ihrer veröffentlichten Pipeline mithilfe der Pipeline-ID:
    
     ```
    # You can find the pipeline ID in Azure Machine Learning studio
    
    published_pipeline = PublishedPipeline.get(ws, id="<pipeline-id-here>")
    published_pipeline.endpoint 
    ```
* [Azure Blob Storage](../storage/blobs/storage-blobs-overview.md) zum Speichern Ihrer Daten.
* [Ein Datenspeicher](how-to-access-data.md) in Ihrem Arbeitsbereich, der die Details Ihres Blob Storage-Kontos enthält.

## <a name="create-a-logic-app"></a>Erstellen einer Logik-App

Erstellen Sie nun eine [Azure Logic Apps](../logic-apps/logic-apps-overview.md)-Instanz. Auf Wunsch können Sie [eine Integrationsdienstumgebung (Integration Service Environment, ISE) verwenden](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) und [einen vom Kunden verwalteten Schlüssel](../logic-apps/customer-managed-keys-integration-service-environment.md) für die Verwendung durch Ihre Logik-App einrichten.

Führen Sie nach der Bereitstellung Ihrer Logik-App die folgenden Schritte aus, um einen Trigger für Ihre Pipeline zu konfigurieren:

1. [Erstellen Sie eine systemseitig zugewiesene verwaltete Identität](../logic-apps/create-managed-service-identity.md), um der App Zugriff auf Ihren Azure Machine Learning-Arbeitsbereich zu gewähren.

1. Navigieren Sie zur Logik-App-Designer-Ansicht, und wählen Sie die Vorlage „Leere Logik-App“ aus. 
    > [!div class="mx-imgBorder"]
    > ![Leere Vorlage](media/how-to-trigger-published-pipeline/blank-template.png)

1. Suchen Sie im Designer nach **Blob**. Wählen Sie den Trigger **Beim Hinzufügen oder Ändern eines Blobs (nur Eigenschaften)** aus, und fügen Sie diesen Trigger Ihrer Logik-App hinzu.
    > [!div class="mx-imgBorder"]
    > ![Hinzufügen eines Triggers](media/how-to-trigger-published-pipeline/add-trigger.png)

1. Geben Sie die Verbindungsinformationen für das Blob Storage-Konto ein, das Sie auf Ergänzungen oder Änderungen an Blobs überwachen möchten. Wählen Sie den zu überwachenden Container aus. 
 
    Wählen Sie ein für Sie geeignetes **Intervall** und eine **Häufigkeit** für den Abruf von Updates aus.  

    > [!NOTE]
    > Mit diesem Auslöser wird der ausgewählte Container überwacht, jedoch keine Unterordner.

1. Fügen Sie eine HTTP-Aktion hinzu, die ausgeführt wird, wenn ein neues oder geändertes Blob erkannt wird. Wählen Sie **+ Neuer Schritt** aus, suchen Sie nach der HTTP-Aktion, und wählen Sie sie aus.

  > [!div class="mx-imgBorder"]
  > ![Suchen einer HTTP-Aktion](media/how-to-trigger-published-pipeline/search-http.png)

  Konfigurieren Sie Ihre Aktion mit den folgenden Einstellungen:

  | Einstellung | Wert | 
  |---|---|
  | HTTP-Aktion | POST |
  | URI |Der Endpunkt für die veröffentlichte Pipeline, den Sie als [Voraussetzung](#prerequisites) festgelegt haben |
  | Authentifizierungsmodus | Verwaltete Identität |

1. Richten Sie Ihren Zeitplan so ein, dass ggf. der Wert aller [DataPath-Pipelineparameter](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/intro-to-pipelines/aml-pipelines-showcasing-datapath-and-pipelineparameter.ipynb) festgelegt wird:

    ```json
    "DataPathAssignments": { 
         "input_datapath": { 
                            "DataStoreName": "<datastore-name>", 
                            "RelativePath": "@triggerBody()?['Name']" 
    } 
    }, 
    "ExperimentName": "MyRestPipeline", 
    "ParameterAssignments": { 
    "input_string": "sample_string3" 
    },
    ```

    Verwenden Sie den `DataStoreName`, den Sie Ihrem Arbeitsbereich als [Voraussetzung](#prerequisites) hinzugefügt haben.
     
    > [!div class="mx-imgBorder"]
    > ![HTTP-Einstellungen](media/how-to-trigger-published-pipeline/http-settings.png)

1. Wählen Sie **Speichern** aus. Der Zeitplan ist nun bereit.

> [!IMPORTANT]
> Wenn mit der rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) den Zugriff auf Ihre Pipeline verwalten, [legen Sie die Berechtigungen für Ihr Pipelineszenario fest (Training oder Bewertung).](how-to-assign-roles.md#q-what-are-the-permissions-needed-to-perform-some-common-scenarios-in-the-azure-machine-learning-service)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter

> [!div class="nextstepaction"]
> [Verwenden von Azure Machine Learning-Pipelines für die Batchbewertung](tutorial-pipeline-batch-scoring-classification.md)

* Weitere Informationen zu [Pipelines](concept-ml-pipelines.md)
* Erfahren Sie mehr über das [Erkunden von Azure Machine Learning mit Jupyter](samples-notebooks.md)

