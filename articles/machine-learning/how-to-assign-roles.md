---
title: Verwalten von Rollen in Ihrem Arbeitsbereich
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie mit der rollenbasierten Zugriffssteuerung auf einen Azure Machine Learning-Arbeitsbereich zugreifen können.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: larryfr
author: Blackmist
ms.date: 11/06/2019
ms.custom: seodec18
ms.openlocfilehash: 5257d9f94f6304c2a8dbea3f1648a71d0ba65e94
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2020
ms.locfileid: "77064749"
---
# <a name="manage-access-to-an-azure-machine-learning-workspace"></a>Verwalten des Zugriffs auf einen Azure Machine Learning-Arbeitsbereich
[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie den Zugriff auf einen Azure Machine Learning-Arbeitsbereich verwalten können. Der Zugriff auf Azure-Ressourcen wird mithilfe der [rollenbasierten Zugriffssteuerung (Role-Bases Access Control, RBAC)](/azure/role-based-access-control/overview) verwaltet. Benutzer in Ihrer Azure Active Directory-Instanz erhalten bestimmte Rollen, anhand derer sie Zugriff auf Ressourcen erhalten. Azure bietet sowohl integrierte Rollen als auch die Möglichkeit, benutzerdefinierte Rollen zu erstellen.

## <a name="default-roles"></a>Standardrollen

Ein Azure Machine Learning-Arbeitsbereich ist eine Azure-Ressource. Wie jede andere Azure-Ressource verfügt ein neu erstellter Azure Machine Learning-Arbeitsbereich über drei Standardrollen. Sie können dem Arbeitsbereich Benutzer hinzufügen und diesen eine dieser integrierten Rollen zuweisen.

| Role | Zugriffsebene |
| --- | --- |
| **Leser** | Schreibgeschützte Aktionen im Arbeitsbereich. Leser können Objekte in einem Arbeitsbereich auflisten und anzeigen, können aber keine neuen erstellen und die vorhandenen nicht anpassen. |
| **Mitwirkender** | Anzeigen, Erstellen, Bearbeiten und Löschen (wo zutreffend) von Objekten in einem Arbeitsbereich. Mitwirkende können z.B. ein Experiment erstellen, einen Computecluster erstellen oder anfügen, eine Ausführung durchführen oder einen Webdienst bereitstellen. |
| **Besitzer** | Vollzugriff auf den Arbeitsbereich, u.a. Anzeigen, Erstellen, Bearbeiten und Löschen (wo zutreffend) von Objekten in einem Arbeitsbereich. Zudem können Besitzer Rollenzuweisungen anpassen. |

> [!IMPORTANT]
> Der Rollenzugriff kann für mehrere Ebenen in Azure gelten. Es kann z. B. sein, dass ein Benutzer mit Vollzugriff für einen Arbeitsbereich für die Ressourcengruppe, die diesen Arbeitsbereich enthält, keinen Vollzugriff hat. Weitere Informationen erhalten Sie im Abschnitt „Funktionsweise von RBAC“ unter [Was ist die rollenbasierte Zugriffssteuerung für Azure-Ressourcen?](/azure/role-based-access-control/overview#how-rbac-works).

Weitere Informationen zu Rollen finden Sie unter [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](/azure/role-based-access-control/built-in-roles).

## <a name="manage-workspace-access"></a>Verwalten des Arbeitsbereichszugriffs

Wenn Sie Besitzer eines Arbeitsbereichs sind, können Sie für den Arbeitsbereich Rollen hinzufügen und entfernen. Sie können Benutzern auch Rollen zuweisen. Unter den folgenden Links erhalten Sie Informationen zur Zugriffsverwaltung:
- [Verwalten des Zugriffs auf Azure-Ressourcen mit RBAC und dem Azure-Portal](/azure/role-based-access-control/role-assignments-portal)
- [PowerShell](/azure/role-based-access-control/role-assignments-powershell)
- [Azure-Befehlszeilenschnittstelle](/azure/role-based-access-control/role-assignments-cli)
- [REST-API](/azure/role-based-access-control/role-assignments-rest)
- [Azure-Ressourcen-Manager-Vorlagen](/azure/role-based-access-control/role-assignments-template)

Wenn Sie die [Azure Machine Learning-CLI](reference-azure-machine-learning-cli.md) installiert haben, können Sie Benutzern Rollen auch über einen CLI-Befehl zuweisen.

```azurecli-interactive 
az ml workspace share -w <workspace_name> -g <resource_group_name> --role <role_name> --user <user_corp_email_address>
```

Das Feld `user` ist die E-Mail-Adresse eines vorhandenen Benutzers in der Azure Active Directory-Instanz, in der sich das übergeordnete Abonnement des Arbeitsbereichs befindet. Im folgenden Ausschnitt wird die Verwendung des Befehls veranschaulicht:

```azurecli-interactive 
az ml workspace share -w my_workspace -g my_resource_group --role Contributor --user jdoe@contoson.com
```

## <a name="create-custom-role"></a>Erstellen einer benutzerdefinierten Rolle

Wenn die integrierten Rolle nicht ausreichend sind, können Sie benutzerdefinierte Rollen erstellen. Benutzerdefinierte Rollen können über Berechtigung zum Lesen, Schreiben, Löschen und für Computeressourcen in diesem Arbeitsbereich verfügen. Sie können die Rolle auf einer bestimmten Arbeitsbereichsebene, einer bestimmten Ressourcengruppenebene oder einer bestimmten Abonnementebene verfügbar machen.

> [!NOTE]
> Sie müssen auf der entsprechenden Ebene Besitzer der Ressource sein, um eine benutzerdefinierte Rolle in dieser Ressource erstellen zu können.

Erstellen Sie zunächst eine JSON-Datei mit einer Rollendefinition, die die Berechtigungen und den Geltungsbereich der Rolle angibt, um eine benutzerdefinierte Rolle zu erstellen. Im folgenden Beispiel wird die benutzerdefinierte Rolle „Data Scientist“ definiert, die für eine bestimmte Ebene des Arbeitsbereichs gilt:

`data_scientist_role.json` :
```json
{
    "Name": "Data Scientist",
    "IsCustom": true,
    "Description": "Can run experiment but can't create or delete compute.",
    "Actions": ["*"],
    "NotActions": [
        "Microsoft.MachineLearningServices/workspaces/*/delete",
        "Microsoft.MachineLearningServices/workspaces/computes/*/write",
        "Microsoft.MachineLearningServices/workspaces/computes/*/delete", 
        "Microsoft.Authorization/*/write"
    ],
    "AssignableScopes": [
        "/subscriptions/<subscription_id>/resourceGroups/<resource_group_name>/providers/Microsoft.MachineLearningServices/workspaces/<workspace_name>"
    ]
}
```

Sie können das Feld `AssignableScopes` anpassen, um den Geltungsbereich für diese benutzerdefinierte Rolle auf Abonnementebene, Ressourcengruppenebene oder eine bestimmte Arbeitsbereichsebene festzulegen.

Mit dieser benutzerdefinierten Rolle kann ein Benutzer alle Aktionen außer die folgenden in einem Arbeitsbereich durchführen:

- Er kann keine Computeressourcen erstellen oder aktualisieren.
- Er kann keine Computeressourcen löschen.
- Er kann keine Rollenzuweisungen hinzufügen, löschen oder anpassen.
- Er kann den Arbeitsbereich nicht löschen.

Mit dem folgenden Azure CLI-Befehl können Sie die benutzerdefinierte Rolle bereitstellen:

```azurecli-interactive 
az role definition create --role-definition data_scientist_role.json
```

Nach der Bereitstellung ist die Rolle im angegebenen Arbeitsbereich verfügbar. Jetzt können Sie diese Rolle über das Azure-Portal hinzufügen und zuweisen. Alternativ können Sie diese Rolle einem Benutzer mit dem CLI-Befehl `az ml workspace share` zuweisen:

```azurecli-interactive
az ml workspace share -w my_workspace -g my_resource_group --role "Data Scientist" --user jdoe@contoson.com
```

Weitere Informationen zu benutzerdefinierten Rollen finden Sie unter [Benutzerdefinierte Rollen für Azure-Ressourcen](/azure/role-based-access-control/custom-roles).

Weitere Informationen zu den Vorgängen (Aktionen), die mit benutzerdefinierten Rollen durchgeführt werden können, finden Sie unter [Vorgänge für Ressourcenanbieter](/azure/role-based-access-control/resource-provider-operations#microsoftmachinelearningservices).


## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen


### <a name="q-what-are-the-permissions-needed-to-perform-various-actions-in-the-azure-machine-learning-service"></a>Q. Welche Berechtigungen sind erforderlich, um verschiedene Aktionen in Azure Machine Learning Service auszuführen?

Die folgende Tabelle ist eine Zusammenfassung der Azure Machine Learning-Aktivitäten und der erforderlichen Berechtigungen, um sie im kleinstmöglichen Umfang auszuführen. Wenn eine Aktivität beispielsweise im Bereich eines Arbeitsbereichs (Spalte 4) ausgeführt werden kann, funktionieren alle umfassenderen Bereiche mit dieser Berechtigung automatisch ebenfalls. Alle Pfade in dieser Tabelle sind **relativ** zu `Microsoft.MachineLearningServices/`.

| Aktivität | Bereich der Abonnementebene | Bereich einer Ressourcengruppe | Bereich eines Arbeitsbereichs |
|---|---|---|---|
| Erstellen eines neuen Arbeitsbereichs | Nicht erforderlich | „Besitzer“ oder „Mitwirkender“ | – (wird nach der Erstellung zum Besitzer oder erbt eine Rolle mit umfassenderem Bereich) |
| Erstellen eines neuen Computeclusters | Nicht erforderlich | Nicht erforderlich | „Besitzer“, „Mitwirkender“ oder benutzerdefinierte Rolle mit folgenden Berechtigungen: `workspaces/computes/write` |
| Erstellen einer neuen Notebook-VM | Nicht erforderlich | „Besitzer“ oder „Mitwirkender“ | Nicht möglich |
| Erstellen einer neuen Computeinstanz | Nicht erforderlich | Nicht erforderlich | „Besitzer“, „Mitwirkender“ oder benutzerdefinierte Rolle mit folgenden Berechtigungen: `workspaces/computes/write` |
| Aktivität auf Datenebene, z. B. Übermitteln einer Ausführung, Zugreifen auf Daten, Bereitstellen von Modellen oder Veröffentlichen einer Pipeline | Nicht erforderlich | Nicht erforderlich | „Besitzer“, „Mitwirkender“ oder benutzerdefinierte Rolle mit folgenden Berechtigungen: `workspaces/*/write` <br/> Beachten Sie, dass Sie auch einen im Arbeitsbereich registrierten Datenspeicher benötigen, damit MSI auf Daten in Ihrem Speicherkonto zugreifen kann. |


### <a name="q-how-do-i-list-all-the-custom-roles-in-my-subscription"></a>Q. Wie liste ich alle benutzerdefinierten Rollen in meinem Abonnement auf?

Führen Sie in der Azure-Befehlszeilenschnittstelle den folgenden Befehl aus.

```azurecli-interactive
az role definition list --subscription <sub-id> --custom-role-only true
```

### <a name="q-how-do-i-find-the-role-definition-for-a-role-in-my-subscription"></a>Q. Wie suche ich die Rollendefinition für eine Rolle in meinem Abonnement?

Führen Sie in der Azure-Befehlszeilenschnittstelle den folgenden Befehl aus. Beachten Sie, dass `<role-name>` das Format aufweisen sollte, das vom obigen Befehl zurückgegeben wird.

```azurecli-interactive
az role definition list -n <role-name> --subscription <sub-id>
```

### <a name="q-how-do-i-update-a-role-definition"></a>Q. Wie aktualisiere ich eine Rollendefinition?

Führen Sie in der Azure-Befehlszeilenschnittstelle den folgenden Befehl aus.

```azurecli-interactive
az role definition update --role-definition update_def.json --subscription <sub-id>
```

Beachten Sie, dass Sie über Berechtigungen für den gesamten Bereich der neuen Rollendefinition verfügen müssen. Wenn diese neue Rolle beispielsweise als Bereich drei Abonnements umfasst, müssen Sie über Berechtigungen für alle drei Abonnements verfügen. 

> [!NOTE]
> Es kann bei einem Rollenupdate zwischen 15 Minuten und einer Stunde dauern, alle Rollenzuweisungen in diesem Bereich anzuwenden.
### <a name="q-can-i-define-a-role-that-prevents-updating-the-workspace-edition"></a>Q. Kann ich eine Rolle definieren, die das Aktualisieren der Arbeitsbereichs-Edition verhindert? 

Ja, Sie können eine Rolle definieren, die das Aktualisieren der Arbeitsbereichs-Edition verhindert. Da es sich bei der Arbeitsbereichsaktualisierung um einen PATCH-Aufruf für das Arbeitsbereichsobjekt handelt, erreichen Sie dies, indem Sie die folgende Aktion in das `"NotActions"`-Array in Ihrer JSON-Definition einfügen: 

`"Microsoft.MachineLearningServices/workspaces/write"`

### <a name="q-what-permissions-are-needed-to-perform-quota-operations-in-a-workspace"></a>Q. Welche Berechtigungen sind erforderlich, um Kontingentvorgänge in einem Arbeitsbereich auszuführen? 

Sie benötigen Berechtigungen auf Abonnementebene, um auf das Kontingent bezogene Vorgänge im Arbeitsbereich auszuführen. Das bedeutet, dass das Festlegen eines Kontingents auf Abonnementebene oder auf Arbeitsbereichsebene für Ihre verwalteten Computeressourcen nur möglich ist, wenn Sie über Schreibberechtigungen im Abonnementbereich verfügen. 


## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über Unternehmenssicherheit](concept-enterprise-security.md)
- [Sicheres Ausführen von Experimenten und Ziehen von Rückschlüssen oder Bewerten innerhalb eines virtuellen Azure-Netzwerks](how-to-enable-virtual-network.md)
- [Tutorial: Trainieren von Modellen](tutorial-train-models-with-aml.md)
- [Vorgänge für Ressourcenanbieter](/azure/role-based-access-control/resource-provider-operations#microsoftmachinelearningservices)
