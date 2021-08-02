---
title: Einschränken des Bereitstellungsumfangs für Änderungsnachverfolgung und Bestand von Azure Automation
description: Dieser Artikel enthält Informationen zur Verwendung von Bereichskonfigurationen, um den Umfang einer Bereitstellung von Änderungsnachverfolgung und Bestand einzuschränken.
services: automation
ms.subservice: change-inventory-management
ms.date: 05/27/2021
ms.topic: conceptual
ms.openlocfilehash: 6812a0b0688efdb75d847a36d661ba87017a8b9d
ms.sourcegitcommit: 1b698fb8ceb46e75c2ef9ef8fece697852c0356c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2021
ms.locfileid: "110653193"
---
# <a name="limit-change-tracking-and-inventory-deployment-scope"></a>Einschränken des Bereitstellungsumfangs für Änderungsnachverfolgung und Bestand

In diesem Artikel erfahren Sie, wie Sie mit Bereichskonfigurationen arbeiten, wenn Sie das Feature [Änderungsnachverfolgung und Bestand](overview.md) verwenden, um Änderungen für Ihre VMs bereitzustellen. Weitere Informationen finden Sie unter [Zielgruppenadressierung für Überwachungslösungen in Azure Monitor (Vorschau)](../../azure-monitor/insights/solution-targeting.md).

## <a name="about-scope-configurations"></a>Informationen zu Bereichskonfigurationen

Eine Bereichskonfiguration enthält mindestens eine gespeicherte Suche oder eine Gruppe von gespeicherten Suchen (Abfragen), um den Bereich von Änderungsnachverfolgung und Bestand auf bestimmte Computer zu begrenzen. Die Bereichskonfiguration wird innerhalb des Log Analytics-Arbeitsbereichs verwendet, um die Computer anzugeben, für die ein Feature aktiviert werden soll. Wenn Sie einen Computer hinzufügen, um Änderungen von Feature zu empfangen, wird der betreffende Computer außerdem einer gespeicherten Suche im Arbeitsbereich hinzugefügt.

Standardmäßig erstellt Änderungsnachverfolgung und Bestand eine Computergruppe namens **ChangeTracking__MicrosoftDefaultComputerGroup**, je nachdem, wie die VMs aktiviert wurden:

* Im Automation-Konto wurde **+ Azure-VMs hinzufügen** ausgewählt.
* Im Automation-Konto wurde **Computer verwalten** und dann die Option **Auf allen verfügbaren Computern aktivieren** oder **Auf ausgewählten Computern aktivieren** ausgewählt.

Wenn eine der oben genannten Methoden verwendet wurde, wird diese Computergruppe der Bereichskonfiguration **MicrosoftDefaultScopeConfig-ChangeTracking** hinzugefügt. Entsprechend Ihrer Verwaltungserfordernisse können Sie diesem Bereich auch eine oder mehrere benutzerdefinierte Computergruppen hinzufügen, um zu steuern, wie einzelne Computer für die Verwaltung mit Änderungsnachverfolgung und Bestand aktiviert werden.

Informationen zum Entfernen einer oder mehrerer VMs aus der **ChangeTracking__MicrosoftDefaultComputerGroup**, um sie nicht mehr mit Änderungsnachverfolgung und Bestand zu verwalten, finden Sie unter [Entfernen von VMs aus Änderungsnachverfolgung und Bestand](remove-vms-from-change-tracking.md).

## <a name="set-the-scope-limit"></a>Festlegen der Bereichsgrenze

So schränken Sie den Umfang Ihrer Bereitstellung von Änderungsnachverfolgung und Bestand ein:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Navigieren Sie im Azure-Portal zu **Log Analytics-Arbeitsbereiche**. Wählen Sie Ihren Arbeitsbereich in der Liste aus.

3. Wählen Sie in Ihrem Log Analytics-Arbeitsbereich im linken Menü **Bereichskonfigurationen (Vorschau)** aus.

4. Wählen Sie zuerst das Auslassungszeichen rechts von der Bereichskonfiguration **MicrosoftDefaultScopeConfig-ChangeTracking** und dann **Bearbeiten** aus.

5. Klappen Sie im Bearbeitungsbereich die Option **Computergruppen auswählen** auf. Im Bereich **Computergruppen** werden die gespeicherten Suchen angezeigt, die zur Bereichskonfiguration hinzugefügt werden. Von der Updateverwaltung wird die folgende gespeicherte Suche verwendet:

    |Name     |Category  |Alias  |
    |---------|---------|---------|
    |MicrosoftDefaultComputerGroup     | ChangeTracking        | ChangeTracking__MicrosoftDefaultComputerGroup         |

6. Wenn Sie eine benutzerdefinierte Gruppe hinzugefügt haben, wird sie in der Liste angezeigt. Deaktivieren Sie das Kontrollkästchen links neben dem Element, um die Auswahl aufzuheben. Um dem Bereich eine benutzerdefinierte Gruppe hinzuzufügen, wählen Sie ihn aus und klicken Sie auf **Auswählen**, wenn Sie die Änderungen abgeschlossen haben.

7. Klicken Sie auf der Seite **Bereichskonfiguration bearbeiten** auf **OK**, um die Änderungen zu speichern.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zur Verwendung von „Änderungsnachverfolgung und Bestand“ finden Sie unter [Verwalten der Änderungsnachverfolgung und des Bestands](manage-change-tracking.md).
* Informationen zur Behandlung allgemeiner Probleme mit dem Feature finden Sie unter [Behandeln von Problemen mit Änderungsnachverfolgung und Bestand](../troubleshoot/change-tracking.md).
