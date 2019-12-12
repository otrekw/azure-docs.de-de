---
title: Hinzufügen oder Entfernen von Rollenzuweisungen mithilfe von Azure RBAC und der Azure-Befehlszeilenschnittstelle
description: Hier erfahren Sie, wie Sie den Zugriff auf Azure-Ressourcen für Benutzer, Gruppen, Dienstprinzipale und verwaltete Identitäten mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) und der Azure-Befehlszeilenschnittstelle gewähren.
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
ms.date: 11/25/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 0351721283df68fde910ae16b16d567954c3e6fb
ms.sourcegitcommit: c69c8c5c783db26c19e885f10b94d77ad625d8b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74707890"
---
# <a name="add-or-remove-role-assignments-using-azure-rbac-and-azure-cli"></a>Hinzufügen oder Entfernen von Rollenzuweisungen mithilfe von Azure RBAC und der Azure-Befehlszeilenschnittstelle

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control-definition-grant.md)] In diesem Artikel wird das Zuweisen von Rollen mithilfe der Azure-Befehlszeilenschnittstelle beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen über Folgendes verfügen, um Rollenzuweisungen hinzufügen oder entfernen zu können:

- `Microsoft.Authorization/roleAssignments/write`- und `Microsoft.Authorization/roleAssignments/delete`-Berechtigungen, wie z.B. [Benutzerzugriffsadministrator](built-in-roles.md#user-access-administrator) oder [Besitzer](built-in-roles.md#owner)
- [Bash in Azure Cloud Shell](/azure/cloud-shell/overview) oder der [Azure-Befehlszeilenschnittstelle](/cli/azure)

## <a name="get-object-ids"></a>Abrufen von Objekt-IDs

Sie müssen ggf. die eindeutige ID eines Objekts angeben, um Rollenzuweisungen hinzufügen oder entfernen zu können. Die ID weist dieses Format auf: `11111111-1111-1111-1111-111111111111`. Sie können die ID über das Azure-Portal oder die Azure-Befehlszeilenschnittstelle (Azure CLI) abrufen.

### <a name="user"></a>Benutzer

Zum Abrufen der Objekt-ID für einen Azure AD-Benutzer können Sie [az ad user show](/cli/azure/ad/user#az-ad-user-show) verwenden.

```azurecli
az ad user show --id "{email}" --query objectId --output tsv
```

### <a name="group"></a>Group

Zum Abrufen der Objekt-ID für eine Azure AD-Gruppe können Sie [az ad group show](/cli/azure/ad/group#az-ad-group-show) oder [az ad group list](/cli/azure/ad/group#az-ad-group-list) verwenden.

```azurecli
az ad group show --group "{name}" --query objectId --output tsv
```

### <a name="application"></a>Anwendung

Zum Abrufen der Objekt-ID für einen Azure AD-Dienstprinzipal (eine von einer Anwendung verwendete Identität) können Sie [az ad sp list](/cli/azure/ad/sp#az-ad-sp-list) verwenden. Verwenden Sie für einen Dienstprinzipal die Objekt-ID und **nicht** die Anwendungs-ID.

```azurecli
az ad sp list --display-name "{name}" --query [].objectId --output tsv
```

## <a name="add-a-role-assignment"></a>Hinzufügen einer Rollenzuweisung

In RBAC fügen Sie zum Gewähren des Zugriffs eine Rollenzuweisung hinzu.

### <a name="user-at-a-resource-group-scope"></a>Benutzer im Ressourcengruppenbereich

Verwenden Sie [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create), um für einen Benutzer im Ressourcengruppenbereich eine Rollenzuweisung hinzuzufügen.

```azurecli
az role assignment create --role <role_name_or_id> --assignee <assignee> --resource-group <resource_group>
```

Im folgenden Beispiel wird dem Benutzer *patlong\@contoso.com* im Ressourcengruppenkontext *pharma-sales* die Rolle *Mitwirkender für virtuelle Computer* zugewiesen:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee patlong@contoso.com --resource-group pharma-sales
```

### <a name="using-the-unique-role-id"></a>Verwenden der eindeutigen Rollen-ID

In bestimmten Fällen kann sich ein Rollenname ändern, z. B.:

- Sie verwenden eine eigene benutzerdefinierte Rolle und beschließen, den Namen zu ändern.
- Sie verwenden eine als Vorschauversion verfügbare Rolle, deren Name den Zusatz **(Vorschauversion)** enthält. Beim Veröffentlichen wird die Rolle umbenannt.

> [!IMPORTANT]
> Eine Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Auch wenn eine Rolle umbenannt wird, ändert sich die Rollen-ID nicht. Wenn Sie Ihre Rollenzuweisungen mithilfe von Skripts oder mittels Automatisierung erstellen, wird empfohlen, die eindeutige Rollen-ID anstelle des Rollennamens zu verwenden. Wird eine Rolle umbenannt, ist auf diese Weise die Wahrscheinlichkeit größer, dass Ihre Skripts funktionieren.

Verwenden Sie [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create), um mit der eindeutigen Rollen-ID eine Rollenzuweisung anstelle des Rollennamens hinzuzufügen.

```azurecli
az role assignment create --role <role_id> --assignee <assignee> --resource-group <resource_group>
```

Im folgenden Beispiel wird dem Benutzer *patlong\@contoso.com* im Ressourcengruppenkontext *pharma-sales* die Rolle [Mitwirkender für virtuelle Computer](built-in-roles.md#virtual-machine-contributor) zugewiesen. Zum Abrufen der eindeutigen Rollen-ID können Sie [az role definition list](/cli/azure/role/definition#az-role-definition-list) verwenden. Weitere Informationen finden Sie unter [Integrierte Rollen für Azure-Ressourcen](built-in-roles.md).

```azurecli
az role assignment create --role 9980e02c-c2be-4d73-94e8-173b1dc7cf3c --assignee patlong@contoso.com --resource-group pharma-sales
```

### <a name="group-at-a-subscription-scope"></a>Gruppe im Abonnementbereich

Sie verwenden [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create), um eine Rollenzuweisung für eine Gruppe hinzuzufügen. Informationen zum Abrufen der Objekt-ID für die Gruppe finden Sie unter [Abrufen von Objekt-IDs](#get-object-ids).

```azurecli
az role assignment create --role <role_name_or_id> --assignee-object-id <assignee_object_id> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

Im folgenden Beispiel wird der Gruppe *Ann Mack Team* mit der ID 22222222-2222-2222-2222-222222222222 im Abonnementbereich die Rolle *Reader* zugewiesen.

```azurecli
az role assignment create --role Reader --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/00000000-0000-0000-0000-000000000000
```

### <a name="group-at-a-resource-scope"></a>Gruppe im Ressourcenbereich

Sie verwenden [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create), um eine Rollenzuweisung für eine Gruppe hinzuzufügen. Informationen zum Abrufen der Objekt-ID für die Gruppe finden Sie unter [Abrufen von Objekt-IDs](#get-object-ids).

Im folgenden Beispiel wird die Rolle *Mitwirkender für virtuelle Computer* der Gruppe *Ann Mack Team* mit der ID 22222222-2222-2222-2222-222222222222 im Ressourcenbereich für ein virtuelles Netzwerk mit dem Namen *pharma-sales-project-network* zugewiesen.

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/pharma-sales/providers/Microsoft.Network/virtualNetworks/pharma-sales-project-network
```

### <a name="application-at-a-resource-group-scope"></a>Anwendung im Ressourcengruppenbereich

Verwenden Sie [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create), um eine Rollenzuweisung für eine Anwendung hinzuzufügen. Informationen zum Abrufen der Objekt-ID für die Anwendung finden Sie unter [Abrufen von Objekt-IDs](#get-object-ids).

```azurecli
az role assignment create --role <role_name_or_id> --assignee-object-id <assignee_object_id> --resource-group <resource_group>
```

Im folgenden Beispiel wird einer Anwendung mit der Objekt-ID 44444444-4444-4444-4444-444444444444 im Ressourcengruppenkontext *pharma-sales* die Rolle *Virtual Machine Contributor* zugewiesen.

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 44444444-4444-4444-4444-444444444444 --resource-group pharma-sales
```

### <a name="user-at-a-subscription-scope"></a>Benutzer im Abonnementbereich

Verwenden Sie [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create), um für einen Benutzer im Abonnementbereich eine Rollenzuweisung hinzuzufügen. Die Abonnement-ID können Sie über das Blatt **Abonnements** im Azure-Portal oder durch Verwenden von [az account list](/cli/azure/account#az-account-list) abrufen.

```azurecli
az role assignment create --role <role_name_or_id> --assignee <assignee> --subscription <subscription_name_or_id>
```

Im folgenden Beispiel wird dem Benutzer *annm\@example.com* im Abonnementbereich die Rolle *Reader* zugewiesen.

```azurecli
az role assignment create --role "Reader" --assignee annm@example.com --subscription 00000000-0000-0000-0000-000000000000
```

### <a name="user-at-a-management-group-scope"></a>Benutzer im Verwaltungsgruppenbereich

Verwenden Sie [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create), um für einen Benutzer im Verwaltungsgruppenbereich eine Rollenzuweisung hinzuzufügen. Die Verwaltungsgruppen-ID befindet sich auf dem Blatt **Verwaltungsgruppen** im Azure-Portal, oder Sie können zum Abrufen auch [az account management-group list](/cli/azure/ext/managementgroups/account/management-group#ext-managementgroups-az-account-management-group-list) verwenden.

```azurecli
az role assignment create --role <role_name_or_id> --assignee <assignee> --scope /providers/Microsoft.Management/managementGroups/<group_id>
```

Im folgenden Beispiel wird dem Benutzer *alain\@example.com* im Verwaltungsgruppenbereich die Rolle *Abrechnungsleser* zugewiesen.

```azurecli
az role assignment create --role "Billing Reader" --assignee alain@example.com --scope /providers/Microsoft.Management/managementGroups/marketing-group
```

### <a name="new-service-principal"></a>Neuer Dienstprinzipal

Wenn Sie einen neuen Dienstprinzipal erstellen und sofort versuchen, diesem eine Rolle zuzuweisen, kann die Rollenzuweisung in einigen Fällen fehlschlagen. Wenn Sie z.B. ein Skript verwenden, um eine neue verwaltete Identität zu erstellen, und dann versuchen, dem Dienstprinzipal eine Rolle zuzuweisen, kann die Rollenzuweisung fehlschlagen. Der Grund für diesen Fehler ist wahrscheinlich eine Replikationsverzögerung. Der Dienstprinzipal wird in einer Region erstellt, die Rollenzuweisung kann aber in einer anderen Region stattfinden, in die der Dienstprinzipal noch nicht repliziert wurde. Um dieses Szenario zu beheben, sollten Sie beim Erstellen der Rollenzuweisung den Prinzipaltyp angeben.

Verwenden Sie [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create) zum Hinzufügen einer Rollenzuweisung, geben Sie einen Wert für `--assignee-object-id` an, und legen Sie `--assignee-principal-type` dann auf `ServicePrincipal` fest.

```azurecli
az role assignment create --role <role_name_or_id> --assignee-object-id <assignee_object_id> --assignee-principal-type <assignee_principal_type> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

Im folgenden Beispiel wird der verwalteten Identität *msi-test* im Ressourcengruppenkontext *pharma-sales* die Rolle *Mitwirkender für virtuelle Computer* zugewiesen:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 33333333-3333-3333-3333-333333333333 --assignee-principal-type ServicePrincipal --resource-group pharma-sales
```

## <a name="remove-a-role-assignment"></a>Entfernen einer Rollenzuweisung

In RBAC entfernen Sie mit [az role assignment delete](/cli/azure/role/assignment#az-role-assignment-delete) eine Rollenzuweisung, um den Zugriff zu entfernen:

```azurecli
az role assignment delete --assignee <assignee> --role <role_name_or_id> --resource-group <resource_group>
```

Im folgenden Beispiel wird die Zuweisung der Rolle *Mitwirkender für virtuelle Computer* von Benutzer *patlong\@contoso.com* für die Ressourcengruppe *pharma-sales* entfernt:

```azurecli
az role assignment delete --assignee patlong@contoso.com --role "Virtual Machine Contributor" --resource-group pharma-sales
```

Im folgenden Beispiel wird die Rolle *Reader* von der Gruppe *Ann Mack Team* mit der ID 22222222-2222-2222-2222-222222222222 im Abonnementbereich entfernt. Informationen zum Abrufen der Objekt-ID für die Gruppe finden Sie unter [Abrufen von Objekt-IDs](#get-object-ids).

```azurecli
az role assignment delete --assignee 22222222-2222-2222-2222-222222222222 --role "Reader" --subscription 00000000-0000-0000-0000-000000000000
```

Im folgenden Beispiel wird dem Benutzer *alain\@example.com* im Verwaltungsgruppenbereich die Rolle *Abrechnungsleser* entfernt. Um die ID der Verwaltungsgruppe zu erhalten, können Sie [az account management-group list](/cli/azure/ext/managementgroups/account/management-group#ext-managementgroups-az-account-management-group-list) verwenden.

```azurecli
az role assignment delete --assignee alain@example.com --role "Billing Reader" --scope /providers/Microsoft.Management/managementGroups/marketing-group
```

## <a name="next-steps"></a>Nächste Schritte

- [Auflisten von Rollenzuweisungen mithilfe von Azure RBAC und der Azure-Befehlszeilenschnittstelle](role-assignments-list-cli.md)
- [Verwalten von Azure-Ressourcen und -Ressourcengruppen mithilfe der Azure-Befehlszeilenschnittstelle](../azure-resource-manager/cli-azure-resource-manager.md)
