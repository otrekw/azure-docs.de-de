---
title: Verschlüsselung ruhender Daten mit einem kundenseitig verwalteten Schlüssel
description: Erfahren Sie mehr über die Verschlüsselung ruhender Daten Ihrer Azure Container Registry und wie Sie Ihre Premium-Registrierung mit einem kundenseitig verwalteten Schlüssel verschlüsseln, der in Azure Key Vault gespeichert ist.
ms.topic: article
ms.date: 08/26/2020
ms.custom: ''
ms.openlocfilehash: 0e1810c8e3da334570dd1c4d6adb500e2cfa95e3
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2020
ms.locfileid: "89487231"
---
# <a name="encrypt-registry-using-a-customer-managed-key"></a>Verschlüsseln der Registrierung mithilfe eines kundenseitig verwalteten Schlüssels

Wenn Sie Images und andere Artefakte in einer Azure Container Registry speichern, verschlüsselt Azure automatisch den ruhenden Registrierungsinhalt mit [dienstseitig verwalteten Schlüsseln](../security/fundamentals/encryption-models.md). Sie können die Standardverschlüsselung durch eine zusätzliche Verschlüsselungsebene ergänzen, indem Sie einen Schlüssel verwenden, den Sie in Azure Key Vault erstellen und verwalten. Dieser Artikel führt Sie Schritt für Schritt durch die Verwendung der Azure CLI und des Azure-Portals.

Die serverseitige Verschlüsselung mit kundenseitig verwalteten Schlüsseln wird durch die Integration in [Azure Key Vault](../key-vault/general/overview.md) unterstützt. Sie können Ihre eigenen Verschlüsselungsschlüssel erstellen und in einem Schlüsseltresor speichern oder mit Azure Key Vault-APIs Schlüssel generieren. Mit Azure Key Vault können Sie auch die Schlüsselverwendung überwachen.

Diese Funktion ist auf der Dienstebene **Premium** der Containerregistrierung verfügbar. Informationen zu den Tarifen des Registrierungsdiensts und zu den Einschränkungen finden Sie unter [Azure Container Registry-Tarife](container-registry-skus.md).


## <a name="things-to-know"></a>Wichtige Hinweise

* Sie können einen benutzerseitig verwalteten Schlüssel zurzeit nur aktivieren, wenn Sie eine Registrierung erstellen. Beim Aktivieren des Schlüssels konfigurieren Sie eine *benutzerseitig zugewiesene* verwaltete Identität für den Zugriff auf den Schlüsseltresor.
* Nachdem die Verschlüsselung mit einem kundenseitig verwalteten Schlüssel in einer Registrierung aktiviert wurde, können Sie die Verschlüsselung nicht mehr deaktivieren.  
* Die [Inhaltsvertrauensstellung](container-registry-content-trust.md) wird derzeit nicht in einer Registrierung unterstützt, die mit einem kundenseitig verwalteten Schlüssel verschlüsselt ist.
* Führen Sie in einer Registrierung, die mit einem kundenseitig verwalteten Schlüssel verschlüsselt ist, Protokolle für [ACR Tasks](container-registry-tasks-overview.md) aus, die derzeit nur für 24 Stunden aufbewahrt werden. Wenn Sie Protokolle für einen längeren Zeitraum aufbewahren müssen, finden Sie weitere Informationen in dieser Anleitung zum [Exportieren und Speichern von Task-Ausführungsprotokollen](container-registry-tasks-logs.md#alternative-log-storage).


> [!NOTE]
> Wenn der Zugriff auf Ihren Azure-Schlüsseltresor mithilfe eines virtuellen Netzwerks mit einer [Key Vault-Firewall](../key-vault/general/network-security.md) eingeschränkt ist, sind zusätzliche Konfigurationsschritte erforderlich. Nachdem Sie die Registrierung erstellt und den kundenseitig verwalteten Schlüssel aktiviert haben, richten Sie den Zugriff auf den Schlüssel mithilfe der *systemseitig zugewiesenen* verwalteten Identität der Registrierung ein, und konfigurieren Sie die Registrierung so, dass die Key Vault-Firewall umgangen wird. Führen Sie die Schritte in diesem Artikel zuerst aus, um die Verschlüsselung mit einem kundenseitig verwalteten Schlüssel zu aktivieren, und sehen Sie sich dann die Anleitung für [Erweitertes Szenario: Key Vault-Firewall](#advanced-scenario-key-vault-firewall) weiter unten in diesem Artikel an.

## <a name="prerequisites"></a>Voraussetzungen

Um die in diesem Artikel geschilderten Schritte für die Azure CLI durchzuführen, benötigen Sie Azure CLI, Version 2.2.0 oder höher. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

## <a name="enable-customer-managed-key---cli"></a>Aktivieren kundenseitig verwalteter Schlüssel: CLI

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Führen Sie bei Bedarf den Befehl [az group create][az-group-create] aus, um eine Ressourcengruppe zum Erstellen des Schlüsseltresors, der Containerregistrierung und anderer erforderlicher Ressourcen zu erstellen.

```azurecli
az group create --name <resource-group-name> --location <location>
```

### <a name="create-a-user-assigned-managed-identity"></a>Erstellen einer benutzerseitig zugewiesenen verwalteten Identität

Erstellen Sie eine benutzerseitig zugewiesene [verwaltete Identität für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md) mit dem Befehl [az identity create][az-identity-create]. Diese Identität wird von Ihrer Registrierung verwendet, um auf den Key Vault-Dienst zuzugreifen.

```azurecli
az identity create \
  --resource-group <resource-group-name> \
  --name <managed-identity-name>
```

Notieren Sie sich die folgenden Werte in der Befehlsausgabe: `id` und `principalId`. Sie benötigen diese Werte in späteren Schritten, um den Registrierungszugriff auf den Schlüsseltresor zu konfigurieren.

```JSON
{
  "clientId": "xxxx2bac-xxxx-xxxx-xxxx-192cxxxx6273",
  "clientSecretUrl": "https://control-eastus.identity.azure.net/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myidentityname/credentials?tid=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx&oid=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx&aid=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myresourcegroup",
  "location": "eastus",
  "name": "myidentityname",
  "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "resourceGroup": "myresourcegroup",
  "tags": {},
  "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "Microsoft.ManagedIdentity/userAssignedIdentities"
}
```

Speichern Sie diese Werte aus Gründen der Bequemlichkeit in Umgebungsvariablen:

```azurecli
identityID=$(az identity show --resource-group <resource-group-name> --name <managed-identity-name> --query 'id' --output tsv)

identityPrincipalID=$(az identity show --resource-group <resource-group-name> --name <managed-identity-name> --query 'principalId' --output tsv)
```

### <a name="create-a-key-vault"></a>Erstellen eines Schlüsseltresors

Erstellen Sie einen Schlüsseltresor mit [az keyvault create][az-keyvault-create], um einen kundenseitig verwalteten Schlüssel für die Registrierungsverschlüsselung zu speichern.

Um Datenverluste aufgrund versehentlich gelöschter Schlüssel oder Schlüsseltresore zu vermeiden, müssen Sie die folgenden Einstellungen aktivieren: **Vorläufiges Löschen** und **Löschschutz**. Das folgende Beispiel enthält Parameter für diese Einstellungen:

```azurecli
az keyvault create --name <key-vault-name> \
  --resource-group <resource-group-name> \
  --enable-soft-delete \
  --enable-purge-protection
```

### <a name="add-key-vault-access-policy"></a>Hinzufügen einer Schlüsseltresor-Zugriffsrichtlinie

Konfigurieren Sie eine Richtlinie für den Schlüsseltresor, damit die Identität darauf zugreifen kann. In dem folgenden Befehl [az keyvault set-policy][az-keyvault-set-policy] übergeben Sie die zuvor in einer Umgebungsvariablen gespeicherte Prinzipal-ID der verwalteten Identität, die Sie zuvor erstellt hatten. Legen Sie Schlüsselberechtigungen auf **get**, **unwrapkey** und **wrapkey** fest.  

```azurecli
az keyvault set-policy \
  --resource-group <resource-group-name> \
  --name <key-vault-name> \
  --object-id $identityPrincipalID \
  --key-permissions get unwrapKey wrapKey
```

### <a name="create-key-and-get-key-id"></a>Erstellen eines Schlüssels und Abrufen der Schlüssel-ID

Führen Sie den Befehl [az keyvault key create][az-keyvault-key-create] aus, um einen Schlüssel im Schlüsseltresor zu erstellen.

```azurecli
az keyvault key create \
  --name <key-name> \
  --vault-name <key-vault-name>
```

Notieren Sie sich die ID des Schlüssels (`kid`) in der Befehlsausgabe. Sie verwenden diese ID im nächsten Schritt:

```JSON
[...]
  "key": {
    "crv": null,
    "d": null,
    "dp": null,
    "dq": null,
    "e": "AQAB",
    "k": null,
    "keyOps": [
      "encrypt",
      "decrypt",
      "sign",
      "verify",
      "wrapKey",
      "unwrapKey"
    ],
    "kid": "https://mykeyvault.vault.azure.net/keys/mykey/xxxxxxxxxxxxxxxxxxxxxxxx",
    "kty": "RSA",
[...]
```
Speichern Sie diesen Wert aus Gründen der Bequemlichkeit in einer Umgebungsvariablen:

```azurecli
keyID=$(az keyvault key show \
  --name <keyname> \
  --vault-name <key-vault-name> \
  --query 'key.kid' --output tsv)
```

### <a name="create-a-registry-with-customer-managed-key"></a>Erstellen einer Registrierung mit kundenseitig verwaltetem Schlüssel

Führen Sie den Befehl [az acr create][az-acr-create] aus, um eine Registrierung im Premium-Tarif zu erstellen und den kundenseitig verwalteten Schlüssel zu aktivieren. Übergeben Sie die Prinzipal-ID der verwalteten Identität und die Schlüssel-ID, die Sie zuvor in Umgebungsvariablen gespeichert haben:

```azurecli
az acr create \
  --resource-group <resource-group-name> \
  --name <container-registry-name> \
  --identity $identityID \
  --key-encryption-key $keyID \
  --sku Premium
```

### <a name="show-encryption-status"></a>Anzeigen des Verschlüsselungsstatus

Um anzuzeigen, ob die Registrierungsverschlüsselung mit einem kundenseitig verwalteten Schlüssel aktiviert ist, führen Sie den Befehl [az acr encryption show][az-acr-encryption-show] aus:

```azurecli
az acr encryption show --name <registry-name>
```

Die Ausgabe Die Ausgabe lautet in etwa wie folgt:

```console
{
  "keyVaultProperties": {
    "identity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "keyIdentifier": "https://myvault.vault.azure.net/keys/myresourcegroup/abcdefg123456789...",
    "versionedKeyIdentifier": "https://myvault.vault.azure.net/keys/myresourcegroup/abcdefg123456789..."
  },
  "status": "enabled"
}
```

## <a name="enable-customer-managed-key---portal"></a>Aktivieren kundenseitig verwalteter Schlüssel: Portal

### <a name="create-a-managed-identity"></a>Erstellen einer verwalteten Identität

Erstellen Sie eine benutzerseitig zugewiesene [verwaltete Identität für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md) im Azure-Portal. Eine schrittweise Anleitung hierfür finden Sie unter [Erstellen einer benutzerseitig zugewiesenen Identität](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity).

Sie verwenden den Namen der Identität in späteren Schritten.

![Erstellen einer benutzerseitig zugewiesenen verwalteten Identität im Azure-Portal](./media/container-registry-customer-managed-keys/create-managed-identity.png)

### <a name="create-a-key-vault"></a>Erstellen eines Schlüsseltresors

Schritte zum Erstellen eines Schlüsseltresors finden Sie unter [Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe des Azure-Portals](../key-vault/secrets/quick-create-portal.md).

Wenn Sie einen Schlüsseltresor für einen kundenseitig verwalteten Schlüssel erstellen, aktivieren Sie auf der Registerkarte **Grundlagen** die folgenden Schutzeinstellungen: **Vorläufiges Löschen** und **Löschschutz**. Diese Einstellungen helfen bei der Verhinderung von Datenverlusten aufgrund versehentlich gelöschter Schlüssel oder Schlüsseltresore.

![Erstellen eines Schlüsseltresors im Azure-Portal](./media/container-registry-customer-managed-keys/create-key-vault.png)

### <a name="add-key-vault-access-policy"></a>Hinzufügen einer Schlüsseltresor-Zugriffsrichtlinie

Konfigurieren Sie eine Richtlinie für den Schlüsseltresor, damit die Identität darauf zugreifen kann.

1. Navigieren Sie zu Ihrem Schlüsseltresor.
1. Wählen Sie **Einstellungen** > **Zugriffsrichtlinien > +Zugriffsrichtlinie hinzufügen** aus.
1. Wählen Sie **Schlüsselberechtigungen** aus, und aktivieren Sie **Abrufen**, **Schlüssel entpacken** und **Schlüssel packen**.
1. Wählen Sie **Prinzipal auswählen** aus, und wählen Sie den Ressourcennamen Ihrer benutzerseitig zugewiesenen verwalteten Identität aus.  
1. Wählen Sie **Hinzufügen** und dann **Speichern** aus.

![Erstellen einer Schlüsseltresor-Zugriffsrichtlinie](./media/container-registry-customer-managed-keys/add-key-vault-access-policy.png)

### <a name="create-key"></a>Erstellen eines Schlüssels

1. Navigieren Sie zu Ihrem Schlüsseltresor.
1. Wählen Sie **Einstellungen** > **Schlüssel** aus.
1. Wählen Sie **+Generieren/Importieren** aus, und geben Sie einen eindeutigen Namen für den Schlüssel ein.
1. Akzeptieren Sie die verbleibenden Standardwerte, und wählen Sie **Erstellen** aus.
1. Wählen Sie nach der Erstellung den Schlüssel aus, und notieren Sie sich die aktuelle Schlüsselversion.

### <a name="create-azure-container-registry"></a>Erstellen einer Azure-Containerregistrierung

1. Klicken Sie auf **Ressource erstellen** > **Container** > **Container Registry**.
1. Wählen Sie auf der Registerkarte **Grundlagen** eine Ressourcengruppe aus, oder erstellen Sie eine, und geben Sie einen Registrierungsnamen ein. Wählen Sie unter **SKU** die Option **Premium** aus.
1. Wählen Sie auf der Registerkarte **Verschlüsselung** unter **Kundenseitig verwalteter Schlüssel** die Option **Aktiviert** aus.
1. Wählen Sie unter **Identität** die verwaltete Identität aus, die Sie erstellt haben.
1. Wählen Sie unter **Verschlüsselung** die Option **Aus Schlüsseltresor auswählen** aus.
1. Wählen Sie im Fenster **Schlüssel aus Azure Key Vault auswählen** den Schlüsseltresor, Schlüssel und die Version aus, die Sie im vorherigen Abschnitt erstellt haben.
1. Wählen Sie auf der Registerkarte **Verschlüsselung** die Option **Überprüfen + erstellen** aus.
1. Wählen Sie **Erstellen**, um die Registrierungsinstanz bereitzustellen.

![Erstellen einer Containerregistrierung im Azure-Portal](./media/container-registry-customer-managed-keys/create-encrypted-registry.png)

Navigieren Sie zu Ihrer Registrierung, um den Verschlüsselungsstatus Ihrer Registrierung im Portal anzuzeigen. Wählen Sie unter **Einstellungen** die Option **Verschlüsselung** aus.

## <a name="enable-customer-managed-key---template"></a>Aktivieren kundenseitig verwalteter Schlüssel: Vorlage

Sie können auch eine Resource Manager-Vorlage verwenden, um eine Registrierung zu erstellen und die Verschlüsselung mit einem kundenseitig verwalteten Schlüssel zu aktivieren.

Die folgende Vorlage erstellt eine neue Containerregistrierung und eine benutzerseitig zugewiesene verwaltete Identität. Kopieren Sie den folgenden Inhalt in eine neue Datei, und speichern Sie sie unter einem Dateinamen, z. B. `CMKtemplate.json`.

```JSON
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "vault_name": {
      "defaultValue": "",
      "type": "String"
    },
    "registry_name": {
      "defaultValue": "",
      "type": "String"
    },
    "identity_name": {
      "defaultValue": "",
      "type": "String"
    },
    "kek_id": {
      "type": "String"
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.ContainerRegistry/registries",
      "apiVersion": "2019-12-01-preview",
      "name": "[parameters('registry_name')]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Premium",
        "tier": "Premium"
      },
      "identity": {
        "type": "UserAssigned",
        "userAssignedIdentities": {
          "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities', parameters('identity_name'))]": {}
        }
      },
      "dependsOn": [
        "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', parameters('identity_name'))]"
      ],
      "properties": {
        "adminUserEnabled": false,
        "encryption": {
          "status": "enabled",
          "keyVaultProperties": {
            "identity": "[reference(resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', parameters('identity_name')), '2018-11-30').clientId]",
            "KeyIdentifier": "[parameters('kek_id')]"
          }
        },
        "networkRuleSet": {
          "defaultAction": "Allow",
          "virtualNetworkRules": [],
          "ipRules": []
        },
        "policies": {
          "quarantinePolicy": {
            "status": "disabled"
          },
          "trustPolicy": {
            "type": "Notary",
            "status": "disabled"
          },
          "retentionPolicy": {
            "days": 7,
            "status": "disabled"
          }
        }
      }
    },
    {
      "type": "Microsoft.KeyVault/vaults/accessPolicies",
      "apiVersion": "2018-02-14",
      "name": "[concat(parameters('vault_name'), '/add')]",
      "dependsOn": [
        "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', parameters('identity_name'))]"
      ],
      "properties": {
        "accessPolicies": [
          {
            "tenantId": "[subscription().tenantId]",
            "objectId": "[reference(resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', parameters('identity_name')), '2018-11-30').principalId]",
            "permissions": {
              "keys": [
                "get",
                "unwrapKey",
                "wrapKey"
              ]
            }
          }
        ]
      }
    },
    {
      "type": "Microsoft.ManagedIdentity/userAssignedIdentities",
      "apiVersion": "2018-11-30",
      "name": "[parameters('identity_name')]",
      "location": "[resourceGroup().location]"
    }
  ]
}

```

Führen Sie zum Erstellen der folgenden Ressourcen die Schritte aus den vorherigen Abschnitten durch:

* Schlüsseltresor, identifiziert anhand des Namens.
* Schlüsseltresorschlüssel, identifiziert anhand der Schlüssel-ID.

Führen Sie den folgenden Befehl [az group deployment create][az-group-deployment-create] aus, um die Registrierung mithilfe der vorangehenden Vorlagendatei zu erstellen. Geben Sie, wo angegeben, einen neuen Registrierungsnamen und den Namen der verwalteten Identität an sowie den Namen des Schlüsseltresors und die Schlüssel-ID, die Sie erstellt haben.

```bash
az group deployment create \
  --resource-group <resource-group-name> \
  --template-file CMKtemplate.json \
  --parameters \
    registry_name=<registry-name> \
    identity_name=<managed-identity> \
    vault_name=<key-vault-name> \
    kek_id=<key-vault-key-id>
```

### <a name="show-encryption-status"></a>Anzeigen des Verschlüsselungsstatus

Um den Status der Registrierungsverschlüsselung anzuzeigen, führen Sie den Befehl [az acr encryption show][az-acr-encryption-show] aus:

```azurecli
az acr encryption show --name <registry-name>
```

## <a name="use-the-registry"></a>Verwenden der Registrierung

Nachdem Sie einen kundenseitig verwalteten Schlüssel in einer Registrierung aktiviert haben, können Sie dieselben Registrierungsvorgänge ausführen, die Sie auch in einer Registrierung ausführen, die nicht mit einem kundenseitig verwalteten Schlüssel verschlüsselt ist. Beispielsweise können Sie sich mit der Registrierung authentifizieren und Docker-Images per Push übertragen. Beispielbefehle finden Sie unter [Push- und Pull-Vorgänge für Images](container-registry-get-started-docker-cli.md).

## <a name="rotate-key"></a>Rotieren von Schlüsseln

Rotieren Sie einen kundenseitig verwalteten Schlüssel, der für die Registrierungsverschlüsselung verwendet wird, gemäß Ihren Compliancerichtlinien. Erstellen Sie einen neuen Schlüssel, oder aktualisieren Sie eine Schlüsselversion, und aktualisieren Sie dann die Registrierung, damit Daten mit dem Schlüssel verschlüsselt werden. Sie können diese Schritte auch mit der Azure CLI oder im Portal ausführen.

Beim Rotieren eines Schlüssels geben Sie in der Regel dieselbe Identität an, die beim Erstellen der Registrierung verwendet wurde. Optional konfigurieren Sie eine neue benutzerseitig zugewiesene Identität für den Schlüsselzugriff, oder Sie aktivieren die systemseitig zugewiesene Identität der Registrierung und geben diese an.

> [!NOTE]
> Stellen Sie sicher, dass die erforderliche [Schlüsseltresorzugriffs-Richtlinie](#add-key-vault-access-policy) für die Identität festgelegt ist, die Sie für den Schlüsselzugriff konfigurieren.

### <a name="azure-cli"></a>Azure CLI

Verwenden Sie [az keyvault key][az-keyvault-key]-Befehle, um Ihre Schlüsseltresorschlüssel zu erstellen oder zu verwalten. Um beispielsweise eine neue Schlüsselversion oder einen neuen Schlüssel zu erstellen, führen Sie den Befehl [az keyvault key create][az-keyvault-key-create] aus:

```azurecli
# Create new version of existing key
az keyvault key create \
  –-name <key-name> \
  --vault-name <key-vault-name>

# Create new key
az keyvault key create \
  –-name <new-key-name> \
  --vault-name <key-vault-name>
```

Führen Sie dann den Befehl [az acr encryption rotate-key][az-acr-encryption-rotate-key] aus, und übergeben Sie dabei die neue Schlüssel-ID und die Identität, die Sie konfigurieren möchten:

```azurecli
# Rotate key and use user-assigned identity
az acr encryption rotate-key \
  --name <registry-name> \
  --key-encryption-key <new-key-id> \
  --identity <principal-id-user-assigned-identity>

# Rotate key and use system-assigned identity
az acr encryption rotate-key \
  --name <registry-name> \
  --key-encryption-key <new-key-id> \
  --identity [system]
```

### <a name="portal"></a>Portal

Verwenden Sie die **Verschlüsselungs**einstellungen der Registrierung, um die Einstellungen für die Schlüsselversion, den Schlüssel, den Schlüsseltresor oder die Identität zu aktualisieren, die für den kundenseitig verwalteten Schlüssel verwendet werden.

Um beispielsweise eine neue Schlüsselversion zu generieren und zu konfigurieren:

1. Navigieren Sie im Portal zu Ihrer Registrierung.
1. Wählen Sie unter **Einstellungen** die Option **Verschlüsselung** > **Schlüssel ändern** aus.
1. Wählen Sie **Schlüssel auswählen** aus.

    ![Rotieren des Schlüssels im Azure-Portal](./media/container-registry-customer-managed-keys/rotate-key.png)
1. Wählen Sie im Fenster **Schlüssel aus Azure Key Vault auswählen** den Schlüsseltresor und Schlüssel aus, die Sie zuvor konfiguriert haben, und wählen Sie in **Version** die Option **Neu erstellen** aus.
1. Wählen Sie im Fenster **Schlüssel erstellen** den Befehl **Generieren** und dann **Erstellen** aus.
1. Vervollständigen Sie die Schlüsselauswahl, und wählen Sie **Speichern** aus.

## <a name="revoke-key"></a>Widerrufen eines Schlüssels

Widerrufen Sie den kundenseitig verwalteten Verschlüsselungsschlüssel, indem Sie die Zugriffsrichtlinie für den Schlüsseltresor ändern oder den Schlüssel löschen. Verwenden Sie z. B. den Befehl [az keyvault delete-policy][az-keyvault-delete-policy], um die Zugriffsrichtlinie für die verwaltete Identität zu ändern, die von Ihrer Registrierung verwendet wird:

```azurecli
az keyvault delete-policy \
  --resource-group <resource-group-name> \
  --name <key-vault-name> \
  --object-id $identityPrincipalID
```

Das Widerrufen des Schlüssels blockiert praktisch den Zugriff auf alle Registrierungsdaten, da die Registrierung nicht auf den Verschlüsselungsschlüssel zugreifen kann. Wenn der Zugriff auf den Schlüssel aktiviert oder der gelöschte Schlüssel wiederhergestellt wird, wählt Ihre Registrierung den Schlüssel aus, sodass Sie wieder auf die verschlüsselten Registrierungsdaten zugreifen können.

## <a name="advanced-scenario-key-vault-firewall"></a>Erweitertes Szenario: Key Vault-Firewall

Wenn Ihr Azure-Schlüsseltresor in einem virtuellen Netzwerk mit einer Key Vault-Firewall bereitgestellt ist, führen Sie die folgenden zusätzlichen Schritte aus, nachdem Sie die Verschlüsselung mit kundenseitig verwaltetem Schlüssel in Ihrer Registrierung aktiviert haben.

1. Konfigurieren der Registrierungsverschlüsselung für die Verwendung der systemseitig zugewiesenen Identität
1. Aktivieren der Registrierung für die Umgehung der Key Vault-Firewall
1. Rotieren des kundenseitig verwalteten Schlüssels

### <a name="configure-system-assigned-identity"></a>Konfigurieren der systemseitig zugewiesenen Identität

Sie können die systemseitig zugewiesene verwaltete Identität einer Registrierung für den Zugriff auf den Schlüsseltresor für Verschlüsselungsschlüssel konfigurieren. Wenn Sie mit den verschiedenen verwalteten Identitäten für Azure-Ressourcen nicht vertraut sind, sehen Sie sich die [Übersicht](../active-directory/managed-identities-azure-resources/overview.md) an.

So aktivieren Sie die systemseitig zugewiesene Identität im Portal

1. Navigieren Sie im Portal zu Ihrer Registrierung.
1. Wählen Sie **Einstellungen** >  **Identität** aus.
1. Legen Sie unter **Systemseitig zugewiesen** **Status** auf **Ein** fest. Wählen Sie **Speichern** aus.
1. Kopieren Sie die **Objekt-ID** der Identität.

So gewähren Sie der Identität Zugriff auf Ihren Schlüsseltresor

1. Navigieren Sie zu Ihrem Schlüsseltresor.
1. Wählen Sie **Einstellungen** > **Zugriffsrichtlinien > +Zugriffsrichtlinie hinzufügen** aus.
1. Wählen Sie **Schlüsselberechtigungen** aus, und aktivieren Sie **Abrufen**, **Schlüssel entpacken** und **Schlüssel packen**.
1. Wählen Sie **Prinzipal auswählen** aus, und suchen Sie nach der Objekt-ID Ihrer systemseitig zugewiesenen verwalteten Identität oder dem Namen Ihrer Registrierung.  
1. Wählen Sie **Hinzufügen** und dann **Speichern** aus.

So aktualisieren Sie die Verschlüsselungseinstellungen der Registrierung für die Verwendung der Identität

1. Navigieren Sie im Portal zu Ihrer Registrierung.
1. Wählen Sie unter **Einstellungen** die Option **Verschlüsselung** > **Schlüssel ändern** aus.
1. Wählen Sie in **Identität** zuerst **Systemseitig zugewiesen** und dann **Speichern** aus.

### <a name="enable-key-vault-bypass"></a>Aktivieren der Key Vault-Umgehung

Um auf einen mit einer Key Vault-Firewall konfigurierten Schlüsseltresor zuzugreifen, muss die Registrierung die Firewall umgehen. Konfigurieren Sie den Schlüsseltresor so, dass der Zugriff durch jeden [vertrauenswürdigen Dienst](../key-vault/general/overview-vnet-service-endpoints.md#trusted-services) zugelassen wird. Azure Container Registry ist einer der vertrauenswürdigen Dienste.

1. Navigieren Sie im Portal zu Ihrem Schlüsseltresor.
1. Wählen Sie **Einstellungen** > **Netzwerk** aus.
1. Bestätigen, aktualisieren oder fügen Sie Einstellungen des virtuellen Netzwerks hinzu. Detaillierte Schritte finden Sie unter [Konfigurieren von Azure Key Vault-Firewalls und virtuellen Netzwerken](../key-vault/general/network-security.md).
1. Wählen Sie in **Vertrauenswürdigen Microsoft-Diensten die Umgehung dieser Firewall erlauben** die Option **Ja** aus. 

### <a name="rotate-the-customer-managed-key"></a>Rotieren des kundenseitig verwalteten Schlüssels

Nachdem Sie die vorangehenden Schritte ausgeführt haben, rotieren Sie den Schlüssel in einen neuen Schlüssel im Schlüsseltresor hinter einer Firewall. Weitere Informationen finden Sie unter [Rotieren von Schlüsseln](#rotate-key) in diesem Artikel.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur [Verschlüsselung ruhender Daten in Azure](../security/fundamentals/encryption-atrest.md).
* Unter [Sicherer Zugriff auf einen Schlüsseltresor](../key-vault/general/secure-your-key-vault.md) erfahren Sie mehr über Zugriffsrichtlinien und das Sichern Ihres Schlüsseltresors.


<!-- LINKS - external -->

<!-- LINKS - internal -->

[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-show]: /cli/azure/feature#az-feature-show
[az-group-create]: /cli/azure/group#az-group-create
[az-identity-create]: /cli/azure/identity#az-identity-create
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[az-keyvault-create]: /cli/azure/keyvault#az-keyvault-create
[az-keyvault-key-create]: /cli/azure/keyvault/key#az-keyvault-key-create
[az-keyvault-key]: /cli/azure/keyvault/key
[az-keyvault-set-policy]: /cli/azure/keyvault#az-keyvault-set-policy
[az-keyvault-delete-policy]: /cli/azure/keyvault#az-keyvault-delete-policy
[az-resource-show]: /cli/azure/resource#az-resource-show
[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-acr-encryption-rotate-key]: /cli/azure/acr/encryption#az-acr-encryption-rotate-key
[az-acr-encryption-show]: /cli/azure/acr/encryption#az-acr-encryption-show
