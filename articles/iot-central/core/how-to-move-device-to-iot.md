---
title: Verschieben eines Geräts von einem IoT-Hub nach Azure IoT Central
description: Verschieben von Geräten von einem IoT-Hub nach Azure IoT Central
author: TheRealJasonAndrew
ms.author: v-anjaso
ms.date: 12/20/2020
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 7898f842529b81b80febff444c97b199fbebba3c
ms.sourcegitcommit: 431bf5709b433bb12ab1f2e591f1f61f6d87f66c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2021
ms.locfileid: "98146425"
---
# <a name="how-to-transfer-a-device-to-azure-iot-central-from-iot-hub"></a>Übertragen eines Geräts von einem IoT-Hub nach Azure IoT Central

*Dieser Artikel gilt für Operatoren und Geräteentwickler.*  

In diesem Artikel wird beschrieben, wie Geräte von einem IoT-Hub in eine Azure IoT Central-Anwendung übertragen werden. 

Ein Gerät stellt zunächst eine Verbindung mit einem DPS-Endpunkt her, um die Informationen abzurufen, die für die Verbindung mit Ihrer Anwendung erforderlich sind. Intern verarbeitet Ihre IoT Central Anwendung Verbindungen mit Geräten über einen IoT-Hub.  

Ein Gerät kann mit einer Verbindungszeichenfolge oder DPS direkt mit einem IoT-Hub verbunden werden. [Azure IoT Hub Device Provisioning Service (DPS)](../../iot-dps/about-iot-dps.md) bietet eine Route zu IoT Central.

## <a name="to-move-the-device"></a>So verschieben Sie das Gerät

Damit Sie ein Gerät vom IoT-Hub mit IoT Central verbinden können, muss es mit folgenden Informationen aktualisiert werden:

* Die [Bereichs-ID](../../iot-dps/concepts-service.md) der IoT Central-Anwendung
* Einem Schlüssel, der entweder vom Schlüssel der [Gruppen-SAS](concepts-get-connected.md) oder dem [X.509-Zertifikat](../../iot-hub/iot-hub-x509ca-overview.md) abgeleitet ist

Für die Interaktion mit IoT Central ist eine Gerätevorlage erforderlich, die alle vom Gerät implementierten Eigenschaften/Telemetriedaten/Befehle modelliert. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit IoT Central](concepts-get-connected.md) und [Was sind Gerätevorlagen?](concepts-device-templates.md).

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie ein Geräteentwickler sind, werden einige der folgenden nächsten Schritte empfohlen:

- Beispielcode, der die Verwendung von SAS-Token zeigt, finden Sie in [Tutorial: Erstellen einer Node.js-Clientanwendung und Verbinden der Anwendung mit Ihrer Azure IoT Central-Anwendung (Node.js)](tutorial-connect-device.md)
- Erfahren Sie mehr über [Informationen zum Verbinden von Geräten mit X.509-Zertifikaten mithilfe des Node.js-Geräte-SDK für IoT Central-Anwendung](how-to-connect-devices-x509.md).
- Informieren Sie sich über [Überwachen der Gerätekonnektivität per Azure CLI](./howto-monitor-devices-azure-cli.md).
- Informieren Sie sich über [Definieren eines neuen IoT-Gerätetyps in Ihrer Azure IoT Central-Anwendung](./howto-set-up-template.md).
- Informieren Sie sich über [Azure IoT Edge-Geräte und Azure IoT Central](./concepts-iot-edge.md).