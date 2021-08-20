---
title: Dataset für „NYC Taxi and Limousine – Yellow“
titleSuffix: Azure Open Datasets
description: Erfahren Sie, wie Sie das Dataset für „NYC Taxi and Limousine – Yellow“ in Azure Open Datasets verwenden.
ms.service: open-datasets
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 04/16/2021
ms.openlocfilehash: 5bf2a3b363c7d8a1cd0b10b1c958c4cfbb567deb
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112038923"
---
# <a name="nyc-taxi--limousine-commission---yellow-taxi-trip-records"></a>NYC Taxi & Limousine Commission – Fahrtenaufzeichnungen für „Yellow Taxi“

Datensätze zu Fahrten mit gelben Taxis enthalten Felder mit Datum/Uhrzeit für Abholung und Ankunft, Start- und Zielort, Fahrtentfernungen, Einzelkosten, Tarifarten, Zahlungsarten und vom Fahrer gemeldeten Fahrgastzahlen.

[!INCLUDE [Open Dataset usage notice](../../includes/open-datasets-usage-note.md)]

## <a name="volume-and-retention"></a>Volume und Aufbewahrung

Dieses Dataset wird im Parquet-Format gespeichert. Aktuell (Stand: 2018) gibt es etwa 1,5 Milliarden Zeilen (50 GB).

Dieses Dataset enthält alle Datensätze der Jahre 2009 bis 2018. Verwenden Sie Parametereinstellungen im SDK, um Daten innerhalb eines bestimmten Zeitbereichs abzurufen.

## <a name="storage-location"></a>Speicherort

Dieses Dataset wird in der Azure-Region „USA, Osten“ gespeichert. Aus Gründen der Affinität wird die Zuweisung von Computeressourcen in der Region „USA, Osten“ empfohlen.

## <a name="additional-information"></a>Zusätzliche Informationen

NYC Taxi and Limousine Commission (TLC):

Die Daten wurden erfasst und von Technologieanbietern, die an den Programmen Taxicab Passenger Enhancement (TPEP) und Livery Passenger Enhancement (LPEP) teilnehmen, an die TLC weitergegeben. Die Fahrtdaten wurden nicht von der TLC erstellt, und die TLC übernimmt keine Garantie für die Korrektheit dieser Daten.

Zeigen Sie den [ursprünglichen Speicherort des Datasets](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page) und die [ursprünglichen Nutzungsbedingungen](https://www1.nyc.gov/home/terms-of-use.page) an.

## <a name="columns"></a>Spalten
| Name                 | Datentyp | Eindeutig      | Beispielwerte                         | BESCHREIBUNG                                                                                                                                                                                                                                            |
|----------------------|-----------|-------------|-----------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| doLocationId         | Zeichenfolge    | 265         | 161 236                                 | TLC-Taxizone, in der das Taxameter abgeschalten wurde.                                                                                                                                                                                                   |
| endLat               | double    | 961,994     | 41.366138 40.75                         |                                                                                                                                                                                                                                                        |
| endLon               | double    | 1,144,935   | -73.137393 -73.9824                     |                                                                                                                                                                                                                                                        |
| extra                | double    | 877         | 0,5 1,0                                 | Verschiedene Extras und Zusatzkosten. Dies umfasst derzeit nur Gebühren von 0,50 USD und 1 USD für Fahrten zu Stoßzeiten oder in der Nacht.                                                                                                                                   |
| fareAmount           | double    | 18,935      | 6.5 4.5                                 | Die vom Taxameter anhand der Zeit und Strecke berechneten Kosten.                                                                                                                                                                                                    |
| improvementSurcharge | Zeichenfolge    | 60          | 0,3 0                                   | Zusatzkosten in Höhe von 0,30 USD für kurze Distanzen. Diese Zusatzkosten wurden 2015 eingeführt.                                                                                                                                     |
| mtaTax               | double    | 360         | 0,5 -0,5                                | MTA-Steuer von 0,50 USD, die automatisch anhand der berechneten Kosten ausgelöst wird.                                                                                                                                                                        |
| passengerCount       | INT       | 64          | 1 2                                     | Die Anzahl der Mitfahrer in dem Fahrzeug. Dieser Wert wird vom Fahrer eingegeben.                                                                                                                                                                               |
| paymentType          | Zeichenfolge    | 6\.282       | CSH CRD                                 | Ein numerischer Code, der angibt, wie der Kunde für die Fahrt bezahlt hat. 1 = Kreditkarte; 2 = Bargeld; 3 = Keine Gebühr; 4 = Konflikt; 5 = Unbekannt; 6 = Ungültige Fahrt.                                                                                                          |
| puLocationId         | Zeichenfolge    | 266         | 237 161                                 | TLC-Taxizone, in der das Taxameter aktiviert wurde.                                                                                                                                                                                                      |
| puMonth              | INT       | 12          | 3 5                                     |                                                                                                                                                                                                                                                        |
| puYear               | INT       | 29          | 2012 2011                               |                                                                                                                                                                                                                                                        |
| rateCodeId           | INT       | 56          | 1 2                                     | Der zuletzt geltende Tarifode am Ende der Fahrt. 1 = Standardpreis; 2 = JFK; 3 = Newark; 4 = Nassau oder Westchester; 5 = Ausgehandelter Fahrpreis; 6 = Gruppenfahrt.                                                                                                |
| startLat             | double    | 833,016     | 41.366138 40.7741                       |                                                                                                                                                                                                                                                        |
| startLon             | double    | 957,428     | -73.137393 -73.9821                     |                                                                                                                                                                                                                                                        |
| storeAndFwdFlag      | Zeichenfolge    | 8           | N 0                                     | Diese Kennzeichnung gibt an, ob der Datensatz zu der Fahrt im Speicher des Fahrzeugs gespeichert wurde, bevor er an den Verkäufer gesendet wurde (auch als „Speicherung und Weiterleitung“ bezeichnet), weil das Fahrzeug keine Verbindung mit dem Server hatte. Y = Fahrt mit Speicherung und Weiterleitung; N = Keine Fahrt mit Speicherung und Weiterleitung. |
| tipAmount            | double    | 12,121      | 1,0 2,0                                 | In dieses Feld werden automatisch Trinkgelder eingetragen, die mit einer Kreditkarte gezahlt werden. Trinkgelder in bar sind nicht enthalten.                                                                                                                                                                |
| tollsAmount          | double    | 6,634       | 5.33 4.8                                | Summe der gezahlten Mautgebühren für die Fahrt.                                                                                                                                                                                                                |
| totalAmount          | double    | 39,707      | 7.0 7.8                                 | Der Gesamtbetrag, der Passagieren in Rechnung gestellt wird. Umfasst keine baren Trinkgelder.                                                                                                                                                                                    |
| tpepDropoffDateTime  | timestamp | 290,185,010 | 2010-11-07 01:29:00 2013-11-03 01:22:00 | Datum und Uhrzeit für Abschaltung des Taxameters.                                                                                                                                                                                                       |
| tpepPickupDateTime   | timestamp | 289,948,585 | 2010-11-07 01:00:00 2009-11-01 01:05:00 | Datum und Uhrzeit für Einschaltung des Taxameters.                                                                                                                                                                                                          |
| tripDistance         | double    | 14,003      | 1.0 0.9                                 | Die zurückgelegte Strecke in Meilen entsprechend dem Taxameter.                                                                                                                                                                                          |
| vendorID             | Zeichenfolge    | 7           | VTS CMT                                 | Ein Code, der den TPEP-Anbieter angibt, der den Datensatz bereitgestellt hat. 1 = Creative Mobile Technologies, LLC; 2 = VeriFone Inc.                                                                                                                                   |
| vendorID             | INT       | 2           | 2 1                                     | Ein Code für den LPEP-Anbieter, der den Datensatz bereitgestellt hat. 1 = Creative Mobile Technologies, LLC; 2 = VeriFone Inc.                                                                                                                                   |

## <a name="preview"></a>Vorschau

| vendorID | tpepPickupDateTime    | tpepDropoffDateTime   | passengerCount | tripDistance | puLocationId | doLocationId | rateCodeId | storeAndFwdFlag | paymentType | fareAmount | extra | mtaTax | improvementSurcharge | tipAmount | tollsAmount | totalAmount | puYear | puMonth |
|----------|-----------------------|-----------------------|----------------|--------------|--------------|--------------|------------|-----------------|-------------|------------|-------|--------|----------------------|-----------|-------------|-------------|--------|---------|
| 2        | 1/24/2088 12:25:39 AM | 1/24/2088 7:28:25 AM  | 1              | 4.05         | 24           | 162          | 1          | N               | 2           | 14.5       | 0     | 0,5    | 0,3                  | 0         | 0           | 15,3        | 2088   | 1       |
| 2        | 1/24/2088 12:15:42 AM | 1/24/2088 12:19:46 AM | 1              | 0,63         | 41           | 166          | 1          | N               | 2           | 4,5        | 0     | 0,5    | 0,3                  | 0         | 0           | 5.3         | 2088   | 1       |
| 2        | 11/4/2084 12:32:24 PM | 11/4/2084 12:47:41 PM | 1              | 1,34         | 238          | 236          | 1          | N               | 2           | 10         | 0     | 0,5    | 0,3                  | 0         | 0           | 10,8        | 2084   | 11      |
| 2        | 11/4/2084 12:25:53 PM | 11/4/2084 12:29:00 PM | 1              | 0,32         | 238          | 238          | 1          | N               | 2           | 4          | 0     | 0,5    | 0,3                  | 0         | 0           | 4.8         | 2084   | 11      |
| 2        | 11/4/2084 12:08:33 PM | 11/4/2084 12:22:24 PM | 1              | 1,85         | 236          | 238          | 1          | N               | 2           | 10         | 0     | 0,5    | 0,3                  | 0         | 0           | 10,8        | 2084   | 11      |
| 2        | 11/4/2084 11:41:35 AM | 11/4/2084 11:59:41 AM | 1              | 1,65         | 68           | 237          | 1          | N               | 2           | 12,5       | 0     | 0,5    | 0,3                  | 0         | 0           | 13.3        | 2084   | 11      |
| 2        | 11/4/2084 11:27:28 AM | 11/4/2084 11:39:52 AM | 1              | 1.07         | 170          | 68           | 1          | N               | 2           | 9          | 0     | 0,5    | 0,3                  | 0         | 0           | 9,8         | 2084   | 11      |
| 2        | 11/4/2084 11:19:06 AM | 11/4/2084 11:26:44 AM | 1              | 1.3          | 107          | 170          | 1          | N               | 2           | 7,5        | 0     | 0,5    | 0,3                  | 0         | 0           | 8.3         | 2084   | 11      |
| 2        | 11/4/2084 11:02:59 AM | 11/4/2084 11:15:51 AM | 1              | 1,85         | 113          | 137          | 1          | N               | 2           | 10         | 0     | 0,5    | 0,3                  | 0         | 0           | 10,8        | 2084   | 11      |
| 2        | 11/4/2084 10:46:05 AM | 11/4/2084 10:50:09 AM | 1              | 0.62         | 231          | 231          | 1          | N               | 2           | 4,5        | 0     | 0,5    | 0,3                  | 0         | 0           | 5.3         | 2084   | 11      |

## <a name="data-access"></a>Datenzugriff

### <a name="azure-notebooks"></a>Azure Notebooks

# <a name="azureml-opendatasets"></a>[azureml-opendatasets](#tab/azureml-opendatasets)

```python
# This is a package in preview.
from azureml.opendatasets import NycTlcYellow

from datetime import datetime
from dateutil import parser


end_date = parser.parse('2018-06-06')
start_date = parser.parse('2018-05-01')
nyc_tlc = NycTlcYellow(start_date=start_date, end_date=end_date)
nyc_tlc_df = nyc_tlc.to_pandas_dataframe()

nyc_tlc_df.info()
```

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

```python
# Pip install packages
import os, sys

!{sys.executable} -m pip install azure-storage-blob
!{sys.executable} -m pip install pyarrow
!{sys.executable} -m pip install pandas

# Azure storage access info
azure_storage_account_name = "azureopendatastorage"
azure_storage_sas_token = r""
container_name = "nyctlc"
folder_name = "yellow"

from azure.storage.blob import BlockBlobServicefrom azure.storage.blob import BlobServiceClient, BlobClient, ContainerClient

if azure_storage_account_name is None or azure_storage_sas_token is None:
    raise Exception(
        "Provide your specific name and key for your Azure Storage account--see the Prerequisites section earlier.")

print('Looking for the first parquet under the folder ' +
      folder_name + ' in container "' + container_name + '"...')
container_url = f"https://{azure_storage_account_name}.blob.core.windows.net/"
blob_service_client = BlobServiceClient(
    container_url, azure_storage_sas_token if azure_storage_sas_token else None)

container_client = blob_service_client.get_container_client(container_name)
blobs = container_client.list_blobs(folder_name)
sorted_blobs = sorted(list(blobs), key=lambda e: e.name, reverse=True)
targetBlobName = ''
for blob in sorted_blobs:
    if blob.name.startswith(folder_name) and blob.name.endswith('.parquet'):
        targetBlobName = blob.name
        break

print('Target blob to download: ' + targetBlobName)
_, filename = os.path.split(targetBlobName)
blob_client = container_client.get_blob_client(targetBlobName)
with open(filename, 'wb') as local_file:
    blob_client.download_blob().download_to_stream(local_file)

# Read the parquet file into Pandas data frame
import pandas as pd

print('Reading the parquet file into Pandas data frame')
df = pd.read_parquet(filename)

# you can add your filter at below
print('Loaded as a Pandas data frame: ')
df
```

# <a name="pyspark"></a>[pyspark](#tab/pyspark)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

---

### <a name="azure-databricks"></a>Azure Databricks

# <a name="azureml-opendatasets"></a>[azureml-opendatasets](#tab/azureml-opendatasets)

```python
# This is a package in preview.
# You need to pip install azureml-opendatasets in Databricks cluster. https://docs.microsoft.com/en-us/azure/data-explorer/connect-from-databricks#install-the-python-library-on-your-azure-databricks-cluster
from azureml.opendatasets import NycTlcYellow

from datetime import datetime
from dateutil import parser


end_date = parser.parse('2018-06-06')
start_date = parser.parse('2018-05-01')
nyc_tlc = NycTlcYellow(start_date=start_date, end_date=end_date)
nyc_tlc_df = nyc_tlc.to_spark_dataframe()

display(nyc_tlc_df.limit(5))
```

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

# <a name="pyspark"></a>[pyspark](#tab/pyspark)

```python
# Azure storage access info
blob_account_name = "azureopendatastorage"
blob_container_name = "nyctlc"
blob_relative_path = "yellow"
blob_sas_token = r"

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

# <a name="azureml-opendatasets"></a>[azureml-opendatasets](#tab/azureml-opendatasets)

```python
# This is a package in preview.
from azureml.opendatasets import NycTlcYellow

from datetime import datetime
from dateutil import parser

end_date = parser.parse('2018-06-06')
start_date = parser.parse('2018-05-01')
nyc_tlc = NycTlcYellow(start_date=start_date, end_date=end_date)
nyc_tlc_df = nyc_tlc.to_spark_dataframe()

# Display top 5 rows
display(nyc_tlc_df.limit(5))
```

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

# <a name="pyspark"></a>[pyspark](#tab/pyspark)

```python
# Azure storage access info
blob_account_name = "azureopendatastorage"
blob_container_name = "nyctlc"
blob_relative_path = "yellow"
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