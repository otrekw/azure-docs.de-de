---
title: Azure Data Lake Storage Gen2 Python SDK für Dateien und Zugriffssteuerungslisten
description: Verwenden Sie Python zum Verwalten von Verzeichnissen, Dateien und Zugriffssteuerungslisten (ACLs) in Speicherkonten, bei denen der hierarchische Namespace (HNS) aktiviert wurde.
author: normesta
ms.service: storage
ms.date: 01/22/2021
ms.author: normesta
ms.topic: how-to
ms.subservice: data-lake-storage-gen2
ms.reviewer: prishet
ms.custom: devx-track-python
ms.openlocfilehash: 5036930c7bb49578582fbc1b347b11518579b53e
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2021
ms.locfileid: "98740617"
---
# <a name="use-python-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2"></a>Verwenden von Python zum Verwalten von Verzeichnissen, Dateien und Zugriffssteuerungslisten in Azure Data Lake Storage Gen2

In diesem Artikel erfahren Sie, wie Sie mithilfe von Python Verzeichnisse, Dateien und Berechtigungen in Speicherkonten erstellen und verwalten, bei denen der hierarchische Namespace (HNS) aktiviert wurde. 

[Paket (Python-Paketindex)](https://pypi.org/project/azure-storage-file-datalake/) | [Beispiele](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/samples) | [API-Referenz](/python/api/azure-storage-file-datalake/azure.storage.filedatalake) | [Zuordnung von Gen1 zu Gen2](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/GEN1_GEN2_MAPPING.md) | [Feedback geben](https://github.com/Azure/azure-sdk-for-python/issues)

## <a name="prerequisites"></a>Voraussetzungen

> [!div class="checklist"]
> * Ein Azure-Abonnement. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
> * Ein Speicherkonto, für das der hierarchische Namespace aktiviert ist. Befolgen Sie [diese Anleitung](../common/storage-account-create.md) für die Erstellung.

## <a name="set-up-your-project"></a>Einrichten des Projekts

Installieren Sie die Azure Data Lake Storage-Clientbibliothek für Python mithilfe von [pip](https://pypi.org/project/pip/).

```
pip install azure-storage-file-datalake
```

Fügen Sie diese Importanweisungen am Anfang Ihrer Codedatei hinzu.

```python
import os, uuid, sys
from azure.storage.filedatalake import DataLakeServiceClient
from azure.core._match_conditions import MatchConditions
from azure.storage.filedatalake._models import ContentSettings
```

## <a name="connect-to-the-account"></a>Herstellen einer Verbindung mit dem Konto

Wenn Sie die Codeausschnitte in diesem Artikel verwenden möchten, müssen Sie eine **DataLakeServiceClient**-Instanz erstellen, die das Speicherkonto darstellt. 

### <a name="connect-by-using-an-account-key"></a>Herstellen einer Verbindung per Kontoschlüssel

Dies ist die einfachste Möglichkeit, eine Verbindung mit einem Konto herzustellen. 

In diesem Beispiel wird eine **DataLakeServiceClient**-Instanz mithilfe eines Kontoschlüssels erstellt.

:::code language="python" source="~/azure-storage-snippets/blobs/howto/python/python-v12/crud_datalake.py" id="Snippet_AuthorizeWithKey":::
 
- Ersetzen Sie den Platzhalterwert `storage_account_name` durch den Namen Ihres Speicherkontos.

- Ersetzen Sie den Platzhalterwert `storage_account_key` durch den Zugriffsschlüssel Ihres Speicherkontos.

### <a name="connect-by-using-azure-active-directory-ad"></a>Herstellen einer Verbindung mit Azure Active Directory (AD)

Sie können die [Azure-Identitätsclientbibliothek für Python](https://pypi.org/project/azure-identity/) verwenden, um Ihre Anwendung bei Azure AD zu authentifizieren.

In diesem Beispiel wird eine **DataLakeServiceClient**-Instanz mithilfe einer Client-ID, eines Clientgeheimnisses und einer Mandanten-ID erstellt.  Informationen zum Abrufen dieser Werte finden Sie unter [Abrufen eines Tokens von Azure AD zum Autorisieren von Anforderungen von einer Clientanwendung](../common/storage-auth-aad-app.md).

:::code language="python" source="~/azure-storage-snippets/blobs/howto/python/python-v12/crud_datalake.py" id="Snippet_AuthorizeWithAAD":::

> [!NOTE]
> Weitere Beispiele finden Sie in der Dokumentation zur [Azure-Identitätsclientbibliothek für Python](https://pypi.org/project/azure-identity/).

## <a name="create-a-container"></a>Erstellen eines Containers

Ein Container fungiert als Dateisystem für Ihre Dateien. Sie können eines erstellen, indem Sie die Methode **FileSystemDataLakeServiceClient.create_file_system** aufrufen.

In diesem Beispiel wird ein Container namens `my-file-system` erstellt.

:::code language="python" source="~/azure-storage-snippets/blobs/howto/python/python-v12/crud_datalake.py" id="Snippet_CreateContainer":::

## <a name="create-a-directory"></a>Erstellen eines Verzeichnisses

Erstellen Sie eine Verzeichnisreferenz, indem Sie die Methode **FileSystemClient.create_directory** aufrufen.

In diesem Beispiel wird einem Container das Verzeichnis `my-directory` hinzugefügt. 

:::code language="python" source="~/azure-storage-snippets/blobs/howto/python/python-v12/crud_datalake.py" id="Snippet_CreateDirectory":::

## <a name="rename-or-move-a-directory"></a>Umbenennen oder Verschieben eines Verzeichnisses

Rufen Sie die Methode **DataLakeDirectoryClient.rename_directory** auf, um ein Verzeichnis umzubenennen oder zu verschieben. Übergeben Sie den Pfad des gewünschten Verzeichnisses als Parameter. 

Im folgenden Beispiel wird ein Unterverzeichnis in `my-subdirectory-renamed` umbenannt.

:::code language="python" source="~/azure-storage-snippets/blobs/howto/python/python-v12/crud_datalake.py" id="Snippet_RenameDirectory":::

## <a name="delete-a-directory"></a>Löschen eines Verzeichnisses

Sie können ein Verzeichnis löschen, indem Sie die Methode **DataLakeDirectoryClient.delete_directory** aufrufen.

In diesem Beispiel wird das Verzeichnis `my-directory` gelöscht.  

:::code language="python" source="~/azure-storage-snippets/blobs/howto/python/python-v12/crud_datalake.py" id="Snippet_DeleteDirectory":::

## <a name="upload-a-file-to-a-directory"></a>Hochladen einer Datei in ein Verzeichnis 

Erstellen Sie zunächst eine Dateireferenz im Zielverzeichnis, in dem Sie eine Instanz der Klasse **DataLakeFileClient** erstellen. Laden Sie eine Datei hoch, indem Sie die Methode **DataLakeFileClient.append_data** aufrufen. Stellen Sie sicher, dass Sie den Uploadvorgang durch Aufrufen der Methode **DataLakeFileClient.flush_data** abschließen.

In diesem Beispiel wird eine Textdatei in das Verzeichnis `my-directory` hochgeladen.   

:::code language="python" source="~/azure-storage-snippets/blobs/howto/python/python-v12/crud_datalake.py" id="Snippet_UploadFile":::

> [!TIP]
> Wenn Ihre Datei groß ist, muss Ihr Code die **DataLakeFileClient.append_data**-Methode mehrmals aufrufen. Erwägen Sie stattdessen die **DataLakeFileClient.upload_data**-Methode. Auf diese Weise können Sie die gesamte Datei in einem einzigen Aufruf hochladen. 

## <a name="upload-a-large-file-to-a-directory"></a>Hochladen einer großen Datei in ein Verzeichnis

Verwenden Sie die **DataLakeFileClient.upload_data**-Methode, um große Dateien hochzuladen, ohne die **DataLakeFileClient.append_data**-Methode mehrfach aufrufen zu müssen.

:::code language="python" source="~/azure-storage-snippets/blobs/howto/python/python-v12/crud_datalake.py" id="Snippet_UploadFileBulk":::

## <a name="download-from-a-directory"></a>Herunterladen aus einem Verzeichnis 

Öffnen Sie eine lokale Datei zum Schreiben. Erstellen Sie dann eine **DataLakeFileClient**-Instanz, die die herunterzuladende Datei darstellt. Rufen Sie die Methode **DataLakeFileClient.read_file** auf, um Bytes aus der Datei zu lesen und anschließend in die lokale Datei zu schreiben. 

:::code language="python" source="~/azure-storage-snippets/blobs/howto/python/python-v12/crud_datalake.py" id="Snippet_DownloadFromDirectory":::

## <a name="list-directory-contents"></a>Auflisten des Verzeichnisinhalts

Listen Sie den Verzeichnisinhalt auf, indem Sie die Methode **FileSystemClient.get_paths** aufrufen und die Ergebnisse anschließend durchlaufen.

In diesem Beispiel wird der Pfad der einzelnen Unterverzeichnisse und Dateien gedruckt, die im Verzeichnis `my-directory` gespeichert sind.

:::code language="python" source="~/azure-storage-snippets/blobs/howto/python/python-v12/crud_datalake.py" id="Snippet_ListFilesInDirectory":::

## <a name="manage-access-control-lists-acls"></a>Verwalten von Zugriffssteuerungslisten (Access Control Lists, ACLs)

Sie können Zugriffsberechtigungen für Verzeichnisse und Dateien abrufen, festlegen und aktualisieren.

> [!NOTE]
> Wenn Sie Azure Active Directory (Azure AD) verwenden, um den Zugriff zu autorisieren, sollten Sie sicherstellen, dass Ihrem Sicherheitsprinzipal die Rolle [Besitzer von Speicherblobdaten](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner) zugewiesen wurde. Weitere Informationen dazu, wie ACL-Berechtigungen angewandt werden und wie sich Änderungen daran auswirken, finden Sie unter [Zugriffssteuerung in Azure Data Lake Storage Gen2](./data-lake-storage-access-control.md).

### <a name="manage-directory-acls"></a>Verwalten von Verzeichnis-ACLs

Rufen Sie die Zugriffssteuerungsliste (Access Control List, ACL) eines Verzeichnisses ab, indem Sie die Methode **DataLakeDirectoryClient.get_access_control** aufrufen und die ACL durch Aufrufen der Methode **DataLakeDirectoryClient.set_access_control** festlegen.

> [!NOTE]
> Wenn in Ihrer Anwendung der Zugriff mithilfe von Azure Active Directory (Azure AD) autorisiert wird, müssen Sie sicherstellen, dass dem Sicherheitsprinzipal, der in der Anwendung zum Autorisieren des Zugriffs verwendet wird, die [Rolle „Besitzer von Speicherblobdaten“](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner) zugewiesen ist. Weitere Informationen dazu, wie ACL-Berechtigungen angewandt werden und wie sich Änderungen daran auswirken, finden Sie unter [Zugriffssteuerung in Azure Data Lake Storage Gen2](./data-lake-storage-access-control.md).

In diesem Beispiel wird die ACL des Verzeichnisses `my-directory` abgerufen und dann festgelegt. Mit der Zeichenfolge `rwxr-xrw-` werden dem zuständigen Benutzer Lese-, Schreib- und Ausführungsberechtigungen und der zuständigen Gruppe nur Lese- und Ausführungsberechtigungen gewährt, während allen anderen lediglich Lese- und Schreibzugriff gewährt wird.

:::code language="python" source="~/azure-storage-snippets/blobs/howto/python/python-v12/ACL_datalake.py" id="Snippet_ACLDirectory":::

Sie können auch die ACL des Stammverzeichnisses eines Containers abrufen und festlegen. Um das Stammverzeichnis abzurufen, rufen Sie die **FileSystemClient._get_root_directory_client**-Methode auf.

### <a name="manage-file-permissions"></a>Verwalten von Dateiberechtigungen

Rufen Sie die Zugriffssteuerungsliste (Access Control List, ACL) einer Datei ab, indem Sie die Methode **DataLakeFileClient.get_access_control** aufrufen und die ACL durch Aufrufen der Methode **DataLakeFileClient.set_access_control** festlegen.

> [!NOTE]
> Wenn in Ihrer Anwendung der Zugriff mithilfe von Azure Active Directory (Azure AD) autorisiert wird, müssen Sie sicherstellen, dass dem Sicherheitsprinzipal, der in der Anwendung zum Autorisieren des Zugriffs verwendet wird, die [Rolle „Besitzer von Speicherblobdaten“](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner) zugewiesen ist. Weitere Informationen dazu, wie ACL-Berechtigungen angewandt werden und wie sich Änderungen daran auswirken, finden Sie unter [Zugriffssteuerung in Azure Data Lake Storage Gen2](./data-lake-storage-access-control.md).

Im folgenden Beispiel wird die ACL der Datei `my-file.txt` abgerufen und dann festgelegt. Mit der Zeichenfolge `rwxr-xrw-` werden dem zuständigen Benutzer Lese-, Schreib- und Ausführungsberechtigungen und der zuständigen Gruppe nur Lese- und Ausführungsberechtigungen gewährt, während allen anderen lediglich Lese- und Schreibzugriff gewährt wird.

:::code language="python" source="~/azure-storage-snippets/blobs/howto/python/python-v12/ACL_datalake.py" id="Snippet_FileACL":::

### <a name="set-an-acl-recursively"></a>Rekursives Festlegen einer Zugriffssteuerungsliste

Sie können Zugriffssteuerungslisten rekursiv für die vorhandenen untergeordneten Elemente eines übergeordneten Verzeichnisses hinzufügen, aktualisieren und entfernen, ohne diese Änderungen für jedes untergeordnete Element einzeln vornehmen zu müssen. Weitere Informationen finden Sie unter [Festlegen von Zugriffssteuerungslisten (ACLs) für Azure Data Lake Storage Gen2](recursive-access-control-lists.md).

## <a name="see-also"></a>Weitere Informationen

* [API-Referenzdokumentation](/python/api/azure-storage-file-datalake/azure.storage.filedatalake)
* [Paket (Python-Paketindex)](https://pypi.org/project/azure-storage-file-datalake/)
* [Beispiele](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/samples)
* [Zuordnung von Gen1 zu Gen2](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/GEN1_GEN2_MAPPING.md)
* [Bekannte Probleme](data-lake-storage-known-issues.md#api-scope-data-lake-client-library)
* [Feedback geben](https://github.com/Azure/azure-sdk-for-python/issues)