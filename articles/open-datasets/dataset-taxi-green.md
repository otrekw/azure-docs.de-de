---
title: Dataset für „NYC Taxi and Limousine – Green“
titleSuffix: Azure Open Datasets
description: Erfahren Sie, wie Sie das Dataset für „NYC Taxi and Limousine – Green“ in Azure Open Datasets verwenden.
ms.service: open-datasets
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 04/16/2021
ms.openlocfilehash: 058969168b09c1eb394b6fc7a9f06c401c7f9763
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112038797"
---
# <a name="nyc-taxi--limousine-commission---green-taxi-trip-records"></a>NYC Taxi and Limousine Commission – Fahrtenaufzeichnungen für „Green Taxi“

Datensätze zu Fahrten mit grünen Taxis enthalten Felder mit Datum/Uhrzeit für Abholung und Ankunft, Start- und Zielort, zurückgelegten Entfernungen, Einzelkosten, Tarifarten, Zahlungsarten und vom Fahrer gemeldeten Fahrgastzahlen.

[!INCLUDE [Open Dataset usage notice](../../includes/open-datasets-usage-note.md)]

## <a name="volume-and-retention"></a>Volume und Aufbewahrung

Dieses Dataset wird im Parquet-Format gespeichert. Aktuell (Stand: 2018) gibt es etwa 80 Millionen Zeilen (2 GB).

Dieses Dataset enthält alle Datensätze der Jahre 2009 bis 2018. Verwenden Sie Parametereinstellungen im SDK, um Daten innerhalb eines bestimmten Zeitbereichs abzurufen.

## <a name="storage-location"></a>Speicherort

Dieses Dataset wird in der Azure-Region „USA, Osten“ gespeichert. Aus Gründen der Affinität wird die Zuweisung von Computeressourcen in der Region „USA, Osten“ empfohlen.

## <a name="additional-information"></a>Zusätzliche Informationen

NYC Taxi and Limousine Commission (TLC):

Die Daten wurden erfasst und von Technologieanbietern, die an den Programmen Taxicab Passenger Enhancement (TPEP) und Livery Passenger Enhancement (LPEP) teilnehmen, an die TLC weitergegeben. Die Fahrtdaten wurden nicht von der TLC erstellt, und die TLC übernimmt keine Garantie für die Korrektheit dieser Daten.

Zeigen Sie den [ursprünglichen Speicherort des Datasets](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page) und die [ursprünglichen Nutzungsbedingungen](https://www1.nyc.gov/home/terms-of-use.page) an.

## <a name="columns"></a>Spalten

| Name                 | Datentyp | Eindeutig     | Beispielwerte                         | BESCHREIBUNG                                                                                                                                                                                                                                          |
|----------------------|-----------|------------|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| doLocationId         | Zeichenfolge    | 264        | 74 42                                   | DOLocationID TLC-Taxizone, in der das Taxameter aktiviert wurde.                                                                                                                                                                                    |
| dropoffLatitude      | double    | 109.721    | 40,7743034362793 40,77431869506836      | Nicht mehr erfasst ab Juli 2016                                                                                                                                                                                                                       |
| dropoffLongitude     | double    | 75.502     | -73,95272827148438 -73,95274353027344   | Nicht mehr erfasst ab Juli 2016                                                                                                                                                                                                                       |
| extra                | double    | 202        | 0,5 1,0                                 | Verschiedene Extras und Zusatzkosten. Dies umfasst derzeit nur Gebühren von 0,50 USD und 1 USD für Fahrten zu Stoßzeiten oder in der Nacht.                                                                                                                                 |
| fareAmount           | double    | 10.367     | 6,0 5,5                                 | Die vom Taxameter anhand der Zeit und Strecke berechneten Kosten.                                                                                                                                                                                                  |
| improvementSurcharge | Zeichenfolge    | 92         | 0,3 0                                   | Zusatzkosten in Höhe von 0,30 USD für Kurzfahrten, bei denen das Taxi herangewunken wurde. Diese Zusatzkosten wurden 2015 eingeführt.                                                                                                                         |
| lpepDropoffDatetime  | timestamp | 58.100.713 | 2016-05-22 00:00:00 2016-05-09 00:00:00 | Datum und Uhrzeit für Abschaltung des Taxameters.                                                                                                                                                                                                     |
| lpepPickupDatetime   | timestamp | 58.157.349 | 2013-10-22 12:40:36 2014-08-09 15:54:25 | Datum und Uhrzeit für Einschaltung des Taxameters.                                                                                                                                                                                                        |
| mtaTax               | double    | 34         | 0,5 -0,5                                | MTA-Steuer von 0,50 USD, die automatisch anhand der berechneten Kosten ausgelöst wird.                                                                                                                                                                      |
| passengerCount       | INT       | 10         | 1 2                                     | Die Anzahl der Mitfahrer in dem Fahrzeug. Dieser Wert wird vom Fahrer eingegeben.                                                                                                                                                                             |
| paymentType          | INT       | 5          | 2 1                                     | Ein numerischer Code, der angibt, wie der Kunde für die Fahrt bezahlt hat. 1 = Kreditkarte 2 = Bargeld 3 = Keine Gebühr 4 = Konflikt 5 = Unbekannt 6 = Ungültige Fahrt                                                                                                              |
| pickupLatitude       | double    | 95.110     | 40,721351623535156 40,721336364746094   | Nicht mehr erfasst ab Juli 2016                                                                                                                                                                                                                       |
| pickupLongitude      | double    | 55.722     | -73,84429931640625 -73,84429168701172   | Nicht mehr erfasst ab Juli 2016                                                                                                                                                                                                                       |
| puLocationId         | Zeichenfolge    | 264        | 74 41                                   | TLC-Taxizone, in der das Taxameter aktiviert wurde.                                                                                                                                                                                                    |
| puMonth              | INT       | 12         | 3 5                                     |                                                                                                                                                                                                                                                      |
| puYear               | INT       | 14         | 2015 2016                               |                                                                                                                                                                                                                                                      |
| rateCodeID           | INT       | 7          | 1 5                                     | Der zuletzt geltende Tarifode am Ende der Fahrt. 1 = Standardpreis 2 = JFK 3 = Newark 4 = Nassau oder Westchester 5 = Ausgehandelter Fahrpreis 6 = Gruppenfahrt                                                                                                    |
| storeAndFwdFlag      | Zeichenfolge    | 2          | N Y                                     | Diese Kennzeichnung gibt an, ob der Datensatz zu der Fahrt im Speicher des Fahrzeugs gespeichert wurde, bevor er an den Verkäufer gesendet wurde (auch als „Speicherung und Weiterleitung“ bezeichnet), weil das Fahrzeug keine Verbindung mit dem Server hatte. Y = Fahrt mit Speicherung und Weiterleitung N = Keine Fahrt mit Speicherung und Weiterleitung |
| tipAmount            | double    | 6\.206      | 1,0 2,0                                 | Höhe des Trinkgeldbetrags: Dieses Feld wird automatisch für Trinkgelder aufgefüllt, die mit einer Kreditkarte gezahlt werden. Trinkgelder in bar sind nicht enthalten.                                                                                                                                                 |
| tollsAmount          | double    | 2\.150      | 5,54 5,76                               | Summe der gezahlten Mautgebühren für die Fahrt.                                                                                                                                                                                                              |
| totalAmount          | double    | 20.188     | 7,8 6,8                                 | Der Gesamtbetrag, der Passagieren in Rechnung gestellt wird. Umfasst keine baren Trinkgelder.                                                                                                                                                                                  |
| tripDistance         | double    | 7\.060      | 0,9 1,0                                 | Die zurückgelegte Strecke in Meilen entsprechend dem Taxameter.                                                                                                                                                                                        |
| tripType             | INT       | 3          | 1 2                                     | Ein Code, der angibt, ob das Taxi auf der Straße herangewunken oder von der Zentrale losgeschickt wurde. Dieser Code wird automatisch anhand der gemessenen Fahrtkosten zugewiesen, kann aber vom Fahrer geändert werden. 1 = Herangewunken 2 = Über Zentrale                                                      |
| vendorID             | INT       | 2          | 2 1                                     | Ein Code für den LPEP-Anbieter, der den Datensatz bereitgestellt hat. 1 = Creative Mobile Technologies, LLC; 2 = VeriFone Inc.                                                                                                                                 |

## <a name="preview"></a>Vorschau

| vendorID | lpepPickupDatetime     | lpepDropoffDatetime    | passengerCount | tripDistance | puLocationId | doLocationId | rateCodeID | storeAndFwdFlag | paymentType | fareAmount | extra | mtaTax | improvementSurcharge | tipAmount | tollsAmount | totalAmount | tripType | puYear | puMonth |
|----------|------------------------|------------------------|----------------|--------------|--------------|--------------|------------|-----------------|-------------|------------|-------|--------|----------------------|-----------|-------------|-------------|----------|--------|---------|
| 2        | 6/24/2081 5:40:37 PM   | 6/24/2081 6:42:47 PM   | 1              | 16,95        | 93           | 117          | 1          | N               | 1           | 52         | 1     | 0,5    | 0,3                  | 0         | 2.16        | 55,96       | 1        | 2081   | 6       |
| 2        | 11/28/2030 12:19:29 AM | 11/28/2030 12:25:37 AM | 1              | 1.08         | 42           | 247          | 1          | N               | 2           | 6,5        | 0     | 0,5    | 0,3                  | 0         | 0           | 7.3         | 1        | 2030   | 11      |
| 2        | 11/28/2030 12:14:50 AM | 11/28/2030 12:14:54 AM | 1              | 0,03         | 42           | 42           | 5          | N               | 2           | 5          | 0     | 0      | 0                    | 0         | 0           | 5           | 2        | 2030   | 11      |
| 2        | 11/14/2020 11:38:07 AM | 11/14/2020 11:42:22 AM | 1              | 0,63         | 129          | 129          | 1          | N               | 2           | 4,5        | 1     | 0,5    | 0,3                  | 0         | 0           | 6.3         | 1        | 2020   | 11      |
| 2        | 11/14/2020 9:55:36 AM  | 11/14/2020 10:04:54 AM | 1              | 3.8          | 82           | 138          | 1          | N               | 2           | 12,5       | 1     | 0,5    | 0,3                  | 0         | 0           | 14.3        | 1        | 2020   | 11      |
| 2        | 8/26/2019 4:18:37 PM   | 8/26/2019 4:19:35 PM   | 1              | 0            | 264          | 264          | 1          | N               | 2           | 1          | 0     | 0,5    | 0,3                  | 0         | 0           | 1.8         | 1        | 2019   | 8       |
| 2        | 7/1/2019 8:28:33 AM    | 7/1/2019 8:32:33 AM    | 1              | 0.71         | 7            | 7            | 1          | N               | 1           | 5          | 0     | 0,5    | 0,3                  | 1,74      | 0           | 7,54        | 1        | 2019   | 7       |
| 2        | 7/1/2019 12:04:53 AM   | 7/1/2019 12:21:56 AM   | 1              | 2.71         | 223          | 145          | 1          | N               | 2           | 13         | 0,5   | 0,5    | 0,3                  | 0         | 0           | 14.3        | 1        | 2019   | 7       |
| 2        | 7/1/2019 12:04:11 AM   | 7/1/2019 12:21:15 AM   | 1              | 3,14         | 166          | 142          | 1          | N               | 2           | 14.5       | 0,5   | 0,5    | 0,3                  | 0         | 0           | 18,55       | 1        | 2019   | 7       |
| 2        | 7/1/2019 12:03:37 AM   | 7/1/2019 12:09:27 AM   | 1              | 0,78         | 74           | 74           | 1          | N               | 1           | 6          | 0,5   | 0,5    | 0,3                  | 1,46      | 0           | 8,76        | 1        | 2019   | 7       |

## <a name="data-access"></a>Datenzugriff

### <a name="azure-notebooks"></a>Azure Notebooks

# <a name="azureml-opendatasets"></a>[azureml-opendatasets](#tab/azureml-opendatasets)

```python
# This is a package in preview.
from azureml.opendatasets import NycTlcGreen

from datetime import datetime
from dateutil import parser


end_date = parser.parse('2018-06-06')
start_date = parser.parse('2018-05-01')
nyc_tlc = NycTlcGreen(start_date=start_date, end_date=end_date)
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
folder_name = "green"

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
from azureml.opendatasets import NycTlcGreen

from datetime import datetime
from dateutil import parser


end_date = parser.parse('2018-06-06')
start_date = parser.parse('2018-05-01')
nyc_tlc = NycTlcGreen(start_date=start_date, end_date=end_date)
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
blob_relative_path = "green"
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

# <a name="azureml-opendatasets"></a>[azureml-opendatasets](#tab/azureml-opendatasets)

```python
# This is a package in preview.
from azureml.opendatasets import NycTlcGreen

from datetime import datetime
from dateutil import parser


end_date = parser.parse('2018-06-06')
start_date = parser.parse('2018-05-01')
nyc_tlc = NycTlcGreen(start_date=start_date, end_date=end_date)
nyc_tlc_df = nyc_tlc.to_spark_dataframe()

# Display top 5 rows
display(nyc_tlc_df.limit(5))

# Display data statistic information
display(nyc_tlc_df, summary = True)
```

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

# <a name="pyspark"></a>[pyspark](#tab/pyspark)

```python
# Azure storage access info
blob_account_name = "azureopendatastorage"
blob_container_name = "nyctlc"
blob_relative_path = "green"
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