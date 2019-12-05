---
title: Definieren eines neuen Azure IoT Edge-Gerätetyps in Azure IoT Central | Microsoft-Dokumentation
description: In diesem Tutorial für Lösungsentwickler erfahren Sie, wie Sie in Ihrer Azure IoT Central-Anwendung ein neues Azure IoT Edge-Gerät erstellen. Sie definieren die Telemetriedaten, den Zustand, die Eigenschaften und die Befehle für den Typ.
author: rangv
ms.author: rangv
ms.date: 10/22/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 0a293d74c9e37a6771c5bb246b74bda38db3b7c3
ms.sourcegitcommit: dd0304e3a17ab36e02cf9148d5fe22deaac18118
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2019
ms.locfileid: "74406479"
---
# <a name="tutorial-define-a-new-azure-iot-edge-device-type-in-your-azure-iot-central-application-preview-features"></a>Tutorial: Definieren eines neuen Azure IoT Edge-Gerätetyps in Ihrer Azure IoT Central-Anwendung (Previewfunktionen)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

In diesem Tutorial für Ersteller erfahren Sie, wie Sie in Ihrer Azure IoT Central-Anwendung mithilfe einer Gerätevorlage eine neue Art von Azure IoT Edge-Gerät definieren. 

Eine Übersicht finden Sie unter [Was ist Azure IoT Central (Previewfunktionen)?](overview-iot-central.md). 

IoT Edge besteht aus drei Komponenten:
* **IoT Edge-Module** sind Container, die Azure-Dienste, Partnerdienste oder Ihren Code ausführen. Module werden auf IoT Edge-Geräten bereitgestellt und lokal auf diesen Geräten ausgeführt.
* Die **IoT Edge-Runtime** wird auf jedem IoT Edge-Gerät ausgeführt und dient zum Verwalten der Module, die auf einem Gerät jeweils bereitgestellt wurden.
* Mit einer **cloudbasierten Schnittstelle** können Sie für IoT Edge-Geräte die Remoteüberwachung und -verwaltung durchführen. IoT Central bildet die Cloudschnittstelle.

Ein **Azure IoT Edge**-Gerät kann ein Gatewaygerät mit nachgeschalteten Geräten sein, die eine Verbindung mit dem IoT Edge-Gerät herstellen. In diesem Tutorial werden weitere Informationen zu den Verbindungsmustern von nachgeschalteten Geräten angegeben.

Mit **Gerätevorlagen** werden die Funktionen Ihrer Geräte und IoT Edge-Module definiert. Zu den Funktionen zählen die vom Modul gesendeten Telemetriedaten, die Moduleigenschaften und die Befehle, auf die ein Modul reagiert.

In diesem Tutorial erstellen Sie eine Gerätevorlage für einen Umgebungssensor. Ein Umgebungssensorgerät ermöglicht Folgendes:

* Es sendet Telemetriedaten, z. B. Temperatur.
* Es reagiert auf schreibbare Eigenschaften, wenn diese in der Cloud aktualisiert werden, z. B. Sendeintervall für Telemetriedaten.
* Es reagiert auf Befehle, z. B. Zurücksetzen der Temperatur.

In diesem Tutorial erstellen Sie auch eine Gerätevorlage für ein Umgebungsgateway. Ein Umgebungsgatewaygerät ermöglicht Folgendes:

* Es sendet Telemetriedaten, z. B. Temperatur.
* Es reagiert auf schreibbare Eigenschaften, wenn diese in der Cloud aktualisiert werden, z. B. Sendeintervall für Telemetriedaten.
* Es reagiert auf Befehle, z. B. Zurücksetzen der Temperatur.
* Es lässt Beziehungen mit anderen Gerätefunktionsmodellen zu.


In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer neuen Gerätevorlage für Azure IoT Edge-Geräte
> * Hochladen eines Bereitstellungsmanifests
> * Erstellen von Funktionen mit Telemetriedaten, Eigenschaften und Befehlen für die einzelnen Module
> * Definieren einer Visualisierung für die Modultelemetriedaten
> * Hinzufügen von Beziehungen mit Vorlagen für nachgeschaltete Geräte
> * Veröffentlichen Ihrer Gerätevorlage

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial müssen Sie [eine Azure IoT Central-Anwendung erstellen](quick-deploy-iot-central.md).


## <a name="downstream-device-relationships-with-a-gateway-and-modules"></a>Beziehungen nachgeschalteter Geräte mit einem Gateway und Modulen

Nachgeschaltete Geräte können über das Modul `$edgeHub` eine Verbindung mit einem IoT Edge-Gatewaygerät herstellen. In diesem Szenario wird das IoT Edge-Gerät zu einem transparenten Gateway.

![Diagramm des transparenten Gateways](./media/tutorial-define-edge-device-type/gateway-transparent.png)

Nachgeschaltete Geräte können auch über ein benutzerdefiniertes Modul eine Verbindung mit einem IoT Edge-Gatewaygerät herstellen. Im folgenden Szenario stellen nachgeschaltete Geräte eine Verbindung über ein benutzerdefiniertes ModBus-Modul her.

![Diagramm der Verbindung über das benutzerdefinierte Modul](./media/tutorial-define-edge-device-type/gateway-module.png)

Das folgende Diagramm zeigt die Verbindung mit einem IoT Edge-Gatewaygerät über beide Modultypen (benutzerdefiniert und `$edgeHub`).  

![Diagramm der Verbindung über beide Verbindungsmodule](./media/tutorial-define-edge-device-type/gateway-module-transparent.png)

Schließlich können nachgeschaltete Geräte über mehrere benutzerdefinierte Module eine Verbindung mit einem IoT Edge-Gatewaygerät herstellen. Das folgende Diagramm zeigt nachgeschaltete Geräte, die über ein benutzerdefiniertes ModBus-Modul, ein benutzerdefiniertes BLE-Modul und das `$edgeHub`-Modul verbunden sind. 

![Diagramm der Verbindung über mehrere benutzerdefinierte Module](./media/tutorial-define-edge-device-type/gateway-module2-transparent.png)


## <a name="create-a-template"></a>Erstellen einer Vorlage

Als Lösungsentwickler können Sie IoT Edge-Gerätevorlagen in Ihrer Anwendung erstellen und bearbeiten. Nachdem Sie eine Gerätevorlage veröffentlicht haben, können Sie eine Verbindung mit echten Geräten herstellen, die die Gerätevorlage implementieren.

### <a name="select-device-template-type"></a>Auswählen des Gerätevorlagentyps 

Wählen Sie im linken Bereich **Gerätevorlagen** aus, um Ihrer Anwendung eine neue Gerätevorlage hinzuzufügen.

![Screenshot der Vorschauanwendung mit hervorgehobenen Option „Gerätevorlagen“](./media/tutorial-define-edge-device-type/edgedevicetemplate.png)

Wählen Sie **+ Neu** aus, um mit der Erstellung einer neuen Gerätevorlage zu beginnen.

![Screenshot der Seite „Gerätevorlagen“ mit markierter Option „Neu“](./media/tutorial-define-edge-device-type/edgedevicetemplatenew.png)

Wählen Sie auf der Seite **Vorlagentyp auswählen** die Option **Azure IoT Edge** aus und dann **Weiter: Anpassen**.

![Screenshot der Seite „Vorlagentyp auswählen“](./media/tutorial-define-edge-device-type/selectiotedge.png)

### <a name="customize-device-template"></a>Anpassen der Gerätevorlage

In IoT Edge können Sie Geschäftslogik in Form von Modulen bereitstellen und verwalten. IoT Edge-Module sind die kleinste der von IoT Edge verwalteten Recheneinheiten und können Azure-Dienste (z. B. Azure Stream Analytics) oder Ihren eigenen lösungsspezifischen Code enthalten. Informationen zum Entwickeln, Bereitstellen und Verwalten von Modulen finden Sie unter [IoT Edge-Module](../../iot-edge/iot-edge-modules.md).

Im Grunde ist ein Bereitstellungsmanifest eine Liste von Modulzwillingen, die mit den gewünschten Eigenschaften konfiguriert sind. Ein Bereitstellungsmanifest teilt einem IoT Edge-Gerät (oder einer Gruppe von Geräten) mit, welche Module installiert und wie diese konfiguriert werden müssen. Bereitstellungsmanifeste enthalten die gewünschten Eigenschaften für jeden Modulzwilling. IoT Edge-Geräte geben die gemeldeten Eigenschaften für jedes Modul zurück.

Verwenden Sie Visual Studio Code, um ein Bereitstellungsmanifest zu erstellen. Weitere Informationen finden Sie unter [Azure IoT Edge für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge).

In diesem Tutorial wird das folgende Beispiel für ein einfaches Bereitstellungsmanifest mit einem einzelnen Modul verwendet. Kopieren Sie den folgenden JSON-Code, und speichern Sie ihn als JSON-Datei. 

   ```JSON
   {
     "modulesContent": {
       "$edgeAgent": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "runtime": {
             "type": "docker",
             "settings": {
               "minDockerVersion": "v1.25",
               "loggingOptions": "",
               "registryCredentials": {}
             }
           },
           "systemModules": {
             "edgeAgent": {
               "type": "docker",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
                 "createOptions": "{}"
               }
             },
             "edgeHub": {
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
                 "createOptions": "{}"
               }
             }
           },
           "modules": {
             "SimulatedTemperatureSensor": {
               "version": "1.0",
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
                 "createOptions": "{}"
               }
             }
           }
         }
       },
       "$edgeHub": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "routes": {
               "route": "FROM /* INTO $upstream"
           },
           "storeAndForwardConfiguration": {
             "timeToLiveSecs": 7200
           }
         }
       },
       "SimulatedTemperatureSensor": {
         "properties.desired": {
              "SendData": true,
              "SendInterval": 10
         }
       }
     }
   }
   ```

#### <a name="upload-an-iot-edge-deployment-manifest"></a>Hochladen eines IoT Edge-Bereitstellungsmanifests

Wählen Sie auf der Seite **Gerät anpassen** unter **Azure IoT Edge-Bereitstellungsmanifest hochladen** die Option **Durchsuchen** aus. 

![Screenshot der Seite „Gerät anpassen“ mit hervorgehobener Option „Durchsuchen“](./media/tutorial-define-edge-device-type/edgedevicetemplateuploadmanifest.png)

Wenn Sie eine IoT Edge-Gatewaygerätevorlage erstellen möchten, aktivieren Sie **Gatewaygerät mit nachgeschalteten Geräten**.

![Screenshot der Seite „Gerät anpassen“ mit hervorgehobenem Gatewaygerät mit nachgeschalteten Geräten](./media/tutorial-define-edge-device-type/gateway-upload.png)

Wählen Sie im Dialogfeld zur Dateiauswahl die Datei für das Bereitstellungsmanifest und dann **Öffnen** aus.

IoT Edge validiert die Bereitstellungsmanifestdatei anhand eines Schemas. Wenn die Validierung erfolgreich ist, wählen Sie **Überprüfen** aus.

![Screenshot der Seite „Gerät anpassen“ mit hervorgehobenem Bereitstellungsmanifest und Option „Überprüfen“](./media/tutorial-define-edge-device-type/deploymentmanifestvalidate.png)

Das folgende Flussdiagramm zeigt den Lebenszyklus eines Bereitstellungsmanifests in IoT Central.

![Flussdiagramm des Lebenszyklus eines Bereitstellungsmanifests](./media/tutorial-define-edge-device-type/dmflow.png)

Als Nächstes wird die Seite „Überprüfen“ mit Details zum Bereitstellungsmanifest angezeigt. Auf dieser Seite wird eine Liste der Module aus dem Bereitstellungsmanifest angezeigt. Beachten Sie, dass in diesem Tutorial das Modul `SimulatedTemperatureSensor` aufgeführt ist. Klicken Sie auf **Erstellen**.

![Screenshot der Seite „Überprüfen“ mit hervorgehobenem Modul und Option „Erstellen“](./media/tutorial-define-edge-device-type/edgedevicetemplatereview.png)

Wenn Sie ein Gatewaygerät ausgewählt haben, wird die folgende Überprüfungsseite angezeigt.

![Screenshot der Seite „Überprüfen“ mit hervorgehobenem Azure IoT Edge-Gateway](./media/tutorial-define-edge-device-type/gateway-review.png)


Sie erstellen eine Gerätevorlage mit Modulfunktionsmodellen. In diesem Tutorial erstellen Sie eine Gerätevorlage mit dem Modulfunktionsmodell `SimulatedTemperatureSensor`. 

Ändern Sie den Titel der Gerätevorlage in **Environment Sensor Device Template**.

![Screenshot der Gerätevorlage mit hervorgehobenem aktualisiertem Titel](./media/tutorial-define-edge-device-type/edgedevicetemplatelanding.png)

Modellieren Sie im IoT Edge-Gerät IoT-Plug & Play wie folgt:
* Jede IoT Edge-Gerätevorlage verfügt über ein Gerätefunktionsmodell.
* Für jedes benutzerdefinierte Modul im Bereitstellungsmanifest wird ein Modulfunktionsmodell generiert.
* Zwischen den einzelnen Modulfunktionsmodellen und einem Gerätefunktionsmodell wird eine Beziehung eingerichtet.
* Im Modulfunktionsmodell werden Modulschnittstellen implementiert.
* Jede Modulschnittstelle enthält Telemetrie, Eigenschaften und Befehle.

![Diagramm der IoT Edge-Modellierung](./media/tutorial-define-edge-device-type/edgemodelling.png)

#### <a name="add-capabilities-to-a-module-capability-model"></a>Hinzufügen von Funktionen zu einem Modulfunktionsmodell

Hier ist eine Beispielausgabe des Moduls `SimulatedTemperatureSensor`:
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

Sie können dem Modul `SimulatedTemperatureSensor` Funktionen hinzufügen, die der oben stehenden Ausgabe entsprechen. 

1. Um eine Schnittstelle des Modulfunktionsmodells `SimulatedTemperatureSensor` zu verwalten, wählen Sie **Verwalten** > **Funktionalität hinzufügen** aus. 

    ![Screenshot der Umgebungssensorvorlage mit hervorgehobener Option „Funktionalität hinzufügen“](./media/tutorial-define-edge-device-type/edgetemplateaddcapability.png)
  
1. Fügen Sie eine Maschine als Objekttyp hinzu.
  
    ![Screenshot der Seite mit den Funktionen der Umgebungssensorvorlage mit hervorgehobener Option „Schema“](./media/tutorial-define-edge-device-type/edgetemplatemachineobject.png)

1. Wählen Sie **Definieren** aus. Ändern Sie im angezeigten Dialogfeld den Objektnamen in **machine**. Erstellen Sie Eigenschaften für Temperatur und Druck, und wählen Sie **Anwenden** aus.
  
    ![Screenshot des Dialogfelds für die Attribute mit verschiedenen hervorgehobenen Optionen](./media/tutorial-define-edge-device-type/edgetemplatemachineattributes.png)
  
1. Fügen Sie den Objekttyp **ambient** hinzu.

1. Wählen Sie **Definieren** aus. Ändern Sie im angezeigten Dialogfeld den Objektnamen in **ambient**. Erstellen Sie Eigenschaften für Temperatur und Feuchtigkeit, und wählen Sie **Anwenden** aus.
  
    ![Screenshot des Dialogfelds für die Attribute mit verschiedenen hervorgehobenen Optionen](./media/tutorial-define-edge-device-type/edgetemplateambientattributes.png)

  
1. Fügen Sie `timeCreated` als `DateTime`-Typ hinzu, und wählen Sie **Speichern** aus.
  
    ![Screenshot der Umgebungssensorvorlage mit hervorgehobener Option „Speichern“](./media/tutorial-define-edge-device-type/edgetemplateallattributes.png)


### <a name="add-relationships"></a>Hinzufügen von Beziehungen

Wenn Sie angegeben haben, dass es sich bei dem IoT Edge-Gerät um ein Gatewaygerät handelt, können Sie für Geräte, die eine Verbindung mit dem Gatewaygerät herstellen sollen, Downstreambeziehungen mit Gerätefunktionsmodellen hinzufügen.
  
  ![Screenshot der Umgebungsgatewayvorlage mit hervorgehobener Option „Beziehung hinzufügen“](./media/tutorial-define-edge-device-type/gateway-add-relationship.png)

Sie können eine Beziehung auf Geräte- oder Modulebene hinzufügen.
  
  ![Screenshot der Umgebungsgatewayvorlage mit hervorgehobenen Beziehungen auf Geräte- und Modulebene](./media/tutorial-define-edge-device-type/gateway-relationship-types.png)


Sie können ein nachgeschaltetes Gerätefunktionsmodell oder das Sternchensymbol auswählen. 
  
  ![Screenshot der Umgebungsgatewayvorlage mit hervorgehobenem Ziel](./media/tutorial-define-edge-device-type/gateway-downstream-rel.png)

  Wählen Sie für dieses Tutorial das Sternchen aus. Diese Option ermöglicht eine beliebige Downstreambeziehung. Klicken Sie dann auf **Speichern**.

  ![Screenshot der Umgebungsgatewayvorlage mit hervorgehobenem Ziel](./media/tutorial-define-edge-device-type/gateway-add-relationship-asterix.png)


### <a name="add-cloud-properties"></a>Hinzufügen von Cloudeigenschaften

Eine Gerätevorlage kann Cloudeigenschaften enthalten. Cloudeigenschaften sind nur in der IoT Central-Anwendung vorhanden und werden niemals an ein Gerät gesendet oder von einem Gerät empfangen.

1. Wählen Sie **Cloudeigenschaften** >  **+ Cloudeigenschaft hinzufügen** aus. Verwenden Sie die Informationen in der folgenden Tabelle, um Ihrer Gerätevorlage eine Cloudeigenschaft hinzuzufügen.

    | `Display name`      | Semantischer Typ | Schema |
    | ----------------- | ------------- | ------ |
    | Datum der letzten Wartung | Keine          | Date   |
    | Kundenname     | Keine          | Zeichenfolge |

2. Wählen Sie **Speichern** aus.

  
    ![Screenshot der Umgebungssensorvorlage mit hervorgehobener Option „Speichern“](./media/tutorial-define-edge-device-type/edgetemplatecloudproperties.png)

### <a name="add-customizations"></a>Hinzufügen von Anpassungen

Verwenden Sie Anpassungen, um eine Schnittstelle zu ändern oder einer Funktion, für die Sie Ihr Gerätefunktionsmodell nicht mit einer Versionsangabe versehen müssen, IoT Central-spezifische Features hinzuzufügen. Sie können Felder anpassen, wenn sich das Funktionsmodell im Entwurfszustand oder veröffentlichten Zustand befindet. Sie können nur Felder anpassen, die die Schnittstellenkompatibilität nicht beeinträchtigen. Sie haben beispielsweise folgende Möglichkeiten:

- Anpassen des Anzeigenamens und der Einheiten einer Funktion
- Hinzufügen einer Standardfarbe, die beim Anzeigen des Werts in einem Diagramm verwendet werden soll
- Angeben der anfänglichen, minimalen und maximalen Werte für eine Eigenschaft

(Der Funktionsname oder -typ kann nicht angepasst werden.)

Wählen Sie nach Abschluss der Anpassung **Speichern** aus.
  
![Screenshot der Seite „Anpassen“ für die Umgebungssensorvorlage](./media/tutorial-define-edge-device-type/edgetemplatecustomize.png)


### <a name="create-views"></a>Erstellen von Ansichten

Als Ersteller können Sie die Anwendung so anpassen, dass relevante Informationen zum Umgebungssensorgerät einem Bediener angezeigt werden. Ihre Anpassungen ermöglichen dem Bediener die Verwaltung des mit der Anwendung verbundenen Umgebungssensors. Sie können zwei Arten von Ansichten erstellen, die von einem Bediener zum Interagieren mit Geräten verwendet werden:

* Formulare zum Anzeigen und Bearbeiten von Geräte- und Cloudeigenschaften
* Dashboards zum Visualisieren von Geräten

### <a name="configure-a-view-to-visualize-devices"></a>Konfigurieren einer Ansicht zum Visualisieren von Geräten

Mit einem Gerätedashboard kann ein Bediener ein Gerät mithilfe von Diagrammen und Metriken visualisieren. Als Ersteller können Sie definieren, welche Informationen auf einem Gerätedashboard angezeigt werden sollen. Sie können mehrere Dashboards für Geräte definieren. Wählen Sie zum Erstellen eines Dashboards für die Visualisierung der Umgebungssensor-Telemetriedaten **Ansichten** > **Gerät visualisieren** aus:

  
![Screenshot der Seite „Ansichten“ für die Umgebungssensorvorlage mit hervorgehobener Option „Gerät visualisieren“](./media/tutorial-define-edge-device-type/visualizingthedevice.png)


Bei „Ambient Telemetry“ und „Machine Telemetry“ handelt es sich um komplexe Objekte. So erstellen Sie Diagramme

1. Ziehen Sie **Ambient Telemetry**, und wählen Sie **Liniendiagramm** aus. 
  
   ![Screenshot der Umgebungssensorvorlage mit hervorgehobenem Objekt „Ambient Telemetry“ und Option „Liniendiagramm“](./media/tutorial-define-edge-device-type/sensorambientchart.png)

1. Wählen Sie das Symbol „Konfigurieren“ aus. Wählen Sie **Temperature** und **Humidity** aus, um die Daten zu visualisieren, und wählen Sie dann **Konfiguration aktualisieren** aus. 
  
   ![Screenshot der Umgebungssensorvorlage mit verschiedenen hervorgehobenen Optionen](./media/tutorial-define-edge-device-type/sensorambienttelemetrychart.png)

1. Wählen Sie **Speichern** aus.

Sie können weitere Kacheln hinzufügen, auf denen andere Eigenschaften oder Telemetriewerte angezeigt werden. Sie können auch statischen Text, Links und Bilder hinzufügen. Zeigen Sie zum Verschieben oder Ändern der Größe einer Kachel im Dashboard auf die Kachel, und ziehen Sie die Kachel an eine neue Position, oder ändern Sie ihre Größe.
  
![Screenshot der Dashboardansicht für die Umgebungssensorvorlage](./media/tutorial-define-edge-device-type/viewsdashboard.png)

### <a name="add-a-device-form"></a>Hinzufügen eines Geräteformulars

Mit einem Geräteformular kann ein Bediener schreibbare Geräteeigenschaften und Cloudeigenschaften bearbeiten. Als Ersteller können Sie mehrere Formulare definieren und auswählen, welche Geräte- und Cloudeigenschaften in einem Formular jeweils angezeigt werden. Sie können in einem Formular auch schreibgeschützte Geräteeigenschaften anzeigen.

Erstellen Sie wie folgt ein Formular, um Umgebungssensoreigenschaften anzuzeigen und zu bearbeiten:

1. Wechseln Sie in der **Umgebungssensorvorlage** zu **Ansichten**. Wählen Sie die Kachel **Geräte- und Clouddaten bearbeiten** aus, um eine neue Ansicht hinzuzufügen.
  
   ![Screenshot der Seite „Ansichten“ für die Umgebungssensorvorlage mit hervorgehobener Option „Geräte- und Clouddaten werden bearbeitet“](./media/tutorial-define-edge-device-type/editingdeviceandclouddata.png)

1. Geben Sie den Formularnamen **Environmental Sensor properties** (Umgebungssensoreigenschaften) ein.

1. Ziehen Sie die Cloudeigenschaften **Kundenname** und **Datum der letzten Wartung** in den vorhandenen Abschnitt im Formular.
  
   ![Screenshot der Seite „Ansichten“ für die Umgebungssensorvorlage mit verschiedenen hervorgehobenen Optionen](./media/tutorial-define-edge-device-type/views-properties.png)

1. Wählen Sie **Speichern** aus.

## <a name="publish-a-device-template"></a>Veröffentlichen einer Gerätevorlage

Bevor Sie einen simulierten Umgebungssensor erstellen oder einen echten Umgebungssensor verbinden können, müssen Sie Ihre Gerätevorlage veröffentlichen.

Veröffentlichen Sie wie folgt eine Gerätevorlage:

1. Navigieren Sie über die Seite **Gerätevorlagen** zu Ihrer Gerätevorlage.

2. Wählen Sie **Veröffentlichen**.
  
    ![Screenshot der Umgebungssensorvorlage mit hervorgehobener Option „Veröffentlichen“](./media/tutorial-define-edge-device-type/edgetemplatepublish.png)

1. Wählen Sie im Dialogfeld **Publish a Device Template** (Gerätevorlage veröffentlichen) die Option **Veröffentlichen** aus.
  
    ![Screenshot des Dialogfelds „Publish a Device Template“ (Gerätevorlage veröffentlichen) mit hervorgehobener Option „Veröffentlichen“](./media/tutorial-define-edge-device-type/edgepublishtemplate.png)

Nachdem eine Gerätevorlage veröffentlicht wurde, wird sie auf der Seite **Geräte** und für den Bediener angezeigt. In einer veröffentlichten Gerätevorlage können Sie ein Gerätefunktionsmodell nicht bearbeiten, ohne eine neue Versionsnummer zu erstellen. Sie können jedoch Cloudeigenschaften, Anpassungen und Ansichten in einer veröffentlichten Gerätevorlage aktualisieren. Diese Aktualisierungen bewirken nicht das Erstellen einer neuen Version. Wählen Sie nach etwaigen Änderungen **Veröffentlichen** aus, um diese Änderungen für Ihren Bediener bereitzustellen.
  
![Screenshot der Liste „Gerätevorlagen“ mit veröffentlichten Vorlagen](./media/tutorial-define-edge-device-type/publishedtemplate.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

* Erstellen eines neuen Edgegeräts als Blattgerätevorlage
* Generieren von Modulen auf der Grundlage eines hochgeladenen Bereitstellungsmanifests
* Hinzufügen von komplexer Telemetrie und Eigenschaften
* Erstellen von Cloudeigenschaften
* Erstellen von Anpassungen
* Definieren einer Visualisierung für die Gerätetelemetriedaten
* Veröffentlichen Ihrer Edgegerätevorlage

Nachdem Sie nun eine Gerätevorlage in Ihrer Azure IoT Central-Anwendung erstellt haben, können Sie sich als Nächstes Folgendes ansehen:

> [!div class="nextstepaction"]
> [Verbinden eines Geräts](./tutorial-connect-pnp-device.md)
