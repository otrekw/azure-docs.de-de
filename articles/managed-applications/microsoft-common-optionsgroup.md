---
title: Benutzeroberflächenelement „OptionsGroup“ in Azure | Microsoft-Dokumentation
description: Hier wird das Benutzeroberflächenelement „Microsoft.Common.OptionsGroup“ für das Azure-Portal beschrieben. Es ermöglicht Benutzern die Auswahl aus verfügbaren Optionen beim Bereitstellen einer verwalteten Anwendung.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: 1210c24687c0cd1f38e33674d297dd37fe4d2995
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2019
ms.locfileid: "74151883"
---
# <a name="microsoftcommonoptionsgroup-ui-element"></a>Benutzeroberflächenelement „Microsoft.Common.OptionsGroup“

Ein Auswahlsteuerelement mit einer Zeile der verfügbaren Optionen.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel

![Microsoft.Common.OptionsGroup](./media/managed-application-elements/microsoft.common.optionsgroup.png)

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

## <a name="remarks"></a>Anmerkungen

- Die Bezeichnung für `constraints.allowedValues` ist der Anzeigetext für ein Element, und sein Wert ist der Ausgabewert des Elements bei der Auswahl.
- Wenn ein Wert angegeben wird, muss der Standardwert einer in `constraints.allowedValues` vorhandenen Bezeichnung entsprechen. Wird kein Wert angegeben, wird standardmäßig das erste Element in `constraints.allowedValues` ausgewählt. Der Standardwert lautet **null**.
- `constraints.allowedValues` muss mindestens ein Element aufweisen.

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
