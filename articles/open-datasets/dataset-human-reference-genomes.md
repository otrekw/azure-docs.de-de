---
title: Menschliche Referenzgenome
titleSuffix: Azure Open Datasets
description: Erfahren Sie, wie Sie das Dataset für menschliche Referenzgenome in Azure Open Datasets verwenden können.
ms.service: open-datasets
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 04/16/2021
ms.openlocfilehash: 1e1b1fa785d4ab91329216983c5b694cc484860a
ms.sourcegitcommit: 8b7d16fefcf3d024a72119b233733cb3e962d6d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2021
ms.locfileid: "114297722"
---
# <a name="human-reference-genomes"></a>Menschliche Referenzgenome

Dieses Dataset enthält zwei Humangenomreferenzen, die vom [Genome Reference Consortium](https://www.ncbi.nlm.nih.gov/grc) zusammengestellt wurden: Hg19 und Hg38.

Weitere Informationen zu Hg19-Daten (GRCh37) finden Sie im [Bericht zu GRCh37 des NCBI](https://www.ncbi.nlm.nih.gov/assembly/GCF_000001405.13/).

Weitere Informationen zu Hg38-Daten finden Sie im [Bericht zu GRCh38 des NCBI](http://www.ncbi.nlm.nih.gov/assembly/GCF_000001405.26/).

Weitere Details zu den Daten finden Sie unter [NCBI RefSeq](https://www.ncbi.nlm.nih.gov/refseq/).

[!INCLUDE [Open Dataset usage notice](../../includes/open-datasets-usage-note.md)]

## <a name="data-source"></a>Datenquelle

Dieses Dataset stammt aus zwei FTP-Quellen:

ftp://ftp.ncbi.nih.gov/genomes/refseq/vertebrate_mammalian/Homo_sapiens/all_assembly_versions/GCF_000001405.25_GRCh37.p13/

ftp://ftp.ncbi.nih.gov/genomes/refseq/vertebrate_mammalian/Homo_sapiens/latest_assembly_versions/GCF_000001405.39_GRCh38.p13/

Blobnamen wird der URI-Bestandteil „vertebrate_mammalian“ vorangestellt.

## <a name="data-volumes-and-update-frequency"></a>Datenvolumes und Aktualisierungshäufigkeit

Dieses Dataset enthält etwa 10 GB Daten und wird täglich aktualisiert.

## <a name="storage-location"></a>Speicherort

Dieses Dataset wird in den Azure-Regionen „USA, Westen 2“ und „USA, Westen-Mitte“ gespeichert. Aus Gründen der Affinität wird die Zuweisung von Computeressourcen in die Regionen „USA, Westen 2“ oder „USA, Westen-Mitte“ empfohlen.

## <a name="data-access"></a>Datenzugriff

USA, Westen 2: ‚ https://datasetreferencegenomes.blob.core.windows.net/dataset ‘

USA, Westen-Mitte: ‚ https://datasetreferencegenomes-secondary.blob.core.windows.net/dataset ‘

[SAS Token](../storage/common/storage-sas-overview.md): sv=2019-02-02&se=2050-01-01T08%3A00%3A00Z&si=prod&sr=c&sig=JtQoPFqiC24GiEB7v9zHLi4RrA2Kd1r%2F3iFt2l9%2FlV8%3D

## <a name="use-terms"></a>Nutzungsbedingungen

Die Daten stehen uneingeschränkt zur Verfügung. Weitere Informationen und Zitatdetails finden Sie auf der [Website der NCBI-Referenzsequenzdatenbank](https://www.ncbi.nlm.nih.gov/refseq/).

## <a name="contact"></a>Contact

Wenn Sie Fragen oder Feedback zu diesem Dataset haben, wenden Sie sich an das [Genome Reference Consortium](https://www.ncbi.nlm.nih.gov/grc/contact-us).

## <a name="data-access"></a>Datenzugriff

### <a name="azure-notebooks"></a>Azure Notebooks

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=genomics-reference-genomes -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=genomics-reference-genomes)** .

## <a name="getting-the-reference-genomes-from-azure-open-datasets"></a>Abrufen der Referenzgenome aus Azure Open Datasets

[Hier](https://azure.microsoft.com/services/open-datasets/catalog/) finden Sie eine Reihe von öffentlichen Genomikdaten, die in Azure Open Dataset hochgeladen wurden. Es wurde ein Blob-Dienst erstellt, der mit Open Dataset verknüpft ist. Beispiele zur Vorgehensweise beim Aufrufen von Daten aus Azure Open Datasets für das Dataset `Reference Genomes` finden Sie unten.

Die Benutzer können mit diesem Notebook den folgenden Pfad aufrufen und herunterladen: ‚ https://datasetreferencegenomes.blob.core.windows.net/dataset/vertebrate_mammalian/Homo_sapiens/latest_assembly_versions/GCF_000001405.39_GRCh38.p13/GCF_000001405.39_GRCh38.p13_assembly_structure/genomic_regions_definitions.txt ‘

**Wichtiger Hinweis:** Zum Anzeigen der Daten mit dem Azure ML SDK müssen sich Benutzer über die Azure CLI bei ihrem Azure-Konto anmelden. Zum Herunterladen der Daten sind jedoch keine Aktionen erforderlich.

[Installieren Sie die Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli).

### <a name="calling-the-data-from--reference-genome-datasets"></a>Aufrufen der Daten aus „Referenzgenom-Datasets“

```python
import azureml.core
print("Azure ML SDK Version: ", azureml.core.VERSION)
```

```python
from azureml.core import  Dataset
reference_dataset = Dataset.File.from_files('https://datasetreferencegenomes.blob.core.windows.net/dataset')
mount = reference_dataset.mount()
```

```python
import os

REF_DIR = '/vertebrate_mammalian/Homo_sapiens/latest_assembly_versions/GCF_000001405.39_GRCh38.p13/GCF_000001405.39_GRCh38.p13_assembly_structure'
path = mount.mount_point + REF_DIR

with mount:
    print(os.listdir(path))
```

```python
import pandas as pd

# create mount context
mount.start()

# specify path to genomic_regions_definitions.txt file
REF_DIR = 'vertebrate_mammalian/Homo_sapiens/latest_assembly_versions/GCF_000001405.39_GRCh38.p13/GCF_000001405.39_GRCh38.p13_assembly_structure'
metadata_filename = '{}/{}/{}'.format(mount.mount_point, REF_DIR, 'genomic_regions_definitions.txt')

# read genomic_regions_definitions.txt file
metadata = pd.read_table(metadata_filename)
metadata
```

### <a name="download-the-specific-file"></a>Herunterladen der Datei

```python
import os
import uuid
import sys
from azure.storage.blob import BlockBlobService, PublicAccess

blob_service_client = BlockBlobService(account_name='datasetreferencegenomes',sas_token='sv=2019-02-02&se=2050-01-01T08%3A00%3A00Z&si=prod&sr=c&sig=JtQoPFqiC24GiEB7v9zHLi4RrA2Kd1r%2F3iFt2l9%2FlV8%3D')     
blob_service_client.get_blob_to_path('dataset/vertebrate_mammalian/Homo_sapiens/latest_assembly_versions/GCF_000001405.39_GRCh38.p13/GCF_000001405.39_GRCh38.p13_assembly_structure', 'genomic_regions_definitions.txt', './genomic_regions_definitions.txt')
```

<!-- nbend -->

---

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich mit den restlichen Datasets im [Open Datasets-Katalog](dataset-catalog.md) vertraut.