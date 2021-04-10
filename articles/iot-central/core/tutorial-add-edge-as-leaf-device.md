---
title: 'Tutorial: Hinzufügen eines Azure IoT Edge-Geräts zu Azure IoT Central | Microsoft-Dokumentation'
description: 'Tutorial: Fügen Sie Ihrer Azure IoT Central-Anwendung als Operator ein Azure IoT Edge-Gerät hinzu.'
author: rangv
ms.author: rangv
ms.date: 05/29/2020
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom:
- mvc
- device-developer
- iot-edge
ms.openlocfilehash: 373d144b4df818a075f0088e9cbf31cb5027e747
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "101724879"
---
# <a name="tutorial-add-an-azure-iot-edge-device-to-your-azure-iot-central-application"></a>Tutorial: Hinzufügen eines Azure IoT Edge-Geräts zu Ihrer Azure IoT Central-Anwendung

*Dieser Artikel richtet sich an Operators, Lösungsentwickler und Geräteentwickler.*

In diesem Tutorial erfahren Sie, wie Sie ein Azure IoT Edge-Gerät konfigurieren und Ihrer Azure IoT Central-Anwendung hinzufügen. Hierzu wird mithilfe eines IoT Edge-fähigen virtuellen Linux-Computers (VM) ein IoT Edge-Gerät simuliert. Das IoT Edge-Gerät verwendet ein Modul, das simulierte Telemetriedaten der Umgebung generiert. Die Telemetriedaten werden auf einem Dashboard in Ihrer IoT Central-Anwendung angezeigt.

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

In diesem Abschnitt erstellen Sie eine IoT Central-Gerätevorlage für ein IoT Edge-Gerät. Sie importieren zunächst ein IoT Edge-Manifest und ändern dann die Vorlage, um Telemetriedefinitionen und Ansichten hinzuzufügen:

### <a name="import-manifest-to-create-template"></a>Importieren eines Manifests für die Vorlagenerstellung

So erstellen Sie eine Gerätevorlage auf der Grundlage eines IoT Edge-Manifests:

1. Navigieren Sie in Ihrer IoT Central-Anwendung zu **Gerätevorlagen**, und wählen Sie **+ Neu** aus.

1. Wählen Sie auf der Seite **Vorlagentyp auswählen** die Kachel **Azure IoT Edge** aus. Wählen Sie anschließend **Next: Anpassen**.

1. Geben Sie auf der Seite **Azure IoT Edge-Bereitstellungsmanifest hochladen** den Text *Environmental Sensor Edge Device* als Namen für die Gerätevorlage ein. Wählen Sie anschließend **Durchsuchen** aus, um die zuvor heruntergeladene Datei **EnvironmentalSensorManifest.json** hochzuladen. Wählen Sie anschließend **Next: Review** (Weiter: Überprüfen).

1. Wählen Sie auf der Seite **Überprüfen** die Option **Erstellen** aus.

1. Wählen Sie im Modul **SimulatedTemperatureSensor** die Oberfläche **Verwalten** aus, um die beiden im Manifest definierten Eigenschaften anzuzeigen:

:::image type="content" source="media/tutorial-add-edge-as-leaf-device/imported-manifest.png" alt-text="Auf Grundlage eines IoT Edge-Manifests erstellte Gerätevorlage":::

> [!TIP]
> Das Bereitstellungsmanifest pullt Modul-Images aus einem Azure Container Registry-Repository, bei dem zum Herstellen einer Verbindung keine Anmeldeinformationen erforderlich sind. Wenn Sie Modul-Images aus einem privaten Repository verwenden möchten, legen Sie die Anmeldeinformationen für die Containerregistrierung im Manifest fest.

### <a name="add-telemetry-to-manifest"></a>Hinzufügen von Telemetriedaten zu Ihrem Manifest

Die von einem Modul gesendeten Telemetriedaten werden nicht in einem IoT Edge-Manifest definiert. Sie fügen die Telemetriedefinitionen der Gerätevorlage in IoT Central hinzu. Das Modul **SimulatedTemperatureSensor** sendet Telemetrienachrichten, die wie der folgende JSON-Code aussehen:

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

:::image type="content" source="media/tutorial-add-edge-as-leaf-device/manage-interface.png" alt-text="Oberfläche mit den Telemetrietypen „machine“ und „ambient“":::

### <a name="add-views-to-template"></a>Hinzufügen von Ansichten zur Vorlage

Die Gerätevorlage verfügt noch über keine Ansicht, die einem Bediener das Anzeigen von Telemetriedaten des IoT Edge-Geräts ermöglicht. So fügen Sie der Gerätevorlage eine Ansicht hinzu:

1. Wählen Sie in der Vorlage **Environmental Sensor Edge Device** die Option **Ansichten** aus.

1. Wählen Sie auf der Seite **Hinzufügen einer neuen Ansicht auswählen** die Kachel **Das Gerät wird visualisiert** aus.

1. Ändern Sie den Namen der Ansicht in *View IoT Edge device telemetry*.

1. Wählen Sie die Telemetrietypen **ambient** und **machine** aus. Wählen Sie dann **Kachel hinzufügen** aus.

1. Wählen Sie **Speichern** aus, um die Ansicht **View IoT Edge device telemetry** zu speichern.

:::image type="content" source="media/tutorial-add-edge-as-leaf-device/template-telemetry-view.png" alt-text="Gerätevorlage mit Telemetrieansicht":::

### <a name="publish-the-template"></a>Veröffentlichen der Vorlage

Bevor Sie ein Gerät hinzufügen können, das die Vorlage **Environmental Sensor Edge Device** verwendet, müssen Sie die Vorlage veröffentlichen.

Navigieren Sie zur Vorlage **Environmental Sensor Edge Device**, und wählen Sie **Veröffentlichen** aus. Wählen Sie im Bereich **Diese Gerätevorlage für die Anwendung veröffentlichen** die Option **Veröffentlichen** aus, um die Vorlage zu veröffentlichen:

:::image type="content" source="media/tutorial-add-edge-as-leaf-device/publish-template.png" alt-text="Veröffentlichen der Gerätevorlage":::

## <a name="add-iot-edge-device"></a>Hinzufügen eines IoT Edge-Geräts

Nachdem Sie die Vorlage **Environmental Sensor Edge Device** veröffentlicht haben, können Sie Ihrer IoT Central-Anwendung ein Gerät hinzufügen:

1. Navigieren Sie in Ihrer IoT Central-Anwendung zur Seite **Geräte**, und wählen Sie in der Liste mit den verfügbaren Vorlagen die Vorlage **Environmental Sensor Edge Device** aus.

1. Wählen Sie **+ Neu** aus, um basierend auf der Vorlage ein neues Gerät hinzuzufügen. Wählen Sie auf der Seite **Neues Gerät erstellen** die Option **Erstellen** aus.

Sie verfügen nun über ein neues Gerät mit dem Status **Registriert**:

:::image type="content" source="media/tutorial-add-edge-as-leaf-device/new-device.png" alt-text="Neues registriertes Gerät":::

### <a name="get-the-device-credentials"></a>Abrufen der Geräteanmeldeinformationen

Bei der späteren Bereitstellung des IoT Edge-Geräts in diesem Tutorial benötigen Sie die Anmeldeinformationen, die es dem Gerät ermöglichen, eine Verbindung mit Ihrer IoT Central-Anwendung herzustellen. So rufen Sie die Anmeldeinformationen ab:

1. Wählen Sie auf der Seite **Gerät** das von Ihnen erstellte Gerät aus.

1. Wählen Sie **Verbinden**.

1. Notieren Sie sich auf der Seite **Geräteverbindung** die **Bereichs-ID**, die **Geräte-ID** und den **Primärschlüssel**. Diese Werte werden später benötigt.

1. Klicken Sie auf **Schließen**.

Ihre IoT Central-Anwendung ist nun so weit konfiguriert, dass von einem IoT Edge-Gerät eine Verbindung hergestellt werden kann.

## <a name="deploy-an-iot-edge-device"></a>Bereitstellen eines IoT Edge-Geräts

In diesem Tutorial verwenden Sie einen in Azure erstellten Azure IoT Edge-fähigen virtuellen Linux-Computer, um ein IoT Edge-Gerät zu simulieren. Klicken Sie auf die folgende Schaltfläche, um die IoT Edge-fähige VM in Ihrem Azure-Abonnement zu erstellen:

[![Schaltfläche „In Azure bereitstellen“ für „iotedge-vm-deploy“](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fiotedge-vm-deploy%2Fmaster%2FedgeDeploy.json)

Gehen Sie auf der Seite **Benutzerdefinierte Bereitstellung** wie folgt vor:

1. Wählen Sie Ihr Azure-Abonnement.

1. Wählen Sie **Neu erstellen** aus, um eine neue Ressourcengruppe mit dem Namen *central-edge-rg* zu erstellen.

1. Wählen Sie eine Region in Ihrer Nähe aus.

1. Fügen Sie ein eindeutiges **Präfix der DNS-Bezeichnung** hinzu, z. B. *contoso-central-edge*.

1. Wählen Sie einen Administratorbenutzernamen für den virtuellen Computer aus.

1. Geben Sie *temp* als Verbindungszeichenfolge ein. Später konfigurieren Sie das Gerät so, dass die Verbindung per DPM hergestellt wird.

1. Übernehmen Sie die Standardwerte für die VM-Größe, die Ubuntu-Version und den Standort.

1. Wählen Sie als Authentifizierungstyp die Option **Kennwort** aus.

1. Geben Sie ein Kennwort für die VM ein.

1. Wählen Sie dann **Überprüfen + erstellen** aus.

1. Überprüfen Sie Ihre Auswahl, und wählen Sie anschließend **Erstellen** aus:

    :::image type="content" source="media/tutorial-add-edge-as-leaf-device/vm-deployment.png" alt-text="IoT Edge-VM erstellen":::

Die Bereitstellung nimmt einige Minuten in Anspruch. Navigieren Sie im Azure-Portal zur Ressourcengruppe **central-edge-rg**, nachdem die Bereitstellung abgeschlossen ist.

### <a name="configure-the-iot-edge-vm"></a>Konfigurieren der IoT Edge-VM

Gehen Sie wie folgt vor, um IoT Edge auf der VM für die Verwendung des DPM zum Registrieren und Herstellen der Verbindung mit Ihrer IoT Central-Anwendung zu konfigurieren:

1. Wählen Sie in der Ressourcengruppe **contoso-edge-rg** die Instanz des virtuellen Computers aus.

1. Wählen Sie im Abschnitt **Support + Problembehandlung** die Option **Serielle Konsole** aus. Befolgen Sie die Anweisungen im Portal, wenn Sie zum Konfigurieren der Startdiagnose aufgefordert werden.

1. Drücken Sie die **EINGABETASTE**, um die Eingabeaufforderung `login:` anzuzeigen. Geben Sie Ihren Benutzernamen und Ihr Kennwort ein, um sich anzumelden.

1. Führen Sie den folgenden Befehl aus, um die IoT Edge-Runtimeversion zu überprüfen. Zum Zeitpunkt der Artikelerstellung lautet die Version 1.0.9.1:

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
    #  device_connection_string: "temp"
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

    > [!TIP]
    > Vergewissern Sie sich, dass vor `provisioning:` keine Leerstellen mehr vorhanden sind.

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

    In der folgenden Beispielausgabe sind die ausgeführten Module enthalten:

    ```bash
    NAME                        STATUS           DESCRIPTION      CONFIG
    SimulatedTemperatureSensor  running          Up 20 seconds    mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0
    edgeAgent                   running          Up 27 seconds    mcr.microsoft.com/azureiotedge-agent:1.0
    edgeHub                     running          Up 22 seconds    mcr.microsoft.com/azureiotedge-hub:1.0
    ```

    > [!TIP]
    > Unter Umständen müssen Sie warten, bis alle Module gestartet wurden.

## <a name="view-the-telemetry"></a>Anzeigen der Telemetrie

Das simulierte IoT Edge-Gerät wird nun auf dem virtuellen Computer ausgeführt. In Ihrer IoT Central-Anwendung lautet der Gerätestatus auf der Seite **Geräte** nun **Bereitgestellt**:

:::image type="content" source="media/tutorial-add-edge-as-leaf-device/provisioned-device.png" alt-text="Bereitgestelltes IoT Edge-Gerät":::

Die Telemetriedaten des Geräts werden auf der Seite **View IoT Edge device telemetry** (Telemetriedaten des IoT Edge-Geräts anzeigen) angezeigt:

:::image type="content" source="media/tutorial-add-edge-as-leaf-device/device-telemetry-view.png" alt-text="Gerätetelemetrie":::

Auf der Seite **Module** wird der Status der IoT Edge-Module des Geräts angezeigt:

:::image type="content" source="media/tutorial-add-edge-as-leaf-device/edge-module-status.png" alt-text="Status der Gerätemodule":::

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie die IoT Edge-VM weiter nutzen möchten, können Sie die in diesem Tutorial verwendeten Ressourcen beibehalten und wiederverwenden. Andernfalls können Sie die in diesem Tutorial erstellten Ressourcen löschen, um zusätzliche Gebühren zu vermeiden:

* Um die IoT Edge-VM und die zugehörigen Ressourcen zu löschen, müssen Sie im Azure-Portal die Ressourcengruppe **contoso-edge-rg** löschen.
* Navigieren Sie zum Löschen der IoT Central-Anwendung im Abschnitt **Verwaltung** der Anwendung zur Seite **Ihre Anwendung**, und wählen Sie **Löschen** aus.

Nachdem Sie als Lösungsentwickler bzw. Operator nun wissen, wie Sie mit IoT Edge-Geräten in IoT Central arbeiten und diese Geräte verwalten, empfehlen wir Ihnen als Nächstes den folgenden Artikel:

> [!div class="nextstepaction"]
> [Verwenden von Gerätegruppen zum Analysieren von Gerätetelemetriedaten](./tutorial-use-device-groups.md)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie als Geräteentwickler nun wissen, wie Sie mit IoT Edge-Geräten in IoT Central arbeiten und wie Sie diese Geräte verwalten, wird als Nächstes der folgende Artikel empfohlen:

> [!div class="nextstepaction"]
> [Entwickeln von IoT Edge-Modulen](../../iot-edge/tutorial-develop-for-linux.md)