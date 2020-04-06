---
title: Was ist automatisiertes maschinelles Lernen? / AutoML
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Azure Machine Learning automatisch einen Algorithmus für Sie auswählen und ein Modell daraus generieren kann, um Ihnen Zeit zu sparen, indem er die von Ihnen angegebenen Parameter und Kriterien verwendet, um den besten Algorithmus für Ihr Modell auszuwählen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: cartacioS
ms.author: sacartac
ms.date: 02/28/2020
ms.openlocfilehash: c8864e00be9f491d87478c253286070b9334a26e
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2020
ms.locfileid: "80383190"
---
# <a name="what-is-automated-machine-learning"></a>Was ist automatisiertes maschinelles Lernen?

Automatisiertes maschinelles Lernen, auch als automatisiertes ML bezeichnet, ist der Prozess des Automatisierens der zeitaufwändigen, iterativen Aufgaben der Entwicklung eines Machine Learning-Modells. Es versetzt Data Scientists, Analysten und Entwickler in die Lage, ML-Modelle mit hoher Skalierbarkeit, Effizienz und Produktivität zu erstellen und gleichzeitig die Modellqualität zu erhalten. Automatisiertes Machine Learning basiert auf einem Durchbruch der [Microsoft Research-Abteilung](https://arxiv.org/abs/1705.05355).

Die Entwicklung traditioneller Machine Learning-Modelle ist ressourcenintensiv und erfordert viel Fachwissen und Zeit, um Dutzende von Modellen zu erstellen und zu vergleichen. Mit automatisiertem maschinellem Lernen verkürzen Sie die Zeit, die benötigt wird, um produktionsbereite ML-Modelle mit großer Leichtigkeit und Effizienz zu erhalten.

 


## <a name="when-to-use-automated-ml"></a>Nutzung von automatisiertem Machine Learning

Sie arbeiten mit automatisiertem ML, wenn Sie möchten, dass Azure Machine Learning mit der von Ihnen angegebenen Zielmetrik ein Modell für Sie trainiert und optimiert. Automatisiertes ML demokratisiert den Entwicklungsprozess eines Machine Learning-Modells und befähigt seine Benutzer, unabhängig von deren Data Science-Kenntnissen, eine durchgängige Machine Learning-Pipeline für jedes Problem zu bestimmen.

Data Scientists, Analysten und Entwickler aus den verschiedensten Branchen können automatisiertes ML für Folgendes verwenden:

+ Implementieren von Machine Learning-Lösungen ohne umfangreiche Programmierkenntnisse
+ Sparen von Zeit und Ressourcen
+ Nutzen von bewährten Methoden aus der Data Science
+ Bereitstellen flexibler Problemlösungen

In der folgenden Tabelle sind gängige Anwendungsfälle für automatisiertes maschinelles Lernen aufgeführt. 

Klassifizierung| Zeitreihenvorhersage | Regression
---|---|---
[Betrugserkennung](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-credit-card-fraud/auto-ml-classification-credit-card-fraud.ipynb)|[Verkaufsvorhersage](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-orange-juice-sales/auto-ml-forecasting-orange-juice-sales.ipynb)|[CPU-Leistungsvorhersage](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/regression-hardware-performance-explanation-and-featurization/auto-ml-regression-hardware-performance-explanation-and-featurization.ipynb)
|[Marketingvorhersage](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb)|[Bedarfsvorhersage](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)|
|[Newsgroup-Datenklassifizierung](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-text-dnn/auto-ml-classification-text-dnn.ipynb)|[Vorhersage für die Getränkeproduktion](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-beer-remote/auto-ml-forecasting-beer-remote.ipynb)|

## <a name="design-automated-ml-experiments"></a>Entwerfen automatisierter ML-Experimente

Mithilfe von **Azure Machine Learning** können Sie automatisierte ML-Trainingsexperimente mit den folgenden Schritten entwerfen und ausführen:

1. **Identifizieren des ML-Problems**, das gelöst werden soll: Klassifizierung, Vorhersage oder Regression.

1. **Angeben der Quelle und des Formats der bezeichneten Trainingsdaten**: NumPy-Arrays oder Pandas-Datenrahmen.

1. **Konfigurieren des Computeziels für das Modelltraining**, z.B. [lokaler Computer, Azure Machine Learning Computes, Remote-VMs oder Azure Databricks](how-to-set-up-training-targets.md).  Weitere Informationen zu automatisiertem Training [für eine Remoteressource](how-to-auto-train-remote.md).

1. **Konfigurieren der automatisierten Machine Learning-Parameter**, die die Anzahl der Iterationen über verschiedene Modelle, die Hyperparametereinstellungen, erweiterte Vorverarbeitung/Featurebereitstellung und die Metriken bestimmen, die bei der Ermittlung des besten Modells zu berücksichtigen sind.  Sie können die Einstellungen für ein automatisches Trainingsexperiment [in Azure Machine Learning Studio](https://ml.azure.com) oder [mit dem SDK](how-to-configure-auto-train.md) konfigurieren. 

    [!INCLUDE [aml-applies-to-enterprise-sku](../../includes/aml-applies-to-enterprise-sku-inline.md)]

1. **Übermitteln der Trainingsausführung.**

## <a name="how-automated-ml-works"></a>Funktionsweise von automatisiertem ML

Während des Trainings erstellt Azure Machine Learning eine Reihe von parallelen Pipelines, die unterschiedliche Algorithmen und Parametern ausprobieren. Der Dienst durchläuft die ML-Algorithmen iterativ im Zusammenspiel mit der jeweiligen Featureauswahl, wobei für jede Iteration ein Modell mit einer Trainingsbewertung erzeugt wird. Je höher die Bewertung ist, desto besser wird das Modell als „passend“ für Ihre Daten angesehen.  Die Ausführung wird beendet, sobald die im Experiment definierten Beendigungskriterien erreicht werden. Dieser Prozess wird anhand des folgenden Diagramms veranschaulicht. 

  ![Automatisiertes maschinelles Lernen](./media/concept-automated-ml/automl-concept-diagram2.png)


Sie können die protokollierten Ausführungsinformationen auch untersuchen. Sie enthalten die während der Ausführung [erfassten Metriken](how-to-understand-automated-ml.md). Bei der Trainingsausführung wird ein serialisiertes Python-Objekt (`.pkl`-Datei) generiert, das die Vorabverarbeitung des Modells und der Daten enthält.

Obwohl die Modellerstellung automatisiert ist, können Sie auch [ermitteln, wie wichtig oder relevant Features für die generierten Modelle sind](how-to-configure-auto-train.md#explain).

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2Xc9t]

<a name="preprocess"></a>

## <a name="preprocessing"></a>Preprocessing (Vorverarbeitung)

In jedem automatisierten Machine Learning-Experiment werden Ihre Daten mit den Standardmethoden und optional durch eine erweiterte Vorverarbeitung vorverarbeitet.

> [!NOTE]
> Die Schritte zur Vorverarbeitung bei automatisiertem maschinellen Lernen (Featurenormalisierung, Behandlung fehlender Daten, Umwandlung von Text in numerische Daten usw.) werden Teil des zugrunde liegenden Modells. Bei Verwendung des Modells für Vorhersagen werden die während des Trainings angewendeten Vorverarbeitungsschritte automatisch auf Ihre Eingabedaten angewendet.

### <a name="automatic-preprocessing-standard"></a>Automatische Vorverarbeitung (Standard)

In jedem automatisierten Machine Learning-Experiment werden Ihre Daten automatisch skaliert oder normalisiert, damit die Algorithmen gut funktionieren.  Während des Modelltrainings wird eine der folgenden Skalierungs- oder Normalisierungstechniken auf jedes Modell angewendet.

|Skalierung&nbsp;&&nbsp;Normalisierung| BESCHREIBUNG |
| ------------- | ------------- |
| [StandardScaleWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)  | Standardisieren von Features durch Entfernen des Mittelwerts und Skalierung auf Einheitenvarianz.  |
| [MinMaxScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html)  | Transformieren von Features durch Skalieren jedes Features anhand des Mindest- und Maximalwerts dieser Spalte.  |
| [MaxAbsScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MaxAbsScaler.html#sklearn.preprocessing.MaxAbsScaler) |Skalieren jedes Features anhand seines maximalen Absolutwerts. |
| [RobustScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.RobustScaler.html) |Diese Skalarfunktionen skaliert Features nach ihren Quantilbereich. |
| [PCA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html) |Lineare Dimensionalitätsreduzierung unter Verwendung von Singulärwertzerlegung der Daten, um sie auf einen niedrigeren Dimensionsbereich zu projizieren. |
| [TruncatedSVDWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.TruncatedSVD.html) |Diese Transformation führt Dimensionalitätsreduzierung durch abgeschnittene Singulärwertzerlegung (SVD) durch. Im Gegensatz zu PCA zentriert diese Schätzfunktion die Daten nicht vor der Berechnung der Singulärwertzerlegung, d. h., dass scipy.sparse-Matrizen effizient verwendet werden können. |
| [SparseNormalizer](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.Normalizer.html) | Jede Stichprobe (d.h. jede Zeile der Datenmatrix) mit mindestens einer Komponente ungleich Null wird unabhängig von anderen Stichproben neu skaliert, sodass ihre Norm (l1 oder l2) gleich eins ist. |

### <a name="advanced-preprocessing-optional-featurization"></a>Erweiterte Vorverarbeitung: optionale Featurebereitstellung

Außerdem stehen zusätzliche erweiterte Vorverarbeitung und Featurebereitstellung zur Verfügung, z. B. Schutzmaßnahmen für Daten, Codierung und Transformationen. [Weitere Informationen zur enthaltenen Featurebereitstellung](how-to-use-automated-ml-for-ml-models.md#featurization). Diese Einstellung kann aktiviert werden über:

+ Azure Machine Learning Studio: Aktivieren Sie die **Automatische Merkmalserstellung** im Abschnitt **Konfigurationsausführung** [mit diesen Schritten](how-to-use-automated-ml-for-ml-models.md#create-and-run-experiment).

+ Python SDK: Angeben von `"feauturization": 'auto' / 'off' / 'FeaturizationConfig'` für die [`AutoMLConfig`-Klasse](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig). 

## <a name="classification--regression"></a>Klassifizierung und Regression

Klassifikation und Regression sind die häufigsten Arten von Aufgaben des maschinellen Lernens. Beides sind Arten des überwachten Lernens, bei denen Modelle anhand von Trainingsdaten lernen und diese Erkenntnisse auf neue Daten anwenden. Azure Machine Learning bietet Featurebereitstellungen speziell für diese Aufgaben, z. B. Textfeaturizer für Deep Neural Network zur Klassifizierung. Erfahren Sie mehr über die [Optionen für die Featurebereitstellung](how-to-use-automated-ml-for-ml-models.md#featurization). 

Das Hauptziel von Klassifizierungsmodellen besteht darin, auf der Grundlage der Erkenntnisse aus den Trainingsdaten vorherzusagen, in welche Kategorien neue Daten fallen werden. Zu den gängigen Klassifizierungsbeispielen gehören Betrugserkennung, Handschrifterkennung und Objekterkennung.  Hier erhalten Sie weitere Informationen und ein Beispiel für die [Klassifizierung mit automatisiertem maschinellen Lernen](tutorial-train-models-with-aml.md).

Anders als bei der Klassifizierung, bei der die vorhergesagten Ausgabewerte kategorisch sind, sagen Regressionsmodelle numerische Ausgabewerte auf der Grundlage unabhängiger Vorhersagefaktoren voraus. Bei der Regression besteht das Ziel darin, die Beziehung zwischen diesen unabhängigen Vorhersagevariablen herzustellen, indem geschätzt wird, wie eine Variable die anderen beeinflusst. Beispiel: Der Fahrzeugpreis basierend auf Merkmalen wie Kraftstoffverbrauch, Sicherheitseinstufung, usw. Hier erhalten Sie weitere Informationen und ein Beispiel für die [Regression mit automatisiertem maschinellen Lernen](tutorial-auto-train-models.md).

## <a name="time-series-forecasting"></a>Zeitreihenvorhersagen

Die Erstellung von Vorhersagen ist ein integraler Bestandteil jedes Unternehmens, unabhängig davon, ob es sich um Einnahmen, Lagerbestände, Umsätze oder Kundennachfrage handelt. Sie können automatisiertes maschinelles Lernen verwenden, um verschiedene Techniken und Ansätze zu kombinieren. Außerdem erhalten Sie dabei eine beliebte und hochwertige Zeitreihenprognose.

Automatisierte Zeitreihenexperimente werden als multivariate Regressionsprobleme behandelt. Zeitreihenwerte aus der Vergangenheit werden „pivotiert“ und dienen so zusammen mit anderen Vorhersageelementen als zusätzliche Dimensionen für den Regressor. Dieser Ansatz hat im Gegensatz zu klassischen Zeitreihenmethoden den Vorteil, dass mehrere kontextbezogene Variablen und deren Beziehungen zueinander beim Training auf natürliche Weise integriert werden. Beim automatisierten maschinellen Lernen wird ein zwar einfaches, aber häufig in interne Verzweigungen unterteiltes Modell für alle Elemente im Dataset und in den Vorhersagehorizonten erlernt. Dadurch sind mehr Daten verfügbar, um Modellparameter zu schätzen, und die Generalisierung von unbekannten Reihen wird möglich.

Hier erhalten Sie weitere Informationen und ein Beispiel zum [automatisierten maschinellen Lernen für die Zeitreihenvorhersage](how-to-auto-train-forecast.md). Oder sehen Sie sich das Notebook zu Energiebedarf ([auto-ml-forecasting-energy-demand.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)) an. Dort finden Sie ausführliche Codebeispiele zu einer erweiterten Vorhersagekonfiguration, einschließlich:

* Feiertagserkennung und Erstellen zusätzlicher Merkmale (Featurization)
* Zeitreihen und DNN-Lernmodule (Auto-ARIMA, Prophet, ForecastTCN)
* Unterstützung vieler Modelle mithilfe von Gruppierungen
* Kreuzvalidierung mit rollierendem Ursprung (Rolling Origin Validation)
* Konfigurierbare Verzögerungen (Lags)
* Aggregierte Zeitfenstermerkmale (Rolling Window Features)

## <a name="ensemble-models"></a><a name="ensemble"></a> Ensemblemodelle

Automatisiertes Machine Learning unterstützt Ensemblemodelle, die standardmäßig aktiviert sind. Das Lernen mit Ensembles verbessert die Ergebnisse des maschinellen Lernens und die Vorhersageleistung, da nicht einzelne Modelle verwendet, sondern mehrere Modelle kombiniert werden. Die Ensemble-Iterationen erfolgen als abschließende Iterationen Ihrer Ausführung. Automatisiertes Machine Learning verwendet die beiden Ensemble-Methoden „voting“ (Abstimmen) und „stacking“ (Stapeln) gemeinsam, um Modelle zu kombinieren:

* **Voting**: Trifft Vorhersagen auf Grundlage des gewichteten Durchschnitts der vorhergesagten Klassenwahrscheinlichkeiten (für Klassifizierungsaufgaben) oder auf Grundlage der vorhergesagten Regressionsziele (für Regressionsaufgaben).
* **Stacking**: Stacking kombiniert heterogene Modelle und trainiert ein Metamodell, basierend auf der Ausgabe der einzelnen Modelle. Die aktuellen Standardmetamodelle sind LogisticRegression für Klassifizierungsaufgaben und ElasticNet für Regressions-/Vorhersageaufgaben.

Der [Caruana-Algorithmus für die Ensembleauswahl](http://www.niculescu-mizil.org/papers/shotgun.icml04.revised.rev2.pdf) mit sortierter Ensembleinitialisierung wird verwendet, um zu entscheiden, welche Modell innerhalb des Ensembles verwendet werden sollen. Generell initialisiert dieser Algorithmus das Ensemble mit bis zu fünf Modellen mit den besten Einzelbewertungen und überprüft, ob diese Modelle innerhalb des 5 %-Schwellenwerts der besten Bewertung liegen, um ein schlechtes Ausgangsensemble zu vermeiden. Dann wird für jede Ensemble-Iteration ein neues Modell zum vorhandenen Ensemble hinzugefügt, und die resultierende Bewertung wird berechnet. Wenn ein neues Modell die vorhandene Ensemblebewertung verbessert hat, wird das Ensemble so aktualisiert, dass es das neue Modell aufnimmt.

Informationen zum Ändern der Standard-Ensembleeinstellungen beim automatisierten Machine Learning finden Sie unter [Gewusst wie:](how-to-configure-auto-train.md#ensemble).

## <a name="use-with-onnx-in-c-apps"></a>Verwenden von ONNX in C#-Apps

Mit Azure Machine Learning können Sie automatisiertes ML verwenden, um ein Python-Modell zu erstellen und in das ONNX-Format zu konvertieren. Die ONNX-Runtime unterstützt C#, sodass Sie das erstellte Modell automatisch in Ihren C#-Apps verwenden können, ohne dass Sie es neu codieren oder die Netzwerklatenzen in Kauf nehmen müssen, die REST-Endpunkte mit sich bringen. Testen Sie ein Beispiel für diese Vorgehensweise [in diesem Jupyter Notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb).

## <a name="automated-ml-in-azure-machine-learning"></a>Automatisiertes maschinelles Lernen in Azure Machine Learning

Azure Machine Learning bietet zwei Möglichkeiten für die Arbeit mit automatisiertem maschinellen Lernen

* Für Kunden mit Codierungserfahrung, [Azure Machine Learning Python SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) 

* Für Kunden mit begrenzten oder keinen Codierungserfahrungen, Azure Machine Learning Studio unter [https://ml.azure.com](https://ml.azure.com/)  

Im Folgenden werden die allgemeinen automatisierten Funktionen für maschinelles Lernen zusammengefasst, die in der jeweiligen Option unterstützt werden.

<a name="parity"></a>

### <a name="experiment-settings"></a>Einstellungen für das Experiment 

Mit den folgenden Einstellungen können Sie Ihr Experiment für automatisiertes maschinelles Lernen konfigurieren. 

| | Python SDK| Studio
----|:----:|:----:
Aufteilen der Daten in Trainings-/Validierungssätze| ✓|✓
Unterstützt Aufgaben für maschinelles Lernen: Klassifizierung, Regression und Vorhersage| ✓| ✓
Optimiert auf Grundlage der primären Metrik| ✓| ✓
Unterstützt AML-Compute als Computeziel | ✓|✓
Konfigurieren des Vorhersagehorizonts, der Zielverzögerungen und des rollierenden Fensters|✓|✓
Festlegen der Beendigungskriterien |✓|✓ 
Festlegen gleichzeitiger Iterationen| ✓|✓
Löschen von Spalten (drop_columns)| ✓|✓
Blockieren von Algorithmen|✓|✓
Kreuzvalidierung im Vergleich |✓|✓
Unterstützt das Training für Azure Databricks-Cluster| ✓|
Anzeigen der Namen der technischen Features|✓|
Featurezusammenfassung| ✓|
Feiertagsfeaturearisierung|✓|
Ausführlichkeitsgrad für Protokolldateien| ✓|

### <a name="model-settings"></a>Modelleinstellungen

Diese Einstellungen können auf das beste Modell als Ergebnis Ihres Experiments für das automatisierte maschinelle Lernen angewendet werden.

||Python SDK|Studio
----|:----:|:----:
Beste Modellregistrierung| ✓|✓
Beste Modellimplementierung| ✓| ✓
Beste Modellerklärung| ✓|✓
Aktivieren von Abstimmungsensemble- und Stapelensemble-Modellen| ✓|✓
Anzeigen des besten Modells auf der Basis von nicht primärer Metrik|✓|
Aktivieren/Deaktivieren der Kompatibilität des ONNX-Modells|✓|
Testen des Modells | ✓| |

### <a name="run-control-settings"></a>Ausführen der Steuerelementeinstellungen

Mit diesen Einstellungen können Sie Ihre Experimentausführungen und die Ausführungen der ihnen untergeordneten Elemente überprüfen und steuern. 

||Python SDK| Studio
----|:----:|:----:
Ausführen der Zusammenfassungstabelle| ✓|✓
Abbrechen der Ausführung| ✓|✓
Abbrechen der Ausführung des untergeordneten Elements| ✓| ✓
Abrufen von Schutzmaßnahmen| ✓|✓
Anhalten der Ausführung| ✓| 
Fortsetzen der Ausführung| ✓| 

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Modelle mithilfe des automatisierten maschinellen Lernens erstellt werden, und sehen Sie sich Beispiele dazu an:

+ [Tutorial: Automatisches Trainieren eines Regressionsmodell mithilfe von Azure Machine Learning](tutorial-auto-train-models.md)

+ Konfigurieren Sie die Einstellungen für ein automatisches Trainingsexperiment:
  + Verwenden Sie in Azure Machine Learning Studio [diese Schritte](how-to-use-automated-ml-for-ml-models.md).
  + Mit dem Python SDK [verwenden Sie diese Schritte](how-to-configure-auto-train.md).

+ Erfahren Sie, wie automatisches Training mithilfe von Zeitreihendaten funktioniert, indem Sie [diese Schritte verwenden](how-to-auto-train-forecast.md).

+ Testen Sie [Jupyter Notebook-Beispiele für automatisiertes maschinelles Lernen](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/)

* Automatisiertes ML ist auch in anderen Lösungen von Microsoft verfügbar: [ML.NET](https://docs.microsoft.com/dotnet/machine-learning/automl-overview), [HDInsight](../hdinsight/spark/apache-spark-run-machine-learning-automl.md), [Power BI](https://docs.microsoft.com/power-bi/service-machine-learning-automated) und [SQL Server](https://cloudblogs.microsoft.com/sqlserver/2019/01/09/how-to-automate-machine-learning-on-sql-server-2019-big-data-clusters/)
