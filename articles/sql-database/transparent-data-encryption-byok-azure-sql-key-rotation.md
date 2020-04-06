---
title: Rotieren einer TDE-Schutzvorrichtung – PowerShell
description: Erfahren Sie, wie die Transparent Data Encryption-Schutzvorrichtung (TDE) für einen Azure SQL-Server rotiert wird.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
ms.reviewer: vanto
ms.date: 03/12/2019
ms.openlocfilehash: aaed06ac086893f63fde530e46b936b3fb637766
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80067170"
---
# <a name="rotate-the-transparent-data-encryption-tde-protector-using-powershell"></a>Rotieren der Transparent Data Encryption (TDE)-Schutzvorrichtung mithilfe von PowerShell

Dieser Artikel beschreibt die Schlüsselrotation für einen Azure SQL-Server mithilfe einer TDE-Schutzvorrichtung aus Azure Key Vault. Das Rotieren der TDE-Schutzvorrichtung eines Azure SQL-Servers bedeutet, dass zu einem neuen asymmetrischen Schlüssel gewechselt wird, der die Datenbanken auf dem Server schützt. Die Schlüsselrotation ist ein Onlinevorgang, der in wenigen Sekunden abgeschlossen sein sollte, da nur der Datenverschlüsselungsschlüssel der Datenbank entschlüsselt und wieder verschlüsselt wird, aber nicht die gesamte Datenbank.

Dieser Leitfaden erläutert zwei Optionen zum Rotieren der TDE-Schutzvorrichtung auf dem Server.

> [!NOTE]
> Ein angehaltener SQL Data Warehouse-Dienst muss vor den Schlüsselrotationen fortgesetzt werden.

> [!IMPORTANT]
> Löschen Sie keine vorherigen Versionen des Schlüssels nach einem Rollover. Bei einem Rollover von Schlüsseln sind einige Daten noch immer mit den vorherigen Schlüsseln verschlüsselt, z.B. ältere Datenbanksicherungen.

## <a name="prerequisites"></a>Voraussetzungen

- In diesem Leitfaden wird davon ausgegangen, dass Sie bereits einen Schlüssel aus Azure Key Vault als TDE-Schutzvorrichtung für eine Azure SQL-Datenbank- oder Data Warehouse-Instanz verwenden. Siehe [Transparent Data Encryption mit BYOK-Unterstützung](transparent-data-encryption-byok-azure-sql.md).
- Azure PowerShell muss installiert sein und ausgeführt werden.
- [Empfohlen, aber optional] Erstellen Sie das Schlüsselmaterial für die TDE-Schutzvorrichtung zunächst in einem Hardwaresicherheitsmodul (HSM) oder einem lokalen Schlüsselspeicher, und importieren Sie das Schlüsselmaterial in Azure Key Vault. Weitere Informationen erhalten Sie, wenn Sie die [Anweisungen zur Verwendung eines Hardwaresicherheitsmodells (HSM) und Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-get-started) befolgen.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Anweisungen zur Installation des Az-Moduls finden Sie unter [Install Azure PowerShell](/powershell/azure/install-az-ps) (Installieren von Azure PowerShell). Spezifische Cmdlets finden Sie unter [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/).

> [!IMPORTANT]
> Das Azure Resource Manager-Modul von PowerShell wird von Azure SQL-Datenbank weiterhin unterstützt, aber alle zukünftigen Entwicklungen erfolgen für das Az.Sql-Modul. Das AzureRM-Modul erhält mindestens bis Dezember 2020 weiterhin Fehlerbehebungen.  Die Argumente für die Befehle im Az-Modul und den AzureRm-Modulen sind im Wesentlichen identisch. Weitere Informationen zur Kompatibilität finden Sie in der [Einführung in das neue Azure PowerShell Az-Modul](/powershell/azure/new-azureps-module-az).

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Informationen zur Installation finden Sie unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

* * *

## <a name="manual-key-rotation"></a>Manuelle Schlüsselrotation

Die manuelle Schlüsselrotation nutzt die folgenden Befehle zum Hinzufügen eines völlig neuen Schlüssels, der unter einem neuen Schlüsselnamen oder sogar in einem anderen Schlüsseltresor gespeichert werden kann. Bei diesem Ansatz kann derselbe Schlüssel unterschiedlichen Schlüsseltresoren hinzugefügt werden, um Hochverfügbarkeit und geografisch verteilte Notfallwiederherstellungen zu unterstützen.

> [!NOTE]
> Die kombinierte Länge für den Schlüsseltresornamen und Schlüsselnamen darf 94 Zeichen nicht überschreiten.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Verwenden Sie die Cmdlets [Add-AzKeyVaultKey](/powershell/module/az.keyvault/Add-AzKeyVaultKey), [Add-AzSqlServerKeyVaultKey](/powershell/module/az.sql/add-azsqlserverkeyvaultkey) und [Set-AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector).

```powershell
# add a new key to Key Vault
Add-AzKeyVaultKey -VaultName <keyVaultName> -Name <keyVaultKeyName> -Destination <hardwareOrSoftware>

# add the new key from Key Vault to the server
Add-AzSqlServerKeyVaultKey -KeyId <keyVaultKeyId> -ServerName <logicalServerName> -ResourceGroup <SQLDatabaseResourceGroupName>
  
# set the key as the TDE protector for all resources under the server
Set-AzSqlServerTransparentDataEncryptionProtector -Type AzureKeyVault -KeyId <keyVaultKeyId> `
   -ServerName <logicalServerName> -ResourceGroup <SQLDatabaseResourceGroupName>
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Verwenden Sie die Befehle [az keyvault key create](/cli/azure/keyvault/key#az-keyvault-key-create), [az sql server key create](/cli/azure/sql/server/key#az-sql-server-key-create) und [az sql server tde-key set](/cli/azure/sql/server/tde-key#az-sql-server-tde-key-set).

```azurecli
# add a new key to Key Vault
az keyvault key create --name <keyVaultKeyName> --vault-name <keyVaultName> --protection <hsmOrSoftware>

# add the new key from Key Vault to the server
az sql server key create --kid <keyVaultKeyId> --resource-group <SQLDatabaseResourceGroupName> --server <logicalServerName>

# set the key as the TDE protector for all resources under the server
az sql server tde-key set --server-key-type AzureKeyVault --kid <keyVaultKeyId> --resource-group <SQLDatabaseResourceGroupName> --server <logicalServerName>
```

* * *

## <a name="useful-powershell-cmdlets"></a>Nützliche PowerShell-cmdlets

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

- Zum Ändern der TDE-Schutzvorrichtung vom durch Microsoft verwalteten Modus in den BYOK-Modus verwenden Sie das Cmdlet [Set-AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector).

   ```powershell
   Set-AzSqlServerTransparentDataEncryptionProtector -Type AzureKeyVault `
       -KeyId <keyVaultKeyId> -ServerName <logicalServerName> -ResourceGroup <SQLDatabaseResourceGroupName>
   ```

- Zum Ändern der TDE-Schutzvorrichtung vom BYOK-Modus zum durch Microsoft verwalteten Modus verwenden Sie das Cmdlet [Set-AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector).

   ```powershell
   Set-AzSqlServerTransparentDataEncryptionProtector -Type ServiceManaged `
       -ServerName <logicalServerName> -ResourceGroup <SQLDatabaseResourceGroupName>
   ```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

In den folgenden Beispielen wird der Befehl [az sql server tde-key set](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) verwendet,

- um die TDE-Schutzvorrichtung aus der Microsoft-Verwaltung in den BYOK-Modus (Bring Your Own Key) zu wechseln.

   ```azurecli
   az sql server tde-key set --server-key-type AzureKeyVault --kid <keyVaultKeyId> --resource-group <SQLDatabaseResourceGroupName> --server <logicalServerName>
   ```

- um die TDE-Schutzvorrichtung aus dem BYOK-Modus in die Microsoft-Verwaltung zu wechseln.

   ```azurecli
   az sql server tde-key set --server-key-type ServiceManaged --resource-group <SQLDatabaseResourceGroupName> --server <logicalServerName>
   ```

* * *

## <a name="next-steps"></a>Nächste Schritte

- Im Falle eines Sicherheitsrisikos können Sie hier nachsehen, wie Sie eine möglicherweise kompromittierte TDE-Schutzvorrichtung entfernen: [Entfernen eines möglicherweise kompromittierten Schlüssels](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md)

- Erste Schritte mit der Azure Key Vault-Integration und BYOK-Unterstützung (Bring Your Own Key) für TDE: [Aktivieren von TDE mit BYOK aus Key Vault mithilfe von PowerShell](transparent-data-encryption-byok-azure-sql-configure.md)
