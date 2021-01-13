---
title: Einschränken des Bereitstellungsumfangs für Änderungsnachverfolgung und Bestand von Azure Automation
description: Dieser Artikel enthält Informationen zur Verwendung von Bereichskonfigurationen, um den Umfang einer Bereitstellung von Änderungsnachverfolgung und Bestand einzuschränken.
services: automation
ms.subservice: change-inventory-management
ms.date: 10/14/2020
ms.topic: conceptual
ms.openlocfilehash: 6eb9a20920f0a340491459f0875fc85b90dfa193
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92209106"
---
# <a name="limit-change-tracking-and-inventory-deployment-scope"></a>Einschränken des Bereitstellungsumfangs für Änderungsnachverfolgung und Bestand

In diesem Artikel erfahren Sie, wie Sie mit Bereichskonfigurationen arbeiten, wenn Sie das Feature [Änderungsnachverfolgung und Bestand](overview.md) verwenden, um Änderungen für Ihre VMs bereitzustellen. Weitere Informationen finden Sie unter [Zielgruppenadressierung für Überwachungslösungen in Azure Monitor (Vorschau)](../../azure-monitor/insights/solution-targeting.md).

## <a name="about-scope-configurations"></a>Informationen zu Bereichskonfigurationen

Eine Bereichskonfiguration enthält mindestens eine gespeicherte Suche oder eine Gruppe von gespeicherten Suchen (Abfragen), um den Bereich von Änderungsnachverfolgung und Bestand auf bestimmte Computer zu begrenzen. Die Bereichskonfiguration wird innerhalb des Log Analytics-Arbeitsbereichs verwendet, um die Computer anzugeben, für die ein Feature aktiviert werden soll. Wenn Sie einen Computer hinzufügen, um Änderungen von Feature zu empfangen, wird der betreffende Computer außerdem einer gespeicherten Suche im Arbeitsbereich hinzugefügt.

## <a name="set-the-scope-limit"></a>Festlegen der Bereichsgrenze

So schränken Sie den Umfang Ihrer Bereitstellung von Änderungsnachverfolgung und Bestand ein:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Wählen Sie im Azure-Portal **Alle Dienste** aus. Geben Sie in der Liste mit den Ressourcen **Automation** ein. Sobald Sie mit der Eingabe beginnen, werden Vorschläge in der Liste auf Grundlage Ihrer Eingabe gefiltert. Wählen Sie die Option **Automation-Konten** .

3. Wählen Sie in Ihrer Liste der Automation-Konten das Konto aus, das Sie beim Aktivieren von „Änderungsnachverfolgung und Bestand“ ausgewählt haben.

4. Wählen Sie in Ihrem Automation-Konto unter **Verwandte Ressourcen** die Option **Verknüpfter Arbeitsbereich** aus.

5. Klicken Sie auf **Zu Arbeitsbereich wechseln** .

6. Wählen Sie unter **Arbeitsbereichsdatenquellen** die Option **Bereichskonfigurationen (Vorschau)** aus.

7. Wählen Sie die Auslassungspunkte rechts neben der Bereichskonfigurationen `MicrosoftDefaultScopeConfig-ChangeTracking` aus, und klicken Sie auf **Bearbeiten** .

8. Wählen Sie im Bearbeitungsbereich die Option **Computergruppen auswählen** aus. Im Bereich „Computergruppen“ werden die gespeicherten Suchen angezeigt, die zum Erstellen der Bereichskonfiguration verwendet werden. Dies ist die gespeicherte Suche, die von Änderungsnachverfolgung und Inventur verwendet wird:

    |Name     |Category  |Alias  |
    |---------|---------|---------|
    |MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |

9. Wählen Sie die gespeicherte Suche aus, um die zum Auffüllen der Gruppe verwendete Abfrage anzuzeigen und zu bearbeiten. Die folgende Abbildung zeigt die Abfrage und ihre Ergebnisse:

    ![Gespeicherte Suchvorgänge](media/manage-scope-configurations/logsearch.png)

## <a name="next-steps"></a>Nächste Schritte

* Informationen zur Verwendung von „Änderungsnachverfolgung und Bestand“ finden Sie unter [Verwalten der Änderungsnachverfolgung und des Bestands](manage-change-tracking.md).
* Informationen zur Behandlung allgemeiner Probleme mit dem Feature finden Sie unter [Behandeln von Problemen mit Änderungsnachverfolgung und Bestand](../troubleshoot/change-tracking.md).
