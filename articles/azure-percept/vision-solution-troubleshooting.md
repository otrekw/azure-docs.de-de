---
title: Behandeln von Problemen mit Azure Percept-Vision und Vision-Modulen
description: Hier finden Sie Tipps zur Problembehandlung für einige der häufigsten Probleme während der Vision-KI-Prototyperstellung.
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 03/29/2021
ms.custom: template-how-to
ms.openlocfilehash: 78de5ef0ef77a181d4a2da91e4b468db1b47f208
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106074691"
---
# <a name="vision-solution-troubleshooting"></a>Problembehandlung für die Vision-Lösung

In den folgenden Anleitungen finden Sie Informationen zum Beheben von Problemen mit Vision-Lösungen ohne Code in Azure Percept Studio.

## <a name="delete-a-vision-project"></a>Löschen eines Vision-Projekts

1. Wechseln Sie zu https://www.customvision.ai/projects.

1. Zeigen Sie auf das Projekt, das Sie löschen möchten, und klicken Sie auf das Papierkorbsymbol, um das Projekt zu löschen.

    :::image type="content" source="./media/vision-solution-troubleshooting/vision-delete-project.png" alt-text="Seite „Projekte“ in Custom Vision mit hervorgehobenem Löschsymbol":::

## <a name="check-which-modules-are-on-a-device"></a>Überprüfen, welche Module sich auf einem Gerät befinden

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_Azure_Iothub=aduprod&microsoft_azure_marketplace_ItemHideKey=Microsoft_Azure_ADUHidden#home).

1. Klicken Sie auf das Symbol **IoT Hub**.

    :::image type="content" source="./media/vision-solution-troubleshooting/vision-iot-hub-2-inline.png" alt-text="Startseite des Azure-Portals mit hervorgehobenem IoT Hub-Symbol" lightbox= "./media/vision-solution-troubleshooting/vision-iot-hub-2.png":::

1. Wählen Sie die IoT Hub-Instanz aus, mit der das Zielgerät verbunden ist.

    :::image type="content" source="./media/vision-solution-troubleshooting/vision-iot-hub.png" alt-text="Liste der IoT Hub-Instanzen":::

1. Wählen Sie **IoT Edge** aus, und klicken Sie unter der Registerkarte **Geräte-ID** auf Ihr Gerät.

    :::image type="content" source="./media/vision-solution-troubleshooting/vision-iot-edge.png" alt-text="IoT Edge-Startseite":::

1. Ihre Gerätemodule sind auf der Registerkarte **Module** aufgeführt.

    :::image type="content" source="./media/vision-solution-troubleshooting/vision-device-modules-inline.png" alt-text="Azure Iot Edge-Seite für ausgewähltes Gerät mit dem Inhalt der Registerkarte Module." lightbox= "./media/vision-solution-troubleshooting/vision-device-modules.png":::

## <a name="delete-a-device"></a>Löschen eines Mediums

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_Azure_Iothub=aduprod&microsoft_azure_marketplace_ItemHideKey=Microsoft_Azure_ADUHidden#home).

1. Klicken Sie auf das Symbol **IoT Hub**.

1. Wählen Sie die IoT Hub-Instanz aus, mit der das Zielgerät verbunden ist.

1. Wählen Sie **IoT Edge** aus, und aktivieren Sie das Kontrollkästchen neben der Zielgeräte-ID. Klicken Sie auf das Papierkorbsymbol, um das Gerät zu löschen.

    :::image type="content" source="./media/vision-solution-troubleshooting/vision-delete-device.png" alt-text="Hervorgehobenes Löschsymbol auf der IoT Edge-Startseite.":::

## <a name="eye-module-troubleshooting-tips"></a>Tipps zur Problembehandlung für das visuelle Modul

### <a name="check-the-runtime-status-of-azureeyemodule"></a>Prüfen des Laufzeitstatus von azureeyemodule

Vergewissern Sie sich bei einem Problem mit **WebStreamModule**, dass **azureeyemodule**, das (zuständig für Vision-Modellrückschlüsse) ausgeführt wird. Navigieren Sie zum Überprüfen des Laufzeitstatus zum [Azure-Portal](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_Azure_Iothub=aduprod&microsoft_azure_marketplace_ItemHideKey=Microsoft_Azure_ADUHidden#home) und anschließend zu **Alle Ressourcen** ->  **\<your IoT hub>**  -> **IoT Edge** ->  **\<your device ID>** . Klicken Sie auf die Registerkarte **Module**, um den Laufzeitstatus aller installierten Module anzuzeigen.

:::image type="content" source="./media/vision-solution-troubleshooting/over-the-air-iot-edge-device-page-inline.png" alt-text="Bildschirm mit dem Laufzeitstatus der Gerätemodule" lightbox= "./media/vision-solution-troubleshooting/over-the-air-iot-edge-device-page.png":::

Ist der Laufzeitstatus von **azureeyemodule** nicht als **Wird ausgeführt** angegeben, klicken Sie auf **Module festlegen** -> **azureeyemodule**. Legen Sie auf der Seite **Moduleinstellungen** die Option **Gewünschter Status** auf **Wird ausgeführt** fest, und klicken Sie auf **Aktualisieren**.

 :::image type="content" source="./media/vision-solution-troubleshooting/firmware-desired-status-stopped.png" alt-text="Konfigurationsbildschirm für Moduleinstellungen":::

### <a name="update-telemetryintervalneuralnetworkms"></a>TelemetryIntervalNeuralNetworkMs aktualisieren

Wenn der folgende Fehler bei der Zählbegrenzung auftritt, muss der Wert TelemetryIntervalNeuralNetworkMs in den Modulzwilling-Einstellungen des azureeyemodule-Moduls aktualisiert werden.

|Fehlermeldung|
|------|
|Die Gesamtanzahl der Nachrichten auf IotHub 'xxxxxxxxx' hat die zugewiesene Quote überschritten. Maximal erlaubte Nachrichtenanzahl: '8000', aktuelle Meldungsanzahl: 'xxxx'. Sende- und Empfangsvorgänge sind für diesen Hub bis zum nächsten UTC-Tag gesperrt. Erwägen Sie eine Erhöhung der Einheiten für diesen Hub, um die Quote zu erhöhen.|

TelemetryIntervalNeuralNetworkMs bestimmt, wie oft Nachrichten (in Millisekunden) vom neuronalen Netzwerk gesendet werden sollen. Azure-Abonnements haben eine begrenzte Anzahl von Nachrichten pro Tag, abhängig von Ihrer Abonnementstufe. Wenn Sie sich selbst aussperren, weil Sie zu viele Nachrichten gesendet haben, erhöhen Sie diesen Wert auf eine höhere Zahl. 12000 (d.h. einmal alle 12 Sekunden) ergibt eine schöne runde Zahl von 7200 Nachrichten pro Tag, was unter dem Limit von 8000 Nachrichten für das kostenlose Abonnement liegt.

Gehen Sie folgendermaßen vor, um den Wert TelemetryIntervalNeuralNetworkMs zu aktualisieren:

1. Melden Sie sich beim [Azure-Portal](https://ms.portal.azure.com/?feature.canmodifystamps=true&Microsoft_Azure_Iothub=aduprod#home) an, und öffnen Sie **Alle Ressourcen**.

1. Klicken Sie auf der Seite **Alle Ressourcen** auf den Namen der IOT Hub, die während des Setups in Ihrem devkit bereitgestellt wurde.

1. Klicken Sie Links auf der Seite IOT Hub unter **Automatische Geräteverwaltung** auf **Azure IOT Edge**. Suchen Sie auf der Seite Azure IOT Edge-Geräte die Geräte-ID Ihres devkit. Klicken Sie auf die Geräte-ID Ihres devkit, um die Seite Azure IOT Edge-Geräts zu öffnen.

1. Wählen Sie unter der Registerkarte **Module** die Option **azureeyemodule** aus.

1. Öffnen Sie auf der Seite azureeyemodule den Typ **Zwilling der Modulkennung**.

    :::image type="content" source="./media/vision-solution-troubleshooting/module-page-inline.png" alt-text="Screenshot der Modulseite." lightbox= "./media/vision-solution-troubleshooting/module-page.png":::

1. Scrollen Sie nach unten zu **Eigenschaften**. Beachten Sie, dass die Eigenschaften „Laufen“ und „Protokollierung“ zu diesem Zeitpunkt nicht aktiv sind.

    :::image type="content" source="./media/vision-solution-troubleshooting/module-identity-twin-inline.png" alt-text="Screenshot: Eigenschaften von Modulzwillingen." lightbox= "./media/vision-solution-troubleshooting/module-identity-twin.png":::

1. Aktualisieren Sie den Wert von **TelemetryIntervalNeuralNetworkMs** wie gewünscht, und klicken Sie auf das Symbol **Speichern**.

## <a name="view-device-rtsp-video-stream"></a>Anzeigen des Geräte-RTSP-Videostreams

Zeigen Sie den RTSP-Videostream Ihres Geräts über [Azure Percept Studio](./how-to-view-video-stream.md) oder [VLC Media Player](https://www.videolan.org/vlc/index.html) an.

Navigieren Sie zum Öffnen des RTSP-Stream in VLC Media Player zu **Medien** -> **Netzwerkstream öffnen** -> **rtsp://[Geräte-IP-Adresse]/result**.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Problembehandlung von Azure Percept DK finden Sie im [allgemeinen Problembehandlungsleitfaden](./troubleshoot-dev-kit.md).