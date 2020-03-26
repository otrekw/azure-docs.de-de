---
title: Hinzufügen eines Azure IoT Edge-Geräts zu Azure IoT Central | Microsoft-Dokumentation
description: Fügen Sie Ihrer Azure IoT Central-Anwendung als Bediener ein Azure IoT Edge-Gerät hinzu.
author: rangv
ms.author: rangv
ms.date: 12/09/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: bf74784998de6bbad6310c48c24d6353512bff13
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "77026393"
---
# <a name="tutorial-add-an-azure-iot-edge-device-to-your-azure-iot-central-application"></a>Tutorial: Hinzufügen eines Azure IoT Edge-Geräts zu Ihrer Azure IoT Central-Anwendung

In diesem Tutorial erfahren Sie, wie Sie ein Azure IoT Edge-Gerät konfigurieren und Ihrer Azure IoT Central-Anwendung hinzufügen. Hierzu wird mithilfe eines IoT Edge-fähigen virtuellen Linux-Computers (Virtual Machine, VM) aus dem Azure Marketplace ein IoT Edge-Gerät simuliert. Das IoT Edge-Gerät verwendet ein Modul, das simulierte Telemetriedaten der Umgebung generiert. Die Telemetriedaten werden auf einem Dashboard in Ihrer IoT Central-Anwendung angezeigt.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Gerätevorlage für ein IoT Edge-Gerät
> * Erstellen eines IoT Edge-Geräts in IoT Central
> * Bereitstellen eines simulierten IoT Edge-Geräts auf einem virtuellen Linux-Computer

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die Schritte der Schnellstartanleitung [Erstellen einer Azure IoT Central-Anwendung](./quick-deploy-iot-central.md) aus, um mit der Vorlage **Benutzerdefinierte App > Benutzerdefinierte Anwendung** eine IoT Central-Anwendung zu erstellen.

Für die Schritte in diesem Tutorial benötigen Sie ein aktives Azure-Abonnement.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Laden Sie die IoT Edge-Manifestdatei von GitHub herunter. Klicken Sie mit der rechten Maustaste auf den folgenden Link, und wählen Sie **Link speichern unter...** aus: [EnvironmentalSensorManifest.json](https://raw.githubusercontent.com/Azure-Samples/iot-central-docs-samples/master/iotedge/EnvironmentalSensorManifest.json)

## <a name="create-device-template"></a>Erstellen einer Gerätevorlage

In diesem Abschnitt erstellen Sie eine Gerätevorlage für ein IoT Edge-Gerät, das eine Verbindung mit Ihrer IoT Central-Anwendung herstellt. Sie importieren zunächst ein IoT Edge-Manifest und ändern dann die Vorlage, um Telemetriedefinitionen und Ansichten hinzuzufügen:

### <a name="import-manifest-to-create-template"></a>Importieren eines Manifests für die Vorlagenerstellung

So erstellen Sie eine Gerätevorlage auf der Grundlage eines IoT Edge-Manifests:

1. Navigieren Sie in Ihrer IoT Central-Anwendung zu **Gerätevorlagen**, und wählen Sie **+ Neu** aus.

1. Wählen Sie auf der Seite **Vorlagentyp auswählen** die Kachel **Azure IoT Edge** aus. Wählen Sie anschließend **Next: Anpassen**.

1. Wählen Sie auf der Seite **Azure IoT Edge-Bereitstellungsmanifest hochladen** die Option **Durchsuchen** aus, um die zuvor heruntergeladene Datei **EnvironmentalSensorManifest.json** hochzuladen. Wählen Sie anschließend **Next: Review** (Weiter: Überprüfen).

1. Wählen Sie auf der Seite **Überprüfen** die Option **Erstellen** aus.

1. Wenn die Vorlage erstellt wurde, ändern Sie den Namen der Vorlage in *Environmental Sensor Edge Device*.

1. Wählen Sie im Modul **SimulatedTemperatureSensor** die Oberfläche **Verwalten** aus, um die beiden im Manifest definierten Eigenschaften anzuzeigen:

![Auf der Grundlage eines IoT Edge-Manifests erstellte Gerätevorlage](./media/tutorial-add-edge-as-leaf-device/imported-manifest.png)

### <a name="add-telemetry-to-manifest"></a>Hinzufügen von Telemetriedaten zu Ihrem Manifest

Die von einem Modul gesendeten Telemetriedaten werden nicht in einem IoT Edge-Manifest definiert. Die Telemetriedefinitionen müssen der Gerätevorlage hinzugefügt werden. Das Modul **SimulatedTemperatureSensor** sendet Telemetrienachrichten, die wie der folgende JSON-Code aussehen:

```json
{
    "machine": {
        "temperature": 75.0,
        "pressure": 40.2
    },
    "ambient": {
        "temperature": 23.0,
        "humidity": 30.0
    },
    "timeCreated": ""
}
```

So fügen Sie die Telemetriedefinitionen der Gerätevorlage hinzu:

1. Wählen Sie in der Vorlage **Environmental Sensor Edge Device** die Oberfläche **Verwalten** aus.

1. Wählen Sie **+ Funktion hinzufügen** aus. Geben Sie *machine* als **Anzeigename** ein, und vergewissern Sie sich, dass der **Funktionstyp** auf **Telemetrie** festgelegt ist.

1. Wählen Sie als Schematyp die Option **Objekt** und anschließend **Definieren** aus. Fügen Sie auf der Objektdefinitionsseite *temperature* und *pressure* als Attribute vom Typ **Double** hinzu, und wählen Sie anschließend **Anwenden** aus.

1. Wählen Sie **+ Funktion hinzufügen** aus. Geben Sie *ambient* als **Anzeigename** ein, und vergewissern Sie sich, dass der **Funktionstyp** auf **Telemetrie** festgelegt ist.

1. Wählen Sie als Schematyp die Option **Objekt** und anschließend **Definieren** aus. Fügen Sie auf der Objektdefinitionsseite *temperature* und *humidity* als Attribute vom Typ **Double** hinzu, und wählen Sie anschließend **Anwenden** aus.

1. Wählen Sie **+ Funktion hinzufügen** aus. Geben Sie *timeCreated* als **Anzeigename** ein, und vergewissern Sie sich, dass der **Funktionstyp** auf **Telemetrie** festgelegt ist.

1. Wählen Sie als Schematyp die Option **DateTime** aus.

1. Wählen Sie **Speichern** aus, um die Vorlage zu aktualisieren.

Auf der Oberfläche **Verwalten** befinden sich nun die Telemetrietypen **machine**, **ambient** und **timeCreated**:

![Oberfläche mit den Telemetrietypen „machine“ und „ambient“](./media/tutorial-add-edge-as-leaf-device/manage-interface.png)

### <a name="add-views-to-template"></a>Hinzufügen von Ansichten zur Vorlage

Die Gerätevorlage verfügt noch über keine Ansicht, die einem Bediener das Anzeigen von Telemetriedaten des IoT Edge-Geräts ermöglicht. So fügen Sie der Gerätevorlage eine Ansicht hinzu:

1. Wählen Sie in der Vorlage **Environmental Sensor Edge Device** die Option **Ansichten** aus.

1. Wählen Sie auf der Seite **Hinzufügen einer neuen Ansicht auswählen** die Kachel **Das Gerät wird visualisiert** aus.

1. Ändern Sie den Namen der Ansicht in *View IoT Edge device telemetry*.

1. Wählen Sie die Telemetrietypen **ambient** und **machine** aus. Wählen Sie dann **Kachel hinzufügen** aus.

1. Wählen Sie **Speichern** aus, um die Ansicht **View IoT Edge device telemetry** zu speichern.

![Gerätevorlage mit Telemetrieansicht](./media/tutorial-add-edge-as-leaf-device/template-telemetry-view.png)

### <a name="publish-the-template"></a>Veröffentlichen der Vorlage

Bevor Sie ein Gerät hinzufügen können, das die Vorlage **Environmental Sensor Edge Device** verwendet, müssen Sie die Vorlage veröffentlichen.

Navigieren Sie zur Vorlage **Environmental Sensor Edge Device**, und wählen Sie **Veröffentlichen** aus. Wählen Sie anschließend **Veröffentlichen** aus, um die Vorlage zu veröffentlichen:

![Veröffentlichen der Gerätevorlage](./media/tutorial-add-edge-as-leaf-device/publish-template.png)

## <a name="add-iot-edge-device"></a>Hinzufügen eines IoT Edge-Geräts

Nachdem Sie die Vorlage **Environmental Sensor Edge Device** veröffentlicht haben, können Sie Ihrer IoT Central-Anwendung ein Gerät hinzufügen:

1. Navigieren Sie in Ihrer IoT Central-Anwendung zur Seite **Geräte**, und wählen Sie in der Liste mit den verfügbaren Vorlagen die Vorlage **Environmental Sensor Edge Device** aus.

1. Wählen Sie **+** aus, um ein neues Gerät auf der Grundlage der Vorlage hinzuzufügen. Wählen Sie auf der Seite **Neues Gerät erstellen** die Option **Erstellen** aus.

Sie verfügen nun über ein neues Gerät mit dem Status **Registriert**:

![Veröffentlichen der Gerätevorlage](./media/tutorial-add-edge-as-leaf-device/new-device.png)

### <a name="get-the-device-credentials"></a>Abrufen der Geräteanmeldeinformationen

Bei der späteren Bereitstellung des IoT Edge-Geräts in diesem Tutorial benötigen Sie die Anmeldeinformationen, die es dem Gerät ermöglichen, eine Verbindung mit Ihrer IoT Central-Anwendung herzustellen. So rufen Sie die Anmeldeinformationen ab:

1. Wählen Sie auf der Seite **Gerät** das von Ihnen erstellte Gerät aus.

1. Wählen Sie **Verbinden**.

1. Notieren Sie sich auf der Seite **Geräteverbindung** die **Bereichs-ID**, die **Geräte-ID** und den **Primärschlüssel**. Diese Werte werden später benötigt.

1. Klicken Sie auf **Schließen**.

Ihre IoT Central-Anwendung ist nun so weit konfiguriert, dass von einem IoT Edge-Gerät eine Verbindung hergestellt werden kann.

## <a name="deploy-an-iot-edge-device"></a>Bereitstellen eines IoT Edge-Geräts

In diesem Tutorial verwenden Sie einen in Azure erstellten Azure IoT Edge-fähigen virtuellen Linux-Computer, um ein IoT Edge-Gerät zu simulieren. So erstellen Sie den IoT Edge-fähigen virtuellen Computer:

1. Navigieren Sie im Azure Marketplace zu [Azure IoT Edge on Ubuntu](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft_iot_edge.iot_edge_vm_ubuntu?tab=Overview). Wählen Sie **Jetzt holen** aus.

1. Wählen Sie auf der Seite **Diese App in Azure erstellen** die Option **Weiter** aus. Über diesen Link gelangen Sie zum Azure-Portal, wo Sie sich unter Umständen bei Ihrem Azure-Abonnement anmelden müssen.

1. Wählen Sie im Azure-Portal auf der Seite **Azure IoT Edge on Ubuntu** die Option **Erstellen** aus.

1. Gehen Sie auf der Seite **Virtuellen Computer erstellen > Grundlegende Einstellungen** wie folgt vor:

    - Wählen Sie Ihr Azure-Abonnement.
    - Erstellen Sie eine neue Ressourcengruppe namens **iot-edge-devices**.
    - Verwenden Sie den VM-Namen **iotedgevm**.
    - Wählen Sie die nächstgelegene Region aus.
    - Legen Sie den Authentifizierungstyp auf **Kennwort** fest.
    - Wählen Sie einen Benutzernamen und ein Kennwort.
    - Bei den anderen Optionen können Sie die Standardwerte übernehmen.
    - Klicken Sie auf **Überprüfen + erstellen**.

1. Wählen Sie nach Abschluss der Überprüfung die Option **Erstellen** aus.

Die Bereitstellung dauert ein paar Minuten. Wählen Sie nach Abschluss der Bereitstellung die Option **Zu Ressourcengruppe wechseln** aus.

### <a name="provision-vm-as-an-iot-edge-device"></a>Bereitstellen eines virtuellen Computers als IoT Edge-Gerät 

So stellen Sie einen virtuellen Computer als IoT Edge-Gerät bereit:

1. Wählen Sie im Abschnitt **Support + Problembehandlung** die Option **Serielle Konsole** aus.

1. Drücken Sie die **EINGABETASTE**, um die Eingabeaufforderung `login:` anzuzeigen. Geben Sie Ihren Benutzernamen und Ihr Kennwort ein, um sich anzumelden.

1. Führen Sie den folgenden Befehl aus, um die IoT Edge-Runtimeversion zu überprüfen. Zum Zeitpunkt der Artikelerstellung lautet die Version 1.0.8:

    ```bash
    sudo iotedge --version
    ```

1. Öffnen Sie die IoT Edge-Datei „config.yaml“ im `nano`-Editor:

    ```bash
    sudo nano /etc/iotedge/config.yaml
    ```

1. Scrollen Sie nach unten zu `# Manual provisioning configuration`. Kommentieren Sie die nächsten drei Zeilen aus, wie im folgenden Codeausschnitt gezeigt:

    ```yaml
    # Manual provisioning configuration
    #provisioning:
    #  source: "manual"
    #  device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
    ```

1. Scrollen Sie nach unten zu `# DPS symmetric key provisioning configuration`. Heben Sie die Auskommentierung der nächsten acht Zeilen aus, wie im folgenden Codeausschnitt gezeigt:

    ```yaml
    # DPS symmetric key provisioning configuration
    provisioning:
      source: "dps"
      global_endpoint: "https://global.azure-devices-provisioning.net"
      scope_id: "{scope_id}"
      attestation:
        method: "symmetric_key"
        registration_id: "{registration_id}"
        symmetric_key: "{symmetric_key}"
    ```

1. Ersetzen Sie `{scope_id}` durch die **Bereichs-ID**, die Sie sich zuvor notiert haben.

1. Ersetzen Sie `{registration_id}` durch die **Geräte-ID**, die Sie sich zuvor notiert haben.

1. Ersetzen Sie `{symmetric_key}` durch den **Primärschlüssel**, den Sie sich zuvor notiert haben.

1. Speichern Sie die Änderungen durch Drücken von **STRG+O**, und beenden Sie den `nano`-Editor durch Drücken von **STRG+X**.

1. Führen Sie den folgenden Befehl aus, um den IoT Edge-Daemon neu zu starten:

    ```bash
    sudo systemctl restart iotedge
    ```

1. Führen Sie den folgenden Befehl aus, um den Status der IoT Edge-Module zu überprüfen:

    ```bash
    iotedge list
    ```

    Die Ausgabe sieht wie folgt aus:

    ```bash
    NAME                        STATUS           DESCRIPTION      CONFIG
    SimulatedTemperatureSensor  running          Up 20 seconds    mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0
    edgeAgent                   running          Up 27 seconds    mcr.microsoft.com/azureiotedge-agent:1.0
    edgeHub                     running          Up 22 seconds    mcr.microsoft.com/azureiotedge-hub:1.0
    ```

## <a name="view-the-telemetry"></a>Anzeigen der Telemetrie

Das simulierte IoT Edge-Gerät wird nun auf dem virtuellen Computer ausgeführt. In Ihrer IoT Central-Anwendung lautet der Gerätestatus auf der Seite **Geräte** nun **Bereitgestellt**:

![Bereitgestelltes Gerät](./media/tutorial-add-edge-as-leaf-device/provisioned-device.png)

Die Telemetriedaten werden auf der Seite **View IoT Edge device telemetry** angezeigt:

![Gerätetelemetrie](./media/tutorial-add-edge-as-leaf-device/device-telemetry-view.png)

Auf der Seite **Module** wird der Status der IoT Edge-Module angezeigt:

![Gerätetelemetrie](./media/tutorial-add-edge-as-leaf-device/edge-module-status.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun wissen, wie Sie mit IoT Edge-Geräten in IoT Central arbeiten, wird als Nächstes der folgende Schritt empfohlen:

<!-- Next how-tos in the sequence -->

> [!div class="nextstepaction"]
> [Konfigurieren eines transparenten Gateways](../../iot-edge/how-to-create-transparent-gateway.md)
