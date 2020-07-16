---
title: V3 API-Abfragezeichenfolge-Parameter
ms.topic: include
ms.date: 06/30/2020
ms.openlocfilehash: 47727f6df772669fa323a10cd099764a6d35cb6a
ms.sourcegitcommit: 32592ba24c93aa9249f9bd1193ff157235f66d7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2020
ms.locfileid: "85610783"
---
Zu den V3 API-Abfragezeichenfolge-Parametern zählen:

|Query parameter (Abfrageparameter)|LUIS-Portalname|type|Version|Standard|Zweck|
|--|--|--|--|--|--|
|`log`|Protokolle speichern|boolean|V2 und V3|false|Speichern Sie die Abfrage in der Protokolldatei. Der Standardwert ist „false“.|
|`query`|-|Zeichenfolge|Nur V3|Kein Standardwert: in GET-Anforderung erforderlich|**In V2** enthält der `q`-Parameter die vorherzusagende Äußerung. <br><br>**In V3** wird der `query`-Parameter verwendet, um anzugeben, dass dieses Feature verwendet werden soll.|
|`show-all-intents`|Ergebnisse für alle Absichten einbeziehen|boolean|Nur V3|false|Alle Absichten mit der entsprechenden Bewertung werden innerhalb des **prediction.intents**-Objekts zurückgegeben. Absichten werden als Objekte in einem übergeordneten `intents`-Objekt zurückgegeben. `prediction.intents.give` ermöglicht den programmgesteuerten Zugriff, ohne die Absicht im Array suchen zu müssen. In V2 werden diese Absichten in einem Array zurückgegeben. |
|`verbose`|Mehr Entitätendetails einbeziehen|boolean|V2 und V3|false|Wenn **in V2** TRUE festgelegt wird, werden alle vorhergesagten Absichten zurückgegeben. Wenn Sie alle vorhergesagten Absichten abrufen müssen, verwenden Sie den V3-Parameter von `show-all-intents`.<br><br>**In V3** stellt dieser Parameter nur Details zu Entitätsmetadaten einer Entitätsvorhersage bereit.  |
|`timezoneOffset`|-|Zeichenfolge|V2|-|Zeitzone angewendet auf datetimeV2-Entitäten.|
|`datetimeReference`|-|Zeichenfolge|V3|-|[Zeitzone](../luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) angewendet auf datetimeV2-Entitäten. Ersetzt `timezoneOffset` aus V2.|