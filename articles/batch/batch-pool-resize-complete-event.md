---
title: 'Azure Batch: Ereignis zum Abschluss der Größenänderung von Pools'
description: Referenz zum Batch-Ereignis zum Abschluss der Größenänderung von Pools. Sehen Sie sich ein Beispiel für einen Pool an, der vergrößert und erfolgreich abgeschlossen wurde.
services: batch
author: LauraBrenner
manager: evansma
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: labrenne
ms.openlocfilehash: e2c66471ad9fe8d917d1ffddceb6e01c339d62dd
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77022221"
---
# <a name="pool-resize-complete-event"></a>Ereignis zum Abschluss der Größenänderung von Pools

 Dieses Ereignis wird ausgegeben, wenn die Größenänderung eines Pools abgeschlossen wurde oder fehlgeschlagen ist.

 Das folgende Beispiel zeigt den Text eines Ereignisses zum Abschluss der Größenänderung von Pools für einen Pool, dessen Größe erfolgreich erhöht wurde.

```
{
    "id": "myPool",
    "nodeDeallocationOption": "invalid",
        "currentDedicatedNodes": 10,
        "targetDedicatedNodes": 10,
    "currentLowPriorityNodes": 5,
        "targetLowPriorityNodes": 5,
    "enableAutoScale": false,
    "isAutoPool": false,
    "startTime": "2016-09-09T22:13:06.573Z",
    "endTime": "2016-09-09T22:14:01.727Z",
    "resultCode": "Success",
    "resultMessage": "The operation succeeded"
}
```

|Element|type|Notizen|
|-------------|----------|-----------|
|`id`|String|Die ID des Pools.|
|`nodeDeallocationOption`|String|Gibt ab, ob Knoten ggf. aus dem Pool entfernt werden müssen, wenn sich die Poolgröße verringert.<br /><br /> Mögliche Werte:<br /><br /> **requeue**: Beendet ausgeführte Tasks, die erneut in die Warteschlange gestellt werden. Die Tasks werden erneut ausgeführt, sobald der Auftrag aktiviert ist. Entfernen Sie Knoten, sobald Tasks abgeschlossen wurden.<br /><br /> **terminate**: Beendet derzeit ausgeführte Tasks. Die Tasks werden nicht erneut ausgeführt. Entfernen Sie Knoten, sobald Tasks abgeschlossen wurden.<br /><br /> **taskcompletion**: Lässt das Abschließen aktuell ausgeführter Tasks zu. Planen Sie beim Warten keine neuen Tasks. Entfernen Sie Knoten, sobald alle Aufgaben abgeschlossen sind.<br /><br /> **Retaineddata**: Lässt das Abschließen aktuell ausgeführter Tasks zu und wartet dann, bis die Aufbewahrungszeiträume aller Taskdaten abgelaufen sind. Planen Sie beim Warten keine neuen Tasks. Entfernen Sie Knoten, sobald die Aufbewahrungszeiträume aller Tasks abgelaufen sind.<br /><br /> Der Standardwert ist „requeue“.<br /><br /> Wenn die Poolgröße zunimmt, wird der Wert auf **invalid** festgelegt.|
|`currentDedicatedNodes`|Int32|Die Anzahl der dedizierten Computeknoten, die dem Pool derzeit zugewiesen sind.|
|`targetDedicatedNodes`|Int32|Die Anzahl der dedizierten Computeknoten, die für den Pool angefordert werden.|
|`currentLowPriorityNodes`|Int32|Die Anzahl der Computeknoten mit niedriger Priorität, die dem Pool derzeit zugewiesen sind.|
|`targetLowPriorityNodes`|Int32|Die Anzahl der Computeknoten mit niedriger Priorität, die für den Pool angefordert werden.|
|`enableAutoScale`|Bool|Gibt an, ob die Poolgröße mit der Zeit automatisch angepasst wird.|
|`isAutoPool`|Bool|Gibt an, ob der Pool über den AutoPool-Mechanismus eines Auftrags erstellt wurde.|
|`startTime`|Datetime|Die Startzeit der Größenänderung des Pools.|
|`endTime`|Datetime|Die Uhrzeit des Abschlusses der Größenänderung des Pools.|
|`resultCode`|String|Das Ergebnis der Größenänderung.|
|`resultMessage`|String| Eine ausführliche Nachricht zum Ergebnis.<br /><br /> Wenn die Größenänderung erfolgreich abgeschlossen wurde, wird angegeben, dass der Vorgang erfolgreich war.|
