---
title: Informationen zu Azure Machine Learning-Umgebungen
titleSuffix: Azure Machine Learning
description: In diesem Artikel erfahren Sie mehr über die Vorteile von Machine Learning-Umgebungen, die reproduzierbare, überprüfbare und portierbare Machine Learning-Abhängigkeitsdefinitionen über verschiedene Computeziele hinweg ermöglichen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: trbye
author: trevorbye
ms.date: 01/06/2020
ms.openlocfilehash: 8906299cc9e2c000dab2ac9d2a345d9aaf238260
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2020
ms.locfileid: "76045861"
---
# <a name="what-are-azure-machine-learning-environments"></a>Was sind Azure Machine Learning-Umgebungen?
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning-Umgebungen geben die Python-Pakete, Umgebungsvariablen und Softwareeinstellungen bezüglich Ihrer Trainings- und Bewertungsskripts an. Sie geben auch Laufzeiten an (Python, Spark oder Docker). Dabei handelt es sich um verwaltete und versionsverwaltete Entitäten innerhalb Ihres Machine Learning-Arbeitsbereichs, die reproduzierbare, überprüfbare und portierbare Machine Learning-Workflows über verschiedene Computeziele hinweg ermöglichen.

Sie können ein `Environment`-Objekt auf Ihrem lokalen Computer verwenden, um Folgendes zu erreichen:
* Entwickeln Sie Ihr Trainingsskript.
* Verwenden Sie dieselbe Umgebung in Azure Machine Learning Compute für das skalierte Modelltraining wieder.
* Stellen Sie Ihr Modell in derselben Umgebung bereit.

Das folgende Diagramm veranschaulicht, wie Sie ein einzelnes `Environment`-Objekt sowohl in Ihrer Ausführungskonfiguration, für Trainingszwecke als auch in Ihrer Rückschluss- und Bereitstellungskonfiguration für Webdienstbereitstellungen verwenden können.

![Diagramm einer Umgebung im Machine Learning-Workflow](./media/concept-environments/ml-environment.png)

## <a name="types-of-environments"></a>Typen von Umgebungen

Umgebungen lassen sich weitgehend in drei Kategorien unterteilen: *zusammengestellt*, *benutzerverwaltet* und *systemverwaltet*.

Zusammengestellte Umgebungen werden von Azure Machine Learning bereitgestellt und sind standardmäßig in Ihrem Arbeitsbereich verfügbar. Sie enthalten Sammlungen von Python-Paketen und -Einstellungen, die Ihnen helfen, verschiedene Machine Learning-Frameworks zu starten. 

In benutzerverwalteten Umgebungen sind Sie für die Einrichtung Ihrer Umgebung und die Installation aller Pakete, die Ihr Trainingsskript auf dem Computeziel benötigt, verantwortlich. Conda wird Ihre Umgebung nicht überprüfen oder Installationen für Sie vornehmen. Beim Definieren einer eigenen Umgebung müssen Sie `azureml-defaults` mit der Version `>= 1.0.45` als Pip-Abhängigkeit auflisten. Dieses Paket enthält die erforderlichen Funktionen zum Hosten des Modells als Webdienst.

Sie verwenden systemverwaltete Umgebungen, wenn Sie möchten, dass [Conda](https://conda.io/docs/) die Python-Umgebung und die Skriptabhängigkeiten für Sie verwalten soll. Der Dienst setzt diesen Typ von Umgebung standardmäßig voraus, da er für Remotecomputeziele, die nicht manuell konfiguriert werden können, sehr praktisch ist.

## <a name="create-and-manage-environments"></a>Erstellen und Verwalten von Umgebungen

Sie können Umgebungen folgendermaßen erstellen:

* Durch Definieren von neuen `Environment`-Objekten, entweder mithilfe einer kuratierten Umgebung oder durch Definieren eigener Abhängigkeiten.
* Durch Verwenden vorhandener `Environment`-Objekte aus Ihrem Arbeitsbereich. Dieser Ansatz ermöglicht konsistente und reproduzierbare Abhängigkeiten.
* Durch Importieren aus einer vorhandenen Anaconda-Umgebungsdefinition.
* Verwenden der Azure Machine Learning-CLI

Bestimmte Codebeispiele finden Sie im Abschnitt „Erstellen einer Umgebung“ unter [Wiederverwenden von Umgebungen für Training und Bereitstellung](how-to-use-environments.md#create-an-environment). Umgebungen können auch problemlos über Ihren Arbeitsbereich verwaltet werden. Sie sollten folgende Funktionen umfassen:

* Umgebungen werden beim Übermitteln eines Experiments automatisch bei Ihrem Arbeitsbereich registriert. Sie können auch manuell registriert werden.
* Sie können Umgebungen aus Ihrem Arbeitsbereich abrufen, um sie zum Trainieren, Bereitstellen oder Vornehmen von Änderungen an der Umgebungsdefinition zu verwenden.
* Mit der Versionsverwaltung können Sie Änderungen an Ihren Umgebungen im Laufe der Zeit erkennen, was die Reproduzierbarkeit sicherstellt.
* Sie können Docker-Images automatisch aus Ihren Umgebungen erstellen.

Codebeispiele finden Sie im Abschnitt „Verwalten von Umgebungen“ unter [Wiederverwenden von Umgebungen für Training und Bereitstellung](how-to-use-environments.md#manage-environments).

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie [Umgebungen in Azure Machine Learning erstellen und verwenden](how-to-use-environments.md).
* Die [Umgebungsklasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py) finden Sie in der Referenzdokumentation des Python SDK.
* [Umgebungen](https://azure.github.io/azureml-sdk-for-r/reference/index.html#section-environments) finden Sie in der Referenzdokumentation des R SDK.
