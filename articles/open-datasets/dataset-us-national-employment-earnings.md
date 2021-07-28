---
title: Nationale Beschäftigungszeit und Einnahmen in den USA
titleSuffix: Azure Open Datasets
description: Erfahren Sie, wie Sie das Dataset „Nationale Beschäftigungszeit und Einnahmen in den USA“ in Azure Open Datasets verwenden.
ms.service: open-datasets
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 04/16/2021
ms.openlocfilehash: ffbd29a3aa19b999c9202a0cea36cafee6a4c5f4
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112038776"
---
# <a name="us-national-employment-hours-and-earnings"></a>Nationale Beschäftigungszeit und Einnahmen in den USA

Das CES-Programm (Current Employment Statistics) produziert detaillierte Branchenschätzungen für nicht landwirtschaftliche Beschäftigung, Arbeitszeit und Einnahmen von Arbeitnehmern in den USA, die Gehaltsabrechnungen erhalten.

[!INCLUDE [Open Dataset usage notice](../../includes/open-datasets-usage-note.md)]

Die [INFODATEI](https://download.bls.gov/pub/time.series/ce/ce.txt) mit der Datei für ausführliche Informationen zu diesem Dataset ist am ursprünglichen [Speicherort des Datasets](https://download.bls.gov/pub/time.series/ce/) verfügbar.

Dieses Dataset wird aus den vom [US Bureau of Labor Statistics (BLS)](https://www.bls.gov/) veröffentlichten [Daten der aktuellen (nationalen) Beschäftigungsstatistik](https://www.bls.gov/ces/) erzeugt. Lesen Sie die [Informationen zu Verknüpfungen und Copyright](https://www.bls.gov/bls/linksite.htm) und [wichtige Hinweise zur Website](https://www.bls.gov/bls/website-policies.htm), um mehr über die Bestimmungen für die Nutzung dieses Datasets zu erfahren.

## <a name="storage-location"></a>Speicherort

Dieses Dataset wird in der Azure-Region „USA, Osten“ gespeichert. Aus Gründen der Affinität wird die Zuweisung von Computeressourcen in der Region „USA, Osten“ empfohlen.

## <a name="related-datasets"></a>Zugehörige Datasets

- [Beschäftigungszeit und Einnahmen in den USA nach Bundesstaat](dataset-us-state-employment-earnings.md)
- [Statistik zur Arbeitslosigkeit in den USA nach lokaler Umgebung](dataset-us-local-unemployment.md)
- [Statistik zu Arbeitskräften in den USA](dataset-us-labor-force.md)

## <a name="columns"></a>Spalten

| Name | Datentyp | Eindeutig | Beispielwerte | BESCHREIBUNG |
|-|-|-|-|-|
| data_type_code | Zeichenfolge | 37 | 1 10 | Siehe https://download.bls.gov/pub/time.series/ce/ce.datatype. |
| data_type_text | Zeichenfolge | 37 | ALLE MITARBEITER, TAUSENDE WEIBLICHE MITARBEITER, TAUSENDE | Siehe https://download.bls.gov/pub/time.series/ce/ce.datatype. |
| footnote_codes | Zeichenfolge | 2 | nan P |  |
| industry_code | Zeichenfolge | 902 | 30000000 32000000 | Unterschiedliche abgedeckte Branchen. Siehe https://download.bls.gov/pub/time.series/ce/ce.industry |
| industry_name | Zeichenfolge | 895 | Verbrauchsgüter Langlebige Güter | Unterschiedliche abgedeckte Branchen. Siehe https://download.bls.gov/pub/time.series/ce/ce.industry |
| period (Zeitraum) | Zeichenfolge | 13 | M03 M06 | Siehe https://download.bls.gov/pub/time.series/ce/ce.period. |
| saisonal | Zeichenfolge | 2 | U S A |  |
| series_id | Zeichenfolge | 26.021 | CEU3100000008 CEU9091912001 | Unterschiedliche im Dataset verfügbare Datenserientypen. Siehe https://download.bls.gov/pub/time.series/ce/ce.series |
| series_title | Zeichenfolge | 25.685 | Alle Beschäftigten, Tausende, Verbrauchsgüter, nicht saisonabhängig angepasst Alle Mitarbeiter, Tausende, Verbrauchsgüter, nicht saisonabhängig angepasst | Titel der unterschiedlichen im Dataset verfügbare Datenserientypen. Siehe https://download.bls.gov/pub/time.series/ce/ce.series |
| supersector_code | Zeichenfolge | 22 | 31 60 | Klassifizierung für Branche oder Sektor auf höherer Ebene. Siehe https://download.bls.gov/pub/time.series/ce/ce.supersector. |
| supersector_name | Zeichenfolge | 22 | Langlebige Güter Professionelle und geschäftliche Dienste | Klassifizierung für Branche oder Sektor auf höherer Ebene. Siehe https://download.bls.gov/pub/time.series/ce/ce.supersector. |
| value | float | 572.372 | 38,5 38,400001525878906 |  |
| year | INT | 81 | 2017 2012 |  |

## <a name="preview"></a>Vorschau

| data_type_code | industry_code | supersector_code | series_id | year | period (Zeitraum) | value | footnote_codes | saisonal | series_title | supersector_name | industry_name | data_type_text |
|-|-|-|-|-|-|-|-|-|-|-|-|-|
| 26 | 5000000 | 5 | CES0500000026 | 1939 | M04 | 52 | nan | E | Alle Mitarbeiter, Veränderung im 3-Monats-Durchschnitt, saisonabhängig angepasst, Tausende, gesamt privat, saisonabhängig angepasst | Gesamt privat | Gesamt privat | ALLE MITARBEITER, VERÄNDERUNG IM 3-MONATS-DURCHSCHNITT, SAISONABHÄNGIG ANGEPASST, TAUSENDE |
| 26 | 5000000 | 5 | CES0500000026 | 1939 | M05 | 65 | nan | E | Alle Mitarbeiter, Veränderung im 3-Monats-Durchschnitt, saisonabhängig angepasst, Tausende, gesamt privat, saisonabhängig angepasst | Gesamt privat | Gesamt privat | ALLE MITARBEITER, VERÄNDERUNG IM 3-MONATS-DURCHSCHNITT, SAISONABHÄNGIG ANGEPASST, TAUSENDE |
| 26 | 5000000 | 5 | CES0500000026 | 1939 | M06 | 74 | nan | E | Alle Mitarbeiter, Veränderung im 3-Monats-Durchschnitt, saisonabhängig angepasst, Tausende, gesamt privat, saisonabhängig angepasst | Gesamt privat | Gesamt privat | ALLE MITARBEITER, VERÄNDERUNG IM 3-MONATS-DURCHSCHNITT, SAISONABHÄNGIG ANGEPASST, TAUSENDE |
| 26 | 5000000 | 5 | CES0500000026 | 1939 | M07 | 103 | nan | E | Alle Mitarbeiter, Veränderung im 3-Monats-Durchschnitt, saisonabhängig angepasst, Tausende, gesamt privat, saisonabhängig angepasst | Gesamt privat | Gesamt privat | ALLE MITARBEITER, VERÄNDERUNG IM 3-MONATS-DURCHSCHNITT, SAISONABHÄNGIG ANGEPASST, TAUSENDE |
| 26 | 5000000 | 5 | CES0500000026 | 1939 | M08 | 108 | nan | E | Alle Mitarbeiter, Veränderung im 3-Monats-Durchschnitt, saisonabhängig angepasst, Tausende, gesamt privat, saisonabhängig angepasst | Gesamt privat | Gesamt privat | ALLE MITARBEITER, VERÄNDERUNG IM 3-MONATS-DURCHSCHNITT, SAISONABHÄNGIG ANGEPASST, TAUSENDE |
| 26 | 5000000 | 5 | CES0500000026 | 1939 | M09 | 152 | nan | E | Alle Mitarbeiter, Veränderung im 3-Monats-Durchschnitt, saisonabhängig angepasst, Tausende, gesamt privat, saisonabhängig angepasst | Gesamt privat | Gesamt privat | ALLE MITARBEITER, VERÄNDERUNG IM 3-MONATS-DURCHSCHNITT, SAISONABHÄNGIG ANGEPASST, TAUSENDE |
| 26 | 5000000 | 5 | CES0500000026 | 1939 | M10 | 307 | nan | E | Alle Mitarbeiter, Veränderung im 3-Monats-Durchschnitt, saisonabhängig angepasst, Tausende, gesamt privat, saisonabhängig angepasst | Gesamt privat | Gesamt privat | ALLE MITARBEITER, VERÄNDERUNG IM 3-MONATS-DURCHSCHNITT, SAISONABHÄNGIG ANGEPASST, TAUSENDE |
| 26 | 5000000 | 5 | CES0500000026 | 1939 | M11 | 248 | nan | E | Alle Mitarbeiter, Veränderung im 3-Monats-Durchschnitt, saisonabhängig angepasst, Tausende, gesamt privat, saisonabhängig angepasst | Gesamt privat | Gesamt privat | ALLE MITARBEITER, VERÄNDERUNG IM 3-MONATS-DURCHSCHNITT, SAISONABHÄNGIG ANGEPASST, TAUSENDE |

## <a name="data-access"></a>Datenzugriff

### <a name="azure-notebooks"></a>Azure Notebooks

# <a name="azureml-opendatasets"></a>[azureml-opendatasets](#tab/azureml-opendatasets)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azureml-opendatasets&registryId=us-employment-hours-earnings-national -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azureml-opendatasets&registryId=us-employment-hours-earnings-national)** .

```python
# This is a package in preview.
from azureml.opendatasets import UsLaborEHENational

usLaborEHENational = UsLaborEHENational()
usLaborEHENational_df = usLaborEHENational.to_pandas_dataframe()
```

```python
usLaborEHENational_df.info()
```

<!-- nbend -->

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=us-employment-hours-earnings-national -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=us-employment-hours-earnings-national)** .

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
folder_name = "ehe_national/"
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

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=azureml-opendatasets&registryId=us-employment-hours-earnings-national -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=azureml-opendatasets&registryId=us-employment-hours-earnings-national)** .

```python
# This is a package in preview.
from azureml.opendatasets import UsLaborEHENational

usLaborEHENational = UsLaborEHENational()
usLaborEHENational_df = usLaborEHENational.to_spark_dataframe()
```

```python
display(usLaborEHENational_df.limit(5))
```

<!-- nbend -->
 
# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

# <a name="pyspark"></a>[pyspark](#tab/pyspark)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=pyspark&registryId=us-employment-hours-earnings-national -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=pyspark&registryId=us-employment-hours-earnings-national)** .

```python
# Azure storage access info
blob_account_name = "azureopendatastorage"
blob_container_name = "laborstatisticscontainer"
blob_relative_path = "ehe_national/"
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

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=pyspark&registryId=us-employment-hours-earnings-national -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=pyspark&registryId=us-employment-hours-earnings-national)** .

```python
# Azure storage access info
blob_account_name = "azureopendatastorage"
blob_container_name = "laborstatisticscontainer"
blob_relative_path = "ehe_national/"
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