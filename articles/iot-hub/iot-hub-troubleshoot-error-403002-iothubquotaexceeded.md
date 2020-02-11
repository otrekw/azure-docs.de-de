---
title: Problembehandlung – Azure IoT Hub-Fehler „403002 IoTHubQuotaExceeded“
description: Grundlegendes zum Beheben des Fehlers „403002 IoTHubQuotaExceeded“
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 8312c3267e826088f34b3bffe1520703eec00bdc
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2020
ms.locfileid: "76960391"
---
# <a name="403002-iothubquotaexceeded"></a>403002 IoTHubQuotaExceeded

In diesem Artikel werden die Ursachen des Fehlers **403002 IoTHubQuotaExceeded** und die Lösungen dafür beschrieben.

## <a name="symptoms"></a>Symptome

Alle Anforderungen an IoT Hub schlagen mit der Meldung **403002 IoTHubQuotaExceeded** fehl. Im Azure-Portal wird die Liste mit IoT-Hub-Geräten nicht geladen.

## <a name="cause"></a>Ursache

Das tägliche Nachrichtenkontingent für den IoT-Hub wurde überschritten. 

## <a name="solution"></a>Lösung

[Aktualisieren oder erhöhen Sie die Anzahl der Einheiten auf dem IoT-Hub](iot-hub-upgrade.md), oder warten Sie bis zum nächsten UTC-Tag, an dem das Tageskontingent aktualisiert wird.

## <a name="next-steps"></a>Nächste Schritte

* Informationen dazu, wie Vorgänge, z. B. Zwillingsabfragen und direkte Methoden, in das Kontingent eingerechnet werden, finden Sie unter [Grundlegendes zu den Preisen von Azure IoT Hub](iot-hub-devguide-pricing.md#charges-per-operation).
* Richten Sie zum Einrichten der Überwachung für die tägliche Kontingentnutzung eine Warnung mit der Metrik *Gesamtzahl verwendeter Nachrichten* ein. Schrittanleitungen finden Sie unter [Einrichten von Metriken und Warnungen mit IoT Hub](tutorial-use-metrics-and-diags.md#set-up-metrics).