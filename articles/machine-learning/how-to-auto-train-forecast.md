---
title: Automatisches Trainieren eines Modells für die Zeitreihenprognose
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie mit Azure Machine Learning ein Regressionsmodell für die Zeitreihenprognose mit automatisiertem maschinellem Lernen trainieren.
services: machine-learning
author: trevorbye
ms.author: trbye
ms.service: machine-learning
ms.subservice: core
ms.reviewer: trbye
ms.topic: conceptual
ms.date: 03/09/2020
ms.openlocfilehash: bfb53893031300926944ca97a760aec199f699c0
ms.sourcegitcommit: 309cf6876d906425a0d6f72deceb9ecd231d387c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2020
ms.locfileid: "84266439"
---
# <a name="auto-train-a-time-series-forecast-model"></a>Automatisches Trainieren eines Modells für die Zeitreihenprognose
[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie ein Regressionsmodell für die Zeitreihenprognose mit automatisiertem maschinellem Lernen im [Azure Machine Learning Python SDK](https://docs.microsoft.com/python/api/overview/azure/ml/?view=azure-ml-py) konfigurieren und trainieren. 

Falls Sie wenig Erfahrung mit Code haben, lesen Sie das [Tutorial: Vorhersage des Bedarfs mithilfe von automatisiertem maschinellem Lernen](tutorial-automated-ml-forecast.md) für ein Zeitreihenvorhersagebeispiel mit automatisiertem maschinellen Lernen im [Azure Machine Learning-Studio](https://ml.azure.com/).

Das Konfigurieren eines Vorhersagemodells ähnelt zwar der Einrichtung eines Standard-Regressionsmodells mit automatisiertem maschinellem Lernen, die Verwendung von Zeitreihendaten erfordert jedoch bestimmte Konfigurationsoptionen und Vorverarbeitungsschritte. 

Sie können beispielsweise [konfigurieren](#config), wie weit die Vorhersage in die Zukunft reichen soll (Vorhersagehorizont), und ob es Verzögerungen geben soll. Beim automatisierten maschinellen Lernen wird ein zwar einfaches, aber häufig in interne Verzweigungen unterteiltes Modell für alle Elemente im Dataset und in den Vorhersagehorizonten erlernt. Dadurch sind mehr Daten verfügbar, um Modellparameter zu schätzen, und die Generalisierung von unbekannten Reihen wird möglich.

In den Beispielen dieses Artikels wird Folgendes gezeigt:

* Vorbereiten von Daten für die Zeitreihenmodellierung
* Konfigurieren spezifischer Zeitreihenparameter in einem Objekt vom Typ [`AutoMLConfig`](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig)
* Ausführen von Vorhersagen mit Zeitreihendaten

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2X1GW]

Im Gegensatz zu klassischen Methoden für Zeitreihen werden beim automatisierten maschinellen Lernen Zeitreihenwerte aus der Vergangenheit „pivotiert“ und dienen so zusammen mit anderen Vorhersageelementen als zusätzliche Dimensionen für den Regressor. Dieser Ansatz umfasst mehrere Kontextvariablen und deren Beziehung zueinander beim Training. Da sich mehrere Faktoren auf eine Vorhersage auswirken können, richtet sich diese Methode gut an realen Vorhersageszenarios aus. Wenn z. B. Verkaufszahlen vorhergesagt werden sollen, wird das Ergebnis auf der Grundlage von Interaktionen von Trends aus der Vergangenheit, des Wechselkurses und des Preises berechnet. 

Features, die aus den Trainingsdaten extrahiert werden, spielen eine wichtige Rolle. Zudem werden beim automatisierten maschinellen Lernen einige Standardschritte für die Vorverarbeitung durchgeführt, und es werden zusätzliche Zeitreihenfeatures generiert, um saisonale Auswirkungen zu erfassen und die Vorhersage so genau wie möglich zu gestalten.

## <a name="time-series-and-deep-learning-models"></a>Zeitreihen- und Deep Learning-Modelle

Deep Learning mit automatisiertem maschinellem Lernen ermöglicht das Vorhersagen von ein- und mehrdimensionalen Zeitreihendaten.

Deep Learning-Modelle weisen drei intrinsische Funktionen auf:
1. Sie können von beliebigen Zuordnungen von Eingaben zu Ausgaben lernen.
1. Sie unterstützen mehrere Eingaben und Ausgaben.
1. Sie können automatisch Muster in Eingabedaten extrahieren, die lange Folgen umfassen.

Mit größeren Daten können Deep Learning-Modelle wie ForecastTCN von Microsoft die Scores des resultierenden Modells verbessern. Erfahren Sie, wie Sie [Ihr Experiment für Deep Learning konfigurieren](#configure-a-dnn-enable-forecasting-experiment).

Automatisiertes maschinelles Lernen bietet Benutzern sowohl native Zeitreihen- als auch Deep Learning-Modelle als Teil des Empfehlungssystems. 

Modelle| BESCHREIBUNG | Vorteile
----|----|---
Prophet (Vorschauversion)|Prophet funktioniert am besten mit Zeitreihen, die starke saisonale Effekte aufweisen und viele Saisons von historischen Daten umfassen. Wenn Sie dieses Modell nutzen möchten, installieren Sie es mithilfe von `pip install fbprophet` lokal. | Schnell und genau, stabil gegenüber Ausreißern, fehlenden Daten und dramatischen Änderungen in den Zeitreihen
Auto-ARIMA (Vorschauversion)|Die ARIMA-Methode (autoregressiver integrierter gleitender Mittelwert) erzielt die optimale Leistung, wenn die Daten stationär sind. Das bedeutet, dass die statistischen Eigenschaften wie der Mittelwert und Varianz für das gesamte Dataset konstant sind. Wenn Sie beispielsweise eine Münze werfen, ist Ihre Wahrscheinlichkeit für Kopf 50 %, ganz egal, ob Sie die Münze heute, morgen oder im nächsten Jahr werfen.| Dies eignet sich für univariate Reihen, da vergangene Werte für die Vorhersage zukünftiger Werte verwendet werden.
ForecastTCN (Preview)| ForecastTCN ist ein neuronales Netzwerkmodell, das für die aufwändigsten Vorhersageaufgaben konzipiert wurde und nicht lineare lokale und globale Trends in Ihren Daten sowie Beziehungen zwischen Zeitreihen erfasst.|Es kann komplexe Trends in Ihren Daten nutzen und problemlos auf die größten Datasets skaliert werden.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure Machine Learning-Arbeitsbereich. Informationen zum Erstellen des Arbeitsbereichs finden Sie unter [Erstellen eines Azure Machine Learning-Arbeitsbereichs](how-to-manage-workspace.md).
* In diesem Artikel werden Grundkenntnisse im Zusammenhang mit der Einrichtung eines Experiments mit automatisiertem maschinellem Lernen vorausgesetzt. Machen Sie sich anhand des [Tutorials](tutorial-auto-train-models.md) oder der [Anleitung](how-to-configure-auto-train.md) mit den grundlegenden Entwurfsmustern vertraut.

## <a name="preparing-data"></a>Aufbereiten der Daten

Der wichtigste Unterschied zwischen einem Regressionsaufgabentyp für die Vorhersage und einem regulären Regressionsaufgabentyp innerhalb des automatisierten maschinellen Lernens liegt in der Einbeziehung eines Features in Ihre Daten, das eine gültige Zeitreihe darstellt. Eine reguläre Zeitreihe besitzt ein klar definiertes und konsistentes Intervall sowie einen Wert an jedem Stichprobenpunkt in einem ununterbrochenen Zeitraum. Betrachten Sie die folgende Momentaufnahme der Datei `sample.csv`:

    day_datetime,store,sales_quantity,week_of_year
    9/3/2018,A,2000,36
    9/3/2018,B,600,36
    9/4/2018,A,2300,36
    9/4/2018,B,550,36
    9/5/2018,A,2100,36
    9/5/2018,B,650,36
    9/6/2018,A,2400,36
    9/6/2018,B,700,36
    9/7/2018,A,2450,36
    9/7/2018,B,650,36

Dieses Dataset ist ein einfaches Beispiel für die täglichen Verkaufsdaten eines Unternehmens mit zwei Filialen: A und B. Darüber hinaus ist ein Feature für `week_of_year` vorhanden, das dem Modell die Erkennung der wöchentlichen Saisonalität ermöglicht. Das Feld `day_datetime` stellt eine bereinigte Zeitreihe mit täglichem Intervall dar. Das Feld `sales_quantity` ist die Zielspalte für laufende Vorhersagen. Lesen Sie die Daten in einen Pandas-Datenrahmen ein, und verwenden Sie anschließend die Funktion `to_datetime`, um sicherzustellen, dass eine Zeitreihe vom Typ `datetime` verwendet wird.

```python
import pandas as pd
data = pd.read_csv("sample.csv")
data["day_datetime"] = pd.to_datetime(data["day_datetime"])
```

In diesem Fall sind die Daten bereits aufsteigend nach dem Zeitfeld `day_datetime` sortiert. Bei der Einrichtung eines Experiments muss jedoch darauf geachtet werden, dass die gewünschte Zeitspalte in aufsteigender Reihenfolge sortiert wird, um eine gültige Zeitreihe zu erstellen. Nehmen Sie an, die Daten umfassen 1.000 Datensätze, und teilen Sie die Daten deterministisch auf, um Trainings- und Testdatasets zu erstellen. Bestimmen Sie den Spaltennamen der Bezeichnung, und legen Sie ihn auf die Bezeichnung fest. In diesem Beispiel lautet die Bezeichnung `sales_quantity`. Trennen Sie dann das Bezeichnungsfeld von `test_data`, um die `test_target`-Menge zu bilden.

```python
train_data = data.iloc[:950]
test_data = data.iloc[-50:]

label =  "sales_quantity"
 
test_labels = test_data.pop(label).values
```

> [!NOTE]
> Stellen Sie beim Trainieren eines Modells für die Vorhersage zukünftiger Werte sicher, dass alle während des Trainings verwendeten Features beim Ausführen von Vorhersagen für Ihren gewünschten Vorhersagehorizont verwendet werden können. Wenn Sie also beispielsweise eine Nachfrageprognose erstellen, lässt sich die Trainingsgenauigkeit durch die Einbeziehung eines Features für den aktuellen Aktienkurs erheblich verbessern. Wenn Sie bei Ihrer Vorhersage allerdings einen Vorhersagehorizont verwenden, der weit in der Zukunft liegt, lassen sich zukünftige Aktienkurse für zukünftige Zeitreihenpunkte ggf. nicht präzise vorhersagen, was sich nachteilig auf die Modellgenauigkeit auswirken kann.

<a name="config"></a>

## <a name="train-and-validation-data"></a>Trainings- und Überprüfungsdaten
Sie können separate Datensätze für Training und Überprüfung direkt im `AutoMLConfig`-Konstruktor angeben.

### <a name="rolling-origin-cross-validation"></a>Kreuzvalidierung mit rollierendem Ursprung
Die ROCV (Kreuzvalidierung mit rollierendem Ursprung) wird bei der Zeitreihenvorhersage verwendet, um Zeitreihen auf temporal konsistente Weise zu trennen. Die ROCV teilt die Reihe in Trainings- und Validierungsdaten mit einem Ursprungszeitpunkt auf. Wenn der Ursprung zeitlich verschoben wird, werden Teilmengen für die Kreuzvalidierung erstellt.  

![alt text](./media/how-to-auto-train-forecast/ROCV.svg)

Mit dieser Strategie wird die Datenintegrität von Zeitreihen beibehalten und das Risiko von Datenlecks vermieden. Die ROCV wird automatisch für Vorhersageaufgaben verwendet, indem die Trainings- und Validierungsdaten gemeinsam übergeben und die Anzahl der Teilmengen für die Kreuzvalidierung mithilfe von `n_cross_validations` festgelegt wird. Erfahren Sie mehr darüber, wie das automatisierte maschinelle Lernen die Kreuzvalidierung anwendet, um eine [Überanpassung von Modellen zu verhindern](concept-manage-ml-pitfalls.md#prevent-over-fitting).

```python
automl_config = AutoMLConfig(task='forecasting',
                             n_cross_validations=3,
                             ...
                             **time_series_settings)
```
Erfahren Sie mehr über [AutoMLConfig](#configure-and-run-experiment).

## <a name="configure-and-run-experiment"></a>Konfigurieren und Ausführen des Experiments

Bei Vorhersageaufgaben nutzt das automatisierte maschinelle Lernen spezifische Vorverarbeitungs- und Schätzschritte für Zeitreihendaten. Folgende Vorverarbeitungsschritte werden ausgeführt:

* Erkennen des Intervalls der Zeitreihenstichprobe (z. B. stündlich, täglich, wöchentlich) und Erstellen neuer Datensätze für fehlende Zeitpunkte, um eine ununterbrochene Reihe zu erhalten.
* Imputieren fehlender Werte in der Zielspalte (mittels Forward-Fill) und der Featurespalte (mittels Median-Spaltenwerten)
* Erstellen granularitätsbasierter Features, um reihenübergreifend feste Effekte zu ermöglichen
* Erstellen zeitbasierter Features zur Ermittlung saisonaler Muster
* Codieren kategorischer Variablen zu numerischen Mengen

Das Objekt [`AutoMLConfig`](https://docs.microsoft.com/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig?view=azure-ml-py) definiert die erforderlichen Einstellungen und Daten für eine Aufgabe mit automatisiertem maschinellen Lernen. Sie definieren Standardtrainingsparameter wie Aufgabentyp, Iterationsanzahl, Trainingsdaten und Anzahl von Kreuzvalidierungen (ähnlich wie bei einem Regressionsproblem). Bei Vorhersageaufgaben müssen allerdings noch weitere Parameter für das Experiment festgelegt werden. In der folgenden Tabelle werden die einzelnen Parameter sowie deren Verwendung erläutert:

| Parametername&nbsp; | BESCHREIBUNG | Erforderlich |
|-------|-------|-------|
|`time_column_name`|Dient zum Angeben der Datetime-Spalte in den Eingabedaten, die zum Erstellen der Zeitreihe sowie zum Ableiten des Intervalls verwendet wird.|✓|
|`grain_column_names`|Namen zum Definieren individueller Reihengruppen in den Eingabedaten. Ohne definierte Granularität wird bei dem Dataset von einer einzelnen Zeitreihe ausgegangen.||
|`max_horizon`|Definiert den maximal gewünschten Vorhersagehorizont in Einheiten von Zeitreihen. Die Einheiten basieren auf dem Zeitintervall Ihrer Trainingsdaten, z. B. monatlich oder wöchentlich, die vorhergesagt werden sollen.|✓|
|`target_lags`|Anzahl der Zeilen, um die die Zielwerte basierend auf der Häufigkeit der Daten verzögert werden sollen. Diese Verzögerung wird als Liste oder als einzelner Integer dargestellt. Die Verzögerung sollte verwendet werden, wenn die Beziehung zwischen den unabhängigen Variablen und der abhängigen Variable standardmäßig nicht übereinstimmt oder korreliert. Wenn Sie beispielsweise versuchen, die Nachfrage nach einem Produkt vorherzusagen, hängt die Nachfrage in einem Monat möglicherweise vom Preis für bestimmte Produkte vor 3 Monaten ab. In diesem Beispiel möchten Sie möglicherweise den Zielwert (Nachfrage) um 3 Monate negativ verzögern, sodass das Modell mit der richtigen Beziehung trainiert wird.||
|`target_rolling_window_size`|*n* Historische Zeiträume zum Generieren der vorhergesagten Werte, < = Größe Trainingsmenge. Wenn nicht angegeben, ist *n* die vollständige Trainingsmenge. Geben Sie diesen Parameter an, wenn Sie beim Trainieren des Modells nur eine bestimmte Menge des Verlaufs beachten möchten.||
|`enable_dnn`|Aktivieren Sie Vorhersage-DNNs.||

Weitere Informationen finden Sie in der [Referenzdokumentation](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig).

Erstellen Sie die Zeitreiheneinstellungen als Wörterbuchobjekt. Legen Sie `time_column_name` auf das Feld `day_datetime` im Dataset fest. Definieren Sie den Parameter `grain_column_names`, um sicherzustellen, dass für die Daten **zwei separate Zeitreihengruppen** erstellt werden (jeweils eine für die Filialen A und B). Legen Sie abschließend `max_horizon` auf „50“ fest, um Vorhersagen für den gesamten Testsatz zu generieren. Legen Sie mit `target_rolling_window_size` ein Vorhersagefenster auf 10 Zeiträume fest, und geben Sie mit dem Parameter `target_lags` eine einzelne Verzögerung für das Ziel um zwei Zeiträume vorwärts an. Es wird empfohlen, `max_horizon`, `target_rolling_window_size` und `target_lags` auf „auto“ festzulegen, wodurch diese Werte automatisch für Sie erkannt werden. Im folgenden Beispiel wurden die „auto“-Einstellungen für diese Parameter verwendet. 

```python
time_series_settings = {
    "time_column_name": "day_datetime",
    "grain_column_names": ["store"],
    "max_horizon": "auto",
    "target_lags": "auto",
    "target_rolling_window_size": "auto",
    "preprocess": True,
}
```

> [!NOTE]
> Die Schritte zur Vorverarbeitung bei automatisiertem maschinellen Lernen (Featurenormalisierung, Behandlung fehlender Daten, Umwandlung von Text in numerische Daten usw.) werden Teil des zugrunde liegenden Modells. Bei Verwendung des Modells für Vorhersagen werden die während des Trainings angewendeten Vorverarbeitungsschritte automatisch auf Ihre Eingabedaten angewendet.

Durch die Definition von `grain_column_names` im obigen Codeausschnitt erstellt AutoML zwei separate Zeitreihengruppen, die auch als mehrfache Zeitreihen bezeichnet werden. Wenn keine Körnung definiert ist, geht AutoML davon aus, dass es sich bei dem Dataset um eine einzelne Zeitreihe handelt. Weitere Informationen zu einzelnen Zeitreihen finden Sie unter [energy_demand_notebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand).

Erstellen Sie als Nächstes ein Standardobjekt vom Typ `AutoMLConfig`, geben Sie den Aufgabentyp `forecasting` an, und übermitteln Sie das Experiment. Rufen Sie nach Fertigstellung des Modells die Iteration mit der besten Ausführung ab.

```python
from azureml.core.workspace import Workspace
from azureml.core.experiment import Experiment
from azureml.train.automl import AutoMLConfig
import logging

automl_config = AutoMLConfig(task='forecasting',
                             primary_metric='normalized_root_mean_squared_error',
                             experiment_timeout_minutes=15,
                             enable_early_stopping=True,
                             training_data=train_data,
                             label_column_name=label,
                             n_cross_validations=5,
                             enable_ensembling=False,
                             verbosity=logging.INFO,
                             **time_series_settings)

ws = Workspace.from_config()
experiment = Experiment(ws, "forecasting_example")
local_run = experiment.submit(automl_config, show_output=True)
best_run, fitted_model = local_run.get_output()
```

Sehen Sie sich die [Notebooks zum Vorhersagebeispiel](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning) an. Dort finden Sie ausführliche Codebeispiele zu einer erweiterten Vorhersagekonfiguration, einschließlich:

* [Feiertagserkennung und Erstellen zusätzlicher Merkmale (Featurization)](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-bike-share/auto-ml-forecasting-bike-share.ipynb)
* [Kreuzvalidierung mit rollierendem Ursprung (Rolling Origin Validation)](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)
* [Konfigurierbare Verzögerungen (Lags)](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-bike-share/auto-ml-forecasting-bike-share.ipynb)
* [Aggregierte Zeitfenstermerkmale (Rolling Window Features)](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)
* [DNN](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-beer-remote/auto-ml-forecasting-beer-remote.ipynb)

### <a name="configure-a-dnn-enable-forecasting-experiment"></a>Konfigurieren eines DNN-fähigen Vorhersageexperiments

> [!NOTE]
> Die DNN-Unterstützung für Vorhersagen beim automatisierten maschinellen Lernen befindet sich in der Vorschauphase und wird für lokale Ausführungen nicht unterstützt.

Um DNNs für die Vorhersage zu nutzen, müssen Sie den Parameter `enable_dnn` in AutoMLConfig auf TRUE festlegen. 

```python
automl_config = AutoMLConfig(task='forecasting',
                             enable_dnn=True,
                             ...
                             **time_series_settings)
```
Erfahren Sie mehr über [AutoMLConfig](#configure-and-run-experiment).

Alternativ können Sie die Option `Enable deep learning` in Studio auswählen.
![Alternativer Text](./media/how-to-auto-train-forecast/enable_dnn.png)

Es wird die Verwendung eines AML-Computeclusters mit GPU-SKUs und mindestens zwei Knoten als Computeziel empfohlen. Es wird empfohlen, das Experimenttimeout auf mindestens einige Stunden festzulegen, damit ausreichend Zeit für das DNN-Training zur Verfügung steht.
Weitere Informationen zu AML-Compute und den VM-Größen mit GPUs finden Sie in den Dokumentationen [AML-Compute](how-to-set-up-training-targets.md#amlcompute) und [Für GPU optimierte VM-Größen](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu).

Ein detailliertes Codebeispiel für die Nutzung von DNNs finden Sie im [Notebook für die Vorhersage der Getränkeproduktion](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-beer-remote/auto-ml-forecasting-beer-remote.ipynb).

### <a name="target-rolling-window-aggregation"></a>Rollierende Zeitfensteraggregationen als Ziel
In vielen Fällen ist die beste Information, über die ein Vorhersagemodell verfügen kann, der aktuelle Wert des Ziels. Durch das Erstellen kumulativer Statistiken des Ziels kann die Genauigkeit Ihrer Vorhersagen erhöht werden. Wenn Sie rollierende Zeitfensteraggregationen als Ziel verwenden, können Sie eine rollierende Aggregation von Datenwerten als Features hinzufügen. Legen Sie `target_rolling_window_size` auf Ihre gewünschte ganzzahlige Zeitfensterlänge fest, um rollierende Zeitfenster als Ziel zu ermöglichen. 

Ein Beispiel hierfür finden Sie beim Vorhersagen des Energiebedarfs. Angenommen, Sie fügen ein Feature für rollierende Zeitfenster von drei Tagen hinzu, um thermische Veränderungen beheizter Räume zu erfassen. Im folgenden Beispiel wurde dieses Zeitfenster mit dem Wert 3 erstellt, indem `target_rolling_window_size=3` im `AutoMLConfig`-Konstruktor festgelegt wurde. In der Tabelle wird das Feature Engineering dargestellt, das auftritt, wenn die Zeitfensteraggregation angewendet wird. Spalten für die Werte „minimum“, „maximum“ und „sum“ werden in einem gleitenden Fenster über drei Einträge basierend auf den definierten Einstellungen generiert. Jede Zeile verfügt über ein neues berechnetes Feature, für den Zeitstempel „8. September 2017, 4:00 Uhr“ werden die Werte „maximum“, „minimum“ und „sum“ mithilfe der Anforderungswerte für den Zeitstempel „8. September 2017, 3:00 Uhr“ berechnet. Dieses drei Einträge umfassende Fenster wird verschoben, um die verbleibenden Zeilen mit Daten aufzufüllen.

![alt text](./media/how-to-auto-train-forecast/target-roll.svg)

Durch Erzeugen und Verwenden dieser zusätzlichen Features als zusätzliche Kontextdaten wird die Genauigkeit des Trainingsmodells gesteigert.

Sehen Sie sich ein Python-Codebeispiel an, in dem das [Feature für rollierende Zeitfensteraggregationen als Ziel](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb) verwendet wird.

### <a name="view-feature-engineering-summary"></a>Anzeigen der Zusammenfassung der Featureentwicklung

Für Aufgabentypen mit Zeitreihen beim automatisierten maschinellen Lernen können Sie Details aus dem Featureentwicklungsprozess anzeigen. Der folgende Code zeigt jedes Rohfeature sowie die folgenden Attribute:

* Name des Rohfeatures
* Anzahl der entwickelten Features, die aus diesem Rohfeature gebildet wurden
* Erkannter Typ
* Ob das Feature gelöscht wurde
* Liste der Featuretransformationen für dieses Rohfeature

```python
fitted_model.named_steps['timeseriestransformer'].get_featurization_summary()
```

## <a name="forecasting-with-best-model"></a>Vorhersagen mit dem besten Modell

Verwenden Sie die beste Modelliteration, um Werte für das Testdataset vorherzusagen.

Die `forecast()`-Funktion sollte anstelle von `predict()` verwendet werden. Dadurch kann festgelegt werden, wann Vorhersagen beginnen sollen. Im folgenden Beispiel ersetzen Sie zunächst alle Werte in `y_pred` durch `NaN`. Der Ursprung der Vorhersage liegt in diesem Fall am Ende der Trainingsdaten, wie es normalerweise bei der Verwendung von `predict()` der Fall wäre. Wenn Sie jedoch nur die zweite Hälfte von `y_pred` durch `NaN` ersetzen, lässt die Funktion die numerischen Werte in der ersten Hälfte unverändert, sagt aber die `NaN`-Werte in der zweiten Hälfte voraus. Die Funktion gibt sowohl die vorhergesagten Werte als auch die angepassten Features zurück.

Sie können den Parameter `forecast_destination` in der Funktion `forecast()` auch verwenden, um Werte bis zu einem bestimmten Datum vorherzusagen.

```python
label_query = test_labels.copy().astype(np.float)
label_query.fill(np.nan)
label_fcst, data_trans = fitted_pipeline.forecast(
    test_data, label_query, forecast_destination=pd.Timestamp(2019, 1, 8))
```

Berechnen Sie den RMSE-Wert (Root Mean Squared Error) zwischen den tatsächlichen Werten (`actual_labels`) und den vorhergesagten Werten (`predict_labels`).

```python
from sklearn.metrics import mean_squared_error
from math import sqrt

rmse = sqrt(mean_squared_error(actual_labels, predict_labels))
rmse
```

Nach der Ermittlung der allgemeinen Modellgenauigkeit besteht der nächste Schritt in der Regel darin, mithilfe des Modells unbekannte zukünftige Werte vorherzusagen. Stellen Sie ein Dataset im gleichen Format wie das Testdataset `test_data`, aber mit zukünftigen Datums-/Uhrzeitwerten bereit, um einen Vorhersagesatz mit Vorhersagewerten für die einzelnen Zeitreihenschritte zu erhalten. Angenommen, die letzten Zeitreihendatensätze im Dataset waren für den 31.12.2018. Wenn Sie die Nachfrage für den Folgetag (oder für beliebig viele Vorhersagezeiträume < = `max_horizon`) vorhersagen möchten, erstellen Sie für jede Filiale einen einzelnen Zeitreihendatensatz für den 01.01.2019.

    day_datetime,store,week_of_year
    01/01/2019,A,1
    01/01/2019,A,1

Wiederholen Sie die erforderlichen Schritte, um diese zukünftigen Daten in einen Datenrahmen zu laden, und führen Sie anschließend `best_run.predict(test_data)` aus, um zukünftige Werte vorherzusagen.

> [!NOTE]
> Werte können nur für eine Anzahl von Zeiträumen vorhergesagt werden, die maximal dem Wert von `max_horizon` entspricht. Das Modell muss mit einem weiter in der Zukunft liegenden Vorhersagehorizont neu trainiert werden, um zukünftige Werte vorhersagen können, die über den aktuellen Vorhersagehorizont hinausgehen.

## <a name="next-steps"></a>Nächste Schritte

* Absolvieren Sie das [Tutorial](tutorial-auto-train-models.md), um zu erfahren, wie Sie Experimente mit automatisiertem maschinellem Lernen erstellen.
* Machen Sie sich mit der Referenzdokumentation [Azure Machine Learning SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) vertraut.
