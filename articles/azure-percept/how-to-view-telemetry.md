---
title: Anzeigen der Telemetriedaten für Azure Percept DK-Modellrückschlüsse
description: Hier erfahren Sie, wie Sie im Azure IoT-Explorer die Rückschlusstelemetriedaten Ihres Azure Percept DK-Vision-Modells anzeigen.
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/17/2021
ms.custom: template-how-to
ms.openlocfilehash: 07abbc5f5e85c75b73774d11b6b81dd2085735b7
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102095322"
---
# <a name="view-your-azure-percept-dks-model-inference-telemetry"></a>Anzeigen der Telemetriedaten für Azure Percept DK-Modellrückschlüsse

Befolgen Sie diese Anleitung, um die Rückschlusstelemetriedaten Ihres Azure Percept DK-Vision-Modells im [Azure IoT-Explorer](https://github.com/Azure/azure-iot-explorer/releases) anzuzeigen.

## <a name="prerequisites"></a>Voraussetzungen

- Azure Percept DK (DevKit)
- [Azure-Abonnement](https://azure.microsoft.com/free/)
- [Azure Percept DK-Setup:](./quickstart-percept-dk-set-up.md) Sie haben Ihr DevKit mit einem WLAN verbunden, eine IoT Hub-Instanz erstellt und das DevKit mit der IoT Hub-Instanz verbunden.
- [Das Vision-KI-Modell wurde für Azure Percept DK bereitgestellt.](./how-to-deploy-model.md)

## <a name="view-telemetry"></a>Anzeigen von Telemetriedaten

1. Schalten Sie Ihr DevKit ein.

1. Laden Sie den [Azure IoT-Explorer](https://github.com/Azure/azure-iot-explorer/releases) herunter, und installieren Sie ihn. Wählen Sie als Windows-Benutzer die MSI-Datei aus.

    :::image type="content" source="./media/how-to-view-telemetry/azure-iot-explorer-download.png" alt-text="Downloadbildschirm für Azure IoT-Explorer":::

1. Verbinden Sie Ihre IoT Hub-Instanz mit Azure IoT-Explorer:

    1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).

    1. Wählen Sie **Alle Ressourcen**.

        :::image type="content" source="./media/how-to-view-telemetry/azure-portal.png" alt-text="Azure-Portal: Startseite":::

    1. Wählen Sie die IoT Hub-Instanz aus, mit der Azure Percept DK verbunden ist.

        :::image type="content" source="./media/how-to-view-telemetry/iot-hub.png" alt-text="IoT Hub-Liste im Azure-Portal":::

    1. Wählen Sie im linken Bereich der IoT Hub-Seite die Option **Freigegebene Zugriffsrichtlinien** aus.

        :::image type="content" source="./media/how-to-view-telemetry/shared-access-policies.png" alt-text="IoT Hub-Seite mit den freigegebenen Zugriffsrichtlinien":::

    1. Klicken Sie auf **iothubowner**.

        :::image type="content" source="./media/how-to-view-telemetry/iothubowner.png" alt-text="Bildschirm „Freigegebene Zugriffsrichtlinien“, auf dem „iothubowner“ hervorgehoben ist":::

    1. Klicken Sie neben **Verbindungszeichenfolge – Primärschlüssel** auf das blaue Kopiersymbol.

        :::image type="content" source="./media/how-to-view-telemetry/connection-string.png" alt-text="Fenster „iothubowner“ mit hervorgehobener Schaltfläche zum Kopieren der Verbindungszeichenfolge":::

    1. Öffnen Sie den Azure IoT-Explorer, und klicken Sie auf **+ Verbindung hinzufügen**.

    1. Fügen Sie die Verbindungszeichenfolge im Fenster **Verbindung hinzufügen** in das Feld **Verbindungszeichenfolge** ein, und klicken Sie auf **Speichern**.

        :::image type="content" source="./media/how-to-view-telemetry/add-connection-string.png" alt-text="Azure IoT-Explorer-Fenster mit dem Feld zum Einfügen der Verbindungszeichenfolge":::

    1. Verweisen Sie das Vision-SOM auf ein Objekt für Modellrückschlüsse.

    1. Wählen Sie **Telemetrie** aus.

    1. Klicken Sie auf **Start**, um Telemetrieereignisse vom Gerät anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte
Informieren Sie sich darüber, wie Sie den [Azure Percept DK-Videostream](./how-to-view-video-stream.md) anzeigen.