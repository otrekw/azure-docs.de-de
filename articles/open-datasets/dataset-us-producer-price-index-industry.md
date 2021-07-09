---
title: US-Erzeugerpreisindex – Branche
titleSuffix: Azure Open Datasets
description: Erfahren Sie, wie Sie das Dataset für „US-Erzeugerpreisindex – Branche“ in Azure Open Datasets verwenden.
ms.service: open-datasets
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 04/16/2021
ms.openlocfilehash: cd0662fc4990e6a2baf0a29ba2e96cec50509a05
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112038770"
---
# <a name="us-consumer-price-index-industry"></a>US-Verbraucherpreisindex – Branche

Der Erzeugerpreisindex (EPI) ist ein Maß für die durchschnittliche zeitliche Veränderung der Verkaufspreise, die inländische Erzeuger für ihre Leistungen erzielen. Die im EPI enthaltenen Preise stammen aus der ersten kommerziellen Transaktion für die erfassten Produkte und Dienstleistungen.

[!INCLUDE [Open Dataset usage notice](../../includes/open-datasets-usage-note.md)]

Die aktuellen Reihenindexe der Überarbeitung des Erzeugerpreisindexes spiegeln die Preisbewegungen für die Nettoleistung der Erzeuger wider, die nach dem North American Industry Classification System (NAICS) organisiert sind. Das PC-Dataset ist mit einer breiten Palette von NAICS-basierten wirtschaftlichen Zeitreihen kompatibel, einschließlich Produktivität, Produktion, Beschäftigung, Löhne und Gehälter.

Der EPI-Bereich umfasst die Leistung aller Branchen in den warenproduzierenden Sektoren der US-Wirtschaft (Bergbau, Produktion, Landwirtschaft, Fischerei und Forstwirtschaft) sowie Erdgas, Elektrizität, Baugewerbe und Güter, die mit denen der produzierenden Industrie konkurrieren, wie Abfälle und Ausschussmaterialien. Darüber hinaus umfasste das EPI-Programm ab Januar 2011 mehr als drei Viertel der Leistung des Dienstleistungssektors und veröffentlichte Daten für ausgewählte Branchen in den folgenden Sektoren: Groß- und Einzelhandel; Transport und Lagerung; Informationsbereich; Finanzen und Versicherungen; Immobilienvermittlung, -vermietung und -Leasing; professionelle, wissenschaftliche und technische Dienstleistungen; Verwaltungs-, Unterstützungs- und Entsorgungsdienstleistungen; Gesundheitswesen und Sozialhilfe sowie Beherbergung.

[README](https://download.bls.gov/pub/time.series/wp/wp.txt) mit detaillierten Informationen zu diesem Dataset finden Sie am [Speicherort des ursprünglichen Datasets](https://download.bls.gov/pub/time.series/wp/). Es sind zusätzliche Informationen in den [FAQs](https://www.bls.gov/ppi/ppifaq.htm) verfügbar.

Dieses Dataset wird aus den vom [US Bureau of Labor Statistics (BLS)](https://www.bls.gov/) veröffentlichten [Daten der Erzeugerpreisindizes](https://www.bls.gov/ppi/) erzeugt. Lesen Sie die [Informationen zu Verknüpfungen und Copyright](https://www.bls.gov/bls/linksite.htm) und [wichtige Hinweise zur Website](https://www.bls.gov/bls/website-policies.htm), um mehr über die Bestimmungen für die Nutzung dieses Datasets zu erfahren.

## <a name="storage-location"></a>Speicherort

Dieses Dataset wird in der Azure-Region „USA, Osten“ gespeichert. Aus Gründen der Affinität wird die Zuweisung von Computeressourcen in der Region „USA, Osten“ empfohlen.

## <a name="related-datasets"></a>Zugehörige Datasets

- [US-Verbraucherpreisindex](dataset-us-consumer-price-index.md)
- [US-Erzeugerpreisindex – Güter](dataset-us-producer-price-index-commodities.md)

## <a name="columns"></a>Spalten

| Name | Datentyp | Eindeutig | Beispielwerte | BESCHREIBUNG |
|-|-|-|-|-|
| footnote_codes | Zeichenfolge | 3 | nan P | Bezeichnet Fußnoten für die Datenreihe. Die meisten Werte sind NULL. Siehe https://download.bls.gov/pub/time.series/pc/pc.footnote. |
| industry_code | Zeichenfolge | 1\.064 | 221122 325412 | NAICS-Code für Industrie. Die Codes und Namen finden Sie unter https://download.bls.gov/pub/time.series/pc/pc.industry. |
| industry_name | Zeichenfolge | 842 | Stromverteilung Herstellung pharmazeutischer Präparate | Name, der dem Code für die Industrie entspricht. Die Codes und Namen finden Sie unter https://download.bls.gov/pub/time.series/pc/pc.industry. |
| period (Zeitraum) | Zeichenfolge | 13 | M06 M07 | Identifiziert den Zeitraum, über den Daten beobachtet werden. Eine vollständige Liste finden Sie unter https://download.bls.gov/pub/time.series/pc/pc.period. |
| product_code | Zeichenfolge | 4\.822 | 324121P 316--- | Code, der das Produkt identifiziert, auf das sich die Datenbeobachtung bezieht. Weitere Informationen zur Zuordnung von Branchencodes, Produktcodes und Produktnamen finden Sie unter https://download.bls.gov/pub/time.series/pc/pc.product. |
| product_name | Zeichenfolge | 3\.313 | Primäre Produkte Sekundäre Produkte | Name des Produkts, auf das sich die erfassten Daten beziehen. Weitere Informationen zur Zuordnung von Branchencodes, Produktcodes und Produktnamen finden Sie unter https://download.bls.gov/pub/time.series/pc/pc.product. |
| saisonal | Zeichenfolge | 1 | U | Code, der angibt, ob die Daten saisonabhängig angepasst sind. S=Seasonally Adjusted (saisonabhängig angepasst); U=Unadjusted (nicht angepasst) |
| series_id | Zeichenfolge | 4\.822 | PCU332323332323M PCU333415333415C | Code, der die spezifische Reihen angibt. Eine Zeitreihe umfasst Daten, die über einen längeren Zeitraum in konsistenten Zeitintervallen erfasst werden. Informationen zu Reihen wie Code, Name, Start- und Endjahr usw. finden Sie unter https://download.bls.gov/pub/time.series/pc/pc.series. |
| series_title | Zeichenfolge | 4\.588 | PPI-Branchendaten zur Stromverteilung – Westen - Süden - Mitte, nicht saisonabhängig angepasste PPI-Branchendaten für die Stromverteilung – Pazifik, nicht saisonabhängig angepasst |  |
| value | float | 7\.658 | 100,0 100,4000015258789 | Preisindex für Artikel. |
| year | INT | 22 | 2015 2017 | Jahr, in dem die Überwachung stattgefunden hat. |

## <a name="preview"></a>Vorschau

| product_code | industry_code | series_id | year | period (Zeitraum) | value | footnote_codes | saisonal | series_title | industry_name | product_name |
|-|-|-|-|-|-|-|-|-|-|-|
| 2123240 | 212324 | PCU2123242123240 | 1998 | M01 | 117 | nan | U | PPI-Branchendaten für den Abbau von Tonerde bestehend aus Kaolin – Tonerde bestehend aus Kaolin, nicht saisonabhängig angepasst | Abbau von Tonerde bestehend aus Kaolin | Tonerde bestehend aus Kaolin |
| 2123240 | 212324 | PCU2123242123240 | 1998 | M02 | 116,9 | nan | U | PPI-Branchendaten für den Abbau von Tonerde bestehend aus Kaolin – Tonerde bestehend aus Kaolin, nicht saisonabhängig angepasst | Abbau von Tonerde bestehend aus Kaolin | Tonerde bestehend aus Kaolin |
| 2123240 | 212324 | PCU2123242123240 | 1998 | M03 | 116,3 | nan | U | PPI-Branchendaten für den Abbau von Tonerde bestehend aus Kaolin – Tonerde bestehend aus Kaolin, nicht saisonabhängig angepasst | Abbau von Tonerde bestehend aus Kaolin | Tonerde bestehend aus Kaolin |
| 2123240 | 212324 | PCU2123242123240 | 1998 | M04 | 116 | nan | U | PPI-Branchendaten für den Abbau von Tonerde bestehend aus Kaolin – Tonerde bestehend aus Kaolin, nicht saisonabhängig angepasst | Abbau von Tonerde bestehend aus Kaolin | Tonerde bestehend aus Kaolin |
| 2123240 | 212324 | PCU2123242123240 | 1998 | M05 | 116,2 | nan | U | PPI-Branchendaten für den Abbau von Tonerde bestehend aus Kaolin – Tonerde bestehend aus Kaolin, nicht saisonabhängig angepasst | Abbau von Tonerde bestehend aus Kaolin | Tonerde bestehend aus Kaolin |
| 2123240 | 212324 | PCU2123242123240 | 1998 | M06 | 116,3 | nan | U | PPI-Branchendaten für den Abbau von Tonerde bestehend aus Kaolin – Tonerde bestehend aus Kaolin, nicht saisonabhängig angepasst | Abbau von Tonerde bestehend aus Kaolin | Tonerde bestehend aus Kaolin |
| 2123240 | 212324 | PCU2123242123240 | 1998 | M07 | 116,6 | nan | U | PPI-Branchendaten für den Abbau von Tonerde bestehend aus Kaolin – Tonerde bestehend aus Kaolin, nicht saisonabhängig angepasst | Abbau von Tonerde bestehend aus Kaolin | Tonerde bestehend aus Kaolin |

## <a name="data-access"></a>Datenzugriff

### <a name="azure-notebooks"></a>Azure Notebooks

# <a name="azureml-opendatasets"></a>[azureml-opendatasets](#tab/azureml-opendatasets)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azureml-opendatasets&registryId=us-producer-price-index-industry -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azureml-opendatasets&registryId=us-producer-price-index-industry)** .

```python
# This is a package in preview.
from azureml.opendatasets import UsLaborPPIIndustry

labor = UsLaborPPIIndustry()
labor_df = labor.to_pandas_dataframe()
```

```python
labor_df.info()
```

<!-- nbend -->

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=us-producer-price-index-industry -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=us-producer-price-index-industry)** .

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
folder_name = "ppi_industry/"
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

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=azureml-opendatasets&registryId=us-producer-price-index-industry -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=azureml-opendatasets&registryId=us-producer-price-index-industry)** .

```python
# This is a package in preview.
from azureml.opendatasets import UsLaborPPIIndustry

labor = UsLaborPPIIndustry()
labor_df = labor.to_spark_dataframe()
```

```python
display(labor_df.limit(5))
```

<!-- nbend -->
 
# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

# <a name="pyspark"></a>[pyspark](#tab/pyspark)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=pyspark&registryId=us-producer-price-index-industry -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=pyspark&registryId=us-producer-price-index-industry)** .

```python
# Azure storage access info
blob_account_name = "azureopendatastorage"
blob_container_name = "laborstatisticscontainer"
blob_relative_path = "ppi_industry/"
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

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=pyspark&registryId=us-producer-price-index-industry -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=pyspark&registryId=us-producer-price-index-industry)** .

```python
# Azure storage access info
blob_account_name = "azureopendatastorage"
blob_container_name = "laborstatisticscontainer"
blob_relative_path = "ppi_industry/"
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