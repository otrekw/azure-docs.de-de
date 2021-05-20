---
title: Offlinebetrieb von Geräten – Azure IoT Edge | Microsoft-Dokumentation
description: Hier erfahren Sie, wie IoT Edge-Geräte und -Module über längere Zeiträume ohne Internetverbindung betrieben werden können und wie IoT Edge auch den Offlinebetrieb normaler IoT-Geräte ermöglichen kann.
author: kgremban
ms.author: kgremban
ms.date: 11/22/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 41610bb956273fa69119d6b87a016072a5e4faa2
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108749877"
---
# <a name="understand-extended-offline-capabilities-for-iot-edge-devices-modules-and-child-devices"></a>Grundlegendes zu erweiterten Offlinefunktionen für IoT Edge-Geräte und -Module sowie untergeordnete Geräte

[!INCLUDE [iot-edge-version-all-supported](../../includes/iot-edge-version-all-supported.md)]

Azure IoT Edge unterstützt erweiterte Offlinevorgänge auf Ihren IoT Edge-Geräten und ermöglicht Offlinevorgänge auch auf untergeordneten Geräten. Wenn ein IoT Edge-Gerät einmal eine Verbindung mit IoT Hub herstellen konnte, kann das Gerät zusammen mit allen untergeordneten Geräten auch mit unregelmäßiger oder ohne Internetverbindung funktionieren.

## <a name="how-it-works"></a>Funktionsweise

Wenn ein IoT Edge-Gerät in den Offlinemodus wechselt, übernimmt der IoT Edge-Hub drei Rollen. Zunächst erfasst er alle für die Upstream-Übermittlung bestimmten Nachrichten und speichert sie, bis das Gerät wieder eine Verbindung herstellt. Zweitens: Sie agiert als IoT Hub-Instanz bei der Authentifizierung von Modulen und untergeordneten Geräten, um deren Funktion aufrechtzuerhalten. Drittens: Sie ermöglicht die Kommunikation zwischen den untergeordneten Geräten, für die normalerweise die IoT Hub-Instanz verantwortlich wäre.

Das folgende Beispiel veranschaulicht ein IoT Edge-Szenario im Offlinemodus:

1. **Geräte konfigurieren**

   Für IoT Edge-Geräte werden automatisch Offlinefunktionen aktiviert. Um diese Funktion auf andere Geräte auszudehnen, müssen Sie die untergeordneten Geräte so konfigurieren, dass sie dem ihnen zugewiesenen übergeordneten Gerät vertrauen und die Geräte-zu-Cloud-Kommunikation über das übergeordnete Gerät als Gateway leiten.

2. **Synchronisieren mit IoT Hub**

   Mindestens einmal nach der Installation der IoT Edge-Runtime muss das IoT Edge-Gerät online sein, damit die Synchronisierung mit IoT Hub ausgeführt wird. Bei dieser Synchronisierung empfängt das IoT Edge-Gerät Details zu allen zugewiesenen untergeordneten Geräten. Das IoT Edge-Gerät aktualisiert zudem auf sichere Weise seinen lokalen Cache, um Offlinevorgänge zu aktivieren und ruft Einstellungen für das lokale Speichern von Telemetrienachrichten.

3. **Offlineschalten**

   Während sie von IoT Hub getrennt sind, können das IoT Edge-Gerät, seine bereitgestellten Module und alle untergeordneten Geräte ohne zeitliche Beschränkung betrieben werden. Module und untergeordnete Geräte können gestartet und neu gestartet werden, indem im Offlinemodus die Authentifizierung mit dem IoT Edge-Hub durchgeführt wird. Telemetriedaten für die Upstreamübermittlung an IoT Hub werden lokal gespeichert. Die Kommunikation zwischen Modulen und untergeordneten Geräten wird durch direkte Methoden oder Nachrichten aufrechterhalten.

4. **Erneutes Verbinden und Synchronisieren mit IoT Hub**

   Nach dem Wiederherstellen der Verbindung mit IoT Hub wird die Synchronisierung des IoT Edge-Geräts wieder ausgeführt. Lokal gespeicherte Nachrichten werden umgehend an IoT Hub übermittelt, dies hängt jedoch von der Verbindungsgeschwindigkeit, der IoT Hub-Latenz und ähnlichen Faktoren ab. Die Nachrichten werden in der Reihenfolge übermittelt, in der sie gespeichert wurden.

   Unterschiede zwischen den gewünschten und gemeldeten Eigenschaften der Module und Geräte werden ausgeglichen. Das IoT Edge-Gerät aktualisiert seine Gruppe von zugewiesenen untergeordneten Geräten mit den Änderungen.

## <a name="restrictions-and-limits"></a>Einschränkungen

Die in diesem Artikel beschriebenen erweiterten Offlinefunktionen sind in der [IoT Edge-Version 1.0.7 oder höher](https://github.com/Azure/azure-iotedge/releases) verfügbar. Frühere Versionen verfügen nur über einen Teil dieser Offlinefunktionen. Für vorhandene IoT Edge-Geräte, die über keine erweiterten Offlinefunktionen verfügen, kann kein Upgrade durch Ändern der Runtime-Version ausgeführt werden. Stattdessen sind sie mit einer neuen IoT Edge-Geräteidentität zu konfigurieren, damit diese Funktionen verfügbar werden.

<!-- 1.1 -->
:::moniker range="iotedge-2018-06"

Lediglich Nicht-IoT Edge-Geräte können als untergeordnete Geräte hinzugefügt werden.

:::moniker-end
<!-- end 1.1 -->

Azure IOT Edge-Geräte und ihre zugewiesenen untergeordneten Geräte können nach dem anfänglichen einmaligen synchronisieren unbegrenzt Offline funktionieren. Die Speicherung von Nachrichten hängt jedoch von der Gültigkeitsdauer (Time to Live, TTL) und dem verfügbaren Speicherplatz zum Speichern der Nachrichten ab.

## <a name="set-up-parent-and-child-devices"></a>Einrichten von übergeordneten und untergeordneten Geräten

Übergeordnete Geräte können über mehrere untergeordnete Geräte verfügen, doch weist ein untergeordnetes Gerät nur ein übergeordnetes Gerät auf.

<!-- 1.1 -->
:::moniker range="iotedge-2018-06"

Untergeordnete Geräte können beliebige Nicht-IoT Edge-Geräte sein, die beim selben IoT Hub registriert sind.

:::moniker-end
<!-- end 1.1 -->

<!-- 1.2 -->
:::moniker range="iotedge-2020-11"

Untergeordnete Geräte können beliebige Geräte (IoT Edge- oder Nicht-IoT Edge-Geräte) sein, die beim selben IoT Hub registriert sind.

:::moniker-end
<!-- end 1.2 -->

Wenn Sie mit dem Erstellen einer Beziehung zwischen einem über- und untergeordnetem Element (zwischen einem IoT Edge- und einem IoT-Gerät) nicht vertraut sind, lesen Sie den Abschnitt [Authentifizieren eines nachgeschalteten Geräts bei Azure IoT Hub](how-to-authenticate-downstream-device.md). Die Abschnitte zu symmetrischen Schlüsseln, selbstsigniertem X.509 und durch Zertifizierungsstelle signiertem X.509 zeigen Beispiele, wie Sie das Azure-Portal und die Azure-Befehlszeilenschnittstelle verwenden, um die Beziehungen zwischen über- und untergeordnetem Element beim Erstellen von Geräten zu definieren. Bei vorhandenen Geräten können Sie die Beziehung auf der Seite mit den Gerätedetails entweder des übergeordneten oder des untergeordneten Geräts deklarieren.

<!-- 1.2 -->
:::moniker range="iotedge-2020-11"

Wenn Sie mit dem Erstellen einer über- und untergeordneten Beziehung zwischen zwei IoT Edge-Geräten nicht vertraut sind, finden Sie weitere Informationen unter [Verbinden eines nachgeschalteten IoT Edge-Geräts mit einem Azure IoT Edge-Gateway](how-to-connect-downstream-iot-edge-device.md).

:::moniker-end
<!-- end 1.2 -->

### <a name="set-up-the-parent-device-as-a-gateway"></a>Einrichten des übergeordneten Geräts als Gateway

Sie können sich eine Beziehung über- und untergeordneter Geräte als ein transparentes Gateway vorstellen, bei dem das untergeordnete Gerät über eine eigene Identität in IoT Hub verfügt, aber mit der Cloud über das übergeordnete Gerät kommuniziert. Für eine sichere Kommunikation muss das untergeordnete Gerät überprüfen können, dass das übergeordnete Gerät aus einer vertrauenswürdigen Quelle stammt. Andernfalls könnten Dritte schädliche Geräte einrichten, die die Identität übergeordneter Geräte annehmen und die Kommunikation abfangen.

Eine Möglichkeit zum Erstellen dieser Vertrauensstellung wird in den folgenden Artikeln ausführlich beschrieben:

* [Konfigurieren eines IoT Edge-Geräts als transparentes Gateway](how-to-create-transparent-gateway.md)
* [Verbinden eines nachgeschalteten (untergeordneten) Geräts mit einem Azure IoT Edge-Gateway](how-to-connect-downstream-device.md)

## <a name="specify-dns-servers"></a>Angeben von DNS-Servern

Zur Verbesserung der Stabilität empfiehlt es sich dringend, die in Ihrer Umgebung verwendeten DNS-Serveradressen anzugeben. Wenn Sie Ihren DNS-Server für IoT Edge festlegen möchten, lesen Sie die Lösung zu [Edge-Agent-Modul meldet ständig „empty config file“, und es werden keine Module auf dem Gerät gestartet](troubleshoot-common-errors.md#edge-agent-module-reports-empty-config-file-and-no-modules-start-on-the-device) im Artikel zur Problembehandlung.

## <a name="optional-offline-settings"></a>Optionale Offlineeinstellungen

Wenn Ihre Geräte offline geschaltet werden, speichert das übergeordnete IoT Edge-Gerät alle Gerät-zu-Cloud-Nachrichten, bis die Verbindung wiederhergestellt ist. Das IoT Edge-Hub-Modul verwaltet die Speicherung und Weiterleitung von Offlinenachrichten. Bei Geräten, die möglicherweise über längere Zeiträume offline sind, optimieren Sie die Leistung, indem zwei IoT Edge-Hub-Einstellungen konfigurieren.

Legen Sie zunächst eine höhere Einstellung für die Gültigkeitsdauer fest, damit der IoT Edge-Hub Nachrichten so lange aufbewahrt, bis Ihr Gerät wieder verbunden ist. Fügen Sie dann zusätzlichen Speicherplatz für den Nachrichtenspeicher hinzu.

### <a name="time-to-live"></a>Gültigkeitsdauer

Die Gültigkeitsdauer entspricht der Zeit (in Sekunden), die eine Nachricht auf die Übermittlung warten kann, ehe sie abläuft. Der Standardwert ist 7.200 Sekunden (zwei Stunden). Der Höchstwert ist nur durch den Höchstwert einer ganzzahligen Variablen begrenzt und beträgt ca. 2 Milliarden.

Diese Einstellung ist eine gewünschte Eigenschaft des IoT Edge-Hub, die im Modulzwilling gespeichert wird. Sie können sie im Azure-Portal oder direkt im Bereitstellungsmanifest konfigurieren.

```json
"$edgeHub": {
    "properties.desired": {
        "schemaVersion": "1.0",
        "routes": {},
        "storeAndForwardConfiguration": {
            "timeToLiveSecs": 7200
        }
    }
}
```

### <a name="host-storage-for-system-modules"></a>Hostspeicher für Systemmodule

Nachrichten- und Modulzustandsinformationen werden standardmäßig im lokalen Containerdateisystem des IoT Edge-Hubs gespeichert. Für eine höhere Zuverlässigkeit, insbesondere beim Offlinebetrieb, können Sie auch Speicher auf dem Host-IoT Edge-Gerät reservieren. Weitere Informationen finden Sie unter [Gewähren des Zugriffs auf den lokalen Speicher eines Geräts für Module](how-to-access-host-storage-from-module.md).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über das Einrichten eines transparenten Gateways für Ihre Verbindungen über- und untergeordneter Geräte:

* [Konfigurieren eines IoT Edge-Geräts als transparentes Gateway](how-to-create-transparent-gateway.md)
* [Authentifizieren eines nachgeschalteten Geräts bei Azure IoT Hub](how-to-authenticate-downstream-device.md)
* [Verbinden eines nachgeschalteten Geräts mit einem Azure IoT Edge-Gateway](how-to-connect-downstream-device.md)
