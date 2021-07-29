---
title: Bewährte Methoden für die Geräteentwicklung in Azure IoT Central | Microsoft-Dokumentation
description: In diesem Artikel werden bewährte Methoden für die Gerätekonnektivität in Azure IoT Central beschrieben.
author: dominicbetts
ms.author: dobett
ms.date: 03/03/2021
ms.topic: conceptual
ms.service: iot-central
services: iot-central
ms.custom:
- device-developer
ms.openlocfilehash: a3cbfa17d3b063ddcef90820dc31a080a768cbcd
ms.sourcegitcommit: bb9a6c6e9e07e6011bb6c386003573db5c1a4810
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110493761"
---
# <a name="best-practices-for-device-development"></a>Bewährte Methoden für die Geräteentwicklung

Diese Empfehlungen unterstützen Sie dabei, Geräte so zu implementieren, dass Sie die Vorteile der integrierten Notfallwiederherstellung und automatischen Skalierung in IoT Central nutzen können.

In der folgenden Liste wird der allgemeine Flow dargestellt, wenn ein Gerät eine Verbindung zu IoT Central herstellt:

1. Verwenden Sie DPS, um das Gerät bereitzustellen und eine Verbindungszeichenfolge für das Gerät abzurufen.

1. Verwenden Sie die Verbindungszeichenfolge, um den internen IoT Hub-Endpunkt von IoT Central zu verbinden. Senden Sie Daten an die IoT Central-Anwendung, und empfangen Sie Daten von der Anwendung.

1. Wenn für das Gerät Verbindungsfehler auftreten, können Sie je nach Fehlertyp entweder versuchen, die Verbindung neu herzustellen oder das Gerät neu bereitzustellen.

## <a name="use-dps-to-provision-the-device"></a>Bereitstellen des Geräts mithilfe von DPS

Wenn Sie ein Gerät mit DPS bereitstellen möchten, verwenden Sie diese Bereichs-ID sowie die Geräte-ID Ihrer IoT Central-Anwendung. Weitere Informationen zu den verschiedenen Typen an Anmeldeinformationen finden Sie unter [Gruppenregistrierung mit X.509](concepts-get-connected.md#x509-group-enrollment) und [Gruppenregistrierung mit SAS](concepts-get-connected.md#sas-group-enrollment). Weitere Informationen zu Geräte-IDs finden Sie unter [Geräteregistrierung](concepts-get-connected.md#device-registration).

Bei Erfolg gibt DPS eine Verbindungszeichenfolge zurück, mit der das Gerät eine Verbindung zu Ihrer IoT Central-Anwendung herstellen kann. Informationen zum Troubleshooting für Bereitstellungsfehler finden Sie unter [Überprüfen des Bereitstellungsstatus Ihres Geräts](troubleshoot-connection.md#check-the-provisioning-status-of-your-device).

Das Gerät kann die Verbindungszeichenfolge zwischenspeichern und für spätere Verbindungen verwenden. Das Gerät muss jedoch darauf vorbereitet sein, [Verbindungsfehler zu verarbeiten](#handle-connection-failures).

## <a name="connect-to-iot-central"></a>Herstellen einer Verbindung zu IoT Central

Verwenden Sie die Verbindungszeichenfolge, um den internen IoT Hub-Endpunkt von IoT Central zu verbinden. Die Verbindung ermöglicht es Ihnen, Telemetriedaten an Ihre IoT Central-Anwendung zu senden, Eigenschaftswerte mit Ihrer IoT Central-Anwendung zu synchronisieren und auf Befehle zu reagieren, die von Ihrer IoT Central-Anwendung gesendet werden.

## <a name="handle-connection-failures"></a>Verarbeiten von Verbindungsfehlern

Zu Skalierungs- oder Notfallwiederherstellungszwecken kann IoT Central den zugrunde liegenden IoT-Hub aktualisieren. Damit die Konnektivität bestehen bleibt, sollte Ihr Gerätecode bestimmte Verbindungsfehler verarbeiten können, indem eine Verbindung zum neuen IoT Hub-Endpunkt hergestellt wird.

Wenn für das Gerät beim Herstellen einer Verbindung einer der folgenden Fehler angezeigt wird, sollte es den Bereitstellungsprozess mit DPS wiederholen, um eine neue Verbindungszeichenfolge abzurufen. Diese Fehler deuten darauf hin, dass die vom Gerät verwendete Verbindungszeichenfolge nicht mehr gültig ist:

- Der IoT Hub-Endpunkt kann nicht erreicht werden.
- Das Sicherheitstoken ist abgelaufen.
- Das Gerät wurde in IoT Hub deaktiviert.

Wenn für das Gerät beim Herstellen einer Verbindung einer der folgenden Fehler angezeigt wird, sollte eine Backoffstrategie verwendet werden, um noch mal zu versuchen, die Verbindung herzustellen. Diese Fehler bedeuten, dass die Verbindungszeichenfolge, die das Gerät verwendet, immer noch gültig ist. Vorübergehende Bedingungen verhindern jedoch, dass das Gerät eine Verbindung herstellen kann:

- Der Operator hat das Gerät blockiert.
- Es wird ein interner Fehler 500 vom Dienst ausgegeben.

Weitere Informationen zu Fehlercodes für Geräte finden Sie unter [Troubleshooting für Geräteverbindungen](troubleshoot-connection.md).

## <a name="test-failover-capabilities"></a>Testen von Failoverfunktionen

Über die Azure CLI können Sie die Failoverfunktionen Ihres Geräteclientcodes testen. Der CLI-Befehl verschiebt eine Geräteregistrierung vorübergehend auf einen anderen internen IoT-Hub. Sie können nachprüfen, ob das Gerätefailover funktioniert hat, indem Sie überprüfen, ob das Gerät weiterhin Telemetriedaten sendet und auf Befehle in Ihrer IoT Central-Anwendung reagiert.

Wenn Sie den Failovertest für Ihr Gerät durchführen möchten, führen Sie den folgenden Befehl aus:

```azurecli
az iot central device manual-failover \
    --app-id {Application ID of your IoT Central application} \
    --device-id {Device ID of the device you're testing} \
    --ttl-minutes {How to wait before moving the device back to it's original IoT hub}
```

> [!TIP]
> Zum Suchen der **Anwendungs-ID** navigieren Sie in Ihrer IoT Central-Anwendung zu **Verwaltung > Ihre Anwendung**.

Wenn der Befehl erfolgreich ausgeführt wurde, wird eine Ausgabe angezeigt, die etwa so aussieht:

```output
Command group 'iot central device' is in preview and under development. Reference and support levels: https://aka.ms/CLI_refstatus
{
  "hubIdentifier": "6bd4...bafa",
  "message": "Success! This device is now being failed over. You can check your device'’'s status using 'iot central device registration-info' command. The device will revert to its original hub at Tue, 18 May 2021 11:03:45 GMT. You can choose to failback earlier using device-manual-failback command. Learn more: https://aka.ms/iotc-device-test"
}
```

Weitere Informationen zum CLI-Befehl finden Sie unter [az iot central device manual-failover](/cli/azure/iot/central/device#az_iot_central_device_manual_failover).

Jetzt können Sie überprüfen, ob die Telemetriedaten aus dem Gerät Ihre IoT Central Anwendung weiterhin erreichen.

Beispiele für Gerätecode zur Verarbeitung von Failovern in verschiedenen Programmiersprachen finden Sie unter [IoT-Hochverfügbarkeitsclients](https://github.com/iot-for-all/iot-central-high-availability-clients).

## <a name="next-steps"></a>Nächste Schritte

Als Nächstes werden die folgenden Schritte empfohlen:

- Beispielcode, der die Verwendung von SAS-Token zeigt, finden Sie in [Tutorial: Erstellen einer Node.js-Clientanwendung und Verbinden der Anwendung mit Ihrer Azure IoT Central-Anwendung (Node.js)](tutorial-connect-device.md)
- Erfahren Sie mehr über [Informationen zum Verbinden von Geräten mit X.509-Zertifikaten mithilfe des Node.js-Geräte-SDK für IoT Central-Anwendung](how-to-connect-devices-x509.md).
- Informieren Sie sich über [Überwachen der Gerätekonnektivität per Azure CLI](./howto-monitor-devices-azure-cli.md).
- Informieren Sie sich über [Definieren eines neuen IoT-Gerätetyps in Ihrer Azure IoT Central-Anwendung](./howto-set-up-template.md).
- Informieren Sie sich über [Azure IoT Edge-Geräte und Azure IoT Central](./concepts-iot-edge.md).
