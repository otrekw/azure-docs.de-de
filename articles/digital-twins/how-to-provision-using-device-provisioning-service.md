---
title: Automatisches Verwalten von Geräten mithilfe des Device Provisioning Service
titleSuffix: Azure Digital Twins
description: Hier erfahren Sie, wie Sie einen automatisierten Prozess für die Bereitstellung und Außerbetriebnahme von IoT-Geräten in Azure Digital Twins mit dem Device Provisioning Service (DPS) einrichten.
author: baanders
ms.author: baanders
ms.date: 3/21/2021
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: ad1351b7c9a649a553ce54422b99a13c286437d6
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107107294"
---
# <a name="auto-manage-devices-in-azure-digital-twins-using-device-provisioning-service-dps"></a>Automatisches Verwalten von Geräten in Azure Digital Twins mithilfe des Device Provisioning Service (DPS)

In diesem Artikel erfahren Sie, wie Sie Azure Digital Twins in den [Device Provisioning Service (DPS)](../iot-dps/about-iot-dps.md) integrieren.

Mithilfe der in diesem Artikel beschriebenen Lösung können Sie den Prozess zum **_Bereitstellen_** und **_Außerbetriebnehmen_** von IoT Hub-Geräten in Azure Digital Twins mithilfe des Device Provisioning Service automatisieren. 

Weitere Informationen zu den Phasen der _Bereitstellung_ und _Außerbetriebnahme_ und zu den Phasen der allgemeinen Geräteverwaltung, die in allen Unternehmens-IoT-Projekten gelten, finden Sie im [Abschnitt *Lebenszyklus von Geräten*](../iot-hub/iot-hub-device-management-overview.md#device-lifecycle) der Dokumentation zur IoT Hub-Geräteverwaltung.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie die Bereitstellung einrichten können, müssen Sie Folgendes einrichten:
* Eine **Azure Digital Twins-Instanz**. Befolgen Sie zum Erstellen einer Azure Digital Twins-Instanz die Anweisungen unter [*Vorgehensweise: Einrichten einer Instanz und der Authentifizierung*](how-to-set-up-instance-portal.md). Erfassen Sie den **_Hostnamen_** der Instanz im Azure-Portal ([Anweisungen](how-to-set-up-instance-portal.md#verify-success-and-collect-important-values)).
* Einen **IoT-Hub**. Anweisungen finden Sie im Abschnitt *Erstellen eines IoT-Hubs* in diesem [IoT Hub-Schnellstart](../iot-hub/quickstart-send-telemetry-cli.md).
* Eine [**Azure-Funktion**](../azure-functions/functions-overview.md), mit der Digital Twins-Informationen auf Grundlage von IoT-Hub-Daten aktualisiert werden. Befolgen Sie zum Erstellen dieser Azure-Funktion die Anweisungen in der [*Anleitung zum Erfassen von IoT-Hub-Daten*](how-to-ingest-iot-hub-data.md). Erfassen Sie den **_Namen_** der Funktion, um ihn in diesem Artikel zu verwenden.

In diesem Beispiel wird auch ein **Gerätesimulator** verwendet, der die Bereitstellung mithilfe des Device Provisioning Service umfasst. Der Gerätesimulator befindet sich hier: [Beispiel für die Integration von Azure Digital Twins und IoT Hub](/samples/azure-samples/digital-twins-iothub-integration/adt-iothub-provision-sample/). Laden Sie das Beispielprojekt auf Ihren Computer herunter, indem Sie zum Beispiellink navigieren und die Schaltfläche *ZIP-Datei herunterladen* unter dem Titel auswählen. Extrahieren Sie den heruntergeladenen Ordner.

Auf Ihrem Computer muss [**Node.js**](https://nodejs.org/download) installiert sein. Der Gerätesimulator basiert auf **Node.js**-Version 10.0. x oder höher.

## <a name="solution-architecture"></a>Lösungsarchitektur

Die folgende Abbildung veranschaulicht die Architektur dieser Lösung mit Azure Digital Twins und dem Device Provisioning Service. Sie zeigt den Ablauf für die Bereitstellung und Außerbetriebnahme von Geräten.

:::image type="content" source="media/how-to-provision-using-dps/flows.png" alt-text="Abbildung eines Geräts und mehrerer Azure-Dienste in einem End-to-End-Szenario. Daten werden zwischen einem Thermostat und DPS übermittelt. Außerdem werden Daten über eine Azure-Funktion mit der Bezeichnung „Zuweisung“ aus DPS zu IoT Hub und zu Azure Digital Twins übertragen. Daten aus einer manuellen Aktion zum Löschen des Geräts durchlaufen IoT Hub > Event Hubs > Azure Functions > Azure-Digital Twins." lightbox="media/how-to-provision-using-dps/flows.png":::

Dieser Artikel ist in zwei Abschnitte unterteilt:
* [*Bereitstellen eines Geräts mithilfe des Device Provisioning Service*](#auto-provision-device-using-device-provisioning-service)
* [*Automatisches Außerbetriebnehmen von Geräten mithilfe von IoT Hub-Lebenszyklusereignissen*](#auto-retire-device-using-iot-hub-lifecycle-events)

Ausführliche Erläuterungen zu den einzelnen Schritten in der Architektur finden Sie in den entsprechenden Abschnitten weiter unten in diesem Artikel.

## <a name="auto-provision-device-using-device-provisioning-service"></a>Bereitstellen eines Geräts mithilfe des Device Provisioning Service

In diesem Abschnitt fügen Sie den Device Provisioning Service an Azure Digital Twins an, um Geräte automatisch über den unten beschriebenen Pfad bereitzustellen. Dies ist ein Auszug aus der [oben dargestellten](#solution-architecture) vollständigen Architektur.

:::image type="content" source="media/how-to-provision-using-dps/provision.png" alt-text="Abbildung eines Bereitstellungsflusses: Auszug aus dem Lösungsarchitekturdiagramm mit nummerierten Abschnitten. Daten werden zwischen einem Thermostat und DPS übermittelt (1 für Gerät > DPS und 5 für DPS > Gerät). Außerdem werden Daten über eine Azure-Funktion mit der Bezeichnung „Zuweisung“ (2) aus DPS zu IoT Hub (4) und zu Azure Digital Twins (3) übertragen." lightbox="media/how-to-provision-using-dps/provision.png":::

Beschreibung des Prozesses:
1. Das Gerät kontaktiert den DPS-Endpunkt und übergibt Informationen, um seine Identität zu bestätigen.
2. DPS überprüft die Geräteidentität, indem die Registrierungs-ID und der Schlüssel anhand der Registrierungsliste überprüft werden, und ruft eine [Azure-Funktion](../azure-functions/functions-overview.md) auf, um die Zuordnung durchzuführen.
3. Die Azure-Funktion erstellt in Azure Digital Twins einen neuen [Zwilling](concepts-twins-graph.md) für das Gerät. Der Zwilling erhält denselben Namen wie die **Registrierungs-ID** des Geräts.
4. DPS registriert das Gerät bei einem IoT-Hub und gibt den gewünschten Zwillingsstatus des Geräts an.
5. Der IoT-Hub gibt Informationen zur Geräte-ID und zur IoT-Hub-Verbindung an das Gerät zurück. Das Gerät kann nun eine Verbindung mit dem IoT-Hub herstellen.

In den folgenden Abschnitten werden die Schritte zum Einrichten dieses Ablaufs für die automatische Bereitstellung von Geräten erläutert.

### <a name="create-a-device-provisioning-service"></a>Erstellen einer Device Provisioning Service-Instanz

Wenn ein neues Gerät mit dem Device Provisioning Service bereitgestellt wird, kann in Azure Digital Twins ein neuer Zwilling für dieses Gerät erstellt werden, dessen Name mit der Registrierungs-ID identisch ist.

Erstellen Sie eine Instanz des Device Provisioning Service, die zum Bereitstellen von IoT-Geräten verwendet wird. Sie können entweder die folgenden Azure CLI Anweisungen oder das Azure-Portal verwenden: [*Schnellstart: Einrichten des IoT Hub Device Provisioning Service über das Azure-Portal*](../iot-dps/quick-setup-auto-provision.md).

Mit dem folgenden Azure CLI Befehl wird ein Device Provisioning Service erstellt. Sie müssen einen Namen, eine Ressourcengruppe und eine Region für den Device Provisioning Service angeben. Informationen zu Regionen mit Unterstützung des Device Provisioning Service finden Sie unter [*Verfügbare Produkte nach Region*](https://azure.microsoft.com/global-infrastructure/services/?products=iot-hub).
Der Befehl kann in [Cloud Shell](https://shell.azure.com) oder lokal ausgeführt werden, wenn die Azure CLI [auf dem Computer installiert](/cli/azure/install-azure-cli) ist.

```azurecli-interactive
az iot dps create --name <Device Provisioning Service name> --resource-group <resource group name> --location <region>
```

### <a name="add-a-function-to-use-with-device-provisioning-service"></a>Hinzufügen einer mit dem Device Provisioning Service zu verwendenden Funktion

Sie erstellen in der Funktions-App, die Sie im Abschnitt [Voraussetzungen](#prerequisites) erstellt haben, eine neue Funktion für die Verwendung mit dem Device Provisioning Service. Diese Funktion wird vom Device Provisioning Service in einer [benutzerdefinierten Zuweisungsrichtlinie](../iot-dps/how-to-use-custom-allocation-policies.md) zur Bereitstellung eines neuen Geräts verwendet.

Öffnen Sie zunächst auf dem Computer das Funktions-App-Projekt in Visual Studio, und führen Sie die folgenden Schritte aus.

#### <a name="step-1-add-a-new-function"></a>Schritt 1: Hinzufügen einer neuen Funktion 

Fügen Sie dem Funktions-App-Projekt in Visual Studio eine neue Funktion vom Typ *HTTP-Trigger* hinzu.

:::image type="content" source="media/how-to-provision-using-dps/add-http-trigger-function-visual-studio.png" alt-text="Screenshot der Visual Studio-Ansicht zum Hinzufügen einer Azure-Funktion vom Typ „HTTP-Trigger“ im Funktions-App-Projekt" lightbox="media/how-to-provision-using-dps/add-http-trigger-function-visual-studio.png":::

#### <a name="step-2-fill-in-function-code"></a>Schritt 2: Angeben des Funktionscodes

Fügen Sie dem Projekt ein neues NuGet-Paket hinzu: [Microsoft.Azure.Devices.Provisioning.Service](https://www.nuget.org/packages/Microsoft.Azure.Devices.Provisioning.Service/). Möglicherweise müssen Sie dem Projekt weitere Pakete hinzufügen, wenn die im Code verwendeten Pakete nicht bereits Teil des Projekts sind.

Fügen Sie in der neu erstellten Funktionscodedatei den folgenden Code ein, benennen Sie die Funktion in *DpsAdtAllocationFunc.cs* um, und speichern Sie die Datei.

:::code language="csharp" source="~/digital-twins-docs-samples-dps/functions/DpsAdtAllocationFunc.cs":::

#### <a name="step-3-publish-the-function-app-to-azure"></a>Schritt 3: Veröffentlichen der Funktions-App in Azure

Veröffentlichen Sie das Projekt mit der Funktion *DpsAdtAllocationFunc.cs* in der Funktions-App in Azure.

[!INCLUDE [digital-twins-publish-and-configure-function-app.md](../../includes/digital-twins-publish-and-configure-function-app.md)]

### <a name="create-device-provisioning-enrollment"></a>Erstellen der Registrierung für den Device Provisioning Service

Als Nächstes müssen Sie eine Registrierung im Device Provisioning Service mithilfe einer **benutzerdefinierten Zuweisungsfunktion** erstellen. Befolgen Sie hierzu die Anweisungen im Abschnitt [*Erstellen der Registrierung*](../iot-dps/how-to-use-custom-allocation-policies.md#create-the-enrollment) des Artikels über benutzerdefinierte Zuordnungsrichtlinien in der Dokumentation zum Device Provisioning Service.

Achten Sie darauf, dass Sie beim Durchführen dieser Schritte die folgenden Optionen auswählen, um die Registrierung mit der Funktion zu verknüpfen, die Sie soeben erstellt haben.

* **Wählen Sie, wie Geräte den Hubs zugewiesen werden sollen**: Benutzerdefiniert (Azure-Funktion verwenden).
* **Wählen Sie die IoT-Hubs aus, denen diese Gruppe zugewiesen werden kann**: Wählen Sie den Namen des IoT-Hubs aus, oder klicken Sie auf die Schaltfläche *Neuen IoT-Hub verknüpfen*, und wählen Sie in der Dropdownliste den IoT-Hub aus.

Klicken Sie als Nächstes auf die Schaltfläche *Neue Funktion auswählen*, um die Funktions-App mit der Registrierungsgruppe zu verknüpfen. Geben Sie anschließend die folgenden Werte ein:

* **Abonnement**: Ihr Azure-Abonnement wird automatisch ausgefüllt. Stellen Sie sicher, dass es das richtige Abonnement ist.
* **Funktions-App**: Wählen Sie den Namen Ihrer Funktions-App aus.
* **Funktion**: Wählen Sie „DpsAdtAllocationFunc“ aus.

Speichern Sie die Informationen.                  

:::image type="content" source="media/how-to-provision-using-dps/link-enrollment-group-to-iot-hub-and-function-app.png" alt-text="Screenshot des Fensters „Details der Registrierungsgruppe“ für eine benutzerdefinierte Registrierungsgruppe mit Auswahl von „Benutzerdefiniert“ (Azure-Funktion verwenden) und dem Namen Ihres IoT-Hubs in den Abschnitten „Auswählen, wie Geräte Hubs zugewiesen werden sollen“ und „Auswählen der IoT-Hubs, denen diese Gruppe zugewiesen werden kann“. Wählen Sie außerdem Ihr Abonnement und die Funktions-App aus der Dropdownliste aus, und stellen Sie sicher, dass Sie DpsAdtAllocationFunc auswählen." lightbox="media/how-to-provision-using-dps/link-enrollment-group-to-iot-hub-and-function-app.png":::

Nachdem Sie die Registrierung erstellt haben, wird später in diesem Artikel der **Primärschlüssel** für die Registrierung zum Konfigurieren des Gerätesimulators verwendet.

### <a name="set-up-the-device-simulator"></a>Einrichten des Gerätesimulators

In diesem Beispiel wird ein Gerätesimulator verwendet, der die Bereitstellung mithilfe des Device Provisioning Service umfasst. Der Gerätesimulator befindet sich hier: [Beispiel für die Integration von Azure Digital Twins und IoT Hub](/samples/azure-samples/digital-twins-iothub-integration/adt-iothub-provision-sample/). Wenn Sie dies noch nicht getan haben, laden Sie das Beispielprojekt auf Ihren Computer herunter, indem Sie zum Beispiellink navigieren und die Schaltfläche *ZIP-Datei herunterladen* unter dem Titel auswählen. Extrahieren Sie den heruntergeladenen Ordner.

#### <a name="upload-the-model"></a>Hochladen des Modells

Der Gerätesimulator ist ein Gerät vom Typ „Thermostat“, von dem das Modell mit der folgenden ID verwendet wird: `dtmi:contosocom:DigitalTwins:Thermostat;1`. Sie müssen dieses Modell in Azure Digital Twins hochladen, bevor Sie einen Zwilling dieses Typs für das Gerät erstellen können.

[!INCLUDE [digital-twins-thermostat-model-upload.md](../../includes/digital-twins-thermostat-model-upload.md)]

Weitere Informationen zu Modellen finden Sie unter [*Verwalten von Azure Digital Twins-Modellen*](how-to-manage-model.md#upload-models).

#### <a name="configure-and-run-the-simulator"></a>Konfigurieren und Ausführen des Simulators

Navigieren Sie im Befehlsfenster zu dem heruntergeladenen Beispiel für die *Integration von Azure Digital Twins und IoT Hub*, das Sie zuvor entzippt haben, und dann zum Verzeichnis *device-simulator* . Installieren Sie dann die Abhängigkeiten für das Projekt, indem Sie den folgenden Befehl ausführen:

```cmd
npm install
```

Kopieren Sie als Nächstes die Datei mit der Erweiterung „.env.template“ im Verzeichnis „device-simulator“ in eine neue Datei mit der Erweiterung „.env“, und erfassen Sie die folgenden Werte zum Festlegen der Einstellungen:

* PROVISIONING_IDSCOPE: Um diesen Wert zu erhalten, navigieren Sie im [Azure-Portal](https://portal.azure.com/) zum Device Provisioning Service, wählen Sie in den Menüoptionen *Übersicht* aus, und suchen Sie das Feld *ID-Bereich*.

    :::image type="content" source="media/how-to-provision-using-dps/id-scope.png" alt-text="Screenshot der Seite „Übersicht“ für den Device Provisioning Service im Azure-Portal zum Kopieren des Werts für „ID-Bereich“" lightbox="media/how-to-provision-using-dps/id-scope.png":::

* PROVISIONING_REGISTRATION_ID: Sie können eine Registrierungs-ID für Ihr Gerät auswählen.
* ADT_MODEL_ID: `dtmi:contosocom:DigitalTwins:Thermostat;1`
* PROVISIONING_SYMMETRIC_KEY: Dies ist der Primärschlüssel für die Registrierung, die Sie zuvor eingerichtet haben. Um diesen Wert erneut zu erhalten, navigieren Sie im Azure-Portal zum Device Provisioning Service, wählen Sie *Registrierungen verwalten* aus, wählen Sie dann die zuvor erstellte Registrierungsgruppe aus, und kopieren Sie den *Primärschlüssel*.

    :::image type="content" source="media/how-to-provision-using-dps/sas-primary-key.png" alt-text="Screenshot der Seite „Registrierungen verwalten“ für den Device Provisioning Service im Azure-Portal zum Kopieren des Werts für den SAS-Primärschlüssel" lightbox="media/how-to-provision-using-dps/sas-primary-key.png":::

Verwenden Sie nun die oben aufgeführten Werte, um die Einstellungen für die ENV-Datei zu aktualisieren.

```cmd
PROVISIONING_HOST = "global.azure-devices-provisioning.net"
PROVISIONING_IDSCOPE = "<Device Provisioning Service Scope ID>"
PROVISIONING_REGISTRATION_ID = "<Device Registration ID>"
ADT_MODEL_ID = "dtmi:contosocom:DigitalTwins:Thermostat;1"
PROVISIONING_SYMMETRIC_KEY = "<Device Provisioning Service enrollment primary SAS key>"
```

Speichern und schließen Sie die Datei.

### <a name="start-running-the-device-simulator"></a>Ausführen des Gerätesimulators

Starten Sie im Verzeichnis *device-simulator* den Gerätesimulator mit dem folgenden Befehl:

```cmd
node .\adt_custom_register.js
```

Sie sollten sehen, dass das Gerät registriert und mit IoT Hub verbunden ist und Nachrichten sendet.
:::image type="content" source="media/how-to-provision-using-dps/output.png" alt-text="Screenshot des Befehlsfensters mit Geräteregistrierung und gesendeten Nachrichten" lightbox="media/how-to-provision-using-dps/output.png":::

### <a name="validate"></a>Überprüfen

Durch den in diesem Artikel eingerichteten Ablauf wird das Gerät automatisch in Azure Digital Twins registriert. Suchen Sie mit dem folgenden [Azure Digital Twins-CLI-Befehl](how-to-use-cli.md) den Zwilling des Geräts in der von Ihnen erstellten Azure Digital Twins-Instanz.

```azurecli-interactive
az dt twin show -n <Digital Twins instance name> --twin-id "<Device Registration ID>"
```

Sie sollten den Zwilling des Geräts in der Azure Digital Twins-Instanz sehen.
:::image type="content" source="media/how-to-provision-using-dps/show-provisioned-twin.png" alt-text="Screenshot des Befehlsfensters mit dem neu erstellten Zwilling" lightbox="media/how-to-provision-using-dps/show-provisioned-twin.png":::

## <a name="auto-retire-device-using-iot-hub-lifecycle-events"></a>Automatisches Außerbetriebnehmen von Geräten mithilfe von IoT Hub-Lebenszyklusereignissen

In diesem Abschnitt fügen Sie IoT Hub-Lebenszyklusereignisse an Azure Digital Twins an, um Geräte automatisch über den folgenden Ablauf außer Betrieb zu nehmen. Dies ist ein Auszug aus der [oben dargestellten](#solution-architecture) vollständigen Architektur.

:::image type="content" source="media/how-to-provision-using-dps/retire.png" alt-text="Abbildung des Ablaufs für die Außerbetriebnahme eines Geräts: Auszug aus dem Lösungsarchitekturdiagramm mit nummerierten Abschnitten. Der Thermostat wird ohne Verbindungen mit Azure-Diensten im Diagramm gezeigt. Daten aus einer manuellen Aktion zum Löschen des Geräts durchlaufen IoT Hub (1) > Event Hubs (2) > Azure Functions > Azure-Digital Twins (3)." lightbox="media/how-to-provision-using-dps/retire.png":::

Beschreibung des Prozesses:
1. Ein externer oder manueller Prozess löst das Löschen eines Geräts in IoT Hub aus.
2. IoT Hub löscht das Gerät und generiert ein [Gerätelebenszyklusereignis](../iot-hub/iot-hub-device-management-overview.md#device-lifecycle), das an einen [Event Hub](../event-hubs/event-hubs-about.md) weitergeleitet wird.
3. Eine Azure-Funktion löscht den Zwilling des Geräts in Azure Digital Twins.

In den folgenden Abschnitten werden die Schritte zum Einrichten dieses Ablaufs für die automatische Außerbetriebnahme von Geräten erläutert.

### <a name="create-an-event-hub"></a>Erstellen eines Ereignis-Hubs

Als Nächstes erstellen Sie einen [Azure Event Hub](../event-hubs/event-hubs-about.md) zum Empfangen von IoT Hub-Lebenszyklusereignissen. 

Führen Sie die in der Schnellstartanleitung zum [*Erstellen eines Event Hubs*](../event-hubs/event-hubs-create.md) beschriebenen Schritte aus. Benennen Sie den Event Hub mit *lifecycleevents*. Sie verwenden diesen Event Hub-Namen, wenn Sie in den nächsten Abschnitten eine IoT Hub-Route und eine Azure-Funktion einrichten.

Der folgende Screenshot veranschaulicht die Erstellung des Event Hubs.
:::image type="content" source="media/how-to-provision-using-dps/create-event-hub-lifecycle-events.png" alt-text="Screenshot des Azure-Portal-Fensters zum Erstellen eines Event Hubs mit dem Namen „lifecycleevents“" lightbox="media/how-to-provision-using-dps/create-event-hub-lifecycle-events.png":::

#### <a name="create-sas-policy-for-your-event-hub"></a>Erstellen einer SAS-Richtlinie für den Event Hub

Als Nächstes müssen Sie eine [SAS-Richtlinie (Shared Access Signature)](../event-hubs/authorize-access-shared-access-signature.md) erstellen, um den Event Hub mit Ihrer Funktions-App zu konfigurieren.
Gehen Sie dazu folgendermaßen vor:
1. Navigieren Sie zu dem Event Hub, die Sie soeben im Azure-Portal erstellt haben, und wählen Sie in den Menüoptionen auf der linken Seite die Option **Freigegebene Zugriffsrichtlinien** aus.
2. Wählen Sie **Hinzufügen** aus. Geben Sie im daraufhin geöffneten Fenster *SAS-Richtlinie hinzufügen* einen Richtliniennamen Ihrer Wahl ein, und aktivieren Sie das Kontrollkästchen *Lauschen*.
3. Klicken Sie auf **Erstellen**.
    
:::image type="content" source="media/how-to-provision-using-dps/add-event-hub-sas-policy.png" alt-text="Screenshot des Azure-Portals zum Hinzufügen einer Event Hub-SAS-Richtlinie" lightbox="media/how-to-provision-using-dps/add-event-hub-sas-policy.png":::

#### <a name="configure-event-hub-with-function-app"></a>Konfigurieren des Event Hubs mit der Funktions-App

Konfigurieren Sie als Nächstes die Azure-Funktions-App, die Sie im Abschnitt [Voraussetzungen](#prerequisites) eingerichtet haben, für die Verwendung mit dem neuen Event Hub. Dabei legen Sie in der Funktions-App mit der Verbindungszeichenfolge des Event Hubs eine Umgebungsvariable fest.

1. Öffnen Sie die soeben erstellte Richtlinie, und kopieren Sie den Wert von **Verbindungszeichenfolge – Primärschlüssel**.

    :::image type="content" source="media/how-to-provision-using-dps/event-hub-sas-policy-connection-string.png" alt-text="Screenshot des Azure-Portals zum Kopieren des Werts von „Verbindungszeichenfolge – Primärschlüssel“" lightbox="media/how-to-provision-using-dps/event-hub-sas-policy-connection-string.png":::

2. Fügen Sie mit dem folgenden Azure CLI-Befehl die Verbindungszeichenfolge als Variable in den Funktions-App-Einstellungen hinzu. Der Befehl kann in [Cloud Shell](https://shell.azure.com) oder lokal ausgeführt werden, wenn die Azure CLI [auf dem Computer installiert](/cli/azure/install-azure-cli) ist.

    ```azurecli-interactive
    az functionapp config appsettings set --settings "EVENTHUB_CONNECTIONSTRING=<Event Hubs SAS connection string Listen>" -g <resource group> -n <your App Service (function app) name>
    ```

### <a name="add-a-function-to-retire-with-iot-hub-lifecycle-events"></a>Hinzufügen einer Funktion zur Außerbetriebnahme mit IoT Hub-Lebenszyklusereignissen

Sie erstellen in dem Funktions-App-Projekt, das Sie im Abschnitt [Voraussetzungen](#prerequisites) erstellt haben, eine neue Funktion, um ein vorhandenes Gerät mithilfe von IoT Hub-Lebenszyklusereignissen außer Betrieb zu nehmen.

Weitere Informationen zu Lebenszyklusereignissen finden Sie unter [*Nicht telemetriebezogene Ereignisse in IoT Hub*](../iot-hub/iot-hub-devguide-messages-d2c.md#non-telemetry-events). Weitere Informationen zur Verwendung von Event Hubs mit Azure-Funktionen finden Sie unter [*Azure Event Hubs-Trigger für Azure Functions*](../azure-functions/functions-bindings-event-hubs-trigger.md).

Öffnen Sie zunächst auf dem Computer das Funktions-App-Projekt in Visual Studio, und führen Sie die folgenden Schritte aus.

#### <a name="step-1-add-a-new-function"></a>Schritt 1: Hinzufügen einer neuen Funktion
     
Fügen Sie dem Funktions-App-Projekt in Visual Studio eine neue Funktion vom Typ *Event Hubs-Trigger* hinzu.

:::image type="content" source="media/how-to-provision-using-dps/create-event-hub-trigger-function.png" alt-text="Screenshot des Visual Studio-Fensters zum Hinzufügen einer Azure-Funktion vom Typ „Event Hubs-Trigger“ im Funktions-App-Projekt" lightbox="media/how-to-provision-using-dps/create-event-hub-trigger-function.png":::

#### <a name="step-2-fill-in-function-code"></a>Schritt 2: Angeben des Funktionscodes

Fügen Sie in der neu erstellten Funktionscodedatei den folgenden Code ein, benennen Sie die Funktion in `DeleteDeviceInTwinFunc.cs` um, und speichern Sie die Datei.

:::code language="csharp" source="~/digital-twins-docs-samples-dps/functions/DeleteDeviceInTwinFunc.cs":::

#### <a name="step-3-publish-the-function-app-to-azure"></a>Schritt 3: Veröffentlichen der Funktions-App in Azure

Veröffentlichen Sie das Projekt mit der Funktion *DeleteDeviceInTwinFunc.cs* in der Funktions-App in Azure.

[!INCLUDE [digital-twins-publish-and-configure-function-app.md](../../includes/digital-twins-publish-and-configure-function-app.md)]

### <a name="create-an-iot-hub-route-for-lifecycle-events"></a>Erstellen einer IoT Hub-Route für Lebenszyklusereignisse

Jetzt richten Sie eine IoT Hub-Route ein, um Lebenszyklusereignisse für das Gerät weiterzuleiten. In diesem Fall lauschen Sie speziell auf Ereignisse, bei denen ein Gerät gelöscht wird. Diese sind durch `if (opType == "deleteDeviceIdentity")` gekennzeichnet. Dadurch wird das Löschen des Elements für den digitalen Zwilling und die Außerbetriebnahme eines Geräts und des zugehörigen digitalen Zwillings auslöst.

Zunächst müssen Sie im IoT-Hub einen Event Hub-Endpunkt erstellen. Anschließend fügen Sie im IoT-Hub eine Route hinzu, um Lebenszyklusereignisse an diesen Event Hub-Endpunkt zu senden.
Führen Sie die folgenden Schritte aus, um einen Event Hub-Endpunkt zu erstellen:

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/)zu dem IoT-Hub, den Sie im Abschnitt [Voraussetzungen](#prerequisites) erstellt haben, und wählen Sie in den Menüoptionen auf der linken Seite **Nachrichtenrouting** aus.
2. Wählen Sie die Registerkarte **Benutzerdefinierte Endpunkte**.
3. Wählen Sie **+ Hinzufügen** und dann **Event Hubs** aus, um einen Endpunkt vom Typ „Event Hubs“ hinzuzufügen.

    :::image type="content" source="media/how-to-provision-using-dps/event-hub-custom-endpoint.png" alt-text="Screenshot des Visual Studio-Fensters zum Hinzufügen eines benutzerdefinierten Event Hub-Endpunkts" lightbox="media/how-to-provision-using-dps/event-hub-custom-endpoint.png":::

4. Wählen Sie im daraufhin geöffneten Fenster *Event Hub-Endpunkt hinzufügen* die folgenden Werte aus:
    * **Endpunktname**: Wählen Sie einen Endpunktnamen aus.
    * **Event Hub-Namespace**: Wählen Sie in der Dropdownliste den Event Hub-Namespace aus.
    * **Event Hub-Instanz**: Wählen Sie den Event Hub-Namen aus, den Sie im vorherigen Schritt erstellt haben.
5. Klicken Sie auf **Erstellen**. Lassen Sie dieses Fenster geöffnet, um im nächsten Schritt eine Route hinzuzufügen.

    :::image type="content" source="media/how-to-provision-using-dps/add-event-hub-endpoint.png" alt-text="Screenshot des Visual Studio-Fensters zum Hinzufügen eines Event Hub-Endpunkts" lightbox="media/how-to-provision-using-dps/add-event-hub-endpoint.png":::

Als Nächstes fügen Sie eine Route hinzu, die eine Verbindung mit dem im obigen Schritt erstellten Endpunkt herstellt, mit einer Routingabfrage, die die Löschereignisse sendet. Führen Sie zum Erstellen einer Route die folgenden Schritte aus:

1. Navigieren Sie zur Registerkarte *Routen*, und wählen Sie **Hinzufügen** aus, um eine Route hinzuzufügen.

    :::image type="content" source="media/how-to-provision-using-dps/add-message-route.png" alt-text="Screenshot des Visual Studio-Fensters zum Hinzufügen einer Route zum Senden von Ereignissen" lightbox="media/how-to-provision-using-dps/add-message-route.png":::

2. Wählen Sie auf der daraufhin geöffneten Seite *Route hinzufügen* die folgenden Werte aus:

   * **Name**: Wählen Sie einen Namen für die Route aus. 
   * **Endpunkt**: Wählen Sie in der Dropdownliste den Event Hubs-Endpunkt aus, den Sie zuvor erstellt haben.
   * **Datenquelle**: Wählen Sie *Ereignisse im Gerätelebenszyklus* aus.
   * **Routingabfrage**: Geben Sie `opType='deleteDeviceIdentity'` ein. Diese Abfrage schränkt das Senden der Gerätelebenszklusereignisse auf Löschereignisse ein.

3. Wählen Sie **Speichern**.

    :::image type="content" source="media/how-to-provision-using-dps/lifecycle-route.png" alt-text="Screenshot des Azure-Portal-Fensters zum Hinzufügen einer Route, um Lebenszyklusereignissen zu senden" lightbox="media/how-to-provision-using-dps/lifecycle-route.png":::

Wenn Sie diese Schritte durchlaufen haben, ist alles für die End-to-End-Außerbetriebnahme der Geräte vorbereitet.

### <a name="validate"></a>Überprüfen

Um die Außerbetriebnahme auszulösen, müssen Sie das Gerät manuell aus IoT Hub löschen.

Dies können Sie mit einem [Azure CLI Befehl](/cli/azure/iot/hub/module-identity#az_iot_hub_module_identity_delete) oder mit dem Azure-Portal durchführen. Führen Sie die folgenden Schritte aus, um das Gerät im Azure Portal zu löschen:

1. Navigieren Sie zum IoT-Hub, und wählen Sie in den Menüoptionen auf der linken Seite **IoT-Geräte** aus. 
2. Es wird ein Gerät mit der Geräteregistrierungs-ID angezeigt, die Sie in der [ersten Hälfte dieses Artikels](#auto-provision-device-using-device-provisioning-service) ausgewählt haben. Alternativ können Sie auch ein beliebiges anderes Gerät zum Löschen auswählen, solange es über einen Zwilling in Azure Digital Twins verfügt, damit Sie überprüfen können, ob der Zwilling nach dem Löschen des Geräts automatisch gelöscht wird.
3. Wählen Sie das Gerät aus, und klicken Sie auf **Löschen**.

:::image type="content" source="media/how-to-provision-using-dps/delete-device-twin.png" alt-text="Screenshot des Azure-Portals zum Löschen des Gerätezwillings von den IoT-Geräten" lightbox="media/how-to-provision-using-dps/delete-device-twin.png":::

Es kann einige Minuten dauern, bis die Änderungen in Azure Digital Twins angezeigt werden.

Mit dem folgenden [Azure Digital Twins CLI-Befehl](how-to-use-cli.md) können Sie überprüfen, ob der Zwilling des Geräts in der Azure Digital Twins-Instanz gelöscht wurde.

```azurecli-interactive
az dt twin show -n <Digital Twins instance name> --twin-id "<Device Registration ID>"
```

Sie sollten sehen, dass der Zwilling des Geräts in der Azure Digital Twins-Instanz nicht mehr gefunden werden kann.

:::image type="content" source="media/how-to-provision-using-dps/show-retired-twin.png" alt-text="Screenshot des Befehlsfensters mit der Information, dass der Zwilling nicht gefunden wurde" lightbox="media/how-to-provision-using-dps/show-retired-twin.png":::

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die in diesem Artikel erstellten Ressourcen nicht mehr benötigen, folgen Sie den Schritten unten, um sie zu löschen.

Bei Verwendung von Azure Cloud Shell oder der lokalen Azure CLI können Sie alle Azure-Ressourcen in einer Ressourcengruppe mit dem Befehl [az group delete](/cli/azure/group#az-group-delete) löschen. Hierdurch werden die Ressourcengruppe, die Azure Digital Twins-Instanz, der IoT-Hub und die Registrierung des Hubgeräts, das Event Grid-Thema und die zugehörigen Abonnements, der Event Hub-Namespace sowie beide Azure Functions-Apps (einschließlich zugeordneter Ressourcen, z. B. Speicher) entfernt.

> [!IMPORTANT]
> Das Löschen einer Ressourcengruppe kann nicht rückgängig gemacht werden. Die Ressourcengruppe und alle darin enthaltenen Ressourcen werden unwiderruflich gelöscht. Achten Sie daher darauf, dass Sie nicht versehentlich die falsche Ressourcengruppe oder die falschen Ressourcen löschen. 

```azurecli-interactive
az group delete --name <your-resource-group>
```

Löschen Sie dann den Beispielordner des Projekts, den Sie heruntergeladen haben, von Ihrem lokalen Computer.

## <a name="next-steps"></a>Nächste Schritte

Die für die Geräte erstellten digitalen Zwillinge werden als flache Hierarchie in Azure Digital Twins gespeichert, können jedoch für die Organisation mit Modellinformationen und einer Hierarchie mit mehreren Ebenen erweitert werden. Weitere Informationen zu diesem Vorgang finden Sie hier:

* [*Konzepte: Digital Twins und der Digital Twins-Graph*](concepts-twins-graph.md)

Weitere Informationen zur Verwendung von HTTP-Anforderungen mit Azure-Funktionen finden Sie unter:

* [*HTTP-Trigger in Azure Functions*](../azure-functions/functions-bindings-http-webhook-trigger.md)

Sie können eine benutzerdefinierte Logik schreiben, um diese Informationen mithilfe der bereits in Azure Digital Twins gespeicherten Modell- und Diagrammdaten automatisch bereitzustellen. Weitere Informationen zum Verwalten, Aktualisieren und Abrufen von Informationen aus dem Zwillingsgraphen finden Sie in den folgenden Referenzen:

* [*Verwenden Verwalten digitaler Zwillinge*](how-to-manage-twin.md)
* [*Verwenden Abfragen des Zwillingsgraphen*](how-to-query-graph.md)