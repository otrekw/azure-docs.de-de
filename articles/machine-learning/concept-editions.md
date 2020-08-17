---
title: Die Editionen Enterprise und Basic für Azure Machine Learning
description: Informieren Sie sich über die Unterschiede der verschiedenen Azure Machine Learning-Editionen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: j-martens
ms.author: jmartens
ms.date: 06/11/2020
ms.openlocfilehash: aa754868677802b7d0000045f22090fbca62d9b6
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "87927418"
---
# <a name="enterprise-and-basic-editions-of-azure-machine-learning"></a>Die Editionen Enterprise und Basic für Azure Machine Learning 

Azure Machine Learning bietet zwei Editionen auf Ihre Anforderungen an maschinelles Lernen abgestimmte Editionen. Mit diesen Editionen wird ermittelt, welche Machine Learning-Tools für Entwickler und Data Scientists in ihrem Arbeitsbereich verfügbar sind.

## <a name="choose-an-edition"></a>Auswählen einer Edition

Sie weisen die Edition jeweils zu, wenn Sie einen Arbeitsbereich erstellen. Kunden sind während dieses Zeitraums für Kosten für Computevorgänge und für andere Azure-Ressourcen verantwortlich. Erfahren Sie mehr unter [Verwalten von Kosten für Azure Machine Learning](concept-plan-manage-cost.md).

Informieren Sie sich über das [Durchführen eines Upgrades eines Basic-Arbeitsbereichs auf die Enterprise-Edition](how-to-manage-workspace.md#upgrade). 

## <a name="whats-in-each-edition"></a>Was haben die Editionen zu bieten?

### <a name="data-for-machine-learning-capabilities"></a>Daten für Machine Learning-Funktionen  

| Funktionen                     | Edition                 |
|------------------------------------------------------------------------------------|:-----------:|
| Beschriftung: [Erstellen und Verwalten von Beschriftungsprojekten](tutorial-labeling.md) in Studio (Web)                                                | All                     |
| Beschriftung: Beschriftungsersteller in Studio (Web)                                    | All                     |
| Beschriftung: Verwenden privater Mitarbeiter                               | All                     |
| Beschriftung: [ML-gestützte Bildklassifizierung und Objekterkennung](how-to-label-images.md)                  | Nur Enterprise Edition |
| Datasets und Datenspeicher: Erstellen und Verwalten in Python                       | All                     |
| Datasets und Datenspeicher: Erstellen und Verwalten in Studio (Web)                         | All                     |
| Drift: Anzeigen und Verwalten von Datasetmonitoren in Python                           | All                     |
| Drift: Anzeigen und Verwalten von Datasetmonitoren in Studio (Web)                            | Nur Enterprise Edition |


<br/>
<br/>

### <a name="automated-training-capabilities-automl"></a>Automatisierte Trainingsfunktionen (AutoML)

| Funktionen    | Edition                 |
|------------------------------------------------------------------------------------|:-----------:|
| Erstellen und Ausführen von [AutoML-Experimenten in Notebooks](how-to-configure-auto-train.md)               | All                     |
| Erstellen und Ausführen von [AutoML-Experimenten in Studio (Web)](how-to-use-automated-ml-for-ml-models.md)   | Nur Enterprise Edition |
| Branchenführende AutoML-Vorhersagefunktionen             | Nur Enterprise Edition |
| Unterstützung für Deep Learning und andere erweiterte Lernmoduloptionen | Nur Enterprise Edition |
| Unterstützung von Klassifizierungs- und Regressionsaufgaben mit großen Datenmengen (bis zu 100 GB)                     | Nur Enterprise Edition |


<br/>
<br/>

### <a name="responsible-machine-learning"></a>Verantwortungsvolles Machine Learning

| Funktionen    | Edition                 |
|------------------------------------------------------------------------------------|:-----------:|
| [Erklärbarkeit von Modellen](how-to-machine-learning-interpretability-automl.md)                                              | All                     |
| [Differenzieller Datenschutz](how-to-differential-privacy.md)                          | All                     |
| Benutzerdefinierte Tags zum Implementieren von Datenblättern    | All                     |
| Fairness bei der AzureML-Integration                                      | All                     |

<br/>
<br/>


### <a name="build-and-train-capabilities"></a>Erstellungs- und Trainingsfunktionen

| Funktionen    | Edition                 |
|------------------------------------------------------------------------------------|:-----------:|
| Visual Studio Code-Integration                                                     | All                     |
| Vertiefendes Lernen                                                             | All                     |
| Benutzeroberfläche zum Experimentieren                                                                 | All                     |
| Jupyter, JupyterLab-Integration                                                    | All                     |
| Python SDK-Unterstützung                                                                 | All                     |
| R SDK-Unterstützung                                                                      | All                     |
| ML-Pipelines: Erstellen, Ausführen und Veröffentlichen in Python                           | All                     |
| ML-Pipelines: Erstellen, Bearbeiten und Löschen geplanter Ausführungen von Pipelines in Python| All                     |
| ML-Pipelines: Erstellen von Endpunkten von Pipelines in Python SDK                                   | All                     |
| ML-Pipelines: Anzeigen von Ausführungsdetails in Studio (Web)                                              | All                     |
| ML-Pipelines: Erstellen, Ausführen, Visualisieren und Veröffentlichen im Designer                  | Nur Enterprise Edition |
| ML-Pipelines: Erstellen von Endpunkten von Pipelines im Designer | Nur Enterprise Edition |
| Verwaltete Compute-Instanzen für integrierte Notebooks                                 | All                     |


<br/>
<br/>

### <a name="deployment-and-model-management-capabilities"></a>Bereitstellungs- und Modellverwaltungsfunktionen

| Funktionen                            | Edition                 |
|------------------------------------------------------------------------------------|:-----------:|
| Die Azure DevOps-Erweiterung für Machine Learning und die Azure ML CLI                 | All                     |
| [Event Grid-Integration](how-to-use-event-grid.md)                                                             | All                     |
| Integrieren von Azure Stream Analytics mit Azure Machine Learning                       | All                     |
| Erstellen von ML-Pipelines im SDK                                                         | All                     |
| Batchrückschluss                                                                  | All                     |
| Auf FPGA basierende hardwarebeschleunigte Modelle                                             | All                     |
| Erstellung von Modellprofilen                                                                    | All                     |

<br/>
<br/>

### <a name="security-governance-and-control-capabilities"></a>Sicherheit, Governance und Steuerungsfunktionen

| Funktionen     | Edition                 |
|------------------------------------------------------------------------------------|:-----------:|
| Support für [rollenbasierte Zugriffssteuerung von Azure (Azure RBAC)](how-to-assign-roles.md)                                           | All                     |
| Unterstützung von [virtuellen Netzwerken (VNETs)](how-to-enable-virtual-network.md) für Compute                                         | All                     |
| Bewerten der Endpunktauthentifizierung                                                    | All                     |
| [Arbeitsbereich mit Private Link](how-to-configure-private-link.md)                                                            | All                     |
| [Kontingentverwaltung](how-to-manage-quotas.md) in Arbeitsbereichen                                                 | Nur Enterprise Edition |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den verfügbaren Optionen von Azure Machine Learning finden Sie auf der [Seite mit der Übersicht über Editionen und Preise](https://azure.microsoft.com/pricing/details/machine-learning/). 

Informieren Sie sich über das [Durchführen eines Upgrades eines Basic-Arbeitsbereichs auf die Enterprise-Edition](how-to-manage-workspace.md#upgrade). 
