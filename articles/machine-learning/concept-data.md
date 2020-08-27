---
title: Sicherer Datenzugriff in der Cloud
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie eine sichere Verbindung mit Ihren Daten von Azure Machine Learning herstellen und wie Sie Datasets und Datenspeicher für ML-Aufgaben verwenden. Datenspeicher können für die Speicherung von Daten aus einem Azure-Blob, Azure Data Lake Gen 1 und 2, SQL-DB, Databricks u. a. verwendet werden.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: nibaccam
author: nibaccam
ms.author: nibaccam
ms.date: 04/24/2020
ms.custom: devx-track-python
ms.openlocfilehash: dadd3a8316efc5bf090a84a738c8f6da223d4572
ms.sourcegitcommit: 271601d3eeeb9422e36353d32d57bd6e331f4d7b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88651793"
---
# <a name="secure-data-access-in-azure-machine-learning"></a>Sicherer Datenzugriff in Azure Machine Learning

Azure Machine Learning macht es einfach, eine Verbindung mit Ihren Daten in der Cloud herzustellen.  Es wird eine Abstraktionsschicht über dem zugrunde liegenden Speicherdienst bereitgestellt, sodass Sie sicher auf Ihre Daten zugreifen und diese bearbeiten können, ohne für Ihren Speichertyp spezifischen Code schreiben zu müssen. Azure Machine Learning bietet auch folgende Datenfunktionen:

*    Versionsverwaltung und Nachverfolgung der Datenherkunft
*    Datenbeschriftung 
*    Überwachung von Datenabweichungen
*    Interoperabilität mit Pandas und Spark DataFrames

## <a name="data-workflow"></a>Datenworkflow

Wenn Sie bereit sind, die Daten in ihrer cloudbasierten Speicherlösung zu verwenden, wird der folgende Datenübermittlungsworkflow empfohlen. Dieser Workflow setzt voraus, dass Sie über ein [Azure-Speicherkonto](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal) und Daten in einem cloudbasierten Speicherdienst in Azure verfügen. 

1. Erstellen Sie einen [Azure Machine Learning-Datenspeicher](#datastores), um Verbindungsinformationen in Ihrem Azure-Speicher zu speichern.

2. Erstellen Sie in diesem Datenspeicher ein [Azure Machine Learning-Dataset](#datasets), um auf (eine) bestimmte Datei(en) im zugrunde liegenden Speicher zu verweisen. 

3. Um dieses Dataset in Ihrem Machine Learning-Experiment zu verwenden, können Sie es entweder
    1. in das Computeziel Ihres Experiments zum Modelltraining einbinden,

        **OR** 

    1. direkt in Azure Machine Learning-Lösungen, z. B. Experimentausführungen für automatisiertes maschinelles Lernen (automatisiertes ML), Machine Learning-Pipelines oder [Azure Machine Learning-Designer](concept-designer.md), verwenden.

4. Erstellen Sie [Datasetüberwachungen](#data-drift) für Ihr Modellausgabedataset, um Datendrift zu erkennen. 

5. Wenn Datendrift erkannt wird, aktualisieren Sie Ihr Eingabedataset, und trainieren Sie Ihr Modell entsprechend neu.

In der folgenden Abbildung ist dieser empfohlene Workflow dargestellt.

![Abbildung mit Datenkonzept](./media/concept-data/data-concept-diagram.svg)

## <a name="datastores"></a>Datenspeicher

Azure Machine Learning-Datenspeicher speichern die Informationen zur Verbindung mit Ihrem Azure-Speicher sicher, sodass Sie sie nicht in Ihren Skripts programmieren müssen. [Registrieren und erstellen Sie einen Datenspeicher](how-to-access-data.md), um auf einfache Weise eine Verbindung mit Ihrem Speicherkonto herzustellen und auf die Daten in Ihrem zugrunde liegenden Azure-Speicherdienst zuzugreifen. 

Unterstützte cloudbasierte Speicherdienste in Azure, die als Datenspeicher registriert werden können:

+ Azure-Blobcontainer
+ Azure-Dateifreigabe
+ Azure Data Lake
+ Azure Data Lake Gen2
+ Azure SQL-Datenbank
+ Azure Database for PostgreSQL
+ Databricks-Dateisystem
+ Azure Database for MySQL

## <a name="datasets"></a>Datasets

Azure Machine Learning-Datasets sind Verweise auf die Daten in Ihrem Speicherdienst. Es handelt sich nicht um Kopien Ihrer Daten, sodass keine zusätzlichen Speicherkosten anfallen und die Integrität Ihrer Originaldatenquellen nicht gefährdet ist.

 [Erstellen Sie ein Dataset](how-to-create-register-datasets.md), um auf Daten in Ihrem Speicher zuzugreifen und die Daten in einem nutzbaren Objekt für Machine Learning-Aufgaben zusammenzustellen. Registrieren Sie das Dataset in Ihrem Arbeitsbereich, um es freizugeben und in verschiedenen Experimenten ohne Datenerfassungskomplexität wiederzuverwenden.

Datasets können aus lokalen Dateien, öffentlichen URLs, [Azure Open Datasets](https://azure.microsoft.com/services/open-datasets/) oder Azure-Speicherdiensten in Datenspeichern erstellt werden. Um ein Dataset aus einem In-Memory-Pandas-Dataframe zu erstellen, schreiben Sie die Daten in eine lokale Datei, z. B. eine Parquet-Datei, und erstellen Ihr Dataset aus dieser Datei.  

Wir unterstützen zwei Arten von Datasets: 

+ Ein [FileDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.file_dataset.filedataset?view=azure-ml-py) verweist auf eine einzelne Datei oder auf mehrere Dateien in Ihren Datenspeichern oder öffentlichen URLs. Wenn Ihre Daten bereits bereinigt und für Trainingsexperimente bereit sind, können Sie Dateien, auf die von FileDatasets verwiesen wird, auf Ihr Computeziel [herunterladen oder sie einbinden](how-to-train-with-datasets.md#mount-files-to-remote-compute-targets).

+ Ein [TabularDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) stellt Daten in einem tabellarischen Format dar, indem die bereitgestellte Datei oder Liste von Dateien analysiert wird. Sie können ein TabularDataset zur weiteren Verarbeitung oder zur Bereinigung in einen Pandas- oder Spark-Datenrahmen laden. Eine vollständige Liste der Datenformate, aus denen Sie TabularDatasets erstellen können, finden Sie im Artikel über die [TabularDatasetFactory-Klasse](https://aka.ms/tabulardataset-api-reference).

Weitere Funktionalität für Datasets finden Sie in der folgenden Dokumentation:

+ [Versionieren und Nachverfolgen von Datasets in Experimenten](how-to-version-track-datasets.md).
+ [Überwachen Ihres Datasets](how-to-monitor-datasets.md), um die Erkennung von Datendrift zu unterstützen.    

## <a name="work-with-your-data"></a>Arbeiten mit Ihren Daten

Mit Datasets können Sie eine Reihe von Machine Learning-Aufgaben über nahtlose Integration in Azure Machine Learning-Funktionen ausführen. 

+ Erstellen eines [Datenbeschriftungsprojekts](#label)
+ Trainieren von Machine Learning-Modellen:
     + [automatisierten ML-Experimenten](how-to-use-automated-ml-for-ml-models.md)
     + dem [Designer](tutorial-designer-automobile-price-train-score.md#import-data)
     + [Notebooks](how-to-train-with-datasets.md)
     + [Azure Machine Learning-Pipelines](how-to-create-your-first-pipeline.md)
+ Zugreifen auf Datasets zur Bewertung mit [Batchrückschluss](how-to-use-parallel-run-step.md) in [Machine Learning-Pipelines](how-to-create-your-first-pipeline.md).
+ Einrichten einer Datasetüberwachung zur Erkennung von [Datendrift](#drift)

<a name="label"></a>

## <a name="data-labeling"></a>Datenbeschriftung

Das Beschriften großer Datenmengen bereitet in Projekten für maschinelles Lernen (ML) häufig Kopfzerbrechen. Projekte mit einer Komponente für maschinelles Sehen – z. B. zur Bildklassifizierung oder Objekterkennung – erfordern im Allgemeinen Tausende von Bildern und entsprechende Beschriftungen.

Azure Machine Learning ist Ihr zentraler Ort zum Erstellen, Verwalten und Überwachen von Beschriftungsprojekten. Beschriftungsprojekte helfen dabei, Daten, Beschriftungen und Teammitglieder zu koordinieren, sodass Sie die Beschriftungsaufgaben effizienter verwalten können. Zu den zurzeit unterstützten Aufgaben gehören die Bildklassifizierung – mit mehreren Beschriftungen oder mehreren Klassen – und die Objektidentifikation mithilfe von Begrenzungsrahmen.

Erstellen Sie ein [Datenbeschriftungsprojekt](how-to-create-labeling-projects.md), und geben Sie ein Dataset aus, das in Machine Learning-Experimenten verwendet werden kann.

<a name="drift"></a>

## <a name="data-drift"></a>Datendrift

Im Zusammenhang mit maschinellem Lernen ist die Datenabweichung die Änderung der Modelleingabedaten, die zu einem Abfallen der Modellleistung führt. Datendrift ist einer der Hauptgründe für die Abnahme der Modellgenauigkeit im Lauf der Zeit, daher hilft das Überwachen auf Datendrift bei der Erkennung von Problemen bei der Leistung eines Modells.

Weitere Informationen dazu, wie Sie Datendrift bei neuen Daten in einem Dataset erkennen und melden können, finden Sie im Artikel [Erkennen von Datendrift (Vorschau) in Datasets](how-to-monitor-datasets.md).

## <a name="next-steps"></a>Nächste Schritte 

+ Erstellen Sie ein Dataset in Azure Machine Learning Studio oder mit dem Python SDK, indem Sie [die folgenden Schritte ausführen](how-to-create-register-datasets.md).
+ Testen Sie Beispiele zum Trainieren von Datasets mit unseren [Beispielnotebooks](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/work-with-data/).
