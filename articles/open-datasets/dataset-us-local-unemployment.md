---
title: Statistik zur Arbeitslosigkeit in den USA nach lokaler Umgebung
titleSuffix: Azure Open Datasets
description: Es wird beschrieben, wie Sie das Dataset „Statistik zur Arbeitslosigkeit in den USA nach lokaler Umgebung“ in Azure Open Datasets verwenden.
ms.service: open-datasets
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 04/16/2021
ms.openlocfilehash: 13af967282d1f9be8f289612936ab7d33a9fce11
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112038779"
---
# <a name="us-local-area-unemployment-statistics"></a>Statistik zur Arbeitslosigkeit in den USA nach lokaler Umgebung

Das Programm der „Local Area Unemployment Statistics“ (regionale Arbeitslosenstatistik) produziert monatliche und jährliche Daten zu Beschäftigung, Arbeitslosigkeit und Arbeitskräften für Volkszählungsregionen und -abteilungen, Staaten, Landkreise, Metropolregionen und zahlreiche Städte in den Vereinigten Staaten.

[!INCLUDE [Open Dataset usage notice](../../includes/open-datasets-usage-note.md)]

[README](https://download.bls.gov/pub/time.series/la/la.txt) mit detaillierten Informationen zu diesem Dataset finden Sie am [Speicherort des ursprünglichen Datasets](https://download.bls.gov/pub/time.series/la/).

Dieses Dataset wird aus den vom [US Bureau of Labor Statistics (BLS)](https://www.bls.gov/) veröffentlichten [Local Area Unemployment Statistics data](https://www.bls.gov/lau/) (Daten der regionalen Arbeitslosenstatistik) erzeugt. Lesen Sie die [Informationen zu Verknüpfungen und Copyright](https://www.bls.gov/bls/linksite.htm) und [wichtige Hinweise zur Website](https://www.bls.gov/bls/website-policies.htm), um mehr über die Bestimmungen für die Nutzung dieses Datasets zu erfahren.

## <a name="storage-location"></a>Speicherort

Dieses Dataset wird in der Azure-Region „USA, Osten“ gespeichert. Aus Gründen der Affinität wird die Zuweisung von Computeressourcen in der Region „USA, Osten“ empfohlen.

## <a name="related-datasets"></a>Zugehörige Datasets

- [Nationale Beschäftigungszeit und Einnahmen in den USA](dataset-us-national-employment-earnings.md)
- [Beschäftigungszeit und Einnahmen in den USA nach Bundesstaat](dataset-us-state-employment-earnings.md)
- [Statistik zu Arbeitskräften in den USA](dataset-us-labor-force.md)

## <a name="columns"></a>Spalten

| Name | Datentyp | Eindeutig | Werte (Beispiel) | BESCHREIBUNG |
|-|-|-|-|-|
| area_code | Zeichenfolge | 8,290 | ST3400000000000 RD8200000000000 | Code, der den geografischen Bereich identifiziert. Siehe https://download.bls.gov/pub/time.series/la/la.area. |
| area_text | Zeichenfolge | 8,238 | District of Columbia Oregon | Name des geografischen Bereichs. Siehe https://download.bls.gov/pub/time.series/la/la.area |
| area_type_code | Zeichenfolge | 14 | F G | Eindeutig Code, der den Bereichstyp definiert. Siehe https://download.bls.gov/pub/time.series/la/la.area_type |
| areatype_text | Zeichenfolge | 14 | Counties and equivalents Cities and towns above 25,000 population | Name des Bereichstyps. |
| footnote_codes | Zeichenfolge | 5 | nan P |  |
| measure_code | Zeichenfolge | 4 | 5 4 | Gemessenes codeidentifizierendes Element. 03: Arbeitslosenquote, 04: Arbeitslosigkeit, 05: Erwerbstätigkeit, 06: Arbeitskraft. Siehe https://download.bls.gov/pub/time.series/la/la.measure. |
| measure_text | Zeichenfolge | 4 | employment unemployment | Name des gemessenen Elements. Siehe https://download.bls.gov/pub/time.series/la/la.measure |
| period (Zeitraum) | Zeichenfolge | 13 | M07 M05 | Identifiziert den Zeitraum, in der Regel den Monat. Siehe https://download.bls.gov/pub/time.series/la/la.period |
| saisonal | Zeichenfolge | 2 | U S A |  |
| series_id | Zeichenfolge | 33,476 | LASST160000000000006 LASST200000000000006 | Code, der die Serie identifiziert. Eine vollständige Liste der Serie finden Sie unter https://download.bls.gov/pub/time.series/la/la.series. |
| series_title | Zeichenfolge | 33,268 | Employment: Philadelphia County/city, PA (U) Labor Force: Manassas city, VA (U) | Titel, der die Serie identifiziert. Eine vollständige Liste der Serie finden Sie unter https://download.bls.gov/pub/time.series/la/la.series. |
| srd_code | Zeichenfolge | 53 | 48 23 | Staat, Region oder Bereichscode. |
| srd_text | Zeichenfolge | 53 | Texas Maine |  |
| value | float | 600,099 | 4.0 5.0 | Wert für bestimmtes Measure. |
| year | INT | 44 | 2009 2008 |  |

## <a name="preview"></a>Vorschau

| area_code | area_type_code | srd_code | measure_code | series_id | year | period (Zeitraum) | value | footnote_codes | saisonal | series_title | measure_text | srd_text | areatype_text | area_text |
|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|
| CA3653200000000 | E | 36 | 3 | LAUCA365320000000003 | 2000 | M01 | 4,7 | nan | U | Unemployment Rate: Syracuse-Auburn, NY Combined Statistical Area (U) | unemployment rate | New York | Combined areas | Syracuse-Auburn, NY Combined Statistical Area |
| CA3653200000000 | E | 36 | 3 | LAUCA365320000000003 | 2000 | M02 | 4,7 | nan | U | Unemployment Rate: Syracuse-Auburn, NY Combined Statistical Area (U) | unemployment rate | New York | Combined areas | Syracuse-Auburn, NY Combined Statistical Area |
| CA3653200000000 | E | 36 | 3 | LAUCA365320000000003 | 2000 | M03 | 4,2 | nan | U | Unemployment Rate: Syracuse-Auburn, NY Combined Statistical Area (U) | unemployment rate | New York | Combined areas | Syracuse-Auburn, NY Combined Statistical Area |
| CA3653200000000 | E | 36 | 3 | LAUCA365320000000003 | 2000 | M04 | 3.6 | nan | U | Unemployment Rate: Syracuse-Auburn, NY Combined Statistical Area (U) | unemployment rate | New York | Combined areas | Syracuse-Auburn, NY Combined Statistical Area |
| CA3653200000000 | E | 36 | 3 | LAUCA365320000000003 | 2000 | M05 | 3.6 | nan | U | Unemployment Rate: Syracuse-Auburn, NY Combined Statistical Area (U) | unemployment rate | New York | Combined areas | Syracuse-Auburn, NY Combined Statistical Area |
| CA3653200000000 | E | 36 | 3 | LAUCA365320000000003 | 2000 | M06 | 3.6 | nan | U | Unemployment Rate: Syracuse-Auburn, NY Combined Statistical Area (U) | unemployment rate | New York | Combined areas | Syracuse-Auburn, NY Combined Statistical Area |
| CA3653200000000 | E | 36 | 3 | LAUCA365320000000003 | 2000 | M07 | 3.6 | nan | U | Unemployment Rate: Syracuse-Auburn, NY Combined Statistical Area (U) | unemployment rate | New York | Combined areas | Syracuse-Auburn, NY Combined Statistical Area |

## <a name="data-access"></a>Datenzugriff

### <a name="azure-notebooks"></a>Azure Notebooks

# <a name="azureml-opendatasets"></a>[azureml-opendatasets](#tab/azureml-opendatasets)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azureml-opendatasets&registryId=us-local-area-unemployment-statistics -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azureml-opendatasets&registryId=us-local-area-unemployment-statistics)** .

```python
# This is a package in preview.
from azureml.opendatasets import UsLaborLAUS

usLaborLAUS = UsLaborLAUS()
usLaborLAUS_df = usLaborLAUS.to_pandas_dataframe()
```

```python
usLaborLAUS_df.info()
```

<!-- nbend -->

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=us-local-area-unemployment-statistics -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=us-local-area-unemployment-statistics)** .

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
container_name = "laborstatisticscontainer"
folder_name = "laus/"
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

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=azureml-opendatasets&registryId=us-local-area-unemployment-statistics -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=azureml-opendatasets&registryId=us-local-area-unemployment-statistics)** .

```python
# This is a package in preview.
from azureml.opendatasets import UsLaborLAUS

usLaborLAUS = UsLaborLAUS()
usLaborLAUS_df = usLaborLAUS.to_spark_dataframe()
```

```python
display(usLaborLAUS_df.limit(5))
```

<!-- nbend -->

 
# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

# <a name="pyspark"></a>[pyspark](#tab/pyspark)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=pyspark&registryId=us-local-area-unemployment-statistics -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=pyspark&registryId=us-local-area-unemployment-statistics)** .

```python
# Azure storage access info
blob_account_name = "azureopendatastorage"
blob_container_name = "laborstatisticscontainer"
blob_relative_path = "laus/"
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

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

# <a name="pyspark"></a>[pyspark](#tab/pyspark)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=pyspark&registryId=us-local-area-unemployment-statistics -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=pyspark&registryId=us-local-area-unemployment-statistics)** .

```python
# Azure storage access info
blob_account_name = "azureopendatastorage"
blob_container_name = "laborstatisticscontainer"
blob_relative_path = "laus/"
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