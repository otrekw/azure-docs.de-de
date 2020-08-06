---
title: Benutzeroberflächenelement „IdentitySelector“
description: Beschreibt das Benutzeroberflächenelement Microsoft.ManagedIdentity.IdentitySelector für das Azure-Portal. Sie können damit einer Ressource verwaltete Identitäten zuweisen.
author: tfitzmac
ms.topic: conceptual
ms.date: 02/06/2020
ms.author: tomfitz
ms.openlocfilehash: 91e3cd0b99825fd72eb342ce7a8555b046455538
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87063370"
---
# <a name="microsoftmanagedidentityidentityselector-ui-element"></a>Microsoft.ManagedIdentity.IdentitySelector UI element

Ein Steuerelement zum Zuweisen [verwalteter Identitäten](../../active-directory/managed-identities-azure-resources/overview.md) für eine Ressource in einer Bereitstellung.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel

Das Steuerelement besteht aus den folgenden Elementen:

![Microsoft.ManagedIdentity.IdentitySelector – erster Schritt](./media/managed-application-elements/microsoft-managedidentity-identityselector-1.png)

Wenn der Benutzer **Hinzufügen** auswählt, wird das folgende Formular geöffnet. Der Benutzer kann eine oder mehrere vom Benutzer zugewiesene Identitäten für die Ressource auswählen.

![Microsoft.ManagedIdentity.IdentitySelector – zweiter Schritt](./media/managed-application-elements/microsoft-managedidentity-identityselector-2.png)

Die ausgewählten Identitäten werden in der Tabelle angezeigt. Der Benutzer kann dieser Tabelle Elemente hinzufügen oder sie daraus entfernen.

![Microsoft.ManagedIdentity.IdentitySelector – dritter Schritt](./media/managed-application-elements/microsoft-managedidentity-identityselector-3.png)

## <a name="schema"></a>Schema

```json
{
  "name": "identity",
  "type": "Microsoft.ManagedIdentity.IdentitySelector",
  "label": "Managed Identity Configuration",
  "toolTip": {
    "systemAssignedIdentity": "Enable system assigned identity to grant the resource access to other existing resources.",
    "userAssignedIdentity": "Add user assigned identities to grant the resource access to other existing resources."
  },
  "defaultValue": {
    "systemAssignedIdentity": "Off"
  },
  "options": {
    "hideSystemAssignedIdentity": false,
    "hideUserAssignedIdentity": false
  },
  "visible": true
}
```

## <a name="sample-output"></a>Beispielausgabe

```json
{
  "identity": {
    "value": {
      "type": "UserAssigned",
      "userAssignedIdentities": {
        "/subscriptions/xxxx/resourceGroups/TestResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/TestUserIdentity1": {}
      }
    }
  }
}
```

## <a name="remarks"></a>Bemerkungen

- Verwenden Sie **defaultValue.systemAssignedIdentity**, um einen Anfangswert für das Optionssteuerelement für die systemseitig zugewiesene Identität festzulegen. Der Standardwert ist **Off**. Folgende Werte sind zulässig:
  - **On:** Der Ressource wird eine systemseitig zugewiesene Identität zugewiesen.
  - **Off:** Der Ressource wird keine systemseitig zugewiesene Identität zugewiesen.
  - **OnOnly:** Der Ressource wird eine systemseitig zugewiesene Identität zugewiesen. Benutzer können diesen Wert während der Bereitstellung nicht bearbeiten.
  - **OffOnly:** Der Ressource wird keine systemseitig zugewiesene Identität zugewiesen. Benutzer können diesen Wert während der Bereitstellung nicht bearbeiten.

- Wenn **options.hideSystemAssignedIdentity** auf **true** festgelegt ist, wird die Benutzeroberfläche zum Konfigurieren der systemseitig zugewiesenen Identität nicht angezeigt. Der Standardwert für diese Option ist **false**.
- Wenn **options.hideUserAssignedIdentity** auf **true** festgelegt ist, wird die Benutzeroberfläche zum Konfigurieren der benutzerseitig zugewiesenen Identität nicht angezeigt. Der Ressource wird keine benutzerseitig zugewiesene Identität zugewiesen. Der Standardwert für diese Option ist **false**.

## <a name="next-steps"></a>Nächste Schritte

- Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
- Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).