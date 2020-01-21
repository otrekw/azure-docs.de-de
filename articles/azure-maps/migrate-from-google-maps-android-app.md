---
title: 'Tutorial: Migrieren einer Android-App | Microsoft Azure Maps'
description: Hier erfahren Sie, wie Sie eine Android-App aus Google Maps zu Microsoft Azure Maps migrieren.
author: rbrundritt
ms.author: richbrun
ms.date: 12/17/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: ''
ms.openlocfilehash: 643d48cb931bcec1a8a3385d2ec24a394660c368
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2020
ms.locfileid: "75909191"
---
# <a name="migrate-an-android-app-from-google-maps"></a>Migrieren von Android-Apps aus Google Maps

Das Android SDK für Azure Maps verfügt über eine API-Schnittstelle, die dem Web SDK sehr ähnlich ist. Für beide SDKs gelten teilweise die gleichen Konzepte, Best Practices und Architekturen bei der Entwicklung, und wenn Sie eines bereits verwendet haben, sollten Sie Ihr Wissen auch auf das andere anwenden können.

Das Android SDK für Azure Maps unterstützt API 21 als Mindestversion von Android: Android 5.0.0 (Lollipop).

Alle Beispiele wurden in Java geschrieben, aber Sie können auch Kotlin für das Android SDK für Azure Maps verwenden.

Weitere Informationen zum Entwickeln mit diesem SDK finden Sie unter [Erste Schritte mit dem Android SDK für Azure Maps](how-to-use-android-map-control-library.md).

## <a name="load-a-map"></a>Laden einer Karte

Wenn Sie mithilfe von Google Maps oder Azure Maps eine Karte in einer Android-App laden möchten, müssen Sie bei beiden SDKs teilweise sehr ähnlich vorgehen. Bei beiden SDKs müssen Sie die folgenden Schritte ausführen:

- Sie benötigen einen API- oder Abonnementschlüssel, um auf eine der beiden Plattformen zugreifen zu können.
- Fügen Sie XML-Code zu einer Aktivität hinzu, um festzulegen, wo die App gerendert und angeordnet werden soll.
- Leiten Sie alle Lebenszyklusmethoden aus der Aktivität weiter, die die Kartenansicht der zugehörigen Methoden in der map-Klasse enthält. Insbesondere müssen Sie die folgenden Methoden weiterleiten:
    - `onCreate(Bundle)`
    - `onStart()`
    - `onResume()`
    - `onPause()`
    - `onStop()`
    - `onDestroy()`
    - `onSaveInstanceState(Bundle)`
    - `onLowMemory()`
- Warten Sie, bis die Karte bereit ist, bevor Sie versuchen, programmgesteuert auf diese zuzugreifen.

**Vorher: Google Maps**

Wenn Sie mithilfe des Google Maps SDK für Android eine Karte abrufen möchten, gehen Sie wie folgt vor:

1.  Vergewissern Sie sich, dass die Google Play-Dienste installiert sind.
2.  Fügen Sie zur Datei **gradle.build** des Moduls eine Abhängigkeit für den Google Maps-Dienst hinzu: 

    `implementation 'com.google.android.gms:play-services-maps:17.0.0'`

1.  Fügen Sie im Anwendungsabschnitt der Datei **google\_maps\_api.xml** einen API-Schlüssel für Google Maps hinzu:
    
    ```xml
    <meta-data android:name="com.google.android.geo.API_KEY" android:value="YOUR_GOOGLE_MAPS_KEY"/>
    ```

1.  Fügen Sie der Hauptaktivität ein Kartenfragment hinzu:

    ```xml
    <com.google.android.gms.maps.MapView
            android:id="@+id/myMap"
            android:layout_width="match_parent"
            android:layout_height="match_parent"/>
    ```

1.  Sie müssen zur Datei **MainActivity.java** Importe für das Google Maps SDK hinzufügen. Leiten Sie alle Lebenszyklusmethoden aus der Aktivität weiter, die die Kartenansicht der zugehörigen Methoden in der map-Klasse enthält. Mithilfe der Methode `getMapAsync(OnMapReadyCallback)` kann eine `MapView`-Instanz aus dem Kartenfragment abgerufen werden. Das `MapView`-Objekt initialisiert automatisch das Kartensystem und die Ansicht. Bearbeiten Sie die Datei **MainActivity.java** wie folgt:

    ```java
    import com.google.android.gms.maps.GoogleMap;
    import com.google.android.gms.maps.MapView;
    import com.google.android.gms.maps.OnMapReadyCallback;
    
    import android.support.v7.app.AppCompatActivity;
    import android.os.Bundle;
    
    public class MainActivity extends AppCompatActivity implements OnMapReadyCallback {
    
        MapView mapView;
    
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            mapView = findViewById(R.id.myMap);
    
            mapView.onCreate(savedInstanceState);
            mapView.getMapAsync(this);
        }
    
        @Override
    
        public void onMapReady(GoogleMap map) {
            //Add your post map load code here.
        }
    
        @Override
        public void onResume() {
            super.onResume();
            mapView.onResume();
        }
    
        @Override
        protected void onStart(){
            super.onStart();
            mapView.onStart();
        }
    
        @Override
        public void onPause() {
            super.onPause();
            mapView.onPause();
        }
    
        @Override
        public void onStop() {
            super.onStop();
            mapView.onStop();
        }
    
        @Override
        public void onLowMemory() {
            super.onLowMemory();
            mapView.onLowMemory();
        }
    
        @Override
        protected void onDestroy() {
            super.onDestroy();
            mapView.onDestroy();
        }
    
        @Override
        protected void onSaveInstanceState(Bundle outState) {
            super.onSaveInstanceState(outState);
            mapView.onSaveInstanceState(outState);
        }
    }
    ```

Wenn das Kartensteuerelement in einer App ausgeführt wird, wird es wie folgt geladen:

<center>

![Einfache Google Maps-Karte](media/migrate-google-maps-android-app/simple-google-maps.png)</center>

**Nachher: Azure Maps**

Wenn Sie mithilfe des Azure Maps SDK für Android eine Karte anzeigen lassen möchten, gehen Sie wie folgt vor:

1. Öffnen Sie die Datei **build.gradle** auf der obersten Ebene, und fügen Sie dem Blockabschnitt **all projects**, **repositories** den folgenden Code hinzu:

    ```
    maven {
            url "https://atlas.microsoft.com/sdk/android"
    }
    ```

2. Aktualisieren Sie **app/build.gradle**, und fügen Sie den folgenden Code hinzu:
    
    1. Stellen Sie sicher, dass **minSdkVersion** Ihres Projekts mindestens auf API 21 festgelegt ist.

    2. Fügen Sie den folgenden Code dem Abschnitt „Android“ hinzu:

        ```
        compileOptions {
            sourceCompatibility JavaVersion.VERSION_1_8
            targetCompatibility JavaVersion.VERSION_1_8
        }
        ```
    3. Aktualisieren Sie Ihren Block „dependencies“, und fügen Sie eine neue Implementierungsabhängigkeitszeile für das neueste Android SDK von Azure Maps hinzu:

        ```
        implementation "com.microsoft.azure.maps:mapcontrol:0.2"
        ```

        > [!Note]
        > Das Android SDK für Azure Maps wird regelmäßig aktualisiert und verbessert. In der Dokumentation [Erste Schritte mit dem Android-Kartensteuerelement](how-to-use-android-map-control-library.md) finden Sie die neueste Implementierungsversionsnummer von Azure Maps. Außerdem können Sie die Versionsnummer von „0.2“ auf „0+“ festlegen, damit immer auf die neueste Version verwiesen wird.
    
    4. Wechseln Sie auf der Symbolleiste zu **Datei**, und klicken Sie auf **Sync Project with Gradle Files** (Projekt mit Gradle-Dateien synchronisieren).
3. Fügen Sie der Hauptaktivität ein Kartenfragment hinzu (res \> Layout \> activity\_main.xml):
    
    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <FrameLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        >

        <com.microsoft.azure.maps.mapcontrol.MapControl
            android:id="@+id/mapcontrol"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            />
    </FrameLayout>
    ```

4. In der Datei **MainActivity.java** müssen Sie Folgendes ausführen:
    
    * Hinzufügen von Importen für das Azure Maps SDK
    * Festlegen Ihrer Azure Maps-Authentifizierungsinformationen
    * Abrufen der Kartensteuerelementinstanz in der **onCreate**-Methode

    Wenn Sie die Authentifizierungsinformationen für die Klasse `AzureMaps` mithilfe der Methode `setSubscriptionKey` oder der Methode `setAadProperties` global festlegen, müssen Sie nicht für jede Ansicht Ihre Authentifizierungsinformationen hinzufügen. 

    Das Kartensteuerelement enthält eigene Lebenszyklusmethoden zur Verwaltung des OpenGL-Lebenszyklus von Android, die direkt aus der enthaltenen Activity aufgerufen werden müssen. Damit Ihre App ordnungsgemäß funktioniert, rufen Sie die Lebenszyklusmethoden des Kartensteuerelements auf. Sie müssen die folgenden Lebenszyklusmethoden in der Aktivität, die das Kartensteuerelement enthält, überschreiben und die entsprechende Kartensteuerelementmethode aufrufen. 

    * `onCreate(Bundle)` 
    * `onStart()` 
    * `onResume()` 
    * `onPause()` 
    * `onStop()` 
    * `onDestroy()` 
    * `onSaveInstanceState(Bundle)` 
    * `onLowMemory()`

    Bearbeiten Sie die Datei **MainActivity.java** wie folgt:
    
    ```java
    package com.example.myapplication;

    import android.support.v7.app.AppCompatActivity;
    import android.os.Bundle;
    import com.microsoft.azure.maps.mapcontrol.AzureMaps;
    import com.microsoft.azure.maps.mapcontrol.MapControl;
    import com.microsoft.azure.maps.mapcontrol.layer.SymbolLayer;
    import com.microsoft.azure.maps.mapcontrol.options.MapStyle;
    import com.microsoft.azure.maps.mapcontrol.source.DataSource;

    public class MainActivity extends AppCompatActivity {
        
        static {
            AzureMaps.setSubscriptionKey("<Your Azure Maps subscription key>");
        }

        MapControl mapControl;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);

            mapControl = findViewById(R.id.mapcontrol);

            mapControl.onCreate(savedInstanceState);
    
            //Wait until the map resources are ready.
            mapControl.onReady(map -> {
                //Add your post map load code here.
    
            });
        }

        @Override
        public void onResume() {
            super.onResume();
            mapControl.onResume();
        }

        @Override
        protected void onStart(){
            super.onStart();
            mapControl.onStart();
        }

        @Override
        public void onPause() {
            super.onPause();
            mapControl.onPause();
        }

        @Override
        public void onStop() {
            super.onStop();
            mapControl.onStop();
        }

        @Override
        public void onLowMemory() {
            super.onLowMemory();
            mapControl.onLowMemory();
        }

        @Override
        protected void onDestroy() {
            super.onDestroy();
            mapControl.onDestroy();
        }

        @Override
        protected void onSaveInstanceState(Bundle outState) {
            super.onSaveInstanceState(outState);
            mapControl.onSaveInstanceState(outState);
        }
    }
    ```

Wenn Sie Ihre Anwendung ausführen, lädt das Kartensteuerelement wie folgt.

<center>

![Einfache Azure Maps-Karte](media/migrate-google-maps-android-app/simple-azure-maps.png)</center>

Hinweis: Das Azure Maps-Steuerelement unterstützt das Zoomen und bietet eine Weltansicht.

> [!TIP]
> Wenn Sie einen Android-Emulator in Windows verwenden, wird die Karte möglicherweise aufgrund von Konflikten mit OpenGL und dem von der Software beschleunigten Rendern von Grafiken nicht gerendert. In einigen Fällen konnte dieses Problem wie folgt behoben werden: Öffnen Sie den AVD-Manager, und wählen Sie das zu bearbeitende virtuelle Gerät aus. Legen Sie im Abschnitt **Emulated Performance** (Emulierte Leistung) die Option **Grafiken** auf **Hardware** fest.

## <a name="localizing-the-map"></a>Lokalisieren der Karte

Wenn sich Ihre Zielgruppe über mehrere Länder erstreckt oder verschiedene Sprachen spricht, ist Lokalisierung ein wichtiger Punkt.

**Vorher: Google Maps**

Sie können die Sprache der Karte in der Methode `onCreate` der Hauptaktivität festlegen, indem Sie den folgenden Code hinzufügen, bevor Sie die Kontextansicht der Karte festlegen. Im folgenden Code wird mithilfe des Sprachcodes „fr“ die Sprache auf Französisch festgelegt.

```java
String languageToLoad = "fr";
Locale locale = new Locale(languageToLoad);
Locale.setDefault(locale);
Configuration config = new Configuration();
config.locale = locale;
getBaseContext().getResources().updateConfiguration(config,
        getBaseContext().getResources().getDisplayMetrics());
```

Dies ist ein Beispiel für Google Maps mit der Spracheinstellung „fr“.

<center>

![Lokalisierung in Google Maps](media/migrate-google-maps-android-app/google-maps-localization.png)</center>

**Nachher: Azure Maps**

Azure Maps bietet drei verschiedene Möglichkeiten, um die Sprache und die regionale Ansicht für die Karte festzulegen. Die erste Option besteht darin, die Informationen zur Sprache und regionalen Ansicht mithilfe der statischen Methoden `setLanguage` und `setView` global an die Klasse `AzureMaps` zu übergeben. Dadurch werden die festgelegte Standardsprache und die regionale Ansicht für alle Azure Maps-Steuerelemente in Ihre App geladen. Mit dem folgenden Code wird mithilfe des Sprachcodes „fr-FR“ die Sprache auf Französisch festgelegt.

```java
static {
    //Set your Azure Maps Key.
    AzureMaps.setSubscriptionKey("<Your Azure Maps Key>");

    //Set the language to be used by Azure Maps.
    AzureMaps.setLanguage("fr-FR");

    //Set the regional view to be used by Azure Maps.
    AzureMaps.setView("auto");
}
```

Die zweite Option besteht darin, die Informationen zur Sprache und Ansicht an die Kartensteuerelement-XML zu übergeben.

```xml
<com.microsoft.azure.maps.mapcontrol.MapControl
    android:id="@+id/myMap"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:mapcontrol_language="fr-FR"
    app:mapcontrol_view="auto"
    />
```

Die dritte Option besteht darin, die Sprache und regionale Ansicht der Karte mithilfe der Kartenmethode `setStyle` programmgesteuert festzulegen. Dies kann jederzeit geschehen, um die Sprache und regionale Ansicht der Karte zu ändern.

```java
mapControl.onReady(map -> {
    map.setStyle(StyleOptions.language("fr-FR"));
    map.setStyle(StyleOptions.view("auto"));
});
```

Dies ist ein Beispiel für Azure Maps mit der Spracheinstellung „fr-FR“.

<center>

![Lokalisierung in Azure Maps](media/migrate-google-maps-android-app/azure-maps-localization.png)</center>

Eine vollständige Liste der unterstützten Sprachen und regionalen Ansichten ist [hier](supported-languages.md) dokumentiert.

## <a name="setting-the-map-view"></a>Festlegen der Kartenansicht

Dynamische Karten können sowohl in Azure Maps als auch in Google Maps programmgesteuert durch Aufrufen der passenden Methoden an neue geografische Standorte verschoben werden. Die Beispiele unten demonstrieren das Festlegen der Karte auf Satellitenluftaufnahmen, das Zentrieren der Karte über einem Standort mit Koordinaten (Breitengrad: 35,0272; Längengrad: -111,0225) und das Ändern der Zoomstufe auf 15 in Google Maps.

> [!NOTE]
> Google Maps verwendet Kacheln mit der Abmessung 256 Pixel, während Azure Maps größere Kacheln mit 512 Pixel verwendet. Dadurch wird die Anzahl der Netzwerkanforderungen reduziert, die Azure Maps zum Laden des gleichen Kartenbereichs wie Google Maps benötigt. Aufgrund der Funktionsweise von Kachelpyramiden in Kartensteuerelementen müssen Sie für größere Kacheln in Azure Maps allerdings die in Google Maps verwendete Zoomstufe in Azure Maps um 1 verringern, damit in Azure Maps derselbe Bereich angezeigt wird wie in Google Maps.

**Vorher: Google Maps**

Die Kamera des Kartensteuerelements in Google Maps kann mithilfe der Methode `moveCamera` programmgesteuert verschoben werden, wodurch Sie den Mittelpunkt der Karte und eine Zoomstufe angeben können. Die Methode `setMapType` kann verwendet werden, um den Typ der angezeigten Karte zu ändern.

```java
@Override
public void onMapReady(GoogleMap googleMap) {
    mapView = googleMap;

    mapView.moveCamera(CameraUpdateFactory.newLatLngZoom(new LatLng(35.0272, -111.0225), 15));
    mapView.setMapType(GoogleMap.MAP_TYPE_SATELLITE);
}
```

<center>

![Festgelegte Google Maps-Ansicht](media/migrate-google-maps-android-app/google-maps-set-view.png)</center>

**Nachher: Azure Maps**

Wie zuvor bereits erwähnt, wird derselbe Bereich in Azure Maps angezeigt, wenn Sie die Zoomstufe in Google Maps um 1 verringern. Verwenden Sie in diesem Beispiel die Zoomstufe 14.

Die ursprüngliche Kartenansicht kann in XML-Attributen auf dem Kartensteuerelement festgelegt werden.

```xml
<com.microsoft.azure.maps.mapcontrol.MapControl
    android:id="@+id/myMap"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:mapcontrol_cameraLat="35.0272"
    app:mapcontrol_cameraLng="-111.0225"
    app:mapcontrol_zoom="14"
    app:mapcontrol_style="satellite"
    />
```

Die Kartenansicht kann mithilfe der Kartenmethoden `setCamera` und `setStyle` programmgesteuert aktualisiert werden.

```java
mapControl.onReady(map -> {
    //Set the camera of the map.
    map.setCamera(center(35.0272, -111.0225), zoom(14));

    //Set the style of the map.
    map.setStyle(style(MapStyle.SATELLITE));
});
```

<center>

![Festgelegte Azure Maps-Ansicht](media/migrate-google-maps-android-app/azure-maps-set-view.png)</center>

**Zusätzliche Ressourcen:**

- [Unterstützte Kartenstile](supported-map-styles.md)

## <a name="adding-a-marker"></a>Hinzufügen eines Markers

Punktdaten werden häufig mithilfe eines Bilds auf der Karte gerendert. Diese Bilder werden oft als Marker, Stecknadeln, Pins oder Symbole bezeichnet. In den folgenden Beispielen werden Punktdaten als Marker auf der Karte gerendert (Breitengrad: 51,5; Längengrad: -0,2).

**Vorher: Google Maps**

In Google Maps werden Marker mithilfe der Kartenmethode `addMarker` hinzugefügt.

```java
@Override
public void onMapReady(GoogleMap googleMap) {
    mapView = googleMap;

    mapView.addMarker(new MarkerOptions().position(new LatLng(47.64, -122.33)));
}
```

<center>

![Google Maps-Marker](media/migrate-google-maps-android-app/google-maps-marker.png)</center>

**Nachher: Azure Maps**

In Azure Maps können Sie Punktdaten auf der Karte rendern, indem Sie zuerst die Daten zu einer Datenquelle hinzufügen und anschließend diese Datenquellen zu einer Symbolebene hinzufügen. Die Datenquelle optimiert die Verwaltung räumlicher Daten im Kartensteuerelement, und die Symbolebene legt fest, wie Datenpunkte als Bilder und/oder Text gerendert werden sollen.

```java
mapControl.onReady(map -> {
    //Create a data source and add it to the map.
    DataSource dataSource = new DataSource();
    map.sources.add(dataSource);

    //Create a point feature and add it to the data source.
    dataSource.add(Feature.fromGeometry(Point.fromLngLat(-122.33, 47.64)));

    //Create a symbol layer and add it to the map.
    map.layers.add(new SymbolLayer(dataSource));
});
```

<center>

![Azure Maps-Marker](media/migrate-google-maps-android-app/azure-maps-marker.png)</center>

## <a name="adding-a-custom-marker"></a>Hinzufügen eines benutzerdefinierten Markers

Benutzerdefinierte Bilder können zur Darstellung von Punkten auf einer Karte verwendet werden. Das folgende Bild wird in den Beispielen unten als benutzerdefiniertes Bild verwendet, um einen Punkt auf der Karte anzuzeigen (Breitengrad: 51,5, Längengrad: -0,2), und versetzt die Position des Markers so, dass der Punkt des Stecknadelsymbols sich auf die richtige Position auf der Karte ausrichtet.

<center>

![Bild: gelbe Stecknadel](media/migrate-google-maps-web-app/ylw_pushpin.png)<br/>
ylw\_pushpin.png</center>

In beiden Beispielen wird das obige Bild dem Ordner „drawable“ der App-Ressourcen hinzugefügt.

**Vorher: Google Maps**

Mit Google Maps können Sie benutzerdefinierte Bilder für Marker verwenden, indem Sie sie über die Option `icon` des Markers laden. Sie können die Option `anchor` verwenden, um den Punkt des Bilds auf die Koordinate auszurichten. Die Option „anchor“ steht in Relation zu den Maßen des Bilds. In diesem Fall ist es 0,2 Einheiten breit und eine Einheit hoch.

```java
@Override
public void onMapReady(GoogleMap googleMap) {
    mapView = googleMap;

    mapView.addMarker(new MarkerOptions().position(new LatLng(47.64, -122.33))
    .icon(BitmapDescriptorFactory.fromResource(R.drawable.ylw_pushpin))
    .anchor(0.2f, 1f));
}
```

<center>

![Benutzerdefinierter Marker in Google Maps](media/migrate-google-maps-android-app/google-maps-custom-marker.png)</center>

**Nachher: Azure Maps**

Symbolebenen in Azure Maps unterstützen ebenfalls benutzerdefinierte Bilder, diese müssen jedoch zuerst in die Kartenressourcen geladen werden, und ihnen muss jeweils eine eindeutige ID zugewiesen sein. Anschließend kann die Symbolebene auf diese ID verweisen. Das Symbol kann mithilfe der Option `iconOffset` entsprechend dem richtigen Datenpunkt auf dem Bild versetzt werden. Beachten Sie, dass für die Option „iconOffset“ Pixelwerte verwendet werden. Standardmäßig steht diese Option in Relation zum unteren mittleren Bereich des Bilds. Dies kann aber mithilfe der Option `iconAnchor` angepasst werden. In diesem Beispiel wird die Option `iconAnchor` auf `"center"` festgelegt, und die Option „iconOffset“ wird verwendet, um das Bild 5 Pixel nach rechts und 15 Pixel nach oben zu verschieben, sodass dieses auf den Punkt des Stecknadelbilds ausgerichtet ist.

```java
mapControl.onReady(map -> {
    //Create a data source and add it to the map.
    DataSource dataSource = new DataSource();
    map.sources.add(dataSource);

    //Create a point feature and add it to the data source.
    dataSource.add(Feature.fromGeometry(Point.fromLngLat(-122.33, 47.64)));

    //Load the custom image icon into the map resources.
    map.images.add("my-yellow-pin", R.drawable.ylw_pushpin);

    //Create a symbol that uses the custom image icon and add it to the map.
    map.layers.add(new SymbolLayer(dataSource,
        iconImage("my-yellow-pin"),
        iconAnchor(AnchorType.CENTER
        iconOffset(new Float[]{5f, -15f})));
});
```

<center>

![Benutzerdefinierter Marker in Azure Maps](media/migrate-google-maps-android-app/azure-maps-custom-marker.png)</center>

## <a name="adding-a-polyline"></a>Hinzufügen einer Polylinie

Polylinien werden verwendet, um eine Linie oder einen Pfad auf der Karte darzustellen. In den folgenden Beispielen wird gezeigt, wie Sie eine gestrichelte Polylinie auf der Karte erstellen.

**Vorher: Google Maps**

Mit Google Maps kann mithilfe der Klasse `PolylineOptions` eine Polylinie erstellt und über die Methode `addPolyline` zur Karte hinzugefügt werden. Die Strichfarbe kann mithilfe der Option `color` festgelegt werden, die Strichbreite wird mithilfe der Option „width“ festgelegt, und Sie können mithilfe der Option `pattern` ein Stricharray festlegen.

```java
@Override
public void onMapReady(GoogleMap googleMap) {
    mapView = googleMap;

    //Create the options for the polyline.
    PolylineOptions lineOptions = new PolylineOptions()
            .add(new LatLng(46, -123))
            .add(new LatLng(49, -122))
            .add(new LatLng(46, -121))
            .color(Color.RED)
            .width(10f)
            .pattern(Arrays.<PatternItem>asList(
                    new Dash(30f), new Gap(30f)));

    //Add the polyline to the map.
    Polyline polyline = mapView.addPolyline(lineOptions);
}
```

<center>

![Google Maps-Polylinie](media/migrate-google-maps-android-app/google-maps-polyline.png)</center>

**Nachher: Azure Maps**

In Azure Maps werden Polylinien als LineString- oder MultiLineString-Objekte bezeichnet. Diese Objekte können einer Datenquelle hinzugefügt und mithilfe einer Linienebene gerendert werden. Hinweis: Die Strichbreite und die Pixeleinheiten des Stricharrays werden auf das Web SDK für Azure Maps ausgerichtet, sodass bei der Verwendung derselben Werte in beiden SDKs auch dieselben Ergebnisse erzeugt werden.

```java
mapControl.onReady(map -> {
    //Create a data source and add it to the map.
    DataSource dataSource = new DataSource();
    map.sources.add(dataSource);

    //Create an array of points.
    List<Point> points = Arrays.asList(
        Point.fromLngLat(-123, 46),
        Point.fromLngLat(-122, 49),
        Point.fromLngLat(-121, 46));

    //Create a LineString feature and add it to the data source.
    dataSource.add(Feature.fromGeometry(LineString.fromLngLats(points)));

    //Create a line layer and add it to the map.
    map.layers.add(new LineLayer(dataSource,
        strokeColor("red"),
        strokeWidth(4f),
        strokeDashArray(new Float[]{3f, 3f})));
});
```

<center>

![Azure Maps-Polylinie](media/migrate-google-maps-android-app/azure-maps-polyline.png)</center>

## <a name="adding-a-polygon"></a>Hinzufügen eines Polygons

Polygone werden verwendet, um einen Bereich auf der Karte darzustellen. In den folgenden Beispielen wird gezeigt, wie ein Polygon erstellt wird, das ein Dreieck bildet, das auf der Mittelpunktkoordinate der Karte basiert.

**Vorher: Google Maps**

Mit Google Maps kann mithilfe der Klasse `PolygonOptions` ein Polygon erstellt und über die Methode `addPolygon` zur Karte hinzugefügt werden. Die Füll- und Strichfarben können mithilfe der Optionen `fillColor` und `strokeColor` festgelegt werden und die Strichbreite mithilfe der Option `strokeWidth`.

```java
@Override
public void onMapReady(GoogleMap googleMap) {
    mapView = googleMap;

    //Create the options for the polygon.
    PolygonOptions polygonOptions = new PolygonOptions()
            .add(new LatLng(46, -123))
            .add(new LatLng(49, -122))
            .add(new LatLng(46, -121))
            .add(new LatLng(46, -123))  //Close the polygon.
            .fillColor(Color.argb(128, 0, 128, 0))
            .strokeColor(Color.RED)
            .strokeWidth(5f);

    //Add the polygon to the map.
    Polygon polygon = mapView.addPolygon(polygonOptions);
}
```

<center>

![Google Maps-Polygon](media/migrate-google-maps-android-app/google-maps-polygon.png)</center>

**Nachher: Azure Maps**

In Azure Maps können einer Datenquelle Polygon- und MultiPolygon-Objekte hinzugefügt und mithilfe von Ebenen auf der Karte gerendert werden. Der Bereich eines Polygons kann in einer Polygonebene gerendert werden. Der Umriss eines Polygons kann mithilfe einer Linienebene gerendert werden. Hinweis: Die Strichbreite und die Pixeleinheiten des Stricharrays werden auf das Web SDK für Azure Maps ausgerichtet, sodass bei der Verwendung derselben Werte in beiden SDKs auch dieselben Ergebnisse erzeugt werden.

```java
mapControl.onReady(map -> {
    //Create a data source and add it to the map.
    DataSource dataSource = new DataSource();
    map.sources.add(dataSource);

    //Create an array of point arrays to create polygon rings.
    List<List<Point>> rings = Arrays.asList(Arrays.asList(
        Point.fromLngLat(-123, 46),
        Point.fromLngLat(-122, 49),
        Point.fromLngLat(-121, 46),
        Point.fromLngLat(-123, 46)));

    //Create a Polygon feature and add it to the data source.
    dataSource.add(Feature.fromGeometry(Polygon.fromLngLats(rings)));

    //Add a polygon layer for rendering the polygon area.
    map.layers.add(new PolygonLayer(dataSource,
        fillColor("green"),
        fillOpacity(0.5f)));

    //Add a line layer for rendering the polygon outline.
    map.layers.add(new LineLayer(dataSource,
        strokeColor("red"),
        strokeWidth(2f)));
});
```

<center>

![Azure Maps-Polygon](media/migrate-google-maps-android-app/azure-maps-polygon.png)</center>

## <a name="overlay-a-tile-layer"></a>Überlagern einer Kachelebene

Kachelebenen, die in Google Maps auch als Bildüberlagerungen bezeichnet werden, ermöglichen es Ihnen, Bilder mit mehreren Ebenen zu überlagern, die in kleinere gekachelte Bilder aufgeteilt wurden, die sich am Kachelsystem für Karten orientieren. Dies ist eine gängige Methode zum Überlagern von Bildern mit mehreren Ebenen oder sehr großer Datasets.

In den folgenden Beispielen wird eine Kachelebene eines Wetterradars aus dem Iowa Environmental Mesonet der Iowa State University überlagert. Die Kacheln sind 256 Pixel groß.

**Vorher: Google Maps**

Mit Google Maps können Sie mithilfe der Klasse eine Kachelebene über der Karte anordnen, indem Sie die Klasse `TileOverlayOptions` verwenden und mithilfe der Methode `addTileLauer` zur Karte hinzufügen. Die Kacheln nehmen ein halbtransparentes Format an, wenn die Option `transparency` auf 0,2 oder 20 % Transparenz festgelegt wird.

```java
@Override
public void onMapReady(GoogleMap googleMap) {
    mapView = googleMap;

    //Create the options for the tile layer.
    TileOverlayOptions tileLayer = new TileOverlayOptions()
        .tileProvider(new UrlTileProvider(256, 256) {
            @Override
            public URL getTileUrl(int x, int y, int zoom) {

                try {
                    //Define the URL pattern for the tile images.
                    return new URL(String.format("https://mesonet.agron.iastate.edu/cache/tile.py/1.0.0/nexrad-n0q-900913/%d/%d/%d.png", zoom, x, y));
                }catch (MalformedURLException e) {
                    throw new AssertionError(e);
                }
            }
        }).transparency(0.2f);

    //Add the tile layer to the map.
    mapView.addTileOverlay(tileLayer);
}
```

<center>

![Google Maps-Kachelebene](media/migrate-google-maps-android-app/google-maps-tile-layer.png)</center>

**Nachher: Azure Maps**

In Azure Maps können Sie die Kachelebenen einer Karte auf ähnliche Weise wie jede andere Ebene hinzufügen. Eine formatierte URL, die x, y und Zoomplatzhalter bzw. `{x}`, `{y}`, `{z}` aufweist, wird dazu verwendet, die Ebene anzuweisen, an welcher Position sie auf die Kacheln zugreifen soll. Kachelebenen in Azure Maps unterstützen außerdem `{quadkey}`-, `{bbox-epsg-3857}`- und `{subdomain}`-Platzhalter. Die Kachelebene wird halbtransparent angezeigt, wenn der Wert 0,8 für die Deckkraft verwendet wird. Beachten Sie, dass für Deckkraft und Transparenz jeweils der umgekehrte Wert verwendet wird, obwohl sich beide Aspekte ähneln. Wenn Sie die Werte konvertieren möchten, subtrahieren sie diese jeweils einfach von der Zahl 1.

> [!TIP]
> In Azure Maps können Ebenen leicht unterhalb von anderen Ebenen gerendert werden, einschließlich Basiskartenebenen. Es ist häufig wünschenswert, Kachelebenen unterhalb der Kartenbezeichnungen zu rendern, damit sie leicht zu lesen sind. Die Methode `map.layers.add` nimmt einen zweiten Parameter an, bei dem es sich um die ID der Ebene handelt, unter der die neue Ebene eingefügt werden soll. Mithilfe des folgenden Codes können Sie eine Kachelebene unterhalb der Kartenbezeichnungen einfügen: `map.layers.add(myTileLayer, "labels");`

```java
mapControl.onReady(map -> {
    //Add a tile layer to the map, below the map labels.
    map.layers.add(new TileLayer(
        tileUrl("https://mesonet.agron.iastate.edu/cache/tile.py/1.0.0/nexrad-n0q-900913/{z}/{x}/{y}.png"),
        opacity(0.8f),
        tileSize(256)
    ), "labels");
});
```

<center>

![Azure Maps-Kachelebene](media/migrate-google-maps-android-app/azure-maps-tile-layer.png)</center>

## <a name="show-traffic"></a>Anzeigen des Verkehrs

Sowohl Azure- als auch Google-Karten können mit Verkehrsdaten überlagert werden.

**Vorher: Google Maps**

Sie können mithilfe von Google Maps Daten zum Verkehr zu der Karte hinzufügen, indem Sie den Wert TRUE an die Methode `setTrafficEnabled` der Karte übergeben.

```java
@Override
public void onMapReady(GoogleMap googleMap) {
    mapView = googleMap;

    mapView.setTrafficEnabled(true);
}
```

<center>

![Verkehrsinformationen in Google Maps](media/migrate-google-maps-android-app/google-maps-traffic.png)</center>

**Nachher: Azure Maps**

Azure Maps bietet verschiedene Optionen zum Anzeigen von Verkehrsinformationen. Ereignisse wie etwa Straßensperrungen und Unfälle können als Symbole auf der Karte angezeigt werden. Der Verkehrsfluss und farbig codierte Straßen können auf der Karte eingeblendet werden, die Farben können geändert werden und auf dem veröffentlichten Tempolimit, der normalerweise zu erwartenden Verzögerung oder der absoluten Verzögerung basieren. Die Ereignisdaten werden in Azure Maps minütlich aktualisiert, die Daten zum Verkehrsfluss alle zwei Minuten.

```java
mapControl.onReady(map -> {
    map.setTraffic(
        incidents(true),
        flow(TrafficFlow.RELATIVE));
});
```

<center>

![Verkehrsinformationen in Azure Maps](media/migrate-google-maps-android-app/azure-maps-traffic.png)</center>

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Android SDK für Azure Maps:

> [!div class="nextstepaction"]
> [Verwenden des Android-Kartensteuerelements](how-to-use-android-map-control-library.md)
