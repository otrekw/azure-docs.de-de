---
title: US-Bevölkerung nach Postleitzahl
titleSuffix: Azure Open Datasets
description: Hier erfahren Sie, wie Sie das Dataset „US-Bevölkerung nach Postleitzahl“ in Azure Open Datasets verwenden.
ms.service: open-datasets
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 04/16/2021
ms.openlocfilehash: 2dcf696b996edabbd34af163fd293fdefae8601e
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112038773"
---
# <a name="us-population-by-zip-code"></a>US-Bevölkerung nach Postleitzahl

US-Bevölkerung nach Geschlecht und ethnischer Gruppe für alle US-Postleitzahlen gemäß der alle zehn Jahre stattfindenden Volkszählung von 2000 und 2010.

Dieses Dataset stammt aus den [Dataset-APIs zur alle zehn Jahre stattfindenden Volkszählung](https://www.census.gov/data/developers/data-sets/decennial-census.html) des statistischen Bundesamts der USA. Überprüfen Sie die [Nutzungsbedingungen](https://www.census.gov/data/developers/about/terms-of-service.html) sowie die [Richtlinien und Hinweise](https://www.census.gov/about/policies.html) zu den Nutzungsbedingungen für die Verwendung dieses Datasets.

[!INCLUDE [Open Dataset usage notice](../../includes/open-datasets-usage-note.md)]

## <a name="volume-and-retention"></a>Volume und Aufbewahrung

Dieses Dataset wird im Parquet-Format gespeichert und enthält Daten für das Jahr 2010.

## <a name="storage-location"></a>Speicherort
Dieses Dataset wird in der Azure-Region „USA, Osten“ gespeichert. Aus Gründen der Affinität wird die Zuweisung von Computeressourcen in der Region „USA, Osten“ empfohlen.

## <a name="related-datasets"></a>Zugehörige Datasets

- [US-Bevölkerung nach County](dataset-us-population-county.md)

## <a name="columns"></a>Spalten

| Name | Datentyp | Eindeutig | Beispielwerte | BESCHREIBUNG |
|-|-|-|-|-|
| decennialTime | Zeichenfolge | 1 | 2010 | Jahr, in dem die Volkszählung stattfand, z. B. 2010, 2000. |
| maxAge | INT | 23 | 54 21 | Max. Spanne für die Altersgruppe. Bei NULL werden alle Altersgruppen berücksichtigt oder es gibt kein Höchstalter (z. B. Alter > 85). |
| minAge | INT | 23 | 45 30 | Mindestspanne für die Altersgruppe. Bei NULL werden alle Altersgruppen berücksichtigt. |
| Auffüllung | INT | 29.274 | 1 2 | Gesamtbevölkerung für dieses Segment. |
| race | Zeichenfolge | 8 | NUR ANDERE ETHNISCHE GRUPPE, NUR SCHWARZ ODER AFROAMERIKANISCH | Kategorie der ethnischen Gruppe in den Zensusdaten. Wenn der Wert NULL ist, gelten die Werte für alle Gruppen. |
| sex | Zeichenfolge | 3 | Weiblich, Männlich | Männlich oder weiblich. Bei NULL werden beide Geschlechter berücksichtigt. |
| year | INT | 1 | 2010 | Jahr (Integerwert) der alle zehn Jahre stattfindenden Volkszählung. |
| zipCode | Zeichenfolge | 33.120 | 39218 87420 | 5-Digit ZIP Code Tabulation Area (ZCTA5). |

## <a name="preview"></a>Vorschau

| decennialTime | zipCode | Auffüllung | race | sex | minAge | maxAge | year |
|-|-|-|-|-|-|-|-|
| 2010 | 77477 | 265 | NUR WEISS | Female | 15 | 17 | 2010 |
| 2010 | 77477 | 107 | NUR ANDERE ETHNISCHE GRUPPE | Female | 15 | 17 | 2010 |
| 2010 | 77477 | 12 | NUR ANDERE ETHNISCHE GRUPPE | Female | 65 | 66 | 2010 |
| 2010 | 77477 | 101 | NUR ASIATISCH | Female | 60 | 61 | 2010 |
| 2010 | 77477 | 221 | NUR ASIATISCH | Male | 10 | 14 | 2010 |
| 2010 | 77478 | 256 | NUR WEISS | Female | 15 | 17 | 2010 |
| 2010 | 77478 | 17 | NUR ANDERE ETHNISCHE GRUPPE | Female | 15 | 17 | 2010 |
| 2010 | 77478 | 3 | NUR ANDERE ETHNISCHE GRUPPE | Female | 65 | 66 | 2010 |

## <a name="data-access"></a>Datenzugriff

### <a name="azure-notebooks"></a>Azure Notebooks

# <a name="azureml-opendatasets"></a>[azureml-opendatasets](#tab/azureml-opendatasets)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azureml-opendatasets&registryId=us-decennial-census-zip -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azureml-opendatasets&registryId=us-decennial-census-zip)** .

```python
# This is a package in preview.
from azureml.opendatasets import UsPopulationZip

population = UsPopulationZip()
population_df = population.to_pandas_dataframe()
```

```python
population_df.info()
```

<!-- nbend -->

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=us-decennial-census-zip -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=us-decennial-census-zip)** .

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
container_name = "censusdatacontainer"
folder_name = "release/us_population_zip/"
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

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=azureml-opendatasets&registryId=us-decennial-census-zip -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=azureml-opendatasets&registryId=us-decennial-census-zip)** .

```python
# This is a package in preview.
from azureml.opendatasets import UsPopulationZip

population = UsPopulationZip()
population_df = population.to_spark_dataframe()
```

```python
display(population_df.limit(5))
```

<!-- nbend -->

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

# <a name="pyspark"></a>[pyspark](#tab/pyspark)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=pyspark&registryId=us-decennial-census-zip -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=pyspark&registryId=us-decennial-census-zip)** .

```python
# Azure storage access info
blob_account_name = "azureopendatastorage"
blob_container_name = "censusdatacontainer"
blob_relative_path = "release/us_population_zip/"
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

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=azureml-opendatasets&registryId=us-decennial-census-zip -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=azureml-opendatasets&registryId=us-decennial-census-zip)** .

```python
# This is a package in preview.
from azureml.opendatasets import UsPopulationZip

population = UsPopulationZip()
population_df = population.to_spark_dataframe()
```

```python
# Display top 5 rows
display(population_df.limit(5))
```

<!-- nbend -->

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

# <a name="pyspark"></a>[pyspark](#tab/pyspark)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=pyspark&registryId=us-decennial-census-zip -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=pyspark&registryId=us-decennial-census-zip)** .

```python
# Azure storage access info
blob_account_name = "azureopendatastorage"
blob_container_name = "censusdatacontainer"
blob_relative_path = "release/us_population_zip/"
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

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich mit den restlichen Datasets im [Open Datasets-Katalog](dataset-catalog.md) vertraut.