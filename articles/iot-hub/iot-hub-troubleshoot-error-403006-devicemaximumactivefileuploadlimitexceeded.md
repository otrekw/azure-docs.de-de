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
ms.openlocfilehash: 1e3c05e4cc3ccf34573b55d3729aded16e26d66e
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2020
ms.locfileid: "76960307"
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