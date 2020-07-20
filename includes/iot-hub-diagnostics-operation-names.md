---
title: include file
description: include file
services: iot-hub
author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 04/28/2020
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 362c13771e7382ead1ba5aebd99a69fd86cd718c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84793271"
---
<!-- operation names for the diag logs for IoT Hub -->

|Vorgangsname|Ebene|Beschreibung|
|------------- |-----|-----------|
|UndefinedRouteEvaluation|Information|Die Nachricht kann mit einer angegebenen Bedingung nicht ausgewertet werden. Dieser Fall kann beispielsweise eintreten, wenn eine Eigenschaft aus der Routenabfrage in der Nachricht fehlt. Weitere Informationen zur Abfragesyntax für das IoT Hub-Nachrichtenrouting finden Sie [hier](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-routing-query-syntax).|
|RouteEvaluationError|Fehler|Beim Auswerten der Nachricht ist ein Fehler aufgrund eines Problems mit dem Nachrichtenformat aufgetreten. Dieser Fehler wird beispielsweise protokolliert, wenn die Inhaltscodierung nicht angegeben wurde oder der Inhaltstyp in der Nachricht nicht gültig ist. Diese müssen in den [Systemeigenschaften](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-routing-query-syntax#system-properties) festgelegt werden.|
|DroppedMessage|Fehler|Die Nachricht wurde gelöscht und nicht weitergeleitet. Dies kann beispielsweise darauf zurückzuführen sein, dass die Nachricht keiner Routingabfrage entsprach oder dass der Endpunkt inaktiv war und die Nachricht auch nach mehreren Versuchen nicht zugestellt werden konnte. Es empfiehlt sich, mithilfe der REST-API [get endpoint health](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth) weitere Details zum Endpunkt abzurufen.|
|EndpointUnhealthy|Fehler|Der Endpunkt hat Nachrichten von IoT Hub akzeptiert, und von IoT Hub wird versucht, die Nachrichten erneut zu senden. Es empfiehlt sich, mithilfe der REST-API [get endpoint health](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth) den letzten bekannten Fehler zu beobachten.|
|EndpointDead|Fehler|Der Endpunkt hat über eine Stunde lang keine Nachrichten von IoT Hub akzeptiert. Es empfiehlt sich, mithilfe der REST-API [get endpoint health](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth) den letzten bekannten Fehler zu beobachten.|
|EndpointHealthy|Information|Der Endpunkt ist fehlerfrei und empfängt Nachrichten von IoT Hub. Diese Meldung wird nicht fortlaufend protokolliert, sondern nur, wenn der Endpunkt wieder fehlerfrei wird. Diese Meldung bedeutet, dass von IoT Hub keine Nachrichten an den Endpunkt gesendet werden konnten, der Endpunkt nun aber wieder fehlerfrei ist.|
|OrphanedMessage|Information|Die Nachricht entspricht keiner Route.|
|InvalidMessage|Fehler|Die Nachricht ist nicht mit dem Endpunkt kompatibel und daher ungültig. Es empfiehlt sich, die Konfigurationen des Endpunkts zu überprüfen.|


Die Vorgänge *UndefinedRouteEvaluation*, *RouteEvaluationError* und *OrphanedMessage* werden gedrosselt und maximal einmal pro Minute und IoT Hub protokolliert.