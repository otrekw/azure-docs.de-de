---
title: Arbeiten mit Bereichskonfigurationen für „Änderungsnachverfolgung und Bestand“ von Azure Automation
description: Dieser Artikel enthält Informationen zur Verwendung von Bereichskonfigurationen im Zusammenhang mit „Änderungsnachverfolgung und Bestand“.
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 4fac94cc2f8f378b7e9d8e9485baed6a0ffa838b
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83832162"
---
# <a name="work-with-scope-configurations-for-change-tracking-and-inventory"></a>Arbeiten mit Bereichskonfigurationen für „Änderungsnachverfolgung und Bestand“

In diesem Artikel erfahren Sie, wie Sie mit Bereichskonfigurationen arbeiten, wenn Sie das Feature [Updateverwaltung](automation-update-management.md) für virtuelle Computer aktivieren. 

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="check-the-scope-configuration"></a><a name="scope-configuration"></a>Überprüfen der Bereichskonfiguration

Von der Updateverwaltung wird eine Bereichskonfiguration innerhalb des Log Analytics-Arbeitsbereichs verwendet, um die Computer anzugeben, für die Updateverwaltung aktiviert werden soll. Die Bereichskonfiguration enthält mindestens eine gespeicherte Suche oder eine Gruppe von gespeicherten Suchen, um den Bereich des Features auf bestimmte Computer zu begrenzen. So greifen Sie auf die Bereichskonfigurationen zu

1. Wählen Sie in Ihrem Automation-Konto unter **Verwandte Ressourcen** die Option **Arbeitsbereich** aus. 

2. Wählen unter **Arbeitsbereichsdatenquellen** den Arbeitsbereich und anschließend **Bereichskonfigurationen** aus.

3. Sollte das Feature „Updateverwaltung“ für den ausgewählten Arbeitsbereich noch nicht aktiviert sein, wird die Bereichskonfiguration `MicrosoftDefaultScopeConfig-ChangeTracking` erstellt. 

4. Ist das Feature bereits für den ausgewählten Arbeitsbereich aktiviert, wird es nicht erneut bereitgestellt, und die Bereichskonfiguration wird nicht hinzugefügt. 

5. Wählen Sie die Auslassungspunkte einer der Bereichskonfigurationen aus, und klicken Sie auf **Bearbeiten**. 

6. Wählen Sie im Bearbeitungsbereich die Option **Computergruppen auswählen** aus. Im Bereich „Computergruppen“ werden die gespeicherten Suchen angezeigt, die zum Erstellen der Bereichskonfiguration verwendet werden.

## <a name="view-a-saved-search"></a>Anzeigen einer gespeicherten Suche

Wenn ein Computer zu „Änderungsnachverfolgung und Bestand“ hinzugefügt wird, wird er auch einer gespeicherten Suche in Ihrem Arbeitsbereich hinzugefügt. Bei der gespeicherten Suche handelt es sich um eine Abfrage, die die Zielcomputer enthält.

1. Navigieren Sie zu Ihrem Log Analytics-Arbeitsbereich, und wählen Sie **Gespeicherte Suchvorgänge** unter **Allgemein** aus. Von der Updateverwaltung wird die folgende gespeicherte Suche verwendet:

    |Name     |Category  |Alias  |
    |---------|---------|---------|
    |MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |

2. Wählen Sie die gespeicherte Suche aus, um die zum Auffüllen der Gruppe verwendete Abfrage anzuzeigen. Die folgende Abbildung zeigt die Abfrage und ihre Ergebnisse:

    ![Gespeicherte Suchvorgänge](media/automation-scope-configurations-change-tracking/logsearch.png)

## <a name="next-steps"></a>Nächste Schritte

* Informationen zur Verwendung von „Änderungsnachverfolgung und Bestand“ finden Sie unter [Verwalten der Änderungsnachverfolgung und des Bestands](change-tracking-file-contents.md).
* Informationen zur Behandlung allgemeiner Probleme mit dem Feature finden Sie unter [Behandeln von Problemen mit Änderungsnachverfolgung und Bestand](troubleshoot/change-tracking.md).