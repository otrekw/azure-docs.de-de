---
title: Aktivieren von SQL-TDE mit Azure Key Vault
description: Erfahren Sie, wie eine Azure SQL-Datenbank- und Data Warehouse-Instanz mithilfe von PowerShell oder CLI für die Verwendung von Transparent Data Encryption (TDE) zur Verschlüsselung ruhender Daten konfiguriert wird.
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
ms.openlocfilehash: e647725c5d30f35f39263757508bdd1725552731
ms.sourcegitcommit: d614a9fc1cc044ff8ba898297aad638858504efa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2019
ms.locfileid: "74997308"
---
# <a name="powershell-and-cli-enable-transparent-data-encryption-with-customer-managed-key-from-azure-key-vault"></a>PowerShell und Befehlszeilenschnittstelle: Aktivieren von Transparent Data Encryption mithilfe eines vom Kunden verwalteten Azure Key Vault-Schlüssels

In diesem Artikel wird die Verwendung eines Schlüssels aus Azure Key Vault für Transparent Data Encryption (TDE) auf einer SQL-Datenbank- oder Data Warehouse-Instanz erläutert. Weitere Informationen zur Integration von TDE mit Azure Key Vault und BYOK-Unterstützung (Bring Your Own Key) finden Sie unter [TDE mit vom Kunden verwalteten Schlüsseln in Azure Key Vault](transparent-data-encryption-byok-azure-sql.md).

## <a name="prerequisites-for-powershell"></a>Voraussetzungen für PowerShell

- Sie müssen über ein Azure-Abonnement verfügen und ein Administrator für dieses Abonnement sein.
- [Empfohlen, aber optional] Sie sollten über ein Hardwaresicherheitsmodul (HSM) oder einen lokalen Schlüsselspeicher zum Erstellen einer lokalen Kopie des Schlüsselmaterials der TDE-Schutzvorrichtung verfügen.
- Azure PowerShell Version muss installiert sein und ausgeführt werden.
- Erstellen Sie eine Azure Key Vault-Instanz und einen Schlüssel zur Verwendung für TDE.
  - [Anweisungen zur Verwendung eines Hardwaresicherheitsmodells (HSM) und Key Vault](../key-vault/key-vault-hsm-protected-keys.md)
    - Der Schlüsseltresor muss die folgende Eigenschaft aufweisen, um für TDE verwendet werden zu können:
  - [Vorläufiges Löschen](../key-vault/key-vault-ovw-soft-delete.md) und Löschschutz
- Der Schlüssel muss die folgenden Attribute aufweisen, um für TDE verwendet werden zu können:
   - Kein Ablaufdatum
   - Nicht deaktiviert
   - Fähigkeit zum Ausführen des *get*-, *wrap key*- und *unwrap key*-Vorgangs

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Anweisungen zur Installation des Az-Moduls finden Sie unter [Install Azure PowerShell](/powershell/azure/install-az-ps) (Installieren von Azure PowerShell). Spezifische Cmdlets finden Sie unter [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/).

Weitere Informationen zu Key Vault finden Sie im Artikel zu [PowerShell-Anweisungen von Key Vault](../key-vault/quick-create-powershell.md) und unter [Verwenden des vorläufigen Löschens in Key Vault mit PowerShell](../key-vault/key-vault-soft-delete-powershell.md).

> [!IMPORTANT]
> Das Azure Resource Manager-Modul von PowerShell wird von Azure SQL-Datenbank weiterhin unterstützt, aber alle zukünftigen Entwicklungen erfolgen für das Az.Sql-Modul. Das AzureRM-Modul erhält mindestens bis Dezember 2020 weiterhin Fehlerbehebungen.  Die Argumente für die Befehle im Az-Modul und den AzureRm-Modulen sind im Wesentlichen identisch. Weitere Informationen zur Kompatibilität finden Sie in der [Einführung in das neue Azure PowerShell Az-Modul](/powershell/azure/new-azureps-module-az).

## <a name="assign-an-azure-ad-identity-to-your-server"></a>Zuweisen einer Azure AD-Identität zu einem Server

Wenn bereits ein Server vorhanden ist, fügen Sie ihm mit dem folgenden Code eine Azure AD-Identität hinzu:

   ```powershell
   $server = Set-AzSqlServer -ResourceGroupName <SQLDatabaseResourceGroupName> -ServerName <LogicalServerName> -AssignIdentity
   ```

Wenn Sie einen Server erstellen, verwenden Sie das Cmdlet [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) mit dem Tag „-Identity“, um während der Servererstellung eine Azure AD-Identität hinzuzufügen:

   ```powershell
   $server = New-AzSqlServer -ResourceGroupName <SQLDatabaseResourceGroupName> -Location <RegionName> `
       -ServerName <LogicalServerName> -ServerVersion "12.0" -SqlAdministratorCredentials <PSCredential> -AssignIdentity
   ```

## <a name="grant-key-vault-permissions-to-your-server"></a>Gewähren von Key Vault-Berechtigungen für Ihren Server

Verwenden Sie das Cmdlet [Set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy), um Ihrem Server Zugriff auf den Schlüsseltresor zu gewähren, bevor Sie einen Schlüssel daraus für TDE verwenden.

   ```powershell
   Set-AzKeyVaultAccessPolicy -VaultName <KeyVaultName> `
       -ObjectId $server.Identity.PrincipalId -PermissionsToKeys get, wrapKey, unwrapKey
   ```

## <a name="add-the-key-vault-key-to-the-server-and-set-the-tde-protector"></a>Hinzufügen eines Key Vault-Schlüssels zum Server und Festlegen der TDE-Schutzvorrichtung

- Verwenden Sie das Cmdlet [Get-AzKeyVaultKey](/powershell/module/az.keyvault/get-azkeyvaultkey?view=azps-2.4.0), um die Schlüssel-ID aus Key Vault abzurufen.
- Fügen Sie dem Server mit dem Cmdlet [Add-AzSqlServerKeyVaultKey](/powershell/module/az.sql/add-azsqlserverkeyvaultkey) den Schlüssel aus Key Vault hinzu.
- Legen Sie den Schlüssel mit dem Cmdlet [Set-AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) als TDE-Schutzvorrichtung für alle Serverressourcen fest.
- Vergewissern Sie sich mit dem Cmdlet [Get-AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/get-azsqlservertransparentdataencryptionprotector), dass die TDE-Schutzvorrichtung wie vorgesehen konfiguriert wurde.

> [!NOTE]
> Die kombinierte Länge für den Schlüsseltresornamen und Schlüsselnamen darf 94 Zeichen nicht überschreiten.

> [!TIP]
> Beispiel für die KeyId-Eigenschaft in Key Vault: https://contosokeyvault.vault.azure.net/keys/Key1/1a1a2b2b3c3c4d4d5e5e6f6f7g7g8h8h

```powershell
# add the key from Key Vault to the server
Add-AzSqlServerKeyVaultKey -ResourceGroupName <SQLDatabaseResourceGroupName> -ServerName <LogicalServerName> -KeyId <KeyVaultKeyId>

# set the key as the TDE protector for all resources under the server
Set-AzSqlServerTransparentDataEncryptionProtector -ResourceGroupName <SQLDatabaseResourceGroupName> -ServerName <LogicalServerName> `
   -Type AzureKeyVault -KeyId <KeyVaultKeyId>

# confirm the TDE protector was configured as intended
Get-AzSqlServerTransparentDataEncryptionProtector -ResourceGroupName <SQLDatabaseResourceGroupName> -ServerName <LogicalServerName>
```

## <a name="turn-on-tde"></a>Aktivieren von TDE

Aktivieren Sie TDE mit dem Cmdlet [Set-AzSqlDatabaseTransparentDataEncryption](/powershell/module/az.sql/set-azsqldatabasetransparentdataencryption).

```powershell
Set-AzSqlDatabaseTransparentDataEncryption -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> -DatabaseName <DatabaseName> -State "Enabled"
```

Nun ist TDE für die Datenbank oder Data Warehouse-Instanz mit einem Verschlüsselungsschlüssel in Key Vault aktiviert.

## <a name="check-the-encryption-state-and-encryption-activity"></a>Überprüfen des Verschlüsselungsstatus und der Verschlüsselungsaktivität

Rufen Sie den Verschlüsselungsstatus mit dem Cmdlet [Get-AzSqlDatabaseTransparentDataEncryption](/powershell/module/az.sql/get-azsqldatabasetransparentdataencryption) ab, und überprüfen Sie den Verschlüsselungsfortschritt für eine Datenbank oder Data Warehouse-Instanz mit dem Cmdlet [Get-AzSqlDatabaseTransparentDataEncryptionActivity](/powershell/module/az.sql/get-azsqldatabasetransparentdataencryptionactivity).

```powershell
# get the encryption state
Get-AzSqlDatabaseTransparentDataEncryption -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> -DatabaseName <DatabaseName> `

# check the encryption progress for a database or data warehouse
Get-AzSqlDatabaseTransparentDataEncryptionActivity -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> -DatabaseName <DatabaseName>  
```

# <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Weitere Informationen zum Installieren der erforderlichen Version der Befehlszeilenschnittstelle (2.0 oder höher) und zum Verbindungsaufbau mit Ihrem Azure-Abonnement finden Sie unter [Installieren und Konfigurieren der plattformübergreifenden Azure-Befehlszeilenschnittstelle 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).

Weitere Informationen zu Key Vault finden Sie unter [Verwalten von Key Vault mit CLI-Version 2.0](../key-vault/key-vault-manage-with-cli2.md) und unter [Verwenden des vorläufigen Löschens in Key Vault mit der CLI](../key-vault/key-vault-soft-delete-cli.md).

## <a name="assign-an-azure-ad-identity-to-your-server"></a>Zuweisen einer Azure AD-Identität zu einem Server

```powershell
# create server (with identity) and database
az sql server create --name <servername> --resource-group <rgname>  --location <location> --admin-user <user> --admin-password <password> --assign-identity
az sql db create --name <dbname> --server <servername> --resource-group <rgname>
```

> [!TIP]
> Behalten Sie die „principalID“ aus der Servererstellung bei. Dies ist die Objekt-ID, die zum Zuweisen von Key Vault-Berechtigungen im nächsten Schritt verwendet wird.

## <a name="grant-key-vault-permissions-to-your-server"></a>Gewähren von Key Vault-Berechtigungen für Ihren Server

```powershell
# create key vault, key and grant permission
az keyvault create --name <kvname> --resource-group <rgname> --location <location> --enable-soft-delete true
az keyvault key create --name <keyname> --vault-name <kvname> --protection software
az keyvault set-policy --name <kvname>  --object-id <objectid> --resource-group <rgname> --key-permissions wrapKey unwrapKey get
```

> [!TIP]
> Behalten Sie den Schlüssel-URI oder die KeyID für den nächsten Schritt bei, zum Beispiel: https://contosokeyvault.vault.azure.net/keys/Key1/1a1a2b2b3c3c4d4d5e5e6f6f7g7g8h8h

## <a name="add-the-key-vault-key-to-the-server-and-set-the-tde-protector"></a>Hinzufügen eines Key Vault-Schlüssels zum Server und Festlegen der TDE-Schutzvorrichtung

```powershell
# add server key and update encryption protector
az sql server key create --server <servername> --resource-group <rgname> --kid <keyID>
az sql server tde-key set --server <servername> --server-key-type AzureKeyVault  --resource-group <rgname> --kid <keyID>
```

> [!NOTE]
> Die kombinierte Länge für den Schlüsseltresornamen und Schlüsselnamen darf 94 Zeichen nicht überschreiten.

## <a name="turn-on-tde"></a>Aktivieren von TDE

```powershell
# enable encryption
az sql db tde set --database <dbname> --server <servername> --resource-group <rgname> --status Enabled
```

Nun ist TDE für die Datenbank oder Data Warehouse-Instanz mit einem vom Kunden verwalteten Verschlüsselungsschlüssel in Azure Key Vault aktiviert.

## <a name="check-the-encryption-state-and-encryption-activity"></a>Überprüfen des Verschlüsselungsstatus und der Verschlüsselungsaktivität

```powershell
# get encryption scan progress
az sql db tde list-activity --database <dbname> --server <servername> --resource-group <rgname>  

# get whether encryption is on or off
az sql db tde show --database <dbname> --server <servername> --resource-group <rgname>
```

* * *

## <a name="useful-powershell-cmdlets"></a>Nützliche PowerShell-cmdlets

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

- Aktivieren Sie TDE mit dem Cmdlet [Set-AzSqlDatabaseTransparentDataEncryption](/powershell/module/az.sql/set-azsqldatabasetransparentdataencryption).

   ```powershell
   Set-AzSqlDatabaseTransparentDataEncryption -ServerName <LogicalServerName> -ResourceGroupName <SQLDatabaseResourceGroupName> `
      -DatabaseName <DatabaseName> -State "Disabled”
   ```

- Mit dem Cmdlet [Get-AzSqlServerKeyVaultKey](/powershell/module/az.sql/get-azsqlserverkeyvaultkey) geben Sie die Liste der Key Vault-Schlüssel zurück, die dem Server hinzugefügt wurden.

   ```powershell
   # KeyId is an optional parameter, to return a specific key version
   Get-AzSqlServerKeyVaultKey -ServerName <LogicalServerName> -ResourceGroupName <SQLDatabaseResourceGroupName>
   ```

- Mit dem Cmdlet [Remove-AzSqlServerKeyVaultKey](/powershell/module/az.sql/remove-azsqlserverkeyvaultkey) entfernen Sie einen Key Vault-Schlüssel vom Server.

   ```powershell
   # the key set as the TDE Protector cannot be removed
   Remove-AzSqlServerKeyVaultKey -KeyId <KeyVaultKeyId> -ServerName <LogicalServerName> -ResourceGroupName <SQLDatabaseResourceGroupName>
   ```

# <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

- Allgemeine Datenbankeinstellungen finden Sie unter [az sql](/cli/azure/sql).

- Die Einstellungen für Tresorschlüssel finden Sie unter [az sql server key](/cli/azure/sql/server/key).

- Die TDE-Einstellungen finden Sie unter [az sql server tde-key](/cli/azure/sql/server/tde-key) und [az sql db tde](/cli/azure/sql/db/tde).

* * *

## <a name="troubleshooting"></a>Problembehandlung

Überprüfen Sie Folgendes, wenn ein Problem auftritt:

- Wenn der Schlüsseltresor nicht gefunden werden kann, stellen Sie sicher, dass Sie sich im richtigen Abonnement befinden.

   # <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

   ```powershell
   Get-AzSubscription -SubscriptionId <SubscriptionId>
   ```

   # <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

   ```powershell
   az account show - s <SubscriptionId>
   ```

   * * *

- Wenn der neue Schlüssel dem Server nicht hinzugefügt oder nicht als TDE-Schutzvorrichtung aktualisiert werden kann, überprüfen Sie Folgendes:
   - Der Schlüssel darf kein Ablaufdatum haben.
   - Für den Schlüssel müssen die *get*-, *wrap key*- und *unwrap key*-Vorgänge aktiviert sein.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie die TDE-Schutzvorrichtung eines Servers rotieren, um Sicherheitsanforderungen zu erfüllen: [Rotieren der Transparent Data Encryption-Schutzvorrichtung mithilfe von PowerShell](transparent-data-encryption-byok-azure-sql-key-rotation.md).
- Im Fall eines Sicherheitsrisikos können Sie hier nachsehen, wie Sie eine möglicherweise kompromittierte TDE-Schutzvorrichtung entfernen: [Entfernen eines möglicherweise kompromittierten Schlüssels](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md).
