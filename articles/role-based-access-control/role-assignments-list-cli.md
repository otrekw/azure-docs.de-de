---
title: Auflisten von Rollenzuweisungen mithilfe von Azure RBAC und der Azure-Befehlszeilenschnittstelle
description: Erfahren Sie, wie Sie mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) und der Azure-Befehlszeilenschnittstelle ermitteln können, auf welche Ressourcen Benutzer, Gruppen, Dienstprinzipale und verwaltete Identitäten Zugriff haben.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/10/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 5716e7bb89d017866bd1575256e2d119bb7acbe5
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2020
ms.locfileid: "80385060"
---
# <a name="list-role-assignments-using-azure-rbac-and-azure-cli"></a>Auflisten von Rollenzuweisungen mithilfe von Azure RBAC und der Azure-Befehlszeilenschnittstelle

[!INCLUDE [Azure RBAC definition list access](../../includes/role-based-access-control-definition-list.md)] In diesem Artikel wird beschrieben, wie Sie mit der Azure-Befehlszeilenschnittstelle Rollenzuweisungen auflisten.

> [!NOTE]
> Wenn Ihre Organisation Verwaltungsfunktionen an einen Dienstanbieter ausgelagert hat, der die [delegierte Azure-Ressourcenverwaltung](../lighthouse/concepts/azure-delegated-resource-management.md) verwendet, werden die von diesem Dienstanbieter autorisierten Rollenzuweisungen hier nicht angezeigt.

## <a name="prerequisites"></a>Voraussetzungen

- [Bash in der Azure Cloud Shell](/azure/cloud-shell/overview) oder [Azure-Befehlszeilenschnittstelle](/cli/azure)

## <a name="list-role-assignments-for-a-user"></a>Liste von Rollenzuweisungen für einen Benutzer

Zum Auflisten der Rollenzuweisungen für einen bestimmten Benutzer verwenden Sie [az role assignment list](/cli/azure/role/assignment#az-role-assignment-list):

```azurecli-interactive
az role assignment list --assignee <assignee>
```

Standardmäßig werden nur Rollenzuweisungen für das aktuelle Abonnement angezeigt. Um Rollenzuweisungen für das aktuelle Abonnement und darin befindliche Abonnements anzuzeigen, fügen Sie den Parameter `--all` hinzu. Um geerbte Rollenzuweisungen anzuzeigen, fügen Sie den Parameter `--include-inherited` hinzu.

Im folgenden Beispiel werden die Rollenzuweisungen aufgelistet, die dem Benutzer *\@contoso.com* direkt zugewiesen sind:

```azurecli-interactive
az role assignment list --all --assignee patlong@contoso.com --output json | jq '.[] | {"principalName":.principalName, "roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

```
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Backup Operator",
  "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"
}
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Virtual Machine Contributor",
  "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"
}
```

## <a name="list-role-assignments-for-a-resource-group"></a>Auflisten von Rollenzuweisungen für eine Ressourcengruppe

Zum Auflisten der Rollenzuweisungen, die für einen Ressourcengruppenbereich vorhanden sind, verwenden Sie [az role assignment list](/cli/azure/role/assignment#az-role-assignment-list):

```azurecli-interactive
az role assignment list --resource-group <resource_group>
```

Im folgenden Beispiel werden die Rollenzuweisungen für die Ressourcengruppe *pharma-sales* aufgelistet:

```azurecli-interactive
az role assignment list --resource-group pharma-sales --output json | jq '.[] | {"principalName":.principalName, "roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

```
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Backup Operator",
  "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"
}
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Virtual Machine Contributor",
  "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"
}

...
```

## <a name="list-role-assignments-for-a-subscription"></a>Auflisten der Rollenzuweisungen für ein Abonnement

Um alle Rollenzuweisungen in einem Abonnementbereich aufzulisten, verwenden Sie [az role assignment list](/cli/azure/role/assignment#az-role-assignment-list). Die Abonnement-ID können Sie über das Blatt **Abonnements** im Azure-Portal oder durch Verwenden von [az account list](/cli/azure/account#az-account-list) abrufen.

```azurecli-interactive
az role assignment list --subscription <subscription_name_or_id>
```

Beispiel:

```azurecli-interactive
az role assignment list --subscription 00000000-0000-0000-0000-000000000000 --output json | jq '.[] | {"principalName":.principalName, "roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

## <a name="list-role-assignments-for-a-management-group"></a>Auflisten von Rollenzuweisungen für eine Verwaltungsgruppe

Um alle Rollenzuweisungen in einem Verwaltungsgruppenbereich aufzulisten, verwenden Sie [az role assignment list](/cli/azure/role/assignment#az-role-assignment-list). Die Verwaltungsgruppen-ID befindet sich auf dem Blatt **Verwaltungsgruppen** im Azure-Portal, oder Sie können zum Abrufen auch [az account management-group list](/cli/azure/account/management-group#az-account-management-group-list) verwenden.

```azurecli-interactive
az role assignment list --scope /providers/Microsoft.Management/managementGroups/<group_id>
```

Beispiel:

```azurecli-interactive
az role assignment list --scope /providers/Microsoft.Management/managementGroups/marketing-group --output json | jq '.[] | {"principalName":.principalName, "roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

## <a name="list-role-assignments-for-a-managed-identity"></a>Auflisten von Rollenzuweisungen für eine verwaltete Identität

1. Rufen Sie die Objekt-ID der systemseitig oder benutzerseitig zugewiesenen verwalteten Identität ab.

    Um die Objekt-ID einer benutzerseitig zugewiesenen verwalteten Identität abzurufen, können Sie [az ad sp list](/cli/azure/ad/sp#az-ad-sp-list) oder [az identity list](/cli/azure/identity#az-identity-list) verwenden.

    ```azurecli-interactive
    az ad sp list --display-name "<name>" --query [].objectId --output tsv
    ```

    Um die Objekt-ID einer systemseitig zugewiesenen verwalteten Identität abzurufen, können Sie [az ad sp list](/cli/azure/ad/sp#az-ad-sp-list) verwenden.

    ```azurecli-interactive
    az ad sp list --display-name "<vmname>" --query [].objectId --output tsv
    ```

1. Zum Auflisten der Rollenzuweisungen verwenden Sie [az role assignment list](/cli/azure/role/assignment#az-role-assignment-list).

    Standardmäßig werden nur Rollenzuweisungen für das aktuelle Abonnement angezeigt. Um Rollenzuweisungen für das aktuelle Abonnement und darin befindliche Abonnements anzuzeigen, fügen Sie den Parameter `--all` hinzu. Um geerbte Rollenzuweisungen anzuzeigen, fügen Sie den Parameter `--include-inherited` hinzu.

    ```azurecli-interactive
    az role assignment list --assignee <objectid>
    ```

## <a name="next-steps"></a>Nächste Schritte

- [Hinzufügen oder Entfernen von Rollenzuweisungen mithilfe von Azure RBAC und der Azure-Befehlszeilenschnittstelle](role-assignments-cli.md)
