---
title: 'Schnellstart: Bereitstellen von IoT Connector (Vorschauversion) mithilfe des Azure-Portals'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe des Azure-Portals das Feature „IoT Connector“ von Azure API for FHIR bereitstellen, konfigurieren und verwenden.
services: healthcare-apis
author: ms-puneet-nagpal
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: quickstart
ms.date: 05/11/2020
ms.author: punagpal
ms.openlocfilehash: bc85765666ba3baeae7ec795118f615f6b4b4368
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87101071"
---
# <a name="quickstart-deploy-iot-connector-preview-using-azure-portal"></a>Schnellstart: Bereitstellen von IoT Connector (Vorschauversion) mithilfe des Azure-Portals

IoT Connector ist ein optionales Feature von Azure API for FHIR, das die Erfassung der Daten von IoMT-Geräten (Internet of Medical Things) ermöglicht. Während der Vorschauphase steht das Feature „IoT Connector“ kostenlos zur Verfügung. In diesem Schnellstart erfahren Sie, wie die folgenden Aufgaben ausgeführt werden:
- Bereitstellen und Konfigurieren von IoT Connector über das Azure-Portal
- Verwenden eines simulierten Geräts zum Senden von Daten an IoT Connector
- Anzeigen von Ressourcen, die von IoT Connector in Azure API for FHIR erstellt wurden

## <a name="prerequisites"></a>Voraussetzungen

- Ein aktives Azure-Abonnement. ([Kostenloses Abonnement erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F))
- Eine Azure API for FHIR-Ressource: [Bereitstellen von Azure API for FHIR mithilfe des Azure-Portals](fhir-paas-portal-quickstart.md)

## <a name="go-to-azure-api-for-fhir-resource"></a>Navigieren zur Azure API for FHIR-Ressource

Öffnen Sie das [Azure-Portal](https://portal.azure.com), und navigieren Sie zur **Azure API for FHIR**-Ressource, für die Sie das Feature „IoT Connector“ erstellen möchten.

[![Azure API for FHIR-Ressource](media/quickstart-iot-fhir-portal/portal-azure-api-fhir.jpg)](media/quickstart-iot-fhir-portal/portal-azure-api-fhir.jpg#lightbox)

Klicken Sie im Navigationsmenü auf der linken Seite im Abschnitt **Add-Ins** auf **IoT Connector (preview)** (IoT Connector (Vorschauversion)), um die Seite **IoT Connectors** (IoT Connector-Instanzen) zu öffnen.

[![Feature „IoT Connector“](media/quickstart-iot-fhir-portal/portal-iot-connectors.jpg)](media/quickstart-iot-fhir-portal/portal-iot-connectors.jpg#lightbox)

## <a name="create-new-iot-connector-preview"></a>Erstellen einer neuen IoT Connector-Instanz (Vorschauversion)

Klicken Sie auf die Schaltfläche **Hinzufügen**, um die Seite **Create IoT Connector** (IoT Connector erstellen) zu öffnen.

[![Hinzufügen einer IoT Connector-Instanz](media/quickstart-iot-fhir-portal/portal-iot-connectors-add.jpg)](media/quickstart-iot-fhir-portal/portal-iot-connectors-add.jpg#lightbox)

Geben Sie Einstellungen für die neue IoT Connector-Instanz ein. Klicken Sie auf die Schaltfläche **Erstellen**, und warten Sie die IoT Connector-Bereitstellung ab.

> [!NOTE]
> Für diese Installation muss für das Dropdownmenü **Auflösungstyp** der Wert **Erstellen** ausgewählt werden. 

[![Erstellen einer IoT Connector-Instanz](media/quickstart-iot-fhir-portal/portal-iot-connector-create.jpg)](media/quickstart-iot-fhir-portal/portal-iot-connector-create.jpg#lightbox)

|Einstellung|Wert|BESCHREIBUNG |
|---|---|---|
|Connectorname|Ein eindeutiger Name|Geben Sie einen Namen zum Identifizieren der IoT Connector-Instanz ein. Dieser Name muss innerhalb einer Azure API for FHIR-Ressource eindeutig sein. Der Name darf nur Kleinbuchstaben, Zahlen und den Bindestrich (-) enthalten. Er muss mit einem Buchstaben oder einer Zahl beginnen und enden und 3 bis 24 Zeichen lang sein.|
|Auflösungstyp|Nachschlagen oder Erstellen|Wählen Sie **Nachschlagen** aus, wenn Sie einen Out-of-Band-Prozess zum Erstellen der FHIR-Ressourcen vom Typ [Gerät](https://www.hl7.org/fhir/device.html) und [Patient](https://www.hl7.org/fhir/patient.html) in Ihrer Azure API for FHIR-Instanz verwenden. Die IoT Connector-Instanz verwendet beim Erstellen der FHIR-Ressource [Beobachtung](https://www.hl7.org/fhir/observation.html) einen Verweis auf diese Ressourcen zur Darstellung der Gerätedaten. Wählen Sie **Erstellen** aus, wenn von IoT Connector Barebone-Ressourcen für Geräte und Patienten in Ihrer Azure API for FHIR-Instanz erstellt und dabei die entsprechenden Bezeichnerwerte in den Gerätedaten herangezogen werden sollen.|

Nach Abschluss der Installation wird die neu erstellte IoT Connector-Instanz auf der Seite **IoT Connectors** (IoT Connector-Instanzen) angezeigt.

[![Erstellte IoT Connector-Instanz](media/quickstart-iot-fhir-portal/portal-iot-connector-created.jpg)](media/quickstart-iot-fhir-portal/portal-iot-connector-created.jpg#lightbox)

## <a name="configure-iot-connector-preview"></a>Konfigurieren von IoT Connector (Vorschauversion)

IoT Connector benötigt zwei Zuordnungsvorlagen, um Gerätemeldungen in FHIR-basierte Beobachtungsressourcen zu transformieren: **Gerätezuordnung** und **FHIR-Zuordnung**. Ihre IoT Connector-Instanz ist voll funktionsfähig, wenn diese Zuordnungen hochgeladen wurden.

[![Fehlende IoT Connector-Zuordnungen](media/quickstart-iot-fhir-portal/portal-iot-connector-missing-mappings.jpg)](media/quickstart-iot-fhir-portal/portal-iot-connector-missing-mappings.jpg#lightbox)

Klicken Sie zum Hochladen der Zuordnungsvorlagen auf die neu bereitgestellte IoT Connector-Instanz, um die Seite **IoT Connector** aufzurufen.

[![Klicken auf die IoT Connector-Instanz](media/quickstart-iot-fhir-portal/portal-iot-connector-click.jpg)](media/quickstart-iot-fhir-portal/portal-iot-connector-click.jpg#lightbox)

#### <a name="device-mapping"></a>Gerätezuordnung

Mit der Vorlage für die Gerätezuordnung werden Gerätedaten in ein normalisiertes Schema transformiert. Klicken Sie auf der Seite **IoT Connector** auf die Schaltfläche **Configure device mapping** (Gerätezuordnung konfigurieren), um die Seite **Gerätezuordnung** aufzurufen. 

[![IoT Connector: Klicken auf „Configure device mapping“ (Gerätezuordnung konfigurieren)](media/quickstart-iot-fhir-portal/portal-iot-connector-click-device-mapping.jpg)](media/quickstart-iot-fhir-portal/portal-iot-connector-click-device-mapping.jpg#lightbox)

Fügen Sie auf der Seite **Gerätezuordnung** das folgende Skript zum JSON-Editor hinzu, und klicken Sie auf **Speichern**.

```json
{
  "templateType": "CollectionContent",
  "template": [
    {
      "templateType": "IotJsonPathContent",
      "template": {
        "typeName": "heartrate",
        "typeMatchExpression": "$..[?(@Body.HeartRate)]",
        "patientIdExpression": "$.SystemProperties.iothub-connection-device-id",
        "values": [
          {
            "required": "true",
            "valueExpression": "$.Body.HeartRate",
            "valueName": "hr"
          }
        ]
      }
    }
  ]
}
```

[![IoT Connector: Gerätezuordnung](media/quickstart-iot-fhir-portal/portal-iot-device-mapping.jpg)](media/quickstart-iot-fhir-portal/portal-iot-device-mapping.jpg#lightbox)


#### <a name="fhir-mapping"></a>FHIR-Zuordnung

Mit der Vorlage für die FHIR-Zuordnung wird eine normalisierte Meldung in eine FHIR-basierte Beobachtungsressource transformiert. Klicken Sie auf der Seite **IoT Connector** auf die Schaltfläche **Configure FHIR mapping** (FHIR-Zuordnung konfigurieren), um die Seite **FHIR-Zuordnung** aufzurufen.  

[![IoT Connector: Klicken auf „Configure FHIR mapping“ (FHIR-Zuordnung konfigurieren)](media/quickstart-iot-fhir-portal/portal-iot-connector-click-fhir-mapping.jpg)](media/quickstart-iot-fhir-portal/portal-iot-connector-click-fhir-mapping.jpg#lightbox)

Fügen Sie auf der Seite **FHIR-Zuordnung** das folgende Skript zum JSON-Editor hinzu, und klicken Sie auf **Speichern**.

```json
{
  "templateType": "CollectionFhir",
  "template": [
    {
      "templateType": "CodeValueFhir",
      "template": {
        "codes": [
          {
            "code": "8867-4",
            "system": "http://loinc.org",
            "display": "Heart rate"
          }
        ],
        "periodInterval": 0,
        "typeName": "heartrate",
        "value": {
          "unit": "count/min",
          "valueName": "hr",
          "valueType": "Quantity"
        }
      }
    }
  ]
}
```

[![IoT Connector: FHIR-Zuordnung](media/quickstart-iot-fhir-portal/portal-iot-fhir-mapping.jpg)](media/quickstart-iot-fhir-portal/portal-iot-fhir-mapping.jpg#lightbox)

## <a name="generate-a-connection-string"></a>Generieren einer Verbindungszeichenfolge

IoMT-Geräte benötigen eine Verbindungszeichenfolge, um eine Verbindung mit IoT Connector herzustellen und Nachrichten an IoT Connector zu senden. Wählen Sie auf der Seite **IoT Connector** für die neu bereitgestellte IoT Connector-Instanz die Schaltfläche **Manage client connections** (Clientverbindungen verwalten) aus. 

[![IoT Connector: Klicken Auf „Manage client connections“ (Clientverbindungen verwalten)](media/quickstart-iot-fhir-portal/portal-iot-connector-click-client-connections.jpg)](media/quickstart-iot-fhir-portal/portal-iot-connector-click-client-connections.jpg#lightbox)

Klicken Sie auf der Seite **Verbindungen** auf die Schaltfläche **Hinzufügen**, um eine neue Verbindung zu erstellen. 

[![IoT Connector: Verbindungen](media/quickstart-iot-fhir-portal/portal-iot-connections.jpg)](media/quickstart-iot-fhir-portal/portal-iot-connections.jpg#lightbox)

Geben Sie im Überlagerungsfenster einen Anzeigenamen für diese Verbindung ein, und wählen Sie die Schaltfläche **Erstellen** aus.

[![IoT Connector: neue Verbindung](media/quickstart-iot-fhir-portal/portal-iot-new-connection.jpg)](media/quickstart-iot-fhir-portal/portal-iot-new-connection.jpg#lightbox)

Wählen Sie auf der Seite **Verbindungen** die neu erstellte Verbindung aus, und kopieren Sie den Wert des Felds **Primäre Verbindungszeichenfolge** aus dem Überlagerungsfenster auf der rechten Seite.

[![IoT Connector: Verbindungszeichenfolge](media/quickstart-iot-fhir-portal/portal-iot-connection-string.jpg)](media/quickstart-iot-fhir-portal/portal-iot-connection-string.jpg#lightbox)

Speichern Sie diese Verbindungszeichenfolge für einen späteren Schritt. 

## <a name="connect-your-devices-to-iot"></a>Herstellen einer Verbindung zwischen Geräten und IoT

Azure bietet eine umfangreiche Sammlung von IoT-Produkten zum Herstellen einer Verbindung mit und zum Verwalten von IoT-Geräten. Sie können mithilfe von Azure IoT Hub eigene PaaS-basierte Lösungen erstellen oder mit Azure IoT Central mit einer verwalteten IoT-App-Plattform beginnen. In diesem Tutorial nutzen Sie Azure IoT Central. Diese Lösung bietet branchenspezifische Lösungsvorlagen, die Ihnen den Einstieg erleichtern.

Stellen Sie die [Anwendungsvorlage für die ständige Überwachung von Patienten](https://docs.microsoft.com/azure/iot-central/healthcare/tutorial-continuous-patient-monitoring#create-an-application-template) bereit. Diese Vorlage enthält zwei simulierte Geräte, die Echtzeitdaten erzeugen, um Ihnen den Einstieg zu erleichtern: **Smart Vitals Patch** und **Smart Knee Brace**.

> [!NOTE]
> Wenn Ihre echten Geräte bereit sind, können Sie dieselbe IoT Central-Anwendung verwenden, um das [Onboarding für Ihre Geräte durchzuführen](https://docs.microsoft.com/azure/iot-central/core/howto-set-up-template) und Gerätesimulatoren zu ersetzen. Ihre Gerätedaten werden ebenfalls automatisch an FHIR übermittelt. 

## <a name="connect-your-iot-data-with-the-iot-connector-preview"></a>Verbinden Ihrer Gerätedaten mit IoT Connector (Vorschauversion)
Nach der Bereitstellung Ihrer IoT Central-Anwendung beginnen Ihre beiden vordefinierten simulierten Geräte mit dem Generieren von Telemetriedaten. In diesem Tutorial erfassen Sie die Telemetriedaten vom Simulator *Smart Vitals Patch* in FHIR über IoT Connector. Wenn Sie Ihre IoT-Daten in IoT Connector exportieren möchten, sollten Sie einen [fortlaufenden Datenexport in IoT Central einrichten](https://docs.microsoft.com/azure/iot-central/core/howto-export-data#set-up-data-export). Gehen Sie auf der Seite für den kontinuierlichen Datenexport wie folgt vor:
- Wählen Sie als Exportziel *Azure Event Hubs* aus.
- Wählen Sie für das Feld **Event Hubs-Namespace** den Wert *Verbindungszeichenfolge verwenden* aus.
- Geben Sie die IoT Connector-Verbindungszeichenfolge an, die Sie in einem vorherigen Schritt für das Feld **Verbindungszeichenfolge** abgerufen haben.
- Übernehmen Sie für die Option **Telemetrie** die Option *Ein* für das Feld **Zu exportierende Daten**.

## <a name="view-device-data-in-azure-api-for-fhir"></a>Anzeigen von Gerätedaten in Azure API for FHIR

Mithilfe von Postman können Sie die FHIR-basierten Überwachungsressourcen anzeigen, die von IoT Connector für Azure API for FHIR erstellt wurden. Richten Sie [Postman für den Zugriff auf Azure API for FHIR](access-fhir-postman-tutorial.md) ein, und senden Sie eine Anforderung vom Typ `GET` an `https://your-fhir-server-url/Observation?code=http://loinc.org|8867-4`, um FHIR-Überwachungsressourcen mit Herzfrequenz anzuzeigen. 

> [!TIP]
> Vergewissern Sie sich, dass Ihr Benutzer über die erforderlichen Zugriffsrechte für die Azure API for FHIR-Datenebene verfügt. Verwenden Sie die [rollenbasierte Zugriffssteuerung in Azure](configure-azure-rbac.md), um die erforderlichen Datenebenenrollen zuzuweisen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie eine IoT Connector-Instanz nicht mehr benötigen, können Sie sie löschen. Entfernen Sie dazu die zugehörige Ressourcengruppe, den zugehörigen Azure API for FHIR-Dienst oder die IoT Connector-Instanz selbst. 

Um eine IoT Connector-Instanz direkt zu entfernen, wählen Sie die Instanz auf der Seite **IoT Connectors** (IoT Connector-Instanzen) aus, um die Seite **IoT Connector** aufzurufen, und klicken Sie auf die Schaltfläche **Löschen**. Wählen Sie **Ja** aus, wenn Sie zur Bestätigung aufgefordert werden. 

[![Löschen einer IoT Connector-Instanz](media/quickstart-iot-fhir-portal/portal-iot-connector-delete.jpg)](media/quickstart-iot-fhir-portal/portal-iot-connector-delete.jpg#lightbox)

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie das IoT Connector-Feature in Ihrer Azure API for FHIR-Ressource bereitgestellt. Weitere Informationen zu IoT Connector finden Sie in den folgenden Schritten:

Machen Sie sich mit verschiedenen Datenflussphasen in IoT Connector vertraut:

>[!div class="nextstepaction"]
>[IoT Connector (Vorschau): Datenfluss](iot-data-flow.md)

Erfahren Sie, wie Sie IoT Connector mithilfe von Geräte- und FHIR-Zuordnungsvorlagen konfigurieren:

>[!div class="nextstepaction"]
>[IoT Connector (Vorschau): Zuordnungsvorlagen](iot-mapping-templates.md)

FHIR ist ein eingetragenes Markenzeichen von HL7 und wird mit Erlaubnis von HL7 verwendet.
