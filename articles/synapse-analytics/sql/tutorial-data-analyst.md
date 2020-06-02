---
title: 'Tutorial für Datenanalysten: Analysieren von Azure Open Datasets in Azure Synapse Studio (Vorschauversion) mithilfe von SQL On-Demand (Vorschauversion)'
description: In diesem Tutorial erfahren Sie, wie Sie ganz einfach eine explorative Datenanalyse durchführen. Dabei werden mithilfe von SQL On-Demand (Vorschauversion) verschiedene Azure Open Datasets miteinander kombiniert und die Ergebnisse in Azure Synapse Studio visualisiert.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: tutorial
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: b2fe4dea27564b96c5ef1734dc16ca4525011d17
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83745642"
---
# <a name="use-sql-on-demand-preview-to-analyze-azure-open-datasets-and-visualize-the-results-in-azure-synapse-studio-preview"></a>Analysieren von Azure Open Datasets und Visualisieren der Ergebnisse in Azure Synapse Studio (Vorschauversion) mithilfe von SQL On-Demand (Vorschauversion)

In diesem Tutorial erfahren Sie, wie Sie eine explorative Datenanalyse durchführen, indem Sie mithilfe von SQL On-Demand verschiedene Azure Open Datasets miteinander kombinieren und die Ergebnisse anschließend in Azure Synapse Studio visualisieren.

Hierzu analysieren Sie speziell das [Dataset für Taxifahren in New York City (NYC)](https://azure.microsoft.com/services/open-datasets/catalog/nyc-taxi-limousine-commission-yellow-taxi-trip-records/). Dieses enthält Felder mit Datum/Uhrzeit für Abholung und Ankunft, Start- und Zielort, Fahrtentfernungen, Einzelkosten, Tarifarten, Zahlungsarten und vom Fahrer gemeldeten Fahrgastzahlen.

Der Schwerpunkt der Analyse liegt auf der Ermittlung von Trends bei der Veränderung der Anzahl von Taxifahrten im Zeitverlauf. Zum besseren Verständnis der Ausreißer bei der Anzahl von Taxifahren werden außerdem noch zwei andere Azure Open Datasets ([Gesetzliche Feiertage](https://azure.microsoft.com/services/open-datasets/catalog/public-holidays/) und [Wetterdaten](https://azure.microsoft.com/services/open-datasets/catalog/noaa-integrated-surface-data/)) analysiert.

## <a name="create-data-source"></a>Erstellen der Datenquelle

Das Datenquellenobjekt wird zum Verweisen auf das Azure-Speicherkonto verwendet, in dem Sie Daten analysieren müssen. Bei öffentlich verfügbarem Speicher werden keine Anmeldeinformationen für den Zugriff darauf benötigt.

```sql
-- There is no credential in data surce. We are using public storage account which doesn't need a credential.
CREATE EXTERNAL DATA SOURCE AzureOpenData
WITH ( LOCATION = 'https://azureopendatastorage.blob.core.windows.net/')
```

## <a name="automatic-schema-inference"></a>Automatischer Schemarückschluss

Da die Daten im Parquet-Dateiformat vorliegen, steht der automatische Schemarückschluss zur Verfügung, sodass Daten problemlos abgefragt werden können, ohne die Datentypen aller Spalten in den Dateien auflisten zu müssen. Darüber hinaus können Sie den Mechanismus für virtuelle Spalten sowie die Dateipfadfunktion verwenden, um einen Teil der Dateien herauszufiltern.

Führen Sie zunächst die folgende Abfrage aus, um sich mit den NYC-Taxidaten vertraut zu machen:

```sql
SELECT TOP 100 * FROM
    OPENROWSET(
        BULK 'nyctlc/yellow/puYear=*/puMonth=*/*.parquet',
        DATA_SOURCE = 'AzureOpenData',
        FORMAT='PARQUET'
    ) AS [nyc]
```

Hier sehen Sie den Ergebnisausschnitt für die NYC-Taxidaten:

![Ergebnisausschnitt](./media/tutorial-data-analyst/1.png)

Analog dazu können Sie mithilfe der folgenden Abfrage das Dataset für gesetzliche Feiertage abfragen:

```sql
SELECT TOP 100 * FROM
    OPENROWSET(
        BULK 'holidaydatacontainer/Processed/*.parquet',
        DATA_SOURCE = 'AzureOpenData',
        FORMAT='PARQUET'
    ) AS [holidays]
```

Hier sehen Sie den Ergebnisausschnitt für das Dataset mit den gesetzlichen Feiertagen:

![Ergebnisausschnitt 2](./media/tutorial-data-analyst/2.png)

Und schließlich können Sie mithilfe der folgenden Abfrage das Dataset mit den Wetterdaten abfragen:

```sql
SELECT
    TOP 100 *
FROM  
    OPENROWSET(
        BULK 'isdweatherdatacontainer/ISDWeather/year=*/month=*/*.parquet',
        DATA_SOURCE = 'AzureOpenData',
        FORMAT='PARQUET'
    ) AS [weather]
```

Hier sehen Sie den Ergebnisausschnitt für das Dataset mit den Wetterdaten:

![Ergebnisausschnitt 3](./media/tutorial-data-analyst/3.png)

Weitere Informationen zur Bedeutung der einzelnen Spalten finden Sie in den Beschreibungen der Datasets für [NYC-Taxis](https://azure.microsoft.com/services/open-datasets/catalog/nyc-taxi-limousine-commission-yellow-taxi-trip-records/), [gesetzliche Feiertage](https://azure.microsoft.com/services/open-datasets/catalog/public-holidays/) und [Wetterdaten](https://azure.microsoft.com/services/open-datasets/catalog/noaa-integrated-surface-data/).

## <a name="time-series-seasonality-and-outlier-analysis"></a>Zeitreihen, Saisonalität und Ausreißeranalyse

Mit der folgenden Abfrage können Sie ganz einfach die jährliche Anzahl von Taxifahrten zusammenfassen:

```sql
SELECT
    YEAR(tpepPickupDateTime) AS current_year,
    COUNT(*) AS rides_per_year
FROM
    OPENROWSET(
        BULK 'nyctlc/yellow/puYear=*/puMonth=*/*.parquet',
        DATA_SOURCE = 'AzureOpenData',
        FORMAT='PARQUET'
    ) AS [nyc]
WHERE nyc.filepath(1) >= '2009' AND nyc.filepath(1) <= '2019'
GROUP BY YEAR(tpepPickupDateTime)
ORDER BY 1 ASC
```

Hier sehen Sie den Ergebnisausschnitt für die jährliche Anzahl von Taxifahrten:

![Ergebnisausschnitt 4](./media/tutorial-data-analyst/4.png)

Die Daten können in Synapse Studio visualisiert werden. Wechseln Sie hierzu von der Tabellen- zur Diagrammansicht. Sie können zwischen verschiedenen Diagrammtypen (Flächen-, Balken-, Säulen-, Linien-, Kreis- oder Punktdiagramm) wählen. In diesem Fall wird ein Säulendiagramm mit der Kategoriespalte „current_year“ verwendet:

![Ergebnisvisualisierung 5](./media/tutorial-data-analyst/5.png)

In dieser Visualisierung ist ganz eindeutig ein rückläufiger Trend bei den Fahrten erkennbar – vermutlich bedingt durch die zunehmende Beliebtheit von Carsharing-Unternehmen.

> [!NOTE]
> Zum Zeitpunkt der Erstellung dieses Tutorials liegen für 2019 nur unvollständige Daten vor, was einen signifikanten Rückgang bei der Anzahl von Fahrten für dieses Jahr zur Folge hat.

Als Nächstes konzentrieren wir uns bei der Analyse auf ein einzelnes Jahr, beispielsweise 2016. Mit der folgenden Abfrage wird die tägliche Anzahl von Fahrten in diesem Jahr zurückgegeben:

```sql
SELECT
    CAST([tpepPickupDateTime] AS DATE) AS [current_day],
    COUNT(*) as rides_per_day
FROM
    OPENROWSET(
        BULK 'nyctlc/yellow/puYear=*/puMonth=*/*.parquet',
        DATA_SOURCE = 'AzureOpenData',
        FORMAT='PARQUET'
    ) AS [nyc]
WHERE nyc.filepath(1) = '2016'
GROUP BY CAST([tpepPickupDateTime] AS DATE)
ORDER BY 1 ASC
```

Hier sehen Sie den Ergebnisausschnitt für diese Abfrage:

![Ergebnisausschnitt 6](./media/tutorial-data-analyst/6.png)

Auch diese Daten können ganz einfach in einem Säulendiagramm mit der Kategoriespalte „current_day“ und der Legendenspalte (Reihen) „rides_per_day“ visualisiert werden.

![Ergebnisvisualisierung 7](./media/tutorial-data-analyst/7.png)

Der Plot zeigt ein wöchentliches Muster mit Samstag als Spitze. In den Sommermonaten finden urlaubsbedingt weniger Taxifahrten statt. Es gibt jedoch auch einige signifikante Einbrüche bei der Anzahl von Taxifahrten, ohne dass ein klares Muster für den Zeitpunkt und den Grund dieser Rückgänge erkennbar wäre.

Daher wird als Nächstes untersucht, ob diese Einbrüche möglicherweise mit gesetzlichen Feiertagen zusammenhängen. Hierzu werden die NYC-Taxifahrten mit dem Dataset für gesetzliche Feiertage verknüpft:

```sql
WITH taxi_rides AS
(
    SELECT
        CAST([tpepPickupDateTime] AS DATE) AS [current_day],
        COUNT(*) as rides_per_day
    FROM  
        OPENROWSET(
            BULK 'nyctlc/yellow/puYear=*/puMonth=*/*.parquet',
            DATA_SOURCE = 'AzureOpenData',
            FORMAT='PARQUET'
        ) AS [nyc]
    WHERE nyc.filepath(1) = '2016'
    GROUP BY CAST([tpepPickupDateTime] AS DATE)
),
public_holidays AS
(
    SELECT
        holidayname as holiday,
        date
    FROM
        OPENROWSET(
            BULK 'holidaydatacontainer/Processed/*.parquet',
            DATA_SOURCE = 'AzureOpenData',
            FORMAT='PARQUET'
        ) AS [holidays]
    WHERE countryorregion = 'United States' AND YEAR(date) = 2016
)
SELECT
*
FROM taxi_rides t
LEFT OUTER JOIN public_holidays p on t.current_day = p.date
ORDER BY current_day ASC
```

![Ergebnisvisualisierung 8](./media/tutorial-data-analyst/8.png)

Diesmal soll die Anzahl von Taxifahrten während der öffentlichen Feiertage hervorgehoben werden. Hierzu wird für die Kategoriespalte „None“ und für die Legendenspalten (Reihe) „rides_per_day“ und „holiday“ ausgewählt.

![Ergebnisvisualisierung 9](./media/tutorial-data-analyst/9.png)

Der Plot zeigt deutlich, dass die Anzahl von Taxifahrten an gesetzlichen Feiertagen niedriger ist. Es gibt jedoch immer noch einen deutlichen Einbruch am 23. Januar, der dadurch nicht zu erklären ist. Daher fragen wir als Nächstes das Dataset mit den Wetterdaten ab, um das Wetter in NYC an diesem Tag zu überprüfen:

```sql
SELECT
    AVG(windspeed) AS avg_windspeed,
    MIN(windspeed) AS min_windspeed,
    MAX(windspeed) AS max_windspeed,
    AVG(temperature) AS avg_temperature,
    MIN(temperature) AS min_temperature,
    MAX(temperature) AS max_temperature,
    AVG(sealvlpressure) AS avg_sealvlpressure,
    MIN(sealvlpressure) AS min_sealvlpressure,
    MAX(sealvlpressure) AS max_sealvlpressure,
    AVG(precipdepth) AS avg_precipdepth,
    MIN(precipdepth) AS min_precipdepth,
    MAX(precipdepth) AS max_precipdepth,
    AVG(snowdepth) AS avg_snowdepth,
    MIN(snowdepth) AS min_snowdepth,
    MAX(snowdepth) AS max_snowdepth
FROM
    OPENROWSET(
        BULK 'isdweatherdatacontainer/ISDWeather/year=*/month=*/*.parquet',
        DATA_SOURCE = 'AzureOpenData',
        FORMAT='PARQUET'
    ) AS [weather]
WHERE countryorregion = 'US' AND CAST([datetime] AS DATE) = '2016-01-23' AND stationname = 'JOHN F KENNEDY INTERNATIONAL AIRPORT'
```

![Ergebnisvisualisierung 10](./media/tutorial-data-analyst/10.png)

Die Ergebnisse der Abfrage zeigen, dass der Einbruch bei der Anzahl von Taxifahren auf Folgendes zurückzuführen ist:

- Schneesturm an diesem Tag in NYC aufgrund von starkem Schneefall (~ 30 cm)
- Kälte (Temperatur unter Null Grad Celsius)
- Wind (~ 10 m/s)

In diesem Tutorial haben Sie gelernt, wie Datenanalysten schnell eine explorative Datenanalyse durchführen, mithilfe von SQL On-Demand verschiedene Datasets miteinander kombinieren und die Ergebnisse mithilfe von Azure Synapse Studio visualisieren können.

## <a name="next-steps"></a>Nächste Schritte

Im Artikel [Tutorial: Verbinden von SQL On-Demand mit Power BI Desktop und Erstellen eines Berichts](tutorial-connect-power-bi-desktop.md) erfahren Sie, wie Sie SQL On-Demand mit Power BI Desktop verbinden und Berichte erstellen.
 
