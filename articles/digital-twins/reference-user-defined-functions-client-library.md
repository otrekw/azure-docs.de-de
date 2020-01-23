---
title: Clientbibliotheksreferenz für benutzerdefinierte Funktionen – Azure Digital Twins | Microsoft-Dokumentation
description: Dokumentation zur Clientbibliotheksreferenz für benutzerdefinierte Funktionen in Azure Digital Twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: article
ms.date: 01/17/2020
ms.custom: seodec18
ms.openlocfilehash: bd6095daca51ddca0cfb4b34ca86e763df9a3d02
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2020
ms.locfileid: "76276817"
---
# <a name="user-defined-functions-client-library-reference"></a>Clientbibliotheksreferenz für benutzerdefinierte Funktionen

Das Dokument enthält Referenzinformationen für die Clientbibliothek mit benutzerdefinierten Funktionen in Azure Digital Twins.

## <a name="helper-methods"></a>Hilfsmethoden

Die Clientbibliothek definiert Hilfsmethoden für häufig verwendete Vorgänge.

### <a name="getspacemetadataid--space"></a>getSpaceMetadata(id) ⇒ `space`

Bei angegebenem Raumbezeichner ruft diese Funktion den Raum aus dem Graphen ab.

**Art**: globale Funktion

| Parameter  | type                | Beschreibung  |
| ---------- | ------------------- | ------------ |
| *id*  | `guid` | Raumbezeichner |

### <a name="getsensormetadataid--sensor"></a>getSensorMetadata(id) ⇒ `sensor`

Bei angegebenem Sensorbezeichner ruft diese Funktion den Sensor aus dem Graphen ab.

**Art**: globale Funktion

| Parameter  | type                | Beschreibung  |
| ---------- | ------------------- | ------------ |
| *id*  | `guid` | Sensorbezeichner |

### <a name="getdevicemetadataid--device"></a>getDeviceMetadata(id) ⇒ `device`

Bei angegebenem Gerätebezeichner ruft diese Funktion das Gerät aus dem Graphen ab.

**Art**: globale Funktion

| Parameter  | type                | Beschreibung  |
| ------ | ------------------- | ------------ |
| *id* | `guid` | Gerätebezeichner |

### <a name="getsensorvaluesensorid-datatype--value"></a>getSensorValue(sensorId, dataType) ⇒ `value`

Nach Angabe eines Sensorbezeichners und dessen Datentyps ruft diese Funktion den aktuellen Wert dieses Sensors ab.

**Art**: globale Funktion

| Parameter  | type                | Beschreibung  |
| ------ | ------------------- | ------------ |
| *sensorId*  | `guid` | Sensorbezeichner |
| *dataType*  | `string` | Sensordatentyp |

### <a name="getspacevaluespaceid-valuename--value"></a>getSpaceValue(spaceId, valueName) ⇒ `value`

Nach Angabe eines Raumbezeichners und des Wertnamens ruft diese Funktion den aktuellen Wert für diese Raumeigenschaft ab.

**Art**: globale Funktion

| Parameter  | type                | Beschreibung  |
| ------ | ------------------- | ------------ |
| *spaceId*  | `guid` | Raumbezeichner |
| *valueName* | `string` | Name der Raumeigenschaft |

### <a name="getsensorhistoryvaluessensorid-datatype--value"></a>getSensorHistoryValues(sensorId, dataType) ⇒ `value[]`

Nach Angabe eines Sensorbezeichners und dessen Datentyps ruft diese Funktion die früheren Werte dieses Sensors ab.

**Art**: globale Funktion

| Parameter  | type                | Beschreibung  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | Sensorbezeichner |
| *dataType* | `string` | Sensordatentyp |

### <a name="getspacehistoryvaluesspaceid-datatype--value"></a>getSpaceHistoryValues(spaceId, dataType) ⇒ `value[]`

Nach Angabe eines Raumbezeichners und des Wertnamens ruft diese Funktion die früheren Werte dieser Raumeigenschaft ab.

**Art**: globale Funktion

| Parameter  | type                | Beschreibung  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Raumbezeichner |
| *valueName* | `string` | Name der Raumeigenschaft |

### <a name="getspacechildspacesspaceid--space"></a>getSpaceChildSpaces(spaceId) ⇒ `space[]`

Nach Angabe eines Raumbezeichners ruft diese Funktion die untergeordneten Räume dieses übergeordneten Raums ab.

**Art**: globale Funktion

| Parameter  | type                | Beschreibung  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Raumbezeichner |

### <a name="getspacechildsensorsspaceid--sensor"></a>getSpaceChildSensors(spaceId) ⇒ `sensor[]`

Nach Angabe eines Raumbezeichners ruft diese Funktion die untergeordneten Sensoren dieses übergeordneten Raums ab.

**Art**: globale Funktion

| Parameter  | type                | Beschreibung  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Raumbezeichner |

### <a name="getspacechilddevicesspaceid--device"></a>getSpaceChildDevices(spaceId) ⇒ `device[]`

Nach Angabe eines Raumbezeichners ruft diese Funktion die untergeordneten Geräte dieses übergeordneten Raums ab.

**Art**: globale Funktion

| Parameter  | type                | Beschreibung  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Raumbezeichner |

### <a name="getdevicechildsensorsdeviceid--sensor"></a>getDeviceChildSensors(deviceId) ⇒ `sensor[]`

Nach Angabe eines Gerätebezeichners ruft diese Funktion die untergeordneten Sensoren dieses übergeordneten Geräts ab.

**Art**: globale Funktion

| Parameter  | type                | Beschreibung  |
| ------ | ------------------- | ------------ |
| *deviceId* | `guid` | Gerätebezeichner |

### <a name="getspaceparentspacechildspaceid--space"></a>getSpaceParentSpace(childSpaceId) ⇒ `space`

Nach Angabe eines Raumbezeichners ruft diese Funktion den übergeordneten Raum ab.

**Art**: globale Funktion

| Parameter  | type                | Beschreibung  |
| ------ | ------------------- | ------------ |
| *childSpaceId* | `guid` | Raumbezeichner |

### <a name="getsensorparentspacechildsensorid--space"></a>getSensorParentSpace(childSensorId) ⇒ `space`

Nach Angabe eines Sensorbezeichners ruft diese Funktion den übergeordneten Raum ab.

**Art**: globale Funktion

| Parameter  | type                | Beschreibung  |
| ------ | ------------------- | ------------ |
| *childSensorId* | `guid` | Sensorbezeichner |

### <a name="getdeviceparentspacechilddeviceid--space"></a>getDeviceParentSpace(childDeviceId) ⇒ `space`

Nach Angabe eines Gerätebezeichners ruft diese Funktion den übergeordneten Raum ab.

**Art**: globale Funktion

| Parameter  | type                | Beschreibung  |
| ------ | ------------------- | ------------ |
| *childDeviceId* | `guid` | Gerätebezeichner |

### <a name="getsensorparentdevicechildsensorid--space"></a>getSensorParentDevice(childSensorId) ⇒ `space`

Nach Angabe eines Sensorbezeichners ruft diese Funktion das übergeordnete Gerät ab.

**Art**: globale Funktion

| Parameter  | type                | Beschreibung  |
| ------ | ------------------- | ------------ |
| *childSensorId* | `guid` | Sensorbezeichner |

### <a name="getspaceextendedpropertyspaceid-propertyname--extendedproperty"></a>getSpaceExtendedProperty(spaceId, propertyName) ⇒ `extendedProperty`

Nach Angabe eines Raumbezeichners ruft diese Funktion die Eigenschaft und deren Wert aus dem Raum ab.

**Art**: globale Funktion

| Parameter  | type                | Beschreibung  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Raumbezeichner |
| *propertyName* | `string` | Name der Raumeigenschaft |

### <a name="getsensorextendedpropertysensorid-propertyname--extendedproperty"></a>getSensorExtendedProperty(sensorId, propertyName) ⇒ `extendedProperty`

Nach Angabe eines Sensorbezeichners ruft diese Funktion die Eigenschaft und deren Wert vom Sensor ab.

**Art**: globale Funktion

| Parameter  | type                | Beschreibung  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | Sensorbezeichner |
| *propertyName* | `string` | Name der Sensoreigenschaft |

### <a name="getdeviceextendedpropertydeviceid-propertyname--extendedproperty"></a>getDeviceExtendedProperty(deviceId, propertyName) ⇒ `extendedProperty`

Nach Angabe eines Gerätebezeichners ruft diese Funktion die Eigenschaft und deren Wert vom Gerät ab.

**Art**: globale Funktion

| Parameter  | type                | Beschreibung  |
| ------ | ------------------- | ------------ |
| *deviceId* | `guid` | Gerätebezeichner |
| *propertyName* | `string` | Name der Geräteeigenschaft |

### <a name="setsensorvaluesensorid-datatype-value"></a>setSensorValue(sensorId, dataType, value)

Diese Funktion legt den Wert des Sensorobjekts mit dem angegebenen Datentyp fest.

**Art**: globale Funktion

| Parameter  | type                | Beschreibung  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | Sensorbezeichner |
| *dataType*  | `string` | Sensordatentyp |
| *value*  | `string` | value |

### <a name="setspacevaluespaceid-datatype-value"></a>setSpaceValue(spaceId, dataType, value)

Diese Funktion legt den Wert des Raumobjekts mit dem angegebenen Datentyp fest.

**Art**: globale Funktion

| Parameter  | type                | Beschreibung  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Raumbezeichner |
| *dataType* | `string` | Datentyp |
| *value* | `string` | value |

### <a name="logmessage"></a>log(message)

Diese Funktion protokolliert die folgende Meldung in der benutzerdefinierten Funktion.

**Art**: globale Funktion

| Parameter  | type                | Beschreibung  |
| ------ | ------------------- | ------------ |
| *Nachricht* | `string` | Die zu protokollierende Meldung |

### <a name="sendnotificationtopologyobjectid-topologyobjecttype-payload"></a>sendNotification(topologyObjectId, topologyObjectType, payload)

Diese Funktion sendet eine benutzerdefinierte Benachrichtigung, die erledigt werden soll.

**Art**: globale Funktion

| Parameter  | type                | Beschreibung  |
| ------ | ------------------- | ------------ |
| *topologyObjectId*  | `guid` | Bezeichner des Graphobjekts. Beispiele: Raum-, Sensor- und Geräte-ID.|
| *topologyObjectType*  | `string` | Beispiele: Sensor und Gerät.|
| *payload*  | `string` | Die JSON-Nutzlast, die mit der Benachrichtigung gesendet werden soll. |

## <a name="return-types"></a>Rückgabetypen

Die Antwortmodelle, die von Clientreferenz-Hilfsmethoden zurückgegeben werden, sind nachfolgend beschrieben.

### <a name="space"></a>LeerZchn

```JSON
{
  "id": "00000000-0000-0000-0000-000000000000",
  "name": "Space",
  "friendlyName": "Conference Room",
  "typeId": 0,
  "parentSpaceId": "00000000-0000-0000-0000-000000000001",
  "subtypeId": 0
}
```

### <a name="space-methods"></a>Raummethoden

#### <a name="parent--space"></a>Parent() ⇒ `space`

Diese Funktion gibt den übergeordneten Raum des aktuellen Raums zurück.

#### <a name="childsensors--sensor"></a>ChildSensors() ⇒ `sensor[]`

Diese Funktion gibt die untergeordneten Sensoren des aktuellen Raums zurück.

#### <a name="childdevices--device"></a>ChildDevices() ⇒ `device[]`

Diese Funktion gibt die untergeordneten Geräte des aktuellen Raums zurück.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Diese Funktion gibt die erweiterte Eigenschaft und deren Wert für den aktuellen Raum zurück.

| Parameter  | type                | Beschreibung  |
| ------ | ------------------- | ------------ |
| *propertyName* | `string` | Name der erweiterten Eigenschaft |

#### <a name="valuevaluename--value"></a>Value(valueName) ⇒ `value`

Diese Funktion gibt den Wert des aktuellen Raums zurück.

| Parameter  | type                | Beschreibung  |
| ------ | ------------------- | ------------ |
| *valueName* | `string` | Name des Werts |

#### <a name="historyvaluename--value"></a>History(valueName) ⇒ `value[]`

Diese Funktion gibt die früheren Werte des aktuellen Raums zurück.

| Parameter  | type                | Beschreibung  |
| ------ | ------------------- | ------------ |
| *valueName* | `string` | Name des Werts |

#### <a name="notifypayload"></a>Notify(payload)

Diese Funktion sendet eine Benachrichtigung mit der angegebenen Nutzlast.

| Parameter  | type                | Beschreibung  |
| ------ | ------------------- | ------------ |
| *payload* | `string` | JSON-Nutzlast, die in die Benachrichtigung eingefügt werden soll |

### <a name="device"></a>Sicherungsmedium

```JSON
{
  "id": "00000000-0000-0000-0000-000000000002",
  "name": "Device",
  "friendlyName": "Temperature Sensing Device",
  "description": "This device contains a sensor that captures temperature readings.",
  "type": "None",
  "subtype": "None",
  "typeId": 0,
  "subtypeId": 0,
  "hardwareId": "ABC123",
  "gatewayId": "ABC",
  "spaceId": "00000000-0000-0000-0000-000000000000"
}
```

### <a name="device-methods"></a>Gerätemethoden

#### <a name="parent--space"></a>Parent() ⇒ `space`

Diese Funktion gibt den übergeordneten Raum des aktuellen Geräts zurück.

#### <a name="childsensors--sensor"></a>ChildSensors() ⇒ `sensor[]`

Diese Funktion gibt die untergeordneten Sensoren des aktuellen Geräts zurück.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Diese Funktion gibt die erweiterte Eigenschaft und deren Wert für das aktuelle Gerät zurück.

| Parameter  | type                | Beschreibung  |
| ------ | ------------------- | ------------ |
| *propertyName* | `string` | Name der erweiterten Eigenschaft |

#### <a name="notifypayload"></a>Notify(payload)

Diese Funktion sendet eine Benachrichtigung mit der angegebenen Nutzlast.

| Parameter  | type                | Beschreibung  |
| ------ | ------------------- | ------------ |
| *payload* | `string` | JSON-Nutzlast, die in die Benachrichtigung eingefügt werden soll |

### <a name="sensor"></a>Sensor

```JSON
{
  "id": "00000000-0000-0000-0000-000000000003",
  "port": "30",
  "pollRate": 3600,
  "dataType": "Temperature",
  "dataSubtype": "None",
  "type": "Classic",
  "portType": "None",
  "dataUnitType": "FahrenheitTemperature",
  "spaceId": "00000000-0000-0000-0000-000000000000",
  "deviceId": "00000000-0000-0000-0000-000000000001",
  "portTypeId": 0,
  "dataUnitTypeId": 0,
  "dataTypeId": 0,
  "dataSubtypeId": 0,
  "typeId": 0  
}
```

### <a name="sensor-methods"></a>Sensormethoden

#### <a name="space--space"></a>Space() ⇒ `space`

Diese Funktion gibt den übergeordneten Raum des aktuellen Sensors zurück.

#### <a name="device--device"></a>Device() ⇒ `device`

Diese Funktion gibt das übergeordnete Gerät des aktuellen Sensors zurück.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Diese Funktion gibt die erweiterte Eigenschaft und deren Wert für den aktuellen Sensor zurück.

| Parameter  | type                | Beschreibung  |
| ------ | ------------------- | ------------ |
| *propertyName* | `string` | Name der erweiterten Eigenschaft |

#### <a name="value--value"></a>Value() ⇒ `value`

Diese Funktion gibt den Wert des aktuellen Sensors zurück.

#### <a name="history--value"></a>History() ⇒ `value[]`

Diese Funktion gibt die früheren Werte des aktuellen Sensors zurück.

#### <a name="notifypayload"></a>Notify(payload)

Diese Funktion sendet eine Benachrichtigung mit der angegebenen Nutzlast.

| Parameter  | type                | Beschreibung  |
| ------ | ------------------- | ------------ |
| *payload* | `string` | JSON-Nutzlast, die in die Benachrichtigung eingefügt werden soll |

### <a name="value"></a>value

```JSON
{
  "dataType": "Temperature",
  "value": "70",
  "createdTime": ""
}
```

### <a name="extended-property"></a>Erweiterte Eigenschaft

```JSON
{
  "name": "OccupancyStatus",
  "value": "Occupied"
}
```

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu [Benutzerdefinierten Funktionen in Azure Digital Twins](./concepts-user-defined-functions.md).

- Informationen zum [Erstellen von benutzerdefinierten Funktionen](./how-to-user-defined-functions.md).

- Informationen zum [Debuggen von benutzerdefinierten Funktionen](./how-to-diagnose-user-defined-functions.md).
