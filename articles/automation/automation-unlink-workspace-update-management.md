---
title: Aufheben der Verknüpfung eines Arbeitsbereichs mit einem Automation-Konto für die Updateverwaltung
description: In diesem Artikel erfahren Sie, wie Sie die Verknüpfung eines Log Analytics-Arbeitsbereichs mit dem Automation-Konto für die Updateverwaltung aufheben.
services: automation
ms.date: 4/11/2019
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 9129d10071a4c8da0376cbad3d64c10cbaceb8b9
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83835783"
---
# <a name="unlink-workspace-from-automation-account-for-update-management"></a>Aufheben der Verknüpfung eines Arbeitsbereichs mit einem Automation-Konto für die Updateverwaltung

Sie müssen Ihr Automation-Konto während Vorgängen der [Updateverwaltung](automation-update-management.md) nicht in einen Log Analytics-Arbeitsbereich integrieren. In diesem Artikel erfahren Sie, wie Sie die Verknüpfung des Arbeitsbereichs mit Ihrem Konto aufheben.

1. Melden Sie sich unter https://portal.azure.com bei Azure an.

2. Entfernen Sie die Updateverwaltung für Ihre virtuellen Computer. Informationen hierzu finden Sie unter [Entfernen virtueller Computer aus der Updateverwaltung](automation-remove-vms-from-update-management.md).

3. Wenn die Updateverwaltung frühere Versionen der Azure SQL-Überwachung beinhaltet, wurden bei der Einrichtung des Features möglicherweise Automation-Ressourcen erstellt, die Sie entfernen sollten. Entfernen Sie zur Nutzung der Updateverwaltung ggf. die folgenden Elemente, die nicht mehr benötigt werden:

   * Zeitpläne für Updates: Jeder weist einen Namen auf, der einer von Ihnen erstellten Updatebereitstellung entspricht.
   * Für die Updateverwaltung erstellte Hybrid Worker-Gruppen: Diese haben Namen wie „machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8“.

4. Öffnen Sie Ihr Automation-Konto, und wählen Sie auf der linken Seite unter **Zugehörige Ressourcen** die Option **Verknüpfter Arbeitsbereich** aus.

5. Klicken Sie auf der Seite „Verknüpfung des Arbeitsbereichs aufheben“ auf **Verknüpfung des Arbeitsbereichs aufheben**, und gehen Sie entsprechend den Aufforderungen vor.

   ![Seite „Verknüpfung des Arbeitsbereichs aufheben“](media/automation-unlink-workspace-update-management/automation-unlink-workspace-blade.png)erforderlich.

6. Während Azure Automation versucht, die Verknüpfung mit Ihrem Log Analytics-Arbeitsbereich aufzuheben, können Sie den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen.

Alternativ können Sie Ihren Log Analytics-Arbeitsbereich auch von Ihrem Automation-Konto in dem Arbeitsbereich trennen:

1. Wählen Sie in Ihrem Arbeitsbereich unter **Verwandte Ressourcen** die Option **Automation-Konto**. 
2. Klicken Sie auf der Seite „Automation-Konto“ auf **Verknüpfung zu diesem Konto aufheben**.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zur Verwendung des Features finden Sie unter [Verwalten von Updates und Patches für Ihre virtuellen Azure-Computer](automation-tutorial-update-management.md).
* Informationen zum Behandeln von Fehlern beim Feature finden Sie unter [Beheben von Problemen mit der Lösung für die Updateverwaltung](troubleshoot/update-management.md).
* Informationen zum Behandeln von Problemen mit dem Windows Update-Agent finden Sie unter [Beheben von Problemen mit dem Windows Update-Agent](troubleshoot/update-agent-issues.md).
* Informationen zum Behandeln von Problemen mit dem Linux Update-Agent finden Sie unter [Beheben von Problemen mit dem Linux Update-Agent](troubleshoot/update-agent-issues-linux.md).
