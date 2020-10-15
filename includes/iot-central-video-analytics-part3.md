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
ms.openlocfilehash: 60a62733a17d1a3dcc4ba80ed7ceb1c37c8ac5d6
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91876664"
---
## <a name="create-the-azure-iot-edge-gateway-device"></a>Erstellen des Azure IoT Edge-Gatewaygeräts

Die Anwendung „Videoanalyse: Objekt- und Bewegungserkennung“ enthält die Gerätevorlagen **LVA Edge Object Detector** (LVA-Edgeobjekterkennung) und **LVA Edge Motion Detection** (LVA-Edgebewegungserkennung). In diesem Abschnitt erstellen Sie eine Vorlage für ein Gatewaygerät, indem Sie das Bereitstellungsmanifest verwenden, und fügen das Gatewaygerät Ihrer IoT Central-Anwendung hinzu.

### <a name="create-a-device-template-for-the-lva-edge-gateway"></a>Erstellen einer Gerätevorlage für das LVA-Edgegateway

Gehen Sie wie folgt vor, um das Bereitstellungsmanifest zu importieren und die Gerätevorlage vom Typ **LVA Edge Gateway** (LVA-Edgegateway) zu erstellen:

1. Navigieren Sie in Ihrer IoT Central-Anwendung zu **Gerätevorlagen**, und wählen Sie **+ Neu** aus.

1. Wählen Sie auf der Seite **Vorlagentyp auswählen** die Kachel **Azure IoT Edge** aus. Wählen Sie anschließend **Next: Anpassen**.

1. Geben Sie auf der Seite **Azure IoT Edge-Bereitstellungsmanifest hochladen** den Namen *LVA-Edgegateway* als Vorlagennamen ein, und aktivieren Sie die Option **Gatewaygerät mit nachgeschalteten Geräten**.

    Suchen Sie noch nicht nach dem Bereitstellungsmanifest. Wenn Sie die Suche doch durchführen, erwartet der Bereitstellungs-Assistent eine Schnittstelle für jedes Modul. Sie müssen aber nur die Schnittstelle für **LvaEdgeGatewayModule** verfügbar machen. Sie laden das Manifest in einem späteren Schritt hoch.

    :::image type="content" source="./media/iot-central-video-analytics-part3/upload-deployment-manifest.png" alt-text="Bereitstellungsmanifest nicht hochladen":::

    Klicken Sie auf **Weiter: Review** (Weiter: Überprüfen).

1. Wählen Sie auf der Seite **Überprüfen** die Option **Erstellen** aus.

### <a name="import-the-device-capability-model"></a>Importieren des Gerätefunktionsmodells

Die Gerätevorlage muss ein Gerätefunktionsmodell enthalten. Wählen Sie auf der Seite **LVA-Edgegateway** die Kachel **Funktionsmodell importieren** aus. Navigieren Sie zum zuvor erstellten Ordner *lva-configuration*, und wählen Sie die Datei *LvaEdgeGatewayDcm.json* aus.

Die Gerätevorlage **LVA-Edgegateway** enthält jetzt das **LVA-Edgegatewaymodul** und drei Schnittstellen: **Device information** (Geräteinformationen), **LVA Edge Gateway Settings** (LVA-Edgegatewayeinstellungen) und **LVA Edge Gateway Interface** (LVA-Edgegatewayschnittstelle).
