---
title: Sicherheitsdaten zu Boston
titleSuffix: Azure Open Datasets
description: Es wird beschrieben, wie Sie das Dataset mit den Sicherheitsdaten zu Boston in Azure Open Datasets verwenden.
ms.service: open-datasets
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 04/16/2021
ms.openlocfilehash: 1c9a4917f1b5d2b719a247d111b62897f8063bc9
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112038881"
---
# <a name="boston-safety-data"></a>Sicherheitsdaten zu Boston

Anrufe an 311, die der Stadt Boston gemeldet wurden.

Über den folgenden Link erhalten Sie weitere Informationen zu [BOS:311](https://www.cityofboston.gov/311/).

[!INCLUDE [Open Dataset usage notice](../../includes/open-datasets-usage-note.md)]

## <a name="volume-and-retention"></a>Volume und Aufbewahrung

Dieses Dataset wird im Parquet-Format gespeichert. Es wird täglich aktualisiert und enthält für den Zeitraum ab 2019 insgesamt etwa 100.000 Zeilen (10 MB).

Dieses Dataset enthält Datensätze, die von 2011 bis heute gesammelt wurden. Verwenden Sie Parametereinstellungen im SDK, um Daten innerhalb eines bestimmten Zeitbereichs abzurufen.

## <a name="storage-location"></a>Speicherort

Dieses Dataset wird in der Azure-Region „USA, Osten“ gespeichert. Aus Gründen der Affinität wird die Zuweisung von Computeressourcen in der Region „USA, Osten“ empfohlen.

## <a name="additional-information"></a>Zusätzliche Informationen

Dieses Dataset wurde von der Stadtverwaltung von Boston bezogen. Weitere Informationen finden Sie auf der [Dataset-Site von Boston](https://data.boston.gov/dataset/311-service-requests). Informationen zur Datasetlizenzierung finden Sie unter [Open Data Commons Public Domain Dedication and License (ODC PDDL)](http://opendefinition.org/licenses/odc-pddl/).

## <a name="columns"></a>Spalten

| Name | Datentyp | Eindeutig | Beispielwerte | BESCHREIBUNG |
|-|-|-|-|-|
| address | Zeichenfolge | 140,612 | \" \" 1 City Hall Plz Boston MA 02108 | Der Standort. |
| category | Zeichenfolge | 54 | Straßenreinigung/Entsorgung | Grund für die Serviceanfrage. |
| dataSubtype | Zeichenfolge | 1 | 311_All | „311_All“ |
| dataType | Zeichenfolge | 1 | Sicherheit | “Safety” |
| dateTime | timestamp | 1,529,075 | 2015-07-23 10:51:00 2015-07-23 10:47:00 | Öffnen des Datums und der Uhrzeit der Dienstanforderung |
| latitude | double | 1.622 | 42.3594 42.3603 | Dies ist der Wert für den Breitengrad. Die Linien des Breitengrads sind parallel zum Äquator. |
| longitude | double | 1,806 | -71.0587 -71.0583 | Dies ist der Wert für den Längengrad. Die Linien der Längengrade verlaufen senkrecht zu den Linien der Breitengrade und durchlaufen beide Pole. |
| source | Zeichenfolge | 7 | Einzelne Meldung über die Citizens Connect-App | Ursprüngliche Quelle des Falls. |
| status | Zeichenfolge | 2 | Geschlossen Offen | Fallstatus. |
| subcategory | Zeichenfolge | 209 | Aufforderungen zur Durchsetzung von Parkvorschriften für die Straßenreinigung | Typ der Serviceanfrage. |

## <a name="preview"></a>Vorschau

| ataType | dataSubtype | dateTime | category | subcategory | status | address | latitude | longitude | source | extendedProperties |
|-|-|-|-|-|-|-|-|-|-|-|
| Sicherheit | 311_All | 4/27/2021 11:45:49 PM | Durchsetzung & Verlassene Fahrzeuge | Durchsetzung von Parkvorschriften | Öffnen | 51 Gardner St Allston MA 02134 | 42.3535 | -71.1285 | Citizens Connect-App |  |
| Sicherheit | 311_All | 4/27/2021 11:43:43 PM | Entsorgung | Verpasster Müll/Wertstoff/Gartenabfall/Sperrmüll | Öffnen | 4 Putnam Pl Roxbury MA 02119 | 42.3298 | -71.0883 | Self-Service |  |
| Sicherheit | 311_All | 4/27/2021 11:37:19 PM | Durchsetzung & Verlassene Fahrzeuge | Durchsetzung von Parkvorschriften | Öffnen | 36 Raven St Dorchester MA 02125 | 42.3177 | -71.0546 | Citizens Connect-App |  |
| Sicherheit | 311_All | 4/27/2021 11:30:00 PM | Entsorgung | Verpasster Müll/Wertstoff/Gartenabfall/Sperrmüll | Öffnen | 58 Bicknell St Dorchester MA 02121 | 42.2984 | -71.0834 | Einzelne Meldung |  |
| Sicherheit | 311_All | 4/27/2021 11:10:20 PM | Durchsetzung & Verlassene Fahrzeuge | Durchsetzung von Parkvorschriften | Öffnen | 2000 Commonwealth Ave Brighton MA 02135 | 42.3394 | -71.1585 | Citizens Connect-App |  |
| Sicherheit | 311_All | 4/27/2021 11:06:00 PM | Lärmbelästigung | Laute Partys/Musik/Personen | Öffnen | INTERSECTION of Lewis St & North St Boston MA | 42.3594 | -71.0587 | Einzelne Meldung |  |
| Sicherheit | 311_All | 4/27/2021 11:05:00 PM | Durchsetzung & Verlassene Fahrzeuge | Durchsetzung von Parkvorschriften | Öffnen | 1 Nassau St Boston MA 02111 | 42.3486 | -71.0629 | Einzelne Meldung |  |
| Sicherheit | 311_All | 4/27/2021 11:00:55 PM | Codedurchsetzung | Schlechter Immobilienzustand | Öffnen | 17 Mercer St South Boston MA 02127 | 42.3332 | -71.0492 | Citizens Connect-App |  |

## <a name="data-access"></a>Datenzugriff

### <a name="azure-notebooks"></a>Azure Notebooks

# <a name="azureml-opendatasets"></a>[azureml-opendatasets](#tab/azureml-opendatasets)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azureml-opendatasets&registryId=city_safety_boston -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azureml-opendatasets&registryId=city_safety_boston)** .

```
# This is a package in preview.
from azureml.opendatasets import BostonSafety

from datetime import datetime
from dateutil import parser

end_date = parser.parse('2016-01-01')
start_date = parser.parse('2015-05-01')
safety = BostonSafety(start_date=start_date, end_date=end_date)
safety = safety.to_pandas_dataframe()
```

```
safety.info()
```

<!-- nbend -->


# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=city_safety_boston -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=city_safety_boston)** .

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
folder_name = "Safety/Release/city=Boston"
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

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=azureml-opendatasets&registryId=city_safety_boston -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=azureml-opendatasets&registryId=city_safety_boston)** .

```
# This is a package in preview.
# You need to pip install azureml-opendatasets in Databricks cluster. https://docs.microsoft.com/en-us/azure/data-explorer/connect-from-databricks#install-the-python-library-on-your-azure-databricks-cluster
from azureml.opendatasets import BostonSafety

from datetime import datetime
from dateutil import parser


end_date = parser.parse('2016-01-01')
start_date = parser.parse('2015-05-01')
safety = BostonSafety(start_date=start_date, end_date=end_date)
safety = safety.to_spark_dataframe()
```

```
display(safety)
```

<!-- nbend -->

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

# <a name="pyspark"></a>[pyspark](#tab/pyspark)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=pyspark&registryId=city_safety_boston -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=pyspark&registryId=city_safety_boston)** .

```python
# Azure storage access info
blob_account_name = "azureopendatastorage"
blob_container_name = "citydatacontainer"
blob_relative_path = "Safety/Release/city=Boston"
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

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=azureml-opendatasets&registryId=city_safety_boston -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=azureml-opendatasets&registryId=city_safety_boston)** .

```python
from azureml.opendatasets import BostonSafety

from datetime import datetime
from dateutil import parser


end_date = parser.parse('2016-01-01')
start_date = parser.parse('2015-05-01')
safety = BostonSafety(start_date=start_date, end_date=end_date)
safety = safety.to_spark_dataframe()
```

```python
display(safety)
```

<!-- nbend -->

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

# <a name="pyspark"></a>[pyspark](#tab/pyspark)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=pyspark&registryId=city_safety_boston -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=pyspark&registryId=city_safety_boston)** .

```python
# Azure storage access info
blob_account_name = "azureopendatastorage"
blob_container_name = "citydatacontainer"
blob_relative_path = "Safety/Release/city=Boston"
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