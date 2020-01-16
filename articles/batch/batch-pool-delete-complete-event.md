---
title: 'Azure Batch: Ereignis zum Abschluss des Löschvorgangs von Pools | Microsoft-Dokumentation'
description: Referenz zum Batch-Ereignis zum Abschluss des Löschvorgangs von Pools.
services: batch
author: ju-shim
manager: gwallace
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: jushiman
ms.openlocfilehash: bcbfe009e2880c0155a067305cc28317678cbfa6
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "76026695"
---
# <a name="pool-delete-complete-event"></a>Ereignis zum Abschluss des Löschvorgangs von Pools

 Dieses Ereignis wird ausgegeben, wenn ein Löschvorgang eines Pools abgeschlossen wurde.

 Das folgende Beispiel zeigt den Text eines Ereignisses zum Abschluss des Löschvorgangs von Pools.

```
{
    "id": "myPool1",
    "startTime": "2016-09-09T22:13:48.579Z",
    "endTime": "2016-09-09T22:14:08.836Z"
}
```

|Element|type|Notizen|
|-------------|----------|-----------|
|`id`|String|Die ID des Pools.|
|`startTime`|Datetime|Die Startzeit des Löschvorgangs des Pools.|
|`endTime`|Datetime|Die Endzeit des Löschvorgangs des Pools.|

## <a name="remarks"></a>Bemerkungen
Weitere Informationen zu Status und Fehlercodes für den Löschvorgang des Pools finden Sie unter [Löschen eines Pools aus einem Konto](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).
