---
title: Abrufen von Shared Access Signature-Token im Code | Azure Key Vault
description: Das Feature für verwaltete Speicherkonten bietet eine nahtlose Integration zwischen Azure Key Vault und einem Azure-Speicherkonto.
ms.topic: conceptual
ms.service: key-vault
ms.subservice: secrets
author: msmbaldwin
ms.author: mbaldwin
manager: rkarlin
ms.date: 09/10/2019
ms.openlocfilehash: e429115ce2624685c413ae252229964feee70137
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2020
ms.locfileid: "86232592"
---
# <a name="fetch-shared-access-signature-tokens-in-code"></a>Abrufen von Shared Access Signature-Token im Code

Sie können Ihr Speicherkonto mit den SAS-Token (Shared Access Signature) verwalten, die in Ihrem Schlüsseltresor gespeichert sind. Weitere Informationen finden Sie unter [Gewähren von eingeschränktem Zugriff auf Azure Storage-Ressourcen mithilfe von SAS](../../storage/common/storage-sas-overview.md).

Dieser Artikel enthält Beispiele für .NET-Code, der ein SAS-Token abruft und verschiedene Vorgänge damit ausführt. Weitere Informationen zum Erstellen und Speichern von SAS-Token finden Sie unter [Azure-Befehlszeilenschnittstelle](overview-storage-keys.md) oder [Verwalten von Speicherkontoschlüsseln mit Azure Key Vault und Azure PowerShell](overview-storage-keys-powershell.md).

## <a name="code-samples"></a>Codebeispiele

In diesem Beispiel ruft der Code ein SAS-Token aus Ihrem Schlüsselspeicher ab, verwendet es zur Erstellung eines neuen Speicherkontos und erstellt dann einen neuen Blobdienstclient.

```cs
// The shared access signature is stored as a secret in keyvault. 
// After you get a security token, create a new SecretClient with vault credentials and the key vault URI.
// The format for the key vault URI (kvuri) is https://<YourKeyVaultName>.vault.azure.net

var kv = new SecretClient(new Uri(kvUri), new DefaultAzureCredential());

// Now retrive your storage SAS token from Key Vault using the name of the secret (secretName).

KeyVaultSecret secret = client.GetSecret(secretName);
var sasToken = secret.Value;

// Create new storage credentials using the SAS token.
StorageCredentials accountSAS = new StorageCredentials(sasToken);

// Use these credentials and your storage account name to create a Blob service client.
CloudStorageAccount accountWithSAS = new CloudStorageAccount(accountSAS, "<storage-account>", endpointSuffix: null, useHttps: true);
CloudBlobClient blobClientWithSAS = accountWithSAS.CreateCloudBlobClient();
```

Sollte Ihr SAS-Token bald ablaufen, können Sie das SAS-Token aus Ihrem Schlüsseltresor abrufen und den Code aktualisieren.

```cs
// If your shared access signature token is about to expire,
// get the shared access signature token again from Key Vault and update it.
KeyVaultSecret secret = client.GetSecret(secretName);
var sasToken = secret.Value;
accountSAS.UpdateSASToken(sasToken);
```


## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie Sie [mithilfe von SAS eingeschränkten Zugriff auf Azure Storage-Ressourcen gewähren](../../storage/common/storage-sas-overview.md).
- Lernen Sie Folgendes: [Verwalten von Speicherkontoschlüsseln mit Azure Key Vault und der Azure-Befehlszeilenschnittstelle](overview-storage-keys.md) oder [Azure PowerShell](overview-storage-keys-powershell.md).
- Weitere Informationen finden Sie unter [Beispiele für verwaltete Speicherkontoschlüssel](https://github.com/Azure-Samples?utf8=%E2%9C%93&q=key+vault+storage&type=&language=).
