---
title: Azure Event Grid – Sicherheit und Authentifizierung
description: In diesem Artikel werden Azure Event Grid und die zugehörigen Begriffe beschrieben.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: babanisa
ms.openlocfilehash: 03bc2f9de6f50f08c9f62f86a3d1791a067cecd0
ms.sourcegitcommit: 668b3480cb637c53534642adcee95d687578769a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2020
ms.locfileid: "78899004"
---
# <a name="authorizing-access-to-event-grid-resources"></a>Autorisieren des Zugriffs auf Event Grid-Ressourcen
Azure Event Grid bietet die Möglichkeit, den Umfang zu steuern, in dem unterschiedliche Benutzer Zugriff auf verschiedene Verwaltungsvorgänge erhalten, z.B. Auflisten und Erstellen von Ereignisabonnements und Generieren von Schlüsseln. Event Grid nutzt die rollenbasierte Zugriffsüberprüfung (Role Based Access Control, RBAC) von Azure.

## <a name="operation-types"></a>Vorgangstypen

Event Grid unterstützt die folgenden Aktionen:

* Microsoft.EventGrid/*/read
* Microsoft.EventGrid/*/write
* Microsoft.EventGrid/*/delete
* Microsoft.EventGrid/eventSubscriptions/getFullUrl/action
* Microsoft.EventGrid/topics/listKeys/action
* Microsoft.EventGrid/topics/regenerateKey/action

Die letzten drei Vorgänge geben potenziell geheime Informationen zurück, die aus normalen Lesevorgängen herausgefiltert werden. Es empfiehlt sich, den Zugriff auf diese Vorgänge einzuschränken. 

## <a name="built-in-roles"></a>Integrierte Rollen

Event Grid stellt zwei integrierte Rollen zum Verwalten von Ereignisabonnements bereit. Sie sind wichtig beim Implementieren von [Ereignisdomänen](event-domains.md), da sie Benutzern die erforderlichen Berechtigungen erteilen, um Themen in Ihrer Ereignisdomäne zu abonnieren. Diese Rollen gelten für Ereignisabonnements und gewähren keinen Zugriff auf Aktionen. Das heißt, sie berechtigen Sie beispielsweise nicht zum Erstellen von Themen.

Sie können [diese Rollen einem Benutzer oder Gruppen zuweisen](../role-based-access-control/quickstart-assign-role-user-portal.md).

**EventGrid EventSubscription Mitwirkender:** Verwalten von Event Grid-Abonnementvorgängen

```json
[
  {
    "Description": "Lets you manage EventGrid event subscription operations.",
    "IsBuiltIn": true,
    "Id": "428e0ff05e574d9ca2212c70d0e0a443",
    "Name": "EventGrid EventSubscription Contributor",
    "IsServiceRole": false,
    "Permissions": [
      {
        "Actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.EventGrid/eventSubscriptions/*",
          "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Resources/deployments/*",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Support/*"
        ],
        "NotActions": [],
        "DataActions": [],
        "NotDataActions": [],
        "Condition": null
      }
    ],
    "Scopes": [
      "/"
    ]
  }
]
```

**EventGrid EventSubscription Reader:** Lesen von Event Grid-Abonnements

```json
[
  {
    "Description": "Lets you read EventGrid event subscriptions.",
    "IsBuiltIn": true,
    "Id": "2414bbcf64974faf8c65045460748405",
    "Name": "EventGrid EventSubscription Reader",
    "IsServiceRole": false,
    "Permissions": [
      {
        "Actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.EventGrid/eventSubscriptions/read",
          "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read"
        ],
        "NotActions": [],
        "DataActions": [],
        "NotDataActions": []
       }
    ],
    "Scopes": [
      "/"
    ]
  }
]
```

## <a name="custom-roles"></a>Benutzerdefinierte Rollen

Wenn Sie Berechtigungen angeben müssen, die sich von den integrierten Rollen unterscheiden, können Sie benutzerdefinierte Rollen erstellen.

Im Folgenden finden Sie Beispiele für Event Grid-Rollendefinitionen, die Benutzern das Durchführen unterschiedlicher Aktionen ermöglichen. Diese benutzerdefinierten Rollen unterscheiden sich von den integrierten Rollen, da sie einen breiteren Zugriff ermöglichen als Ereignisabonnements.

**EventGridReadOnlyRole.json**: Ausschließliches Zulassen schreibgeschützter Operationen.

```json
{
  "Name": "Event grid read only role",
  "Id": "7C0B6B59-A278-4B62-BA19-411B70753856",
  "IsCustom": true,
  "Description": "Event grid read only role",
  "Actions": [
    "Microsoft.EventGrid/*/read"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription Id>"
  ]
}
```

**EventGridNoDeleteListKeysRole.json**: Zulassen eingeschränkter Post-Aktionen, nicht aber von Löschaktionen.

```json
{
  "Name": "Event grid No Delete Listkeys role",
  "Id": "B9170838-5F9D-4103-A1DE-60496F7C9174",
  "IsCustom": true,
  "Description": "Event grid No Delete Listkeys role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action"
  ],
  "NotActions": [
    "Microsoft.EventGrid/*/delete"
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

**EventGridContributorRole.json**: Zulassen aller Event Grid-Aktionen.

```json
{
  "Name": "Event grid contributor role",
  "Id": "4BA6FB33-2955-491B-A74F-53C9126C9514",
  "IsCustom": true,
  "Description": "Event grid contributor role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/*/delete",
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
  ],
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

Benutzerdefinierte Rollen können mit [PowerShell](../role-based-access-control/custom-roles-powershell.md), der [Azure CLI](../role-based-access-control/custom-roles-cli.md) oder der [REST-API](../role-based-access-control/custom-roles-rest.md) erstellt werden.



### <a name="encryption-at-rest"></a>Verschlüsselung ruhender Daten

Alle Ereignisse oder Daten, die vom Event Grid-Dienst auf den Datenträger geschrieben werden, werden unter Verwendung eines von Microsoft verwalteten Schlüssels verschlüsselt, um die Verschlüsselung der ruhenden Daten zu gewährleisten. Ereignisse und Daten werden außerdem maximal 24 Stunden lang aufbewahrt (in Übereinstimmung mit der [Event Grid-Wiederholungsrichtlinie](delivery-and-retry.md)). Nach 24 Stunden oder nach Ablauf der Ereignislebensdauer (je nachdem, welcher Zeitraum kürzer ist) werden alle Ereignisse und Daten von Event Grid gelöscht.

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zu Event Grid finden Sie unter [Informationen zu Event Grid](overview.md).
