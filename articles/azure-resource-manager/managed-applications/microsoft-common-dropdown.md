---
title: Benutzeroberflächenelement „DropDown“
description: Hier wird das Benutzeroberflächenelement Microsoft.Common.DropDown für das Azure-Portal beschrieben. Es wird beim Bereitstellen einer verwalteten Anwendung zum Auswählen aus verfügbaren Optionen verwendet.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: a09f9695c18f368a585dbcd0d1e654dee4adfa03
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75649790"
---
# <a name="microsoftcommondropdown-ui-element"></a>Benutzeroberflächenelement „Microsoft.Common.DropDown“

Ein Auswahlsteuerelement mit einer Dropdownliste.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel

![Microsoft.Common.DropDown](./media/managed-application-elements/microsoft.common.dropdown.png)

## <a name="schema"></a>Schema

```json
{
  "name": "element1",
  "type": "Microsoft.Common.DropDown",
  "label": "Example drop down",
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
- Falls angegeben, muss der Standardwert einer in `constraints.allowedValues` vorhandenen Bezeichnung entsprechen. Ohne Angabe wird das erste Element in `constraints.allowedValues` ausgewählt. Der Standardwert ist **NULL**.
- `constraints.allowedValues` muss mindestens ein Element aufweisen.
- Fügen Sie zum Emulieren eines nicht benötigten Werts `""` ein Element mit einer Bezeichnung und dem Wert `constraints.allowedValues` (leere Zeichenfolge) hinzu.

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
