---
title: Anzeigen von Verkehrsdaten auf Android-Karten | Microsoft Azure Maps
description: In diesem Artikel erfahren Sie, wie Sie mithilfe des Android SDK für Microsoft Azure Maps auf einer Karte Verkehrsdaten anzeigen.
author: rbrundritt
ms.author: richbrun
ms.date: 12/04/2020
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 113f39ac2976b870c9e07851cdd0919e2578940f
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2020
ms.locfileid: "97680460"
---
# <a name="show-traffic-data-on-the-map-android-sdk"></a>Anzeigen von Verkehrsdaten auf der Karte (Android SDK)

Flussdaten und Störungsdaten sind die zwei Arten von Verkehrsdaten, die auf der Karte angezeigt werden können. In diesem Leitfaden wird gezeigt, wie Sie beide Arten von Verkehrsdaten anzeigen können. Störungsdaten bestehen aus punkt- und linienbasierten Daten für Dinge wie Baustellen, Straßensperrungen und Unfälle. Flussdaten zeigen Metriken zum Verkehrsfluss auf der Straße.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass Sie die Schritte im Dokument [Schnellstart: Erstellen einer Android-App](quick-android-map.md) ausführen. Codeblöcke in diesem Artikel können in den `onReady`-Ereignishandler von Karten eingefügt werden.

## <a name="show-traffic-on-the-map"></a>Anzeigen von Datenverkehr auf einer Karte

In Azure Maps sind zwei Arten von Verkehrsdaten verfügbar:

- Vorfallsdaten: Punkt- und linienbasierte Daten, z. B. für Baustellen, Straßensperrungen und Unfälle.
- Flussdaten: Stellt Metriken zum Verkehrsfluss auf Straßen bereit. Daten zum Verkehrsfluss werden häufig verwendet, um Straßen farbig zu markieren. Die Farben basieren auf dem Verkehrsaufkommen, das aufgrund einer Geschwindigkeitsbeschränkung oder anderen Metrik zu einer Verlangsamung des Verkehrsflusses führt. An die Verkehrsflussoption `flow` der Karte können vier Werte übergeben werden.

    |Wert des Verkehrsflusses | BESCHREIBUNG|
    | :-- | :-- |
    | TrafficFlow.NONE | Zeigt keine Verkehrsdaten auf der Karte an. |
    | TrafficFlow.RELATIVE | Zeigt Verkehrsdaten an, die relativ zur Geschwindigkeit bei ungehindertem Verkehrsfluss auf der Straße sind. |
    | TrafficFlow.RELATIVE_DELAY | Zeigt Bereiche mit einer langsameren Geschwindigkeit als der durchschnittlichen erwarteten Verzögerung an. |
    | TrafficFlow.ABSOLUTE | Zeigt die absolute Geschwindigkeit aller Fahrzeuge auf der Straße an. |

Der folgende Code verdeutlicht, wie Sie Verkehrsdaten auf der Karte anzeigen.

```java
//Show traffic on the map using the traffic options.
map.setTraffic(
    incidents(true),
    flow(TrafficFlow.RELATIVE)
);
```

Der folgende Screenshot veranschaulicht, wie durch den oben aufgeführten Code Verkehrsinformationen in Echtzeit auf der Karte gerendert werden.

![Karte mit Verkehrsinformationen in Echtzeit](media/how-to-show-traffic-android/android-show-traffic.png)

## <a name="get-traffic-incident-details"></a>Abrufen von Details zu Verkehrsstörungen

Details zu Verkehrsstörungen sind in den Eigenschaften des Features verfügbar, das zum Anzeigen der Störung auf der Karte verwendet wird. Verkehrsstörungen werden der Karte mithilfe des Vektorkacheldiensts für Verkehrsstörungen in Azure Maps hinzugefügt. Das Format der Daten in diesen Vektorkacheln ist [hier dokumentiert](https://developer.tomtom.com/traffic-api/traffic-api-documentation-traffic-incidents/vector-incident-tiles). Durch den folgenden Code werden der Karte ein Click-Ereignis hinzugefügt, das Feature für Verkehrsstörungen abgerufen, auf das geklickt wurde, und eine Popupmeldung mit einigen Details angezeigt.

```java
//Show traffic information on the map.
map.setTraffic(
    incidents(true),
    flow(TrafficFlow.RELATIVE)
);

//Add a click event to the map.
map.events.add((OnFeatureClick) (features) -> {

    if (features != null && features.size() > 0) {
        Feature incident = features.get(0);

        //Ensure that the clicked feature is an traffic incident feature.
        if (incident.properties() != null && incident.hasProperty("incidentType")) {

            StringBuilder sb = new StringBuilder();
            String incidentType = incident.getStringProperty("incidentType");

            if (incidentType != null) {
                sb.append(incidentType);
            }

            if (sb.length() > 0) {
                sb.append("\n");
            }

            //If the road is closed, find out where it is closed from.
            if ("Road Closed".equals(incidentType)) {
                String from = incident.getStringProperty("from");

                if (from != null) {
                    sb.append(from);
                }
            } else {
                //Get the description of the traffic incident.
                String description = incident.getStringProperty("description");

                if (description != null) {
                    sb.append(description);
                }
            }

            String message = sb.toString();

            if (message.length() > 0) {
                Toast.makeText(this, message, Toast.LENGTH_LONG).show();
            }
        }
    }
});
```

Der folgende Screenshot veranschaulicht, wie durch den oben aufgeführten Code Verkehrsinformationen in Echtzeit auf der Karte gerendert und eine Popupmeldung mit Details zur Störung angezeigt werden.

![Karte mit Verkehrsinformationen in Echtzeit und einer Popupmeldung, die Details zur Störung enthält](media/how-to-show-traffic-android/android-traffic-details.png)

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die folgenden Leitfäden an, um zu erfahren, wie Sie Ihrer Karte weitere Daten hinzufügen:

> [!div class="nextstepaction"]
> [Hinzufügen einer Kachelebene](how-to-add-tile-layer-android-map.md)
