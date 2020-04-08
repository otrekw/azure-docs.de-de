---
title: Verwenden von Azure Monitor-Protokollen mit Azure Logic Apps und Power Automate
description: Erfahren Sie, wie Sie mit Azure Logic Apps und Power Automate wiederholbare Prozesse mithilfe des Azure Monitor-Connectors schnell automatisieren können.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/13/2020
ms.openlocfilehash: 6961b7bd94c9b3fe70365055851c488efa2cbeca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79480010"
---
# <a name="azure-monitor-logs-connector-for-logic-apps-and-flow"></a>Connector für Azure Monitor-Protokolle für Azure Logic Apps und Flow
Mit [Azure Logic Apps](/azure/logic-apps/) und [Power Automate](https://ms.flow.microsoft.com) können Sie automatisierte Workflows erstellen, indem Sie Hunderte von Aktionen für eine Vielzahl von Diensten verwenden. Mit dem Connector für Azure Monitor-Protokolle können Sie Workflows erstellen, die Daten aus einem Log Analytics-Arbeitsbereich oder einer Application Insights-Anwendung in Azure Monitor abrufen. In diesem Artikel werden die mit dem Connector möglichen Aktionen beschrieben. Außerdem wird eine Anleitung zur Erstellung eines Workflows unter Verwendung dieser Daten gegeben.

Beispielsweise können Sie eine Logik-App zum Verwenden von Azure Monitor-Protokolldaten in einer E-Mail-Benachrichtigung aus Office 365, zum Erstellen eines Fehlers in Azure DevOps oder zum Posten einer Slack-Nachricht nutzen.  Sie können einen Workflow auslösen, indem Sie einen einfachen Zeitplan oder eine Aktion in einem verbundenen Dienst verwenden, z.B. bei Erhalt einer E-Mail oder eines Tweets. 

## <a name="actions"></a>Aktionen
In der folgenden Tabelle werden die Aktionen beschrieben, die mit dem Connector für die Azure Monitor-Protokolle möglich sind. Beide ermöglichen Ihnen, eine Protokollabfrage auf einen Log Analytics-Arbeitsbereich oder eine Application Insights-Anwendung anzuwenden. Der Unterschied besteht darin, wie die Daten zurückgegeben werden.

> [!NOTE]
> Der Connector für Azure Monitor-Protokolle ersetzt den [Azure Log Analytics-Connector](https://docs.microsoft.com/connectors/azureloganalytics/) und [Azure Application Insights-Connector](https://docs.microsoft.com/connectors/applicationinsights/). Dieser Konnektor bietet die gleiche Funktionalität wie die anderen und ist die bevorzugte Methode zum Anwenden einer Abfrage auf einen Log Analytics-Arbeitsbereich oder eine Application Insights-Anwendung.


| Aktion | Beschreibung |
|:---|:---|
| [Abfrage ausführen und Ergebnisse auflisten](https://docs.microsoft.com/connectors/azuremonitorlogs/#run-query-and-list-results) | Gibt jede Zeile als eigenes Objekt zurück. Verwenden Sie diese Aktion, wenn Sie im weiteren Verlauf des Workflows mit jeder Zeile einzeln arbeiten möchten. Auf die Aktion folgt in der Regel eine [FOR EACH-Aktivität](../../logic-apps/logic-apps-control-flow-loops.md#foreach-loop). |
| [Abfrage ausführen und Ergebnisse visualisieren](https://docs.microsoft.com/connectors/azuremonitorlogs/#run-query-and-visualize-results) | Gibt alle Zeilen im Resultset als einzelnes formatiertes Objekt zurück. Wählen Sie diese Aktion, wenn Sie das Resultset zusammen im restlichen Workflow verwenden wollen, z. B. wenn Sie die Ergebnisse in einer E-Mail senden möchten.  |

## <a name="walkthroughs"></a>Exemplarische Vorgehensweisen
Die folgenden Tutorials veranschaulichen die Verwendung der Azure Monitor-Connectors in Azure Logic Apps. Sie können dasselbe Beispiel mit Power Automate durchführen. Der einzige Unterschied besteht darin, wie Sie den anfänglichen Workflow erstellen und anschließend ausführen. Die Konfiguration des Workflows und der Aktionen ist bei beiden identisch. Lesen Sie zum Einstieg [Erstellen eines Flows anhand einer Vorlage in Power Automate](https://docs.microsoft.com/power-automate/get-started-logic-template).


### <a name="create-a-logic-app"></a>Erstellen einer Logik-App

Navigieren Sie im Azure-Portal zu **Logic Apps**, und klicken Sie auf **Hinzufügen**. Wählen Sie ein **Abonnement**, eine **Ressourcengruppe** und eine **Region** aus, um die neue Logik-App zu speichern, und geben Sie ihr einen eindeutigen Namen. Sie können die Einstellung **Log Analytics** aktivieren, um Informationen über Laufzeitdaten und Ereignisse zu sammeln, wie unter [Einrichten von Azure Monitor-Protokollen und Sammeln von Diagnosedaten für Azure Logic Apps](../../logic-apps/monitor-logic-apps-log-analytics.md) beschrieben. Diese Einstellung ist für die Verwendung des Connectors für Azure Monitor-Protokolle nicht erforderlich.

![Erstellen einer Logik-App](media/logicapp-flow-connector/create-logic-app.png)


Klicken Sie auf **Überprüfen + erstellen** und danach auf **Erstellen**. Wenn die Bereitstellung abgeschlossen ist, klicken Sie auf **Zu Ressource wechseln**, um den **Designer für Logik-Apps** zu öffnen.

### <a name="create-a-trigger-for-the-logic-app"></a>Erstellen eines Triggers für die Logik-App
Wählen Sie unter **Starten Sie mit einem gängigen Trigger** die Option **Wiederholung** aus. Dadurch wird eine Logik-App erstellt, die in regelmäßigen Abständen automatisch ausgeführt wird. Wählen Sie im Feld **Häufigkeit** der Aktion **Tag** aus, und geben Sie in das Feld **Intervall** den Wert **1** ein, um den Workflow einmal pro Tag auszuführen.

![Wiederholungsaktion](media/logicapp-flow-connector/recurrence-action.png)

## <a name="walkthrough-mail-visualized-results"></a>Exemplarische Vorgehensweise: Senden visualisierter Ergebnisse per E-Mail
Das folgende Tutorial zeigt Ihnen, wie Sie eine Logik-App erstellen, die die Ergebnisse einer Azure Monitor-Protokollabfrage per E-Mail sendet. 

### <a name="add-azure-monitor-logs-action"></a>Hinzufügen der Aktion „Azure Monitor-Protokolle“
Klicken Sie auf **+ Neuer Schritt**, um eine Aktion hinzuzufügen, die nach der Wiederholungsaktion ausgeführt wird. Geben Sie unter **Aktion auswählen** den Text **azure monitor** ein, und wählen Sie dann **Azure Monitor-Protokolle** aus.

![Aktion „Azure Monitor-Protokolle“](media/logicapp-flow-connector/select-azure-monitor-connector.png)

Klicken Sie auf **Azure Log Analytics – Abfrage ausführen und Ergebnisse visualisieren**.

![Aktion „Abfrage ausführen und Ergebnisse visualisieren“](media/logicapp-flow-connector/select-query-action-visualize.png)


### <a name="add-azure-monitor-logs-action"></a>Hinzufügen der Aktion „Azure Monitor-Protokolle“

Wählen Sie das **Abonnement** und die **Ressourcengruppe** für Ihren Log Analytics-Arbeitsbereich aus. Wählen Sie *Log Analytics-Arbeitsbereich* als **Ressourcentyp** und anschließend unter **Ressourcenname** den Namen des Arbeitsbereichs aus.

Fügen Sie dem Fenster **Abfrage** die unten angegebene Protokollabfrage hinzu.  

```Kusto
Event
| where EventLevelName == "Error" 
| where TimeGenerated > ago(1day)
| summarize TotalErrors=count() by Computer
| sort by Computer asc   
```

Wählen Sie *In Abfrage festlegen* für **Zeitbereich** und **HTML-Tabelle** für **Diagrammtyp** aus.
   
![Aktion „Abfrage ausführen und Ergebnisse visualisieren“](media/logicapp-flow-connector/run-query-visualize-action.png)

Die E-Mail wird vom Konto gesendet, das der aktuellen Verbindung zugeordnet ist. Sie können ein anderes Konto angeben, indem Sie auf **Verbindung ändern** klicken.

### <a name="add-email-action"></a>Hinzufügen der Aktion „E-Mail“

Klicken Sie auf **+ Neuer Schritt** und anschließend auf **+ Aktion hinzufügen**. Geben Sie unter **Aktion wählen** den Text **outlook** ein, und wählen Sie dann **Office 365 Outlook** aus.

![Outlook-Connector auswählen](media/logicapp-flow-connector/select-outlook-connector.png)

Wählen Sie **E-Mail senden (V2)** aus.

![Fenster zum Auswählen von Office 365 Outlook](media/logicapp-flow-connector/select-mail-action.png)

Klicken Sie auf eine beliebige Stelle im Feld **Textkörper**, um das Fenster **Dynamischer Inhalt** mit den Werten aus den vorherigen Aktionen in der Logik-App zu öffnen. Wählen Sie **Mehr anzeigen** und dann **Textkörper** aus, wobei es sich um die Ergebnisse der Abfrage in der Log Analytics-Aktion handelt.

![Auswählen eines Textkörpers](media/logicapp-flow-connector/select-body.png)

Geben Sie die E-Mail-Adresse eines Empfängers im Fenster **An** und einen Betreff für die E-Mail unter **Betreff** an. 

![Aktion „E-Mail“](media/logicapp-flow-connector/mail-action.png)


### <a name="save-and-test-your-logic-app"></a>Speichern und Testen Ihrer Logik-App
Klicken Sie auf **Speichern** und dann auf **Ausführen**, um einen Testlauf der Logik-App durchzuführen.

![Speichern und ausführen](media/logicapp-flow-connector/save-run.png)


Prüfen Sie nach Abschluss der Logik-App den Posteingang des von Ihnen angegebenen Empfängers.  Es sollte eine E-Mail mit einem Text eingegangen sein, der etwa wie folgt lautet:

![Beispiel-E-Mail](media/logicapp-flow-connector/sample-mail.png)



## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Protokollabfragen in Azure Monitor](../log-query/log-query-overview.md).
- Weitere Informationen zu [Logik-Apps](/azure/logic-apps/)
- Erfahren Sie mehr über [Microsoft Flow](https://ms.flow.microsoft.com).

