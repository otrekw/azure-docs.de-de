---
title: Erstellen von Datasets mit Azure Open Datasets
titleSuffix: Azure Open Datasets
description: Hier erfahren Sie, wie Sie ein Azure Machine Learning-Dataset über Azure Open Datasets erstellen.
ms.service: open-datasets
ms.topic: conceptual
ms.author: nibaccam
author: nibaccam
ms.date: 08/05/2020
ms.custom: how-to, tracking-python
ms.openlocfilehash: e6f3a541f1e9dbca2c9949fb0c5cde28cd43e8e5
ms.sourcegitcommit: faeabfc2fffc33be7de6e1e93271ae214099517f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2020
ms.locfileid: "88183020"
---
# <a name="create-azure-machine-learning-datasets-from-azure-open-datasets"></a>Erstellen von Azure Machine Learning-Datasets über Azure Open Datasets
[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie ein Azure Machine Learning-Dataset über [Azure Open Datasets](https://azure.microsoft.com/services/open-datasets/) erstellen, um auf Daten für Ihre lokalen oder remotebasierten Experimente zuzugreifen. 

Durch Erstellen eines [Azure Machine Learning-Datasets](../machine-learning/how-to-create-register-datasets.md) erstellen Sie einen Verweis auf den Speicherort der Datenquelle sowie eine Kopie der zugehörigen Metadaten. Da die Daten an ihrem bisherigen Speicherort verbleiben, entstehen Ihnen keine zusätzlichen Speicherkosten, und Sie riskieren nicht die unbeabsichtigte Änderung Ihrer Datenquellen. Darüber hinaus werden Datasets verzögert ausgewertet, was zur Steigerung der Workflowleistung beiträgt.
 
Informationen dazu, welche Rolle Datasets im Workflow für den Datenzugriff in Azure Machine Learning spielen, finden Sie im Artikel [Datenzugriff in Azure Machine Learning](../machine-learning/concept-data.md#data-workflow).


Open Datasets befindet sich bei Microsoft Azure in der Cloud und ist sowohl im [Azure Machine Learning Python SDK](#create-datasets-with-the-sdk) und in [Azure Machine Learning Studio](#create-datasets-with-the-studio) enthalten.

## <a name="why-use-azure-open-datasets"></a>Gründe für die Verwendung von Azure Open Datasets? 

Öffentliche Azure-Datasets sind kuratierte öffentliche Datasets, mit denen Sie Lösungen mit maschinellem Lernen szenariospezifische Features hinzufügen können, um genauere Modelle zu erzielen. Die Datasets umfassen gemeinfreie Daten für Wetter, Volkszählungen, Feiertage, öffentliche Sicherheit und Orte, mit denen Sie Machine Learning-Modelle trainieren und Vorhersagelösungen anreichern können. 

Weitere Informationen finden Sie unter [Was sind öffentliche Datasets?](overview-what-are-open-datasets.md).

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes, um Datasets zu erstellen und zu nutzen:

* Ein Azure-Abonnement. Wenn Sie keines haben, erstellen Sie ein kostenloses Konto, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) aus.

* Ein [Azure Machine Learning-Arbeitsbereich](../machine-learning/how-to-manage-workspace.md).

* Eine [Installation des Azure Machine Learning-SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), in dem das Paket „azureml-datasets“ enthalten ist.

    * Erstellen Sie eine [Azure Machine Learning-Computeinstanz](../machine-learning/concept-compute-instance.md#managing-a-compute-instance), bei der es sich um eine vollständig konfigurierte und verwaltete Entwicklungsumgebung handelt, die integrierte Notebooks und das bereits installierte SDK beinhaltet.

    **OR**

    * Arbeiten Sie an Ihrem eigenen Jupyter-Notebook, und installieren Sie selbst das SDK mit [diesen Anweisungen](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

> [!NOTE]
> Einige Datasetklassen sind vom Paket [azureml-dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py) abhängig, das nur mit 64-Bit Python kompatibel ist. Für Linux-Benutzer werden diese Klassen nur unter den folgenden Distributionen unterstützt:  Red Hat Enterprise Linux (7, 8), Ubuntu (14.04, 16.04, 18.04), Fedora (27, 28), Debian (8, 9) und CentOS (7).

## <a name="create-datasets-with-the-sdk"></a>Erstellen von Datasets mit dem SDK

Wenn Sie Datasets über Open Datasets-Klassen im Azure Machine Learning Python SDK erstellen möchten, vergewissern Sie sich, dass Sie das Paket mit `pip install azureml-opendatasets` installiert haben. Jedes einzelne Dataset wird durch seine eigene Klasse im SDK dargestellt, und bestimmte Klassen sind entweder als `TabularDataset`, als `FileDataset` oder als beides verfügbar. Eine vollständige Liste der Klassen finden Sie in der [Referenzdokumentation](https://docs.microsoft.com/python/api/azureml-opendatasets/azureml.opendatasets?view=azure-ml-py).

Bestimmte Klassen können entweder als `TabularDataset` oder als `FileDataset` abgerufen werden, sodass Sie die Dateien direkt bearbeiten und/oder herunterladen können. Andere Klassen können ein Dataset **nur** abrufen, indem sie eine der Funktionen `get_tabular_dataset()` oder `get_file_dataset()` verwenden. Im folgenden Codebeispiel sind einige Beispiele für diese Typen von Klassen gezeigt.

```python
from azureml.opendatasets import MNIST

# MNIST class can return either TabularDataset or FileDataset
tabular_dataset = MNIST.get_tabular_dataset()
file_dataset = MNIST.get_file_dataset()

from azureml.opendatasets import Diabetes

# Diabetes class can return ONLY TabularDataset and must be called from the static function
diabetes_tabular = Diabetes.get_tabular_dataset()
```

Wenn Sie ein Dataset registrieren, das aus Open Datasets erstellt wurde, werden keine Daten sofort heruntergeladen, sondern die Daten werden später, wenn sie angefordert werden (z. B. während des Trainings), aus einem zentralen Speicherort abgerufen.

## <a name="create-datasets-with-the-studio"></a>Erstellen von Datasets mit Studio

Sie können Datasets auch über Open Datasets mit [Azure Machine Learning Studio](https://ml.azure.com) erstellen.

1. Wählen Sie in Ihrem Arbeitsbereich unter **Assets** die Registerkarte **Datasets** aus. Wählen Sie im Dropdownmenü **Dataset erstellen** die Option **Aus Open Datasets** aus.

    ![Öffnen eines Datasets über die Benutzeroberfläche](./media/how-to-create-dataset-from-open-dataset/open-datasets-1.png)

1. Wählen Sie ein Dataset aus, indem Sie die entsprechende Kachel auswählen. (Über die Suchleiste kann gefiltert werden.) Wählen Sie **Weiter** aus.

    ![Dataset auswählen](./media/how-to-create-dataset-from-open-dataset/open-datasets-2.png)

1. Wählen Sie einen Namen, unter dem das Dataset registriert werden soll, und filtern Sie die Daten optional mithilfe der verfügbaren Filter. Filtern Sie in diesem Fall das Dataset der **gesetzlichen Feiertage** nach Zeitraum (ein Jahr) und Ländercode (nur USA). Klicken Sie auf **Erstellen**.

    ![Dataset-Parameter festlegen und Dataset erstellen](./media/how-to-create-dataset-from-open-dataset/open-datasets-3.png)

    Das Dataset ist nun in Ihrem Arbeitsbereich unter **Datasets** verfügbar. Sie können es auf die gleiche Weise verwenden wie andere Datasets, die Sie erstellt haben.


## <a name="access-datasets-for-your-experiments"></a>Zugreifen auf Datasets für Ihre Experimente

Verwenden Sie Ihre Datasets in Ihren Machine Learning-Experimenten zum Trainieren von ML-Modellen. [Erfahren Sie mehr über das Trainieren mit Datasets](../machine-learning/how-to-train-with-datasets.md).

## <a name="example-notebooks"></a>Beispielnotebooks

Beispiele und Demos für die Open Datasets-Funktionen finden Sie in [diesen Notebooks](https://github.com/Azure/OpenDatasetsNotebooks/tree/master/tutorials).

## <a name="next-steps"></a>Nächste Schritte

* [Trainieren eines Modells mit Datasets](../machine-learning/how-to-train-with-datasets.md)

* [Erstellen eines Azure Machine Learning-Datasets](../machine-learning/how-to-create-register-datasets.md)



