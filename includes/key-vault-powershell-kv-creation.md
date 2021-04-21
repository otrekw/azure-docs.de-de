---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 07/20/2020
ms.author: msmbaldwin
ms.openlocfilehash: d19c656946817b06cd620d8a48073bed8299af7d
ms.sourcegitcommit: 3b5cb7fb84a427aee5b15fb96b89ec213a6536c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107502254"
---
Verwenden Sie das Azure PowerShell-Cmdlet [New-AzKeyVault](/powershell/module/az.keyvault/new-azkeyvault), um in der Ressourcengruppe aus dem vorherigen Schritt eine Key Vault-Instanz zu erstellen. Dazu müssen einige Informationen angegeben werden:

- Name des Schlüsseltresors: Eine Zeichenfolge mit 3 bis 24 Zeichen, die nur Zahlen (0–9), Buchstaben (a–z, A–Z) und Bindestriche (-) enthalten darf.

  > [!Important]
  > Jeder Schlüsseltresor muss einen eindeutigen Namen haben. Ersetzen Sie in den folgenden Beispielen „<your-unique-keyvault-name>“ durch den Namen Ihres Schlüsseltresors.

- Ressourcengruppennamen: **myResourceGroup**
- Standort: **EastUS**

```azurepowershell-interactive
New-AzKeyVault -Name "<your-unique-keyvault-name>" -ResourceGroupName "myResourceGroup" -Location "East US"
```

Die Ausgabe dieses Cmdlets zeigt Eigenschaften des neu erstellten Schlüsseltresors. Beachten Sie die beiden folgenden Eigenschaften:

- **Tresorname**: Der Name, den Sie oben für den Parameter „--name“ angegeben haben.
- **Tresor-URI**: In dem Beispiel ist dies „https://&lt;Ihr-eindeutiger-Schlüsseltresor-Name&gt;.vault.azure.net/“. Anwendungen, die Ihren Tresor über die zugehörige REST-API nutzen, müssen diesen URI verwenden.

An diesem Punkt ist nur Ihr Azure-Konto zum Ausführen von Vorgängen für den neuen Tresor autorisiert.
