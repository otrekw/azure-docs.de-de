---
title: Identitätsbasierter Datenzugriff auf Speicherdienste in Azure
titleSuffix: Azure Machine Learning
description: Es wird beschrieben, wie Sie den identitätsbasierten Datenzugriff nutzen, um eine Verbindung mit Speicherdiensten in Azure herzustellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
ms.reviewer: nibaccam
ms.date: 02/22/2021
ms.custom: how-to, contperf-fy21q1, devx-track-python, data4ml
ms.openlocfilehash: 68d07481e228b1d1b2f4571a783f925add261cff
ms.sourcegitcommit: 956dec4650e551bdede45d96507c95ecd7a01ec9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2021
ms.locfileid: "102520012"
---
# <a name="connect-to-storage-with-identity-based-data-access-preview"></a>Herstellen einer Verbindung mit Speicher mit identitätsbasiertem Datenzugriff (Vorschau)

>[!IMPORTANT]
> Die in diesem Artikel beschriebenen Funktionen befinden sich in der Vorschauphase und sollten als [experimentelle](/python/api/overview/azure/ml/#stable-vs-experimental) Previewfunktionen angesehen werden, die sich im Laufe der Zeit ändern können.

In diesem Artikel erfahren Sie, wie Sie eine Verbindung mit Speicherdiensten in Azure per identitätsbasiertem Datenzugriff und Azure Machine Learning-Datenspeicher herstellen, indem Sie das [Python-SDK für Azure Machine Learning](/python/api/overview/azure/ml/intro) verwenden.  

Für Datenspeicher wird normalerweise der auf Anmeldeinformationen basierende Datenzugriff genutzt, um zu verifizieren, dass Sie über die Berechtigung zum Zugreifen auf den Speicherdienst verfügen. Verbindungsinformationen, z. B. Ihre Abonnement-ID und Tokenautorisierung, werden in Ihrem [Schlüsseltresor](https://azure.microsoft.com/services/key-vault/) gespeichert, der dem Arbeitsbereich zugeordnet ist. Wenn Sie einen Datenspeicher erstellen, für den der identitätsbasierte Zugriff verwendet wird, wird anhand Ihrer Azure-Anmeldung ([Azure Active Directory-Token](../active-directory/fundamentals/active-directory-whatis.md)) verifiziert, ob Sie über die Berechtigung für den Zugriff auf den Speicherdienst verfügen. In diesem Szenario werden keine Anmeldeinformationen für die Authentifizierung gespeichert, sondern nur die Speicherkontoinformationen im Datenspeicher. 

Informationen zum Erstellen von Datenspeichern, für die die Authentifizierung mit Anmeldeinformationen genutzt wird, z. B. über Zugriffsschlüssel oder Dienstprinzipale, finden Sie unter [Herstellen einer Verbindung mit Speicherdiensten in Azure](how-to-access-data.md).

## <a name="identity-based-data-access-in-azure-machine-learning"></a>Identitätsbasierter Datenzugriff in Azure Machine Learning

Es gibt zwei Bereiche für die Anwendung des identitätsbasierten Datenzugriffs in Azure Machine Learning. Dies gilt besonders, wenn vertrauliche Daten genutzt werden und eine präzisere Verwaltung des Datenzugriffs erforderlich ist. 

1. Zugreifen auf Speicherdienste
1. Trainieren von Machine Learning-Modellen mit privaten Daten

### <a name="accessing-storage-services"></a>Zugreifen auf Speicherdienste

Sie können eine Verbindung mit Speicherdiensten per identitätsbasiertem Zugriff herstellen, indem Sie Azure Machine Learning-Datenspeicher oder [Azure Machine Learning-Datasets](how-to-create-register-datasets.md) nutzen. 

Normalerweise werden Ihre Anmeldeinformationen für die Authentifizierung in einem Datenspeicher aufbewahrt, mit dem sichergestellt wird, dass Sie über die Berechtigung für den Zugriff auf den Speicherdienst verfügen. Wenn diese Anmeldeinformationen bei Datenspeichern registriert werden, können diese von allen Benutzern mit der Rolle *Leser* für Arbeitsbereiche abgerufen werden. In einigen Organisationen kann dies ein Sicherheitsproblem darstellen. [Erfahren Sie mehr zur Rolle *Leser* für Arbeitsbereiche](how-to-assign-roles.md#default-roles). 

Wenn Sie den identitätsbasierten Datenzugriff nutzen, werden Sie von Azure Machine Learning zum Eingeben Ihres Azure Active Directory-Tokens für die Authentifizierung für den Datenzugriff aufgefordert, und Ihre Anmeldeinformationen werden nicht im Datenspeicher aufbewahrt. Dies ermöglicht die Verwaltung des Datenzugriffs auf Speicherebene und sorgt für die Vertraulichkeit der Anmeldeinformationen. 

Dieses Verhalten gilt auch für das

* [Erstellen eines Datasets direkt aus Speicher-URLs](#use-data-in-storage). 
* Nutzen Sie Daten interaktiv über ein Jupyter Notebook auf Ihrem lokalen Computer bzw. Ihrer [Compute-Instanz](concept-compute-instance.md).

> [!NOTE]
> Bei der Authentifizierung mit Anmeldeinformationen wird Folgendes gespeichert: Abonnement-ID, SAS-Token (Shared Access Signature), Speicherzugriffsschlüssel und Dienstprinzipalinformationen, z. B. Client-ID und Mandanten-ID.

### <a name="model-training-on-private-data"></a>Modelltraining für private Daten

Bei bestimmten Machine Learning-Szenarien werden Modelle auch mit privaten Daten trainiert. In diesen Fällen müssen wissenschaftliche Fachkräfte für Daten (Data Scientists) Trainingsworkflows ausführen, ohne dass vertrauliche Eingabedaten verfügbar gemacht werden. Bei diesem Szenario wird eine verwaltete Identität der Trainingscomputeressource für die Authentifizierung des Datenzugriffs verwendet. Auf diese Weise können Speicheradministratoren dem **Leser von Speicherblobdaten** Zugriff auf die verwaltete Identität gewähren, die von der Trainingscomputeressource zum Ausführen des Trainingsauftrags verwendet wird. Dieser Schritt muss dann nicht von den einzelnen Data Scientists durchgeführt werden. Informieren Sie sich über das [Einrichten einer verwalteten Identität auf einer Compute-Instanz](how-to-create-attach-compute-cluster.md#managed-identity).


## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) aus.

- Ein Azure Storage-Konto mit einem unterstützten Speichertyp. Die folgenden Speichertypen werden für die Vorschauversion unterstützt. 
    - [Azure Blob Storage](../storage/blobs/storage-blobs-overview.md)
    - [Azure Data Lake Gen1](../data-lake-store/index.yml)
    - [Azure Data Lake Gen2](../storage/blobs/data-lake-storage-introduction.md)
    - [Azure SQL-Datenbank](../azure-sql/database/sql-database-paas-overview.md)

- Das [Azure Machine Learning SDK für Python](/python/api/overview/azure/ml/install).

- Ein Azure Machine Learning-Arbeitsbereich.
  
  Sie können wahlweise [einen Azure Machine Learning-Arbeitsbereich erstellen](how-to-manage-workspace.md) oder [mithilfe des Python SDKs einen vorhandenen Arbeitsbereich verwenden](how-to-manage-workspace.md#connect-to-a-workspace). 

## <a name="storage-access-permissions"></a>Berechtigungen für den Speicherzugriff

Um sicherzustellen, dass eine sichere Verbindung mit Ihrem Speicherdienst in Azure hergestellt wird, müssen Sie für Azure Machine Learning über die Berechtigung zum Zugreifen auf den entsprechenden Datenspeicher verfügen.

Beim identitätsbasierten Datenzugriff werden nur Verbindungen mit den folgenden Speicherdiensten unterstützt:

* Azure Blob Storage
* Azure Data Lake Generation 1
* Azure Data Lake Generation 2
* Azure SQL-Datenbank

Für den Zugriff auf diese Speicherdienste benötigen Sie mindestens die Zugriffsberechtigung **Leser von Speicherblobdaten**. Erfahren Sie mehr über [Storage-Blobdatenleser](../role-based-access-control/built-in-roles.md#storage-blob-data-reader). Nur Speicherkontobesitzer können [Ihre Zugriffsebene über das Azure-Portal ändern](../storage/common/storage-auth-aad-rbac-portal.md).

Wenn Sie ein Modell auf einem Remotecomputeziel trainieren, muss der Compute-Identität vom Speicherdienst mindestens die Rolle **Leser von Speicherblobdaten** gewährt werden. Informieren Sie sich über das [Einrichten einer verwalteten Identität auf einer Compute-Instanz](how-to-create-attach-compute-cluster.md#managed-identity).

## <a name="work-with-virtual-networks"></a>Verwenden von virtuellen Netzwerken

Standardmäßig kann Azure Machine Learning nicht mit einem Speicherkonto kommunizieren, das sich hinter einer Firewall oder in einem virtuellen Netzwerk befindet.

Speicherkonten können so konfiguriert werden, dass der Zugriff nur aus bestimmten virtuellen Netzwerken zulässig ist. Hierfür müssen zusätzliche Konfigurationen durchgeführt werden, um sicherzustellen, dass die Daten innerhalb des Netzwerks bleiben. Dieses Verhalten ist für den Datenzugriff über Anmeldeinformationen identisch. Informieren Sie sich darüber, [welche Konfigurationen erforderlich sind und wie sie in Szenarien mit virtuellen Netzwerken angewendet werden](how-to-access-data.md#virtual-network). 

## <a name="create-and-register-datastores"></a>Erstellen und Registrieren von Datenspeichern

Wenn Sie einen Speicherdienst in Azure als Datenspeicher registrieren, erstellen Sie diesen Datenspeicher automatisch und registrieren ihn in einem bestimmten Arbeitsbereich. Eine Anleitung zu den erforderlichen Berechtigungstypen finden Sie im Abschnitt [Berechtigungen für den Speicherzugriff](#storage-access-permissions), und Informationen zur Verbindungsherstellung mit einem Datenspeicher hinter virtuellen Netzwerken finden Sie im Abschnitt [Verwenden von virtuellen Netzwerken](#work-with-virtual-networks).

Beachten Sie im folgenden Code das Fehlen der Authentifizierungsparameter, z. B. `sas_token`, `account_key`, `subscription_id` oder `client_id` für den Dienstprinzipal. Das Fehlen dieser Parameter ist ein Hinweis darauf, dass Azure Machine Learning den identitätsbasierten Datenzugriff für die Authentifizierung verwendet. Da die Erstellung von Datenspeichern normalerweise interaktiv in einem Notebook oder per Studio erfolgt, wird Ihr Azure Active Directory-Token für die Authentifizierung des Datenzugriffs genutzt.

> [!NOTE]
> Datenspeichernamen dürfen nur Kleinbuchstaben, Ziffern und Unterstriche enthalten. 

### <a name="azure-blob-container"></a>Azure-Blobcontainer

Um einen Azure-Blobcontainer als Datenspeicher zu registrieren, verwenden Sie [`register_azure_blob_container()`](/python/api/azureml-core/azureml.core.datastore%28class%29#register-azure-blob-container-workspace--datastore-name--container-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false--blob-cache-timeout-none--grant-workspace-access-false--subscription-id-none--resource-group-none-).

Mit dem folgenden Code wird der Datenspeicher `credentialless_blob` erstellt und beim Arbeitsbereich `ws` registriert. Anschließend wird er der Variablen `blob_datastore` zugewiesen. Dieser Datenspeicher greift auf den Blobcontainer `my_container_name` unter dem Speicherkonto `my-account-name` zu.

```Python
# create blob datastore without credentials
blob_datastore = Datastore.register_azure_blob_container(workspace=ws,
                                                      datastore_name='credentialless_blob',
                                                      container_name='my_container_name',
                                                      account_name='my_account_name')
```

### <a name="azure-data-lake-storage-generation-1"></a>Azure Data Lake Storage Generation 1

Verwenden Sie für einen Datenspeicher vom Typ „Azure Data Lake Storage Generation 1“ (ADLS Gen1) [register_azure_data_lake()](/python/api/azureml-core/azureml.core.datastore.datastore#register-azure-data-lake-workspace--datastore-name--store-name--tenant-id-none--client-id-none--client-secret-none--resource-url-none--authority-url-none--subscription-id-none--resource-group-none--overwrite-false--grant-workspace-access-false-), um einen Datenspeicher zu registrieren, mit dem eine Verbindung mit einem Speicher vom Typ „Azure Data Lake Generation 1“ hergestellt wird.

Mit dem folgenden Code wird der Datenspeicher `credentialless_adls1` erstellt und beim Arbeitsbereich `workspace` registriert. Anschließend wird er der Variablen `adls_dstore` zugewiesen. Dieser Datenspeicher greift auf das Azure Data Lake Store-Speicherkonto `adls_storage` zu.

```Python
# create adls gen1 without credentials
adls_dstore = Datastore.register_azure_data_lake(workspace = workspace,
                                                 datastore_name='credentialless_adls1',
                                                 store_name='adls_storage')

```

### <a name="azure-data-lake-storage-generation-2"></a>Azure Data Lake Storage Generation 2

Verwenden Sie für einen Datenspeicher vom Typ „Azure Data Lake Storage Generation 2“ (ADLS Gen2) [register_azure_data_lake_gen2()](/python/api/azureml-core/azureml.core.datastore.datastore#register-azure-data-lake-gen2-workspace--datastore-name--filesystem--account-name--tenant-id--client-id--client-secret--resource-url-none--authority-url-none--protocol-none--endpoint-none--overwrite-false-), um einen Datenspeicher zu registrieren, mit dem eine Verbindung mit einem Speicher vom Typ „Azure Data Lake Gen2“ hergestellt wird.

Mit dem folgenden Code wird der Datenspeicher `credentialless_adls2` erstellt und beim Arbeitsbereich `ws` registriert. Anschließend wird er der Variablen `adls2_dstore` zugewiesen. Dieser Datenspeicher greift auf das Dateisystem `tabular` unter dem Speicherkonto `myadls2` zu.  

```python
# createn adls2 datastore without credentials
adls2_dstore = Datastore.register_azure_data_lake_gen2(workspace=ws, 
                                                       datastore_name='credentialless_adls2', 
                                                       filesystem='tabular', 
                                                       account_name='myadls2')
```

## <a name="use-data-in-storage"></a>Verwenden von Daten im Speicher

[Azure Machine Learning-Datasets](how-to-create-register-datasets.md) sind die empfohlene Vorgehensweise für die Interaktion mit Ihren Daten im Speicher per Azure Machine Learning. 

Datasets packen Ihre Daten in ein selten ausgewertetes Objekt für Aufgaben des maschinellen Lernens wie das Training. Mithilfe von Datasets können Sie zudem Dateien in allen Formaten aus Azure-Speicherdiensten, z. B. Azure Blob Storage und Azure Data Lakes, auf ein Computeziel [herunterladen oder darin einbinden](how-to-train-with-datasets.md#mount-vs-download).


Für die **Erstellung von Datasets mit identitätsbasiertem Zugriff** haben Sie die folgenden Optionen. Bei dieser Art der Dataseterstellung wird Ihr Azure Active Directory-Token für die Authentifizierung des Datenzugriffs genutzt. 

*  Verweisen auf Pfade aus Datenspeichern, für die auch der identitätsbasierte Datenzugriff verwendet wird: 
<br>Im folgenden Beispiel wurde zuvor `blob_datastore` über den identitätsbasierten Datenzugriff erstellt.   

    ```python
    blob_dataset = Dataset.Tabular.from_delimited_files(blob_datastore,'test.csv') 
    ```

* Überspringen der Datenspeichererstellung und direktes Erstellen von Datasets aus Speicher-URLs: Derzeit werden für diese Funktionen nur Azure-Blobs und Azure Data Lake Storage Gen1 und Gen2 unterstützt.

    ```python
    blob_dset = Dataset.File.from_files('https://myblob.blob.core.windows.net/may/keras-mnist-fashion/')
    ```

**Wenn Sie aber einen Trainingsauftrag übermitteln, für den ein Dataset genutzt wird, das per identitätsbasiertem Datenzugriff erstellt wurde**, gilt Folgendes: Für die Authentifizierung des Datenzugriffs wird nicht Ihr Azure Active Directory-Token verwendet, sondern die verwaltete Identität der Trainingscomputeressource. Stellen Sie bei diesem Szenario sicher, dass der verwalteten Identität der Computeressource vom Speicherdienst mindestens die Rolle **Leser von Speicherblobdaten** gewährt wird. Informieren Sie sich über das [Einrichten einer verwalteten Identität auf einer Compute-Instanz](how-to-create-attach-compute-cluster.md#managed-identity). 

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen Sie ein Azure Machine Learning-Dataset](how-to-create-register-datasets.md).
* [Trainieren Sie mit Datasets](how-to-train-with-datasets.md).
* [Erstellen eines Datenspeichers mit schlüsselbasiertem Datenzugriff](how-to-access-data.md)
