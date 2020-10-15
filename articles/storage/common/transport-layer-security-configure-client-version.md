---
title: Konfigurieren der Transport Layer Security (TLS) für eine Clientanwendung
titleSuffix: Azure Storage
description: Konfigurieren Sie eine Clientanwendung für die Kommunikation mit Azure Storage mit einer Mindestversion von Transport Layer Security (TLS).
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 07/08/2020
ms.author: tamram
ms.reviewer: fryu
ms.subservice: common
ms.custom: devx-track-csharp
ms.openlocfilehash: 37b8c79df5b208feea185292fa09c323b64fa27d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89001806"
---
# <a name="configure-transport-layer-security-tls-for-a-client-application"></a>Konfigurieren der Transport Layer Security (TLS) für eine Clientanwendung

Aus Sicherheitsgründen ist es für ein Azure Storage-Konto möglicherweise erforderlich, dass Clients eine Mindestversion von Transport Layer Security (TLS) zum Senden von Anforderungen verwenden. Aufrufe an Azure Storage schlagen fehl, wenn der Client eine TLS-Version verwendet, die niedriger als die erforderliche Mindestversion ist. Wenn für ein Speicherkonto zum Beispiel TLS 1.2 erforderlich ist, schlägt eine Anforderung fehl, die von einem Client gesendet wird, der TLS 1.1 verwendet.

In diesem Artikel wird beschrieben, wie Sie eine Clientanwendung für die Verwendung einer bestimmten TLS-Version konfigurieren. Informationen dazu, wie Sie eine mindestens erforderliche Version von TLS für ein Azure Storage-Konto konfigurieren, finden Sie unter [Konfigurieren der erforderlichen Mindestversion der Transport Layer Security (TLS) für ein Speicherkonto](transport-layer-security-configure-minimum-version.md).

## <a name="configure-the-client-tls-version"></a>Konfigurieren der TLS-Clientversion

Damit ein Client eine Anforderung mit einer bestimmten TLS-Version senden kann, muss das Betriebssystem diese Version unterstützen.

In den folgenden Beispielen wird gezeigt, wie die TLS-Version des Clients von PowerShell oder .NET auf 1.2 festgelegt wird. Die vom Client verwendete .NET Framework-Instanz muss TLS 1.2 unterstützen. Weitere Informationen finden Sie unter [Unterstützung für TLS 1.2](/dotnet/framework/network-programming/tls#support-for-tls-12).

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Im folgenden Beispiel wird gezeigt, wie Sie TLS 1.2 in einem PowerShell-Client aktivieren:

```powershell
# Set the TLS version used by the PowerShell client to TLS 1.2.
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12;

# Create a new container.
$storageAccount = Get-AzStorageAccount -ResourceGroupName $rgName -Name $accountName
$ctx = $storageAccount.Context
New-AzStorageContainer -Name "sample-container" -Context $ctx
```

# <a name="net-v12"></a>[.NET v12](#tab/dotnet)

Im folgenden Beispiel wird gezeigt, wie Sie TLS 1.2 in einem .NET-Client mit Version 12 der Azure Storage-Clientbibliothek aktivieren.

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Networking.cs" id="Snippet_ConfigureTls12":::

# <a name="net-v11"></a>[.NET v11](#tab/dotnet11)

Im folgenden Beispiel wird gezeigt, wie Sie TLS 1.2 in einem .NET-Client mit Version 11 der Azure Storage-Clientbibliothek aktivieren:

```csharp
static void EnableTls12()
{
    // Enable TLS 1.2 before connecting to Azure Storage
    System.Net.ServicePointManager.SecurityProtocol = System.Net.SecurityProtocolType.Tls12;

    // Add your connection string here.
    string connectionString = "";

    // Connect to Azure Storage and create a new container.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
    container.CreateIfNotExists();
}
```

---

## <a name="verify-the-tls-version-used-by-a-client"></a>Überprüfen der von einem Client verwendeten TLS-Version

Wenn Sie überprüfen möchten, ob die angegebene TLS-Version vom Client verwendet wurde, um eine Anforderung zu senden, können Sie [Fiddler](https://www.telerik.com/fiddler) oder ein ähnliches Tool verwenden. Öffnen Sie Fiddler, um den Netzwerkdatenverkehr auf dem Client zu erfassen, und führen Sie dann eines der Beispiele im vorherigen Abschnitt aus. Sehen Sie sich die Fiddler-Ablaufverfolgung an, um zu bestätigen, dass wie im Bild unten dargestellt die richtige TLS-Version zum Senden der Anforderung verwendet wurde.

:::image type="content" source="media/transport-layer-security-configure-client-version/fiddler-trace-tls-version.png" alt-text="Screenshot der Fiddler-Ablaufverfolgung mit der für die Anforderung verwendeten TLS-Version":::

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren der erforderlichen Mindestversion der Transport Layer Security (TLS) für ein Speicherkonto](transport-layer-security-configure-minimum-version.md)
- [Sicherheitsempfehlungen für Blob Storage](../blobs/security-recommendations.md)
