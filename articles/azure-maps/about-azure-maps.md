---
title: Übersicht für Microsoft Azure Maps
description: Hier finden Sie Informationen zu Diensten und Funktionen in Microsoft Azure Maps sowie zur Verwendung in Ihren Anwendungen.
author: anastasia-ms
ms.author: v-stharr
ms.date: 07/31/2020
ms.topic: overview
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc, references_regions
ms.openlocfilehash: 11a0ddc5c7b297d0700e6fd07d60f8efe0e55a8f
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91285331"
---
# <a name="what-is-azure-maps"></a>Was ist Azure Maps?

Azure Maps ist eine Sammlung von Geodiensten und SDKs, bei denen aktuelle Kartendaten verwendet werden, um einen geografischen Kontext für Webanwendungen und mobile Anwendungen bereitzustellen. Azure Maps umfasst Folgendes:

* REST-APIs zum Rendern von Vektor- und Rasterkarten in verschiedenen Stilen und Satellitenbildern
* Erstellerdienste zum Erstellen und Rendern von Karten auf der Grundlage privater Gebäudeplandaten.
* Suchdienste für die Suche nach Adressen, Orten und Points of Interest auf der ganzen Welt
* Verschiedene Routenplanungsoptionen – beispielsweise von A nach B, mehrere Punkte, Optimierung bei Verwendung mehrerer Punkte, Isochrone, Elektrofahrzeuge, gewerbliche Fahrzeuge, Berücksichtigung der Verkehrslage und Matrixroutenplanung
* Anzeige von Verkehrsfluss und -störungen für Anwendungen, die Verkehrsinformationen in Echtzeit benötigen
* Mobilitätsdienst zum Anfordern von Informationen zum öffentlichen Nahverkehr und Planen von Routen durch Kombinieren verschiedener Transportmittel und Ankunftszeiten in Echtzeit.
* Zeitzonen- und Geolocationdienste.
* Geofencingdienst und Speicher für Kartendaten mit in Azure gehosteten Standortinformationen
* Location Intelligence mittels Geoanalysen

Darüber hinaus sind Azure Maps-Dienste über das Web SDK und das Android SDK verfügbar. Mit diesen Tools können Entwickler im Handumdrehen Lösungen entwickeln und skalieren, die Standortinformationen in Azure-Lösungen integrieren.

Sie können sich für ein kostenloses [Azure Maps-Konto](https://azure.microsoft.com/services/azure-maps/) registrieren und direkt loslegen.

Im folgenden Video wird Azure Maps ausführlich beschrieben:

<br/>

<iframe src="https://channel9.msdn.com/Shows/Internet-of-Things-Show/Azure-Maps/player?format=ny" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="map-controls"></a>Kartensteuerelemente

### <a name="web-sdk"></a>Web-SDK

Mit dem Azure Maps Web SDK können Sie interaktive Karten mit eigenen Inhalten und Bildern anpassen. Diese interaktive Karte können Sie sowohl für Ihre Webanwendungen als auch für Ihre mobilen Anwendungen verwenden. Das Kartensteuerelement nutzt WebGL, sodass Sie umfangreiche Datasets mit hoher Leistung rendern können. Für die Entwicklung mit dem SDK können Sie JavaScript oder TypeScript verwenden.

:::image type="content" source="./media/about-azure-maps/intro_web_map_control.png" alt-text="Beispielkarte zur Bevölkerungsentwicklung, erstellt mit dem Azure Maps Web SDK":::

### <a name="android-sdk"></a>Android-SDK

Mit dem Azure Maps Android SDK können Sie mobile Kartenanwendungen erstellen.

:::image type="content" source="./media/about-azure-maps/android_sdk.png" border="false" alt-text="Beispielkarte zur Bevölkerungsentwicklung, erstellt mit dem Azure Maps Web SDK":::

## <a name="services-in-azure-maps"></a>Dienste in Azure Maps

Azure Maps besteht aus den folgenden Diensten, die für Ihre Azure-Anwendungen geografischen Kontext liefern können.

### <a name="data-service"></a>Datendienst

Daten sind für Karten unverzichtbar. Mit dem Datendienst können Sie räumliche Daten zur Verwendung mit räumlichen Vorgängen oder mit der Bildkomposition hochladen und speichern.  Durch die Einbindung von Kundendaten in den Azure Maps-Dienst verringern Sie die Wartezeit und steigern die Produktivität und können neue Szenarien in Ihren Anwendungen erstellen. Ausführliche Informationen zu diesem Dienst finden Sie in der [Dokumentation zum Datendienst](https://docs.microsoft.com/rest/api/maps/data).

### <a name="geolocation-service"></a>Geolocation-Dienst

Mit dem Geolocationdienst können Sie eine Vorschau für den abgerufenen zweistelligen Länder-/Regionscode für eine IP-Adresse erstellen. Mithilfe dieses Diensts können Sie die Benutzeroberfläche verbessern, indem Sie angepasste Anwendungsinhalte auf der Grundlage des geografischen Standorts bereitstellen.

Weitere Informationen finden Sie in der [Dokumentation zum Geolocationdienst](https://docs.microsoft.com/rest/api/maps/geolocation).

### <a name="mobility-service"></a>Mobilitätsdienst

Der Mobilitätsdienst für Azure Maps verbessert die Entwicklungszeit für Anwendungen mit Features für den öffentlichen Nahverkehr, z. B. Verkehrsroutenplanung und Suche nach nahe gelegenen Haltestellen des öffentlichen Nahverkehrs. Benutzer können ausführliche Informationen zu Haltestellen, Linien und Fahrplänen abrufen. Der Mobilitätsdienst ermöglicht Benutzern außerdem das Abrufen von Geometrien für Haltestellen und Fahrpläne, Warnungen für Haltestellen, Linien und Betriebsbereiche sowie Ankunfts- und Betriebsbenachrichtigungen in Echtzeit für den öffentlichen Nahverkehr. Darüber hinaus bietet der Mobilitätsdienst Routenplanungsfunktionen mit Optionen für eine multimodale Routenplanung. Bei der multimodalen Routenplanung werden Optionen für Fußgänger und Fahrradfahrer sowie öffentliche Verkehrsmittel für eine Strecke berücksichtigt. Benutzer können auf detaillierte und multimodale ausführliche Routen zugreifen.

Weitere Informationen zu diesem Dienst finden Sie in der [Dokumentation zum Mobilitätsdienst](https://docs.microsoft.com/rest/api/maps/mobility).

### <a name="render-service"></a>Renderingdienst

Mit dem [Renderingdienst V2](https://docs.microsoft.com/rest/api/maps/renderv2), der sich jetzt in der Vorschauphase befindet, wird eine neue Version der [Get Map Tile V2-API](https://docs.microsoft.com/rest/api/maps/renderv2/getmaptilepreview) eingeführt. Mit der Get Map Tile V2-API können Kunden nun Azure Maps-Straßenkacheln, Wetterkacheln oder die mit Azure Maps Creator erstellten Kartenkacheln anfordern. Die Verwendung der neuen Get Map Tile V2-API wird empfohlen.  

:::image type="content" source="./media/about-azure-maps/intro_map.png" border="false" alt-text="Beispielkarte zur Bevölkerungsentwicklung, erstellt mit dem Azure Maps Web SDK":::

Weitere Informationen finden Sie in der [Dokumentation zum Renderingdienst V2](https://docs.microsoft.com/rest/api/maps/renderv2).

Weitere Informationen zum Renderingdienst V1, der allgemein verfügbar ist (GA), finden Sie in der [Dokumentation zum Renderingdienst V1](https://docs.microsoft.com/rest/api/maps/render).  

### <a name="route-service"></a>Routendienst

Die Routendienste können verwendet werden, um die geschätzten Ankunftszeiten (Estimated Arrival Times, ETAs) für jede angeforderte Wegstrecke zu berechnen. Die Wegbeschreibungs-APIs berücksichtigen Faktoren wie Echtzeit-Verkehrsinformationen und Verkehrsdaten aus der Vergangenheit, wie etwa die typischen Reisegeschwindigkeiten am angeforderten Wochentag zur angeforderten Tageszeit. Die APIs geben die kürzesten oder schnellsten verfügbaren Routen zu mehreren Zielen zugleich in der Reihenfolge der Eingabe oder in einer für Zeit oder Entfernung optimierten Reihenfolge zurück. Entwickler können mithilfe des Diensts Wegbeschreibungen für mehrere Fortbewegungsmittel (z. B. PKW, LKW, Fahrrad, zu Fuß oder Elektrofahrzeug) generieren. Darüber hinaus berücksichtigt der Dienst Faktoren wie Abfahrtszeit, Gewichtsbeschränkungen oder Transport von Gefahrgütern.

:::image type="content" source="./media/about-azure-maps/intro_route.png" border="false" alt-text="Beispielkarte zur Bevölkerungsentwicklung, erstellt mit dem Azure Maps Web SDK":::

Der Routendienst bietet erweiterte Gruppenfunktionen, beispielsweise:

* Batchverarbeitung für mehrere Routenanforderungen
* Matrizen für Fahrzeit und Entfernung zwischen verschiedenen Ausgangsorten und Zielen
* Ermittlung von Routen oder Entfernungen, die Benutzer basierend auf Zeit- oder Kraftstoffanforderungen zurücklegen können

Ausführliche Informationen zu den Routingfunktionen finden Sie in der [Dokumentation zum Routendienst](https://docs.microsoft.com/rest/api/maps/route).

### <a name="search-service"></a>Suchdienst

Mit dem Suchdienst können Entwickler nach Adressen, Orten, Geschäftseinträgen nach Name oder Kategorie und anderen geografischen Informationen suchen. Mit Diensten kann auch eine [inverse Geocodierung](https://en.wikipedia.org/wiki/Reverse_geocoding) für Adressen und Querstraßen basierend auf Breiten- und Längengraden durchgeführt werden.

:::image type="content" source="./media/about-azure-maps/intro_search.png" border="false" alt-text="Beispielkarte zur Bevölkerungsentwicklung, erstellt mit dem Azure Maps Web SDK":::

Der Suchdienst umfasst außerdem erweiterte Funktionen, beispielsweise:

* Suche entlang einer Route
* Suche innerhalb eines weiteren Bereichs
* Zusammenfassen einer Gruppe von Suchanfragen zu einem Batch
* Suchen von Ladestationen für Elektrofahrzeuge und Point of Interest-Daten (POI) nach Markennamen.

Weitere Informationen zu den Suchfunktionen finden Sie in der [Dokumentation zum Suchdienst](https://docs.microsoft.com/rest/api/maps/search).

### <a name="spatial-service"></a>Räumlicher Dienst

Der räumliche Dienst analysiert schnell Standortinformationen, um Kunden über aktuelle Ereignisse zu informieren. Er ermöglicht die Analyse- und Vorhersagemodellierung von Ereignissen nahezu in Echtzeit.

Mithilfe des Diensts können Kunden ihre Location Intelligence mit einer Bibliothek aus gängigen räumlichen Berechnungen optimieren. Zu diesen gängigen Berechnungen zählen beispielsweise nächstgelegener Punkt, Großkreisabstand und Puffer. Weitere Informationen zum Dienst und zu den verschiedenen Funktionen finden Sie in der [Dokumentation zum räumlichen Dienst](https://docs.microsoft.com/rest/api/maps/spatial).

### <a name="timezone-service"></a>Zeitzonendienst

Mit dem Zeitzonendienst können Sie aktuelle, vergangene und zukünftige Zeitzoneninformationen abfragen. Hierzu können Sie entweder Breitengrad-Längengrad-Paare oder eine [IANA-ID](https://www.iana.org/) als Eingabe verwenden. Mit dem Zeitzonendienst ist außerdem Folgendes möglich:

* Konvertieren von Microsoft Windows-Zeitzonen-IDs in IANA-Zeitzonen
* Abrufen eines Zeitzonenoffsets zu UTC
* Abrufen der aktuellen Zeit einer bestimmten Zeitzone

Eine typische JSON-Antwort für eine Abfrage mit dem Zeitzonendienst sieht wie folgt aus:

```JSON
{
  "Version": "2020a",
  "ReferenceUtcTimestamp": "2020-07-31T19:15:14.4570053Z",
  "TimeZones": [
    {
      "Id": "America/Los_Angeles",
      "Names": {
        "ISO6391LanguageCode": "en",
        "Generic": "Pacific Time",
        "Standard": "Pacific Standard Time",
        "Daylight": "Pacific Daylight Time"
      },
      "ReferenceTime": {
        "Tag": "PDT",
        "StandardOffset": "-08:00:00",
        "DaylightSavings": "01:00:00",
        "WallTime": "2020-07-31T12:15:14.4570053-07:00",
        "PosixTzValidYear": 2020,
        "PosixTz": "PST+8PDT,M3.2.0,M11.1.0"
      }
    }
  ]
}
```

Ausführliche Informationen zu diesem Dienst finden Sie in der [Dokumentation zum Zeitzonendienst](https://docs.microsoft.com/rest/api/maps/timezone).

### <a name="traffic-service"></a>Verkehrsinfodienst

Der Verkehrsinfodienst ist eine Suite von Webdiensten, mit denen Entwickler webbasierte und mobile Anwendungen erstellen können, die Verkehrsinformationen benötigen. Der Dienst stellt zwei Typen von Daten bereit:

* Verkehrsfluss: In Echtzeit ermittelte Geschwindigkeiten und Reisezeiten für alle wichtigen Straßen des Verkehrsnetzes
* Verkehrsmeldungen: Aktuelle Übersicht über die Staus und Störungen bzw. Unfälle im Straßennetz

![Beispiel für eine Karte mit Verkehrsinformationen](media/about-azure-maps/intro_traffic.png)

Weitere Informationen finden Sie in der [Dokumentation zum Verkehrsinfodienst](https://docs.microsoft.com/rest/api/maps/traffic).

### <a name="weather-service"></a>Wetterdienst

Der Wetterinfodienst bietet APIs, mit denen Entwickler Wetterinformationen für einen bestimmten Standort abrufen können. Die Informationen enthalten Details wie z. B das Datum und die Uhrzeit der Beobachtung, eine kurze Beschreibung der Wetterbedingungen, das Wettersymbol, Niederschlagsanzeigen, Temperatur und Windgeschwindigkeit. Weitere Details, wie z. B. RealFeel™-Temperatur und UV-Index, werden ebenfalls zurückgegeben.

Entwickler können die [Get Weather along Route-API](https://docs.microsoft.com/rest/api/maps/weather/getweatheralongroutepreview) verwenden, um Wetterinformationen auf einer bestimmten Route abzurufen. Außerdem unterstützt der Dienst die Generierung von Wetterbenachrichtigungen für Wegpunkte, die von Wetterrisiken betroffenen sind, z. B. Überflutung oder starker Regen.

Mit der [Get Map Tile V2-API](https://docs.microsoft.com/rest/api/maps/renderv2/getmaptilepreview) können Sie frühere, aktuelle und zukünftige Radar- und Satellitenkacheln anfordern.

![Beispiel für Karte Echtzeit-Wetterradarkacheln](media/about-azure-maps/intro_weather.png)

### <a name="maps-creator-service"></a>Kartenerstellungsdienst

Der Kartenerstellungsdienst ist eine Suite von Webdiensten, mit denen Entwickler Anwendungen mit Kartenfunktionen erstellen können, die auf Gebäudeplandaten basieren.

Die Kartenerstellung stellt drei Kerndienste bereit:

* [Datasetdienst](https://docs.microsoft.com/rest/api/maps/dataset). Verwenden Sie den Datasetdienst, um aus einem konvertierten Zeichnungspaket ein Dataset zu erstellen. Informationen zum den Anforderungen für Zeichnungspakete finden Sie unter Anforderungen für Zeichnungspakete.

* [Konvertierungsdienst](https://docs.microsoft.com/rest/api/maps/dataset). Verwenden Sie den Konvertierungsdienst, um eine DWG-Designdatei in Zeichnungspaketdaten für die Karten zu konvertieren.

* [Kachelsetdienst](https://docs.microsoft.com/rest/api/maps/tileset). Verwenden Sie den Kachelsetdienst, um eine vektorbasierte Darstellung eines Datasets zu erstellen. Anwendungen können ein Kachelset verwenden, um eine auf visuellen Kacheln basierende Ansicht des Datasets darzustellen.

* [Featurezustandsdienst](https://docs.microsoft.com/rest/api/maps/featurestate). Verwenden Sie den Featurezustandsdienst, um dynamische Kartenstile zu unterstützen. Dynamische Kartenstile ermöglichen es, dass Anwendungen Echtzeitereignisse in von IoT-Systemen bereitgestellten Bereichen darstellen.

* [WFS-Dienst](https://docs.microsoft.com/rest/api/maps/featurestate). Verwenden Sie den WFS-Dienst, um Ihre Gebäudeplandaten abzufragen. Der WFS-Dienst folgt den Standards der [Open Geospatial Consortium-API](http://docs.opengeospatial.org/is/17-069r3/17-069r3.html) zum Abfragen eines einzelnen Datasets.

## <a name="programming-model"></a>Programmiermodell

Azure Maps ist auf Mobilität ausgelegt und kann zum Entwickeln plattformübergreifender Anwendungen eingesetzt werden. Es wird ein Programmiermodell genutzt, das sprachunabhängig ist und die JSON-Ausgabe über [REST-APIs](https://docs.microsoft.com/rest/api/maps/) unterstützt.

Darüber hinaus bietet Azure Maps ein praktisches [JavaScript-Kartensteuerelement](https://docs.microsoft.com/javascript/api/azure-maps-control) mit einem einfachen Programmiermodell. Dies ermöglicht eine schnelle und einfache Entwicklung für webbasierte und mobile Anwendungen.

## <a name="power-bi-visual"></a>Power BI-Visualisierung

Das Azure Maps-Visual für Power BI ermöglicht umfangreiche Datenvisualisierungen für räumliche Daten auf einer Karte. Es wird geschätzt, dass mehr als 80 % der Geschäftsdaten einen Standortkontext aufweisen. Das Azure Maps-Visual bietet eine Lösung ohne Code, mit der Sie Erkenntnisse zur Beziehung zwischen diesem Standortkontext und Ihren Geschäftsdaten sowie zur Auswirkung des Standortkontexts auf Ihre Geschäftsdaten gewinnen können.

:::image type="content" source="./media/about-azure-maps/intro-power-bi.png" border="false" alt-text="Beispielkarte zur Bevölkerungsentwicklung, erstellt mit dem Azure Maps Web SDK":::

Weitere Informationen finden Sie in der Dokumentation [Erste Schritte mit dem Azure Maps-Visual für Power BI](power-bi-visual-getting-started.md).

## <a name="usage"></a>Verwendung

Für den Zugriff auf Azure Maps-Dienste wechseln Sie zum [Azure-Portal](https://portal.azure.com), und erstellen Sie ein Azure Maps-Konto.

Für Azure Maps wird ein schlüsselbasiertes Authentifizierungsschema verwendet. Wenn Sie Ihr Konto erstellen, werden zwei Schlüssel generiert. Für die Authentifizierung bei Azure Maps-Diensten können Sie beide Schlüssel verwenden.

Hinweis: Azure Maps gibt vom Kunden bereitgestellte Adress- oder Standortabfragen („Abfragen“) für die Zuordnungsfunktionalität an den Drittanbieter TomTom weiter. Abfragen werden nicht mit Kunden oder Endbenutzern verknüpft, wenn sie für TomTom bereitgestellt werden, und können nicht für die Identifikation von Personen verwendet werden. Die Mobilitäts- und Wetterinfodienste, die die Integration in Moovit und AccuWeather beinhalten, befinden sich derzeit in der [VORSCHAUPHASE](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Microsoft ist derzeit dabei, TomTom, Moovit und AccuWeather zur Liste der Onlinedienstzulieferer hinzuzufügen.

## <a name="supported-regions"></a>Unterstützte Regionen

Azure Maps-Dienste sind derzeit mit Ausnahme der folgenden Länder/Regionen überall verfügbar:

* China
* Südkorea

Vergewissern Sie sich, dass sich der Standort Ihrer aktuellen IP-Adresse in einem unterstützten Land bzw. in einer unterstützten Region befindet.

## <a name="next-steps"></a>Nächste Schritte

Testen Sie eine Beispiel-App zur Veranschaulichung von Azure Maps:

[Schnellstart: Erstellen einer Web-App](quick-demo-map-app.md)

Halten Sie sich über Azure Maps auf dem Laufenden:

[Blog zu Azure Maps](https://azure.microsoft.com/blog/topics/azure-maps/)
