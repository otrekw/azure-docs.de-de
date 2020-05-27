---
title: Verwenden der Azure Maps-Schnellansicht für Zeichnungsfehler
description: In diesem Artikel erfahren Sie, wie Sie von der Creator-Konvertierungs-API zurückgegebene Warnungen und Fehler visualisieren.
author: anastasia-ms
ms.author: v-stharr
ms.date: 05/18/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: e1b5b16d5522285f2d028303f3295cc1fb740330
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83596659"
---
# <a name="using-the-azure-maps-drawing-error-visualizer"></a>Verwenden der Azure Maps-Schnellansicht für Zeichnungsfehler

Die Schnellansicht für Zeichnungsfehler ist eine eigenständige Webanwendung zum Anzeigen von [Warnungen und Fehlern im Zusammenhang mit Zeichnungspaketen](drawing-conversion-error-codes.md), die im Zuge des Konvertierungsprozesses erkannt wurden. Bei der Webanwendung für die Fehlerschnellansicht handelt es sich um eine statische Seite, die ohne Internetverbindung verwendet werden kann.  Die Fehlerschnellansicht kann zur Behebung von Fehlern und Warnungen verwendet werden, um die [Anforderungen für Zeichnungspakete](drawing-requirements.md) zu erfüllen. Im Falle eines erkannten Fehlers wird von der [Azure Maps-Konvertierungs-API](https://docs.microsoft.com/rest/api/maps/data/conversion) lediglich eine Antwort mit einem Link zur Fehlerschnellansicht zurückgegeben.

## <a name="prerequisites"></a>Voraussetzungen

Vor dem Herunterladen der Schnellansicht für Zeichnungsfehler müssen folgende Schritte ausgeführt werden:

1. [Erstellen eines Azure Maps-Kontos](quick-demo-map-app.md#create-an-account-with-azure-maps)
2. [Abrufen eines Primärschlüssels](quick-demo-map-app.md#get-the-primary-key-for-your-account) (auch primärer Schlüssel oder Abonnementschlüssel genannt)
3. [Erstellen einer Creator-Ressource](how-to-manage-creator.md)

In diesem Tutorial wird die Anwendung [Postman](https://www.postman.com/) verwendet. Sie können aber auch eine andere API-Entwicklungsumgebung verwenden.

## <a name="download"></a>Download

1. Laden Sie Ihr Zeichnungspaket in den Azure Maps Creator-Dienst hoch, um eine eindeutige Daten-ID (`udid`) für das hochgeladene Paket zu erhalten. Die Schritte zum Hochladen eines Pakets finden Sie unter [Hochladen eines Zeichnungspakets](tutorial-creator-indoor-maps.md#upload-a-drawing-package).

2. Nach dem Hochladen des Zeichnungspakets wird die eindeutige Daten-ID (`udid`) für das hochgeladene Paket verwendet, um das Paket in Kartendaten zu konvertieren. Die Schritte zum Konvertieren eines Pakets finden Sie unter [Konvertieren eines Zeichnungspakets](tutorial-creator-indoor-maps.md#convert-a-drawing-package).

    >[!NOTE]
    >Nach erfolgreichem Abschluss des Konvertierungsvorgangs wird kein Link zum Tool für die Fehlerschnellansicht zurückgegeben.

3. Sehen Sie sich in der Postman-Anwendung auf der Registerkarte **Headers** (Header) die von der Konvertierungs-API zurückgegebene Eigenschaft `diagnosticPackageLocation` an. Die Antwort sollte in etwa wie der folgende JSON-Code aussehen:

    ```json
    {
        "operationId": "77dc9262-d3b8-4e32-b65d-74d785b53504",
        "created": "2020-04-22T19:39:54.9518496+00:00",
        "status": "Failed",
        "resourceLocation": "https://atlas.microsoft.com/conversion/{conversionId}?api-version=1.0",
        "properties": {
            "diagnosticPackageLocation": "https://atlas.microsoft.com/mapData/ce61c3c1-faa8-75b7-349f-d863f6523748?api-version=1.0"
        }
    }
    ```

4. Laden Sie die Schnellansicht für Zeichnungspaketfehler herunter, indem Sie eine Anforderung vom Typ `HTTP-GET` an die URL `diagnosticPackageLocation` senden.

## <a name="setup"></a>Einrichten

Das unter dem Link `diagnosticPackageLocation` heruntergeladene ZIP-Paket enthält zwei Dateien:

* _VisualizationTool.zip_: Enthält den Quellcode, die Medien und die Webseite für die Schnellansicht für Zeichnungsfehler.
* _ConversionWarningsAndErrors.json_: Enthält eine formatierte Liste mit Warnungen, Fehlern und zusätzlichen Details, die von der Schnellansicht für Zeichnungsfehler verwendet werden.

Entzippen Sie den Ordner _VisualizationTool.zip_. Er enthält folgende Elemente:

* Ordner _assets_: Bilder und Mediendateien
* Ordner _static_: Quellcode
* Datei _index.html_: die Webanwendung

Öffnen Sie die Datei _index.html_ mit einer der im Anschluss angegebenen Browserversionen. Sie können auch eine andere Version verwenden, sofern diese ein vergleichbar kompatibles Verhalten wie die aufgeführte Version bietet.

* Microsoft Edge 80
* Safari 13
* Chrome 80
* Firefox 74

## <a name="using-the-drawing-error-visualizer-tool"></a>Verwenden des Schnellansichtstools für Zeichnungsfehler

Nachdem Sie das Schnellansichtstool für Zeichnungsfehler gestartet haben, wird die Uploadseite angezeigt. Die Uploadseite enthält ein Drag & Drop-Feld. Das Drag & Drop-Feld kann auch als Schaltfläche zum Starten eines Datei-Explorer-Dialogfelds verwendet werden.

![Schnellansicht-App für Zeichnungsfehler: Startseite](./media/drawing-errors-visualizer/start-page.png)

Die Datei _ConversionWarningsAndErrors.json_ wurde am Stamm des heruntergeladenen Verzeichnisses platziert. Zum Laden der Datei _ConversionWarningsAndErrors.json_ können Sie sie entweder per Drag & Drop auf das Feld ziehen oder auf das Feld klicken, im Datei-Explorer-Dialogfeld zu der Datei navigieren und sie anschließend hochladen.

![Schnellansicht-App für Zeichnungsfehler: Drag & Drop-Bereich zum Laden von Daten](./media/drawing-errors-visualizer/loading-data.gif)

Nachdem die Datei _ConversionWarningsAndErrors.json_ geladen wurde, wird eine Liste Ihrer Zeichnungspaketfehler und -warnungen angezeigt. Für alle Fehler bzw. Warnungen sind jeweils die Schicht, die Ebene und eine ausführliche Meldung angegeben. Nun können Sie zu den einzelnen Fehlern navigieren, um weitere Informationen zur Fehlerbehebung zu erhalten.  

![Schnellansicht-App für Zeichnungsfehler: Fehler und Warnungen](./media/drawing-errors-visualizer/errors.png)

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr [Zeichnungspaket den Anforderungen entspricht](drawing-requirements.md), können Sie es mithilfe des [Azure Maps-Konvertierungsdiensts](https://docs.microsoft.com/rest/api/maps/data/conversion) in ein Dataset konvertieren. Anschließend können Sie das Webmodul für Gebäudepläne verwenden, um Ihre Anwendung zu entwickeln. Weitere Informationen finden Sie in den folgenden Artikeln:

> [!div class="nextstepaction"]
> [Fehler und Warnungen bei der Zeichnungskonvertierung](drawing-conversion-error-codes.md)

> [!div class="nextstepaction"]
> [Creator für Gebäudepläne](creator-indoor-maps.md)

> [!div class="nextstepaction"]
> [Verwenden des Moduls „Gebäudepläne“ von Azure Maps](how-to-use-indoor-module.md)

> [!div class="nextstepaction"]
> [Implementieren dynamischer Stile für Creator-Gebäudepläne](indoor-map-dynamic-styling.md)