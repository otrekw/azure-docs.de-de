---
title: Verwenden von autoML zum Erstellen und Bereitstellen von Modellen
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie mit Azure Machine Learning automatisierte Machine Learning-Modelle erstellen, überprüfen und bereitstellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: nibaccam
author: tsikiksr
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 03/10/2020
ms.openlocfilehash: 0d6fa02578814c4c5d034be05cbc63093d70603b
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "81257231"
---
# <a name="create-review-and-deploy-automated-machine-learning-models-with-azure-machine-learning"></a>Erstellen, Überprüfen und Bereitstellen von automatisierten Machine Learning-Modellen mit Azure Machine Learning
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie automatisierte Machine Learning-Modelle ohne eine einzige Codezeile über die Azure Machine Learning Studio-Oberfläche erstellen, untersuchen und bereitstellen. Automatisiertes Machine Learning ist ein Prozess, bei dem der beste Machine Learning-Algorithmus für Ihre spezifischen Daten für Sie ausgewählt wird. Dieser Prozess ermöglicht Ihnen die schnelle Erstellung von Machine Learning-Modellen. [Weitere Informationen zu automatisiertem Machine Learning](concept-automated-ml.md).
 
Ein End-to-End-Beispiel finden Sie im [Tutorial zum Erstellen eines Klassifizierungsmodells mit der Oberfläche für automatisiertes maschinelles Lernen von Azure Machine Learning](tutorial-first-experiment-automated-ml.md). 

Wenn Sie eine auf Python-Code basierende Umgebung bevorzugen, [konfigurieren Sie Ihre Experimenten mit automatisiertem maschinellem Lernen](how-to-configure-auto-train.md) mit dem Azure Machine Learning SDK.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) noch heute aus.

* Ein Azure Machine Learning-Arbeitsbereich mit dem Typ **Enterprise Edition**. Weitere Informationen finden Sie unter [Erstellen eines Azure Machine Learning-Arbeitsbereichs](how-to-manage-workspace.md).  Informationen zum Upgraden eines vorhandenen Arbeitsbereichs auf Enterprise Edition finden Sie unter [Upgrade auf Enterprise Edition](how-to-manage-workspace.md#upgrade).

## <a name="get-started"></a>Erste Schritte

1. Melden Sie sich unter https://ml.azure.com bei Azure Machine Learning an. 

1. Wählen Sie Ihr Abonnement und Ihren Arbeitsbereich aus. 

1. Navigieren Sie zum linken Bereich. Wählen Sie im Abschnitt **Erstellen** die Option **Automatisiertes ML** aus.

[![Navigationsbereich von Azure Machine Learning Studio](media/how-to-use-automated-ml-for-ml-models/nav-pane.png)](media/how-to-use-automated-ml-for-ml-models/nav-pane-expanded.png)

 Wenn Sie zum ersten Mal Experimente ausführen, werden eine leere Liste und Links zur Dokumentation angezeigt. 

Andernfalls wird eine Liste ihrer letzten automatisierten Machine Learning-Experimente angezeigt, einschließlich derjenigen, die mit dem SDK erstellt wurden. 

## <a name="create-and-run-experiment"></a>Erstellen und Ausführen eines Experiments

1. Wählen Sie **+ Neue Ausführung von automatisiertem ML** aus, und füllen Sie das Formular aus.

1. Wählen Sie ein Dataset in Ihrem Speichercontainer aus, oder erstellen Sie ein neues Dataset. Datasets können aus lokalen Dateien, Web-URLs, Datenspeichern oder Azure Open Datasets erstellt werden. 

    >[!Important]
    > Anforderungen für Trainingsdaten:
    >* Die Daten müssen in Tabellenform vorliegen.
    >* Der Wert, den Sie vorhersagen möchten (Zielspalte), muss in den Daten vorhanden sein.

    1. Um ein neues Dataset aus einer Datei auf Ihrem lokalen Computer zu erstellen, wählen Sie **Durchsuchen** aus, und wählen Sie dann die Datei aus. 

    1. Weisen Sie Ihrem Dataset einen eindeutigen Namen zu, und geben Sie eine optionale Beschreibung ein. 

    1. Wählen Sie **Weiter** aus, um das Formular **Datenspeicher- und Dateiauswahl** zu öffnen. In diesem Formular wählen Sie aus, wo das Dataset hochgeladen werden soll. Dies kann der Standardspeichercontainer sein, der automatisch mit Ihrem Arbeitsbereich erstellt wird, oder ein Speichercontainer, den Sie für das Experiment auswählen. 

    1. Überprüfen Sie das Formular **Einstellungen und Vorschau** auf Genauigkeit. Das Formular wird ausgehend vom Dateityp intelligent aufgefüllt. 

        Feld| BESCHREIBUNG
        ----|----
        Dateiformat| Definiert das Layout und den Typ der in einer Datei gespeicherten Daten.
        Trennzeichen| Mindestens ein Zeichen zum Angeben der Grenze zwischen separaten, unabhängigen Regionen in Nur-Text- oder anderen Datenströmen.
        Codieren| Gibt an, welche Bit-zu-Zeichen-Schematabelle verwendet werden soll, um Ihr Dataset zu lesen.
        Spaltenüberschriften| Gibt an, wie die Header des Datasets, sofern vorhanden, behandelt werden.
        Zeilen überspringen | Gibt an, wie viele Zeilen im Dataset übersprungen werden.
    
        Wählen Sie **Weiter** aus.

    1. Das Formular **Schema** wird auf intelligente Weise entsprechend den auf dem Formular **Einstellungen und Vorschau** ausgewählten Optionen aufgefüllt. Konfigurieren Sie hier den Datentyp für jede Spalte, überprüfen Sie die Spaltennamen, und wählen Sie für die Spalten, die nicht in Ihr Experiment eingeschlossen werden sollen, die Option **Nicht einschließen** aus. 
            
        Wählen Sie **Weiter** aus.

    1. Das Formular **Details bestätigen** ist eine Zusammenfassung der Informationen, die zuvor in die Formulare **Grundlegende Infos** und **Einstellungen und Vorschau** eingetragen wurden. Sie haben auch die Möglichkeit, ein Datenprofil für Ihr Dataset zu erstellen, indem Sie Compute mit aktivierter Profilerstellung verwenden. Weitere Informationen zur [Datenprofilerstellung](#profile).

        Wählen Sie **Weiter** aus.
1. Wählen Sie Ihr neu erstelltes Dataset aus, sobald es angezeigt wird. Sie können auch eine Vorschau des Datasets und der Stichprobenstatistiken anzeigen. 

1. Geben Sie auf dem Formular **Ausführung konfigurieren** einen eindeutigen Experimentnamen ein.

1. Wählen Sie eine Zielspalte aus: Dies ist die Spalte, für die Sie Vorhersagen ausführen möchten.

1. Wählen Sie einen Compute für den Datenprofilerstellungs- und Trainingsauftrag aus. Eine Liste Ihrer vorhandenen Computes ist in der Dropdownliste verfügbar. Folgen Sie den Anweisungen in Schritt 7, um einen neuen Compute zu erstellen.

1. Wählen Sie **Create a new compute** (Neue Computeressource erstellen) aus, um den Computekontext für dieses Experiment zu konfigurieren.

    Feld|BESCHREIBUNG
    ---|---
    Computename| Geben Sie einen eindeutigen Namen ein, der Ihren Computekontext identifiziert.
    Größe des virtuellen Computers| Wählen Sie die Größe für Ihren Computes aus.
    Min/Max nodes (Min./Max. Knoten) (unter „Erweiterte Einstellungen“)| Um ein Datenprofil zu erstellen, müssen Sie mindestens einen Knoten angeben. Geben Sie die maximale Anzahl von Knoten für Ihren Compute ein. Der Standardwert ist 6 Knoten für einen AML-Compute.
    
    Klicken Sie auf **Erstellen**. Das Erstellen einer neuen Computeressource kann einige Minuten dauern.

    >[!NOTE]
    > Ihr Computename gibt an, ob für den von Ihnen ausgewählten/erstellten Compute *Profilerstellung aktiviert* ist. (Weitere Informationen finden Sie im Abschnitt [Datenprofilerstellung](#profile).)

    Wählen Sie **Weiter** aus.

1. Wählen Sie auf dem Formular **Aufgabentyp und Einstellungen** den Aufgabentyp aus: Klassifizierung, Regression oder Prognose (Vorhersage). Weitere Informationen finden Sie unter [Definieren einer Aufgabe für maschinelles Lernen](how-to-define-task-type.md).

    1. Für eine Klassifizierung können Sie auch Deep Learning aktivieren, das für Textmerkmalserstellungen verwendet wird.

    1. Für Vorhersagen:
        1. Wählen Sie die Zeitspalte aus: Diese Spalte enthält die zu verwendenden Zeitdaten.

        1. Wählen Sie den Vorhersagehorizont aus: Geben Sie an, wie viele Zeiteinheiten (Minuten/Stunden/Tage/Wochen/Monate/Jahre) das Modell die Zukunft vorhersagen können soll. Je weiter das Modell die Zukunft vorhersagen muss, desto ungenauer wird es. [Weitere Informationen zu Vorhersagen und zum Vorhersagehorizont](how-to-auto-train-forecast.md).

1. (Optional:) Anzeigen weiterer Konfigurationseinstellungen: zusätzliche Einstellungen, mit denen Sie den Trainingsauftrag besser steuern können. Andernfalls werden die Standardwerte auf Basis der Experimentauswahl und -daten angewendet. 

    Zusätzliche Konfigurationen|BESCHREIBUNG
    ------|------
    Primary metric (Primäre Metrik)| Die wichtigste Metrik, die für die Bewertung Ihres Modells verwendet wird. [Weitere Informationen zur Modellmetriken](how-to-configure-auto-train.md#explore-model-metrics).
    Automatische Featurebereitstellung| Aktivieren oder deaktivieren Sie mit dieser Option die Vorverarbeitung durch automatisiertes Machine Learning. Vorverarbeitung umfasst die automatische Datenbereinigung, die Vorbereitung und die Transformation, um synthetische Features zu generieren. Für den Aufgabentyp Zeitreihenvorhersagen nicht unterstützt. [Weitere Informationen zur Vorverarbeitung](#featurization). 
    Explain best model (Bestes Modell erläutern) | Wählen Sie diese Option aus, um die Erläuterungsfähigkeit des empfohlenen besten Modells anzuzeigen oder zu deaktivieren.
    Blocked algorithm (Blockierter Algorithmus)| Wählen Sie Algorithmen aus, die Sie aus den Trainingsauftrag ausschließen möchten.
    Beendigungskriterium| Wenn eines dieser Kriterien erfüllt ist, wird der Trainingsauftrag beendet. <br> *Training job time (hours)* Trainingsauftragszeit (Stunden): Gibt an, wie lange der Trainingsauftrag ausgeführt werden soll. <br> *Metric score threshold* (Metrischer Bewertungsschwellenwert):  Die Metrikmindestbewertung für alle Pipelines. Auf diese Weise wird sichergestellt, dass Sie nicht mehr Zeit für den Trainingsauftrag aufwenden als nötig, wenn Sie eine definierte Zielmetrik verwenden, die Sie erreichen möchten.
    Überprüfen| Wählen Sie eine der Optionen für Kreuzvalidierung aus, die im Trainingsauftrag verwendet werden soll. [Weitere Informationen zur Kreuzvalidierung](how-to-configure-auto-train.md).
    Parallelität| *Max concurrent iterations* (Maximale Anzahl gleichzeitiger Iterationen): Die maximale Anzahl von Pipelines (Iterationen), die im Trainingsauftrag getestet werden. Der Auftrag wird nicht häufiger als die angegebene Anzahl von Iterationen ausgeführt.

1. (Optional:) Anzeigen von Featureerstellungseinstellungen: Wenn Sie **Automatische Merkmalserstellung** im Formular **Additional configuration settings** (Zusätzliche Konfigurationseinstellungen) aktivieren, geben Sie in diesem Formular an, für welche Spalten diese Featureerstellung durchgeführt werden soll, und wählen den statistischen Wert aus, der für fehlende Werte verwendet werden soll.

<a name="profile"></a>

## <a name="data-profiling--summary-stats"></a>Statistiken für Datenprofilerstellung und Zusammenfassung

Sie können eine Vielzahl von Zusammenfassungsstatistiken zu Ihrem Dataset erhalten, um zu überprüfen, ob Ihr Dataset für ML bereit ist. Für nicht numerische Spalten sind nur grundlegende Statistiken wie Mindestwert, Maximalwert und Fehleranzahl enthalten. Für numerische Spalten können Sie auch ihre statistische Zeit und die geschätzten Quantile überprüfen. Unser Datenprofil umfasst Folgendes:

>[!NOTE]
> Leere Einträge werden für Features mit irrelevanten Typen angezeigt.

Statistik|BESCHREIBUNG
------|------
Funktion| Der Name der Spalte, die zusammengefasst wird.
Profil| Eine Inlinevisualisierung basierend auf dem abgeleiteten Typ. Beispielsweise weisen Zeichenfolgen, boolesche Werte und Datumsangaben Wertanzahlen auf, während Dezimalwerte (numerische Werte) Näherungswerte für Histogramme besitzen. Dadurch können Sie einen schnellen Überblick über die Verteilung der Daten erhalten.
Typverteilung| Eine Inlinewertanzahl von Typen in einer Spalte. NULL-Werte sind eigene Typen, sodass diese Visualisierung für die Erkennung von ungewöhnlichen oder fehlenden Werten nützlich ist.
type|Der abgeleitete Typ der Spalte. Mögliche Werte: Zeichenfolgen, boolesche Werte, Datumsangaben und Dezimalwerte.
Min| Der Mindestwert der Spalte. Leere Einträge werden für Features angezeigt, deren Typ keine inhärente Reihenfolge aufweist (z.B. boolesche Werte).
Max| Der Höchstwert der Spalte. 
Anzahl| Die Gesamtanzahl der fehlenden und nicht fehlenden Einträge in der Spalte.
Fehlt nicht (Anzahl)| Die Anzahl der nicht fehlenden Einträge in der Spalte. Leere Zeichenfolgen und Fehler werden als Werte behandelt, damit sie nicht zur „Anzahl nicht fehlender“ beitragen.
Quantile| Die geschätzten Werte in jedem Quantil, um einen Eindruck von der Verteilung der Daten bereitzustellen.
Mittelwert| Das arithmetische Mittel der Spalte.
Standardabweichung| Das Maß der Verteilung oder Abweichung der Daten dieser Spalte.
Variance| Das Maß, wie weit die Daten dieser Spalte gegenüber dem Durchschnittswert verteilt sind. 
Schiefe| Das Maß für die Unterschiede zwischen den Daten dieser Spalte und der normalen Verteilung.
Kurtosis| Das Maß für die schweren Ränder der Daten dieser Spalte im Vergleich mit der normalen Verteilung.

<a name="featurization"></a>

## <a name="advanced-featurization-options"></a>Erweiterte Optionen bei der Featurebereitstellung

Automatisiertes maschinelles Lernen bietet eine Vorverarbeitung und automatische Schutzmaßnahmen für Daten, damit Sie potenzielle Probleme mit Ihren Daten identifizieren und verwalten können. 

### <a name="preprocessing"></a>Preprocessing (Vorverarbeitung)

> [!NOTE]
> Wenn Sie Ihre mit ML automatisch erstellten Modelle in ein [ONNX-Modell](concept-onnx.md) exportieren möchten, beachten Sie, dass nur die mit * gekennzeichneten Optionen zur Featurebereitstellung im ONNX-Format unterstützt werden. Erfahren Sie mehr über das [Konvertieren von Modellen zu ONNX](concept-automated-ml.md#use-with-onnx). 

|Vorverarbeitungsschritte&nbsp;| BESCHREIBUNG |
| ------------- | ------------- |
|Löschen von Funktionen mit hoher Kardinalität oder ohne Varianz* |Löscht diese aus Trainings- und Validierungssets. Dazu gehören Funktionen, denen alle Werte fehlen, die denselben Wert für alle Zeilen haben oder die eine sehr hohe Kardinalität (z.B. Hashwerte, IDs oder GUIDs) aufweisen.|
|Imputieren von fehlenden Werten* |Bei numerischen Features werden fehlende Werte mit dem Durchschnitt der Werte in der Spalte imputiert.<br/><br/>Bei kategorischen Features werden fehlende Werte mit dem am häufigsten vorkommenden Wert imputiert.|
|Generieren zusätzlicher Funktionen* |Für DateTime-Funktionen: Jahr, Monat, Tag, Tag der Woche, Tag des Jahres, Quartal, Woche des Jahres, Stunde, Minute, Sekunde.<br/><br/>Für Text-Funktionen: Ausdruckshäufigkeit basierend auf Unigrammen, Bigrammen und Trigrammen.|
|Transformieren und codieren*|Numerische Features mit wenigen eindeutigen Werten werden in kategorische Features transformiert.<br/><br/>One-Hot-Codierung wird für niedrige Kardinalität kategorisch durchgeführt, für hohe Kardinalität gilt One-Hot-Hashcodierung.|
|Worteinbettungen|Ein Textfeaturizer, der Vektoren von Texttoken mithilfe eines vortrainierten Modells in Satzvektoren konvertiert. Der Einbettungsvektor jedes Worts in einem Dokument wird in einem Dokumentenfeaturevektor zusammengefasst.|
|Zielcodierungen|Bei kategorischen Features wird jede Kategorie mit gemitteltem Zielwert für Regressionsprobleme und der Klassenwahrscheinlichkeit für jede Klasse für Klassifizierungsprobleme zugeordnet. Häufigkeitsbasierte Gewichtung und k-fache Kreuzvalidierung werden angewendet, um die Überanpassung der Zuordnung und das Rauschen durch dünn besetzte Datenkategorien zu verringern.|
|Textzielcodierung|Für die Texteingabe wird ein gestapeltes lineares Modell mit Bag-of-Words verwendet, um die Wahrscheinlichkeit der einzelnen Klassen zu generieren.|
|Gewichtung der Beweise (Weight of Evidence, WoE)|Berechnet den WoE-Wert als ein Maß für die Korrelation von kategorischen Spalten zur Zielspalte. Dieser Wert wird als Logarithmus des Verhältnisses von In-Class- zu Out-of-Class-Wahrscheinlichkeiten berechnet. Dieser Schritt gibt eine numerische Featurespalte pro Klasse aus und erspart die Notwendigkeit, fehlende Werte und Ausreißerbehandlung explizit zu berechnen.|
|Clusterabstand|Trainiert ein K-Means-Clusteringmodell für alle numerischen Spalten.  Gibt k neue Features aus, ein neues numerisches Feature pro Cluster, das den Abstand jeder Stichprobe zum Schwerpunkt jedes Clusters enthält.|

### <a name="data-guardrails"></a>Schutzmaßnahmen für Daten

Schutzmaßnahmen für Daten werden angewendet, wenn die automatische Merkmalserstellung aktiviert ist oder die Überprüfung auf „automatisch“ eingestellt ist. Schutzmaßnahmen für Daten helfen Ihnen dabei, potenzielle Probleme mit Ihren Daten zu erkennen (z. B. fehlende Werte, Klassenungleichgewicht) und Korrekturmaßnahmen zur Verbesserung der Ergebnisse zu ergreifen. 

Benutzer können die Schutzmaßnahmen für Daten in Studio innerhalb der Registerkarte **Schutzmaßnahmen für Daten** einer automatisierten ML-Ausführung oder durch Festlegen von ```show_output=True``` beim Übermitteln eines Experiments mit dem Python-SDK überprüfen. 

#### <a name="data-guardrail-states"></a>Zustände von Schutzmaßnahmen für Daten

Die Schutzmaßnahmen für Daten werden einen von drei Zuständen anzeigen: **Erfolgreich**, **Fertig** oder **Benachrichtigt**.

State| BESCHREIBUNG
----|----
Erfolgreich| Es wurden keine Datenprobleme erkannt, und es ist keine Benutzeraktion erforderlich. 
Vorgehensweise| Die Änderungen wurden auf Ihre Daten angewendet. Den Benutzern wird empfohlen, die vom automatisierten maschinellen Lernen ergriffenen Korrekturmaßnahmen zu überprüfen, um sicherzustellen, dass die Änderungen mit den erwarteten Ergebnissen übereinstimmen. 
Benachrichtigt| Ein Datenproblem, das nicht behoben werden konnte, wurde erkannt. Den Benutzern wird empfohlen, das Problem zu überarbeiten und zu beheben. 

>[!NOTE]
> Frühere Versionen des automatisierten ML-Experiments zeigten einen vierten Zustand an: **Behoben**. Neuere Experimente zeigen diesen Zustand nicht mehr an, und alle Schutzmaßnahmen, die den Zustand **Behoben** angezeigt haben, zeigen jetzt **Fertig** an.   

In der folgenden Tabelle werden die derzeit unterstützten Schutzmaßnahmen für Daten und die zugehörigen Statuswerte beschrieben, die Benutzern beim Übermitteln Ihres Experiments unter Umständen angezeigt werden.

Schutzmaßnahme|Status|Bedingung&nbsp;für&nbsp;Auslösung
---|---|---
Fehlende Featurewerte durch Imputation ergänzen |**Erfolgreich** <br><br><br> **Fertig**| Es wurden keine fehlenden Featurewerte in Ihren Trainingsdaten festgestellt. Erfahren Sie mehr über die [Imputation fehlender Werte](https://docs.microsoft.com/azure/machine-learning/how-to-use-automated-ml-for-ml-models#advanced-featurization-options). <br><br> Fehlende Featurewerte wurden in Ihren Trainingsdaten festgestellt und zugerechnet.
Behandeln von Features mit hoher Kardinalität |**Erfolgreich** <br><br><br> **Fertig**| Ihre Eingaben wurden analysiert, und es wurden keine Features mit hoher Kardinalität gefunden. Erfahren Sie mehr über das [Erkennen von Features mit hoher Kardinalität.](https://docs.microsoft.com/azure/machine-learning/how-to-use-automated-ml-for-ml-models#advanced-featurization-options) <br><br> Features mit hoher Kardinalität wurden in Ihren Eingaben erkannt und behandelt.
Verarbeitung der Überprüfungsaufteilung |**Fertig**| *Die Überprüfungskonfiguration wurde auf „auto“ festgelegt, und die Trainingsdaten enthielten **weniger** als 20.000 Zeilen.* <br> Jede Iteration des trainierten Modells wurde durch Kreuzvalidierung überprüft. Erfahren Sie mehr über [Überprüfungsdaten](https://docs.microsoft.com/azure/machine-learning/how-to-configure-auto-train#train-and-validation-data). <br><br> *Die Überprüfungskonfiguration wurde auf „auto“ festgelegt, und die Trainingsdaten enthielten **mehr** als 20.000 Zeilen.* <br> Die Eingabedaten wurden zur Überprüfung des Modells in ein Trainingsdataset und ein Validierungsdataset aufgeteilt.
Ausgewogenheitserkennung für Klassen |**Erfolgreich** <br><br><br><br> **Benachrichtigt** | Ihre Eingaben wurden analysiert, und alle Klassen in Ihren Trainingsdaten sind ausgeglichen. Ein Dataset gilt als ausgewogen, wenn jede Klasse im Datensatz gemessen an Anzahl und Verhältnis der Stichproben gut repräsentiert ist. <br><br><br> In Ihren Eingaben wurden unausgeglichene Klassen erkannt. Beheben Sie das Ausgleichsproblem, um den Modelltrend zu beheben. Erfahren Sie mehr über [unausgeglichene Daten](https://docs.microsoft.com/azure/machine-learning/concept-manage-ml-pitfalls#identify-models-with-imbalanced-data).
Erkennung von Arbeitsspeicherproblemen |**Erfolgreich** <br><br><br><br> **Fertig** |<br> Die ausgewählten Horizont-, Verzögerungs- und Zeitfensterwerte wurden analysiert, und es wurden keine potenziellen Probleme aufgrund von unzureichendem Speicherplatz erkannt. Erfahren Sie mehr über [Vorhersagekonfigurationen](https://docs.microsoft.com/azure/machine-learning/how-to-auto-train-forecast#configure-and-run-experiment) von Zeitreihen. <br><br><br>Die ausgewählten Horizont-, Verzögerungs- und Zeitfensterwerte wurden analysiert, und für Ihr Experiment wird möglicherweise nicht genügend Speicherplatz zur Verfügung stehen. Die Konfigurationen für Verzögerung oder rollierende Zeitfenster wurden deaktiviert.
Häufigkeitserkennung |**Erfolgreich** <br><br><br><br> **Fertig** |<br> Die Zeitreihe wurde analysiert, und alle Datenpunkte entsprechen der erkannten Häufigkeit. <br> <br> Die Zeitreihe wurde analysiert, und es wurden Datenpunkte ermittelt, die nicht mit der erkannten Häufigkeit übereinstimmen. Diese Datenpunkte wurden aus dem Dataset entfernt. Erfahren Sie mehr über die [Datenaufbereitung für die Zeitreihenvorhersage](https://docs.microsoft.com/azure/machine-learning/how-to-auto-train-forecast#preparing-data).

## <a name="run-experiment-and-view-results"></a>Ausführen des Experiments und Anzeigen der Ergebnisse

Wählen Sie **Fertig stellen** aus, um das Experiment auszuführen. Der Vorgang zum Vorbereiten eines Experiments kann bis zu 10 Minuten dauern. Ein Ausführen von Trainingsaufträgen kann für jede Pipeline weitere 2 bis 3 Minuten beanspruchen.

### <a name="view-experiment-details"></a>Anzeigen von Details zum Experiment

Die Anzeige für **Ausführungsdetails** wird mit der Registerkarte **Details** geöffnet. In dieser Anzeige wird eine Zusammenfassung der Experimentausführung einschließlich einer Statusleiste oben neben der Ausführungsnummer angezeigt. 

Die Registerkarte **Modelle** enthält eine Liste der erstellten Modelle, wobei diese nach der Metrikbewertung (Metrikscore) geordnet sind. Standardmäßig steht das Modell, das anhand der ausgewählten Metrik die höchste Bewertung erhält, in der Liste ganz oben. Während der Trainingsauftrag weitere Modelle testet, werden diese zur Liste hinzugefügt. Verwenden Sie diese Liste, um einen schnellen Vergleich der Metriken für die bisher generierten Modelle zu erhalten.

[![Dashboard mit den Ausführungsdetails](media/how-to-use-automated-ml-for-ml-models/run-details.png)](media/how-to-use-automated-ml-for-ml-models/run-details-expanded.png#lightbox)

### <a name="view-training-run-details"></a>Anzeigen der Details der Trainingsausführung

Führen Sie einen Drilldown für eines der abgeschlossenen Modelle aus, um Details zur Trainingsausführung anzuzeigen, z. B. Ausführungsmetriken auf der Registerkarte **Modelldetails** oder Leistungsdiagramme auf der Registerkarte **Visualisierungen**. [Weitere Informationen zu Diagrammen](how-to-understand-automated-ml.md).

[![Details zur Iteration](media/how-to-use-automated-ml-for-ml-models/iteration-details.png)](media/how-to-use-automated-ml-for-ml-models/iteration-details-expanded.png)

## <a name="deploy-your-model"></a>Bereitstellen Ihres Modells

Sobald Ihnen das beste Modell vorliegt, ist es an der Zeit, das Modell als Webdienst bereitzustellen, um Vorhersagen für neue Daten zu erstellen.

Automatisiertes maschinelles Lernen unterstützt Sie dabei, das Modell bereitzustellen, ohne Code zu schreiben:

1. Sie haben einige Optionen für die Bereitstellung. 

    + Option 1: Um das beste Modell (entsprechend den definierten Metrikkriterien) bereitzustellen, wählen Sie die Schaltfläche **Deploy Best Model** (Bestes Modell bereitstellen) auf der Registerkarte **Details** aus.

    + Option 2: Um eine bestimmte Modelliteration aus diesem Experiment bereitzustellen, führen Sie einen Drilldown für das Modell aus, um die Registerkarte **Modelldetails** zu öffnen, und wählen Sie **Modell bereitstellen** aus.

1. Füllen Sie den Bereich **Modell bereitstellen** aus.

    Feld| Wert
    ----|----
    Name| Geben Sie einen eindeutigen Namen für die Bereitstellung ein.
    BESCHREIBUNG| Geben Sie eine Beschreibung ein, um den Zweck dieser Bereitstellung genauer anzugeben.
    Computetyp| Wählen Sie die Art des bereitzustellenden Endpunkts aus: *Azure Kubernetes Service (AKS)* oder *Azure-Containerinstanz (ACI)* .
    Computename| *Nur für AKS:* Wählen Sie den Namen des AKS-Clusters aus, der als Ziel für die Bereitstellung verwendet werden soll.
    Authentifizierung aktivieren | Wählen Sie diese Option aus, um eine token- oder schlüsselbasierte Authentifizierung zu ermöglichen.
    Use custom deployment assets (Benutzerdefinierte Bereitstellungsressourcen verwenden)| Aktivieren Sie dieses Feature, wenn Sie Ihr eigenes Bewertungsskript und Ihre eigene Umgebungsdatei hochladen möchten. Weitere Informationen zu Bewertungsskripts finden Sie [hier](how-to-deploy-and-where.md#script).

    >[!Important]
    > Ein Dateiname muss weniger als 32 Zeichen haben und muss mit einem alphanumerischen Zeichen beginnen und enden. Dazwischen darf ein Name Bindestriche, Unterstriche, Punkte und alphanumerische Zeichen enthalten. Leerzeichen sind nicht zulässig.

    Das Menü *Erweitert* enthält Standard-Bereitstellungsfeatures wie [Datensammlung](how-to-enable-app-insights.md) und Einstellungen für die Ressourcenauslastung. Wenn Sie diese Standardeinstellungen überschreiben möchten, verwenden Sie dafür dieses Menü.

1. Klicken Sie auf **Bereitstellen**. Die Bereitstellung kann bis zu 20 Minuten dauern.

Nun haben Sie einen einsatzfähigen Webdienst, mit dem Vorhersagen generiert werden können! Sie können die Vorhersagen testen, indem Sie den Dienst über die [in Power BI integrierte Azure Machine Learning-Unterstützung](how-to-consume-web-service.md#consume-the-service-from-power-bi) abfragen.

## <a name="next-steps"></a>Nächste Schritte

* [Informationen zum Nutzen eines Webdiensts](https://docs.microsoft.com/azure/machine-learning/how-to-consume-web-service)
* [Grundlagen von Ergebnissen des automatisierten maschinellen Lernens](how-to-understand-automated-ml.md)
* [Weitere Informationen zu automatisiertem Machine Learning](concept-automated-ml.md) und Azure Machine Learning.
