---
title: Azure Machine Learning und Machine Learning Studio (klassisch)
description: Was ist der Unterschied zwischen Azure Machine Learning und Machine Learning Studio (Classic)?
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: overview
author: j-martens
ms.author: jmartens
ms.date: 03/25/2020
ms.openlocfilehash: 065fd166fb2ce82a3338322d55ef1ee5886ac56b
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2020
ms.locfileid: "85210370"
---
# <a name="azure-machine-learning-vs-machine-learning-studio-classic"></a>Vergleich von Azure Machine Learning und Machine Learning Studio (Classic)

In diesem Artikel wird der Unterschied zwischen Azure Machine Learning und Machine Learning Studio (Classic) beschrieben. 

Azure Machine Learning verfügt über Python- und R-SDKs **und** über den Drag & Drop-Designer zum Erstellen und Bereitstellen von Machine Learning-Modellen. Studio (Classic) bietet nur eine eigenständige Drag & Drop-Umgebung.

Unsere Empfehlung für neue Benutzer ist die Wahl von Azure Machine Learning, um einen umfassenden Satz hochmoderner Machine Learning-Tools zu erhalten.

## <a name="quick-comparison"></a>Schnellvergleich

In der folgenden Tabelle sind einige wichtige Unterschiede zwischen Azure Machine Learning und Studio (Classic) zusammengefasst:

| | Machine Learning Studio (klassisch) | Azure Machine Learning |
|---| --- | --- |
| Drag & Drop-Oberfläche | Unterstützt | Unterstützt: [Azure Machine Learning-Designer (Vorschau)](concept-designer.md) <br/>(Setzt einen Unternehmensarbeitsbereich voraus) | 
| Experiment | Skalierbar (Limit für Trainingsdaten: 10 GB) | Skalieren mit Computeziel |
| Trainieren von Computezielen | Proprietäres Computeziel, nur CPU-Unterstützung | Viele anpassbare [Computeziele für das Training](concept-compute-target.md#train). GPU- und CPU-Unterstützung | 
| Computeziele für die Bereitstellung | Proprietäres Webdienstformat, nicht anpassbar | Viele anpassbare [Computeziele für die Bereitstellung](concept-compute-target.md#deploy). GPU- und CPU-Unterstützung |
| ML-Pipeline | Nicht unterstützt | Erstellung von flexiblen modularen [Pipelines](concept-ml-pipelines.md) zum Automatisieren von Workflows |
| MLOps | Einfache Modellverwaltung und -bereitstellung | Entitätsversionierung (Modell, Daten, Workflows), Workflowautomatisierung, Integration in CICD-Tools [und mehr](concept-model-management-and-deployment.md) |
| Modellformat | Proprietäres Format, nur Studio (klassisch) | Mehrere unterstützte Formate, je nach Typ des Trainingsauftrags |
| Automatisiertes Modelltraining und Optimieren von Hyperparametern |  Nicht unterstützt | [Unterstützung im SDK und visuellen Arbeitsbereich](concept-automated-ml.md) | 
| Datendrifterkennung | Nicht unterstützt | [Unterstützung im SDK und visuellen Arbeitsbereich](how-to-monitor-datasets.md) |


## <a name="migrate-from-machine-learning-studio-classic"></a>Migrieren aus Machine Learning Studio (Classic)

Derzeit besteht keine Möglichkeit, Ressourcen von Studio (Classic) zu Azure Machine Learning-Designer (Vorschau) zu migrieren. Benutzer von Studio (Classic) können ihre Machine Learning-Ressourcen weiterhin verwenden. Wir raten aber allen Benutzern, den Designer zu verwenden. Er verfügt über eine vertraute Drag & Drop-Benutzeroberfläche mit verbessertem Workflow **und** bietet Skalierbarkeit, Versionskontrolle und Unternehmenssicherheit.

## <a name="get-started-with-azure-machine-learning"></a>Erste Schritte mit Azure Machine Learning

Die folgenden Ressourcen dienen Ihnen als Hilfe beim Einstieg in Azure Machine Learning. 

- Lesen Sie die [Übersicht über Azure Machine Learning](overview-what-is-azure-ml.md).

- Erstellen Sie Ihr [erstes Experiment mit dem Python SDK](tutorial-1st-experiment-sdk-setup.md).

- [Erstellen Sie Ihre erste Designer-Pipeline](tutorial-designer-automobile-price-train-score.md) zum Vorhersagen von Fahrzeugpreisen.

![Azure Machine Learning-Designer: Beispiel](media/concept-designer/designer-drag-and-drop.gif)

## <a name="next-steps"></a>Nächste Schritte

Zusätzlich zu den Drag & Drop-Funktionen im Designer verfügt Azure Machine Learning noch über weitere Tools:  
  + [Verwenden von Python-Notebooks zum Trainieren und Bereitstellen von ML-Modellen](tutorial-1st-experiment-sdk-setup.md)
  + [Verwenden von R Markdown zum Trainieren und Bereitstellen von ML-Modellen](tutorial-1st-r-experiment.md) 
  + [Verwenden von automatisiertem Machine Learning zum Trainieren und Bereitstellen von ML-Modellen](tutorial-first-experiment-automated-ml.md)  
  + [Verwenden der Machine Learning-CLI zum Trainieren und Bereitstellen eines Modells](tutorial-train-deploy-model-cli.md)

