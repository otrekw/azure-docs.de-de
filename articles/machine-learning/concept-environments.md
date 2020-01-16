---
title: Was sind ML-Umgebungen?
titleSuffix: Azure Machine Learning
description: In diesem Artikel erfahren Sie mehr über die Vorteile von Machine Learning-Umgebungen, die reproduzierbare, überprüfbare und portierbare Machine Learning-Abhängigkeitsdefinitionen über verschiedene Computeziele hinweg ermöglichen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: trbye
author: trevorbye
ms.date: 01/06/2020
ms.openlocfilehash: ad520c7e6503f28de0bd5538662c223575a078fa
ms.sourcegitcommit: 2f8ff235b1456ccfd527e07d55149e0c0f0647cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/07/2020
ms.locfileid: "75692626"
---
# <a name="what-are-azure-machine-learning-environments"></a>Was sind Azure Machine Learning-Umgebungen?
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Umgebungen geben die Python-Pakete, Umgebungsvariablen und Softwareeinstellungen bezüglich Ihrer Trainings- und Bewertungsskripts sowie die Laufzeiten (Python, Spark oder Docker) an. Dabei handelt es sich um verwaltete und versionsverwaltete Entitäten innerhalb Ihres Azure Machine Learning-Arbeitsbereichs, die reproduzierbare, überprüfbare und portierbare Machine Learning-Workflows über verschiedene Computeziele hinweg ermöglichen.

Sie können ein Umgebungsobjekt auf Ihrem lokalen Compute verwenden, um Ihr Trainingsskript zu entwickeln, dieselbe Umgebung auf Azure Machine Learning Compute für das skalierte Modelltraining wiederverwenden und Ihr Modell sogar mit dieser selben Umgebung bereitstellen.

Im Folgenden wird veranschaulicht, dass dasselbe Umgebungsobjekt sowohl in Ihrer Laufzeitkonfiguration für das Training als auch in der Rückschluss- und Bereitstellungskonfiguration für Webdienstbereitstellungen verwendet werden kann.

![Diagramm der Umgebung im Machine Learning-Workflow](./media/concept-environments/ml-environment.png)

## <a name="types-of-environments"></a>Typen von Umgebungen

Umgebungen lassen sich weitgehend in drei Kategorien unterteilen: **zusammengestellt**, **benutzerverwaltet** und **systemverwaltet**.

Zusammengestellte Umgebungen werden von Azure Machine Learning bereitgestellt und sind standardmäßig in Ihrem Arbeitsbereich verfügbar. Sie enthalten Sammlungen von Python-Paketen und -Einstellungen, die Ihnen helfen, verschiedene Machine Learning-Frameworks zu starten. 

Bei benutzerverwalteten Umgebungen sind Sie für die Einrichtung Ihrer Umgebung und die Installation aller Pakete, die Ihr Trainingsskript auf dem Computeziel benötigt, selbst verantwortlich. Conda wird Ihre Umgebung nicht überprüfen oder Installationen für Sie vornehmen. Beachten Sie, dass Sie beim Definieren einer eigenen Umgebungsdatei `azureml-defaults` mit einer Version `>= 1.0.45` als Pip-Abhängigkeit auflisten müssen. Dieses Paket enthält die erforderlichen Funktionen zum Hosten des Modells als Webdienst.

Systemverwaltete Umgebungen werden verwendet, wenn Sie möchten, dass [Conda](https://conda.io/docs/) die Python-Umgebung und die Skriptabhängigkeiten für Sie verwalten soll. Der Dienst setzt diesen Typ von Umgebung standardmäßig voraus, da er für Remotecomputeziele, die nicht manuell konfiguriert werden können, sehr praktisch ist.

## <a name="creating-and-managing-environments"></a>Erstellen und Verwalten von Umgebungen

Umgebungen können wie folgt erstellt werden:

* Durch Definieren von neuen `Environment`-Objekten entweder mithilfe einer kuratierten Umgebung oder durch Definieren eigener Abhängigkeiten
* Durch Verwenden vorhandener `Environment`-Objekte aus Ihrem Arbeitsbereich. Dies ermöglicht konsistente und reproduzierbare Abhängigkeiten.
* Durch Importieren aus einer vorhandenen Anaconda-Umgebungsdefinition.

Informationen zu bestimmten Codebeispielen finden Sie in der [Anleitung](how-to-use-environments.md#create-an-environment). Umgebungen können auch problemlos über Ihren Arbeitsbereich verwaltet werden und umfassen die folgenden Funktionen:

* Umgebungen werden beim Übermitteln eines Experiments automatisch bei Ihrem Arbeitsbereich registriert. Sie können auch manuell registriert werden.
* Rufen Sie Umgebungen aus Ihrem Arbeitsbereich ab, und verwenden Sie sie zum Trainieren, Bereitstellen oder Vornehmen von Änderungen an der Umgebungsdefinition.
* Mit der Versionsverwaltung können Sie Änderungen an ihren Umgebungen im Zeitverlauf anzeigen und die Reproduzierbarkeit sicherstellen.
* Automatisches Erstellen von Docker-Images aus Ihren Umgebungen

Weitere Informationen finden Sie im Abschnitt [Verwalten von Umgebungen](how-to-use-environments.md#manage-environments) in der Anleitung für Codebeispiele.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie [Umgebungen in Azure Machine Learning erstellen und verwenden](how-to-use-environments.md).
* Die [Umgebungsklasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py) finden Sie in den Referenzdokumenten des Python SDK.
* Weitere Informationen zu [Umgebungen](https://azure.github.io/azureml-sdk-for-r/reference/index.html#section-environments) finden Sie in den Referenzdokumenten des R SDK.