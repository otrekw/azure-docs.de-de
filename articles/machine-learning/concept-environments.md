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
ms.date: 07/08/2020
ms.openlocfilehash: 437db3e1ad367a75d6c8d3a77b138bce79b5f61e
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86147130"
---
# <a name="what-are-azure-machine-learning-environments"></a>Was sind Azure Machine Learning-Umgebungen?
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning-Umgebungen geben die Python-Pakete, Umgebungsvariablen und Softwareeinstellungen bezüglich Ihrer Trainings- und Bewertungsskripts an. Sie geben auch Laufzeiten an (Python, Spark oder Docker). Die Umgebungen sind verwaltete und versionsverwaltete Entitäten innerhalb Ihres Machine Learning-Arbeitsbereichs, die reproduzierbare, überprüfbare und portierbare Machine Learning-Workflows über verschiedene Computeziele hinweg ermöglichen.

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
* [Verwenden der VS Code-Erweiterung](how-to-manage-resources-vscode.md#create-environment)

Bestimmte Codebeispiele finden Sie im Abschnitt „Erstellen einer Umgebung“ unter [Wiederverwenden von Umgebungen für Training und Bereitstellung](how-to-use-environments.md#create-an-environment). Umgebungen können auch problemlos über Ihren Arbeitsbereich verwaltet werden. Sie sollten folgende Funktionen umfassen:

* Umgebungen werden beim Übermitteln eines Experiments automatisch bei Ihrem Arbeitsbereich registriert. Sie können auch manuell registriert werden.
* Sie können Umgebungen aus Ihrem Arbeitsbereich abrufen, um sie zum Trainieren, Bereitstellen oder Vornehmen von Änderungen an der Umgebungsdefinition zu verwenden.
* Mit der Versionsverwaltung können Sie Änderungen an Ihren Umgebungen im Laufe der Zeit erkennen, was die Reproduzierbarkeit sicherstellt.
* Sie können Docker-Images automatisch aus Ihren Umgebungen erstellen.

Codebeispiele finden Sie im Abschnitt „Verwalten von Umgebungen“ unter [Wiederverwenden von Umgebungen für Training und Bereitstellung](how-to-use-environments.md#manage-environments).

## <a name="environment-building-caching-and-reuse"></a>Erstellen, Zwischenspeichern und Wiederverwenden von Umgebungen

Der Azure Machine Learning-Dienst erstellt Umgebungsdefinitionen in Docker-Images und Conda-Umgebungen. Außerdem werden die Umgebungen zwischengespeichert, sodass sie bei späteren Trainingsdurchläufen und Bereitstellungen von Dienstendpunkten wiederverwendet werden können.

### <a name="building-environments-as-docker-images"></a>Erstellen von Umgebungen als Docker-Images

Normalerweise ruft der Azure Machine Learning-Dienst beim ersten Übermitteln einer Ausführung in einer Umgebung eine [ACR-Buildaufgabe](https://docs.microsoft.com/azure/container-registry/container-registry-tasks-overview) in der Azure Container Registry (ACR) auf, die dem Arbeitsbereich zugeordnet ist. Das erstellte Docker-Image wird dann in der ACR des Arbeitsbereichs zwischengespeichert. Zu Beginn der Ausführung wird das Image vom Computeziel abgerufen.

Die Imageerstellung besteht aus zwei Schritten:

 1. Herunterladen eines Basisimages und Ausführen beliebiger Docker-Schritte
 2. Erstellen einer Conda-Umgebung gemäß den in der Umgebungsdefinition angegebenen Conda-Abhängigkeiten.

Der zweite Schritt entfällt, wenn Sie [vom Benutzer verwaltete Abhängigkeiten](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.pythonsection?view=azure-ml-py) angeben. In diesem Fall sind Sie für die Installation von Python-Paketen verantwortlich, indem Sie diese in Ihr Basisimage einbeziehen oder im ersten Schritt benutzerdefinierte Docker-Schritte festlegen. Sie sind auch dafür verantwortlich, den richtigen Speicherort für die ausführbare Python-Datei anzugeben.

### <a name="image-caching-and-reuse"></a>Zwischenspeicherung und Wiederverwendung von Images

Wenn Sie dieselbe Umgebungsdefinition für eine weitere Ausführung verwenden, verwendet der Azure Machine Learning-Dienst das zwischengespeicherte Image aus der ACR des Arbeitsbereichs wieder. 

Um die Details eines zwischengespeicherten Images anzuzeigen, verwenden Sie die Methode [Environment.get_image_details](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#get-image-details-workspace-).

Um zu bestimmen, ob ein zwischengespeichertes Image wiederverwendet oder ein neues erstellt werden soll, berechnet der Dienst [einen Hashwert](https://en.wikipedia.org/wiki/Hash_table) aus der Umgebungsdefinition und vergleicht ihn mit den Hashwerten bestehender Umgebungen. Der Hash basiert auf:
 
 * Eigenschaftswert des Basisimages
 * Eigenschaftswert für benutzerdefinierte Docker-Schritte
 * Liste der Python-Pakete in der Conda-Definition
 * Liste der Pakete in der Spark-Definition 

Der Hash hängt nicht vom Namen oder der Version der Umgebung ab. Änderungen der Umgebungsdefinition, z. B. das Hinzufügen oder Entfernen eines Python-Pakets oder die Änderung der Paketversion, führen zu einer Änderung des Hashwerts und lösen eine Neuerstellung des Images aus. Wenn Sie jedoch Ihre Umgebung einfach umbenennen oder eine neue Umgebung mit den genauen Eigenschaften und Paketen einer vorhandenen Umgebung erstellen, bleibt der Hashwert unverändert und das zwischengespeicherte Image wird verwendet.

Betrachten Sie das folgende Diagramm, das drei Umgebungsdefinitionen zeigt. Zwei von ihnen haben unterschiedliche Namen und Versionen, aber ein identisches Basisimage und identische Python-Pakete. Sie haben denselben Hashwert und entsprechen daher demselben zwischengespeicherten Image. Die dritte Umgebung weist verschiedene Python-Pakete und -Versionen auf und entspricht daher einem anderen zwischengespeicherten Image.

![Diagramm zur Zwischenspeicherung der Umgebung als Docker-Images](./media/concept-environments/environment-caching.png)

>[!IMPORTANT]
> Wenn Sie eine Umgebung mit losgelöster Paketabhängigkeit erstellen, z. B. ```numpy```, wird diese Umgebung weiterhin die Paketversion verwenden, _die zum Zeitpunkt der Erstellung der Umgebung installiert war_. Außerdem wird jede zukünftige Umgebung mit passender Definition weiterhin die alte Version verwenden. 

Geben Sie zum Aktualisieren des Pakets eine Versionsnummer an, um eine Neuerstellung des Images zu erzwingen, z. B. ```numpy==1.18.1```. Beachten Sie, dass neue Abhängigkeiten, einschließlich geschachtelter Abhängigkeiten, installiert werden, die ein zuvor funktionierendes Szenario unterbrechen könnten.

> [!WARNING]
>  Die Methode [Environment.build](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#build-workspace--image-build-compute-none-) erstellt das zwischengespeicherte Image neu, mit dem möglichen Nebeneffekt, dass losgelöste Pakete aktualisiert werden und die Reproduzierbarkeit für alle Umgebungsdefinitionen, die diesem zwischengespeicherten Image entsprechen, unterbrochen wird.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie [Umgebungen in Azure Machine Learning erstellen und verwenden](how-to-use-environments.md).
* Die [Umgebungsklasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py) finden Sie in der Referenzdokumentation des Python SDK.
* [Umgebungen](https://azure.github.io/azureml-sdk-for-r/reference/index.html#section-environments) finden Sie in der Referenzdokumentation des R SDK.
