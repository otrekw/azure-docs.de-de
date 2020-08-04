---
title: Benutzeroberflächenelement „DropDown“
description: Hier wird das Benutzeroberflächenelement Microsoft.Common.DropDown für das Azure-Portal beschrieben. Es wird beim Bereitstellen einer verwalteten Anwendung zum Auswählen aus verfügbaren Optionen verwendet.
author: tfitzmac
ms.topic: conceptual
ms.date: 07/14/2020
ms.author: tomfitz
ms.openlocfilehash: f5eac1d331bd439ad4066d1dea1b9aa950fcce60
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87004450"
---
# <a name="microsoftcommondropdown-ui-element"></a>Benutzeroberflächenelement „Microsoft.Common.DropDown“

Ein Auswahlsteuerelement mit einer Dropdownliste. Sie können die Auswahl nur eines einzelnen Elements oder mehrerer Elemente zulassen. Optional können Sie auch eine Beschreibung zu den Elementen einschließen.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel

Das DropDown-Element besitzt verschiedene Optionen, die seine Darstellung im Portal bestimmen.

Wenn nur ein einzelnes Element für die Auswahl zulässig ist, sieht das Steuerelement wie folgt aus:

:::image type="content" source="./media/managed-application-elements/microsoft-common-dropdown-1.png" alt-text="Microsoft.Common.DropDown mit Einfachauswahl":::

Wenn Beschreibungen enthalten sind, wird das Steuerelement wie folgt angezeigt:

:::image type="content" source="./media/managed-application-elements/microsoft-common-dropdown-2.png" alt-text="Microsoft.Common.DropDown mit Einfachauswahl mit Beschreibungen":::

Wenn Mehrfachauswahl aktiviert ist, fügt das Steuerelement eine **Alles auswählen**-Option sowie Kontrollkästchen für die Auswahl mehrerer Elemente hinzu:

:::image type="content" source="./media/managed-application-elements/microsoft-common-dropdown-3.png" alt-text="Microsoft.Common.DropDown mit Mehrfachauswahl":::

Beschreibungen können bei aktivierter Mehrfachauswahl eingeschlossen werden.

:::image type="content" source="./media/managed-application-elements/microsoft-common-dropdown-4.png" alt-text="Microsoft.Common.DropDown mit Mehrfachauswahl mit Beschreibungen":::

Wenn Filtern aktiviert ist, enthält das Steuerelement ein Textfeld zum Hinzufügen des Filterwerts.

:::image type="content" source="./media/managed-application-elements/microsoft-common-dropdown-5.png" alt-text="Microsoft.Common.DropDown mit Mehrfachauswahl mit Beschreibungen":::

## <a name="schema"></a>Schema

```json
{
    "name": "element1",
    "type": "Microsoft.Common.DropDown",
    "label": "Example drop down",
    "defaultValue": "Value two",
    "toolTip": "",
    "multiselect": true,  
    "selectAll": true,  
    "filter": true,  
    "filterPlaceholder": "Filter items ...",  
    "multiLine": true,  
    "defaultDescription": "A value for selection",  
    "constraints": {
        "allowedValues": [
            {
                "label": "Value one",
                "description": "The value to select for option 1.",
                "value": "one"
            },
            {
                "label": "Value two",
                "description": "The value to select for option 2.",
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

- Verwenden Sie `multiselect`, um anzugeben, ob Benutzer mehr als ein Element auswählen können.
- Standardmäßig ist `selectAll` `true`, wenn Mehrfachauswahl aktiviert ist.
- Die `filter`-Eigenschaft ermöglicht Benutzer, innerhalb einer langen Liste von Optionen zu suchen.
- Die Bezeichnung für `constraints.allowedValues` ist der Anzeigetext für ein Element, und sein Wert ist der Ausgabewert des Elements bei der Auswahl.
- Falls angegeben, muss der Standardwert einer in `constraints.allowedValues` vorhandenen Bezeichnung entsprechen. Ohne Angabe wird das erste Element in `constraints.allowedValues` ausgewählt. Der Standardwert lautet **null**.
- `constraints.allowedValues` muss mindestens ein Element aufweisen.
- Fügen Sie zum Emulieren eines nicht benötigten Werts `constraints.allowedValues` ein Element mit einer Bezeichnung und dem Wert `""` (leere Zeichenfolge) hinzu.
- Die `defaultDescription`-Eigenschaft wird für Elemente verwendet, die keine Beschreibung besitzen.

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
