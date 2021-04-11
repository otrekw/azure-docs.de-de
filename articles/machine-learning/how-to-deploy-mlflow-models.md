---
title: Bereitstellen von MLflow-Modellen als Webdienst
titleSuffix: Azure Machine Learning
description: Richten Sie MLflow mit Azure Machine Learning ein, um Ihre ML-Modelle als Azure-Webdienst bereitzustellen.
services: machine-learning
author: shivp950
ms.author: shipatel
ms.service: machine-learning
ms.subservice: core
ms.reviewer: nibaccam
ms.date: 12/23/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python
ms.openlocfilehash: c45b819f9fc02fae40c2bf7fc5c2247c8c0a6147
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "102517479"
---
# <a name="deploy-mlflow-models-as-azure-web-services-preview"></a>Bereitstellen von MLflow-Modellen als Azure-Webdienst (Vorschau)

In diesem Artikel erfahren Sie, wie Sie Ihr [MLflow](https://www.mlflow.org)-Modell als Azure-Webdienst bereitstellen, um die Azure Machine Learning-Funktionen für die Modellverwaltung und Datendrifterkennung nutzen und auf Ihre Produktionsmodelle anwenden zu können.

Azure Machine Learning bietet Bereitstellungskonfigurationen für Folgendes:
* Azure Container Instance (ACI) ist eine geeignete Wahl für die schnelle Bereitstellung von Dev/Test-Funktionen.
* Azure Kubernetes Service (AKS) empfiehlt sich für skalierbare Produktionsbereitstellungen.
> [!TIP]
> Dieses Dokument richtet sich primär an Data Scientists und Entwickler, die ihr MLflow-Modell an einem Azure Machine Learning-Webdienstendpunkt bereitstellen möchten. Wenn Sie Administrator sind und sich für die Überwachung der Nutzung und Ereignisse von Azure Machine Learning (z. B. Kontingente, abgeschlossene Trainingsausführungen oder abgeschlossene Modellimplementierungen) interessieren, helfen Ihnen die Informationen im Artikel [Überwachen von Azure Machine Learning](monitor-azure-machine-learning.md) weiter.
## <a name="mlflow-with-azure-machine-learning-deployment"></a>Bereitstellung von MLflow mit Azure Machine Learning

MLFlow ist eine Open-Source-Bibliothek zum Verwalten des Lebenszyklus Ihrer Machine Learning-Experimente. Dank der Integration in Azure Machine Learning können Sie von der Modelltrainingsphase bis hin zur Bereitstellungsphase Ihres Produktionsmodells von diesen Verwaltungsfunktionen profitieren.

Das folgende Diagramm veranschaulicht, wie Sie mit der MLflow-Bereitstellungs-API und Azure Machine Learning mit beliebten Frameworks (PyTorch, Tensorflow, scikit-learn usw.) erstellte Modelle als Azure-Webdienst bereitstellen und im Arbeitsbereich verwalten können. 

![ Bereitstellen von MLflow-Modellen mit Azure Machine Learning](./media/how-to-deploy-mlflow-models/mlflow-diagram-deploy.png)


>[!NOTE]
> Als Open Source-Bibliothek wird MLflow häufig geändert. Daher sollte die Funktionalität, die über die Integration von Azure Machine Learning und MLflow zur Verfügung gestellt wird, als Vorschau betrachtet und nicht vollständig von Microsoft unterstützt werden.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Machine Learning-Modell. Wenn Sie über kein trainiertes Modell verfügen, suchen Sie in [diesem Repository](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks/using-mlflow) das Notebook, das sich am besten für Ihr Computeszenario eignet, und befolgen Sie die Anweisungen. 
* [Richten Sie die MLflow-Nachverfolgungs-URI für die Verbindung mit Azure Machine Learning ein](how-to-use-mlflow.md#track-local-runs).
* Installieren Sie das `azureml-mlflow`-Paket. 
    * Mit diesem Paket wird automatisch `azureml-core` des [The Azure Machine Learning Python SDK](/python/api/overview/azure/ml/install) bereitgestellt, um MLflow den Zugriff auf Ihren Arbeitsbereich zu ermöglichen.
* Überprüfen Sie, welche [Zugriffsberechtigungen Sie benötigen, um Ihre MLflow-Vorgänge mit Ihrem Arbeitsbereich](how-to-assign-roles.md#mlflow-operations) auszuführen. 

## <a name="deploy-to-azure-container-instance-aci"></a>Bereitstellen in Azure Container Instances (ACI)

Damit Sie Ihr MLflow-Modell in einem Azure Machine Learning-Webdienst bereitstellen können, muss das Modell mit der [MLflow-Nachverfolgungs-URI zum Herstellen der Verbindung mit Azure Machine Learning](how-to-use-mlflow.md) eingerichtet sein. 

Richten Sie Ihre Bereitstellungskonfiguration mit der Methode [deploy_configuration()](/python/api/azureml-core/azureml.core.webservice.aciwebservice#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none-) ein. Sie können auch Tags und Beschreibungen hinzufügen, um den Überblick über Ihren Webdienst zu behalten.

```python
from azureml.core.webservice import AciWebservice, Webservice

# Set the model path to the model folder created by your run
model_path = "model"

# Configure 
aci_config = AciWebservice.deploy_configuration(cpu_cores=1, 
                                                memory_gb=1, 
                                                tags={'method' : 'sklearn'}, 
                                                description='Diabetes model',
                                                location='eastus2')
```

Dann können Sie das Modell mithilfe der [deploy](https://www.mlflow.org/docs/latest/python_api/mlflow.azureml.html#mlflow.azureml.deploy)-Methode von MLflow in einem einzigen Schritt für Azure Machine Learning registrieren und bereitstellen. 

```python
(webservice,model) = mlflow.azureml.deploy( model_uri='runs:/{}/{}'.format(run.id, model_path),
                      workspace=ws,
                      model_name='sklearn-model', 
                      service_name='diabetes-model-1', 
                      deployment_config=aci_config, 
                      tags=None, mlflow_home=None, synchronous=True)

webservice.wait_for_deployment(show_output=True)
```

## <a name="deploy-to-azure-kubernetes-service-aks"></a>Bereitstellen für Azure Kubernetes Service (AKS)

Damit Sie Ihr MLflow-Modell in einem Azure Machine Learning-Webdienst bereitstellen können, muss das Modell mit der [MLflow-Nachverfolgungs-URI zum Herstellen der Verbindung mit Azure Machine Learning](how-to-use-mlflow.md) eingerichtet sein. 

Zur Bereitstellung für AKS erstellen Sie zunächst einen AKS-Cluster. Erstellen Sie mit der [ComputeTarget.create()](/python/api/azureml-core/azureml.core.computetarget#create-workspace--name--provisioning-configuration-)-Methode einen AKS-Cluster. Die Erstellung eines neuen Clusters kann 20 bis 25 Minuten dauern.

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Use the default configuration (can also provide parameters to customize)
prov_config = AksCompute.provisioning_configuration()

aks_name = 'aks-mlflow'

# Create the cluster
aks_target = ComputeTarget.create(workspace=ws, 
                                  name=aks_name, 
                                  provisioning_configuration=prov_config)

aks_target.wait_for_completion(show_output = True)

print(aks_target.provisioning_state)
print(aks_target.provisioning_errors)
```
Richten Sie Ihre Bereitstellungskonfiguration mit der Methode [deploy_configuration()](/python/api/azureml-core/azureml.core.webservice.aciwebservice#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none-) ein. Sie können auch Tags und Beschreibungen hinzufügen, um den Überblick über Ihren Webdienst zu behalten.

```python
from azureml.core.webservice import Webservice, AksWebservice

# Set the web service configuration (using default here with app insights)
aks_config = AksWebservice.deploy_configuration(enable_app_insights=True, compute_target_name='aks-mlflow')

```

Dann können Sie das Modell mithilfe der [deploy](https://www.mlflow.org/docs/latest/python_api/mlflow.azureml.html#mlflow.azureml.deploy)-Methode von MLflow in einem einzigen Schritt für Azure Machine Learning registrieren und bereitstellen. 

```python

# Webservice creation using single command
from azureml.core.webservice import AksWebservice, Webservice

# set the model path 
model_path = "model"

(webservice, model) = mlflow.azureml.deploy( model_uri='runs:/{}/{}'.format(run.id, model_path),
                      workspace=ws,
                      model_name='sklearn-model', 
                      service_name='my-aks', 
                      deployment_config=aks_config, 
                      tags=None, mlflow_home=None, synchronous=True)


webservice.wait_for_deployment()
```

Die Bereitstellung des Diensts kann mehrere Minuten dauern.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie den bereitgestellten Webdienst nicht verwenden werden, löschen Sie ihn mithilfe von `service.delete()` von Ihrem Notebook.  Weitere Informationen finden Sie in der Dokumentation zu [WebService.delete()](/python/api/azureml-core/azureml.core.webservice%28class%29#delete--).

## <a name="example-notebooks"></a>Beispielnotebooks

In den [Notebooks „MLflow mit Azure Machine Learning“](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks/using-mlflow) werden die in diesem Artikel vorgestellten Konzepte weiter erläutert und demonstriert.

> [!NOTE]
> Ein Communityrepository mit Beispielen, die MLflow verwenden, finden Sie unter https://github.com/Azure/azureml-examples.

## <a name="next-steps"></a>Nächste Schritte

* [Verwalten Ihrer Modelle](concept-model-management-and-deployment.md)
* Überwachen Ihrer Produktionsmodelle auf [Datenabweichungen](./how-to-enable-data-collection.md)
* [Nachverfolgen von Azure Databricks-Ausführungen mit MLflow](how-to-use-mlflow-azure-databricks.md)

