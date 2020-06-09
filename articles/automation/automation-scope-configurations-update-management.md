---
title: Arbeiten mit Bereichskonfigurationen für die Updateverwaltung von Azure Automation
description: Dieser Artikel enthält Informationen zur Verwendung von Bereichskonfigurationen im Zusammenhang mit der Updateverwaltung.
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 23ec49f2d68cf376ef0beb118d8bf69ada7bc0de
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83832026"
---
# <a name="work-with-scope-configurations-for-update-management"></a>Arbeiten mit Bereichskonfigurationen für die Updateverwaltung

In diesem Artikel erfahren Sie, wie Sie mit Bereichskonfigurationen arbeiten, wenn Sie das Feature [Updateverwaltung](automation-update-management.md) für virtuelle Computer verwenden. 

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="check-the-scope-configuration"></a><a name="scope-configuration"></a>Überprüfen der Bereichskonfiguration

Von der Updateverwaltung wird eine Bereichskonfiguration innerhalb des Log Analytics-Arbeitsbereichs verwendet, um die Computer anzugeben, für die das Feature aktiviert werden soll. Die Bereichskonfiguration enthält mindestens eine gespeicherte Suche oder eine Gruppe von gespeicherten Suchen, um den Bereich des Features auf bestimmte Computer zu begrenzen. So greifen Sie auf die Bereichskonfigurationen zu

1. Wählen Sie in Ihrem Automation-Konto unter **Verwandte Ressourcen** die Option **Arbeitsbereich** aus. 

2. Wählen unter **Arbeitsbereichsdatenquellen** den Arbeitsbereich und anschließend **Bereichskonfigurationen** aus.

3. Sollte das Feature „Updateverwaltung“ für den ausgewählten Arbeitsbereich noch nicht aktiviert sein, wird die Bereichskonfiguration `MicrosoftDefaultScopeConfig-Updates` erstellt. 

4. Ist das Feature bereits für den ausgewählten Arbeitsbereich aktiviert, wird es nicht erneut bereitgestellt, und die Bereichskonfiguration wird nicht hinzugefügt. 

5. Wählen Sie die Auslassungspunkte einer der Bereichskonfigurationen aus, und klicken Sie auf **Bearbeiten**. 

6. Wählen Sie im Bearbeitungsbereich die Option **Computergruppen auswählen** aus. Im Bereich „Computergruppen“ werden die gespeicherten Suchen angezeigt, die zum Erstellen der Bereichskonfiguration verwendet werden.

## <a name="view-a-saved-search"></a>Anzeigen einer gespeicherten Suche

Wenn ein Computer der Updateverwaltung hinzugefügt wird, wird er auch einer gespeicherten Suche in Ihrem Arbeitsbereich hinzugefügt. Bei der gespeicherten Suche handelt es sich um eine Abfrage, die die Zielcomputer enthält.

1. Navigieren Sie zu Ihrem Log Analytics-Arbeitsbereich, und wählen Sie **Gespeicherte Suchvorgänge** unter **Allgemein** aus. Von der Updateverwaltung wird die folgende gespeicherte Suche verwendet:

|Name     |Category  |Alias  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     | Aktualisierungen        | Updates__MicrosoftDefaultComputerGroup         |

2. Wählen Sie die gespeicherte Suche aus, um die zum Auffüllen der Gruppe verwendete Abfrage anzuzeigen. Die folgende Abbildung zeigt die Abfrage und ihre Ergebnisse:

    ![Gespeicherte Suchvorgänge](media/automation-scope-configurations-update-management/logsearch.png)

## <a name="next-steps"></a>Nächste Schritte

* Informationen zur Verwendung des Features finden Sie unter [Verwalten von Updates und Patches für Ihre virtuellen Azure-Computer](automation-tutorial-update-management.md).
* Informationen zum Behandeln von Fehlern beim Feature finden Sie unter [Beheben von Problemen mit der Lösung für die Updateverwaltung](troubleshoot/update-management.md).
* Informationen zum Behandeln von Problemen mit dem Windows Update-Agent finden Sie unter [Beheben von Problemen mit dem Windows Update-Agent](troubleshoot/update-agent-issues.md).
* Informationen zum Behandeln von Problemen mit dem Linux Update-Agent finden Sie unter [Beheben von Problemen mit dem Linux Update-Agent](troubleshoot/update-agent-issues-linux.md).