---
title: ClinVar Annotations
titleSuffix: Azure Open Datasets
description: Hier erfahren Sie, wie Sie das Dataset „ClinVar-Anmerkungen“ in Azure Open Datasets verwenden.
ms.service: open-datasets
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 04/16/2021
ms.openlocfilehash: 384eb565882cf5f62cec1c6d38769647a82ec712
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112038869"
---
# <a name="clinvar-annotations"></a>ClinVar Annotations

[ClinVar](https://www.ncbi.nlm.nih.gov/clinvar/) ist ein kostenloses, öffentliches Archiv mit Berichten zu den Beziehungen zwischen menschlichen Variationen und Phänotypen und unterstützenden Beweisen. Es vereinfacht den Zugriff auf und die Kommunikation über Beziehungen, die zwischen menschlichen Variationen und dem beobachteten Gesundheitszustand zugeschrieben werden, sowie auf die Interpretationsgeschichte. Zudem bietet es Zugriff auf eine größere Anzahl klinischer Interpretationen, die in Workflows und Anwendungen zur Genomik integriert werden können.

Weitere Informationen zu den Daten finden Sie im [Datenwörterbuch](https://www.ncbi.nlm.nih.gov/projects/clinvar/ClinVarDataDictionary.pdf) und in den [Häufig gestellten Fragen (FAQ)](https://www.ncbi.nlm.nih.gov/clinvar/docs/faq/).

[!INCLUDE [Open Dataset usage notice](../../includes/open-datasets-usage-note.md)]

## <a name="data-source"></a>Datenquelle

Dieses Dataset ist eine Spiegelung von ftp://ftp.ncbi.nlm.nih.gov/pub/clinvar/xml/.

## <a name="data-volumes-and-update-frequency"></a>Datenvolumes und Aktualisierungshäufigkeit

Dieses Dataset enthält etwa 56 GB Daten und wird täglich aktualisiert.

## <a name="storage-location"></a>Speicherort

Dieses Dataset wird in den Azure-Regionen „USA, Westen 2“ und „USA, Westen-Mitte“ gespeichert. Aus Gründen der Affinität wird die Zuweisung von Computeressourcen in die Regionen „USA, Westen 2“ oder „USA, Westen-Mitte“ empfohlen.

## <a name="data-access"></a>Datenzugriff

USA, Westen 2: https://datasetclinvar.blob.core.windows.net/dataset

USA, Westen-Mitte: https://datasetclinvar-secondary.blob.core.windows.net/dataset

[SAS Token](../storage/common/storage-sas-overview.md): sv=2019-02-02&se=2050-01-01T08%3A00%3A00Z&si=prod&sr=c&sig=qFPPwPba1RmBvaffkzkLuzabYU5dZstSTgMwxuLNME8%3D

## <a name="use-terms"></a>Nutzungsbedingungen
Die Daten stehen uneingeschränkt zur Verfügung. Weitere Informationen und Zitatdetails finden Sie unter [Zugreifen auf und Verwenden von Daten in ClinVar](https://www.ncbi.nlm.nih.gov/clinvar/docs/maintenance_use/).

## <a name="contact"></a>Contact

Bei Fragen oder Feedback zu diesem Dataset wenden Sie sich an clinvar@ncbi.nlm.nih.gov.

## <a name="data-access"></a>Datenzugriff

### <a name="azure-notebooks"></a>Azure Notebooks

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=genomics-clinvar -->

> [!TIP]
> **[Laden Sie das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=genomics-clinvar)** .

## <a name="getting-the-clinvar-data-from-azure-open-dataset"></a>Abrufen der ClinVar-Daten aus Azure Open Dataset

[Hier](https://azure.microsoft.com/services/open-datasets/catalog/) finden Sie eine Reihe von öffentlichen Genomikdaten, die in Azure Open Dataset hochgeladen wurden. Es wurde ein Blob-Dienst erstellt, der mit dieser Open Dataset-Instanz verknüpft ist. Beispiele zur Vorgehensweise beim Aufrufen von Daten aus Azure Open Dataset für das Dataset `ClinVar` finden Sie unten.

Benutzer können mit diesem Notebook den folgenden Pfad aufrufen und herunterladen: https://datasetclinvar.blob.core.windows.net/dataset/ClinVarFullRelease_00-latest.xml.gz.md5.

> [!NOTE]
> Zum Anzeigen der Daten mit dem Azure ML SDK müssen sich Benutzer über die Azure CLI bei ihrem Azure-Konto anmelden. Zum Herunterladen der Daten sind jedoch keine Aktionen erforderlich.

Weitere Informationen zum Installieren der Azure CLI finden Sie unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

### <a name="calling-the-data-from--clinvar-data-set"></a>Aufrufen der Daten aus „ClinVar Data Set“

```python
import azureml.core
print("Azure ML SDK Version: ", azureml.core.VERSION)
```

```python
from azureml.core import  Dataset
reference_dataset = Dataset.File.from_files('https://datasetclinvar.blob.core.windows.net/dataset')
mount = reference_dataset.mount()
```

```python
import os

REF_DIR = '/dataset'
path = mount.mount_point + REF_DIR

with mount:
    print(os.listdir(path))
```

```python
import pandas as pd

# create mount context
mount.start()

# specify path to README file
REF_DIR = '/dataset'
metadata_filename = '{}/{}/{}'.format(mount.mount_point, REF_DIR, '_README')

# read README file
metadata = pd.read_table(metadata_filename)
metadata
```

### <a name="download-the-specific-file"></a>Herunterladen der Datei

```python
import os
import uuid
import sys
from azure.storage.blob import BlockBlobService, PublicAccess

blob_service_client = BlockBlobService(account_name='datasetclinvar', sas_token='sv=2019-02-02&se=2050-01-01T08%3A00%3A00Z&si=prod&sr=c&sig=qFPPwPba1RmBvaffkzkLuzabYU5dZstSTgMwxuLNME8%3D')     
blob_service_client.get_blob_to_path('dataset', 'ClinVarFullRelease_00-latest.xml.gz.md5', './ClinVarFullRelease_00-latest.xml.gz.md5')
```

<!-- nbend -->

---

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich mit den restlichen Datasets im [Open Datasets-Katalog](dataset-catalog.md) vertraut.