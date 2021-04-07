---
title: include file
description: include file
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 03/15/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: b20c97bf919073b01f564b59b3a55eccb7614005
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "89043008"
---
<!-- This is the note explaining about the avro and json formats when routing to blob storage. -->
> [!NOTE]
> Die Daten können entweder im [Apache Avro](https://avro.apache.org/)-Format (Standardeinstellung) oder im JSON-Format (Vorschauversion) in den Blobspeicher geschrieben werden. 
>    
> Die Funktion zum Codieren im JSON-Format befindet sich in allen Regionen, in denen IoT Hub verfügbar ist, in der Vorschauphase (mit Ausnahme von „USA, Osten“ und „Europa, Westen“). Das Codierungsformat kann nur beim Konfigurieren des Blob Storage-Endpunkt festgelegt werden. Für bereits eingerichtete Endpunkte kann das Format nicht mehr geändert werden. Bei Verwendung der JSON-Codierung müssen Sie in den Nachrichtensystemeigenschaften den Inhaltstyp (contentType) auf „JSON“ und die Inhaltscodierung (contentEncoding) auf „UTF-8“ festlegen. 
>
> Ausführlichere Informationen zur Verwendung eines Blob Storage-Endpunkts finden Sie unter [Leitfaden zum Weiterleiten an den Speicher](../articles/iot-hub/iot-hub-devguide-messages-d2c.md#azure-storage-as-a-routing-endpoint).
>