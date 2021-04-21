---
title: Zuweisen einer Azure Key Vault-Zugriffsrichtlinie (Befehlszeilenschnittstelle)
description: Verwenden der Azure-Befehlszeilenschnittstelle zum Zuweisen einer Key Vault-Zugriffsrichtlinie zu einem Sicherheitsprinzipal oder einer Anwendungsidentität.
services: key-vault
author: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 08/27/2020
ms.author: mbaldwin
ms.openlocfilehash: 349d7453962a736c9f15bb7d31d5a44098f463a4
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107791947"
---
# <a name="assign-a-key-vault-access-policy"></a>Zuweisen einer Key Vault-Zugriffsrichtlinie

Eine Key Vault-Zugriffsrichtlinie legt fest, ob ein bestimmter Sicherheitsprinzipal (eine Anwendung oder eine Benutzergruppe) verschiedene Vorgänge für [Geheimnisse](../secrets/index.yml), [Schlüssel](../keys/index.yml) und [Zertifikate](../certificates/index.yml) in Key Vault ausführen kann. Sie können mithilfe des [Azure-Portals](assign-access-policy-portal.md), der Azure-Befehlszeilenschnittstelle (dieser Artikel) oder von [Azure PowerShell](assign-access-policy-powershell.md) Zugriffsrichtlinien zuweisen.

[!INCLUDE [key-vault-access-policy-limits.md](../../../includes/key-vault-access-policy-limits.md)]

Weitere Informationen zum Erstellen von Gruppen in Azure Active Directory mithilfe der Azure-Befehlszeilenschnittstelle finden Sie unter [az ad group create](/cli/azure/ad/group#az_ad_group_create) und [az ad group member add](/cli/azure/ad/group/member#az_ad_group_member_add).

## <a name="configure-the-azure-cli-and-sign-in"></a>Konfigurieren der Azure-Befehlszeilenschnittstelle und Anmelden

1. Wenn Sie Azure CLI-Befehle lokal ausführen möchten, installieren Sie die [Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli).
 
    Um Befehle direkt in der Cloud auszuführen, verwenden Sie [Azure Cloud Shell](../../cloud-shell/overview.md).

1. Nur lokale Befehlszeilenschnittstelle: Melden Sie sich mit `az login` bei Azure an:

    ```bash
    az login
    ```

    Der Befehl `az login` öffnet ggf. ein Browserfenster zum Erfassen von Anmeldeinformationen.

## <a name="acquire-the-object-id"></a>Abrufen der Objekt-ID

Ermitteln Sie die Objekt-ID der Anwendung, der Gruppe oder des Benutzers, der oder dem Sie die Zugriffsrichtlinie zuweisen möchten:

- Anwendungen und andere Dienstprinzipale: Verwenden Sie den Befehl [az ad sp list](/cli/azure/ad/sp#az_ad_sp_list), um Ihre Dienstprinzipale abzurufen. Untersuchen Sie die Ausgabe des Befehls, um die Objekt-ID des Sicherheitsprinzipals zu ermitteln, dem die Zugriffsrichtlinie zugewiesen werden soll.

    ```azurecli-interactive
    az ad sp list --show-mine
    ```

- Gruppen: Verwenden Sie den Befehl [az ad group list](/cli/azure/ad/group#az_ad_group_list), und filtern Sie dabei die Ergebnisse mit dem Parameter `--display-name`:

     ```azurecli-interactive
    az ad group list --display-name <search-string>
    ```

- Benutzer: Verwenden Sie den Befehl [az ad user show](/cli/azure/ad/user#az_ad_user_show), und übergeben Sie die E-Mail-Adresse des Benutzers im Parameter `--id`:

    ```azurecli-interactive
    az ad user show --id <email-address-of-user>
    ```

## <a name="assign-the-access-policy"></a>Zuweisen der Zugriffsrichtlinie
    
Verwenden Sie den Befehl [az keyvault set-policy](/cli/azure/keyvault#az_keyvault_set_policy), um die gewünschten Berechtigungen zuzuweisen:

```azurecli-interactive
az keyvault set-policy --name myKeyVault --object-id <object-id> --secret-permissions <secret-permissions> --key-permissions <key-permissions> --certificate-permissions <certificate-permissions>
```

Ersetzen Sie `<object-id>` durch die Objekt-ID des Sicherheitsprinzipals.

Sie müssen `--secret-permissions`, `--key-permissions` und `--certificate-permissions` nur einschließen, wenn Sie diesen spezifischen Typen Berechtigungen zuweisen. Die zulässigen Werte für `<secret-permissions>`, `<key-permissions>` und `<certificate-permissions>` sind in der Dokumentation zu [az keyvault set-policy](/cli/azure/keyvault#az_keyvault_set_policy) angegeben.

## <a name="next-steps"></a>Nächste Schritte

- [Azure Key Vault – Sicherheit: Identitäts- und Zugriffsverwaltung](security-overview.md#identity-management)
- [Sicherer Zugriff auf einen Schlüsseltresor](security-overview.md)
- [Entwicklerhandbuch zu Azure Key Vault](developers-guide.md)