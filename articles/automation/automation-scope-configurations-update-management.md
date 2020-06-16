---
title: Einschränken des Bereitstellungsumfangs der Azure Automation-Updateverwaltung
description: In diesem Artikel wird erläutert, wie die Bereichskonfigurationen verwendet werden, um den Umfang einer Bereitstellung der Updateverwaltung zu beschränken.
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 72065b388f348da1d268f875a10d5b13d2f8cf3b
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "84117401"
---
# <a name="limit-update-management-deployment-scope"></a>Einschränken des Bereitstellungsumfangs der Updateverwaltung

In diesem Artikel erfahren Sie, wie Sie mit Bereichskonfigurationen arbeiten, wenn Sie das Feature [Updateverwaltung](automation-update-management.md) verwenden, um Updates und Patches für Ihre VMs bereitzustellen. Weitere Informationen finden Sie unter [Zielgruppenadressierung für Überwachungslösungen in Azure Monitor (Vorschau)](https://docs.microsoft.com/azure/azure-monitor/insights/solution-targeting). 

## <a name="about-scope-configurations"></a>Informationen zu Bereichskonfigurationen

Eine Bereichskonfiguration enthält mindestens eine gespeicherte Suche oder eine Gruppe von gespeicherten Suchen (Abfragen), um den Bereich der Updateverwaltung auf bestimmte Computer zu begrenzen. Die Bereichskonfiguration wird innerhalb des Log Analytics-Arbeitsbereichs verwendet, um die Computer anzugeben, für die ein Feature aktiviert werden soll. Wenn Sie einen Computer hinzufügen, um Updates von der Updateverwaltung zu empfangen, wird der betreffende Computer außerdem einer gespeicherten Suche im Arbeitsbereich hinzugefügt.

## <a name="set-the-scope-limit"></a>Festlegen der Bereichsgrenze

So schränken Sie den Geltungsbereich für Ihre Bereitstellung der Updateverwaltung ein:

1. Wählen Sie in Ihrem Automation-Konto unter **Verwandte Ressourcen** die Option **Verknüpfter Arbeitsbereich** aus.

2. Klicken Sie auf **Zu Arbeitsbereich wechseln**.

3. Wählen Sie unter **Arbeitsbereichsdatenquellen** die Option **Bereichskonfigurationen (Vorschau)** aus.

4. Wählen Sie die Auslassungspunkte rechts neben der Bereichskonfigurationen `MicrosoftDefaultScopeConfig-Updates` aus, und klicken Sie auf **Bearbeiten**. 

5. Klappen Sie im Bearbeitungsbereich die Option **Computergruppen auswählen** auf. Im Bereich „Computergruppen“ werden die gespeicherten Suchen angezeigt, die zum Erstellen der Bereichskonfiguration verwendet werden. Von der Updateverwaltung wird die folgende gespeicherte Suche verwendet:

    |Name     |Category  |Alias  |
    |---------|---------|---------|
    |MicrosoftDefaultComputerGroup     | Aktualisierungen        | Updates__MicrosoftDefaultComputerGroup         |

6. Wählen Sie die gespeicherte Suche aus, um die zum Auffüllen der Gruppe verwendete Abfrage anzuzeigen und zu bearbeiten. Die folgende Abbildung zeigt die Abfrage und ihre Ergebnisse:

    ![Gespeicherte Suchvorgänge](media/automation-scope-configurations-update-management/logsearch.png)

## <a name="next-steps"></a>Nächste Schritte

* Informationen zur Verwendung des Features finden Sie unter [Verwalten von Updates und Patches für Ihre virtuellen Azure-Computer](automation-tutorial-update-management.md).
* Informationen zum Behandeln von Fehlern beim Feature finden Sie unter [Beheben von Problemen mit der Lösung für die Updateverwaltung](troubleshoot/update-management.md).
* Informationen zum Behandeln von Problemen mit dem Windows Update-Agent finden Sie unter [Beheben von Problemen mit dem Windows Update-Agent](troubleshoot/update-agent-issues.md).
* Informationen zum Behandeln von Problemen mit dem Linux Update-Agent finden Sie unter [Beheben von Problemen mit dem Linux Update-Agent](troubleshoot/update-agent-issues-linux.md).