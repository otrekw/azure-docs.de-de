---
title: Überwachen von Apache Spark-Anwendungen mithilfe von Synapse Studio
description: Verwenden Sie Synapse Studio, um Ihre Apache Spark-Anwendungen zu überwachen.
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: monitoring
ms.date: 04/15/2020
ms.author: mahi
ms.reviewer: mahi
ms.openlocfilehash: 4d6c50436ddf68e2610aeb10ddfaaab0a5d060f3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87387352"
---
# <a name="use-synapse-studio-to-monitor-your-apache-spark-applications"></a>Verwenden von Synapse Studio zum Überwachen Ihrer Apache Spark-Anwendungen

Mit Azure Synapse Analytics können Sie mithilfe von Apache Spark Notebooks, Aufträge und andere Arten von Anwendungen in ihren Apache Spark-Pools in Ihrem Arbeitsbereich ausführen.

In diesem Artikel wird erläutert, wie Sie Ihre Apache Spark-Anwendungen überwachen, was es Ihnen gestattet, den aktuellen Status, die aktuellen Probleme und den Fortschritt im Auge zu behalten.

Dieses Tutorial enthält die folgenden Aufgaben:

* Überwachen aktiver Apache Spark-Anwendungen
* Anzeigen einer abgeschlossenen Apache Spark-Anwendung
* Anzeigen einer abgebrochenen Apache Spark-Anwendung
* Debuggen von Fehlern bei Apache Spark-Anwendungen

## <a name="prerequisites"></a>Voraussetzungen

Vergewissern Sie sich zunächst, dass die folgenden Anforderungen erfüllt sind bzw. dass Folgendes vorhanden ist:

- Ein Synapse Studio-Arbeitsbereich. Entsprechende Anweisungen finden Sie unter [Erstellen eines Synapse Studio-Arbeitsbereichs](https://docs.microsoft.com/azure/machine-learning/how-to-manage-workspace#create-a-workspace).

- Ein Apache Spark-Pool.

## <a name="monitor-running-apache-spark-application"></a>Überwachen aktiver Apache Spark-Anwendungen

Öffnen Sie **Überwachen**, und wählen Sie dann **Apache Spark-Anwendungen** aus. Um die Details zu den ausgeführten Apache Spark-Anwendungen anzuzeigen, wählen Sie die sendende Apache Spark-Anwendung aus, und zeigen Sie die Details an. Wenn die Apache Spark-Anwendung noch ausgeführt wird, können Sie den Fortschritt überwachen.

  ![Ausgeführten Auftrag auswählen](./media/how-to-monitor-spark-applications/select-running-job.png)

1. Überprüfen Sie **Abgeschlossene Aufgaben**, **Status** und **Gesamtdauer**.

2. Brechen Sie die Apache Spark-Anwendung ab.

3. Aktualisieren Sie die Protokollabfrage.

4. Sehen Sie sich das Diagramm an.

5. Überprüfen Sie die Informationen unter **Zusammenfassung**.

6. Überprüfen Sie die **Protokolle**. Die Protokollinformationen sind während der Ausführung leer.

    ![Ausgeführten Auftrag anzeigen](./media/how-to-monitor-spark-applications/view-running-job.png)

## <a name="view-completed-apache-spark-application"></a>Anzeigen abgeschlossener Apache Spark-Anwendungen

Öffnen Sie **Überwachen**, und wählen Sie dann **Apache Spark-Anwendungen** aus. Um die Details zu abgeschlossenen Apache Spark-Anwendungen anzuzeigen, wählen Sie die Apache Spark-Anwendung aus, und zeigen Sie die Details an.

  ![Abgeschlossenen Auftrag auswählen](./media/how-to-monitor-spark-applications/select-completed-job.png)

1. Überprüfen Sie **Abgeschlossene Aufgaben**, **Status** und **Gesamtdauer**.

2. Aktualisieren Sie die Protokollabfrage.

3. Öffnen Sie den Link zum Apache Spark-Verlaufsserver, indem Sie auf **Spark-Verlaufsserver** klicken.

4. Überprüfen Sie die Informationen unter **Zusammenfassung**, indem Sie auf das Symbol im Diagramm klicken.

5. Überprüfen Sie die **Protokolle**. Sie können in der Dropdownliste verschiedene Protokolltypen auswählen, und Sie können die Protokollinformationen herunterladen, indem Sie auf **Protokolle herunterladen** klicken.

6. Sie sehen eine Übersicht Ihres Auftrags in dem generierten Auftragsdiagramm. Standardmäßig werden in dem Diagramm alle Aufträge angezeigt. Sie können diese Ansicht anhand der **Auftrags-ID** filtern.

7. Standardmäßig ist die Anzeige **Status** ausgewählt. Sie können den Datenfluss überprüfen, indem Sie **Read** (Gelesen) oder **Written** (Geschrieben) in der Dropdownliste **Anzeige** auswählen.

8. Wählen Sie **Wiedergabe** aus, um den Auftrag wiederzugeben. Sie können jederzeit **Stopp** auswählen, um die Wiedergabe zu beenden.

9. Scrollen Sie mit Ihrem Mausrad, um in das Auftragsdiagramm hinein- oder herauszuzoomen, oder wählen Sie **Mit Zoom anpassen** aus, um das Diagramm an die Anzeige anzupassen.

10. Der Auftragsdiagrammknoten zeigt die folgenden Informationen zu den einzelnen Phasen an:

    * ID

    * Name oder Beschreibung

    * Anzahl der Aufträge insgesamt

    * Gelesene Daten: die Summe der Eingabegröße und der Shuffle-Lesegröße

    * Geschriebene Daten: die Summe der Ausgabegröße und der Shuffle-Schreibgröße.

    * Ausführungszeit: die Zeit zwischen der Startzeit des ersten Versuchs und der Endzeit des letzten Versuchs

    * Zeilenanzahl: die Summe der Eingabe und Ausgabedatensätze sowie die Datensätze der zufälligen Lese- und Schreibvorgänge

    * Status

     ![Abgeschlossenen Auftrag anzeigen](./media/how-to-monitor-spark-applications/view-completed-job.png)
    
11. Wenn Sie auf das Diagramm klicken, werden die Details für die Phase angezeigt.

   ![Details für Phase](./media/how-to-monitor-spark-applications/details-for-stage.png)

## <a name="view-canceled-apache-spark-application"></a>Anzeigen abgebrochener Apache Spark-Anwendungen

Öffnen Sie **Überwachen**, und wählen Sie dann **Apache Spark-Anwendungen** aus. Um die Details zu abgebrochenen Apache Spark-Anwendungen anzuzeigen, wählen Sie die Apache Spark-Anwendung aus, und zeigen Sie die Details an.

 ![Abgebrochenen Auftrag auswählen](./media/how-to-monitor-spark-applications/select-cancelled-job.png) 

1. Überprüfen Sie **Abgeschlossene Aufgaben**, **Status** und **Gesamtdauer**.

2. Aktualisieren Sie die Protokollabfrage.

3. Öffnen Sie den Link zum Apache Spark-Verlaufsserver, indem Sie auf **Spark-Verlaufsserver** klicken.

4. Sehen Sie sich das Diagramm an.

5. Überprüfen Sie die Informationen unter **Zusammenfassung**.

6. Überprüfen Sie die **Protokolle**. Sie können in der Dropdownliste verschiedene Protokolltypen auswählen, und Sie können die Protokollinformationen herunterladen, indem Sie auf **Protokolle herunterladen** klicken.

   ![Abgebrochenen Auftrag anzeigen](./media/how-to-monitor-spark-applications/view-cancelled-job.png)

## <a name="debug-failed-apache-spark-application"></a>Debuggen von Fehlern bei Apache Spark-Anwendungen

Öffnen Sie **Überwachen**, und wählen Sie dann **Apache Spark-Anwendungen** aus. Um die Details zu Apache Spark-Anwendungen mit Fehlern anzuzeigen, wählen Sie die Apache Spark-Anwendung aus, und zeigen Sie die Details an.

![Auftrag mit Fehlern auswählen](./media/how-to-monitor-spark-applications/select-failed-job.png)

1. Überprüfen Sie **Abgeschlossene Aufgaben**, **Status** und **Gesamtdauer**.

2. Aktualisieren Sie die Protokollabfrage.

3. Öffnen Sie den Link zum Apache Spark-Verlaufsserver, indem Sie auf **Spark-Verlaufsserver** klicken.

4. Sehen Sie sich das Diagramm an.

5. Überprüfen Sie die Informationen unter **Zusammenfassung**.

6. Überprüfen Sie die Fehlerinformationen.

   ![Informationen zu Auftrag mit Fehlern](./media/how-to-monitor-spark-applications/failed-job-info.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Überwachen von Pipelineausführungen finden Sie im Artikel [Überwachen von Pipelineausführungen mithilfe von Synapse Studio](how-to-monitor-pipeline-runs.md).  
