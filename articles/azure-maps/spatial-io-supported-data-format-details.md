---
title: Details zu unterstützten Datenformaten | Microsoft Azure Maps
description: Hier erfahren Sie, wie durch Trennzeichen getrennte räumliche Daten im räumlichen E/A-Modul analysiert werden.
author: anastasia-ms
ms.author: v-stharr
ms.date: 03/03/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
ms.openlocfilehash: 278dae8b3569dd0ff92d3ba12197d1049070167a
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122355692"
---
# <a name="supported-data-format-details"></a>Details zu unterstützten Datenformaten

Dieser Artikel enthält Einzelheiten zur Lese- und Schreibunterstützung für alle XML-Tags und Well-Known-Text-Geometrietypen. Außerdem erfahren Sie, wie durch Trennzeichen getrennte räumliche Daten im räumlichen E/A-Modul analysiert werden.

## <a name="supported-xml-namespaces"></a>Unterstützte XML-Namespaces

Das räumliche E/A-Modul unterstützt XML-Tags aus den folgenden Namespaces:

| Namespacepräfix | Namespace-URI   | Hinweise                                                                    |
|:------------------|:-----------------|:----------------------------------------|
| `atom`           | `http://www.w3.org/2005/Atom`   |                                         |
| `geo`            | `http://www.w3.org/2003/01/geo/wgs84_pos#`  | Schreibgeschützte Unterstützung in GeoRSS-Dateien.           |
| `georss`         | `http://www.georss.org/georss`  |                                                |
| `geourl`         | `http://geourl.org/rss/module/` | Schreibgeschützte Unterstützung in GeoRSS-Dateien.                       |
| `gml`            | `http://www.opengis.net/gml`    |                                                        |
| `gpx`            | `http://www.topografix.com/GPX/1/1` |                                                   |
| `gpxx`           | `http://www.garmin.com/xmlschemas/GpxExtensions/v3` | Schreibgeschützte Unterstützung in GPX-Dateien. Analysiert und verwendet „DisplayColor“. Alle anderen Eigenschaften werden den Formmetadaten hinzugefügt. |
| `gpx_style`      | `http://www.topografix.com/GPX/gpx_style/0/2`      | Unterstützt in GPX-Dateien. Verwendet Linienfarbe. |
| `gx`             | `http://www.google.com/kml/ext/2.2` |                                                      |
| `kml`            | `http://www.opengis.net/kml/2.2`    |                                                      |
| `rss`            |                                 | Schreibgeschützt. Für GeoRSS-Schreibvorgänge wird das Atom-Format verwendet.              |

## <a name="supported-xml-elements"></a>Unterstützte XML-Elemente

Das räumliche E/A-Modul unterstützt die folgenden XML-Elemente. Alle nicht unterstützten XML-Tags werden in ein JSON-Objekt konvertiert. Anschließend wird jedes Tag als Eigenschaft im Feld `properties` der übergeordneten Form oder Ebene hinzugefügt.

### <a name="kml-elements"></a>KML-Elemente

Das räumliche E/A-Modul unterstützt die folgenden KML-Elemente:

| Elementname         | Lesen    | Schreiben   | Hinweise                                                                                                                      |
|----------------------|---------|---------|----------------------------------------------------------------------------------------------------------------------------|
| `address`            | partial | ja     | Das Objekt wird analysiert, aber nicht zum Positionieren der Form verwendet.                                                                    |
| `AddressDetails`     | partial | nein      | Das Objekt wird analysiert, aber nicht zum Positionieren der Form verwendet.                                                                    |
| `atom:author`        | ja     | Ja     |                                                                                                                            |
| `atom:link`          | Ja     | Ja     |                                                                                                                            |
| `atom:name`          | Ja     | ja     |                                                                                                                            |
| `BalloonStyle`       | partial | partial | `displayMode` wird nicht unterstützt. Wird in `PopupTemplate` konvertiert. Fügen Sie zum Schreiben eine Eigenschaft vom Typ `popupTemplate` als Eigenschaft des Features hinzu, für das der Schreibvorgang ausgeführt werden soll. |
| `begin`              | ja     | Ja     |                                                                                                                            |
| `color`              | Ja     | ja     | Beinhaltet `#AABBGGRR` und `#BBGGRR`. Wird analysiert, um eine CSS-Farbzeichenfolge zu erhalten.                                                           |
| `colorMode`          | ja     | Nein      |                                                                                                                            |
| `coordinates`        | Ja     | Ja     |                                                                                                                            |
| `Data`               | Ja     | Ja     |                                                                                                                            |
| `description`        | Ja     | Ja     |                                                                                                                            |
| `displayName`        | Ja     | Ja     |                                                                                                                            |
| `Document`           | Ja     | ja     |                                                                                                                            |
| `drawOrder`          | partial | nein      | Wird für Bodenüberlagerungen gelesen und zum Sortieren dieser Überlagerungen verwendet. 
| `east`               | ja     | Ja     |                                                                                                                            |
| `end`                | Ja     | Ja     |                                                                                                                            |
| `ExtendedData`       | Ja     | ja     | Unterstützt nicht typisierte Elemente vom Typ `Data`, `SimpleData` und `Schema` sowie Entitätsersetzungen im Format `$[dataName]`.                      |
| `extrude`            | partial | partial | Wird nur für Polygone unterstützt. MultiGeometry-Objekte mit Polygonen verschiedener Höhe werden in einzelne Features unterteilt. Linienformate werden nicht unterstützt. Polygone mit der Höhe „0“ werden als flaches Polygon gerendert. Beim Lesen wird die Höhe der ersten Koordinate im äußeren Ring als Höheneigenschaft des Polygons hinzugefügt. Anschließend wird die Höhe der ersten Koordinate verwendet, um das Polygon auf der Karte zu rendern. |
| `fill`               | ja     | Ja     |                                                                                                                            |
| `Folder`             | Ja     | Ja     |                                                                                                                            |
| `GroundOverlay`      | Ja     | ja     | `color` wird nicht unterstützt.                                                                                                   |
| `heading`            | partial | nein      | Wird analysiert, aber nicht von `SimpleDataLayer` gerendert. Ein Schreibvorgang wird nur ausgeführt, wenn Daten in der Eigenschaft der Form gespeichert werden.                 |
| `hotSpot`            | ja     | partial | Ein Schreibvorgang wird nur ausgeführt, wenn Daten in der Eigenschaft der Form gespeichert werden. Einheiten werden nur als Pixel ausgegeben.                         |
| `href`               | ja     | ja     |                                                                                                                            |
| `Icon`               | partial | partial | Wird analysiert, aber nicht von `SimpleDataLayer` gerendert. Die Symboleigenschaft der Form wird nur geschrieben, wenn sie URI-Daten enthält. Nur `href` wird unterstützt. |
| `IconStyle`          | partial | partial | Werte vom Typ `icon`, `heading`, `colorMode` und `hotspots` werden analysiert, aber nicht von `SimpleDataLayer` gerendert.         |
| `innerBoundaryIs`    | ja     | Ja     |                                                                                                                            |
| `kml`                | Ja     | Ja     |                                                                                                                            |
| `LabelStyle`         | Nein      | Nein      |                                                                                                                            |
| `LatLonBox`          | Ja     | Ja     |                                                                                                                            |
| `gx:LatLonQuad`      | Ja     | Ja     |                                                                                                                            |
| `LinearRing`         | Ja     | Ja     |                                                                                                                            |
| `LineString`         | Ja     | Ja     |                                                                                                                            |
| `LineStyle`          | Ja     | ja     | `colorMode` wird nicht unterstützt.                                                                                         |
| `Link`               | ja     | nein      | Für Netzwerkverbindungen wird nur die Eigenschaft `href` unterstützt.                                                                   |
| `MultiGeometry`      | partial | partial | Wird beim Lesen ggf. in einzelne Features unterteilt.                                                                     |
| `name`               | ja     | Ja     |                                                                                                                            |
| `NetworkLink`        | Ja     | nein      | Links müssen sich in der gleichen Domäne befinden wie das Dokument.                                                                  |
| `NetworkLinkControl` | nein      | Nein      |                                                                                                                            |
| `north`              | Ja     | Ja     |                                                                                                                            |
| `open`               | Ja     | Ja     |                                                                                                                            |
| `outerBoundaryIs`    | Ja     | Ja     |                                                                                                                            |
| `outline`            | Ja     | Ja     |                                                                                                                            |
| `overlayXY`          | Nein      | nein      |                                                                                                                            |
| `Pair`               | partial | nein      | Es wird nur der Stil `normal` in einem Element vom Typ `StyleMap` unterstützt. `highlight` wird nicht unterstützt.                                   |
| `phoneNumber`        | ja     | Ja     |                                                                                                                            |
| `PhotoOverlay`       | Nein      | Nein      |                                                                                                                            |
| `Placemark`          | Ja     | Ja     |                                                                                                                            |
| `Point`              | Ja     | Ja     |                                                                                                                            |
| `Polygon`            | Ja     | Ja     |                                                                                                                            |
| `PolyStyle`          | Ja     | ja     |                                                                                                                            |
| `Region`             | partial | partial | `LatLongBox` wird auf der Dokumentebene unterstützt.                                                                      |
| `rotation`           | nein      | Nein      |                                                                                                                            |
| `rotationXY`         | Nein      | Nein      |                                                                                                                            |
| `scale`              | Nein      | Nein      |                                                                                                                            |
| `Schema`             | Ja     | Ja     |                                                                                                                            |
| `SchemaData`         | Ja     | ja     |                                                                                                                            |
| `schemaUrl`          | partial | ja     | Das Laden von Stilen aus externen Dokumenten, die in keiner KMZ-Datei enthalten sind, wird nicht unterstützt.                             |
| `ScreenOverlay`      | nein      | Nein      |                                                                                                                            |
| `screenXY`           | Nein      | Nein      |                                                                                                                            |
| `SimpleData`         | Ja     | Ja     |                                                                                                                            |
| `SimpleField`        | Ja     | Ja     |                                                                                                                            |
| `size`               | Nein      | nein      |                                                                                                                            |
| `Snippet`            | partial | partial | Das Attribut `maxLines` wird ignoriert.                                                                                  |
| `south`              | ja     | Ja     |                                                                                                                            |
| `Style`              | Ja     | ja     |                                                                                                                            |
| `StyleMap`           | partial | nein      | Nur der normale Stil in einem Element vom Typ `StyleMap` wird unterstützt.                                                                        |
| `styleUrl`           | partial | ja     | URLs für externe Stile werden nicht unterstützt.                                                                         |
| `text`               | ja     | ja     | Das Ersetzen von `$[geDirections]` wird nicht unterstützt.                                                                          |
| `textColor`          | ja     | Ja     |                                                                                                                            |
| `TimeSpan`           | Ja     | Ja     |                                                                                                                            |
| `TimeStamp`          | Ja     | Ja     |                                                                                                                            |
| `value`              | Ja     | ja     |                                                                                                                            |
| `viewRefreshMode`    | partial | nein      |  Wenn auf einen WMS-Dienst verwiesen wird, wird für Bodenüberlagerungen nur `onStop` unterstützt. `BBOX=[bboxWest],[bboxSouth],[bboxEast],[bboxNorth]` wird an die URL angefügt und aktualisiert, wenn die Karte verschoben wird.  |
| `visibility`         | ja     | Ja     |                                                                                                                            |
| `west`               | Ja     | Ja     |                                                                                                                            |
| `when`               | Ja     | Ja     |                                                                                                                            |
| `width`              | Ja     | ja     |                                                                                                                            |

### <a name="georss-elements"></a>GeoRSS-Elemente

Das räumliche E/A-Modul unterstützt die folgenden GeoRSS-Elemente:

| Elementname             | Lesen    | Schreiben | Hinweise                                                                                          |
|--------------------------|---------|-------|------------------------------------------------------------------------------------------------|
| `atom:author`            | ja     | Ja   |                                                                                                |
| `atom:category`          | Ja     | Ja   |                                                                                                |
| `atom:content`           | Ja     | Ja   |                                                                                                |
| `atom:contributor`       | Ja     | Ja   |                                                                                                |
| `atom:email`             | Ja     | Ja   |                                                                                                |
| `atom:entry`             | Ja     | Ja   |                                                                                                |
| `atom:feed`              | Ja     | Ja   |                                                                                                |
| `atom:icon`              | Ja     | Ja   |                                                                                                |
| `atom:id`                | Ja     | Ja   |                                                                                                |
| `atom:link`              | Ja     | Ja   |                                                                                                |
| `atom:logo`              | Ja     | Ja   |                                                                                                |
| `atom:name`              | Ja     | Ja   |                                                                                                |
| `atom:published`         | Ja     | Ja   |                                                                                                |
| `atom:rights`            | Ja     | Ja   |                                                                                                |
| `atom:source`            | Ja     | Ja   |                                                                                                |
| `atom:subtitle`          | Ja     | Ja   |                                                                                                |
| `atom:summary`           | Ja     | Ja   |                                                                                                |
| `atom:title`             | Ja     | Ja   |                                                                                                |
| `atom:updated`           | Ja     | Ja   |                                                                                                |
| `atom:uri`               | Ja     | Ja   |                                                                                                |
| `geo:lat`                | Ja     | nein    | Wird als `georss:point` geschrieben.                                                                   |
| `geo:lon`                | ja     | nein    | Wird als `georss:point` geschrieben.                                                                   |
| `geo:long`               | ja     | nein    | Wird als `georss:point` geschrieben.                                                                   |
| `georss:box`             | ja     | nein    | Wird als Polygon gelesen und mit der `subType`-Eigenschaft „Rectangle“ (Rechteck) versehen.                                |
| `georss:circle`          | ja     | Ja   |                                                                                                |
| `georss:elev`            | Ja     | Ja   |                                                                                                |
| `georss:featurename`     | Ja     | Ja   |                                                                                                |
| `georss:featuretypetag`  | Ja     | Ja   |                                                                                                |
| `georss:floor`           | Ja     | Ja   |                                                                                                |
| `georss:line`            | Ja     | Ja   |                                                                                                |
| `georss:point`           | Ja     | Ja   |                                                                                                |
| `georss:polygon`         | Ja     | Ja   |                                                                                                |
| `georss:radius`          | Ja     | Ja   |                                                                                                |
| `georss:relationshiptag` | Ja     | Ja   |                                                                                                |
| `georss:where`           | Ja     | Ja   |                                                                                                |
| `geourl:latitude`        | Ja     | nein    | Wird als `georss:point` geschrieben.                                                                   |
| `geourl:longitude`       | ja     | nein    | Wird als `georss:point` geschrieben.                                                                   |
| `position`               | ja     | nein    | Bei manchen XML-Feeds wird GML mit einem Positionstag (anstelle eines `georss:where`-Tags) umschlossen. Dieses Tag wird zwar gelesen, beim Schreiben wird jedoch ein `georss:where`-Tag verwendet. |
| `rss`                    | ja     | nein    | GeoRSS (im ATOM-Format geschrieben).                                                                 |
| `rss:author`             | ja     | partial | Wird als `atom:author` geschrieben.                                                                 |
| `rss:category`           | ja     | partial | Wird als `atom:category` geschrieben.                                                               |
| `rss:channel`            | ja     | Nein    |                                                                                                |
| `rss:cloud`              | Ja     | Nein    |                                                                                                |
| `rss:comments`           | Ja     | Nein    |                                                                                                |
| `rss:copyright`          | ja     | partial | Wird als `atom:rights` geschrieben, wenn die Form noch nicht über eine Eigenschaft vom Typ `rights` `properties` verfügt.       |
| `rss:description`        | ja     | partial | Wird als `atom:content` geschrieben, wenn die Form noch nicht über eine Eigenschaft vom Typ `content` `properties` verfügt.      |
| `rss:docs`               | ja     | Nein    |                                                                                                |
| `rss:enclosure`          | Ja     | Nein    |                                                                                                |
| `rss:generator`          | Ja     | Nein    |                                                                                                |
| `rss:guid`               | ja     | partial | Wird als `atom:id` geschrieben, wenn die Form noch nicht über eine Eigenschaft vom Typ `id` `properties` verfügt.         |
| `rss:image`              | ja     | partial | Wird als `atom:logo` geschrieben, wenn die Form noch nicht über eine Eigenschaft vom Typ `logo` `properties` verfügt.      |
| `rss:item`               | ja     | partial | Wird als `atom:entry` geschrieben.                                                                  |
| `rss:language`           | ja     | Nein    |                                                                                                |
| `rss:lastBuildDate`      | ja     | partial | Wird als `atom:updated` geschrieben, wenn die Form noch nicht über eine Eigenschaft vom Typ `updated` `properties` verfügt.     |
| `rss:link`               | ja     | partial | Wird als `atom:link` geschrieben.                                                                   |
| `rss:managingEditor`     | ja     | partial | Wird als `atom:contributor` geschrieben.                                                            |
| `rss:pubDate`            | ja     | partial | Wird als `atom:published` geschrieben, wenn die Form noch nicht über eine Eigenschaft vom Typ `published` `properties` verfügt.  |
| `rss:rating`             | ja     | Nein    |                                                                                                |
| `rss:skipDays`           | Ja     | Nein    |                                                                                                |
| `rss:skipHours`          | Ja     | Nein    |                                                                                                |
| `rss:source`             | ja     | partial | Wird als `atom:source` geschrieben mit `atom:link` geschrieben.                                       |
| `rss:textInput`          | ja     | Nein    |                                                                                                |
| `rss:title`              | ja     | partial | Wird als `atom:title` geschrieben.                                                                  |
| `rss:ttl`                | ja     | Nein    |                                                                                                |
| `rss:webMaster`          | Ja     | nein    |                                                                                                |

### <a name="gml-elements"></a>GML-Elemente

Das räumliche E/A-Modul unterstützt die folgenden GML-Elemente: 

| Elementname            | Lesen | Schreiben | Hinweise                                                                                  |
|-------------------------|------|-------|----------------------------------------------------------------------------------------|
| `gml:coordinates`       | ja  | nein    | Wird als `gml:posList` geschrieben.                                                              |
| `gml:curveMember`       | ja  | Nein    |                                                                                        |
| `gml:curveMembers`      | Ja  | Nein    |                                                                                        |
| `gml:Box`               | Ja  | nein    | Wird als `gml:Envelope` geschrieben.                                                             |
| `gml:description`       | ja  | Ja   |                                                                                        |
| `gml:Envelope`          | Ja  | Ja   |                                                                                        |
| `gml:exterior`          | Ja  | Ja   |                                                                                        |
| `gml:Feature`           | Ja  | nein    | Wird als Form geschrieben.                                                                    |
| `gml:FeatureCollection` | ja  | nein    | Wird als Geometrieauflistung geschrieben.                                                      |
| `gml:featureMember`     | ja  | nein    | Wird als Geometrieauflistung geschrieben.                                                      |
| `gml:geometry`          | ja  | nein    | Wird als Form geschrieben.                                                                    |
| `gml:geometryMember`    | ja  | Ja   |                                                                                        |
| `gml:geometryMembers`   | Ja  | Ja   |                                                                                        |
| `gml:identifier`        | Ja  | Ja   |                                                                                        |
| `gml:innerBoundaryIs`   | Ja  | nein    | Wird mithilfe von `gml.interior` geschrieben.                                                          |
| `gml:interior`          | ja  | Ja   |                                                                                        |
| `gml:LinearRing`        | Ja  | Ja   |                                                                                        |
| `gml:LineString`        | Ja  | Ja   |                                                                                        |
| `gml:lineStringMember`  | Ja  | Ja   |                                                                                        |
| `gml:lineStringMembers` | Ja  | Nein    |                                                                                        |
| `gml:MultiCurve`        | Ja  | nein    | Liest nur Member vom Typ `gml:LineString`. Wird als `gml.MultiLineString` geschrieben.                  |
| `gml:MultiGeometry`     | partial  | partial   | Wird nur als FeatureCollection-Element gelesen.                                              |
| `gml:MultiLineString`   | ja  | Ja   |                                                                                        |
| `gml:MultiPoint`        | Ja  | Ja   |                                                                                        |
| `gml:MultiPolygon`      | Ja  | Ja   |                                                                                        |
| `gml:MultiSurface`      | Ja  | nein    | Liest nur Member vom Typ `gml:Polygon`. Wird als `gml.MultiPolygon` geschrieben.                        |
| `gml:name`              | ja  | Ja   |                                                                                        |
| `gml:outerBoundaryIs`   | Ja  | nein    | Wird mithilfe von `gml.exterior` geschrieben.                                                          |
| `gml:Point`             | ja  | Ja   |                                                                                        |
| `gml:pointMember`       | Ja  | Ja   |                                                                                        |
| `gml:pointMembers`      | Ja  | Nein    |                                                                                        |
| `gml:Polygon`           | Ja  | Ja   |                                                                                        |
| `gml:polygonMember`     | Ja  | Ja   |                                                                                        |
| `gml:polygonMembers`    | Ja  | Nein    |                                                                                        |
| `gml:pos`               | Ja  | Ja   |                                                                                        |
| `gml:posList`           | Ja  | Ja   |                                                                                        |
| `gml:surfaceMember`     | Ja  | ja   |                                                                                        |

#### <a name="additional-notes"></a>Weitere Hinweise

- Member-Elemente werden nach einer Geometrie durchsucht, die sich möglicherweise in untergeordneten Elementen befindet. Dieser Suchvorgang ist erforderlich, da viele auf GML-basierende XML-Formate möglicherweise keine Geometrie als direkt untergeordnetes Element eines Member-Elements platzieren.
- `srsName` wird für WGS84-Koordinaten und die folgenden Codes teilweise unterstützt: [EPSG:4326](https://epsg.io/4326) und Web Mercator ([EPSG:3857](https://epsg.io/3857)) oder einer der alternativen Codes. Alle anderen Koordinatensysteme werden unverändert als WGS84 analysiert.
- Sofern beim Lesen eines XML-Feeds nicht angegeben, wird die Achsenreihenfolge auf der Grundlage von Hinweisen im XML-Feed ermittelt. Die Achsenreihenfolge „Breitengrad, Längengrad“ wird bevorzugt.
- Ist beim Schreiben in eine GML-Datei kein benutzerdefinierter GML-Namespace für die Eigenschaften angegeben, werden keine zusätzlichen Eigenschaftsinformationen hinzugefügt.

### <a name="gpx-elements"></a>GPX-Elemente

Das räumliche E/A-Modul unterstützt die folgenden GPX-Elemente:

| Elementname             | Lesen    | Schreiben   | Hinweise                                                                                       |
|--------------------------|---------|---------|---------------------------------------------------------------------------------------------|
| `gpx:ageofdgpsdata`      | ja     | Ja     |                                                                                             |
| `gpx:author`             | Ja     | Ja     |                                                                                             |
| `gpx:bounds`             | Ja     | ja     | Wird beim Lesen in ein LocationRect-Element konvertiert.                                                    |
| `gpx:cmt`                | ja     | Ja     |                                                                                             |
| `gpx:copyright`          | Ja     | Ja     |                                                                                             |
| `gpx:desc`               | Ja     | ja     | Wird zur Abstimmung auf andere XML-Formate beim Lesen in eine Beschreibungseigenschaft kopiert.               |
| `gpx:dgpsid`             | ja     | Ja     |                                                                                             |
| `gpx:ele`                | Ja     | ja     |                                                                                             |
| `gpx:extensions`         | partial | partial | Beim Lesen werden Stilinformationen extrahiert. Alle anderen Erweiterungen werden in einem einfachen JSON-Objekt zusammengefasst. Nur Informationen zum Formstil werden geschrieben. |
| `gpx:geoidheight`        | ja     | Ja     |                                                                                             |
| `gpx:gpx`                | Ja     | Ja     |                                                                                             |
| `gpx:hdop`               | Ja     | Ja     |                                                                                             |
| `gpx:link`               | Ja     | Ja     |                                                                                             |
| `gpx:magvar`             | Ja     | Ja     |                                                                                             |
| `gpx:metadata`           | Ja     | Ja     |                                                                                             |
| `gpx:name`               | Ja     | Ja     |                                                                                             |
| `gpx:pdop`               | Ja     | Ja     |                                                                                             |
| `gpx:rte`                | Ja     | Ja     |                                                                                             |
| `gpx:rtept`              | Ja     | Ja     |                                                                                             |
| `gpx:sat`                | Ja     | Ja     |                                                                                             |
| `gpx:src`                | Ja     | Ja     |                                                                                             |
| `gpx:sym`                | Ja     | ja     | Der Wert wird erfasst, aber nicht zum Ändern des Reißzweckensymbols verwendet.                               |
| `gpx:text`               | ja     | Ja     |                                                                                             |
| `gpx:time`               | Ja     | Ja     |                                                                                             |
| `gpx:trk`                | Ja     | Ja     |                                                                                             |
| `gpx:trkpt`              | Ja     | Ja     |                                                                                             |
| `gpx:trkseg`             | Ja     | Ja     |                                                                                             |
| `gpx:type`               | Ja     | Ja     |                                                                                             |
| `gpx:vdop`               | Ja     | Ja     |                                                                                             |
| `gpx:wpt`                | Ja     | Ja     |                                                                                             |
| `gpx_style:color`        | Ja     | ja     |                                                                                             |
| `gpx_style:line`         | partial | partial | `color`, `opacity`, `width` und `lineCap` werden unterstützt.                                           |
| `gpx_style:opacity`      | ja     | Ja     |                                                                                             |
| `gpx_style:width`        | Ja     | Ja     |                                                                                             |
| `gpxx:DisplayColor`      | Ja     | nein      | Dient zum Angeben der Farbe einer Form. Beim Schreiben wird stattdessen die Farbe `gpx_style:line` verwendet.  |
| `gpxx:RouteExtension`    | partial | nein      | Alle Eigenschaften werden in `properties` eingelesen. Es wird nur `DisplayColor` verwendet.                     |
| `gpxx:TrackExtension`    | partial | nein      | Alle Eigenschaften werden in `properties` eingelesen. Es wird nur `DisplayColor` verwendet.                     |
| `gpxx:WaypointExtension` | partial | nein      | Alle Eigenschaften werden in `properties` eingelesen. Es wird nur `DisplayColor` verwendet.                     |
| `gpx:keywords`           | ja     | Ja     |                                                                                             |
| `gpx:fix`                | Ja     | ja     |                                                                                             |

#### <a name="additional-notes"></a>Weitere Hinweise

Beim Schreiben gilt Folgendes:

- MultiPoint-Elemente werden in einzelne Wegpunkte unterteilt.
- Polygon- und MultiPolygon-Elemente werden als Strecken geschrieben. 
  
## <a name="supported-well-known-text-geometry-types"></a>Unterstützte Well-Known-Text-Geometrietypen

| Geometrietyp | Lesen | Schreiben |
|--------------|:----:|:-----:|
| POINT | x | x |
| POINT Z | x | x | 
| POINT M | x | x<sup>[2]</sup> |
| POINT ZM | x<sup>[1]</sup><sup>[2]</sup> | | 
| LINESTRING | x | x |
| LINESTRING Z | x | x | 
| LINESTRING M | x | x<sup>[2]</sup> |
| LINESTRING ZM | x<sup>[1]</sup><sup>[2]</sup> | | 
| POLYGON | x | x |
| POLYGON Z | x | x |
| POLYGON M | x | x<sup>[2]</sup> |
| POLYGON ZM | x<sup>[1]</sup><sup>[2]</sup> | | 
| MULTIPOINT | x | x |
| MULTIPOINT Z | x | x | 
| MULTIPOINT M | x | x<sup>[2]</sup> |
| POMULTIPOINTINT ZM | x<sup>[1]</sup><sup>[2]</sup> | | 
| MULTILINESTRING | x | x |
| MULTILINESTRING Z | x | x | 
| MULTILINESTRING M | x | x<sup>[2]</sup> |
| MULTILINESTRING ZM | x<sup>[1]</sup><sup>[2]</sup> | | 
| MULTIPOLYGON | x | x |
| MULTIPOLYGON Z | x | x | 
| MULTIPOLYGON M | x | x<sup>[2]</sup> |
| MULTIPOLYGON ZM | x<sup>[1]</sup><sup>[2]</sup> | | 
| GEOMETRYCOLLECTION | x | x |
| GEOMETRYCOLLECTION Z | x | x | 
| GEOMETRYCOLLECTION M | x | x<sup>[2]</sup> | 
| GEOMETRYCOLLECTION ZM | x<sup>[1]</sup><sup>[2]</sup> | x | 

\[1\] Nur der Z-Parameter wird erfasst und als dritter Wert in den Positionswert eingefügt.

\[2\] Der M-Parameter wird nicht erfasst.

## <a name="delimited-spatial-data-support"></a>Unterstützung von durch Trennzeichen getrennten räumlichen Daten

Durch Trennzeichen getrennte räumliche Daten (beispielsweise CSV-Dateien) enthalten häufig Spalten mit räumlichen Daten. Dabei kann es sich beispielsweise um Spalten mit Breiten- und Längengradinformationen handeln. Im Well-Known-Text-Format kann eine Spalte mit räumlichen Geometriedaten enthalten sein.

### <a name="spatial-data-column-detection"></a>Erkennung von Spalten mit räumlichen Daten

Beim Lesen einer durch Trennzeichen getrennten Datei mit räumlichen Daten wird durch Analysieren der Kopfzeile ermittelt, welche Spalten Positionsfelder enthalten. Wenn die Kopfzeile Typinformationen enthält, werden diese verwendet, um die Zellwerte in den entsprechenden Typ umzuwandeln. Ist keine Kopfzeile angegeben, wird die erste Zeile analysiert und zum Generieren einer Kopfzeile verwendet. Bei der Analyse der ersten Zeile werden Spaltennamen ohne Berücksichtigung der Groß-/Kleinschreibung mit den folgenden Namen abgeglichen. Die Reihenfolge der Namen entspricht der Priorität für den Fall, dass in einer Datei mehrere Namen vorhanden sind.

#### <a name="latitude"></a>Breitengrad

- `latitude`
- `lat`
- `latdd`
- `lat_dd`
- `latitude83`
- `latdecdeg`
- `y`
- `ycenter`
- `point-y`

#### <a name="longitude"></a>Längengrad

- `longitude`
- `lon`
- `lng`
- `long`
- `longdd`
- `long_dd`
- `longitude83`
- `longdecdeg`
- `x`
- `xcenter`
- `point-x`

#### <a name="elevation"></a>Elevation

- `elevation`
- `elv`
- `altitude`
- `alt`
- `z`

#### <a name="geography"></a>Gebiet

Die erste Datenzeile wird nach Zeichenfolgen im Well-Known-Text-Format durchsucht. 

### <a name="delimited-data-column-types"></a>Spaltentypen für durch Trennzeichen getrennte Daten

Bei der Überprüfung der Kopfzeile werden im Spaltennamen enthaltene Typinformationen extrahiert und zur Umwandlung der Zellen in dieser Spalte verwendet. Das folgende Beispiel zeigt einen Spaltennamen mit einem Typwert: "ColumnName (typeName)". Die folgenden Namen werden unterstützt (ohne Berücksichtigung der Groß-/Kleinschreibung):

#### <a name="numbers"></a>Zahlen

- edm.int64
- INT
- long
- edm.double
- float
- double
- number

#### <a name="booleans"></a>Boolesche Werte

- edm.boolean
- bool
- boolean

#### <a name="dates"></a>Datumsangaben

- edm.datetime
- date
- datetime

#### <a name="geography"></a>Gebiet

- edm.geography
- geography

#### <a name="strings"></a>Zeichenfolgen

- edm.string
- varchar
- text
- Zeichenfolge

Falls keine Typinformationen aus der Kopfzeile extrahiert werden können und beim Lesen die Option für die dynamische Typzuweisung aktiviert ist, wird jede Zelle einzeln analysiert, um zu bestimmen, welcher Datentyp am besten für die Umwandlung geeignet ist.

## <a name="next-steps"></a>Nächste Schritte

In den folgenden Artikeln finden Sie weitere Codebeispiele, die Sie Ihren Karten hinzufügen können:

[Lesen und Schreiben räumlicher Daten](spatial-io-read-write-spatial-data.md)
