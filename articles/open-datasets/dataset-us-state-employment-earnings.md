---
title: Beschäftigungszeit und Einnahmen in den USA nach Bundesstaat
titleSuffix: Azure Open Datasets
description: Erfahren Sie, wie Sie das Dataset „Beschäftigungszeit und Einnahmen in den USA nach Bundesstaat“ in Azure Open Datasets verwenden.
ms.service: open-datasets
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 04/16/2021
ms.openlocfilehash: a1cb79e704eeae13e8794cad7409e0b945889d22
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112038911"
---
# <a name="us-state-employment-hours-and-earnings"></a>Beschäftigungszeit und Einnahmen in den USA nach Bundesstaat

Das CES-Programm (Current Employment Statistics) produziert detaillierte Branchenschätzungen für nicht landwirtschaftliche Beschäftigung, Arbeitszeit und Einnahmen von Arbeitnehmern in den USA, die Gehaltsabrechnungen erhalten.

[!INCLUDE [Open Dataset usage notice](../../includes/open-datasets-usage-note.md)]

Dieses Dataset wird aus den von der [US Bureau of Labor Statistics (BLS)](https://www.bls.gov/) veröffentlichten Daten zur Beschäftigung ([State and Metro Area Employment](https://www.bls.gov/sae/)), den Arbeitszeiten und den Gehältern ([Hours & Earnings data](https://www.bls.gov/sae/)) in den Bundesstaaten und Regionen erstellt. Lesen Sie die [Informationen zu Verknüpfungen und Copyright](https://www.bls.gov/bls/linksite.htm) und [wichtige Hinweise zur Website](https://www.bls.gov/bls/website-policies.htm), um mehr über die Bestimmungen für die Nutzung dieses Datasets zu erfahren.

## <a name="storage-location"></a>Speicherort

Dieses Dataset wird in der Azure-Region „USA, Osten“ gespeichert. Aus Gründen der Affinität wird die Zuweisung von Computeressourcen in der Region „USA, Osten“ empfohlen.

## <a name="related-datasets"></a>Zugehörige Datasets

- [Nationale Beschäftigungszeit und Einnahmen in den USA](dataset-us-national-employment-earnings.md)
- [Statistik zur Arbeitslosigkeit in den USA nach lokaler Umgebung](dataset-us-local-unemployment.md)
- [Statistik zu Arbeitskräften in den USA](dataset-us-labor-force.md)

## <a name="columns"></a>Spalten

| Name | Datentyp | Eindeutig | Beispielwerte |
|-|-|-|-|
| area_code | Zeichenfolge | 446 | 0 31084 |
| area_name | Zeichenfolge | 446 | Statewide Los Angeles-Long Beach-Glendale, CA Metropolitan Division |
| data_type_code | Zeichenfolge | 9 | 1 3 |
| data_type_text | Zeichenfolge | 9 | Alle Mitarbeiter, in Tausend durchschnittliche wöchentliche Arbeitsstunden aller Mitarbeiter |
| footnote_codes | Zeichenfolge | 3 | nan P |
| industry_code | Zeichenfolge | 343 | 0 5000000 |
| industry_name | Zeichenfolge | 343 | Gesamtanzahl nicht landwirtschaftlichen Lands Gesamt privat |
| period (Zeitraum) | Zeichenfolge | 13 | M04 M05 |
| saisonal | Zeichenfolge | 2 | U S A |
| series_id | Zeichenfolge | 23,853 | SMU12000000000000001 SMU36000000000000001 |
| state_code | Zeichenfolge | 53 | 6 48 |
| state_name | Zeichenfolge | 53 | Kalifornien Texas |
| supersector_code | Zeichenfolge | 22 | 90 60 |
| supersector_name | Zeichenfolge | 22 | Staatliche Fachdienstleistungen und Unternehmensdienstleistungen |
| value | float | 132,565 | 0.30000001192092896 0.10000000149011612 |
| year | INT | 81 | 2014 2018 |

## <a name="preview"></a>Vorschau

| area_code | state_code | data_type_code | industry_code | supersector_code | series_id | year | period (Zeitraum) | value | footnote_codes | saisonal | supersector_name | industry_name | data_type_text | state_name | area_name |
|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|
| 13460 | 41 | 26 | 0 | 0 | SMS41134600000000026 | 1990 | M04 | 0.2 | nan | E | Gesamtanzahl nicht landwirtschaftlichen Lands | Gesamtanzahl nicht landwirtschaftlichen Lands | Alle Mitarbeiter, Veränderung im 3-Monats-Durchschnitt, In Tausend, saisonabhängig angepasst | Oregon | Bend-Redmond, OR |
| 13460 | 41 | 26 | 0 | 0 | SMS41134600000000026 | 1990 | M05 | 0.2 | nan | E | Gesamtanzahl nicht landwirtschaftlichen Lands | Gesamtanzahl nicht landwirtschaftlichen Lands | Alle Mitarbeiter, Veränderung im 3-Monats-Durchschnitt, In Tausend, saisonabhängig angepasst | Oregon | Bend-Redmond, OR |
| 13460 | 41 | 26 | 0 | 0 | SMS41134600000000026 | 1990 | M06 | 0.1 | nan | E | Gesamtanzahl nicht landwirtschaftlichen Lands | Gesamtanzahl nicht landwirtschaftlichen Lands | Alle Mitarbeiter, Veränderung im 3-Monats-Durchschnitt, In Tausend, saisonabhängig angepasst | Oregon | Bend-Redmond, OR |
| 13460 | 41 | 26 | 0 | 0 | SMS41134600000000026 | 1990 | M07 | 0.1 | nan | E | Gesamtanzahl nicht landwirtschaftlichen Lands | Gesamtanzahl nicht landwirtschaftlichen Lands | Alle Mitarbeiter, Veränderung im 3-Monats-Durchschnitt, In Tausend, saisonabhängig angepasst | Oregon | Bend-Redmond, OR |
| 13460 | 41 | 26 | 0 | 0 | SMS41134600000000026 | 1990 | M08 | 0.2 | nan | E | Gesamtanzahl nicht landwirtschaftlichen Lands | Gesamtanzahl nicht landwirtschaftlichen Lands | Alle Mitarbeiter, Veränderung im 3-Monats-Durchschnitt, In Tausend, saisonabhängig angepasst | Oregon | Bend-Redmond, OR |
| 13460 | 41 | 26 | 0 | 0 | SMS41134600000000026 | 1990 | M09 | 0.2 | nan | E | Gesamtanzahl nicht landwirtschaftlichen Lands | Gesamtanzahl nicht landwirtschaftlichen Lands | Alle Mitarbeiter, Veränderung im 3-Monats-Durchschnitt, In Tausend, saisonabhängig angepasst | Oregon | Bend-Redmond, OR |
| 13460 | 41 | 26 | 0 | 0 | SMS41134600000000026 | 1990 | M10 | 0.1 | nan | E | Gesamtanzahl nicht landwirtschaftlichen Lands | Gesamtanzahl nicht landwirtschaftlichen Lands | Alle Mitarbeiter, Veränderung im 3-Monats-Durchschnitt, In Tausend, saisonabhängig angepasst | Oregon | Bend-Redmond, OR |

## <a name="data-access"></a>Datenzugriff

### <a name="azure-notebooks"></a>Azure Notebooks

# <a name="azureml-opendatasets"></a>[azureml-opendatasets](#tab/azureml-opendatasets)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azureml-opendatasets&registryId=us-employment-hours-earnings-state -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azureml-opendatasets&registryId=us-employment-hours-earnings-state)** .

```python
# This is a package in preview.
from azureml.opendatasets import UsLaborEHEState

usLaborEHEState = UsLaborEHEState()
usLaborEHEState_df = usLaborEHEState.to_pandas_dataframe()
```

```python
usLaborEHEState_df.info()
```

<!-- nbend -->

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=us-employment-hours-earnings-state -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=us-employment-hours-earnings-state)** .

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
folder_name = "ehe_state/"
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

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=azureml-opendatasets&registryId=us-employment-hours-earnings-state -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=azureml-opendatasets&registryId=us-employment-hours-earnings-state)** .

```python
# This is a package in preview.
from azureml.opendatasets import UsLaborEHEState

usLaborEHEState = UsLaborEHEState()
usLaborEHEState_df = usLaborEHEState.to_spark_dataframe()
```

```python
display(usLaborEHEState_df.limit(5))
```

<!-- nbend -->
 
# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

# <a name="pyspark"></a>[pyspark](#tab/pyspark)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=pyspark&registryId=us-employment-hours-earnings-state -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=pyspark&registryId=us-employment-hours-earnings-state)** .

```python
# Azure storage access info
blob_account_name = "azureopendatastorage"
blob_container_name = "laborstatisticscontainer"
blob_relative_path = "ehe_state/"
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

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=pyspark&registryId=us-employment-hours-earnings-state -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=pyspark&registryId=us-employment-hours-earnings-state)** .

```python
# Azure storage access info
blob_account_name = "azureopendatastorage"
blob_container_name = "laborstatisticscontainer"
blob_relative_path = "ehe_state/"
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