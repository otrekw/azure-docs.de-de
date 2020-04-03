---
title: Benutzerdefinierte Rollen für Azure-Ressourcen | Microsoft-Dokumentation
description: Informationen zum Erstellen benutzerdefinierter Rollen mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) für die präzise Verwaltung des Zugriffs auf Azure-Ressourcen
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: e4206ea9-52c3-47ee-af29-f6eef7566fa5
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/19/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9454962e210781559f2fdceb1c36f499c4ae8ff7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80062166"
---
# <a name="custom-roles-for-azure-resources"></a>Benutzerdefinierte Rollen für Azure-Ressourcen

> [!IMPORTANT]
> Das Hinzufügen einer Verwaltungsgruppe zu `AssignableScopes` befindet sich derzeit in der Vorschauphase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Wenn die [integrierten Rollen für Azure-Ressourcen](built-in-roles.md) den Ansprüchen Ihrer Organisation nicht entsprechen, können Sie Ihre eigenen benutzerdefinierten Rollen erstellen. Genauso wie integrierte Rollen können auch benutzerdefinierte Rollen Benutzern, Gruppen und Dienstprinzipalen im Verwaltungsgruppen-, Abonnement- und Ressourcengruppenbereich zugewiesen werden.

Benutzerdefinierte Rollen können von Abonnements, die demselben Azure AD-Verzeichnis vertrauen, gemeinsam genutzt werden. Es gilt ein Limit von **5.000** benutzerdefinierte Rollen pro Verzeichnis. (Für Azure Deutschland und Azure China 21ViaNet beträgt das Limit 2.000 benutzerdefinierte Rollen.) Benutzerdefinierte Rollen können über das Azure-Portal (Vorschau), mit Azure PowerShell, über die Azure CLI oder mithilfe der REST-API erstellt werden.

## <a name="custom-role-example"></a>Beispiel einer benutzerdefinierten Rolle

Das folgende Beispiel zeigt, wie eine benutzerdefinierte Rolle im JSON-Format angezeigt aussieht. Diese benutzerdefinierte Rolle kann zum Überwachen und Neustarten virtueller Computer verwendet werden.

```json
{
  "Name": "Virtual Machine Operator",
  "Id": "88888888-8888-8888-8888-888888888888",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.ResourceHealth/availabilityStatuses/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Insights/diagnosticSettings/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": [
    "/subscriptions/{subscriptionId1}",
    "/subscriptions/{subscriptionId2}",
    "/providers/Microsoft.Management/managementGroups/{groupId1}"
  ]
}
```

Wenn Sie eine benutzerdefinierte Rolle erstellt haben, wird sie im Azure-Portal mit einem orangefarbenen Ressourcensymbol angezeigt.

![Symbol der benutzerdefinierten Rolle](./media/custom-roles/roles-custom-role-icon.png)

## <a name="steps-to-create-a-custom-role"></a>Schritte zum Erstellen einer benutzerdefinierten Rolle

1. Entscheiden Sie, wie Sie die benutzerdefinierte Rolle erstellen möchten.

    Benutzerdefinierte Rollen können im [Azure-Portal](custom-roles-portal.md) (Vorschau), mit [Azure PowerShell](custom-roles-powershell.md), [Azure CLI](custom-roles-cli.md) oder der [REST-API](custom-roles-rest.md) erstellt werden.

1. Bestimmen der Berechtigungen, die Sie benötigen

    Wenn Sie eine benutzerdefinierte Rolle erstellen, müssen Sie die Ressourcenanbietervorgänge kennen, die zum Definieren der Berechtigungen verfügbar sind. Wie Sie die Liste der Vorgänge anzeigen, erfahren Sie unter [Vorgänge für Azure Resource Manager-Ressourcenanbieter](resource-provider-operations.md). Sie fügen die Vorgänge der `Actions`- oder `NotActions`-Eigenschaft der [Rollendefinition](role-definitions.md) hinzu. Wenn Sie über Datenvorgänge verfügen, fügen Sie diese der `DataActions`- oder `NotDataActions`-Eigenschaft hinzu.

1. Erstellen der benutzerdefinierten Rolle

    In der Regel beginnen Sie mit einer vorhandenen integrierten Rolle und ändern sie Ihren Anforderungen entsprechend. Dann erstellen Sie die benutzerdefinierte Rolle mit dem Befehl [New-AzRoleDefinition](/powershell/module/az.resources/new-azroledefinition) oder [az role definition create](/cli/azure/role/definition#az-role-definition-create). Um eine benutzerdefinierte Rolle zu erstellen, benötigen Sie die `Microsoft.Authorization/roleDefinitions/write`-Berechtigung für alle `AssignableScopes`, wie z.B. [Besitzer](built-in-roles.md#owner) oder [Benutzerzugriffsadministrator](built-in-roles.md#user-access-administrator).

1. Testen der benutzerdefinierten Rolle

    Sobald Sie über Ihre benutzerdefinierte Rolle verfügen, müssen Sie sie testen, um sicherzustellen, dass sie wie erwartet funktioniert. Wenn Sie später Anpassungen vornehmen müssen, können Sie die benutzerdefinierte Rolle aktualisieren.

Ein ausführliches Tutorial zum Erstellen einer benutzerdefinierten Rolle finden Sie unter [Tutorial: Erstellen einer benutzerdefinierten Rolle mithilfe von Azure PowerShell](tutorial-custom-role-powershell.md) oder [Tutorial: Erstellen einer benutzerdefinierten Rolle mithilfe der Azure CLI](tutorial-custom-role-cli.md).

## <a name="custom-role-properties"></a>Eigenschaften der benutzerdefinierten Rolle

Eine benutzerdefinierte Rolle hat die folgenden Eigenschaften.

| Eigenschaft | Erforderlich | type | BESCHREIBUNG |
| --- | --- | --- | --- |
| `Name` | Ja | String | Der Anzeigename der benutzerdefinierten Rolle. Obwohl es sich bei einer Rollendefinition um eine Ressource auf Verwaltungsgruppen- oder Abonnementebene handelt, kann eine solche Definition in mehreren Abonnements verwendet werden, die dasselbe Azure AD-Verzeichnis gemeinsam nutzen. Der Anzeigename muss im Bereich des Azure AD-Verzeichnisses eindeutig sein. Er kann Buchstaben, Ziffern, Leerzeichen und Sonderzeichen enthalten. Die maximale Anzahl von Zeichen ist 128. |
| `Id` | Ja | String | Die eindeutige ID der benutzerdefinierten Rolle. Für Azure PowerShell und Azure CLI wird diese ID automatisch generiert, wenn Sie eine neue Rolle erstellen. |
| `IsCustom` | Ja | String | Gibt an, ob dies eine benutzerdefinierte Rolle ist. Legen Sie für benutzerdefinierte Rollen `true` fest. |
| `Description` | Ja | String | Die Beschreibung der benutzerdefinierten Rolle. Er kann Buchstaben, Ziffern, Leerzeichen und Sonderzeichen enthalten. Die maximale Anzahl von Zeichen ist 1.024. |
| `Actions` | Ja | String[] | Ein Array von Zeichenfolgen, das die Verwaltungsvorgänge angibt, deren Ausführung die Rolle zulässt. Weitere Informationen finden Sie unter [Aktionen](role-definitions.md#actions). |
| `NotActions` | Nein | String[] | Ein Array von Zeichenfolgen, das die Verwaltungsvorgänge angibt, die von den zulässigen `Actions` ausgeschlossen sind. Weitere Informationen finden Sie unter [NotActions](role-definitions.md#notactions). |
| `DataActions` | Nein | String[] | Ein Array von Zeichenfolgen, das die Datenvorgänge angibt, deren Ausführung für Ihre Daten innerhalb des Objekts die Rolle zulässt. Wenn Sie eine benutzerdefinierte Rolle mit `DataActions`erstellen, kann diese Rolle im Verwaltungsgruppenbereich nicht zugewiesen werden. Weitere Informationen finden Sie unter [DataActions](role-definitions.md#dataactions). |
| `NotDataActions` | Nein | String[] | Ein Array von Zeichenfolgen, das die Datenvorgänge angibt, die von den zulässigen `DataActions` ausgeschlossen sind. Weitere Informationen finden Sie unter [NotDataActions](role-definitions.md#notdataactions). |
| `AssignableScopes` | Ja | String[] | Ein Array von Zeichenfolgen, das die Bereiche angibt, in denen die benutzerdefinierte Rolle zur Zuweisung verfügbar ist. Sie können in den `AssignableScopes` einer benutzerdefinierten Rolle nur eine einzige Verwaltungsgruppe definieren. Das Hinzufügen einer Verwaltungsgruppe zu `AssignableScopes` befindet sich derzeit in der Vorschauphase. Weitere Informationen finden Sie unter [AssignableScopes](role-definitions.md#assignablescopes). |

## <a name="who-can-create-delete-update-or-view-a-custom-role"></a>Rollen zum Erstellen, Löschen, Aktualisieren oder Anzeigen einer benutzerdefinierten Rolle

Wie integrierte Rollen legt die `AssignableScopes`-Eigenschaft die Bereiche fest, in denen die Rolle zur Zuweisung verfügbar ist. Die `AssignableScopes`-Eigenschaft für eine benutzerdefinierte Rolle steuert auch, wer zum Erstellen, Löschen, Aktualisieren oder Anzeigen der benutzerdefinierten Rolle berechtigt ist.

| Aufgabe | Vorgang | BESCHREIBUNG |
| --- | --- | --- |
| Erstellen/Löschen einer benutzerdefinierten Rolle | `Microsoft.Authorization/ roleDefinitions/write` | Benutzer, die für alle `AssignableScopes` der benutzerdefinierten Rolle zu diesem Vorgang berechtigt sind, können benutzerdefinierte Rollen für die Verwendung in diesen Bereichen erstellen (oder löschen). Hierzu gehören beispielsweise [Besitzer](built-in-roles.md#owner) und [Benutzerzugriffsadministratoren](built-in-roles.md#user-access-administrator) von Verwaltungsgruppen, Abonnements und Ressourcengruppen. |
| Aktualisieren einer benutzerdefinierten Rolle | `Microsoft.Authorization/ roleDefinitions/write` | Benutzer, die für alle `AssignableScopes` der benutzerdefinierten Rolle zu diesem Vorgang berechtigt sind, können benutzerdefinierte Rollen in diesen Bereichen aktualisieren. Hierzu gehören beispielsweise [Besitzer](built-in-roles.md#owner) und [Benutzerzugriffsadministratoren](built-in-roles.md#user-access-administrator) von Verwaltungsgruppen, Abonnements und Ressourcengruppen. |
| Anzeigen einer benutzerdefinierten Rolle | `Microsoft.Authorization/ roleDefinitions/read` | Benutzer, die in einem Bereich zu diesem Vorgang berechtigt sind, können die benutzerdefinierten Rollen anzeigen, die für die Zuweisung in diesem Bereich verfügbar sind. Benutzerdefinierte Rollen können bei allen integrierten Rollen für die Zuweisung verfügbar sein. |

## <a name="custom-role-limits"></a>Grenzwerte bei benutzerdefinierten Rollen

In der nachstehenden Liste werden die Grenzwerte bei benutzerdefinierten Rollen beschrieben.

- Jedes Verzeichnis kann bis zu **5.000** benutzerdefinierte Rollen enthalten.
- In Azure Deutschland und Azure China 21ViaNet kann es für jedes Verzeichnis bis zu 2.000 benutzerdefinierte Rollen geben.
- `AssignableScopes` kann nicht auf den Stammbereich (`"/"`) festgelegt werden.
- Sie können in den `AssignableScopes` einer benutzerdefinierten Rolle nur eine einzige Verwaltungsgruppe definieren. Das Hinzufügen einer Verwaltungsgruppe zu `AssignableScopes` befindet sich derzeit in der Vorschauphase.
- Benutzerdefinierte Rollen mit `DataActions` können im Verwaltungsgruppenbereich nicht zugewiesen werden.
- Azure Resource Manager überprüft nicht, ob die Verwaltungsgruppe im zuweisbaren Bereich der Rollendefinition vorhanden ist.

Weitere Informationen zu benutzerdefinierten Rollen und Verwaltungsgruppen finden Sie unter [Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen](../governance/management-groups/overview.md#custom-rbac-role-definition-and-assignment).

## <a name="next-steps"></a>Nächste Schritte
- [Erstellen oder Aktualisieren von benutzerdefinierten Azure-Rollen über das Azure-Portal (Vorschau)](custom-roles-portal.md)
- [Grundlegendes zu Rollendefinitionen für Azure-Ressourcen](role-definitions.md)
- [Problembehandlung von RBAC für Azure-Ressourcen](troubleshooting.md)
