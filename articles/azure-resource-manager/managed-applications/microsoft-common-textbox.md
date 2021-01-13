---
title: Benutzeroberflächenelement „TextBox“
description: Hier wird das Benutzeroberflächenelement Microsoft.Common.TextBox für das Azure-Portal beschrieben. Es wird zum Hinzufügen von unformatiertem Text verwendet.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: c89bc434d9d67144a95b5c2f23e7664078fe7825
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87474307"
---
# <a name="microsoftcommontextbox-ui-element"></a>Benutzeroberflächenelement „Microsoft.Common.TextBox UI“

Ein Steuerelement, das zum Bearbeiten von unformatiertem Text verwendet werden kann.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel

![Microsoft.Common.TextBox](./media/managed-application-elements/microsoft-common-textbox.png)

## <a name="schema"></a>Schema

```json
{
    "name": "nameInstance",
    "type": "Microsoft.Common.TextBox",
    "label": "Name",
    "defaultValue": "contoso123",
    "toolTip": "Use only allowed characters",
    "placeholder": "",
    "constraints": {
        "required": true,
        "validations": [
            {
                "regex": "^[a-z0-9A-Z]{1,30}$",
                "message": "Only alphanumeric characters are allowed, and the value must be 1-30 characters long."
            },
            {
                "isValid": "[startsWith(steps('resourceConfig').nameInstance, 'contoso')]",
                "message": "Must start with 'contoso'."
            }
        ]
    },
    "visible": true
}
```

## <a name="sample-output"></a>Beispielausgabe

```json
"contoso123"
```

## <a name="remarks"></a>Bemerkungen

- Wenn `constraints.required` auf **TRUE** festgelegt ist, muss das Textfeld für eine erfolgreiche Überprüfung einen Wert enthalten. Der Standardwert ist **false**.
- Die `validations`-Eigenschaft ist ein Array, in dem Sie Bedingungen zum Überprüfen des im Textfeld angegebenen Werts hinzufügen.
- Die Eigenschaft `regex` ist ein reguläres JavaScript-Ausdrucksmuster. Wenn der Wert des Textfelds angegeben wird, muss er für eine erfolgreiche Überprüfung dem Muster entsprechen. Der Standardwert lautet **null**.
- Die `isValid`-Eigenschaft enthält einen Ausdruck, der zu „true“ oder „false“ ausgewertet wird. Innerhalb des Ausdrucks definieren Sie die Bedingung, die bestimmt, ob das Textfeld gültig ist.
- Die Eigenschaft `message` ist eine Zeichenfolge, die angezeigt wird, wenn bei der Überprüfung des Werts des Textfelds ein Fehler auftritt.
- Für `regex` kann ein Wert angegeben werden, wenn `required` auf **false** festgelegt ist. In diesem Szenario ist für eine erfolgreiche Überprüfung des Textfelds kein Wert erforderlich. Wird ein Wert angegeben, muss er dem Muster des regulären Ausdrucks entsprechen.
- Die `placeholder`-Eigenschaft ist Hilfetext, der ausgeblendet wird, wenn der Benutzer mit der Bearbeitung beginnt. Wenn sowohl `placeholder` als auch `defaultValue` definiert ist, hat `defaultValue` Vorrang und wird angezeigt.

## <a name="example"></a>Beispiel

Im folgenden Beispiel wird ein Textfeld mit dem [Microsoft.Solutions.ArmApiControl](microsoft-solutions-armapicontrol.md)-Steuerelement verwendet, um die Verfügbarkeit eines Ressourcennamens zu überprüfen.

```json
"basics": [
    {
        "name": "nameApi",
        "type": "Microsoft.Solutions.ArmApiControl",
        "request": {
            "method": "POST",
            "path": "[concat(subscription().id, '/providers/Microsoft.Storage/checkNameAvailability?api-version=2019-06-01')]",
            "body": "[parse(concat('{\"name\": \"', basics('txtStorageName'), '\", \"type\": \"Microsoft.Storage/storageAccounts\"}'))]"
        }
    },
    {
        "name": "txtStorageName",
        "type": "Microsoft.Common.TextBox",
        "label": "Storage account name",
        "constraints": {
            "validations": [
                {
                    "isValid": "[not(equals(basics('nameApi').nameAvailable, false))]",
                    "message": "[concat('Name unavailable: ', basics('txtStorageName'))]"
                }
            ]
        }
    }
]
```

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
