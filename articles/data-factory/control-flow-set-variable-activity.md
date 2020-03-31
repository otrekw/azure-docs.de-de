---
title: Aktivität „Variable festlegen“ in Azure Data Factory
description: Erfahren Sie, wie Sie die Aktivität „Variable festlegen“ zum Festlegen des Werts einer vorhandenen Variablen verwenden, die in einer Data Factory-Pipeline definiert ist.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/10/2018
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.openlocfilehash: 88500ecbc56b34551a0cbd3ca94727ba4bbcda9f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "74930643"
---
# <a name="set-variable-activity-in-azure-data-factory"></a>Aktivität „Variable festlegen“ in Azure Data Factory

Verwenden Sie die Aktivität „Variable festlegen“, um den Wert einer vorhandenen Variablen vom Typ „String“, „Bool“ oder „Array“ festzulegen, die in einer Data Factory-Pipeline definiert ist.

## <a name="type-properties"></a>Typeigenschaften

Eigenschaft | BESCHREIBUNG | Erforderlich
-------- | ----------- | --------
name | Der Name der Aktivität in der Pipeline. | Ja
description | Text, der beschreibt, welche Aktion die Aktivität ausführt. | nein
type | Der Aktivitätstyp ist SetVariable. | ja
value | Ein Zeichenfolgenliteral- oder Ausdrucksobjektwert, der zum Festlegen der angegebenen Variable verwendet wird. | ja
variableName | Der Name der Variablen, die von dieser Aktivität festgelegt wird. | ja


## <a name="next-steps"></a>Nächste Schritte
Machen Sie sich mit einer verwandten Ablaufsteuerungsaktivität vertraut, die von Data Factory unterstützt wird: 

- [Aktivität „Variable anfügen“](control-flow-append-variable-activity.md)
