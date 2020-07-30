---
title: Benutzeroberflächenelement „Section“
description: Hier wird das Benutzeroberflächenelement „Microsoft.Common.Section“ für das Azure-Portal beschrieben. Es wird zum Gruppieren von Elementen im Portal für die Bereitstellung verwalteter Anwendungen verwendet.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: 924aff8f2ba3d796b65f52494845f3b10018065c
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87063978"
---
# <a name="microsoftcommonsection-ui-element"></a>Benutzerobeflächenelement „Microsoft.Common.Section“

Ein Steuerelement, das Elemente unter einer Überschrift zusammenfasst.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel

![Microsoft.Common.Section](./media/managed-application-elements/microsoft-common-section.png)

## <a name="schema"></a>Schema

```json
{
  "name": "section1",
  "type": "Microsoft.Common.Section",
  "label": "Example section",
  "elements": [
    {
      "name": "text1",
      "type": "Microsoft.Common.TextBox",
      "label": "Example text box 1"
    },
    {
      "name": "text2",
      "type": "Microsoft.Common.TextBox",
      "label": "Example text box 2"
    }
  ],
  "visible": true
}
```

## <a name="remarks"></a>Bemerkungen

- `elements` muss mindestens ein Element und darf alle Elementtypen außer `Microsoft.Common.Section` enthalten.
- Dieses Element unterstützt die `toolTip`-Eigenschaft nicht.

## <a name="sample-output"></a>Beispielausgabe
Verwenden Sie zum Zugreifen auf die Ausgabewerte von Elementen in `elements` die Funktion [basics()](create-ui-definition-referencing-functions.md#basics) oder [steps()](create-ui-definition-referencing-functions.md#steps) und die Punktnotation:

```json
steps('configuration').section1.text1
```

Elemente des Typs `Microsoft.Common.Section` haben selbst keine Ausgabewerte.

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
