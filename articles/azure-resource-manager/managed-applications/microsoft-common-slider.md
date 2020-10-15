---
title: Schieberegler-Benutzeroberflächenelement
description: Hier wird das Benutzeroberflächenelement „Microsoft.Common.Slider“ für das Azure-Portal beschrieben. Ermöglicht Benutzern das Festlegen eines Werts aus einem Bereich von Optionen.
author: tfitzmac
ms.topic: conceptual
ms.date: 07/10/2020
ms.author: tomfitz
ms.openlocfilehash: bb168a4bff6d2570f539307530232b5063bb535c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87094800"
---
# <a name="microsoftcommonslider-ui-element"></a>Microsoft.Common.Slider-Benutzeroberflächenelement

Das Schieberegler-Steuerelement ermöglicht Benutzern die Auswahl aus einem Bereich zulässiger Werte.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel

:::image type="content" source="./media/managed-application-elements/microsoft-common-slider.png" alt-text="Microsoft.Common.Slider":::

## <a name="schema"></a>Schema

```json
{
    "name": "memorySize",
    "type": "Microsoft.Common.Slider",
    "min": 1,
    "max": 64,
    "label": "Memory",
    "subLabel": "MB",
    "defaultValue": 24,
    "showStepMarkers": false,
    "toolTip": "Pick the size in MB",
    "constraints": {
        "required": false
    },
    "visible": true
}
```

## <a name="sample-output"></a>Beispielausgabe

```json
26
```

## <a name="remarks"></a>Bemerkungen

- Die Werte `min` und `max` sind erforderlich. Mit ihnen werden der Start- und der Endpunkt des Schiebereglers festgelegt.
- Die Eigenschaft `showStepMarkers` hat den Standardwert „true“. Die Schrittmarkierungen werden nur angezeigt, wenn der Bereich von Min. bis Max. 100 oder kleiner ist.


## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
