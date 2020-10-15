---
title: Aufheben der Verknüpfung eines Arbeitsbereichs mit einem Automation-Konto für „Änderungsnachverfolgung und Bestand“
description: In diesem Artikel erfahren Sie, wie Sie die Verknüpfung eines Log Analytics-Arbeitsbereichs mit dem Automation-Konto für „Änderungsnachverfolgung und Bestand“ aufheben.
services: automation
ms.date: 4/11/2019
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 2be702ec6e820fe71dd8d2da7aa4cf831b52402e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "83828252"
---
# <a name="unlink-workspace-from-automation-account"></a>Aufheben der Verknüpfung eines Arbeitsbereichs über ein Automation-Konto

Sie können sich bei der Aktivierung von Vorgängen für [Änderungsnachverfolgung und Bestand](change-tracking.md) gegen die Integration Ihres Automation-Kontos in einen Log Analytics-Arbeitsbereich entscheiden. In diesem Artikel erfahren Sie, wie Sie die Verknüpfung des Arbeitsbereichs mit Ihrem Konto aufheben.

1. Melden Sie sich unter https://portal.azure.com bei Azure an.

2. Entfernen Sie die Updateverwaltung für Ihre virtuellen Computer. Weitere Informationen finden Sie unter [Entfernen von VMs aus Änderungsnachverfolgung und Bestand](automation-remove-vms-from-change-tracking.md).

3. Wenn „Änderungsnachverfolgung und Bestand“ frühere Versionen der Azure SQL-Überwachung beinhaltet, wurden bei der Einrichtung des Features möglicherweise Automation-Ressourcen erstellt, die Sie entfernen sollten. Suchen Sie nach diesen Ressourcen, und entfernen Sie sie.

4. Öffnen Sie Ihr Automation-Konto, und wählen Sie auf der linken Seite unter **Zugehörige Ressourcen** die Option **Verknüpfter Arbeitsbereich** aus.

5. Klicken Sie auf der Seite „Verknüpfung des Arbeitsbereichs aufheben“ auf **Verknüpfung des Arbeitsbereichs aufheben**, und gehen Sie entsprechend den Aufforderungen vor.

   ![Seite „Verknüpfung des Arbeitsbereichs aufheben“](media/automation-unlink-workspace-change-tracking/automation-unlink-workspace-blade.png)erforderlich.

6. Während Azure Automation versucht, die Verknüpfung mit Ihrem Log Analytics-Arbeitsbereich aufzuheben, können Sie den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen.

Alternativ können Sie die Verknüpfung Ihres Log Analytics-Arbeitsbereichs mit Ihrem Automation-Konto auch innerhalb des Arbeitsbereichs aufheben:

1. Wählen Sie in Ihrem Arbeitsbereich unter **Verwandte Ressourcen** die Option **Automation-Konto**. 
2. Klicken Sie auf der Seite „Automation-Konto“ auf **Verknüpfung zu diesem Konto aufheben**.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zur Verwendung von „Änderungsnachverfolgung und Bestand“ finden Sie unter [Verwalten der Änderungsnachverfolgung und des Bestands](change-tracking-file-contents.md).
* Informationen zur Behandlung allgemeiner Probleme mit dem Feature finden Sie unter [Behandeln von Problemen mit Änderungsnachverfolgung und Bestand](troubleshoot/change-tracking.md).
