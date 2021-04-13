---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 07/20/2020
ms.author: msmbaldwin
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 6745d1f7d97a8f4d08078bf93b61762ab04aad46
ms.sourcegitcommit: f5448fe5b24c67e24aea769e1ab438a465dfe037
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "106072905"
---
Erstellen Sie ein Geheimnis mit dem Namen **mySecret** und dem Wert **Success!** . Bei einem Geheimnis kann es sich beispielsweise um ein Kennwort, eine SQL-Verbindungszeichenfolge oder um beliebige andere Informationen handeln, die sowohl sicher aufbewahrt werden als auch für Ihre Anwendung verfügbar sein müssen. 

Fügen Sie Ihrem neu erstellten Schlüsseltresor mit dem folgenden Befehl ein Geheimnis hinzu:

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)
```azurecli
az keyvault secret set --vault-name "<your-unique-keyvault-name>" --name "mySecret" --value "Success!"
```
# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azurepowershell)

```azurepowershell
$secret = ConvertTo-SecureString -String 'Success!' -AsPlainText
Set-AzKeyVaultSecret -VaultName <your-unique-keyvault-name> -Name mySecret -SecretValue $secret
```
---