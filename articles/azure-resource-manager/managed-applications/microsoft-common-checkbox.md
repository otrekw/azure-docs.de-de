---
title: Benutzeroberflächenelement „CheckBox“
description: Hier wird das Benutzeroberflächenelement „Microsoft.Common.CheckBox“ für das Azure-Portal beschrieben. Hiermit können Benutzer eine Option aktivieren oder deaktivieren.
author: tfitzmac
ms.topic: conceptual
ms.date: 07/09/2020
ms.author: tomfitz
ms.openlocfilehash: 9f40f223cca34df58d2af7373d8f60fd7f383162
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87094804"
---
# <a name="microsoftcommoncheckbox-ui-element"></a>Benutzeroberflächenelement „Microsoft.Common.CheckBox“

Mit dem CheckBox-Steuerelement können Benutzer eine Option aktivieren oder deaktivieren. Das Steuerelement gibt **TRUE** zurück, wenn es aktiviert ist. Wenn es nicht aktiviert ist, gibt es **FALSE** zurück.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel

:::image type="content" source="./media/managed-application-elements/microsoft-common-checkbox.png" alt-text="Microsoft.Common.CheckBox":::

## <a name="schema"></a>Schema

```json
{
    "name": "legalAccept",
    "type": "Microsoft.Common.CheckBox",
    "label": "I agree to the terms and conditions.",
    "constraints": {
        "required": true,
        "validationMessage": "Please acknowledge the legal conditions."
    }
}
```

## <a name="sample-output"></a>Beispielausgabe

```json
true
```

## <a name="remarks"></a>Bemerkungen

Wenn Sie **required** auf **TRUE** festlegen, muss der Benutzer das Kontrollkästchen aktivieren. Wenn der Benutzer das Kontrollkästchen nicht aktiviert, wird eine Validierungsmeldung angezeigt.

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
