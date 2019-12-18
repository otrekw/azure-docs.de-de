---
title: Verwenden von .NET zum Erstellen einer SAS für die Benutzerdelegierung für einen Container oder ein Blob
titleSuffix: Azure Storage
description: Erfahren Sie, wie Sie mithilfe der .NET-Clientbibliothek für Azure Storage eine SAS für die Benutzerdelegierung (Vorschau) mit Azure Active Directory-Anmeldeinformationen erstellen.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 12/04/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: blobs
ms.openlocfilehash: 915f2895fbc1e543fbbfbb408ba27eb758a40515
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2019
ms.locfileid: "74892531"
---
# <a name="create-a-user-delegation-sas-for-a-container-or-blob-with-net-preview"></a>Erstellen einer SAS für die Benutzerdelegierung für einen Container oder ein Blob mit .NET (Vorschau)

[!INCLUDE [storage-auth-sas-intro-include](../../../includes/storage-auth-sas-intro-include.md)]

In diesem Artikel wird beschrieben, wie Sie unter Verwendung von Azure AD-Anmeldeinformationen (Azure Active Directory) eine SAS für die Benutzerdelegierung (Vorschau) für einen Container oder ein Blob mit der Azure Storage-Clientbibliothek für .NET erstellen.

[!INCLUDE [storage-auth-user-delegation-include](../../../includes/storage-auth-user-delegation-include.md)]

## <a name="assign-rbac-roles-for-access-to-data"></a>Zuweisen von RBAC-Rollen für den Datenzugriff

Wenn ein Azure AD-Sicherheitsprinzipal versucht, auf Blobdaten zuzugreifen, muss dieser Sicherheitsprinzipal über Berechtigungen für die Ressource verfügen. Dem Sicherheitsprinzipal muss eine RBAC-Rolle zugewiesen werden, die den Zugriff auf Blobdaten in Azure Storage ermöglicht. Dabei spielt es keine Rolle, ob es sich bei dem Sicherheitsprinzipal um eine verwaltete Identität in Azure oder um ein Azure AD-Benutzerkonto handelt, mit dem Code in der Entwicklungsumgebung ausgeführt wird. Informationen zur Zuweisung von Berechtigungen per RBAC finden Sie unter **Autorisieren des Zugriffs auf Azure-Blobs und -Warteschlangen mit Azure Active Directory** im Abschnitt [Zuweisen von RBAC-Rollen für Zugriffsrechte](../common/storage-auth-aad.md#assign-rbac-roles-for-access-rights).

[!INCLUDE [storage-install-packages-blob-and-identity-include](../../../includes/storage-install-packages-blob-and-identity-include.md)]

Weitere Informationen zum Authentifizieren mit der Azure Identity-Clientbibliothek über Azure Storage finden Sie im Abschnitt **Authentifizieren mit der Azure Identity-Bibliothek** unter [Autorisieren des Zugriffs auf Blobs und Warteschlangen mit Azure Active Directory und verwalteten Identitäten für Azure-Ressourcen](../common/storage-auth-aad-msi.md?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json#authenticate-with-the-azure-identity-library).

## <a name="add-using-directives"></a>Hinzufügen von using-Direktiven

Fügen Sie dem Code die folgenden `using`-Anweisungen hinzu, um die Azure Identity- und Azure Storage-Clientbibliotheken zu verwenden.

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using Azure;
using Azure.Identity;
using Azure.Storage.Sas;
using Azure.Storage.Blobs;
using Azure.Storage.Blobs.Models;
```

## <a name="get-an-authenticated-token-credential"></a>Abrufen authentifizierter Tokenanmeldeinformationen

Erstellen Sie eine Instanz der Klasse [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential), um Tokenanmeldeinformationen abzurufen, die in Ihrem Code zum Autorisieren von Anforderungen für Azure Storage verwendet werden können.

Im folgenden Codeausschnitt sehen Sie, wie Sie die authentifizierten Tokenanmeldeinformationen abrufen und damit einen Dienstclient für Blob Storage erstellen:

```csharp
// Construct the blob endpoint from the account name.
string blobEndpoint = string.Format("https://{0}.blob.core.windows.net", accountName);

// Create a new Blob service client with Azure AD credentials.
BlobServiceClient blobClient = new BlobServiceClient(new Uri(blobEndpoint),
                                                     new DefaultAzureCredential());
```

## <a name="get-the-user-delegation-key"></a>Abrufen des Benutzerdelegierungsschlüssels

Jede SAS wird mit einem Schlüssel signiert. Zum Erstellen einer SAS für die Benutzerdelegierung müssen Sie zuerst einen Benutzerdelegierungsschlüssel anfordern, der dann zum Signieren der SAS verwendet wird. Der Benutzerdelegierungsschlüssel ist analog zum Kontoschlüssel, mit dem eine Dienst-SAS oder eine Konto-SAS signiert wird, mit der Ausnahme, dass er auf Ihren Azure AD-Anmeldeinformationen basiert. Wenn ein Client einen Benutzerdelegierungsschlüssel mithilfe eines OAuth 2.0-Tokens anfordert, gibt Azure Storage den Benutzerdelegierungsschlüssel im Namen des Benutzers zurück.

Nachdem Sie den Benutzerdelegierungsschlüssel abgerufen haben, können Sie mit diesem Schlüssel während seiner Lebensdauer eine beliebige Anzahl von Shared Access Signatures (SAS) für die Benutzerdelegierung erstellen. Der Benutzerdelegierungsschlüssel ist unabhängig vom OAuth 2.0-Token, das zum Abrufen des Schlüssels verwendet wird. Daher muss das Token nicht erneuert werden, solange der Schlüssel gültig ist. Sie können angeben, dass der Schlüssel für einen Zeitraum von bis zu 7 Tagen gültig ist.

Fordern Sie den Benutzerdelegierungsschlüssel mit einer der folgenden Methoden an:

- [GetUserDelegationKey](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.getuserdelegationkey)
- [GetUserDelegationKeyAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.getuserdelegationkeyasync)

Im folgenden Codeausschnitt werden der Benutzerdelegierungsschlüssel abgerufen und die zugehörigen Eigenschaften geschrieben:

```csharp
// Get a user delegation key for the Blob service that's valid for seven days.
// You can use the key to generate any number of shared access signatures over the lifetime of the key.
UserDelegationKey key = await blobClient.GetUserDelegationKeyAsync(DateTimeOffset.UtcNow,
                                                                   DateTimeOffset.UtcNow.AddDays(7));

// Read the key's properties.
Console.WriteLine("User delegation key properties:");
Console.WriteLine("Key signed start: {0}", key.SignedStartsOn);
Console.WriteLine("Key signed expiry: {0}", key.SignedExpiresOn);
Console.WriteLine("Key signed object ID: {0}", key.SignedObjectId);
Console.WriteLine("Key signed tenant ID: {0}", key.SignedTenantId);
Console.WriteLine("Key signed service: {0}", key.SignedService);
Console.WriteLine("Key signed version: {0}", key.SignedVersion);
```

## <a name="create-the-sas-token"></a>Erstellen des SAS-Tokens

Der folgende Codeausschnitt zeigt, wie ein neuer [BlobSasBuilder](/dotnet/api/azure.storage.sas.blobsasbuilder) erstellt wird und die Parameter für die SAS für die Benutzerdelegierung angegeben werden. Im Codeausschnitt werden dann die [ToSasQueryParameters](/dotnet/api/azure.storage.sas.blobsasbuilder.tosasqueryparameters) aufgerufen, um die SAS-Tokenzeichenfolge abzurufen. Schließlich wird mit dem Code der gesamte URI erstellt, einschließlich der Ressourcenadresse und dem SAS-Token.

```csharp
// Create a SAS token that's valid for one hour.
BlobSasBuilder sasBuilder = new BlobSasBuilder()
{
    BlobContainerName = containerName,
    BlobName = blobName,
    Resource = "b",
    StartsOn = DateTimeOffset.UtcNow,
    ExpiresOn = DateTimeOffset.UtcNow.AddHours(1)
};

// Specify read permissions for the SAS.
sasBuilder.SetPermissions(BlobSasPermissions.Read);

// Use the key to get the SAS token.
string sasToken = sasBuilder.ToSasQueryParameters(key, accountName).ToString();

// Construct the full URI, including the SAS token.
UriBuilder fullUri = new UriBuilder()
{
    Scheme = "https",
    Host = string.Format("{0}.blob.core.windows.net", accountName),
    Path = string.Format("{0}/{1}", containerName, blobName),
    Query = sasToken
};
```

## <a name="example-get-a-user-delegation-sas"></a>Beispiel: Abrufen einer SAS für die Benutzerdelegierung

In der folgenden Beispielmethode ist der gesamte Code für die Authentifizierung des Dienstprinzipals und die Erstellung der SAS für die Benutzerdelegierung zu sehen:

```csharp
async static Task<Uri> GetUserDelegationSasBlob(string accountName, string containerName, string blobName)
{
    // Construct the blob endpoint from the account name.
    string blobEndpoint = string.Format("https://{0}.blob.core.windows.net", accountName);

    // Create a new Blob service client with Azure AD credentials.  
    BlobServiceClient blobClient = new BlobServiceClient(new Uri(blobEndpoint), 
                                                            new DefaultAzureCredential());

    // Get a user delegation key for the Blob service that's valid for seven days.
    // You can use the key to generate any number of shared access signatures over the lifetime of the key.
    UserDelegationKey key = await blobClient.GetUserDelegationKeyAsync(DateTimeOffset.UtcNow, 
                                                                        DateTimeOffset.UtcNow.AddDays(7));

    // Read the key's properties.
    Console.WriteLine("User delegation key properties:");
    Console.WriteLine("Key signed start: {0}", key.SignedStartsOn);
    Console.WriteLine("Key signed expiry: {0}", key.SignedExpiresOn);
    Console.WriteLine("Key signed object ID: {0}", key.SignedObjectId);
    Console.WriteLine("Key signed tenant ID: {0}", key.SignedTenantId);
    Console.WriteLine("Key signed service: {0}", key.SignedService);
    Console.WriteLine("Key signed version: {0}", key.SignedVersion);
    Console.WriteLine();

    // Create a SAS token that's valid for one hour.
    BlobSasBuilder sasBuilder = new BlobSasBuilder()
    {
        BlobContainerName = containerName,
        BlobName = blobName,
        Resource = "b",
        StartsOn = DateTimeOffset.UtcNow,
        ExpiresOn = DateTimeOffset.UtcNow.AddHours(1)
    };

    // Specify read permissions for the SAS.
    sasBuilder.SetPermissions(BlobSasPermissions.Read);

    // Use the key to get the SAS token.
    string sasToken = sasBuilder.ToSasQueryParameters(key, accountName).ToString();

    // Construct the full URI, including the SAS token.
    UriBuilder fullUri = new UriBuilder()
    {
        Scheme = "https",
        Host = string.Format("{0}.blob.core.windows.net", accountName),
        Path = string.Format("{0}/{1}", containerName, blobName),
        Query = sasToken
    };

    Console.WriteLine("User delegation SAS URI: {0}", fullUri);
    Console.WriteLine();
    return fullUri.Uri;
}
```

## <a name="example-read-a-blob-with-a-user-delegation-sas"></a>Beispiel: Lesen eines Blobs mit einer SAS für die Benutzerdelegierung

Im folgenden Beispiel wird die im vorherigen Beispiel erstellte SAS für die Benutzerdelegierung in einer simulierten Clientanwendung getestet. Wenn die SAS gültig ist, können die Inhalte des Blobs in der Clientanwendung gelesen werden. Wenn die SAS ungültig ist, z. B. weil sie abgelaufen ist, wird in Azure Storage der Fehlercode 403 (Verboten) zurückgegeben.

```csharp
private static async Task ReadBlobWithSasAsync(Uri sasUri)
{
    // Try performing blob operations using the SAS provided.

    // Create a blob client object for blob operations.
    BlobClient blobClient = new BlobClient(sasUri, null);

    // Download and read the contents of the blob.
    try
    {
        // Download blob contents to a stream and read the stream.
        BlobDownloadInfo blobDownloadInfo = await blobClient.DownloadAsync();
        using (StreamReader reader = new StreamReader(blobDownloadInfo.Content, true))
        {
            string line;
            while ((line = reader.ReadLine()) != null)
            {
                Console.WriteLine(line);
            }
        }

        Console.WriteLine();
        Console.WriteLine("Read operation succeeded for SAS {0}", sasUri);
        Console.WriteLine();
    }
    catch (RequestFailedException e)
    {
        // Check for a 403 (Forbidden) error. If the SAS is invalid, 
        // Azure Storage returns this error.
        if (e.Status == 403)
        {
            Console.WriteLine("Read operation failed for SAS {0}", sasUri);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }
        else
        {
            Console.WriteLine(e.Message);
            Console.ReadLine();
            throw;
        }
    }
}
```

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

## <a name="see-also"></a>Weitere Informationen

- [Gewähren von eingeschränktem Zugriff auf Azure Storage-Ressourcen mithilfe von SAS (Shared Access Signature)](../common/storage-sas-overview.md)
- [Vorgang zum Abrufen eines Benutzerdelegierungsschlüssels](/rest/api/storageservices/get-user-delegation-key)
- [Erstellen einer SAS für die Benutzerdelegierung (REST-API)](/rest/api/storageservices/create-user-delegation-sas)
