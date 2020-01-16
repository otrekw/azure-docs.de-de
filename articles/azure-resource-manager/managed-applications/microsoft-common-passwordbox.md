---
title: Benutzeroberflächenelement „PasswordBox“
description: Hier wird das Benutzeroberflächenelement Microsoft.Common.PasswordBox für das Azure-Portal beschrieben. Es ermöglicht Benutzern die Angabe eines Geheimniswerts beim Bereitstellen von verwalteten Anwendungen.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: 5f7d3a47482ccec9778b102d3509b5f8ef343185
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2020
ms.locfileid: "75649766"
---
# <a name="microsoftcommonpasswordbox-ui-element"></a>Benutzeroberflächenelement „Microsoft.Common.PasswordBox“

Ein Steuerelement, das zum Angeben und Bestätigen eines Kennworts verwendet werden kann.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel

![Microsoft.Common.PasswordBox](./media/managed-application-elements/microsoft.common.passwordbox.png)

## <a name="schema"></a>Schema

```json
{
  "name": "element1",
  "type": "Microsoft.Common.PasswordBox",
  "label": {
    "password": "Password",
    "confirmPassword": "Confirm password"
  },
  "toolTip": "",
  "constraints": {
    "required": true,
    "regex": "^[a-zA-Z0-9]{8,}$",
    "validationMessage": "Password must be at least 8 characters long, contain only numbers and letters"
  },
  "options": {
    "hideConfirmation": false
  },
  "visible": true
}
```

## <a name="sample-output"></a>Beispielausgabe

```json
"p4ssw0rd"
```

## <a name="remarks"></a>Bemerkungen

- Dieses Element unterstützt die `defaultValue`-Eigenschaft nicht.
- Implementierungsdetails zu `constraints` finden Sie unter [Microsoft.Common.TextBox](microsoft-common-textbox.md).
- Wenn `options.hideConfirmation` auf **true** festgelegt ist, wird das zweite Textfeld zum Bestätigen des Benutzerkennworts ausgeblendet. Der Standardwert ist **false**.

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
