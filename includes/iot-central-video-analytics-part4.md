---
title: Includedatei
description: Includedatei
services: iot-central
author: dominicbetts
ms.service: iot-central
ms.topic: include
ms.date: 10/06/2020
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 164f5803b6e9e62447423735e98f6e4c36c73f13
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91876669"
---
### <a name="add-relationships"></a>Hinzufügen von Beziehungen

Wählen Sie in der Gerätevorlage **LVA-Edgegateway** unter **Modules/LVA Edge Gateway Module** (Module/LVA-Edgegatewaymodul) die Option **Beziehungen** aus. Wählen Sie **+ Beziehung hinzufügen** aus, und fügen Sie die beiden folgenden Beziehungen hinzu:

|Anzeigename               |Name          |Ziel |
|-------------------------- |------------- |------ |
|LVA Edge Motion Detector (LVA-Edgebewegungserkennung)   |Verwenden Sie den Standardwert.   |LVA Edge Motion Detector Device (Gerät für LVA-Edgebewegungserkennung) |
|LVA Edge Object Detector (LVA-Edgeobjekterkennung)   |Verwenden Sie den Standardwert.   |LVA Edge Object Detector Device (Gerät für LVA-Edgeobjekterkennung) |

Klicken Sie dann auf **Speichern**.

:::image type="content" source="media/iot-central-video-analytics-part4/relationships.png" alt-text="Hinzufügen von Beziehungen":::

### <a name="add-views"></a>Hinzufügen von Ansichten

Die Gerätevorlage **LVA-Edgegateway** enthält keine Ansichtsdefinitionen.

So fügen Sie der Gerätevorlage eine Ansicht hinzu:

1. Navigieren Sie in der Gerätevorlage **LVA-Edgegateway** zu **Ansichten**, und wählen Sie die Kachel **Das Gerät wird visualisiert** aus.

1. Geben Sie *LVA Edge Gateway device* (Gerät für LVA-Edgegateway) als Namen für die Ansicht ein.

1. Fügen Sie der Ansicht die folgenden Kacheln hinzu:

    * Eine Kachel mit den Eigenschaften für **Geräteinformationen**: **Gerätemodell**, **Hersteller**, **Betriebssystem**, **Prozessorarchitektur**, **Softwareversion**, **Gesamter Arbeitsspeicher** und **Speicher gesamt**.
    * Eine Liniendiagramm-Kachel mit den Telemetriewerten für **Freier Arbeitsspeicher** und **System Heartbeat** (Systemheartbeat).
    * Eine Kachel für den Ereignisverlauf mit den folgenden Ereignissen: **Kamera erstellen**, **Kamera löschen**, **Modulneustart**, **Modul gestartet**, **Modul angehalten**.
    * Eine 2x1-Kachel mit dem letzten bekannten Wert, auf der die Telemetriedaten für den **IoT Central-Clientzustand** angezeigt werden.
    * Eine 2x1-Kachel mit dem letzten bekannten Wert, auf der die Telemetriedaten für den **Modulzustand** angezeigt werden.
    * Eine 1x1-Kachel mit dem letzten bekannten Wert, auf der die Telemetriedaten für den **System Heartbeat** (Systemheartbeat) angezeigt werden.
    * Eine 1x1-Kachel mit dem letzten bekannten Wert, auf der die Telemetriedaten für **Connected Cameras** (Angeschlossene Kameras) angezeigt werden.

    :::image type="content" source="media/iot-central-video-analytics-part4/gateway-dashboard.png" alt-text="Hinzufügen von Beziehungen":::

1. Wählen Sie **Speichern** aus.

### <a name="publish-the-device-template"></a>Veröffentlichen der Gerätevorlage

Bevor Sie der Anwendung ein Gerät hinzufügen können, müssen Sie die Gerätevorlage veröffentlichen:

1. Wählen Sie in der Gerätevorlage **LVA-Edgegateway** die Option **Veröffentlichen** aus.

1. Wählen Sie auf der Seite **Diese Gerätevorlage für die Anwendung veröffentlichen** die Option **Veröffentlichen** aus.

**LVA-Edgegateway** ist jetzt als Gerätetyp verfügbar, der in der Anwendung auf der Seite **Geräte** verwendet werden kann.

## <a name="add-a-gateway-device"></a>Hinzufügen eines Gatewaygeräts

Fügen Sie der Anwendung wie folgt ein Gerät vom Typ **LVA-Edgegateway** hinzu:

1. Navigieren Sie zur Seite **Geräte**, und wählen Sie die Gerätevorlage **LVA-Edgegateway** aus.

1. Wählen Sie **+ Neu** aus.

1. Ändern Sie im Dialogfeld **Neues Gerät erstellen** den Gerätenamen in *LVA-Gateway 001* und die Geräte-ID in *lva-gateway-001*.

    > [!NOTE]
    > Die Geräte-ID muss in der Anwendung eindeutig sein.

1. Klicken Sie auf **Erstellen**.

Der Gerätestatus lautet **Registriert**.

### <a name="get-the-device-credentials"></a>Abrufen der Geräteanmeldeinformationen

Sie benötigen die Anmeldeinformationen, mit denen das Gerät eine Verbindung mit Ihrer IoT Central-Anwendung herstellen kann. So rufen Sie die Anmeldeinformationen ab:

1. Wählen Sie auf der Seite **Geräte** das von Ihnen erstellte Gerät **lva-gateway-001** aus.

1. Wählen Sie **Verbinden**.

1. Notieren Sie sich auf der Seite **Geräteverbindung** in der Datei *scratchpad.txt* den **ID-Bereich**, die **Geräte-ID** und den **Primärschlüssel** des Geräts. Diese Werte werden später benötigt.

1. Stellen Sie sicher, dass die Verbindungsmethode auf **Shared Access Signature** festgelegt ist.

1. Klicken Sie auf **Schließen**.

## <a name="next-steps"></a>Nächste Schritte

Sie haben jetzt mit der Anwendungsvorlage **Videoanalyse: Objekt- und Bewegungserkennung** eine IoT Central-Anwendung und dann eine Gerätevorlage für das Gatewaygerät erstellt und der Anwendung anschließend ein Gatewaygerät hinzugefügt.

Gehen Sie wie folgt vor, falls Sie die Anwendung vom Typ „Videoanalyse: Objekt- und Bewegungserkennung“ mit IoT Edge-Modulen ausprobieren möchten, für die eine Cloud-VM mit simulierten Videodatenströmen ausgeführt wird:

> [!div class="nextstepaction"]
> [Erstellen einer IoT Edge-Instanz für die Videoanalyse (Linux-VM)](../articles/iot-central/retail/tutorial-video-analytics-iot-edge-vm.md)

Gehen Sie wie folgt vor, falls Sie die Anwendung vom Typ „Videoanalyse: Objekt- und Bewegungserkennung“ mit IoT Edge-Modulen ausprobieren möchten, für die ein echtes Gerät mit einer echten **ONVIF**-Kamera ausgeführt wird:

> [!div class="nextstepaction"]
> [Erstellen einer IoT Edge-Instanz für die Videoanalyse (Intel NUC)](../articles/iot-central/retail/tutorial-video-analytics-iot-edge-nuc.md)
