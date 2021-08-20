---
title: 'Schnellstart: Azure Storage Files Share-Bibliothek v12 - C++'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe der Azure Storage Files Share-Clientbibliothek, Version 12 für C++, eine Dateifreigabe und eine Datei erstellen. Als Nächstes erfahren Sie, wie Sie Metadaten festlegen und abrufen und dann die Datei auf Ihren lokalen Computer herunterladen.
author: kyle-patterson
ms.author: kylepa
ms.date: 06/22/2021
ms.service: storage
ms.subservice: files
ms.topic: quickstart
ms.openlocfilehash: 4da02f46ef793ae03a11fa4895471488f78d0db1
ms.sourcegitcommit: 5be51a11c63f21e8d9a4d70663303104253ef19a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2021
ms.locfileid: "112894287"
---
# <a name="develop-for-azure-files-with-c"></a>Entwickeln für Azure Files mit C++

[!INCLUDE [storage-selector-file-include](../../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../../includes/storage-try-azure-tools-files.md)]

## <a name="applies-to"></a>Gilt für:
| Dateifreigabetyp | SMB | NFS |
|-|:-:|:-:|
| Standard-Dateifreigaben (GPv2), LRS/ZRS | ![Ja](../media/icons/yes-icon.png) | ![Nein](../media/icons/no-icon.png) |
| Standard-Dateifreigaben (GPv2), GRS/GZRS | ![Ja](../media/icons/yes-icon.png) | ![Nein](../media/icons/no-icon.png) |
| Premium-Dateifreigaben (FileStorage), LRS/ZRS | ![Ja](../media/icons/yes-icon.png) | ![Nein](../media/icons/no-icon.png) |

## <a name="about-this-tutorial"></a>Informationen zu diesem Tutorial

In diesem Tutorial erfahren Sie, wie Sie grundlegende Vorgänge in Azure Files mit C++ durchführen. Wenn Sie noch nicht mit Azure Files vertraut sind, helfen die in den folgenden Abschnitten erläuterten Konzepte, die Beispiele besser zu verstehen. Einige der behandelten Beispiele sind:

* Erstellen und Löschen von Azure-Dateifreigaben
* Erstellen und Löschen von Verzeichnissen
* Hochladen, Herunterladen und Löschen einer Datei
* Festlegen und Auflisten der Metadaten für eine Datei

> [!Note]  
> Da der Zugriff auf Azure Files über SMB möglich ist, können Sie einfache Anwendungen schreiben, die über die standardmäßigen E/A-Klassen und -Funktionen von C++ auf die Azure-Dateifreigabe zugreifen. In diesem Artikel erfahren Sie, wie Sie Anwendungen schreiben, die das Azure Storage C++ SDK verwenden, das über die [Datei-REST-API](/rest/api/storageservices/file-service-rest-api) mit Azure Files kommuniziert.


## <a name="prerequisites"></a>Voraussetzungen

- [Azure-Abonnement](https://azure.microsoft.com/free/)
- [Azure-Speicherkonto](../common/storage-account-create.md)
- [C++-Compiler](https://azure.github.io/azure-sdk/cpp_implementation.html#supported-platforms)
- [CMake](https://cmake.org/)
- [Vcpkg: C- und C++-Paket-Manager](https://github.com/microsoft/vcpkg/blob/master/docs/README.md)

## <a name="setting-up"></a>Einrichten

In diesem Abschnitt wird beschrieben, wie ein Projekt zur Verwendung mit der Azure Blob Storage-Clientbibliothek v12 für C++ vorbereitet wird.

### <a name="install-the-packages"></a>Installieren der Pakete

Mit dem Befehl `vcpkg install` werden das Azure Storage Blobs SDK für C++ und die erforderlichen Abhängigkeiten installiert:

```console
vcpkg.exe install azure-storage-files-shares-cpp:x64-windows
```

Besuchen Sie GitHub, um weitere Informationen zum Abrufen und Erstellen des [Azure SDK für C++](https://github.com/Azure/azure-sdk-for-cpp/) zu erhalten.

### <a name="create-the-project"></a>Erstellen des Projekts

Erstellen Sie in Visual Studio eine neue C++-Konsolenanwendung für Windows mit dem Namen *FilesShareQuickstartV12*.

:::image type="content" source="./media/quickstart-files-c-plus-plus/visual-studio-create-project.png" alt-text="Visual Studio-Dialogfeld zum Konfigurieren einer neuen C++-Windows-Konsolen-App":::

[!INCLUDE [storage-quickstart-credentials-include](../../../includes/storage-quickstart-credentials-include.md)]

## <a name="code-examples"></a>Codebeispiele

Mit diesen Beispielcodeausschnitten wird veranschaulicht, wie folgende Vorgänge mit der Azure Files Share-Clientbibliothek für C++ durchgeführt werden:

- [Hinzufügen von Includedateien](#add-include-files)
- [Abrufen der Verbindungszeichenfolge](#get-the-connection-string)
- [Erstellen einer Dateifreigabe](#create-a-files-share)
- [Hochladen von Dateien auf eine Dateifreigabe](#upload-files-to-a-files-share)
- [Festlegen der Metadaten einer Datei](#set-the-metadata-of-a-file)
- [Auflisten der Metadaten einer Datei](#list-the-metadata-of-a-file)
- [Herunterladen von Dateien](#download-files)
- [Löschen von Dateien](#delete-a-file)
- [Löschen einer Dateifreigabe](#delete-a-files-share)

### <a name="add-include-files"></a>Hinzufügen von Includedateien

Über das Projektverzeichnis:

1. Öffnen Sie in Visual Studio die Projektmappendatei *FilesShareQuickstartV12.sln*.
1. Öffnen Sie in Visual Studio die Quelldatei *FilesShareQuickstartV12.cpp*.
1. Entfernen Sie den Code in `main`, der automatisch generiert wurde.
1. Fügen Sie `#include`-Anweisungen hinzu.

:::code language="cpp" source="~/azure-storage-snippets/files/quickstarts/C++/V12/FilesShareQuickStartV12/FilesShareQuickStartV12/FilesShareQuickStartV12.cpp" ID="Snippet_Includes":::

### <a name="get-the-connection-string"></a>Abrufen der Verbindungszeichenfolge

Der folgende Code ruft die Verbindungszeichenfolge für das Speicherkonto aus der Umgebungsvariablen ab, die unter [Konfigurieren der Speicherverbindungszeichenfolge](#configure-your-storage-connection-string) erstellt wurde.

Fügen Sie diesen Code in `main()` hinzu:

:::code language="cpp" source="~/azure-storage-snippets/files/quickstarts/C++/V12/FilesShareQuickStartV12/FilesShareQuickStartV12/FilesShareQuickStartV12.cpp" ID="Snippet_ConnectionString":::

### <a name="create-a-files-share"></a>Erstellen einer Dateifreigabe

Erstellen Sie eine Instanz der Klasse [ShareClient](https://azuresdkdocs.blob.core.windows.net/$web/cpp/azure-storage-files-shares/12.0.0/class_azure_1_1_storage_1_1_files_1_1_shares_1_1_share_client.html), indem Sie die Funktion [CreateFromConnectionString](https://azuresdkdocs.blob.core.windows.net/$web/cpp/azure-storage-files-shares/12.0.0/class_azure_1_1_storage_1_1_files_1_1_shares_1_1_share_client.html#a7462bcad8a9144f84b0acc70735240e2) aufrufen. Rufen Sie anschließend [CreateIfNotExists](https://azuresdkdocs.blob.core.windows.net/$web/cpp/azure-storage-files-shares/12.0.0/class_azure_1_1_storage_1_1_files_1_1_shares_1_1_share_client.html#a6432b4cc677ac03257c7bf234129ec5b) auf, um die tatsächliche Dateifreigabe in Ihrem Speicherkonto zu erstellen.

Fügen Sie am Ende von `main()` den folgenden Code hinzu:

:::code language="cpp" source="~/azure-storage-snippets/files/quickstarts/C++/V12/FilesShareQuickStartV12/FilesShareQuickStartV12/FilesShareQuickStartV12.cpp" ID="Snippet_CreateFilesShare":::

### <a name="upload-files-to-a-files-share"></a>Hochladen von Dateien in eine Dateifreigabe

Der folgende Codeausschnitt führt folgende Aktionen durch:

1. Deklarieren einer Zeichenfolge, die „Hello Azure!“ enthält
1. Abrufen eines Verweises auf ein [ShareFileClient](https://azuresdkdocs.blob.core.windows.net/$web/cpp/azure-storage-files-shares/12.0.0/class_azure_1_1_storage_1_1_files_1_1_shares_1_1_share_file_client.html)-Objekt, indem der Stamm [ShareDirectoryClient](https://azuresdkdocs.blob.core.windows.net/$web/cpp/azure-storage-files-shares/12.0.0/class_azure_1_1_storage_1_1_files_1_1_shares_1_1_share_directory_client.html) abgerufen wird und dann [GetFileClient](https://azuresdkdocs.blob.core.windows.net/$web/cpp/azure-storage-files-shares/12.0.0/class_azure_1_1_storage_1_1_files_1_1_shares_1_1_share_directory_client.html#a93545b8d82ee94f9de183c4d277059d8) für die Dateifreigabe aus dem Abschnitt [Erstellen einer Dateifreigabe](#create-a-files-share) aufgerufen wird
1. Hochladen der Zeichenfolge in die Datei durch Aufrufen der Funktion [UploadFrom](https://azuresdkdocs.blob.core.windows.net/$web/cpp/azure-storage-files-shares/12.0.0/class_azure_1_1_storage_1_1_files_1_1_shares_1_1_share_file_client.html#aa80c1f0da6e6784aa0a67cff03d128ba) Mit dieser Funktion wird die Datei erstellt, falls sie nicht bereits vorhanden ist, oder aktualisiert, falls sie bereits vorhanden ist.

Fügen Sie am Ende von `main()` den folgenden Code hinzu:

:::code language="cpp" source="~/azure-storage-snippets/files/quickstarts/C++/V12/FilesShareQuickStartV12/FilesShareQuickStartV12/FilesShareQuickStartV12.cpp" ID="Snippet_UploadFile":::

### <a name="set-the-metadata-of-a-file"></a>Festlegen der Metadaten einer Datei

Legen Sie die Metadateneigenschaften für eine Datei fest, indem Sie die Funktion [ShareFileClient.SetMetadata](https://azuresdkdocs.blob.core.windows.net/$web/cpp/azure-storage-files-shares/12.0.0/class_azure_1_1_storage_1_1_files_1_1_shares_1_1_share_file_client.html#a2f5a40b71040003e41ba8495101f67bb) aufrufen.

Fügen Sie am Ende von `main()` den folgenden Code hinzu:

:::code language="cpp" source="~/azure-storage-snippets/files/quickstarts/C++/V12/FilesShareQuickStartV12/FilesShareQuickStartV12/FilesShareQuickStartV12.cpp" ID="Snippet_SetFileMetadata":::

### <a name="list-the-metadata-of-a-file"></a>Auflisten der Metadaten einer Datei

Rufen Sie die Metadateneigenschaften für eine Datei ab, indem Sie die Funktion [ShareFileClient.GetProperties](https://azuresdkdocs.blob.core.windows.net/$web/cpp/azure-storage-files-shares/12.0.0/class_azure_1_1_storage_1_1_files_1_1_shares_1_1_share_file_client.html#a220017815847877cfe8f244b482fb45c) aufrufen. Die Metadaten befinden sich unter dem `Metadata`-Feld des zurückgegebenen `Value`. Die Metadaten sind ein Schlüssel-Wert-Paar, ähnlich wie im Beispiel unter [Festlegen der Metadaten einer Datei](#set-the-metadata-of-a-file).

:::code language="cpp" source="~/azure-storage-snippets/files/quickstarts/C++/V12/FilesShareQuickStartV12/FilesShareQuickStartV12/FilesShareQuickStartV12.cpp" ID="Snippet_GetFileMetadata":::

### <a name="download-files"></a>Herunterladen von Dateien

Nachdem Sie die Eigenschaften der Datei unter [Auflisten der Metadaten einer Datei](#list-the-metadata-of-a-file) abgerufen haben, ein neues `std::vector<uint8_t>`-Objekt, indem Sie die Eigenschaften der hochgeladenen Datei verwenden. Laden Sie die zuvor erstellte Datei in das neue `std::vector<uint8_t>`-Objekt herunter, indem Sie die Funktion [DownloadTo](https://azuresdkdocs.blob.core.windows.net/$web/cpp/azure-storage-files-shares/12.0.0/class_azure_1_1_storage_1_1_files_1_1_shares_1_1_share_file_client.html#a256e7b317fbf762c4f8f5023a2cd8cb9) in der [ShareFileClient](https://azuresdkdocs.blob.core.windows.net/$web/cpp/azure-storage-files-shares/12.0.0/class_azure_1_1_storage_1_1_files_1_1_shares_1_1_share_file_client.html)-Basisklasse aufrufen. Zeigen Sie abschließend die heruntergeladenen Dateidaten an.

Fügen Sie am Ende von `main()` den folgenden Code hinzu:

:::code language="cpp" source="~/azure-storage-snippets/files/quickstarts/C++/V12/FilesShareQuickStartV12/FilesShareQuickStartV12/FilesShareQuickStartV12.cpp" ID="Snippet_DownloadFile":::

### <a name="delete-a-file"></a>Löschen von Dateien

Mit dem folgenden Code wird das Blob durch Aufrufen der Funktion [ShareFileClient.Delete](https://azuresdkdocs.blob.core.windows.net/$web/cpp/azure-storage-files-shares/12.0.0/class_azure_1_1_storage_1_1_files_1_1_shares_1_1_share_file_client.html#a86036b445dce67a96f7bf6c45f163f59) aus der Azure Storage-Dateifreigabe gelöscht.

:::code language="cpp" source="~/azure-storage-snippets/files/quickstarts/C++/V12/FilesShareQuickStartV12/FilesShareQuickStartV12/FilesShareQuickStartV12.cpp" ID="Snippet_DeleteFile":::

### <a name="delete-a-files-share"></a>Löschen einer Dateifreigabe

Mit dem folgenden Code werden die von der App erstellten Ressourcen bereinigt, indem die gesamte Dateifreigabe mithilfe von [ShareClient.Delete](https://azuresdkdocs.blob.core.windows.net/$web/cpp/azure-storage-files-shares/12.0.0/class_azure_1_1_storage_1_1_files_1_1_shares_1_1_share_client.html#a9915630503f1675b5f49eb9c01ca2601) gelöscht wird.

Fügen Sie am Ende von `main()` den folgenden Code hinzu:

:::code language="cpp" source="~/azure-storage-snippets/files/quickstarts/C++/V12/FilesShareQuickStartV12/FilesShareQuickStartV12/FilesShareQuickStartV12.cpp" ID="Snippet_DeleteFilesShare":::

## <a name="run-the-code"></a>Ausführen des Codes

Diese App erstellt einen Container und lädt eine Textdatei in Azure Blob Storage hoch. Im Beispiel werden dann die Blobs im Container aufgelistet, und die Datei wird heruntergeladen und der Dateiinhalt angezeigt. Schließlich löscht die App das Blob und den Container.

Die Ausgabe der App sieht etwa wie das folgende Beispiel aus:

```output
Azure Files Shares storage v12 - C++ quickstart sample
Creating files share: sample-share
Uploading file: sample-file
Listing file metadata...
key1:value1
key2:value2
Downloaded file contents: Hello Azure!
Deleting file: sample-file
Deleting files share: sample-share
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung wurde beschrieben, wie Sie Dateien per C++ hochladen, herunterladen und auflisten. Außerdem haben Sie erfahren, wie Sie eine Azure Storage-Dateifreigabe erstellen und löschen.

Ein C++-Blob Storage-Beispiel finden Sie hier:

> [!div class="nextstepaction"]
> [Azure Storage Files Share SDK v12 für C++ – Beispiele](https://github.com/Azure/azure-sdk-for-cpp/tree/main/sdk/storage/azure-storage-files-shares/sample)
