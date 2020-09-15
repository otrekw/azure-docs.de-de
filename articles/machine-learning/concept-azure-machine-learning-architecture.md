---
title: Architektur und wichtige Konzepte
titleSuffix: Azure Machine Learning
description: Enthält Informationen zu Architektur, Begriffen und Konzepten von Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 08/20/2020
ms.custom: seoapril2019, seodec18
ms.openlocfilehash: c3abd6a57eac851a5440ecdef6185cb310305434
ms.sourcegitcommit: d7352c07708180a9293e8a0e7020b9dd3dd153ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2020
ms.locfileid: "89146775"
---
# <a name="how-azure-machine-learning-works-architecture-and-concepts"></a>So funktioniert Azure Machine Learning: Architektur und Konzepte

Enthält Informationen zu Architektur und Konzepten von [Azure Machine Learning](overview-what-is-azure-ml.md).  In diesem Artikel erhalten Sie ein allgemeines Verständnis der Komponenten und deren Zusammenarbeit, um das Entwickeln, Bereitstellen und Warten von Machine Learning-Modellen zu unterstützen.

## <a name="workspace"></a><a name="workspace"></a> Arbeitsbereich

Ein [Arbeitsbereich für maschinelles Lernen](concept-workspace.md) ist die Ressource der obersten Ebene für Azure Machine Learning.

:::image type="content" source="media/concept-azure-machine-learning-architecture/architecture.svg" alt-text="Diagramm: Azure Machine Learning-Architektur eines Arbeitsbereichs und seiner Komponenten":::

Der Arbeitsbereich ist der zentrale Ort für:

* Verwalten von Ressourcen, die Sie für Training und Bereitstellung von Modellen wie [Berechnungen](#compute-instance) verwenden.
* Speichern von Ressourcen, die Sie erstellen, wenn Sie Azure Machine Learning verwenden, einschließlich:
  * [Umgebungen](#environments)
  * [Experimente](#experiments)
  * [Pipelines](#ml-pipelines)
  * [Datasets](#datasets-and-datastores)
  * [Modelle](#models)
  * [Endpunkte](#endpoints)

Ein Arbeitsbereich enthält andere Azure-Ressourcen, die er verwendet:

+ [Azure Container Registry (ACR)](https://azure.microsoft.com/services/container-registry/): Registriert die Docker-Container, die Sie während des Trainings und bei der Modellbereitstellung verwenden. Um Kosten zu minimieren, wird ACR nur erstellt, wenn Bereitstellungsimages erstellt werden.
+ [Azure Storage-Konto](https://azure.microsoft.com/services/storage/): Wird als Standarddatenspeicher für den Arbeitsbereich verwendet.  Jupyter-Notebooks, die mit Ihren Azure Machine Learning-Compute-Instanzen verwendet werden, werden ebenfalls hier gespeichert.
+ [Azure Application Insights](https://azure.microsoft.com/services/application-insights/): Speichert Überwachungsinformationen zu Ihren Modellen.
+ [Azure Key Vault](https://azure.microsoft.com/services/key-vault/): Speichert Geheimnisse, die von Computezielen verwendet werden, sowie andere vertrauliche Informationen, die vom Arbeitsbereich benötigt werden.

Sie können einen Arbeitsbereich mit anderen Benutzern teilen.

## <a name="computes"></a>Berechnungen

<a name="compute-targets"></a> Ein [Computeziel](concept-compute-target.md) ist ein Computer oder eine Computergruppe, wo Ihr Trainingsskript ausgeführt oder Ihre Dienstbereitstellung gehostet wird. Sie können Ihren lokalen Computer oder eine Remotecomputeressource als Computeziel verwenden.  Mit Computezielen können Sie das Training auf dem lokalen Computer starten und dann horizontal auf die Cloud hochskalieren, ohne Ihr Trainingsskript zu ändern.

Azure Machine Learning führt zwei vollständig verwaltete, cloudbasierte virtuelle Computer (VM) ein, die für Machine Learning-Aufgaben konfiguriert sind:

* <a name="compute-instance"></a> **Compute-Instanz**: Eine Compute-Instanz ist eine VM, die mehrere für maschinelles Lernen installierte Tools und Umgebungen umfasst. Eine Compute-Instanz wird primär für Ihre Entwicklungsarbeitsstation verwendet.  Sie können ohne Setup mit der Ausführung von Beispiel-Notebooks beginnen. Eine Compute-Instanz kann auch als Computeziel für Trainings- und Rückschlussaufträge verwendet werden.

* **Computecluster**: Computecluster sind Cluster von VMs, in denen mehrere Knoten skaliert werden können. Computecluster eignen sich besser für Computeziele für große Aufträge und Produktion.  Beim Übermitteln eines Auftrags wird der Cluster automatisch hochskaliert.  Verwenden Sie ihn als Trainingscomputeziel oder zur Dev/Test-Bereitstellung.

Weitere Informationen zum Trainieren von Computezielen finden Sie unter [Trainieren von Computezielen](concept-compute-target.md#train).  Weitere Informationen zur Bereitstellung von Computezielen finden Sie unter [Bereitstellungsziele](concept-compute-target.md#deploy).

## <a name="datasets-and-datastores"></a>Datasets und Datenspeicher

[**Azure Machine Learning-Datasets**](concept-data.md#datasets) erleichtern Ihnen den Zugriff auf Ihre Daten und die Arbeit damit. Durch Erstellen eines Datasets erstellen Sie einen Verweis auf den Speicherort der Datenquelle sowie eine Kopie der zugehörigen Metadaten. Da die Daten an ihrem bisherigen Speicherort verbleiben, entstehen Ihnen keine zusätzlichen Speicherkosten und Sie riskieren nicht die Integrität Ihrer Datenquellen.

Weitere Informationen finden Sie unter [Erstellen und Registrieren von Azure Machine Learning Datasets](how-to-create-register-datasets.md).  Weitere Beispiele für die Verwendung von Datasets finden Sie in den [Beispielnotebooks](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/work-with-data/datasets-tutorial).

Datasets verwenden [Datenspeicher](concept-data.md#datastores), um eine sichere Verbindung mit ihren Azure-Speicherdiensten herzustellen. Datenspeicher speichern Verbindungsinformationen ohne Ihre Anmeldeinformationen für die Authentifizierung und die Integrität Ihrer ursprünglichen Datenquelle zu gefährden. Darin werden Verbindungsinformationen wie Ihre Abonnement-ID und die Tokenautorisierung in Ihrer mit dem Arbeitsbereich verknüpften Key Vault-Instanz gespeichert, damit Sie sicher auf Ihren Speicher zuzugreifen können, ohne diese Informationen in Ihren Skripts hartcodieren zu müssen.

## <a name="environments"></a>Umgebungen

[Arbeitsbereich](#workspace) > **Umgebungen**

Eine [Umgebung](concept-environments.md) ist die Kapselung der Umgebung, in der das Training oder die Bewertung Ihres Machine Learning-Modells stattfindet. Die Umgebung gibt die Python-Pakete, Umgebungsvariablen und Softwareeinstellungen bezüglich Ihrer Trainings- und Bewertungsskripts an.  

Codebeispiele finden Sie im Abschnitt „Verwalten von Umgebungen“ unter [Verwenden von Umgebungen](how-to-use-environments.md#manage-environments).

## <a name="experiments"></a>Experimente

[Arbeitsbereich](#workspace) > **Experimente**

Ein Experiment ist eine Gruppierung vieler Ausführungen aus einem bestimmten Skript. Es gehört immer zu einem Arbeitsbereich. Beim Übermitteln einer Ausführung geben Sie einen Experimentnamen an. Die Informationen für die Ausführung werden unter diesem Experiment gespeichert. Wenn der Name nicht vorhanden ist, wenn Sie ein Experiment übermitteln, wird automatisch ein neues Experiment erstellt.
  
Ein Beispiel zum Verwenden eines Experiments finden Sie im [Tutorial: Trainieren Ihres ersten Modells](tutorial-1st-experiment-sdk-train.md).

### <a name="runs"></a>Ausführungen

[Arbeitsbereich](#workspace) > [Experimente](#experiments) > **Ausführen**

Eine Ausführung ist eine einzelne Ausführung eines Trainingsskripts. Ein Experiment enthält in der Regel mehrere Durchläufe.

Azure Machine Learning zeichnet alle Ausführungen auf und speichert die folgenden Informationen im Experiment:

* Metadaten zur Ausführung (Zeitstempel, Dauer usw.)
* Metriken, die von Ihrem Skript protokolliert werden
* Ausgabedateien, die automatisch vom Experiment gesammelt oder explizit von Ihnen hochgeladen werden
* Eine Momentaufnahme des Verzeichnisses, in dem Ihre Skripts enthalten sind, vor der Ausführung

Eine Ausführung wird ausgelöst, wenn Sie ein Skript zum Trainieren eines Modells übermitteln. Eine Ausführung kann über null oder mehr untergeordnete Elemente verfügen. Die Ausführung der obersten Ebene weist also unter Umständen zwei untergeordnete Ausführungen auf, die beide jeweils selbst eine untergeordnete Ausführung aufweisen können.

### <a name="run-configurations"></a>Laufzeitkonfigurationen

[Arbeitsbereich](#workspace) > [Experimente](#experiments) > [Ausführen](#runs) > **Ausführen der Konfiguration**

Eine Laufzeitkonfiguration ist ein Satz mit Anweisungen, mit denen definiert wird, wie ein Skript auf einem bestimmten Computeziel ausgeführt werden sollte. Die Konfiguration umfasst einen weiten Bereich von Verhaltensdefinitionen, z.B. die Vorgabe, ob eine vorhandene Python-Umgebung oder eine aus einer Spezifikation erstellte Conda-Umgebung verwendet werden soll.

Eine Laufzeitkonfiguration kann in einer Datei in dem Verzeichnis, in dem Ihr Trainingsskript enthalten ist, beständig gespeichert werden.   Alternativ kann sie als Objekt im Arbeitsspeicher erstellt und zum Übermitteln einer Ausführung verwendet werden.

Beispiele für Laufzeitkonfigurationen finden Sie unter [Verwenden eines Computeziels zum Trainieren Ihres Modells](how-to-set-up-training-targets.md).

### <a name="estimators"></a>Schätzfunktionen

Um das Training von Modellen mit beliebten Frameworks zu vereinfachen, können Sie mit der Klasse der Schätzfunktionen (Estimator) problemlos Laufzeitkonfigurationen erstellen. Sie können einen generischen [Kalkulator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py) erstellen und verwenden, um Trainingsskripts zu übermitteln, die ein beliebiges, von Ihnen ausgewähltes Learning-Framework verwenden (z. B. scikit-learn).

Weitere Informationen zu Schätzern finden Sie unter [Trainieren von ML-Modellen mit Kalkulatoren](how-to-train-ml-models.md).

### <a name="snapshots"></a>Momentaufnahmen

[Arbeitsbereich](#workspace) > [Experimente](#experiments) > [Ausführen](#runs) > **Momentaufnahme**

Beim Übermitteln einer Ausführung komprimiert Azure Machine Learning das Verzeichnis, in dem das Skript als ZIP-Datei enthalten ist, und sendet es an das Computeziel. Die ZIP-Datei wird dann extrahiert, und das Skript wird ausgeführt. Azure Machine Learning speichert die ZIP-Datei im Rahmen der Ausführungsaufzeichnung zudem als Momentaufnahme. Alle Benutzer mit Zugriff auf den Arbeitsbereich können eine Ausführungsaufzeichnung durchsuchen und die Momentaufnahme herunterladen.


### <a name="logging"></a>Protokollierung

Verwenden Sie beim Entwickeln Ihrer Lösung das Azure Machine Learning Python SDK in Ihrem Python-Skript, um beliebige Metriken zu protokollieren. Fragen Sie nach der Ausführung die Metriken ab, um zu ermitteln, ob von der Ausführung das Modell erzeugt wurde, das Sie bereitstellen möchten.


> [!NOTE]
> [!INCLUDE [amlinclude-info](../../includes/machine-learning-amlignore-gitignore.md)]

### <a name="git-tracking-and-integration"></a>Git-Nachverfolgung und -Integration

Wenn Sie eine Trainingsausführung starten, bei der das Quellverzeichnis ein lokales Git-Repository ist, werden Informationen über das Repository im Ausführungsverlauf gespeichert. Dies funktioniert für Ausführungen, die über eine Schätzfunktion, ML-Pipeline oder Skriptausführung übermittelt wurden. Dies funktioniert auch für Ausführungen, die aus dem SDK oder der Machine Learning-CLI übermittelt wurden.

Weitere Informationen finden Sie unter [Git-Integration für Azure Machine Learning](concept-train-model-git-integration.md).

## <a name="models"></a>Modelle

Im einfachsten Fall ist ein Modell ein Codeabschnitt, für den eine Eingabe verwendet wird, um eine Ausgabe zu erzeugen. Die Erstellung eines Machine Learning-Modells umfasst die Auswahl eines Algorithmus, die Bereitstellung der zugehörigen Daten und die [Optimierung von Hyperparametern](how-to-tune-hyperparameters.md). Das Training ist ein iterativer Prozess, bei dem ein trainiertes Modell mit den während des Trainingsprozesses erlernten Informationen erzeugt wird.

Sie können ein Modell verwenden, das außerhalb von Azure Machine Learning trainiert wurde. Oder Sie können ein Modell trainieren, indem Sie eine [Ausführung](#runs) eines [Experiments](#experiments) an ein [Computeziel ](#compute-targets) in Azure Machine Learning übermitteln. Sobald Sie ein fertiges Modell haben, [registrieren](#register-model) Sie es im Arbeitsbereich.

Azure Machine Learning ist „framework-agnostisch“. Wenn Sie ein Modell erstellen, können Sie alle gängigen Frameworks für maschinelles Lernen verwenden, wie Scikit-learn, XGBoost, PyTorch, TensorFlow und Chainer.

Ein Beispiel zum Trainieren eines Modells mittels Scikit-learn finden Sie im [Tutorial: Trainieren eines Bildklassifizierungsmodells mit Azure Machine Learning](tutorial-train-models-with-aml.md).


### <a name="model-registry"></a><a name="register-model"></a> Modellregistrierung

[Arbeitsbereich](#workspace) > **Modelle**

Mit der **Modellregistrierung** können Sie alle Modelle in Ihrem Azure Machine Learning-Arbeitsbereich verfolgen.

Die Modelle werden anhand des Namens und der Version identifiziert. Jedes Mal, wenn Sie ein Modell mit dem gleichen Namen wie ein bereits vorhandenes Modell registrieren, geht die Registrierung davon aus, dass es sich um eine neue Version handelt. Die Version wird inkrementiert, und das neue Modell wird unter demselben Namen registriert.

Bei der Registrierung des Modells können Sie zusätzliche Metadatentags bereitstellen und diese Tags dann beim Suchen nach Modellen verwenden.

> [!TIP]
> Ein registriertes Modell ist ein logischer Container für eine oder mehrere Dateien, aus denen Ihr Modell besteht. Wenn Sie beispielsweise ein Modell verwenden, das in mehreren Dateien gespeichert ist, können Sie diese als einzelnes Modell in Ihrem Azure Machine Learning-Arbeitsbereich registrieren. Nach der Registrierung können Sie das registrierte Modell dann herunterladen oder bereitstellen und alle Dateien empfangen, die registriert wurden.

Sie können kein registriertes Modell löschen, das von einer aktiven Bereitstellung verwendet wird.

Ein Beispiel für das Registrieren eines Modells finden Sie unter [Trainieren eines Bildklassifizierungsmodells mit Azure Machine Learning Service](tutorial-train-models-with-aml.md).

## <a name="deployment"></a>Bereitstellung

Sie stellen ein [registriertes Modell](#register-model) als Dienstendpunkt bereit. Sie benötigen folgende Komponenten:

* **Umgebung**. Diese Umgebung kapselt die Abhängigkeiten, die zur Ausführung Ihres Modells für Rückschlüsse erforderlich sind.
* **Bewertungscode**. Dieses Skript akzeptiert Anforderungen, bewertet die Anforderungen über Verwenden des Modells und gibt die Ergebnisse zurück.
* **Rückschlusskonfiguration**. Die Rückschlusskonfiguration gibt die Umgebung, das Eingabeskript und andere Komponenten an, die zur Ausführung des Modells als Dienst benötigt werden.

Weitere Informationen zu diesen Komponenten finden Sie unter [Bereitstellen von Modellen mit Azure Machine Learning](how-to-deploy-and-where.md).

### <a name="endpoints"></a>Endpunkte

[Arbeitsbereich](#workspace) > **Endpunkte**

Ein Endpunkt ist eine Instanziierung Ihres Modells in einem Webdienst, der in der Cloud gehostet werden kann, oder in einem IoT-Modul für Bereitstellungen von integrierten Diensten.

#### <a name="web-service-endpoint"></a>Webdienst-Endpunkt

Wenn Sie ein Modell als Webdienst bereitstellen, kann der Endpunkt auf Azure Container Instances, Azure Kubernetes Service oder FPGAs bereitgestellt werden. Sie erstellen den Dienst aus Ihrem Modell, dem Skript und zugeordneten Dateien. Diese werden in ein Basiscontainerimage eingefügt, das die Ausführungsumgebung für das Modell enthält. Das Image verfügt über einen HTTP-Endpunkt mit Lastenausgleich, der die an den Webdienst gesendeten Bewertungsanforderungen empfängt.

Sie können Application Insights-Telemetrie oder Modelltelemetrie aktivieren, um den Webdienst zu überwachen. Die Telemetriedaten sind nur für Sie zugänglich.  Sie werden in Ihren Application Insights- und Speicherkontoinstanzen gespeichert.

Wenn Sie die automatische Skalierung aktiviert haben, führt Azure für Ihre Bereitstellung automatisch eine Skalierung durch.

Ein Beispiel für die Bereitstellung eines Modells als Webdienst finden Sie unter [Bereitstellen eines Bildklassifizierungsmodells in Azure Container Instances](tutorial-deploy-models-with-aml.md).

#### <a name="iot-module-endpoints"></a>IoT-Modulendpunkte

Ein bereitgestellter IoT-Modulendpunkt ist ein Docker-Container, der Ihr Modell und das zugeordnete Skript oder die Anwendung sowie alle zusätzlichen Abhängigkeiten enthält. Sie stellen diese Module mit Azure IoT Edge auf Edge-Geräten bereit.

Wenn Sie die Überwachung aktiviert haben, erfasst Azure Telemetriedaten aus dem Modell im Azure IoT Edge-Modul. Die Telemetriedaten sind nur für Sie zugänglich und werden in Ihrer Speicherkontoinstanz gespeichert.

Azure IoT Edge stellt sicher, dass Ihr Modul ausgeführt wird, und überwacht das Gerät, auf dem es gehostet wird. 
## <a name="automation"></a>Automation

### <a name="azure-machine-learning-cli"></a>Azure Machine Learning-CLI 

Die [Azure Machine Learning-CLI](reference-azure-machine-learning-cli.md) ist eine Erweiterung der Azure CLI, eine plattformübergreifende Befehlszeilenschnittstelle für die Azure-Plattform. Diese Erweiterung bietet Befehle zum Automatisieren Ihrer Machine Learning-Aktivitäten.

### <a name="ml-pipelines"></a>ML-Pipelines

[Machine Learning-Pipelines](concept-ml-pipelines.md) werden zum Erstellen und Verwalten von Workflows verwendet, die Machine Learning-Phasen zusammenfügen. Eine Pipeline kann beispielsweise eine Datenaufbereitungs-, eine Modelltrainings-, eine Modellimplementierungs- und eine Rückschluss-/Bewertungsphase enthalten. Jede Phase kann mehrere Schritte umfassen, von denen wiederum jeder Schritt auf verschiedenen Computezielen unbeaufsichtigt ausgeführt werden kann. 

Pipelineschritte sind wiederverwendbar und können ohne erneute Ausführung der vorherigen Schritte ausgeführt werden, wenn sich die Ausgabe dieser Schritte nicht geändert hat. Beispielsweise können Sie ein Modell erneut trainieren, ohne teure Schritte zur Datenvorbereitung erneut auszuführen, wenn sich die Daten nicht geändert haben. Pipelines ermöglichen Data Scientists auch, bei der Arbeit an separaten Bereichen eines Machine Learning-Workflows zusammenzuarbeiten.

## <a name="interacting-with-your-workspace"></a>Interaktion mit Ihrem Arbeitsbereich

### <a name="studio"></a>Studio

[Azure Machine Learning Studio](https://ml.azure.com) stellt eine Webansicht aller Artefakte in Ihrem Arbeitsbereich bereit.  Sie können Ergebnisse und Details zu ihren Datasets, Experimenten, Pipelines, Modellen und Endpunkten anzeigen.  Sie können auch Computeressourcen und Datenspeicher in Studio verwalten.

In Studio greifen Sie auch auf die interaktiven Tools zu, die Teil von Azure Machine Learning sind:

+ [Azure Machine Learning-Designer (Vorschauversion)](concept-designer.md) zur Ausführung der Workflowschritte ohne Programmierung
+ Weboberfläche für [automatisiertes maschinelles Lernen](concept-automated-ml.md)
+ [Datenbezeichnungsprojekte](how-to-create-labeling-projects.md) zum Erstellen, Verwalten und Überwachen von Projekten zur Bezeichnung Ihrer Daten

### <a name="programming-tools"></a>Programmiertools

> [!IMPORTANT]
> Die unten markierten Tools (Vorschau) sind aktuell als öffentliche Vorschau verfügbar.
> Die Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

+  Interagieren Sie mit dem Dienst in einer beliebigen Python-Umgebung, indem Sie das [Azure Machine Learning SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) verwenden.
+ Interagieren Sie mit dem Dienst in einer beliebigen R-Umgebung, indem Sie das [Azure Machine Learning SDK für R](https://azure.github.io/azureml-sdk-for-r/reference/index.html) (Vorschauversion) verwenden.
+ Verwenden Sie die [Azure Machine Learning-CLI](https://docs.microsoft.com/azure/machine-learning/reference-azure-machine-learning-cli) zur Automatisierung.
+ Der [Many Models Solution Accelerator](https://aka.ms/many-models) (Preview) (Projektmappenbeschleuniger für viele Modelle (Vorschau)) baut auf Azure Machine Learning auf und ermöglicht Ihnen Training, Betrieb und Verwaltung von hunderten oder sogar tausenden von Machine Learning-Modellen.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den ersten Schritten mit Azure Machine Learning finden Sie unter:

* [Was ist Azure Machine Learning?](overview-what-is-azure-ml.md)
* [Erstellen eines Azure Machine Learning-Arbeitsbereichs](how-to-manage-workspace.md)
* [Tutorial (Teil 1): Trainieren eines Modells](tutorial-train-models-with-aml.md)
