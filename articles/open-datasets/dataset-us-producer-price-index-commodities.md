---
title: US-Erzeugerpreisindex – Güter
titleSuffix: Azure Open Datasets
description: Erfahren Sie, wie Sie das Dataset für „US-Erzeugerpreisindex – Güter“ in Azure Open Datasets verwenden.
ms.service: open-datasets
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 04/16/2021
ms.openlocfilehash: 224a5d0278237d5a7d87ee8e3c9adaedb71f193a
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112038914"
---
# <a name="us-producer-price-index---commodities"></a>US-Erzeugerpreisindex – Güter

Der Erzeugerpreisindex (EPI) ist ein Maß für die durchschnittliche zeitliche Veränderung der Verkaufspreise, die inländische Erzeuger für ihre Leistungen erzielen. Die im EPI enthaltenen Preise stammen aus der ersten kommerziellen Transaktion für die erfassten Produkte und Dienstleistungen.

[!INCLUDE [Open Dataset usage notice](../../includes/open-datasets-usage-note.md)]

Jeden Monat werden etwa 10.000 EPIs für einzelne Produkte und Produktgruppen veröffentlicht. EPIs stehen für die Leistung fast aller Industrien in den warenproduzierenden Sektoren der US-Wirtschaft (Bergbau, Produktion, Landwirtschaft, Fischerei und Forstwirtschaft) zur Verfügung sowie für Erdgas, Elektrizität, Baugewerbe und Güter, die mit denen der produzierenden Industrie konkurrieren, wie Abfälle und Ausschussmaterialien. Das EPI-Programm deckt etwa 72 Prozent der Leistung des Dienstleistungssektors ab, gemessen am Umsatz der Wirtschaftszählung von 2007. Die Daten umfassen Branchen in den folgenden Sektoren: Groß- und Einzelhandel; Transport und Lagerung; Informationsbereich; Finanzen und Versicherungen; Immobilienvermittlung, -vermietung und -Leasing; professionelle, wissenschaftliche und technische Dienstleistungen; Verwaltungs-, Unterstützungs- und Entsorgungsdienstleistungen; Gesundheitswesen und Sozialhilfe sowie Beherbergung.

[README](https://download.bls.gov/pub/time.series/wp/wp.txt) mit detaillierten Informationen zu diesem Dataset finden Sie am [Speicherort des ursprünglichen Datasets](https://download.bls.gov/pub/time.series/wp/). Es sind zusätzliche Informationen in den [FAQs](https://www.bls.gov/ppi/ppifaq.htm) verfügbar.

Dieses Dataset wird aus den vom [US Bureau of Labor Statistics (BLS)](https://www.bls.gov/) veröffentlichten [Daten der Erzeugerpreisindizes](https://www.bls.gov/ppi/) erzeugt. Lesen Sie die [Informationen zu Verknüpfungen und Copyright](https://www.bls.gov/bls/linksite.htm) und [wichtige Hinweise zur Website](https://www.bls.gov/bls/website-policies.htm), um mehr über die Bestimmungen für die Nutzung dieses Datasets zu erfahren.

## <a name="storage-location"></a>Speicherort

Dieses Dataset wird in der Azure-Region „USA, Osten“ gespeichert. Aus Gründen der Affinität wird die Zuweisung von Computeressourcen in der Region „USA, Osten“ empfohlen.

## <a name="related-datasets"></a>Zugehörige Datasets

- [US-Verbraucherpreisindex](dataset-us-consumer-price-index.md)
- [US-Erzeugerpreisindex – Industrie](dataset-us-producer-price-index-industry.md)

## <a name="columns"></a>Spalten

| Name | Datentyp | Eindeutig | Beispielwerte | BESCHREIBUNG |
|-|-|-|-|-|
| footnote_codes | Zeichenfolge | 3 | nan P | Bezeichnet Fußnoten für die Datenreihe. Die meisten Werte sind NULL. Siehe https://download.bls.gov/pub/time.series/wp/wp.footnote. |
| group_code | Zeichenfolge | 56 | 02 01 | Code, der die wichtigsten Warengruppen nach Index angibt. Die Codes und Namen von Gruppen finden Sie unter https://download.bls.gov/pub/time.series/wp/wp.group. |
| group_name | Zeichenfolge | 56 | Verarbeitete Lebens- und Futtermittel Agrarerzeugnisse | Name der wichtigsten Warengruppen nach Index. Die Codes und Namen von Gruppen finden Sie unter https://download.bls.gov/pub/time.series/wp/wp.group. |
| item_code | Zeichenfolge | 2,949 | 1 11 | Identifiziert den Artikel, auf den die erfassten Daten zutreffen. Die Codes und Namen des Artikels finden Sie unter https://download.bls.gov/pub/time.series/wp/wp.item. |
| item_name | Zeichenfolge | 3,410 | Lagerhaltung, Lagerung und zugehörige Dienste Pkw-Vermietung | Vollständige Namen der Artikel. Die Codes und Namen des Artikels finden Sie unter https://download.bls.gov/pub/time.series/wp/wp.item. |
| period (Zeitraum) | Zeichenfolge | 13 | M06 M07 | Identifiziert den Zeitraum, über den Daten beobachtet werden. Eine Liste von Werten für den Zeitraum finden Sie unter https://download.bls.gov/pub/time.series/wp/wp.period. |
| saisonal | Zeichenfolge | 2 | U S A | Code, der angibt, ob die Daten saisonabhängig angepasst sind. S=Seasonally Adjusted (saisonabhängig angepasst); U=Unadjusted (nicht angepasst) |
| series_id | Zeichenfolge | 5,458 | WPU101 WPU091 | Code, der die spezifische Reihen angibt. Eine Zeitreihe umfasst Daten, die über einen längeren Zeitraum in konsistenten Zeitintervallen erfasst werden. Informationen zu Reihen wie Code, Name, Start- und Endjahr usw. finden Sie unter https://download.bls.gov/pub/time.series/wp/wp.series. |
| series_title | Zeichenfolge | 4,379 | EPI-Güterdaten für Bergbaudienstleistungen, nicht saisonabhängig angepasst, EPI-Güterdaten für Dienstleistungen im Bereich der Metallbehandlung, nicht saisonabhängig angepasst | Titel der spezifischen Reihe. Eine Zeitreihe umfasst Daten, die über einen längeren Zeitraum in konsistenten Zeitintervallen erfasst werden. Informationen zu Reihen wie ID, Name, Start- und Endjahr usw. finden Sie unter https://download.bls.gov/pub/time.series/wp/wp.series. |
| value | float | 6,788 | 100.0 99.0999984741211 | Preisindex für Artikel. |
| year | INT | 26 | 2018 2017 | Jahr, in dem die Überwachung stattgefunden hat. |

## <a name="preview"></a>Vorschau

| item_code | group_code | series_id | year | period (Zeitraum) | value | footnote_codes | saisonal | series_title | group_name | item_name |
|-|-|-|-|-|-|-|-|-|-|-|
| 120922 | 05 | WPU05120922 | 2008 | M06 | 100 | nan | U | EPI-Güterdaten für Brennstoffe und verwandte Produkte und Energie – Aufbereitete Steinkohle Untertagewerk, nur mechanisch zerkleinert/gesiebt/nach Größe sortiert, nicht saisonabhängig angepasst | Brennstoffe und verwandte Produkte und Energie | Aufbereitete Steinkohle Untertagewerk, nur mechanisch zerkleinert/gesiebt/nach Größe sortiert |
| 120922 | 05 | WPU05120922 | 2008 | M07 | 104.6 | nan | U | EPI-Güterdaten für Brennstoffe und verwandte Produkte und Energie – Aufbereitete Steinkohle Untertagewerk, nur mechanisch zerkleinert/gesiebt/nach Größe sortiert, nicht saisonabhängig angepasst | Brennstoffe und verwandte Produkte und Energie | Aufbereitete Steinkohle Untertagewerk, nur mechanisch zerkleinert/gesiebt/nach Größe sortiert |
| 120922 | 05 | WPU05120922 | 2008 | M08 | 104.4 | nan | U | EPI-Güterdaten für Brennstoffe und verwandte Produkte und Energie – Aufbereitete Steinkohle Untertagewerk, nur mechanisch zerkleinert/gesiebt/nach Größe sortiert, nicht saisonabhängig angepasst | Brennstoffe und verwandte Produkte und Energie | Aufbereitete Steinkohle Untertagewerk, nur mechanisch zerkleinert/gesiebt/nach Größe sortiert |
| 120922 | 05 | WPU05120922 | 2008 | M09 | 98.3 | nan | U | EPI-Güterdaten für Brennstoffe und verwandte Produkte und Energie – Aufbereitete Steinkohle Untertagewerk, nur mechanisch zerkleinert/gesiebt/nach Größe sortiert, nicht saisonabhängig angepasst | Brennstoffe und verwandte Produkte und Energie | Aufbereitete Steinkohle Untertagewerk, nur mechanisch zerkleinert/gesiebt/nach Größe sortiert |
| 120922 | 05 | WPU05120922 | 2008 | M10 | 101,5 | nan | U | EPI-Güterdaten für Brennstoffe und verwandte Produkte und Energie – Aufbereitete Steinkohle Untertagewerk, nur mechanisch zerkleinert/gesiebt/nach Größe sortiert, nicht saisonabhängig angepasst | Brennstoffe und verwandte Produkte und Energie | Aufbereitete Steinkohle Untertagewerk, nur mechanisch zerkleinert/gesiebt/nach Größe sortiert |
| 120922 | 05 | WPU05120922 | 2008 | M11 | 95.2 | nan | U | EPI-Güterdaten für Brennstoffe und verwandte Produkte und Energie – Aufbereitete Steinkohle Untertagewerk, nur mechanisch zerkleinert/gesiebt/nach Größe sortiert, nicht saisonabhängig angepasst | Brennstoffe und verwandte Produkte und Energie | Aufbereitete Steinkohle Untertagewerk, nur mechanisch zerkleinert/gesiebt/nach Größe sortiert |

## <a name="data-access"></a>Datenzugriff

### <a name="azure-notebooks"></a>Azure Notebooks

# <a name="azureml-opendatasets"></a>[azureml-opendatasets](#tab/azureml-opendatasets)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azureml-opendatasets&registryId=us-producer-price-index-commodities -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azureml-opendatasets&registryId=us-producer-price-index-commodities)** .

```python
# This is a package in preview.
from azureml.opendatasets import UsLaborPPICommodity

labor = UsLaborPPICommodity()
labor_df = labor.to_pandas_dataframe()
```

```python
labor_df.info()
```

<!-- nbend -->


# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=us-producer-price-index-commodities -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=us-producer-price-index-commodities)** .

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
folder_name = "ppi_commodity/"
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

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=azureml-opendatasets&registryId=us-producer-price-index-commodities -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=azureml-opendatasets&registryId=us-producer-price-index-commodities)** .

```python
# This is a package in preview.
from azureml.opendatasets import UsLaborPPICommodity

labor = UsLaborPPICommodity()
labor_df = labor.to_spark_dataframe()
```

```python
display(labor_df.limit(5))
```

<!-- nbend -->
 
# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

# <a name="pyspark"></a>[pyspark](#tab/pyspark)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=pyspark&registryId=us-producer-price-index-commodities -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=pyspark&registryId=us-producer-price-index-commodities)** .

```python
# Azure storage access info
blob_account_name = "azureopendatastorage"
blob_container_name = "laborstatisticscontainer"
blob_relative_path = "ppi_commodity/"
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

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=pyspark&registryId=us-producer-price-index-commodities -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=pyspark&registryId=us-producer-price-index-commodities)** .

```python
# Azure storage access info
blob_account_name = "azureopendatastorage"
blob_container_name = "laborstatisticscontainer"
blob_relative_path = "ppi_commodity/"
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