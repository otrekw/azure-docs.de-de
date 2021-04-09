---
title: 'Azure Batch: Ereignis zum Abschluss der Löschung von Pools'
description: Referenz zum Batch-Ereignis zum Abschluss des Löschvorgangs von Pools. Dieses Ereignis wird ausgegeben, wenn ein Löschvorgang eines Pools abgeschlossen wurde.
ms.topic: reference
ms.date: 12/28/2020
ms.openlocfilehash: be6411a150ae6be424c0621eed768157154c7408
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97803730"
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

Weitere Informationen zu Status und Fehlercodes für den Löschvorgang des Pools finden Sie unter [Löschen eines Pools aus einem Konto](/rest/api/batchservice/delete-a-pool-from-an-account).
