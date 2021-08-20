---
title: Sicherheitsdaten zu San Francisco
titleSuffix: Azure Open Datasets
description: Es wird beschrieben, wie Sie das Dataset mit den Sicherheitsdaten zu San Francisco in Azure Open Datasets verwenden.
ms.service: open-datasets
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 04/16/2021
ms.openlocfilehash: 4502d51054f43b7b1d876443c12f67418ba84060
ms.sourcegitcommit: 7c44970b9caf9d26ab8174c75480f5b09ae7c3d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2021
ms.locfileid: "112982454"
---
# <a name="san-francisco-safety-data"></a>Sicherheitsdaten zu San Francisco

Anrufe bei der Feuerwehrdienststelle und 311-Fälle in San Francisco.

[!INCLUDE [Open Dataset usage notice](../../includes/open-datasets-usage-note.md)]

Anrufe bei Feuerwehrdienststellen umfassen alle Feuerwehreinsätze. Alle Datensätze enthalten die Rufnummer, Vorfallnummer, Adresse, Kennung der Einheit, Anruftyp und Disposition. Alle relevanten Zeitintervalle sind ebenfalls enthalten. Da dieses Dataset auf Reaktionen basiert und die meisten Anrufe mehrere Einheiten umfassten, bestehen mehrere Einträge für jede Rufnummer. Die Adressen umfassen eine Blocknummer, Kreuzung oder Telefonzelle und keine genaue Adresse.

311 der Fälle umfassen Fälle, die im Allgemeinen einem Ort oder Gegenstand zugeordnet sind (z. B. Parks, Straßen oder Gebäuden). Sie wurden nach dem 1. Juli 2008 erstellt. Fälle, die von einem Benutzer über seine eigenen Anforderungen protokolliert werden, werden ausgeschlossen. Beispiele hierfür sind Fragen zur Grund- oder Unternehmenssteuer, Genehmigungsanforderungen für den Parkplatz und so weiter. Weitere Informationen finden Sie unter dem[Program-Link](https://support.datasf.org/help/311-case-data-faq).
 
## <a name="volume-and-retention"></a>Volume und Aufbewahrung

Dieses Dataset wird im Parquet-Format gespeichert. Es wird täglich aktualisiert und enthält etwa 6 Millionen Zeilen (400 MB), die seit 2019 gesammelt wurden.

Dieses Dataset enthält historische Datensätze, die von 2015 bis heute gesammelt wurden. Verwenden Sie Parametereinstellungen im SDK, um Daten innerhalb eines bestimmten Zeitbereichs abzurufen.

## <a name="storage-location"></a>Speicherort

Dieses Dataset wird in der Azure-Region „USA, Osten“ gespeichert. Aus Gründen der Affinität wird die Zuweisung von Computeressourcen in der Region „USA, Osten“ empfohlen.

## <a name="related-datasets"></a>Zugehörige Datasets

- [Eingehende Anrufe bei der Feuerwehr](https://data.sfgov.org/Public-Safety/Fire-Department-Calls-for-Service/nuek-vuh3)
- [311 Fälle](https://data.sfgov.org/City-Infrastructure/311-Cases/vw6y-z8j6)

## <a name="additional-information"></a>Weitere Informationen

Dieses Dataset wurde von der Stadtverwaltung von San Francisco bezogen. Sehen Sie sich die [Geschäftsbedingungen für dieses Dataset](https://datasf.org/opendata/terms-of-use/) an.

## <a name="columns"></a>Spalten

| Name | Datentyp | Eindeutig | Beispielwerte | BESCHREIBUNG |
|-|-|-|-|-|
| address | Zeichenfolge | 280,652 | Nicht zu einer bestimmten Adresse zugeordnet Block 0 von 6TH ST | Die Adresse des Vorfalls (Hinweis: die Genauigkeit der Adress- und Standortangabe wird auf den Streckenmittelpunkt zwischen zwei Querstraßen, eine Kreuzung oder die nächstgelegene Telefonzelle beschränkt, um die Privatsphäre des Anrufers zu schützen). |
| category | Zeichenfolge | 108 | Die Straßen- und Gehwegreinigung ist potenziell lebensbedrohlich | Der für Menschen lesbare Name des 311-Dienstanforderungstyps oder der Anruftyp für 911-Brandmeldungen. |
| dataSubtype | Zeichenfolge | 2 | 911_Fire 311_All | „911 für Feuer“ oder „311 für alle“. |
| dataType | Zeichenfolge | 1 | Sicherheit | “Safety” |
| dateTime | timestamp | 6,496,563 | 2020-10-19 12:28:08 2020-07-28 06:40:26 | Datum und Uhrzeit der Dienstanforderung oder des Eingangs der Brandmeldung. |
| latitude | double | 1,615,369 | 37.777624238929 37.786117211838 | Breitengrad des Standorts bei Verwendung der WGS84-Projektion. |
| longitude | double | 1,554,612 | -122.39998111124 -122.419854245692 | Längengrad des Standorts bei Verwendung der WGS84-Projektion. |
| source | Zeichenfolge | 9 | Mobiltelefon/Offen 311 | Der Mechanismus oder Weg, auf dem die Serviceanforderung empfangen wurde, in der Regel per „Telefon“, „SMS“, „Website“, „Mobile App“, „Twitter“ usw. Die Bestimmungen können je nach System variieren. |
| status | Zeichenfolge | 3 | Abgeschlossen/Offen | Ein ein Wort langer Indikator des aktuellen Zustands der Dienstanforderung. (Hinweis: GeoReport V2 lässt nur die Werte „offen“ und „geschlossen“ zu.) |
| subcategory | Zeichenfolge | 1\.270 | Massenelemente für medizinische Vorfälle | Der für Menschen lesbare Name des untergeordneten Typs der Dienstanforderung für 311-Fälle oder der Anruftyp für 911-Brandmeldungen. |

## <a name="preview"></a>Vorschau

| dataType | dataSubtype | dateTime | category | subcategory | status | address | latitude | longitude | source | extendedProperties |
|-|-|-|-|-|-|-|-|-|-|-|
| Sicherheit | 911_Fire | 26.04.2021 2:56:13 Uhr | Nicht lebensbedrohlich | Medizinischer Vorfall | ungültig | Block 700 der GEARY STREET | 37.7863607914647 | -122.415616900246 | NULL |  |
| Sicherheit | 911_Fire | 26.04.2021 2:56:13 Uhr | Nicht lebensbedrohlich | Medizinischer Vorfall | ungültig | Block 700 der GEARY STREET | 37.7863607914647 | -122.415616900246 | NULL |  |
| Sicherheit | 911_Fire | 26.04.2021 2:54:03 Uhr | Nicht lebensbedrohlich | Medizinischer Vorfall | ungültig | Block 0 der ESSEX STREET | 37.7860048266229 | -122.395077258809 | NULL |  |
| Sicherheit | 911_Fire | 26.04.2021 2:54:03 Uhr | Nicht lebensbedrohlich | Medizinischer Vorfall | ungültig | Block 0 der ESSEX STREET | 37.7860048266229 | -122.395077258809 | NULL |  |
| Sicherheit | 911_Fire | 26.04.2021 2:52:17 Uhr | Nicht lebensbedrohlich | Medizinischer Vorfall | ungültig | Block 700 der 29TH AVENUE | 37.7751770865322 | -122.488604397217 | NULL |  |
| Sicherheit | 911_Fire | 26.04.2021 2:50:28 Uhr | Potenziell lebensbedrohlich | Medizinischer Vorfall | ungültig | Block 1000 der GEARY STREET | 37.7857350982044 | -122.420555240691 | NULL |  |
| Sicherheit | 911_Fire | 26.04.2021 2:50:28 Uhr | Potenziell lebensbedrohlich | Medizinischer Vorfall | ungültig | Block 1000 der GEARY STREET | 37.7857350982044 | -122.420555240691 | NULL |  |
| Sicherheit | 911_Fire | 26.04.2021 2:33:52 Uhr | Nicht lebensbedrohlich | Medizinischer Vorfall | ungültig | Block 100 der BELVEDERE STREET | 37.767791696654 | -122.449332294394 | NULL |  |
| Sicherheit | 911_Fire | 26.04.2021 2:33:52 Uhr | Nicht lebensbedrohlich | Medizinischer Vorfall | ungültig | Block 100 der BELVEDERE STREET | 37.767791696654 | -122.449332294394 | NULL |  |
| Sicherheit | 911_Fire | 26.04.2021 2:33:51 Uhr | Potenziell lebensbedrohlich | Medizinischer Vorfall | ungültig | Block 100 der 6TH STREET | 37.7807920802756 | -122.408385745499 | NULL |  |

## <a name="data-access"></a>Datenzugriff

### <a name="azure-notebooks"></a>Azure Notebooks

# <a name="azureml-opendatasets"></a>[azureml-opendatasets](#tab/azureml-opendatasets)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azureml-opendatasets&registryId=city_safety_newyork -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azureml-opendatasets&registryId=city_safety_newyork)** .

```
# This is a package in preview.
from azureml.opendatasets import NycSafety

from datetime import datetime
from dateutil import parser


end_date = parser.parse('2016-01-01')
start_date = parser.parse('2015-05-01')
safety = NycSafety(start_date=start_date, end_date=end_date)
safety = safety.to_pandas_dataframe()
```

```
safety.info()
```

<!-- nbend -->

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=city_safety_newyork -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=city_safety_newyork)** .

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
folder_name = "Safety/Release/city=NewYorkCity"
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

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=azureml-opendatasets&registryId=city_safety_newyork -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=azureml-opendatasets&registryId=city_safety_newyork)** .

```
# This is a package in preview.
# You need to pip install azureml-opendatasets in Databricks cluster. https://docs.microsoft.com/en-us/azure/data-explorer/connect-from-databricks#install-the-python-library-on-your-azure-databricks-cluster
from azureml.opendatasets import NycSafety

from datetime import datetime
from dateutil import parser


end_date = parser.parse('2016-01-01')
start_date = parser.parse('2015-05-01')
safety = NycSafety(start_date=start_date, end_date=end_date)
safety = safety.to_spark_dataframe()
```

```
display(safety.limit(5))
```

<!-- nbend -->

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

# <a name="pyspark"></a>[pyspark](#tab/pyspark)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=pyspark&registryId=city_safety_newyork -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=pyspark&registryId=city_safety_newyork)** .

```python
# Azure storage access info
blob_account_name = "azureopendatastorage"
blob_container_name = "citydatacontainer"
blob_relative_path = "Safety/Release/city=NewYorkCity"
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

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=azureml-opendatasets&registryId=city_safety_newyork -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=azureml-opendatasets&registryId=city_safety_newyork)** .

```python
# This is a package in preview.
from azureml.opendatasets import NycSafety

from datetime import datetime
from dateutil import parser

end_date = parser.parse('2016-01-01')
start_date = parser.parse('2015-05-01')
safety = NycSafety(start_date=start_date, end_date=end_date)
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

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=pyspark&registryId=city_safety_newyork -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=pyspark&registryId=city_safety_newyork)** .

```python
# Azure storage access info
blob_account_name = "azureopendatastorage"
blob_container_name = "citydatacontainer"
blob_relative_path = "Safety/Release/city=NewYorkCity"
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
