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
ms.openlocfilehash: 3521933baa8dc328910fbacd8b1b6768832fa5f1
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106061314"
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