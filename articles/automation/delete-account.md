---
title: Löschen des Azure Automation-Kontos
description: In diesem Artikel erfahren Sie, wie Sie Ihr Automation-Konto in den verschiedenen Konfigurationsszenarien löschen.
services: automation
ms.service: automation
ms.subservice: process-automation
ms.date: 03/18/2021
ms.topic: conceptual
ms.openlocfilehash: c3a514aa507fcf069671f987e175b7ae5be59d10
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105734919"
---
# <a name="how-to-delete-your-azure-automation-account"></a>So löschen Sie Ihre Azure Automation-Konto

Wenn Sie ein Azure Automation-Konto zum Automatisieren des IT-oder Geschäftsprozesses oder zum Aktivieren seiner anderen Features zur Unterstützung der Betriebsverwaltung ihrer Azure-und nicht-Azure-Computer (z. b. Updateverwaltung) aktivieren, können Sie das Automation-Konto nicht mehr verwenden. Wenn Sie Funktionen aktiviert haben, die von der Integration mit einem Azure Monitor Log Analytics-Arbeitsbereich abhängen, sind mehr Schritte erforderlich, um diese Aktion abzuschließen.

Das Entfernen dieses Features kann mit einer der folgenden Methoden basierend auf den unterstützten Bereitstellungsmodellen erfolgen:

* Löschen Sie die Ressourcengruppe mit dem Automation-Konto.
* Löschen Sie die Ressourcengruppe, die das Automation-Konto und den verknüpften Azure Monitor Log Analytics-Arbeitsbereich enthält, die jeweils für die Unterstützung dieses Features vorgesehen sind.

    * Das Konto und der Arbeitsbereich sind für die Unterstützung von Updateverwaltung, Änderungsnachverfolgung und Bestand und/oder VMS außerhalb der Geschäftszeiten starten/beenden vorgesehen.
    * Das Konto ist für die Prozessautomatisierung und die Integration in einen Arbeitsbereich zum Senden von Runbook-Auftragsstatus und Auftragsdaten strömen reserviert.

* Heben Sie die Verknüpfung des Log Analytics-Arbeitsbereichs mit dem Automation-Konto auf und löschen Sie das Automation-Konto.
* Löschen Sie die Funktion aus dem verknüpften Arbeitsbereich, entfernen Sie die Verknüpfung des Kontos mit dem Arbeitsbereich, und löschen Sie dann das Automation-Konto.

In diesem Artikel erfahren Sie, wie Sie Ihr Automation-Konto über die Azure Portal, PowerShell, die Azure CLI oder die Rest-API vollständig entfernen.

## <a name="delete-the-dedicated-resource-group"></a>Löschen der dedizierten Ressourcengruppe

Um Ihr Automation-Konto zu löschen, und auch den Log Analytics Arbeitsbereich, wenn Sie mit dem Konto verknüpft sind, das in derselben Ressourcengruppe für das Konto erstellt wurde, führen Sie die Schritte aus, die im Artikel [Azure Resource Manager Ressourcengruppe und Ressourcen-Löschung](../azure-resource-manager/management/delete-resource-group.md) beschrieben werden.

## <a name="delete-a-standalone-automation-account"></a>Erstellen eines eigenständigen Automation-Kontos

Wenn Ihr Automation-Konto nicht mit einem Log Analytics Arbeitsbereich verknüpft ist, führen Sie die folgenden Schritte aus, um es zu löschen.

# <a name="azure-portal"></a>[Azure portal](#tab/azure-portal)

1. Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) bei Azure an.

2. Navigieren Sie im Azure-Portal zu **Automation-Konten**.

3. Öffnen Sie Ihr Automation-Konto, und wählen Sie im Menü **Löschen** aus.

Während die Informationen überprüft werden und das Konto gelöscht wird, können Sie den Status im Menü unter **Benachrichtigungen** nachverfolgen.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Mit diesem Befehl wird das Automation-Konto entfernt, ohne zur Validierung aufzufordern.

```powershell
Remove-AzAutomationAccount -Name "automationAccountName" -Force -ResourceGroupName "resourceGroupName"
```

---

## <a name="delete-a-standalone-automation-account-linked-to-workspace"></a>Löschen eines eigenständigen Automation-Kontos

Wenn Ihr Automation-Konto mit einem Log Analytics Arbeitsbereich verknüpft ist, um Auftragsdaten-Streams und Auftrags-Protokolle zu erfassen, führen Sie die folgenden Schritte aus, um das Konto zu löschen.

Es gibt zwei Optionen für das Aufheben der Verknüpfung des Log Analytics-Arbeitsbereichs mit Ihrem Automation-Konto. Dieser Vorgang lässt sich im Automation-Konto oder verknüpften Arbeitsbereich durchführen.

Führen Sie die folgenden Schritte aus, um die Verknüpfung mit Ihrem Automation-Konto aufzuheben.

1. Navigieren Sie im Azure-Portal zu **Automation-Konten**.

2. Öffnen Sie Ihr Automation-Konto, und wählen Sie auf der linken Seite unter **Zugehörige Ressourcen** die Option **Verknüpfter Arbeitsbereich** aus.

3. Wählen Sie auf der Seite **Verknüpfung des Arbeitsbereichs aufheben** die Option **Verknüpfung des Arbeitsbereichs aufheben** aus, und gehen Sie entsprechend den Aufforderungen vor.

   ![Seite „Verknüpfung des Arbeitsbereichs aufheben“](media/automation-solution-vm-management-remove/automation-unlink-workspace-blade.png)

    Während versucht wird, die Verknüpfung mit dem Log Analytics-Arbeitsbereich aufzuheben, können Sie den Status im Menü unter **Benachrichtigungen** nachverfolgen.

Führen Sie die folgenden Schritte aus, um die Verknüpfung im Arbeitsbereich aufzuheben.

1. Navigieren Sie im Azure-Portal zu **Log Analytics-Arbeitsbereiche**.

2. Wählen Sie im Arbeitsbereich unter **Verwandte Ressourcen** die Option **Automation-Konto** aus.

3. Klicken Sie auf der Seite „Automation-Konto“ auf **Kontoverknüpfung aufheben**, und reagieren Sie auf die Aufforderungen.

Während versucht wird, die Verknüpfung mit dem Automation-Konto aufzuheben, können Sie den Status im Menü unter **Benachrichtigungen** nachverfolgen.

Nachdem die Verknüpfung des Automation-Kontos mit dem Arbeitsbereich erfolgreich aufgehoben wurde, führen Sie die Schritte im Abschnitt [eigenständiges Automation-Konto](#delete-a-standalone-automation-account) aus, um das Konto zu löschen.

## <a name="delete-a-shared-capability-automation-account"></a>Löschen eines Shared Capability Automation-Kontos

Führen Sie die folgenden Schritte aus, um Ihr Automation-Konto zu löschen, das mit einem Log Analytics-Arbeitsbereich verknüpft ist, um Updateverwaltung, Änderungsnachverfolgung und Bestand und/oder VMS außerhalb der Geschäftszeiten starten/beenden zu unterstützen.

### <a name="step-1-delete-the-solution-from-the-linked-workspace"></a>Schritt 1: Löschen Sie die Lösung aus dem verknüpften Arbeitsbereich.

# <a name="azure-portal"></a>[Azure portal](#tab/azure-portal)

1. Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) bei Azure an.

2. Navigieren Sie zu Ihrem Automation-Konto, und wählen Sie unter **Zugehörige Ressourcen** die Option **Verknüpfter Arbeitsbereich** aus.

3. Klicken Sie auf **Zu Arbeitsbereich wechseln**.

4. Klicken Sie unter **Allgemein** auf **Lösungen**.

5. Wählen Sie auf der Seite Lösungen basierend auf den Features, die im Konto bereitgestellt werden, eine der folgenden Aktionen aus:

    * Wählen Sie für VMS außerhalb der Geschäftszeiten starten/beenden **Start-Ende-VM [Arbeitsbereichname]** aus.
    * Wählen Sie für Updateverwaltung **Updates (Name des Arbeitsbereichs)** aus.
    * Wählen Sie für Änderungsnachverfolgung und Bestand die Option **ChangeTracking (Name des Arbeitsbereichs)** aus.

6. Wählen Sie auf der Seite **Lösung** auf der Symbolleiste **Löschen** aus. Wenn mehr als eine der oben aufgeführten Features für das Automation-Konto und den verknüpften Arbeitsbereich bereitgestellt wird, müssen Sie diese auswählen und löschen, bevor Sie den Vorgang fortsetzen.

7. Während die Informationen überprüft werden und die Funktion gelöscht wird, können Sie den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen. Nach dem Löschvorgang kehren Sie zur Seite „Lösungen“ zurück.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Verwenden Sie das Cmdlet [Remove-AzMonitorLogAnalyticsSolution](/powershell/module/az.monitoringsolutions/remove-azmonitorloganalyticssolution), um eine installierte Lösung mithilfe von Azure PowerShell zu entfernen.

```powershell
Remove-AzMonitorLogAnalyticsSolution -ResourceGroupName "resourceGroupName" -Name "solutionName"
```

---

### <a name="step-2-unlink-workspace-from-automation-account"></a>Schritt 2: Aufheben der Verknüpfung eines Arbeitsbereichs über ein Automation-Konto

Es gibt zwei Optionen für das Aufheben der Verknüpfung des Log Analytics-Arbeitsbereichs mit Ihrem Automation-Konto. Dieser Vorgang lässt sich im Automation-Konto oder verknüpften Arbeitsbereich durchführen.

Führen Sie die folgenden Schritte aus, um die Verknüpfung mit Ihrem Automation-Konto aufzuheben.

1. Navigieren Sie im Azure-Portal zu **Automation-Konten**.

2. Öffnen Sie Ihr Automation-Konto, und wählen Sie auf der linken Seite unter **Zugehörige Ressourcen** die Option **Verknüpfter Arbeitsbereich** aus.

3. Wählen Sie auf der Seite **Verknüpfung des Arbeitsbereichs aufheben** die Option **Verknüpfung des Arbeitsbereichs aufheben** aus, und gehen Sie entsprechend den Aufforderungen vor.

   ![Seite „Verknüpfung des Arbeitsbereichs aufheben“](media/automation-solution-vm-management-remove/automation-unlink-workspace-blade.png)

    Während versucht wird, die Verknüpfung mit dem Log Analytics-Arbeitsbereich aufzuheben, können Sie den Status im Menü unter **Benachrichtigungen** nachverfolgen.

Führen Sie die folgenden Schritte aus, um die Verknüpfung im Arbeitsbereich aufzuheben.

1. Navigieren Sie im Azure-Portal zu **Log Analytics-Arbeitsbereiche**.

2. Wählen Sie im Arbeitsbereich unter **Verwandte Ressourcen** die Option **Automation-Konto** aus.

3. Klicken Sie auf der Seite „Automation-Konto“ auf **Kontoverknüpfung aufheben**, und reagieren Sie auf die Aufforderungen.

Während versucht wird, die Verknüpfung mit dem Automation-Konto aufzuheben, können Sie den Status im Menü unter **Benachrichtigungen** nachverfolgen.

### <a name="step-3-delete-automation-account"></a>Schritt 3: Löschen des Automation-Kontos

Nachdem die Verknüpfung des Automation-Kontos mit dem Arbeitsbereich erfolgreich aufgehoben wurde, führen Sie die Schritte im Abschnitt [eigenständiges Automation-Konto](#delete-a-standalone-automation-account) aus, um das Konto zu löschen.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Erstellen eines Automation-Kontos über das Azure-Portal finden Sie unter [Erstellen eines eigenständigen Azure Automation-Kontos](automation-create-standalone-account.md). Wenn Sie Ihr Konto lieber mithilfe einer Vorlage erstellen möchten, finden Sie weitere Informationen unter [Erstellen eines Automation-Kontos mithilfe einer Azure Resource Manager-Vorlage](quickstart-create-automation-account-template.md).