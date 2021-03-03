---
title: Behandeln von Problemen mit Azure Percept-Vision und Vision-Modulen
description: Hier finden Sie Tipps zur Problembehandlung für einige der häufigsten Probleme während der Vision-KI-Prototyperstellung.
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/18/2021
ms.custom: template-how-to
ms.openlocfilehash: e7351079e7325aa7750dc0d10f0923bc847ccc3c
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101678437"
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

    :::image type="content" source="./media/vision-solution-troubleshooting/vision-device-modules-inline.png" alt-text="Iot Edge-Seite für ausgewähltes Gerät mit dem Inhalt der Registerkarte „Module“" lightbox= "./media/vision-solution-troubleshooting/vision-device-modules.png":::

## <a name="delete-a-device"></a>Löschen eines Mediums

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_Azure_Iothub=aduprod&microsoft_azure_marketplace_ItemHideKey=Microsoft_Azure_ADUHidden#home).

1. Klicken Sie auf das Symbol **IoT Hub**.

1. Wählen Sie die IoT Hub-Instanz aus, mit der das Zielgerät verbunden ist.

1. Wählen Sie **IoT Edge** aus, und aktivieren Sie das Kontrollkästchen neben der Zielgeräte-ID. Klicken Sie auf das Papierkorbsymbol, um das Gerät zu löschen.

    :::image type="content" source="./media/vision-solution-troubleshooting/vision-delete-device.png" alt-text="Hervorgehobenes Löschsymbol auf der IoT Edge-Startseite":::

## <a name="eye-module-troubleshooting-tips"></a>Tipps zur Problembehandlung für das visuelle Modul

Vergewissern Sie sich bei einem Problem mit **WebStreamModule**, dass **azureeyemodule** (zuständig für Vision-Modellrückschlüsse) ausgeführt wird. Navigieren Sie zum Überprüfen des Laufzeitstatus zum [Azure-Portal](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_Azure_Iothub=aduprod&microsoft_azure_marketplace_ItemHideKey=Microsoft_Azure_ADUHidden#home) und anschließend zu **Alle Ressourcen** ->  **\<your IoT hub>**  -> **IoT Edge** ->  **\<your device ID>** . Klicken Sie auf die Registerkarte **Module**, um den Laufzeitstatus aller installierten Module anzuzeigen.

:::image type="content" source="./media/vision-solution-troubleshooting/over-the-air-iot-edge-device-page-inline.png" alt-text="Bildschirm mit dem Laufzeitstatus der Gerätemodule" lightbox= "./media/vision-solution-troubleshooting/over-the-air-iot-edge-device-page.png":::

Ist der Laufzeitstatus von **azureeyemodule** nicht als **Wird ausgeführt** angegeben, klicken Sie auf **Module festlegen** -> **azureeyemodule**. Legen Sie auf der Seite **Moduleinstellungen** die Option **Gewünschter Status** auf **Wird ausgeführt** fest, und klicken Sie auf **Aktualisieren**.

 :::image type="content" source="./media/vision-solution-troubleshooting/firmware-desired-status-stopped.png" alt-text="Konfigurationsbildschirm für Moduleinstellungen":::

## <a name="view-device-rtsp-video-stream"></a>Anzeigen des Geräte-RTSP-Videostreams

Zeigen Sie den RTSP-Videostream Ihres Geräts über [Azure Percept Studio](./how-to-view-video-stream.md) oder [VLC Media Player](https://www.videolan.org/vlc/index.html) an.

Navigieren Sie zum Öffnen des RTSP-Stream in VLC Media Player zu **Medien** -> **Netzwerkstream öffnen** -> **rtsp://[Geräte-IP-Adresse]/result**.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Problembehandlung von Azure Percept DK finden Sie im [allgemeinen Problembehandlungsleitfaden](./troubleshoot-dev-kit.md).