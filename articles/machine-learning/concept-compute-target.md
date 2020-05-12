---
title: Was sind Computziele?
titleSuffix: Azure Machine Learning
description: Legen Sie fest, wo Sie Ihr Modell mit Azure Machine Learning trainieren oder bereitstellen möchten.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 03/30/2020
ms.openlocfilehash: ed65d69c18f2dbcd53324fe3cc18af8c51c546b2
ms.sourcegitcommit: 31236e3de7f1933be246d1bfeb9a517644eacd61
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2020
ms.locfileid: "82780112"
---
#  <a name="what-are-compute-targets-in-azure-machine-learning"></a>Was sind Computeziele in Azure Machine Learning? 

Ein **Computeziel** ist eine festgelegte Computeressource/-Umgebung, in der Ihr Trainingsskript ausgeführt oder Ihre Dienstbereitstellung gehostet wird. Hierbei kann es sich um Ihren lokalen Computer oder eine cloudbasierte Computeressource handeln. Mithilfe von Computezielen können Sie Ihre Compute-Umgebung später problemlos ändern, ohne den Code anpassen zu müssen.  

In einem typischen Modellentwicklungslebenszyklus gehen Sie unter Umständen wie folgt vor:
1. Sie entwickeln und experimentieren zunächst mit einer kleinen Datenmenge. In dieser Phase wird empfohlen, Ihre lokale Umgebung (etwa Ihren lokalen Computer oder einen cloudbasierten virtuellen Computer) als Computeziel zu verwenden. 
2. Skalieren Sie Ihre Umgebung für größere Datenmengen hoch, oder führen Sie ein verteiltes Training mithilfe eines dieser [Trainingscomputeziele](#train) durch.  
3. Wenn Ihr Modell bereit ist, stellen Sie es in einer Webhostingumgebung oder auf einem IoT-Gerät mit einem [dieser Bereitstellungscomputeziele](#deploy) bereit.

Die Computeressourcen, die Sie für Ihre Computeziele verwenden, werden an einen [Arbeitsbereich](concept-workspace.md) angefügt. Andere Computeressourcen als der lokale Computer werden von Benutzern des Arbeitsbereichs gemeinsam genutzt.

## <a name="training-compute-targets"></a><a name="train"></a> Trainieren von Computezielen

Azure Machine Learning bietet unterschiedliche Unterstützung für verschiedene Computeressourcen.  Sie können auch Ihre eigene Computeressource anfügen. Die Unterstützung für verschiedene Szenarien kann jedoch variieren.

[!INCLUDE [aml-compute-target-train](../../includes/aml-compute-target-train.md)]

Erfahren Sie mehr über das [Einrichten und Verwenden eines Computeziels für das Modelltraining](how-to-set-up-training-targets.md).

## <a name="deployment-targets"></a><a name="deploy"></a>Bereitstellungsziele

Die folgenden Computeressourcen können zum Hosten Ihrer Modellimplementierung verwendet werden.

[!INCLUDE [aml-compute-target-deploy](../../includes/aml-compute-target-deploy.md)]

Erfahren Sie, [wo und wie Sie Ihr Modell auf einem Computeziel bereitstellen](how-to-deploy-and-where.md).

<a name="amlcompute"></a>
## <a name="azure-machine-learning-compute-managed"></a>Azure Machine Learning-Computeressource (verwaltet)

Eine verwaltete Computeressource wird von Azure Machine Learning erstellt und verwaltet. Diese Computeressource ist für Machine Learning-Workloads optimiert. Azure Machine Learning-Computecluster und -[Compute-Instanzen](concept-compute-instance.md) sind die einzigen verwalteten Computeressourcen. Möglicherweise werden künftig weitere verwaltete Computeressourcen hinzugefügt.

Sie können Azure Machine Learning-Compute-Instanzen (Vorschau) oder -Computecluster erstellen über:
* Azure Machine Learning Studio
* Azure-Portal
* Python SDK-Klassen [ComputeInstance](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computeinstance(class)?view=azure-ml-py) und [AmlCompute](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute(class)?view=azure-ml-py)
* [R SDK](https://azure.github.io/azureml-sdk-for-r/reference/index.html#section-compute-targets)
* Resource Manager-Vorlage

Sie können auch Computecluster mit der [Erweiterung für maschinelles Lernen für die Azure CLI](tutorial-train-deploy-model-cli.md#create-the-compute-target-for-training) erstellen.

Nach der Erstellung sind diese Computeressourcen im Gegensatz zu anderen Arten von Computezielen automatisch Teil Ihres Arbeitsbereichs.

### <a name="compute-clusters"></a>Computecluster

Sie können Azure Machine Learning-Computecluster für zum Trainieren und für Batchrückschlüsse (Vorschau) verwenden.  Diese Computeressource bietet Folgendes:

* Cluster mit einem oder mehreren Knoten
* Automatische Skalierung bei jedem Übermitteln einer Ausführung 
* Automatische Clusterverwaltung und Auftragsplanung 
* Unterstützt CPU- und GPU-Ressourcen

### <a name="supported-vm-series-and-sizes"></a>Unterstützte VM-Serien und -Größen

Wenn Sie eine Knotengröße für eine verwaltete Computeressource in Azure Machine Learning auswählen, können Sie unter den in Azure verfügbaren VM-Größen auswählen. Azure bietet eine Reihe von Größen für Linux und Windows für verschiedene Workloads. Weitere Informationen zu den verschiedenen [Arten und Größen von virtuellen Computern](https://docs.microsoft.com/azure/virtual-machines/linux/sizes) finden Sie hier.

Bei der Auswahl einer VM-Größe gelten einige Ausnahmen und Einschränkungen:
* Einige VM-Serien werden in Azure Machine Learning nicht unterstützt.
* Einige VM-Serien sind eingeschränkt. Um eine eingeschränkte Serie zu verwenden, wenden Sie sich an den Support, und fordern Sie eine Kontingenterhöhung für die Serie an. Informationen zur Kontaktaufnahme mit dem Support finden Sie unter [Azure-Supportoptionen](https://azure.microsoft.com/support/options/)

In der folgenden Tabelle finden Sie weitere Informationen zu unterstützten Serien und Einschränkungen. 

| **Unterstützte VM-Serie**  | **Einschränkungen** |
|------------|------------|
| D | Keine |
| Dv2 | Keine |  
| DSv2 | Keine |  
| FSv2 | Keine |  
| M | Genehmigung erforderlich |
| NC | Keine |    
| NCsv2 | Genehmigung erforderlich |
| NCsv3 | Genehmigung erforderlich |  
| NDs | Genehmigung erforderlich |
| NDv2 | Genehmigung erforderlich |
| SH | Keine |
| NVv3 | Genehmigung erforderlich | 


Obwohl Azure Machine Learning diese VM-Serien unterstützt, sind sie möglicherweise nicht in allen Azure-Regionen verfügbar. Hier können Sie überprüfen, ob die VM-Serie verfügbar ist: [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines).

## <a name="unmanaged-compute"></a>Nicht verwaltete Computeressourcen

Ein nicht verwaltetes Computeziel wird *nicht* von Azure Machine Learning verwaltet. Sie erstellen diese Art von Computeziel außerhalb von Azure Machine Learning und fügen es anschließend an Ihren Arbeitsbereich an. Für nicht verwaltete Computeressourcen sind möglicherweise weitere Schritte erforderlich, um die Leistung von Machine Learning-Workloads beizubehalten oder zu verbessern.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel werden folgende Themen erläutert:
* [Einrichten eines Computeziels für das Modelltraining](how-to-set-up-training-targets.md)
* [Bereitstellen Ihres Modells auf einem Computeziel](how-to-deploy-and-where.md)
