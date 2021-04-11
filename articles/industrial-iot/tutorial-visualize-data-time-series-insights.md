---
title: Visualisieren von OPC UA-Daten in Azure Time Series Insights
description: In diesem Tutorial erfahren Sie, wie Sie Daten mit Time Series Insights visualisieren.
author: jehona-m
ms.author: jemorina
ms.service: industrial-iot
ms.topic: tutorial
ms.date: 3/22/2021
ms.openlocfilehash: 5bd218c0d94922b6137a964e3993f516216ca4b7
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104787260"
---
# <a name="tutorial-visualize-data-with-time-series-insights-tsi"></a>Tutorial: Visualisieren von Daten mit Time Series Insights (TSI)

Das OPC Publisher-Modul stellt eine Verbindung mit OPC UA-Servern her und veröffentlicht Daten von diesen Servern in IoT Hub. Der Telemetrieprozessor der Industrial IoT-Plattform verarbeitet diese Ereignisse und leitet kontextualisierte Beispiele an TSI und andere Consumer weiter.  

In dieser Anleitung erfahren Sie, wie Sie die OPC UA-Telemetriedaten mit dieser Time Series Insights-Umgebung visualisieren und analysieren.

In diesem Tutorial lernen Sie, wie die folgenden Aufgaben ausgeführt werden:

> [!div class="checklist"]
> * Alle Tutorials enthalten eine Liste mit einer Zusammenfassung der auszuführenden Schritte.
> * Jeder dieser Aufzählungspunkte wird an einem Schlüssel (H2) ausgerichtet.
> * Verwenden Sie diese grünen Kontrollkästchen in einem Tutorial.

## <a name="prerequisite"></a>Voraussetzungen

* Stellen Sie die IIoT-Plattform bereit. Eine Time Series Insights-Umgebung wird dann automatisch erstellt.
* Daten werden in IoT Hub veröffentlicht.

## <a name="time-series-insights-explorer"></a>Time Series Insights-Explorer

Der Time Series Insights-Explorer ist eine Web-App, mit der Sie Ihre Telemetriedaten visualisieren können. Öffnen Sie zum Abrufen der URL der Anwendung die Datei `.env`, die im Rahmen der Bereitstellung gespeichert wurde.  Öffnen Sie die URL aus der Variablen `PCS_TSI_URL` in einem Browser.  

Vor der Verwendung des Time Series Insights-Explorers muss den Benutzern, die zum Visualisieren der Daten berechtigt sind, Zugriff auf die TSI-Daten gewährt werden. Da bei einer neuen Bereitstellung standardmäßig keine Datenzugriffsrichtlinien festgelegt werden, können die Daten von niemandem angezeigt werden. Die Datenzugriffsrichtlinien müssen über das Azure-Portal in der Time Series Insights-Umgebung festgelegt werden, die in der von der IIoT-Plattform bereitgestellten Ressourcengruppe bereitgestellt wird. Gehen Sie dazu wie folgt vor:

   ![Time Series Insights-Explorer 1](media/tutorial-iiot-visualize-data-tsi/tutorial-time-series-insights-data-access-1.png)

Wählen Sie die Datenzugriffsrichtlinien aus:

   ![Time Series Insights-Explorer 2](media/tutorial-iiot-visualize-data-tsi/tutorial-time-series-insights-data-access-2.png)

Weisen Sie die erforderlichen Benutzer zu:

   ![Time Series Insights-Explorer 3](media/tutorial-iiot-visualize-data-tsi/tutorial-time-series-insights-data-access-3.png)


Beachten Sie im TSI-Explorer die nicht zugewiesenen Time Series-Instanzen. Eine TSI-Instanz entspricht der Zeit-Wert-Reihe für einen bestimmten Datenpunkt, der von einem veröffentlichten Knoten eines OPC-Servers stammt. Die TSI-Instanz bzw. der OPC UA-Datenpunkt wird anhand von „EndpointId“, „SubscriptionId“ und „NodeId“ eindeutig identifiziert. Die TSI-Instanzenmodelle werden basierend auf den Telemetriedaten, die vom Event Hub des Telemetrieprozessors der IIoT-Plattform erfasst werden, automatisch erkannt und im Explorer angezeigt.

   ![Time Series Insights-Explorer 4](media/tutorial-iiot-visualize-data-tsi/tutorial-time-series-insights-step-0.png)

Die Telemetriedaten können im Diagramm visualisiert werden, indem Sie mit der rechten Maustaste auf die TSI-Instanz klicken und den Wert auswählen. Der im Diagramm zu verwendende Zeitrahmen kann in der rechten oberen Ecke angepasst werden. Der Wert mehrerer Instanzen kann auf der Grundlage der gleichen Zeitbasisauswahl visualisiert werden.

Weitere Informationen finden Sie unter [Schnellstart: Erkunden der Azure Time Series Insights Gen2-Demoumgebung](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-quickstart).

## <a name="define-and-apply-a-new-model"></a>Definieren und Anwenden eines neuen Modells

Da sich die Telemetrieinstanzen im Rohformat befinden, müssen sie mit dem passenden Modell kontextualisiert werden. 

Ausführliche Informationen zu TSI-Modellen finden Sie unter [Zeitreihenmodell in Azure Time Series Insights Gen2](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-tsm).

1. Schritt 1: Definieren Sie im Explorer auf der Registerkarte „Modell“ eine neue Hierarchie für die erfassten Telemetriedaten. Bei einer Hierarchie handelt es sich um die logische Struktur, die es dem Benutzer ermöglichen soll, die Metainformationen einzufügen, die für eine intuitivere Navigation durch die TSI-Instanzen erforderlich sind. Ein Benutzer kann Hierarchievorlagen erstellen, löschen und ändern, die später für die verschiedenen TSI-Instanzen instanziiert werden können.

   ![Schritt 1](media/tutorial-iiot-visualize-data-tsi/tutorial-time-series-insights-step-1.png)

2. Schritt 2: Definieren Sie einen neuen Typ für die Werte. In unserem Beispiel werden nur numerische Datentypen behandelt.

   ![Schritt 2](media/tutorial-iiot-visualize-data-tsi/tutorial-time-series-insights-step-2.png)

3. Schritt 3: Wählen Sie die neue TSI-Instanz aus, die in der zuvor definierten Hierarchie kategorisiert werden muss.

   ![Schritt 3](media/tutorial-iiot-visualize-data-tsi/tutorial-time-series-insights-step-3.png)

4. Schritt 4: Füllen Sie die Instanzeigenschaften (Name, Beschreibung, Datenwert und die Hierarchiefelder) aus, um die logische Struktur abzugleichen. 

   ![Schritt 4](media/tutorial-iiot-visualize-data-tsi/tutorial-time-series-insights-step-4.png)

5. Schritt 5: Wiederholen Sie Schritt 4 für alle nicht kategorisierten TSI-Instanzen.

   ![Schritt 5](media/tutorial-iiot-visualize-data-tsi/tutorial-time-series-insights-step-5.png)

6. Schritt 6: Gehen Sie auf der Hauptseite des TSI-Explorers die Hierarchie der kategorisierten Instanzen durch, und wählen Sie die Werte für die zu analysierenden Datenpunkte aus.

   ![Schritt 6](media/tutorial-iiot-visualize-data-tsi/tutorial-time-series-insights-step-6.png)

## <a name="connect-time-series-insights-to-power-bi"></a>Verbinden von Time Series Insights mit Power BI

Die Time Series Insights-Umgebung kann auch mit Power BI verknüpft werden.  Weitere Informationen finden Sie unter [Visualisieren der Daten von Azure Time Series Insights in Power BI](https://docs.microsoft.com/azure/time-series-insights/how-to-connect-power-bi) sowie unter [Verbinden von Azure Time Series Insights Gen2 mit Power BI](https://docs.microsoft.com/azure/time-series-insights/concepts-power-bi).


## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun wissen, wie Sie Daten in TSI visualisieren, können Sie sich als Nächstes mit dem GitHub-Repository für Industrial IoT vertraut machen:

> [!div class="nextstepaction"]
> [GitHub-Repository der IIoT-Plattform](https://github.com/Azure/iot-edge-opc-publisher)