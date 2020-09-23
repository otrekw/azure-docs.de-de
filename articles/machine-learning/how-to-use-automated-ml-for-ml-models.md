---
title: Erstellen und Bereitstellen von Modellen mithilfe von AutoML
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie mit Azure Machine Learning automatisierte Machine Learning-Modelle erstellen, überprüfen und bereitstellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: nibaccam
author: aniththa
ms.reviewer: nibaccam
ms.date: 07/10/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: 10c0200aae5ffa432c2da037d58d455fc28e8acd
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90904955"
---
# <a name="create-review-and-deploy-automated-machine-learning-models-with-azure-machine-learning"></a>Erstellen, Überprüfen und Bereitstellen von automatisierten Machine Learning-Modellen mit Azure Machine Learning


In diesem Artikel erfahren Sie, wie Sie automatisierte Machine Learning-Modelle ohne eine einzige Codezeile in Azure Machine Learning Studio erstellen, untersuchen und bereitstellen.

Automatisiertes Machine Learning ist ein Prozess, bei dem der beste Machine Learning-Algorithmus für Ihre spezifischen Daten für Sie ausgewählt wird. Dieser Prozess ermöglicht Ihnen die schnelle Erstellung von Machine Learning-Modellen. [Weitere Informationen zu automatisiertem Machine Learning](concept-automated-ml.md).
 
Ein End-to-End-Beispiel finden Sie im [Tutorial zum Erstellen eines Klassifizierungsmodells mit der Oberfläche für automatisiertes maschinelles Lernen von Azure Machine Learning](tutorial-first-experiment-automated-ml.md). 

Wenn Sie eine auf Python-Code basierende Umgebung bevorzugen, [konfigurieren Sie Ihre Experimenten mit automatisiertem maschinellem Lernen](how-to-configure-auto-train.md) mit dem Azure Machine Learning SDK.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) noch heute aus.

* Ein Azure Machine Learning-Arbeitsbereich. Weitere Informationen finden Sie unter [Erstellen eines Azure Machine Learning-Arbeitsbereichs](how-to-manage-workspace.md). 

## <a name="get-started"></a>Erste Schritte

1. Melden Sie sich unter https://ml.azure.com bei Azure Machine Learning an. 

1. Wählen Sie Ihr Abonnement und Ihren Arbeitsbereich aus. 

1. Navigieren Sie zum linken Bereich. Wählen Sie im Abschnitt **Erstellen** die Option **Automatisiertes ML** aus.

[![Navigationsbereich von Azure Machine Learning Studio](media/how-to-use-automated-ml-for-ml-models/nav-pane.png)](media/how-to-use-automated-ml-for-ml-models/nav-pane-expanded.png)

 Wenn Sie zum ersten Mal Experimente ausführen, werden eine leere Liste und Links zur Dokumentation angezeigt. 

Andernfalls wird eine Liste ihrer letzten automatisierten Machine Learning-Experimente angezeigt, einschließlich derjenigen, die mit dem SDK erstellt wurden. 

## <a name="create-and-run-experiment"></a>Erstellen und Ausführen eines Experiments

1. Wählen Sie **+ Neue Ausführung von automatisiertem ML** aus, und füllen Sie das Formular aus.

1. Wählen Sie ein Dataset in Ihrem Speichercontainer aus, oder erstellen Sie ein neues Dataset. Datasets können aus lokalen Dateien, Web-URLs, Datenspeichern oder Azure Open Datasets erstellt werden. Weitere Informationen zur Erstellung von Datasets finden Sie [hier](how-to-create-register-datasets.md).  

    >[!Important]
    > Anforderungen für Trainingsdaten:
    >* Die Daten müssen in Tabellenform vorliegen.
    >* Der Wert, den Sie vorhersagen möchten (Zielspalte), muss in den Daten vorhanden sein.

    1. Um ein neues Dataset aus einer Datei auf Ihrem lokalen Computer zu erstellen, wählen Sie **+Dataset erstellen** und anschließend **Aus lokaler Datei** aus. 

    1. Geben Sie Ihrem Dataset im Formular **Grundlegende Informationen** einen eindeutigen Namen, und geben Sie optional eine Beschreibung an. 

    1. Wählen Sie **Weiter** aus, um das Formular **Datenspeicher- und Dateiauswahl** zu öffnen. In diesem Formular wählen Sie aus, wo das Dataset hochgeladen werden soll. Dies kann der Standardspeichercontainer sein, der automatisch mit Ihrem Arbeitsbereich erstellt wird, oder ein Speichercontainer, den Sie für das Experiment auswählen. 
    
        1. Wenn sich Ihre Daten hinter einem virtuellen Netzwerk befinden, müssen Sie die Funktion zum **Überspringen der Überprüfung** aktivieren, um sicherzustellen, dass der Arbeitsbereich auf Ihre Daten zugreifen kann. Weitere Informationen finden Sie unter [Verwenden von Azure Machine Learning Studio in einem virtuellen Azure-Netzwerk](how-to-enable-studio-virtual-network.md). 
    
    1. Wählen Sie **Durchsuchen** aus, um die Datendatei für das Dataset hochzuladen. 

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
    VM-Priorität| Virtuelle Computer mit niedriger Priorität sind kostengünstiger, bieten jedoch keine garantierten Computeknoten. 
    Typ des virtuellen Computers| Wählen Sie CPU oder GPU als VM-Typ aus.
    Größe des virtuellen Computers| Wählen Sie die Größe für Ihren Computes aus.
    Min/Max nodes (Min./Max. Knoten)| Um ein Datenprofil zu erstellen, müssen Sie mindestens einen Knoten angeben. Geben Sie die maximale Anzahl von Knoten für Ihren Compute ein. Der Standardwert ist 6 Knoten für einen AML-Compute.
    Erweiterte Einstellungen | Mit diesen Einstellungen können Sie ein Benutzerkonto und ein vorhandenes virtuelles Netzwerk für Ihr Experiment konfigurieren. 
    
    Klicken Sie auf **Erstellen**. Das Erstellen einer neuen Computeressource kann einige Minuten dauern.

    >[!NOTE]
    > Ihr Computename gibt an, ob für den von Ihnen ausgewählten/erstellten Compute *Profilerstellung aktiviert* ist. (Weitere Informationen finden Sie im Abschnitt [Datenprofilerstellung](#profile).)

    Wählen Sie **Weiter** aus.

1. Wählen Sie auf dem Formular **Aufgabentyp und Einstellungen** den Aufgabentyp aus: Klassifizierung, Regression oder Prognose (Vorhersage). Weitere Informationen finden Sie unter [Unterstützte Aufgabentypen](concept-automated-ml.md#when-to-use-automl-classify-regression--forecast).

    1. Für die **Klassifizierung** können Sie auch Deep Learning aktivieren.
    
        Wenn Deep Learning aktiviert ist, ist die Überprüfung auf _train_validation split_ beschränkt. [Erfahren Sie mehr zu Überprüfungsoptionen](how-to-configure-cross-validation-data-splits.md).


    1. Für **Vorhersagen** haben Sie folgende Möglichkeiten: 
    
        1. Aktivieren Sie Deep Learning.
    
        1. Auswählen der *Zeitspalte*: Diese Spalte enthält die zu verwendenden Zeitdaten.

        1. Auswählen des *Vorhersagehorizonts*: Geben Sie an, wie viele Zeiteinheiten (Minuten/Stunden/Tage/Wochen/Monate/Jahre) das Modell die Zukunft vorhersagen können soll. Je weiter das Modell die Zukunft vorhersagen muss, desto ungenauer wird es. [Weitere Informationen zu Vorhersagen und zum Vorhersagehorizont](how-to-auto-train-forecast.md).

1. (Optional:) Anzeigen weiterer Konfigurationseinstellungen: zusätzliche Einstellungen, mit denen Sie den Trainingsauftrag besser steuern können. Andernfalls werden die Standardwerte auf Basis der Experimentauswahl und -daten angewendet. 

    Zusätzliche Konfigurationen|BESCHREIBUNG
    ------|------
    Primary metric (Primäre Metrik)| Die wichtigste Metrik, die für die Bewertung Ihres Modells verwendet wird. [Weitere Informationen zur Modellmetriken](how-to-configure-auto-train.md#primary-metric).
    Explain best model (Bestes Modell erläutern) | Wählen Sie diese Option aus, um die Erläuterungen für das empfohlene beste Modell anzuzeigen oder zu deaktivieren. <br> Diese Funktion ist derzeit nicht für [bestimmte Vorhersagealgorithmen](how-to-machine-learning-interpretability-automl.md#interpretability-during-training-for-the-best-model) verfügbar. 
    Blocked algorithm (Blockierter Algorithmus)| Wählen Sie Algorithmen aus, die Sie aus den Trainingsauftrag ausschließen möchten. <br><br> Das Zulassen von Algorithmen ist nur für [SDK-Experimente](how-to-configure-auto-train.md#supported-models) verfügbar. <br> Weitere Informationen finden Sie auf der Seite zu den [unterstützten Modellen für einzelne Aufgabentypen](https://docs.microsoft.com/python/api/azureml-automl-core/azureml.automl.core.shared.constants.supportedmodels?view=azure-ml-py&preserve-view=true).
    Beendigungskriterium| Wenn eines dieser Kriterien erfüllt ist, wird der Trainingsauftrag beendet. <br> *Training job time (hours)* Trainingsauftragszeit (Stunden): Gibt an, wie lange der Trainingsauftrag ausgeführt werden soll. <br> *Metric score threshold* (Metrischer Bewertungsschwellenwert):  Die Metrikmindestbewertung für alle Pipelines. Auf diese Weise wird sichergestellt, dass Sie nicht mehr Zeit für den Trainingsauftrag aufwenden als nötig, wenn Sie eine definierte Zielmetrik verwenden, die Sie erreichen möchten.
    Überprüfen| Wählen Sie eine der Optionen für Kreuzvalidierung aus, die im Trainingsauftrag verwendet werden soll. <br> [Weitere Informationen zur Kreuzvalidierung](how-to-configure-cross-validation-data-splits.md#prerequisites).<br> <br>Für Vorhersagen wird nur die k-fache Kreuzvalidierung unterstützt.
    Parallelität| *Max concurrent iterations* (Maximale Anzahl gleichzeitiger Iterationen): Die maximale Anzahl von Pipelines (Iterationen), die im Trainingsauftrag getestet werden. Der Auftrag wird nicht häufiger als die angegebene Anzahl von Iterationen ausgeführt.

1. (Optional) Anzeigen von Featurisierungseinstellungen: Wenn Sie im Formular **Additional configuration settings** (Zusätzliche Konfigurationseinstellungen) die Option **Automatische Featurisierung** aktivieren, werden standardmäßige Featurisierungstechniken angewendet. Diese Standardeinstellungen können unter **Featurisierungseinstellungen anzeigen** geändert und entsprechend angepasst werden. Informationen zum Anpassen von Featurisierungen finden Sie [hier](#customize-featurization). 

    ![Azure Machine Learning Studio: Formular für den Aufgabentyp](media/how-to-use-automated-ml-for-ml-models/view-featurization-settings.png)

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

## <a name="customize-featurization"></a>Anpassen der Featurisierung

Im Formular **Featurisierung** können Sie die automatische Featurisierung aktivieren/deaktivieren und die Einstellungen der automatischen Featurisierung für Ihr Experiment anpassen. Informationen zum Öffnen dieses Formulars finden Sie in Schritt 10 des Abschnitts [Erstellen und Ausführen eines Experiments](#create-and-run-experiment). 

In der folgenden Tabelle sind die derzeit in Studio verfügbaren Anpassungen zusammengefasst: 

Column| Anpassung
---|---
Enthalten | Gibt an, welche Spalten in das Training einbezogen werden sollen.
Featuretyp| Dient zum Ändern des Werttyps für die ausgewählte Spalte.
Imputation mit| Dient zum Auswählen des Werts, mit dem fehlende Werte in Ihren Daten imputiert werden sollen.

![Benutzerdefinierte Featurisierung für Azure Machine Learning Studio](media/how-to-use-automated-ml-for-ml-models/custom-featurization.png)

## <a name="run-experiment-and-view-results"></a>Ausführen des Experiments und Anzeigen der Ergebnisse

Wählen Sie **Fertig stellen** aus, um das Experiment auszuführen. Der Vorgang zum Vorbereiten eines Experiments kann bis zu 10 Minuten dauern. Ein Ausführen von Trainingsaufträgen kann für jede Pipeline weitere 2 bis 3 Minuten beanspruchen.

### <a name="view-experiment-details"></a>Anzeigen von Details zum Experiment

Die Anzeige für **Ausführungsdetails** wird mit der Registerkarte **Details** geöffnet. In dieser Anzeige wird eine Zusammenfassung der Experimentausführung einschließlich einer Statusleiste oben neben der Ausführungsnummer angezeigt. 

Die Registerkarte **Modelle** enthält eine Liste der erstellten Modelle, wobei diese nach der Metrikbewertung (Metrikscore) geordnet sind. Standardmäßig steht das Modell, das anhand der ausgewählten Metrik die höchste Bewertung erhält, in der Liste ganz oben. Während der Trainingsauftrag weitere Modelle testet, werden diese zur Liste hinzugefügt. Verwenden Sie diese Liste, um einen schnellen Vergleich der Metriken für die bisher generierten Modelle zu erhalten.

[![Dashboard mit den Ausführungsdetails](media/how-to-use-automated-ml-for-ml-models/run-details.png)](media/how-to-use-automated-ml-for-ml-models/run-details-expanded.png#lightbox)

### <a name="view-training-run-details"></a>Anzeigen der Details der Trainingsausführung

Führen Sie einen Drilldown für eines der abgeschlossenen Modelle aus, um Details zur Trainingsausführung anzuzeigen, z. B. eine Modellzusammenfassung auf der Registerkarte **Modell** oder Leistungsmetrikdiagramme auf der Registerkarte **Metriken**. [Weitere Informationen zu Diagrammen](how-to-understand-automated-ml.md).

[![Details zur Iteration](media/how-to-use-automated-ml-for-ml-models/iteration-details.png)](media/how-to-use-automated-ml-for-ml-models/iteration-details-expanded.png)

## <a name="deploy-your-model"></a>Bereitstellen Ihres Modells

Sobald Ihnen das beste Modell vorliegt, ist es an der Zeit, das Modell als Webdienst bereitzustellen, um Vorhersagen für neue Daten zu erstellen.

Automatisiertes maschinelles Lernen unterstützt Sie dabei, das Modell bereitzustellen, ohne Code zu schreiben:

1. Sie haben einige Optionen für die Bereitstellung. 

    + Option 1: Stellen Sie das beste Modell gemäß den von Ihnen definierten Metrikkriterien bereit. 
        1. Navigieren Sie nach Abschluss des Experiments zur Seite mit der übergeordneten Ausführung, indem Sie oben auf dem Bildschirm **Ausführung 1** auswählen. 
        1.  Wählen Sie das Modell aus, das im Abschnitt **Zusammenfassung des besten Modells** aufgeführt ist. 
        1. Wählen Sie oben links im Fenster **Bereitstellen** aus. 

    + Option 2: Stellen Sie eine bestimmte Modelliteration aus diesem Experiment bereit.
        1. Wählen Sie das gewünschte Modell auf der Registerkarte **Modelle** aus.
        1. Wählen Sie oben links im Fenster **Bereitstellen** aus.

1. Füllen Sie den Bereich **Modell bereitstellen** aus.

    Feld| Wert
    ----|----
    Name| Geben Sie einen eindeutigen Namen für die Bereitstellung ein.
    BESCHREIBUNG| Geben Sie eine Beschreibung ein, um den Zweck dieser Bereitstellung genauer anzugeben.
    Computetyp| Wählen Sie die Art des bereitzustellenden Endpunkts aus: *Azure Kubernetes Service (AKS)* oder *Azure-Containerinstanz (ACI)* .
    Computename| *Nur für AKS:* Wählen Sie den Namen des AKS-Clusters aus, der als Ziel für die Bereitstellung verwendet werden soll.
    Authentifizierung aktivieren | Wählen Sie diese Option aus, um eine token- oder schlüsselbasierte Authentifizierung zu ermöglichen.
    Use custom deployment assets (Benutzerdefinierte Bereitstellungsressourcen verwenden)| Aktivieren Sie dieses Feature, wenn Sie Ihr eigenes Bewertungsskript und Ihre eigene Umgebungsdatei hochladen möchten. Weitere Informationen zu Bewertungsskripts finden Sie [hier](how-to-deploy-and-where.md).

    >[!Important]
    > Ein Dateiname muss weniger als 32 Zeichen haben und muss mit einem alphanumerischen Zeichen beginnen und enden. Dazwischen darf ein Name Bindestriche, Unterstriche, Punkte und alphanumerische Zeichen enthalten. Leerzeichen sind nicht zulässig.

    Das Menü *Erweitert* enthält Standard-Bereitstellungsfeatures wie [Datensammlung](how-to-enable-app-insights.md) und Einstellungen für die Ressourcenauslastung. Wenn Sie diese Standardeinstellungen überschreiben möchten, verwenden Sie dafür dieses Menü.

1. Klicken Sie auf **Bereitstellen**. Die Bereitstellung kann bis zu 20 Minuten dauern.
    Sobald die Bereitstellung beginnt, wird die Registerkarte **Modellzusammenfassung** angezeigt. Der Status der Bereitstellung wird im Abschnitt **Bereitstellungsstatus** angezeigt. 

Nun haben Sie einen einsatzfähigen Webdienst, mit dem Vorhersagen generiert werden können! Sie können die Vorhersagen testen, indem Sie den Dienst über die [in Power BI integrierte Azure Machine Learning-Unterstützung](how-to-consume-web-service.md#consume-the-service-from-power-bi) abfragen.

## <a name="next-steps"></a>Nächste Schritte

* [Informationen zum Nutzen eines Webdiensts](https://docs.microsoft.com/azure/machine-learning/how-to-consume-web-service)
* [Grundlagen von Ergebnissen des automatisierten maschinellen Lernens](how-to-understand-automated-ml.md)
* [Weitere Informationen zu automatisiertem Machine Learning](concept-automated-ml.md) und Azure Machine Learning.
