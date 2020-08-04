---
title: Gerätevorlagen in Azure IoT Central | Microsoft-Dokumentation
description: Mit Azure IoT Central-Gerätevorlagen können Sie das Verhalten der mit Ihrer Anwendung verbundenen Geräte festlegen.
author: dominicbetts
ms.author: dobett
ms.date: 05/21/2020
ms.topic: conceptual
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 1eab1022e9b0f03597c108c3c927909aa9bb2712
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87337108"
---
# <a name="what-are-device-templates"></a>Was sind Gerätevorlagen?

_Dieser Artikel richtet sich an Geräte- und Lösungsentwickler._

Eine Gerätevorlage in Azure IoT Central ist eine Blaupause, die die Merkmale und das Verhalten eines Gerätetyps definiert, der eine Verbindung mit Ihrer Anwendung herstellt. Beispielsweise definiert die Gerätevorlage die Telemetrie, die ein Gerät sendet, sodass IoT Central Visualisierungen mit den richtigen Einheiten und Datentypen erstellen kann.

Ein Lösungsgenerator fügt einer IoT Central-Anwendung Gerätevorlagen hinzu. Ein Geräteentwickler schreibt den Gerätecode, der die in der Gerätevorlage definierten Verhalten umsetzt.

Eine Gerätevorlage enthält die folgenden Abschnitte:

- _Ein Gerätefunktionsmodell_. In diesem Teil der Gerätevorlage wird definiert, wie das Gerät mit Ihrer Anwendung interagiert. Ein Geräteentwickler setzt die im Gerätefunktionsmodell definierten Verhalten um.
- _Cloudeigenschaften_. In diesem Teil der Gerätevorlage kann der Entwickler der Lösung alle zu speichernden Gerätemetadaten angeben. Cloudeigenschaften werden nicht mit Geräten synchronisiert und sind ausschließlich in der Anwendung vorhanden. Cloudeigenschaften haben keinen Einfluss auf den Code, den ein Geräteentwickler zur Umsetzung des Gerätefunktionsmodells schreibt.
- _Anpassungen_. In diesem Teil der Gerätevorlage kann der Lösungsentwickler einige der Definitionen im Gerätefunktionsmodell überschreiben. Anpassungen sind nützlich, wenn der Lösungsentwickler präzisieren möchte, wie die Anwendung mit einem Wert umgeht, z. B. wenn der Anzeigename für eine Eigenschaft oder die zur Anzeige eines Telemetriewerts verwendete Farbe geändert werden soll. Anpassungen wirken sich nicht auf den Code aus, der von einem Geräteentwickler zur Umsetzung des Gerätefunktionsmodells geschrieben wird.
- _Ansichten_. In diesem Teil der Gerätevorlage kann der Lösungsentwickler Visualisierungen zum Anzeigen von Daten aus dem Gerät und Formulare zur Verwaltung und Steuerung eines Geräts definieren. Die Ansichten enthalten das Gerätefunktionsmodell, die Cloudeigenschaften und Anpassungen. Ansichten haben keinen Einfluss auf den Code, den ein Geräteentwickler zur Umsetzung des Gerätefunktionsmodells schreibt.

> [!NOTE]
> Das [Aktualisierungsrelease für IoT Plug & Play (öffentliche Vorschau)](../../iot-pnp/overview-iot-plug-and-play.md) richtet sich an Geräteentwickler und OEMs, damit sie mit dem Entwickeln von Geräten beginnen können, die für IoT Plug & Play vor der allgemeinen Verfügbarkeit zertifiziert werden können.

## <a name="device-capability-models"></a>Gerätefunktionsmodelle

Ein Gerätefunktionsmodell definiert, wie ein Gerät mit ihrer IoT Central-Anwendung interagiert. Der Geräteentwickler muss sicherstellen, dass das Gerät die im Gerätefunktionsmodell definierten Verhaltensweisen umsetzt, damit IoT Central das Gerät überwachen und verwalten kann. Ein Gerätefunktionsmodell besteht aus einer oder mehreren _Schnittstellen_. Jede Schnittstelle kann eine Sammlung von _Telemetrietypen_, _Geräteeigenschaften_ und _Befehlen_ definieren. Ein Lösungsentwickler kann eine JSON-Datei mit der Definition des Gerätefunktionsmodells in eine Gerätevorlage importieren oder ein Gerätefunktionsmodell auf der Webbenutzeroberfläche von IoT Central erstellen oder bearbeiten. Änderungen an einem Gerätefunktionsmodell, die auf der Webbenutzeroberfläche vorgenommen werden, erfordern eine [Versionsverwaltung der Gerätevorlage](./howto-version-device-template.md).

Ein Lösungsentwickler kann auch eine JSON-Datei exportieren, die das Gerätefunktionsmodell enthält. Ein Geräteentwickler kann anhand dieses JSON-Dokuments verstehen, wie das Gerät mit der IoT Central-Anwendung kommunizieren soll.

Die JSON-Datei mit der Definition des Gerätefunktionsmodells verwendet [Digital Twin Definition Language (DTDL) V1](https://github.com/Azure/IoTPlugandPlay/tree/master/DTDL). IoT Central erwartet, dass die JSON-Datei das Gerätefunktionsmodell mit inline und nicht in separaten Dateien definierten Schnittstellen enthält.

Ein typisches IoT-Gerät besteht aus folgenden Komponenten:

- Benutzerdefinierte Komponenten, die Ihr Gerät einzigartig machen
- Standardkomponenten, die bei allen Geräten identisch sind

Diese Komponenten werden in einem Gerätefunktionsmodell als _Schnittstellen_ bezeichnet. Schnittstellen definieren die Details der einzelnen mit Ihrem Gerät implementierten Komponenten. Schnittstellen lassen sich in mehreren Gerätefunktionsmodellen wiederverwenden.

Das folgende Beispiel zeigt den Entwurf eines Gerätefunktionsmodells für ein Umgebungssensorgerät mit zwei Schnittstellen:

```json
{
  "@id": "urn:contoso:sensor_device:1",
  "@type": "CapabilityModel",
  "displayName": "Environment Sensor Capability Model",
  "implements": [
    {
      "@type": "InterfaceInstance",
      "name": "deviceinfo",
      "schema": {
        "@id": "urn:azureiot:DeviceManagement:DeviceInformation:1",
        "@type": "Interface",
        "displayName": "Device Information",
        "@context": "http://azureiot.com/v1/contexts/IoTModel.json",
        "contents": [
          ...
        ]
      }
    },
    {
      "@type": "InterfaceInstance",
      "name": "sensor",
      "schema": {
        "@id": "urn:contoso:EnvironmentalSensor:1",
        "@type": "Interface",
        "displayName": "Environmental Sensor",
        "@context": "http://azureiot.com/v1/contexts/IoTModel.json",
        "contents": [
          ...
        ]
      }
    }
  ],
  "@context": "http://azureiot.com/v1/contexts/IoTModel.json"
}
```

Ein Funktionsmodell umfasst einige Pflichtfelder:

- `@id`: eine eindeutige ID im Format eines einfachen Uniform Resource Name
- `@type`: deklariert dieses Objekt als Funktionsmodell
- `@context`: gibt die für das Funktionsmodell verwendete DTDL-Version an
- `implements`: listet die Schnittstellen auf, die das Gerät implementiert

Jeder Eintrag in der Liste der Schnittstellen im Abschnitt „implements“ enthält jeweils Folgendes:

- `name`: der Programmiername der Schnittstelle
- `schema`: die Schnittstelle, die mit dem Funktionsmodell implementiert wird

Eine Schnittstelle umfasst einige Pflichtfelder:

- `@id`: eine eindeutige ID im Format eines einfachen Uniform Resource Name
- `@type`: deklariert dieses Objekt als Schnittstelle
- `@context`: gibt die für die Schnittstelle verwendete DTDL-Version an
- `contents`: listet die Eigenschaften, Telemetriedaten und Befehle auf, die das Gerät ausmachen

Es gibt einige optionale Felder, über die Sie dem Funktionsmodell weitere Details hinzufügen können, z. B. den Anzeigenamen und eine Beschreibung.

### <a name="interface"></a>Schnittstelle

Mit DTDL beschreiben Sie die Funktionen Ihres Geräts. Zusammengehörige Funktionen sind in Schnittstellen gruppiert. Schnittstellen beschreiben die Eigenschaften, Telemetrie und Befehle, die eine Komponente des Geräts implementiert:

- `Properties`. Eigenschaften sind Datenfelder, die den Zustand des Geräts darstellen. Verwenden Sie Eigenschaften, um den dauerhaften Zustand des Geräts darzustellen, z. B. den Ein/Aus-Status einer Kühlmittelpumpe. Eigenschaften können außerdem grundlegende Geräteeigenschaften darstellen, z. B. die Firmwareversion des Geräts. Sie können Eigenschaften als schreibgeschützt oder beschreibbar deklarieren.
- `Telemetry`. Telemetriefelder stellen Messungen von Sensoren dar. Immer wenn das Gerät eine Sensormessung vornimmt, sollte ein Telemetrieereignis mit den Sensordaten gesendet werden.
- `Commands`. Befehle stellen Methoden dar, die Benutzer des Geräts auf dem Gerät ausführen können, beispielsweise ein Befehl zum Zurücksetzen oder ein Befehl zum Ein- oder Ausschalten eines Lüfters.

Das folgende Beispiel zeigt die Definition der Schnittstelle „Environmental Sensor“ (Umgebungssensor):

```json
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
  "@type": [
    "Telemetry",
    "SemanticType/Temperature"
  ],
  "description": "Current temperature on the device",
  "displayName": "Temperature",
  "name": "temp",
  "schema": "double",
  "unit": "Units/Temperature/fahrenheit"
},
{
  "@type": "Command",
  "name": "turnon",
  "comment": "This Commands will turn-on the LED light on the device.",
  "commandType": "synchronous"
},
{
  "@type": "Command",
  "name": "turnoff",
  "comment": "This Commands will turn-off the LED light on the device.",
  "commandType": "synchronous"
}
```

In diesem Beispiel werden zwei Eigenschaften, ein Telemetrietyp und zwei Befehle gezeigt. Eine minimale Feldbeschreibung umfasst Folgendes:

- `@type` zum Angeben des Typs der Funktion (`Telemetry`, `Property` oder `Command`).  In einigen Fällen enthält der Typ einen semantischen Typ, damit IoT Central einige Annahmen darüber treffen kann, wie der Wert zu behandeln ist.
- `name` für den Telemetriewert.
- `schema` zum Angeben des Datentyps der Telemetrie oder Eigenschaft. Dieser Wert kann ein primitiver Typ sein, z. B. „double“, „integer“, „boolean“ oder „string“. Komplexe Objekttypen, Arrays und Zuordnungen werden ebenfalls unterstützt.
- `commandType` zum Angeben, wie der Befehl behandelt werden soll.

Über optionale Felder, z. B. Anzeigename und Beschreibung, können Sie der Schnittstelle und den Funktionen weitere Details hinzufügen.

### <a name="properties"></a>Eigenschaften

Standardmäßig sind Eigenschaften schreibgeschützt. Schreibgeschützte Eigenschaften bedeuten, dass das Gerät Aktualisierungen der Eigenschaftswerte an Ihre IoT Central-Anwendung sendet. Ihre IoT Central-Anwendung kann den Wert einer schreibgeschützten Eigenschaft nicht festlegen.

Sie können eine Eigenschaft auch als beschreibbar für eine Schnittstelle markieren. Ein Gerät kann eine Aktualisierung einer beschreibbaren Eigenschaft von Ihrer IoT Central-Anwendung empfangen sowie Aktualisierungen der Eigenschaftswerte an Ihre Anwendung senden.

Geräte müssen nicht verbunden sein, um Eigenschaftswerte festzulegen. Die aktualisierten Werte werden übertragen, wenn das Gerät das nächste Mal mit der Anwendung verbunden wird. Dieses Verhalten gilt sowohl für schreibgeschützte als auch für beschreibbare Eigenschaften.

Verwenden Sie keine Eigenschaften, um Telemetriedaten von Ihrem Gerät zu senden. Beispielsweise bedeutet die schreibgeschützte `temperatureSetting=80`-Eigenschaft, dass die Gerätetemperatur auf 80 festgelegt wurde und das Gerät versucht, diese Temperatur zu erreichen oder zu halten.

Bei beschreibbaren Eigenschaften gibt die Geräteanwendung den Statuscode, die Version und die Beschreibung eines gewünschten Zustands zurück, um anzugeben, ob der Eigenschaftswert empfangen und angewandt wurde.

### <a name="telemetry"></a>Telemetrie

Mit IoT Central können Sie Telemetrie auf Dashboards und in Diagrammen anzeigen und Regeln verwenden, um Aktionen auszulösen, wenn Schwellenwerte erreicht werden. IoT Central verwendet die Informationen im Gerätefunktionsmodell wie Datentypen, Einheiten und Anzeigenamen, um zu bestimmen, wie Telemetriewerte angezeigt werden sollen.

Sie können die Datenexportfunktion von IoT Central nutzen, um Telemetrie an andere Ziele wie Azure Storage oder Event Hubs zu streamen.

### <a name="commands"></a>Befehle

Befehle sind entweder synchron oder asynchron. Ein synchroner Befehl muss standardmäßig innerhalb von 30 Sekunden ausgeführt werden, und das Gerät muss beim Eingehen des Befehls verbunden sein. Wenn das Gerät nicht rechtzeitig antwortet oder nicht verbunden ist, wird der Befehl nicht ausgeführt.

Verwenden Sie asynchrone Befehle für Vorgänge mit langer Ausführungszeit. Das Gerät sendet Statusinformationen unter Verwendung von Telemetrienachrichten. Diese Statusmeldungen enthalten die folgenden Headereigenschaften:

- `iothub-command-name`: der Befehlsname, z. B. `UpdateFirmware`
- `iothub-command-request-id`: die Anforderungs-ID, die serverseitig generiert und beim ersten Aufruf an das Gerät gesendet wurde
- `iothub-interface-id`:  die ID der Schnittstelle, für die dieser Befehl definiert ist, z. B. `urn:example:AssetTracker:1`
 `iothub-interface-name`: der Instanzname der Schnittstelle, z. B. `myAssetTracker`
- `iothub-command-statuscode`: der vom Gerät zurückgegebene Statuscode, z. B. `202`

## <a name="cloud-properties"></a>Cloudeigenschaften

Cloudeigenschaften sind Teil der Gerätevorlage, aber nicht des Gerätefunktionsmodells. Mit Cloudeigenschaften kann der Lösungsentwickler Gerätemetadaten angeben, die in der IoT Central-Anwendung gespeichert werden sollen. Cloudeigenschaften haben keinen Einfluss auf den Code, den ein Geräteentwickler zur Umsetzung des Gerätefunktionsmodells schreibt.

Ein Lösungsentwickler kann neben den Geräteeigenschaften auch Cloudeigenschaften zu Dashboards und Ansichten hinzufügen, um einen Bediener in die Lage zu versetzen, die mit der Anwendung verbundenen Geräte zu verwalten. Ein Lösungsentwickler kann Cloudeigenschaften auch als Teil einer Regeldefinition verwenden, damit ein Schwellenwert von einem Bediener bearbeitet werden kann.

## <a name="customizations"></a>Anpassungen

Anpassungen sind Teil der Gerätevorlage, jedoch nicht des Gerätefunktionsmodells. Durch Anpassungen kann der Lösungsentwickler einige der Definitionen im Gerätefunktionsmodell optimieren oder überschreiben. Beispielsweise kann ein Lösungsentwickler den Anzeigenamen für einen Telemetrietyp oder eine Eigenschaft ändern. Ein Lösungsentwickler kann auch Anpassungen verwenden, um eine Überprüfung hinzuzufügen, z. B. eine minimale oder maximale Länge für eine Geräteeigenschaft in Form einer Zeichenfolge.

Anpassungen wirken sich ggf. auf den Code aus, der von einem Geräteentwickler zum Umsetzen des Gerätefunktionsmodells geschrieben wird. Eine Anpassung kann beispielsweise minimale und maximale Zeichenfolgenlängen oder minimale und maximale numerische Werte für Telemetrie festlegen.

## <a name="views"></a>Ansichten

Ein Lösungsentwickler erstellt Ansichten, mit denen Bediener angeschlossene Geräte überwachen und verwalten können. Ansichten sind Teil der Gerätevorlage, sodass eine Ansicht einem bestimmten Gerätetyp zugeordnet ist. Eine Ansicht kann Folgendes enthalten:

- Diagramme zum Zeichnen von Telemetrie
- Kacheln zum Anzeigen schreibgeschützter Geräteeigenschaften
- Kacheln, damit der Bediener beschreibbare Geräteeigenschaften bearbeiten kann
- Kacheln, damit der Bediener Cloudeigenschaften bearbeiten kann
- Kacheln, damit der Bediener Befehle aufrufen kann, darunter solche, die eine Nutzlast erwarten
- Kacheln zum Anzeigen von Beschriftungen, Bildern oder Markdowntext

Die Telemetrie, Eigenschaften und Befehle, die Sie einer Ansicht hinzufügen können, werden durch das Gerätefunktionsmodell, die Cloudeigenschaften und benutzerdefinierte Anpassungen in der Gerätevorlage bestimmt.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich als Geräteentwickler über Gerätevorlagen informiert haben, sollten Sie als Nächstes den Artikel [Telemetrie-, Eigenschaften- und Befehlsnutzlasten](./concepts-telemetry-properties-commands.md) lesen, um mehr über die Daten zu erfahren, die ein Gerät mit IoT Central austauscht.

Wir empfehlen Ihnen als Nächstes die Lektüre von [Definieren eines neuen IoT-Gerätetyps in Ihrer Azure IoT Central-Anwendung](./howto-set-up-template.md), um mehr darüber zu erfahren, wie Sie eine Gerätevorlage erstellen können.
