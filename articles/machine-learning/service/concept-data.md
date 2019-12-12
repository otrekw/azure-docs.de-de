---
title: Daten in Azure Machine Learning
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Ihre Daten in Azure Machine Learning verarbeitet und wie die Daten in ihren Machine Learning-Experimenten verwendet werden.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: nibaccam
author: nibaccam
ms.author: nibaccam
ms.date: 12/09/2019
ms.openlocfilehash: 4ea0f7092862ef910e9f44e81cc4f5f40849bc70
ms.sourcegitcommit: 6c01e4f82e19f9e423c3aaeaf801a29a517e97a0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74820472"
---
# <a name="data-access-in-azure-machine-learning"></a>Datenzugriff in Azure Machine Learning

In diesem Artikel erfahren Sie mehr über die Datenverwaltungs- und -integrationslösungen von Azure Machine Learning, die für Ihre Machine Learning-Aufgaben verwendet werden. Für diesen Artikel wird davon ausgegangen, dass Sie bereits ein [Azure Storage-Konto](https://docs.microsoft.comazure/storage/common/storage-quickstart-create-account?tabs=azure-portal) und einen [Azure Storage-Dienst](https://docs.microsoft.com/azure/storage/common/storage-introduction) erstellt haben.

Wenn Sie bereit sind, die Daten in Ihrem Speicher zu verwenden, sollten Sie folgende Schritte ausführen:

1. Erstellen Sie einen Azure Machine Learning-Datenspeicher.
2. Erstellen Sie aus diesem Datenspeicher ein Azure Machine Learning-Dataset. 
3. Verwenden Sie dieses Dataset in Ihrem Machine Learning-Experiment, indem Sie es entweder 
    1. in das Computeziel Ihres Experiments zum Modelltraining einbinden,

        **OR** 

    1. direkt in Azure Machine Learning Lösungen, z. B. Experimentausführungen für automatisiertes maschinelles Lernen (automatisiertes ML), Machine Learning-Pipelines und [Azure Machine Learning-Designer](concept-designer.md), verwenden.
4. Erstellen Sie Datasetüberwachungen für Ihre Modelleingabe- und -ausgabedatasets, damit Datendrift erkannt wird. 
5. Wenn Datendrift erkannt wird, aktualisieren Sie Ihr Dataset, und trainieren Sie Ihr Modell entsprechend neu.

In der folgenden Abbildung ist dieser empfohlene Datenzugriffsablauf dargestellt.

![Abbildung mit Datenkonzept](media/concept-data/data-concept-diagram.svg)

## <a name="access-data-in-storage"></a>Zugreifen auf Daten im Speicher

Für den Zugriff auf Ihre Daten in Ihrem Speicherkonto stellt Azure Machine Learning Datenspeicher und Datasets bereit. Datenspeicher bieten eine Abstraktionsebene über Ihren Speicherdienst. Dies hilft bei der Sicherheit und dem einfacheren Zugriff auf Ihren Speicher, da Verbindungsinformationen im Datenspeicher gespeichert sind und nicht in Skripts verfügbar gemacht werden. Datasets verweisen auf die speziellen Dateien in Ihrem zugrunde liegenden Speicher, die Sie für Ihr Machine Learning-Experiment verwenden möchten. Zusammen bieten Datenspeicher und Datasets einen sicheren, skalierbaren und reproduzierbaren Datenbereitstellungsablauf für Ihre Machine Learning-Aufgaben.

### <a name="datastores"></a>Datenspeicher

Ein Azure Machine Learning-Datenspeicher ist eine Speicherabstraktion über Ihre Azure Storage-Dienste. [Registrieren und erstellen Sie einen Datenspeicher](how-to-access-data.md), um auf einfache Weise eine Verbindung mit Ihrem Azure-Speicherkonto herzustellen und auf die Daten in Ihren zugrunde liegenden Azure Storage-Diensten zuzugreifen.

Unterstützte Azure Storage-Dienste, die als Datenspeicher registriert werden können:
+ Azure-Blobcontainer
+ Azure-Dateifreigabe
+ Azure Data Lake
+ Azure Data Lake Gen2
+ Azure SQL-Datenbank
+ Azure Database for PostgreSQL
+ Databricks-Dateisystem
+ Azure Database for MySQL

### <a name="datasets"></a>Datasets

[Erstellen Sie ein Azure Machine Learning-Dataset](how-to-create-register-datasets.md), um auf Daten in Ihren Datenspeichern zuzugreifen und die Daten in einem nutzbaren Objekt für Machine Learning-Aufgaben zusammenzustellen. Registrieren Sie das Dataset in Ihrem Arbeitsbereich, um es freizugeben und in verschiedenen Experimenten ohne Datenerfassungskomplexität wiederzuverwenden.

Datasets können aus lokalen Dateien, öffentlichen URLs, [Azure Open Datasets](#open) oder bestimmten Dateien in Ihren Datenspeichern erstellt werden. Um ein Dataset aus einem In-Memory-Pandas-Dataframe zu erstellen, schreiben Sie die Daten in eine lokale Datei, z. B. eine CSV-Datei, und erstellen Sie Ihr Dataset aus dieser Datei. Datasets sind keine Kopien Ihrer Daten, sondern Verweise, die auf die Daten in Ihrem Speicherdienst verweisen. Daher fallen keine zusätzlichen Speicherkosten an. 

Die folgende Abbildung zeigt, dass Sie, sofern Sie keinen Azure Storage-Dienst haben, ein Dataset direkt aus lokalen Dateien, öffentlichen URLs oder einem öffentlichen Azure-Dataset erstellen können. Wenn Sie so vorgehen, wird Ihr Dataset mit dem Standarddatenspeicher verbunden, der automatisch mit dem [Azure Machine Learning-Arbeitsbereich](concept-workspace.md) Ihres Experiments erstellt wurde.

![Abbildung mit Datenkonzept](media/concept-data/dataset-workflow.svg)

Weitere Funktionalität für Datasets finden Sie in der folgenden Dokumentation:

+ [Versionieren und Nachverfolgen von Datasets in Experimenten](how-to-version-track-datasets.md).
+ [Überwachen Ihres Datasets](how-to-monitor-datasets.md), um die Erkennung von Datendrift zu unterstützen.
+  Dokumentation zu den beiden Typen von Datasets finden Sie in den folgenden Artikeln:
    + [TabularDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) stellt Daten in einem tabellarischen Format dar, indem die bereitgestellte Datei oder Liste von Dateien analysiert wird. Auf diese Weise können Sie die Daten zur weiteren Verarbeitung oder zur Bereinigung in einem Pandas- oder Spark-Dataframe zusammenstellen. Eine vollständige Liste der Dateien, aus denen Sie TabularDatasets erstellen können, finden Sie im Artikel über die [TabularDatasetFactory-Klasse](https://aka.ms/tabulardataset-api-reference).

    + [FileDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.file_dataset.filedataset?view=azure-ml-py) verweist auf eine einzelne Datei oder auf mehrere Dateien in Ihren Datenspeichern oder öffentlichen URLs. Mithilfe dieser Methode können Sie die jeweils gewünschten Dateien als FileDataset-Objekt in Ihr Computeziel herunterladen oder einbinden.

## <a name="work-with-your-data"></a>Arbeiten mit Ihren Daten

Mit Datasets können Sie eine Reihe von Machine Learning-Aufgaben über nahtlose Integration in Azure Machine Learning-Funktionen ausführen. 

+ [Trainieren von Machine Learning-Modellen](how-to-train-with-datasets.md)
+ Nutzen von Datasets in 
     + [automatisierten ML-Experimenten](how-to-create-portal-experiments.md)
     + dem [Designer](tutorial-designer-automobile-price-train-score.md#import-data) 
+ Zugreifen auf Datasets zur Bewertung mit Batchrückschluss in [Machine Learning-Pipelines](how-to-create-your-first-pipeline.md)
+ Erstellen eines [Datenbeschriftungsprojekts](#label)
+ Einrichten einer Datasetüberwachung zur Erkennung von [Datendrift](#drift)

<a name="open"></a>

## <a name="azure-open-datasets"></a>Azure Open Datasets

[Öffentliche Azure-Datasets](how-to-create-register-datasets.md#create-datasets-with-azure-open-datasets) sind kuratierte öffentliche Datasets, mit denen Sie Lösungen mit maschinellem Lernen szenariospezifische Features hinzufügen können, um genauere Modelle zu erzielen. Öffentliche Datasets (Open Datasets) befinden sich in der Cloud in Microsoft Azure und sind in Azure Machine Learning integriert. Sie können auch über APIs auf die Datasets zugreifen und sie in anderen Produkten wie Power BI und Azure Data Factory verwenden.

Öffentliche Azure-Datasets umfassen gemeinfreie Daten für Wetter, Volkszählungen, Feiertage, öffentliche Sicherheit und Orte, mit denen Sie Machine Learning-Modelle trainieren und Vorhersagelösungen anreichern können. Sie können in den öffentlichen Azure-Datasets auch Ihre eigenen öffentlichen Datasets freigeben.

<a name="label"></a>

## <a name="data-labeling"></a>Datenbeschriftung

Das Beschriften großer Datenmengen bereitet in Projekten für maschinelles Lernen (ML) häufig Kopfzerbrechen. Projekte mit einer Komponente für maschinelles Sehen – z. B. zur Bildklassifizierung oder Objekterkennung – erfordern im Allgemeinen Tausende von Bildern und entsprechende Beschriftungen.

Azure Machine Learning ist Ihr zentraler Ort zum Erstellen, Verwalten und Überwachen von Beschriftungsprojekten. Beschriftungsprojekte helfen dabei, Daten, Beschriftungen und Teammitglieder zu koordinieren, sodass Sie die Beschriftungsaufgaben effizienter verwalten können. Zu den zurzeit unterstützten Aufgaben gehören die Bildklassifizierung – mit mehreren Beschriftungen oder mehreren Klassen – und die Objektidentifikation mithilfe von Begrenzungsrahmen.

+ Erstellen Sie ein [Datenbeschriftungsprojekt](how-to-create-labeling-projects.md), und geben Sie ein Dataset aus, das in Machine Learning-Experimenten verwendet werden kann.

<a name="drift"></a>

## <a name="data-drift"></a>Datendrift

Im Zusammenhang mit maschinellem Lernen ist die Datenabweichung die Änderung der Modelleingabedaten, die zu einem Abfallen der Modellleistung führt. Datendrift ist einer der Hauptgründe für die Abnahme der Modellgenauigkeit im Lauf der Zeit, daher hilft das Überwachen auf Datendrift bei der Erkennung von Problemen bei der Leistung eines Modells.
Weitere Informationen dazu, wie Sie Datendrift bei neuen Daten in einem Dataset erkennen und melden können, finden Sie im Artikel [Erkennen von Datendrift (Vorschau) in Datasets](how-to-monitor-datasets.md).

## <a name="next-steps"></a>Nächste Schritte 

+ Erstellen Sie ein Dataset in Azure Machine Learning Studio oder mit dem Python SDK, indem Sie [die folgenden Schritte ausführen](how-to-create-register-datasets.md).
+ Testen Sie Beispiele zum Trainieren von Datasets mit unseren [Beispielnotebooks](https://aka.ms/dataset-tutorial).
+ Beispiele zu Datendrift finden Sie in diesem [Datendrift-Tutorial](https://aka.ms/datadrift-notebook).