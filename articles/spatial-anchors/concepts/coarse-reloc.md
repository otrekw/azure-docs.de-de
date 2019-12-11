---
title: Ungefähre Standortbestimmung
description: Schnellstart zur ungefähren Standortbestimmung
author: bucurb
manager: dacoghl
services: azure-spatial-anchors
ms.author: bobuc
ms.date: 09/18/2019
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.openlocfilehash: 3477bac051346e4b334ff3437085c402090b2c98
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74765460"
---
# <a name="coarse-relocalization"></a>Ungefähre Standortbestimmung

Die ungefähre Standortbestimmung ist ein Feature, das eine vorläufige Antwort auf folgende Frage liefert: *Wo befindet sich mein Gerät gerade/Welche Inhalte sollte ich beobachten?* Die Antwort ist nicht präzise und weist das folgende Format auf: *Sie befinden sich in der nähe dieser Ankerpunkte. Versuchen Sie, einen von ihnen zu finden.*

Bei der ungefähren Standortbestimmung werden verschiedene Sensormesswerte des Geräts zum Erstellen und Abfragen der Ankerpunkte zugeordnet. Bei Szenarios im Freien handelt es sich bei den Sensordaten üblicherweise um die GPS-Position des Geräts. Wenn GPS nicht verfügbar oder unzuverlässig ist (beispielsweise in Gebäuden), bestehen die Sensordaten aus den WLAN-Zugangspunkten und Bluetooth-Beacons in Reichweite. Alle erfassten Sensordaten tragen zu einem räumlichen Index bei. Der räumliche Index wird vom Ankerdienst genutzt, um die Ankerpunkte zu ermitteln, die sich innerhalb von etwa 100 Metern Ihres Geräts befinden.

Die schnelle Suche nach Ankern, die durch die ungefähre Standortbestimmung aktiviert wird, vereinfacht die Entwicklung von Anwendungen, die von weltweiten Sammlungen von Ankerpunkten (Millionen geografisch verteilter Ankerpunkte) unterstützt werden. Die Komplexität der Ankerverwaltung ist vollkommen verborgen, was Ihnen ermöglicht, sich voll und ganz auf Ihre Anwendungslogik zu konzentrieren. Der Dienst kümmert sich im Hintergrund um die ganze Arbeit mit den Ankern.

## <a name="collected-sensor-data"></a>Erfasste Sensordaten

Folgende Sensordaten können an den Ankerdienst übermittelt werden:

* GPS-Position: Breitengrad, Längengrad und Höhe.
* Signalstärke von WLAN-Zugangspunkten in Reichweite.
* Signalstärke von Bluetooth-Beacons in Reichweite.

Im Allgemeinen benötigt Ihre Anwendung gerätespezifische Berechtigungen für den Zugriff auf GPS-, WLAN- oder BLE-Daten. Darüber hinaus sind einige Sensordaten auf einigen Plattformen entwurfsbedingt nicht verfügbar. Zur Berücksichtigung dieser Situationen ist die Erfassung von Sensordaten optional und standardmäßig deaktiviert.

## <a name="set-up-the-sensor-data-collection"></a>Einrichten der Erfassung von Sensordaten

Zunächst erstellen Sie einen Anbieter für Fingerabdrucksensoren und machen die Sitzung darauf aufmerksam:

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```csharp
// Create the sensor fingerprint provider
sensorProvider = new PlatformLocationProvider();

// Create and configure the session
cloudSpatialAnchorSession = new CloudSpatialAnchorSession();

// Inform the session it can access sensor data from your provider
cloudSpatialAnchorSession.LocationProvider = sensorProvider;
```

# <a name="objctabobjc"></a>[ObjC](#tab/objc)

```objc
// Create the sensor fingerprint provider
ASAPlatformLocationProvider *sensorProvider;
sensorProvider = [[ASAPlatformLocationProvider alloc] init];

// Create and configure the session
cloudSpatialAnchorSession = [[ASACloudSpatialAnchorSession alloc] init];

// Inform the session it can access sensor data from your provider
cloudSpatialAnchorSession.locationProvider = sensorProvider;
```

# <a name="swifttabswift"></a>[Swift](#tab/swift)

```swift
// Create the sensor fingerprint provider
var sensorProvider: ASAPlatformLocationProvider?
sensorProvider = ASAPlatformLocationProvider()

// Create and configure the session
cloudSpatialAnchorSession = ASACloudSpatialAnchorSession()

// Inform the session it can access sensor data from your provider
cloudSpatialAnchorSession!.locationProvider = sensorProvider
```

# <a name="javatabjava"></a>[Java](#tab/java)

```java
// Create the sensor fingerprint provider
PlatformLocationProvider sensorProvider = new PlatformLocationProvider();

// Create and configure the session
cloudSpatialAnchorSession = new CloudSpatialAnchorSession();

// Inform the session it can access sensor data from your provider
cloudSpatialAnchorSession.setLocationProvider(sensorProvider);
```

# <a name="c-ndktabcpp"></a>[C++ NDK](#tab/cpp)

```cpp
// Create the sensor fingerprint provider
std::shared_ptr<PlatformLocationProvider> sensorProvider;
sensorProvider = std::make_shared<PlatformLocationProvider>();

// Create and configure the session
cloudSpatialAnchorSession = std::make_shared<CloudSpatialAnchorSession>();

// Inform the session it can access sensor data from your provider
cloudSpatialAnchorSession->LocationProvider(sensorProvider);
```

# <a name="c-winrttabcppwinrt"></a>[C++ WinRT](#tab/cppwinrt)
```cpp
// Create the sensor fingerprint provider
PlatformLocationProvider sensorProvider = PlatformLocationProvider();

// Create and configure the session
cloudSpatialAnchorSession = CloudSpatialAnchorSession();

// Inform the session it can access sensor data from your provider
cloudSpatialAnchorSession.LocationProvider(sensorProvider);
```
---

Anschließend müssen Sie entscheiden, welche Sensoren Sie für die ungefähre Standortbestimmung verwenden möchten. Diese Entscheidung ist im Allgemeinen von der Anwendung abhängig, die Sie entwickeln. Die im Folgenden aufgeführten Empfehlungen sollten Ihnen jedoch einen guten Ausgangspunkt vermitteln:


|             | In Gebäuden | Im Freien |
|-------------|---------|----------|
| GPS         | Aus | Andererseits |
| WLAN        | Andererseits | Aktiviert (optional) |
| BLE-Beacons | Aktiviert (optional, mit Einschränkungen, siehe unten) | Aus |


### <a name="enabling-gps"></a>Aktivieren von GPS

Sofern Ihre Anwendung bereits über Berechtigungen für den Zugriff auf die GPS-Position des Geräts verfügt, können Sie Azure Spatial Anchors zur Verwendung dieser konfigurieren:

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```csharp
sensorProvider.Sensors.GeoLocationEnabled = true;
```

# <a name="objctabobjc"></a>[ObjC](#tab/objc)

```objc
ASASensorCapabilities *sensors = locationProvider.sensors;
sensors.geoLocationEnabled = true;
```

# <a name="swifttabswift"></a>[Swift](#tab/swift)

```swift
let sensors = locationProvider?.sensors
sensors.geoLocationEnabled = true
```

# <a name="javatabjava"></a>[Java](#tab/java)

```java
SensorCapabilities sensors = sensorProvider.getSensors();
sensors.setGeoLocationEnabled(true);
```

# <a name="c-ndktabcpp"></a>[C++ NDK](#tab/cpp)

```cpp
const std::shared_ptr<SensorCapabilities>& sensors = sensorProvider->Sensors();
sensors->GeoLocationEnabled(true);
```

# <a name="c-winrttabcppwinrt"></a>[C++ WinRT](#tab/cppwinrt)

```cpp
SensorCapabilities sensors = sensorProvider.Sensors()
sensors.GeoLocationEnabled(true);
```

---

Wenn Sie GPS in Ihrer Anwendung verwenden, sollten Sie beachten, dass die von der Hardware bereitgestellten Messwerte üblicherweise:

* asynchron sind und eine niedrige Frequenz aufweisen (weniger als 1 Hz).
* unzuverlässig bzw. ungenau sind (durschnittliche Standardabweichung von 7 m).

Sowohl das Betriebssystem des Geräts als auch Azure Spatial Anchors führen Filterung und Extrapolierung des unformatierten GPS-Signals durch, um diese Probleme zu umgehen. Diese zusätzliche Verarbeitung erfordert Auflösungszeit. Zum Erzielen der besten Ergebnisse sollten Sie Folgendes versuchen:

* Erstellen Sie den Anbieter für den Fingerabdrucksensor in Ihrer Anwendung so früh wie möglich.
* Sorgen Sie dafür, dass der Anbieter für den Fingerabdrucksensor zwischen mehreren Sitzungen beibehalten wird.

Wenn Sie planen, den Anbieter für den Fingerabdrucksensor außerhalb einer Ankersitzung zu verwenden, sollten Sie sicherstellen, dass Sie ihn starten, bevor Sie Sensorschätzungen anfordern. Beispielsweise kümmert sich der folgende Code um Echtzeitupdates der Position Ihres Geräts auf der Karte:

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```csharp
// Game about to start, start tracking the sensors
sensorProvider.Start();

// Game loop
while (m_isRunning)
{
    // Get the GPS estimate
    GeoLocation geoPose = sensorProvider.GetLocationEstimate();

    // Paint it on the map
    drawCircle(
        x: geoPose.Longitude,
        y: geoPose.Latitude,
        radius: geoPose.HorizontalError);
}

// Game ended, no need to track the sensors anymore
sensorProvider.Stop();
```

# <a name="objctabobjc"></a>[ObjC](#tab/objc)

```objc
// Game about to start, start tracking the sensors
[sensorProvider start];

// Game loop
while (m_isRunning)
{
    // Get the GPS estimate
    ASAGeoLocation *geoPose = [sensorProvider getLocationEstimate];

    // Paint it on the map
    drawCircle(geoPose.longitude, geoPose.latitude, geoPose.horizontalError);
}

// Game ended, no need to track the sensors anymore
[sensorProvider stop];
```

# <a name="swifttabswift"></a>[Swift](#tab/swift)

```swift
// Game about to start, start tracking the sensors
sensorProvider?.start()

// Game loop
while m_isRunning
{
    // Get the GPS estimate
    var geoPose: ASAGeoLocation?
    geoPose = sensorProvider?.getLocationEstimate()

    // Paint it on the map
    drawCircle(geoPose.longitude, geoPose.latitude, geoPose.horizontalError)
}

// Game ended, no need to track the sensors anymore
sensorProvider?.stop()
```

# <a name="javatabjava"></a>[Java](#tab/java)

```java
// Game about to start, start tracking the sensors
sensorProvider.start();

// Game loop
while (m_isRunning)
{
    // Get the GPS estimate
    GeoLocation geoPose = sensorProvider.getLocationEstimate();

    // Paint it on the map
    drawCircle(geoPose.getLongitude(), geoPose.getLatitude(), geoPose.getHorizontalError());
}

// Game ended, no need to track the sensors anymore
sensorProvider.stop();
```

# <a name="c-ndktabcpp"></a>[C++ NDK](#tab/cpp)

```cpp
// Game about to start, start tracking the sensors
sensorProvider->Start();

// Game loop
while (m_isRunning)
{
    // Get the GPS estimate
    std::shared_ptr<GeoLocation> geoPose = sensorProvider->GetLocationEstimate();

    // Paint it on the map
    drawCircle(geoPose->Longitude(), geoPose->Latitude(), geoPose->HorizontalError());
}

// Game ended, no need to track the sensors anymore
sensorProvider->Stop();
```

# <a name="c-winrttabcppwinrt"></a>[C++ WinRT](#tab/cppwinrt)

```cpp
// Game about to start, start tracking the sensors
sensorProvider.Start();

// Game loop
while (m_isRunning)
{
    // Get the GPS estimate
    GeoLocation geoPose = sensorProvider.GetLocationEstimate();

    // Paint it on the map
    drawCircle(geoPose.Longitude(), geoPose.Latitude(), geoPose.HorizontalError());
}

// Game ended, no need to track the sensors anymore
sensorProvider.Stop();
```

---

### <a name="enabling-wifi"></a>Aktivieren von WLAN

Sofern Ihre Anwendung bereits über Berechtigungen für den Zugriff auf den WLAN-Status des Geräts verfügt, können Sie Azure Spatial Anchors zur Verwendung dieser konfigurieren:

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```csharp
sensorProvider.Sensors.WifiEnabled = true;
```

# <a name="objctabobjc"></a>[ObjC](#tab/objc)

```objc
ASASensorCapabilities *sensors = locationProvider.sensors;
sensors.wifiEnabled = true;
```

# <a name="swifttabswift"></a>[Swift](#tab/swift)

```swift
let sensors = locationProvider?.sensors
sensors.wifiEnabled = true
```

# <a name="javatabjava"></a>[Java](#tab/java)

```java
SensorCapabilities sensors = sensorProvider.getSensors();
sensors.setWifiEnabled(true);
```

# <a name="c-ndktabcpp"></a>[C++ NDK](#tab/cpp)

```cpp
const std::shared_ptr<SensorCapabilities>& sensors = sensorProvider->Sensors();
sensors->WifiEnabled(true);
```

# <a name="c-winrttabcppwinrt"></a>[C++ WinRT](#tab/cppwinrt)

```cpp
SensorCapabilities sensors = sensorProvider.Sensors()
sensors.WifiEnabled(true);
```

---

Wenn Sie WLAN in Ihrer Anwendung verwenden, sollten Sie beachten, dass die von der Hardware bereitgestellten Messwerte üblicherweise:

* asynchron sind und eine niedrige Frequenz aufweisen (weniger als 0,1 Hz).
* möglicherweise auf Betriebssystemebene gedrosselt wird.
* unzuverlässig bzw. ungenau sind (durschnittliche Standardabweichung von 3 dBm).

Azure Spatial Anchors versucht während einer Sitzung, eine gefilterte Karte der WLAN-Signalstärke zu erstellen, um diese Probleme zu umgehen. Für optimale Ergebnisse sollten Sie Folgendes versuchen:

* Erstellen Sie die Sitzung, bevor Sie den ersten Anker platzieren.
* Behalten Sie die Sitzung so lange wie möglich bei (d. h., erstellen Sie alle Anker und Abfragen innerhalb einer Sitzung).

### <a name="enabling-bluetooth-beacons"></a>Aktivieren von Bluetooth-Beacons

Sofern Ihre Anwendung bereits über Berechtigungen für den Zugriff auf den Bluetooth-Status des Geräts verfügt, können Sie Azure Spatial Anchors zur Verwendung dieser konfigurieren:

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```csharp
sensorProvider.Sensors.BluetoothEnabled = true;
```

# <a name="objctabobjc"></a>[ObjC](#tab/objc)

```objc
ASASensorCapabilities *sensors = locationProvider.sensors;
sensors.bluetoothEnabled = true;
```

# <a name="swifttabswift"></a>[Swift](#tab/swift)

```swift
let sensors = locationProvider?.sensors
sensors.bluetoothEnabled = true
```

# <a name="javatabjava"></a>[Java](#tab/java)

```java
SensorCapabilities sensors = sensorProvider.getSensors();
sensors.setBluetoothEnabled(true);
```

# <a name="c-ndktabcpp"></a>[C++ NDK](#tab/cpp)

```cpp
const std::shared_ptr<SensorCapabilities>& sensors = sensorProvider->Sensors();
sensors->BluetoothEnabled(true);
```

# <a name="c-winrttabcppwinrt"></a>[C++ WinRT](#tab/cppwinrt)

```cpp
SensorCapabilities sensors = sensorProvider.Sensors();
sensors.BluetoothEnabled(true);
```

---

Beacons sind meist verschiedene Geräte, bei denen alles, einschließlich UUIDs und MAC-Adressen, konfiguriert werden kann. Diese Flexibilität kann sich für Azure Spatial Anchors als problematisch erweisen, da Beacons anhand ihrer UUIDs eindeutig identifiziert werden sollen. Wenn diese Eindeutigkeit nicht sichergestellt wird, kommt es wahrscheinlich zu Funklöchern. Für optimale Ergebnisse sollten Sie:

* Ihren Beacons eindeutige UUIDs zuweisen.
* Ihre Beacons in einem gleichmäßigen Muster installieren, beispielsweise in einem Raste.
* die Liste der eindeutigen Beacon-UUIDs an den Anbieter für den Fingerabdrucksensor übergeben.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```csharp
sensorProvider.Sensors.KnownBeaconProximityUuids = new[]
{
    "22e38f1a-c1b3-452b-b5ce-fdb0f39535c1",
    "a63819b9-8b7b-436d-88ec-ea5d8db2acb0",
    . . .
};
```

# <a name="objctabobjc"></a>[ObjC](#tab/objc)

```objc
NSArray *uuids = @[@"22e38f1a-c1b3-452b-b5ce-fdb0f39535c1", @"a63819b9-8b7b-436d-88ec-ea5d8db2acb0"];

ASASensorCapabilities *sensors = locationProvider.sensors;
sensors.knownBeaconProximityUuids = uuids;
```

# <a name="swifttabswift"></a>[Swift](#tab/swift)

```swift
let uuids = [String]()
uuids.append("22e38f1a-c1b3-452b-b5ce-fdb0f39535c1")
uuids.append("a63819b9-8b7b-436d-88ec-ea5d8db2acb0")

let sensors = locationProvider?.sensors
sensors.knownBeaconProximityUuids = uuids
```

# <a name="javatabjava"></a>[Java](#tab/java)

```java
String uuids[] = new String[2];
uuids[0] = "22e38f1a-c1b3-452b-b5ce-fdb0f39535c1";
uuids[1] = "a63819b9-8b7b-436d-88ec-ea5d8db2acb0";

SensorCapabilities sensors = sensorProvider.getSensors();
sensors.setKnownBeaconProximityUuids(uuids);
```

# <a name="c-ndktabcpp"></a>[C++ NDK](#tab/cpp)

```cpp
std::vector<std::string> uuids;
uuids.push_back("22e38f1a-c1b3-452b-b5ce-fdb0f39535c1");
uuids.push_back("a63819b9-8b7b-436d-88ec-ea5d8db2acb0");

const std::shared_ptr<SensorCapabilities>& sensors = sensorProvider->Sensors();
sensors->KnownBeaconProximityUuids(uuids);
```

# <a name="c-winrttabcppwinrt"></a>[C++ WinRT](#tab/cppwinrt)

```cpp
std::vector<winrt::hstring> uuids;
uuids.emplace_back("22e38f1a-c1b3-452b-b5ce-fdb0f39535c1");
uuids.emplace_back("a63819b9-8b7b-436d-88ec-ea5d8db2acb0");

SensorCapabilities sensors = sensorProvider.Sensors();
sensors.KnownBeaconProximityUuids(uuids);
```

---

Azure Spatial Anchors verfolgt nur Bluetooth-Beacons, die in der Liste aufgeführt werden. Schädliche Beacons, die mit UUIDs in der Zulassungsliste programmiert wurden, können sich weiterhin negativ auf die Qualität des Diensts auswirken. Daher sollten Sie Beacons nur in sicheren Bereichen verwenden, bei denen Sie ihre Installation kontrollieren können.

## <a name="querying-with-sensor-data"></a>Abfragen mit Sensordaten

Sobald Sie Anker mit zugeordneten Sensordaten erstellt haben, können Sie sie anhand der Sensormessungen abrufen, die das Gerät meldet. Es ist nicht mehr erforderlich, dem Dienst eine Liste bekannter Anker bereitzustellen, die Sie erwarten. Stattdessen teilen Sie dem Dienst den Standort Ihres Geräts mit, der von den integrierten Sensoren gemeldet wird. Der Spatial Anchors-Dienst ermittelt dann die Anker in der Nähe Ihres Geräts und versucht, diese visuell abzugleichen.

Erstellen Sie zunächst ein Suchkriterium, damit Abfragen die Sensordaten verwenden:

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```csharp
NearDeviceCriteria nearDeviceCriteria = new NearDeviceCriteria();

// Choose a maximum exploration distance between your device and the returned anchors
nearDeviceCriteria.DistanceInMeters = 5;

// Cap the number of anchors returned
nearDeviceCriteria.MaxResultCount = 25;

anchorLocateCriteria = new AnchorLocateCriteria();
anchorLocateCriteria.NearDevice = nearDeviceCriteria;
```

# <a name="objctabobjc"></a>[ObjC](#tab/objc)

```objc
ASANearDeviceCriteria *nearDeviceCriteria = [[ASANearDeviceCriteria alloc] init];

// Choose a maximum exploration distance between your device and the returned anchors
nearDeviceCriteria.distanceInMeters = 5.0f;

// Cap the number of anchors returned
nearDeviceCriteria.maxResultCount = 25;

ASAAnchorLocateCriteria *anchorLocateCriteria = [[ASAAnchorLocateCriteria alloc] init];
anchorLocateCriteria.nearDevice = nearDeviceCriteria;
```

# <a name="swifttabswift"></a>[Swift](#tab/swift)

```swift
let nearDeviceCriteria = ASANearDeviceCriteria()!

// Choose a maximum exploration distance between your device and the returned anchors
nearDeviceCriteria.distanceInMeters = 5.0

// Cap the number of anchors returned
nearDeviceCriteria.maxResultCount = 25

let anchorLocateCriteria = ASAAnchorLocateCriteria()!
anchorLocateCriteria.nearDevice = nearDeviceCriteria
```

# <a name="javatabjava"></a>[Java](#tab/java)

```java
NearDeviceCriteria nearDeviceCriteria = new NearDeviceCriteria();

// Choose a maximum exploration distance between your device and the returned anchors
nearDeviceCriteria.setDistanceInMeters(5.0f);

// Cap the number of anchors returned
nearDeviceCriteria.setMaxResultCount(25);

AnchorLocateCriteria anchorLocateCriteria = new AnchorLocateCriteria();
anchorLocateCriteria.setNearDevice(nearDeviceCriteria);
```

# <a name="c-ndktabcpp"></a>[C++ NDK](#tab/cpp)

```cpp
auto nearDeviceCriteria = std::make_shared<NearDeviceCriteria>();

// Choose a maximum exploration distance between your device and the returned anchors
nearDeviceCriteria->DistanceInMeters(5.0f);

// Cap the number of anchors returned
nearDeviceCriteria->MaxResultCount(25);

auto anchorLocateCriteria = std::make_shared<AnchorLocateCriteria>();
anchorLocateCriteria->NearDevice(nearDeviceCriteria);
```

# <a name="c-winrttabcppwinrt"></a>[C++ WinRT](#tab/cppwinrt)

```cpp
NearDeviceCriteria nearDeviceCriteria = NearDeviceCriteria();

// Choose a maximum exploration distance between your device and the returned anchors
nearDeviceCriteria.DistanceInMeters(5.0f);

// Cap the number of anchors returned
nearDeviceCriteria.MaxResultCount(25);

// Set the session's locate criteria
anchorLocateCriteria = AnchorLocateCriteria();
anchorLocateCriteria.NearDevice(nearDeviceCriteria);
```

---

Der `DistanceInMeters`-Parameter steuert, wie weit der Ankergraph zum Abrufen von Inhalten untersucht wird. Angenommen, Sie haben für Ihre Instanz einen Bereich mit Ankern mit einem regelmäßigen Abstand von 2 Metern ausgestattet. Außerdem befindet ein einzelner Anker im Sichtfeld der Kamera des Geräts, den das Gerät erfolgreich gefunden hat. Sie sind wahrscheinlich daran interessiert, anstelle des einzelnen Ankers, den Sie gerade beobachten, alle in der Nähe platzierten Anker abzurufen. Angenommen, die platzierten Anker sind in einem Graph verbunden, dann kann der Dienst alle Anker in der Nähe für Sie abrufen, indem er den Rändern des Graphs folgt. Die durchlaufene Graph-Strecke wird von `DistanceInMeters` vorgegeben. Es werden Ihnen alle Anker zurückgegeben, die mit dem gefundenen verbunden sind und sich näher als der Wert von `DistanceInMeters` befinden.

Denken Sie daran, dass sich hohe Werte bei `MaxResultCount` negativ auf die Leistung auswirken können. Versuchen Sie, einen sinnvollen Wert für Ihre Anwendung festzulegen.

Zuletzt müssen Sie die Sitzung anweisen, die sensorbasierte Suche zu verwenden:

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```csharp
cloudSpatialAnchorSession.CreateWatcher(anchorLocateCriteria);
```

# <a name="objctabobjc"></a>[ObjC](#tab/objc)

```objc
[cloudSpatialAnchorSession createWatcher:anchorLocateCriteria];
```

# <a name="swifttabswift"></a>[Swift](#tab/swift)

```swift
cloudSpatialAnchorSession!.createWatcher(anchorLocateCriteria)
```

# <a name="javatabjava"></a>[Java](#tab/java)

```java
cloudSpatialAnchorSession.createWatcher(anchorLocateCriteria);
```

# <a name="c-ndktabcpp"></a>[C++ NDK](#tab/cpp)

```cpp
cloudSpatialAnchorSession->CreateWatcher(anchorLocateCriteria);
```

# <a name="c-winrttabcppwinrt"></a>[C++ WinRT](#tab/cppwinrt)

```cpp
cloudSpatialAnchorSession.CreateWatcher(anchorLocateCriteria);
```

---

## <a name="expected-results"></a>Erwartete Ergebnisse

GPS-Geräte für Verbraucher sind in der Regel recht ungenau. In einer Studie von [Zandenbergen und Barbeau (2011)][6] wurde eine durchschnittliche Genauigkeit von etwa 7 Metern bei Mobiltelefonen mit GPS (A-GPS) berichtet. Dieser Wert ist zu groß, als dass man ihn ignorieren könnte. Zur Berücksichtigung dieser Messfehler behandelt der Dienst die Anker als Wahrscheinlichkeitsverteilungen im GPS-Bereich. Daher bezeichnet ein Anker den Bereich eines Standorts, der höchstwahrscheinlich die tatsächliche, unbekannte GPS-Position enthält (d. h. mit einer Konfidenz von mehr als 95 %).

Dieselbe Argumentation wird beim Abfragen mit GPS verwendet. Das Gerät wird als weiterer räumlicher Konfidenzbereich um die tatsächliche, unbekannte GPS-Position dargestellt. Die Ermittlung von Ankerpunkten in der Nähe bedeutet also, dass Ankerpunkte mit Konfidenzbereichen gefunden werden, die sich wie in der folgenden Abbildung gezeigt *nahe genug* am Konfidenzbereich des Geräts befinden:

![Auswahl von Ankerkandidaten mit GPS](media/coarse-reloc-gps-separation-distance.png)

Die Genauigkeit des GPS-Signals hat sowohl bei der Erstellung von Ankern als auch bei Abfragen eine große Auswirkung auf die zurückgegebenen Anker. Im Gegensatz dazu berücksichtigen Abfragen, die auf WLAN oder Beacons basieren, alle Anker mit mindestens einem Zugangspunkt oder Beacon, der mit der Abfrage übereinstimmt. In diesem Sinne wird das Ergebnis einer Abfrage, die auf WLAN oder Beacons basiert, größtenteils von der physischen Reichweite der Zugangspunkte oder Beacons und Hindernissen in der Umgebung beeinflusst.

In der folgenden Tabelle wird der erwartete Suchbereich für jeden Sensortyp geschätzt:

| Sensor      | Suchbereichsradios (ungefähr) | Details |
|-------------|:-------:|---------|
| GPS         | 20 bis 30 m | Wird unabhängig von anderen Faktoren durch GPS bestimmt. Die gemeldeten Werte werden für die durchschnittliche GPS-Genauigkeit von Mobiltelefonen mit A-GPS geschätzt (7 Meter). |
| WLAN        | 50 bis 100 m | Wird von der Reichweite der WLAN-Zugangspunkte bestimmt. Wird von der Frequenz, Signalstärke, physischen Hindernissen, Interferenzen usw. beeinträchtigt. |
| BLE-Beacons |  70 m | Wird von der Reichweite des Beacons bestimmt. Wird von der Frequenz, Signalstärke, physischen Hindernissen, Interferenzen usw. beeinträchtigt. |

## <a name="per-platform-support"></a>Plattformspezifische Unterstützung

In der folgenden Tabelle werden die Sensordaten, die von den einzelnen unterstützten Plattformen unterstützt werden, sowie alle plattformspezifischen Einschränkungen zusammengefasst:


|             | HoloLens | Android | iOS |
|-------------|----------|---------|-----|
| GPS         | – | Wird von [LocationManager][3]-APIs unterstützt (sowohl GPS als auch NETWORK). | Wird von [CLLocationManager][4]-APIs unterstützt. |
| WLAN        | Wird mit einer Rate von etwa einer Überprüfung alle drei Sekunden unterstützt. | Unterstützt. Ab API-Ebene 28, werden WLAN-Überprüfungen auf vier Aufrufe alle 2 Minuten gedrosselt. Ab Android 10 kann die Drosselung über das Menü „Entwicklereinstellungen“ deaktiviert werden. Weitere Informationen finden Sie in der [Android-Dokumentation][5]. | Nicht vorhanden – keine öffentliche API |
| BLE-Beacons | Beschränkt auf [Eddystone][1] und [iBeacon][2] | Beschränkt auf [Eddystone][1] und [iBeacon][2] | Beschränkt auf [Eddystone][1] und [iBeacon][2] |

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie die ungefähre Standortbestimmung in einer App.

> [!div class="nextstepaction"]
> [Unity](../how-tos/set-up-coarse-reloc-unity.md)

> [!div class="nextstepaction"]
> [Objective-C](../how-tos/set-up-coarse-reloc-objc.md)

> [!div class="nextstepaction"]
> [Swift](../how-tos/set-up-coarse-reloc-swift.md)

> [!div class="nextstepaction"]
> [Java](../how-tos/set-up-coarse-reloc-java.md)

> [!div class="nextstepaction"]
> [C++/NDK](../how-tos/set-up-coarse-reloc-cpp-ndk.md)

> [!div class="nextstepaction"]
> [C++/WinRT](../how-tos/set-up-coarse-reloc-cpp-winrt.md)

<!-- Reference links in article -->
[1]: https://developers.google.com/beacons/eddystone
[2]: https://developer.apple.com/ibeacon/
[3]: https://developer.android.com/reference/android/location/LocationManager
[4]: https://developer.apple.com/documentation/corelocation/cllocationmanager?language=objc
[5]: https://developer.android.com/guide/topics/connectivity/wifi-scan
[6]: https://www.cambridge.org/core/journals/journal-of-navigation/article/positional-accuracy-of-assisted-gps-data-from-highsensitivity-gpsenabled-mobile-phones/E1EE20CD1A301C537BEE8EC66766B0A9
