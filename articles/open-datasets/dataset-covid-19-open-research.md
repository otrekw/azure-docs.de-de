---
title: COVID-19 Open Research Dataset
titleSuffix: Azure Open Datasets
description: Erfahren Sie, wie Sie das COVID-19 Open Research Dataset in Azure Open Datasets verwenden.
ms.service: open-datasets
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 04/16/2021
ms.openlocfilehash: 1225cc9f88fb07766f8f6e89e5a2502bdb83653a
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112038953"
---
# <a name="covid-19-open-research-dataset"></a>COVID-19 Open Research Dataset

Ein Volltext- und Metadaten-Dataset von wissenschaftlichen Artikeln im Zusammenhang mit COVID-19 und dem Corona-Virus, die für Maschinenlesbarkeit optimiert und Forschern auf der ganzen Welt zur Nutzung zur Verfügung gestellt wurden.

Als Reaktion auf die COVID-19-Pandemie ist das [Allen Institute for AI](https://allenai.org/) eine Partnerschaft mit führenden Forschungsgruppen eingegangen, um das COVID-19 Open Research Dataset (CORD-19) vorzubereiten und zu verteilen. Dieses Dataset ist eine kostenlose Ressource mit mehr als 47.000 wissenschaftlichen Artikeln, darunter über 36.000 Artikel mit Volltext, zu COVID-19 und die Coronaviren-Familie, die von Forschern auf der ganzen Welt genutzt werden kann.

Dieses Dataset animiert Forscher dazu, aktuelle Fortschritte im Bereich der Verarbeitung natürlicher Sprache anzuwenden und so neue Erkenntnisse zu generieren, die dem Kampf gegen diese Infektionskrankheit dienen.

Der Korpus wird aktualisiert, wenn neue Forschungserkenntnisse in Artikeln mit Peer-Review und Archiven wie [bioRxiv](https://www.biorxiv.org/) oder [medRxiv](https://www.medrxiv.org/) veröffentlicht werden.

[!INCLUDE [Open Dataset usage notice](../../includes/open-datasets-usage-note.md)]

## <a name="license-terms"></a>Lizenzbedingungen

Dieses Dataset wird vom Allen Institute of AI und [Semantic Scholar](https://pages.semanticscholar.org/coronavirus-research) zur Verfügung gestellt. Wenn Sie auf einen beliebigen im CORD-19-Dataset bereitgestellten Inhalt zugreifen, ihn herunterladen oder anderweitig nutzen, stimmen Sie der [Dataset-Lizenz](https://ai2-semanticscholar-cord-19.s3-us-west-2.amazonaws.com/2020-03-13/COVID.DATA.LIC.AGMT.pdf) im Zusammenhang mit der Nutzung dieses Datasets zu. Spezifische Lizenzinformationen zu einzelnen Artikeln im Dataset stehen in der Metadatendatei zur Verfügung. Weitere Lizenzinformationen stehen auf der [PMC-Website](https://www.ncbi.nlm.nih.gov/pmc/tools/openftlist/), der [medRxiv-Website](https://www.medrxiv.org/submit-a-manuscript) und der [bioRxiv-Website](https://www.biorxiv.org/about-biorxiv) zur Verfügung.

## <a name="volume-and-retention"></a>Volume und Aufbewahrung

Dieses Dataset wird im JSON-Format gespeichert, und die neueste Version enthält über 36.000 Volltextartikel. Jedes Paper ist ein einzelnes JSON-Objekt. [Zeigen Sie das Schema an](https://ai2-semanticscholar-cord-19.s3-us-west-2.amazonaws.com/2020-03-13/json_schema.txt).

## <a name="storage-location"></a>Speicherort
Dieses Dataset wird in der Azure-Region „USA, Osten“ gespeichert. Aus Gründen der Affinität wird die Anordnung von Computeressourcen in der Region „USA, Osten“ empfohlen.

## <a name="citation"></a>Quellenangaben

Wenn Sie CORD-19-Daten in eine Veröffentlichung oder Weiterverbreitung einbeziehen, geben Sie das Dataset folgendermaßen an:

Im Literaturverzeichnis:

COVID-19 Open Research Dataset (CORD-19). 2020. Version JJJJ-MM-TT Quelle: [COVID-19 Open Research Dataset (CORD-19)](https://pages.semanticscholar.org/coronavirus-research). Zugriff: TT-MM-JJJJ. doi:10.5281/zenodo.3715505

In Text: (CORD-19, 2020)

## <a name="contact"></a>Contact

Falls Sie Fragen zu diesem Dataset haben, wenden Sie sich an partnerships@allenai.org.

## <a name="data-access"></a>Datenzugriff

### <a name="azure-notebooks"></a>Azure Notebooks

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=covid-19-open-research -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=covid-19-open-research)** .

## <a name="the-cord-19-dataset"></a>Das CORD-19-Dataset

CORD-19 ist eine Sammlung von mehr als 50.000 wissenschaftlichen Artikeln – darunter mehr als 40.000 mit Volltext – zu COVID-19, SARS-CoV-2 und verwandten Coronaviren. Dieses Dataset wird kostenlos zur Verfügung gestellt, um Forscher bei der Bewältigung der COVID-19-Pandemie zu unterstützen.

Mit diesem Notebook sollen zwei Ziele erreicht werden:

1. Veranschaulichung des Zugriffs auf das CORD-19-Dataset in Azure: Wir stellen eine Verbindung mit dem Azure Blob Storage-Konto her, in dem sich das CORD-19-Dataset befindet.
2. Exemplarische Schritte durch die Struktur des Datasets: Artikel im Dataset werden als JSON-Dateien gespeichert. Wir stellen Beispiele bereit, die Folgendes zeigen:

  - Suchen der Artikel (Navigieren im Container)
  - Lesen der Artikel (Navigieren im JSON-Schema)

Abhängigkeiten: Für dieses Notebook sind die folgenden Bibliotheken erforderlich:

- Azure Storage (zum Beispiel `pip install azure-storage`)
- NLTK ([Dokumentation](https://www.nltk.org/install.html))
- Pandas (zum Beispiel `pip install pandas`)

### <a name="getting-the-cord-19-data-from-azure"></a>Abrufen der CORD-19-Daten aus Azure

Die CORD-19-Daten wurden [hier](https://azure.microsoft.com/services/open-datasets/catalog/covid-19-open-research/) als Azure Open Dataset hochgeladen. Wir erstellen einen Blob-Dienst, der mit dem offenen Dataset CORD-19 verknüpft ist.


```python
from azure.storage.blob import BlockBlobService

# storage account details
azure_storage_account_name = "azureopendatastorage"
azure_storage_sas_token = "sv=2019-02-02&ss=bfqt&srt=sco&sp=rlcup&se=2025-04-14T00:21:16Z&st=2020-04-13T16:21:16Z&spr=https&sig=JgwLYbdGruHxRYTpr5dxfJqobKbhGap8WUtKFadcivQ%3D"

# create a blob service
blob_service = BlockBlobService(
    account_name=azure_storage_account_name,
    sas_token=azure_storage_sas_token,
)
```

Wir können diesen Blob-Dienst als Handle für die Daten verwenden. Wir können durch das Dataset navigieren und dabei die `BlockBlobService`-APIs verwenden. Ausführlichere Informationen finden Sie unter Folgendem:

* [Konzepte des Blob-Diensts](/rest/api/storageservices/blob-service-concepts)
* [Vorgänge für Container](/rest/api/storageservices/operations-on-containers)

Die CORD-19-Daten werden im Container `covid19temp` gespeichert. Dies ist die Dateistruktur innerhalb des Containers zusammen mit einer Beispieldatei.

```
metadata.csv
custom_license/
    pdf_json/
        0001418189999fea7f7cbe3e82703d71c85a6fe5.json        # filename is sha-hash
        ...
    pmc_json/
        PMC1065028.xml.json                                  # filename is the PMC ID
        ...
noncomm_use_subset/
    pdf_json/
        0036b28fddf7e93da0970303672934ea2f9944e7.json
        ...
    pmc_json/
        PMC1616946.xml.json
        ...
comm_use_subset/
    pdf_json/
        000b7d1517ceebb34e1e3e817695b6de03e2fa78.json
        ...
    pmc_json/
        PMC1054884.xml.json
        ...
biorxiv_medrxiv/                                             # note: there is no pmc_json subdir
    pdf_json/
        0015023cc06b5362d332b3baf348d11567ca2fbb.json
        ...
```

Jede JSON-Datei entspricht einem einzelnen Artikel im Dataset. Hier werden Titel, Autoren, abstrakte Daten und (sofern verfügbar) die Volltextdaten gespeichert.

### <a name="using-metadatacsv"></a>Verwenden von metadata.csv
Das CORD-19-Dataset enthält `metadata.csv` – eine einzelne Datei, die grundlegende Informationen zu allen Dokumenten aufzeichnet, die im CORD-19-Dataset verfügbar sind. Dies ist ein guter Ausgangspunkt für Erkundungen!


```python
# container housing CORD-19 data
container_name = "covid19temp"

# download metadata.csv
metadata_filename = 'metadata.csv'
blob_service.get_blob_to_path(
    container_name=container_name,
    blob_name=metadata_filename,
    file_path=metadata_filename
)
```

```python
import pandas as pd

# read metadata.csv into a dataframe
metadata_filename = 'metadata.csv'
metadata = pd.read_csv(metadata_filename)
```

```python
metadata.head(3)
```

Auf den ersten Blick gibt es viel zu begreifen, daher wenden wir ein wenig Feinschliff an.

```python
simple_schema = ['cord_uid', 'source_x', 'title', 'abstract', 'authors', 'full_text_file', 'url']

def make_clickable(address):
    '''Make the url clickable'''
    return '<a href="{0}">{0}</a>'.format(address)

def preview(text):
    '''Show only a preview of the text data.'''
    return text[:30] + '...'

format_ = {'title': preview, 'abstract': preview, 'authors': preview, 'url': make_clickable}

metadata[simple_schema].head().style.format(format_)
```

```python
# let's take a quick look around
num_entries = len(metadata)
print("There are {} many entries in this dataset:".format(num_entries))

metadata_with_text = metadata[metadata['full_text_file'].isna() == False]
with_full_text = len(metadata_with_text)
print("-- {} have full text entries".format(with_full_text))

with_doi = metadata['doi'].count()
print("-- {} have DOIs".format(with_doi))

with_pmcid = metadata['pmcid'].count()
print("-- {} have PubMed Central (PMC) ids".format(with_pmcid))

with_microsoft_id = metadata['Microsoft Academic Paper ID'].count()
print("-- {} have Microsoft Academic paper ids".format(with_microsoft_id))
```

```
There are 51078 many entries in this dataset:
-- 42511 have full text entries
-- 47741 have DOIs
-- 41082 have PubMed Central (PMC) ids
-- 964 have Microsoft Academic paper ids
```

## <a name="example-read-full-text"></a>Beispiel: Volltext lesen

`metadata.csv` enthält nicht den Volltext selbst. Sehen wir uns ein Beispiel an, wie das gelesen wird. Suchen Sie die Volltext-JSON-Datei, entpacken Sie sie, und konvertieren Sie sie in eine Liste von Sätzen.

```python
# choose a random example with pdf parse available
metadata_with_pdf_parse = metadata[metadata['has_pdf_parse']]
example_entry = metadata_with_pdf_parse.iloc[42]

# construct path to blob containing full text
blob_name = '{0}/pdf_json/{1}.json'.format(example_entry['full_text_file'], example_entry['sha'])  # note the repetition in the path
print("Full text blob for this entry:")
print(blob_name)
```

Wir können nun den JSON-Inhalt, der diesem Blob zugeordnet ist, wie folgt lesen.

```python
import json
blob_as_json_string = blob_service.get_blob_to_text(container_name=container_name, blob_name=blob_name)
data = json.loads(blob_as_json_string.content)

# in addition to the body text, the metadata is also stored within the individual json files
print("Keys within data:", ', '.join(data.keys()))
```

Im Rahmen dieses Beispiels sind wir an `body_text` interessiert, wo die Textdaten wie folgt gespeichert sind:

```json
"body_text": [                      # list of paragraphs in full body
    {
        "text": <str>,
        "cite_spans": [             # list of character indices of inline citations
                                    # e.g. citation "[7]" occurs at positions 151-154 in "text"
                                    #      linked to bibliography entry BIBREF3
            {
                "start": 151,
                "end": 154,
                "text": "[7]",
                "ref_id": "BIBREF3"
            },
            ...
        ],
        "ref_spans": <list of dicts similar to cite_spans>,     # e.g. inline reference to "Table 1"
        "section": "Abstract"
    },
    ...
]
```

Das vollständige JSON-Schema finden Sie [hier](https://ai2-semanticscholar-cord-19.s3-us-west-2.amazonaws.com/2020-03-13/json_schema.txt).


```python
from nltk.tokenize import sent_tokenize

# the text itself lives under 'body_text'
text = data['body_text']

# many NLP tasks play nicely with a list of sentences
sentences = []
for paragraph in text:
    sentences.extend(sent_tokenize(paragraph['text']))

print("An example sentence:", sentences[0])
```    

### <a name="pdf-vs-pmc-xml-parse"></a>PDF Parse gegenüber PMC XML Parse

Im obigen Beispiel haben wir uns einen Fall mit `has_pdf_parse == True` angesehen. In dem Fall hatte der Blobdateipfad folgendes Format:

```
'<full_text_file>/pdf_json/<sha>.json'
```

Verwenden Sie alternativ für Fälle mit `has_pmc_xml_parse == True` das folgende Format:

```
'<full_text_file>/pmc_json/<pmcid>.xml.json'
```

Beispiel:


```python
# choose a random example with pmc parse available
metadata_with_pmc_parse = metadata[metadata['has_pmc_xml_parse']]
example_entry = metadata_with_pmc_parse.iloc[42]

# construct path to blob containing full text
blob_name = '{0}/pmc_json/{1}.xml.json'.format(example_entry['full_text_file'], example_entry['pmcid'])  # note the repetition in the path
print("Full text blob for this entry:")
print(blob_name)

blob_as_json_string = blob_service.get_blob_to_text(container_name=container_name, blob_name=blob_name)
data = json.loads(blob_as_json_string.content)

# the text itself lives under 'body_text'
text = data['body_text']

# many NLP tasks play nicely with a list of sentences
sentences = []
for paragraph in text:
    sentences.extend(sent_tokenize(paragraph['text']))

print("An example sentence:", sentences[0])
```
```
Full text blob for this entry:
custom_license/pmc_json/PMC546170.xml.json
An example sentence: Double-stranded small interfering RNA (siRNA) molecules have drawn much attention since it was unambiguously shown that they mediate potent gene knock-down in a variety of mammalian cells (1).
```    

## <a name="iterate-through-blobs-directly"></a>Direktes Durchlaufen von Blobs

In den obigen Beispielen haben wir die Datei `metadata.csv` verwendet, um durch die Daten zu navigieren, den Blobdateipfad zu erstellen und Daten aus dem Blob zu lesen. Eine Alternative ist das Durchlaufen der Blobs selbst.


```python
# get and sort list of available blobs
blobs = blob_service.list_blobs(container_name)
sorted_blobs = sorted(list(blobs), key=lambda e: e.name, reverse=True)
```

Nun können wir die Blobs direkt durchlaufen. Zählen wir beispielsweise die Anzahl der verfügbaren JSON-Dateien.


```python
# we can now iterate directly though the blobs
count = 0
for blob in sorted_blobs:
    if blob.name[-5:] == ".json":
        count += 1
print("There are {} many json files".format(count))
```

```
There are 59784 many json files
```  

## <a name="appendix"></a>Anhang

### <a name="data-quality-issues"></a>Probleme mit der Datenqualität

Dies ist ein großes Dataset, das aus offensichtlichen Gründen eher hastig zusammengestellt wurde! Hier sind einige Datenqualitätsprobleme, die wir beobachtet haben.

#### <a name="multiple-shas"></a>Mehrere Shas

Wir beobachten, dass es in einigen Fällen mehrere Shas für einen bestimmten Eintrag gibt.

```python
metadata_multiple_shas = metadata[metadata['sha'].str.len() > 40]

print("There are {} many entries with multiple shas".format(len(metadata_multiple_shas)))

metadata_multiple_shas.head(3)
```
```
There are 1999 many entries with multiple shas
```

## <a name="layout-of-the-container"></a>Layout des Containers

Hier verwenden wir einen einfachen regulären Ausdruck, um die Dateistruktur des Containers zu untersuchen, falls dies in Zukunft aktualisiert wird.

```python
container_name = "covid19temp"
blobs = blob_service.list_blobs(container_name)
sorted_blobs = sorted(list(blobs), key=lambda e: e.name, reverse=True)
```

```python
import re
dirs = {}

pattern = '([\w]+)\/([\w]+)\/([\w.]+).json'
for blob in sorted_blobs:
    
    m = re.match(pattern, blob.name)
    
    if m:
        dir_ = m[1] + '/' + m[2]
        
        if dir_ in dirs:
            dirs[dir_] += 1
        else:
            dirs[dir_] = 1
        
dirs
```

<!-- nbend -->

# <a name="azureml-opendatasets"></a>[azureml-opendatasets](#tab/azureml-opendatasets)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azureml-opendatasets&registryId=covid-19-open-research -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azureml-opendatasets&registryId=covid-19-open-research)** .

## <a name="the-cord-19-dataset"></a>Das CORD-19-Dataset

CORD-19 ist eine Sammlung von mehr als 50.000 wissenschaftlichen Artikeln – darunter mehr als 40.000 mit Volltext – zu COVID-19, SARS-CoV-2 und verwandten Coronaviren. Dieses Dataset wird kostenlos zur Verfügung gestellt, um Forscher bei der Bewältigung der COVID-19-Pandemie zu unterstützen.

Mit diesem Notebook sollen zwei Ziele erreicht werden:

1. Veranschaulichung des Zugriffs auf das CORD-19-Dataset in Azure: Wir verwenden das AzureML-Dataset, um einen Kontext für die CORD-19-Daten bereitzustellen.
2. Exemplarische Schritte durch die Struktur des Datasets: Artikel im Dataset werden als JSON-Dateien gespeichert. Wir stellen Beispiele bereit, die Folgendes zeigen:

  - Suchen der Artikel (Navigieren in der Verzeichnisstruktur)
  - Lesen der Artikel (Navigieren im JSON-Schema)

Abhängigkeiten: Für dieses Notebook sind die folgenden Bibliotheken erforderlich:

- AzureML Python SDK (zum Beispiel `pip install --upgrade azureml-sdk`)
- Pandas (zum Beispiel `pip install pandas`)
- NLTK ([Dokumentation](https://www.nltk.org/install.html)) (zum Beispiel `pip install nltk`)

Wenn Ihr NLTK nicht über ein `punkt`-Paket verfügt, müssen Sie Folgendes ausführen:

```
import nltk
nltk.download('punkt')
```

### <a name="getting-the-cord-19-data-from-azure"></a>Abrufen der CORD-19-Daten aus Azure

Die CORD-19-Daten wurden [hier](https://azure.microsoft.com/services/open-datasets/catalog/covid-19-open-research/) als Azure Open Dataset hochgeladen. In diesem Notebook verwenden wir das AzureML-[Dataset](/python/api/azureml-core/azureml.core.dataset.dataset), um auf das offene Dataset CORD-19 zu verweisen.

```python
import azureml.core
print("Azure ML SDK Version: ", azureml.core.VERSION)
```

```python
from azureml.core import  Dataset
cord19_dataset = Dataset.File.from_files('https://azureopendatastorage.blob.core.windows.net/covid19temp')
mount = cord19_dataset.mount()
```

Die `mount()`-Methode erstellt einen Kontext-Manager zum Einbinden von Dateisystemdatenströmen, die vom Dataset als lokale Dateien definiert werden.

Verwenden Sie `mount.start()` und `mount.stop()` oder alternativ `with mount():`, um Kontext zu verwalten.

Die Einbindung (Mount) wird nur unter Unix- oder Unix-ähnlichen Betriebssystemen unterstützt, und „libfuse“ muss vorhanden sein. Wenn die Ausführung in einem Docker-Container erfolgt, muss der Docker-Container mit dem `--privileged`-Flag gestartet werden oder mit `--cap-add SYS_ADMIN --device /dev/fuse` gestartet werden. Weitere Informationen finden Sie in der [Dokumentation](/python/api/azureml-core/azureml.data.file_dataset.filedataset?view=azure-ml-py#mount-mount-point-none----kwargs-&preserve-view=true).


```python
import os

COVID_DIR = '/covid19temp'
path = mount.mount_point + COVID_DIR

with mount:
    print(os.listdir(path))
```
```
['antiviral_with_properties_compressed.sdf', 'biorxiv_medrxiv', 'biorxiv_medrxiv_compressed.tar.gz', 'comm_use_subset', 'comm_use_subset_compressed.tar.gz', 'custom_license', 'custom_license_compressed.tar.gz', 'metadata.csv', 'noncomm_use_subset', 'noncomm_use_subset_compressed.tar.gz']
```

Dies ist die Dateistruktur innerhalb des CORD-19-Datasets zusammen mit einer Beispieldatei.

```
metadata.csv
custom_license/
    pdf_json/
        0001418189999fea7f7cbe3e82703d71c85a6fe5.json        # filename is sha-hash
        ...
    pmc_json/
        PMC1065028.xml.json                                  # filename is the PMC ID
        ...
noncomm_use_subset/
    pdf_json/
        0036b28fddf7e93da0970303672934ea2f9944e7.json
        ...
    pmc_json/
        PMC1616946.xml.json
        ...
comm_use_subset/
    pdf_json/
        000b7d1517ceebb34e1e3e817695b6de03e2fa78.json
        ...
    pmc_json/
        PMC1054884.xml.json
        ...
biorxiv_medrxiv/                                             # note: there is no pmc_json subdir
    pdf_json/
        0015023cc06b5362d332b3baf348d11567ca2fbb.json
        ...
```

Jede JSON-Datei entspricht einem einzelnen Artikel im Dataset. Hier werden Titel, Autoren, abstrakte Daten und (sofern verfügbar) die Volltextdaten gespeichert.

## <a name="using-metadatacsv"></a>Verwenden von metadata.csv

Das CORD-19-Dataset enthält `metadata.csv` – eine einzelne Datei, die grundlegende Informationen zu allen Dokumenten aufzeichnet, die im CORD-19-Dataset verfügbar sind. Dies ist ein guter Ausgangspunkt für Erkundungen!


```python
import pandas as pd

# create mount context
mount.start()

# specify path to metadata.csv
COVID_DIR = 'covid19temp'
metadata_filename = '{}/{}/{}'.format(mount.mount_point, COVID_DIR, 'metadata.csv')

# read metadata
metadata = pd.read_csv(metadata_filename)
metadata.head(3)
```

```python
simple_schema = ['cord_uid', 'source_x', 'title', 'abstract', 'authors', 'full_text_file', 'url']

def make_clickable(address):
    '''Make the url clickable'''
    return '<a href="{0}">{0}</a>'.format(address)

def preview(text):
    '''Show only a preview of the text data.'''
    return text[:30] + '...'

format_ = {'title': preview, 'abstract': preview, 'authors': preview, 'url': make_clickable}

metadata[simple_schema].head().style.format(format_)
```

```python
# let's take a quick look around
num_entries = len(metadata)
print("There are {} many entries in this dataset:".format(num_entries))

metadata_with_text = metadata[metadata['full_text_file'].isna() == False]
with_full_text = len(metadata_with_text)
print("-- {} have full text entries".format(with_full_text))

with_doi = metadata['doi'].count()
print("-- {} have DOIs".format(with_doi))

with_pmcid = metadata['pmcid'].count()
print("-- {} have PubMed Central (PMC) ids".format(with_pmcid))

with_microsoft_id = metadata['Microsoft Academic Paper ID'].count()
print("-- {} have Microsoft Academic paper ids".format(with_microsoft_id))
```    

### <a name="example-read-full-text"></a>Beispiel: Volltext lesen

`metadata.csv` enthält nicht den Volltext selbst. Sehen wir uns ein Beispiel an, wie das gelesen wird. Suchen Sie die Volltext-JSON-Datei, entpacken Sie sie, und konvertieren Sie sie in eine Liste von Sätzen.


```python
# choose a random example with pdf parse available
metadata_with_pdf_parse = metadata[metadata['has_pdf_parse']]
example_entry = metadata_with_pdf_parse.iloc[42]

# construct path to blob containing full text
filepath = '{0}/{1}/pdf_json/{2}.json'.format(path, example_entry['full_text_file'], example_entry['sha'])
print("Full text filepath:")
print(filepath)
```    

Wir können nun den JSON-Inhalt, der dieser Datei zugeordnet ist, wie folgt lesen.

```python
import json

try:
    with open(filepath, 'r') as f:
        data = json.load(f)
except FileNotFoundError as e:
    # in case the mount context has been closed
    mount.start()
    with open(filepath, 'r') as f:
        data = json.load(f)
        
# in addition to the body text, the metadata is also stored within the individual json files
print("Keys within data:", ', '.join(data.keys()))
```

```
Keys within data: paper_id, metadata, abstract, body_text, bib_entries, ref_entries, back_matter
```

Im Rahmen dieses Beispiels sind wir an `body_text` interessiert, wo die Textdaten wie folgt gespeichert sind:

```json
"body_text": [                      # list of paragraphs in full body
    {
        "text": <str>,
        "cite_spans": [             # list of character indices of inline citations
                                    # e.g. citation "[7]" occurs at positions 151-154 in "text"
                                    #      linked to bibliography entry BIBREF3
            {
                "start": 151,
                "end": 154,
                "text": "[7]",
                "ref_id": "BIBREF3"
            },
            ...
        ],
        "ref_spans": <list of dicts similar to cite_spans>,     # e.g. inline reference to "Table 1"
        "section": "Abstract"
    },
    ...
]
```

Zeigen Sie das [vollständige JSON-Schema](https://ai2-semanticscholar-cord-19.s3-us-west-2.amazonaws.com/2020-03-13/json_schema.txt) an.


```python
from nltk.tokenize import sent_tokenize
# the text itself lives under 'body_text'
text = data['body_text']

# many NLP tasks play nicely with a list of sentences
sentences = []
for paragraph in text:
    sentences.extend(sent_tokenize(paragraph['text']))

print("An example sentence:", sentences[0])
```
    

#### <a name="pdf-vs-pmc-xml-parse"></a>PDF Parse gegenüber PMC XML Parse

Im obigen Beispiel haben wir uns einen Fall mit `has_pdf_parse == True` angesehen. In dem Fall hatte der Dateipfad folgendes Format:

```
'<full_text_file>/pdf_json/<sha>.json'
```

Verwenden Sie alternativ für Fälle mit `has_pmc_xml_parse == True` das folgende Format:

```
'<full_text_file>/pmc_json/<pmcid>.xml.json'
```

Beispiel:


```python
# choose a random example with pmc parse available
metadata_with_pmc_parse = metadata[metadata['has_pmc_xml_parse']]
example_entry = metadata_with_pmc_parse.iloc[42]

# construct path to blob containing full text
filename = '{0}/pmc_json/{1}.xml.json'.format(example_entry['full_text_file'], example_entry['pmcid'])  # note the repetition in the path
print("Path to file: {}\n".format(filename))

with open(mount.mount_point + '/' + COVID_DIR + '/' + filename, 'r') as f:
    data = json.load(f)

# the text itself lives under 'body_text'
text = data['body_text']

# many NLP tasks play nicely with a list of sentences
sentences = []
for paragraph in text:
    sentences.extend(sent_tokenize(paragraph['text']))

print("An example sentence:", sentences[0])
```

## <a name="appendix"></a>Anhang

### <a name="data-quality-issues"></a>Probleme mit der Datenqualität

Dies ist ein großes Dataset, das aus offensichtlichen Gründen eher hastig zusammengestellt wurde! Hier sind einige Datenqualitätsprobleme, die wir beobachtet haben.


```python
metadata_multiple_shas = metadata[metadata['sha'].str.len() > 40]

print("There are {} many entries with multiple shas".format(len(metadata_multiple_shas)))

metadata_multiple_shas.head(3)
```

<!-- nbend -->

---

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich mit den restlichen Datasets im [Open Datasets-Katalog](dataset-catalog.md) vertraut.