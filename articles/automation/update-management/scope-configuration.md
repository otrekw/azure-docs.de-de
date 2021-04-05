---
title: Einschränken des Bereitstellungsumfangs der Azure Automation-Updateverwaltung
description: In diesem Artikel wird erläutert, wie die Bereichskonfigurationen verwendet werden, um den Umfang einer Bereitstellung der Updateverwaltung zu beschränken.
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 76063c479950d12985d5f3f52393f9bb0d5ecd8d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92221621"
---
# <a name="limit-update-management-deployment-scope"></a>Einschränken des Bereitstellungsumfangs der Updateverwaltung

In diesem Artikel erfahren Sie, wie Sie mit Bereichskonfigurationen arbeiten, wenn Sie das Feature [Updateverwaltung](overview.md) verwenden, um Updates und Patches für Ihre VMs bereitzustellen. Weitere Informationen finden Sie unter [Zielgruppenadressierung für Überwachungslösungen in Azure Monitor (Vorschau)](../../azure-monitor/insights/solution-targeting.md).

## <a name="about-scope-configurations"></a>Informationen zu Bereichskonfigurationen

Eine Bereichskonfiguration enthält mindestens eine gespeicherte Suche oder eine Gruppe von gespeicherten Suchen (Abfragen), um den Bereich der Updateverwaltung auf bestimmte Computer zu begrenzen. Die Bereichskonfiguration wird innerhalb des Log Analytics-Arbeitsbereichs verwendet, um die Computer anzugeben, für die ein Feature aktiviert werden soll. Wenn Sie einen Computer hinzufügen, um Updates von der Updateverwaltung zu empfangen, wird der betreffende Computer außerdem einer gespeicherten Suche im Arbeitsbereich hinzugefügt.

## <a name="set-the-scope-limit"></a>Festlegen der Bereichsgrenze

So schränken Sie den Geltungsbereich für Ihre Bereitstellung der Updateverwaltung ein:

1. Wählen Sie in Ihrem Automation-Konto unter **Verwandte Ressourcen** die Option **Verknüpfter Arbeitsbereich** aus.

2. Klicken Sie auf **Zu Arbeitsbereich wechseln**.

3. Wählen Sie unter **Arbeitsbereichsdatenquellen** die Option **Bereichskonfigurationen (Vorschau)** aus.

4. Wählen Sie die Auslassungspunkte rechts neben der `MicrosoftDefaultScopeConfig-Updates`-Bereichskonfigurationen aus, und wählen Sie **Bearbeiten** aus.

5. Klappen Sie im Bearbeitungsbereich die Option **Computergruppen auswählen** auf. Im Bereich „Computergruppen“ werden die gespeicherten Suchen angezeigt, die zum Erstellen der Bereichskonfiguration verwendet werden. Von der Updateverwaltung wird die folgende gespeicherte Suche verwendet:

    |Name     |Category  |Alias  |
    |---------|---------|---------|
    |MicrosoftDefaultComputerGroup     | Aktualisierungen        | Updates__MicrosoftDefaultComputerGroup         |

6. Wählen Sie die gespeicherte Suche aus, um die zum Auffüllen der Gruppe verwendete Abfrage anzuzeigen und zu bearbeiten. Die folgende Abbildung zeigt die Abfrage und ihre Ergebnisse:

    [ ![Gespeicherte Suchen](./media/scope-configuration/logsearch.png)](./media/scope-configuration/logsearch-expanded.png#lightbox)

## <a name="next-steps"></a>Nächste Schritte

Sie können [Azure Monitor-Protokolle abfragen](query-logs.md), um Updatebewertungen, Bereitstellungen und andere verwandte Verwaltungsaufgaben zu analysieren. Es umfasst vordefinierte Abfragen, die Ihnen beim Einstieg helfen sollen.
