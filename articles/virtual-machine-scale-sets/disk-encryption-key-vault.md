---
title: Erstellen und Konfigurieren eines Schlüsseltresors für Azure Disk Encryption
description: Dieser Artikel enthält Schritte zum Erstellen und Konfigurieren eines Schlüsseltresors für die Verwendung mit Azure Disk Encryption.
author: ju-shim
ms.author: jushiman
ms.topic: tutorial
ms.service: virtual-machine-scale-sets
ms.subservice: disks
ms.date: 10/10/2019
ms.reviewer: mimckitt
ms.custom: mimckitt, devx-track-azurecli
ms.openlocfilehash: d0b35b0976b46dd22bbf7f15ebb1def3ec721b5d
ms.sourcegitcommit: 5ce88326f2b02fda54dad05df94cf0b440da284b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107891462"
---
# <a name="create-and-configure-a-key-vault-for-azure-disk-encryption"></a>Erstellen und Konfigurieren eines Schlüsseltresors für Azure Disk Encryption

Azure Disk Encryption verwendet Azure Key Vault zum Steuern und Verwalten von Verschlüsselungsschlüsseln und Geheimnissen für die Datenträgerverschlüsselung.  Weitere Informationen zu Schlüsseltresoren finden Sie unter [Informationen zu Azure Key Vault](../key-vault/general/overview.md) und [Azure Key Vault-Sicherheit](../key-vault/general/security-features.md).

Das Erstellen und Konfigurieren eines Schlüsseltresors für Azure Disk Encryption umfasst drei Schritte:

1. Erstellen einer Ressourcengruppe (bei Bedarf)
2. Erstellen eines Schlüsseltresors 
3. Festlegen von erweiterten Zugriffsrichtlinien für den Schlüsseltresor

Diese Schritte werden in den folgenden Schnellstarts veranschaulicht:

Sie können auch einen Schlüssel für die Schlüsselverschlüsselung (Key Encryption Key, KEK) generieren oder importieren.

## <a name="install-tools-and-connect-to-azure"></a>Installieren von Tools und Herstellen einer Verbindung mit Azure

Die Schritte in diesem Artikel können mit der [Azure-Befehlszeilenschnittstelle](/cli/azure/), dem [Azure PowerShell-Az-Modul](/powershell/azure/) oder dem [Azure-Portal](https://portal.azure.com) abgeschlossen werden.

### <a name="connect-to-your-azure-account"></a>Herstellen einer Verbindung mit Ihrem Azure-Konto

Vor der Verwendung der Azure-Befehlszeilenschnittstelle oder von Azure PowerShell müssen Sie zunächst eine Verbindung mit Ihrem Azure-Abonnement herstellen. Dies erfolgt durch [Anmelden mit der Azure-Befehlszeilenschnittstelle](/cli/azure/authenticate-azure-cli), [Anmelden mit Azure PowerShell](/powershell/azure/authenticate-azureps) oder Bereitstellen der Anmeldeinformationen im Azure-Portal nach Aufforderung.

```azurecli-interactive
az login
```

```azurepowershell-interactive
Connect-AzAccount
```

[!INCLUDE [disk-encryption-key-vault](../../includes/disk-encryption-key-vault.md)]
 
## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über Azure Disk Encryption](disk-encryption-overview.md)
- [Verschlüsseln von Betriebssystem- und angefügten Datenträgern in einer VM-Skalierungsgruppe mit Azure CLI](disk-encryption-cli.md)
- [Verschlüsseln von Betriebssystem- und angefügten Datenträgern in einer VM-Skalierungsgruppe mit Azure PowerShell](disk-encryption-powershell.md)
