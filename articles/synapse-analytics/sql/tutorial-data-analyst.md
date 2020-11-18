---
title: 'Tutorial: Analysieren von Azure Open Datasets in Azure Synapse Studio (Vorschauversion) mithilfe eines serverlosen SQL-Pools (Vorschauversion)'
description: In diesem Tutorial wird gezeigt, wie Sie ganz einfach eine explorative Datenanalyse durchführen. Dabei werden mithilfe eines serverlosen SQL-Pools (Vorschauversion) verschiedene Azure Open Datasets-Instanzen miteinander kombiniert und die Ergebnisse in Azure Synapse Studio visualisiert.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: tutorial
ms.subservice: sql
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: cec16041edd10a57088df4ae9cfe0587906919eb
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2020
ms.locfileid: "93309293"
---
# <a name="tutorial-use-serverless-sql-pool-to-analyze-azure-open-datasets-and-visualize-the-results-in-azure-synapse-studio"></a>Tutorial: Verwenden eines serverlosen SQL-Pools zum Analysieren von Azure Open Datasets und Visualisieren der Ergebnisse in Azure Synapse Studio

In diesem Tutorial erfahren Sie, wie Sie eine explorative Datenanalyse durchführen, indem Sie mithilfe eines serverlosen SQL-Pools verschiedene Azure Open Datasets-Instanzen miteinander kombinieren und die Ergebnisse anschließend in Azure Synapse Studio visualisieren.

Hierzu analysieren Sie speziell das [Dataset für Taxifahren in New York City (NYC)](https://azure.microsoft.com/services/open-datasets/catalog/nyc-taxi-limousine-commission-yellow-taxi-trip-records/). Dieses enthält Folgendes:

- Datum/Uhrzeit für Abholung und Ankunft
- Start- und Zielort 
- Fahrtentfernungen
- Einzelkosten
- Tarifarten
- Zahlungsarten 
- Vom Fahrer gemeldete Fahrgastzahlen

## <a name="automatic-schema-inference"></a>Automatischer Schemarückschluss

Da die Daten im Parquet-Dateiformat vorliegen, steht der automatische Schemarückschluss zur Verfügung. Die Daten können somit problemlos abgefragt werden, ohne die Datentypen aller Spalten in den Dateien aufzulisten. Außerdem können Sie den Mechanismus für virtuelle Spalten sowie die Dateipfadfunktion verwenden, um einen Teil der Dateien herauszufiltern.

Führen Sie zunächst die folgende Abfrage aus, um sich mit den NYC-Taxidaten vertraut zu machen:

```sql
SELECT TOP 100 * FROM
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/nyctlc/yellow/puYear=*/puMonth=*/*.parquet',
        FORMAT='PARQUET'
    ) AS [nyc]
```

Der folgende Ausschnitt zeigt das Ergebnis für die NYC-Taxidaten:

![Ergebnisausschnitt für die NYC-Taxidaten](./media/tutorial-data-analyst/1.png)

Analog dazu können Sie mithilfe der folgenden Abfrage das Dataset für gesetzliche Feiertage abfragen:

```sql
SELECT TOP 100 * FROM
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/holidaydatacontainer/Processed/*.parquet',
        FORMAT='PARQUET'
    ) AS [holidays]
```

Der folgende Ausschnitt zeigt das Ergebnis für das Dataset mit den gesetzlichen Feiertagen:

![Ergebnisausschnitt für das Dataset mit den gesetzlichen Feiertagen](./media/tutorial-data-analyst/2.png)

Und schließlich können Sie mithilfe der folgenden Abfrage das Dataset mit den Wetterdaten abfragen:

```sql
SELECT
    TOP 100 *
FROM  
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/isdweatherdatacontainer/ISDWeather/year=*/month=*/*.parquet',
        FORMAT='PARQUET'
    ) AS [weather]
```

Der folgende Ausschnitt zeigt das Ergebnis für das Dataset mit den Wetterdaten:

![Ergebnisausschnitt für das Dataset mit den Wetterdaten](./media/tutorial-data-analyst/3.png)

Weitere Informationen zur Bedeutung der einzelnen Spalten finden Sie in den Beschreibungen der Datasets für [NYC-Taxis](https://azure.microsoft.com/services/open-datasets/catalog/nyc-taxi-limousine-commission-yellow-taxi-trip-records/), [gesetzliche Feiertage](https://azure.microsoft.com/services/open-datasets/catalog/public-holidays/) und [Wetterdaten](https://azure.microsoft.com/services/open-datasets/catalog/noaa-integrated-surface-data/).

## <a name="time-series-seasonality-and-outlier-analysis"></a>Zeitreihen, Saisonalität und Ausreißeranalyse

Mit der folgenden Abfrage können Sie ganz einfach die jährliche Anzahl von Taxifahrten zusammenfassen:

```sql
SELECT
    YEAR(tpepPickupDateTime) AS current_year,
    COUNT(*) AS rides_per_year
FROM
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/nyctlc/yellow/puYear=*/puMonth=*/*.parquet',
        FORMAT='PARQUET'
    ) AS [nyc]
WHERE nyc.filepath(1) >= '2009' AND nyc.filepath(1) <= '2019'
GROUP BY YEAR(tpepPickupDateTime)
ORDER BY 1 ASC
```

Der folgende Ausschnitt zeigt das Ergebnis für die jährliche Anzahl von Taxifahrten:

![Ergebnisausschnitt für die jährliche Anzahl von Taxifahrten](./media/tutorial-data-analyst/4.png)

Die Daten können in Synapse Studio visualisiert werden. Wechseln Sie hierzu von der **Tabellenansicht** zur **Diagrammansicht**. Sie können zwischen verschiedenen Diagrammtypen (**Flächendiagramm**, **Balkendiagramm**, **Säulendiagramm**, **Liniendiagramm**, **Kreisdiagramm** oder **Punktdiagramm**) wählen. Erstellen Sie in diesem Fall ein **Säulendiagramm**, und legen Sie die **Kategoriespalte** auf **current_year** fest:

![Säulendiagramm mit Fahrten pro Jahr](./media/tutorial-data-analyst/5.png)

In dieser Visualisierung ist ganz eindeutig ein rückläufiger Trend bei den Fahrten erkennbar. Dieser Rückgang ist vermutlich auf die zunehmende Beliebtheit von Carsharing-Unternehmen zurückzuführen.

> [!NOTE]
> Zum Zeitpunkt der Erstellung dieses Tutorials liegen für 2019 nur unvollständige Daten vor. Dies hat einen signifikanten Rückgang bei der Anzahl von Fahrten für dieses Jahr zur Folge.

Als Nächstes konzentrieren wir uns bei der Analyse auf ein einzelnes Jahr, beispielsweise 2016. Von der folgenden Abfrage wird die tägliche Anzahl von Fahrten in diesem Jahr zurückgegeben:

```sql
SELECT
    CAST([tpepPickupDateTime] AS DATE) AS [current_day],
    COUNT(*) as rides_per_day
FROM
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/nyctlc/yellow/puYear=*/puMonth=*/*.parquet',
        FORMAT='PARQUET'
    ) AS [nyc]
WHERE nyc.filepath(1) = '2016'
GROUP BY CAST([tpepPickupDateTime] AS DATE)
ORDER BY 1 ASC
```

Der folgende Ausschnitt zeigt das Ergebnis für diese Abfrage:

![Ergebnisausschnitt für die tägliche Anzahl von Fahrten im Jahr 2016](./media/tutorial-data-analyst/6.png)

Auch diese Daten können ganz einfach in einem **Säulendiagramm** visualisiert werden, indem die **Kategoriespalte** auf **current_day** und die **Legendenspalte (Reihen)** auf **rides_per_day** festgelegt wird.

![Säulendiagramm mit täglicher Anzahl von Fahrten im Jahr 2016](./media/tutorial-data-analyst/7.png)

Im ausgegebenen Diagramm ist zu sehen, dass es ein wöchentliches Muster gibt, wobei der jeweilige Samstag der Spitzentag ist. In den Sommermonaten werden urlaubsbedingt weniger Taxifahrten durchgeführt. Sie können außerdem einige signifikante Einbrüche bei der Anzahl von Taxifahrten sehen, ohne dass ein klares Muster für den Zeitpunkt und den Grund dieser Rückgänge erkennbar wäre.

Daher untersuchen wir als Nächstes, ob diese Einbrüche mit gesetzlichen Feiertagen zusammenhängen, indem wir das Dataset der NYC-Taxifahrten mit dem Dataset der gesetzlichen Feiertage verknüpfen:

```sql
WITH taxi_rides AS
(
    SELECT
        CAST([tpepPickupDateTime] AS DATE) AS [current_day],
        COUNT(*) as rides_per_day
    FROM  
        OPENROWSET(
            BULK 'https://azureopendatastorage.blob.core.windows.net/nyctlc/yellow/puYear=*/puMonth=*/*.parquet',
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
            BULK 'https://azureopendatastorage.blob.core.windows.net/holidaydatacontainer/Processed/*.parquet',
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

![Ergebnisvisualisierung für die Datasets der NYC-Taxifahrten und der gesetzlichen Feiertage](./media/tutorial-data-analyst/8.png)

Diesmal soll die Anzahl von Taxifahrten während der öffentlichen Feiertage hervorgehoben werden. Hierzu werden **none** für die **Kategoriespalte** sowie **rides_per_day** und **holiday** als **Legendenspalten (Reihe)** ausgewählt.

![Diagramm zur Anzahl von Taxifahrten während gesetzlicher Feiertage](./media/tutorial-data-analyst/9.png)

Wie Sie dem Diagramm entnehmen können, ist die Anzahl von Taxifahrten an gesetzlichen Feiertagen geringer. Es gibt jedoch immer noch einen deutlichen Einbruch am 23. Januar, der dadurch nicht zu erklären ist. Daher fragen wir als Nächstes das Dataset mit den Wetterdaten ab, um das Wetter in NYC an diesem Tag zu überprüfen:

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
        BULK 'https://azureopendatastorage.blob.core.windows.net/isdweatherdatacontainer/ISDWeather/year=*/month=*/*.parquet',
        FORMAT='PARQUET'
    ) AS [weather]
WHERE countryorregion = 'US' AND CAST([datetime] AS DATE) = '2016-01-23' AND stationname = 'JOHN F KENNEDY INTERNATIONAL AIRPORT'
```

![Ergebnisvisualisierung für das Dataset mit den Wetterdaten](./media/tutorial-data-analyst/10.png)

Die Ergebnisse der Abfrage zeigen, dass der Einbruch bei der Anzahl von Taxifahren auf Folgendes zurückzuführen ist:

- An diesem Tag gab es in NYC einen Schneesturm mit starkem Schneefall (~ 30 cm).
- Es war kalt (Temperatur unter Null Grad Celsius).
- Es war windig (~ 10 m/s).

In diesem Tutorial haben Sie gelernt, wie Datenanalysten schnell eine explorative Datenanalyse durchführen, mithilfe eines serverlosen SQL-Pools verschiedene Datasets miteinander kombinieren und die Ergebnisse mithilfe von Azure Synapse Studio visualisieren können.

## <a name="next-steps"></a>Nächste Schritte

Im Tutorial [Verwenden eines serverlosen SQL-Pools mit Power BI Desktop und Erstellen eines Berichts](tutorial-connect-power-bi-desktop.md) erfahren Sie, wie Sie einen serverlosen SQL-Pool mit Power BI Desktop verbinden und Berichte erstellen.
 
