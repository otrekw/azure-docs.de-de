---
title: Einschränken des Bereitstellungsumfangs für Änderungsnachverfolgung und Bestand von Azure Automation
description: Dieser Artikel enthält Informationen zur Verwendung von Bereichskonfigurationen, um den Umfang einer Bereitstellung von Änderungsnachverfolgung und Bestand einzuschränken.
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 49655d11858086b16099a1864fd4d2dc5988f02a
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "84117426"
---
# <a name="limit-change-tracking-and-inventory-deployment-scope"></a>Einschränken des Bereitstellungsumfangs für Änderungsnachverfolgung und Bestand

In diesem Artikel erfahren Sie, wie Sie mit Bereichskonfigurationen arbeiten, wenn Sie das Feature [Änderungsnachverfolgung und Bestand](change-tracking.md) verwenden, um Änderungen für Ihre VMs bereitzustellen. Weitere Informationen finden Sie unter [Zielgruppenadressierung für Überwachungslösungen in Azure Monitor (Vorschau)](https://docs.microsoft.com/azure/azure-monitor/insights/solution-targeting). 

## <a name="about-scope-configurations"></a>Informationen zu Bereichskonfigurationen

Eine Bereichskonfiguration enthält mindestens eine gespeicherte Suche oder eine Gruppe von gespeicherten Suchen (Abfragen), um den Bereich von Änderungsnachverfolgung und Bestand auf bestimmte Computer zu begrenzen. Die Bereichskonfiguration wird innerhalb des Log Analytics-Arbeitsbereichs verwendet, um die Computer anzugeben, für die ein Feature aktiviert werden soll. Wenn Sie einen Computer hinzufügen, um Änderungen von Feature zu empfangen, wird der betreffende Computer außerdem einer gespeicherten Suche im Arbeitsbereich hinzugefügt.

## <a name="set-the-scope-limit"></a>Festlegen der Bereichsgrenze

So schränken Sie den Umfang Ihrer Bereitstellung von Änderungsnachverfolgung und Bestand ein:

1. Wählen Sie in Ihrem Automation-Konto unter **Verwandte Ressourcen** die Option **Verknüpfter Arbeitsbereich** aus.

2. Klicken Sie auf **Zu Arbeitsbereich wechseln**.

3. Wählen Sie unter **Arbeitsbereichsdatenquellen** die Option **Bereichskonfigurationen (Vorschau)** aus.

4. Wählen Sie die Auslassungspunkte rechts neben der Bereichskonfigurationen `MicrosoftDefaultScopeConfig-ChangeTracking` aus, und klicken Sie auf **Bearbeiten**. 

5. Wählen Sie im Bearbeitungsbereich die Option **Computergruppen auswählen** aus. Im Bereich „Computergruppen“ werden die gespeicherten Suchen angezeigt, die zum Erstellen der Bereichskonfiguration verwendet werden. Dies ist die gespeicherte Suche, die von Änderungsnachverfolgung und Inventur verwendet wird:

    |Name     |Category  |Alias  |
    |---------|---------|---------|
    |MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |

6. Wählen Sie die gespeicherte Suche aus, um die zum Auffüllen der Gruppe verwendete Abfrage anzuzeigen und zu bearbeiten. Die folgende Abbildung zeigt die Abfrage und ihre Ergebnisse:

    ![Gespeicherte Suchvorgänge](media/automation-scope-configurations-change-tracking/logsearch.png)

## <a name="next-steps"></a>Nächste Schritte

* Informationen zur Verwendung von „Änderungsnachverfolgung und Bestand“ finden Sie unter [Verwalten der Änderungsnachverfolgung und des Bestands](change-tracking-file-contents.md).
* Informationen zur Behandlung allgemeiner Probleme mit dem Feature finden Sie unter [Behandeln von Problemen mit Änderungsnachverfolgung und Bestand](troubleshoot/change-tracking.md).