---
title: US-Verbraucherpreisindex
titleSuffix: Azure Open Datasets
description: Erfahren Sie, wie Sie das Dataset „US-Verbraucherpreisindex“ in Azure Open Datasets verwenden.
ms.service: open-datasets
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 04/16/2021
ms.openlocfilehash: fffe6b1521f3f9ab4973a64f03ef52e4a9019658
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112038785"
---
# <a name="us-consumer-price-index"></a>US-Verbraucherpreisindex

Der Verbraucherpreisindex zeigt die durchschnittliche Veränderung der Preise, die von Konsumenten in Städten für bestimmte Waren und Dienstleistungen bezahlt werden, im Laufe der Zeit.

[!INCLUDE [Open Dataset usage notice](../../includes/open-datasets-usage-note.md)]

[README](https://download.bls.gov/pub/time.series/cu/cu.txt) mit detaillierten Informationen zu diesem Dataset finden Sie am [Speicherort des ursprünglichen Datasets](https://download.bls.gov/pub/time.series/cu/).

Dieses Dataset wird aus den vom [US Bureau of Labor Statistics (BLS)](https://www.bls.gov/) veröffentlichten [Daten der Verbraucherpreisindizes](https://www.bls.gov/cpi/) erzeugt. Lesen Sie die Geschäftsbedingungen unter den [Links und Copyrightinformationen](https://www.bls.gov/bls/linksite.htm) sowie unter den [wichtigen Websitehinweisen](https://www.bls.gov/bls/website-policies.htm).

## <a name="storage-location"></a>Speicherort

Dieses Dataset wird in der Azure-Region „USA, Osten“ gespeichert. Wir empfehlen Ihnen, aus Affinitätsgründen Computeressourcen in „USA, Osten“ anzuordnen.

## <a name="related-datasets"></a>Zugehörige Datasets

- [US-Erzeugerpreisindex – Industrie](dataset-us-producer-price-index-industry.md)
- [US-Erzeugerpreisindex – Güter](dataset-us-producer-price-index-commodities.md)

## <a name="columns"></a>Spalten

| Name | Datentyp | Eindeutig | Beispielwerte | BESCHREIBUNG |
|-|-|-|-|-|
| area_code | Zeichenfolge | 70 | 0000 0300 | Eindeutiger Code, der zum Identifizieren eines spezifischen geografischen Gebiets verwendet wird. Vollständige Gebietscodes finden Sie unter http://download.bls.gov/pub/time.series/cu/cu.area. |
| area_name | Zeichenfolge | 69 | Stadtdurchschnitt in den USA, Süden | Name des spezifischen geografischen Gebiets. Die Namen und Codes des Gebiets finden Sie unter https://download.bls.gov/pub/time.series/cu/cu.area. |
| footnote_codes | Zeichenfolge | 3 | nan U | Bezeichnet Fußnote für die Datenreihen. Die meisten Werte sind NULL. |
| item_code | Zeichenfolge | 515 | SA0E SAF11 | Identifiziert den Artikel, auf den die erfassten Daten zutreffen. Alle Namen und Codes des Artikels finden Sie unter https://download.bls.gov/pub/time.series/cu/cu.item. |
| item_name | Zeichenfolge | 515 | Energie, Lebensmittel zu Hause | Vollständige Namen der Artikel. Die Namen und Codes des Artikels finden Sie unter https://download.bls.gov/pub/time.series/cu/cu.txt. |
| period (Zeitraum) | Zeichenfolge | 16 | S01 S02 | Identifiziert den Zeitraum, über den Daten beobachtet werden. Format: M01-M13 oder S01-S03 (M = monatlich, M13 = Jahresdurchschnitt, S = halbjährlich). Beispiel: M06 = Juni. Die Namen und Codes der Zeiträume finden Sie unter https://download.bls.gov/pub/time.series/cu/cu.period. |
| periodicity_code | Zeichenfolge | 3 | R S | Häufigkeit der Datenerfassung. S=Semi-Annual (halbjährlich); R=Regular (regelmäßig). |
| saisonal | Zeichenfolge | 1\.043 | U S A | Code, der identifiziert, ob Daten nach Saison angepasst werden. S=Seasonally Adjusted (saisonabhängig angepasst); U=Unadjusted (nicht angepasst). |
| series_id | Zeichenfolge | 16.683 | CWURS400SA0E CWUR0100SA0E | Code, der die spezifische Reihen angibt. Eine Zeitreihe bezieht sich auf einen Datensatz, der über einen längeren Zeitraum hinweg über konsistente Zeitintervalle (z. B. monatlich, vierteljährlich, halbjährlich, jährlich) beobachtet wird. Die BLS-Zeitreihendaten werden in der Regel in monatlichen Intervallen erstellt und stellen Daten dar, die von einem bestimmten Konsumgut in einer bestimmten geografischen Region, dessen Preis monatlich erfasst wird, bis hin zu einer Kategorie an Arbeitnehmern in einer bestimmten Branche reichen, deren Beschäftigungsrate monatlich erfasst wird usw. Weitere Informationen finden Sie unter https://download.bls.gov/pub/time.series/cu/cu.txt. |
| series_title | Zeichenfolge | 8\.336 | Alkoholische Getränke im Städtedurchschnitt, alle städtischen Verbraucher, nicht saisonabhängig angepasster Transport in Los Angeles-Long Beach-Anaheim, CA, alle städtischen Verbraucher, nicht saisonabhängig angepasst | Reihenname der zugehörigen Reihen-ID (series_id). Die Reihen-IDs und -Namen finden Sie unter https://download.bls.gov/pub/time.series/cu/cu.series. |
| value | float | 310.603 | 100,0 101,0999984741211 | Preisindex für Artikel. |
| year | INT | 25 | 2018 2017 | Jahr, in dem die Überwachung stattgefunden hat. |

## <a name="preview"></a>Vorschau

| area_code | item_code | series_id | year | period (Zeitraum) | value | footnote_codes | saisonal | periodicity_code | series_title | item_name | area_name |
|-|-|-|-|-|-|-|-|-|-|-|-|
| S49E | SEHF01 | CUURS49ESEHF01 | 2017 | M12 | 279,974 | nan | U | R | Elektrizität in San Diego-Carlsbad, CA, alle städtischen Verbraucher, nicht saisonabhängig angepasst | Elektrizität | San Diego-Carlsbad, CA |
| S49E | SEHF01 | CUURS49ESEHF01 | 2017 | M12 | 279,974 | nan | U | R | Elektrizität in San Diego-Carlsbad, CA, alle städtischen Verbraucher, nicht saisonabhängig angepasst | Elektrizität | San Diego-Carlsbad, CA |
| S49E | SEHF01 | CUURS49ESEHF01 | 2017 | M12 | 279,974 | nan | U | R | Elektrizität in San Diego-Carlsbad, CA, alle städtischen Verbraucher, nicht saisonabhängig angepasst | Elektrizität | San Diego-Carlsbad, CA |
| S49E | SEHF01 | CUURS49ESEHF01 | 2017 | M12 | 279,974 | nan | U | R | Elektrizität in San Diego-Carlsbad, CA, alle städtischen Verbraucher, nicht saisonabhängig angepasst | Elektrizität | San Diego-Carlsbad, CA |
| S49E | SEHF01 | CUURS49ESEHF01 | 2017 | M12 | 279,974 | nan | U | R | Elektrizität in San Diego-Carlsbad, CA, alle städtischen Verbraucher, nicht saisonabhängig angepasst | Elektrizität | San Diego-Carlsbad, CA |
| S49E | SEHF01 | CUURS49ESEHF01 | 2017 | M12 | 279,974 | nan | U | R | Elektrizität in San Diego-Carlsbad, CA, alle städtischen Verbraucher, nicht saisonabhängig angepasst | Elektrizität | San Diego-Carlsbad, CA |

## <a name="data-access"></a>Datenzugriff

### <a name="azure-notebooks"></a>Azure Notebooks

# <a name="azureml-opendatasets"></a>[azureml-opendatasets](#tab/azureml-opendatasets)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azureml-opendatasets&registryId=us-consumer-price-index -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azureml-opendatasets&registryId=us-consumer-price-index)** .

```python
# This is a package in preview.
from azureml.opendatasets import UsLaborCPI

usLaborCPI = UsLaborCPI()
usLaborCPI_df = usLaborCPI.to_pandas_dataframe()
```

```python
usLaborCPI_df.info()
```

<!-- nbend -->

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=us-consumer-price-index -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=us-consumer-price-index)** .

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
folder_name = "cpi/"
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

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=azureml-opendatasets&registryId=us-consumer-price-index -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=azureml-opendatasets&registryId=us-consumer-price-index)** .

```python
# This is a package in preview.
from azureml.opendatasets import UsLaborCPI

usLaborCPI = UsLaborCPI()
usLaborCPI_df = usLaborCPI.to_spark_dataframe()
```

```python
display(usLaborCPI_df.limit(5))
```

<!-- nbend -->
 
# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

# <a name="pyspark"></a>[pyspark](#tab/pyspark)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=pyspark&registryId=us-consumer-price-index -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=pyspark&registryId=us-consumer-price-index)** .

```python
# Azure storage access info
blob_account_name = "azureopendatastorage"
blob_container_name = "laborstatisticscontainer"
blob_relative_path = "cpi/"
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

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=pyspark&registryId=us-consumer-price-index -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=pyspark&registryId=us-consumer-price-index)** .

```python
# Azure storage access info
blob_account_name = "azureopendatastorage"
blob_container_name = "laborstatisticscontainer"
blob_relative_path = "cpi/"
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