---
title: 'Verwaltetes HSM: Rollenverwaltung auf der Datenebene: Azure Key Vault | Microsoft-Dokumentation'
description: In diesem Artikel erfahren Sie, wie Sie Rollenzuweisungen für Ihr verwaltetes HSM verwalten.
services: key-vault
author: amitbapat
ms.service: key-vault
ms.subservice: managed-hsm
ms.topic: tutorial
ms.date: 09/15/2020
ms.author: ambapat
ms.openlocfilehash: 4d36b2c2178c7205246cd7c59aefedef3358e473
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104951741"
---
# <a name="managed-hsm-role-management"></a>Rollenverwaltung für verwaltetes HSM

> [!NOTE]
> Von Key Vault werden zwei Arten von Ressourcen unterstützt: Tresore und verwaltete HSMs. In diesem Artikel werden **verwaltete HSMs** behandelt. Informationen zum Verwalten eines Tresors finden Sie unter [Verwalten von Key Vault mit der Azure CLI](../general/manage-with-cli2.md).

Eine Übersicht über verwaltete HSMs finden Sie unter [Was ist verwaltetes HSM von Azure Key Vault? (Vorschauversion)](overview.md). Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

In diesem Artikel erfahren Sie, wie Sie Rollen für die Datenebene eines verwalteten HSM verwalten. Weitere Informationen zum Zugriffssteuerungsmodell für verwaltete HSMs finden Sie unter [Zugriffssteuerung für verwaltetes HSM](access-control.md).

Damit ein Sicherheitsprinzipal (beispielsweise ein Benutzer, ein Dienstprinzipal, eine Gruppe oder eine verwaltete Identität) Vorgänge auf der Datenebene eines verwalteten HSM ausführen kann, muss ihm eine Rolle zugewiesen werden, die die Ausführung dieser Vorgänge ermöglicht. Wenn Sie also beispielsweise einer Anwendung das Ausführen eines Signierungsvorgangs mit einem Schlüssel erlauben möchten, muss ihr eine Rolle zugewiesen werden, die „Microsoft.KeyVault/managedHSM/keys/sign/action“ als eine der Datenaktionen enthält. Eine Rolle kann in einem bestimmten Bereich zugewiesen werden. Die lokale RBAC für verwaltete HSMs unterstützt zwei Bereiche: HSM-weit (`/` oder `/keys`) und schlüsselspezifisch (`/keys/<keyname>`).

Eine Liste mit allen integrierten Rollen für verwaltete HSMs sowie mit den zulässigen Vorgängen finden Sie unter [Integrierte Rollen der lokalen RBAC für verwaltete HSMs](built-in-roles.md).

## <a name="prerequisites"></a>Voraussetzungen

Für die Verwendung der Azure CLI-Befehle in diesem Artikel benötigen Sie Folgendes:

* Ein Abonnement für Microsoft Azure. Falls Sie über kein Azure-Abonnement verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial) registrieren.
* Azure CLI, Version 2.21.0 oder höher. Führen Sie `az --version` aus, um die Version zu ermitteln. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI]( /cli/azure/install-azure-cli).
* Ein verwaltetes HSM in Ihrem Abonnement. Weitere Informationen finden Sie unter [Schnellstart: Bereitstellen und Aktivieren eines verwalteten HSM mithilfe der Azure-Befehlszeilenschnittstelle](quick-create-cli.md). Dort erfahren Sie, wie Sie ein verwaltetes HSM bereitstellen und aktivieren.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Geben Sie Folgendes ein, um sich über die Befehlszeilenschnittstelle bei Azure anzumelden:

```azurecli
az login
```

Weitere Informationen zu den Anmeldeoptionen für die Befehlszeilenschnittstelle finden Sie unter [Anmelden mit der Azure CLI](/cli/azure/authenticate-azure-cli).

## <a name="create-a-new-role-assignment"></a>Erstellen einer neuen Rollenzuweisung

### <a name="assign-roles-for-all-keys"></a>Zuweisen von Rollen für alle Schlüssel

Verwenden Sie den Befehl `az keyvault role assignment create`, um dem durch den Benutzerprinzipalnamen **user2\@contoso.com** identifizierten Benutzer eine Rolle vom Typ **Managed HSM Crypto Officer** (Kryptoverantwortlicher für verwaltete HSMs) für alle **Schlüssel** (Bereich `/keys`) in „ContosoHSM“ zuzuweisen.

```azurecli-interactive
az keyvault role assignment create --hsm-name ContosoMHSM --role "Managed HSM Crypto Officer" --assignee user2@contoso.com  --scope /keys
```

### <a name="assign-role-for-a-specific-key"></a>Zuweisen einer Rolle für einen bestimmten Schlüssel

Verwenden Sie den Befehl `az keyvault role assignment create`, um dem durch den Benutzerprinzipalnamen **user2\@contoso.com** identifizierten Benutzer eine Rolle vom Typ **Managed HSM Crypto Officer** (Kryptoverantwortlicher für verwaltete HSMs) für einen bestimmten Schlüssel namens **myrsakey** zuzuweisen.

```azurecli-interactive
az keyvault role assignment create --hsm-name ContosoMHSM --role "Managed HSM Crypto Officer" --assignee user2@contoso.com  --scope /keys/myrsakey
```

## <a name="list-existing-role-assignments"></a>Auflisten der vorhandenen Rollenzuweisungen

Verwenden Sie `az keyvault role assignment list`, um Rollenzuweisungen aufzulisten.

Alle Rollenzuweisungen im Bereich „/“ (Standard ohne Angabe von „--scope“) für alle Benutzer (Standard ohne Angabe von „--assignee“):

```azurecli-interactive
az keyvault role assignment list --hsm-name ContosoMHSM
```

Alle Rollenzuweisungen auf der HSM-Ebene für einen bestimmten Benutzer ( **user1@contoso.com** ):

```azurecli-interactive
az keyvault role assignment list --hsm-name ContosoMHSM --assignee user@contoso.com
```

> [!NOTE]
> Wird für Bereich „/“ (oder „/keys“) festgelegt, werden mit dem Auflistungsbefehl nur alle Rollenzuweisungen auf der obersten Ebene aufgeführt, aber keine Rollenzuweisungen auf der Ebene einzelner Schlüssel.

Alle Rollenzuweisungen für einen bestimmten Benutzer ( **user2@contoso.com** ) und einen bestimmten Schlüssel (**myrsakey**).

```azurecli-interactive
az keyvault role assignment list --hsm-name ContosoMHSM --assignee user2@contoso.com --scope /keys/myrsakey
```

Eine bestimmte Rollenzuweisung für die Rolle **Managed HSM Crypto Officer** (Kryptoverantwortlicher für verwaltete HSMs), einen bestimmten Benutzer ( **user2@contoso.com** ) und einen bestimmten Schlüssel (**myrsakey**):


```azurecli-interactive
az keyvault role assignment list --hsm-name ContosoMHSM --assignee user2@contoso.com --scope /keys/myrsakey --role "Managed HSM Crypto Officer"
```

## <a name="delete-a-role-assignment"></a>Löschen einer Rollenzuweisung

Verwenden Sie den Befehl `az keyvault role assignment delete`, um eine Rolle vom Typ **Managed HSM Crypto Officer** (Kryptoverantwortlicher für verwaltete HSMs) zu löschen, die dem Benutzer **user2\@contoso.com** für den Schlüssel **myrsakey2** zugewiesen ist.

```azurecli-interactive
az keyvault role assignment delete --hsm-name ContosoMHSM --role "Managed HSM Crypto Officer" --assignee user2@contoso.com  --scope /keys/myrsakey2
```

## <a name="list-all-available-role-definitions"></a>Auflisten aller verfügbaren Rollendefinitionen

Verwenden Sie den Befehl `az keyvault role definition list`, um alle Rollendefinitionen aufzulisten:

```azurecli-interactive
az keyvault role definition list --hsm-name ContosoMHSM
```

## <a name="create-a-new-role-definition"></a>Erstellen einer neuen Rollendefinition

Verwaltetes HSM verfügt über mehrere integrierte (vordefinierte) Rollen, die für die gängigsten Verwendungsszenarien nützlich sind. Sie können eine eigene Rolle mit einer Liste spezifischer Aktionen definieren, die die Rolle ausführen darf. Anschließend können Sie diese Rolle Prinzipalen zuweisen, um Ihnen die Berechtigungen für die angegebenen Aktionen zu erteilen. 

Führen Sie unter Verwendung einer JSON-Zeichenfolge den Befehl `az keyvault role definition create` für eine Rolle namens **My Custom Role** aus.
```azurecli-interactive
az keyvault role definition create --hsm-name ContosoMHSM --role-definition '{
    "roleName": "My Custom Role",
    "description": "The description of the custom rule.",
    "actions": [],
    "notActions": [],
    "dataActions": [
        "Microsoft.KeyVault/managedHsm/keys/read/action"
    ],
    "notDataActions": []
}'
```

Verwenden Sie den Befehl `az keyvault role definition create` für eine Rolle aus einer Datei mit dem Namen **my-custom-role-definition.json**, die die JSON-Zeichenfolge für eine Rollendefinition enthält. Siehe Beispiel oben.
```azurecli-interactive
az keyvault role definition create --hsm-name ContosoMHSM --role-definition @my-custom-role-definition.json
```

## <a name="show-details-of-a-role-definition"></a>Anzeigen der Details einer Rollendefinition

Verwenden Sie den Befehl `az keyvault role definition show`, um mithilfe des Namens (GUID) Details zu einer bestimmten Rollendefinition anzuzeigen.

```azurecli-interactive
az keyvault role definition show --hsm-name ContosoMHSM --name xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

## <a name="update-a-custom-role-definition"></a>Aktualisieren einer benutzerdefinierten Rollendefinition

Verwenden Sie den Befehl `az keyvault role definition update`, um mithilfe einer JSON-Zeichenfolge eine Rolle namens **My Custom Role** zu aktualisieren.
```azurecli-interactive
az keyvault role definition create --hsm-name ContosoMHSM --role-definition '{
            "roleName": "My Custom Role",
            "name": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "id": "Microsoft.KeyVault/providers/Microsoft.Authorization/roleDefinitions/xxxxxxxx-
        xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "description": "The description of the custom rule.",
            "actions": [],
            "notActions": [],
            "dataActions": [
                "Microsoft.KeyVault/managedHsm/keys/read/action",
                "Microsoft.KeyVault/managedHsm/keys/write/action",
                "Microsoft.KeyVault/managedHsm/keys/backup/action",
                "Microsoft.KeyVault/managedHsm/keys/create"
            ],
            "notDataActions": []
        }'
```

## <a name="delete-custom-role-definition"></a>Löschen von benutzerdefinierten Rollendefinition

Verwenden Sie den Befehl `az keyvault role definition delete`, um mithilfe des Namens (GUID) Details zu einer bestimmten Rollendefinition anzuzeigen. 
```azurecli-interactive
az keyvault role definition delete --hsm-name ContosoMHSM --name xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

> [!NOTE]
> Integrierte Rollen können nicht gelöscht werden. Wenn benutzerdefinierte Rollen gelöscht werden, werden alle Rollenzuweisungen, die diese benutzerdefinierte Rolle verwenden, außer Kraft gesetzt.


## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich eine Übersicht über die [rollenbasierte Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC)](../../role-based-access-control/overview.md) an.
- Sehen Sie sich ein Tutorial zur [Rollenverwaltung für verwaltetes HSM](role-management.md) an.
- Informieren Sie sich ausführlicher über das [Zugriffssteuerungsmodell für verwaltete HSMs](access-control.md).
- Sehen Sie sich alle [integrierten Rollen der lokalen RBAC für verwaltete HSMs](built-in-roles.md) an.
