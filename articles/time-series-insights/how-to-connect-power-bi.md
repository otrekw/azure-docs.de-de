---
title: Verbinden Ihrer Umgebung mit Power BI – Azure Time Series Insights | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure Time Series Insights mit Power BI verbinden können, um Daten unternehmensweit freizugeben, aufzuzeichnen und anzuzeigen.
author: shreyasharmamsft
ms.author: shresha
manager: dpalled
services: time-series-insights
ms.service: time-series-insights
ms.topic: conceptual
ms.date: 12/14/2020
ms.openlocfilehash: b4ed5a419df97f98b883a07825184122945e092e
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/27/2021
ms.locfileid: "98879560"
---
# <a name="visualize-data-from-azure-time-series-insights-in-power-bi"></a>Visualisieren der Daten von Azure Time Series Insights in Power BI

Azure Time Series Insights ist eine Plattform zum Speichern, Verwalten, Abfragen und Visualisieren von Zeitreihendaten in der Cloud. [Power BI](https://powerbi.microsoft.com) ist ein Tool zur Unternehmensanalyse mit umfangreichen Visualisierungsfunktionen, mit dem Sie Erkenntnisse und Ergebnisse unternehmensweit teilen können. Beide Dienste können jetzt integriert werden, um die leistungsstarken Analysefunktionen von Azure Time Series Insights mit den nützlichen Datenvisualisierungsfunktionen und einfachen Freigabeoptionen von Power BI zu verbinden.

Sie lernen Folgendes:

* Herstellen einer Verbindung für Azure Time Series Insights mit Power BI über den nativen Azure Time Series Insights-Connector
* Erstellen von visuellen Elementen mit Ihren Zeitreihendaten in Power BI
* Veröffentlichen des Berichts für Power BI und Teilen mit dem restlichen Unternehmen


## <a name="prerequisites"></a>Voraussetzungen

* Registrieren Sie sich für ein [kostenloses Azure-Abonnement](https://azure.microsoft.com/free/), falls Sie noch keins besitzen.
* Herunterladen und Installieren der aktuellen Version von [Power BI Desktop](https://powerbi.microsoft.com/downloads/)
* [Azure Time Series Insights Gen2-Umgebung](./how-to-provision-manage.md). Erstellen Sie eine, falls Sie nicht über eine solche Umgebung verfügen.

Lesen Sie den Artikel [Gewähren von Datenzugriff für eine Umgebung](./concepts-access-policies.md), und stellen Sie sicher, dass Sie entweder über Direkt- oder Gastzugriff auf die Azure Time Series Insights Gen2-Umgebung verfügen. 

> [!IMPORTANT]
> * Führen Sie den Download und die Installation der aktuellen Version von [Power BI Desktop](https://powerbi.microsoft.com/downloads/) durch. Vergewissern Sie sich, dass Sie mindestens die Version vom Dezember 2020 (2.88.321.0) von Power BI Desktop installiert haben, damit Sie die Schritte in diesem Artikel ausführen können. 

## <a name="connect-data-from-azure-time-series-insights-to-power-bi"></a>Verbinden der Daten von Azure Time Series Insights mit Power BI

### <a name="1-export-data-into-power-bi-desktop"></a>1. Exportieren Sie Daten in Power BI Desktop.

Erste Schritte:

1. Öffnen Sie Azure Time Series Insights Gen2-Explorer, und stellen Sie Ihre Daten zusammen. Dies sind die Daten, die nach Power BI exportiert werden.
1. Nachdem Sie eine Ansicht erstellt haben, mit der Sie zufrieden sind, navigieren Sie zum Dropdownmenü **Weitere Aktionen**, und wählen Sie dann **Mit Power BI verbinden** aus.

    [![Export von Azure Time Series Insights Gen2-Explorer](media/how-to-connect-power-bi/export-from-explorer.jpg)](media/how-to-connect-power-bi/export-from-explorer.jpg#lightbox)

1. Legen Sie Ihre Parameter für den Export fest:

   * **Datenformat:** Wählen Sie aus, ob Sie **aggregierte Daten** oder **Rohereignisse** nach Power BI exportieren möchten. 

       > [!NOTE]
       > * Wenn Sie Rohereignisse exportieren, können Sie diese Daten später in Power BI aggregieren. Nach dem Export von aggregierten Daten ist die Umstellung auf Rohdaten in Power BI nicht mehr möglich. 
       > * Für Daten der Ebene „Rohereignisse“ gilt ein Grenzwert von 250.000 für die Ereignisanzahl.

   * **Zeitbereich**: Geben Sie an, ob in Power BI ein **fester** Zeitbereich oder die **aktuellen** Daten angezeigt werden sollen. Bei Auswahl eines festen Zeitraums werden die Daten nach Power BI exportiert, die im von Ihnen angegebenen Suchzeitraum enthalten sind. Die Auswahl des aktuellen Zeitbereichs bedeutet, dass für Power BI die aktuellen Daten für den von Ihnen gewählten Suchzeitraum exportiert werden. (Wenn Sie beispielsweise eine beliebige Stunde mit Daten und die Einstellung „Aktuell“ auswählen, werden vom Power BI-Connector immer Abfragen für die Daten der aktuellen Stunde durchgeführt.)
  
   * **Speichertyp**: Wählen Sie aus, ob Ihre ausgewählte Abfrage für **Warm Storage** oder **Cold Storage** durchgeführt werden soll. 

    > [!TIP]
    > * Vom Azure Time Series Insights-Explorer werden die empfohlenen Parameter anhand der Daten, die Sie für den Export festgelegt haben, automatisch ausgewählt. 

1. Wählen Sie die Option **Abfrage in Zwischenablage kopieren** aus, nachdem Sie Ihre Einstellungen konfiguriert haben.

    [![Azure Time Series Insights-Explorer: Modales Dialogfeld](media/how-to-connect-power-bi/choose-explorer-parameters.jpg)](media/how-to-connect-power-bi/choose-explorer-parameters.jpg#lightbox)

2. Starten Sie Power BI Desktop.
   
3. Wählen Sie in Power BI Desktop auf der Registerkarte **Start** die Option **Daten abrufen** in der oberen linken Ecke und anschließend **Mehr** aus.

    [![Abrufen von Daten in Power BI](media/how-to-connect-power-bi/get-data-power-bi.jpg)](media/how-to-connect-power-bi/get-data-power-bi.jpg#lightbox)

4. Suchen Sie nach **Azure Time Series Insights**, und wählen Sie **Azure Time Series Insights (Beta)** und dann **Verbinden** aus.

    [![Verbinden von Power BI mit Azure Time Series Insights](media/how-to-connect-power-bi/select-tsi-connector.jpg)](media/how-to-connect-power-bi/select-tsi-connector.jpg#lightbox)

    Alternativ können Sie auch zur Registerkarte **Azure** navigieren, **Azure Time Series Insights (Beta)** und dann **Verbinden** auswählen.

5. Fügen Sie die Abfrage, die Sie aus dem Azure Time Series Insights-Explorer kopiert haben, in das Feld **Benutzerdefinierte Abfrage** ein, und wählen Sie dann **OK** aus.

    [![Einfügen der benutzerdefinierten Abfrage und Auswählen von „OK“](media/how-to-connect-power-bi/custom-query-load.png)](media/how-to-connect-power-bi/custom-query-load.png#lightbox)  

6.  Die Datentabelle wird jetzt geladen. Drücken Sie **Laden**, um sie in Power BI zu laden. Falls Sie Transformationen für die Daten durchführen möchten, haben Sie jetzt die Möglichkeit dazu, indem Sie auf **Daten transformieren** klicken. Sie können Ihre Daten auch transformieren, nachdem sie geladen wurden.

    [![Überprüfen der Daten in der Tabelle und Auswählen von „Laden“](media/how-to-connect-power-bi/review-the-loaded-data-table.png)](media/how-to-connect-power-bi/review-the-loaded-data-table.png#lightbox)  

## <a name="create-a-report-with-visuals"></a>Erstellen eines Berichts mit Visuals

Nachdem Sie die Daten jetzt in Power BI importiert haben, ist es an der Zeit, einen Bericht mit visuellen Elementen zu erstellen.

1. Stellen Sie auf der linken Seite des Fensters sicher, dass Sie die Ansicht **Bericht** ausgewählt haben.

    [![Screenshot, der das Symbol der Berichtsansicht zeigt.](media/how-to-connect-power-bi/select-the-report-view.png)](media/how-to-connect-power-bi/select-the-report-view.png#lightbox)

1. Wählen Sie in der Spalte **Visualisierungen** das gewünschte visuelle Element aus. Wählen Sie z. B. **Liniendiagramm** aus. Dadurch wird Ihrem Canvas ein leeres Liniendiagramm hinzugefügt.

1.  Wählen Sie in der Liste **Felder** die Option **_Timestamp** (Zeitstempel) aus, und ziehen Sie sie in das Feld **Achse**, um Zeitdaten entlang der X-Achse anzuzeigen. Stellen Sie sicher, dass als Wert für die **Achse** zu **_Timestamp** gewechselt wird (Standard ist **Datumshierarchie**).

    [![Auswählen von „_Timestamp“](media/how-to-connect-power-bi/select-timestamp.png)](media/how-to-connect-power-bi/select-timestamp.png#lightbox)

1.  Wählen Sie in der Liste **Felder** die Variable aus, die dargestellt werden soll, und ziehen Sie sie in das Feld **Werte**, um die Werte entlang der Y-Achse anzuzeigen. Wählen Sie Ihren Wert für die Zeitreihen-ID aus, und ziehen Sie ihn in das Feld **Legende**, um im Diagramm mehrere Linien zu erstellen (eine pro Zeitreihen-ID). Dies führt zu einer Anzeige, die der Anzeige des Azure Time Series Insights-Explorers ähnelt! 

    [![Erstellen eines einfachen Liniendiagramms](media/how-to-connect-power-bi/power-bi-line-chart.png)](media/how-to-connect-power-bi/power-bi-line-chart.png#lightbox)

1. Um ein weiteres Diagramm zum Canvas hinzuzufügen, klicken Sie an eine beliebige Stelle auf dem Canvas außerhalb des Liniendiagramms, und wiederholen Sie diesen Vorgang.

    [![Erstellen zusätzlicher freizugebender Diagramme](media/how-to-connect-power-bi/power-bi-additional-charts.png)](media/how-to-connect-power-bi/power-bi-additional-charts.png#lightbox)

Nachdem Sie Ihren Bericht erstellt haben, können Sie ihn für die Reporting Services von Power BI veröffentlichen und für andere Personen in Ihrer Organisation freigeben.

## <a name="advanced-editing"></a>Erweiterte Bearbeitung
Wenn Sie bereits ein Dataset in Power BI geladen haben, die Abfrage aber ändern möchten (z. B. Datum/Uhrzeit oder Umgebungs-ID-Parameter), können Sie dies über die „Erweiterter Editor“-Funktion von Power BI erreichen. Weitere Informationen zum Vornehmen von Änderungen mit dem **Power Query-Editor** finden Sie in der [Power BI-Dokumentation](/power-bi/desktop-query-overview). 

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu [Power BI Desktop](/power-bi/desktop-query-overview).

* Lesen Sie die weiteren Informationen zum [Abfragen von Daten](concepts-query-overview.md) in Azure Time Series Insights Gen2.