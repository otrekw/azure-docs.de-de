---
title: Verschlüsselung ruhender Daten mit kundenseitig verwalteten Schlüsseln
description: Erfahren Sie mehr über die Verschlüsselung ruhender Daten Ihrer Azure Container Registry und wie Sie Ihre Registrierung mit einem kundenseitig verwalteten Schlüssel verschlüsseln, der in Azure Key Vault gespeichert ist.
ms.topic: article
ms.date: 03/10/2020
ms.custom: ''
ms.openlocfilehash: 2d5561998cf0b19698c8059a861a4014a171a7e7
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81461751"
---
# <a name="encryption-using-customer-managed-keys"></a>Verschlüsselung mithilfe kundenseitig verwalteter Schlüssel

Wenn Sie Images und andere Artefakte in einer Azure Container Registry speichern, verschlüsselt Azure automatisch den ruhenden Registrierungsinhalt mit [dienstseitig verwalteten Schlüsseln](../security/fundamentals/encryption-atrest.md#data-encryption-models). Sie können die Standardverschlüsselung durch eine zusätzliche Verschlüsselungsebene ergänzen, indem Sie einen Schlüssel verwenden, den Sie in Azure Key Vault erstellen und verwalten. Dieser Artikel führt Sie Schritt für Schritt durch die Verwendung der Azure CLI und des Azure-Portals.

Die serverseitige Verschlüsselung mit kundenseitig verwalteten Schlüsseln wird durch die Integration in [Azure Key Vault](../key-vault/general/overview.md) unterstützt. Sie können Ihre eigenen Verschlüsselungsschlüssel erstellen und in einem Schlüsseltresor speichern oder mit Azure Key Vault-APIs Verschlüsselungsschlüssel generieren. Mit Azure Key Vault können Sie auch die Schlüsselverwendung überwachen.

Diese Funktion ist auf der Dienstebene **Premium** der Containerregistrierung verfügbar. Weitere Informationen zu den Tarifen des Registrierungsdiensts und zu den Einschränkungen finden Sie unter [Azure Container Registry-SKUs](container-registry-skus.md).

> [!IMPORTANT]
> Diese Funktion befindet sich derzeit in der Vorschauphase. Es gelten einige [Einschränkungen](#preview-limitations). Vorschauversionen werden Ihnen zur Verfügung gestellt, wenn Sie die [zusätzlichen Nutzungsbedingungen][terms-of-use] akzeptieren. Einige Aspekte dieses Features werden bis zur allgemeinen Verfügbarkeit unter Umständen noch geändert.
>
   
## <a name="preview-limitations"></a>Einschränkungen der Vorschau 

* Sie können diese Funktion zurzeit nur aktivieren, wenn Sie eine Registrierung erstellen.
* Nachdem ein kundenseitig verwalteter Schlüssel in einer Registrierung aktiviert wurde, können Sie ihn nicht mehr deaktivieren.
* Die [Inhaltsvertrauensstellung](container-registry-content-trust.md) wird derzeit nicht in einer Registrierung unterstützt, die mit einem kundenseitig verwalteten Schlüssel verschlüsselt ist.
* Führen Sie in einer Registrierung, die mit einem kundenseitig verwalteten Schlüssel verschlüsselt ist, Protokolle für [ACR Tasks](container-registry-tasks-overview.md) aus, die derzeit nur für 24 Stunden aufbewahrt werden. Wenn Sie Protokolle für einen längeren Zeitraum aufbewahren müssen, finden Sie weitere Informationen in dieser Anleitung zum [Exportieren und Speichern von Task-Ausführungsprotokollen](container-registry-tasks-logs.md#alternative-log-storage).

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
keyID=$(az keyvault key show --name <keyname> --vault-name <key-vault-name> --query 'key.kid' --output tsv)
```

### <a name="create-a-registry-with-customer-managed-key"></a>Erstellen einer Registrierung mit kundenseitig verwaltetem Schlüssel

Führen Sie den Befehl [az acr create][az-acr-create] aus, um eine Registrierung zu erstellen und den kundenseitig verwalteten Schlüssel zu aktivieren. Übergeben Sie die Prinzipal-ID der verwalteten Identität und die Schlüssel-ID, die Sie zuvor in Umgebungsvariablen gespeichert haben:

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

## <a name="enable-customer-managed-key---portal"></a>Aktivieren kundenseitig verwalteter Schlüssel: Portal

### <a name="create-a-managed-identity"></a>Erstellen einer verwalteten Identität

Erstellen Sie eine benutzerseitig zugewiesene [verwaltete Identität für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md) im Azure-Portal. Eine schrittweise Anleitung hierfür finden Sie unter [Erstellen einer benutzerseitig zugewiesenen Identität](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity).

Notieren Sie sich den **Ressourcennamen** der verwalteten Identität. Sie benötigen diesen Namen in den späteren Schritten.

![Erstellen einer benutzerseitig zugewiesenen verwalteten Identität im Azure-Portal](./media/container-registry-customer-managed-keys/create-managed-identity.png)

### <a name="create-a-key-vault"></a>Erstellen eines Schlüsseltresors

Schritte zum Erstellen eines Schlüsseltresors finden Sie unter [Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe des Azure-Portals](../key-vault/secrets/quick-create-portal.md).

Wenn Sie einen Schlüsseltresor für einen kundenseitig verwalteten Schlüssel erstellen, müssen Sie auf der Registerkarte **Grundlagen** die folgenden Schutzeinstellungen aktivieren: **Vorläufiges Löschen** und **Löschschutz**. Diese Einstellungen helfen bei der Verhinderung von Datenverlusten aufgrund versehentlich gelöschter Schlüssel oder Schlüsseltresore.

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
1. Wählen Sie unter **Verschlüsselungsschlüssel** die Option **Aus Schlüsseltresor auswählen** aus.
1. Wählen Sie im Fenster **Schlüssel aus Azure Key Vault auswählen** den Schlüsseltresor, Schlüssel und die Version aus, die Sie im vorherigen Abschnitt erstellt haben.
1. Wählen Sie auf der Registerkarte **Verschlüsselung** die Option **Überprüfen + erstellen** aus.
1. Wählen Sie **Erstellen**, um die Registrierungsinstanz bereitzustellen.

![Erstellen einer Containerregistrierung im Azure-Portal](./media/container-registry-customer-managed-keys/create-encrypted-registry.png)

Navigieren Sie zu Ihrer Registrierung, um den Verschlüsselungsstatus Ihrer Registrierung im Portal anzuzeigen. Wählen Sie unter **Einstellungen** die Option **Verschlüsselung (Vorschau)** aus.

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

Um den Status der Registrierungsverschlüsselung anzuzeigen, führen Sie den Befehl [az acr encryption show-status] aus:

```azurecli
az acr encryption show-status --name <registry-name> 
```

## <a name="use-the-registry"></a>Verwenden der Registrierung

Nachdem Sie eine Registrierung für die Verschlüsselung von Daten mithilfe eines kundenseitig verwalteten Schlüssels aktiviert haben, können Sie dieselben Registrierungsvorgänge ausführen, die Sie auch in einer Registrierung ausführen, die nicht mit einem kundenseitig verwalteten Schlüssel verschlüsselt ist. Beispielsweise können Sie sich mit der Registrierung authentifizieren und Docker-Images per Push übertragen. Beispielbefehle finden Sie unter [Push- und Pull-Vorgänge für Images](container-registry-get-started-docker-cli.md).

## <a name="rotate-key"></a>Rotieren von Schlüsseln

Sie können einen vom Kunden verwalteten Schlüssel in Azure Key Vault entsprechend Ihren Konformitätsrichtlinien rotieren. Erstellen Sie einen neuen Schlüssel, und aktualisieren Sie dann die Registrierung, damit Daten mit dem neuen Schlüssel verschlüsselt werden. Sie können diese Schritte auch mit der Azure CLI oder im Portal ausführen.

Führen Sie beispielsweise den Befehl [az keyvault key create][az-keyvault-key-create] aus, um einen neuen Schlüssel zu erstellen:

```azurecli
az keyvault key create –-name <new-key-name> --vault-name <key-vault-name> 
```

Führen Sie dann den Befehl [az acr encryption rotate-key][az-acr-encryption-rotate-key] aus, und übergeben Sie dabei die neue Schlüssel-ID und die Prinzipal-ID der verwalteten Identität, die Sie zuvor konfiguriert haben:

```azurecli
az acr encryption rotatekey \
  --name <registry-name> \
  --key-encryption-key <new-key-id> \
  --identity $identityPrincipalID
```

## <a name="revoke-key"></a>Widerrufen eines Schlüssels

Widerrufen Sie den kundenseitig verwalteten Verschlüsselungsschlüssel, indem Sie die Zugriffsrichtlinie für den Schlüsseltresor ändern oder den Schlüssel löschen. Verwenden Sie z. B. den Befehl [az keyvault delete-policy][az-keyvault-delete-policy], um die Zugriffsrichtlinie für die verwaltete Identität zu ändern, die von Ihrer Registrierung verwendet wird. Beispiel:

```azurecli
az keyvault delete-policy \
  --resource-group <resource-group-name> \
  --name <key-vault-name> \
  --object-id $identityPrincipalID
```

Das Widerrufen des Schlüssels blockiert praktisch den Zugriff auf alle Registrierungsdaten, da die Registrierung nicht auf den Verschlüsselungsschlüssel zugreifen kann. Wenn der Zugriff auf den Schlüssel aktiviert oder der gelöschte Schlüssel wiederhergestellt wird, wählt Ihre Registrierung den Schlüssel aus, sodass Sie wieder auf die verschlüsselten Registrierungsdaten zugreifen können.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur [Verschlüsselung ruhender Daten in Azure](../security/fundamentals/encryption-atrest.md).
* Unter [Sicherer Zugriff auf einen Schlüsseltresor](../key-vault/general/secure-your-key-vault.md) erfahren Sie mehr über Zugriffsrichtlinien und das Sichern Ihres Schlüsseltresors.
* Wenn Sie Feedback zu kundenseitig verwalteten Schlüsseln für Azure Container Registry bereitstellen möchten, besuchen Sie die [ACR GitHub-Site](https://aka.ms/acr/issues).


<!-- LINKS - external -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms

<!-- LINKS - internal -->

[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-show]: /cli/azure/feature#az-feature-show
[az-group-create]: /cli/azure/group#az-group-create
[az-identity-create]: /cli/azure/identity#az-identity-create
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[az-keyvault-create]: /cli/azure/keyvault#az-keyvault-create
[az-keyvault-key-create]: /cli/azure/keyvault/key#az-keyvault-key-create
[az-keyvault-set-policy]: /cli/azure/keyvault#az-keyvault-set-policy
[az-keyvault-delete-policy]: /cli/azure/keyvault#az-keyvault-delete-policy
[az-resource-show]: /cli/azure/resource#az-resource-show
[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-acr-encryption-rotate-key]: /cli/azure/acr/encryption#az-acr-encryption-rotate-key
[az-acr-encryption-show]: /cli/azure/acr/encryption#az-acr-encryption-show
