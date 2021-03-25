---
title: Benutzeroberflächenelement „ResourceSelector“
description: Hier wird das Benutzeroberflächenelement „Microsoft.Solutions.ResourceSelector“ für das Azure-Portal beschrieben. Mit diesem Element wird eine Liste der vorhandenen Ressourcen abgerufen.
author: tfitzmac
ms.topic: conceptual
ms.date: 07/13/2020
ms.author: tomfitz
ms.openlocfilehash: d4b21f092733f6ebb3c2bf5aa06c1d2782323730
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "87094975"
---
# <a name="microsoftsolutionsresourceselector-ui-element"></a>Benutzeroberflächenelement „Microsoft.Solutions.ResourceSelector“

Mit ResourceSelector können Benutzer eine vorhandene Ressource aus einem Abonnement auswählen.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel

![Microsoft.Solutions.ResourceSelector](./media/managed-application-elements/microsoft-solutions-resourceselector.png)

## <a name="schema"></a>Schema

```json
{
    "name": "storageSelector",
    "type": "Microsoft.Solutions.ResourceSelector",
    "label": "Select storage accounts",
    "resourceType": "Microsoft.Storage/storageAccounts",
    "options": {
        "filter": {
            "subscription": "onBasics",
            "location": "onBasics"
        }
    }
}
```

## <a name="sample-output"></a>Beispielausgabe

```json
"name": "{resource-name}",
"id": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{resource-provider-namespace}/{resource-type}/{resource-name}",
"location": "{deployed-location}"
```

## <a name="remarks"></a>Bemerkungen

Geben Sie in der Eigenschaft `resourceType` den Ressourcenanbieter-Namespace und den Ressourcentypname für die Ressource an, die in der Liste angezeigt werden soll.

Mit der Eigenschaft `filter` werden die verfügbaren Optionen für die Ressourcen eingeschränkt. Sie können die Ergebnisse nach Standort oder Abonnement einschränken. Verwenden Sie `onBasics`, um nur Ressourcen anzuzeigen, die der Auswahl unter „Grundlagen“ entsprechen. Verwenden Sie `all`, um alle Ressourcen anzuzeigen. Standardwert: `all`.

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
