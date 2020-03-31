---
title: Aktivität „Variable anfügen“ in Azure Data Factory
description: Erfahren Sie, wie Sie die Aktivität „Variable anfügen“ so festlegen, dass ein Wert einer vorhandenen Arrayvariablen hinzugefügt wird, die in einer Data Factory-Pipeline definiert ist.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.date: 10/09/2018
ms.openlocfilehash: bcecd3411b6e6e751032f33862a671d8adbca046
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "73679973"
---
# <a name="append-variable-activity-in-azure-data-factory"></a>Aktivität „Variable anfügen“ in Azure Data Factory

Verwenden Sie die Aktivität „Variable anfügen“ zum Hinzufügen eines Werts zu einer vorhandenen Arrayvariablen, die in einer Data Factory-Pipeline definiert ist.

## <a name="type-properties"></a>Typeigenschaften

Eigenschaft | BESCHREIBUNG | Erforderlich
-------- | ----------- | --------
name | Der Name der Aktivität in der Pipeline. | Ja
description | Text, der beschreibt, welche Aktion die Aktivität ausführt. | nein
type | Der Aktivitätstyp ist AppendVariable. | ja
value | Ein Zeichenfolgenliteral- oder Ausdrucksobjektwert, der zum Anfügen an die angegebene Variable verwendet wird. | ja
variableName | Der Name der Variablen, die von der Aktivität geändert wird. Die Variable muss vom Typ „Array“ sein. | ja

## <a name="next-steps"></a>Nächste Schritte
Machen Sie sich mit einer verwandten Ablaufsteuerungsaktivität vertraut, die von Data Factory unterstützt wird: 

- [Aktivität „Variable festlegen“](control-flow-set-variable-activity.md)
