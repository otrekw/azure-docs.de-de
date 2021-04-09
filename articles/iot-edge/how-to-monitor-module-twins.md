---
title: Überwachen von Modulzwillingen – Azure IoT Edge
description: Interpretieren von Geräte- und Modulzwillingen zum Ermitteln der Konnektivität und Integrität.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 05/29/2020
ms.topic: conceptual
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 0b7013979199eefa873a651d99e87dc8b2c47856
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103201601"
---
# <a name="monitor-module-twins"></a>Überwachen von Modulzwillingen

[!INCLUDE [iot-edge-version-all-supported](../../includes/iot-edge-version-all-supported.md)]

Modulzwillinge ermöglichen in Azure IoT Hub die Überwachung der Konnektivität und Integrität ihrer IoT Edge-Bereitstellungen. Modulzwillinge speichern nützliche Informationen über die Leistung Ihrer laufenden Module in Ihrem IoT-Hub. Das [IoT Edge-Agent](iot-edge-runtime.md#iot-edge-agent)- und [IoT Edge-Hub](iot-edge-runtime.md#iot-edge-hub)-Runtimemodul verwalten jeweils ihre Modulzwillinge (`$edgeAgent` und `$edgeHub`), genauer:

* `$edgeAgent` enthält Integritäts- und Konnektivitätsdaten sowohl zum IoT Edge-Agent- als auch IoT Edge-Hub-Runtimemodul sowie zu Ihren benutzerdefinierten Modulen. Der IoT Edge-Agent ist verantwortlich für die Bereitstellung der Module, deren Überwachung und das Berichten des Verbindungsstatus an Ihren Azure IoT-Hub.
* `$edgeHub` enthält Daten über die Kommunikation zwischen dem IoT Edge-Hub, der auf einem Gerät ausgeführt wird, und Ihrem Azure IoT-Hub. Dies schließt die Verarbeitung von nachgeschalteten Geräten eingehender Nachrichten ein. Der IoT Edge-Hub ist für die Verarbeitung der Kommunikation zwischen dem Azure IoT-Hub und den IoT Edge-Geräten und -Modulen verantwortlich.

Die Daten werden zusammen mit den gewünschten und gemeldeten Eigenschaftssätzen in den JSON-Strukturen der Modulzwillinge in Metadaten und Tags organisiert. Die gewünschten Eigenschaften, die Sie in der Datei „deployment.json“ angegeben haben, werden in die Modulzwillinge kopiert. Der IoT Edge-Agent und der IoT Edge-Hub aktualisieren die gemeldeten Eigenschaften jeweils für ihre Module.

Entsprechend werden die für Ihre benutzerdefinierten Module in der Datei „deployment.json“ angegebenen gewünschten Eigenschaften in den jeweiligen Modulzwilling kopiert, aber Ihre Lösung ist dafür verantwortlich, die gemeldeten Eigenschaftswerte bereitzustellen.

In diesem Artikel wird beschrieben, wie Sie die Modulzwillinge in Azure-Portal, Azure CLI und Visual Studio Code überprüfen. Informationen dazu, wie Sie überwachen, wie Ihre Geräte die Bereitstellungen erhalten, finden Sie unter [Überwachen von IoT Edge-Bereitstellungen](how-to-monitor-iot-edge-deployments.md). Eine Übersicht über das Konzept von Modulzwillingen finden Sie unter [Verstehen und Verwenden von Modulzwillingen in IoT Hub](../iot-hub/iot-hub-devguide-module-twins.md).

> [!TIP]
> Die gemeldeten Eigenschaften eines Runtimemoduls können veraltet sein, wenn ein IoT Edge-Gerät von seinem IoT-Hub getrennt wird. Sie können das `$edgeAgent`-Modul [pingen](how-to-edgeagent-direct-method.md#ping), um zu ermitteln, ob die Verbindung unterbrochen wurde.

## <a name="monitor-runtime-module-twins"></a>Überwachen von Runtimemodulzwillingen

Überprüfen Sie zum Beheben von Problemen bei der Bereitstellungskonnektivität den IoT Edge-Agent- und IoT Edge-Hub-Runtimemodulzwilling, und führen Sie dann einen Drilldown bei anderen Modulen aus.

### <a name="monitor-iot-edge-agent-module-twin"></a>Überwachen des IoT Edge-Agent-Modulzwillings

Der folgende JSON-Code zeigt den `$edgeAgent`-Modulzwilling in Visual Studio Code, wobei die meisten JSON-Abschnitte reduziert sind.

```json
{
  "deviceId": "Windows109",
  "moduleId": "$edgeAgent",
  "etag": "AAAAAAAAAAU=",
  "deviceEtag": "NzgwNjA1MDUz",
  "status": "enabled",
  "statusUpdateTime": "0001-01-01T00:00:00Z",
  "connectionState": "Disconnected",
  "lastActivityTime": "0001-01-01T00:00:00Z",
  "cloudToDeviceMessageCount": 0,
  "authenticationType": "sas",
  "x509Thumbprint": {
    "primaryThumbprint": null,
    "secondaryThumbprint": null
  },
  "version": 53,
  "properties": {
    "desired": { "···" },
    "reported": {
      "schemaVersion": "1.0",
      "version": { "···" },
      "lastDesiredStatus": { "···" },
      "runtime": { "···" },
      "systemModules": {
        "edgeAgent": { "···" },
        "edgeHub": { "···" }
      },
      "lastDesiredVersion": 5,
      "modules": {
        "SimulatedTemperatureSensor": { "···" }
      },
      "$metadata": { "···" },
      "$version": 48
    }
  }
}
```

Der JSON-Code kann in den folgenden Abschnitten oben beginnend beschrieben werden:

* Metadata: enthält Konnektivitätsdaten. Interessanterweise befindet sich der IoT Edge-Agent immer im Verbindungsstatus „Getrennt“: `"connectionState": "Disconnected"`. Der Grund dafür ist, dass sich der Verbindungsstatus auf D2C-Nachrichten (Device-to-Cloud) bezieht, und der IoT Edge-Agent sendet keine D2C-Nachrichten.
* Properties: enthält die Unterabschnitte `desired` und `reported`.
* Properties.desired (reduziert angezeigt): erwartete Eigenschaftswerte, die vom Operator in der Datei „deployment.json“ festgelegt wurden.
* Properties.reported: zuletzt vom IoT Edge-Agent gemeldete Eigenschaftswerte.

Die Struktur der Abschnitte `properties.desired` und `properties.reported` ist ähnlich und sie enthalten zusätzliche Metadaten für Schema-, Versions- und Runtimeinformationen. Außerdem ist der `modules`-Abschnitt für alle benutzerdefinierten Module (z. B. `SimulatedTemperatureSensor`) eingeschlossen und der `systemModules`-Abschnitt für `$edgeAgent` und die `$edgeHub`-Runtimemodule.

Wenn Sie die gemeldeten Eigenschaftswerte mit den gewünschten Werten vergleichen, können Sie Abweichungen feststellen und Trennungen ermitteln, die Ihnen bei der Problembehandlung helfen können. Überprüfen Sie in diesen Vergleichen den gemeldeten Wert `$lastUpdated` im `metadata`-Abschnitt für die Eigenschaft, die Sie untersuchen.

Die folgenden Eigenschaften sind für die Untersuchung zur Problembehandlung wichtig:

* **Exitcode**: Jeder andere Wert als 0 (null) gibt an, dass das Modul mit einem Fehler angehalten wurde. Allerdings werden die Fehlercodes 137 oder 143 verwendet, wenn für ein Modul absichtlich der Angehalten-Status festgelegt wurde.

* **lastStartTimeUtc**: Zeigt **DateTime** für den letzten Start des Containers an. Dieser Wert ist „0001-01-01T00:00:00Z“, wenn der Container nicht gestartet wurde.

* **lastExitTimeUtc**: Zeigt **DateTime** für das letzte Ausführungsende des Containers an. Dieser Wert ist „0001-01-01T00:00:00Z“, wenn der Container ausgeführt wird und nie angehalten wurde.

* **runtimeStatus**: Kann einen der folgenden Werte aufweisen:

    | Wert | BESCHREIBUNG |
    | --- | --- |
    | unknown | Standardstatus, bis die Bereitstellung erstellt wird. |
    | backoff | Der Start des Moduls ist geplant, es läuft jedoch zurzeit nicht. Dieser Wert ist nützlich für ein Modul, das beim Neustart Statusänderungen durchläuft. Wenn ein fehlerhaftes Modul während des Abkühlzeitraums einen Neustart erwartet, befindet es sich im Status „backoff“. |
    | „Wird ausgeführt“ | Zeigt an, dass das Modul derzeit läuft. |
    | Fehlerhaft | Gibt an, dass bei einem Integritätstest ein Fehler aufgetreten ist oder die Zeit überschritten wurde. |
    | stopped | Gibt an, dass das Modul erfolgreich angehalten wurde (mit einem Exitcode von 0 (null)). |
    | „Fehlgeschlagen“ | Gibt an, dass das Modul mit einem Fehlerexitcode (ungleich 0 (null)) angehalten wurde. Das Modul kann abhängig von der geltenden Neustartrichtlinie von diesem Status zurück zu „backoff“ wechseln. Dieser Status kann darauf hinweisen, dass im Modul ein nicht behebbarer Fehler aufgetreten ist. Ein Fehler tritt auf, wenn der Microsoft Monitoring Agent (MMA) das Modul nicht mehr aktivieren kann, sodass eine neue Bereitstellung erforderlich ist. |

Weitere Informationen finden Sie unter [Gemeldete EdgeAgent-Eigenschaften](module-edgeagent-edgehub.md#edgeagent-reported-properties).

### <a name="monitor-iot-edge-hub-module-twin"></a>Überwachen des IoT Edge-Hub-Modulzwillings

Der folgende JSON-Code zeigt den `$edgeHub`-Modulzwilling in Visual Studio Code, wobei die meisten JSON-Abschnitte reduziert sind.

```json
{
  "deviceId": "Windows109",
  "moduleId": "$edgeHub",
  "etag": "AAAAAAAAAAU=",
  "deviceEtag": "NzgwNjA1MDU2",
  "status": "enabled",
  "statusUpdateTime": "0001-01-01T00:00:00Z",
  "connectionState": "Connected",
  "lastActivityTime": "0001-01-01T00:00:00Z",
  "cloudToDeviceMessageCount": 0,
  "authenticationType": "sas",
  "x509Thumbprint": {
    "primaryThumbprint": null,
    "secondaryThumbprint": null
  },
  "version": 102,
    "properties": {
      "desired": { "···" },
      "reported": {
        "schemaVersion": "1.0",
        "version": { "···" },
      "lastDesiredVersion": 5,
      "lastDesiredStatus": { "···" },
      "clients": {
        "Windows109/SimulatedTemperatureSensor": {
          "status": "Disconnected",
          "lastConnectedTimeUtc": "2020-04-08T21:42:42.1743956Z",
          "lastDisconnectedTimeUtc": "2020-04-09T07:02:42.1398325Z"
        }
      },
      "$metadata": { "···" },
      "$version": 97
    }
  }
}

```

Der JSON-Code kann in den folgenden Abschnitten oben beginnend beschrieben werden:

* Metadata: enthält Konnektivitätsdaten.

* Properties: enthält die Unterabschnitte `desired` und `reported`.
* Properties.desired (reduziert angezeigt): erwartete Eigenschaftswerte, die vom Operator in der Datei „deployment.json“ festgelegt wurden.
* Properties.reported: zuletzt vom IoT Edge-Hub gemeldete Eigenschaftswerte.

Wenn Probleme mit den nachgeschalteten Geräten auftreten, wäre die Untersuchung dieser Daten ein guter Ausgangspunkt.

## <a name="monitor-custom-module-twins"></a>Überwachen benutzerdefinierter Modulzwillinge

Die Informationen zur Konnektivität Ihrer benutzerdefinierten Module werden im IoT Edge-Agent-Modulzwilling verwaltet. Der Modulzwilling für Ihr benutzerdefiniertes Modul wird hauptsächlich zum Verwalten von Daten für Ihre Lösung verwendet. Die gewünschten Eigenschaften, die Sie in der Datei „deployment.json“ definiert haben, spiegeln sich im Modulzwilling wider, und das Modul kann gemeldete Eigenschaftswerte nach Bedarf aktualisieren.

Sie können Ihre bevorzugte Programmiersprache mit den [Azure IoT Hub-Geräte-SDKs](../iot-hub/iot-hub-devguide-sdks.md#azure-iot-hub-device-sdks) verwenden, um gemeldete Eigenschaftswerte im Modulzwilling basierend auf dem Anwendungscode Ihres Moduls zu aktualisieren. Im folgenden Verfahren wird das Azure SDK für .NET hierfür verwendet, wobei Code aus dem [SimulatedTemperatureSensor](https://github.com/Azure/iotedge/blob/dd5be125df165783e4e1800f393be18e6a8275a3/edge-modules/SimulatedTemperatureSensor/src/Program.cs)-Modul verwendet wird:

1. Erstellen Sie eine Instanz des [ModuleClient](/dotnet/api/microsoft.azure.devices.client.moduleclient) mit der [CreateFromEnvironmentAysnc](/dotnet/api/microsoft.azure.devices.client.moduleclient.createfromenvironmentasync)-Methode.

1. Rufen Sie eine Sammlung der Eigenschaften des Modulzwillings mit der [GetTwinAsync](/dotnet/api/microsoft.azure.devices.client.moduleclient.gettwinasync)-Methode ab.

1. Erstellen Sie einen Listener (Übergabe eines Rückrufs), um Änderungen an den gewünschten Eigenschaften mit der [SetDesiredPropertyUpdateCallbackAsync](/dotnet/api/microsoft.azure.devices.client.deviceclient.setdesiredpropertyupdatecallbackasync)-Methode abzufangen.

1. Aktualisieren Sie in Ihrer Rückrufmethode die gemeldeten Eigenschaften im Modulzwilling mit der [UpdateReportedPropertiesAsync](/dotnet/api/microsoft.azure.devices.client.moduleclient)-Methode, und übergeben Sie dabei eine [TwinCollection](/dotnet/api/microsoft.azure.devices.shared.twincollection) der Eigenschaftswerte, die Sie festlegen möchten.

## <a name="access-the-module-twins"></a>Zugreifen auf die Modulzwillinge

Sie können den JSON-Code für Modulzwillinge in Azure IoT Hub, in Visual Studio Code und mit Azure CLI überprüfen.

### <a name="monitor-in-azure-iot-hub"></a>Überwachen in Azure IoT Hub

So zeigen Sie den JSON-Code für den Modulzwilling an:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrem IoT Hub.
1. Wählen Sie im Menü auf der linken Seite die Option **IoT Edge** aus.
1. Wählen Sie auf der Registerkarte **IoT Edge-Geräte** die **Geräte-ID** des Geräts mit den Modulen aus, die Sie überwachen möchten.
1. Wählen Sie auf der Registerkarte **Module** den Modulnamen aus, und wählen Sie dann in der oberen Menüleiste **Zwilling der Modulkennung** aus.

  ![Auswählen eines Modulzwillings zur Anzeige im Azure-Portal](./media/how-to-monitor-module-twins/select-module-twin.png)

Wenn die Meldung „Eine Modulkennung ist für dieses Modul nicht vorhanden“ angezeigt wird, weist dieser Fehler darauf hin, dass die Back-End-Lösung, die ursprünglich die Kennung erstellt hat, nicht mehr verfügbar ist.

### <a name="monitor-module-twins-in-visual-studio-code"></a>Überwachen von Modulzwillingen in Visual Studio Code

So überprüfen und bearbeiten Sie einen Modulzwilling:

1. Falls noch nicht installiert, installieren Sie die [Azure IoT Tools-Erweiterung](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) für Visual Studio Code.
1. Erweitern Sie im **Explorer** die Option **Azure IoT Hub**, und erweitern Sie dann das Gerät mit dem Modul, das Sie überwachen möchten.
1. Klicken Sie mit der rechten Maustaste auf das Modul, und wählen Sie **Modulzwilling bearbeiten** aus. Eine temporäre Datei des Modulzwillings wird auf Ihren Computer heruntergeladen und in Visual Studio Code angezeigt.

  ![Abrufen eines Modulzwillings zum Bearbeiten in Visual Studio Code](./media/how-to-monitor-module-twins/edit-module-twin-vscode.png)

Wenn Sie Änderungen vornehmen, wählen Sie **Modulzwilling aktualisieren** oberhalb des Codes im Editor aus, um die Änderungen in Ihrem IoT-Hub zu speichern.

  ![Aktualisieren eines Modulzwillings in Visual Studio Code](./media/how-to-monitor-module-twins/update-module-twin-vscode.png)

### <a name="monitor-module-twins-in-azure-cli"></a>Überwachen von Modulzwillingen in Azure CLI

Um festzustellen, ob IoT Edge ausgeführt wird, pingen Sie mit [az iot hub invoke-module-method](how-to-edgeagent-direct-method.md#ping) den IoT Edge-Agent.

Die [az iot hub module-twin](/cli/azure/ext/azure-iot/iot/hub/module-twin)-Struktur bietet folgende Befehle:

* **az iot hub module-twin show**: Zeigt eine Modulzwillingsdefinition an.
* **az iot hub module-twin update**: Aktualisiert eine Modulzwillingsdefinition.
* **az iot hub module-twin replace**: Ersetzt eine Modulzwillingsdefinition durch einen JSON-Zielcode.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [über integrierte direkte Methoden mit EdgeAgent kommunizieren](how-to-edgeagent-direct-method.md).
