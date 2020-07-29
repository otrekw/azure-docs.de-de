---
title: Beispiele für Pipelines und Datasets für den Designer (Vorschau)
titleSuffix: Azure Machine Learning
description: Beschleunigen Sie den Start Ihrer Machine Learning-Pipelines mithilfe von Beispielen im Azure Machine Learning-Designer.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 03/29/2020
ms.custom: designer
ms.openlocfilehash: c1d927583ecf0ac5684c607b7d203c3224fe87b1
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87318826"
---
# <a name="example-pipelines--datasets-for-azure-machine-learning-designer-preview"></a>Beispiele für Pipelines und Datasets für Azure Machine Learning-Designer (Vorschau)

Verwenden Sie die integrierten Beispiele in Azure Machine Learning-Designer, um die Erstellung eigener Machine Learning-Pipelines zu beschleunigen. Das [GitHub-Repository](https://github.com/Azure/MachineLearningDesigner) des Azure Machine Learning-Designers enthält eine ausführliche Dokumentation mit grundlegenden Informationen zu einigen gängigen Machine Learning-Szenarien.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://aka.ms/AMLFree) erstellen.
* Ein Azure Machine Learning-Arbeitsbereich mit der Enterprise-SKU.

[!INCLUDE [machine-learning-missing-ui](../../includes/machine-learning-missing-ui.md)]

## <a name="use-sample-pipelines"></a>Verwenden von Beispielpipelines

Der Designer speichert eine Kopie der Beispielpipelines in Ihrem Studio-Arbeitsbereich. Sie können die Pipeline an Ihre Anforderungen anpassen und als eigene Pipeline speichern. Verwenden Sie sie als Ausgangspunkt, um Ihre Projekte zu beschleunigen.

So verwenden Sie ein Beispiel im Designer:

1. Melden Sie sich bei <a href="https://ml.azure.com?tabs=jre" target="_blank">ml.azure.com</a> an, und wählen Sie den gewünschten Arbeitsbereich aus.

1. Wählen Sie **Designer** aus.

1. Wählen Sie im Abschnitt **Neue Pipeline** eine Beispielpipeline aus.

    Wählen Sie **Mehr Stichproben anzeigen** aus, um eine vollständige Liste mit Beispielen anzuzeigen.

1. Zum Ausführen einer Pipeline müssen Sie zuerst das Standardcomputeziel für die Pipelineausführung festlegen.

   1. Wählen Sie rechts neben der Canvas im Bereich **Einstellungen** die Option **Computeziel auswählen** aus.

   1. Wählen Sie im angezeigten Dialogfeld ein vorhandenes Computeziel aus, oder erstellen Sie ein neues. Wählen Sie **Speichern** aus.

   1. Wählen Sie oben auf der Canvas **Übermitteln** aus, um eine Pipelineausführung zu übermitteln.

   Abhängig von der Beispielpipeline und den Computeeinstellungen kann die Ausführung einige Zeit in Anspruch nehmen. In den Standardcomputeeinstellungen ist eine minimale Knotengröße von 0 festgelegt. Das bedeutet, dass der Designer Ressourcen nach dem Leerlauf zuordnen muss. Wiederholte Pipelineausführungen werden schneller abgeschlossen, da die Computeressourcen bereits zugeordnet sind. Außerdem verwendet der Designer für jedes Modul zwischengespeicherte Ergebnisse, um die Effizienz weiter zu steigern.


1. Nach Abschluss der Pipelineausführung können Sie die Pipeline überprüfen und die Ausgabe der einzelnen Module anzeigen, um weitere Informationen zu erhalten. Führen Sie die folgenden Schritte aus, um die Modulausgaben anzuzeigen:

   1. Wählen Sie ein Modul auf der Canvas aus.

   1. Wählen Sie rechts neben der Canvas im Bereich mit den Moduldetails die Option **Ausgaben und Protokolle** aus. Wählen Sie das Diagrammsymbol ![Visualisierungssymbol](./media/tutorial-designer-automobile-price-train-score/visualize-icon.png) aus, um die Ergebnisse der einzelnen Module anzuzeigen. 

   Verwenden Sie die Beispiele als Ausgangspunkt für einige der gängigsten Machine Learning-Szenarien.

## <a name="regression"></a>Regression

Durchsuchen Sie diese integrierten Regressionsbeispiele.

| Beispieltitel | BESCHREIBUNG | 
| --- | --- |
| [Sample 1: Regression - Automobile Price Prediction (Basic)](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-regression-automobile-price-basic.md) (Beispiel 1: Regression: Automobilpreisvorhersage (Standard)) | Dient zum Vorhersagen von Automobilpreisen mittels linearer Regression. |
| [Sample 2: Regression - Automobile Price Prediction (Advanced)](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-regression-automobile-price-compare-algorithms.md) (Beispiel 2: Regression: Automobilpreisvorhersage (Erweitert)) | Dient zum Vorhersagen von Automobilpreisen mittels Entscheidungsstruktur und Boosted Decision Tree-Regressoren. Vergleichen Sie die Modelle, um den besten Algorithmus zu ermitteln.

## <a name="classification"></a>Klassifizierung

Durchsuchen Sie diese integrierten Klassifizierungsbeispiele. Anstelle der Dokumentationslinks können Sie auch die Beispiele öffnen und sich die Modulkommentare ansehen, um mehr über die Beispiele zu erfahren.

| Beispieltitel | BESCHREIBUNG | 
| --- | --- |
| [Sample 3: Binary Classification with Feature Selection - Income Prediction](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-classification-predict-income.md) (Beispiel 3: Binäre Klassifizierung mit Featureauswahl: Vorhersage des Einkommens) | Dient zum Vorhersagen des Einkommens (hoch oder gering) mittels Two-Class Boosted Decision Tree. Für die Featureauswahl wird die Pearson-Korrelation verwendet.
| [Sample 4: Binary Classification with custom Python script - Credit Risk Prediction](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-classification-credit-risk-cost-sensitive.md) (Beispiel 4: Binäre Klassifizierung mit benutzerdefiniertem Python-Skript: Vorhersage des Kreditrisikos) | Dient zum Klassifizieren des Risikos von Kreditanträgen (hoch oder gering). Zur Gewichtung Ihrer Daten wird das Modul „Python-Skript ausführen“ verwendet.
| [Sample 5: Binary Classification - Customer Relationship Prediction](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-classification-churn.md) (Beispiel 5: Binäre Klassifizierung: Vorhersage von Kundenbeziehungen) | Dient zum Vorhersagen der Kundenabwanderung mittels Two-Class Boosted Decision Tree. Für das Sampling verzerrter Daten wird SMOTE verwendet.
| [Sample 7: Text Classification - Wikipedia SP 500 Dataset](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-text-classification.md) (Beispiel 7: Textklassifizierung: Wikipedia SP 500-Dataset) | Dient zum Klassifizieren von Unternehmenstypen aus Wikipedia-Artikeln mittels logistischer Regression mit mehreren Klassen. |
| Beispiel 12: Klassifizierung mit mehreren Klassen: Buchstabenerkennung | Dient zum Erstellen eines Ensembles von binären Klassifizierern für die Klassifizierung geschriebener Buchstaben. |

## <a name="recommender"></a>Empfehlung (Recommender)

Durchsuchen Sie diese integrierten Empfehlungsbeispiele. Anstelle der Dokumentationslinks können Sie auch die Beispiele öffnen und sich die Modulkommentare ansehen, um mehr über die Beispiele zu erfahren.

| Beispieltitel | BESCHREIBUNG | 
| --- | --- |
| Beispiel 10: Empfehlung: Filmbewertungstweets | Dient zum Erstellen einer Filmempfehlungsengine auf der Grundlage von Filmtiteln und -bewertungen. |

## <a name="utility"></a>Hilfsprogramm

Hier finden Sie weitere Informationen zu den Beispielen, mit denen ML-Hilfsprogramme und -Features veranschaulicht werden. Anstelle der Dokumentationslinks können Sie auch die Beispiele öffnen und sich die Modulkommentare ansehen, um mehr über die Beispiele zu erfahren.

| Beispieltitel | BESCHREIBUNG | 
| --- | --- |
| [Sample 6: Use custom R script - Flight Delay Prediction](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-classification-flight-delay.md) (Beispiel 6: Verwenden eines benutzerdefinierten R-Skripts: Vorhersage von Flugverspätungen) |
| Beispiel 8: Kreuzvalidierung für binäre Klassifizierung: Vorhersage des Einkommens von Erwachsenen | Dient zum Erstellen eines binären Klassifizierers für das Einkommen von Erwachsenen mittels Kreuzvalidierung.
| Beispiel 9: Permutation Feature Importance (PFI) | Dient zum Berechnen von Wichtigkeitsbewertungen für das Testdataset mittels Permutation Feature Importance. 
| Beispiel 11: Optimierungsparameter für binäre Klassifizierung: Vorhersage des Einkommens von Erwachsenen | Dient zum Ermitteln optimaler Hyperparameter für die Erstellung eines binären Klassifizierers unter Verwendung von „Tune Model Hyperparameters“. |

## <a name="datasets"></a>Datasets

Beim Erstellen einer neuen Pipeline in Azure Machine Learning-Designer ist eine Reihe von Beispieldatasets standardmäßig enthalten. Diese Beispieldatasets werden von den Beispielpipelines auf der Designerstartseite verwendet. 

Die Beispieldatasets sind unter der Kategorie **Datasets**-**Beispiele** verfügbar. Sie finden diese in der Modulpalette links von der Canvas im Designer. Sie können alle diese Datasets für Ihre eigene Pipeline verwenden, indem Sie sie auf die Canvas ziehen.

| DataSetName&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| Datasetbeschreibung |
|-------------|:--------------------|
| Dataset "Adult Census Income Binary Classification" | Eine Teilmenge der Volkszählungsdatenbank von 1994, die arbeitende Erwachsene (älter als 16 Jahre) mit einem bereinigten Einkommensindex über 100 verwendet.<br/>**Verwendung**: Klassifizierung von Personen mithilfe von demografischen Daten für die Vorhersage, ob eine Person mehr als 50.000 pro Jahr verdient.<br/> **Zugrunde liegende Untersuchungen:** Kohavi, R., Becker, B., (1996). [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml). Irvine, CA: University of California, School of Information and Computer Science|
|Automobile price data (Raw)|Informationen zu Automobilen nach Marke und Modell, einschließlich Preis und Merkmalen wie Zylinderanzahl und Verbrauch sowie einer Risikoeinstufung der Versicherung.<br/> Der Risikobewertung ist anfänglich mit dem Fahrzeugpreis verknüpft. Sie wird dann in einem Prozess, den Versicherungsfachleute als „Symbolisierung“ bezeichnen, an das tatsächliche Risiko angepasst. Der Wert +3 weist auf ein Fahrzeug mit hohem Risiko hin, während der Wert -3 auf ein voraussichtlich sicheres Fahrzeug hinweist.<br/>**Verwendung:** </b> Vorhersage der Risikoeinstufung nach Merkmalen unter Verwendung der Regression oder multivariaten Klassifizierung.<br/>**Zugrunde liegende Untersuchungen:** </b> Schlimmer, J.C. (1987). [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml). Irvine, CA: University of California, School of Information and Computer Science |
| CRM Appetency Bezeichnungen freigegeben |Beschriftungen vom KDD Cup 2009 (Kundenbeziehungsprognose, [orange_small_train_appetency.labels](http://www.sigkdd.org/site/2009/files/orange_small_train_appetency.labels)).|
|CRM Codeänderung Bezeichnungen freigegeben|Beschriftungen vom KDD Cup 2009 (Kundenbeziehungsprognose, [orange_small_train_churn.labels](http://www.sigkdd.org/site/2009/files/orange_small_train_churn.labels)).|
|CRM-Dataset gemeinsam genutzt | Diese Daten stammen vom KDD Cup 2009, Kundenbeziehungsprognose ([orange_small_train.data.zip](http://www.sigkdd.org/site/2009/files/orange_small_train.data.zip)). <br/>Das DataSet enthält 50.000 Kunden des französischen Telekommunikationsunternehmens Orange. Jeder Kunde verfügt über 230 anonymisierte Merkmale, von denen 190 numerisch und 40 kategorisch sortiert sind. Die Merkmale sind sehr karg. |
|CRM Upselling Bezeichnungen freigegeben|Beschriftungen vom KDD Cup 2009 (Kundenbeziehungsprognose, [orange_large_train_upselling.labels](http://www.sigkdd.org/site/2009/files/orange_large_train_upselling.labels)|
|Flugverspätungsdaten|Pünktlichkeitsratendaten zu Passagierflügen aus der TranStats-Datensammlung des US-Verkehrsministeriums ([On-Time](https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time)).<br/>Das DataSet deckt den Zeitraum April bis Oktober 2013 ab. Das Dataset wurde vor dem Hochladen im Designer wie folgt verarbeitet: <br/>-    Es wurde gefiltert, damit nur die 70 verkehrsreichsten Flughäfen in Kontinental-USA enthalten waren. <br/>- Ausgefallene Flüge wurden als um mehr als 15 Minuten verspätet gekennzeichnet. <br/>- Umgeleitete Flüge wurden herausgefiltert. <br/>- Folgende Spalten wurden ausgewählt: Year, Month, DayofMonth, DayOfWeek, Carrier, OriginAirportID, DestAirportID, CRSDepTime, DepDelay, DepDel15, CRSArrTime, ArrDelay, ArrDel15, Canceled|
|Deutsche Kreditkarte UCI-Dataset|Das DataSet „UCI Statlog“ (deutsche Kreditkarte) ([Statlog+German+Credit+Data](https://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data))), das die Datei „german.data“ verwendet.<br/>Dieses DataSet klassifiziert Personen anhand verschiedener Attribute in hohes und niedriges Kreditrisiko. Jedes Beispiel stellt eine Person dar. Insgesamt existieren 20 numerische und kategorische Merkmale sowie eine binäre Beschriftung (der Wert für das Kreditrisiko). Hohe Kreditrisiken tragen die Beschriftung = 2, während geringe Kreditrisiken die Beschriftung = 1 tragen. Die Belastung für die falsche Klassifizierung eines Beispiels mit geringem Risiko beträgt 1, während die Belastung für die falsche Klassifizierung eines Beispiels mit hohem Risiko 5 beträgt.|
|IMDB-Filmtitel|Das Dataset enthält Informationen über Filme, die in Tweets auf Twitter bewertet wurden: Film-ID in der IMDB, Filmname, Genre und Produktionsjahr. Das DataSet enthält 17.000 Filme. Das Dataset wurde im Dokument „S. Dooms, T. De Pessemier und L. Martens. MovieTweetings: a Movie Rating Dataset Collected From Twitter. Workshop on Crowdsourcing and Human Computation for Recommender Systems, CrowdRec at RecSys 2013“ eingeführt.|
|Filmbewertungen|Das Dataset ist eine erweiterte Version des Films Tweetings Datasets. Das Dataset enthält 170K Bewertungen für Filme, extrahiert aus gut strukturierten Tweets auf Twitter. Jede Instanz stellt einen Tweet dar und ist ein Tupel: Benutzer-ID, IMDB Film-ID, Bewertung, Timestamp, Anzahl von Favoriten für diesen Tweet und Anzahl der Retweets dieses Tweets. Das Dataset wurde von A. Said, S. Dooms, B. Loni und D. Tikk für Recommender Systems Challenge 2014 zur Verfügung gestellt.|
|Wetter-Dataset|Stündliche flächenbasierte Wetterbeobachtungen aus NOAA ([zusammengeführte Daten von 201304 bis 201310](https://az754797.vo.msecnd.net/data/WeatherDataset.csv)).<br/>Die Wetterdaten umfassen Beobachtungen der Wetterstationen von Flughäfen für den Zeitraum April bis Oktober 2013. Das Dataset wurde vor dem Hochladen im Designer wie folgt verarbeitet:    <br/> -    Die IDs der Wetterstationen wurden den entsprechenden Flughafen-IDs zugeordnet.    <br/> -    Wetterstationen, die nicht zu den 70 verkehrsreichsten Flughäfen gehören, wurden herausgefiltert    <br/> -    Die Datumsspalte wurde in separate Jahres-, Monats- und Tagesspalten aufgeteilt.    <br/> - Folgende Spalten wurden ausgewählt: AirportID, Year, Month, Day, Time, TimeZone, SkyCondition, Visibility, WeatherType, DryBulbFarenheit, DryBulbCelsius, WetBulbFarenheit, WetBulbCelsius, DewPointFarenheit, DewPointCelsius, RelativeHumidity, WindSpeed, WindDirection, ValueForWindCharacter, StationPressure, PressureTendency, PressureChange, SeaLevelPressure, RecordType, HourlyPrecip, Altimeter|
|Wikipedia SP 500 Dataset|Die Daten stammen aus Wikipedia (https://www.wikipedia.org/) und basieren auf Artikeln der einzelnen S&P-500-Unternehmen, die als XML-Daten gespeichert wurden.    <br/>Das Dataset wurde vor dem Hochladen im Designer wie folgt verarbeitet:    <br/> - Extrahieren der Textinhalte für die einzelnen Unternehmen    <br/> -    Entfernen der Wiki-Formatierung    <br/> -    Entfernen aller nicht alphanumerischen Zeichen    <br/> -    Konvertieren sämtlicher Texte in Kleinbuchstaben    <br/> -    Bekannte Firmenkategorien wurden hinzugefügt.    <br/>Beachten Sie, dass für einige Unternehmen keine Artikel gefunden werden konnten, daher ist die Anzahl der Datensätze kleiner als 500.|


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen von Predictive Analytics und maschinellem Lernen werden hier erläutert: [Tutorial: Prognostizieren von Automobilpreisen mit dem Designer](tutorial-designer-automobile-price-train-score.md).

