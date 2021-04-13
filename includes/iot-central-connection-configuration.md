---
title: include file
description: include file
services: iot-central
author: dominicbetts
ms.service: iot-central
ms.topic: include
ms.date: 11/03/2020
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: e4f9fd537d7743a5bbb9d129b21c4bf0a529d32d
ms.sourcegitcommit: bfa7d6ac93afe5f039d68c0ac389f06257223b42
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106491162"
---
Wenn Sie die Beispielgeräteanwendung später in diesem Tutorial ausführen, benötigen Sie die folgenden Konfigurationswerte:

* ID-Bereich: Navigieren Sie in Ihrer IoT Central-Anwendung zu **Verwaltung > Geräteverbindung**. Notieren Sie sich den Wert für **ID-Bereich**.
* Gruppenprimärschlüssel: Navigieren Sie in Ihrer IoT Central-Anwendung zu **Verwaltung > Geräteverbindung > SAS-IoT-Devices**. Notieren Sie sich den Wert für den SAS-**Primärschlüssel** (Shared Access Signature).

Verwenden Sie Cloud Shell, um einen Geräteschlüssel auf der Grundlage des von Ihnen abgerufenen Gruppenprimärschlüssels zu generieren:

```azurecli-interactive
az extension add --name azure-iot
az iot central device compute-device-key --device-id sample-device-01 --pk <the group primary key value>
```

Notieren Sie sich den generierten Geräteschlüssel. Sie verwenden ihn später in diesem Tutorial.
