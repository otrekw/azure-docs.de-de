---
title: 'Tutorial: Laden von GeoJSON-Daten in das Android SDK für Azure Maps | Microsoft Azure Maps'
description: Tutorial zum Laden einer GeoJSON-Datendatei in das Android-Karten-SDK für Azure Maps.
author: rbrundritt
ms.author: richbrun
ms.date: 12/10/2020
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: cpendle
zone_pivot_groups: azure-maps-android
ms.openlocfilehash: 8300a7c120ce816c8068a88fa69f4f978fa664ca
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102034505"
---
# <a name="tutorial-load-geojson-data-into-azure-maps-android-sdk"></a>Tutorial: Laden von GeoJSON-Daten in das Android SDK für Azure Maps

In diesem Tutorial wird Schritt für Schritt beschrieben, wie Sie eine GeoJSON-Datei mit Standortdaten in das Android SDK für Azure Maps importieren. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Hinzufügen von Azure Maps zu einer Android-Anwendung
> * Erstellen einer Datenquelle und Laden in eine GeoJSON-Datei aus einer lokalen Datei oder dem Web
> * Anzeigen der Daten in der Karte

## <a name="prerequisites"></a>Voraussetzungen

1. Schließen Sie den [Schnellstart: Erstellen einer Android-App](quick-android-map.md) ab. In diesem Tutorial wird der Code erweitert, der in dieser Schnellstartanleitung verwendet wurde.
2. Laden Sie die GeoJSON-Datei mit den [Point of Interest-Beispielen](https://raw.githubusercontent.com/Azure-Samples/AzureMapsCodeSamples/master/AzureMapsCodeSamples/Common/data/geojson/SamplePoiDataSet.json) herunter.

### <a name="import-geojson-data-from-web-or-assets-folder"></a>Importieren von GeoJSON-Daten aus dem Web oder einem Ressourcenordner

Bei den meisten GeoJSON-Dateien sind alle Daten mit einem `FeatureCollection`-Element umschlossen. Dies bedeutet Folgendes: Wenn die GeoJSON-Dateien als Zeichenfolge in die Anwendung geladen werden, können sie an die statische `fromJson`-Methode der Funktionssammlung übergeben werden. Mit dieser Methode wird die Zeichenfolge in ein `FeatureCollection`-GeoJSON-Objekt deserialisiert, das der Karte hinzugefügt werden kann.

In den folgenden Schritten wird veranschaulicht, wie Sie eine GeoJSON-Datei in die Anwendung importieren und als `FeatureCollection`-GeoJSON-Objekt deserialisieren.

1. Schließen Sie den [Schnellstart: Erstellen einer Android-App](quick-android-map.md) ab, da die folgenden Schritte auf dieser Anwendung aufbauen.
2. Klicken Sie im Projektbereich von Android Studio mit der rechten Maustaste auf den Ordner **app**, und navigieren Sie zu `New > Folder > Assets Folder`.
3. Ziehen Sie die GeoJSON-Datei mit den [Point of Interest-Beispielen](https://raw.githubusercontent.com/Azure-Samples/AzureMapsCodeSamples/master/AzureMapsCodeSamples/Common/data/geojson/SamplePoiDataSet.json) in den Ressourcenordner.

::: zone pivot="programming-language-java-android"

4. Erstellen Sie eine neue Datei mit dem Namen **Utils.java**, und fügen Sie ihr den folgenden Code hinzu. Dieser Code enthält die statische `importData`-Methode, mit der ein asynchroner Importvorgang einer Datei aus dem Ordner `assets` der Anwendung oder aus dem Web durchgeführt wird, indem eine URL als Zeichenfolge verwendet wird. Anschließend wird sie mit einer einfachen Rückrufmethode an den UI-Thread zurückgegeben.

    ```java
    //Modify the package name as needed to align with your application.
    package com.example.myapplication;
    
    import android.content.Context;
    import android.os.Handler;
    import android.os.Looper;
    import android.webkit.URLUtil;
    
    import java.io.BufferedReader;
    import java.io.IOException;
    import java.io.InputStream;
    import java.io.InputStreamReader;
    import java.net.URL;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;
    
    import javax.net.ssl.HttpsURLConnection;
    
    public class Utils {
    
        interface SimpleCallback {
            void notify(String result);
        }
    
        /**
         * Imports data from a web url or asset file name and returns it to a callback.
         * @param urlOrFileName A web url or asset file name that points to data to load.
         * @param context The context of the app.
         * @param callback The callback function to return the data to.
         */
        public static void importData(String urlOrFileName, Context context, SimpleCallback callback){
            importData(urlOrFileName, context, callback, null);
        }
        
        /**
         * Imports data from a web url or asset file name and returns it to a callback.
         * @param urlOrFileName A web url or asset file name that points to data to load.
         * @param context The context of the app.
         * @param callback The callback function to return the data to.
         * @param error A callback function to return errors to.
         */
        public static void importData(String urlOrFileName, Context context, SimpleCallback callback, SimpleCallback error){
            if(urlOrFileName != null && callback != null) {
                ExecutorService executor = Executors.newSingleThreadExecutor();
                Handler handler = new Handler(Looper.getMainLooper());
    
                executor.execute(() -> {
                    String data = null;
    
                    try {
    
                        if(URLUtil.isNetworkUrl(urlOrFileName)){
                            data = importFromWeb(urlOrFileName);
                        } else {
                            //Assume file is in assets folder.
                            data = importFromAssets(context, urlOrFileName);
                        }
    
                        final String result = data;
    
                        handler.post(() -> {
                            //Ensure the resulting data string is not null or empty.
                            if (result != null && !result.isEmpty()) {
                                callback.notify(result);
                            } else {
                                error.notify("No data imported.");
                            }
                        });
                    } catch(Exception e) {
                        if(error != null){
                            error.notify(e.getMessage());
                        }
                    }
                });
            }
        }
    
        /**
         * Imports data from an assets file as a string.
         * @param context The context of the app.
         * @param fileName The asset file name.
         * @return
         * @throws IOException
         */
        private static String importFromAssets(Context context, String fileName) throws IOException {
            InputStream stream = null;
    
            try {
                stream = context.getAssets().open(fileName);
    
                if(stream != null) {
                    return readStreamAsString(stream);
                }
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                // Close Stream and disconnect HTTPS connection.
                if (stream != null) {
                    stream.close();
                }
            }
    
            return null;
        }
    
        /**
         * Imports data from the web as a string.
         * @param url URL to the data.
         * @return
         * @throws IOException
         */
        private static String importFromWeb(String url) throws IOException {
            InputStream stream = null;
            HttpsURLConnection connection = null;
            String result = null;
    
            try {
                connection = (HttpsURLConnection) new URL(url).openConnection();
    
                //For this use case, set HTTP method to GET.
                connection.setRequestMethod("GET");
    
                //Open communications link (network traffic occurs here).
                connection.connect();
    
                int responseCode = connection.getResponseCode();
                if (responseCode != HttpsURLConnection.HTTP_OK) {
                    throw new IOException("HTTP error code: " + responseCode);
                }
    
                //Retrieve the response body as an InputStream.
                stream = connection.getInputStream();
    
                if (stream != null) {
                    return readStreamAsString(stream);
                }
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                // Close Stream and disconnect HTTPS connection.
                if (stream != null) {
                    stream.close();
                }
                if (connection != null) {
                    connection.disconnect();
                }
            }
    
            return result;
        }
    
        /**
         * Reads an input stream as a string.
         * @param stream Stream to convert.
         * @return
         * @throws IOException
         */
        private static String readStreamAsString(InputStream stream) throws IOException {
            //Convert the contents of an InputStream to a String.
            BufferedReader in = new BufferedReader(new InputStreamReader(stream, "UTF-8"));
    
            String inputLine;
            StringBuffer response = new StringBuffer();
    
            while ((inputLine = in.readLine()) != null) {
                response.append(inputLine);
            }
    
            in.close();
    
            return response.toString();
        }
    }
    ```

5. Öffnen Sie die Datei **MainActivity.java**, und fügen Sie im Rückruf für das Ereignis `mapControl.onReady` den folgenden Code hinzu. Das Ereignis befindet sich innerhalb der `onCreate`-Methode. Bei diesem Code wird das Importhilfsprogramm genutzt, um die Datei **SamplePoiDataSet.json** als Zeichenfolge einzulesen. Anschließend erfolgt die Deserialisierung als Funktionssammlung, indem die statische `fromJson`-Methode der `FeatureCollection`-Klasse verwendet wird. Darüber hinaus wird mit diesem Code auch der Begrenzungsrahmenbereich für alle Daten der Funktionssammlung berechnet und genutzt, um den Fokus der Kartenkamera auf die Daten festzulegen.

    ```java
    //Create a data source and add it to the map.
    DataSource source = new DataSource();
    map.sources.add(source);
    
    //Import the GeoJSON data and add it to the data source.
    Utils.importData("SamplePoiDataSet.json",
        this,
        (String result) -> {
            //Parse the data as a GeoJSON Feature Collection.
            FeatureCollection fc = FeatureCollection.fromJson(result);
    
            //Add the feature collection to the data source.
            source.add(fc);
    
            //Optionally, update the maps camera to focus in on the data.
    
            //Calculate the bounding box of all the data in the Feature Collection.
            BoundingBox bbox = MapMath.fromData(fc);
    
            //Update the maps camera so it is focused on the data.
            map.setCamera(
                bounds(bbox),

                //Padding added to account for pixel size of rendered points.
                padding(20)
            );
        });
    ```

6. Wenn Sie den Code zum Laden der GeoJSON-Daten als Datenquelle verwenden, müssen Sie nun angeben, wie diese Daten in der Karte angezeigt werden sollen. Es gibt mehrere unterschiedliche Renderingebenen für Punktdaten. [Blasenebenen](map-add-bubble-layer-android.md), [Symbolebenen](how-to-add-symbol-to-android-map.md) und [Wärmebildebenen](map-add-heat-map-layer-android.md) werden am häufigsten verwendet. Fügen Sie den folgenden Code, mit dem die Daten auf einer Blasenebene im Rückruf für das Ereignis `mapControl.onReady` gerendert werden, nach dem Code für den Datenimport hinzu.

    ```java
    //Create a layer and add it to the map.
    BubbleLayer layer = new BubbleLayer(source);
    map.layers.add(layer);
    ```

::: zone-end

::: zone pivot="programming-language-kotlin"

4. Erstellen Sie eine neue Datei mit dem Namen **Utils.kt**, und fügen Sie ihr den folgenden Code hinzu. Dieser Code enthält die statische `importData`-Methode, mit der ein asynchroner Importvorgang einer Datei aus dem Ordner `assets` der Anwendung oder aus dem Web durchgeführt wird, indem eine URL als Zeichenfolge verwendet wird. Anschließend wird sie mit einer einfachen Rückrufmethode an den UI-Thread zurückgegeben.

    ```kotlin
    //Modify the package name as needed to align with your application.
    package com.example.myapplication;

    import android.content.Context
    import android.os.Handler
    import android.os.Looper
    import android.webkit.URLUtil
    import java.net.URL
    import java.util.concurrent.ExecutorService
    import java.util.concurrent.Executors
    
    class Utils {
        companion object {
    
            /**
             * Imports data from a web url or asset file name and returns it to a callback.
             * @param urlOrFileName A web url or asset file name that points to data to load.
             * @param context The context of the app.
             * @param callback The callback function to return the data to.
             */
            fun importData(urlOrFileName: String?, context: Context, callback: (String?) -> Unit) {
                importData(urlOrFileName, context, callback, null)
            }
    
            /**
             * Imports data from a web url or asset file name and returns it to a callback.
             * @param urlOrFileName A web url or asset file name that points to data to load.
             * @param context The context of the app.
             * @param callback The callback function to return the data to.
             * @param error A callback function to return errors to.
             */
            public fun importData(urlOrFileName: String?, context: Context, callback: (String?) -> Unit, error: ((String?) -> Unit)?) {
                if (urlOrFileName != null && callback != null) {
                    val executor: ExecutorService = Executors.newSingleThreadExecutor()
                    val handler = Handler(Looper.getMainLooper())
                    executor.execute {
                        var data: String? = null
                        
                        try {
                            data = if (URLUtil.isNetworkUrl(urlOrFileName)) {
                                URL(urlOrFileName).readText()
                            } else { //Assume file is in assets folder.
                                context.assets.open(urlOrFileName).bufferedReader().use{
                                    it.readText()
                                }
                            }
    
                            handler.post {
                                //Ensure the resulting data string is not null or empty.
                                if (data != null && !data.isEmpty()) {
                                    callback(data)
                                } else {
                                    error!!("No data imported.")
                                }
                            }
                        } catch (e: Exception) {
                            error!!(e.message)
                        }
                    }
                }
            }
        }
    }
    ```

5. Öffnen Sie die Datei **MainActivity.kt**, und fügen Sie im Rückruf für das Ereignis `mapControl.onReady` den folgenden Code hinzu. Das Ereignis befindet sich innerhalb der `onCreate`-Methode. Bei diesem Code wird das Importhilfsprogramm genutzt, um die Datei **SamplePoiDataSet.json** als Zeichenfolge einzulesen. Anschließend erfolgt die Deserialisierung als Funktionssammlung, indem die statische `fromJson`-Methode der `FeatureCollection`-Klasse verwendet wird. Darüber hinaus wird mit diesem Code auch der Begrenzungsrahmenbereich für alle Daten der Funktionssammlung berechnet und genutzt, um den Fokus der Kartenkamera auf die Daten festzulegen.

    ```kotlin
    //Create a data source and add it to the map.
    DataSource source = new DataSource();
    map.sources.add(source);
    
    //Import the GeoJSON data and add it to the data source.
    Utils.importData("SamplePoiDataSet.json", this) { 
        result: String? ->
            //Parse the data as a GeoJSON Feature Collection.
             val fc = FeatureCollection.fromJson(result!!)
    
            //Add the feature collection to the data source.
            source.add(fc)
    
            //Optionally, update the maps camera to focus in on the data.
    
            //Calculate the bounding box of all the data in the Feature Collection.
            val bbox = MapMath.fromData(fc);

            //Update the maps camera so it is focused on the data.
            map.setCamera(
                bounds(bbox),

                //Padding added to account for pixel size of rendered points.
                padding(20)
            )
        }
    ```

6. Wenn Sie den Code zum Laden der GeoJSON-Daten als Datenquelle verwenden, müssen Sie nun angeben, wie diese Daten in der Karte angezeigt werden sollen. Es gibt mehrere unterschiedliche Renderingebenen für Punktdaten. [Blasenebenen](map-add-bubble-layer-android.md), [Symbolebenen](how-to-add-symbol-to-android-map.md) und [Wärmebildebenen](map-add-heat-map-layer-android.md) werden am häufigsten verwendet. Fügen Sie den folgenden Code, mit dem die Daten auf einer Blasenebene im Rückruf für das Ereignis `mapControl.onReady` gerendert werden, nach dem Code für den Datenimport hinzu.

    ```kotlin
    //Create a layer and add it to the map.
    val layer = new BubbleLayer(source)
    map.layers.add(layer)
    ```

::: zone-end

7. Führen Sie die Anwendung aus. Es wird eine Kartenansicht der USA angezeigt, die sich überlagernde Kreise für die einzelnen Standorte der GeoJSON-Datei enthält.

    ![Karte der USA mit angezeigten Daten aus einer GeoJSON-Datei](media/tutorial-load-geojson-file-android/android-import-geojson.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Führen Sie die folgenden Schritte aus, um die Ressourcen dieses Tutorials zu bereinigen:

1. Schließen Sie Android Studio, und löschen Sie die von Ihnen erstellte Anwendung.
2. Falls Sie die Anwendung auf einem externen Gerät getestet haben, sollten Sie die Anwendung auf diesem Gerät deinstallieren.

## <a name="next-steps"></a>Nächste Schritte

Weitere Codebeispiele und eine Benutzeroberfläche für das interaktive Codieren:

> [!div class="nextstepaction"]
> [Verwenden von datengesteuerten Formatvorlagenausdrücken](data-driven-style-expressions-android-sdk.md)

> [!div class="nextstepaction"]
> [Anzeigen von Featureinformationen](display-feature-information-android.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Symbolebene](how-to-add-symbol-to-android-map.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Linienebene](android-map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Polygonebene](how-to-add-shapes-to-android-map.md)
