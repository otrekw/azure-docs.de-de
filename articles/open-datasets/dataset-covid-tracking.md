---
title: COVID-Nachverfolgungsprojekt
titleSuffix: Azure Open Datasets
description: Hier erfahren Sie, wie das Dataset des COVID-Nachverfolgungsprojekts in Azure Open Datasets verwendet wird.
ms.service: open-datasets
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 04/16/2021
ms.openlocfilehash: 55814a6b1b78673c20447d6129f609058d5c794f
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114453700"
---
# <a name="covid-tracking-project"></a>COVID-Nachverfolgungsprojekt

Das Dataset zum „The COVID Tracking Project“ enthält aktuelle Zahlen zu Tests, bestätigten Fällen, in Krankenhäuser eingewiesenen Patienten und Patientenergebnissen aus allen US-Bundesstaaten und -Gebieten.

Weitere Informationen zu diesem Dataset finden Sie im [GitHub-Projektrepository](https://github.com/COVID19Tracking/covid-tracking-data).

[!INCLUDE [Open Dataset usage notice](../../includes/open-datasets-usage-note.md)]

## <a name="datasets"></a>Datasets

Geänderte Versionen des Datasets sind im CSV-, JSON-, JSONL- und Parquet-Format verfügbar.
- https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/covid_tracking/latest/covid_tracking.csv
- https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/covid_tracking/latest/covid_tracking.json
- https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/covid_tracking/latest/covid_tracking.jsonl
- https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/covid_tracking/latest/covid_tracking.parquet

Allen geänderten Versionen wurden ISO 3166-Teilgebietscodes und Ladezeiten hinzugefügt. Außerdem wurden die Spaltennamen in Kleinbuchstaben umformatiert und mit Unterstrichen versehen.

Rohdaten: https://pandemicdatalake.blob.core.windows.net/public/raw/covid-19/covid_tracking/latest/daily.json

Vorgängerversionen der geänderten Daten und Rohdaten: https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/covid_tracking/

https://pandemicdatalake.blob.core.windows.net/public/raw/covid-19/covid_tracking/

## <a name="data-volume"></a>Datenvolumen
Alle Datasets werden täglich aktualisiert. Nach Stand des 13. Mai 2020 enthalten diese 4.100 Zeilen (CSV: 574 KB, JSON: 1,8 MB, JSONL: 1,8 MB, Parquet: 334 KB).

## <a name="data-source"></a>Datenquellen-

Diese Daten stammen ursprünglich aus dem „COVID Tracking Project“ von The Atlantic. Rohdaten werden mithilfe der Datei [states_daily_4p_et.csv](https://github.com/COVID19Tracking/covid-tracking-data/blob/master/data/states_daily_4pm_et.csv) aus dem GitHub-Repository für die COVID-Nachverfolgung erfasst. Weitere Informationen zu diesem Dataset, einschließlich seiner Ursprünge aus der Projekt-API für die COVID-Nachverfolgung, finden Sie im [GitHub-Projektrepository](https://github.com/COVID19Tracking/covid-tracking-data).

## <a name="data-quality"></a>Datenqualität
Das COVID-Nachverfolgungsprojekt bewertet die Datenqualität für jeden Bundesstaat und bietet weitere Informationen zur Bewertung der Qualität dieser Daten. Weitere Informationen finden Sie auf der [Seite mit den Daten des COVID-Nachverfolgungsprojekts](https://covidtracking.com/data). Die Daten im GitHub-Repository können bis zu eine Stunde älter als die Daten in der API sein. Sie müssen die API verwenden, um auf die aktuellsten Daten zuzugreifen.

## <a name="license-and-use-rights-attribution"></a>Anerkennung der Lizenzbedingungen und Nutzungsrechte

Diese Daten werden gemäß den Bestimmungen der [Apache-Lizenz 2.0](https://github.com/COVID19Tracking/covid-tracking-data/blob/master/LICENSE) lizenziert.

Bei Nutzung der Daten müssen stets vollständige Angaben zu Copyright, Patenten, Marken und Urheberrechtshinweisen gemacht werden.

## <a name="contact"></a>Contact

Wenn Sie Fragen oder Feedback zu diesem Dataset oder anderen Datasets im COVID-19-Data Lake haben, wenden Sie sich an askcovid19dl@microsoft.com.

## <a name="columns"></a>Spalten

| Name                        | Datentyp | Eindeutig | Beispielwerte                                                                   | BESCHREIBUNG                                                                                                                 |
|-----------------------------|-----------|--------|-----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| date                        | date      | 420    | 2020-11-10 2021-01-30                                                             | Datum, für das die Tagesgesamtwerte erfasst wurden                                                                             |
| date_checked                | Zeichenfolge    | 9\.487  | 2020-12-01T00:00:00Z 2020-09-01T00:00:00Z                                         | Als veraltet markiert                                                                                                                  |
| death                       | SMALLINT  | 7\.327  | 2 5                                                                               | Gesamtanzahl der bisher infolge von COVID-19 Verstorbenen                                                        |
| death_increase              | SMALLINT  | 429    | 1 2                                                                               | Als veraltet markiert                                                                                                                  |
| fips                        | SMALLINT  | 56     | 26 55                                                                             | FIPS-Zensuscode des Bundesstaats                                                                                             |
| fips_code                   | Zeichenfolge    | 60     | 53 25                                                                             | FIPS-Zensuscode des Bundesstaats                                                                                             |
| hash                        | Zeichenfolge    | 20.780 | 63df8cccd23a5476bab2d8111b138e4c9becd35e c606cd6990f16086b5382e12d84f6206172d493d | Ein Hash für diesen Datensatz                                                                                                      |
| hospitalized                | INT       | 7\.641  | 89995 4                                                                           | Als veraltet markiert                                                                                                                  |
| hospitalized_cumulative     | INT       | 7\.641  | 89995 4                                                                           | Gesamtanzahl der bisher aufgrund von COVID-19 im Krankenhaus behandelten Personen, einschließlich inzwischen Genesener und Verstorbener |
| hospitalized_currently      | SMALLINT  | 3\.886  | 8 13                                                                              | Anzahl der am betreffenden Tag aufgrund von COVID-19 im Krankenhaus liegenden Personen                                                                      |
| hospitalized_increase       | SMALLINT  | 615    | 1 2                                                                               | Als veraltet markiert                                                                                                                  |
| in_icu_cumulative           | SMALLINT  | 2\.295  | 990 220                                                                           | Gesamtanzahl der bisher aufgrund von COVID-19 auf der Intensivstation behandelten Personen, einschließlich inzwischen Genesener und Verstorbener      |
| in_icu_currently            | SMALLINT  | 1\.643  | 2 8                                                                               | Gesamtanzahl der am betreffenden Tag aufgrund von COVID-19 auf der Intensivstation liegenden Personen                                                                 |
| iso_country                 | Zeichenfolge    | 1      | US                                                                                | Landes- oder Regionscode gemäß ISO 3166                                                                                             |
| iso_subdivision             | Zeichenfolge    | 57     | US-UM US-WA                                                                       | Code der staatlichen Untereinheit gemäß ISO 3166                                                                                                   |
| last_update_et              | timestamp | 9\.487  | 2020-12-01 00:00:00 2020-09-01 00:00:00                                           | Letzte Aktualisierung der Tagesdaten                                                                                        |
| load_time                   | timestamp | 1      | 2021-04-26 00:06:49.883000                                                        | Datum und Uhrzeit des Ladens der Daten aus der Quelle in Azure                                                                  |
| Negativ                    | INT       | 10.864 | 305972 2140                                                                       | Gesamtanzahl der bisher negativ auf COVID-19 getesteten Personen                                                        |
| negative_increase           | INT       | 7\.328  | 6 17                                                                              | Als veraltet markiert                                                                                                                  |
| on_ventilator_cumulative    | SMALLINT  | 677    | 411 412                                                                           | Gesamtanzahl der bisher aufgrund von COVID-19 mit einem Beatmungsgerät beatmeten Personen, einschließlich inzwischen Genesener und Verstorbener    |
| on_ventilator_currently     | SMALLINT  | 837    | 4 10                                                                              | Anzahl der am betreffenden Tag aufgrund von COVID-19 über ein Beatmungsgerät beatmeten Personen                                                               |
| pending                     | SMALLINT  | 944    | 2 17                                                                              | Anzahl der Tests mit ausstehendem Ergebnis                                                                    |
| pos_neg                     | INT       | 18.282 | 2140 2                                                                            | Als veraltet markiert                                                                                                                  |
| Positiv                    | INT       | 16.837 | 2 1                                                                               | Gesamtanzahl der bisher positiv auf COVID-19 getesteten Personen                                                        |
| positive_increase           | SMALLINT  | 4\.754  | 1 2                                                                               | Als veraltet markiert                                                                                                                  |
| recovered                   | INT       | 8\.286  | 29 19                                                                             | Gesamtanzahl der bisher von COVID-19 Genesenen                                                             |
| state                       | Zeichenfolge    | 56     | MI PA                                                                             | Aus zwei Buchstaben bestehender Code für den betreffenden Bundesstaat                                                                                              |
| total                       | INT       | 18.283 | 2140 2                                                                            | Als veraltet markiert                                                                                                                  |
| total_test_results          | INT       | 18.648 | 2140 3                                                                            | Insgesamt vom betreffenden Bundesstaat bereitgestellte Testergebnisse                                                                                    |
| total_test_results_increase | INT       | 13.463 | 1 2                                                                               | Als veraltet markiert                                                                                                                  |

## <a name="preview"></a>Vorschau

| date       | state | Positiv | hospitalized_currently | hospitalized_cumulative | on_ventilator_currently | data_quality_grade | last_update_et        | hash                                     | date_checked          | death | hospitalized | total   | total_test_results | pos_neg | fips | death_increase | hospitalized_increase | negative_increase | positive_increase | total_test_results_increase | fips_code | iso_subdivision | load_time             | iso_country | Negativ | in_icu_cumulative | on_ventilator_cumulative | recovered | in_icu_currently |
|------------|-------|----------|------------------------|-------------------------|-------------------------|--------------------|-----------------------|------------------------------------------|-----------------------|-------|--------------|---------|--------------------|---------|------|----------------|-----------------------|-------------------|-------------------|-----------------------------|-----------|-----------------|-----------------------|-------------|----------|-------------------|--------------------------|-----------|------------------|
| 2021-03-07 | AK    | 56.886    | 33                     | 1\.293                    | 2                       | NULL               | 3/5/2021 3:59:00 AM   | dc4bccd4bb885349d7e94d6fed058e285d4be164 | 3/5/2021 3:59:00 AM   | 305   | 1\.293         | 56.886   | 1\.731.628            | 56.886   | 2    | 0              | 0                     | 0                 | 0                 | 0                           | 2         | US-AK           | 4/26/2021 12:06:49 AM | US          |          |                   |                          |           |                  |
| 2021-03-07 | AL    | 499.819   | 494                    | 45.976                   |                         | ungültig               | 3/7/2021 11:00:00 AM  | 997207b430824ea40b8eb8506c19a93e07bc972e | 3/7/2021 11:00:00 AM  | 10.148 | 45.976        | 2\.431.530 | 2\.323.788            | 2\.431.530 | 1    | -1             | 0                     | 2\.087              | 408               | 2347                        | 1         | US-AL           | 4/26/2021 12:06:49 AM | US          | 1\.931.711  | 2\.676              | 1515                     | 295.690    |                  |
| 2021-03-07 | AR    | 324.818   | 335                    | 14.926                   | 65                      | ungültig               | 3/7/2021 12:00:00 AM  | 50921aeefba3e30d31623aa495b47fb2ecc72fae | 3/7/2021 12:00:00 AM  | 5319  | 14.926        | 2\.805.534 | 2\.736.442            | 2\.805.534 | 5    | 22             | 11                    | 3267              | 165               | 3380                        | 5         | US-AR           | 4/26/2021 12:06:49 AM | US          | 2\.480.716  |                   | 1533                     | 315.517    | 141              |
| 2021-03-07 | AS    | 0        |                        |                         |                         | NULL               | 12/1/2020 12:00:00 AM | 96d23f888c995b9a7f3b4b864de6414f45c728ff | 12/1/2020 12:00:00 AM | 0     |              | 2\.140    | 2\.140               | 2\.140    | 60   | 0              | 0                     | 0                 | 0                 | 0                           | 60        | US-AS           | 4/26/2021 12:06:49 AM | US          | 2\.140     |                   |                          |           |                  |
| 2021-03-07 | RP    | 826.454   | 963                    | 57.907                   | 143                     | ungültig               | 3/7/2021 12:00:00 AM  | 0437a7a96f4471666f775e63e86923eb5cbd8cdf | 3/7/2021 12:00:00 AM  | 16.328 | 57.907        | 3\.899.464 | 7\.908.105            | 3\.899.464 | 4    | 5              | 44                    | 13.678             | 1\.335              | 45.110                       | 4         | US-AZ           | 4/26/2021 12:06:49 AM | US          | 3\.073.010  |                   |                          |           | 273              |
| 2021-03-07 | CA    | 3\.501.394  | 4\.291                   |                         |                         | ungültig               | 3/7/2021 2:59:00 AM   | 63c5c0fd2daef2fb65150e9db486de98ed3f7b72 | 3/7/2021 2:59:00 AM   |       |              | 3\.501.394 | 49.646.014           | 3\.501.394 | 6    | 258            | 0                     | 0                 | 3816              | 133.186                      | 6         | US-CA           | 4/26/2021 12:06:49 AM | US          |          |                   |                          |           | 1159             |
| 2021-03-07 | CO    | 436.602   | 326                    | 23.904                   |                         | ungültig               | 3/7/2021 1:59:00 AM   | 444746cda3a596f183f3fa3269c8cab68704e819 | 3/7/2021 1:59:00 AM   | 5989  | 23.904        | 2\.636.060 | 6\.415.123            | 2\.636.060 | 8    | 3              | 18                    | 0                 | 840               | 38.163                       | 8         | US-CO           | 4/26/2021 12:06:49 AM | US          | 2\.199.458  |                   |                          |           |                  |
| 2021-03-07 | CT    | 285.330   | 428                    | 12.257                   |                         | ungültig               | 3/4/2021 11:59:00 PM  | bcc0f7bc8c2bf77eec31b25f8b59d510f679d3e7 | 3/4/2021 11:59:00 PM  | 7704  | 12.257        | 285.330  | 6\.520.366            | 285.330  | 9    | 0              | 0                     | 0                 | 0                 | 0                           | 9         | US-CT           | 4/26/2021 12:06:49 AM | US          |          |                   |                          |           |                  |
| 2021-03-07 | DC    | 41.419    | 150                    |                         | 16                      | ungültig               | 3/6/2021 12:00:00 AM  | a3aa0d623d538807fb9577ad64354f48cf728cc8 | 3/6/2021 12:00:00 AM  | 1030  |              | 41.419   | 1\.261.363            | 41.419   | 11   | 0              | 0                     | 0                 | 146               | 5\.726                        | 11        | US-DC           | 4/26/2021 12:06:49 AM | US          |          |                   |                          | 29.570     | 38               |
| 2021-03-07 | DE    | 88.354    | 104                    |                         |                         | ungültig               | 3/6/2021 6:00:00 PM   | 059d870e689d5cc19c35f5eb398214d7d9856373 | 3/6/2021 6:00:00 PM   | 1473  |              | 633.424  | 1\.431.942            | 633.424  | 10   | 9              | 0                     | 917               | 215               | 5\.867                        | 10        | US-DE           | 4/26/2021 12:06:49 AM | US          | 545.070   |                   |                          |           | 13               |

## <a name="data-access"></a>Datenzugriff

### <a name="azure-notebooks"></a>Azure Notebooks

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

URLs verschiedener Datasetdateiformate, die in Azure Blob Storage gehostet werden:

CSV: https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/covid_tracking/latest/covid_tracking.csv

JSON: https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/covid_tracking/latest/covid_tracking.json

JSONL: https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/covid_tracking/latest/covid_tracking.jsonl

Parquet: https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/covid_tracking/latest/covid_tracking.parquet

Laden Sie die Datasetdatei mithilfe des integrierten Funktionsdownloads in Pandas über die HTTP-URL herunter. Pandas verfügt über Reader für verschiedene Dateiformate:

https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_parquet.html

https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html

```python
import pandas as pd
import numpy as np
%matplotlib inline
import matplotlib.pyplot as plt

df = pd.read_parquet("https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/covid_tracking/latest/covid_tracking.parquet ")
df.head(10)

df.dtypes

df.groupby('state').first().filter(['date','positive', 'death'])

df.groupby(df.state).agg({'state': 'count','positive_increase': 'sum','death_increase': 'sum'})

df_NY=df[df['state'] == 'NY']
df_NY.plot(kind='line',x='date',y="positive",grid=True)
df_NY.plot(kind='line',x='date',y="positive_increase",grid=True)
df_NY.plot(kind='line',x='date',y="death",grid=True)
df_NY.plot(kind='line',x='date',y="death_increase",grid=True)

df_US=df.groupby(df.date).agg({'positive': 'sum','positive_increase': 'sum','death':'sum','death_increase': 'sum'}).reset_index()

df_US.plot(kind='line',x='date',y="positive",grid=True)
df_US.plot(kind='line',x='date',y="positive_increase",grid=True)
df_US.plot(kind='line',x='date',y="death",grid=True)
df_US.plot(kind='line',x='date',y="death_increase",grid=True)



```

# <a name="pyspark"></a>[pyspark](#tab/pyspark)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

---

### <a name="azure-databricks"></a>Azure Databricks

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

# <a name="pyspark"></a>[pyspark](#tab/pyspark)

```python
# Azure storage access info
blob_account_name = "pandemicdatalake"
blob_container_name = "public"
blob_relative_path = "curated/covid-19/covid_tracking/latest/covid_tracking.parquet"
blob_sas_token = r""

# Allow SPARK to read from Blob remotely
wasbs_path = 'wasbs://%s@%s.blob.core.windows.net/%s' % (blob_container_name, blob_account_name, blob_relative_path)
spark.conf.set(
  'fs.azure.sas.%s.%s.blob.core.windows.net' % (blob_container_name, blob_account_name),
  blob_sas_token)
print('Remote blob path: ' + wasbs_path)

# SPARK read parquet, note that it won't load any data yet by now
df = spark.read.parquet(wasbs_path)
print('Register the DataFrame as a SQL temporary view: source')
df.createOrReplaceTempView('source')

# Display top 10 rows
print('Displaying top 10 rows: ')
display(spark.sql('SELECT * FROM source LIMIT 10'))
```

---

### <a name="azure-synapse"></a>Azure Synapse

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

# <a name="pyspark"></a>[pyspark](#tab/pyspark)

```python
# Azure storage access info
blob_account_name = "pandemicdatalake"
blob_container_name = "public"
blob_relative_path = "curated/covid-19/covid_tracking/latest/covid_tracking.parquet"
blob_sas_token = r""

# Allow SPARK to read from Blob remotely
wasbs_path = 'wasbs://%s@%s.blob.core.windows.net/%s' % (blob_container_name, blob_account_name, blob_relative_path)
spark.conf.set(
  'fs.azure.sas.%s.%s.blob.core.windows.net' % (blob_container_name, blob_account_name),
  blob_sas_token)
print('Remote blob path: ' + wasbs_path)

# SPARK read parquet, note that it won't load any data yet by now
df = spark.read.parquet(wasbs_path)
print('Register the DataFrame as a SQL temporary view: source')
df.createOrReplaceTempView('source')

# Display top 10 rows
print('Displaying top 10 rows: ')
display(spark.sql('SELECT * FROM source LIMIT 10'))
```

---

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich mit den restlichen Datasets im [Open Datasets-Katalog](dataset-catalog.md) vertraut.
