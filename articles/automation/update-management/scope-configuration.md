---
title: Einschränken des Bereitstellungsumfangs der Azure Automation-Updateverwaltung
description: In diesem Artikel wird erläutert, wie die Bereichskonfigurationen verwendet werden, um den Umfang einer Bereitstellung der Updateverwaltung zu beschränken.
services: automation
ms.date: 06/03/2021
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 230be9fcdb30e51802a2e288cde86fd8047f70f0
ms.sourcegitcommit: 67cdbe905eb67e969d7d0e211d87bc174b9b8dc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111854952"
---
# <a name="limit-update-management-deployment-scope"></a>Einschränken des Bereitstellungsumfangs der Updateverwaltung

In diesem Artikel erfahren Sie, wie Sie mit Bereichskonfigurationen arbeiten, wenn Sie das Feature [Updateverwaltung](overview.md) verwenden, um Updates und Patches für Ihre Computer bereitzustellen. Weitere Informationen finden Sie unter [Zielgruppenadressierung für Überwachungslösungen in Azure Monitor (Vorschau)](../../azure-monitor/insights/solution-targeting.md).

## <a name="about-scope-configurations"></a>Informationen zu Bereichskonfigurationen

Eine Bereichskonfiguration enthält mindestens eine gespeicherte Suche oder eine Gruppe von gespeicherten Suchen (Abfragen), um den Bereich der Updateverwaltung auf bestimmte Computer zu begrenzen. Die Bereichskonfiguration wird innerhalb des Log Analytics-Arbeitsbereichs verwendet, um die Computer anzugeben, für die ein Feature aktiviert werden soll. Wenn Sie einen Computer hinzufügen, um Updates von der Updateverwaltung zu empfangen, wird der betreffende Computer außerdem einer gespeicherten Suche im Arbeitsbereich hinzugefügt. 

Standardmäßig erstellt die Updateverwaltung eine Computergruppe namens **Updates__MicrosoftDefaultComputerGroup**, je nachdem, wie Sie Computer mit Updateverwaltung aktiviert haben:

* Im Automation-Konto hatten Sie **+ Azure-VMs hinzufügen** ausgewählt.
* Im Automation-Konto hatten Sie **Computer verwalten** und dann die Option **Auf allen verfügbaren Computern aktivieren** ausgewählt, oder Sie hatten **Auf ausgewählten Computern aktivieren** ausgewählt.

Wenn eine der oben genannten Methoden ausgewählt ist, wird diese Computergruppe der Bereichskonfiguration **MicrosoftDefaultScopeConfig-Updates** hinzugefügt. Entsprechend Ihrer Verwaltungserfordernisse können Sie diesem Bereich auch eine oder mehrere benutzerdefinierte Computergruppen hinzufügen, um zu steuern, wie einzelne Computer für die Verwaltung mit der Updateverwaltung aktiviert werden.

Informationen zum Entfernen eines oder mehrerer Computer aus der **Updates__MicrosoftDefaultComputerGroup**, um deren Verwaltung mit der Updateverwaltung zu beenden, finden Sie unter [Entfernen von VMs aus der Updateverwaltung](remove-vms.md).

## <a name="set-the-scope-limit"></a>Festlegen der Bereichsgrenze

So schränken Sie den Geltungsbereich für Ihre Bereitstellung der Updateverwaltung ein:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Navigieren Sie im Azure-Portal zu **Log Analytics-Arbeitsbereiche**. Wählen Sie Ihren Arbeitsbereich in der Liste aus.

3. Wählen Sie in Ihrem Log Analytics-Arbeitsbereich im linken Menü **Bereichskonfigurationen (Vorschau)** aus.

4. Wählen Sie zuerst das Auslassungszeichen rechts von der Bereichskonfiguration **MicrosoftDefaultScopeConfig-Updates** und dann **Bearbeiten** aus.

5. Klappen Sie im Bearbeitungsbereich die Option **Computergruppen auswählen** auf. Im Bereich **Computergruppen** werden die gespeicherten Suchen angezeigt, die zur Bereichskonfiguration hinzugefügt werden. Von der Updateverwaltung wird die folgende gespeicherte Suche verwendet:

    |Name     |Category  |Alias  |
    |---------|---------|---------|
    |MicrosoftDefaultComputerGroup     | Aktualisierungen        | Updates__MicrosoftDefaultComputerGroup         |

6. Wenn Sie eine benutzerdefinierte Gruppe hinzugefügt haben, wird sie in der Liste angezeigt. Deaktivieren Sie das Kontrollkästchen links neben dem Element, um die Auswahl aufzuheben. Um dem Bereich eine benutzerdefinierte Gruppe hinzuzufügen, wählen Sie ihn aus und klicken Sie auf **Auswählen**, wenn Sie die Änderungen abgeschlossen haben.

7. Klicken Sie auf der Seite **Bereichskonfiguration bearbeiten** auf **OK**, um die Änderungen zu speichern.

## <a name="next-steps"></a>Nächste Schritte

Sie können [Azure Monitor-Protokolle abfragen](query-logs.md), um Updatebewertungen, Bereitstellungen und andere verwandte Verwaltungsaufgaben zu analysieren. Es umfasst vordefinierte Abfragen, die Ihnen beim Einstieg helfen sollen.
