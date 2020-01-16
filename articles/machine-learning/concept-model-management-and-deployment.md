---
title: 'MLOps: ML-Modellverwaltung'
titleSuffix: Azure Machine Learning
description: 'Erfahren Sie mehr über die Modellverwaltung mit Azure Machine Learning (MLOps). Stellen Sie Modelle bereit, und verwalten und überwachen Sie diese, um sie kontinuierlich zu verbessern. '
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: jpe316
ms.author: jordane
ms.date: 11/22/2019
ms.custom: seodec18
ms.openlocfilehash: 9cde171f1c031b4b02a71fe0dac1a689e3ec0722
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75772583"
---
# <a name="mlops-model-management-deployment-and-monitoring-with-azure-machine-learning"></a>MLOps: Verwaltung, Bereitstellung und Überwachung von Modellen mit Azure Machine Learning

In diesem Artikel erfahren Sie, wie Sie Azure Machine Learning verwenden, um den Lebenszyklus Ihrer Modelle zu verwalten. Azure Machine Learning verwendet einen Machine Learning Operations-Ansatz (MLOps). MLOps verbessert die Qualität und Konsistenz ihrer Machine Learning-Lösungen. 

Azure Machine Learning bietet die folgenden MLOps-Features:

- **Erstellen reproduzierbarer ML-Pipelines**. Mit Pipelines können Sie wiederholbare und wiederverwendbare Schritte für Ihre Datenaufbereitungs-, Trainings- und Bewertungsprozesse definieren.
- **Registrieren, Verpacken und Bereitstellen von Modellen von überall aus** mit Nachverfolgung der zugeordneten Metadaten, die für die Verwendung des Modells erforderlich sind.
- **Sammeln der erforderlichen Governancedaten zur Erfassung des ML-Lebenszyklus End-to-End**, einschließlich der Personen, die Modelle veröffentlichen, der Gründe für Änderungen und des Zeitpunkts, zu dem Modelle bereitgestellt oder in der Produktion verwendet wurden.
- **Benachrichtigen und Warnen bei Ereignissen im ML-Lebenszyklus**, wie etwa dem Abschluss von Experimenten, der Modellregistrierung, der Modellimplementierung und der Erkennung von Datendrift.
- **Überwachen von ML-Anwendungen auf betriebs- und ML-bezogene Probleme**. Vergleichen Sie Modelleingaben zwischen Trainings- und Inferenzphase, untersuchen Sie modellspezifische Metriken, und stellen Sie Überwachungsfunktionen und Warnungen für Ihre ML-Infrastruktur bereit.
- **Automatisieren des End-to-End-ML-Lebenszyklus mit Azure Machine Learning und Azure DevOps**: Ermöglicht das häufige Aktualisieren von Modellen, Testen neuer Modelle und Durchführen fortlaufender Rollouts von neuen ML-Modellen zusätzlich zu Ihren anderen Anwendungen und Diensten.

## <a name="create-reproducible-ml-pipelines"></a>Erstellen reproduzierbarer ML-Pipelines

Verwenden Sie ML-Pipelines aus Azure Machine Learning, um alle Schritte zu verbinden, die am Trainingsprozess Ihres Modells beteiligt sind.

Eine ML-Pipeline kann Schritte aus der Datenaufbereitung über die Featureextraktion und die Optimierung von Hyperparametern bis hin zur Modellauswertung enthalten. Weitere Informationen finden Sie im Artikel zu [ML-Pipelines](concept-ml-pipelines.md).

Wenn Sie den [Designer](concept-designer.md) zum Erstellen Ihrer ML-Pipelines verwenden, können Sie jederzeit auf die **"..."** oben rechts auf der Designer-Seite klicken und dann **Klonen** auswählen. Durch das Klonen der Pipeline können Sie das Pipelinedesign durchlaufen, ohne Ihre alten Versionen zu verlieren.  

## <a name="register-package-and-deploy-models-from-anywhere"></a>Registrieren, Verpacken und Bereitstellen von Modellen von überall aus

### <a name="register-and-track-ml-models"></a>Registrieren und Nachverfolgen von ML-Modellen

Die Modellregistrierung ermöglicht es Ihnen, Ihre Modelle in der Azure-Cloud in Ihrem Arbeitsbereich zu speichern und zu versionieren. Die Modellregistrierung erleichtert das Organisieren und Verwalten von trainierten Modellen.

> [!TIP]
> Ein registriertes Modell ist ein logischer Container für eine oder mehrere Dateien, aus denen Ihr Modell besteht. Wenn Sie beispielsweise ein Modell verwenden, das in mehreren Dateien gespeichert ist, können Sie diese als einzelnes Modell in Ihrem Azure Machine Learning-Arbeitsbereich registrieren. Nach der Registrierung können Sie das registrierte Modell dann herunterladen oder bereitstellen und alle Dateien empfangen, die registriert wurden.

Registrierte Modelle werden anhand des Namens und der Version identifiziert. Jedes Mal, wenn Sie ein Modell mit dem gleichen Namen wie ein bereits vorhandenes Modell registrieren, erhöht die Registrierung die Versionsnummer. Zusätzliche Metadatentags können während der Registrierung bereitgestellt werden. Diese Tags werden dann bei der Suche nach einem Modell verwendet. Azure Machine Learning unterstützt Modelle, die mit Python 3.5.2 oder höher geladen werden können.

> [!TIP]
> Sie können auch Modelle registrieren, die außerhalb von Azure Machine Learning trainiert wurden.

Sie können kein registriertes Modell löschen, das in einer aktiven Bereitstellung verwendet wird.
Weitere Informationen finden Sie im Abschnitt „Registrieren eines Modells“ von [Bereitstellen von Modellen](how-to-deploy-and-where.md#registermodel).

### <a name="package-and-debug-models"></a>Packen und Debuggen von Modellen

Vor dem Bereitstellen eines Modells in der Produktion wird es in ein Docker-Image gepackt. In den meisten Fällen erfolgt die Imageerstellung während der Bereitstellung automatisch im Hintergrund. Sie können das Image auch manuell angeben.

Wenn Probleme bei der Bereitstellung auftreten, können Sie ein Modell für Problembehandlung und Debugging in Ihrer lokalen Entwicklungsumgebung bereitstellen.

Weitere Informationen finden Sie unter [Bereitstellen von Modellen](how-to-deploy-and-where.md#registermodel) und [Problembehandlung von Bereitstellungen](how-to-troubleshoot-deployment.md).

### <a name="validate-and-profile-models"></a>Validierung und Profilerstellung von Modellen

Azure Machine Learning kann die Profilerstellung verwenden, um die idealen CPU- und Arbeitsspeichereinstellungen für die Bereitstellung Ihres Modells zu ermitteln. Die Modellvalidierung erfolgt im Rahmen dieses Prozesses unter Verwendung von Daten, die Sie für den Profilerstellungsvorgang bereitstellen.

### <a name="convert-and-optimize-models"></a>Konvertieren und Optimieren von Modellen

Die Konvertierung Ihres Modells zu [Open Neural Network Exchange](https://onnx.ai) (ONNX) kann die Leistung verbessern. Im Durchschnitt lässt sich durch eine Konvertierung zu ONNX eine 2-fache Leistungssteigerung erzielen.

Weitere Informationen zu ONNX mit Azure Machine Learning finden Sie im Artikel [Erstellen und Beschleunigen von ML-Modellen](concept-onnx.md).

### <a name="use-models"></a>Verwenden von Modellen

Trainierte Machine Learning-Modelle werden als Webdienste in der Cloud oder lokal bereitgestellt. Sie können Modelle auch auf Azure IoT Edge-Geräten bereitstellen. Bereitstellungen verwenden CPUs, GPUs oder Field Programmable Gate Arrays (FPGAs) für das Ziehen von Rückschlüssen. Sie können auch Modelle aus Power BI verwenden.

Wenn Sie ein Modell als Webdienst oder auf einem IoT Edge-Gerät verwenden, stellen Sie die folgenden Elemente bereit:

* Die Modelle, die zum Bewerten der Daten dienen, die an den Dienst bzw. das Gerät übermittelt werden.
* Ein Eingabeskript. Dieses Skript akzeptiert Anforderungen, verwendet die Modelle zum Bewerten der Daten und gibt eine Antwort zurück.
* Eine Conda-Umgebungsdatei, die die Abhängigkeiten beschreibt, die von den Modellen und vom Eingabeskript benötigt werden.
* Zusätzliche Ressourcen wie Text, Daten usw., die von den Modellen und vom Eingabeskript benötigt werden.

Sie stellen auch die Konfiguration der Zielbereitstellungsplattform bereit. Beispielsweise den Typ der VM-Familie, den verfügbaren Arbeitsspeicher und die Anzahl von Kernen bei der Bereitstellung in Azure Kubernetes Service.

Wenn das Image erstellt wird, werden auch die Komponenten hinzugefügt, die von Azure Machine Learning benötigt werden. Beispielsweise die Ressourcen, die erforderlich sind, um den Webdienst auszuführen und mit IoT Edge zu interagiere.

#### <a name="batch-scoring"></a>Batchbewertung
Die Batchbewertung durch ML-Pipelines wird unterstützt. Weitere Informationen finden Sie im Artikel [Batchvorhersagen für Big Data](how-to-run-batch-predictions.md).

#### <a name="real-time-web-services"></a>Echtzeit-Webdienste

Sie können Ihre Modelle in **Webdiensten** mit den folgenden Computezielen verwenden:

* Azure Container Instances
* Azure Kubernetes Service
* Lokale Entwicklungsumgebung

Um das Modell als Webdienst bereitzustellen, müssen Sie folgende Elemente zur Verfügung stellen:

* Das Modell oder ein Ensemble von Modellen.
* Abhängigkeiten, die zur Nutzung des Modells erforderlich sind. Beispielsweise ein Skript, das Anforderungen akzeptiert und das Modell aufruft, Conda-Abhängigkeiten, usw.
* Eine Bereitstellungskonfiguration, die beschreibt, wie und wo das Modell bereitgestellt werden soll.

Weitere Informationen finden Sie unter [Bereitstellen von Modellen](how-to-deploy-and-where.md).

#### <a name="iot-edge-devices"></a>IoT Edge-Geräte

Sie können Modelle mit IoT-Geräten über **Azure IoT Edge-Module** verwenden. IoT Edge-Module werden auf einem Hardwaregerät bereitgestellt, wodurch Rückschlüsse bzw. Modellbewertungen auf dem Gerät ermöglicht werden.

Weitere Informationen finden Sie unter [Bereitstellen von Modellen](how-to-deploy-and-where.md).

### <a name="analytics"></a>Analytics

Microsoft Power BI unterstützt die Verwendung von Machine Learning-Modellen für Datenanalysen. Weitere Informationen finden Sie unter [Integration von Azure Machine Learning in Power BI (Vorschau)](https://docs.microsoft.com/power-bi/service-machine-learning-integration).

## <a name="capture-the-governance-data-required-for-capturing-the-end-to-end-ml-lifecycle"></a>Sammeln der erforderlichen Governancedaten zur Erfassung des ML-Lebenszyklus End-to-End

Bei Azure ML können Sie den End-to-End-Überwachungspfad Ihrer gesamten ML-Ressourcen nachverfolgen. Dies gilt insbesondere in folgenden Fällen:

- Azure ML ist [mit Git integriert](how-to-set-up-training-targets.md#gitintegration), um nachverfolgen zu können, von welchem Repository, welcher Branch oder welchem Commit Ihr Code stammt.
- Mit [Azure ML-Datasets](how-to-create-register-datasets.md) können Sie Daten nachverfolgen, Profile erstellen und Versionen verwalten. 
- Der Azure ML-Ausführungsverlauf speichert eine Momentaufnahme des Codes, der Daten und des Computevorgangs, mit denen ein Modell trainiert wurde.
- Die Azure ML-Modellregistrierung erfasst alle Metadaten, die Ihrem Modell zugeordnet sind (welches Experiment wurde für das Training verwendet, wo wird es bereitgestellt und ob die Bereitstellungen fehlerfrei sind).

## <a name="notify-automate-and-alert-on-events-in-the-ml-lifecycle"></a>Benachrichtigen, Automatisieren und Warnen bei Ereignissen im ML-Lebenszyklus
Azure ML veröffentlicht wichtige Ereignisse in Azure EventGrid, das für Benachrichtigung und Automatisierung bei Ereignissen im ML-Lebenszyklus verwendet werden kann. Weitere Informationen finden Sie in [diesem Dokument](how-to-use-event-grid.md).


## <a name="monitor-for-operational--ml-issues"></a>Überwachen auf Betriebs- und ML-Probleme

Die Überwachung ermöglicht Ihnen zu verstehen, welche Daten an Ihr Modell gesendet werden und welche Vorhersagen es zurückgibt.

Diese Informationen helfen Ihnen zu verstehen, wie Ihr Modell genutzt wird. Die gesammelten Eingabedaten können auch zum Trainieren künftiger Versionen des Modells nützlich sein.

Weitere Informationen finden Sie unter [Gewusst wie: Aktivieren der Modelldatensammlung](how-to-enable-data-collection.md).

## <a name="automate-the-ml-lifecycle"></a>Automatisieren des ML-Lebenszyklus 

Sie können GitHub und Azure Pipelines verwenden, um einen Continuous Integration-Prozess zu erstellen, der ein Modell trainiert. In einem typischen Szenario, in dem ein Data Scientist eine Änderung in das Git-Repository für ein Projekt eincheckt, startet die Azure-Pipeline einen Trainingsdurchlauf. Die Ergebnisse dieses Durchlaufs können dann untersucht werden, um die Leistungsmerkmale des trainierten Modells zu ermitteln. Sie können auch eine Pipeline erstelle, die das Modell als Webdienst bereitstellt.

Die [Azure Machine Learning-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-air-aiagility.vss-services-azureml) erleichtert die Arbeit mit Azure Pipelines. Sie bietet folgende Verbesserungen für Azure Pipelines:

* Ermöglicht beim Definieren einer Dienstverbindung die Auswahl eines Arbeitsbereichs.
* Ermöglicht das Auslösen von Releasepipelines durch trainierte Modelle, die in einer Trainingspipeline erstellt wurden.

Weitere Informationen zum Verwenden von Azure Pipelines mit Azure Machine Learning finden Sie im Artikel [Trainieren und Bereitstellen von Machine Learning-Modellen](/azure/devops/pipelines/targets/azure-machine-learning) und im Repository [Azure Machine Learning MLOps](https://aka.ms/mlops).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Ressourcen:

+ [Bereitstellen von Modellen](how-to-deploy-and-where.md) mit Azure Machine Learning

+ [Tutorial: Bereitstellen eines Bildklassifizierungsmodells in Azure Container Instances](tutorial-deploy-models-with-aml.md)

+ [Repository mit End-to-End-Beispielen für MLOps](https://github.com/microsoft/MLOps)

+ [CI/CD von ML-Modellen mit Azure Pipelines](/azure/devops/pipelines/targets/azure-machine-learning)

+ Erstellen von Clients, die ein [bereitgestelltes Modell nutzen](how-to-consume-web-service.md)

+ [Bedarfsorientiertes Machine Learning](/azure/architecture/data-guide/big-data/machine-learning-at-scale)

+ [Repository mit bewährten Methoden und Referenzarchitekturen für Azure KI](https://github.com/microsoft/AI)
