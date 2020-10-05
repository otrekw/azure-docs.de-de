---
title: Hinzufügen von Kacheln zu Ihrem Dashboard in Azure IoT Central | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie als Ersteller das Standarddashboard Ihrer Azure IoT Central-Anwendung mit Kacheln konfigurieren.
author: Haley-Rowland
ms.author: harowl
ms.date: 05/27/2020
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.openlocfilehash: beeb771ea5053dd0ad867a7568aa64bbb2d0b4ed
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90985262"
---
# <a name="configure-the-application-dashboard"></a>Konfigurieren des Anwendungsdashboards

Nach dem Herstellen einer Verbindung zu einer Azure IoT Central-Anwendung wird als Erstes das **Dashboard** angezeigt. Wenn Sie die Anwendung auf Grundlage einer der branchenbezogenen [Anwendungsvorlagen](./concepts-app-templates.md) erstellt haben, startet Ihre Anwendung mit einem vordefinierten Dashboard. Wenn Sie die Anwendung über die Anwendungsvorlage [Benutzerdefinierte Anwendung](./concepts-app-templates.md) erstellt haben, werden auf dem Dashboard Tipps für den Einstieg angezeigt.

> [!TIP]
> Zusätzlich zum Standarddashboard der Anwendung können Benutzer [mehrere Dashboards erstellen](howto-create-personal-dashboards.md). Diese Dashboards können nur für den Benutzer persönlich bestimmt sein oder für alle Benutzer der Anwendung freigegeben werden.  

## <a name="add-tiles"></a>Hinzufügen von Kacheln

Der folgende Screenshot zeigt das Dashboard in einer Anwendung, die über die Vorlage **Benutzerdefinierte Anwendung** erstellt wurde. Wenn Sie das aktuelle Dashboard anpassen möchten, klicken Sie auf **Bearbeiten**. Wenn Sie ein benutzerdefiniertes privates oder freigegebenes Dashboard hinzufügen möchten, klicken Sie auf **Neu**:

:::image type="content" source="media/howto-add-tiles-to-your-dashboard/dashboard-sample-contoso.png" alt-text="Dashboard für Anwendungen, die auf der Vorlage „Benutzerdefinierte Anwendung“ basieren":::

Nachdem Sie auf **Bearbeiten** oder **Neu** geklickt haben, befindet sich das Dashboard im *Bearbeitungsmodus*. Sie können die Tools im Bereich **Dashboard bearbeiten** verwenden, um dem Dashboard Kacheln hinzuzufügen und Kacheln auf dem Dashboard selbst anzupassen und von dort zu entfernen. Wenn Sie beispielsweise eine Kachel mit **Telemetriedaten** hinzufügen möchten, auf der die von einem oder mehreren Geräten gemeldete aktuelle Temperatur angezeigt wird, gehen Sie folgendermaßen vor:

1. Klicken Sie im Bereich **Dashboard bearbeiten** auf die Option **Gerätegruppe**.
1. Wählen Sie im Dropdown **Geräte** eines oder mehrere Geräte aus, die auf der Kachel angezeigt werden sollen. Nun werden die verfügbare Telemetrie, die Eigenschaften und die Befehle dieser Geräte angezeigt.
1. Klicken Sie im Telemetriebereich auf **Temperatur** und dann auf **Kachel hinzufügen**. Die Kachel wird nun auf dem Dashboard angezeigt. Dort können Sie die Visualisierung ändern, die Größe der Kachel ändern und sie konfigurieren:

:::image type="content" source="media/howto-add-tiles-to-your-dashboard/device-details.png" alt-text="Dashboard für Anwendungen, die auf der Vorlage „Benutzerdefinierte Anwendung“ basieren":::

Wenn Sie das Hinzufügen und Anpassen der Kacheln auf dem Dashboard abgeschlossen haben, klicken Sie auf **Speichern**.

## <a name="customize-tiles"></a>Anpassen von Kacheln

Wenn Sie eine Kachel auf dem Dashboard anpassen möchten, muss sich das Dashboard im Bearbeitungsmodus befinden. Die verfügbaren Anpassungsoptionen hängen vom [Kacheltyp](#tile-types) ab:

* Über das Linealsymbol auf einer Kachel können Sie die Visualisierung ändern. Zu den Visualisierungen gehören Liniendiagramme, letzte bekannte Werte und Wärmebilder.

* Über das quadratische Symbol können Sie die Größe der Kachel ändern.

* Das Zahnradsymbol ermöglicht die Konfiguration der Visualisierung. Bei einer Liniendiagrammvisualisierung können Sie beispielsweise die Legende und Achsen anzeigen und einen Zeitbereich auswählen, der dargestellt werden soll.

## <a name="tile-types"></a>Kacheltypen

Die folgende Tabelle beschreibt die verschiedenen Typen von Kacheln, die Sie einem Dashboard hinzufügen können:

| Kachel             | BESCHREIBUNG |
| ---------------- | ----------- |
| Markdown         | Kacheln mit Unterstützung für Markdown sind Kacheln, auf die geklickt werden kann und in denen Überschriften und Beschreibungstext mithilfe von Markdown angezeigt werden. Die URL kann ein relativer Link zu einer anderen Seite der Anwendung sein, oder ein absoluter Link zu einer externen Website.|
| Image            | Auf Bildkacheln, auf denen ein benutzerdefiniertes Bild angezeigt wird, kann geklickt werden. Die URL kann ein relativer Link zu einer anderen Seite der Anwendung sein, oder ein absoluter Link zu einer externen Website.|
| Bezeichnung            | Auf Kacheln mit einer Beschriftung auf einem Dashboard wird benutzerdefinierter Text angezeigt. Sie können die Größe des Texts festlegen. Auf einer Kachel mit Beschriftung können Sie dem Dashboard relevante Informationen wie Beschreibungen, Kontaktinformationen oder Hilfe hinzufügen.|
| Anzahl            | Diese Kachel zeigt die Anzahl der Geräte in einer Gerätegruppe an.|
| Karte              | Kartenkacheln zeigen den Standort eines oder mehrerer Geräte auf einer Karte an. Sie können bis zu 100 Punkte im Standortverlauf eines Geräts anzeigen. Sie können beispielsweise ein Routenmuster der Orte anzeigen, an denen sich ein Gerät in der vergangenen Woche befand.|
| KPI              |  KPI-Kacheln zeigen aggregierte Telemetriewerte für ein oder mehrere Geräte über einen bestimmten Zeitraum hinweg an. Damit können Sie beispielsweise die Maximaltemperatur und den Druck eines oder mehrerer Geräte in der letzten Stunde anzeigen.|
| Liniendiagramm       | Liniendiagrammkacheln zeigen einen oder mehrere aggregierte Telemetriewerte für ein oder mehrere Geräte über einen bestimmten Zeitraum hinweg an. Sie können beispielsweise ein Liniendiagramm anzeigen, das die durchschnittliche Temperatur und den durchschnittlichen Druck eines oder mehrere Geräte in der letzten Stunde anzeigt.|
| Balkendiagramm        | Balkendiagrammkacheln zeigen einen oder mehrere aggregierte Telemetriewerte für ein oder mehrere Geräte über einen bestimmten Zeitraum hinweg an. Sie können beispielsweise ein Balkendiagramm anzeigen, das die durchschnittliche Temperatur und den durchschnittlichen Druck eines oder mehrerer Geräte in der letzten Stunde anzeigt.|
| Kreisdiagramm        | Kreisdiagrammkacheln zeigen einen oder mehrere aggregierte Telemetriewerte für ein oder mehrere Geräte über einen bestimmten Zeitraum hinweg an.|
| Heat Map         | Auf Wärmebildkacheln werden Informationen zu einem oder mehreren Geräten farblich dargestellt.|
| Letzter bekannter Wert | Kacheln mit letzten bekannten Werten zeigen die aktuellen Telemetriewerte für ein oder mehrere Geräte an. Auf dieser Kachel können Sie beispielsweise die aktuellen Messungen von Temperatur, Druck und Feuchtigkeit für ein oder mehrere Gerät anzeigen. |
| Ereignisverlauf    | Auf einer Kachel mit dem Ereignisverlauf werden die Ereignisse für ein Gerät in einem Zeitraum gezeigt. Auf dieser Kachel können Sie beispielsweise alle Öffnungs- und Schließereignisse der Ventile für ein oder mehrere Geräte während der letzten Stunde anzeigen.|
| Eigenschaft         |  Auf Eigenschaftenkacheln wird der aktuelle Wert für Eigenschaften und Cloudeigenschaften eines oder mehrerer Geräts angezeigt. Sie können diese Kachel z. B. verwenden, um Geräteeigenschaften wie den Hersteller oder die Firmwareversion eines Geräts anzuzeigen. |

Aktuell können Sie Kacheln, die mehrere Geräte unterstützen, bis zu zehn Geräte hinzufügen.

### <a name="customizing-visualizations"></a>Anpassen von Visualisierungen

Klicken Sie bei Kacheln, die aggregierte Werte anzeigen, auf das Zahnradsymbol neben dem Telemetrietyp im Bereich **Diagramm konfigurieren**, um die Aggregation auszuwählen. Zur Verfügung stehen die Aggregationen für den Durchschnitt, die Summe, das Maximum, das Minimum und die Anzahl.

Bei Liniendiagrammen, Balkendiagrammen und Kreisdiagrammen können Sie die Farbe der verschiedenen Telemetriewerte anpassen. Klicken Sie hierzu auf das Farbpalettensymbol neben den Telemetriedaten, die Sie anpassen möchten:

:::image type="content" source="media/howto-add-tiles-to-your-dashboard/color-customization.png" alt-text="Dashboard für Anwendungen, die auf der Vorlage „Benutzerdefinierte Anwendung“ basieren":::

Bei Kacheln, die Zeichenfolgeneigenschaften oder Telemetriewerte zeigen, können Sie auswählen, wie der Text angezeigt werden soll. Wenn das Gerät beispielsweise eine URL in einer Zeichenfolgeneigenschaft speichert, können Sie sie als Link anzeigen, auf den geklickt werden kann. Wenn die URL auf ein Bild verweist, können Sie das Bild in einer Kachel mit dem letzten bekannten Wert oder einer Eigenschaftenkachel rendern. Wenn Sie ändern möchten, wie eine Zeichenfolge angezeigt wird, klicken Sie in der Kachelkonfiguration auf das Zahnradsymbol neben dem Telemetrietyp oder der Eigenschaft:

:::image type="content" source="media/howto-add-tiles-to-your-dashboard/string-customization.png" alt-text="Dashboard für Anwendungen, die auf der Vorlage „Benutzerdefinierte Anwendung“ basieren":::

Für die numerischen Kacheln **KPI**, **Letzter bekannter Wert** und **Eigenschaften** können Sie mithilfe von bedingter Formatierung die Farbe der jeweiligen Kachel basierend auf dem aktuellen Wert anpassen. Um eine bedingte Formatierung anzuwenden, wählen Sie die Kachel **Konfigurieren** und dann neben dem anzupassenden Wert das Symbol **Bedingte Formatierung** aus:

:::image type="content" source="media/howto-add-tiles-to-your-dashboard/conditional-formatting-1.png" alt-text="Dashboard für Anwendungen, die auf der Vorlage „Benutzerdefinierte Anwendung“ basieren":::

Fügen Sie Ihre Regeln für bedingte Formatierung hinzu:

:::image type="content" source="media/howto-add-tiles-to-your-dashboard/conditional-formatting-2.png" alt-text="Dashboard für Anwendungen, die auf der Vorlage „Benutzerdefinierte Anwendung“ basieren":::

Der folgende Screenshot zeigt die Auswirkung der Regel für bedingte Formatierung:

:::image type="content" source="media/howto-add-tiles-to-your-dashboard/conditional-formatting-3.png" alt-text="Dashboard für Anwendungen, die auf der Vorlage „Benutzerdefinierte Anwendung“ basieren":::

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie gelernt haben, wie Sie das Standarddashboard der Azure IoT Central-Anwendung konfigurieren, [erfahren Sie nun, wie Sie ein persönliches Dashboard erstellen](howto-create-personal-dashboards.md).
