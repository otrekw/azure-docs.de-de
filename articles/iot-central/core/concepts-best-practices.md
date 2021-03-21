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
ms.openlocfilehash: e8ae8b0173e53c0a46ded1a2690175e367997c9f
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102054113"
---
# <a name="best-practices-for-device-development"></a>Bewährte Methoden für die Geräteentwicklung

*Dieser Artikel gilt für Geräteentwickler.*

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

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie ein Geräteentwickler sind, werden einige der folgenden nächsten Schritte empfohlen:

- Beispielcode, der die Verwendung von SAS-Token zeigt, finden Sie in [Tutorial: Erstellen einer Node.js-Clientanwendung und Verbinden der Anwendung mit Ihrer Azure IoT Central-Anwendung (Node.js)](tutorial-connect-device.md)
- Erfahren Sie mehr über [Informationen zum Verbinden von Geräten mit X.509-Zertifikaten mithilfe des Node.js-Geräte-SDK für IoT Central-Anwendung](how-to-connect-devices-x509.md).
- Informieren Sie sich über [Überwachen der Gerätekonnektivität per Azure CLI](./howto-monitor-devices-azure-cli.md).
- Informieren Sie sich über [Definieren eines neuen IoT-Gerätetyps in Ihrer Azure IoT Central-Anwendung](./howto-set-up-template.md).
- Informieren Sie sich über [Azure IoT Edge-Geräte und Azure IoT Central](./concepts-iot-edge.md).
