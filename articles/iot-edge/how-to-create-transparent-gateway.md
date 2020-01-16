---
title: Erstellen eines transparenten Gatewaygeräts mit Azure IoT Edge | Microsoft-Dokumentation
description: Verwenden eines Azure IoT Edge-Geräts als transparentes Gateway, das Informationen von nachgeschalteten Geräten verarbeiten kann
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/30/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 2fb552578bf7c1af70b6efb4f2f6f02a2f20f2be
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75434363"
---
# <a name="configure-an-iot-edge-device-to-act-as-a-transparent-gateway"></a>Konfigurieren eines IoT Edge-Geräts als transparentes Gateway

Dieser Artikel enthält detaillierte Anweisungen zur Konfiguration eines IoT Edge-Geräts, das als transparentes Gateway für andere Geräte zur Kommunikation mit IoT Hub dient. In diesem Artikel wird mit dem Begriff *IoT Edge-Gateway* auf ein IoT Edge-Gerät verwiesen, das als transparentes Gateway konfiguriert wurde. Weitere Informationen finden Sie unter [Verwendung eines IoT Edge-Geräts als Gateway](./iot-edge-as-gateway.md).

>[!NOTE]
>Derzeit gilt Folgendes:
> * Edge-fähige Geräte können keine Verbindung mit IoT Edge-Gateways herstellen. 
> * Nachgeschaltete Geräte können keinen Dateiupload verwenden.

Es gibt drei allgemeine Schritte zum Einrichten einer erfolgreichen Verbindung mit einem transparenten Gateway. In diesem Artikel wird der erste Schritt behandelt:

1. **Das Gatewaygerät muss eine sichere Verbindung mit nachgeschalteten Geräten herstellen, Nachrichten von nachgeschalteten Geräten empfangen und Nachrichten an das richtige Ziel weiterleiten können.**
2. Das nachgeschaltete Gerät benötigt eine Geräteidentität, damit es sich bei IoT Hub authentifizieren kann und weiß, dass es über sein Gatewaygerät kommunizieren kann. Weitere Informationen finden Sie unter [Authentifizieren eines nachgeschalteten Geräts bei Azure IoT Hub](how-to-authenticate-downstream-device.md).
3. Das nachgeschaltete Gerät muss eine sichere Verbindung mit dem Gatewaygerät herstellen. Weitere Informationen finden Sie unter [Verbinden eines nachgeschalteten Geräts mit einem Azure IoT Edge-Gateway](how-to-connect-downstream-device.md).


Damit ein Gerät als Gateway fungieren kann, muss es in der Lage sein, sichere Verbindungen mit nachgeschalteten Geräten herzustellen. Mit Azure IoT Edge können Sie mithilfe der Public Key-Infrastruktur sichere Verbindungen zwischen Geräten einrichten. In diesem Fall lassen wir zu, dass ein nachgeschaltetes Gerät eine Verbindung mit einem IoT Edge-Gerät, das als transparentes Gateway fungiert, herstellt. Um eine angemessene Sicherheit zu gewährleisten, sollte das nachgeschaltete Gerät die Identität des Gatewaygeräts bestätigen. Diese Überprüfung der Identität verhindert, dass Ihre Geräte Verbindungen mit potenziell schädlichen Gateways herstellen.

Ein nachgeschaltetes Gerät im Szenario eines transparenten Gateways kann eine beliebige Anwendung oder Plattform sein, deren Identität mit dem [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub)-Clouddienst erstellt wurde. Oft verwenden diese Anwendungen das [Azure IoT-Geräte-SDK](../iot-hub/iot-hub-devguide-sdks.md). In der Praxis kann ein nachgeschaltetes Gerät sogar eine Anwendung sein, die auf dem IoT Edge-Gatewaygerät selbst ausgeführt wird. Allerdings kann ein IoT Edge-Gerät einem IoT Edge-Gateway nicht nachgeschaltet werden. 

Sie können eine beliebige Zertifikatinfrastruktur erstellen, die die für Ihre Gerät-zu-Gateway-Topologie erforderliche Vertrauensstellung ermöglicht. In diesem Artikel wird von der gleichen Zertifikateinrichtung ausgegangen, die Sie auch zum Aktivieren der [X.509-Zertifizierungsstellenzertifikat](../iot-hub/iot-hub-x509ca-overview.md) in IoT Hub verwenden. Hierbei ist ein X.509-Zertifizierungsstellenzertifikat einem bestimmten IoT-Hub zugeordnet (der Stammzertifizierungsstelle von IoT Hub), und es sind eine Reihe von Zertifikaten, die mit dieser Zertifizierungsstelle signiert werden, sowie eine Zertifizierungsstelle für die IoT Edge-Geräte vorhanden.

![Gatewayzertifikateinrichtung](./media/how-to-create-transparent-gateway/gateway-setup.png)

>[!NOTE]
>Der in diesem Artikel verwendete Begriff „Stammzertifizierungsstelle“ (Stamm-ZS) bezieht sich auf die oberste Zertifizierungsstelle des öffentlichen Zertifikats der PKI-Vertrauenskette. Es muss nicht unbedingt der Zertifikatstamm einer fremden Zertifizierungsstelle sein. In vielen Fällen ist es tatsächlich nur ein öffentliches Zertifikat einer Zwischenzertifizierungsstelle. 

Das Gateway legt dem nachgeschalteten Gerät während der Initiierung der Verbindung das Zertifizierungsstellenzertifikat des eigenen IoT Edge-Geräts vor. Das nachgeschaltete Gerät überprüft, ob das Zertifizierungsstellenzertifikat des IoT Edge-Geräts mit dem Zertifikat der Stammzertifizierungsstelle signiert wurde. Durch diesen Vorgang kann das nachgeschaltete Gerät bestätigen, dass das Gateway aus einer vertrauenswürdigen Quelle stammt.

In den folgenden Schritten werden Sie durch den Prozess zum Erstellen der Zertifikate und zum Installieren an den richtigen Orten auf dem Gateway geführt. Sie können mit jedem beliebigen Computer Zertifikate generieren und sie dann auf Ihr IoT Edge-Gerät kopieren. 

## <a name="prerequisites"></a>Voraussetzungen

Ein Azure IoT Edge-Gerät, das mit [Produktionszertifikaten](how-to-install-production-certificates.md) konfiguriert wurde.

## <a name="deploy-edgehub-to-the-gateway"></a>Bereitstellen von Edge Hub für das Gateway

Wenn Sie IoT Edge zum ersten Mal auf einem Gerät installieren, startet nur ein Systemmodul automatisch: der IoT Edge-Agent. Nachdem Sie die erste Bereitstellung erstellt haben, wird ein weiteres Gerät, das zweite Systemmodul (der IoT Edge-Hub), ebenfalls gestartet. 

Der IoT Edge-Hub ist für den Empfang der eingehenden Nachrichten von nachgeschalteten Geräten und deren Weiterleitung an das nächste Ziel zuständig. Wenn das Modul **edgeHub** auf Ihrem Gerät nicht ausgeführt wird, erstellen Sie eine erste Bereitstellung für das Gerät. Die Bereitstellung sieht leer aus, weil Sie keine Module hinzufügen. Sie stellt jedoch sicher, dass beide Systemmodule ausgeführt werden. 

Sie können überprüfen, welche Module auf einem Gerät ausgeführt werden, indem Sie dessen Details im Azure-Portal überprüfen, den Gerätestatus in Visual Studio oder Visual Studio Code anzeigen oder den Befehl `iotedge list` auf dem Gerät selbst ausführen. 

Wenn das Modul **edgeAgent** ohne das Modul **edgeHub** ausgeführt wird, führen Sie die folgenden Schritte aus:

1. Navigieren Sie im Azure-Portal zu Ihrem IoT Hub.

2. Wechseln Sie zu **IoT Edge**, und wählen Sie das IoT Edge-Gerät aus, das Sie als Gateway verwenden möchten.

3. Wählen Sie **Module festlegen** aus.

4. Wählen Sie **Weiter** aus.

5. Auf der Seite **Routen angeben** sollte eine Standardroute aufgeführt sein, über die alle Nachrichten von allen Modulen an IoT Hub gesendet werden. Ist das nicht der Fall, können Sie den folgenden Code eingeben und dann **Weiter** auswählen.

   ```JSON
   {
       "routes": {
           "route": "FROM /* INTO $upstream"
       }
   }
   ```

6. Wählen Sie auf der Seite **Vorlage überprüfen** die Option **Senden** aus.

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

Ab [Release v1.0.4](https://github.com/Azure/azure-iotedge/releases/tag/1.0.4) der IoT Edge-Runtime können das Gatewaygerät und die nachgeschalteten Geräte, die mit ihm verbunden sind, für einen erweiterten Offlinebetrieb konfiguriert werden. 

Mit dieser Funktion können lokale Module oder nachgeschaltete Geräte bei Bedarf eine erneute Authentifizierung beim IoT Edge-Gerät durchführen und über Nachrichten und Methoden miteinander kommunizieren, auch wenn sie vom IoT-Hub getrennt sind. Weitere Informationen finden Sie unter [Grundlegendes zu erweiterten Offlinefunktionen für IoT Edge-Geräte und -Module sowie untergeordnete Geräte](offline-capabilities.md).

Für die Aktivierung der erweiterten Offlinefunktionen, stellen Sie eine Beziehung mit über- und untergeordneten Geräten zwischen dem IoT Edge-Gatewaygerät und den nachgeschalteten Geräten her, die eine Verbindung mit diesem herstellen. Diese Schritte werden unter [Authentifizieren eines nachgeschalteten Geräts bei Azure IoT Hub](how-to-authenticate-downstream-device.md) ausführlicher erläutert.

## <a name="next-steps"></a>Nächste Schritte

Da nun ein IoT Edge-Gerät als transparentes Gateway fungiert, müssen Sie Ihre nachgeschalteten Geräte so konfigurieren, dass sie dem Gateway vertrauen und Nachrichten daran senden. Lesen Sie [Authentifizieren eines nachgeschalteten Geräts bei Azure IoT Hub](how-to-authenticate-downstream-device.md), um die nächsten Schritte zum Einrichten Ihres Szenarios eines transparenten Gateways auszuführen. 
