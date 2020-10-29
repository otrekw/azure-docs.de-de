---
title: Telemetrie-, Eigenschaften- und Befehlsnutzlasten in Azure IoT Central | Microsoft-Dokumentation
description: Mithilfe von Azure IoT Central-Gerätevorlagen können Sie die Telemetriedaten, Eigenschaften und Befehle angeben, die ein Gerät implementieren muss. Hier erfahren Sie Grundlegendes zum Format der Daten, die ein Gerät mit IoT Central austauschen kann.
author: dominicbetts
ms.author: dobett
ms.date: 06/12/2020
ms.topic: conceptual
ms.service: iot-central
services: iot-central
ms.custom: device-developer
ms.openlocfilehash: 9e5288bb177d5827f05003e4561bc79240a71b59
ms.sourcegitcommit: 6906980890a8321dec78dd174e6a7eb5f5fcc029
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2020
ms.locfileid: "92427862"
---
# <a name="telemetry-property-and-command-payloads"></a>Telemetrie-, Eigenschaften- und Befehlsnutzlasten

_Dieser Artikel gilt für Geräteentwickler._

Eine Gerätevorlage in Azure IoT Central ist eine Blaupause, die Folgendes definiert:

* Die Telemetriedaten, die ein Gerät an IoT Central sendet.
* Die Eigenschaften, die ein Gerät mit IoT Central synchronisiert.
* Die Befehle, die IoT Central auf einem Gerät aufruft.

In diesem Artikel werden für Geräteentwickler die JSON-Nutzlasten beschrieben, die Geräte für die in einer Gerätevorlage definierten Telemetriedaten, Eigenschaften und Befehle senden und empfangen.

Zwar wird nicht jeder mögliche Typ von Telemetrie-, Eigenschaften- und Befehlsnutzlast beschrieben, aber die Beispiele veranschaulichen alle wichtigen Typen.

Jedes Beispiel zeigt einen Ausschnitt aus dem Gerätefunktionsmodell (Device Capability Model, DCM), das den Typ und die JSON-Beispielnutzlasten definiert, um zu veranschaulichen, wie das Gerät mit der IoT Central-Anwendung interagieren sollte.

> [!NOTE]
> IoT Central akzeptiert jeden beliebigen gültigen JSON-Code, kann für Visualisierungen aber nur dann verwendet werden, wenn er mit einer Definition im DCM übereinstimmt. Sie können Daten exportieren, die keiner Definition entsprechen. Weitere Informationen finden Sie unter [Exportieren von IoT-Daten zu Zielen in Azure](howto-export-data.md).

Die JSON-Datei mit der Definition des Gerätefunktionsmodells verwendet [Digital Twin Definition Language (DTDL) V1](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v1-preview/dtdlv1.md). Diese Spezifikation enthält die Definition des Eigenschaftsformats `@id`.

Ein Beispiel für Gerätecode, der einige dieser gerade verwendeten Nutzlasten zeigt, finden Sie in den Tutorials [Erstellen einer Clientanwendung und Verbinden der Anwendung mit Ihrer Azure IoT Central-Anwendung (Node.js)](tutorial-connect-device-nodejs.md) und [Erstellen einer Clientanwendung und Verbinden der Anwendung mit Ihrer Azure IoT Central-Anwendung (Python)](tutorial-connect-device-python.md).

## <a name="view-raw-data"></a>Anzeigen von Rohdaten

In IoT Central können Sie die Rohdaten anzeigen, die ein Gerät an eine Anwendung sendet. Diese Ansicht ist hilfreich bei der Behebung von Problemen mit der von einem Gerät gesendeten Nutzlast. So zeigen Sie die von einem Gerät gesendeten Rohdaten an:

1. Navigieren Sie auf der Seite **Geräte** zum Gerät.

1. Wählen Sie die Registerkarte **Rohdaten** aus:

    :::image type="content" source="media/concepts-telemetry-properties-commands/raw-data.png" alt-text="Ansicht „Rohdaten“":::

    In dieser Ansicht können Sie die anzuzeigenden Spalten auswählen und einen Zeitbereich zum Anzeigen festlegen. In der Spalte für **Nicht modellierte Daten** werden die Daten des Geräts angezeigt, die keiner der Definitionen für Eigenschaften oder Telemetriedaten in der Gerätevorlage entsprechen.

## <a name="telemetry"></a>Telemetrie

### <a name="primitive-types"></a>Einfache Typen

In diesem Abschnitt werden Beispiele für einfache Telemetrietypen gezeigt, die ein Gerät an eine IoT Central-Anwendung streamt.

Der folgende Codeausschnitt aus einem DCM zeigt die Definition eines `boolean`-Telemetrietyps:

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "BooleanTelemetry"
  },
  "name": "BooleanTelemetry",
  "schema": "boolean"
}
```

Ein Geräteclient sollte die Telemetriedaten als JSON-Code senden, der wie im folgenden Beispiel aussieht:

```json
{ "BooleanTelemetry": true }
```

Der folgende Codeausschnitt aus einem DCM zeigt die Definition eines `string`-Telemetrietyps:

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "StringTelemetry"
  },
  "name": "StringTelemetry",
  "schema": "string"
}
```

Ein Geräteclient sollte die Telemetriedaten als JSON-Code senden, der wie im folgenden Beispiel aussieht:

```json
{ "StringTelemetry": "A string value - could be a URL" }
```

Der folgende Codeausschnitt aus einem DCM zeigt die Definition eines `integer`-Telemetrietyps:

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "IntegerTelemetry"
  },
  "name": "IntegerTelemetry",
  "schema": "integer"
}

```

Ein Geräteclient sollte die Telemetriedaten als JSON-Code senden, der wie im folgenden Beispiel aussieht:

```json
{ "IntegerTelemetry": 23 }
```

Der folgende Codeausschnitt aus einem DCM zeigt die Definition eines `double`-Telemetrietyps:

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "DoubleTelemetry"
  },
  "name": "DoubleTelemetry",
  "schema": "double"
}
```

Ein Geräteclient sollte die Telemetriedaten als JSON-Code senden, der wie im folgenden Beispiel aussieht:

```json
{ "DoubleTelemetry": 56.78 }
```

Der folgende Codeausschnitt aus einem DCM zeigt die Definition eines `dateTime`-Telemetrietyps:

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "DateTimeTelemetry"
  },
  "name": "DateTimeTelemetry",
  "schema": "dateTime"
}
```

Ein Geräteclient sollte die Telemetriedaten als JSON-Code senden, der wie im folgenden Beispiel aussieht. Dabei müssen `DateTime`-Typen ISO 8061-kompatibel sein:

```json
{ "DateTimeTelemetry": "2020-08-30T19:16:13.853Z" }
```

Der folgende Codeausschnitt aus einem DCM zeigt die Definition eines `duration`-Telemetrietyps:

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "DurationTelemetry"
  },
  "name": "DurationTelemetry",
  "schema": "duration"
}
```

Ein Geräteclient sollte die Telemetriedaten als JSON-Code senden, der wie im folgenden Beispiel aussieht. Dabei müssen Zeitspannen ISO 8601-kompatibel sein:

```json
{ "DurationTelemetry": "PT10H24M6.169083011336625S" }
```

### <a name="complex-types"></a>Komplexe Typen

In diesem Abschnitt werden Beispiele für komplexe Telemetrietypen gezeigt, die ein Gerät an eine IoT Central-Anwendung streamt.

Der folgende Codeausschnitt aus einem DCM zeigt die Definition eines `geopoint`-Telemetrietyps:

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "GeopointTelemetry"
  },
  "name": "GeopointTelemetry",
  "schema": "geopoint"
}
```

Ein Geräteclient sollte die Telemetriedaten als JSON-Code senden, der wie im folgenden Beispiel aussieht. IoT Central zeigt den Wert als Stecknadel auf einer Karte an:

```json
{
  "GeopointTelemetry": {
    "lat": 47.64263,
    "lon": -122.13035,
    "alt": 0
  }
}
```

Der folgende Codeausschnitt aus einem DCM zeigt die Definition eines `Enum`-Telemetrietyps:

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "EnumTelemetry"
  },
  "name": "EnumTelemetry",
  "schema": {
    "@id": "<element id>",
    "@type": "Enum",
    "displayName": {
      "en": "Enum"
    },
    "valueSchema": "integer",
    "enumValues": [
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Item1"
        },
        "enumValue": 0,
        "name": "Item1"
      },
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Item2"
        },
        "enumValue": 1,
        "name": "Item2"
      },
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Item3"
        },
        "enumValue": 2,
        "name": "Item3"
      }
    ]
  }
}
```

Ein Geräteclient sollte die Telemetriedaten als JSON-Code senden, der wie im folgenden Beispiel aussieht. Mögliche Werte sind `0`, `1` und `2`, die in IoT Central als `Item1`, `Item2` und `Item3` angezeigt werden:

```json
{ "EnumTelemetry": 1 }
```

Der folgende Codeausschnitt aus einem DCM zeigt die Definition eines `Object`-Telemetrietyps. Dieses Objekt enthält drei Felder mit den Typen `dateTime`, `integer` und `Enum`:

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "ObjectTelemetry"
  },
  "name": "ObjectTelemetry",
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
          "en": "Property1"
        },
        "name": "Property1",
        "schema": "dateTime"
      },
      {
        "@id": "<element id>",
        "@type": "SchemaField",
        "displayName": {
          "en": "Property2"
        },
        "name": "Property2",
        "schema": "integer"
      },
      {
        "@id": "<element id>",
        "@type": "SchemaField",
        "displayName": {
          "en": "Property3"
        },
        "name": "Property3",
        "schema": {
          "@id": "<element id>",
          "@type": "Enum",
          "displayName": {
            "en": "Enum"
          },
          "valueSchema": "integer",
          "enumValues": [
            {
              "@id": "<element id>",
              "@type": "EnumValue",
              "displayName": {
                "en": "Item1"
              },
              "enumValue": 0,
              "name": "Item1"
            },
            {
              "@id": "<element id>",
              "@type": "EnumValue",
              "displayName": {
                "en": "Item2"
              },
              "enumValue": 1,
              "name": "Item2"
            },
            {
              "@id": "<element id>",
              "@type": "EnumValue",
              "displayName": {
                "en": "Item3"
              },
              "enumValue": 2,
              "name": "Item3"
            }
          ]
        }
      }
    ]
  }
}
```

Ein Geräteclient sollte die Telemetriedaten als JSON-Code senden, der wie im folgenden Beispiel aussieht. `DateTime`-Typen müssen ISO 8061-kompatibel sein. Mögliche Werte für `Property3` sind `0` und `1`, die in IoT Central als `Item1`, `Item2` und `Item3` angezeigt werden:

```json
{
  "ObjectTelemetry": {
      "Property1": "2020-09-09T03:36:46.195Z",
      "Property2": 37,
      "Property3": 2
  }
}
```

Der folgende Codeausschnitt aus einem DCM zeigt die Definition eines `vector`-Telemetrietyps:

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "VectorTelemetry"
  },
  "name": "VectorTelemetry",
  "schema": "vector"
}
```

Ein Geräteclient sollte die Telemetriedaten als JSON-Code senden, der wie im folgenden Beispiel aussieht:

```json
{
  "VectorTelemetry": {
    "x": 74.72395045538597,
    "y": 74.72395045538597,
    "z": 74.72395045538597
  }
}
```

### <a name="event-and-state-types"></a>Ereignis- und Zustandstypen

In diesem Abschnitt werden Beispiele für Telemetrieereignisse und -zustände gezeigt, die ein Gerät an eine IoT Central-Anwendung sendet.

Der folgende Codeausschnitt aus einem DCM zeigt die Definition eines `integer`-Ereignistyps:

```json
{
  "@id": "<element id>",
  "@type": [
    "Telemetry",
    "SemanticType/Event"
  ],
  "displayName": {
    "en": "IntegerEvent"
  },
  "name": "IntegerEvent",
  "schema": "integer"
}
```

Ein Geräteclient sollte die Ereignisdaten als JSON-Code senden, der wie im folgenden Beispiel aussieht:

```json
{ "IntegerEvent": 74 }
```

Der folgende Codeausschnitt aus einem DCM zeigt die Definition eines `integer`-Zustandstyps:

```json
{
  "@id": "<element id>",
  "@type": [
    "Telemetry",
    "SemanticType/State"
  ],
  "displayName": {
    "en": "IntegerState"
  },
  "name": "IntegerState",
  "schema": {
    "@id": "<element id>",
    "@type": "Enum",
    "valueSchema": "integer",
    "enumValues": [
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Level1"
        },
        "enumValue": 1,
        "name": "Level1"
      },
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Level2"
        },
        "enumValue": 2,
        "name": "Level2"
      },
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Level3"
        },
        "enumValue": 3,
        "name": "Level3"
      }
    ]
  }
}
```

Ein Geräteclient sollte den Zustand als JSON-Code senden, der wie im folgenden Beispiel aussieht. Mögliche ganzzahlige Zustandswerte sind `1`, `2`oder `3`:

```json
{ "IntegerState": 2 }
```

## <a name="properties"></a>Eigenschaften

> [!NOTE]
> Die Nutzlastformate für Eigenschaften gelten für Anwendungen, die am 14.07.2020 oder danach erstellt wurden.

### <a name="primitive-types"></a>Einfache Typen

In diesem Abschnitt werden Beispiele für einfache Eigenschaftstypen gezeigt, die ein Gerät an eine IoT Central-Anwendung sendet.

Der folgende Codeausschnitt aus einem DCM zeigt die Definition eines `boolean`-Eigenschaftstyps:

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "BooleanProperty"
  },
  "name": "BooleanProperty",
  "schema": "boolean"
}
```

Ein Geräteclient sollte eine JSON-Nutzlast, die wie im folgenden Beispiel aussieht, als gemeldete Eigenschaft im Gerätezwilling senden:

```json
{ "BooleanProperty": false }
```

Der folgende Codeausschnitt aus einem DCM zeigt die Definition eines `boolean`-Eigenschaftstyps:

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "LongProperty"
  },
  "name": "LongProperty",
  "schema": "long"
}
```

Ein Geräteclient sollte eine JSON-Nutzlast, die wie im folgenden Beispiel aussieht, als gemeldete Eigenschaft im Gerätezwilling senden:

```json
{ "LongProperty": 439 }
```

Der folgende Codeausschnitt aus einem DCM zeigt die Definition eines `date`-Eigenschaftstyps:

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "DateProperty"
  },
  "name": "DateProperty",
  "schema": "date"
}
```

Ein Geräteclient sollte eine JSON-Nutzlast, die wie im folgenden Beispiel aussieht, als gemeldete Eigenschaft im Gerätezwilling senden. `Date`-Typen müssen ISO 8061-kompatibel sein:

```json
{ "DateProperty": "2020-05-17" }
```

Der folgende Codeausschnitt aus einem DCM zeigt die Definition eines `duration`-Eigenschaftstyps:

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "DurationProperty"
  },
  "name": "DurationProperty",
  "schema": "duration"
}
```

Ein Geräteclient sollte eine JSON-Nutzlast, die wie im folgenden Beispiel aussieht, als gemeldete Eigenschaft im Gerätezwilling senden. Dabei müssen Zeitspannen ISO 8601-kompatibel sein:

```json
{ "DurationProperty": "PT10H24M6.169083011336625S" }
```

Der folgende Codeausschnitt aus einem DCM zeigt die Definition eines `float`-Eigenschaftstyps:

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "FloatProperty"
  },
  "name": "FloatProperty",
  "schema": "float"
}
```

Ein Geräteclient sollte eine JSON-Nutzlast, die wie im folgenden Beispiel aussieht, als gemeldete Eigenschaft im Gerätezwilling senden:

```json
{ "FloatProperty": 1.9 }
```

Der folgende Codeausschnitt aus einem DCM zeigt die Definition eines `string`-Eigenschaftstyps:

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "StringProperty"
  },
  "name": "StringProperty",
  "schema": "string"
}
```

Ein Geräteclient sollte eine JSON-Nutzlast, die wie im folgenden Beispiel aussieht, als gemeldete Eigenschaft im Gerätezwilling senden:

```json
{ "StringProperty": "A string value - could be a URL" }
```

### <a name="complex-types"></a>Komplexe Typen

In diesem Abschnitt werden Beispiele für komplexe Eigenschaftstypen gezeigt, die ein Gerät an eine IoT Central-Anwendung sendet.

Der folgende Codeausschnitt aus einem DCM zeigt die Definition eines `geopoint`-Eigenschaftstyps:

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "GeopointProperty"
  },
  "name": "GeopointProperty",
  "schema": "geopoint"
}
```

Ein Geräteclient sollte eine JSON-Nutzlast, die wie im folgenden Beispiel aussieht, als gemeldete Eigenschaft im Gerätezwilling senden:

```json
{
  "GeopointProperty": {
    "lat": 47.64263,
    "lon": -122.13035,
    "alt": 0
  }
}
```

Der folgende Codeausschnitt aus einem DCM zeigt die Definition eines `Enum`-Eigenschaftstyps:

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "EnumProperty"
  },
  "name": "EnumProperty",
  "schema": {
    "@id": "<element id>",
    "@type": "Enum",
    "displayName": {
      "en": "Enum"
    },
    "valueSchema": "integer",
    "enumValues": [
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Item1"
        },
        "enumValue": 0,
        "name": "Item1"
      },
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Item2"
        },
        "enumValue": 1,
        "name": "Item2"
      },
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Item3"
        },
        "enumValue": 2,
        "name": "Item3"
      }
    ]
  }
}
```

Ein Geräteclient sollte eine JSON-Nutzlast, die wie im folgenden Beispiel aussieht, als gemeldete Eigenschaft im Gerätezwilling senden. Mögliche Werte sind `0`, `1` und ‚2‘, die in IoT Central als `Item1`, `Item2` und `Item3` angezeigt werden:

```json
{ "EnumProperty": 1 }
```

Der folgende Codeausschnitt aus einem DCM zeigt die Definition eines `Object`-Eigenschaftstyps. Dieses Objekt enthält zwei Felder mit den Typen `string` und `integer`:

```json
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

Ein Geräteclient sollte eine JSON-Nutzlast, die wie im folgenden Beispiel aussieht, als gemeldete Eigenschaft im Gerätezwilling senden:

```json
{
  "ObjectProperty": {
    "Field1": 37,
    "Field2": "A string value"
  }
}
```

Der folgende Codeausschnitt aus einem DCM zeigt die Definition eines `vector`-Eigenschaftstyps:

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "VectorProperty"
  },
  "name": "VectorProperty",
  "schema": "vector"
}
```

Ein Geräteclient sollte eine JSON-Nutzlast, die wie im folgenden Beispiel aussieht, als gemeldete Eigenschaft im Gerätezwilling senden:

```json
{
  "VectorProperty": {
    "x": 74.72395045538597,
    "y": 74.72395045538597,
    "z": 74.72395045538597
  }
}
```

### <a name="writeable-property-types"></a>Schreibbare Eigenschaftstypen

In diesem Abschnitt werden Beispiele für schreibbare Eigenschaftstypen gezeigt, die ein Gerät von einer IoT Central-Anwendung empfängt.

IoT Central erwartet vom Gerät eine Antwort zu Aktualisierungen von schreibbaren Eigenschaften. Die Antwortnachricht sollte die Felder `ac` und `av` enthalten. Das Feld `ad` ist optional. Beispiele hierzu finden Sie in den folgenden Codeausschnitten.

`ac` ist ein numerisches Feld, in dem die Werte in der folgenden Tabelle verwendet werden:

| Wert | Bezeichnung | BESCHREIBUNG |
| ----- | ----- | ----------- |
| `'ac': 200` | Abgeschlossen | Der Vorgang einer Eigenschaftsänderung wurde erfolgreich abgeschlossen. |
| `'ac': 202` oder `'ac': 201` | Ausstehend | Der Vorgang einer Eigenschaftsänderung ist ausstehend oder in Bearbeitung. |
| `'ac': 4xx` | Fehler | Die angeforderte Eigenschaftsänderung war ungültig oder fehlerhaft. |
| `'ac': 5xx` | Fehler | Beim Verarbeiten der angeforderten Änderung ist auf dem Gerät ein unerwarteter Fehler aufgetreten. |

`av` ist die an das Gerät gesendete Versionsnummer.

`ad` ist eine Beschreibung der Optionszeichenfolge.

Der folgende Codeausschnitt aus einem DCM zeigt die Definition eines schreibbaren `string`-Eigenschaftstyps:

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "StringPropertyWritable"
  },
  "name": "StringPropertyWritable",
  "writable": true,
  "schema": "string"
}
```

Das Gerät empfängt die folgende Nutzlast von IoT Central:

```json
{  
  "StringPropertyWritable": "A string from IoT Central", "$version": 7
}
```

Das Gerät sollte nach Verarbeitung des Updates die folgende JSON-Nutzlast an IoT Central senden. Diese Nachricht enthält die Versionsnummer des ursprünglichen Updates, das von IoT Central empfangen wurde. Wenn IoT Central diese Nachricht empfängt, kennzeichnet es die Eigenschaft in der Benutzeroberfläche als **synchronisiert** :

```json
{
  "StringPropertyWritable": {
    "value": "A string from IoT Central",
    "ac": 200,
    "ad": "completed",
    "av": 7
  }
}
```

Der folgende Codeausschnitt aus einem DCM zeigt die Definition eines schreibbaren `Enum`-Eigenschaftstyps:

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "EnumPropertyWritable"
  },
  "name": "EnumPropertyWritable",
  "writable": true,
  "schema": {
    "@id": "<element id>",
    "@type": "Enum",
    "displayName": {
      "en": "Enum"
    },
    "valueSchema": "integer",
    "enumValues": [
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Item1"
        },
        "enumValue": 0,
        "name": "Item1"
      },
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Item2"
        },
        "enumValue": 1,
        "name": "Item2"
      },
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Item3"
        },
        "enumValue": 2,
        "name": "Item3"
      }
    ]
  }
}
```

Das Gerät empfängt die folgende Nutzlast von IoT Central:

```json
{  
  "EnumPropertyWritable":  1 , "$version": 10
}
```

Das Gerät sollte nach Verarbeitung des Updates die folgende JSON-Nutzlast an IoT Central senden. Diese Nachricht enthält die Versionsnummer des ursprünglichen Updates, das von IoT Central empfangen wurde. Wenn IoT Central diese Nachricht empfängt, kennzeichnet es die Eigenschaft in der Benutzeroberfläche als **synchronisiert** :

```json
{
  "EnumPropertyWritable": {
    "value": 1,
    "ac": 200,
    "ad": "completed",
    "av": 10
  }
}
```

## <a name="commands"></a>Befehle

### <a name="synchronous-command-types"></a>Synchrone Befehlstypen

Der folgende Codeausschnitt aus einem DCM zeigt die Definition eines synchronen Befehls, der keine Parameter hat und nicht erwartet, dass das Gerät etwas zurückgibt:

```json
{
  "@id": "<element id>",
  "@type": "Command",
  "commandType": "synchronous",
  "durable": false,
  "displayName": {
    "en": "SynchronousCommandBasic"
  },
  "name": "SynchronousCommandBasic"
}
```

Das Gerät empfängt in der Anforderung eine leere Nutzlast und sollte in der Antwort eine leere Nutzlast mit dem HTTP-Antwortcode `200` zur Angabe von Erfolg zurückgeben.

Der folgende Codeausschnitt aus einem DCM zeigt die Definition eines synchronen Befehls, der einen ganzzahligen Parameter hat und erwartet, dass das Gerät einen ganzzahligen Wert zurückgibt:

```json
{
  "@id": "<element id>",
  "@type": "Command",
  "commandType": "synchronous",
  "durable": false,
  "request": {
    "@id": "<element id>",
    "@type": "SchemaField",
    "displayName": {
      "en": "RequestParam"
    },
    "name": "RequestParam",
    "schema": "integer"
  },
  "response": {
    "@id": "<element id>",
    "@type": "SchemaField",
    "displayName": {
      "en": "ResponseParam"
    },
    "name": "ResponseParam",
    "schema": "integer"
  },
  "displayName": {
    "en": "SynchronousCommandSimple"
  },
  "name": "SynchronousCommandSimple"
}
```

Das Gerät empfängt einen ganzzahligen Wert als Anforderungsnutzlast. Das Gerät sollte einen ganzzahligen Wert als Antwortnutzlast mit dem HTTP-Antwortcode `200` zur Angabe von Erfolg zurückgeben.

Der folgende Codeausschnitt aus einem DCM zeigt die Definition eines synchronen Befehls, der einen Objektparameter hat und erwartet, dass das Gerät ein Objekt zurückgibt. In diesem Beispiel enthalten beide Objekte ganzzahlige Felder und Zeichenfolgenfelder:

```json
{
  "@id": "<element id>",
  "@type": "Command",
  "commandType": "synchronous",
  "durable": false,
  "request": {
    "@id": "<element id>",
    "@type": "SchemaField",
    "displayName": {
      "en": "RequestParam"
    },
    "name": "RequestParam",
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
  },
  "response": {
    "@id": "<element id>",
    "@type": "SchemaField",
    "displayName": {
      "en": "ResponseParam"
    },
    "name": "ResponseParam",
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
  },
  "displayName": {
    "en": "SynchronousCommandComplex"
  },
  "name": "SynchronousCommandComplex"
}
```

Der folgende Codeausschnitt zeigt ein Beispiel für eine Anforderungsnutzlast, die an das Gerät gesendet wird:

```json
{ "Field1": 56, "Field2": "A string value" }
```

Der folgende Codeausschnitt zeigt ein Beispiel für eine Antwortnutzlast, die vom Gerät gesendet wurde. Verwenden Sie den HTTP-Antwortcode `200` zur Angabe von Erfolg:

```json
{ "Field1": 87, "Field2": "Another string value" }
```

### <a name="asynchronous-command-types"></a>Asynchrone Befehlstypen

Der folgende Codeausschnitt aus einem DCM zeigt die Definition eines asynchronen Befehls. Der Befehl hat einen ganzzahligen Parameter und erwartet, dass das Gerät einen ganzzahligen Wert zurückgibt:

```json
{
  "@id": "<element id>",
  "@type": "Command",
  "commandType": "asynchronous",
  "durable": false,
  "request": {
    "@id": "<element id>",
    "@type": "SchemaField",
    "displayName": {
      "en": "RequestParam"
    },
    "name": "RequestParam",
    "schema": "integer"
  },
  "response": {
    "@id": "<element id>",
    "@type": "SchemaField",
    "displayName": {
      "en": "ResponseParam"
    },
    "name": "ResponseParam",
    "schema": "integer"
  },
  "displayName": {
    "en": "AsynchronousCommandSimple"
  },
  "name": "AsynchronousCommandSimple"
}
```

Das Gerät empfängt einen ganzzahligen Wert als Anforderungsnutzlast. Das Gerät sollte eine leere Antwortnutzlast mit dem HTTP-Antwortcode `202` zurückgeben, um anzugeben, dass das Gerät die Anforderung für asynchrone Verarbeitung akzeptiert hat.

Wenn das Gerät die Verarbeitung der Anforderung abgeschlossen hat, sollte es eine Eigenschaft an IoT Central senden, die wie im folgenden Beispiel aussieht. Der Eigenschaftsname muss mit dem Befehlsnamen identisch sein:

```json
{
  "AsynchronousCommandSimple": {
    "value": 87
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich als Geräteentwickler mit Gerätevorlagen vertraut gemacht haben, finden Sie unter [Herstellen einer Verbindung mit Azure IoT Central](./concepts-get-connected.md) weitere Informationen dazu, wie Sie Geräte bei IoT Central registrieren und wie IoT Central Geräteverbindungen schützt.
