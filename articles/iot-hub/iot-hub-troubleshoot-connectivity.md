---
title: Überwachen und Behandeln von Problemen bei der Trennung von Geräteverbindungen mit Azure IoT Hub
description: Hier erfahren Sie, wie Sie häufige Fehler bei der Gerätekonnektivität für Azure IoT Hub überwachen und behandeln.
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: bed6736fda0c1815964f9017adb1e6fffa9335d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "77110676"
---
# <a name="monitor-diagnose-and-troubleshoot-disconnects-with-azure-iot-hub"></a>Überwachen, Diagnostizieren und Behandeln von Problemen bei der Trennung von Geräteverbindungen mit Azure IoT Hub

Verbindungsprobleme bei IoT-Geräten können schwierig zu behandeln sein, weil es viele mögliche Fehlerquellen gibt. Anwendungslogik, physische Netzwerke, Protokolle, Hardware, IoT Hub und andere Clouddienste können Probleme verursachen. Die Möglichkeit zum Erkennen und Ermitteln der Ursache eines Problems ist wichtig. Weil aber bei einer IoT-Lösung im großen Stil Tausende von Geräten betroffen sein könnten, ist es nicht praktikabel, einzelne Geräte manuell zu überprüfen. Nutzen Sie als Hilfe beim Erkennen, Diagnostizieren und Beheben dieser Probleme im großen Stil die Überwachungsfunktionen, die IoT Hub über Azure Monitor bereitstellt. Weil diese Funktionen auf das, was IoT Hub beobachten kann, begrenzt sind, empfehlen wir außerdem, dass Sie die bewährten Methoden für die Überwachung Ihrer Geräte und anderer Azure-Dienste befolgen.

## <a name="get-alerts-and-error-logs"></a>Abrufen von Warnungen und Fehlerprotokollen

Verwenden Sie Azure Monitor zum Abrufen von Warnungen und Schreiben von Protokollen, wenn Geräteverbindungen getrennt werden.

### <a name="turn-on-diagnostic-logs"></a>Aktivieren der Diagnoseprotokolle

Aktivieren Sie Diagnosen für IoT Hub, um Geräteverbindungsereignisse und -fehler zu protokollieren. Wir empfehlen, diese Diagnoseprotokolle frühestmöglich zu aktivieren, denn wenn die Protokolle nicht aktiviert sind, erhalten Sie im Fall einer Trennung von Geräteverbindungen keine Informationen zur Behebung des Problems.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Navigieren Sie zu Ihrer IoT Hub-Instanz.

3. Wählen Sie **Diagnoseeinstellungen** aus.

4. Wählen Sie **Diagnose aktivieren**.

5. Aktivieren Sie **Verbindungen**, damit Protokolle erfasst werden.

6. Aktivieren Sie zur Vereinfachung der Analyse die Option **An Log Analytics senden** ([siehe Preisübersicht](https://azure.microsoft.com/pricing/details/log-analytics/)). Siehe dazu das Beispiel unter [Lösen von Konnektivitätsproblemen](#resolve-connectivity-errors).

   ![Empfohlene Einstellungen](./media/iot-hub-troubleshoot-connectivity/diagnostic-settings-recommendation.png)

Weitere Informationen finden Sie unter [Schnelle Überwachung der Integrität von Azure IoT Hub und Diagnose von Problemen](iot-hub-monitor-resource-health.md).

### <a name="set-up-alerts-for-device-disconnect-at-scale"></a>Einrichten von Warnungen im großen Stil bei einer Trennung der Geräteverbindung

Um Warnungen zu erhalten, wenn die Verbindung von Geräten getrennt wird, konfigurieren Sie Warnungen in der Metrik **Verbundene Geräte (Vorschau)** .

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Navigieren Sie zu Ihrer IoT Hub-Instanz.

3. Wählen Sie **Warnungen**.

4. Wählen Sie **Neue Warnungsregel** aus.

5. Wählen Sie **Bedingung hinzufügen** und dann „Verbundene Geräte (Vorschau)“ aus.

6. Richten Sie Schwellenwert und Warnung ein, indem Sie den Anweisungen folgen.

Weitere Informationen finden Sie unter [Was sind Warnungen in Microsoft Azure?](../azure-monitor/platform/alerts-overview.md).

#### <a name="detecting-individual-device-disconnects"></a>Erkennen einzelner getrennter Geräteverbindungen

Wenn Sie getrennte Geräteverbindungen *pro Gerät* erkennen möchten und beispielsweise wissen müssen, dass eine Factory gerade offline geschaltet wurde, [konfigurieren Sie Ereignisse der Trennung von Geräteverbindungen mithilfe von Event Grid](iot-hub-event-grid.md).

## <a name="resolve-connectivity-errors"></a>Lösen von Konnektivitätsproblemen

Wenn Sie Diagnoseprotokolle und Warnungen für verbundene Geräte aktivieren, erhalten Sie Warnungen, wenn Fehler auftreten. In diesem Abschnitt wird beschrieben, wie Sie nach häufig auftretenden Problemen suchen, wenn Sie eine Warnung erhalten haben. In den folgenden Schritten wird davon ausgegangen, dass Sie für Ihre Diagnoseprotokolle Azure Monitor-Protokolle eingerichtet haben.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Navigieren Sie zu Ihrer IoT Hub-Instanz.

1. Wählen Sie **Protokolle** aus.

1. Um Verbindungsfehlerprotokolle für IoT Hub zu isolieren, geben Sie die folgende Abfrage ein, und wählen Sie dann **Ausführen** aus:

    ```kusto
    AzureDiagnostics
    | where ( ResourceType == "IOTHUBS" and Category == "Connections" and Level == "Error")
    ```

1. Wenn Ergebnisse vorhanden sind, suchen Sie nach `OperationName`, `ResultType` (Fehlercode) und `ResultDescription` (Fehlermeldung), um weitere Details zu dem Fehler anzuzeigen.

   ![Beispiel für ein Fehlerprotokoll](./media/iot-hub-troubleshoot-connectivity/diag-logs.png)

1. Folgen Sie den Anleitungen zur Problemlösung für die häufigsten Fehler:

    - **[404104 DeviceConnectionClosedRemotely](iot-hub-troubleshoot-error-404104-deviceconnectionclosedremotely.md)**
    - **[401003 IoTHubUnauthorized](iot-hub-troubleshoot-error-401003-iothubunauthorized.md)**
    - **[409002 LinkCreationConflict](iot-hub-troubleshoot-error-409002-linkcreationconflict.md)**
    - **[500001 ServerError](iot-hub-troubleshoot-error-500xxx-internal-errors.md)**
    - **[500008 GenericTimeout](iot-hub-troubleshoot-error-500xxx-internal-errors.md)**

## <a name="i-tried-the-steps-but-they-didnt-work"></a>Ich habe die Schritte ausprobiert, aber sie funktionieren nicht.

Wenn das Problem mit den vorstehenden Schritten nicht behoben werden konnte, versuchen Sie Folgendes:

* Wenn Sie Zugriff auf die problematischen Geräte haben, entweder physisch oder remote (wie SSH), folgen Sie der [Anleitung zur geräteseitigen Problembehandlung](https://github.com/Azure/azure-iot-sdk-node/wiki/Troubleshooting-Guide-Devices), um die Fehlerbehebung fortzusetzen.

* Überprüfen Sie, ob Ihre Geräte im Azure-Portal > IoT Hub > IoT-Geräte **aktiviert** sind.

* Wenn Ihr Gerät das MQTT-Protokoll verwendet, vergewissern Sie sich, dass Port 8883 geöffnet ist. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit IoT Hub (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub).

* Weitere Hilfe erhalten Sie im [Azure IoT Hub-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azureiothub), im [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-iot-hub) oder vom [Azure-Support](https://azure.microsoft.com/support/options/).

Um die Dokumentation allgemein zu verbessern, schreiben Sie im Feedbackabschnitt einen Kommentar, wenn Ihnen diese Anleitung bei der Problembehandlung nicht geholfen hat.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Beheben von vorübergehenden Problemen finden Sie unter [Behandlung vorübergehender Fehler](/azure/architecture/best-practices/transient-faults).

* Mehr über das Azure IoT-SDK und den Umgang mit Wiederholungen erfahren Sie unter [Verwalten von Konnektivität und zuverlässigem Messaging mithilfe von Azure IoT Hub-Geräte-SDKs](iot-hub-reliability-features-in-sdks.md#connection-and-retry).
