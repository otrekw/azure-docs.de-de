---
title: Azure Data Lake Storage Gen2 Python SDK für Dateien und Zugriffssteuerungslisten (Vorschau)
description: Verwenden Sie Python zum Verwalten von Verzeichnissen, Dateien und Zugriffssteuerungslisten (ACLs) in Speicherkonten, bei denen der hierarchische Namespace (HNS) aktiviert wurde.
author: normesta
ms.service: storage
ms.date: 11/24/2019
ms.author: normesta
ms.topic: article
ms.subservice: data-lake-storage-gen2
ms.reviewer: prishet
ms.openlocfilehash: cb2e1c16c1419d9925bd837bb4e12119f08d56c4
ms.sourcegitcommit: 5bbe87cf121bf99184cc9840c7a07385f0d128ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2020
ms.locfileid: "76119532"
---
# <a name="use-python-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2-preview"></a>Verwenden von Python zum Verwalten von Verzeichnissen, Dateien und Zugriffssteuerungslisten in Azure Data Lake Storage Gen2 (Vorschau)

In diesem Artikel erfahren Sie, wie Sie mithilfe von Python Verzeichnisse, Dateien und Berechtigungen in Speicherkonten erstellen und verwalten, bei denen der hierarchische Namespace (HNS) aktiviert wurde. 

> [!IMPORTANT]
> Die Azure Data Lake Storage-Clientbibliothek für Python ist zurzeit als öffentliche Vorschauversion verfügbar.

[Paket (Python-Paketindex)](https://pypi.org/project/azure-storage-file-datalake/) | [Beispiele](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/samples) | [API-Referenz](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-storage-file-datalake/12.0.0b5/index.html) | [Zuordnung von Gen1 zu Gen2](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/GEN1_GEN2_MAPPING.md) | [Feedback geben](https://github.com/Azure/azure-sdk-for-python/issues)

## <a name="prerequisites"></a>Voraussetzungen

> [!div class="checklist"]
> * Ein Azure-Abonnement. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
> * Ein Speicherkonto, für das der hierarchische Namespace aktiviert ist. Befolgen Sie [diese Anleitung](data-lake-storage-quickstart-create-account.md) für die Erstellung.

## <a name="set-up-your-project"></a>Einrichten des Projekts

Installieren Sie die Azure Data Lake Storage-Clientbibliothek für Python mithilfe von [pip](https://pypi.org/project/pip/).

```
pip install azure-storage-file-datalake --pre
```

Fügen Sie diese Importanweisungen am Anfang Ihrer Codedatei hinzu.

```python
import os, uuid, sys
from azure.storage.filedatalake import DataLakeServiceClient
```

## <a name="connect-to-the-account"></a>Herstellen einer Verbindung mit dem Konto

Wenn Sie die Codeausschnitte in diesem Artikel verwenden möchten, müssen Sie eine **DataLakeServiceClient**-Instanz erstellen, die das Speicherkonto darstellt. Am einfachsten können Sie dafür einen Kontoschlüssel verwenden. 

In diesem Beispiel wird mithilfe eines Kontoschlüssels eine **DataLakeServiceClient**-Instanz erstellt, die das Speicherkonto darstellt. 

```python
try:  
    global service_client
        
    service_client = DataLakeServiceClient(account_url="{}://{}.dfs.core.windows.net".format(
        "https", storage_account_name), credential=storage_account_key)
    
except Exception as e:
    print(e)
```
 
- Ersetzen Sie den Platzhalterwert `storage_account_name` durch den Namen Ihres Speicherkontos.

- Ersetzen Sie den Platzhalterwert `storage_account_key` durch den Zugriffsschlüssel Ihres Speicherkontos.

## <a name="create-a-file-system"></a>Erstellen eines Dateisystems

Ein Dateisystem fungiert als Container für Ihre Dateien. Sie können eines erstellen, indem Sie die Methode **FileSystemDataLakeServiceClient.create_file_system** aufrufen.

In diesem Beispiel wird das Dateisystem `my-file-system` erstellt.

```python
def create_file_system():
    try:
        global file_system_client

        file_system_client = service_client.create_file_system(file_system="my-file-system")
    
    except Exception as e:
        print(e) 
```


## <a name="create-a-directory"></a>Erstellen eines Verzeichnisses

Erstellen Sie eine Verzeichnisreferenz, indem Sie die Methode **FileSystemClient.create_directory** aufrufen.

In diesem Beispiel wird einem Dateisystem das Verzeichnis `my-directory` hinzugefügt. 

```python
def create_directory():
    try:
        file_system_client.create_directory("my-directory")
    
    except Exception as e:
     print(e) 
```

## <a name="rename-or-move-a-directory"></a>Umbenennen oder Verschieben eines Verzeichnisses

Rufen Sie die Methode **DataLakeDirectoryClient.rename_directory** auf, um ein Verzeichnis umzubenennen oder zu verschieben. Übergeben Sie den Pfad des gewünschten Verzeichnisses als Parameter. 

Im folgenden Beispiel wird ein Unterverzeichnis in `my-subdirectory-renamed` umbenannt.

```python
def rename_directory():
    try:
       
       file_system_client = service_client.get_file_system_client(file_system="my-file-system")
       directory_client = file_system_client.get_directory_client("my-directory")
       
       new_dir_name = "my-directory-renamed"
       directory_client.rename_directory(rename_destination=directory_client.file_system_name + '/' + new_dir_name)

    except Exception as e:
     print(e) 
```

## <a name="delete-a-directory"></a>Löschen eines Verzeichnisses

Sie können ein Verzeichnis löschen, indem Sie die Methode **DataLakeDirectoryClient.delete_directory** aufrufen.

In diesem Beispiel wird das Verzeichnis `my-directory` gelöscht.  

```python
def delete_directory():
    try:
        file_system_client = service_client.get_file_system_client(file_system="my-file-system")
        directory_client = file_system_client.get_directory_client("my-directory")

        directory_client.delete_directory()
    except Exception as e:
     print(e) 
```

## <a name="manage-directory-permissions"></a>Verwalten von Verzeichnisberechtigungen

Rufen Sie die Zugriffssteuerungsliste (Access Control List, ACL) eines Verzeichnisses ab, indem Sie die Methode **DataLakeDirectoryClient.get_access_control** aufrufen und die ACL durch Aufrufen der Methode **DataLakeDirectoryClient.set_access_control** festlegen.

> [!NOTE]
> Wenn in Ihrer Anwendung der Zugriff mithilfe von Azure Active Directory (Azure AD) autorisiert wird, müssen Sie sicherstellen, dass dem Sicherheitsprinzipal, der in der Anwendung zum Autorisieren des Zugriffs verwendet wird, die [Rolle „Besitzer von Speicherblobdaten“](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner) zugewiesen ist. Weitere Informationen dazu, wie ACL-Berechtigungen angewandt werden und wie sich Änderungen daran auswirken, finden Sie unter [Zugriffssteuerung in Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control).

In diesem Beispiel wird die ACL des Verzeichnisses `my-directory` abgerufen und dann festgelegt. Mit der Zeichenfolge `rwxr-xrw-` werden dem zuständigen Benutzer Lese-, Schreib- und Ausführungsberechtigungen und der zuständigen Gruppe nur Lese- und Ausführungsberechtigungen gewährt, während allen anderen lediglich Lese- und Schreibzugriff gewährt wird.

```python
def manage_directory_permissions():
    try:
        file_system_client = service_client.get_file_system_client(file_system="my-file-system")

        directory_client = file_system_client.get_directory_client("my-directory")
        
        acl_props = directory_client.get_access_control()
        
        print(acl_props['permissions'])
        
        new_dir_permissions = "rwxr-xrw-"
        
        directory_client.set_access_control(permissions=new_dir_permissions)
        
        acl_props = directory_client.get_access_control()
        
        print(acl_props['permissions'])
    
    except Exception as e:
     print(e) 
```

## <a name="upload-a-file-to-a-directory"></a>Hochladen einer Datei in ein Verzeichnis 

Erstellen Sie zunächst eine Dateireferenz im Zielverzeichnis, in dem Sie eine Instanz der Klasse **DataLakeFileClient** erstellen. Laden Sie eine Datei hoch, indem Sie die Methode **DataLakeFileClient.append_data** aufrufen. Stellen Sie sicher, dass Sie den Uploadvorgang durch Aufrufen der Methode **DataLakeFileClient.flush_data** abschließen.

In diesem Beispiel wird eine Textdatei in das Verzeichnis `my-directory` hochgeladen.   

```python
def upload_file_to_directory():
    try:

        file_system_client = service_client.get_file_system_client(file_system="my-file-system")

        directory_client = file_system_client.get_directory_client("my-directory")
        
        file_client = directory_client.create_file("uploaded-file.txt")
        local_file = open("C:\\file-to-upload.txt",'r')

        file_contents = local_file.read()

        file_client.append_data(data=file_contents, offset=0, length=len(file_contents))

        file_client.flush_data(len(file_contents))

    except Exception as e:
      print(e) 
```

## <a name="manage-file-permissions"></a>Verwalten von Dateiberechtigungen

Rufen Sie die Zugriffssteuerungsliste (Access Control List, ACL) einer Datei ab, indem Sie die Methode **DataLakeFileClient.get_access_control** aufrufen und die ACL durch Aufrufen der Methode **DataLakeFileClient.set_access_control** festlegen.

> [!NOTE]
> Wenn in Ihrer Anwendung der Zugriff mithilfe von Azure Active Directory (Azure AD) autorisiert wird, müssen Sie sicherstellen, dass dem Sicherheitsprinzipal, der in der Anwendung zum Autorisieren des Zugriffs verwendet wird, die [Rolle „Besitzer von Speicherblobdaten“](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner) zugewiesen ist. Weitere Informationen dazu, wie ACL-Berechtigungen angewandt werden und wie sich Änderungen daran auswirken, finden Sie unter [Zugriffssteuerung in Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control).

Im folgenden Beispiel wird die ACL der Datei `my-file.txt` abgerufen und dann festgelegt. Mit der Zeichenfolge `rwxr-xrw-` werden dem zuständigen Benutzer Lese-, Schreib- und Ausführungsberechtigungen und der zuständigen Gruppe nur Lese- und Ausführungsberechtigungen gewährt, während allen anderen lediglich Lese- und Schreibzugriff gewährt wird.

```python
def manage_file_permissions():
    try:
        file_system_client = service_client.get_file_system_client(file_system="my-file-system")

        directory_client = file_system_client.get_directory_client("my-directory")
        
        file_client = directory_client.get_file_client("uploaded-file.txt")

        acl_props = file_client.get_access_control()
        
        print(acl_props['permissions'])
        
        new_file_permissions = "rwxr-xrw-"
        
        file_client.set_access_control(permissions=new_file_permissions)
        
        acl_props = file_client.get_access_control()
        
        print(acl_props['permissions'])

    except Exception as e:
     print(e) 
```

## <a name="download-from-a-directory"></a>Herunterladen aus einem Verzeichnis 

Öffnen Sie eine lokale Datei zum Schreiben. Erstellen Sie dann eine **DataLakeFileClient**-Instanz, die die herunterzuladende Datei darstellt. Rufen Sie die Methode **DataLakeFileClient.read_file** auf, um Bytes aus der Datei zu lesen und anschließend in die lokale Datei zu schreiben. 

```python
def download_file_from_directory():
    try:
        file_system_client = service_client.get_file_system_client(file_system="my-file-system")

        directory_client = file_system_client.get_directory_client("my-directory")
        
        local_file = open("C:\\file-to-download.txt",'wb')

        file_client = directory_client.get_file_client("uploaded-file.txt")

        downloaded_bytes = file_client.read_file()

        local_file.write(downloaded_bytes)

        local_file.close()

    except Exception as e:
     print(e)
```
## <a name="list-directory-contents"></a>Auflisten des Verzeichnisinhalts

Listen Sie den Verzeichnisinhalt auf, indem Sie die Methode **FileSystemClient.get_paths** aufrufen und die Ergebnisse anschließend durchlaufen.

In diesem Beispiel wird der Pfad der einzelnen Unterverzeichnisse und Dateien gedruckt, die im Verzeichnis `my-directory` gespeichert sind.

```python
def list_directory_contents():
    try:
        
        file_system_client = service_client.get_file_system_client(file_system="my-file-system")

        paths = file_system_client.get_paths(path="my-directory")

        for path in paths:
            print(path.name + '\n')

    except Exception as e:
     print(e) 
```

## <a name="see-also"></a>Weitere Informationen

* [API-Referenzdokumentation](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-storage-file-datalake/12.0.0b5/index.html)
* [Paket (Python-Paketindex)](https://pypi.org/project/azure-storage-file-datalake/)
* [Beispiele](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/samples)
* [Zuordnung von Gen1 zu Gen2](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-file-datalake/GEN1_GEN2_MAPPING.md)
* [Bekannte Probleme](data-lake-storage-known-issues.md#api-scope-data-lake-client-library)
* [Feedback geben](https://github.com/Azure/azure-sdk-for-python/issues)
