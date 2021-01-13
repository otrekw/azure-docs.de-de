---
title: Entfernen des Features „Änderungsnachverfolgung und Bestand“ von Azure Automation
description: In diesem Artikel wird beschrieben, wie Sie die Verwendung von „Änderungsnachverfolgung und Bestand“ beenden und die Verknüpfung eines Automation-Kontos mit dem Log Analytics-Arbeitsbereich aufheben.
services: automation
ms.subservice: change-inventory-management
ms.date: 10/14/2020
ms.topic: conceptual
ms.openlocfilehash: 2e3e5abdfbb2bf2e9d7a12a677422adc67336775
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92209241"
---
# <a name="remove-change-tracking-and-inventory-from-automation-account"></a>Entfernen von „Änderungsnachverfolgung und Bestand“ aus dem Automation-Konto

Nachdem Sie für die Verwaltung Ihrer virtuellen Computer das Feature „Änderungsnachverfolgung und Bestand“ von Azure Automation aktiviert haben, treffen Sie unter Umständen irgendwann die Entscheidung, dass Sie es nicht mehr verwenden und die Konfiguration aus dem Konto und dem verknüpften Log Analytics-Arbeitsbereich entfernen möchten. In diesem Artikel wird beschrieben, wie Sie „Änderungsnachverfolgung und Bestand“ vollständig von den verwalteten virtuellen Computern, aus Ihrem Automation-Konto und dem Log Analytics-Arbeitsbereich entfernen.

## <a name="sign-into-the-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="remove-management-of-vms"></a>Entfernen der Verwaltung virtueller Computer

Bevor Sie „Änderungsnachverfolgung und Bestand“ entfernen, müssen Sie zunächst die Verwaltung Ihrer VMs beenden. Weitere Informationen zur Aufhebung der Registrierung finden Sie unter [Entfernen virtueller Computer aus Änderungsnachverfolgung und Bestand](remove-vms-from-change-tracking.md).

## <a name="remove-changetracking-solution"></a>Entfernen der ChangeTracking-Lösung

Damit Sie die Verknüpfung des Automation-Kontos mit dem Arbeitsbereich aufheben können, müssen Sie die folgenden Schritte ausführen, um „Änderungsnachverfolgung und Bestand“ vollständig zu entfernen. Sie entfernen die **ChangeTracking** -Lösung aus dem Arbeitsbereich.

1. Wählen Sie im Azure-Portal **Alle Dienste** aus. Geben Sie in der Liste mit den Ressourcen **Log Analytics** ein. Sobald Sie mit der Eingabe beginnen, werden Vorschläge in der Liste auf Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Log Analytics** .

2. Wählen Sie in der Liste mit den Log Analytics-Arbeitsbereichen den Arbeitsbereich aus, den Sie beim Aktivieren von „Änderungsnachverfolgung und Bestand“ ausgewählt haben.

3. Wählen Sie links die Option **Lösungen** aus.  

4. Wählen Sie in der Liste mit den Lösungen den Eintrag **ChangeTracking(Name des Arbeitsbereichs)** aus. Wählen Sie auf der Seite **Übersicht** für die Lösung die Option **Löschen** aus. Wenn Sie aufgefordert werden, die Auswahl zu bestätigen, klicken Sie auf **Ja** .

## <a name="unlink-workspace-from-automation-account"></a>Aufheben der Verknüpfung eines Arbeitsbereichs über ein Automation-Konto

1. Wählen Sie im Azure-Portal **Automation-Konten** aus.

2. Öffnen Sie Ihr Automation-Konto, und wählen Sie auf der linken Seite unter **Zugehörige Ressourcen** die Option **Verknüpfter Arbeitsbereich** aus.

3. Wählen Sie auf der Seite **Verknüpfung des Arbeitsbereichs aufheben** die Option **Verknüpfung des Arbeitsbereichs aufheben** aus, und gehen Sie entsprechend den Aufforderungen vor.

   ![Seite „Verknüpfung des Arbeitsbereichs aufheben“](media/remove-feature/automation-unlink-workspace-blade.png)

Während versucht wird, die Verknüpfung mit dem Log Analytics-Arbeitsbereich aufzuheben, können Sie den Status im Menü unter **Benachrichtigungen** nachverfolgen.

Alternativ können Sie die Verknüpfung Ihres Log Analytics-Arbeitsbereichs mit Ihrem Automation-Konto auch innerhalb des Arbeitsbereichs aufheben:

1. Wählen Sie im Azure-Portal **Log Analytics** aus.

2. Wählen Sie im Arbeitsbereich unter **Verwandte Ressourcen** die Option **Automation-Konto** aus.

3. Klicken Sie auf der Seite „Automation-Konto“ auf **Verknüpfung zu diesem Konto aufheben** .

Während versucht wird, die Verknüpfung mit dem Automation-Konto aufzuheben, können Sie den Status im Menü unter **Benachrichtigungen** nachverfolgen.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum erneuten Aktivieren dieses Features finden Sie unter [Aktivieren des Features für Änderungsnachverfolgung und Bestand über ein Automation-Konto](enable-from-automation-account.md), [Aktivieren des Features für Änderungsnachverfolgung und Bestand durch das Durchsuchen des Azure-Portals](enable-from-portal.md), [Aktivieren des Features für Änderungsnachverfolgung und Bestand über ein Runbook](enable-from-runbook.md) oder [Aktivieren des Features für Änderungsnachverfolgung und Bestand über eine Azure-VM](enable-from-vm.md).
