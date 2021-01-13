---
title: EditableGrid-Benutzeroberflächenelement
description: Beschreibt das Benutzeroberflächenelement Microsoft.Common.EditableGrid für das Azure-Portal. Ermöglicht Benutzern das Erfassen von Tabelleneingaben.
author: tfitzmac
ms.topic: conceptual
ms.date: 08/24/2020
ms.author: tomfitz
ms.openlocfilehash: 04f86883a75110985d1cbe050fe3fd3e0582986a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88893629"
---
# <a name="microsoftcommoneditablegrid-ui-element"></a>Benutzeroberflächenelement Microsoft.Common.EditableGrid

Ein Steuerelement zum Sammeln von Tabelleneingaben. Alle Felder innerhalb des Rasters können bearbeitet werden, und die Anzahl der Zeilen kann variieren.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel

:::image type="content" source="./media/managed-application-elements/microsoft-common-editablegrid.png" alt-text="Microsoft.Common.EditableGrid":::

## <a name="schema"></a>Schema

```json
{
  "name": "people",
  "type": "Microsoft.Common.EditableGrid",
  "ariaLabel": "Enter information per person",
  "label": "People",
  "constraints": {
    "width": "Full",
    "rows": {
      "count": {
        "min": 1,
        "max": 10
      }
    },
    "columns": [
      {
        "id": "colName",
        "header": "Name",
        "width": "1fr",
        "element": {
          "type": "Microsoft.Common.TextBox",
          "placeholder": "Full name",
          "constraints": {
            "required": true,
            "validations": [
              {
                "isValid": "[startsWith(last(take(steps('grid').people, $rowIndex)).colName, 'contoso')]",
                "message": "Must start with 'contoso'."
              },
              {
                "regex": "^[a-z0-9A-Z]{1,30}$",
                "message": "Only alphanumeric characters are allowed, and the value must be 1-30 characters long."
              }
            ]
          }
        }
      },
      {
        "id": "colGender",
        "header": "Gender",
        "width": "1fr",
        "element": {
          "name": "dropDown1",
          "type": "Microsoft.Common.DropDown",
          "placeholder": "Select a gender...",
          "constraints": {
            "allowedValues": [
              {
                "label": "Female",
                "value": "female"
              },
              {
                "label": "Male",
                "value": "male"
              },
              {
                "label": "Other",
                "value": "other"
              }
            ],
            "required": true
          }
        }
      },
      {
        "id": "colContactPreference",
        "header": "Contact preference",
        "width": "1fr",
        "element": {
          "type": "Microsoft.Common.OptionsGroup",
          "constraints": {
            "allowedValues": [
              {
                "label": "Email",
                "value": "email"
              },
              {
                "label": "Text",
                "value": "text"
              }
            ],
            "required": true
          }
        }
      }
    ]
  }
}
```

## <a name="sample-output"></a>Beispielausgabe

```json
{
  "colName": "contoso",
  "colGender": "female",
  "colContactPreference": "email"
}
```

## <a name="remarks"></a>Bemerkungen

- Die einzigen gültigen Steuerelemente innerhalb des Spaltenarrays sind [TextBox](microsoft-common-textbox.md), [OptionsGroup](microsoft-common-optionsgroup.md) und [DropDown](microsoft-common-dropdown.md).
- Die Variable `$rowIndex` ist nur in Ausdrücken gültig, die in den untergeordneten Elementen der Rasterspalten enthalten sind. Dabei handelt es sich um einen Integer, der den relativen Zeilenindex des Elements darstellt. Die Zählung beginnt bei eins und wird jeweils um eins erhöht. Wie im Abschnitt `"columns":` des Schemas gezeigt, wird der `$rowIndex` für die Validierung verwendet.
- Wenn Überprüfungen mit der Variable `$rowIndex` ausgeführt werden, ist es möglich, den Wert der aktuellen Zeile durch Kombinieren der Befehle `last()` und `take()` abzurufen.

  Beispiel:

  `last(take(<reference_to_grid>, $rowIndex))`

- Die `label`-Eigenschaft wird nicht als Teil des Steuerelements angezeigt, sondern auf der abschließenden Registerkartenzusammenfassung.
- Die `ariaLabel`-Eigenschaft ist die Barrierefreiheitsbezeichnung für das Raster. Geben Sie einen hilfreichen Text für Benutzer an, die die Sprachausgabe verwenden.
- Mit der `constraints.width`-Eigenschaft wird die Gesamtbreite des Rasters festgelegt. Die Optionen lauten _Full_, _Medium_ und _Small_. Der Standardwert ist _Full_.
- Die `width`-Eigenschaft in untergeordneten Elementen von Spalten legt die Spaltenbreite fest. Die Breite wird mithilfe von Bruchteileinheiten festgelegt (z. B. _3fr_). Die gesamte verfügbare Breite wird proportional gemäß den Einheiten auf die Spalten aufgeteilt. Wenn keine Spaltenbreite angegeben ist, wird der Standardwert _1fr_ verwendet.

## <a name="next-steps"></a>Nächste Schritte

- Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
- Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
