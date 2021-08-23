---
title: Erkunden von Azure Stream Analytics-Aufträgen mit Visual Studio Code
description: In diesem Artikel wird veranschaulicht, wie ein Azure Stream Analytics-Auftrag in ein lokales Projekt exportiert wird, Aufträge aufgelistet und Auftragsentitäten angezeigt werden können.
ms.service: stream-analytics
author: su-jie
ms.author: sujie
ms.date: 07/21/2021
ms.topic: how-to
ms.openlocfilehash: 85b6dece608f47818f2115d0462dd4a56752581f
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122349560"
---
# <a name="explore-azure-stream-analytics-with-visual-studio-code-preview"></a>Erkunden von Azure Stream Analytics mit Visual Studio Code (Vorschauversion)

Die Azure Stream Analytics-Erweiterung für Visual Studio Code bietet Entwicklern eine einfache Verwaltung Ihrer Stream Analytics-Aufträge. Sie funktioniert unter Windows, Mac und Linux. Mit der Azure Stream Analytics-Erweiterung ist Folgendes möglich:

- [Erstellen](quick-create-visual-studio-code.md), Starten und Aufträge beenden
- Exportieren vorhandener Aufträge in ein lokales Projekt
- Auflisten von Aufträgen und Anzeigen von Auftragsentitäten
- Anzeigen des Auftragsdiagramms und Debuggen im Auftragsmonitor

## <a name="export-a-job-to-a-local-project"></a>Exportieren eines Auftrags in ein lokales Projekt

Suchen Sie den zu exportierenden Auftrag im **Stream Analytics-Explorer** in Visual Studio Code, um einen Auftrag in ein lokales Projekt zu exportieren. Wählen Sie dann einen Ordner für Ihr Projekt aus. Das Projekt wird in den von Ihnen ausgewählten Ordner exportiert, und Sie können den Auftrag weiterhin über Visual Studio Code verwalten. Weitere Informationen zur Verwendung von Visual Studio Code zum Verwalten von Stream Analytics-Aufträgen finden Sie im Visual Studio Code-[Schnellstartanleitung](quick-create-visual-studio-code.md).

![Exportieren von ASA-Aufträgen in Visual Studio Code](./media/vscode-explore-jobs/export-job.png)

## <a name="list-job-and-view-job-entities"></a>Auflisten von Aufträgen und Anzeigen von Auftragsentitäten

In der Auftragsansicht können Sie mit Azure Stream Analytics-Aufträgen aus Visual Studio arbeiten.


1. Klicken Sie auf der Aktivitätsleiste von Visual Studio Code auf das **Azure**-Symbol, und erweitern Sie dann den Knoten **Stream Analytics**. Ihre Aufträge sollten unter Ihren Abonnements angezeigt werden.

   ![Öffnen von Stream Analytics-Explorer](./media/vscode-explore-jobs/open-explorer.png)

2. Erweitern Sie Ihren Auftragsknoten, und Sie können die Auftragsabfrage, Konfiguration, Eingaben, Ausgaben und Funktionen öffnen und anzeigen. 

3. Klicken Sie mit der rechten Maustaste auf Ihren Auftragsknoten, und wählen Sie den Knoten **Auftragsansicht im Portal öffnen** aus, um die Auftragsansicht im Azure-Portal zu öffnen.

   ![Auftragsansicht im Portal öffnen](./media/vscode-explore-jobs/open-job-view.png)

## <a name="view-job-diagram-and-debug-in-job-monitor"></a>Anzeigen des Auftragsdiagramms und Debuggen im Auftragsmonitor

Sie können den Auftragsmonitor in Visual Studio Code verwenden, um Ihre Azure Stream Analytics-Aufträge anzuzeigen und Probleme damit zu behandeln.

### <a name="view-job-diagram-and-job-summary"></a>Anzeigen von Auftragsdiagramm und Auftragszusammenfassung
1. Wählen Sie **Auftragsmonitor** aus. Ihr Auftragsmonitor sollte angezeigt werden, und das Auftragsdiagramm sollte automatisch geladen werden.
   
   ![Öffnen des Auftragsmonitors](./media/vscode-explore-jobs/open-job-monitor.png)

2.  Sie können ihr Auftragsdiagramm anzeigen und auf **Auftragszusammenfassung** klicken, um Eigenschaften und Informationen ihres Auftrags anzuzeigen. 

      ![Anzeigen der Auftragszusammenfassung](./media/vscode-explore-jobs/view-jobs-summary.png)

3.  Sie können auf die Schaltfläche **Verbindung testen** klicken, um die Verbindung mit Ihrer Eingabe und Ausgabe zu testen.

      ![Testen der Verbindung](./media/vscode-explore-jobs/test-connection.png)

4.  Sie können auch auf die Schaltfläche **Skript suchen** klicken, um Ihre Abfrage anzuzeigen.
   
      ![Anzeigen der Abfrage](./media/vscode-explore-jobs/view-query.png)

### <a name="monitor-and-debug-with-metrics"></a>Überwachen und Debuggen mit Metriken

1.  Klicken Sie auf die Pfeilschaltfläche, um den Bereich „Metriken“ zu öffnen.

      ![Öffnen des Bereichs „Metriken“](./media/vscode-explore-jobs/open-metrics-panel.png)

2.  Sie können damit interagieren und Ihren Auftrag mit wichtigen Metriken analysieren, was im Diagramm angezeigt wird. Sie können Metriken auf Auftragsebene oder Metriken auf Knotenebene anzeigen. Sie können außerdem entscheiden, welche Metriken im Diagramm angezeigt werden sollen.

      ![Anzeigen von Metriken](./media/vscode-explore-jobs/view-metrics.png)

### <a name="debug-with-diagnostic-logs-and-activity-logs"></a>Debuggen mit Diagnoseprotokollen und Aktivitätsprotokollen

Sie können die Diagnoseprotokolle und Aktivitätsprotokolle Ihres Auftrags zur Problembehandlung anzeigen.

1. Wählen Sie die Registerkarte **Diagnoseprotokolle** aus.

   ![Anzeigen von Diagnoseprotokollen](./media/vscode-explore-jobs/view-diagnostic-log.png)

2. Wählen Sie die Registerkarte **Aktivitätsprotokolle** aus. 

   ![Anzeigen von Aktivitätsprotokollen](./media/vscode-explore-jobs/view-activity-logs.png)

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen eines Azure Stream Analytics-Cloudauftrags in Visual Studio Code (Vorschauversion)](quick-create-visual-studio-code.md)
