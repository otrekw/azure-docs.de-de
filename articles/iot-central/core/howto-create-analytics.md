---
title: Analysieren von Gerätedaten in Ihrer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Analysieren Sie Gerätedaten in Ihrer Azure IoT Central-Anwendung.
author: ankitscribbles
ms.author: ankitgup
ms.date: 11/27/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: abjork
ms.openlocfilehash: 8f329b6377915a3df6e3f3e212cbd8d41f9ed80d
ms.sourcegitcommit: 43558caf1f3917f0c535ae0bf7ce7fe4723391f9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2020
ms.locfileid: "90017659"
---
# <a name="how-to-use-analytics-to-analyze-device-data"></a>Analysieren von Gerätedaten mithilfe von Analytics

*Dieser Artikel gilt für Betreiber, Ersteller und Administratoren.*

Azure IoT Central bietet umfassende Analysefunktionen, mit denen Sie historische Trends analysieren und verschiedene Telemetriedaten von ihren Geräten korrelieren können. Um mit diesem Vorgang zu beginnen, wechseln Sie im linken Bereich zu **Analyse**.

## <a name="understanding-the-analytics-ui"></a>Grundlegendes zur Analytics-Benutzeroberfläche
Die Analytics-Benutzeroberfläche besteht aus drei Hauptkomponenten:
- **Bereich für Datenkonfiguration:** Wählen Sie im Konfigurationsbereich zunächst die Gerätegruppe aus, für die Sie die Daten analysieren möchten. Wählen Sie als Nächstes die Telemetriedaten, die Sie analysieren möchten, und dann die Aggregationsmethode für die einzelnen Telemetriedaten aus. Mithilfe des Steuerelements **Aufteilen nach** können Sie die Daten gruppieren und dazu die Geräteeigenschaften als Dimensionen verwenden.

- **Zeitsteuerelement:** Mithilfe des Zeitsteuerelements können Sie die Dauer auswählen, für die Sie die Daten analysieren möchten. Sie können eines der Enden des Schiebereglers ziehen, um den gewünschten Zeitraum auszuwählen. Das Zeitsteuerelement enthält auch den Schieberegler **Intervallgröße**, über den der Bucket oder die Intervallgröße zum Aggregieren der Daten gesteuert wird. 

- **Diagrammsteuerelement:** Das Diagrammsteuerelement visualisiert die Daten als Liniendiagramm. Durch Interaktion mit der Diagrammlegende können Sie die Sichtbarkeit bestimmter Zeilen umschalten. 


  ![Übersicht über die Analytics-Benutzeroberfläche](media/howto-create-analytics/analyticsui.png)


## <a name="querying-your-data"></a>Abfragen Ihrer Daten

Sie müssen zunächst eine **Gerätegruppe** und die Telemetriedaten auswählen, die Sie analysieren möchten. Wählen Sie anschließend **Analysieren** aus, um mit der Visualisierung Ihrer Daten zu beginnen.

- **Gerätegruppe:** Eine [Gerätegruppe](tutorial-use-device-groups.md) ist eine benutzerdefinierte Gruppe Ihrer Geräte. Beispiele: „Alle Kühlschränke in Oakland“ oder „Alle Windturbinen der Version 2.0“.

- **Telemetrie:** Wählen Sie die Telemetriedaten aus, die Sie analysieren und untersuchen möchten. Sie können mehrere Telemetrien zum gemeinsamen Analysieren auswählen. Die Standardaggregationsmethode für den Datentyp „Numeric“ (Numerisch) ist auf „Average“ (Durchschnitt)und für den Datentyp „String“ (Zeichenfolge) auf „Count“ (Anzahl) festgelegt. Die unterstützten Aggregationsmethoden für den Datentyp „Numeric“ sind „Average“, „Maximum“, „Minimum“, „Count“ und „Sum“ (Summe).  Die unterstützte Aggregationsmethode für den Datentyp „String“ ist „Count“.

- **Aufteilen nach:** Mithilfe des Steuerelements „Aufteilen nach“ können Sie die Daten gruppieren und dazu die Geräteeigenschaften als Dimensionen verwenden. Die Werte der Geräte- und Cloudeigenschaften werden mit den vom Gerät gesendeten Telemetriedaten verknüpft. Wenn die Cloud- oder Geräteeigenschaft aktualisiert wurde, werden die Telemetriedaten – nach verschiedenen Werten gruppiert – im Diagramm angezeigt.

    > [!TIP]
    > Wenn Sie Daten für jedes Gerät getrennt anzeigen möchten, wählen Sie im Steuerelement „Aufteilen nach" die Option „Geräte-ID“ aus.

## <a name="interacting-with-your-data"></a>Interaktion mit Ihren Daten

Nachdem Sie Ihre Daten abgefragt haben, können Sie mit deren Visualisierung im Liniendiagramm beginnen. Sie können Telemetriedaten anzeigen/ausblenden, die Zeitdauer ändern und diese Daten in einem Datenraster anzeigen.

- **Editor-Bereich für Zeiten:** Standardmäßig werden die Daten vom letzten Tag abgerufen. Sie können eines der Enden des Schiebereglers ziehen, um die Zeitdauer zu ändern. Außerdem können Sie mithilfe des Kalendersteuerelements einen der vordefinierten Zeitrahmen oder einen benutzerdefinierten Zeitbereich auswählen. Das Zeitsteuerelement enthält auch den Schieberegler **Intervallgröße**, über den der Bucket oder die Intervallgröße zum Aggregieren der Daten gesteuert wird.

    ![Zeit-Editor](media/howto-create-analytics/timeeditorpanel.png)

    - **Schieberegler für den inneren Datumsbereich**: Ziehen Sie die beiden Endpunktsteuerelemente über den gewünschten Zeitraum. Dieser innere Datumsbereich wird durch den Schieberegler für den äußeren Datumsbereich begrenzt.
    
   
    - **Schieberegler für den äußeren Datumsbereich**: Verwenden Sie die Endpunktsteuerelemente, um den äußeren Datumsbereich auszuwählen. Dieser steht dann für das Steuerelement für den inneren Datumsbereich zur Verfügung.

    - **Schaltflächen zum Erhöhen/Verringern des Datumsbereichs**: Sie können Ihren Zeitraum erhöhen oder verringern, indem Sie eine der Schaltflächen für das gewünschte Intervall auswählen.

    - **Schieberegler „Intervallgröße“** : Hiermit können Sie Intervalle innerhalb eines Zeitraums vergrößern und verkleinern. Diese Aktion bietet eine genauere Steuerung der Bewegung zwischen größeren Zeitabschnitten. Sie können damit präzise, hochauflösende Ansichten Ihrer Daten anzeigen – sogar im Bereich von Millisekunden. Der Standardstartpunkt des Schiebereglers ist auf die optimale Ansicht der Daten aus Ihrer Auswahl festgelegt und bietet ein ausgewogenes Verhältnis zwischen Auflösung, Abfragegeschwindigkeit und Granularität.
    
    - **Datumsbereichsauswahl**: Mit diesem Websteuerelement können Sie komfortabel die gewünschten Datumsbereiche und Zeiträume auswählen. Sie können das Steuerelement auch verwenden, um zwischen verschiedenen Zeitzonen zu wechseln. Nachdem Sie die gewünschten Änderungen für Ihren aktuellen Arbeitsbereich vorgenommen haben, wählen Sie „Speichern“ aus.

    > [!TIP]
    > Die Intervallgröße wird – basierend auf dem ausgewählten Zeitraum – dynamisch bestimmt. Kleinere Zeiträume ermöglichen die Aggregation der Daten in sehr präzise Intervalle von bis zu wenigen Sekunden.


- **Diagrammlegende:** Die Diagrammlegende zeigt die ausgewählten Telemetriedaten im Diagramm. Sie können mit dem Mauszeiger auf jedes Element in der Legende zeigen, um es im Diagramm zu fokussieren. Bei Verwendung von „Aufteilen nach“ werden die Telemetriedaten nach den jeweiligen Werten der ausgewählten Dimension gruppiert. Durch Klicken auf den Gruppennamen können Sie die Sichtbarkeit der einzelnen Telemetriedaten oder der ganzen Gruppe umschalten.  


- **Steuerelement „y-Achsenformat“:** Der Modus „y-Achse“ durchläuft die verfügbaren Ansichtsoptionen für die y-Achse. Dieses Steuerelement ist nur verfügbar, wenn verschiedene Telemetriedaten visualisiert werden. Sie können die y-Achse festlegen, indem Sie einen von drei Modi auswählen:

    - **Gestapelt:** Für jede Telemetrie werden Diagramme als Stapel dargestellt, wobei jedes Diagramm eine eigene y-Achse hat. Dieser Modus wird als Standard festgelegt.
    - **Freigegeben**: Für jede Telemetrie wird ein Diagramm auf derselben y-Achse gezeichnet.
    - **Überlappung:** Ermöglicht das Stapeln mehrerer Zeilen auf derselben y-Achse, wobei sich die y-Achsendaten je nach ausgewählter Zeile ändern.

  ![Anordnung der Daten auf der Y-Achse mit unterschiedlichen Visualisierungsmodi](media/howto-create-analytics/yaxiscontrol.png)

- **Zoomsteuerelement:** Mit Zoom können Sie einen Drillvorgang in Ihre Daten durchführen. Wenn Sie in Ihren Ergebnissen einen Zeitraum finden, auf den Sie sich konzentrieren möchten, erfassen Sie diesen Bereich mit dem Mauszeiger, und ziehen Sie ihn auf den Endpunkt Ihrer Wahl. Klicken Sie anschließend mit der rechten Maustaste auf den ausgewählten Bereich und dann auf „Zoom“.

  ![Vergrößern der Daten](media/howto-create-analytics/zoom.png)

Unter den Auslassungspunkten finden Sie weitere Diagrammsteuerelemente für die Interaktion mit den Daten:

- **Raster anzeigen:** Ihre Ergebnisse stehen in einem Tabellenformat zur Verfügung, damit Sie den genauen Wert für jeden Datenpunkt anzeigen können.

- **Als CSV herunterladen:** Ihre Ergebnisse können als CSV-Datei (Comma-Separated Values, durch Trennzeichen getrennte Werte) exportiert werden. Die CSV-Datei enthält Daten für jedes Gerät. Die Ergebnisse werden auf der Grundlage des angegebenen Intervalls und Zeitrahmens exportiert. 

- **Marker setzen:** Das Steuerelement „Marker setzen“ ermöglicht das Verankern bestimmter Datenpunkte im Diagramm. Das ist hilfreich, wenn Sie versuchen, Daten für mehrere Zeilen über verschiedene Zeiträume hinweg zu vergleichen.

  ![Rasteransicht für Ihre Analyse](media/howto-create-analytics/additionalchartcontrols.png)
