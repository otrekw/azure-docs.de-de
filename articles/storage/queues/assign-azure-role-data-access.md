---
title: Zuweisen einer Azure-Rolle für den Zugriff auf Warteschlangendaten
titleSuffix: Azure Storage
description: Hier erfahren Sie, wie Sie Berechtigungen für Warteschlangendaten für einen Azure Active Directory-Sicherheitsprinzipal mit rollenbasierter Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC) zuweisen können. Azure Storage unterstützt integrierte und benutzerdefinierte Azure-Rollen für die Authentifizierung und Autorisierung über Azure AD.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 06/07/2021
ms.author: tamram
ms.reviewer: sohamnc
ms.subservice: common
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: c10386f6a81fabed61efd63aa4dffe61fe9e5cd7
ms.sourcegitcommit: e39ad7e8db27c97c8fb0d6afa322d4d135fd2066
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111982071"
---
# <a name="assign-an-azure-role-for-access-to-queue-data"></a>Zuweisen einer Azure-Rolle für den Zugriff auf Warteschlangendaten

Azure Active Directory (Azure AD) autorisiert Rechte für den Zugriff auf geschützte Ressourcen über die [rollenbasierte Zugriffssteuerung in Azure](../../role-based-access-control/overview.md) (Azure Role-Based Access Control, Azure RBAC). Azure Storage definiert eine Reihe von in Azure integrierten Rollen mit allgemeinen Berechtigungssätzen für den Zugriff auf Warteschlangen.

Wenn einem Azure AD-Sicherheitsprinzipal eine Azure-Rolle zugewiesen wird, gewährt Azure diesem Sicherheitsprinzipal Zugriff auf diese Ressourcen. Der Zugriff kann auf die Ebene des Abonnements, der Ressourcengruppe, des Speicherkontos oder einer einzelnen Warteschlange begrenzt werden. Eine Azure AD-Sicherheitsprinzipal kann ein Benutzer, eine Gruppe, ein Anwendungsdienstprinzipal oder eine [verwaltete Identität für Azure-Ressourcen](../../active-directory/managed-identities-azure-resources/overview.md) sein.

In diesem Artikel erfahren Sie, wie Sie Azure-Rollen für den Datenzugriff auf Warteschlangen zuweisen können.

## <a name="azure-roles-for-queues"></a>Azure-Rollen für Warteschlangen

[!INCLUDE [storage-auth-rbac-roles-queue-include](../../../includes/storage-auth-rbac-roles-queue-include.md)]

## <a name="determine-resource-scope"></a>Bestimmen des Ressourcenumfangs

[!INCLUDE [storage-auth-resource-scope-queue-include](../../../includes/storage-auth-resource-scope-queue-include.md)]

## <a name="assign-an-azure-role"></a>Zuweisen einer Azure-Rolle

Sie können über das Azure-Portal, PowerShell oder Azure CLI eine Rolle für den Datenzugriff zuweisen.

# <a name="azure-portal"></a>[Azure portal](#tab/portal)

Für den Zugriff auf Warteschlangendaten im Azure-Portal mit Azure AD Anmeldeinformationen muss ein Benutzer über die folgenden Rollenzuweisungen verfügen:

- Eine Datenzugriffsrolle, z. B. **Mitwirkender an Storage-Warteschlangendaten**
- Die Azure Resource Manager-Rolle **Leser**

Wenn Sie erfahren möchten, wie Sie diese Rollen einem Benutzer zuweisen können, führen Sie die Anleitungen unter [Zuweisen von Azure-Rollen über das Azure-Portal](../../role-based-access-control/role-assignments-portal.md) aus.

Die Rolle [Leser](../../role-based-access-control/built-in-roles.md#reader) ist eine Azure Resource Manager-Rolle, die es Benutzern ermöglicht, Ressourcen im Speicherkonto anzuzeigen, ohne sie ändern zu können. Sie bietet keine Leseberechtigungen für Daten in Azure Storage, sondern nur für Ressourcen zur Kontoverwaltung. Die Rolle **Leser** ist erforderlich, damit Benutzer im Azure-Portal zu Warteschlangen und Nachrichten navigieren können.

Wenn Sie beispielsweise dem Benutzer Mario auf der Ebene der Warteschlange **sample-queue** die Rolle **Mitwirkender an Storage-Warteschlangendaten** zuweisen, erhält Mario Lese-, Schreib- und Löschzugriff auf diese Warteschlange. Wenn Mario jedoch eine Warteschlange im Azure-Portal anzeigen möchte, bietet die Rolle **Mitwirkender an Storage-Warteschlangendaten** allein nicht genügend Berechtigungen, um über das Portal zur Warteschlange zu navigieren, um sie anzuzeigen. Die zusätzlichen Berechtigungen sind erforderlich, um durch das Portal zu navigieren und die anderen dort sichtbaren Ressourcen anzuzeigen.

Einem Benutzer muss die Rolle **Leser** zugewiesen werden, damit er das Azure-Portal mit Azure AD-Anmeldeinformationen nutzen kann. Wenn ihm aber eine Rolle mit den Berechtigungen **Microsoft.Storage/storageAccounts/listKeys/action** zugewiesen wurde, kann er das Portal mit den Speicherkontoschlüsseln über die Autorisierung mit gemeinsam verwendeten Schlüsseln nutzen. Um die Speicherkontoschlüssel verwenden zu können, muss der Zugriff mit gemeinsam verwendeten Schlüsseln für das Speicherkonto zulässig sein. Weitere Informationen zum Erlauben oder Verweigern des Zugriffs auf gemeinsam verwendete Schlüssel finden Sie unter [Verhindern der Autorisierung mit gemeinsam verwendeten Schlüsseln für ein Azure Storage-Konto](../common/shared-key-authorization-prevent.md).

Sie können auch eine Azure Resource Manager-Rolle zuweisen, die zusätzliche Berechtigungen über die Rolle **Leser** hinaus bietet. Das Zuweisen der geringstmöglichen Berechtigungen wird als bewährte Sicherheitsmethode empfohlen. Weitere Informationen finden Sie unter [Bewährte Methoden für Azure RBAC](../../role-based-access-control/best-practices.md).

> [!NOTE]
> Bevor Sie sich eine Rolle für den Datenzugriff zuweisen, können Sie bereits über das Azure-Portal auf Daten in Ihrem Speicherkonto zugreifen, da das Azure-Portal auch den Kontoschlüssel für den Datenzugriff nutzen kann. Weitere Informationen finden Sie unter [Auswählen der Autorisierung des Zugriffs auf Warteschlangendaten im Azure-Portal](../queues/authorize-data-operations-portal.md).
>
> Die Vorschauversion des Storage-Explorers im Azure-Portal unterstützt nicht die Verwendung von Azure AD-Anmeldeinformationen zum Anzeigen und Ändern von Warteschlangendaten. Storage-Explorer im Azure-Portal verwendet immer die Kontoschlüssel für den Zugriff auf Daten. Um Storage-Explorer im Azure-Portal verwenden zu können, muss Ihnen eine Rolle zugewiesen werden, in der **Microsoft.Storage/storageAccounts/listkeys/action** enthalten ist.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Wenn Sie eine Azure-Rolle einem Sicherheitsprinzipal zuweisen möchten, rufen Sie den Befehl [New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment) auf. Das Format des Befehls kann je nach Bereich der Zuweisung unterschiedlich sein. Zur Ausführung des Befehls müssen Sie über eine Rolle verfügen, die **Microsoft.Authorization/roleAssignments/write**-Berechtigungen enthält, die Ihnen im entsprechenden Bereich oder höher zugewiesen wurden.

Geben Sie zum Zuweisen einer auf eine Warteschlange begrenzten Rolle für den Parameter `--scope` eine Zeichenfolge an, die den Warteschlangenbereich enthält. Der Bereich für eine Warteschlange weist folgendes Format auf:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/<queue-name>
```

Im folgenden Beispiel wird die Rolle **Mitwirkender an Storage-Warteschlangendaten** einem Benutzer zugewiesen und auf eine Warteschlange mit dem Namen *sample-queue* begrenzt. Ersetzen Sie die Beispielwerte und die Platzhalterwerte in Klammern durch Ihre eigenen Werte: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Queue Data Contributor" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/sample-queue"
```

Informationen zum Zuweisen von Rollen mit PowerShell im Abonnement-, Ressourcengruppen- oder Speicherkontobereich finden Sie unter [Zuweisen von Azure-Rollen mithilfe von Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md).

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Verwenden Sie den Befehl [az role assignment create](/cli/azure/role/assignment#az_role_assignment_create), um einem Sicherheitsprinzipal eine Azure-Rolle zuzuweisen. Das Format des Befehls kann je nach Bereich der Zuweisung unterschiedlich sein. Das Format des Befehls kann je nach Bereich der Zuweisung unterschiedlich sein. Zur Ausführung des Befehls müssen Sie über eine Rolle verfügen, die **Microsoft.Authorization/roleAssignments/write**-Berechtigungen enthält, die Ihnen im entsprechenden Bereich oder höher zugewiesen wurden.

Geben Sie zum Zuweisen einer auf eine Warteschlange begrenzten Rolle für den Parameter `--scope` eine Zeichenfolge an, die den Warteschlangenbereich enthält. Der Bereich für eine Warteschlange weist folgendes Format auf:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/<queue-name>
```

Im folgenden Beispiel wird die Rolle **Mitwirkender an Storage-Warteschlangendaten** einem Benutzer zugewiesen und auf die Ebene der Warteschlange begrenzt. Ersetzen Sie die Beispielwerte und die Platzhalterwerte in Klammern durch Ihre eigenen Werte:

```azurecli-interactive
az role assignment create \
    --role "Storage Queue Data Contributor" \
    --assignee <email> \
    --scope "/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/<queue>"
```

Informationen zum Zuweisen von Rollen mit PowerShell im Abonnement-, Ressourcengruppen- oder Speicherkontobereich finden Sie unter [Zuweisen von Azure-Rollen mithilfe der Azure CLI](../../role-based-access-control/role-assignments-cli.md).

---

Beachten Sie die folgenden Punkte zu Azure-Rollenzuweisungen in Azure Storage:

- Wenn Sie ein Azure Storage-Konto erstellen, erhalten Sie nicht automatisch Berechtigungen für den Zugriff auf Daten über Azure AD. Sie müssen sich selbst explizit eine Azure-Rolle für Azure Storage zuweisen. Sie können sie auf Ebene Ihres Abonnements, einer Ressourcengruppe, eines Speicherkontos oder einer Warteschlange zuweisen.
- Wenn das Speicherkonto mit einem Schreibschutz von Azure Resource Manager gesperrt wurde, verhindert diese Sperre die Zuweisung von Azure-Rollen, die für das Speicherkonto oder eine Warteschlange gelten.

## <a name="next-steps"></a>Nächste Schritte

- [Was ist die rollenbasierte Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC)?](../../role-based-access-control/overview.md)
- [Bewährte Methoden für Azure RBAC](../../role-based-access-control/best-practices.md)