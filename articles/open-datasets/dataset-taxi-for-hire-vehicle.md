---
title: Dataset für NYC Taxi and Limousine - Mietfahrzeuge
titleSuffix: Azure Open Datasets
description: Erfahren Sie, wie Sie das Dataset für „NYC Taxi and Limousine – Mietfahrzeuge (FHV)“ in Azure Open Datasets verwenden.
ms.service: open-datasets
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 04/16/2021
ms.openlocfilehash: 2875ddfa1bf94121bea9038b576035042accbcfe
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112038715"
---
# <a name="nyc-taxi--limousine-commission---for-hire-vehicle-fhv-trip-records"></a>NYC Taxi & Limousine Commission – Fahrtenaufzeichnungen für Mietfahrzeuge (FHV – For-Hire Vehicle)

Die Datensätze zu Fahrten mit Taxis und Limousinen umfassen Felder für die jeweiligen Lizenznummern der Zentrale, das Datum und die Uhrzeit der Abholung und die Standort-ID der Taxizone (Shape-Datei unten). Diese Datensätze werden anhand der von der Zentrale übermittelten Informationen zu Fahrten mit Taxis und Limousinen erstellt.

[!INCLUDE [Open Dataset usage notice](../../includes/open-datasets-usage-note.md)]

## <a name="volume-and-retention"></a>Volume und Aufbewahrung

Dieses Dataset wird im Parquet-Format gespeichert. Derzeit (Stand: 2018) gibt es 500 Millionen Zeilen (5 GB).

Dieses Dataset enthält alle Datensätze der Jahre 2009 bis 2018. Verwenden Sie Parametereinstellungen im SDK, um Daten innerhalb eines bestimmten Zeitbereichs abzurufen.

## <a name="storage-location"></a>Speicherort

Dieses Dataset wird in der Azure-Region „USA, Osten“ gespeichert. Aus Gründen der Affinität wird die Zuweisung von Computeressourcen in der Region „USA, Osten“ empfohlen.

## <a name="additional-information"></a>Zusätzliche Informationen

NYC Taxi and Limousine Commission (TLC):

Die Daten wurden erfasst und von Technologieanbietern, die an den Programmen Taxicab Passenger Enhancement (TPEP) und Livery Passenger Enhancement (LPEP) teilnehmen, an die TLC weitergegeben. Die Fahrtdaten wurden nicht von der TLC erstellt, und die TLC übernimmt keine Garantie für die Korrektheit dieser Daten.

Zeigen Sie den [ursprünglichen Speicherort des Datasets](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page) und die [ursprünglichen Nutzungsbedingungen](https://www1.nyc.gov/home/terms-of-use.page) an.

## <a name="columns"></a>Spalten

| Name            | Datentyp | Eindeutig      | Beispielwerte                         | BESCHREIBUNG                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|-----------------|-----------|-------------|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| dispatchBaseNum | Zeichenfolge    | 1,144       | B02510 B02764                           | Die TLC-Lizenznummer der Zentrale, von der die Fahrt ausgeht                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| doLocationId    | Zeichenfolge    | 267         | 265 132                                 | TLC-Taxizone, in der die Fahrt endete.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| dropOffDateTime | timestamp | 57,110,352  | 2017-07-31 23:59:00 2017-10-15 00:44:34 | Datum und Uhrzeit des Fahrtendes.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| pickupDateTime  | timestamp | 111,270,396 | 2016-08-16 00:00:00 2016-08-17 00:00:00 | Datum und Uhrzeit der Fahrgastabholung.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| puLocationId    | Zeichenfolge    | 266         | 79 161                                  | TLC-Taxizone, in der die Fahrt begann.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| puMonth         | INT       | 12          | 1 12                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| puYear          | INT       | 5           | 2018 2017                               |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| srFlag          | Zeichenfolge    | 44          | 1 2                                     | Gibt an, ob die Fahrt Teil einer Fahrgemeinschaftskette war, die von einem Personenbeförderungs-Dienstleistungsunternehmen mit hohem Durchsatz (z. B. Uber Pool, Lyft Line) angeboten wird. Bei gemeinschaftlichen Fahrten lautet der Wert „1“. Bei nicht gemeinschaftlichen Fahrten lautet der Wert „NULL“. HINWEIS: Bei den meisten Personenbeförderungs-Dienstleistungsunternehmen mit hohem Durchsatz werden nur gemeinschaftliche Fahrten gekennzeichnet, die angefragt und während der Fahrt einer weiteren Fahrgemeinschaftsanforderung zugeordnet wurden. Jedoch kennzeichnet Lyft (Zentralen mit den amtlichen Kennzeichen B02510 + B02844) auch Fahrten, für die eine gemeinschaftliche Fahrt angefordert, aber kein Passagier gefunden wurde. Daher können Fahrtaufzeichnungen mit der Kennzeichnung „SR_Flag=1“ dieser beiden Zentralen ENTWEDER die erste Fahrt einer Fahrgemeinschaftskette ODER eine Fahrt angeben, für die eine Fahrgemeinschaft angefordert, aber nicht gefunden wurde. Benutzer sollten eine übermäßig hohe Anzahl erfolgreicher gemeinschaftlicher Fahrten von Lyft erwarten. |

## <a name="preview"></a>Vorschau

| dispatchBaseNum | pickupDateTime        | dropOffDateTime      | puLocationId | doLocationId | srFlag | puYear | puMonth |
|-----------------|-----------------------|----------------------|--------------|--------------|--------|--------|---------|
| B03157          | 6/30/2019 11:59:57 PM | 7/1/2019 12:07:21 AM | 264          | NULL         | NULL   | 2019   | 6       |
| B01667          | 6/30/2019 11:59:56 PM | 7/1/2019 12:28:06 AM | 264          | NULL         | NULL   | 2019   | 6       |
| B02849          | 6/30/2019 11:59:55 PM | 7/1/2019 12:14:10 AM | 264          | NULL         | NULL   | 2019   | 6       |
| B02249          | 6/30/2019 11:59:53 PM | 7/1/2019 12:15:53 AM | 264          | NULL         | NULL   | 2019   | 6       |
| B00887          | 6/30/2019 11:59:48 PM | 7/1/2019 12:29:29 AM | 264          | NULL         | NULL   | 2019   | 6       |
| B01626          | 6/30/2019 11:59:45 PM | 7/1/2019 12:18:20 AM | 264          | NULL         | NULL   | 2019   | 6       |
| B01259          | 6/30/2019 11:59:44 PM | 7/1/2019 12:03:15 AM | 264          | NULL         | NULL   | 2019   | 6       |
| B01145          | 6/30/2019 11:59:43 PM | 7/1/2019 12:11:15 AM | 264          | NULL         | NULL   | 2019   | 6       |
| B00887          | 6/30/2019 11:59:42 PM | 7/1/2019 12:34:21 AM | 264          | NULL         | NULL   | 2019   | 6       |
| B00821          | 6/30/2019 11:59:40 PM | 7/1/2019 12:02:57 AM | 264          | NULL         | NULL   | 2019   | 6       |

## <a name="data-access"></a>Datenzugriff

### <a name="azure-notebooks"></a>Azure Notebooks

# <a name="azureml-opendatasets"></a>[azureml-opendatasets](#tab/azureml-opendatasets)

```python
# This is a package in preview.
from azureml.opendatasets import NycTlcFhv

from datetime import datetime
from dateutil import parser


end_date = parser.parse('2018-06-06')
start_date = parser.parse('2018-05-01')
nyc_tlc = NycTlcFhv(start_date=start_date, end_date=end_date)
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
folder_name = "fhv"

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
from azureml.opendatasets import NycTlcFhv

from datetime import datetime
from dateutil import parser


end_date = parser.parse('2018-06-06')
start_date = parser.parse('2018-05-01')
nyc_tlc = NycTlcFhv(start_date=start_date, end_date=end_date)
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
blob_relative_path = "fhv"
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
from azureml.opendatasets import NycTlcFhv

from datetime import datetime
from dateutil import parser


end_date = parser.parse('2018-06-06')
start_date = parser.parse('2018-05-01')
nyc_tlc = NycTlcFhv(start_date=start_date, end_date=end_date)
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
blob_relative_path = "fhv"
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