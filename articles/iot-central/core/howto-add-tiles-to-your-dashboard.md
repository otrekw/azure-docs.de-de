---
title: Hinzufügen von Kacheln zu Ihrem Dashboard | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie als Ersteller das Standarddashboard Ihrer Azure IoT Central-Anwendung konfigurieren.
author: mavoge
ms.author: mavoge
ms.date: 10/17/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: b13349ae4293f6377429e9dc72b6c2cb43f92348
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75435127"
---
# <a name="configure-the-application-dashboard"></a>Konfigurieren des Anwendungsdashboards

Das **Dashboard** ist die Seite, die geladen wird, wenn zugriffsberechtigte Benutzer zur URL der Anwendung navigieren. Wenn Sie die Anwendung auf Grundlage einer der **Anwendungsvorlagen** erstellt haben, startet Ihre Anwendung mit einem vordefinierten Dashboard. Wenn Sie die Anwendung über die Anwendungsvorlage **Legacyanwendung** erstellt haben, ist das Dashboard beim Start leer.

> [!NOTE]
> Zusätzlich zum Standarddashboard der Anwendung können Benutzer [mehrere Dashboards erstellen](howto-create-personal-dashboards.md). Diese Dashboards können nur für den Benutzer persönlich bestimmt sein oder für alle Benutzer der Anwendung freigegeben werden. 

## <a name="add-tiles"></a>Hinzufügen von Kacheln

Der folgende Screenshot zeigt das Dashboard in einer Anwendung, die über die Vorlage **Benutzerdefinierte Anwendung** erstellt wurde. Wählen Sie zum Anpassen des Standarddashboards Ihrer Anwendung oben links auf der Seite die Option **Bearbeiten** aus.

> [!div class="mx-imgBorder"]
> ![Dashboard für Anwendungen auf Basis der „Beispiel Contoso“-Vorlage](media/howto-add-tiles-to-your-dashboard/dashboard-sample-contoso.png)

Wenn Sie **Bearbeiten** auswählen, wird das Bibliotheksfenster des Dashboards geöffnet. Die Bibliothek enthält die Kacheln und Dashboardprimitive, mit denen Sie das Dashboard anpassen können.

> [!div class="mx-imgBorder"]
> ![Dashboardbibliothek](media/howto-add-tiles-to-your-dashboard/dashboard-library.png)

Sie können beispielsweise eine Kachel **Telemetrie** für die aktuelle Temperatur des Geräts hinzufügen. Gehen Sie folgendermaßen vor:
1. Wählen Sie eine **Gerätevorlage** aus.
1. Wählen Sie für das Gerät, das auf einer Dashboardkachel angezeigt werden soll, eine **Geräteinstanz** aus. Dann wird eine Liste der Geräteeigenschaften angezeigt, die auf der Kachel verwendet werden können.
1. Wenn Sie die Kachel auf dem Dashboard erstellen möchten, klicken Sie auf **Temperatur**, und ziehen Sie sie auf den Dashboardbereich. Sie können auch auf das Kontrollkästchen neben **Temperatur** und dann auf **Kombinieren** klicken. Der folgende Screenshot zeigt das Auswählen einer Gerätevorlage und Geräteinstanz und dann das Erstellen der Kachel „Temperaturtelemetrie“ auf dem Dashboard.
1. Wählen Sie oben links **Speichern** aus, um die Kachel auf dem Dashboard zu speichern.

> [!div class="mx-imgBorder"]
> ![Formular „Gerätedetails konfigurieren“ mit Details zu Einstellungen und Eigenschaften](media/howto-add-tiles-to-your-dashboard/device-details.png)

Wenn ein Bediener jetzt das Standarddashboard der Anwendung anzeigt, wird die neue Kachel mit der Eigenschaft **Temperatur** für das Gerät angezeigt. Jede Kachel weist einen vorab ausgewählten Graph, ein vorab ausgewähltes Diagramm usw. auf, der beim Erstellen der Kachel angezeigt wird. Benutzer können diese Visualisierung jedoch bearbeiten und ändern. 

> [!div class="mx-imgBorder"]
> ![Registerkarte „Dashboard“ mit Anzeige der Einstellungen und Eigenschaften für die Kachel](media/howto-add-tiles-to-your-dashboard/settings-and-properties.png)


## <a name="edit-tiles"></a>Bearbeiten von Kacheln

Zum Bearbeiten einer Kachel im Dashboard klicken Sie zuerst in der Seite oben links auf **Bearbeiten**. Dadurch wird in den Bearbeitungsmodus für das Dashboard und alle zugehörigen Kacheln gewechselt. 

> [!div class="mx-imgBorder"]
> ![Dashboard-Bildschirm mit aktiviertem Bearbeitungsmodus für eine ausgewählte Kachel](media/howto-add-tiles-to-your-dashboard/edit-mode.png)

Klicken Sie anschließend in der oberen rechten Ecke der zu bearbeitenden Kachel auf das **Zahnradsymbol**. Hier können Sie die Aspekte der Kachel einschließlich ihres Titels, der Visualisierung, der Aggregation usw. bearbeiten.

> [!div class="mx-imgBorder"]
> ![Dropdownliste mit Aggregationseinstellungen der Kachel](media/howto-add-tiles-to-your-dashboard/aggregation-settings.png)

Sie können auch die Visualisierung der Kachel ändern, indem Sie auf das **Linealsymbol** auf der Kachel klicken.

> [!div class="mx-imgBorder"]
> ![Dropdownliste mit Visualisierungseinstellungen der Kachel](media/howto-add-tiles-to-your-dashboard/visualization-settings.png)

## <a name="tile-types"></a>Kacheltypen

In der folgende Tabelle ist die Verwendung von Kacheln in Azure IoT Central zusammengefasst:
 
| Kachel | Dashboard | BESCHREIBUNG
| ----------- | ------- | ------- |
| Inhalt | Dashboards von Anwendungen und Gerätegruppen |Kacheln mit Unterstützung für Markdown sind Kacheln, auf die geklickt werden kann und in denen Überschriften und Beschreibungstext angezeigt werden. Sie können diese Kachel auch als Link-Kachel verwenden, um Benutzern das Navigieren zu einer URL Ihrer Anwendung zu ermöglichen.|
| Image | Dashboards von Anwendungen und Gerätegruppen |Auf Bildkacheln, auf denen ein benutzerdefiniertes Bild angezeigt wird, kann geklickt werden. Verwenden Sie eine Bildkachel, um einem Dashboard Grafiken hinzuzufügen und optional einem Benutzer die Navigation zu einer URL zu ermöglichen, die für Ihre Anwendung relevant ist.|
| Bezeichnung | Anwendungsdashboards |Auf Kacheln mit einer Beschriftung auf einem Dashboard wird benutzerdefinierter Text angezeigt. Sie können die Größe des Texts festlegen. Auf einer Kachel mit Beschriftung können Sie dem Dashboard relevante Informationen wie Beschreibungen, Kontaktinformationen oder Hilfe hinzufügen.|
| Karte | Dashboards von Anwendungen und Gerätegruppen |Auf Kartenkacheln werden der Standort und Status eines Geräts auf einer Karte angezeigt. So können Sie beispielsweise anzeigen, wo sich ein Gerät befindet und ob sein Lüfter eingeschaltet ist.|
| Liniendiagramm | Dashboards von Anwendungen und Geräten |Liniendiagrammkacheln zeigen eine Grafik der Aggregatmessung für ein Gerät über einen Zeitraum. Sie können beispielsweise ein Liniendiagramm anzeigen, das die durchschnittliche Temperatur und den durchschnittlichen Druck eines Geräts in der letzten Stunde anzeigt.|
| Balkendiagramm | Dashboards von Anwendungen und Geräten |Balkendiagrammkacheln zeigen eine Grafik der Aggregatmessung für ein Gerät über einen Zeitraum. Sie können beispielsweise ein Balkendiagramm anzeigen, das die durchschnittliche Temperatur und den durchschnittlichen Druck eines Geräts in der letzten Stunde anzeigt.|
| Kreisdiagramm | Dashboards von Anwendungen und Gerätegruppen |Kreisdiagrammkacheln zeigen eine Grafik der aggregierten Messungen für ein Gerät über einen Zeitraum.|
| Wärmebild | Dashboards von Anwendungen und Gerätegruppen |Auf Wärmebildkacheln werden Informationen zur Gerätegruppen angezeigt, die mit Farben dargestellt sind.|
| Ereignisverlauf | Dashboards von Anwendungen und Geräten |Auf einer Kachel mit dem Ereignisverlauf werden die Ereignisse für ein Gerät in einem Zeitraum gezeigt. Damit können Sie beispielsweise alle Temperaturänderungen anzeigen, die in der letzten Stunde auf einem Gerät aufgetreten sind.|
| Der Statusverlauf | Dashboards von Anwendungen und Geräten |Auf einer Statusverlaufskachel werden die Messwerte für einen bestimmten Zeitraum gezeigt. Damit können Sie beispielsweise die Temperaturwerte eines Geräts in der letzten Stunde anzeigen.|
| KPI | Dashboards von Anwendungen und Geräten | Auf KPI-Kacheln wird eine aggregierte Telemetrie- oder Ereignismessung für einen Zeitraum gezeigt. Damit können Sie beispielsweise die Maximaltemperatur eines Geräts in der letzten Stunde anzeigen.|
| Letzter bekannter Wert | Dashboards von Anwendungen und Geräten |Auf Kacheln mit dem letzten bekannten Wert wird der neueste Wert für eine Telemetrie- oder Statusmessung ausgegeben. Auf dieser Kachel können Sie beispielsweise die neuesten Messungen von Temperatur, Druck und Feuchtigkeit für ein Gerät anzeigen.|

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun gelernt haben, wie Sie das Standarddashboard der Azure IoT Central-Anwendung konfigurieren, [erfahren Sie nun, wie Sie Bilder vorbereiten und hochladen](howto-prepare-images.md).