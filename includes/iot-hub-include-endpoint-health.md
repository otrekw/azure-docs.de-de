---
title: include file
description: include file
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/28/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 36c05badb3b2292a29b8227c7f03b841474c97ad
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2020
ms.locfileid: "92548269"
---
Sie können die REST-API [Get Endpoint Health](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth) (Endpunktintegrität abrufen) verwenden, um den Integritätsstatus der Endpunkte abzurufen. Die Verwendung von [IoT Hub-Routingmetriken](../articles/iot-hub/monitor-iot-hub-reference.md#routing-metrics) bezüglich der Latenz bei der Nachrichtenweiterleitung wird empfohlen, um Fehler zu identifizieren und zu debuggen, wenn die Integrität des Endpunkts „Inaktiv“ oder „Fehlerhaft“ lautet. Es ist zu erwarten, dass die Latenz höher ist, wenn der Endpunkt einen dieser Status aufweist. Weitere Informationen zur Verwendung von IoT Hub-Metriken finden Sie unter [Überwachen von IoT Hub](../articles/iot-hub/monitor-iot-hub.md).

|Integritätsstatus|Beschreibung|
|---|---|
|healthy|Der Endpunkt akzeptiert Nachrichten erwartungsgemäß.|
|Fehlerhaft|Vom Endpunkt werden keine Nachrichten akzeptiert, und von IoT Hub wird versucht, Nachrichten erneut an diesen Endpunkt zu senden.|
|Unbekannt|Von IoT Hub wurde nicht versucht, Nachrichten an diesen Endpunkt zu übermitteln.|
|Beeinträchtigt|Vom Endpunkt werden Nachrichten langsamer als erwartet akzeptiert, oder der Endpunkt wird wiederhergestellt, nachdem er sich in einem fehlerhaften Zustand befand.|
|Inaktiv|Von IoT Hub werden keine Nachrichten mehr an diesen Endpunkt übermittelt. Das erneute Senden von Nachrichten an diesen Endpunkt war nicht erfolgreich.|
