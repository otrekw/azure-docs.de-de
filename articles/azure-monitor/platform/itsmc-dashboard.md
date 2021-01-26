---
title: Untersuchung von Fehlern auf dem Dashboard
description: Dieses Dokument enthält Informationen zur Untersuchung von Fehlern auf dem Dashboard.
ms.subservice: alerts
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 01/15/2021
ms.openlocfilehash: 9a43318ed935dd1e71b6da75c9f078fbebbd29eb
ms.sourcegitcommit: fc401c220eaa40f6b3c8344db84b801aa9ff7185
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2021
ms.locfileid: "98599022"
---
# <a name="error-investigation-using-the-dashboard"></a>Untersuchung von Fehlern auf dem Dashboard

Diese Seite enthält Informationen zum Dashboard des ITSM-Connectors. Mithilfe dieses Dashboards können Sie den Status Ihres ITSM-Connectors untersuchen.

## <a name="how-to-view-the-dashboard"></a>Anzeigen des Dashboards

Gehen Sie folgendermaßen vor, um Fehler auf dem Dashboard anzuzeigen:

1. Suchen Sie in **Alle Ressourcen** nach **ServiceDesk(*IhrArbeitsbereichsname*)** :

   ![Screenshot der zuletzt verwendeten Ressourcen im Azure-Portal](media/itsmc-definition/create-new-connection-from-resource.png)

2. Wählen Sie im linken Bereich unter **Arbeitsbereichsdatenquellen** die Option **ITSM-Verbindungen** aus:

   ![Screenshot des Menüelements „ITSM-Verbindungen“](media/itsmc-overview/add-new-itsm-connection.png)

3. Wählen Sie unter **Zusammenfassung** im linken Feld **ITSM-Connector** die Option **Zusammenfassung anzeigen** aus:

    ![Screenshot der Zusammenfassungsansicht](media/itsmc-resync-servicenow/dashboard-view-summary.png)

4. Klicken Sie unter **Zusammenfassung** im linken Feld **ITSM-Connector** auf das Diagramm:

    ![Screenshot des Klicks auf das Diagramm](media/itsmc-resync-servicenow/dashboard-graph-click.png)

5. Auf diesem Dashboard können Sie den Status und die Fehler in Ihrem Connector überprüfen.
    ![Screenshot mit dem Connectorstatus](media/itsmc-resync-servicenow/connector-dashboard.png)

## <a name="dashboard-elements"></a>Dashboardelemente

Das Dashboard enthält Informationen zu den Warnungen, die mit diesem Connector an das ITSM-Tool gesendet wurden.
Das Dashboard ist in vier Bereiche unterteilt:

1. Erstellte Arbeitselemente: Das Diagramm und die Tabelle unten enthalten die Anzahl der Arbeitselemente pro Typ. Wenn Sie auf das Diagramm oder die Tabelle klicken, können Sie weitere Details zu den Arbeitselementen anzeigen.
    ![Screenshot der erstellten Arbeitselemente](media/itsmc-resync-servicenow/itsm-dashboard-workitems.png)
2. Betroffene Computer: Die Tabellen enthalten Details zu den Konfigurationselementen, die Konfigurationselemente erstellt haben.
    Durch Klicken auf Zeilen in den Tabellen können Sie weitere Details zu den Konfigurationselementen abrufen.
    Die Tabelle enthält eine begrenzte Anzahl von Zeilen. Wenn Sie die vollständige Liste anzeigen möchten, klicken Sie auf „Alle anzeigen“.
    ![Screenshot der betroffenen Computer](media/itsmc-resync-servicenow/itsm-dashboard-impacted-comp.png)
3. Connectorstatus: Das Diagramm und die Tabelle unten enthalten Meldungen zum Status des Connectors. Durch Klicken auf das Diagramm oder auf Zeilen in der Tabelle können Sie weitere Details zu den Meldungen zum Connectorstatus abrufen.
    Die Tabelle enthält eine begrenzte Anzahl von Zeilen. Wenn Sie die vollständige Liste anzeigen möchten, klicken Sie auf „Alle anzeigen“.

    Details zu den Meldungen in der Tabelle finden Sie [hier](itsmc-dashboard-errors.md).

    ![Screenshot mit dem Connectorstatus](media/itsmc-resync-servicenow/itsm-dashboard-connector-status.png)
4. Warnungsregeln: Die Tabellen enthalten Informationen über die Anzahl der erkannten Warnungsregeln.
    Durch Klicken auf Zeilen in den Tabellen können Sie weitere Details zu den erkannten Regeln abrufen.
    Die Tabelle enthält eine begrenzte Anzahl von Zeilen. Wenn Sie die vollständige Liste anzeigen möchten, klicken Sie auf „Alle anzeigen“.
    ![Screenshot der Warnungsregeln](media/itsmc-resync-servicenow/itsm-dashboard-alert-rules.png)