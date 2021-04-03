---
title: Benutzeroberflächenelement „OptionsGroup“
description: Hier wird das Benutzeroberflächenelement „Microsoft.Common.OptionsGroup“ für das Azure-Portal beschrieben. Es ermöglicht Benutzern die Auswahl aus verfügbaren Optionen beim Bereitstellen einer verwalteten Anwendung.
author: tfitzmac
ms.topic: conceptual
ms.date: 07/09/2020
ms.author: tomfitz
ms.openlocfilehash: aa73b4cbded98291a14792a7151df9fdfb885b53
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "87004188"
---
# <a name="microsoftcommonoptionsgroup-ui-element"></a>Benutzeroberflächenelement „Microsoft.Common.OptionsGroup“

Mit dem OptionsGroup-Steuerelement können Benutzer eine Option aus mindestens zwei Optionen auswählen. Ein Benutzer kann nur eine Option auswählen.

> [!NOTE]
> In der Vergangenheit hat dieses Steuerelement die Optionen horizontal gerendert. Nun stellt das Steuerelement die Optionen vertikal als Optionsfelder dar.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel

:::image type="content" source="./media/managed-application-elements/microsoft-common-optionsgroup-2.png" alt-text="Microsoft.Common.OptionsGroup":::

## <a name="schema"></a>Schema

```json
{
  "name": "element1",
  "type": "Microsoft.Common.OptionsGroup",
  "label": "Some options group",
  "defaultValue": "Value two",
  "toolTip": "",
  "constraints": {
    "allowedValues": [
      {
        "label": "Value one",
        "value": "one"
      },
      {
        "label": "Value two",
        "value": "two"
      }
    ],
    "required": true
  },
  "visible": true
}
```

## <a name="sample-output"></a>Beispielausgabe

```json
"two"
```

## <a name="remarks"></a>Bemerkungen

- Die Bezeichnung für `constraints.allowedValues` ist der Anzeigetext für ein Element, und sein Wert ist der Ausgabewert des Elements bei der Auswahl.
- Falls angegeben, muss der Standardwert einer in `constraints.allowedValues` vorhandenen Bezeichnung entsprechen. Wird kein Wert angegeben, wird standardmäßig das erste Element in `constraints.allowedValues` ausgewählt. Der Standardwert lautet **null**.
- `constraints.allowedValues` muss mindestens ein Element aufweisen.

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
