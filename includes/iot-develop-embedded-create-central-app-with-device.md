---
title: include file
description: include file
author: timlt
ms.service: iot-develop
ms.topic: include
ms.date: 05/17/2021
ms.author: timlt
ms.custom: include file
ms.openlocfilehash: 3620e8021cfe5adf6226f007ea240921951ea6fe
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110061271"
---
## <a name="create-the-cloud-components"></a>Erstellen der Cloudkomponenten

### <a name="create-the-iot-central-application"></a>Erstellen der IoT Central-Anwendung

Es gibt verschiedene Möglichkeiten zum Verbinden von Geräten mit Azure IoT. In diesem Abschnitt wird beschrieben, wie Sie Azure IoT Central verwenden, um die Verbindung für ein Gerät herzustellen. Bei IoT Central handelt es sich um eine IoT-Anwendungsplattform, mit der die Kosten und die Komplexität der Erstellung und Verwaltung von IoT-Lösungen reduziert werden.

Erstellen Sie wie folgt eine neue Anwendung:
1. Wählen Sie im [Azure IoT Central-Portal](https://apps.azureiotcentral.com/) im seitlichen Navigationsmenü die Option **Meine Apps** aus.
1. Wählen Sie **+ Neue Anwendung** aus.
1. Klicken Sie auf **Benutzerdefinierte Apps**.
1. Fügen Sie einen Anwendungsnamen und eine URL hinzu.
1. Wählen Sie den Tarif **Free** aus, um eine 7-tägige Testversion zu aktivieren.

    :::image type="content" source="media/iot-develop-embedded-create-central-app-with-device/iot-central-create-custom.png" alt-text="Erstellen einer benutzerdefinierten App in Azure IoT Central":::

1. Klicken Sie auf **Erstellen**.

    Nachdem die Anwendung von IoT Central bereitgestellt wurde, werden Sie automatisch zum neuen Anwendungsdashboard geleitet.

    > [!NOTE]
    > Falls Sie bereits über eine IoT Central-Anwendung verfügen, können Sie diese verwenden, um die Schritte in diesem Artikel auszuführen, und müssen keine neue Anwendung erstellen.

### <a name="create-a-new-device"></a>Erstellen eines neuen Geräts

In diesem Abschnitt verwenden Sie das IoT Central-Anwendungsdashboard, um ein neues Gerät zu erstellen. In einem späteren Abschnitt verwenden Sie die Verbindungsinformationen für das neu erstellte Gerät, um für Ihr physisches Gerät eine sichere Verbindung herzustellen.

Erstellen Sie wie folgt ein Gerät:
1. Wählen Sie im Anwendungsdashboard im seitlichen Navigationsmenü die Option **Geräte** aus.
1. Wählen Sie **+ Neu** aus, um das Fenster **Neues Gerät erstellen** zu öffnen.
1. Behalten Sie für die Gerätevorlage die Einstellung **Nicht zugewiesen** bei.
1. Geben Sie den gewünschten Gerätenamen und die Geräte-ID ein.

    :::image type="content" source="media/iot-develop-embedded-create-central-app-with-device/iot-central-create-device.png" alt-text="Erstellen eines Geräts in Azure IoT Central":::

1. Wählen Sie die Schaltfläche **Erstellen**.
1. Das neu erstellte Gerät wird in der Liste **Alle Geräte** angezeigt.  Wählen Sie den Gerätenamen aus, um die Details anzuzeigen.
1. Wählen Sie in der Menüleiste oben rechts die Option **Verbinden** aus, um die Verbindungsinformationen anzuzeigen, die im nächsten Abschnitt zum Konfigurieren des Geräts verwendet werden.

    :::image type="content" source="media/iot-develop-embedded-create-central-app-with-device/iot-central-device-connection-info.png" alt-text="Anzeigen der Details zur Geräteverbindung":::

1. Notieren Sie sich die Verbindungswerte für die folgenden Parameter der Verbindungszeichenfolge, die im Dialogfeld **Verbinden** angezeigt werden. Sie fügen diese Werte im nächsten Schritt einer Konfigurationsdatei hinzu:

    > * `ID scope`
    > * `Device ID`
    > * `Primary key`