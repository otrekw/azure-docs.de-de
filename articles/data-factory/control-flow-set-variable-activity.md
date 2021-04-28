---
title: Aktivität „Variable festlegen“ in Azure Data Factory
description: Erfahren Sie, wie Sie die Aktivität „Variable festlegen“ zum Festlegen des Werts einer vorhandenen Variablen verwenden, die in einer Data Factory-Pipeline definiert ist.
ms.service: data-factory
ms.topic: conceptual
ms.date: 04/07/2020
author: chez-charlie
ms.author: chez
ms.reviewer: jburchel
ms.openlocfilehash: 5110160234131bd105ac860efe818506db48f6b0
ms.sourcegitcommit: b4032c9266effb0bf7eb87379f011c36d7340c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107903918"
---
# <a name="set-variable-activity-in-azure-data-factory"></a>Aktivität „Variable festlegen“ in Azure Data Factory
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Verwenden Sie die Aktivität „Variable festlegen“, um den Wert einer vorhandenen Variablen vom Typ „String“, „Bool“ oder „Array“ festzulegen, die in einer Data Factory-Pipeline definiert ist.

## <a name="type-properties"></a>Typeigenschaften

Eigenschaft | BESCHREIBUNG | Erforderlich
-------- | ----------- | --------
name | Der Name der Aktivität in der Pipeline. | ja
description | Text, der beschreibt, welche Aktion die Aktivität ausführt. | nein
type | Muss auf **SetVariable** festgelegt werden. | ja
value | Der Zeichenfolgenliteral- oder Ausdrucksobjektwert, dem die Variable zugewiesen wird. | ja
variableName | Der Name der Variablen, die durch diese Aktivität festgelegt wird. | ja

## <a name="incrementing-a-variable"></a>Schrittweises Erhöhen einer Variablen

Ein häufiges Szenario, bei dem Variablen in Azure Data Factory einbezogen werden, ist die Verwendung einer Variablen als Iterator innerhalb einer „until“- oder „foreach“-Aktivität. In einer Aktivität „Variable festlegen“ können Sie nicht auf die Variable verweisen, die im Feld `value` festgelegt wird. Legen Sie zur Umgehung dieser Einschränkung eine temporäre Variable fest, und erstellen Sie dann eine zweite Aktivität „Variable festlegen“. Die zweite Aktivität „Variable festlegen“ legt den Wert des Iterators auf die temporäre Variable fest. 

Nachstehend sehen Sie ein Beispiel für dieses Muster:

![Variable schrittweise erhöhen](media/control-flow-set-variable-activity/increment-variable.png "Erhöhen eines Variablenwerts")

``` json
{
    "name": "pipeline3",
    "properties": {
        "activities": [
            {
                "name": "Set I",
                "type": "SetVariable",
                "dependsOn": [
                    {
                        "activity": "Increment J",
                        "dependencyConditions": [
                            "Succeeded"
                        ]
                    }
                ],
                "userProperties": [],
                "typeProperties": {
                    "variableName": "i",
                    "value": {
                        "value": "@variables('j')",
                        "type": "Expression"
                    }
                }
            },
            {
                "name": "Increment J",
                "type": "SetVariable",
                "dependsOn": [],
                "userProperties": [],
                "typeProperties": {
                    "variableName": "j",
                    "value": {
                        "value": "@string(add(int(variables('i')), 1))",
                        "type": "Expression"
                    }
                }
            }
        ],
        "variables": {
            "i": {
                "type": "String",
                "defaultValue": "0"
            },
            "j": {
                "type": "String",
                "defaultValue": "0"
            }
        },
        "annotations": []
    }
}
```


## <a name="next-steps"></a>Nächste Schritte
Machen Sie sich mit einer verwandten Ablaufsteuerungsaktivität vertraut, die von Data Factory unterstützt wird: 

- [Aktivität „Variable anfügen“](control-flow-append-variable-activity.md)
