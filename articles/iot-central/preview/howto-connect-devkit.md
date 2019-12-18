---
title: Herstellen einer Verbindung zwischen einem DevKit-Gerät und Ihrer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie als Geräteentwickler ein MXChip IoT DevKit-Gerät mithilfe von IoT Plug & Play mit Ihrer Azure IoT Central-Anwendung verbinden.
author: liydu
ms.author: liydu
ms.date: 12/03/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: jeffya
ms.openlocfilehash: 32dd3fa1fc137d786174e47d842f762c2a479d64
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2019
ms.locfileid: "74848960"
---
# <a name="connect-an-mxchip-iot-devkit-device-to-your-azure-iot-central-application-preview-features"></a>Herstellen einer Verbindung zwischen einem MXChip IoT DevKit-Gerät und Ihrer Azure IoT Central-Anwendung (Vorschaufeatures)

In diesem Artikel erfahren Sie, wie Sie ein MXChip IoT DevKit-Gerät (DevKit-Gerät) mit einer Azure IoT Central-Anwendung verbinden. Das Gerät verwendet das zertifizierte IoT Plug & Play-Modell für das DevKit-Gerät zum Konfigurieren der Verbindung mit IoT Central.

In diesem Anleitungsartikel werden die folgenden Aufgaben beschrieben:

- Abrufen der Verbindungsdetails aus Ihrer IoT Central-Anwendung
- Vorbereiten des Geräts und Verbinden des Geräts mit Ihrer IoT Central-Anwendung
- Anzeigen der Telemetriedaten und Eigenschaften des Geräts in IoT Central

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen der Schritte in diesem Artikel benötigen Sie folgende Ressourcen:

- Ein [DevKit-Gerät](https://aka.ms/iot-devkit-purchase).
- Eine IoT Central-Anwendung, die mit der Vorlage **Vorschauanwendung** erstellt wurde. Sie können die unter [Erstellen einer IoT Plug & Play-Anwendung](./quick-deploy-iot-central.md) beschriebenen Schritte ausführen.

## <a name="get-device-connection-details"></a>Abrufen der Geräteverbindungsdetails

1. Wählen Sie in Ihrer Azure IoT Central-Anwendung die Registerkarte **Gerätevorlagen** und dann **Neu** aus. Wählen Sie im Abschnitt **Vorkonfigurierte Gerätevorlage verwenden** in der Liste den Eintrag **MXChip IoT DevKit** aus. Wählen Sie anschließend **Weiter: Anpassen** und **Erstellen** aus.

    ![Gerätevorlage für MXChip IoT DevKit](media/howto-connect-devkit/device-template.png)

1. Wählen Sie die Registerkarte **Geräte** und in der Geräteliste den Eintrag **MXChip IoT DevKit** aus. Wählen Sie dann **Neu** aus, um auf der Grundlage einer Gerätevorlage ein neues Gerät zu erstellen.

    ![Neues Gerät](media/howto-connect-devkit/new-device.png)

1. Geben Sie im Popupfenster unter **Geräte-ID** die ID `SampleDevKit` und unter **Gerätename** den Namen `MXChip IoT DevKit - Sample` ein. Stellen Sie sicher, dass die Option **Simuliert** deaktiviert ist. Klicken Sie anschließend auf **Erstellen**.

    ![Geräte-ID und -name](media/howto-connect-devkit/device-id-name.png)

1. Klicken Sie auf das soeben erstellte Gerät, und wählen Sie **Verbinden** aus. Notieren Sie sich den **ID-Bereich**, die **Geräte-ID** und den **Primärschlüssel**.

    ![Geräteverbindungsinformationen](media/howto-connect-devkit/device-connection-info.png)

## <a name="prepare-the-device"></a>Vorbereiten des Geräts

1. Laden Sie die neueste [vorgefertigte Azure IoT Central Plug & Play-Firmware](https://github.com/MXCHIP/IoTDevKit/raw/master/pnp/iotc_devkit/bin/iotc_devkit.bin) für das DevKit-Gerät von GitHub herunter.

1. Stellen Sie über ein USB-Kabel eine Verbindung zwischen dem DevKit-Gerät und dem Entwicklungscomputer her. Unter Windows wird ein Datei-Explorer-Fenster für ein Laufwerk geöffnet, das dem Speicher auf dem DevKit-Gerät zugeordnet ist. Das Laufwerk heißt beispielsweise **AZ3166 (D:)** .

1. Ziehen Sie die Datei **iotc_devkit.bin** auf das Laufwerksfenster. Wenn der Kopiervorgang abgeschlossen ist, wird das Gerät mit der neuen Firmware neu gestartet.

    > [!NOTE]
    > Wenn auf dem Bildschirm ein Fehler wie **Kein WLAN** angezeigt wird, liegt das daran, dass das DevKit noch nicht mit dem WLAN verbunden ist.

1. Halten Sie im DevKit die **Taste B** gedrückt, drücken Sie die **Rücksetztaste**, und lassen Sie sie wieder los. Lassen Sie dann die **Taste B** los. Das Gerät befindet sich jetzt im Zugriffspunktmodus. Zur Bestätigung werden auf dem Bildschirm die Angabe „IoT DevKit – AP“ und die IP-Adresse des Konfigurationsportals angezeigt.

1. Stellen Sie auf Ihrem Computer oder Tablet eine Verbindung mit dem WLAN-Namen her, der auf dem Bildschirm des Geräts angezeigt wird. Das WLAN beginnt mit **AZ-** gefolgt von der MAC-Adresse. Wenn Sie eine Verbindung mit diesem Netzwerk herstellen, haben Sie keinen Internetzugriff. Dies ist so beabsichtigt. Sie werden mit diesem Netzwerk nur für kurze Zeit verbunden, während Sie das Gerät konfigurieren.

1. Öffnen Sie Ihren Webbrowser, und navigieren Sie zu [http://192.168.0.1/](http://192.168.0.1/). Die folgende Webseite wird angezeigt:

    ![Benutzeroberfläche für die Konfiguration](media/howto-connect-devkit/config-ui.png)

    Geben Sie auf der Webseite Folgendes ein, und nehmen Sie folgende Einstellungen vor:

    - Den Namen Ihres WLAN (SSID).
    - Ihr WLAN-Kennwort.
    - Die Verbindungsdetails: Geben Sie die **Geräte-ID**, den **ID-Bereich** und den **SAS-Primärschlüssel** ein, die/den Sie sich zuvor notiert haben.

    > [!NOTE]
    > Derzeit kann das IoT DevKit nur eine Verbindung mit einem 2,4 GHz-WLAN herstellen. Frequenzen von 5 GHz werden aufgrund von Hardwareeinschränkungen nicht unterstützt.

1. Wählen Sie **Gerät konfigurieren** aus. Das DevKit-Gerät wird neu gestartet, und die Anwendung wird ausgeführt:

    ![Benutzeroberfläche für den Neustart](media/howto-connect-devkit/reboot-ui.png)

    Auf dem DevKit-Bildschirm wird eine Bestätigung angezeigt, dass die Anwendung ausgeführt wird:

    ![DevKit wird ausgeführt](media/howto-connect-devkit/devkit-running.png)

Das DevKit registriert zuerst ein neues Gerät in der IoT Central-Anwendung und beginnt dann mit dem Senden von Daten.

## <a name="view-the-telemetry"></a>Anzeigen der Telemetrie

In diesem Schritt zeigen Sie die Telemetriedaten in Ihrer Azure IoT Central-Anwendung an.

Wählen Sie in Ihrer IoT Central-Anwendung die Registerkarte **Geräte** aus, und wählen Sie dann das hinzugefügte Gerät aus. Auf der Registerkarte **Übersicht** werden die Telemetriedaten des DevKit-Geräts angezeigt:

![IoT Central-Gerät – Übersicht](media/howto-connect-devkit/mxchip-overview-page.png)

## <a name="review-the-code"></a>Überprüfen des Codes

Navigieren Sie zum Überprüfen oder Ändern und Kompilieren des Codes zu den [Codebeispielen](https://docs.microsoft.com/samples/azure-samples/mxchip-iot-devkit-pnp/sample/).

## <a name="next-steps"></a>Nächste Schritte

Da Sie nun wissen, wie ein DevKit-Gerät mit Ihrer Azure IoT Central-Anwendung verbunden wird, empfiehlt sich als nächster Schritt das [Einrichten einer benutzerdefinierten Gerätevorlage](./howto-set-up-template.md) für Ihr eigenes IoT-Gerät.
