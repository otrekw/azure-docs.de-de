---
title: Featurisierung in AutoML-Experimenten
titleSuffix: Azure Machine Learning
description: Erfahren Sie, welche Featurisierungseinstellungen Azure Machine Learning bietet und wie Feature Engineering in Experimenten mit automatisiertem ML unterstützt wird.
author: nibaccam
ms.author: nibaccam
ms.reviewer: nibaccam
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 05/28/2020
ms.custom: seodec18
ms.openlocfilehash: 93e18a95e30c21a44f9ca7df92925323930a9ce8
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "84122306"
---
# <a name="featurization-with-automated-machine-learning"></a>Featurisierung mit automatisiertem maschinellem Lernen

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Leitfaden erfahren Sie, welche Featurisierungseinstellungen angeboten werden, und wie Sie sie für Ihre [Experimente mit automatisiertem maschinellem Lernen](concept-automated-ml.md) anpassen.

Beim Feature Engineering werden Domänenkenntnisse der Daten zum Erstellen von Features verwendet, mit denen ML-Algorithmen besser lernen können. In Azure Machine Learning werden für das Feature Engineering Datenskalierungs- und Normalisierungstechniken angewandt. Zusammen werden diese Techniken und das Feature Engineering als Featurisierung in Experimenten mit automatisiertem maschinellem Lernen (AutoML) bezeichnet.

In diesem Artikel wird davon ausgegangen, dass Sie bereits mit der Konfiguration eines AutoML-Experiments vertraut sind. Weitere Informationen finden Sie in den folgenden Artikeln:

* Für ein Code First-Konzept: [Konfigurieren von Experimenten mit automatisiertem ML mit dem Python SDK](how-to-configure-auto-train.md)
* Für ein Konzept mit wenig oder gar keinem Code: [Erstellen, Überprüfen und Bereitstellen von Modellen des automatisierten maschinellen Lernens mit Azure Machine Learning Studio](how-to-use-automated-ml-for-ml-models.md)

## <a name="configure-featurization"></a>Konfigurieren der Featurisierung

Standardmäßig werden in jedem Experiment mit automatisiertem maschinellem Lernen [automatische Skalierungs- und Normalisierungstechniken](#featurization) auf Ihre Daten angewandt. Bei diesen Skalierungs- und Normalisierungstechniken handelt es sich um Formen der Featurisierung, die für *bestimmte* Algorithmen hilfreich sind, die auf Features unterschiedlichen Größenordnungen reagieren. Sie können jedoch zusätzliche Featurisierungen wie die **Zuschreibung fehlender Werte, Codierung** und **Transformationen** aktivieren.

> [!NOTE]
> Die Schritte zur Featurebereitstellung bei automatisiertem maschinellen Lernen (Featurenormalisierung, Behandlung fehlender Daten, Umwandlung von Text in numerische Daten usw.) werden Teil des zugrunde liegenden Modells. Bei Verwendung des Modells für Vorhersagen werden die während des Trainings angewendeten Schritte zur Featurebereitstellung automatisch auf Ihre Eingabedaten angewendet.

Bei Experimenten, die mit dem SDK konfiguriert werden, können Sie die Einstellung `featurization` aktivieren oder deaktivieren und die Featurisierungsschritte für Ihr Experiment angeben. Wenn Sie Azure Machine Learning Studio verwenden, finden Sie Informationen zum Aktivieren der Featurisierung [in diesen Schritten](how-to-use-automated-ml-for-ml-models.md#customize-featurization).

In der folgenden Tabelle sind die akzeptierten Einstellungen für `featurization` in der [AutoMLConfig-Klasse](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig) aufgeführt. 

|Konfiguration der Featurebereitstellung | BESCHREIBUNG|
------------- | ------------- |
|**`"featurization": 'auto'`**| Gibt an, dass im Rahmen der Vorverarbeitung die folgenden [Schritte für Datenschutzmaßnahmen und Featurebereitstellung](#featurization) automatisch durchgeführt werden. **Standardeinstellung**.|
|**`"featurization": 'off'`**| Gibt an, dass die Featurisierung nicht automatisch erfolgen soll.|
|**`"featurization":`&nbsp;`'FeaturizationConfig'`**| Gibt an, dass ein angepasster Featurebereitstellungsschritt verwendet werden soll. [Erfahren Sie, wie Sie die Featurebereitstellung anpassen](#customize-featurization).|

<a name="featurization"></a>

## <a name="automatic-featurization"></a>Automatische Featurebereitstellung

In der folgenden Tabelle sind die Verfahren zusammengefasst, die automatisch auf die Daten angewandt werden. Dies geschieht bei Experimenten, die über das SDK oder Studio konfiguriert werden. Um dieses Verhalten zu deaktivieren, legen Sie `"featurization": 'off'` in Ihrem `AutoMLConfig`-Objekt fest.

> [!NOTE]
> Wenn Sie Ihre mit ML automatisch erstellten Modelle in ein [ONNX-Modell](concept-onnx.md) exportieren möchten, beachten Sie, dass nur die mit * gekennzeichneten Optionen zur Featurebereitstellung im ONNX-Format unterstützt werden. Erfahren Sie mehr über das [Konvertieren von Modellen zu ONNX](concept-automated-ml.md#use-with-onnx). 

|Schritte zur&nbsp;Featurisierung| BESCHREIBUNG |
| ------------- | ------------- |
|**Löschen von Features mit hoher Kardinalität oder ohne Varianz*** |Löscht diese aus Trainings- und Validierungssets. Dazu gehören Features, bei denen alle Werte fehlen, die für alle Zeilen denselben Wert aufweisen oder die eine hohe Kardinalität aufweisen (z. B. Hashwerte, IDs oder GUIDs).|
|**Imputieren von fehlenden Werten*** |Bei numerischen Features werden fehlende Werte mit dem Durchschnitt der Werte in der Spalte imputiert.<br/><br/>Bei kategorischen Features werden fehlende Werte mit dem am häufigsten vorkommenden Wert imputiert.|
|**Generieren zusätzlicher Features*** |Für DateTime-Funktionen: Jahr, Monat, Tag, Tag der Woche, Tag des Jahres, Quartal, Woche des Jahres, Stunde, Minute, Sekunde.<br/><br/>Für Text-Funktionen: Ausdruckshäufigkeit basierend auf Unigrammen, Bigrammen und Trigrammen.|
|**Transformieren und Codieren***|Numerische Features mit wenigen eindeutigen Werten werden in kategorische Features transformiert.<br/><br/>One-Hot-Codierung wird für niedrige Kardinalität kategorisch durchgeführt, für hohe Kardinalität gilt One-Hot-Hashcodierung.|
|**Worteinbettungen**|Ein Textfeaturizer, der Vektoren von Texttoken mithilfe eines vortrainierten Modells in Satzvektoren konvertiert. Der Einbettungsvektor jedes Worts in einem Dokument wird in einem Dokumentenfeaturevektor zusammengefasst.|
|**Zielcodierungen**|Bei kategorischen Features wird jede Kategorie mit gemitteltem Zielwert für Regressionsprobleme und der Klassenwahrscheinlichkeit für jede Klasse für Klassifizierungsprobleme zugeordnet. Häufigkeitsbasierte Gewichtung und k-fache Kreuzvalidierung werden angewendet, um die Überanpassung der Zuordnung und das Rauschen durch dünn besetzte Datenkategorien zu verringern.|
|**Textzielcodierung**|Für die Texteingabe wird ein gestapeltes lineares Modell mit Bag-of-Words verwendet, um die Wahrscheinlichkeit der einzelnen Klassen zu generieren.|
|**Gewichtung der Beweise (Weight of Evidence, WoE)**|Berechnet den WoE-Wert als ein Maß für die Korrelation von kategorischen Spalten zur Zielspalte. Dieser Wert wird als Logarithmus des Verhältnisses von In-Class- zu Out-of-Class-Wahrscheinlichkeiten berechnet. Dieser Schritt gibt eine numerische Featurespalte pro Klasse aus und erspart die Notwendigkeit, fehlende Werte und Ausreißerbehandlung explizit zu berechnen.|
|**Clusterabstand**|Trainiert ein K-Means-Clusteringmodell für alle numerischen Spalten.  Gibt k neue Features aus, ein neues numerisches Feature pro Cluster, das den Abstand jeder Stichprobe zum Schwerpunkt jedes Clusters enthält.|

## <a name="data-guardrails"></a>Schutzmaßnahmen für Daten

Der Datenintegritätsschutz hilft Ihnen dabei, potenzielle Probleme mit Ihren Daten zu erkennen (z. B. fehlende Werte, [Klassenungleichgewicht](concept-manage-ml-pitfalls.md#identify-models-with-imbalanced-data)) und Korrekturmaßnahmen zur Verbesserung der Ergebnisse zu ergreifen. 

Der Datenintegritätsschutz wird angewandt: 

* **Bei SDK-Experimenten**, wenn in Ihrem `AutoMLConfig`-Objekt einer der Parameter `"featurization": 'auto'` oder `validation=auto` angegeben wird.
* **Bei Studio-Experimenten**, wenn die *automatische Featurisierung* aktiviert ist.  

Sie können den Datenintegritätsschutz für Ihr Experiment überprüfen:

* Durch Festlegen von `show_output=True` beim Übermitteln eines Experiments mit dem Python SDK.

* In Studio auf der Registerkarte **Datenintegritätsschutz** Ihrer Ausführung des automatisierten maschinellen Lernens.

### <a name="data-guardrail-states"></a>Zustände des Datenintegritätsschutzes

Die Schutzmaßnahmen für Daten werden einen von drei Zuständen anzeigen: **Erfolgreich**, **Fertig** oder **Benachrichtigt**.

|State| BESCHREIBUNG |
|----|---- |
|**Erfolgreich**| Es wurden keine Datenprobleme erkannt, und es ist keine Benutzeraktion erforderlich. |
|**Fertig**| Die Änderungen wurden auf Ihre Daten angewendet. Den Benutzern wird empfohlen, die vom automatisierten maschinellen Lernen ergriffenen Korrekturmaßnahmen zu überprüfen, um sicherzustellen, dass die Änderungen mit den erwarteten Ergebnissen übereinstimmen. |
|**Benachrichtigt**| Ein Datenproblem, das nicht behoben werden konnte, wurde erkannt. Den Benutzern wird empfohlen, das Problem zu überarbeiten und zu beheben.| 

In der folgenden Tabelle werden die derzeit unterstützten Schutzmaßnahmen für Daten und die zugehörigen Statuswerte beschrieben, die Benutzern beim Übermitteln Ihres Experiments unter Umständen angezeigt werden.

Schutzmaßnahme|Status|Bedingung&nbsp;für&nbsp;Auslösung
---|---|---
**Ergänzen fehlender Featurewerte durch Imputation** |*Erfolgreich* <br><br><br> *Fertig*| Es wurden keine fehlenden Featurewerte in Ihren Trainingsdaten festgestellt. Erfahren Sie mehr über die [Imputation fehlender Werte](https://docs.microsoft.com/azure/machine-learning/how-to-use-automated-ml-for-ml-models#advanced-featurization-options). <br><br> Fehlende Featurewerte wurden in Ihren Trainingsdaten festgestellt und zugerechnet.
**Behandeln von Features mit hoher Kardinalität** |*Erfolgreich* <br><br><br> *Fertig*| Ihre Eingaben wurden analysiert, und es wurden keine Features mit hoher Kardinalität gefunden. Erfahren Sie mehr über das [Erkennen von Features mit hoher Kardinalität.](https://docs.microsoft.com/azure/machine-learning/how-to-use-automated-ml-for-ml-models#advanced-featurization-options) <br><br> Features mit hoher Kardinalität wurden in Ihren Eingaben erkannt und behandelt.
**Verarbeitung der Überprüfungsaufteilung** |*Fertig*| Die Überprüfungskonfiguration wurde auf „automatisch“ festgelegt, und die Trainingsdaten enthielten **weniger als 20.000 Zeilen**. <br> Jede Iteration des trainierten Modells wurde durch Kreuzvalidierung überprüft. Erfahren Sie mehr über [Überprüfungsdaten](https://docs.microsoft.com/azure/machine-learning/how-to-configure-auto-train#train-and-validation-data). <br><br> Die Überprüfungskonfiguration wurde auf „automatisch“ festgelegt, und die Trainingsdaten enthielten **mehr als 20.000 Zeilen**. <br> Die Eingabedaten wurden zur Überprüfung des Modells in ein Trainingsdataset und ein Validierungsdataset aufgeteilt.
**Ausgewogenheitserkennung für Klassen** |*Erfolgreich* <br><br><br><br><br> *Benachrichtigt* | Ihre Eingaben wurden analysiert, und alle Klassen in Ihren Trainingsdaten sind ausgeglichen. Ein Dataset gilt als ausgewogen, wenn jede Klasse im Datensatz gemessen an Anzahl und Verhältnis der Stichproben gut repräsentiert ist. <br><br><br> In Ihren Eingaben wurden unausgeglichene Klassen erkannt. Beheben Sie das Ausgleichsproblem, um den Modelltrend zu beheben. Erfahren Sie mehr über [unausgeglichene Daten](https://docs.microsoft.com/azure/machine-learning/concept-manage-ml-pitfalls#identify-models-with-imbalanced-data).
**Erkennung von Arbeitsspeicherproblemen** |*Erfolgreich* <br><br><br><br> *Fertig* |<br> Die ausgewählten Horizont-, Verzögerungs- und Zeitfensterwerte wurden analysiert, und es wurden keine potenziellen Probleme aufgrund von unzureichendem Speicherplatz erkannt. Erfahren Sie mehr über [Vorhersagekonfigurationen](https://docs.microsoft.com/azure/machine-learning/how-to-auto-train-forecast#configure-and-run-experiment) von Zeitreihen. <br><br><br>Die ausgewählten Horizont-, Verzögerungs- und Zeitfensterwerte wurden analysiert, und für Ihr Experiment wird möglicherweise nicht genügend Speicherplatz zur Verfügung stehen. Die Konfigurationen für Verzögerung oder rollierende Zeitfenster wurden deaktiviert.
**Häufigkeitserkennung** |*Erfolgreich* <br><br><br><br> *Fertig* |<br> Die Zeitreihe wurde analysiert, und alle Datenpunkte entsprechen der erkannten Häufigkeit. <br> <br> Die Zeitreihe wurde analysiert, und es wurden Datenpunkte ermittelt, die nicht mit der erkannten Häufigkeit übereinstimmen. Diese Datenpunkte wurden aus dem Dataset entfernt. Erfahren Sie mehr über die [Datenaufbereitung für die Zeitreihenvorhersage](https://docs.microsoft.com/azure/machine-learning/how-to-auto-train-forecast#preparing-data).

## <a name="customize-featurization"></a>Anpassen der Featurisierung

Sie können die Featurisierungseinstellungen anpassen, um sicherzustellen, dass die Daten und Features zum Trainieren Ihres ML-Modells zu relevanten Vorhersagen führen. 

Um die Featurisierung anzupassen, geben Sie `"featurization": FeaturizationConfig` in Ihrem `AutoMLConfig`-Objekt an. Wenn Sie Azure Machine Learning Studio für Ihr Experiment verwenden, finden Sie weitere Informationen in der [Schrittanleitung](how-to-use-automated-ml-for-ml-models.md#customize-featurization).

Die unterstützte Anpassung umfasst Folgendes:

|Anpassung|Definition|
|--|--|
|**Aktualisierung des Spaltenzwecks**|Überschreiben des Featuretyps für die angegebene Spalte.|
|**Aktualisierung von Transformationsparametern** |Aktualisieren der Parameter für den angegebenen Transformator. Unterstützt derzeit „Imputer“ (Mittelwert, häufigster Wert und medianer Wert) und „HashOneHotEncoder“.|
|**Löschen von Spalten** |Spalten, die aus der Featureverwendung gelöscht werden sollen.|
|**Blockieren von Transformatoren**| Blockieren von Transformatoren, die für die Featurebereitstellung verwendet werden.|

Erstellen Sie das FeaturizationConfig-Objekt mit API-Aufrufen:
```python
featurization_config = FeaturizationConfig()
featurization_config.blocked_transformers = ['LabelEncoder']
featurization_config.drop_columns = ['aspiration', 'stroke']
featurization_config.add_column_purpose('engine-size', 'Numeric')
featurization_config.add_column_purpose('body-style', 'CategoricalHash')
#default strategy mean, add transformer param for for 3 columns
featurization_config.add_transformer_params('Imputer', ['engine-size'], {"strategy": "median"})
featurization_config.add_transformer_params('Imputer', ['city-mpg'], {"strategy": "median"})
featurization_config.add_transformer_params('Imputer', ['bore'], {"strategy": "most_frequent"})
featurization_config.add_transformer_params('HashOneHotEncoder', [], {"number_of_bits": 3})
```
## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie Ihre Experimente mit automatisiertem maschinellem Lernen einrichten.

    * Für Kunden mit Programmiererfahrung: [Konfigurieren von Experimenten mit automatisiertem ML mit dem Azure Machine Learning SDK](how-to-configure-auto-train.md)
    * Für Kunden mit wenig oder keiner Programmiererfahrung: [Erstellen von Experimenten mit automatisiertem maschinellem Lernen in Azure Machine Learning Studio](how-to-use-automated-ml-for-ml-models.md)

* Lernen Sie, [wie und wo Sie Modelle bereitstellen](how-to-deploy-and-where.md) können.

* Erfahren Sie mehr über das [Trainieren eines Regressionsmodells mit automatisiertem maschinellem Lernen](tutorial-auto-train-models.md) und das [Trainieren mit automatisiertem maschinellem Lernen auf einer Remoteressource](how-to-auto-train-remote.md).
