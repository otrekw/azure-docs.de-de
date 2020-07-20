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
ms.openlocfilehash: a0d1de622eefad4ae5e55a427f8b0b1bf4360c0a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84792087"
---
Sie können die REST-API [Get Endpoint Health](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth) (Endpunktintegrität abrufen) verwenden, um den Integritätsstatus der Endpunkte abzurufen. Wir empfehlen Ihnen die Verwendung von [IoT Hub-Metriken](../articles/iot-hub/iot-hub-metrics.md) zur Latenz bei der Nachrichtenweiterleitung, um Fehler zu identifizieren und zu debuggen, wenn die Integrität des Endpunkts „Inaktiv“ oder „Fehlerhaft“ lautet. Es ist zu erwarten, dass die Latenz höher ist, wenn der Endpunkt einen dieser Status aufweist.


|Integritätsstatus|Beschreibung|
|---|---|
|Fehlerfrei|Der Endpunkt akzeptiert Nachrichten erwartungsgemäß.|
|Fehlerhaft|Vom Endpunkt werden keine Nachrichten akzeptiert, und von IoT Hub wird versucht, Nachrichten erneut an diesen Endpunkt zu senden.|
|Unbekannt|Von IoT Hub wurde nicht versucht, Nachrichten an diesen Endpunkt zu übermitteln.|
|Beeinträchtigt|Vom Endpunkt werden Nachrichten langsamer als erwartet akzeptiert, oder der Endpunkt wird wiederhergestellt, nachdem er sich in einem fehlerhaften Zustand befand.|
|Inaktiv|Von IoT Hub werden keine Nachrichten mehr an diesen Endpunkt übermittelt. Das erneute Senden von Nachrichten an diesen Endpunkt war nicht erfolgreich.|
