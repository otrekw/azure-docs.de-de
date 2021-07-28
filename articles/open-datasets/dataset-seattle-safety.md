---
title: Sicherheitsdaten zu Seattle
titleSuffix: Azure Open Datasets
description: Es wird beschrieben, wie Sie das Dataset mit den Sicherheitsdaten zu Seattle in Azure Open Datasets verwenden.
ms.service: open-datasets
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 04/16/2021
ms.openlocfilehash: c865e09a6d4591b1f80ddd55c2a260b6e06f154a
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112038803"
---
# <a name="seattle-safety-data"></a>Sicherheitsdaten zu Seattle

911-Einsätze des Seattle Fire Department.

[!INCLUDE [Open Dataset usage notice](../../includes/open-datasets-usage-note.md)]

## <a name="volume-and-retention"></a>Volume und Aufbewahrung

Dieses Dataset wird im Parquet-Format gespeichert. Es wird täglich aktualisiert und hat im Jahr 2019 ca. 800.000 Zeilen (20 MB) enthalten.

Dieses Dataset enthält historische Datensätze, die von 2010 bis heute gesammelt wurden. Verwenden Sie Parametereinstellungen im SDK, um Daten innerhalb eines bestimmten Zeitbereichs abzurufen.

## <a name="storage-location"></a>Speicherort

Dieses Dataset wird in der Azure-Region „USA, Osten“ gespeichert. Wir empfehlen Ihnen, aus Affinitätsgründen Computeressourcen in „USA, Osten“ anzuordnen.

## <a name="additional-information"></a>Zusätzliche Informationen

Dieses Dataset wurde von der Stadtverwaltung von Seattle bezogen. Weitere Informationen finden Sie auf der [Website der Stadt Seattle](http://web5.seattle.gov/MNM/incidentresponse.aspx). Sehen Sie sich die Informationen zur [Lizenzierung und Zuordnung für die Bedingungen der Verwendung dieses Datasets](https://creativecommons.org/publicdomain/zero/1.0/legalcode) an. Senden Sie eine E-Mail an open.data@seattle.gov, wenn Sie Fragen zur Datenquelle haben.

## <a name="columns"></a>Spalten

| Name | Datentyp | Eindeutig | Werte (Beispiel) | BESCHREIBUNG |
|-|-|-|-|-|
| address | Zeichenfolge | 196,965 | 517 3rd Av 318 2nd Av Et S | Ort des Vorfalls. |
| category | Zeichenfolge | 232 | Aid Response Medic Response | Antworttyp. |
| dataSubtype | Zeichenfolge | 1 | 911_Fire | “911_Fire” |
| dataType | Zeichenfolge | 1 | Sicherheit | “Safety” |
| dateTime | timestamp | 1,533,401 | 2020-11-04 06:49:00 2019-06-19 13:49:00 | Datum und Uhrzeit des Anrufs. |
| latitude | double | 94,332 | 47.602172 47.600194 | Dies ist der Wert für den Breitengrad. Die Linien des Breitengrads sind parallel zum Äquator. |
| longitude | double | 79,492 | -122.330863 -122.330541 | Dies ist der Wert für den Längengrad. Die Linien der Längengrade verlaufen senkrecht zu den Linien der Breitengrade und durchlaufen beide Pole. |

## <a name="preview"></a>Vorschau

| dataType | dataSubtype | dateTime | category | subcategory | status | address | latitude | longitude | source | extendedProperties |
|-|-|-|-|-|-|-|-|-|-|-|
| Sicherheit | 911_Fire | 4/28/2021 5:22:00 AM | Rubbish Fire | NULL | NULL | 200 University St | 47.607299 | -122.337087 | NULL |  |
| Sicherheit | 911_Fire | 4/28/2021 5:15:00 AM | Triaged Incident | NULL | NULL | 6th Ave/Olive Way | 47.61313 | -122.336282 | NULL |  |
| Sicherheit | 911_Fire | 4/28/2021 5:12:00 AM | Aid Response | NULL | NULL | 4th Ave S/Seattle Blvd S | 47.596486 | -122.329046 | NULL |  |
| Sicherheit | 911_Fire | 4/28/2021 5:09:00 AM | Rubbish Fire | NULL | NULL | 3rd Ave/University St | 47.607763 | -122.335976 | NULL |  |
| Sicherheit | 911_Fire | 4/28/2021 4:57:00 AM | Low Acuity Response | NULL | NULL | 533 3rd Ave W | 47.623717 | -122.360635 | NULL |  |
| Sicherheit | 911_Fire | 4/28/2021 4:57:00 AM | Trans to AMR | NULL | NULL | 4638 S Austin St | 47.534702 | -122.274812 | NULL |  |
| Sicherheit | 911_Fire | 4/28/2021 4:55:00 AM | Triaged Incident | NULL | NULL | 8th Ave N/Harrison St | 47.622051 | -122.341066 | NULL |  |

## <a name="data-access"></a>Datenzugriff

### <a name="azure-notebooks"></a>Azure Notebooks

# <a name="azureml-opendatasets"></a>[azureml-opendatasets](#tab/azureml-opendatasets)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azureml-opendatasets&registryId=city_safety_seattle -->

> [!TIP]
> Inhalt von _DownloadNotebook?serviceType=AzureNotebooks&package=azureml-opendatasets&registryId=city_safety_seattle_. **[In GitHub öffnen](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azureml-opendatasets&registryId=city_safety_seattle)** .

```
# This is a package in preview.
from azureml.opendatasets import SeattleSafety

from datetime import datetime
from dateutil import parser


end_date = parser.parse('2016-01-01')
start_date = parser.parse('2015-05-01')
safety = SeattleSafety(start_date=start_date, end_date=end_date)
safety = safety.to_pandas_dataframe()
```

```
safety.info()
```

<!-- nbend -->

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=city_safety_seattle -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=city_safety_seattle)** .

```python
# Pip install packages
import os, sys

!{sys.executable} -m pip install azure-storage-blob
!{sys.executable} -m pip install pyarrow
!{sys.executable} -m pip install pandas
```

```python
# Azure storage access info
azure_storage_account_name = "azureopendatastorage"
azure_storage_sas_token = r""
container_name = "citydatacontainer"
folder_name = "Safety/Release/city=Seattle"
```

```python
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
```

```python
# Read the parquet file into Pandas data frame
import pandas as pd

print('Reading the parquet file into Pandas data frame')
df = pd.read_parquet(filename)
```

```python
# you can add your filter at below
print('Loaded as a Pandas data frame: ')
df
```

<!-- nbend -->

# <a name="pyspark"></a>[pyspark](#tab/pyspark)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

# <a name="sql"></a>[SQL](#tab/sql)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

---

### <a name="azure-databricks"></a>Azure Databricks

# <a name="azureml-opendatasets"></a>[azureml-opendatasets](#tab/azureml-opendatasets)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=azureml-opendatasets&registryId=city_safety_seattle -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=azureml-opendatasets&registryId=city_safety_seattle)** .

```
# This is a package in preview.
# You need to pip install azureml-opendatasets in Databricks cluster. https://docs.microsoft.com/en-us/azure/data-explorer/connect-from-databricks#install-the-python-library-on-your-azure-databricks-cluster
from azureml.opendatasets import SeattleSafety

from datetime import datetime
from dateutil import parser


end_date = parser.parse('2016-01-01')
start_date = parser.parse('2015-05-01')
safety = SeattleSafety(start_date=start_date, end_date=end_date)
safety = safety.to_spark_dataframe()
```

```
display(safety.limit(5))
```

<!-- nbend -->

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

# <a name="pyspark"></a>[pyspark](#tab/pyspark)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=pyspark&registryId=city_safety_seattle -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=pyspark&registryId=city_safety_seattle)** .

```python
# Azure storage access info
blob_account_name = "azureopendatastorage"
blob_container_name = "citydatacontainer"
blob_relative_path = "Safety/Release/city=Seattle"
blob_sas_token = r""
```

```python
# Allow SPARK to read from Blob remotely
wasbs_path = 'wasbs://%s@%s.blob.core.windows.net/%s' % (blob_container_name, blob_account_name, blob_relative_path)
spark.conf.set(
  'fs.azure.sas.%s.%s.blob.core.windows.net' % (blob_container_name, blob_account_name),
  blob_sas_token)
print('Remote blob path: ' + wasbs_path)
```

```python
# SPARK read parquet, note that it won't load any data yet by now
df = spark.read.parquet(wasbs_path)
print('Register the DataFrame as a SQL temporary view: source')
df.createOrReplaceTempView('source')
```

```python
# Display top 10 rows
print('Displaying top 10 rows: ')
display(spark.sql('SELECT * FROM source LIMIT 10'))
```

<!-- nbend -->

# <a name="sql"></a>[SQL](#tab/sql)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

---

### <a name="azure-synapse"></a>Azure Synapse

# <a name="azureml-opendatasets"></a>[azureml-opendatasets](#tab/azureml-opendatasets)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=azureml-opendatasets&registryId=city_safety_seattle -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=azureml-opendatasets&registryId=city_safety_seattle)** .

```python
# This is a package in preview.
from azureml.opendatasets import SeattleSafety

from datetime import datetime
from dateutil import parser


end_date = parser.parse('2016-01-01')
start_date = parser.parse('2015-05-01')
safety = SeattleSafety(start_date=start_date, end_date=end_date)
safety = safety.to_spark_dataframe()
```

```python
# Display top 5 rows
display(safety.limit(5))
```

<!-- nbend -->

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

# <a name="pyspark"></a>[pyspark](#tab/pyspark)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=pyspark&registryId=city_safety_seattle -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=pyspark&registryId=city_safety_seattle)** .

```python
# Azure storage access info
blob_account_name = "azureopendatastorage"
blob_container_name = "citydatacontainer"
blob_relative_path = "Safety/Release/city=Seattle"
blob_sas_token = r""
```

```python
# Allow SPARK to read from Blob remotely
wasbs_path = 'wasbs://%s@%s.blob.core.windows.net/%s' % (blob_container_name, blob_account_name, blob_relative_path)
spark.conf.set(
  'fs.azure.sas.%s.%s.blob.core.windows.net' % (blob_container_name, blob_account_name),
  blob_sas_token)
print('Remote blob path: ' + wasbs_path)
```

```python
# SPARK read parquet, note that it won't load any data yet by now
df = spark.read.parquet(wasbs_path)
print('Register the DataFrame as a SQL temporary view: source')
df.createOrReplaceTempView('source')
```

```python
# Display top 10 rows
print('Displaying top 10 rows: ')
display(spark.sql('SELECT * FROM source LIMIT 10'))
```

<!-- nbend -->

# <a name="sql"></a>[SQL](#tab/sql)

```sql
SELECT
    TOP 100 *
FROM
    OPENROWSET(
        BULK             'https://azureopendatastorage.blob.core.windows.net/citydatacontainer/Safety/Release/city=Seattle/*.parquet',
        FORMAT         = 'parquet'
    ) AS [r];
```

---

## <a name="examples"></a>Beispiele

- Sehen Sie sich das Beispiel [City Safety Analytics](https://github.com/scottcounts/CitySafety) auf GitHub an.


## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich mit den restlichen Datasets im [Open Datasets-Katalog](dataset-catalog.md) vertraut.