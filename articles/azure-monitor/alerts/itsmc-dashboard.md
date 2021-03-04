---
title: Untersuchen von Fehlern mithilfe des ITSMC-Dashboards
description: Erfahren Sie, wie Sie das ITSMC-Dashboard (ITSM-Connector) nutzen, um Fehler zu untersuchen.
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 01/15/2021
ms.openlocfilehash: 5e1acf422abf487edda3e871fa99d07212c59b3a
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102039528"
---
# <a name="investigate-errors-by-using-the-itsmc-dashboard"></a>Untersuchen von Fehlern mithilfe des ITSMC-Dashboards

Dieser Artikel enthält Informationen zum ITSMC-Dashboard (ITSM-Connector). Mithilfe dieses Dashboards können Sie den Status Ihres Connectors untersuchen.

## <a name="view-errors"></a>Anzeigen von Fehlern

So zeigen Sie Fehler im Dashboard an:

1. Wählen Sie **Alle Ressourcen** aus, und suchen Sie nach **ServiceDesk(*IhrArbeitsbereichsname*)** .

   ![Screenshot mit den Ressourcen in Azure-Diensten.](media/itsmc-definition/create-new-connection-from-resource.png)

2. Wählen Sie im linken Bereich unter **Arbeitsbereichsdatenquellen** die Option **ITSM-Verbindungen** aus:

   ![Screenshot zur Auswahl von ITSM-Verbindungen unter „Arbeitsbereichsdatenquellen“.](media/itsmc-overview/add-new-itsm-connection.png)

3. Wählen Sie unter **Zusammenfassung** im Bereich **ITSM-Connector** die Option **Zusammenfassung anzeigen** aus:

   ![Screenshot mit der Schaltfläche „Zusammenfassung anzeigen“.](media/itsmc-resync-servicenow/dashboard-view-summary.png)

4. Wenn im Bereich **ITSM-Connector** ein Diagramm angezeigt wird, wählen Sie dieses Diagramm aus:

   ![Screenshot zur Auswahl eines Diagramms.](media/itsmc-resync-servicenow/dashboard-graph-click.png)

5. Das Dashboard wird angezeigt. Überprüfen Sie anhand des Dashboards den Status und die Fehler Ihres Connectors.
   
   ![Screenshot mit dem Connectorstatus im Dashboard.](media/itsmc-resync-servicenow/connector-dashboard.png)

## <a name="understand-dashboard-elements"></a>Grundlegendes zu Dashboardelementen

Das Dashboard enthält Informationen zu den Warnungen, die über diesen Connector an das ITSM-Tool gesendet wurden. Das Dashboard ist in vier Bereiche unterteilt.

### <a name="created-work-items"></a>Erstellte Arbeitselemente 

Das Diagramm und die Tabelle im Bereich **Erstellte Arbeitselemente** enthalten die Anzahl von Arbeitselementen pro Typ. Wenn Sie das Diagramm oder die Tabelle auswählen, können Sie weitere Details zu den Arbeitselementen anzeigen.

![Screenshot eines erstellten Arbeitselements.](media/itsmc-resync-servicenow/itsm-dashboard-workitems.png)

### <a name="affected-computers"></a>Betroffene Computer 

In der Tabelle im Bereich **Betroffene Computer** sind Computer und die zugehörigen Arbeitselemente aufgelistet. Wählen Sie Zeilen in den Tabellen aus, um weitere Details zu den Computern zu erhalten.

Die Tabelle enthält eine begrenzte Anzahl von Zeilen. Wenn Sie alle Zeilen anzeigen möchten, wählen Sie **Alle anzeigen** aus.

![Screenshot der betroffenen Computer.](media/itsmc-resync-servicenow/itsm-dashboard-impacted-comp.png)

### <a name="connector-status"></a>Connectorstatus 

Das Diagramm und die Tabelle im Bereich **Connectorstatus** enthalten Meldungen zum Status des Connectors. Wählen Sie das Diagramm oder Zeilen in der Tabelle aus, um weitere Details zu den Meldungen zu erhalten.

Die Tabelle enthält eine begrenzte Anzahl von Zeilen. Wenn Sie alle Zeilen anzeigen möchten, wählen Sie **Alle anzeigen** aus.

Weitere Informationen zu den Meldungen in der Tabelle finden Sie in [diesem Artikel](itsmc-dashboard-errors.md).

![Screenshot mit dem Connectorstatus](media/itsmc-resync-servicenow/itsm-dashboard-connector-status.png)

### <a name="alert-rules"></a>Warnungsregeln 

Die Tabelle im Bereich **Warnungsregeln** enthält Informationen zur Anzahl von erkannten Warnungsregeln. Wählen Sie Zeilen in der Tabelle aus, um weitere Details zu den erkannten Regeln zu erhalten.
    
Die Tabelle enthält eine begrenzte Anzahl von Zeilen. Wenn Sie alle Zeilen anzeigen möchten, wählen Sie **Alle anzeigen** aus.

![Screenshot der Warnungsregeln](media/itsmc-resync-servicenow/itsm-dashboard-alert-rules.png)
