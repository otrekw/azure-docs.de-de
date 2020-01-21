---
title: 'Tutorial: Migrieren von Webdiensten aus Google Maps | Microsoft Azure Maps'
description: Hier wird erläutert, wie Sie Webdienste aus Google Maps zu Microsoft Azure Maps migrieren.
author: rbrundritt
ms.author: richbrun
ms.date: 12/17/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: ''
ms.openlocfilehash: 51c00524c781d9af58f60b36aa3baeb079c6eafa
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2020
ms.locfileid: "75910757"
---
# <a name="migrate-web-service-from-google-maps"></a>Migrieren von Webdiensten aus Google Maps

Sowohl Azure als auch Google Maps ermöglichen den Zugriff auf räumliche APIs über REST-Webdienste. Die API-Schnittstellen für diese Plattformen bieten ähnliche Funktionen, verwenden jedoch unterschiedliche Namenskonventionen und Antwortobjekte.

In der folgenden Tabelle finden Sie die Azure Maps-Dienst-APIs, die ähnliche Funktionen wie die aufgelisteten Google Maps-Dienst-APIs bereitstellen.

| Google Maps-Dienst-API | Azure Maps-Dienst-API                                                                      |
|-------------------------|---------------------------------------------------------------------------------------------|
| Wegbeschreibungen              | [Route](https://docs.microsoft.com/rest/api/maps/route)                               |
| Entfernungsmatrix         | [Routenmatrix](https://docs.microsoft.com/rest/api/maps/route/postroutematrixpreview) |
| Geocodierung               | [Suchen,](https://docs.microsoft.com/rest/api/maps/search)                             |
| Suche nach Orten           | [Suchen,](https://docs.microsoft.com/rest/api/maps/search)                             |
| AutoVervollständigen von Orten      | [Suchen,](https://docs.microsoft.com/rest/api/maps/search)                             |
| Statische Karte              | [Render](https://docs.microsoft.com/rest/api/maps/render/getmapimage)                 |
| Zeitzone               | [Zeitzone](https://docs.microsoft.com/rest/api/maps/timezone)                        |

Die folgenden Dienst-APIs sind derzeit noch nicht in Azure Maps verfügbar:

- Elevation
- Geolocation
- Details und Fotos zu Orten. Telefonnummern und Website-URLs, die in der Azure Maps-Such-API verfügbar sind.
- Karten-URLs
- Straßen: Daten zur Geschwindigkeitsbegrenzung sind über die Routen- und Reverse-Geocodierungs-APIs in Azure Maps verfügbar.
- Statische Straßenansicht

Azure Maps verfügt über mehrere zusätzliche REST-Webdienste, die möglicherweise von Interesse sind:

- [Räumliche Vorgänge:](https://docs.microsoft.com/rest/api/maps/spatial) Lagern Sie komplexe räumliche Berechnungen und Vorgänge, wie z. B. Geofencing, in einen Dienst aus.
- [Datenverkehr:](https://docs.microsoft.com/rest/api/maps/traffic) Greifen Sie in Echtzeit auf den Datenverkehrsfluss und die Vorfallsdaten zu.

## <a name="geocoding-addresses"></a>Geocodierung von Adressen

Bei der Geocodierung handelt es sich um den Prozess, bei dem eine Adresse (z. B. „1 Microsoft Way, Redmond, WA“) in eine Koordinate (z. B. Längengrad: –122,1298, Breitengrad: 47,64005) konvertiert wird. Koordinaten werden dann häufig zum Positionieren eines Markers auf einer Karte oder zum Zentrieren einer Karte verwendet.

Azure Maps bietet mehrere Methoden zur Geocodierung von Adressen:

- [**Freiform-Geocodierung von Adressen:** ](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress) Geben Sie eine einzelne Adresszeichenfolge an (z. B. „1 Microsoft Way, Redmond, WA“), und verarbeiten Sie die Anforderung sofort. Dies wird empfohlen, wenn Sie einzelne Adressen schnell geocodieren müssen.
- [**Strukturierte Geocodierung von Adressen:** ](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressstructured) Geben Sie die Teile einer einzelnen Adresse an, z. B. den Namen der Straße, die Stadt, das Land und die Postleitzahl, und verarbeiten Sie die Anforderung sofort. Dies wird empfohlen, wenn Sie einzelne Adressen schnell geocodieren müssen und die einzelnen Adressbestandteile in den Daten bereits analysiert wurden.
- [**Batch-Geocodierung von Adressen:** ](https://docs.microsoft.com/rest/api/maps/search/postsearchaddressbatchpreview) Erstellen Sie eine Anforderung mit bis zu 10.000 Adressen, und lassen Sie sie über einen bestimmten Zeitraum verarbeiten. Alle Adressen werden auf dem Server parallel geocodiert. Nach Abschluss des Vorgangs kann das vollständige Resultset heruntergeladen werden. Dies wird für die Geocodierung großer Datasets empfohlen.
- [**Fuzzysuche:** ](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) Diese API kombiniert die Geocodierung von Adressen mit der Suche nach Points of Interest (POI). Diese API übernimmt eine Freiform-Zeichenfolge, die eine Adresse, ein Ort, ein Orientierungspunkt, ein Point of Interest oder eine POI-Kategorie sein kann, und verarbeitet die Anforderung sofort. Diese API wird für Anwendungen empfohlen, in denen Benutzer im selben Textfeld nach Adressen oder Points of Interest suchen können.
- [**Batchfuzzysuche:** ](https://docs.microsoft.com/rest/api/maps/search/postsearchfuzzybatchpreview) Erstellen Sie eine Anforderung mit bis zu 10.000 Adressen, Orten, Orientierungspunkten oder Points of Interest, und lassen Sie sie über einen bestimmten Zeitraum verarbeiten. Alle Daten werden auf dem Server parallel verarbeitet. Nach Abschluss des Vorgangs kann das vollständige Resultset heruntergeladen werden.

In der folgenden Tabelle werden die Parameter der Google Maps-API den vergleichbaren API-Parametern in Azure Maps gegenübergestellt.

| Google Maps-API-Parameter | Vergleichbarer Azure Maps-API-Parameter  |
|---------------------------|--------------------------------------|
| `address`                   | `query`                            |
| `bounds`                    | `topLeft` und `btmRight`           |
| `components`                | `streetNumber`<br/>`streetName`<br/>`crossStreet`<br/>`postalCode`<br/>`municipality`: Stadt/Ort<br/>`municipalitySubdivision`: Umgebung, Vorort/Metropolenregion<br/>`countrySubdivision`: Bundesland oder Kanton<br/>`countrySecondarySubdivision`: Landkreis<br/>`countryTertiarySubdivision`: Verwaltungsbezirk<br/>`countryCode`: zweibuchstabiger Ländercode |
| `key`                       | `subscription-key`: Weitere Informationen finden Sie auch in der Dokumentation zur [Authentifizierung mit Azure Maps](azure-maps-authentication.md). |
| `language`                  | `language`: Lesen Sie auch die Dokumentation zu den [unterstützten Sprachen](supported-languages.md).  |
| `region`                    | `countrySet`                       |

Ein Beispiel für die Verwendung des Suchdiensts finden Sie [hier](how-to-search-for-address.md). Lesen Sie unbedingt die Dokumentation mit [bewährten Methoden für die Suche](how-to-use-best-practices-for-search.md).

> [!TIP]
> Die Freiform-Geocodierung von Adressen und die APIs für die Fuzzysuche können im AutoVervollständigen-Modus verwendet werden, indem der Anforderungs-URL `&amp;typeahead=true` hinzugefügt wird. Dadurch wird dem Server mitgeteilt, dass der Eingabetext wahrscheinlich unvollständig ist, sodass der Vorhersagemodus aktiviert wird.

## <a name="reverse-geocode-a-coordinate"></a>Umgekehrte Geocodierung einer Koordinate

Bei der umgekehrten Geocodierung handelt es sich um den Prozess der Konvertierung geografischer Koordinaten (z. B. Längengrad: –122,1298, Breitengrad: 47,64005) in die ungefähre Adresse (z. B. „1 Microsoft Way, Redmond, WA“).

Azure Maps bietet mehrere Methoden für die umgekehrte Geocodierung:

- [**Umgekehrte Geocodierung von Adressen:** ](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) Geben Sie eine einzelne geografische Koordinate an, um ihre ungefähre Adresse zu erhalten, und verarbeiten Sie die Anforderung sofort.
- [**Umgekehrte Geocodierung nach Straßenkreuzungen:** ](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreversecrossstreet) Geben Sie eine einzelne geografische Koordinate an, um in der Nähe gelegene Straßenkreuzungen (z. B. „1st & main“) abzurufen und die Anforderung sofort zu verarbeiten.
- [**Umgekehrte Batch-Geocodierung von Adressen:** ](https://docs.microsoft.com/rest/api/maps/search/postsearchaddressreversebatchpreview) Erstellen Sie eine Anforderung mit bis zu 10.000 Koordinaten, und lassen Sie sie über einen bestimmten Zeitraum verarbeiten. Alle Daten werden auf dem Server parallel verarbeitet. Nach Abschluss des Vorgangs kann das vollständige Resultset heruntergeladen werden.

In der folgenden Tabelle werden die Parameter der Google Maps-API den vergleichbaren API-Parametern in Azure Maps gegenübergestellt.

| Google Maps-API-Parameter   | Vergleichbarer Azure Maps-API-Parameter   |
|-----------------------------|---------------------------------------|
| `key`                       | `subscription-key`: Weitere Informationen finden Sie auch in der Dokumentation zur [Authentifizierung mit Azure Maps](azure-maps-authentication.md). |
| `language`                  | `language`: Lesen Sie auch die Dokumentation zu den [unterstützten Sprachen](supported-languages.md).  |
| `latlng`                    | `query`  |
| `location_type`             | *N/V*     |
| `result_type`               | `entityType`    |

Lesen Sie unbedingt die Dokumentation mit [bewährten Methoden für die Suche](how-to-use-best-practices-for-search.md).

Die Azure Maps-API für die umgekehrte Geocodierung bietet einige zusätzliche Funktionen, die in Google Maps nicht verfügbar sind und bei der Migration Ihrer App hilfreich sein können:

- Abrufen von Daten zur Geschwindigkeitsbegrenzung
- Abrufen von Informationen zur Straßennutzung: lokale Straße, Umgehungsstraße, eingeschränkte Zufahrt, Bodenschwellen usw.
- Die Straßenseite, auf der die Koordinate liegt

## <a name="search-for-points-of-interest"></a>Suchen nach Points of Interest

Daten zu Points of Interest können in Google Maps mithilfe der Such-API für Orte gesucht werden. Diese API bietet drei verschiedene Möglichkeiten zum Suchen nach Points of Interest:

- **Suchen von Orten aus Text:** Sucht nach einem Point of Interest anhand des Namens, der Adresse oder der Telefonnummer.
- **Umgebungssuche:** Sucht nach Points of Interest innerhalb einer bestimmten Entfernung zum Standort.
- **Textsuche:** Sucht nach Orten anhand von Freiformtext, der einen Point of Interest und Informationen zum Standort enthält. Beispiele sind „Pizza in New York“ oder „Restaurants auf der Hauptstraße“.

Azure Maps stellt mehrere Such-APIs für Points of Interest bereit:

- [**POI-Suche:** ](https://docs.microsoft.com/rest/api/maps/search/getsearchpoi) Sucht nach Points of Interest nach dem Namen. Beispiel: „Starbucks“.
- [**POI-Kategoriesuche:** ](https://docs.microsoft.com/rest/api/maps/search/getsearchpoicategory) Sucht nach Points of Interest nach der Kategorie. Beispiel: „Restaurant“.
- [**Umgebungssuche:** ](https://docs.microsoft.com/rest/api/maps/search/getsearchnearby) Sucht nach Points of Interest innerhalb einer bestimmten Entfernung zum Standort.
- [**Fuzzysuche:** ](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) Diese API kombiniert die Geocodierung von Adressen mit der Suche nach Points of Interest (POI). Diese API übernimmt eine Freiform-Zeichenfolge, die eine Adresse, ein Ort, ein Orientierungspunkt, ein Point of Interest oder eine POI-Kategorie sein kann, und verarbeitet die Anforderung sofort. Diese API wird für Anwendungen empfohlen, in denen Benutzer im selben Textfeld nach Adressen oder Points of Interest suchen können.
- [**Suche innerhalb einer Geometrie:** ](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry) Sucht nach Points of Interest innerhalb einer angegebenen Geometrie (Polygon).
- [**Suche entlang einer Route:** ](https://docs.microsoft.com/rest/api/maps/search/postsearchalongroute) Sucht nach Points of Interest entlang einer angegebenen Route.
- [**Batchfuzzysuche:** ](https://docs.microsoft.com/rest/api/maps/search/postsearchfuzzybatchpreview) Erstellen Sie eine Anforderung mit bis zu 10.000 Adressen, Orten, Orientierungspunkten oder Points of Interest, und lassen Sie sie über einen bestimmten Zeitraum verarbeiten. Alle Daten werden auf dem Server parallel verarbeitet. Nach Abschluss des Vorgangs kann das vollständige Resultset heruntergeladen werden.

Derzeit verfügt Azure Maps nicht über eine mit der Textsuche-API von Google Maps vergleichbare API.

> [!TIP]
> Die APIs für die POI-Suche, die POI-Kategoriesuche und die Fuzzysuche können im AutoVervollständigen-Modus verwendet werden, indem der Anforderungs-URL `&amp;typeahead=true` hinzugefügt wird. Dadurch wird dem Server mitgeteilt, dass der Eingabetext wahrscheinlich unvollständig ist, sodass der Vorhersagemodus aktiviert wird.

Lesen Sie unbedingt die Dokumentation mit [bewährten Methoden für die Suche](how-to-use-best-practices-for-search.md).

### <a name="find-place-from-text"></a>Suchen von Orten aus Text

Wenn Sie Points of Interest nach dem Namen oder der Adresse suchen möchten, können Sie die Azure Maps-APIs für die [POI-Suche](https://docs.microsoft.com/rest/api/maps/search/getsearchpoi) und die [Fuzzysuche](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) verwenden.

In der folgenden Tabelle werden die Parameter der Google Maps-API den vergleichbaren API-Parametern in Azure Maps gegenübergestellt.

| Google Maps-API-Parameter | Vergleichbarer Azure Maps-API-Parameter |
|---------------------------|-------------------------------------|
| `fields`                  | *N/V*                               |
| `input`                   | `query`                             |
| `inputtype`               | *N/V*                               |
| `key`                     | `subscription-key`: Weitere Informationen finden Sie auch in der Dokumentation zur [Authentifizierung mit Azure Maps](azure-maps-authentication.md). |
| `language`                | `language`: Lesen Sie auch die Dokumentation zu den [unterstützten Sprachen](supported-languages.md).  |
| `locationbias`            | `lat`, `lon` und `radius`<br/>`topLeft` und `btmRight`<br/>`countrySet`  |

### <a name="nearby-search"></a>Umgebungssuche

Nahe gelegene Points of Interest können in Azure Maps mithilfe der [Umgebungssuche](https://docs.microsoft.com/rest/api/maps/search/getsearchnearby)-API abgerufen werden.

In der folgenden Tabelle werden die Parameter der Google Maps-API den vergleichbaren API-Parametern in Azure Maps gegenübergestellt.

| Google Maps-API-Parameter | Vergleichbarer Azure Maps-API-Parameter  |
|---------------------------|--------------------------------------|
| `key`                       | `subscription-key`: Weitere Informationen finden Sie auch in der Dokumentation zur [Authentifizierung mit Azure Maps](azure-maps-authentication.md). |
| `keyword`                   | `categorySet` und `brandSet`        |
| `language`                  | `language`: Lesen Sie auch die Dokumentation zu den [unterstützten Sprachen](supported-languages.md).  |
| `location`                  | `lat` und `lon`                     |
| `maxprice`                  | *N/V*                               |
| `minprice`                  | *N/V*                               |
| `name`                      | `categorySet` und `brandSet`        |
| `opennow`                   | *N/V*                               |
| `pagetoken`                 | `ofs` und `limit`                   |
| `radius`                    | `radius`                            |
| `rankby`                    | *N/V*                               |
| `type`                      | `categorySet –` Weitere Informationen finden Sie in der Dokumentation zu den [unterstützten Suchkategorien](supported-search-categories.md).   |

## <a name="calculate-routes-and-directions"></a>Berechnen von Routen und Wegbeschreibungen

Sie können Azure Maps zum Berechnen von Routen und Wegbeschreibungen verwenden. Azure Maps verfügt über viele der gleichen Funktionen wie der Routenplanungsdienst von Google Maps, z. B.:

- Ankunfts- und Abfahrzeiten
- Routen anhand des echtzeitbasierten und vorhergesagten Verkehrsaufkommens
- Unterschiedliche Transportmodi wie mit dem Auto, zu Fuß oder mit dem Fahrrad

> [!NOTE]
> Bei Azure Maps müssen alle Wegpunkte Koordinaten sein. Adressen müssen zunächst geocodiert werden.

Der Routenplanungsdienst von Azure Maps stellt die folgenden APIs zum Berechnen von Routen bereit:

- [**Berechnen der Route:** ](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) Berechnen Sie die Route, und lassen Sie die Anforderung sofort verarbeiten. Diese API unterstützt sowohl GET- als auch POST-Anforderungen. POST-Anforderungen werden empfohlen, wenn eine große Anzahl von Wegpunkten angegeben wird oder wenn viele der Routenoptionen verwendet werden, um sicherzustellen, dass die URL-Anforderung nicht zu lang wird und Probleme verursacht.
- [**Batchroute:** ](https://docs.microsoft.com/rest/api/maps/route/postroutedirectionsbatchpreview) Erstellen Sie eine Anforderung mit bis zu 1.000 Routenanforderungen, und lassen Sie sie über einen bestimmten Zeitraum verarbeiten. Alle Daten werden auf dem Server parallel verarbeitet. Nach Abschluss des Vorgangs kann das vollständige Resultset heruntergeladen werden.
- [**Mobilitätsdienste:** ](https://docs.microsoft.com/rest/api/maps/mobility) Berechnen Sie Routen und Wegbeschreibungen mit dem öffentlichen Nahverkehr.

In der folgenden Tabelle werden die Parameter der Google Maps-API den vergleichbaren API-Parametern in Azure Maps gegenübergestellt.

| Google Maps-API-Parameter    | Vergleichbarer Azure Maps-API-Parameter  |
|------------------------------|--------------------------------------|
| `alternatives`                 | `maxAlternatives`                  |
| `arrival_time`                | `arriveAt`                          |
| `avoid`                        | `avoid`                            |
| `departure_time`              | `departAt`                          |
| `destination`                  | `query`: Koordinaten im Format `"lat0,lon0:lat1,lon1…."`  |
| `key`                          | `subscription-key`: Weitere Informationen finden Sie auch in der Dokumentation zur [Authentifizierung mit Azure Maps](azure-maps-authentication.md). |
| `language`                     | `language`: Lesen Sie auch die Dokumentation zu den [unterstützten Sprachen](supported-languages.md).   |
| `mode`                         | `travelMode`                       |
| `optimize`                     | `computeBestOrder`                 |
| `origin`                       | `query`                            |
| `region`                       | *Nicht verfügbar:* Dies ist ein Feature der Geocodierung. Verwenden Sie den Parameter *countrySet*, wenn Sie die Geocodierung-API von Azure Maps verwenden.  |
| `traffic_model`               | *Nicht verfügbar:* Es kann nur angegeben werden, ob Verkehrsinfos mit dem Parameter *traffic* verwendet werden sollen. |
| `transit_mode`                | Weitere Informationen finden Sie in der [Dokumentation zu den Mobilitätsdiensten](https://docs.microsoft.com/rest/api/maps/mobility). |
| `transit_routing_preference` | Weitere Informationen finden Sie in der [Dokumentation zu den Mobilitätsdiensten](https://docs.microsoft.com/rest/api/maps/mobility). |
| `units`                        | *Nicht verfügbar:* Bei Azure Maps wird ausschließlich das metrische System verwendet.  |
| `waypoints`                    | `query`                            |

> [!TIP]
> Standardmäßig gibt die Routen-API von Azure Maps nur eine Zusammenfassung (Entfernung und Zeiten) sowie die Koordinaten für die Route zurück. Verwenden Sie den Parameter `instructionsType`, um ausführliche Anweisungen abzurufen. Mit dem Parameter `routeRepresentation` können die Zusammenfassung und die Route herausgefiltert werden.

Die Routenplanungs-API von Azure Maps bietet viele zusätzliche Funktionen, die in Google Maps nicht verfügbar sind und bei der Migration Ihrer App hilfreich sein können:

- Unterstützung für den Routentyp: kürzeste, schnellste, attraktivste und kraftstoffeffizienteste
- Unterstützung für zusätzliche Reisemodi: Bus, Motorrad, Taxi, Lkw und Kleinbus
- Unterstützung für 150 Wegpunkte
- Berechnung mehrerer Fahrzeiten in einer einzelnen Anforderung: bisheriger Verkehr, aktueller Verkehr, kein Verkehr
- Vermeidung zusätzlicher Straßentypen: Straßen von Fahrgemeinschaften, ungepflasterte Straßen, bereits verwendete Straßen
- Eigene Angabe zu vermeidender Bereiche
- Begrenzung der Höhenanstiegs für die Route
- Routenplanung nach Motorisierung: Berechnen Sie Routen für Verbrennungs- oder Elektrofahrzeuge basierend auf den Angaben zum verbleibenden Kraftstoff bzw. der verbleibenden Laufzeit.
- Unterstützung für Routenparameter kommerzieller Fahrzeuge: Fahrzeugmaße, Gewicht, Anzahl der Achsen und Art der Ladung
- Angabe der maximale Fahrzeuggeschwindigkeit

Außerdem unterstützt der Routenplanungsdienst von Azure Maps das [Berechnen von Bereichen für Routen](https://docs.microsoft.com/rest/api/maps/route/getrouterange), die auch als Isochronen bezeichnet werden. Sie generieren ein Polygon über einem Bereich, der aus beliebiger Richtung von einem Ausgangspunkt innerhalb eines festgelegten Zeitraums oder mit einer angegebenen Menge an Treibstoff bzw. Ladung zu erreichen ist.

## <a name="retrieve-a-map-image"></a>Abrufen eines Kartenbilds

Azure Maps stellt eine API für das Rendern der statischen Kartenbilder mit überlagerten Daten bereit. Die [Kartenbildrendering](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytile)-API von Azure Maps ist mit der Google Maps-API für statische Karten vergleichbar.

> [!NOTE]
> Für Azure Maps müssen der Mittelpunkt sowie alle Marker- und Pfadstandorte Koordinaten im Format „Längengrad,Breitengrad“ sein, während bei Google Maps das Format „Breitengrad,Längengrad“ verwendet wird. Adressen müssen zunächst geocodiert werden.

In der folgenden Tabelle werden die Parameter der Google Maps-API den vergleichbaren API-Parametern in Azure Maps gegenübergestellt.

| Google Maps-API-Parameter | Vergleichbarer Azure Maps-API-Parameter  |
|---------------------------|--------------------------------------|
| `center`                    | `center`                           |
| `format`                    | `format`: wird als Teil des URL-Pfads angegeben. Derzeit wird nur PNG unterstützt. |
| `key`                       | `subscription-key`: Weitere Informationen finden Sie auch in der Dokumentation zur [Authentifizierung mit Azure Maps](azure-maps-authentication.md). |
| `language`                  | `language`: Lesen Sie auch die Dokumentation zu den [unterstützten Sprachen](supported-languages.md).  |
| `maptype`                   | `layer` und `style`: Lesen Sie die Dokumentation zu den [unterstützten Kartenstilen](supported-map-styles.md). |
| `markers`                   | `pins`                             |
| `path`                      | `path`                             |
| `region`                    | *Nicht verfügbar:* Dies ist ein Feature der Geocodierung. Verwenden Sie den Parameter `countrySet`, wenn Sie die Geocodierung-API von Azure Maps verwenden.  |
| `scale`                     | *N/V*                              |
| `size`                      | `width` und `height`: Es sind Größen bis 8.192 × 8.192 möglich. |
| `style`                     | *N/V*                              |
| `visible`                   | *N/V*                              |
| `zoom`                      | `zoom`                             |

> [!NOTE]
> Azure Maps verwendet ein Kachelsystem mit Kacheln, die doppelt so groß sind wie die in Google Maps verwendeten Kartenkacheln. Aus diesem Grund ist der Wert der Zoomebene in Azure Maps im Vergleich zu Google Maps um eine Zoomebene niedriger. Verringern Sie die Zoomebene in den Anforderungen, die Sie migrieren, um eins, um dies zu kompensieren.

Weitere Informationen finden Sie in der [Schrittanleitung zur Rendering-API für Kartenbilder](how-to-render-custom-data.md).

Damit der Azure Maps-Renderingdienst in der Lage ist, ein statisches Kartenbild zu generieren, bietet er auch die Möglichkeit, direkt auf Kartenkacheln im Raster- (PNG) und im Vektorformat zuzugreifen:

- [**Kartenkachel:** ](https://docs.microsoft.com/rest/api/maps/render/getmaptile) Ruft Raster- (PNG) und Vektorkacheln für die Basiskarten (Straßen, Grenzen, Hintergrund) ab.
- [**Kachel für Kartenbilder:** ](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytile) Ruft Kacheln für Luft- und Satellitenbilder ab.

> [!TIP]
> Viele Google Maps-Anwendungen wurden vor einigen Jahren aus Kostengründen von interaktiven Kartenbildern auf statische Kartenbilder umgestellt. In Azure Maps ist es häufig kostengünstiger, das interaktive Kartensteuerelement im Web SDK zu verwenden, da die Abrechnung auf der Grundlage der Ladevorgänge für Kartenkachel erfolgt. Kartenkacheln in Azure Maps sind groß, und es dauert häufig nur kurze Zeit, dieselbe Kartenansicht wie bei einer statischen Karte erneut zu erstellen, da statische Karten und Kartenkacheln automatisch vom Browser zwischengespeichert werden. Daher generiert das interaktive Kartensteuerelement häufig nur einen Bruchteil einer Transaktion, wenn eine statische Kartenansicht erneut erstellt wird. Durch Schwenken und Zoomen werden weitere Kacheln geladen, es gibt jedoch Optionen im Kartensteuerelement, mit denen dieses Verhalten bei Bedarf deaktiviert werden kann. Das interaktive Kartensteuerelement bietet auch viel mehr Visualisierungsoptionen als statische Kartendienste.

### <a name="marker-url-parameter-format-comparison"></a>Vergleich der Formate von Marker-URL-Parametern

**Vorher: Google Maps**

In Google Maps können Marker einem statischen Kartenbild mithilfe des Parameters `markers` in der URL hinzugefügt werden. Der Parameter `markers` übernimmt einen Stil und eine Liste von Positionen, die auf der Karte mit diesem Stil gerendert werden sollen, wie im Folgenden dargestellt:

```
&markers=markerStyles|markerLocation1|markerLocation2|...
```

Weitere Stile können verwendet werden, indem der URL zusätzliche `markers`-Parameter mit einem anderen Stil und einem anderen Satz von Positionen hinzugefügt werden.

Markerpositionen werden im Format „Breitengrad,Längengrad“ angegeben.

In Google Maps werden Markerstile im Format `optionName:value` hinzugefügt, wobei mehrere Stile durch einen senkrechten Strich (\|) voneinander getrennt werden, z. B. „Optionsname1:Wert1\|Optionsname2: Wert2“. Beachten Sie, dass die Optionsnamen und -werte durch einen Doppelpunkt (:) getrennt sind. Die folgenden Stiloptionsnamen können für den Stil von Markern in Google Maps verwendet werden:

- `color`: die Farbe des Standardmarkersymbols. Dabei kann es sich um eine hexadezimale 24-Bit-Farbe (`0xrrggbb`) oder einen der folgenden Werte handeln: `black`, `brown`, `green`, `purple`, `yellow`, `blue`, `gray`, `orange`, `red`, `white`.
- `label`: ein einzelnes alphanumerisches Zeichen in Großschreibung, das oberhalb des Symbols angezeigt wird.
- `size`: die Größe des Markers. Kann `tiny`, `mid` oder `small` sein.

Benutzerdefinierte Symbole können ebenfalls in Google Maps mithilfe der folgenden Stiloptionsnamen verwendet werden:

- `anchor`: gibt an, wie das Symbolbild an der Koordinate ausgerichtet wird. Es kann sich um einen Pixelwert (x,y) oder einen der folgenden Werte handeln: `top`, `bottom`, `left`, `right`, `center`, `topleft`, `topright`, `bottomleft` oder `bottomright`.
- `icon`: eine URL, die auf das Symbolbild verweist.

In Google Maps kann z. B. mit dem folgenden URL-Parameter ein roter, mittelgroßer Marker der Karte an den Koordinaten (Längengrad: –110, Breitengrad: 45) hinzugefügt werden:

```
&markers=color:red|size:mid|45,-110
```

<center>

![Google Maps-Marker](media/migrate-google-maps-web-services/google-maps-marker.png)</center>

**Nachher: Azure Maps**

In Azure Maps können Marker ebenfalls einem statischen Kartenbild durch Angabe des Parameters `pins` in der URL hinzugefügt werden. Wie bei Google Maps können auch ein Stil und eine Liste mit Positionen in diesem Parameter angegeben werden, und der Parameter `pins` kann mehrmals angegeben werden, um Marker mit unterschiedlichen Stilen zu unterstützen.

```
&pins=iconType|pinStyles||pinLocation1|pinLocation2|...
```

Weitere Stile können verwendet werden, indem der URL zusätzliche `pins`-Parameter mit einem anderen Stil und einem anderen Satz von Positionen hinzugefügt werden.

Für Pins erfordert Azure Maps Koordinaten im Format „Längengrad Breitengrad“ sein, während bei Google Maps das Format „Breitengrad,Längengrad“ verwendet wird. Beachten Sie auch, dass in Azure Maps ein Leerzeichen anstelle eines Kommas als Trennzeichen zwischen Längen- und Breitengrad verwendet wird.

Der Wert `iconType` gibt den Typ der zu erstellenden Pins an. Er kann die folgenden Werte aufweisen:

- `default`: das Standardsymbol für Pins.
- `none`: Es wird kein Symbol angezeigt, sondern nur Bezeichnungen gerendert.
- `custom`: gibt an, dass ein benutzerdefiniertes Symbol verwendet werden soll. Eine URL, die auf das Symbolbild verweist, kann am Ende des Parameters `pins` nach den Informationen zur Pinposition hinzugefügt werden.
- `{udid}`: eine Unique Data ID (UDID) für ein Symbol, das in der Azure Maps-Datenspeicher-Plattform gespeichert ist.

In Azure Maps werden Pinstile im Format `optionNameValue` hinzugefügt, wobei mehrere Stile durch einen senkrechten Strich (\|) voneinander getrennt werden, z. B. `iconType|optionName1Value1|optionName2Value2`. Beachten Sie, dass die Optionsnamen und -werte nicht voneinander getrennt werden. Die folgenden Stiloptionsnamen können für den Stil von Markern in Azure Maps verwendet werden:

- `al`: gibt die Deckkraft (Alpha) des Markers an. Dies kann eine Zahl zwischen 0 und 1 sein.
- `an`: gibt den Pinanker an. Die x- und y-Pixelwerte werden im Format „x y“ angegeben.
- `co`: die Farbe des Pins. Dabei muss es sich um eine hexadezimale 24-Bit-Farbe handeln: `000000` in `FFFFFF`.
- `la`: gibt den Bezeichnungsanker an. Die x- und y-Pixelwerte werden im Format „x y“ angegeben.
- `lc`: die Farbe der Bezeichnung. Dabei muss es sich um eine hexadezimale 24-Bit-Farbe handeln: `000000` in `FFFFFF`.
- `ls`: die Größe der Bezeichnung in Pixel. Kann eine Zahl größer als 0 sein.
- `ro`: ein Wert in Grad, um den das Symbol gedreht werden soll. Dies kann eine Zahl zwischen –360 und 360 sein.
- `sc`: ein Skalierungswert für das Pinsymbol. Kann eine Zahl größer als 0 sein.

Bezeichnungswerte werden für jede Pinposition angegeben, anstatt einen einzelnen Bezeichnungswert zu verwenden, der für alle Marker in der Liste der Positionen gilt. Der Bezeichnungswert kann eine Zeichenfolge mit mehreren Zeichen sein und in einfache Anführungszeichen eingeschlossen werden, um sicherzustellen, dass er nicht als Stil- oder Positionswert interpretiert wird.

In Azure Maps kann z. B. ein rotes (`FF0000`) Standardsymbol mit der Bezeichnung „Space Needle“ unter dem Symbol (15 50) an den Koordinaten (Längengrad: –122,349300, Breitengrad: 47,620180) mithilfe des folgenden URL-Parameters hinzugefügt werden:

```
&pins=default|coFF0000|la15 50||'Space Needle' -122.349300 47.620180
```

<center>

![Azure Maps-Marker](media/migrate-google-maps-web-services/azure-maps-marker.png)</center>

Im folgenden Beispiel werden drei Pins mit den Bezeichnungswerten „1“, „2“ und „3“ hinzugefügt:

```
&pins=default||'1'-122 45|'2'-119.5 43.2|'3'-121.67 47.12
```

<center>

![Mehrere Azure Maps-Marker](media/migrate-google-maps-web-services/azure-maps-multiple-markers.png)</center>

### <a name="path-url-parameter-format-comparison"></a>Vergleich der Formate von Pfad-URL-Parametern

**Vorher: Google Maps**

In Google Maps können Linien und Polygone einem statischen Kartenbild mithilfe des Parameters `path` in der URL hinzugefügt werden. Der Parameter `path` übernimmt einen Stil und eine Liste von Positionen, die auf der Karte mit diesem Stil gerendert werden sollen, wie im Folgenden dargestellt:

```
&path=pathStyles|pathLocation1|pathLocation2|...
```

Weitere Stile können verwendet werden, indem der URL zusätzliche `path`-Parameter mit einem anderen Stil und einem anderen Satz von Positionen hinzugefügt werden.

Pfadpositionen werden in Google Maps im Format `latitude1,longitude1|latitude2,longitude2|…`angegeben. Pfade können codiert werden oder Adressen für Punkte enthalten.

In Google Maps werden Pfadstile im Format `optionName:value` hinzugefügt, wobei mehrere Stile durch einen senkrechten Strich (\|) voneinander getrennt werden, z. B. `optionName1:value1|optionName2:value2`. Beachten Sie, dass die Optionsnamen und -werte durch einen Doppelpunkt (:) getrennt sind. Die folgenden Stiloptionsnamen können für den Stil von Pfaden in Google Maps verwendet werden:

- `color`: die Farbe der Pfad- oder Polygonumrandung. Dabei kann es sich um eine hexadezimale 24-Bit-Farbe (`0xrrggbb`), eine hexadezimale 32-Bit-Farbe (`0xrrggbbbaa`) oder einen der folgenden Werte handeln: „black“, „brown“, „green“, „purple“, „yellow“, „blue“, „gray“, „orange“, „red“, „white“.
- `fillColor`: die Farbe zum Ausfüllen des Pfadbereichs (Polygon). Dabei kann es sich um eine hexadezimale 24-Bit-Farbe (`0xrrggbb`), eine hexadezimale 32-Bit-Farbe (`0xrrggbbbaa`) oder einen der folgenden Werte handeln: „black“, „brown“, „green“, „purple“, „yellow“, „blue“, „gray“, „orange“, „red“, „white“.
- `geodesic`: gibt an, ob der Pfad eine Linie sein soll, die der Erdkrümmung folgt.
- `weight`: die Stärke der Pfadlinie in Pixel.

In Google Maps kann z. B. eine rote Linie mit einer Deckkraft von 50 % und eine Stärke von 4 Pixeln der Karte zwischen den Koordinaten (Längengrad: –110, Breitengrad: 45 und Längengrad: –100, Breitengrad: 50) mithilfe des folgenden URL-Parameters hinzugefügt werden:

```
&path=color:0xFF000088|weight:4|45,-110|50,-100
```

<center>

![Google Maps-Polylinie](media/migrate-google-maps-web-services/google-maps-polyline.png)</center>

**Nachher: Azure Maps**

In Azure Maps können Linien und Polygone ebenfalls einem statischen Kartenbild durch Angabe des Parameters `path` in der URL hinzugefügt werden. Wie bei Google Maps können auch ein Stil und eine Liste mit Positionen in diesem Parameter angegeben werden, und der Parameter `path` kann mehrmals angegeben werden, um mehrere Kreise, Linien und Polygone mit unterschiedlichen Stilen zu rendern.

```
&path=pathStyles||pathLocation1|pathLocation2|...
```

Für Pfadpositionen erfordert Azure Maps Koordinaten im Format „Längengrad Breitengrad“ sein, während bei Google Maps das Format „Breitengrad,Längengrad“ verwendet wird. Beachten Sie auch, dass in Azure Maps ein Leerzeichen anstelle eines Kommas als Trennzeichen zwischen Längen- und Breitengrad verwendet wird. Azure Maps unterstützt keine codierten Pfade oder Adressen für Punkte. Größere Datasets können als GeoJSON-Dateien in die Azure Maps-Datenspeicher-API hochgeladen werden, wie [hier](how-to-render-custom-data.md#get-data-from-azure-maps-data-storage) beschrieben wird.

In Azure Maps werden Pfadstile im Format `optionNameValue` hinzugefügt, wobei mehrere Stile durch einen senkrechten Strich (\|) voneinander getrennt werden, z. B. `optionName1Value1|optionName2Value2`. Beachten Sie, dass die Optionsnamen und -werte nicht voneinander getrennt werden. Die folgenden Stiloptionsnamen können für den Stil von Pfaden in Azure Maps verwendet werden:

- `fa`: die Deckkraft der Füllfarbe (Alpha), die beim Rendern von Polygonen verwendet wird. Dies kann eine Zahl zwischen 0 und 1 sein.
- `fc`: die Füllfarbe, die zum Rendern des Bereichs eines Polygons verwendet wird.
- `la`: die Deckkraft der Linienfarbe (Alpha), die zum Rendern von Linien und der Umrandung von Polygonen verwendet wird. Dies kann eine Zahl zwischen 0 und 1 sein.
- `lc`: die Linienfarbe, die zum Rendern von Linien und der Umrandung von Polygonen verwendet wird.
- `lw`: die Breite der Linie in Pixel.
- `ra`: gibt einen Kreisradius in Metern an.

In Azure Maps kann z. B. eine rote Linie mit einer Deckkraft von 50 % und eine Stärke von 4 Pixeln der Karte zwischen den Koordinaten (Längengrad: -110, Breitengrad: 45 und Längengrad: –100, Breitengrad: 50) mithilfe des folgenden URL-Parameters hinzugefügt werden:

```
&path=lcFF0000|la.5|lw4||-110 45|-100 50
```

<center>

![Azure Maps-Polylinie](media/migrate-google-maps-web-services/azure-maps-polyline.png)</center>

## <a name="calculate-a-distance-matrix"></a>Berechnen einer Entfernungsmatrix

Azure Maps bietet eine API zum Berechnen der Fahrzeiten und Entfernungen zwischen einer Reihe von Positionen als Entfernungsmatrix. Die Azure Maps-Entfernungsmatrix-API ist vergleichbar mit der Entfernungsmatrix-API von Google Maps.

- [**Routenmatrix:** ](https://docs.microsoft.com/rest/api/maps/route/postroutematrixpreview) Berechnet asynchron die Reisezeiten und Entfernungen für einen Satz von Ausgangspunkten und Zielen. Es werden bis zu 700 Zellen pro Anforderung unterstützt (die Anzahl der Ausgangspunkte multipliziert mit der Anzahl der Ziele). Mit dieser Einschränkung sind beispielsweise folgende Matrixabmessungen möglich: 700 × 1, 50 × 10, 10 × 10, 28 × 25, 10 × 70.

> [!NOTE]
> Eine Anforderung an die Entfernungsmatrix-API kann nur mithilfe einer POST-Anforderung erfolgen, die Informationen zum Ausgangs- und Zielpunkt im Anforderungstext enthält. Außerdem erfordert Azure Maps, dass alle Ausgangspunkte und Ziele als Koordinaten angegeben werden. Adressen müssen zunächst geocodiert werden.

In der folgenden Tabelle werden die Parameter der Google Maps-API den vergleichbaren API-Parametern in Azure Maps gegenübergestellt.

| Google Maps-API-Parameter      | Vergleichbarer Azure Maps-API-Parameter  |
|--------------------------------|--------------------------------------|
| `arrivial_time`                | `arriveAt`                           |
| `avoid`                        | `avoid`                              |
| `depature_time`                | `departAt`                           |
| `destinations`                 | `destination`: wird im POST-Anforderungstext als GeoJSON angegeben. |
| `key`                          | `subscription-key`: Weitere Informationen finden Sie auch in der Dokumentation zur [Authentifizierung mit Azure Maps](azure-maps-authentication.md). |
| `language`                     | `language`: Lesen Sie auch die Dokumentation zu den [unterstützten Sprachen](supported-languages.md).  |
| `mode`                         | `travelMode`                         |
| `origins`                      | `origins`: wird im POST-Anforderungstext als GeoJSON angegeben.  |
| `region`                       | *Nicht verfügbar:* Dies ist ein Feature der Geocodierung. Verwenden Sie den Parameter `countrySet`, wenn Sie die Geocodierung-API von Azure Maps verwenden. |
| `traffic_model`                | *Nicht verfügbar:* Es kann nur angegeben werden, ob Verkehrsinfos mit dem Parameter `traffic` verwendet werden sollen. |
| `transit_mode`                 | *Nicht verfügbar:* Verkehrsbasierte Entfernungsmatrizen werden zurzeit nicht unterstützt.  |
| `transit_routing_preference`   | *Nicht verfügbar:* Verkehrsbasierte Entfernungsmatrizen werden zurzeit nicht unterstützt.  |
| `units`                        | *Nicht verfügbar:* Bei Azure Maps wird ausschließlich das metrische System verwendet. |

> [!TIP]
> Alle erweiterten Routenplanungsoptionen, die in der Azure Maps-Routenplanungs-API verfügbar sind (Lkw-Routenplanung, Angaben zur Motorisierung, zu vermeidende Straßen, …), werden in der Azure Maps-Entfernungsmatrix-API unterstützt.

## <a name="get-a-time-zone"></a>Abrufen einer Zeitzone

Azure Maps bietet eine API zum Abrufen der Zeitzone, in der sich eine Koordinate befindet. Die Azure Maps-Zeitzonen-API ist mit der Zeitzonen-API von Google Maps vergleichbar:

- [**Zeitzone nach Koordinate:** ](https://docs.microsoft.com/rest/api/maps/timezone/gettimezonebycoordinates) Wenn Sie eine Koordinate angeben, erhalten Sie die Details zu der Zeitzone, in der sie liegt.

In der folgenden Tabelle werden die Parameter der Google Maps-API den vergleichbaren API-Parametern in Azure Maps gegenübergestellt.

| Google Maps-API-Parameter | Vergleichbarer Azure Maps-API-Parameter   |
|---------------------------|---------------------------------------|
| `key`                       | `subscription-key`: Weitere Informationen finden Sie auch in der Dokumentation zur [Authentifizierung mit Azure Maps](azure-maps-authentication.md).       |
| `language`                  | `language`: Lesen Sie auch die Dokumentation zu den [unterstützten Sprachen](supported-languages.md).    |
| `location`                  | `query`             |
| `timestamp`                 | `timeStamp`         |

Darüber hinaus bietet die Azure Maps-Plattform auch eine Reihe zusätzlicher Zeitzonen-APIs, um Konvertierungen mit Zeitzonennamen und -IDs zu unterstützen:

- [**Zeitzone nach ID:** ](https://docs.microsoft.com/rest/api/maps/timezone/gettimezonebyid) Gibt aktuelle, historische und zukünftige Zeitzoneninformationen für die angegebene IANA-Zeitzonen-ID zurück.
- [**Zeitzonenenumeration für IANA:** ](https://docs.microsoft.com/rest/api/maps/timezone/gettimezoneenumiana) Gibt eine vollständige Liste der IANA-Zeitzonen-IDs zurück. Updates für den IANA-Dienst werden innerhalb eines Tages im System übernommen.
- [**Zeitzonenenumeration für Windows:** ](https://docs.microsoft.com/rest/api/maps/timezone/gettimezoneenumwindows) Gibt eine vollständige Liste der Windows-Zeitzonen-IDs zurück.
- [**IANA-Version der Zeitzone:** ](https://docs.microsoft.com/rest/api/maps/timezone/gettimezoneianaversion) Gibt die aktuelle IANA-Versionsnummer zurück, die von Azure Maps verwendet wird.
- [**Zeitzone: Windows nach IANA:** ](https://docs.microsoft.com/rest/api/maps/timezone/gettimezonewindowstoiana) Gibt die entsprechende IANA-ID für eine angegebene gültige Windows-Zeitzonen-ID zurück. Es können mehrere IANA-IDs eine einzelne Windows-ID zurückgegeben werden.

## <a name="client-libraries"></a>Clientbibliotheken

Azure Maps stellt Clientbibliotheken für die folgenden Programmiersprachen bereit:

- JavaScript, TypeScript, Node.js: [Dokumentation](how-to-use-services-module.md) \| [NPM-Paket](https://www.npmjs.com/package/azure-maps-rest)

Open-Source-Clientbibliotheken für andere Programmiersprachen:

- .NET Standard 2.0: [GitHub-Projekt](https://github.com/perfahlen/AzureMapsRestServices) \| [NuGet-Paket](https://www.nuget.org/packages/AzureMapsRestToolkit/)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Im Folgenden finden Sie zusätzliche Dokumentation und weitere Ressourcen zu den Azure Maps-REST-Diensten.

- [Bewährte Suchmethoden](how-to-use-best-practices-for-search.md)
- [Suchen nach einer Adresse](how-to-search-for-address.md)
- [Azure Maps-REST-Dienste-API – Referenzdokumentation](https://docs.microsoft.com/rest/api/maps/)

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Azure Maps-REST-Dienste:

> [!div class="nextstepaction"]
> [Bewährte Methoden für die Verwendung des Suchdiensts](how-to-use-best-practices-for-search.md)

> [!div class="nextstepaction"]
> [Verwenden des Dienstmoduls (Web SDK)](how-to-use-services-module.md)

> [!div class="nextstepaction"]
> [Codebeispiele](https://docs.microsoft.com/samples/browse/?products=azure-maps)