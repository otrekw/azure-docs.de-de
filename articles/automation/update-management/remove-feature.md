---
title: Entfernen des Features für die Updateverwaltung in Azure Automation
description: In diesem Artikel erfahren Sie, wie Sie die Verwendung der Updateverwaltung beenden und die Verknüpfung eines Automation-Kontos mit dem Log Analytics-Arbeitsbereich aufheben.
services: automation
ms.date: 07/28/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 1d83f859fce33b9499d01c4b58e69f56fdbbb293
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2020
ms.locfileid: "92221629"
---
# <a name="remove-update-management-from-automation-account"></a>Entfernen der Updateverwaltung aus dem Automation-Konto

Nachdem Sie die Verwaltung von Updates auf Ihren virtuellen Computern mithilfe der Azure Automation-Updateverwaltung aktiviert haben, entscheiden Sie sich unter Umständen dazu, sie nicht mehr zu verwenden und die Konfiguration aus dem Konto und dem verknüpften Log Analytics-Arbeitsbereich zu entfernen.  In diesem Artikel erfahren Sie, wie Sie die Updateverwaltung vollständig von den verwalteten virtuellen Computern, aus Ihrem Automation-Konto und dem Log Analytics-Arbeitsbereich entfernen.

## <a name="sign-into-the-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="remove-management-of-vms"></a>Entfernen der Verwaltung virtueller Computer

Bevor Sie die Updateverwaltung entfernen, müssen Sie zunächst die Verwaltung Ihrer virtuellen Computer beenden. Weitere Informationen zum Aufheben der Registrierung für das Feature finden Sie unter [Entfernen virtueller Computer aus der Updateverwaltung](remove-vms.md).

## <a name="remove-updatemanagement-solution"></a>Entfernen der Lösung für die Updateverwaltung

Damit Sie die Verknüpfung des Automation-Kontos mit dem Arbeitsbereich aufheben können, müssen Sie die folgenden Schritte ausführen, um die Updateverwaltung vollständig zu entfernen. Sie entfernen die Lösung **Updates** aus dem Arbeitsbereich.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Wählen Sie im Azure-Portal **Alle Dienste** aus. Geben Sie in der Liste mit den Ressourcen **Log Analytics** ein. Sobald Sie mit der Eingabe beginnen, werden Vorschläge in der Liste auf Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Log Analytics**.

3. Wählen Sie in der Liste der Log Analytics-Arbeitsbereiche den Arbeitsbereich aus, den Sie beim Aktivieren der Updateverwaltung ausgewählt hatten.

4. Wählen Sie links die Option **Lösungen** aus.  

5. Wählen Sie in der Liste der Lösungen den Eintrag **Updates(Arbeitsbereichsname)** aus. Wählen Sie auf der Seite **Übersicht** für die Lösung die Option **Löschen** aus. Wenn Sie aufgefordert werden, die Auswahl zu bestätigen, klicken Sie auf **Ja**.

## <a name="unlink-workspace-from-automation-account"></a>Aufheben der Verknüpfung eines Arbeitsbereichs über ein Automation-Konto

1. Wählen Sie im Azure-Portal **Automation-Konten** aus.

2. Öffnen Sie Ihr Automation-Konto, und wählen Sie auf der linken Seite unter **Zugehörige Ressourcen** die Option **Verknüpfter Arbeitsbereich** aus.

3. Wählen Sie auf der Seite **Verknüpfung des Arbeitsbereichs aufheben** die Option **Verknüpfung des Arbeitsbereichs aufheben** aus, und gehen Sie entsprechend den Aufforderungen vor.

   ![Seite „Verknüpfung des Arbeitsbereichs aufheben“](media/remove-feature/automation-unlink-workspace-blade.png)

Während versucht wird, die Verknüpfung mit dem Log Analytics-Arbeitsbereich aufzuheben, können Sie den Status im Menü unter **Benachrichtigungen** nachverfolgen.

Alternativ können Sie die Verknüpfung Ihres Log Analytics-Arbeitsbereichs mit Ihrem Automation-Konto auch innerhalb des Arbeitsbereichs aufheben:

1. Wählen Sie im Azure-Portal **Log Analytics** aus.

2. Wählen Sie im Arbeitsbereich unter **Verwandte Ressourcen** die Option **Automation-Konto** aus.

3. Klicken Sie auf der Seite „Automation-Konto“ auf **Verknüpfung zu diesem Konto aufheben**.

Während versucht wird, die Verknüpfung mit dem Automation-Konto aufzuheben, können Sie den Status im Menü unter **Benachrichtigungen** nachverfolgen.

## <a name="cleanup-automation-account"></a>Bereinigen des Automation-Kontos

Wenn die Updateverwaltung zur Unterstützung früherer Versionen der Azure SQL-Überwachung konfiguriert wurde, wurden bei der Einrichtung des Features möglicherweise Automation-Ressourcen erstellt, die Sie entfernen sollten. Entfernen Sie zur Nutzung der Updateverwaltung ggf. die folgenden Elemente, die nicht mehr benötigt werden:

   * Zeitpläne für Updates: Jeder weist einen Namen auf, der einer von Ihnen erstellten Updatebereitstellung entspricht.
   * Für die Updateverwaltung erstellte Hybrid Worker-Gruppen: Diese haben Namen wie *machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8)* .

## <a name="next-steps"></a>Nächste Schritte

Informationen zum erneuten Aktivieren dieses Features finden Sie unter [Aktivieren der Updateverwaltung über ein Automation-Konto](enable-from-automation-account.md), [Aktivieren der Updateverwaltung über das Azure-Portal](enable-from-portal.md), [Aktivieren der Updateverwaltung über ein Runbook](enable-from-runbook.md) oder [Aktivieren der Updateverwaltung über einen virtuellen Azure-Computer](enable-from-vm.md).
