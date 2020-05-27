---
title: Anforderungen für Zeichnungspakete in Azure Maps Creator
description: Hier erfahren Sie, welche Anforderungen für Zeichnungspakete erfüllt sein müssen, um Ihre Plandateien für Einrichtungen mithilfe des Azure Maps-Konvertierungsdiensts in Kartendaten konvertieren zu können.
author: anastasia-ms
ms.author: v-stharr
ms.date: 5/18/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philMea
ms.openlocfilehash: c0c81f529dfc959916ff7c102b2b903a808b9672
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83681909"
---
# <a name="drawing-package-requirements"></a>Anforderungen für Zeichnungspakete

Mit dem [Azure Maps-Konvertierungsdienst](https://docs.microsoft.com/rest/api/maps/conversion) können Sie hochgeladene Zeichnungspakete in Kartendaten konvertieren. In diesem Artikel werden die Anforderungen für Zeichnungspakete für die Konvertierungs-API beschrieben. Wenn Sie sich ein Beispielpaket ansehen möchten, können Sie das exemplarische [Zeichnungspaket](https://github.com/Azure-Samples/am-creator-indoor-data-examples) herunterladen.

## <a name="prerequisites"></a>Voraussetzungen

Das Zeichnungspaket enthält gespeicherte Zeichnungen im DWG-Format. Hierbei handelt es sich um das native Dateiformat der AutoCAD®-Software von Autodesk, einer [Marke von Autodesk, Inc](https://www.autodesk.com/company/legal-notices-trademarks/trademarks/guidelines-for-use#section12).

Die Zeichnungen im Zeichnungspaket können mithilfe einer beliebigen CAD-Software erstellt werden.  

Das Zeichnungspaket wird vom [Azure Maps-Konvertierungsdienst](https://docs.microsoft.com/rest/api/maps/conversion) in Kartendaten konvertiert.  Der Konvertierungsdienst wurde mit dem DWG-Dateiformat von AutoCAD entwickelt und getestet. `AC1032` ist die interne Formatversion für die DWG-Dateien. Es wird empfohlen, `AC1032` für die interne DWG-Dateiformatversion auszuwählen.  

Im Anschluss finden Sie ein Glossar mit den in diesem Dokument verwendeten Begriffen:

| Begriff  | Definition |
|:-------|:------------|
| Schicht | Eine AutoCAD-DWG-Schicht.|
| Ebene | Ein Gebäudebereich in einer bestimmten Höhe. Hierbei kann es sich beispielsweise um ein Stockwerk handeln. |
| Querverweis  |Eine Datei im DWG-Dateiformat (*.dwg) von AutoCAD, die als externe Referenz an die Hauptzeichnung angefügt wurde.  |
| Feature | Ein Objekt, das eine Geometrie mit zusätzlichen Metadateninformationen kombiniert. |
| Featureklassen | Eine allgemeine Blaupause für Features. So ist beispielsweise eine Einheit eine Featureklasse, während es sich bei einem Büro um ein Feature handelt. |

## <a name="drawing-package-structure"></a>Struktur eines Zeichnungspakets

Bei einem Zeichnungspaket handelt es sich um ein ZIP-Archiv mit folgenden Dateien:

* DWG-Dateien im DWG-Dateiformat von AutoCAD
* Eine Datei vom Typ _manifest.json_ für eine einzelne Einrichtung

Die DWG-Dateien können innerhalb des Ordners beliebig strukturiert sein. Die Manifestdatei muss sich jedoch im Stammverzeichnis des Ordners befinden. Der Ordner muss in einer einzelnen Archivdatei mit der Erweiterung „.zip“ gezippt sein. In den nächsten Abschnitten werden die Anforderungen für die DWG-Dateien, die Manifestdatei und den Inhalt dieser Dateien erläutert.  

## <a name="dwg-files-requirements"></a>Anforderungen für DWG-Dateien

Für jede Ebene der Einrichtung ist jeweils eine einzelne DWG-Datei erforderlich. Die Daten der Ebene müssen in einer einzelnen DWG-Datei enthalten sein. Externe Referenzen (_Querverweise_) müssen jeweils an die übergeordnete Zeichnung gebunden werden. Darüber hinaus gilt für jede DWG-Datei Folgendes:

* Sie muss die Schichten _Exterior_ (Außenbereich) und _Unit_ (Einheit) definieren. Folgende Schichten können optional definiert werden: _Wall_ (Wand), _Door_ (Tür), _UnitLabel_ (Einheitenbezeichnung), _Zone_ (Zone) und _ZoneLabel_ (Zonenbezeichnung).
* Sie darf keine Features mehrerer Ebenen enthalten.
* Sie darf keine Features mehrerer Einrichtungen enthalten.

Vom [Azure Maps-Konvertierungsdienst](https://docs.microsoft.com/rest/api/maps/conversion) können folgende Featureklassen aus einer DWG-Datei extrahiert werden:

* „Levels“ (Ebenen)
* „Units“ (Einheiten)
* „Zones“ (Zonen)
* „Openings“ (Öffnungen)
* „Walls“ (Wände)
* „Vertical Penetrations“ (Vertikale Durchdringungen)

Alle Konvertierungsaufträge resultieren in einem Mindestsatz von Standardkategorien: „room“, „structure.wall“, „opening.door“, „zone“ und „facility“. Weitere Kategorien beziehen sich auf den jeweiligen Kategorienamen, auf den durch Objekte verwiesen wird.  

Eine DWG-Schicht muss Features einer einzelnen Klasse enthalten. Klassen dürfen sich keine Schicht teilen. Von „Units“ und „Walls“ kann also beispielsweise nicht die gleiche Schicht verwendet werden.

DWG-Schichten müssen außerdem folgende Kriterien erfüllen:

* Der Ursprung von Zeichnungen muss für alle DWG-Dateien am gleichen Breiten- und Längengrad ausgerichtet sein.
* Alle Ebenen müssen über die gleiche Ausrichtung verfügen.
* Polygone, die sich selbst schneiden, werden automatisch repariert, und vom [Azure Maps-Konvertierungsdienst](https://docs.microsoft.com/rest/api/maps/conversion) wird eine entsprechende Warnung ausgelöst. Es empfiehlt sich, das Ergebnis der Reparatur manuell zu überprüfen, da es möglicherweise nicht den Erwartungen entspricht.

Bei allen Schichtentitäten muss es sich um einen der folgenden Typen handeln: Linie, Polylinie, Polygon, Kreisbogen, Kreis oder Text (einzeilig). Alle anderen Entitätstypen werden ignoriert.

In der folgenden Tabelle werden die unterstützten Entitätstypen und die unterstützten Features für die einzelnen Schichten beschrieben. Sollte eine Schicht nicht unterstützte Entitätstypen enthalten, werden diese Entitäten vom [Azure Maps-Konvertierungsdienst](https://docs.microsoft.com/rest/api/maps/conversion) ignoriert.  

| Schicht | Entitätstypen | Features |
| :----- | :-------------------| :-------
| [Exterior](#exterior-layer) | Polygon, Polylinie (geschlossen), Kreis | Levels
| [Unit](#unit-layer) |  Polygon, Polylinie (geschlossen), Kreis | Vertical Penetrations, Units
| [Wall](#wall-layer)  | Polygon, Polylinie (geschlossen), Kreis | Nicht zutreffend. Weitere Informationen finden Sie unter [Wall-Schicht](#wall-layer).
| [Door](#door-layer) | Polygon, Polylinie, Linie, Kreisbogen, Kreis | Openings
| [Zone](#zone-layer) | Polygon, Polylinie (geschlossen), Kreis | Zone
| [UnitLabel](#unitlabel-layer) | Text (einzeilig) | Nicht zutreffend. Von dieser Schicht können nur Eigenschaften zu den Einheitenfeatures der Unit-Schicht hinzugefügt werden. Weitere Informationen finden Sie unter [UnitLabel-Schicht](#unitlabel-layer).
| [ZoneLabel](#zonelabel-layer) | Text (einzeilig) | Nicht zutreffend. Von dieser Schicht können nur Eigenschaften zu den Zonenfeatures der Zone-Schicht hinzugefügt werden. Weitere Informationen finden Sie unter [ZoneLabel-Schicht](#zonelabel-layer).

In den nächsten Abschnitten werden die Anforderungen für die einzelnen Schichten erläutert.

### <a name="exterior-layer"></a>Exterior-Schicht

Die DWG-Datei für die jeweilige Ebene muss eine Schicht zum Definieren der Grenze der betreffenden Ebene enthalten. Diese Schicht wird als Exterior-Schicht bezeichnet. Wenn eine Einrichtung also beispielsweise zwei Ebenen enthält, benötigen Sie zwei DWG-Dateien mit einer Exterior-Schicht für jede Datei.

Das [resultierende Einrichtungsdataset](tutorial-creator-indoor-maps.md#create-a-feature-stateset) enthält jeweils nur **ein** Ebenenfeature pro DWG-Datei – unabhängig davon, wie viele Entitätszeichnungen sich in der Exterior-Schicht befinden. Außerdem zu beachten:

* Außenbereiche müssen als Polygon, Polylinie (geschlossen) oder Kreis gezeichnet werden.

* Außenbereiche dürfen sich zwar überschneiden, werden jedoch zu einer einzelnen Geometrie aufgelöst.

Enthält die Schicht mehrere sich überschneidende Polylinien, werden diese zu einem einzelnen Ebenenfeature aufgelöst. Wenn die Schicht mehrere Polylinien ohne Überschneidung enthält, verfügt das resultierende Ebenenfeature alternativ über eine Multipolygondarstellung.

Ein Beispiel für die Exterior-Schicht ist die OUTLINE-Schicht im [Beispielzeichenpaket](https://github.com/Azure-Samples/am-creator-indoor-data-examples).

### <a name="unit-layer"></a>Unit-Schicht

In der DWG-Datei für die jeweilige Ebene muss eine Schicht mit Einheiten definiert werden.  Bei Einheiten handelt es sich um betretbare Gebäudebereiche wie Büros, Flure, Treppenhäuser und Aufzüge. Die Unit-Schicht muss folgende Anforderungen erfüllen:

* Einheiten müssen als Polygon, Polylinie (geschlossen) oder Kreis gezeichnet werden.
* Einheiten müssen innerhalb der Außengrenzen der Einrichtung liegen.
* Einheiten dürfen sich nicht teilweise überschneiden.
* Einheiten dürfen keine Geometrie enthalten, die sich selbst schneidet.

 Erstellen Sie zum Benennen einer Einheit ein Textobjekt in der Schicht _unitLabel_, und platzieren Sie es innerhalb der Grenzen der Einheit. Weitere Informationen finden Sie unter [UnitLabel-Schicht](#unitlabel-layer).

Ein Beispiel für die Unit-Schicht ist die UNITS-Schicht im [Beispielzeichenpaket](https://github.com/Azure-Samples/am-creator-indoor-data-examples).

### <a name="wall-layer"></a>Wall-Schicht

Die DWG-Datei für die jeweilige Ebene kann eine Schicht enthalten, die die physischen Abmessungen von Wänden, Säulen und anderen Baustrukturen definiert.

* Wände müssen als Polygon, Polylinie (geschlossen) oder Kreis gezeichnet werden.
* Eine Wall-Schicht darf nur als Baustruktur interpretierte Geometrie enthalten.

Ein Beispiel für die Wall-Schicht ist die WALLS-Schicht im [Beispielzeichenpaket](https://github.com/Azure-Samples/am-creator-indoor-data-examples).

### <a name="door-layer"></a>Door-Schicht

Sie können eine DWG-Schicht für Türen einschließen. Jede Tür muss sich mit der Kante einer Einheit aus der Unit-Schicht überschneiden.

Türöffnungen werden in einem Azure Maps-Dataset als Segment mit einer einzelnen Linie dargestellt, das sich mit mehreren Einheitengrenzen überschneidet. Geometrie in der Door-Schicht wird mithilfe folgender Schritte in Öffnungsfeatures in einem Dataset konvertiert:

![Schritte zum Generieren von Öffnungen](./media/drawing-requirements/opening-steps.png)

### <a name="zone-layer"></a>Zone-Schicht

Die DWG-Datei für die jeweilige Ebene kann eine Zone-Schicht enthalten, die die physischen Abmessungen von Zonen definiert. Eine Zone kann ein leerer Innenraum oder ein Hinterhof sein.

* Zonen müssen als Polygon, Polylinie (geschlossen) oder Kreis gezeichnet werden.
* Zonen können sich überschneiden.
* Zonen können innerhalb oder außerhalb der Außengrenze der Einrichtung liegen.

Erstellen Sie zum Benennen einer Zone ein Textobjekt in der Schicht _zoneLabel_, und platzieren Sie es innerhalb der Grenzen der Zone. Weitere Informationen finden Sie unter [ZoneLabel-Schicht](#zonelabel-layer).

Ein Beispiel für die Zone-Schicht ist die ZONES-Schicht im [Beispielzeichenpaket](https://github.com/Azure-Samples/am-creator-indoor-data-examples).

### <a name="unitlabel-layer"></a>UnitLabel-Schicht

Die DWG-Datei für die jeweilige Ebene kann eine Schicht für Einheitenbezeichnungen enthalten. Durch die Schicht für Einheitenbezeichnungen wird aus der Unit-Schicht extrahierten Einheiten eine Namenseigenschaft hinzugefügt. Für Einheiten mit einer Namenseigenschaft können zusätzliche Details in der Manifestdatei angegeben werden.

* Bei Einheitenbezeichnungen muss es sich um einzeilige Textentitäten handeln.
* Einheitenbezeichnungen müssen innerhalb der Grenzen der zugehörigen Einheit liegen.
* Einheiten dürfen nicht mehrere Textentitäten in der Schicht für Einheitenbezeichnungen enthalten.

Ein Beispiel für die UnitLabel-Schicht ist die UNITLABELS-Schicht im [Beispielzeichenpaket](https://github.com/Azure-Samples/am-creator-indoor-data-examples).

### <a name="zonelabel-layer"></a>ZoneLabel-Schicht

Die DWG-Datei für die jeweilige Ebene kann eine Schicht für Zonenbezeichnungen enthalten. Durch diese Schicht wird aus der Zone-Schicht extrahierten Zonen eine Namenseigenschaft hinzugefügt. Für Zonen mit einer Namenseigenschaft können zusätzliche Details in der Manifestdatei angegeben werden.

* Bei Zonenbezeichnungen muss es sich um einzeilige Textentitäten handeln.
* Zonenbezeichnungen müssen innerhalb der Grenzen der zugehörigen Zone liegen.
* Zonen dürfen nicht mehrere Textentitäten in der Schicht für Zonenbezeichnungen enthalten.

Ein Beispiel für die ZoneLabel-Schicht ist die ZONELABELS-Schicht im [Beispielzeichenpaket](https://github.com/Azure-Samples/am-creator-indoor-data-examples).

## <a name="manifest-file-requirements"></a>Anforderungen für die Manifestdatei

Der ZIP-Ordner muss eine Manifestdatei auf der Stammebene des Verzeichnisses enthalten, und die Datei muss **manifest.json** heißen. In dieser Datei werden die DWG-Dateien beschrieben, damit deren Inhalt vom [Azure Maps-Konvertierungsdienst](https://docs.microsoft.com/rest/api/maps/conversion) analysiert werden kann. Bei der Erfassung werden nur die im Manifest angegebenen Dateien berücksichtigt. Dateien, die sich zwar im ZIP-Ordner befinden, aber nicht ordnungsgemäß im Manifest aufgeführt sind, werden ignoriert.

Die Dateipfade im Objekt **buildingLevels** der Manifestdatei müssen relativ zum Stamm des ZIP-Ordners angegeben sein. Der DWG-Dateiname muss exakt dem Namen der Einrichtungsebene entsprechen. Eine DWG-Datei für die Ebene „Keller“ muss also beispielsweise „Keller.dwg“ heißen. Eine DWG-Datei für Ebene 2 muss mit „Ebene_2.dgw“ benannt sein. Sollte der Name Ihrer Ebene ein Leerzeichen enthalten, ersetzen Sie dieses durch einen Unterstrich. 

Im Zusammenhang mit der Verwendung der Manifestobjekte müssen zwar gewisse Anforderungen erfüllt werden, es sind aber nicht alle Objekte erforderlich. Die folgende Tabelle enthält die erforderlichen und optionalen Objekte für die Version 1.1 des [Azure Maps-Konvertierungsdiensts](https://docs.microsoft.com/rest/api/maps/conversion):

| Objekt | Erforderlich | Beschreibung |
| :----- | :------- | :------- |
| directoryInfo | true | Gibt die geografischen Informationen und die Kontaktinformationen für die Einrichtung an. Kann auch zur Angabe der geografischen Informationen und der Kontaktinformationen eines Nutzers verwendet werden. |
| buildingLevels | true | Gibt die Ebenen der Gebäude und die Dateien an, die den Plan der Ebenen enthalten. |
| georeference | true | Enthält numerische geografische Informationen für die Einrichtungszeichnung. |
| dwgLayers | true | Listet die Namen der Schichten auf. Außerdem werden für jede Schicht die Namen der zugehörigen Features aufgelistet. |
| unitProperties | false | Ermöglicht das Einfügen zusätzlicher Metadaten für die Einheitenfeatures. |
| zoneProperties | false | Ermöglicht das Einfügen zusätzlicher Metadaten für die Zonenfeatures. |

In den nächsten Abschnitten werden die Anforderungen für die einzelnen Objekte erläutert.

### <a name="directoryinfo"></a>directoryInfo

| Eigenschaft  | type | Erforderlich | Beschreibung |
|-----------|------|----------|-------------|
| name      | String, Integer | true   |  Name des Gebäudes. |
| streetAddress|    String, Integer |    false    | Adresse des Gebäudes. |
|unit     | String, Integer    |  false    |  Einheit im Gebäude. |
| Örtlichkeit |    String, Integer |    false |    Name einer Gegend, eines Stadtteils oder einer Region. Beispiele wären etwa „Schwaben“ oder „Ludwigsvorstadt“. Die örtliche Lage ist nicht Teil der Anschrift. |
| adminDivisions |    JSON-Array mit Zeichenfolgen |    false     | Ein Array mit Adressangaben wie Land, Bundesland, Stadt oder Land, Präfektur, Stadt, Ort. Verwenden Sie Landescodes gemäß ISO 3166 und Gebietscodes gemäß ISO 3166-2. |
| postalCode |    String, Integer    | false    | Die Postleitzahl. |
| hoursOfOperation |    Zeichenfolge |     false | Entspricht dem [Öffnungszeitenformat von OSM](https://wiki.openstreetmap.org/wiki/Key:opening_hours/specification). |
| phone    | String, Integer |    false |    Telefonnummer für das Gebäude. Muss die Landesvorwahl enthalten. |
| Website    | Zeichenfolge |    false    | Website für das Gebäude. Muss mit „http“ oder „https“ beginnen. |
| nonPublic |    bool    | false | Flag, um anzugeben, ob das Gebäude öffentlich zugänglich ist. |
| anchorLatitude | NUMERIC |    false | Breitengrad eines Einrichtungsankers (Ortsmarke). |
| anchorLongitude | NUMERIC |    false | Längengrad eines Einrichtungsankers (Ortsmarke). |
| anchorHeightAboveSeaLevel  | NUMERIC | false | Höhe des Erdgeschosses der Einrichtung über dem Meeresspiegel, angegeben in Metern. |
| defaultLevelVerticalExtent | NUMERIC | false | Die Standardhöhe (Stärke) einer Ebene dieser Einrichtung für den Fall, dass `verticalExtent` für die Ebene nicht definiert ist. |

### <a name="buildinglevels"></a>buildingLevels

Das Objekt `buildingLevels` enthält ein JSON-Array mit Gebäudeebenen.

| Eigenschaft  | type | Erforderlich | Beschreibung |
|-----------|------|----------|-------------|
|levelName    |String, Integer    |true |    Beschreibender Ebenenname. Beispiele: 1. Stock, Eingangsbereich, blaue Parkebene, Keller usw.|
|ordinal | integer |    true | „ordinal“ dient zum Angeben der vertikalen Reihenfolge von Ebenen. Jede Einrichtung muss über eine Ebene mit der Ordnungszahl 0 verfügen. |
|heightAboveFacilityAnchor | NUMERIC |    false |    Die Ebenenhöhe über dem Erdgeschoss, angegeben in Metern. |
| verticalExtent | NUMERIC | false | Die Höhe vom Boden bis zur Decke (Stärke) der Ebene, angegeben in Metern. |
|filename |    String, Integer |    true |    Dateisystempfad der CAD-Zeichnung für eine Gebäudeebene. Muss relativ zum Stamm der ZIP-Datei des Gebäudes angegeben werden. |

### <a name="georeference"></a>georeference

| Eigenschaft  | type | Erforderlich | Beschreibung |
|-----------|------|----------|-------------|
|lat    | NUMERIC |    true |    Dezimaldarstellung des Breitengrads am Ursprung der Anlagenzeichnung. Die Ursprungskoordinaten müssen als WGS 84-Web-Mercator-Koordinaten (`EPSG:3857`) vorliegen.|
|lon    |NUMERIC|    true|    Dezimaldarstellung des Längengrads am Ursprung der Anlagenzeichnung. Die Ursprungskoordinaten müssen als WGS 84-Web-Mercator-Koordinaten (`EPSG:3857`) vorliegen. |
|angle|    NUMERIC|    true|   Der Winkel im Uhrzeigersinn zwischen geografisch Nord und der vertikalen Achse (Y-Achse) der Zeichnung, angegeben in Grad.   |

### <a name="dwglayers"></a>dwgLayers

| Eigenschaft  | type | Erforderlich | Beschreibung |
|-----------|------|----------|-------------|
|exterior    |Array mit Zeichenfolgen/Integer-Werten|    true|    Namen von Schichten zum Definieren des äußeren Gebäudeprofils.|
|unit|    Array mit Zeichenfolgen/Integer-Werten|    true|    Namen von Schichten zum Definieren von Einheiten.|
|wall|    Array mit Zeichenfolgen/Integer-Werten    |false|    Namen von Schichten zum Definieren von Wänden.|
|door    |Array mit Zeichenfolgen/Integer-Werten|    false   | Namen von Schichten zum Definieren von Türen.|
|unitLabel    |Array mit Zeichenfolgen/Integer-Werten|    false    |Namen von Schichten zum Definieren von Einheitennamen.|
|zone | Array mit Zeichenfolgen/Integer-Werten    | false    | Namen von Schichten zum Definieren von Zonen.|
|zoneLabel | Array mit Zeichenfolgen/Integer-Werten |     false |    Namen von Schichten zum Definieren von Zonennamen.|

### <a name="unitproperties"></a>unitProperties

Das Objekt `unitProperties` enthält ein JSON-Array mit Einheiteneigenschaften.

| Eigenschaft  | type | Erforderlich | Beschreibung |
|-----------|------|----------|-------------|
|unitName    |String, Integer    |true    |Name der Einheit, die diesem Datensatz vom Typ `unitProperty` zugeordnet werden soll. Dieser Datensatz ist nur gültig, wenn die Schichten vom Typ `unitLabel` eine Bezeichnung vom Typ `unitName` enthalten. |
|categoryName|    String, Integer|    false    |Kategoriename. Eine vollständige Kategorienliste finden Sie [hier](https://aka.ms/pa-indoor-spacecategories). |
|navigableBy| Array von Zeichenfolgen |    false    |Gibt an, für welche Arten der Fortbewegung die Einheit geeignet ist (beispielsweise für Fußgänger). Diese Eigenschaft wird bei der Wegfindung genutzt.  Zulässige Werte: `pedestrian`, `wheelchair`, `machine`, `bicycle`, `automobile`, `hiredAuto`, `bus`, `railcar`, `emergency`, `ferry`, `boat` und `disallowed`.|
|routeThroughBehavior|    Zeichenfolge|    false    |Das Durchleitungsverhalten für die Einheit. Zulässige Werte: `disallowed`, `allowed` und `preferred`. Der Standardwert ist `allowed`.|
|occupants    |Array mit Objekten vom Typ „directoryInfo“ |false    |Nutzerliste für die Einheit. |
|nameAlt|    String, Integer|    false|    Alternativer Name der Einheit. |
|nameSubtitle|    String, Integer    |false|    Untertitel der Einheit. |
|addressRoomNumber|    String, Integer|    false|    Raum-/Einheiten-/Wohnungs-/Suitennummer der Einheit.|
|verticalPenetrationCategory|    String, Integer|    false| Wenn diese Eigenschaft definiert ist, ist das resultierende Feature keine Einheit, sondern eine vertikale Durchdringung (Vertical Penetration, VRT). VRTs können verwendet werden, um zu anderen VRT-Features auf den darüber oder darunter befindlichen Ebenen zu gelangen. „verticalPenetration“ ist ein [Kategoriename](https://aka.ms/pa-indoor-spacecategories). Wenn diese Eigenschaft definiert ist, wird die Eigenschaft „categoryName“ durch „verticalPenetrationCategory“ überschrieben. |
|verticalPenetrationDirection|    Zeichenfolge|    false    |Ist `verticalPenetrationCategory` definiert, kann optional die gültige Bewegungsrichtung definiert werden. Zulässige Werte: `lowToHigh`, `highToLow`, `both` und `closed`. Der Standardwert ist `both`.|
| nonPublic | bool | false | Gibt an, ob die Einheit öffentlich zugänglich ist. |
| isRoutable | bool | false | Ist diese Eigenschaft auf `false` festgelegt, kann nicht zu dieser Einheit oder nicht durch diese Einheit navigiert werden. Der Standardwert ist `true`. |
| isOpenArea | bool | false | Ermöglicht den Zugang/die Zufahrt zur Einheit, ohne dass eine Öffnung mit der Einheit verknüpft sein muss. Dieser Wert ist standardmäßig auf `true` festgelegt, es sei denn, die Einheit verfügt über eine Öffnung. |

### <a name="the-zoneproperties-object"></a>Das Objekt „zoneProperties“

Das Objekt `zoneProperties` enthält ein JSON-Array mit Zoneneigenschaften.

| Eigenschaft  | type | Erforderlich | Beschreibung |
|-----------|------|----------|-------------|
|zoneName        |String, Integer    |true    |Name der Zone, der dem Datensatz `zoneProperty` zugeordnet werden soll. Dieser Datensatz ist nur gültig, wenn die Schicht `zoneLabel` der Zone eine Bezeichnung vom Typ `zoneName` enthält.  |
|categoryName|    String, Integer|    false    |Kategoriename. Eine vollständige Kategorienliste finden Sie [hier](https://aka.ms/pa-indoor-spacecategories). |
|zoneNameAlt|    String, Integer|    false    |Alternativer Name der Zone.  |
|zoneNameSubtitle|    String, Integer |    false    |Untertitel der Zone. |

### <a name="sample-drawing-package-manifest"></a>Exemplarisches Zeichnungspaketmanifest

Im Anschluss finden Sie eine exemplarische Manifestdatei für das Beispielzeichnungspaket. Das gesamte Paket kann [hier](https://github.com/Azure-Samples/am-creator-indoor-data-examples) heruntergeladen werden.

#### <a name="manifest-file"></a>Manifestdatei

```JSON
{
    "version": "1.1", 
    "directoryInfo": { 
        "name": "Contoso Building", 
        "streetAddresss": "Contoso Way", 
        "unit": "1", 
        "locality": "Contoso eastside", 
        "postalCode": "98052", 
        "adminDivisions": [ 
            "Contoso city", 
            "Contoso state", 
            "Contoso country" 
        ], 
        "hoursOfOperation": "Mo-Fr 08:00-17:00 open", 
        "phone": "1 (425) 555-1234", 
        "website": "www.contoso.com", 
        "nonPublic": false, 
        "anchorLatitude": 47.636152, 
        "anchorLongitude": -122.132600, 
        "anchorHeightAboveSeaLevel": 1000, 
        "defaultLevelVerticalExtent": 3  
    }, 
    "buildingLevels": { 
        "levels": [ 
            { 
                "levelName": "Basement", 
                "ordinal": -1, 
                "filename": "./Basement.dwg" 
            }, { 
                "levelName": "Ground", 
                "ordinal": 0, 
                "verticalExtent": 5, 
                "filename": "./Ground.dwg" 
            }, { 
                "levelName": "Level 2", 
                "ordinal": 1, 
                "heightAboveFacilityAnchor": 3.5, 
                "filename": "./Level_2.dwg" 
            } 
        ] 
    }, 
    "georeference": { 
        "lat": 47.636152, 
        "lon": -122.132600, 
        "angle": 0 
    }, 
    "dwgLayers": { 
        "exterior": [ 
            "OUTLINE", "WINDOWS" 
        ], 
        "unit": [ 
            "UNITS" 
        ], 
        "wall": [ 
            "WALLS" 
        ], 
        "door": [ 
            "DOORS" 
        ], 
        "unitLabel": [ 
            "UNITLABELS" 
        ], 
        "zone": [ 
            "ZONES" 
        ], 
        "zoneLabel": [ 
            "ZONELABELS" 
        ] 
    }, 
    "unitProperties": [ 
        { 
            "unitName": "B01", 
            "categoryName": "room.office", 
            "navigableBy": ["pedestrian", "wheelchair", "machine"], 
            "routeThroughBehavior": "disallowed", 
            "occupants": [ 
                { 
                    "name": "Joe's Office", 
                    "phone": "1 (425) 555-1234" 
                } 
            ], 
            "nameAlt": "Basement01", 
            "nameSubtitle": "01", 
            "addressRoomNumber": "B01", 
            "nonWheelchairAccessible": false, 
            "nonPublic": true, 
            "isRoutable": true, 
            "isOpenArea": true 
        }, 
        { 
            "unitName": "B02" 
        }, 
        { 
            "unitName": "B05", 
            "categoryName": "room.office" 
        }, 
        { 
            "unitName": "STRB01", 
            "verticalPenetrationCategory": "verticalPenetration.stairs", 
            "verticalPenetrationDirection": "both" 
        }, 
        { 
            "unitName": "ELVB01", 
            "verticalPenetrationCategory": "verticalPenetration.elevator", 
            "verticalPenetrationDirection": "high_to_low" 
        } 
    ], 
    "zoneProperties": 
    [ 
        { 
            "zoneName": "WifiB01", 
            "categoryName": "Zone", 
            "zoneNameAlt": "MyZone", 
            "zoneNameSubtitle": "Wifi", 
            "zoneSetId": "1234" 
        }, 
        { 
            "zoneName": "Wifi101",
            "categoryName": "Zone",
            "zoneNameAlt": "MyZone",
            "zoneNameSubtitle": "Wifi",
            "zoneSetId": "1234"
        }
    ]
}
```

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Zeichnungspaket den Anforderungen entspricht, können Sie es mithilfe des [Azure Maps-Konvertierungsdiensts](https://docs.microsoft.com/rest/api/maps/conversion) in ein Kartendataset konvertieren. Anschließend können Sie mit dem Dataset und dem Modul für Gebäudepläne einen Gebäudeplan generieren. Weitere Informationen zur Verwendung des Moduls für Gebäudepläne finden Sie in den folgenden Artikeln:

> [!div class="nextstepaction"]
>[Creator für Gebäudepläne](creator-indoor-maps.md)

> [!div class="nextstepaction"]
> [Tutorial: Erstellen von Gebäudeplänen mithilfe von Creator](tutorial-creator-indoor-maps.md)

> [!div class="nextstepaction"]
> [Implementieren dynamischer Stile für Creator-Gebäudepläne](indoor-map-dynamic-styling.md)
