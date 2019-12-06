---
title: Verwenden von Java für Dateien und Zugriffssteuerungslisten in Azure Data Lake Storage Gen2 (Vorschau)
description: Verwenden Sie Azure Storage-Bibliotheken für Java, um Verzeichnisse, Dateien und Zugriffssteuerungslisten (ACLs) in Speicherkonten zu verwalten, für die der hierarchische Namespace aktiviert ist.
author: normesta
ms.service: storage
ms.date: 11/24/2019
ms.author: normesta
ms.topic: conceptual
ms.subservice: data-lake-storage-gen2
ms.reviewer: prishet
ms.openlocfilehash: b01370bb8c86da07549775ec1a1399e09c80f9af
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2019
ms.locfileid: "74534249"
---
# <a name="use-java-for-files--acls-in-azure-data-lake-storage-gen2-preview"></a>Verwenden von Java für Dateien und Zugriffssteuerungslisten in Azure Data Lake Storage Gen2 (Vorschau)

In diesem Artikel erfahren Sie, wie Sie Java zum Erstellen und Verwalten von Verzeichnissen, Dateien und Berechtigungen in Speicherkonten verwenden, für die der hierarchische Namespace aktiviert ist. 

> [!IMPORTANT]
> Die in diesem Artikel behandelte Java-Bibliothek befindet sich in der Public Preview.

[Maven-Paket](https://search.maven.org/artifact/com.azure/azure-storage-file-datalake/12.0.0-preview.6/jar) | [Beispiele](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-file-datalake) | [API-Referenz](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.0.0-preview.6/index.html) | [Zuordnung von Gen1 zu Gen2](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-file-datalake/GEN1_GEN2_MAPPING.md) | [Feedback geben](https://github.com/Azure/azure-sdk-for-java/issues)

## <a name="prerequisites"></a>Voraussetzungen

> [!div class="checklist"]
> * Ein Azure-Abonnement. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
> * Ein Speicherkonto, für das der hierarchische Namespace aktiviert ist. Befolgen Sie [diese Anleitung](data-lake-storage-quickstart-create-account.md) für die Erstellung.

## <a name="set-up-your-project"></a>Einrichten des Projekts

Öffnen Sie zunächst die Datei *pom.xml* in Ihrem Text-Editor. Fügen Sie der Gruppe „dependencies“ das folgende Abhängigkeitselement hinzu.

```xml
<dependency>
  <groupId>com.azure</groupId>
  <artifactId>azure-storage-file-datalake</artifactId>
  <version>12.0.0-preview.6</version>
</dependency>
```

Fügen Sie dann die folgenden Import-Anweisungen zu Ihrer Codedatei hinzu.

```java
import com.azure.storage.common.StorageSharedKeyCredential;
import com.azure.storage.file.datalake.DataLakeDirectoryClient;
import com.azure.storage.file.datalake.DataLakeFileClient;
import com.azure.storage.file.datalake.DataLakeFileSystemClient;
import com.azure.storage.file.datalake.DataLakeServiceClient;
import com.azure.storage.file.datalake.DataLakeServiceClientBuilder;
import com.azure.storage.file.datalake.models.ListPathsOptions;
import com.azure.storage.file.datalake.models.PathAccessControl;
import com.azure.storage.file.datalake.models.PathAccessControlEntry;
import com.azure.storage.file.datalake.models.PathItem;
import com.azure.storage.file.datalake.models.PathPermissions;
import com.azure.storage.file.datalake.models.RolePermissions;
```

## <a name="connect-to-the-account"></a>Herstellen einer Verbindung zum Konto 

Sie müssen eine **DataLakeServiceClient**-Instanz erstellen, die das Speicherkonto darstellt, um die Codeausschnitte in diesem Artikel zu verwenden. Die einfachste Möglichkeit, diese zu erhalten, ist die Verwendung eines Kontoschlüssels. 

In diesem Beispiel wird eine **DataLakeServiceClient**-Instanz mithilfe eines Kontoschlüssels erstellt.

```java

static public DataLakeServiceClient GetDataLakeServiceClient
(String accountName, String accountKey){

    StorageSharedKeyCredential sharedKeyCredential =
        new StorageSharedKeyCredential(accountName, accountKey);

    DataLakeServiceClientBuilder builder = new DataLakeServiceClientBuilder();

    builder.credential(sharedKeyCredential);
    builder.endpoint("https://" + accountName + ".dfs.core.windows.net");

    return builder.buildClient();
}      

```
## <a name="create-a-file-system"></a>Erstellen eines Dateisystems

Ein Dateisystem fungiert als Container für Ihre Dateien. Sie können eines erstellen, indem Sie die Methode **DataLakeServiceClient.createFileSystem** aufrufen.

In diesem Beispiel wird ein Dateisystem namens `my-file-system` erstellt. 

```java
static public DataLakeFileSystemClient CreateFileSystem
(DataLakeServiceClient serviceClient){

    return serviceClient.createFileSystem("my-file-system");
}
```

## <a name="create-a-directory"></a>Erstellen eines Verzeichnisses

Erstellen Sie eine Verzeichnisreferenz, indem Sie die Methode **DataLakeFileSystemClient.createDirectory** aufrufen.

Im folgenden Beispiel wird ein Verzeichnis namens `my-directory` zu einem Dateisystem hinzugefügt und anschließend wird ein Unterverzeichnis namens `my-subdirectory` hinzugefügt. 

```java
static public DataLakeDirectoryClient CreateDirectory
(DataLakeServiceClient serviceClient, String fileSystemName){
    
    DataLakeFileSystemClient fileSystemClient =
    serviceClient.getFileSystemClient(fileSystemName);

    DataLakeDirectoryClient directoryClient =
        fileSystemClient.createDirectory("my-directory");

    return directoryClient.createSubDirectory("my-subdirectory");
}
```

## <a name="rename-or-move-a-directory"></a>Umbenennen oder Verschieben eines Verzeichnisses

Rufen Sie die Methode **DataLakeDirectoryClient.rename** auf, um ein Verzeichnis umzubenennen oder zu verschieben. Übergeben Sie den Pfad des gewünschten Verzeichnisses als Parameter. 

Im folgenden Beispiel wird ein Unterverzeichnis in `my-subdirectory-renamed` umbenannt.

```java
static public DataLakeDirectoryClient
    RenameDirectory(DataLakeFileSystemClient fileSystemClient){

    DataLakeDirectoryClient directoryClient =
        fileSystemClient.getDirectoryClient("my-directory/my-subdirectory");

    return directoryClient.rename("my-directory/my-subdirectory-renamed");
}
```

Im folgenden Beispiel wird ein Verzeichnis namens `my-subdirectory-renamed` in ein Unterverzeichnis eines Verzeichnisses namens `my-directory-2` verschoben. 

```java
static public DataLakeDirectoryClient MoveDirectory
(DataLakeFileSystemClient fileSystemClient){

    DataLakeDirectoryClient directoryClient =
        fileSystemClient.getDirectoryClient("my-directory/my-subdirectory-renamed");

    return directoryClient.rename("my-directory-2/my-subdirectory-renamed");                
}
```

## <a name="delete-a-directory"></a>Löschen eines Verzeichnisses

Sie können ein Verzeichnis löschen, indem Sie die Methode **DataLakeDirectoryClient.deleteWithResponse** aufrufen.

Im folgenden Beispiel wird ein Verzeichnis namens `my-directory` gelöscht.   

```java
static public void DeleteDirectory(DataLakeFileSystemClient fileSystemClient){
        
    DataLakeDirectoryClient directoryClient =
        fileSystemClient.getDirectoryClient("my-directory");

    directoryClient.deleteWithResponse(true, null, null, null);
}
```

## <a name="manage-a-directory-acl"></a>Verwalten einer Zugriffssteuerungsliste eines Verzeichnisses

Im folgenden Beispiel wird die ACL (Zugriffssteuerungsliste) eines Verzeichnisses namens `my-directory` abgerufen und dann festgelegt. Im folgenden Beispiel werden dem zuständigen Benutzer Lese-, Schreib- und Ausführungsberechtigungen und der zuständigen Gruppe nur Lese- und Ausführungsberechtigungen gewährt, während allen anderen lediglich Lesezugriff gewährt wird.

```java
static public void ManageDirectoryACLs(DataLakeFileSystemClient fileSystemClient){

    DataLakeDirectoryClient directoryClient =
        fileSystemClient.getDirectoryClient("my-directory");

    PathAccessControl directoryAccessControl =
        directoryClient.getAccessControl();

    List<PathAccessControlEntry> pathPermissions = directoryAccessControl.getAccessControlList();
       
    System.out.println(PathAccessControlEntry.serializeList(pathPermissions));
             
    PathPermissions permissions = new PathPermissions()

      .group(new RolePermissions().execute(true).read(true))
      .owner(new RolePermissions().execute(true).read(true).write(true))
      .other(new RolePermissions().read(true));

    directoryClient.setPermissions(permissions, null, null);

    pathPermissions = directoryClient.getAccessControl().getAccessControlList();
     
    System.out.println(PathAccessControlEntry.serializeList(pathPermissions));

}

```

## <a name="upload-a-file-to-a-directory"></a>Hochladen einer Datei in ein Verzeichnis

Erstellen Sie zunächst eine Dateireferenz im Zielverzeichnis, in dem Sie eine Instanz der **DataLakeFileClient**-Klasse erstellen. Laden Sie eine Datei hoch, indem Sie die Methode **DataLakeFileClient.append** aufrufen. Stellen Sie sicher, dass Sie den Uploadvorgang abschließen, indem Sie die Methode **DataLakeFileClient.FlushAsync** aufrufen.

Im folgenden Beispiel wird eine Textdatei in ein Verzeichnis namens `my-directory` hochgeladen.

```java
static public void UploadFile(DataLakeFileSystemClient fileSystemClient) 
    throws FileNotFoundException{
        
    DataLakeDirectoryClient directoryClient =
        fileSystemClient.getDirectoryClient("my-directory");

    DataLakeFileClient fileClient = directoryClient.createFile("uploaded-file.txt");

    File file = new File("C:\\mytestfile.txt");

    InputStream targetStream = new FileInputStream(file);

    long fileSize = file.length();

    fileClient.append(targetStream, 0, fileSize);

    fileClient.flush(fileSize);
}
```

## <a name="manage-a-file-acl"></a>Verwalten einer Zugriffssteuerungsliste einer Datei

Im folgenden Beispiel wird die ACL einer Datei namens `upload-file.txt` abgerufen und dann festgelegt. Im folgenden Beispiel werden dem zuständigen Benutzer Lese-, Schreib- und Ausführungsberechtigungen und der zuständigen Gruppe nur Lese- und Ausführungsberechtigungen gewährt, während allen anderen lediglich Lesezugriff gewährt wird.

```java
static public void ManageFileACLs(DataLakeFileSystemClient fileSystemClient){

    DataLakeDirectoryClient directoryClient =
        fileSystemClient.getDirectoryClient("my-directory");

    DataLakeFileClient fileClient = 
        directoryClient.getFileClient("uploaded-file.txt");

    PathAccessControl fileAccessControl =
        fileClient.getAccessControl();

    List<PathAccessControlEntry> pathPermissions = fileAccessControl.getAccessControlList();
     
    System.out.println(PathAccessControlEntry.serializeList(pathPermissions));
           
    PathPermissions permissions = new PathPermissions()

        .group(new RolePermissions().execute(true).read(true))
        .owner(new RolePermissions().execute(true).read(true).write(true))
        .other(new RolePermissions().read(false));

    fileClient.setPermissions(permissions, null, null);

    pathPermissions = fileClient.getAccessControl().getAccessControlList();
   
    System.out.println(PathAccessControlEntry.serializeList(pathPermissions));

}
```

## <a name="download-from-a-directory"></a>Herunterladen aus einem Verzeichnis

Erstellen Sie zunächst eine **DataLakeFileClient**-Instanz, die die Datei darstellt, die Sie herunterladen möchten. Verwenden Sie die Methode **DataLakeFileClient.read**, um die Datei zu lesen. Verwenden Sie eine beliebige .NET-API für die Dateiverarbeitung, um Bytes aus dem Datenstrom in einer Datei zu speichern. 

Stellen Sie sicher, dass Sie den Downloadvorgang abschließen, indem Sie die Methode **DataLakeFileClient.flush** aufrufen.

```java
static public void DownloadFile(DataLakeFileSystemClient fileSystemClient)
    throws FileNotFoundException, java.io.IOException{

    DataLakeDirectoryClient directoryClient =
        fileSystemClient.getDirectoryClient("my-directory");

    DataLakeFileClient fileClient = 
        directoryClient.getFileClient("uploaded-file.txt");

    File file = new File("C:\\downloadedFile.txt");

    OutputStream targetStream = new FileOutputStream(file);

    fileClient.read(targetStream);

    targetStream.close();

    fileClient.flush(file.length());
        
}

```

## <a name="list-directory-contents"></a>Auflisten des Verzeichnisinhalts

Im folgenden Beispiel werden die Namen aller Dateien ausgegeben, die sich in einem Verzeichnis namens `my-directory` befinden.

```java
static public void ListFilesInDirectory(DataLakeFileSystemClient fileSystemClient){
        
    ListPathsOptions options = new ListPathsOptions();
    options.setPath("my-directory");
     
    PagedIterable<PathItem> pagedIterable = 
    fileSystemClient.listPaths(options, null);

    java.util.Iterator<PathItem> iterator = pagedIterable.iterator();
       
    PathItem item = iterator.next();

    while (item != null)
    {
        System.out.println(item.getName());


        if (!iterator.hasNext())
        {
            break;
        }
            
        item = iterator.next();
    }

}
```

## <a name="see-also"></a>Weitere Informationen

* [API-Referenzdokumentation](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.0.0-preview.6/index.html)
* [Maven-Paket](https://search.maven.org/artifact/com.azure/azure-storage-file-datalake/12.0.0-preview.6/jar)
* [Beispiele](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-file-datalake)
* [Zuordnung von Gen1 zu Gen2](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-file-datalake/GEN1_GEN2_MAPPING.md)
* [Bekannte Probleme](data-lake-storage-known-issues.md#api-scope-data-lake-client-library)
* [Feedback geben](https://github.com/Azure/azure-sdk-for-java/issues)