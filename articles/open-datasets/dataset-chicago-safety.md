---
title: Sicherheitsdaten zu Chicago
titleSuffix: Azure Open Datasets
description: Es wird beschrieben, wie Sie das Dataset mit den Sicherheitsdaten zu Chicago in Azure Open Datasets verwenden.
ms.service: open-datasets
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 04/16/2021
ms.openlocfilehash: d39c89308f0c33f98f1c5d074746a2008317472a
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112038872"
---
# <a name="chicago-safety-data"></a>Sicherheitsdaten zu Chicago

Serviceanfragen unter der Nummer 311 für Chicago einschließlich der gemeldeten Schlaglöcher, Probleme mit Straßenlaternen und älteren Beschwerden hinsichtlich Verstößen gegen die Abfallentsorgungsregelungen

Sämtliche Beschwerden, die Verstöße gegen die Abfallentsorgungsregelungen betreffen und über die Nummer 311 eingegangen und noch nicht bearbeitet wurden, und alle abgeschlossenen Anfragen seit dem 1. Januar 2011. Das Department of Streets and Sanitation untersucht Verstöße gegen die Abfallentsorgungsregelungen von Chicago und leitet Maßnahmen zur Behebung der Probleme ein. Bei Verstößen wie zu vollen Müllcontainern und Müll auf der Straße können Anwohner Serviceanfragen stellen. Unter der Nummer 311 gehen Beschwerden manchmal doppelt ein. Als Duplikate gekennzeichnete Anfragen wurden im selben geografischen Bereich wie die ursprüngliche Anfrage gestellt und nahezu zeitgleich in das CSR-System (Customer Service Request; Kundendienstanfrage) eingegeben. Doppelt eingereichte Beschwerden werden im Statusfeld entsprechend mit „Open - Dup“ (Offen – Duplikat) oder „Completed - Dup“ (Abgeschlossen – Duplikat) gekennzeichnet.

[!INCLUDE [Open Dataset usage notice](../../includes/open-datasets-usage-note.md)]

Das Chicago Department of Transportation (CDOT) überwacht die Ausbesserung von Schlaglöchern in Ein- und Ausfallstraßen sowie in Anwohnerstraßen auf einer Strecke von über 4.000 Meilen. Schlaglöcher werden dem CDOT über ein Callcenter gemeldet, das unter der Nummer 311 erreichbar ist. Mit einem System zur Erfassung und Überwachung von Schlaglöchern kann das CDOT die zugehörigen Standorte ermitteln und den Einsatz der Ausbesserungsteams planen.

Ein Anruf unter der Nummer 311 kann zur Ausbesserung mehrerer Schlaglöcher führen. Wenn ein gemeldetes Schlagloch von einem Team ausgebessert wird, füllt dieses zusätzlich alle Schlaglöcher innerhalb des Blocks auf. Die Ausbesserungsarbeiten werden innerhalb von sieben Tagen nach dem ersten Anruf unter der Nummer 311 abgeschlossen. Die Dauer der Arbeiten ist abhängig von den Wetterbedingungen und wird durch niedrige Temperaturen und Niederschlag beeinflusst. Bei gutem Wetter ohne Regen können die Ausbesserungsteams Tausende Schlaglöcher auffüllen. 

Wenn bereits eine offene Anfrage vorliegt und eine neue Anfrage gestellt wird, erhält Letztere den Status „Duplicate (Open)“ (Duplikat (offen)), wenn sie sich auf eine Adresse bezieht, die um maximal vier Adressen von der vorherigen Adresse abweicht. Wenn beispielsweise für die Adresse 6535 N Western bereits eine Serviceanfrage vorliegt und eine neue Anfrage für die Adresse 6531 N Western (die um nicht mehr als vier Adressnummern von der ursprünglichen Anfrage abweicht) gestellt wird, erhält die neue Anfrage den Status „Duplicate (Open)“. Nachdem die Straße ausgebessert wurde, wird als Status im CSR-System „Completed“ (Abgeschlossen) für die ursprüngliche Anfrage und „Duplicate (Closed)“ (Duplikat (geschlossen)) für alle doppelten Anfragen festgelegt. Der Status „Completed“ wird auch dann für eine Serviceanfrage festgelegt, wenn an der gemeldeten Adresse keine Schlaglöcher gefunden oder diese bereits aufgefüllt wurden. Wenn weitere Probleme wie Absenkungen oder durch fehlerhafte Instandhaltungsmaßnahmen aufgetretene Straßenschäden gefunden werden, werden diese Probleme an das zuständige Amt oder an den Auftragnehmer weitergeleitet.

Alle offenen und unter der Nummer 311 eingereichten Anfragen des Typs „Street Lights - All Out“ (Straßenlaternen – Totalausfall), der auf einen Ausfall von mindestens drei Straßenlaternen hinweist, und alle abgeschlossenen Anfragen seit dem 1. Januar 2011. Das Chicago Department of Transportation (CDOT) überwacht ca. 250.000 Straßenlaternen, die Ein- und Ausfallstraßen sowie Anwohnerstraßen in Chicago beleuchten. Wenn Anwohner den Ausfall von Straßenlaternen melden, reagiert das CDOT durch Reparaturmaßnahmen und den Austausch von Glühlampen. Wenn beim CDOT ein Bericht eines Totalausfalls eingeht, untersucht der Elektriker, der für die Reparaturarbeiten verantwortlich ist, die Straßenlaternen der zugehörigen Schaltung (insgesamt acht bis sechzehn Straßenlaternen pro Schaltung), um sicherzustellen, dass sie funktionieren. Wenn eine zweite Anfrage wegen eines Ausfalls einer Straßenlaterne in derselben Schaltung innerhalb von vier Kalendertagen nach der ursprünglichen Anfrage auftritt, erhält die neue Anfrage automatisch den Status „Duplicate (Open)“. Da der Elektriker des CDOT die Straßenlaternen einer Schaltung auf ihre Funktion prüft, werden Straßenlaternen an einer Adresse mit dem Status „Duplicate (Open)“ automatisch überwacht und repariert. Nachdem die Straßenlaternen repariert wurden, wird als Status im CSR-System „Completed“ für die ursprüngliche Anfrage und „Duplicate (Closed)“ für alle doppelten Anfragen festgelegt. Der Status „Completed“ wird auch dann für eine Serviceanfrage festgelegt, wenn an den gemeldeten Straßenlaternen keine Fehler festgestellt werden können, die Adresse in der Serviceanfrage nicht existiert oder die Straßenlaternen von einem Auftragnehmer gewartet werden. Die Daten werden täglich aktualisiert.

## <a name="volume-and-retention"></a>Volume und Aufbewahrung

Dieses Dataset wird im Parquet-Format gespeichert. Es wird täglich aktualisiert und enthält für den Zeitraum ab 2018 insgesamt etwa eine Million Zeilen (80 MB).

Dieses Dataset enthält ältere Datensätze der Jahre 2011 bis 2018. Verwenden Sie Parametereinstellungen im SDK, um Daten innerhalb eines bestimmten Zeitbereichs abzurufen.

## <a name="storage-location"></a>Speicherort

Dieses Dataset wird in der Azure-Region „USA, Osten“ gespeichert. Aus Gründen der Affinität wird die Zuweisung von Computeressourcen in der Region „USA, Osten“ empfohlen.

## <a name="related-datasets"></a>Zugehörige Datasets

- [Abfallentsorgung in Chicago](https://data.cityofchicago.org/Service-Requests/311-Service-Requests-Sanitation-Code-Complaints-Hi/me59-5fac)
- [Schlaglöcher in Chicago](https://data.cityofchicago.org/Service-Requests/311-Service-Requests-Pot-Holes-Reported-Historical/7as2-ds3y)
- [Straßenlaternen in Chicago](https://data.cityofchicago.org/Service-Requests/311-Service-Requests-Street-Lights-All-Out-Histori/zuxi-7xem)

## <a name="additional-information"></a>Weitere Informationen

Dieses Dataset wurde von der Stadtverwaltung von Chicago bezogen.

Die Nutzungsbedingungen für dieses Dataset finden Sie hier. Senden Sie eine E-Mail an dataportal@cityofchicago.org, wenn Sie Fragen zur Datenquelle haben.

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

| dataType | dataSubtype | dateTime | category | subcategory | status | address | latitude | longitude | source | extendedProperties |
|-|-|-|-|-|-|-|-|-|-|-|
| Sicherheit | 311_All | 4/25/2021 11:55:04 PM | Beschwerde über Ausfall der Straßenlaternen | ungültig | Öffnen | 4800 W WASHINGTON BLVD | 41.882148426 | -87.74556256 | NULL |  |
| Sicherheit | 311_All | 4/25/2021 11:54:31 PM | 311 NUR INFORMATIONSANRUF | ungültig | Abgeschlossen | 2111 W Lexington ST |  |  | NULL |  |
| Sicherheit | 311_All | 4/25/2021 11:52:11 PM | 311 NUR INFORMATIONSANRUF | ungültig | Abgeschlossen | 2111 W Lexington ST |  |  | NULL |  |
| Sicherheit | 311_All | 4/25/2021 11:49:56 PM | 311 NUR INFORMATIONSANRUF | ungültig | Abgeschlossen | 2111 W Lexington ST |  |  | NULL |  |
| Sicherheit | 311_All | 4/25/2021 11:48:53 PM | Wartung des Müllwagens | ungültig | Öffnen | 3409 E 106TH ST | 41.702545562 | -87.540917602 | NULL |  |
| Sicherheit | 311_All | 4/25/2021 11:46:01 PM | 311 NUR INFORMATIONSANRUF | ungültig | Abgeschlossen | 2111 W Lexington ST |  |  | NULL |  |
| Sicherheit | 311_All | 4/25/2021 11:45:46 PM | Beschwerde über Fluglärm | ungültig | Abgeschlossen | 10510 W ZEMKE RD |  |  | NULL |  |
| Sicherheit | 311_All | 4/25/2021 11:45:02 PM | 311 NUR INFORMATIONSANRUF | ungültig | Abgeschlossen | 2111 W Lexington ST |  |  | NULL |  |
| Sicherheit | 311_All | 4/25/2021 11:44:24 PM | Inspektionsanfrage bei Einbruch der Kanalisation | ungültig | Öffnen | 7246 W THORNDALE AVE | 41.987984339 | -87.808702917 | NULL |  |

## <a name="data-access"></a>Datenzugriff

### <a name="azure-notebooks"></a>Azure Notebooks

# <a name="azureml-opendatasets"></a>[azureml-opendatasets](#tab/azureml-opendatasets)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azureml-opendatasets&registryId=city_safety_chicago -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azureml-opendatasets&registryId=city_safety_chicago)** .

```
# This is a package in preview.
from azureml.opendatasets import ChicagoSafety

from datetime import datetime
from dateutil import parser


end_date = parser.parse('2016-01-01')
start_date = parser.parse('2015-05-01')
safety = ChicagoSafety(start_date=start_date, end_date=end_date)
safety = safety.to_pandas_dataframe()
```

```
safety.info()
```

<!-- nbend -->

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=city_safety_chicago -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=city_safety_chicago)** .

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
folder_name = "Safety/Release/city=Chicago"
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

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=azureml-opendatasets&registryId=city_safety_chicago -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=azureml-opendatasets&registryId=city_safety_chicago)** .

```
# This is a package in preview.
# You need to pip install azureml-opendatasets in Databricks cluster. https://docs.microsoft.com/en-us/azure/data-explorer/connect-from-databricks#install-the-python-library-on-your-azure-databricks-cluster
from azureml.opendatasets import ChicagoSafety

from datetime import datetime
from dateutil import parser


end_date = parser.parse('2016-01-01')
start_date = parser.parse('2015-05-01')
safety = ChicagoSafety(start_date=start_date, end_date=end_date)
safety = safety.to_spark_dataframe()
```

```
display(safety.limit(5))
```

<!-- nbend -->

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

# <a name="pyspark"></a>[pyspark](#tab/pyspark)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=pyspark&registryId=city_safety_chicago -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=pyspark&registryId=city_safety_chicago)** .

```python
# Azure storage access info
blob_account_name = "azureopendatastorage"
blob_container_name = "citydatacontainer"
blob_relative_path = "Safety/Release/city=Chicago"
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

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=azureml-opendatasets&registryId=city_safety_chicago -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=azureml-opendatasets&registryId=city_safety_chicago)** .

```python
# This is a package in preview.
from azureml.opendatasets import ChicagoSafety

from datetime import datetime
from dateutil import parser

end_date = parser.parse('2016-01-01')
start_date = parser.parse('2015-05-01')
safety = ChicagoSafety(start_date=start_date, end_date=end_date)
safety = safety.to_spark_dataframe()
```

```python
# Display top 5 rows
display(safety.limit(5))
```

```python
# Display data statistic information
display(safety, summary = True)
```

<!-- nbend -->

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

# <a name="pyspark"></a>[pyspark](#tab/pyspark)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=pyspark&registryId=city_safety_chicago -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=pyspark&registryId=city_safety_chicago)** .

```python
# Azure storage access info
blob_account_name = "azureopendatastorage"
blob_container_name = "citydatacontainer"
blob_relative_path = "Safety/Release/city=Chicago"
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