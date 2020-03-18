---
title: Datenverschlüsselung für Azure Database for MySQL über das Azure-Portal
description: Erfahren Sie, wie Sie über das Azure-Portal die Datenverschlüsselung für Azure Database for MySQL einrichten und verwalten.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 01/13/2020
ms.openlocfilehash: 55c155dc4396672c02322c6c5727dac57d0ac8ef
ms.sourcegitcommit: 668b3480cb637c53534642adcee95d687578769a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2020
ms.locfileid: "78898731"
---
# <a name="data-encryption-for-azure-database-for-mysql-by-using-the-azure-portal"></a>Datenverschlüsselung für Azure Database for MySQL über das Azure-Portal

Erfahren Sie, wie Sie über das Azure-Portal die Datenverschlüsselung für Azure Database for MySQL einrichten und verwalten.

## <a name="prerequisites-for-azure-cli"></a>Voraussetzungen für die Azure-Befehlszeilenschnittstelle

* Sie müssen über ein Azure-Abonnement verfügen und ein Administrator für dieses Abonnement sein.
* Erstellen Sie in Azure Key Vault einen Schlüsseltresor und einen Schlüssel, der als vom Kunden verwalteter Schlüssel verwendet werden soll.
* Der Schlüsseltresor muss die folgenden Eigenschaften aufweisen, damit er für vom Kunden verwaltete Schlüssel verwendet werden kann:
  * [Vorläufiges Löschen](../key-vault/key-vault-ovw-soft-delete.md)

    ```azurecli-interactive
    az resource update --id $(az keyvault show --name \ <key_vault_name> -o tsv | awk '{print $1}') --set \ properties.enableSoftDelete=true
    ```

  * [Löschschutz](../key-vault/key-vault-ovw-soft-delete.md#purge-protection)

    ```azurecli-interactive
    az keyvault update --name <key_vault_name> --resource-group <resource_group_name>  --enable-purge-protection true
    ```

* Der Schlüssel muss die folgenden Attribute aufweisen, damit er als vom Kunden verwalteter Schlüssel verwendet werden kann:
  * Kein Ablaufdatum
  * Nicht deaktiviert
  * Fähigkeit zum Ausführen der Vorgänge zum Abrufen, Packen und Entpacken von Schlüsseln

## <a name="set-the-right-permissions-for-key-operations"></a>Festlegen der richtigen Berechtigungen für Schlüsselvorgänge

1. Wählen Sie in Key Vault **Zugriffsrichtlinien** > **Zugriffsrichtlinie hinzufügen** aus.

   ![Screenshot von Key Vault mit hervorgehobenen Optionen „Zugriffsrichtlinien“ und „Zugriffsrichtlinie hinzufügen“](media/concepts-data-access-and-security-data-encryption/show-access-policy-overview.png)

2. Wählen Sie **Schlüsselberechtigungen** und anschließend **Abrufen**, **Packen** und **Entpacken** aus. Wählen Sie dann den **Prinzipal** aus (der Name des MySQL-Servers). Wenn Ihr Serverprinzipal nicht in der Liste der vorhandenen Prinzipale enthalten ist, müssen Sie ihn registrieren. Wenn Sie erstmalig versuchen, die Datenverschlüsselung einzurichten, und der Vorgang mit einem Fehler abgebrochen wird, werden Sie aufgefordert, Ihren Serverprinzipal zu registrieren.

   ![Zugriffsrichtlinienübersicht](media/concepts-data-access-and-security-data-encryption/access-policy-wrap-unwrap.png)

3. Wählen Sie **Speichern** aus.

## <a name="set-data-encryption-for-azure-database-for-mysql"></a>Festlegen der Datenverschlüsselung für Azure Database for MySQL

1. Wählen Sie in Azure Database for MySQL die Option **Datenverschlüsselung** aus, um den vom Kunden verwalteten Schlüssel einzurichten.

   ![Screenshot von Azure Database for MySQL mit hervorgehobener Option „Datenverschlüsselung“](media/concepts-data-access-and-security-data-encryption/data-encryption-overview.png)

2. Sie können entweder einen Schlüsseltresor und ein Schlüsselpaar auswählen oder einen Schlüsselbezeichner eingeben.

   ![Screenshot von Azure Database for MySQL mit hervorgehobenen Optionen zur Datenverschlüsselung](media/concepts-data-access-and-security-data-encryption/setting-data-encryption.png)

3. Wählen Sie **Speichern** aus.

4. Starten Sie den Server neu, um sicherzustellen, dass alle Dateien (einschließlich temporäre Dateien) vollständig verschlüsselt werden.

## <a name="restore-or-create-a-replica-of-the-server"></a>Wiederherstellen oder Erstellen eines Serverreplikats

Nachdem Azure Database for MySQL mit einem vom Kunden verwalteten Schlüssel verschlüsselt wurde, der in Key Vault gespeichert ist, wird jede neu erstellte Kopie des Servers ebenfalls verschlüsselt. Sie können diese neue Kopie entweder durch einen lokalen Wiederherstellungsvorgang, einen Geowiederherstellungsvorgang oder durch einen Replikationsvorgang (lokal/regionsübergreifend) erstellen. Sie können für einen verschlüsselten MySQL-Server also die folgenden Schritte ausführen, um einen verschlüsselten wiederhergestellten Server zu erstellen.

1. Wählen Sie auf Ihrem Server **Übersicht** > **Wiederherstellung** aus.

   ![Screenshot von Azure Database for MySQL mit hervorgehobenen Optionen „Übersicht“ und „Wiederherstellung“](media/concepts-data-access-and-security-data-encryption/show-restore.png)

   Wählen Sie alternativ für einen replikationsfähigen Server unter **Einstellungen** die Option **Replikation** aus.

   ![Screenshot von Azure Database for MySQL mit hervorgehobener Option „Replikation“](media/concepts-data-access-and-security-data-encryption/mysql-replica.png)

2. Nachdem der Wiederherstellungsvorgang abgeschlossen wurde, ist der neu erstellte Server mit dem Schlüssel des primären Servers verschlüsselt. Allerdings sind die Features und Optionen auf dem Server deaktiviert, und auf den Server kann nicht zugegriffen werden. Da der Identität des neuen Servers noch keine Berechtigung für den Zugriff auf den Schlüsseltresor erteilt wurde, wird auf diese Weise jegliche Datenbearbeitung verhindert.

   ![Screenshot von Azure Database for MySQL mit hervorgehobenem Status „Kein Zugriff“](media/concepts-data-access-and-security-data-encryption/show-restore-data-encryption.png)

3. Um den Server zugänglich zu machen, überprüfen Sie den Schlüssel auf dem wiederhergestellten Server erneut. Wählen Sie dazu **Datenverschlüsselung** > **Schlüssel erneut überprüfen** aus.

   > [!NOTE]
   > Beim ersten Versuch, den Schlüssel erneut zu überprüfen, tritt ein Fehler auf, da dem Dienstprinzipal des neuen Servers Zugriff auf den Schlüsseltresor gewährt werden muss. Um den Dienstprinzipal zu generieren, wählen Sie **Schlüssel erneut überprüfen** aus. Dies führt zwar zu einem Fehler, doch der Dienstprinzipal wird generiert. Führen Sie anschließend [diese Schritte](#set-the-right-permissions-for-key-operations) weiter oben in diesem Artikel aus.

   ![Screenshot von Azure Database for MySQL mit hervorgehobenem Schritt zur erneuten Überprüfung des Schlüssels](media/concepts-data-access-and-security-data-encryption/show-revalidate-data-encryption.png)

   Sie müssen dem Schlüsseltresor Zugriff auf den neuen Server gewähren.

4. Nachdem Sie den Dienstprinzipal registriert haben, müssen Sie den Schlüssel erneut überprüfen, damit der Server wieder normal funktioniert.

   ![Screenshot von Azure Database for MySQL mit wiederhergestellter Funktionalität](media/concepts-data-access-and-security-data-encryption/restore-successful.png)


## <a name="using-an-azure-resource-manager-template-to-enable-data-encryption"></a>Verwenden einer Azure Resource Manager-Vorlage zum Aktivieren von Datenverschlüsselung

Sie können die Datenverschlüsselung nicht nur im Azure-Portal, sondern auch auf Ihrem Azure Database for MySQL-Einzelserver aktivieren, indem Sie Azure Resource Manager-Vorlagen für neue und vorhandene Server verwenden.

### <a name="for-a-new-server"></a>Neuer Server

Verwenden Sie eine der vorab erstellten Azure Resource Manager-Vorlagen, um den Server mit aktivierter Datenverschlüsselung bereitzustellen: [Beispiel mit Datenverschlüsselung](https://github.com/Azure/azure-mysql/tree/master/arm-templates/ExampleWithDataEncryption)

Diese Azure Resource Manager-Vorlage erstellt einen Azure Database for MySQL-Server und verwendet **KeyVault** und **Key**, die als Parameter übergegeben werden, um die Datenverschlüsselung auf dem Server zu aktivieren.

### <a name="for-an-existing-server"></a>Vorhandener Server
Sie können Azure Resource Manager-Vorlagen auch verwenden, um die Datenverschlüsselung auf vorhandenen Azure Database for MySQL-Servern zu aktivieren.

* Übergeben Sie den zuvor kopierten URI des Azure Key Vault-Schlüssels unter der Eigenschaft `keyVaultKeyUri` im Objekt „properties“.

* Verwenden Sie *2020-01-01-preview* als API-Version.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "location": {
      "type": "string"
    },
    "serverName": {
      "type": "string"
    },
    "keyVaultName": {
      "type": "string",
      "metadata": {
        "description": "Key vault name where the key to use is stored"
      }
    },
    "keyVaultResourceGroupName": {
      "type": "string",
      "metadata": {
        "description": "Key vault resource group name where it is stored"
      }
    },
    "keyName": {
      "type": "string",
      "metadata": {
        "description": "Key name in the key vault to use as encryption protector"
      }
    },
    "keyVersion": {
      "type": "string",
      "metadata": {
        "description": "Version of the key in the key vault to use as encryption protector"
      }
    }
  },
  "variables": {
    "serverKeyName": "[concat(parameters('keyVaultName'), '_', parameters('keyName'), '_', parameters('keyVersion'))]"
  },
  "resources": [
    {
      "type": "Microsoft.DBforMySQL/servers",
      "apiVersion": "2017-12-01",
      "kind": "",
      "location": "[parameters('location')]",
      "identity": {
        "type": "SystemAssigned"
      },
      "name": "[parameters('serverName')]",
      "properties": {
      }
    },
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2019-05-01",
      "name": "addAccessPolicy",
      "resourceGroup": "[parameters('keyVaultResourceGroupName')]",
      "dependsOn": [
        "[resourceId('Microsoft.DBforMySQL/servers', parameters('serverName'))]"
      ],
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "resources": [
            {
              "type": "Microsoft.KeyVault/vaults/accessPolicies",
              "name": "[concat(parameters('keyVaultName'), '/add')]",
              "apiVersion": "2018-02-14-preview",
              "properties": {
                "accessPolicies": [
                  {
                    "tenantId": "[subscription().tenantId]",
                    "objectId": "[reference(resourceId('Microsoft.DBforMySQL/servers/', parameters('serverName')), '2017-12-01', 'Full').identity.principalId]",
                    "permissions": {
                      "keys": [
                        "get",
                        "wrapKey",
                        "unwrapKey"
                      ]
                    }
                  }
                ]
              }
            }
          ]
        }
      }
    },
    {
      "name": "[concat(parameters('serverName'), '/', variables('serverKeyName'))]",
      "type": "Microsoft.DBforMySQL/servers/keys",
      "apiVersion": "2020-01-01-preview",
      "dependsOn": [
        "addAccessPolicy",
        "[resourceId('Microsoft.DBforMySQL/servers', parameters('serverName'))]"
      ],
      "properties": {
        "serverKeyType": "AzureKeyVault",
        "uri": "[concat(reference(resourceId(parameters('keyVaultResourceGroupName'), 'Microsoft.KeyVault/vaults/', parameters('keyVaultName')), '2018-02-14-preview', 'Full').properties.vaultUri, 'keys/', parameters('keyName'), '/', parameters('keyVersion'))]"
      }
    }
  ]
}

```

## <a name="next-steps"></a>Nächste Schritte

 Weitere Informationen zur Datenverschlüsselung finden Sie unter [Azure Database for MySQL-Datenverschlüsselung mit einem vom Kunden verwalteten Schlüssel](concepts-data-encryption-mysql.md).
