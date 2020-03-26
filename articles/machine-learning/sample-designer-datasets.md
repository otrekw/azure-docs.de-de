---
title: Verwenden von Beispieldatasets in Azure Machine Learning-Designer
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie mehr über die Beispieldatasets in Azure Machine Learning-Designer.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: likebupt
ms.author: keli19
ms.date: 03/03/2020
ms.openlocfilehash: 943e23aa9d83df33bbe636ce7aee62e6e5a2950e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "79037302"
---
# <a name="use-the-sample-datasets-in-azure-machine-learning-designer-preview"></a>Verwenden von Beispieldatasets in Azure Machine Learning-Designer (Vorschau)

Beim Erstellen einer neuen Pipeline in Azure Machine Learning-Designer (Vorschau) ist eine Reihe von Beispieldatasets standardmäßig enthalten. Diese Beispieldatasets werden von den Beispielpipelines auf der Designerstartseite verwendet. 

Die Beispieldatasets sind unter der Kategorie **Datasets**-**Beispiele** verfügbar. Sie finden diese in der Modulpalette links von der Canvas im Designer. Sie können alle diese Datasets für Ihre eigene Pipeline verwenden, indem Sie sie auf die Canvas ziehen.

## <a name="datasets"></a>Datasets


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

## <a name="next-steps"></a>Nächste Schritte

* Die Grundlagen von Predictive Analytics und maschinellem Lernen werden hier erläutert: [Tutorial: Prognostizieren von Automobilpreisen mit dem Designer](tutorial-designer-automobile-price-train-score.md).

* Erfahren Sie, wie Sie vorhandene [Designer-Beispiele](samples-designer.md) ändern, um sie an Ihre Anforderungen anzupassen.
