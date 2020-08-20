---
title: Exportieren von Daten aus IoT Central | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie den neuen Datenexport verwenden, um Ihre IoT-Daten nach Azure und benutzerdefinierten Cloudzielen zu exportieren.
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 08/04/2020
ms.topic: how-to
ms.service: iot-central
manager: corywink
ms.openlocfilehash: f51630154b77233aeb2587ac3a2d603c1da6fa4f
ms.sourcegitcommit: bfeae16fa5db56c1ec1fe75e0597d8194522b396
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2020
ms.locfileid: "88036554"
---
# <a name="export-iot-data-to-cloud-destinations-using-data-export-preview"></a>Exportieren von IoT-Daten zu Cloudzielen in Azure mithilfe des Datenexports (Vorschauversion)

> [!Note]
> Suchen Sie nach dem Legacy-Datenexport? Dokumentation zum Datenexport finden Sie [hier](./howto-export-data.md). Wenn Sie etwas über die Unterschiede zwischen dem neuen Datenexport und dem Legacy-Datenexport erfahren möchten, sehen Sie sich die [Vergleichstabelle](#comparison-of-legacy-data-export-and-new-data-export) an.

In diesem Artikel wird beschrieben, wie Sie die Previewfunktion des Datenexports in Azure IoT Central verwenden. Mit diesem Feature können Sie Ihre gefilterten und angereicherten IoT-Daten fortlaufend in Ihre Clouddienste exportieren. Sie können den Datenexport verwenden, um Änderungen in nahezu Echtzeit an andere Teile Ihrer Cloudlösung zu übertragen und so umsetzbare Einblicke, Analyseergebnisse und Speicherinformationen zu erhalten. 

 Sie haben unter anderem folgende Möglichkeiten:
-   Fortlaufender Export von Telemetriedaten und Eigenschaftsänderungen im JSON-Format in nahezu Echtzeit
-   Filtern dieser Datenströme, um bestimmte Funktionen zu exportieren, die benutzerdefinierten Bedingungen entsprechen
-   Anreichern der Datenströme mit benutzerdefinierten Werten und Eigenschaftswerten aus dem Gerät
-   Senden dieser Daten an Ziele wie Azure Event Hubs, Azure Service Bus, Azure Blob Storage und Webhookendpunkte

> [!Note]
> Wenn Sie den Datenexport aktivieren, erhalten Sie nur die Daten ab dem jeweiligen Aktivierungszeitpunkt. Momentan können Daten nicht für Zeiten abgerufen werden, in denen der Datenexport deaktiviert war. Wenn Sie mehr Verlaufsdaten beibehalten möchten, aktivieren Sie den Datenexport frühzeitig.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie den Datenexport (Vorschauversion) verwenden können, müssen Sie über eine V3-Anwendung verfügen und über Datenexportberechtigungen verfügen.

## <a name="set-up-export-destination"></a>Einrichten des Exportziels

Ihr Exportziel muss vorhanden sein, bevor Sie Ihren Datenexport konfigurieren. Dies sind die verfügbaren Typen von Zielen:
  - Azure Event Hubs
  - Azure Service Bus-Warteschlange
  - Azure Service Bus-Thema
  - Azure Blob Storage
  - Webhook

### <a name="create-an-event-hubs-destination"></a>Erstellen eines Event Hubs-Ziels

Wenn Sie keinen vorhandenen Event Hubs-Namespace als Exportziel haben, führen Sie die folgenden Schritte aus:

1. Erstellen Sie einen [neuen Event Hubs-Namespace im Azure-Portal](https://ms.portal.azure.com/#create/Microsoft.EventHub). Weitere Informationen erhalten Sie in der [Azure Event Hubs-Dokumentation](../../event-hubs/event-hubs-create.md).

2. Erstellen Sie einen Event Hub in Ihrem Event Hubs-Namespace. Wechseln Sie zu Ihrem Namespace, und wählen Sie oben **+ Event Hub** aus, um eine Event Hub-Instanz zu erstellen.

3. Generieren Sie einen Schlüssel, den Sie in IoT Central verwenden werden, um den Datenexport einzurichten. 
    - Klicken Sie auf die Event Hub-Instanz, die Sie erstellt haben. 
    - Klicken Sie unter **Einstellungen/Richtlinien für gemeinsamen Zugriff**. 
    - Erstellen Sie einen neuen Schlüssel, oder wählen Sie einen vorhandenen Schlüssel aus, der über Berechtigungen zum **Senden** verfügt. 
    - Kopieren Sie die primäre oder die sekundäre Verbindungszeichenfolge. Sie verwenden diese, um ein neues Ziel in IoT Central einzurichten.

### <a name="create-a-service-bus-queue-or-topic-destination"></a>Erstellen eines Ziels für Service Bus-Warteschlangen oder -Themen

Wenn Sie keinen vorhandenen Service Bus-Namespace als Exportziel haben, führen Sie die folgenden Schritte aus:

1. Erstellen Sie einen [neuen Service Bus-Namespace im Azure-Portal](https://ms.portal.azure.com/#create/Microsoft.ServiceBus.1.0.5). Weitere Informationen erhalten Sie in der [Azure Service Bus-Dokumentation](../../service-bus-messaging/service-bus-create-namespace-portal.md).

2. Wenn Sie eine Warteschlange oder ein Thema als Exportziel erstellen möchten, wechseln Sie zu Ihrem Service Bus-Namespace, und wählen Sie **+ Warteschlange** oder **+ Thema** aus.

3. Generieren Sie einen Schlüssel, den Sie in IoT Central verwenden werden, um den Datenexport einzurichten. 
    - Klicken Sie auf die erstellte Warteschlange oder das Thema. 
    - Klicken Sie unter **Einstellungen/Richtlinien für gemeinsamen Zugriff**. 
    - Erstellen Sie einen neuen Schlüssel, oder wählen Sie einen vorhandenen Schlüssel aus, der über Berechtigungen zum **Senden** verfügt. 
    - Kopieren Sie die primäre oder die sekundäre Verbindungszeichenfolge. Sie verwenden diese, um ein neues Ziel in IoT Central einzurichten.

### <a name="create-an-azure-blob-storage-destination"></a>Erstellen Sie ein Azure Blob Storage-Ziel.

Wenn es noch kein Azure-Speicherkonto als Exportziel gibt, führen Sie die folgenden Schritte aus:

1. Erstellen Sie ein [neues Speicherkonto im Azure-Portal](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM). Sie können sich zum Erstellen neuer [Azure Blob Storage-Konten](https://aka.ms/blobdocscreatestorageaccount) oder neuer [Azure Data Lake Storage v2-Speicherkonten](../../storage/blobs/data-lake-storage-quickstart-create-account.md) genauer informieren. Beim Datenexport können Daten nur in Speicherkonten geschrieben werden, die Blockblobs unterstützen. In der nachstehenden Liste sind die bekannten kompatiblen Speicherkontotypen aufgeführt:

    |Leistungsstufe|Kontotyp|
    |-|-|
    |Standard|Universell v2|
    |Standard|Universell v1|
    |Standard|Blob Storage|
    |Premium|Blockblobspeicher|

2. Erstellen Sie einen Container in Ihrem Speicherkonto. Wechseln Sie zum Speicherkonto. Wählen Sie unter **Blob-Dienst** die Option **Blobs durchsuchen**. Wählen Sie oben die Option **+ Container** aus, um einen neuen Container zu erstellen.

3. Generieren Sie eine Verbindungszeichenfolge für Ihr Speicherkonto, indem Sie zu **Einstellungen/Zugriffsschlüssel** wechseln. Kopieren Sie eine der beiden Verbindungszeichenfolgen.

### <a name="create-a-webhook-endpoint"></a>Erstellen eines Webhookendpunkts
Sie können einen öffentlich zugänglichen HTTP-Endpunkt bereitstellen, zu dem Daten exportiert werden sollen. Sie können einen Webhooktestendpunkt mit RequestBin erstellen. Beachten Sie, dass RequestBin ein festgelegtes Anforderungslimit hat, ehe Anforderungen gedrosselt werden.

1. Öffnen Sie [RequestBin](https://requestbin.net/).
2. Erstellen Sie ein neues RequestBin-Element, und kopieren Sie die Bin-URL.

## <a name="set-up-data-export"></a>Einrichten des Datenexports

Sie haben jetzt ein Exportziel für Ihre Daten. Führen Sie als Nächstes die folgenden Schritte aus, um den Datenexport einzurichten.

1. Melden Sie sich bei Ihrer IoT Central-Anwendung an.

2. Wählen Sie im linken Bereich die Option **Datenexport** aus.

    > [!Tip]
    > Falls **Datenexport** dort nicht angezeigt wird, haben Sie keine Berechtigungen zum Konfigurieren des Datenexports in Ihrer App. Wenden Sie sich an Ihren Administrator, damit dieser den Datenexport einrichtet.

3. Wählen Sie die Schaltfläche **+ Neuer Export** aus. 

4. Geben Sie einen Anzeigenamen für Ihren neuen Export ein, und stellen Sie sicher, dass der Schalter für den Datenfluss auf  **Aktiviert** gestellt ist.

## <a name="1-choose-the-type-of-data-to-export"></a>1. Auswählen des zu exportierenden Datentyps
Sie können zwischen dem fortlaufenden Export verschiedener Datentypen wählen. Dies sind die unterstützten Datentypen:

| Datentyp | Beschreibung | Datenformat |
| :------------- | :---------- | :----------- |
|  Telemetrie | Exportiert Telemetrienachrichten von Geräten nahezu in Echtzeit. Jede exportierte Nachricht enthält den vollständigen Inhalt der ursprünglichen Gerätenachricht (normalisiert).   |  [Format der Telemetrienachricht](#telemetry-format)   |
| Eigenschaftsänderungen | Exportiert Änderungen zu den Geräte- und Cloudeigenschaften nahezu in Echtzeit. Für schreibgeschützte Geräteeigenschaften werden Änderungen an den gemeldeten Werten exportiert. Bei Lese-/Schreibeigenschaften werden sowohl gemeldete als auch gewünschte Werte exportiert. | [Format der Eigenschaftsänderungsnachricht](#property-changes-format) |

## <a name="2-optional-add-filters"></a>2. (Optional) Hinzufügen von Filtern
Fügen Sie Filter hinzu, um die Menge der exportierten Daten anhand der Filterbedingungen zu verringern. Es gibt verschiedene Filtertypen, die für jeden zu exportierenden Datentyp verfügbar sind.

### <a name="telemetry-filters"></a>Telemetriefilter
  - **Funktionsfilter:** Wenn Sie ein Telemetrieelement in der Dropdownliste auswählen, enthält der exportierte Datenstrom nur Telemetriedaten, die die Filterbedingung erfüllen. Wenn Sie ein Gerät oder eine Cloudeigenschaft in der Dropdownliste auswählen, enthält der exportierte Stream nur Telemetriedaten von Geräten, deren Eigenschaften der Filterbedingung entsprechen.
  - **Nachrichteneigenschaftenfilter:** Geräte, die die Geräte-SDKs verwenden, dürfen *Nachrichteneigenschaften* oder *Anwendungseigenschaften* für jede Telemetrienachricht senden, bei der es sich um eine Sammlung von Schlüssel-Wert-Paaren handelt, die im Allgemeinen zum Kennzeichnen der Nachricht mit benutzerdefinierten Bezeichnern verwendet werden. Sie können einen Nachrichteneigenschaftsfilter erstellen, indem Sie den gesuchten Nachrichteneigenschaftsschlüssel eingeben und eine Bedingung angeben. Nur Telemetrienachrichten mit Nachrichteneigenschaften, die der angegebenen Filterbedingung entsprechen, werden exportiert. Nur Vergleichsoperatoren für Zeichenfolgen werden unterstützt (ist gleich, ist nicht gleich, enthält, enthält nicht, ist vorhanden, ist nicht vorhanden). [Weitere Informationen zu Anwendungseigenschaften finden Sie in der IoT Hub-Dokumentation](../../iot-hub/iot-hub-devguide-messages-construct.md).

### <a name="property-changes-filters"></a>Filter für Eigenschaftsänderungen
**Eigenschaftsfilter:** Wählen Sie ein Eigenschaftselement in der Dropdownliste aus, und der exportierte Datenstrom enthält nur Änderungen an der ausgewählten Eigenschaft, die die Filterbedingung erfüllt.

## <a name="3-optional-add-enrichments"></a>3. (Optional) Hinzufügen von Anreicherungen
Fügen Sie Anreicherungen hinzu, um exportierte Nachrichten mit zusätzlichen Metadaten in Schlüssel-Wert-Paaren zu erweitern. Dies sind die verfügbaren Anreicherungen der Typen von Telemetrie- und Eigenschaftsänderungdaten:
  - **Benutzerdefinierte Zeichenfolge:** Fügt jeder Nachricht eine benutzerdefinierte statische Zeichenfolge hinzu. Geben Sie einen beliebigen Schlüssel und einen beliebigen Zeichenfolgenwert ein.
  - **Property:** Fügt jeder Nachricht die aktuelle vom Gerät gemeldete Eigenschaft oder den Cloudeigenschaftswert hinzu. Geben Sie einen beliebigen Schlüssel ein, und wählen Sie eine Geräte- oder Cloudeigenschaft aus. Wenn die exportierte Nachricht von einem Gerät stammt, das nicht über die angegebene Geräte- oder Cloudeigenschaft verfügt, weist die exportierte Nachricht diese Anreicherung nicht auf.

## <a name="4-add-destinations"></a>4. Hinzufügen von Zielen
Erstellen Sie ein neues Ziel, oder fügen Sie ein Ziel hinzu, das Sie bereits erstellt haben. 
  
1. Klicken Sie auf den Link **Neues Ziel erstellen**. Geben Sie die folgenden Informationen ein:
    - **Zielname:** der Anzeigename des Ziels in IoT Central
    - **Zieltyp:** Wählen Sie den Typ des Ziels aus. Falls noch nicht geschehen, [legen Sie das Exportziel fest](#set-up-export-destination).
    - Bei Azure Event Hubs, Azure Service Bus-Warteschlange oder -Thema, fügen Sie die Verbindungszeichenfolge für Ihre Ressource ein. 
    - Bei Azure Blob Storage fügen Sie die Verbindungszeichenfolge für Ihre Ressource ein, und geben Sie den Containernamen ein (Groß-/Kleinschreibung beachten).
    - Bei Webhook fügen Sie die Callback-URL für Ihren Webhookendpunkt ein. 
    - Klicken Sie auf **Erstellen**

2. Klicken Sie auf **+ Ziel**, und wählen Sie ein Ziel aus der Dropdownliste aus. Sie können bis zu fünf Ziele zu einem einzigen Export hinzufügen.

3. Nachdem Sie die Einrichtung des Exports abgeschlossen haben, klicken Sie auf **Speichern**. Nach einigen Minuten werden Ihre Daten an den Zielorten angezeigt.

## <a name="export-contents-and-format"></a>Exportieren von Inhalten und des Formats

### <a name="azure-blob-storage-destination"></a>Azure Blob Storage-Ziel

Daten werden einmal pro Minute exportiert, wobei jede Datei den Batch der Änderungen seit der letzten exportierten Datei enthält. Exportierte Daten werden in drei Ordnern im JSON-Format gespeichert. Die Standardpfade in Ihrem Speicherkonto sind:

- Telemetriedaten: _{container}/{app-id}/{partition_id}/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}_
- Eigenschaftsänderungen: _{container}/{app-id}/{partition_id}/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}_

Wenn Sie die exportierten Dateien im Azure-Portal durchsuchen möchten, navigieren Sie zu der gewünschten Datei, und wählen Sie die Registerkarte **Blob bearbeiten** aus.

### <a name="azure-event-hubs-and-azure-service-bus-destinations"></a>Azure Event Hubs- und Azure Service Bus-Ziele

Daten werden nahezu in Echtzeit exportiert. Die Daten befinden sich im Nachrichtentext und liegen im JSON-Format vor, das als UTF-8 codiert ist. 

In der Sammlung der mit den Anmerkungen bzw. Systemeigenschaften der Nachricht finden Sie `iotcentral-device-id`, `iotcentral-application-id`, `iotcentral-message-source` und `iotcentral-message-type` mit denselben Werten wie die entsprechenden Felder im Nachrichtentext.

### <a name="webhook-destination"></a>Webhookziel
Für Webhookziele werden Daten ebenfalls nahezu in Echtzeit exportiert. Die Daten befinden sich im Nachrichtentext im gleichen Format wie für Event Hubs und Service Bus.


## <a name="telemetry-format"></a>Telemetrieformat
Jede exportierte Nachricht enthält eine normalisierte Form der vollständigen Nachricht, die das Gerät im Nachrichtentext als im UTF-8 codierten JSON-Format gesendet hat. In jeder Nachricht sind weitere Informationen enthalten:

- `applicationId` von der IoT Central-App
- `messageSource`, d. h. *Telemetrie* zum Exportieren von Telemetriedaten
- `deviceId` von dem Gerät, das die Telemetrienachricht sendet
- `schema` ist der Name und die Version des Nutzdatenschemas
- `templateId` ist die dem Gerät zugeordnete Gerätevorlagen-ID
- `enrichments` sind alle Anreicherungen, die beim Export eingerichtet wurden
- `messageProperties` sind die zusätzlichen Datenelemente, die das Gerät zusammen mit der Nachricht gesendet hat. Dies wird auch als *Anwendungseigenschaften* bezeichnet. [Mehr dazu finden Sie in der Dokumentation zu IoT Hubs](../../iot-hub/iot-hub-devguide-messages-construct.md).

Bei Event Hubs und Service Bus exportiert IoT Central eine neue Nachricht schnell, nachdem sie von einem Gerät eingetroffen ist.

Bei Blob Storage werden Nachrichten im Batch zusammengefasst und einmal pro Minute exportiert.

Das folgende Beispiel zeigt eine exportierte Telemetrienachricht:

```json

{
    "applicationId": "1dffa667-9bee-4f16-b243-25ad4151475e",
    "messageSource": "telemetry",
    "deviceId": "1vzb5ghlsg1",
    "schema": "default@preview",
    "templateId": "urn:qugj6vbw5:___qbj_27r",
    "enqueuedTime": "2020-08-05T22:26:55.455Z",
    "telemetry": {
        "Activity": "running",
        "BloodPressure": {
            "Diastolic": 7,
            "Systolic": 71
        },
        "BodyTemperature": 98.73447010562934,
        "HeartRate": 88,
        "HeartRateVariability": 17,
        "RespiratoryRate": 13
    },
    "enrichments": {
      "userSpecifiedKey": "sampleValue"
    },
    "messageProperties": {
      "messageProp": "value"
    }
}
```

## <a name="property-changes-format"></a>Format für Eigenschaftsänderungen

Jede Nachricht bzw. jeder Datensatz stellt eine Änderung an einem Gerät oder einer Cloudeigenschaft dar. Bei Geräteeigenschaften werden nur Änderungen des gemeldeten Werts als separate Nachricht exportiert. In jeder exportierten Nachricht sind weitere Informationen enthalten:

- `applicationId` von der IoT Central-App
- `messageSource`, d. h. *Eigenschaften* zum Exportieren von Eigenschaftsänderungsdaten
- `messageType`, d. h. entweder *cloudPropertyChange* oder *devicePropertyDesiredChange*, *devicePropertyReportedChange*
- `deviceId` von dem Gerät, dessen Eigenschaften sich geändert haben
- `schema` ist der Name und die Version des Nutzdatenschemas
- `templateId` ist die dem Gerät zugeordnete Gerätevorlagen-ID
- `enrichments` sind alle Anreicherungen, die beim Export eingerichtet wurden

Bei Event Hubs und Service Bus exportiert IoT Central neue Nachrichtendaten nahezu in Echtzeit an Ihren Event Hub oder Ihre Service Bus-Warteschlange bzw. Ihr Service Bus-Thema.

Bei Blob Storage werden Nachrichten im Batch zusammengefasst und einmal pro Minute exportiert.

Das folgende Beispiel zeigt eine exportierte Eigenschaftsänderungsnachricht, die in Azure Blob Storage empfangen wurde.

```json
{
    "applicationId": "1dffa667-9bee-4f16-b243-25ad4151475e",
    "messageSource": "properties",
    "messageType": "cloudPropertyChange",
    "deviceId": "18a985g1fta",
    "schema": "default@preview",
    "templateId": "urn:qugj6vbw5:___qbj_27r",
    "enqueuedTime": "2020-08-05T22:37:32.942Z",
    "properties": [{
        "fieldName": "MachineSerialNumber",
        "value": "abc"
    }],
    "enrichments": {
        "userSpecifiedKey" : "sampleValue"
    }
}
```

## <a name="comparison-of-legacy-data-export-and-new-data-export"></a>Vergleich des Legacy-Datenexports und des neuen Datenexports
Dabei handelt es sich um eine Tabelle, in der die Unterschiede zwischen dem Legacy-Datenexport und dem neuen Datenexport hervorgehoben werden. Informationen zum Legacy-Datenexport finden Sie [hier](howto-export-data.md).

| Funktionen  | Legacy-Datenexport | Neuer Datenexport |
| :------------- | :---------- | :----------- |
| Verfügbare Datentypen | Telemetrie, Geräte, Gerätevorlagen | Telemetrie, Eigenschaftsänderungen |
| Filterung | Keiner | Hängt vom exportierten Datentyp ab. Für Telemetrie, Filtern nach Telemetrie, Nachrichteneigenschaften, Eigenschaftswerte |
| Anreicherungen | Keiner | Anreichern mit einer benutzerdefinierten Zeichenfolge oder einem Eigenschaftswert auf dem Gerät |
| Destinations | Azure Event Hubs, Azure Service Bus-Warteschlangen und -Themen, Azure Blob Storage | Wie bei Legacy-Datenexport und Webhooks| 
| Unterstützte Apps | V2 und V3 | Nur V3 |
| Relevante Grenzwerte | 5 Exporte pro App, 1 Ziel pro Export | 10 Exporte-Ziel-Verbindungen pro App | 

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie jetzt mit der Verwendung des neuen Datenexports vertraut sind, können Sie mit dem nächsten Schritt fortfahren:

> [!div class="nextstepaction"]
> [Analysieren von Gerätedaten mithilfe von Analytics](./howto-create-analytics.md)
