---
title: Verwenden von Eigenschaften in einer Azure IoT Central-Lösung
description: Hier erfahren Sie, wie Sie schreibgeschützte und schreibbare Eigenschaften in einer Azure IoT Central-Lösung verwenden können.
author: dominicbetts
ms.author: dobett
ms.date: 08/12/2020
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 1cc4f40374fce83589d2dc10a0422b91f5178c0b
ms.sourcegitcommit: 7dacbf3b9ae0652931762bd5c8192a1a3989e701
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2020
ms.locfileid: "92123782"
---
# <a name="use-properties-in-an-azure-iot-central-solution"></a>Verwenden von Eigenschaften in einer Azure IoT Central-Lösung

In diesem Artikel wird gezeigt, wie Sie Geräteeigenschaften verwenden können, die in Ihrer Azure IoT Central-Anwendung in einer Gerätevorlage definiert werden.

Eigenschaften stellen Zeitpunktwerte dar. Ein Gerät kann beispielsweise eine Eigenschaft verwenden, um die Zieltemperatur zu melden, die es zu erreichen versucht. Mithilfe von Eigenschaften können Sie auch den Zustand zwischen Ihrem Gerät und Ihrer Azure IoT Central-Anwendung synchronisieren. Sie können schreibbare Eigenschaften aus Azure IoT Central heraus festlegen.

Außerdem können Sie Cloudeigenschaften in einer Azure IoT Central-Anwendung definieren. Cloudeigenschaftswerte werden niemals mit einem Gerät ausgetauscht und deshalb in diesem Artikel nicht behandelt.

## <a name="define-your-properties"></a>Definieren Ihrer Eigenschaften

Eigenschaften sind Datenfelder, die den Zustand des Geräts darstellen. Verwenden Sie Eigenschaften zur Darstellung von dessen dauerhaftem Zustand, z. B. seinem „Ein/Aus“-Status. Eigenschaften können auch grundlegende Geräteeigenschaften darstellen, z. B. die Softwareversion des Geräts. Sie können Eigenschaften als schreibgeschützt oder beschreibbar deklarieren.

Der folgende Screenshot zeigt eine Eigenschaftsdefinition in einer Azure IoT Central-Anwendung.

![Der Screenshot zeigt eine Eigenschaftsdefinition in einer Azure IoT Central-Anwendung.](./media/howto-use-properties/property-definition.png)

In der folgenden Tabelle sind die Konfigurationseinstellungen für eine Eigenschaftsfunktion angegeben.

| Feld           | BESCHREIBUNG                                                                                                                                                                                                                        |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Anzeigename    | Der Anzeigename für den Eigenschaftswert, der in Dashboards und Formularen verwendet wird.                                                                                                                                                              |
| Name            | Der Name der Eigenschaft. Azure IoT Central generiert einen Wert für dieses Feld aus dem Anzeigenamen. Bei Bedarf können Sie aber einen eigenen Wert auswählen. Dieses Feld muss alphanumerisch sein.                                                 |
| Funktionstyp | Eigenschaft.                                                                                                                                                                                                                          |
| Semantischer Typ   | Der semantische Typ der Eigenschaft, z. B. Temperatur, Zustand oder Ereignis. Die Auswahl des semantischen Typs bestimmt, welches der folgenden Felder verfügbar ist.                                                                       |
| Schema          | Der Eigenschaftsdatentyp, z.B. „double“, „string“ oder „vector“. Die verfügbaren Optionen werden durch den semantischen Typ bestimmt. Schema ist für die semantischen Typen „Ereignis“ und „Zustand“ nicht verfügbar.                                               |
| Schreibbar       | Wenn die Eigenschaft nicht schreibbar ist, kann das Gerät Eigenschaftswerte an Azure IoT Central melden. Wenn die Eigenschaft schreibbar ist, kann das Gerät Eigenschaftswerte an Azure IoT Central melden. Anschließend kann Azure IoT Central Eigenschaftsaktualisierungen an das Gerät senden. |
| severity        | Nur für den semantischen Typ „Ereignis“ verfügbar. Die Schweregrade lauten **Fehler**, **Information** und **Warnung**.                                                                                                                         |
| Zustandswerte    | Nur für den semantischen Typ „Zustand“ verfügbar. Definieren Sie die möglichen Zustandswerte, die jeweils einen Anzeigenamen, Namen, Enumerationstyp und Wert umfassen.                                                                                   |
| Einheit            | Eine Einheit für den Eigenschaftswert, z. B. **km/h**, **%** oder **&deg; C**.                                                                                                                                                              |
| Anzeigeeinheit    | Eine Anzeigeeinheit zur Verwendung in Dashboards und Formularen.                                                                                                                                                                                    |
| Comment         | Beliebige Kommentare zur Eigenschaftsfunktion.                                                                                                                                                                                        |
| BESCHREIBUNG     | Eine Beschreibung der Eigenschaftsfunktion.                                                                                                                                                                                          |

Die Eigenschaften können auch in einer Schnittstelle in einer Gerätevorlage definiert werden, wie hier gezeigt wird:

``` json
{
  "@type": "Property",
  "displayName": "Device State",
  "description": "The state of the device. Two states online/offline are available.",
  "name": "state",
  "schema": "boolean"
},
{
  "@type": "Property",
  "displayName": "Customer Name",
  "description": "The name of the customer currently operating the device.",
  "name": "name",
  "schema": "string",
  "writable": true
},
{
 "@type": "Property",
 "displayName": "Date ",
 "description": "The date on which the device is currently operating",
 "name": "date",
 "writable": true,
 "schema": "date"
},
{ 
 "@type": "Property",
 "displayName": "Location",
 "description": "The current location of the device",
 "name": "location",
 "writable": true,
 "schema": "geopoint"
},
{
 "@type": "Property",
 "displayName": "Vector Level",
 "description": "The Vector level of the device",
 "name": "vector",
 "writable": true,
 "schema": "vector"
}
```

In diesem Beispiel werden fünf Eigenschaften gezeigt. Diese Eigenschaften können mit der Eigenschaftsdefinition in der Benutzeroberfläche verknüpft werden, wie hier gezeigt wird:

* `@type` – zur Angabe des Funktionstyps: `Property`
* `name` – zur Angabe des Eigenschaftswerts.
* `schema` – zur Angabe des Datentyps für die Eigenschaft. Dieser Wert kann ein einfacher Typ sein, z. B. „double“, „integer“, „Boolean“ oder „string“. Komplexe Objekttypen, Arrays und Zuordnungen werden ebenfalls unterstützt.
* `writable` Eigenschaften sind standardmäßig schreibgeschützt. Mit diesem Feld können Sie eine Eigenschaft als „schreibbar“ kennzeichnen.

Über optionale Felder, z. B. Anzeigename und Beschreibung, können Sie der Schnittstelle und den Funktionen weitere Details hinzufügen.

Beim Erstellen einer Eigenschaft können Sie die komplexen **Schema**typen wie „Objekt“ und „Enumeration“ angeben.

![Der Screenshot zeigt, wie eine Funktion hinzugefügt wird.](./media/howto-use-properties/property.png)

Wenn Sie das komplexe **Schema**, z. B. **Objekt** auswählen, müssen Sie auch das Objekt definieren.

![Der Screenshot zeigt, wie ein Objekt definiert wird.](./media/howto-use-properties/object.png)

Der folgende Code zeigt die Definition eines Eigenschaftstyps „Objekt“. Dieses Objekt hat zwei Felder mit den Typen „string“ und „integer“.

``` json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "ObjectProperty"
  },
  "name": "ObjectProperty",
  "schema": {
    "@id": "<element id>",
    "@type": "Object",
    "displayName": {
      "en": "Object"
    },
    "fields": [
      {
        "@id": "<element id>",
        "@type": "SchemaField",
        "displayName": {
          "en": "Field1"
        },
        "name": "Field1",
        "schema": "integer"
      },
      {
        "@id": "<element id>",
        "@type": "SchemaField",
        "displayName": {
          "en": "Field2"
        },
        "name": "Field2",
        "schema": "string"
      }
    ]
  }
}
```

## <a name="implement-read-only-properties"></a>Implementieren von schreibgeschützten Eigenschaften

Standardmäßig sind Eigenschaften schreibgeschützt. Schreibgeschützte Eigenschaften bedeuten, dass das Gerät Aktualisierungen der Eigenschaftswerte an Ihre Azure IoT Central-Anwendung meldet. Ihre Azure IoT Central-Anwendung kann den Wert einer schreibgeschützten Eigenschaft nicht festlegen.

Azure IoT Central verwendet Gerätezwillinge zum Synchronisieren von Eigenschaftswerten zwischen dem Gerät und der Azure IoT Central-Anwendung. Die gemeldeten Eigenschaften von Gerätezwillingen werden für die Eigenschaftswerte eines Geräts verwendet. Weitere Informationen finden Sie unter [Gerätezwillinge](../../iot-hub/tutorial-device-twins.md).

Der folgende Codeausschnitt aus einem Gerätefunktionsmodell zeigt die Definition eines schreibgeschützten Eigenschaftstyps:

``` json
{
  "@type": "Property",
  "name": "model",
  "displayName": "Device model",
  "schema": "string",
  "comment": "Device model name or ID. Ex. Surface Book 2."
}
```

Schreibgeschützte Eigenschaften werden vom Gerät an Azure IoT Central gesendet. Die Eigenschaften werden als JSON-Nutzlast gesendet. Weitere Informationen finden Sie unter [Payloads](./concepts-telemetry-properties-commands.md) (Nutzlasten).

Sie können mithilfe des Azure IoT-Geräte-SDKs eine Eigenschaftsaktualisierung an Ihre Azure IoT Central-Anwendung senden.

Gerätezwillingseigenschaften können mithilfe der folgenden Funktion an Ihre Azure IoT Central-Anwendung gesendet werden:

``` javascript
hubClient.getTwin((err, twin) => {
    const properties = {
        model: 'environmentalSensor1.0'
    };
    twin.properties.reported.update(properties, (err) => {
        console.log(err ? `error: ${err.toString()}` : `status: success` )
    });
});
```

In diesem Artikel wird der Einfachheit halber „Node.js“ verwendet. Vollständige Informationen zu Beispielen für Geräteanwendungen finden Sie in den folgenden Tutorials:

* [Erstellen einer Clientanwendung und Verbinden der Anwendung mit Ihrer Azure IoT Central-Anwendung (Node.js)](tutorial-connect-device-nodejs.md)
* [Erstellen einer Clientanwendung und Verbinden der Anwendung mit Ihrer Azure IoT Central-Anwendung (Python)](tutorial-connect-device-python.md)

Die folgende Ansicht in der Azure IoT Central-Anwendung zeigt die Eigenschaften, die Sie sehen können. In der Ansicht wird aus der Eigenschaft **Gerätemodell** automatisch eine _schreibgeschützte Geräteeigenschaft_.

![Der Screenshot zeigt die Ansicht einer schreibgeschützten Eigenschaft.](./media/howto-use-properties/read-only.png)

## <a name="implement-writable-properties"></a>Implementieren von schreibbaren Eigenschaften

Schreibbare Eigenschaften werden von einem Operator in der Azure IoT Central-Anwendung in einem Formular festgelegt. Azure IoT Central sendet die-Eigenschaft an das Gerät. Azure IoT Central erwartet eine Bestätigung vom Gerät.

Der folgende Codeausschnitt aus einem Gerätefunktionsmodell zeigt die Definition eines schreibbaren Eigenschaftstyps:

``` json
{
  "@type": "Property",
  "displayName": "Brightness Level",
  "description": "The brightness level for the light on the device. Can be specified as 1 (high), 2 (medium), 3 (low)",
  "name": "brightness",
  "writable": true,
  "schema": "long"
}
```

Ein Geräteclient sollte eine JSON-Nutzlast, die wie im folgenden Beispiel aussieht, als gemeldete Eigenschaft im Gerätezwilling senden:

``` json
{ "Brightness Level": 2 }
```

Zum Definieren und Verarbeiten der schreibbaren Eigenschaften, auf die Ihr Gerät reagiert, können Sie den folgenden Code verwenden:

``` javascript
hubClient.getTwin((err, twin) => {
    twin.on('properties.desired.brightness', function(desiredBrightness) {
        console.log( `Received setting: ${desiredBrightness.value}` );
        var patch = {
            brightness: {
                value: desiredBrightness.value,
                ad: 'success',
                ac: 200,
                av: desiredBrightness.$version
            }
        }
        twin.properties.reported.update(patch, (err) => {
            console.log(err ? `error: ${err.toString()}` : `status: success` )
        });
    });
});
```

Die Antwortnachricht sollte die Felder `ac` und `av` enthalten. Das Feld `ad` ist optional. Beispiele hierzu finden Sie in den folgenden Codeausschnitten:

* `ac` ist ein numerisches Feld, in dem die Werte in der folgenden Tabelle verwendet werden.
* `av` ist die an das Gerät gesendete Versionsnummer.
* `ad` ist eine Beschreibung der Optionszeichenfolge.

| Wert | Bezeichnung | BESCHREIBUNG |
| ----- | ----- | ----------- |
| `'ac': 200` | Abgeschlossen | Der Vorgang einer Eigenschaftsänderung wurde erfolgreich abgeschlossen. |
| `'ac': 202` oder `'ac': 201` | Ausstehend | Der Vorgang einer Eigenschaftsänderung ist ausstehend oder in Bearbeitung. |
| `'ac': 4xx` | Fehler | Die angeforderte Eigenschaftsänderung war ungültig oder fehlerhaft. |
| `'ac': 5xx` | Fehler | Beim Verarbeiten der angeforderten Änderung ist auf dem Gerät ein unerwarteter Fehler aufgetreten. |


Weitere Informationen zu Gerätezwillingen finden Sie unter [Konfigurieren Ihrer Geräte über einen Back-End-Dienst](../../iot-hub/tutorial-device-twins.md).

Wenn der Operator in der Azure IoT Central-Anwendung eine schreibbare Eigenschaft festlegt, verwendet die Anwendung eine gewünschte Eigenschaft des Gerätezwillings, um den Wert an das Gerät zu senden. Das Gerät antwortet dann mithilfe einer gemeldeten Eigenschaft des Gerätezwillings. Wenn Azure IoT Central den gemeldeten Eigenschaftswert empfängt, wird die Eigenschaftsansicht mit dem Status **Akzeptiert** aktualisiert.

In der folgenden Ansicht werden die schreibbaren Eigenschaften gezeigt. Wenn Sie den Wert eingeben und **Speichern** auswählen, lautet der Anfangsstatus **Ausstehend**. Wenn das Gerät die Änderung akzeptiert, wird der Status in **Akzeptiert** geändert.

![Der Screenshot zeigt den Status „Ausstehend“.](./media/howto-use-properties/status-pending.png)

![Der Screenshot zeigt die Eigenschaft „Akzeptiert“.](./media/howto-use-properties/accepted.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, wie Sie Eigenschaften in Ihrer Azure IoT Central-Anwendung verwenden können, lesen Sie jetzt:

* [Payloads](concepts-telemetry-properties-commands.md)
* [Erstellen einer Clientanwendung und Verbinden der Anwendung mit Ihrer Azure IoT Central-Anwendung (Node.js)](tutorial-connect-device-nodejs.md)