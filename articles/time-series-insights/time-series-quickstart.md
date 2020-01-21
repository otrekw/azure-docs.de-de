---
title: 'Schnellstart: Azure Time Series Insights-Explorer: Azure Time Series Insights | Microsoft-Dokumentation'
description: Erfahren Sie mehr zum Einstieg in den Azure Time Series Insights-Explorer. Visualisieren Sie große Mengen von IoT-Daten, und lernen Sie die wichtigsten Features Ihrer Umgebung anhand einer Tour kennen.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc seodec18
ms.date: 01/06/2020
ms.openlocfilehash: a905054b1b2a04fa2b7865d2c1065ccee37cffc0
ms.sourcegitcommit: 12a26f6682bfd1e264268b5d866547358728cd9a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2020
ms.locfileid: "75860429"
---
# <a name="quickstart-explore-azure-time-series-insights"></a>Schnellstart: Erkunden von Azure Time Series Insights

Dieser Schnellstart für den Azure Time Series Insights-Explorer unterstützt Sie bei den ersten Schritten mit Azure Time Series in einer kostenlosen Demoumgebung. In dieser Schnellstartanleitung erfahren Sie, wie Sie große IoT-Datenmengen im Webbrowser visualisieren, und lernen die wichtigsten Features kennen, die nun allgemein verfügbar sind.

Azure Time Series Insights ist ein vollständig verwalteter Analyse-, Speicher- und Visualisierungsdienst, der die simultane Untersuchung und Analyse von Milliarden von IoT-Ereignissen vereinfacht. Sie erhalten eine globale Ansicht Ihrer Daten und können basierend darauf Ihre IoT-Lösung schneller überprüfen und kostenintensive Ausfallzeiten unternehmenskritischer Geräte vermeiden. Mit Azure Time Series Insights können Sie versteckte Trends aufdecken, Anomalien erkennen und Ursachenanalysen nahezu in Echtzeit durchführen.

Zur Erhöhung der Flexibilität kann Azure Time Series Insights über seine leistungsfähigen [REST-APIs](./time-series-insights-update-tsq.md) und das [Client-SDK](https://github.com/microsoft/tsiclient) einer bereits vorhandenen Anwendung hinzugefügt werden. Die APIs ermöglichen es Ihnen, Zeitreihendaten in einer Clientanwendung Ihrer Wahl zu speichern, abzufragen und zu nutzen. Sie können auch das Client-SDK verwenden, um Ihrer vorhandenen Anwendung Benutzeroberflächenkomponenten hinzuzufügen.

In dieser Schnellstartanleitung zum Time Series Insights-Explorer werden die Features beschrieben, die nun allgemein verfügbar sind.

> [!IMPORTANT]
> Erstellen Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio), sofern noch nicht geschehen.

## <a name="prepare-the-demo-environment"></a>Vorbereiten der Demoumgebung

1. Navigieren Sie in Ihrem Browser zur [Demo der allgemeinen Verfügbarkeit](https://insights.timeseries.azure.com/demo).

1. Wenn Sie dazu aufgefordert werden, melden Sie sich mit Ihren Azure-Kontoanmeldeinformationen beim Time Series Insights-Explorer an.

1. Die Seite zur Schnelleinführung in Time Series Insights wird angezeigt. Wählen Sie **Weiter** aus, um die Schnelleinführung zu starten.

   [![Willkommensseite der Schnellstartanleitung: Auswählen von „Weiter“](media/quickstart/quickstart-welcome.png)](media/quickstart/quickstart-welcome.png#lightbox)

## <a name="explore-the-demo-environment"></a>Erkunden der Demoumgebung

1. Der **Zeitauswahlbereich** wird angezeigt. Wählen Sie in diesem Bereich den Zeitrahmen aus, der dargestellt werden soll.

   [![Zeitauswahlbereich](media/quickstart/quickstart-time-selection-panel.png)](media/quickstart/quickstart-time-selection-panel.png#lightbox)

1. Wählen Sie einen Zeitrahmen aus, und ziehen Sie ihn in den Bereich. Wählen Sie anschließend **Suchen** aus.

   [![Auswählen eines Zeitrahmens](media/quickstart/quickstart-select-time.png)](media/quickstart/quickstart-select-time.png#lightbox)

   In Time Series Insights wird eine Diagrammvisualisierung für den angegebenen Zeitrahmen angezeigt. Sie können verschiedene Aktionen im Liniendiagramm ausführen. Nutzen Sie beispielsweise Optionen zum Filtern, Anheften, Sortieren und Stapeln.

   Um zum **Zeitauswahlbereich** zurückzukehren, wählen Sie wie dargestellt den Pfeil nach unten aus:

   [![Diagramm](media/quickstart/quickstart-select-down-arrow.png)](media/quickstart/quickstart-select-down-arrow.png#lightbox)

1. Wählen Sie im **Begriffsbereich** die Option **Hinzufügen** aus, um einen neuen Suchbegriff hinzuzufügen.

   [![Bereich „Suchbegriffe hinzufügen“](media/quickstart/quickstart-add-terms.png)](media/quickstart/quickstart-add-terms.png#lightbox)

1. Im Diagramm können Sie eine Region auswählen, mit der rechten Maustaste auf die Region klicken und **Explore Events** (Ereignisse durchsuchen) auswählen.

   [![Ereignisse untersuchen](media/quickstart/quickstart-explore-events.png)](media/quickstart/quickstart-explore-events.png#lightbox)

   Ein Raster der Rohdaten aus der durchsuchten Region wird angezeigt.

   [![Ereignisse untersuchen: Rasterdatenansicht](media/quickstart/quickstart-explore-events-grid-data.png)](media/quickstart/quickstart-explore-events-grid-data.png#lightbox)

## <a name="select-and-filter-data"></a>Auswählen und Filtern von Daten

1. Bearbeiten Sie Ihre Begriffe, um die Werte im Diagramm zu ändern. Fügen Sie einen weiteren Begriff zum übergreifenden Korrelieren verschiedener Werttypen hinzu.

   [![Hinzufügen eines Begriffs](media/quickstart/quickstart-add-a-term.png)](media/quickstart/quickstart-add-a-term.png#lightbox)

1. Lassen Sie das Feld **Datenreihe filtern** leer, um alle ausgewählten Suchbegriffe anzuzeigen, oder geben Sie im Feld **Datenreihe filtern** einen Filterbegriff ein, um eine improvisierte Filterung von Datenreihen durchzuführen.

   [![Datenreihen filtern](media/quickstart/quickstart-filter-series.png)](media/quickstart/quickstart-filter-series.png#lightbox)

   Geben Sie für den Schnellstart **Station5** ein, um die Temperatur und den Druck für die Station übergreifend zu korrelieren.

Nach Abschluss des Schnellstarts können Sie das Beispiel-DataSet verwenden, um verschiedene Visualisierungen zu erstellen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Bereinigen Sie nach dem Tutorial die von Ihnen erstellten Ressourcen:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im Menü auf der linken Seite **Alle Ressourcen** aus, und suchen Sie Ihre Azure Time Series Insights-Ressourcengruppe.
1. Löschen Sie entweder die gesamte Ressourcengruppe (und alle darin enthaltenen Ressourcen), indem Sie auf **Löschen** klicken, oder entfernen Sie die Ressourcen einzeln.

## <a name="next-steps"></a>Nächste Schritte

Nun können Sie Ihre eigene Time Series Insights-Umgebung erstellen:
> [!div class="nextstepaction"]
> [Planen Ihrer Time Series Insights-Umgebung](time-series-insights-environment-planning.md)
