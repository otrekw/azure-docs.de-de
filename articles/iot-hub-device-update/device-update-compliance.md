---
title: Grundlegendes zur Device Update for Azure IoT Hub-Compliance | Microsoft-Dokumentation
description: Erfahren Sie, wie Device Update for Azure IoT Hub die Konformität von Geräteupdates misst.
author: vimeht
ms.author: vimeht
ms.date: 2/11/2021
ms.topic: conceptual
ms.service: iot-hub-device-update
ms.openlocfilehash: ac6094efde8a32b1fcc04c55bbc537afeb4166f7
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101660685"
---
# <a name="device-update-compliance"></a>Device Update-Konformität

In Device Update for IoT Hub misst die Konformität, wie viele Geräte das höchste versionskompatible Update installiert haben. Ein Gerät ist konform, wenn es das verfügbare Update mit der höchsten Version installiert hat. 

Stellen Sie sich z. B. eine Instanz von Device Update mit den folgenden Updates vor:

|Updatename|Updateversion|Kompatibles Gerätemodell|
|-----------|--------------|-----------------------|
|Update1    |1.0    |Model1|
|Update2    |1.0    |Model2|
|Update3    |2.0    |Model1|

Nehmen wir an, die folgenden Bereitstellungen wurden erstellt:

|Bereitstellungsname    |Updatename    |Zielgruppe|
|-----------|--------------|-------------------|
|Deployment1    |Update1    |Gruppe1|
|Deployment2    |Update2    |Gruppe2|
|Deployment3    |Update3    |Group3|

Betrachten Sie nun die folgenden Geräte mit ihren Gruppenmitgliedschaften und installierten Versionen:

|deviceId   |Gerätemodell   |Installierte Updateversion|Group |Kompatibilität|
|-----------|--------------|-----------------------|-----|---------|
|Device1    |Model1 |1.0    |Gruppe1 |Neue Updates verfügbar</span>|
|Device2    |Model1 |2.0    |Group3 |Aktuellstes Update|
|Device3    |Model2 |1.0    |Gruppe2 |Aktuellstes Update|
|Device4    |Model1 |1.0    |Group3 |Update wird ausgeführt.|

Device1 und Device4 sind nicht konform, weil sie Version 1.0 installiert haben, obwohl in der Device Update-Instanz ein Update mit einer höheren Version (Update3) für ihr Modell kompatibel ist. Device2 und Device3 sind beide konform, weil sie die höchsten für ihre Modelle kompatiblen Versionsupdates installiert haben.

Konformität berücksichtigt nicht, ob ein Update für die Gruppe eines Geräts bereitgestellt wird. Alle Updates, die für Device Update veröffentlicht werden, werden untersucht. Im Beispiel oben wird also Device1 als nicht konform betrachtet, obwohl es das Update installiert hat. Device1 wird weiterhin als nicht konform eingestuft, bis Update3 erfolgreich installiert wurde. Mit dem Konformitätsstatus können Sie ermitteln, ob neue Bereitstellungen erforderlich sind. 

Wie oben gezeigt, gibt es drei Kompatibilitätszustände in Device Update for IoT Hub:

*   **Aktuellstes Update**: Das Gerät hat das höchste versionskompatible Update installiert, das in Device Update veröffentlicht wurde.
*   **Update wird ausgeführt**: Eine aktive Bereitstellung ist aktuell damit beschäftigt, das höchste versionskompatible Update an das Gerät zu übermitteln.
*   **Neue Updates verfügbar**: Ein Gerät hat noch nicht das höchste versionskompatible Update installiert und befindet sich nicht in einer aktiven Bereitstellung für dieses Update.
