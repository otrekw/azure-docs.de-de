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
ms.openlocfilehash: b566fc5c8d490d9fbad8ba64217d9ab3110cd709
ms.sourcegitcommit: fc9fd6e72297de6e87c9cf0d58edd632a8fb2552
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108313516"
---
> [!NOTE]
> Die Daten können entweder im [Apache Avro](https://avro.apache.org/)-Format (Standardeinstellung) oder im JSON-Format in den Blobspeicher geschrieben werden. 
>    
> Das Codierungsformat kann nur beim Konfigurieren des Blob Storage-Endpunkt festgelegt werden. Für bereits eingerichtete Endpunkte kann das Format nicht mehr geändert werden. Bei Verwendung der JSON-Codierung müssen Sie in den Nachrichtensystemeigenschaften den Inhaltstyp (contentType) auf „JSON“ und die Inhaltscodierung (contentEncoding) auf „UTF-8“ festlegen. 
>
> Ausführlichere Informationen zur Verwendung eines Blob Storage-Endpunkts finden Sie unter [Leitfaden zum Weiterleiten an den Speicher](../articles/iot-hub/iot-hub-devguide-messages-d2c.md#azure-storage-as-a-routing-endpoint).
>