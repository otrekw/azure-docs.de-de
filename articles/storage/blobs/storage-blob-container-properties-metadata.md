---
title: Verwalten von Eigenschaften und Metadaten für einen Blobcontainer mit .NET
titleSuffix: Azure Storage
description: Hier erfahren Sie, wie Sie mithilfe der .NET-Clientbibliothek Systemeigenschaften festlegen und abrufen und benutzerdefinierte Metadaten für Blobcontainer in Ihrem Azure Storage-Konto speichern.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 07/01/2020
ms.author: tamram
ms.openlocfilehash: 3a48e598680ee969f3823dee6699c8c981dff392
ms.sourcegitcommit: faeabfc2fffc33be7de6e1e93271ae214099517f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2020
ms.locfileid: "88182475"
---
# <a name="manage-container-properties-and-metadata-with-net"></a>Verwalten von Containereigenschaften und -metadaten mit .NET

Blobcontainer unterstützen neben den enthaltenen Daten auch Systemeigenschaften und benutzerdefinierte Metadaten. In diesem Artikel erfahren Sie, wie Sie Systemeigenschaften und benutzerdefinierte Metadaten mithilfe der [Azure Storage-Clientbibliothek für .NET](/dotnet/api/overview/azure/storage?view=azure-dotnet) verwalten.

## <a name="about-properties-and-metadata"></a>Informationen zu Eigenschaften und Metadaten

- **Systemeigenschaften**: Systemeigenschaften sind in jeder Blobspeicherressource vorhanden. Einige davon können gelesen oder festgelegt werden, während andere schreibgeschützt sind. Darüber hinaus entsprechen einige Systemeigenschaften bestimmten HTTP-Standardheadern. Die Azure Storage-Clientbibliothek für .NET verwaltet diese Eigenschaften für Sie.

- **Benutzerdefinierte Metadaten**: Benutzerdefinierte Metadaten bestehen aus mindestens einem Name-Wert-Paar, das Sie für eine Blobspeicherressource angeben. Metadaten können verwendet werden, um zusätzliche Werte mit der Ressource zu speichern. Metadatenwerte sind nur für Ihre eigenen Zwecke bestimmt und wirken sich nicht auf das Verhalten der Ressource aus.

Name-Wert-Paare für Metadaten sind gültige HTTP-Header und sollten daher allen Einschränkungen für HTTP-Header entsprechen. Metadatennamen müssen gültige HTTP-Headernamen und gültige C#-Bezeichner sein, dürfen nur ASCII-Zeichen enthalten und sollten als „Keine Beachtung von Groß-/Kleinschreibung“ behandelt werden. Metadatenwerte mit Nicht-ASCII-Zeichen sollten Base64- oder URL-codiert werden.

## <a name="retrieve-container-properties"></a>Abrufen von Containereigenschaften

# <a name="net-v12"></a>[.NET v12](#tab/dotnet)

Rufen Sie zum Abrufen von Containereigenschaften eine der folgenden Methoden auf:

- [GetProperties](/dotnet/api/azure.storage.blobs.blobcontainerclient.getproperties)
- [GetPropertiesAsync](/dotnet/api/azure.storage.blobs.blobcontainerclient.getpropertiesasync)

Das folgende Codebeispiel ruft die Systemeigenschaften eines Containers ab und schreibt einige Eigenschaftswerte in ein Konsolenfenster:

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Metadata.cs" id="Snippet_ReadContainerProperties":::

# <a name="net-v11"></a>[.NET v11](#tab/dotnet11)

Das Abrufen von Eigenschafts- und Metadatenwerten einer Blobspeicherressource ist ein zweistufiger Prozess. Bevor Sie diese Werte lesen können, müssen Sie sie explizit durch Aufrufen der **FetchAttributesAsync** oder **FetchAttributesAsync**-Methode abrufen. Ausnahme: Die Methoden **Exists** und **ExistsAsync** rufen die entsprechende Methode vom Typ **FetchAttributes** im Hintergrund auf. Wenn Sie eine dieser Methoden aufrufen, müssen Sie **FetchAttributes** nicht aufrufen.

> [!IMPORTANT]
> Wenn Sie feststellen, dass Eigenschaften- oder Metadatenwerte für eine Speicherressource nicht ausgefüllt wurden, überprüfen Sie, ob Ihr Code die **FetchAttributes**- oder **FetchAttributesAsync**-Methode aufruft.

Rufen Sie zum Abrufen von Containereigenschaften eine der folgenden Methoden auf:

- [FetchAttributes](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.fetchattributes)
- [FetchAttributesAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.fetchattributesasync)

Das folgende Codebeispiel ruft die Systemeigenschaften eines Containers ab und schreibt einige Eigenschaftswerte in ein Konsolenfenster:

```csharp
private static async Task ReadContainerPropertiesAsync(CloudBlobContainer container)
{
    try
    {
        // Fetch some container properties and write out their values.
        await container.FetchAttributesAsync();
        Console.WriteLine("Properties for container {0}", container.StorageUri.PrimaryUri);
        Console.WriteLine("Public access level: {0}", container.Properties.PublicAccess);
        Console.WriteLine("Last modified time in UTC: {0}", container.Properties.LastModified);
    }
    catch (StorageException e)
    {
        Console.WriteLine("HTTP error code {0}: {1}",
                            e.RequestInformation.HttpStatusCode,
                            e.RequestInformation.ErrorCode);
        Console.WriteLine(e.Message);
        Console.ReadLine();
    }
}
```

---

## <a name="set-and-retrieve-metadata"></a>Festlegen und Abrufen von Metadaten

# <a name="net-v12"></a>[.NET v12](#tab/dotnet)

Sie können Metadaten als ein oder mehrere Name-Wert-Paare für eine Blob- oder Containerressource angeben. Fügen Sie zum Festlegen von Metadaten Name-Wert-Paare zu einem Objekt vom Typ [IDictionary](/dotnet/api/system.collections.idictionary) hinzu, und rufen Sie dann eine der folgenden Methoden auf, um die Werte zu schreiben:

- [SetMetadata](/dotnet/api/azure.storage.blobs.blobcontainerclient.setmetadata)
- [SetMetadataAsync](/dotnet/api/azure.storage.blobs.blobcontainerclient.setmetadataasync)

Der Name der Metadaten muss den Benennungskonventionen für C#-Bezeichner entsprechen. Bei Metadatennamen wird die Groß-/Kleinschreibung beibehalten, mit der sie erstellt wurden. Beim Festlegen oder Lesen wird die Groß-/Kleinschreibung allerdings ignoriert. Werden für eine Ressource mehrere Metadatenheader mit dem gleichen Namen übermittelt, werden die Werte durch Kommas getrennt und verkettet. Anschließend wird der HTTP-Antwortcode 200 (OK) zurückgegeben.

Das folgende Codebeispiel legt die Metadaten für einen Container fest.

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Metadata.cs" id="Snippet_AddContainerMetadata":::

Rufen Sie zum Abrufen von Metadaten eine der folgenden Methoden auf:

- [GetProperties](/dotnet/api/azure.storage.blobs.blobcontainerclient.getproperties)
- [GetPropertiesAsync](/dotnet/api/azure.storage.blobs.blobcontainerclient.getpropertiesasync)

Lesen Sie anschließend die Werte, wie im folgenden Beispiel gezeigt:

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Metadata.cs" id="Snippet_ReadContainerMetadata":::

# <a name="net-v11"></a>[.NET v11](#tab/dotnet11)

Sie können Metadaten als ein oder mehrere Name-Wert-Paare für eine Blob- oder Containerressource angeben. Fügen Sie zum Festlegen von Metadaten Name-Wert-Paare zur Sammlung **Metadaten** der Ressource hinzu, und rufen Sie dann eine der folgenden Methoden auf, um die Werte zu schreiben:

- [SetMetadata](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.setmetadata)
- [SetMetadataAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.setmetadataasync)

Der Name der Metadaten muss den Benennungskonventionen für C#-Bezeichner entsprechen. Bei Metadatennamen wird die Groß-/Kleinschreibung beibehalten, mit der sie erstellt wurden. Beim Festlegen oder Lesen wird die Groß-/Kleinschreibung allerdings ignoriert. Werden für eine Ressource mehrere Metadatenheader mit dem gleichen Namen übermittelt, werden die Werte durch Kommas getrennt und verkettet. Anschließend wird der HTTP-Antwortcode 200 (OK) zurückgegeben.

Das folgende Codebeispiel legt die Metadaten für einen Container fest. Mittels der **Hinzufügen** -Methode der Sammlung wird ein Wert festgelegt. Der andere Wert wird mit der impliziten Schlüssel-Wert-Syntax festgelegt. Beide eignen sich hierzu.

```csharp
public static async Task AddContainerMetadataAsync(CloudBlobContainer container)
{
    try
    {
        // Add some metadata to the container.
        container.Metadata.Add("docType", "textDocuments");
        container.Metadata["category"] = "guidance";

        // Set the container's metadata.
        await container.SetMetadataAsync();
    }
    catch (StorageException e)
    {
        Console.WriteLine("HTTP error code {0}: {1}",
                            e.RequestInformation.HttpStatusCode,
                            e.RequestInformation.ErrorCode);
        Console.WriteLine(e.Message);
        Console.ReadLine();
    }
}
```

Zum Abrufen von Metadaten rufen Sie die **FetchAttributes**- oder **FetchAttributesAsync**-Methode für das Blob oder den Container auf, um die **Metadaten**-Sammlung zu füllen, und lesen anschließend die Werte wie im unten stehenden Beispiel gezeigt.

```csharp
public static async Task ReadContainerMetadataAsync(CloudBlobContainer container)
{
    try
    {
        // Fetch container attributes in order to populate the container's properties and metadata.
        await container.FetchAttributesAsync();

        // Enumerate the container's metadata.
        Console.WriteLine("Container metadata:");
        foreach (var metadataItem in container.Metadata)
        {
            Console.WriteLine("\tKey: {0}", metadataItem.Key);
            Console.WriteLine("\tValue: {0}", metadataItem.Value);
        }
    }
    catch (StorageException e)
    {
        Console.WriteLine("HTTP error code {0}: {1}",
                            e.RequestInformation.HttpStatusCode,
                            e.RequestInformation.ErrorCode);
        Console.WriteLine(e.Message);
        Console.ReadLine();
    }
}
```

---

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

## <a name="see-also"></a>Weitere Informationen

- [Get Container Properties](/rest/api/storageservices/get-container-properties)
- [Set Container Metadata](/rest/api/storageservices/set-container-metadata)
- [Get Container Metadata](/rest/api/storageservices/get-container-metadata)
