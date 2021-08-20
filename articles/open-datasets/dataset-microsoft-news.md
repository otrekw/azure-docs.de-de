---
title: Microsoft News-Empfehlungsdataset
titleSuffix: Azure Open Datasets
description: Hier erfahren Sie, wie Sie das Microsoft News-Empfehlungsdataset in Azure Open Datasets verwenden.
ms.service: open-datasets
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 04/16/2021
ms.openlocfilehash: 29a7865a06fd96ce4d27892775fb5af728633a02
ms.sourcegitcommit: 7c44970b9caf9d26ab8174c75480f5b09ae7c3d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2021
ms.locfileid: "112982472"
---
# <a name="microsoft-news-recommendation"></a>Microsoft News-Empfehlung

**Mi** crosoft **N** ews **D** ataset (MIND) ist ein umfangreiches Dataset für die Recherche von News-Empfehlungen. Es wurde aus anonymisierten Verhaltensprotokollen der Microsoft News-Website gesammelt. MIND dient als Benchmark-Dataset für News-Empfehlungen und soll die Recherche im Bereich von News-Empfehlungen und Empfehlungssystemen erleichtern.

MIND umfasst etwa 160.000 englische News-Artikel und mehr als 15 Millionen Aufrufprotokolle, die von 1 Million Benutzern erzeugt wurden. Jeder News-Artikel enthält umfangreiche Textinhalte wie Titel, Zusammenfassung, Textkörper, Kategorie und Entitäten. Jedes Aufrufprotokoll enthält die Klickereignisse, Nicht-Klickereignisse und den Verlauf des News-Klickverhaltens des jeweiligen Benutzers vor dem entsprechenden Aufruf. Aus Gründen des Datenschutzes wurde jeder Benutzer aus dem Produktionssystem „abgekoppelt“, wenn er mit einer anonymisierten ID sicher hashcodiert wurde. Detailliertere Informationen zum MIND-Dataset finden Sie im Artikel [MIND: A Large-scale Dataset for News Recommendation](https://msnews.github.io/assets/doc/ACL2020_MIND.pdf) (MIND: Ein umfangreiches Dataset für News-Empfehlungen).
 
## <a name="volume"></a>Volume

Sowohl die Trainings- als auch die Überprüfungsdaten sind in einem ZIP-komprimierten Ordner gespeichert, der vier verschiedene Dateien enthält:

| DATEINAME | DESCRIPTION |
|-|-|
| behaviors.tsv | Die Klickverläufe und Aufrufprotokolle von Benutzern |
| news.tsv | Die Informationen aus News-Artikeln |
| entity_embedding.vec | Die Einbettungen von Entitäten in News, die aus Knowledge Graph extrahiert wurden |
| relation_embedding.vec | Die Einbettungen von Beziehungen zwischen Entitäten, die aus Knowledge Graph extrahiert wurden |

### <a name="behaviorstsv"></a>behaviors.tsv

Die Datei behaviors.tsv enthält die Aufrufprotokolle und die News-Klickverläufe der Benutzer. Sie besteht aus fünf Spalten, die durch das Registerkartensymbol unterteilt sind:

- Aufruf-ID. Die ID eines Aufrufs.
- Benutzer-ID. Die anonyme ID eines Benutzers.
- Zeit. Die Uhrzeit des Aufrufs im Format „TT/MM/JJJJ HH:MM:SS AM/PM“.
- Verlauf. Der News-Klickverlauf (ID-Liste von geklickten News) dieses Benutzers vor diesem Aufruf.
- Aufrufe. Liste der bei diesem Aufruf angezeigten News und das Klickverhalten des Benutzers für sie (1 für Klick und 0 für kein Klick).

Ein Beispiel dafür wird in der Tabelle unten gezeigt:

| COLUMN | CONTENT |
|-|-|
| Aufruf-ID | 123 |
| Benutzer-ID | U131 |
| Time | 13.11.2019 8:36:57 Uhr |
| Verlauf | N11 N21 N103 |
| Aufrufe | N4-1 N34-1 N156-0 N207-0 N198-0 |

### <a name="newstsv"></a>news.tsv

Die Datei news.tsv enthält die detaillierten Informationen zu News-Artikeln in der Datei behaviors.tsv. Sie besteht aus sieben Spalten, die durch das Registerkartensymbol unterteilt sind:

- News-ID
- Category
- Unterkategorie
- Titel
- Zusammenfassung
- URL
- Titelentitäten (Entitäten, die im Titel dieser News enthalten sind)
- Zusammenfassungsentitäten (Entitäten, die in der Zusammenfassung dieser News enthalten sind)

Aufgrund der Lizenzstruktur werden die vollständigen Textkörper des Inhalts von MSN News-Artikeln zum Herunterladen nicht zur Verfügung gestellt. Der Einfachheit halber haben wir aber ein [Hilfsprogrammskript](https://github.com/msnews/MIND/tree/master/crawler) als Hilfe beim Analysieren der News-Webseite aus den MSN-URLs im Dataset bereitgestellt. Aufgrund von zeitlicher Einschränkung sind einige URLs abgelaufen, sodass darauf nicht erfolgreich zugegriffen werden kann. Wir arbeiten zur Zeit an einer Lösung dieses Problems.

Ein Beispiel dafür wird in der folgenden Tabelle gezeigt:

| COLUMN | CONTENT |
|-|-|
| News-ID | N37378 |
| Category | Sport |
| SubCategory | Golf |
| Titel | PGA Tour-Gewinner |
| Zusammenfassung | Ein Katalog der aktuellen Gewinner der PGA Tour. |
| URL | https://www.msn.com/en-us/sports/golf/pga-tour-winners/ss-AAjnQjj?ocid=chopendata |
| Titelentitäten | [{“Label”: “PGA Tour”, “Type”: “O”, “WikidataId”: “Q910409”, “Confidence”: 1.0, “OccurrenceOffsets”: [0], “SurfaceForms”: [“PGA Tour”]}] |
| Zusammenfassungsentitäten | [{“Label”: “PGA Tour”, “Type”: “O”, “WikidataId”: “Q910409”, “Confidence”: 1.0, “OccurrenceOffsets”: [35], “SurfaceForms”: [“PGA Tour”]}] |

Die Beschreibungen der Wörterbuchschlüssel in der Spalte „Entitäten“ werden folgendermaßen aufgelistet:

| SCHLÜSSEL | DESCRIPTION |
|-|-|
| Bezeichnung | Der Entitätsname im Wikidata-Knowledge Graph |
| type | Der Typ dieser Entität in Wikidata |
| WikidataId | Die Entitäts-ID in Wikidata |
| Confidence | Die Konfidenz der Entitätsverknüpfung |
| OccurrenceOffsets | Das Entitätsoffset auf Zeichenebene im Text des Titels oder der Zusammenfassung |
| SurfaceForms | Die „rohen“ Entitätsnamen im Originaltext |

### <a name="entity_embeddingvec--relation_embeddingvec"></a>entity_embedding.vec & relation_embedding.vec

Die Dateien entity_embedding.vec und relation_embedding.vec enthalten die 100-dimensionalen Einbettungen der Entitäten und Beziehungen, die durch die TransE-Methode aus dem Unterdiagramm (von WikiData-Knowledge Graph) erlernt wurden. In beiden Dateien enthält die erste Spalte die ID von Entität/Beziehung, und die anderen Spalten enthalten die Werte des Einbettungsvektors. Wir hoffen, dass diese Daten die Recherche in wissensintensiven News-Empfehlungen erleichtern können. Ein Beispiel sieht folgendermaßen aus:

| id | EINBETTUNGSWERTE |
|-|-|
| Q42306013 | 0.014516 -0.106958 0.024590 … -0.080382 |

Infolge einiger Gründe beim Erlernen der Einbettung aus dem Unterdiagramm enthalten ein paar Entitäten in der Datei entity_embedding.vec möglicherweise keine Einbettungen.

## <a name="storage-location"></a>Speicherort

Die Daten werden in Blobs in einem Rechenzentrum der Region „USA, Westen/Osten“ gespeichert. Dazu wird folgender Blobcontainer verwendet: https://mind201910small.blob.core.windows.net/release/.

Innerhalb des Containers werden das Trainings- und Überprüfungsset in MINDlarge_train.zip bzw. MINDlarge_dev.zip komprimiert.

## <a name="additional-information"></a>Zusätzliche Informationen

Das MIND-Dataset kann zu Recherchezwecken unter [Microsoft-Software-Lizenzbedingungen](https://github.com/msnews/MIND/blob/master/MSR%20License_Data.pdf) kostenlos heruntergeladen werden. Wenden Sie sich an mind@microsoft.com, wenn Sie Fragen zum Dataset haben.

## <a name="data-access"></a>Datenzugriff

### <a name="azure-notebooks"></a>Azure Notebooks

# <a name="azureml-opendatasets"></a>[azureml-opendatasets](#tab/azureml-opendatasets)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=microsoft-news-dataset -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=microsoft-news-dataset)** .

## <a name="demo-notebook-for-accessing-mind-data-on-azure"></a>Demonotebook für den Zugriff auf MIND-Daten in Azure

Dieses Notebook enthält ein Beispiel für den Zugriff auf MIND-Daten aus Blob Storage in Azure.

MIND-Daten werden im Rechenzentrum „USA, Westen/Osten“ gespeichert, sodass dieses Notebook effizienter auf dem Azure-Compute in der Region „USA, Westen/Osten“ ausgeführt wird.

### <a name="imports-and-environment"></a>Importe und Umgebung

```python
import os
import tempfile
import shutil
import urllib
import zipfile
import pandas as pd

# Temporary folder for data we need during execution of this notebook (we'll clean up
# at the end, we promise)
temp_dir = os.path.join(tempfile.gettempdir(), 'mind')
os.makedirs(temp_dir, exist_ok=True)

# The dataset is split into training and validation set, each with a large and small version.
# The format of the four files are the same.
# For demonstration purpose, we will use small version validation set only.
base_url = 'https://mind201910small.blob.core.windows.net/release'
training_small_url = f'{base_url}/MINDsmall_train.zip'
validation_small_url = f'{base_url}/MINDsmall_dev.zip'
training_large_url = f'{base_url}/MINDlarge_train.zip'
validation_large_url = f'{base_url}/MINDlarge_dev.zip'
```

### <a name="functions"></a>Functions


```python
def download_url(url,
                 destination_filename=None,
                 progress_updater=None,
                 force_download=False,
                 verbose=True):
    """
    Download a URL to a temporary file
    """
    if not verbose:
        progress_updater = None
    # This is not intended to guarantee uniqueness, we just know it happens to guarantee
    # uniqueness for this application.
    if destination_filename is None:
        url_as_filename = url.replace('://', '_').replace('/', '_')
        destination_filename = \
            os.path.join(temp_dir,url_as_filename)
    if (not force_download) and (os.path.isfile(destination_filename)):
        if verbose:
            print('Bypassing download of already-downloaded file {}'.format(
                os.path.basename(url)))
        return destination_filename
    if verbose:
        print('Downloading file {} to {}'.format(os.path.basename(url),
                                                 destination_filename),
              end='')
    urllib.request.urlretrieve(url, destination_filename, progress_updater)
    assert (os.path.isfile(destination_filename))
    nBytes = os.path.getsize(destination_filename)
    if verbose:
        print('...done, {} bytes.'.format(nBytes))
    return destination_filename
```

### <a name="download-and-extract-the-files"></a>Herunterladen und Extrahieren der Dateien

```python
# For demonstration purpose, we will use small version validation set only.
# This file is about 30MB.
zip_path = download_url(validation_small_url, verbose=True)
with zipfile.ZipFile(zip_path, 'r') as zip_ref:
    zip_ref.extractall(temp_dir)

os.listdir(temp_dir)
```

### <a name="read-the-files-with-pandas"></a>Lesen der Dateien mit Pandas

```python
# The behaviors.tsv file contains the impression logs and users' news click histories. 
# It has 5 columns divided by the tab symbol:
# - Impression ID. The ID of an impression.
# - User ID. The anonymous ID of a user.
# - Time. The impression time with format "MM/DD/YYYY HH:MM:SS AM/PM".
# - History. The news click history (ID list of clicked news) of this user before this impression.
# - Impressions. List of news displayed in this impression and user's click behaviors on them (1 for click and 0 for non-click).
behaviors_path = os.path.join(temp_dir, 'behaviors.tsv')
pd.read_table(
    behaviors_path,
    header=None,
    names=['impression_id', 'user_id', 'time', 'history', 'impressions'])
```

```python
# The news.tsv file contains the detailed information of news articles involved in the behaviors.tsv file.
# It has 7 columns, which are divided by the tab symbol:
# - News ID
# - Category
# - Subcategory
# - Title
# - Abstract
# - URL
# - Title Entities (entities contained in the title of this news)
# - Abstract Entities (entities contained in the abstract of this news)
news_path = os.path.join(temp_dir, 'news.tsv')
pd.read_table(news_path,
              header=None,
              names=[
                  'id', 'category', 'subcategory', 'title', 'abstract', 'url',
                  'title_entities', 'abstract_entities'
              ])
```

```python
# The entity_embedding.vec file contains the 100-dimensional embeddings
# of the entities learned from the subgraph by TransE method.
# The first column is the ID of entity, and the other columns are the embedding vector values.
entity_embedding_path = os.path.join(temp_dir, 'entity_embedding.vec')
entity_embedding = pd.read_table(entity_embedding_path, header=None)
entity_embedding['vector'] = entity_embedding.iloc[:, 1:101].values.tolist()
entity_embedding = entity_embedding[[0,
                                     'vector']].rename(columns={0: "entity"})
entity_embedding
```

```python
# The relation_embedding.vec file contains the 100-dimensional embeddings
# of the relations learned from the subgraph by TransE method.
# The first column is the ID of relation, and the other columns are the embedding vector values.
relation_embedding_path = os.path.join(temp_dir, 'relation_embedding.vec')
relation_embedding = pd.read_table(relation_embedding_path, header=None)
relation_embedding['vector'] = relation_embedding.iloc[:,
                                                       1:101].values.tolist()
relation_embedding = relation_embedding[[0, 'vector'
                                         ]].rename(columns={0: "relation"})
relation_embedding
```

### <a name="clean-up-temporary-files"></a>Bereinigen der temporären Dateien

```python
shutil.rmtree(temp_dir)
```

<!-- nbend -->

---

## <a name="examples"></a>Beispiele

Sehen Sie sich die folgenden Beispiele für die Verwendung des Microsoft News Recommender-Datasets an:

- [MIND News Recommendation Challenge](https://www.microsoft.com/research/?post_type=msr-academic-program&p=676692&secret=hCWU3V)
- [Microsoft Recommenders-Repository](https://github.com/microsoft/recommenders)
- [MIND: Microsoft News Dataset](https://msnews.github.io/index.html)


## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich verschiedene Baselinemodelle für Nachrichtenempfehlungen an, die für MIND aus dem [Microsoft Recommenders-Repository](https://github.com/microsoft/recommenders) entwickelt wurden.


Machen Sie sich mit den restlichen Datasets im [Open Datasets-Katalog](dataset-catalog.md) vertraut.
