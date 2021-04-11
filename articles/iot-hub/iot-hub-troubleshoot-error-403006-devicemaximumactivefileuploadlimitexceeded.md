---
title: Problembehandlung – Azure IoT Hub-Fehler „403006 DeviceMaximumActiveFileUploadLimitExceeded“
description: Grundlegendes zum Beheben des Fehlers „403006 DeviceMaximumActiveFileUploadLimitExceeded“
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 3a70c11fd4f2a0549370933c300f431a03ffcf1d
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106061297"
---
# <a name="403006-devicemaximumactivefileuploadlimitexceeded"></a>403006 DeviceMaximumActiveFileUploadLimitExceeded

In diesem Artikel werden die Ursachen des Fehlers **403006 DeviceMaximumActiveFileUploadLimitExceeded** und die Lösungen dafür beschrieben.

## <a name="symptoms"></a>Symptome

Ihre Dateiuploadanforderung schlägt mit dem Fehlercode **403006** und der Meldung „Number of active file upload requests cannot exceed 10“ (Anzahl von aktiven Dateiuploadanforderungen kann 10 nicht überschreiten“) fehl.

## <a name="cause"></a>Ursache

Jeder Geräteclient ist auf [10 gleichzeitige Dateiuploads](./iot-hub-devguide-quotas-throttling.md#other-limits) beschränkt. 

Sie können den Grenzwert leicht überschreiten, wenn Ihr Gerät IoT Hub nicht benachrichtigt, nachdem Dateiuploads abgeschlossen wurden. Dieses Problem wird häufig durch ein unzuverlässiges geräteseitiges Netzwerk verursacht.

## <a name="solution"></a>Lösung

Stellen Sie sicher, dass das Gerät sofort [über den Abschluss des IoT Hub-Dateiuploads benachrichtigen](./iot-hub-devguide-file-upload.md#notify-iot-hub-of-a-completed-file-upload) kann. Versuchen Sie dann, [die SAS-Token-TTL für die Dateiuploadkonfiguration zu verkürzen](iot-hub-configure-file-upload.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Dateiuploads finden Sie unter [Hochladen von Dateien mit IoT Hub](./iot-hub-devguide-file-upload.md) und [Konfigurieren des Dateiuploads in IoT Hub im Azure-Portal](./iot-hub-configure-file-upload.md).