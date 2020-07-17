---
title: Erstellen eines transparenten Gatewaygeräts mit Azure IoT Edge | Microsoft-Dokumentation
description: Verwenden eines Azure IoT Edge-Geräts als transparentes Gateway, das Informationen von nachgeschalteten Geräten verarbeiten kann
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/02/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: 0155294777e1d732e5ff3874102b90049d9a123d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84782584"
---
# <a name="configure-an-iot-edge-device-to-act-as-a-transparent-gateway"></a>Konfigurieren eines IoT Edge-Geräts als transparentes Gateway

Dieser Artikel enthält detaillierte Anweisungen zur Konfiguration eines IoT Edge-Geräts, das als transparentes Gateway für andere Geräte zur Kommunikation mit IoT Hub dient. In diesem Artikel wird mit dem Begriff *IoT Edge-Gateway* auf ein IoT Edge-Gerät verwiesen, das als transparentes Gateway konfiguriert wurde. Weitere Informationen finden Sie unter [Verwendung eines IoT Edge-Geräts als Gateway](./iot-edge-as-gateway.md).

>[!NOTE]
>Derzeit gilt Folgendes:
>
> * Edge-fähige Geräte können keine Verbindung mit IoT Edge-Gateways herstellen.
> * Nachgeschaltete Geräte können keinen Dateiupload verwenden.

Es gibt drei allgemeine Schritte zum Einrichten einer erfolgreichen Verbindung mit einem transparenten Gateway. In diesem Artikel wird der erste Schritt behandelt:

1. **Konfigurieren Sie das Gatewaygerät als Server, sodass sich nachgeschaltete Geräte sicher mit ihm verbinden können. Richten Sie das Gateway so ein, dass es Nachrichten von nachgeschalteten Geräten empfängt und sie an das richtige Ziel weiterleitet.**
2. Erstellen Sie eine Geräteidentität für das nachgeschaltete Gerät, damit es sich beim IoT-Hub authentifizieren kann. Konfigurieren Sie das nachgeschaltete Gerät zum Senden von Nachrichten über das Gatewaygerät. Weitere Informationen finden Sie unter [Authentifizieren eines nachgeschalteten Geräts bei Azure IoT Hub](how-to-authenticate-downstream-device.md).
3. Verbinden Sie das nachgeschaltete Gerät mit dem Gatewaygerät, und beginnen Sie mit dem Senden von Nachrichten. Weitere Informationen finden Sie unter [Verbinden eines nachgeschalteten Geräts mit einem Azure IoT Edge-Gateway](how-to-connect-downstream-device.md).

Damit ein Gerät als Gateway fungieren kann, muss es sichere Verbindungen mit nachgeschalteten Geräten herstellen können. Mit Azure IoT Edge können Sie mithilfe der Public Key-Infrastruktur sichere Verbindungen zwischen Geräten einrichten. In diesem Fall lassen wir zu, dass ein nachgeschaltetes Gerät eine Verbindung mit einem IoT Edge-Gerät, das als transparentes Gateway fungiert, herstellt. Um eine angemessene Sicherheit zu gewährleisten, sollte das nachgeschaltete Gerät die Identität des Gatewaygeräts bestätigen. Diese Überprüfung der Identität verhindert, dass Ihre Geräte Verbindungen mit potenziell schädlichen Gateways herstellen.

Ein nachgeschaltetes Gerät kann eine beliebige Anwendung oder Plattform sein, deren Identität mit dem [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub)-Clouddienst erstellt wurde. Diese Anwendungen verwenden häufig das [Azure IoT-Geräte-SDK](../iot-hub/iot-hub-devguide-sdks.md). Ein nachgeschaltetes Gerät kann sogar eine Anwendung sein, die auf dem IoT Edge-Gatewaygerät selbst ausgeführt wird. Allerdings kann ein IoT Edge-Gerät einem IoT Edge-Gateway nicht nachgeschaltet werden.

Sie können eine beliebige Zertifikatinfrastruktur erstellen, die die für Ihre Gerät-zu-Gateway-Topologie erforderliche Vertrauensstellung ermöglicht. In diesem Artikel wird von der gleichen Zertifikateinrichtung ausgegangen, die Sie auch zum Aktivieren der [X.509-Zertifizierungsstellenzertifikat](../iot-hub/iot-hub-x509ca-overview.md) in IoT Hub verwenden. Hierbei ist ein X.509-Zertifizierungsstellenzertifikat einem bestimmten IoT-Hub zugeordnet (der Stammzertifizierungsstelle von IoT Hub), und es sind eine Reihe von Zertifikaten, die mit dieser Zertifizierungsstelle signiert werden, sowie eine Zertifizierungsstelle für die IoT Edge-Geräte vorhanden.

>[!NOTE]
>Der in diesem Artikel verwendete Begriff *Stammzertifizierungsstellen-Zertifikat* (Stamm-ZS-Zertifikat) bezieht sich auf die oberste Zertifizierungsstelle des öffentlichen Zertifikats der PKI-Vertrauenskette. Es muss nicht unbedingt der Zertifikatstamm einer fremden Zertifizierungsstelle sein. In vielen Fällen ist es tatsächlich nur ein öffentliches Zertifikat einer Zwischenzertifizierungsstelle.

In den folgenden Schritten werden Sie durch den Prozess zum Erstellen der Zertifikate und zum Installieren an den richtigen Orten auf dem Gateway geführt. Sie können mit jedem beliebigen Computer Zertifikate generieren und sie dann auf Ihr IoT Edge-Gerät kopieren.

## <a name="prerequisites"></a>Voraussetzungen

Ein Linux- oder Windows-Gerät, auf dem IoT Edge installiert ist.

## <a name="set-up-the-device-ca-certificate"></a>Einrichten des Zertifikats der Gerätezertifizierungsstelle

Für alle IoT Edge-Gateways muss ein Zertifikat der Gerätezertifizierungsstelle installiert sein. Der IoT Edge-Sicherheitsdaemon verwendet das Zertifizierungsstellenzertifikat für IoT Edge-Geräte zum Signieren eines Zertifikats der Workloadzertifizierungsstelle, das wiederum ein Serverzertifikat für IoT Edge Hub signiert. Das Gateway legt dem nachgeschalteten Gerät während der Initiierung der Verbindung sein Serverzertifikat vor. Das nachgeschaltete Gerät überprüft, ob das Serverzertifikat Teil einer Zertifikatskette ist, die zum Zertifikat der Stammzertifizierungsstelle führt. Durch diesen Vorgang kann das nachgeschaltete Gerät bestätigen, dass das Gateway aus einer vertrauenswürdigen Quelle stammt. Weitere Informationen finden Sie unter [Grundlegendes zur Verwendung von Zertifikaten durch Azure IoT Edge](iot-edge-certs.md).

![Gatewayzertifikateinrichtung](./media/how-to-create-transparent-gateway/gateway-setup.png)

Das Zertifikat der Stammzertifizierungsstelle und das Zertifikat der Gerätezertifizierungsstelle (mit seinem privaten Schlüssel) müssen auf dem IoT-Edge-Gatewaygerät vorhanden und in der IoT Edge-Datei „config.yaml“ konfiguriert sein. Beachten Sie, dass in diesem Fall *Zertifikat der Stammzertifizierungsstelle* die oberste Zertifizierungsstelle für dieses IoT Edge-Szenario ist. Für das Zertifikat der Stammzertifizierungsstelle des Gatewaygeräts und die nachgeschalteten Geräte müssen Sie ein Rollup auf das gleiche Zertifikat der Stammzertifizierungsstelle ausführen.

>[!TIP]
>Der Installationsprozess des Zertifikats der Stammzertifizierungsstelle und des Zertifikats der Gerätezertifizierungsstelle auf einem IoT Edge-Gerät wird auch ausführlicher in [Verwalten von Zertifikaten auf einem IoT Edge-Gerät](how-to-manage-device-certificates.md) erläutert.

Halten Sie die folgenden Dateien bereit:

* Zertifikat der Stammzertifizierungsstelle
* Zertifikat der Gerätezertifizierungsstelle
* Privater Schlüssel des Zertifikats der Gerätezertifizierungsstelle

In Produktionsszenarios sollten diese Dateien mit ihrer eigenen Zertifizierungsstelle generiert werden. Für Entwicklungs- und Testszenarios können Sie Demozertifikate verwenden.

1. Wenn Sie Demozertifikate verwenden, gehen Sie bei der Erstellung Ihrer Dateien wie folgt vor:
   1. [Erstellen des Zertifikats der Stammzertifizierungsstelle](how-to-create-test-certificates.md#create-root-ca-certificate) Am Ende dieser Anweisungen verfügen Sie über eine Zertifikatdatei der Stammzertifizierungsstelle:
      * `<path>/certs/azure-iot-test-only.root.ca.cert.pem`.

   2. [Erstellen von Zertifizierungsstellenzertifikaten für IoT Edge-Geräte](how-to-create-test-certificates.md#create-iot-edge-device-ca-certificates) Am Ende dieser Anweisungen verfügen Sie über zwei Dateien, ein Zertifikat der Gerätezertifizierungsstelle und den dazugehörigen privaten Schlüssel:
      * `<path>/certs/iot-edge-device-<cert name>-full-chain.cert.pem` und
      * `<path>/private/iot-edge-device-<cert name>.key.pem`

2. Wenn Sie diese Dateien auf einem anderen Computer erstellt haben, kopieren Sie sie auf Ihr IoT Edge-Gerät.

3. Öffnen Sie die Konfigurationsdatei des Sicherheitsdaemons auf Ihrem IoT Edge-Gerät.
   * Windows: `C:\ProgramData\iotedge\config.yaml`
   * Linux: `/etc/iotedge/config.yaml`

4. Suchen Sie den Abschnitt **Zertifikate** der Datei, und geben Sie die Datei-URIs für Ihre drei Dateien als Werte für die folgenden Eigenschaften an:
   * **device_ca_cert**: Zertifikat der Gerätezertifizierungsstelle
   * **device_ca_pk**: Privater Schlüssel der Gerätezertifizierungsstelle
   * **trusted_ca_certs**: Zertifikat der Stammzertifizierungsstelle

5. Speichern und schließen Sie die Datei.

6. Starten Sie IoT Edge neu.
   * Windows: `Restart-Service iotedge`
   * Linux: `sudo systemctl restart iotedge`

## <a name="deploy-edgehub-to-the-gateway"></a>Bereitstellen von Edge Hub für das Gateway

Wenn Sie IoT Edge zum ersten Mal auf einem Gerät installieren, startet nur ein Systemmodul automatisch: der IoT Edge-Agent. Nachdem Sie die erste Bereitstellung für ein Gerät erstellt haben, wird das zweite Systemmodul (der IoT Edge-Hub), ebenfalls gestartet.

Der IoT Edge-Hub ist für den Empfang der eingehenden Nachrichten von nachgeschalteten Geräten und deren Weiterleitung an das nächste Ziel zuständig. Wenn das Modul **edgeHub** auf Ihrem Gerät nicht ausgeführt wird, erstellen Sie eine erste Bereitstellung für das Gerät. Die Bereitstellung sieht leer aus, weil Sie keine Module hinzufügen. Sie stellt jedoch sicher, dass beide Systemmodule ausgeführt werden.

Sie können überprüfen, welche Module auf einem Gerät ausgeführt werden, indem Sie dessen Details im Azure-Portal überprüfen, den Gerätestatus in Visual Studio oder Visual Studio Code anzeigen oder den Befehl `iotedge list` auf dem Gerät selbst ausführen.

Wenn das Modul **edgeAgent** ohne das Modul **edgeHub** ausgeführt wird, führen Sie die folgenden Schritte aus:

1. Navigieren Sie im Azure-Portal zu Ihrem IoT Hub.

2. Wechseln Sie zu **IoT Edge**, und wählen Sie das IoT Edge-Gerät aus, das Sie als Gateway verwenden möchten.

3. Wählen Sie **Module festlegen** aus.

4. Klicken Sie auf **Weiter: Routen**.

5. Auf der Seite **Routen** sollten Sie über eine Standardroute verfügen, die alle Nachrichten sendet, unabhängig davon, ob sie von einem Modul oder von einem nachgeschaltetem Gerät an IoT Hub stammen. Andernfalls fügen Sie eine neue Route mit den folgenden Werten hinzu, und wählen Sie dann **Überprüfen + erstellen**:
   * **Name**: `route`
   * **Wert**: `FROM /messages/* INTO $upstream`

6. Wählen Sie auf der Seite **Überprüfen + erstellen** die Option **Erstellen** aus.

## <a name="open-ports-on-gateway-device"></a>Öffnen von Ports auf dem Gatewaygerät

Standard IoT Edge-Geräte benötigen keine eingehende Verbindung, um zu funktionieren, da die gesamte Kommunikation mit dem IoT Hub über ausgehende Verbindungen erfolgt. Gatewaygeräte unterscheiden sich dadurch, dass sie in der Lage sein müssen, Nachrichten von ihren nachgeschalteten Geräten zu empfangen. Wenn zwischen den nachgeschalteten Geräten und dem Gatewaygerät eine Firewall ist, muss die Kommunikation auch über die Firewall möglich sein.

Damit ein Gatewayszenario funktioniert, muss mindestens eines der unterstützten Protokolle des IoT Edge-Hubs für den eingehenden Datenverkehr von nachgeschalteten Geräten offen sein. Die unterstützten Protokolle sind MQTT, AMQP, HTTPS, MQTT über WebSockets und AMQP über WebSockets.

| Port | Protocol |
| ---- | -------- |
| 8883 | MQTT |
| 5671 | AMQP |
| 443 | HTTPS <br> MQTT+WS <br> AMQP+WS |

## <a name="route-messages-from-downstream-devices"></a>Weiterleiten von Nachrichten von nachgeschalteten Geräten

Die IoT Edge-Runtime kann Nachrichten für nachgeschaltete Geräte genau wie Nachrichten von Modulen weiterleiten. Dieses Features ermöglicht es Ihnen, die Analyse in einem auf dem Gateway ausgeführten Modul durchzuführen, bevor Daten an die Cloud gesendet werden.

Zurzeit werden von nachgeschalteten Geräten gesendete Nachrichten weitergeleitet, indem sie von den von Modulen gesendeten Nachrichten unterschieden werden. Im Gegensatz zu Nachrichten, die von nachgeschalteten Geräten gesendet werden, enthalten von Modulen gesendete Nachrichten eine Systemeigenschaft namens **connectionModuleId**. Sie können die WHERE-Klausel der Route verwenden, um Nachrichten mit dieser Systemeigenschaft auszuschließen.

Die unten gezeigte Route ist ein Beispiel, bei dem die Nachrichten von einem beliebigen nachgeschalteten Gerät an das Modul `ai_insights` und dann von `ai_insights` an IoT Hub gesendet werden.

```json
{
    "routes":{
        "sensorToAIInsightsInput1":"FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/ai_insights/inputs/input1\")",
        "AIInsightsToIoTHub":"FROM /messages/modules/ai_insights/outputs/output1 INTO $upstream"
    }
}
```

Weitere Informationen zum Routing von Nachrichten finden Sie unter [Bereitstellen von Modulen und Einrichten von Routen](./module-composition.md#declare-routes).

## <a name="enable-extended-offline-operation"></a>Aktivieren des erweiterten Offlinebetriebs

Ab [Release 1.0.4](https://github.com/Azure/azure-iotedge/releases/tag/1.0.4) der IoT Edge-Runtime können das Gatewaygerät und die nachgeschalteten Geräte, die mit ihm verbunden sind, für einen erweiterten Offlinebetrieb konfiguriert werden.

Mit dieser Funktion können lokale Module oder nachgeschaltete Geräte bei Bedarf eine erneute Authentifizierung beim IoT Edge-Gerät durchführen und über Nachrichten und Methoden miteinander kommunizieren, auch wenn sie vom IoT-Hub getrennt sind. Weitere Informationen finden Sie unter [Grundlegendes zu erweiterten Offlinefunktionen für IoT Edge-Geräte und -Module sowie untergeordnete Geräte](offline-capabilities.md).

Für die Aktivierung der erweiterten Offlinefunktionen, stellen Sie eine Beziehung mit über- und untergeordneten Geräten zwischen dem IoT Edge-Gatewaygerät und den nachgeschalteten Geräten her, die eine Verbindung mit diesem herstellen. Diese Schritte werden im nächsten Artikel dieser Reihe unter [Authentifizieren eines nachgeschalteten Geräts bei Azure IoT Hub](how-to-authenticate-downstream-device.md) ausführlicher erläutert.

## <a name="next-steps"></a>Nächste Schritte

Da nun ein IoT Edge-Gerät als transparentes Gateway eingerichtet ist, müssen Sie Ihre nachgeschalteten Geräte so konfigurieren, dass sie dem Gateway vertrauen und Nachrichten daran senden. Lesen Sie [Authentifizieren eines nachgeschalteten Geräts bei Azure IoT Hub](how-to-authenticate-downstream-device.md), um die nächsten Schritte zum Einrichten Ihres Szenarios eines transparenten Gateways auszuführen.
