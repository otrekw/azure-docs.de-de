---
title: Sicherheitsdaten zu New York City
titleSuffix: Azure Open Datasets
description: Es wird beschrieben, wie Sie das Dataset mit den Sicherheitsdaten zu New York City in Azure Open Datasets verwenden.
ms.service: open-datasets
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 04/16/2021
ms.openlocfilehash: d8efbd7469ed92b4c323ed3d94315ec8f0f4dc5c
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112038740"
---
# <a name="new-york-city-safety-data"></a>Sicherheitsdaten zu New York City

Alle 311-Dienstanforderungen in New York City von 2010 bis heute.

[!INCLUDE [Open Dataset usage notice](../../includes/open-datasets-usage-note.md)]


## <a name="volume-and-retention"></a>Volume und Aufbewahrung

Dieses Dataset wird im Parquet-Format gespeichert. Es wird täglich aktualisiert und enthält für den Zeitraum bis 2019 insgesamt etwa 12 Millionen Zeilen (500 MB).

Dieses Dataset enthält historische Datensätze, die von 2010 bis heute gesammelt wurden. Verwenden Sie Parametereinstellungen im SDK, um Daten innerhalb eines bestimmten Zeitbereichs abzurufen.

## <a name="storage-location"></a>Speicherort

Dieses Dataset wird in der Azure-Region „USA, Osten“ gespeichert. Aus Gründen der Affinität wird die Zuweisung von Computeressourcen in der Region „USA, Osten“ empfohlen.

## <a name="additional-information"></a>Zusätzliche Informationen

Dieses Dataset stammt von den Behörden von New York City. Weitere Informationen finden Sie auf der [Website der Stadt New York](https://data.cityofnewyork.us/Social-Services/311-Service-Requests-from-2010-to-Present/erm2-nwe9). Sehen Sie sich die [Geschäftsbedingungen für dieses Dataset](https://www1.nyc.gov/home/terms-of-use.page) an.

## <a name="columns"></a>Spalten

| Name | Datentyp | Eindeutig | Beispielwerte | BESCHREIBUNG |
|-|-|-|-|-|
| address | Zeichenfolge | 1\.536.593 | 655 EAST 230 STREET 78-15 PARSONS BOULEVARD | Vom Absender angegebene Hausnummer der Adresse, an der sich der Vorfall ereignet hat. |
| category | Zeichenfolge | 446 | Lärm: HEIZUNG/HEISSWASSER | Dies ist die erste Ebene einer Hierarchie, die das Thema des Vorfalls oder der Bedingung (Art der Beschwerde) identifiziert. Sie kann eine entsprechende Unterkategorie (Deskriptor) haben oder allein stehen. |
| dataSubtype | Zeichenfolge | 1 | 311_All | „311_All“ |
| dataType | Zeichenfolge | 1 | Sicherheit | “Safety” |
| dateTime | timestamp | 17.332.609 | 2013-01-24 00:00:00 2015-01-08 00:00:00 | Erstellungsdatum der Serviceanfrage. |
| latitude | double | 1\.513.691 | 40,89187241649303 40,72195913199264 | Geografischer Breitegrad des Standorts, an dem der Vorfall stattgefunden hat. |
| longitude | double | 1\.513.713 | –73,86016845296459 –73,80969682426189 | Geografischer Längengrad des Standorts, an dem sich der Vorfall ereignet hat. |
| status | Zeichenfolge | 13 | Geschlossen Ausstehend | Der Dienstanforderungsstatus wurde gesendet. |
| subcategory | Zeichenfolge | 1\.716 | Laute Musik/Party GESAMTES GEBÄUDE | Diese Unterkategorie ist der entsprechenden Kategorie zugeordnet (Art der Beschwerde) und umfasst weitere Details des Vorfalls oder Zustands. Die Werte sind abhängig von der Art der Beschwerde und sind bei Serviceanträgen nicht immer erforderlich. |

## <a name="preview"></a>Vorschau

| dataType | dataSubtype | dateTime | category | subcategory | status | address | latitude | longitude | source | extendedProperties |
|-|-|-|-|-|-|-|-|-|-|-|
| Sicherheit | 311_All | 4/25/2021 2:05:05 AM | Lärm: Straße/Gehweg | Laute Musik/Party | Vorgang wird ausgeführt | 2766 BATH AVENUE | 40,5906129741766 | –73,9847949011337 | NULL |  |
| Sicherheit | 311_All | 4/25/2021 2:04:33 AM | Lärm: Geschäftsgebäude | Laute Musik/Party | Vorgang wird ausgeführt | 1033 WEBSTER AVENUE | 40,8285784533256 | –73,9117746958432 | NULL |  |
| Sicherheit | 311_All | 4/25/2021 2:04:27 AM | Lärm: Wohngebiet | Laute Musik/Party | Vorgang wird ausgeführt | 620 WEST 141 STREET | 40,8241726554395 | –73,9530069547366 | NULL |  |
| Sicherheit | 311_All | 4/25/2021 2:04:04 AM | Lärm: Wohngebiet | Laute Musik/Party | Vorgang wird ausgeführt | 1647 64 STREET | 40,6218907202382 | –73,9931125332078 | NULL |  |
| Sicherheit | 311_All | 4/25/2021 2:04:01 AM | Lärm: Wohngebiet | Laute Musik/Party | Vorgang wird ausgeführt | 30 LENOX AVENUE | 40,7991622274945 | –73,9517496365803 | NULL |  |
| Sicherheit | 311_All | 4/25/2021 2:03:40 AM | Illegales Parken | Parken in zweiter Reihe Verkehr blockiert | Vorgang wird ausgeführt | 304 WEST 148 STREET | 40,8248229687124 | –73,940696262361 | NULL |  |
| Sicherheit | 311_All | 4/25/2021 2:03:31 AM | Lärm: Straße/Gehweg | Laute Musik/Party | Vorgang wird ausgeführt | ADEE AVENUE | 40,8708386263454 | –73,8382363208686 | NULL |  |
| Sicherheit | 311_All | 4/25/2021 2:03:18 AM | Lärm: Wohngebiet | Laute Musik/Party | Vorgang wird ausgeführt | 340 EVERGREEN AVENUE | 40,6947512704197 | –73,9248330229197 | NULL |  |
| Sicherheit | 311_All | 4/25/2021 2:03:13 AM | Lärm: Wohngebiet | Hämmern/Stampfen | Vorgang wird ausgeführt | 25 REMSEN STREET | 40,6948938116483 | –73,9973494607802 | NULL |  |

## <a name="data-access"></a>Datenzugriff

### <a name="azure-notebooks"></a>Azure Notebooks

# <a name="azureml-opendatasets"></a>[azureml-opendatasets](#tab/azureml-opendatasets)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azureml-opendatasets&registryId=city_safety_sanfrancisco -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azureml-opendatasets&registryId=city_safety_sanfrancisco)** .

```
# This is a package in preview.
from azureml.opendatasets import SanFranciscoSafety

from datetime import datetime
from dateutil import parser


end_date = parser.parse('2016-01-01')
start_date = parser.parse('2015-05-01')
safety = SanFranciscoSafety(start_date=start_date, end_date=end_date)
safety = safety.to_pandas_dataframe()
```

```
safety.info()
```

<!-- nbend -->

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=city_safety_sanfrancisco -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=city_safety_sanfrancisco)** .

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
folder_name = "Safety/Release/city=SanFrancisco"
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

---

### <a name="azure-databricks"></a>Azure Databricks

# <a name="azureml-opendatasets"></a>[azureml-opendatasets](#tab/azureml-opendatasets)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=azureml-opendatasets&registryId=city_safety_sanfrancisco -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=azureml-opendatasets&registryId=city_safety_sanfrancisco)** .

```
# This is a package in preview.
# You need to pip install azureml-opendatasets in Databricks cluster. https://docs.microsoft.com/en-us/azure/data-explorer/connect-from-databricks#install-the-python-library-on-your-azure-databricks-cluster
from azureml.opendatasets import SanFranciscoSafety

from datetime import datetime
from dateutil import parser


end_date = parser.parse('2016-01-01')
start_date = parser.parse('2015-05-01')
safety = SanFranciscoSafety(start_date=start_date, end_date=end_date)
safety = safety.to_spark_dataframe()
```

```
display(safety.limit(5))
```

<!-- nbend -->

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

# <a name="pyspark"></a>[pyspark](#tab/pyspark)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=pyspark&registryId=city_safety_sanfrancisco -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=pyspark&registryId=city_safety_sanfrancisco)** .

```python
# Azure storage access info
blob_account_name = "azureopendatastorage"
blob_container_name = "citydatacontainer"
blob_relative_path = "Safety/Release/city=SanFrancisco"
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

---

### <a name="azure-synapse"></a>Azure Synapse

# <a name="azureml-opendatasets"></a>[azureml-opendatasets](#tab/azureml-opendatasets)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=azureml-opendatasets&registryId=city_safety_sanfrancisco -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=azureml-opendatasets&registryId=city_safety_sanfrancisco)** .

```python
# This is a package in preview.
from azureml.opendatasets import SanFranciscoSafety

from datetime import datetime
from dateutil import parser


end_date = parser.parse('2016-01-01')
start_date = parser.parse('2015-05-01')
safety = SanFranciscoSafety(start_date=start_date, end_date=end_date)
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

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=pyspark&registryId=city_safety_sanfrancisco -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=pyspark&registryId=city_safety_sanfrancisco)** .

```python
# Azure storage access info
blob_account_name = "azureopendatastorage"
blob_container_name = "citydatacontainer"
blob_relative_path = "Safety/Release/city=SanFrancisco"
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

---

## <a name="examples"></a>Beispiele

- Sehen Sie sich das Beispiel [City Safety Analytics](https://github.com/scottcounts/CitySafety) auf GitHub an.


## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich mit den restlichen Datasets im [Open Datasets-Katalog](dataset-catalog.md) vertraut.