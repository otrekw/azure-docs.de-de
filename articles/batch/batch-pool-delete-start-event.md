---
title: 'Azure Batch: Ereignis zum Starten der Löschung von Pools'
description: Referenz zum Batch-Ereignis zum Starten des Löschvorgangs von Pools. Dieses Ereignis wird ausgegeben, wenn ein Löschvorgang eines Pools gestartet wurde.
ms.topic: reference
ms.date: 12/28/2020
ms.openlocfilehash: 86f6eb8e7b269cb45f692398e9e60390375ca073
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97803747"
---
# <a name="pool-delete-start-event"></a>Ereignis zum Starten des Löschvorgangs von Pools

 Dieses Ereignis wird ausgegeben, wenn ein Löschvorgang eines Pools gestartet wurde. Da das Löschen des Pools ein asynchrones Ereignis ist, können Sie damit rechnen, dass ein Ereignis zum Abschluss des Löschvorgangs von Pools ausgegeben wird, sobald der Löschvorgang abgeschlossen ist.

 Das folgende Beispiel zeigt den Text eines Ereignisses zum Starten des Löschvorgangs von Pools.

```
{
   "id": "myPool1"
}
```

|Element|type|Notizen|
|-------------|----------|-----------|
|`id`|String|Die ID des Pools.|
