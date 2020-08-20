---
title: Workflowautomatisierung in Azure Security Center | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Workflows in Azure Security Center erstellen und automatisieren.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 19fcefdfd163bcbd9cd2adfcba6bf6711b9bee18
ms.sourcegitcommit: 1a0dfa54116aa036af86bd95dcf322307cfb3f83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2020
ms.locfileid: "88041669"
---
# <a name="workflow-automation"></a>Workflowautomatisierung

Jedes Sicherheitsprogramm umfasst mehrere Workflows für die Reaktion auf Vorfälle. Diese Prozesse können das Benachrichtigen relevanter Stakeholder, das Starten eines Change Management-Prozesses und das Anwenden spezifischer Korrekturschritte umfassen. Sicherheitsexperten empfehlen, möglichst viele Schritte dieser Verfahren zu automatisieren. Durch Automatisierung wird der Aufwand reduziert. Außerdem können Sie so die Sicherheit erhöhen, indem Sie sicherstellen, dass die Prozessschritte schnell, konsistent und gemäß Ihren vordefinierten Anforderungen ausgeführt werden.

In diesem Artikel wird die Funktion zur Workflowautomatisierung von Azure Security Center beschrieben. Dieses Feature kann Logic Apps bei Sicherheitswarnungen und Empfehlungen auslösen. Beispielsweise können Sie von Security Center E-Mail-Nachrichten an einen bestimmten Benutzer senden, wenn eine Warnung auftritt. Außerdem erfahren Sie, wie Sie Logik-Apps mithilfe von [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview) erstellen.

> [!NOTE]
> Wenn Sie zuvor die Ansicht „Playbooks“ (Vorschauversion) auf der Seitenleiste verwendet haben, finden Sie dieselben Funktionen sowie die erweiterten Funktionen auf der neuen Seite „Workflowautomatisierung“.



## <a name="availability"></a>Verfügbarkeit

|Aspekt|Details|
|----|:----|
|Status des Release:|Allgemein verfügbar|
|Preise:|Free-Tarif|
|Erforderliche Rollen und Berechtigungen:|**Leser** für das Abonnement, das die Exportkonfiguration enthält<br>Rolle **Sicherheitsadministrator** oder **Besitzer** für die Ressourcengruppe<br>Außerdem sind Schreibberechtigungen für die Zielressource erforderlich<br><br>Damit Sie Azure Logic Apps-Workflows verwenden können, benötigen Sie zudem die folgenden Logic Apps-Rollen/-Berechtigungen:<br> Die Berechtigungen der Rolle - [Logik-App-Operator](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#logic-app-operator) oder der Lese-/Triggerzugriff für Logik-Apps sind erforderlich. (Diese Rolle kann keine Logik-Apps erstellen oder bearbeiten, sondern nur vorhandene *ausführen*.)<br> Die Berechtigungen der Rolle - [Logik-App-Mitwirkender](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#logic-app-contributor) sind für die Erstellung und Änderung von Logik-Apps erforderlich.<br>Wenn Sie Logik-App-Connectors verwenden möchten, benötigen Sie möglicherweise zusätzliche Anmeldeinformationen für die Anmeldung bei den jeweiligen Diensten (z. B. Ihren Instanzen von Outlook, Teams oder Slack).|
|Clouds:|![Ja](./media/icons/yes-icon.png) Kommerzielle Clouds<br>![Ja](./media/icons/yes-icon.png) US Gov<br>![Nein](./media/icons/no-icon.png) China Gov/andere Gov-Clouds|
|||



## <a name="create-a-logic-app-and-define-when-it-should-automatically-run"></a>Erstellen einer Logik-App und Definieren des Zeitpunkts ihrer automatischen Ausführung 

1. Wählen Sie auf der Seitenleiste in Security Center **Workflowautomatisierung** aus.

    [![Liste der Workflowautomatisierungen](media/workflow-automation/list-of-workflow-automations.png)](media/workflow-automation/list-of-workflow-automations.png#lightbox)

    Auf dieser Seite können Sie neue Automatisierungsregeln erstellen sowie vorhandene aktivieren, deaktivieren oder löschen.  
1. Um einen neuen Workflow zu definieren, klicken Sie auf **Workflowautomatisierung hinzufügen**. 

    Ein Bereich mit Optionen für die neue Automatisierung wird angezeigt. Darin können Sie Folgendes eingeben:
    1. Einen Namen und eine Beschreibung für die Automatisierung
    1. Die Trigger zum Auslösen dieses automatischen Workflows. Sie könnten beispielsweise Ihre Logik-App ausführen, wenn eine Sicherheitswarnung generiert wird, die „SQL“ enthält.
    1. Die Logik-App, die ausgeführt wird, wenn die Triggerbedingungen erfüllt sind 

        [![Liste der Workflowautomatisierungen](media/workflow-automation/add-workflow.png)](media/workflow-automation/add-workflow.png#lightbox)

1. Klicken Sie im Abschnitt „Aktionen“ auf **Erstellen Sie eine neue**, um mit der Erstellung der Logik-App zu beginnen.

    Sie werden zu Azure Logic Apps umgeleitet.

    [![Erstellen einer neuen Logik-App](media/workflow-automation/logic-apps-create-new.png)](media/workflow-automation/logic-apps-create-new.png#lightbox)

1. Geben Sie einen Namen, eine Ressourcengruppe und einen Speicherort ein, und klicken Sie auf **Erstellen**.

1. Sie können in Ihrer neuen Logik-App zwischen integrierten, vordefinierten Vorlagen der Kategorie „Sicherheit“ auswählen. Sie können aber auch einen benutzerdefinierten Ereignisflow definieren, der beim Auslösen dieses Prozesses gestartet wird.

    Im Logik-App-Designer werden die folgenden Trigger der Security Center-Connectors unterstützt:

    * **Beim Erstellen oder Auslösen einer Azure Security Center-Empfehlung**
    * **Beim Erstellen oder Auslösen einer Azure Security Center-Warnung** 
    
    > [!TIP]
    > Sie können den Auslöser so anpassen, dass er sich nur auf Warnungen mit den für Sie interessanten Schweregraden bezieht.
    
    > [!NOTE]
    > Wenn Sie den älteren Trigger „Beim Auslösen einer Antwort auf eine Azure Security Center-Warnung“ verwenden, wird Ihre Logik-App nicht von der Funktion Workflowautomatisierung gestartet. Verwenden Sie stattdessen einen der oben genannten Trigger. 

    [![Beispiel-Logik-App](media/workflow-automation/sample-logic-app.png)](media/workflow-automation/sample-logic-app.png#lightbox)

1. Kehren Sie nach dem Definieren Ihrer Logik-App zum Bereich zur Definition der Workflowautomatisierung („Workflowautomatisierung hinzufügen“) zurück. Klicken Sie auf **Aktualisieren**, um sicherzustellen, dass Ihre neue Logik-App als Auswahl verfügbar ist.

    ![Aktualisieren](media/workflow-automation/refresh-the-list-of-logic-apps.png)

1. Wählen Sie Ihre Logik-App aus, und speichern Sie die Automatisierung. Beachten Sie, dass in der Logik-App-Dropdownliste nur Logik-Apps mit unterstützenden Security Center-Connectors angezeigt werden.


## <a name="manually-trigger-a-logic-app"></a>Manuelles Auslösen einer Logik-App

Sie können Logik-Apps auch manuell ausführen, wenn Sie eine Sicherheitswarnung oder eine Empfehlung anzeigen, die eine [schnelle Problembehebung](https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations#quick-fix-remediation) anbietet.

Um eine Logik-App manuell auszuführen, öffnen Sie eine Warnung oder eine Empfehlung, die die schnelle Problembehebung unterstützt, und klicken Sie auf **Logik-App auslösen** :

[![Manuelles Auslösen einer Logik-App](media/workflow-automation/manually-trigger-logic-app.png)](media/workflow-automation/manually-trigger-logic-app.png#lightbox)

## <a name="data-types-schemas"></a>Datentypenschemas

Um die unformatierten Ereignisschemas der Sicherheitswarnungen oder Empfehlungsereignisse anzuzeigen, die an die Logik-App-Instanz übermittelt werden, sehen Sie sich die [Schemas für Workflowautomatisierungs-Datentypen](https://aka.ms/ASCAutomationSchemas) an. Dies kann nützlich sein, wenn Sie nicht die oben genannten integrierten Logik-App-Connectors von Security Center verwenden, sondern den generischen HTTP-Connector der Logik-App. Sie können die Analyse bei Bedarf mit dem JSON-Ereignisschema manuell durchführen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie Sie Logik-Apps erstellen, deren Ausführung in Security Center automatisieren und sie manuell ausführen. 

Weitere verwandte Informationen finden Sie auf hier: 

- [Das Microsoft Learn-Modul über die Verwendung der Workflowautomatisierung zur Automatisierung einer Sicherheitsreaktion](https://docs.microsoft.com/learn/modules/resolve-threats-with-azure-security-center/)
- [Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md)
- [Sicherheitswarnungen in Azure Security Center](security-center-alerts-overview.md)
- [Informationen zu Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview)
- [Logik-Apps-Connectors](https://docs.microsoft.com/connectors/)
- [Workflowautomatisierungs-Datentypenschemas](https://aka.ms/ASCAutomationSchemas)
