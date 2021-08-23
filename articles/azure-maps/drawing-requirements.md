---
title: Anforderungen für Zeichnungspakete in Microsoft Azure Maps Creator
description: Erfahren Sie, welche Anforderungen für Zeichnungspakete erfüllt sein müssen, um Ihre Plandateien für Einrichtungen in Kartendaten zu konvertieren
author: anastasia-ms
ms.author: v-stharr
ms.date: 07/02/2021
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
ms.openlocfilehash: 2e6b380b040697a56179f5fec7f7d10796f035fd
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122347168"
---
# <a name="drawing-package-requirements"></a>Anforderungen für Zeichnungspakete

Mit dem [Azure Maps-Konvertierungsdienst](/rest/api/maps/v2/conversion) können Sie hochgeladene Zeichnungspakete in Kartendaten konvertieren. In diesem Artikel werden die Anforderungen für Zeichnungspakete für die Konvertierungs-API beschrieben. Wenn Sie sich ein Beispielpaket ansehen möchten, können Sie das exemplarische [Zeichnungspaket](https://github.com/Azure-Samples/am-creator-indoor-data-examples) herunterladen.

Eine Anleitung zum Vorbereiten des Zeichnungspakets finden Sie unter [Leitfaden zum Konvertieren von Zeichnungspaketen](drawing-package-guide.md).


## <a name="prerequisites"></a>Voraussetzungen

Das Zeichnungspaket enthält gespeicherte Zeichnungen im DWG-Format. Hierbei handelt es sich um das native Dateiformat der AutoCAD®-Software von Autodesk.

Die Zeichnungen im Zeichnungspaket können mithilfe einer beliebigen CAD-Software erstellt werden.  

Das Zeichnungspaket wird vom [Azure Maps-Konvertierungsdienst](/rest/api/maps/v2/conversion) in Kartendaten konvertiert. Der Konvertierungsdienst kann mit dem DWG-Dateiformat `AC1032` von AutoCAD verwendet werden.


## <a name="glossary-of-terms"></a>Glossarbegriffe

Nachfolgend finden Sie einige relevante Begriffe und Definitionen, die für die Inhalte dieses Artikels wichtig sind.

| Begriff  | Definition |
|:-------|:------------|
| Ebene | Eine AutoCAD DWG-Ebene aus der Zeichnungsdatei.|
| Entität | Eine AutoCAD DWG-Entität aus der Zeichnungsdatei. |
| Querverweis  | Eine Datei im DWG-Dateiformat von AutoCAD, die als externe Referenz an die Hauptzeichnung angefügt wird.  |
| Ebene | Ein Gebäudebereich in einer bestimmten Höhe. Hierbei kann es sich beispielsweise um ein Stockwerk handeln. |
| Komponente | Eine Instanz eines vom Konvertierungsdienst erzeugten Objekts, das eine Geometrie mit Metadateninformationen kombiniert. |
| Featureklassen | Eine allgemeine Blaupause für Features. So ist beispielsweise eine *Einheit* eine Featureklasse, während es sich bei einem *Büro* um ein Feature handelt. |

## <a name="drawing-package-structure"></a>Struktur eines Zeichnungspakets

Bei einem Zeichnungspaket handelt es sich um ein ZIP-Archiv mit folgenden Dateien:

- DWG-Dateien im DWG-Dateiformat von AutoCAD
- Eine _manifest.json_-Datei, in der die DWG-Dateien im Zeichnungspaket beschrieben sind.

Das Zeichnungspaket muss als ZIP in einer einzelnen Archivdatei verpackt sein und die ZIP-Erweiterung aufweisen. Die DWG-Dateien können innerhalb des Pakets beliebig strukturiert sein. Die Manifestdatei muss sich jedoch im Stammverzeichnis des gezippten Pakets befinden. In den nächsten Abschnitten werden die Anforderungen für die DWG-Dateien, die Manifestdatei und den Inhalt dieser Dateien erläutert. Wenn Sie sich ein Beispielpaket ansehen möchten, können Sie das [Beispiel-Zeichnungspaket](https://github.com/Azure-Samples/am-creator-indoor-data-examples) herunterladen.

## <a name="dwg-file-conversion-process"></a>DWG-Dateikonvertierungsprozess

Der [Azure Maps-Konvertierungsdienst](/rest/api/maps/v2/conversion) führt für jede DWG-Datei Folgendes aus:

- Extrahiert Featureklassen:
    - Levels
    - Units
    - Zones
    - „Openings“ (Öffnungen)
    - „Walls“ (Wände)
    - „Vertical Penetrations“ (Vertikale Durchdringungen)
- Erzeugt ein *Facility*-Feature.  
- Erzeugt einen minimalen Satz von Standardfeatures der Kategorie, auf die von anderen Features verwiesen werden soll:
    - room
    - structure
    - wall
    - opening.door
    - Zone
    - Funktion
    
## <a name="dwg-file-requirements"></a>Anforderungen an die DWG-Datei

Für jede Ebene der Einrichtung ist jeweils eine einzelne DWG-Datei erforderlich. Alle Daten einer einzelnen Ebene müssen in einer einzelnen DWG-Datei enthalten sein.  Externe Referenzen (_Querverweise_) müssen jeweils an die übergeordnete Zeichnung gebunden werden. Beispielsweise enthält eine Einrichtung mit drei Ebenen drei DWG-Dateien im Zeichnungspaket.

Jede DWG-Datei muss die folgenden Anforderungen erfüllen:

- Die DWG-Datei muss die Schichten _Exterior_ (Außenbereich) und _Unit_ (Einheit) definieren. Folgende Schichten können optional definiert werden: _Wall_ (Wand), _Door_ (Tür), _UnitLabel_ (Einheitenbezeichnung), _Zone_ (Zone) und _ZoneLabel_ (Zonenbezeichnung).
- Die DWG-Datei darf keine Features aus mehreren Ebenen enthalten.
- Die DWG-Datei darf keine Features aus mehreren Einrichtungen enthalten.
- Die DWG-Datei muss auf das gleiche Maßsystem und die gleiche Maßeinheit wie die anderen DWG-Dateien im Zeichnungspaket verweisen.

## <a name="dwg-layer-requirements"></a>DWG-Ebenenanforderungen

Jede DWG-Ebene muss den folgenden Regeln entsprechen:

- Eine Ebene darf nur ausschließlich Features einer einzelnen Klasse enthalten. Beispielsweise können sich Einheiten und Wände nicht auf derselben Ebene befinden.
- Eine einzelne Klasse von Features kann durch mehrere Ebenen dargestellt werden.
- Sich selbst überschneidende Polygone sind zulässig, werden aber automatisch repariert. Nach der Reparatur löst der [Azure Maps-Konvertierungsdienst](/rest/api/maps/v2/conversion) eine Warnung aus. Es empfiehlt sich, die reparierten Ergebnisse manuell zu überprüfen, da sie möglicherweise nicht mit den erwarteten Ergebnissen übereinstimmen.
- Jede Ebene verfügt über eine unterstützte Liste von Entitätstypen. Alle anderen Entitätstypen in einer Ebene werden ignoriert. Beispielsweise werden Textentitäten auf der Wandebene nicht unterstützt.

In der folgenden Tabelle werden die unterstützten Entitätstypen und die konvertierten Map-Funktionen für die einzelnen Schichten beschrieben. Sollte eine Schicht nicht unterstützte Entitätstypen enthalten, werden diese Entitäten vom [Azure Maps-Konvertierungsdienst](/rest/api/maps/v2/conversion) ignoriert.  

| Ebene | Entitätstypen | Konvertierte Funktionen |
| :----- | :-------------------| :-------
| [Exterior](#exterior-layer) | Polygon, Polylinie (geschlossen), Kreis, Ellipse (geschlossen) | Levels
| [Einheit](#unit-layer) |  Polygon, Polylinie (geschlossen), Kreis, Ellipse (geschlossen) |  Einheiten und vertikale Durchdringungen
| [Wall](#wall-layer)  | Polygon, Polylinie (geschlossen), Kreis, Ellipse (geschlossen), Strukturen |
| [Door](#door-layer) | Polygon, Polylinie, Linie, Kreisbogen, Kreis | Openings
| [Zone](#zone-layer) | Polygon, Polylinie (geschlossen), Kreis, Ellipse (geschlossen) | Zones
| [UnitLabel](#unitlabel-layer) | Text (einzeilig) | Nicht zutreffend Von dieser Schicht können nur Eigenschaften zu den Einheitenfeatures der Unit-Schicht hinzugefügt werden. Weitere Informationen finden Sie unter [UnitLabel-Schicht](#unitlabel-layer).
| [ZoneLabel](#zonelabel-layer) | Text (einzeilig) | Nicht zutreffend Von dieser Schicht können nur Eigenschaften zu den Zonenfeatures der Zone-Schicht hinzugefügt werden. Weitere Informationen finden Sie unter [ZoneLabel-Schicht](#zonelabel-layer).

In den Abschnitten unten werden die Anforderungen für die einzelnen Schichten erläutert.

### <a name="exterior-layer"></a>Exterior-Schicht

Die DWG-Datei für die jeweilige Ebene muss eine Schicht zum Definieren der Grenze der betreffenden Ebene enthalten. Diese Schicht wird als *Exterior*-Schicht bezeichnet. Wenn eine Einrichtung also beispielsweise zwei Ebenen enthält, benötigen Sie zwei DWG-Dateien mit einer Exterior-Schicht für jede Datei.

Das [resultierende Einrichtungsdataset](tutorial-creator-indoor-maps.md#create-a-feature-stateset) enthält jeweils nur ein Ebenenfeature pro DWG-Datei – unabhängig davon, wie viele Entitätszeichnungen sich in der Exterior-Schicht befinden. Außerdem zu beachten:

- Außenbereiche müssen als Polygon, Polylinie (geschlossen), Kreis oder Ellipse (geschlossen) gezeichnet werden.
- Außenbereiche überschneiden sich zwar möglicherweise, werden jedoch zu einer einzelnen Geometrie aufgelöst.
- Das resultierende Ebenenfeature muss mindestens 4 Quadratmeter betragen.
- Das resultierende Ebenenfeature darf nicht größer als maximal 400.000 Quadratmeter sein.

Enthält die Schicht mehrere sich überschneidende Polylinien, werden diese zu einem einzelnen Ebenenfeature aufgelöst. Wenn die Schicht dagegen mehrere Polylinien ohne Überschneidung enthält, weist das resultierende Ebenenfeature eine Multipolygondarstellung auf.

Ein Beispiel für die Exterior-Schicht als Outline-Schicht ist im [Beispielzeichenpaket](https://github.com/Azure-Samples/am-creator-indoor-data-examples) enthalten.

### <a name="unit-layer"></a>Unit-Schicht

In der DWG-Datei für die jeweilige Ebene wird eine Schicht mit Einheiten definiert. Bei Einheiten handelt es sich um betretbare Gebäudebereiche wie Büros, Flure, Treppenhäuser und Aufzüge. Wenn die `VerticalPenetrationCategory`-Eigenschaft definiert ist, werden navigierbare Einheiten, die sich über mehrere Ebenen erstrecken, wie etwa Aufzüge und Treppen, in vertikale Durchdringungsfunktionen konvertiert. Vertikale Durchdringungsfunktionen, die einander überschneiden, werden einem `setid` zugewiesen.

Die Unit-Schicht muss folgende Anforderungen erfüllen:

- Einheiten müssen als Polygon, Polylinie (geschlossen), Kreis oder Ellipse (geschlossen) gezeichnet werden.
- Einheiten müssen innerhalb der Außengrenzen der Einrichtung liegen.
- Einheiten dürfen sich nicht teilweise überschneiden.
- Einheiten dürfen keine Geometrie enthalten, die sich selbst schneidet.

Erstellen Sie zum Benennen einer Einheit ein Textobjekt in der Schicht „UnitLabel“, und platzieren Sie es innerhalb der Grenzen der Einheit. Weitere Informationen finden Sie unter [UnitLabel-Schicht](#unitlabel-layer).

Ein Beispiel für die Units-Schicht ist im [Beispielzeichenpaket](https://github.com/Azure-Samples/am-creator-indoor-data-examples) enthalten.

### <a name="wall-layer"></a>Wall-Schicht

Die DWG-Datei für die jeweilige Ebene kann eine Schicht enthalten, die die physischen Abmessungen von Wänden, Säulen und anderen Baustrukturen definiert.

- Walls müssen als Polygon, Polylinie (geschlossen), Kreis oder Ellipse (geschlossen) gezeichnet werden.
- Wall-Schichten dürfen nur eine als Baustruktur interpretierte Geometrie enthalten.

Ein Beispiel für die Walls-Schicht ist im [Beispielzeichenpaket](https://github.com/Azure-Samples/am-creator-indoor-data-examples) enthalten.

### <a name="door-layer"></a>Door-Schicht

Sie können eine DWG-Schicht hinzufügen, die Türen enthält. Jede Tür muss sich mit der Kante einer Einheit aus der Unit-Schicht überschneiden.

Türöffnungen werden in einem Azure Maps-Dataset als Segment mit einer einzelnen Linie dargestellt, das sich mit mehreren Einheitengrenzen überschneidet. Die folgenden Abbildungen zeigen, wie eine Geometrie in der Door-Schicht in Öffnungsfeatures in einem Dataset konvertiert wird.

![Vier Abbildungen zur Veranschaulichung der Schritte, mit denen Öffnungen generiert werden](./media/drawing-requirements/opening-steps.png)

### <a name="zone-layer"></a>Zone-Schicht

Die DWG-Datei für die jeweilige Ebene kann eine Zone-Schicht enthalten, die die physischen Abmessungen von Zonen definiert. Eine Zone ist ein nicht navigierbarer Bereich, der benannt und wiedergegeben werden kann. Zonen können mehrere Ebenen umfassen und mithilfe der ZoneSetId-Eigenschaft gruppiert werden.

- Zonen müssen als Polygon, Polylinie (geschlossen) oder Ellipse (geschlossen) gezeichnet werden.
- Zonen können sich überlappen.
- Zonen können innerhalb oder außerhalb der Außengrenze der Einrichtung liegen.

Erstellen Sie zum Benennen einer Zone ein Textobjekt in der ZoneLabel-Schicht, und platzieren Sie es innerhalb der Grenzen der Zone. Weitere Informationen finden Sie unter [ZoneLabel-Schicht](#zonelabel-layer).

Ein Beispiel für die Zone-Schicht ist im [Beispielzeichenpaket](https://github.com/Azure-Samples/am-creator-indoor-data-examples) enthalten.

### <a name="unitlabel-layer"></a>UnitLabel-Schicht

Die DWG-Datei für die jeweilige Ebene kann eine UnitLabel-Schicht enthalten. Durch die UnitLabel-Schicht wird eine Namenseigenschaft zu Einheiten hinzugefügt, die aus der Unit-Schicht extrahiert wurden. Für Einheiten mit einer Namenseigenschaft können weitere Details in der Manifestdatei angegeben werden.

- Bei Einheitenbezeichnungen muss es sich um einzeilige Textentitäten handeln.
- Einheitenbezeichnungen müssen innerhalb der Grenzen der zugehörigen Einheit liegen.
- Einheiten dürfen nicht mehrere Textentitäten in der UnitLabel-Schicht enthalten.

Ein Beispiel für die UnitLabel-Schicht ist im [Beispielzeichenpaket](https://github.com/Azure-Samples/am-creator-indoor-data-examples) enthalten.

### <a name="zonelabel-layer"></a>ZoneLabel-Schicht

Die DWG-Datei für die jeweilige Ebene kann eine ZoneLabel-Schicht enthalten. Durch diese Schicht wird aus der Zone-Schicht extrahierten Zonen eine Namenseigenschaft hinzugefügt. Für Zonen mit einer Namenseigenschaft können weitere Details in der Manifestdatei angegeben werden.

- Bei Zonenbezeichnungen muss es sich um einzeilige Textentitäten handeln.
- Zonenbezeichnungen müssen innerhalb der Grenzen der zugehörigen Zone liegen.
- Zonen dürfen nicht mehrere Textentitäten in der ZoneLabel-Schicht enthalten.

Ein Beispiel für die ZoneLabel-Schicht ist im [Beispielzeichenpaket](https://github.com/Azure-Samples/am-creator-indoor-data-examples) enthalten.

## <a name="manifest-file-requirements"></a>Anforderungen für die Manifestdatei

Der ZIP-Ordner muss eine Manifestdatei auf der Stammebene des Verzeichnisses enthalten, und die Datei muss **manifest.json** heißen. In dieser Datei werden die DWG-Dateien beschrieben, damit deren Inhalt vom [Azure Maps-Konvertierungsdienst](/rest/api/maps/v2/conversion) analysiert werden kann. Bei der Erfassung werden nur die im Manifest angegebenen Dateien berücksichtigt. Dateien, die sich zwar im ZIP-Ordner befinden, aber nicht ordnungsgemäß im Manifest aufgeführt sind, werden ignoriert.

Die Dateipfade im `buildingLevels`-Objekt der Manifestdatei müssen relativ zum Stamm des ZIP-Ordners angegeben sein. Der DWG-Dateiname muss exakt dem Namen der Einrichtungsebene entsprechen. Eine DWG-Datei für die Ebene „Keller“ heißt also beispielsweise „Keller.dwg“. Eine DWG-Datei für Ebene 2 heißt „Ebene_2.dgw“. Sollte der Name Ihrer Ebene ein Leerzeichen enthalten, ersetzen Sie dieses durch einen Unterstrich.

Im Zusammenhang mit der Verwendung der Manifestobjekte müssen zwar gewisse Anforderungen erfüllt werden, es sind aber nicht alle Objekte erforderlich. Die folgende Tabelle enthält die erforderlichen und optionalen Objekte für die Version 1.1 des [Azure Maps-Konvertierungsdiensts](/rest/api/maps/v2/conversion).

>[!NOTE]
> Sofern nicht anders angegeben, lassen alle Eigenschaften mit dem Eigenschaftstyp Zeichenfolge tausend Zeichen zu.


| Object | Erforderlich | BESCHREIBUNG |
| :----- | :------- | :------- |
| `version` | true |Manifestschemaversion Aktuell wird nur Version 1.1 unterstützt.|
| `directoryInfo` | true | Gibt die geografischen Informationen und die Kontaktinformationen für die Einrichtung an. Kann auch zur Angabe der geografischen Informationen und der Kontaktinformationen eines Nutzers verwendet werden. |
| `buildingLevels` | true | Gibt die Ebenen der Gebäude und die Dateien an, die den Plan der Ebenen enthalten. |
| `georeference` | true | Enthält numerische geografische Informationen für die Einrichtungszeichnung. |
| `dwgLayers` | true | Listet die Namen der Schichten auf. Außerdem werden für jede Schicht die Namen der zugehörigen Features aufgelistet. |
| `unitProperties` | false | Ermöglicht das Einfügen weiterer Metadaten für die Einheitenfeatures. |
| `zoneProperties` | false | Ermöglicht das Einfügen weiterer Metadaten für die Zonenfeatures. |

In den nächsten Abschnitten werden die Anforderungen für die einzelnen Objekte erläutert.

### `directoryInfo`

| Eigenschaft  | type | Erforderlich | BESCHREIBUNG |
|-----------|------|----------|-------------|
| `name`      | Zeichenfolge | true   |  Name des Gebäudes. |
| `streetAddress`|    Zeichenfolge |    false    | Adresse des Gebäudes. |
|`unit`     | Zeichenfolge    |  false    |  Einheit im Gebäude. |
| `locality` |    Zeichenfolge |    false |    Name einer Stadt, eines Bereichs, einer Umgebung oder einer Region.|
| `adminDivisions` |    JSON-Array von Zeichenfolgen |    false     | Ein Array, das Adressbezeichnungen enthält. Beispiel: (Land, Bundesland) Verwenden Sie Landeskennzahlen gemäß ISO 3166 und Gebietscodes gemäß ISO 3166-2. |
| `postalCode` |    Zeichenfolge    | false    | Die Postleitzahl. |
| `hoursOfOperation` |    Zeichenfolge |     false | Entspricht dem [Öffnungszeitenformat von OSM](https://wiki.openstreetmap.org/wiki/Key:opening_hours/specification). |
| `phone`    | Zeichenfolge |    false |    Telefonnummer für das Gebäude. |
| `website`    | Zeichenfolge |    false    | Website für das Gebäude. |
| `nonPublic` |    bool    | false | Flag, um anzugeben, ob das Gebäude öffentlich zugänglich ist. |
| `anchorLatitude` | NUMERIC |    false | Breitengrad eines Einrichtungsankers (Ortsmarke). |
| `anchorLongitude` | NUMERIC |    false | Längengrad eines Einrichtungsankers (Ortsmarke). |
| `anchorHeightAboveSeaLevel`  | NUMERIC | false | Höhe des Erdgeschosses der Einrichtung über dem Meeresspiegel, angegeben in Metern. |
| `defaultLevelVerticalExtent` | NUMERIC | false | Die Standardhöhe (Stärke) einer Ebene dieser Einrichtung für den Fall, dass `verticalExtent` für die Ebene nicht definiert ist. |

### `buildingLevels`

Das Objekt `buildingLevels` enthält ein JSON-Array mit Gebäudeebenen.

| Eigenschaft  | type | Erforderlich | BESCHREIBUNG |
|-----------|------|----------|-------------|
|`levelName`    |Zeichenfolge    |true |    Beschreibender Ebenenname. Beispiel: 1. Stock, Eingangsbereich, blaue Parkebene oder Keller.|
|`ordinal` | integer |    true | Bestimmt die vertikale Reihenfolge der Ebenen. Jede Einrichtung muss über eine Ebene mit der Ordnungszahl 0 verfügen. |
|`heightAboveFacilityAnchor` | NUMERIC | false |    Die Ebenenhöhe über dem Anker in Metern. |
| `verticalExtent` | NUMERIC | false | Die Höhe vom Boden bis zur Decke (Stärke) der Ebene, angegeben in Metern. |
|`filename` |    Zeichenfolge |    true |    Dateisystempfad der CAD-Zeichnung für eine Gebäudeebene. Muss relativ zum Stamm der ZIP-Datei des Gebäudes angegeben werden. |

### `georeference`

| Eigenschaft  | type | Erforderlich | Beschreibung |
|-----------|------|----------|-------------|
|`lat`    | NUMERIC |    true |    Dezimaldarstellung des Breitengrads am Ursprung der Anlagenzeichnung. Die Ursprungskoordinaten müssen als WGS 84-Web-Mercator-Koordinaten (`EPSG:3857`) vorliegen.|
|`lon`    |NUMERIC|    true|    Dezimaldarstellung des Längengrads am Ursprung der Anlagenzeichnung. Die Ursprungskoordinaten müssen als WGS 84-Web-Mercator-Koordinaten (`EPSG:3857`) vorliegen. |
|`angle`|    NUMERIC|    true|   Der Winkel im Uhrzeigersinn zwischen geografisch Nord und der vertikalen Achse (Y-Achse) der Zeichnung, angegeben in Grad.   |

### `dwgLayers`

| Eigenschaft  | type | Erforderlich | Beschreibung |
|-----------|------|----------|-------------|
|`exterior`    |array of strings|    true|    Namen von Schichten zum Definieren des äußeren Gebäudeprofils.|
|`unit`|    array of strings|    true|    Namen von Schichten zum Definieren von Einheiten.|
|`wall`|    array of strings    |false|    Namen von Schichten zum Definieren von Wänden.|
|`door`    |array of strings|    false   | Namen von Schichten zum Definieren von Türen.|
|`unitLabel`    |array of strings|    false    |Namen von Schichten zum Definieren von Einheitennamen.|
|`zone` | array of strings    | false    | Namen von Schichten zum Definieren von Zonen.|
|`zoneLabel` | array of strings |     false |    Namen von Schichten zum Definieren von Zonennamen.|

### `unitProperties`

Das Objekt `unitProperties` enthält ein JSON-Array mit Einheiteneigenschaften.

| Eigenschaft  | type | Erforderlich | BESCHREIBUNG |
|-----------|------|----------|-------------|
|`unitName`    |Zeichenfolge    |true    |Name der Einheit, die diesem Datensatz vom Typ `unitProperty` zugeordnet werden soll. Dieser Datensatz ist nur gültig, wenn die Schichten vom Typ `unitLabel` eine Bezeichnung vom Typ `unitName` enthalten. |
|`categoryName`|    Zeichenfolge|    false    |Zweck der Einheit. Eine Liste der Werte, die von den bereitgestellten Renderingstilen verwendet werden können, finden Sie [hier](https://atlas.microsoft.com/sdk/javascript/indoor/0.1/categories.json). |
|`occupants`    |Array mit Objekten vom Typ „directoryInfo“ |false    |Nutzerliste für die Einheit. |
|`nameAlt`|    Zeichenfolge|    false|    Alternativer Name der Einheit. |
|`nameSubtitle`|    Zeichenfolge    |false|    Untertitel der Einheit. |
|`addressRoomNumber`|    Zeichenfolge|    false|    Raum-, Einheiten-, Wohnungs- oder Suitennummer der Einheit.|
|`verticalPenetrationCategory`|    Zeichenfolge|    false| Wenn diese Eigenschaft definiert ist, ist das resultierende Feature keine Einheit, sondern eine vertikale Durchdringung (Vertical Penetration, VRT). Sie können vertikale Durchdringungen verwenden, um zu anderen Features von vertikalen Durchdringungen in der Ebene darüber oder darunter zu gelangen. „verticalPenetration“ ist ein [Kategoriename](https://aka.ms/pa-indoor-spacecategories). Wenn diese Eigenschaft definiert ist, wird die Eigenschaft `categoryName` durch `verticalPenetrationCategory` überschrieben. |
|`verticalPenetrationDirection`|    Zeichenfolge|    false    |Ist `verticalPenetrationCategory` definiert, kann optional die gültige Bewegungsrichtung definiert werden. Zulässige Werte: `lowToHigh`, `highToLow`, `both` und `closed`. Standardwert: `both`. Bei dem Wert wird die Groß-/Kleinschreibung beachtet.|
| `nonPublic` | bool | false | Gibt an, ob die Einheit öffentlich zugänglich ist. |
| `isRoutable` | bool | false | Wenn diese Eigenschaft auf `false` festgelegt ist, ist keine Bewegung zur oder durch die Einheit möglich. Standardwert: `true`. |
| `isOpenArea` | bool | false | Ermöglicht dem Navigations-Agent den Zugang/die Zufahrt zur Einheit, ohne dass eine Öffnung mit der Einheit verknüpft sein muss. Standardmäßig ist dieser Wert für Einheiten ohne Öffnungen auf `true` festgelegt, für Einheiten mit Öffnungen auf `false`. Das manuelle Festlegen von `isOpenArea` auf `false` für eine Einheit ohne Öffnungen führt zu einer Warnung, da die sich ergebende Einheit nicht durch einen navigierenden Agent erreicht werden kann.|

### `zoneProperties`

Das Objekt `zoneProperties` enthält ein JSON-Array mit Zoneneigenschaften.

| Eigenschaft  | type | Erforderlich | BESCHREIBUNG |
|-----------|------|----------|-------------|
|zoneName        |Zeichenfolge    |true    |Name der Zone, der dem Datensatz `zoneProperty` zugeordnet werden soll. Dieser Datensatz ist nur gültig, wenn die Schicht `zoneLabel` der Zone eine Bezeichnung vom Typ `zoneName` enthält.  |
|categoryName|    Zeichenfolge|    false    |Zweck der Zone. Eine Liste der Werte, die von den bereitgestellten Renderingstilen verwendet werden können, finden Sie [hier](https://atlas.microsoft.com/sdk/javascript/indoor/0.1/categories.json).|
|zoneNameAlt|    Zeichenfolge|    false    |Alternativer Name der Zone.  |
|zoneNameSubtitle|    Zeichenfolge |    false    |Untertitel der Zone. |
|zoneSetId|    Zeichenfolge |    false    | Legen Sie die ID fest, um eine Beziehung zwischen mehreren Zonen herzustellen, damit diese als Gruppe abgefragt oder ausgewählt werden können. Beispielsweise Zonen, die mehrere Ebenen umfassen. |

### <a name="sample-drawing-package-manifest"></a>Exemplarisches Zeichnungspaketmanifest

Unten finden Sie die Manifestdatei für das Beispielzeichnungspaket. Wechseln Sie zum [Beispielzeichnungspaket](https://github.com/Azure-Samples/am-creator-indoor-data-examples), um das gesamte Paket herunterzuladen.

#### <a name="manifest-file"></a>Manifestdatei

```JSON
{
    "version": "1.1", 
    "directoryInfo": { 
        "name": "Contoso Building", 
        "streetAddress": "Contoso Way", 
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
            "occupants": [ 
                { 
                    "name": "Joe's Office", 
                    "phone": "1 (425) 555-1234" 
                } 
            ], 
            "nameAlt": "Basement01", 
            "nameSubtitle": "01", 
            "addressRoomNumber": "B01", 
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

Wenn Ihr Zeichnungspaket den Anforderungen entspricht, können Sie es mithilfe des [Azure Maps-Konvertierungsdiensts](/rest/api/maps/v2/conversion) in ein Kartendataset konvertieren. Anschließend können Sie mit dem Dataset und dem Modul für Gebäudepläne einen Gebäudeplan generieren.

> [!div class="nextstepaction"]
> [Creator: Facility Ontology](creator-facility-ontology.md)

> [!div class="nextstepaction"]
> [Creator für Gebäudepläne](creator-indoor-maps.md)

> [!div class="nextstepaction"]
> [Leitfaden für Zeichnungspakete](drawing-package-guide.md)

> [!div class="nextstepaction"]
>[Creator für Gebäudepläne](creator-indoor-maps.md)

> [!div class="nextstepaction"]
> [Tutorial: Erstellen von Gebäudeplänen mithilfe von Creator](tutorial-creator-indoor-maps.md)

> [!div class="nextstepaction"]
> [Implementieren dynamischer Stile für Gebäudepläne](indoor-map-dynamic-styling.md)