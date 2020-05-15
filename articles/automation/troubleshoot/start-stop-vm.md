---
title: Problembehandlung beim Starten/Beenden von VMs außerhalb der Geschäftszeiten
description: In diesem Artikel finden Sie Informationen zur Behandlung von Problemen mit der Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten.
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 04/04/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 611e8441fab56114ca010d0b555c9ed156ae9d40
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82855059"
---
# <a name="troubleshoot-the-startstop-vms-during-off-hours-solution"></a>Problembehandlung beim Starten/Beenden von VMs außerhalb der Geschäftszeiten

Dieser Artikel enthält Informationen zur Behandlung von Problemen, die auftreten können, wenn Sie mit der Azure Automation-Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten arbeiten.

>[!NOTE]
>Dieser Artikel wurde aktualisiert und beinhaltet jetzt das neue Az-Modul von Azure PowerShell. Sie können das AzureRM-Modul weiterhin verwenden, das bis mindestens Dezember 2020 weiterhin Fehlerbehebungen erhält. Weitere Informationen zum neuen Az-Modul und zur Kompatibilität mit AzureRM finden Sie unter [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Einführung in das neue Az-Modul von Azure PowerShell). Installationsanweisungen für das Az-Modul auf Ihrem Hybrid Runbook Worker finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). In Ihrem Azure Automation-Konto können Sie die Module mithilfe der Informationen unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](../automation-update-azure-modules.md) auf die neueste Version aktualisieren.

## <a name="scenario-the-startstop-vms-during-off-hours-solution-fails-to-properly-deploy"></a><a name="deployment-failure"></a>Szenario: Fehler bei der ordnungsgemäßen Bereitstellung der Lösung zum Starten/Beenden von VMs während der Geschäftszeiten

### <a name="issue"></a>Problem

Bei der Bereitstellung der [Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten](../automation-solution-vm-management.md) erhalten Sie eine der folgenden Fehlermeldungen:

```error
Account already exists in another resourcegroup in a subscription. ResourceGroupName: [MyResourceGroup].
```

```error
Resource 'StartStop_VM_Notification' was disallowed by policy. Policy identifiers: '[{\\\"policyAssignment\\\":{\\\"name\\\":\\\"[MyPolicyName]".
```

```error
The subscription is not registered to use namespace 'Microsoft.OperationsManagement'.
```

```error
The subscription is not registered to use namespace 'Microsoft.Insights'.
```

```error
The scope '/subscriptions/000000000000-0000-0000-0000-00000000/resourcegroups/<ResourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<WorkspaceName>/views/StartStopVMView' cannot perform write operation because following scope(s) are locked: '/subscriptions/000000000000-0000-0000-0000-00000000/resourceGroups/<ResourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<WorkspaceName>/views/StartStopVMView'. Please remove the lock and try again
```

```error
A parameter cannot be found that matches parameter name 'TagName'
```

```error
Start-AzureRmVm : Run Login-AzureRmAccount to login
```

### <a name="cause"></a>Ursache

Fehler bei Bereitstellungen können aus einem der folgenden Gründe auftreten:

- Es ist bereits ein Automation-Konto mit demselben Namen in der ausgewählten Region vorhanden.
- Eine Richtlinie lässt die Bereitstellung der Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten nicht zu.
- Der Ressourcentyp `Microsoft.OperationsManagement`, `Microsoft.Insights` oder `Microsoft.Automation` ist nicht registriert.
- Ihr Log Analytics-Arbeitsbereich ist gesperrt.
- Sie verwenden eine veraltete Version der AzureRM-Module oder der Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten.

### <a name="resolution"></a>Lösung

Sehen Sie sich die folgenden möglichen Lösungen für Ihr Problem an:

* Automation-Konten müssen innerhalb einer Azure-Region einzigartig sein, auch wenn sie sich in unterschiedlichen Ressourcengruppen befinden. Überprüfen Sie Ihre vorhandenen Automation-Konten in der Zielregion.
* Eine vorhandene Richtlinie verhindert, dass eine Ressource, die für die Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten erforderlich ist, bereitgestellt werden kann. Navigieren Sie zu Ihren Richtlinienzuweisungen im Azure-Portal, und überprüfen Sie, ob Sie eine Richtlinienzuweisung verwenden, die die Bereitstellung dieser Ressource nicht zulässt. Weitere Informationen hierzu finden Sie unter [RequestDisallowedByPolicy-Fehler](../../azure-resource-manager/templates/error-policy-requestdisallowedbypolicy.md).
* Um die Lösung zum Starten/Beenden von VMs bereitzustellen, muss Ihr Abonnement in den folgenden Azure-Ressourcennamespaces registriert sein:

    * `Microsoft.OperationsManagement`
    * `Microsoft.Insights`
    * `Microsoft.Automation`

   Weitere Informationen zu Fehlern beim Registrieren von Anbietern finden Sie unter [Beheben von Fehlern bei der Ressourcenanbieterregistrierung](../../azure-resource-manager/templates/error-register-resource-provider.md).
* Wenn eine Sperre für Ihren Log Analytics-Arbeitsbereich vorhanden ist, navigieren Sie im Azure-Portal zu Ihrem Arbeitsbereich, und entfernen Sie alle Sperren für die Ressource.
* Wenn diese Lösungen Ihr Problem nicht beheben, befolgen Sie die Anleitungen unter [Aktualisieren der Lösung](../automation-solution-vm-management.md#update-the-solution), um die Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten erneut bereitzustellen.

## <a name="scenario-all-vms-fail-to-start-or-stop"></a><a name="all-vms-fail-to-startstop"></a>Szenario: Keine VM kann gestartet oder beendet werden

### <a name="issue"></a>Problem

Sie haben die Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten konfiguriert, aber keine VM wird gestartet oder beendet.

### <a name="cause"></a>Ursache

Dieser Fehler kann einen der folgenden Gründe haben:

- Ein Zeitplan ist nicht ordnungsgemäß konfiguriert.
- Das ausführende Konto wurde möglicherweise nicht ordnungsgemäß konfiguriert.
- Bei der Ausführung eines Runbooks sind möglicherweise Fehler aufgetreten.
- Die VMs wurden möglicherweise ausgeschlossen.

### <a name="resolution"></a>Lösung

Sehen Sie sich die folgenden möglichen Lösungen für Ihr Problem an:

* Überprüfen Sie, ob der Zeitplan für die Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten ordnungsgemäß konfiguriert wurde. Informationen zum Konfigurieren eines Zeitplans finden Sie unter [Zeitpläne](../automation-schedules.md).

* Überprüfen Sie die [Auftragsdatenströme](../automation-runbook-execution.md#job-statuses) auf Fehler. Suchen Sie nach Aufträgen von einem der folgenden Runbooks:

  * **AutoStop_CreateAlert_Child**
  * **AutoStop_CreateAlert_Parent**
  * **AutoStop_Disable**
  * **AutoStop_VM_Child**
  * **ScheduledStartStop_Base_Classic**
  * **ScheduledStartStop_Child_Classic**
  * **ScheduledStartStop_Child**
  * **ScheduledStartStop_Parent**
  * **SequencedStartStop_Parent**

* Überprüfen Sie, ob Ihr [ausführendes Konto](../manage-runas-account.md) über die erforderlichen Berechtigungen für die VMs verfügt, die Sie starten oder beenden möchten. Wie Sie die Berechtigungen auf einer Ressource überprüfen, erfahren Sie unter [Schnellstart: Anzeigen der zugewiesenen Rollen von Benutzern mit dem Azure-Portal](../../role-based-access-control/check-access.md). Sie müssen die Anwendungs-ID für den Dienstprinzipal angeben, der vom ausführenden Konto verwendet wird. Sie können diesen Wert abrufen, indem Sie Ihr Automation-Konto im Azure-Portal aufrufen. Klicken Sie dann unter **Kontoeinstellungen** auf **Ausführende Konten**, und wählen Sie dann das entsprechende ausführende Konto aus.

* VMs können nicht gestartet oder beendet werden, wenn sie explizit ausgeschlossen wurden. Ausgeschlossene VMs sind in dem Automation-Konto, in dem die Lösung bereitgestellt ist, in der Variablen `External_ExcludeVMNames` festgelegt. Das folgende Beispiel zeigt, wie Sie diesen Wert mit PowerShell abfragen können.

  ```powershell-interactive
  Get-AzAutomationVariable -Name External_ExcludeVMNames -AutomationAccountName <automationAccountName> -ResourceGroupName <resourceGroupName> | Select-Object Value
  ```

## <a name="scenario-some-of-my-vms-fail-to-start-or-stop"></a><a name="some-vms-fail-to-startstop"></a>Szenario: Einige meiner virtuellen Computer können nicht gestartet oder beendet werden

### <a name="issue"></a>Problem

Sie haben die Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten konfiguriert, aber einige der konfigurierten VMs werden nicht gestartet bzw. beendet.

### <a name="cause"></a>Ursache

Dieser Fehler kann einen der folgenden Gründe haben:

- Im Sequenzszenario könnte ein Tag fehlen oder falsch sein.
- Die VM könnte ausgeschlossen sein.
- Das ausführende Konto verfügt möglicherweise nicht über ausreichende Berechtigungen auf der VM.
- Möglicherweise verhindert ein Problem auf der VM das Starten oder Beenden der VM.

### <a name="resolution"></a>Lösung

Überprüfen Sie die folgende Liste auf mögliche Lösungen für Ihr Problem oder Stellen zum Nachschlagen:

* Wenn Sie das [Sequenzszenario](../automation-solution-vm-management.md) der Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten verwenden, müssen Sie sicherstellen, dass jede VM, die Sie starten oder beenden möchten, über das richtige Tag verfügt. Stellen Sie sicher, dass die VMs, die Sie starten möchten, über das `sequencestart`-Tag verfügen, und die virtuellen Computer, die Sie beenden möchten, über das `sequencestop`-Tag. Beide Tags müssen einen positiven Ganzzahlwert haben. Sie können eine Abfrage wie im folgenden Beispiel verwenden, um nach allen virtuellen Computern mit den Tags und deren Werten zu suchen.

  ```powershell-interactive
  Get-AzResource | ? {$_.Tags.Keys -contains "SequenceStart" -or $_.Tags.Keys -contains "SequenceStop"} | ft Name,Tags
  ```

* VMs können nicht gestartet oder beendet werden, wenn sie explizit ausgeschlossen wurden. Ausgeschlossene VMs sind in dem Automation-Konto, in dem die Lösung bereitgestellt ist, in der Variablen `External_ExcludeVMNames` festgelegt. Das folgende Beispiel zeigt, wie Sie diesen Wert mit PowerShell abfragen können.

  ```powershell-interactive
  Get-AzAutomationVariable -Name External_ExcludeVMNames -AutomationAccountName <automationAccountName> -ResourceGroupName <resourceGroupName> | Select-Object Value
  ```

* Um VMs zu starten und zu beenden, muss das ausführende Konto für das Automation-Konto über die entsprechenden Berechtigungen für die VM verfügen. Wie Sie die Berechtigungen auf einer Ressource überprüfen, erfahren Sie unter [Schnellstart: Anzeigen der zugewiesenen Rollen von Benutzern mit dem Azure-Portal](../../role-based-access-control/check-access.md). Sie müssen die Anwendungs-ID für den Dienstprinzipal angeben, der vom ausführenden Konto verwendet wird. Sie können diesen Wert abrufen, indem Sie Ihr Automation-Konto im Azure-Portal aufrufen. Klicken Sie dann unter **Kontoeinstellungen** auf **Ausführende Konten**, und wählen Sie dann das entsprechende ausführende Konto aus.
* Wenn beim Starten oder Aufheben der Zuordnung der VM Probleme auftreten, kann ein Problem auf der VM selbst vorliegen. Zum Beispiel könnte unter anderem ein Update angewendet werden, wenn die VM versucht, herunterzufahren, oder ein Dienst reagiert nicht mehr. Navigieren Sie zu Ihrer VM-Ressource, und überprüfen Sie die **Aktivitätsprotokolle**, um festzustellen, ob Fehler in den Protokollen vorliegen. Sie können auch versuchen, sich bei der VM anzumelden, um zu überprüfen, ob Fehler in den Ereignisprotokollen vorliegen. Weitere Informationen zur Problembehandlung bei Ihrer VM finden Sie unter [Problembehandlung von virtuellen Azure-Computern](../../virtual-machines/troubleshooting/index.yml).
* Überprüfen Sie die [Auftragsdatenströme](../automation-runbook-execution.md#job-statuses) auf Fehler. Wechseln Sie im Portal zu Ihrem Automation-Konto, und wählen Sie unter **Prozessautomatisierung** die Option **Aufträge** aus.

## <a name="scenario-my-custom-runbook-fails-to-start-or-stop-my-vms"></a><a name="custom-runbook"></a>Szenario: Mein benutzerdefiniertes Runbook startet oder beendet meine virtuellen Computer nicht

### <a name="issue"></a>Problem

Sie haben ein benutzerdefiniertes Runbook erstellt oder aus dem PowerShell-Katalog heruntergeladen, das nicht ordnungsgemäß funktioniert.

### <a name="cause"></a>Ursache

Dieser Fehler kann vielfältige Ursachen haben. Navigieren Sie im Azure-Portal zu Ihrem Automation-Konto, und klicken Sie unter **Prozessautomatisierung** auf **Aufträge**. Suchen Sie auf der Seite **Aufträge** nach Aufträgen aus Ihrem Runbook, um ggf. fehlerhafte Aufträge anzuzeigen.

### <a name="resolution"></a>Lösung

Wir empfehlen Folgendes:

* Verwenden Sie die [Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten](../automation-solution-vm-management.md), um virtuelle Computer in Azure Automation zu starten und zu beenden. Diese Lösung stammt von Microsoft. 
* Beachten Sie, dass Microsoft keine benutzerdefinierten Runbooks unterstützt. Möglicherweise finden Sie eine Lösung für Ihr benutzerdefiniertes Runbook im Artikel [Beheben von Fehlern bei Runbooks](runbooks.md). Überprüfen Sie die [Auftragsdatenströme](../automation-runbook-execution.md#job-statuses) auf Fehler. 

## <a name="scenario-vms-dont-start-or-stop-in-the-correct-sequence"></a><a name="dont-start-stop-in-sequence"></a>Szenario: Virtuelle Computer werden nicht in der richtigen Reihenfolge gestartet oder beendet

### <a name="issue"></a>Problem

Die virtuellen Computer, die Sie in der Lösung konfiguriert haben, werden nicht in der richtigen Reihenfolge gestartet oder beendet.

### <a name="cause"></a>Ursache

Dieses Problem wird durch falsche Tags auf den VMs verursacht.

### <a name="resolution"></a>Lösung

Führen Sie die folgenden Schritte aus, um sicherzustellen, dass die Lösung ordnungsgemäß konfiguriert ist.

1. Stellen sicher, dass alle zu startenden oder beendenden virtuellen Computer je nach Situation mit einem `sequencestart`- oder `sequencestop`-Tag versehen sind. Diese Tags benötigen eine positive ganze Zahl als Wert. VMs werden basierend auf diesem Wert in aufsteigender Reihenfolge verarbeitet.
1. Stellen Sie sicher, dass die Ressourcengruppen für die zu startenden oder beendenden virtuellen Computer sich je nach Situation in der Variablen `External_Start_ResourceGroupNames` oder `External_Stop_ResourceGroupNames` befinden.
1. Testen Sie Ihre Änderungen durch Ausführung des `SequencedStartStop_Parent`-Runbooks mit auf „true“ festgelegtem `WHATIF`-Parameter, um eine Vorschau der Änderungen anzuzeigen.

Weitere Informationen zum Verwenden der Lösung zum Starten und Beenden von VMs in einer bestimmten Reihenfolge finden Sie unter [Starten/Beenden von VMs der Reihe nach](../automation-solution-vm-management.md).

## <a name="scenario-startstop-vms-during-off-hours-job-fails-with-403-forbidden-error"></a><a name="403"></a>Szenario: Fehler „403 – verboten“ bei Aufträgen zum Starten/Beenden von VMs außerhalb der Geschäftszeiten

### <a name="issue"></a>Problem

Sie finden Aufträge, bei denen in den Runbooks zum Starten/Beenden von VMs außerhalb der Geschäftszeiten ein `403 forbidden`-Fehler aufgetreten ist.

### <a name="cause"></a>Ursache

Dieses Problem kann durch ein nicht ordnungsgemäß konfiguriertes oder abgelaufenes ausführendes Konto verursacht werden. Unzureichende Berechtigungen des ausführenden Kontos für die VM-Ressourcen kommen auch als Ursache infrage.

### <a name="resolution"></a>Lösung

Um zu überprüfen, ob Ihr ausführendes Konto ordnungsgemäß konfiguriert ist, wechseln Sie im Azure-Portal zu Ihrem Automation-Konto, und wählen Sie unter **Kontoeinstellungen** die Option **Ausführende Konten** aus. Wenn ein ausführendes Konto abgelaufen oder nicht ordnungsgemäß konfiguriert ist, wird diese Bedingung im Status angezeigt.

Wenn Ihr ausführendes Konto falsch konfiguriert ist, löschen Sie es, und erstellen Sie es neu. Weitere Informationen finden Sie unter [Verwalten von ausführenden Azure Automation-Konten](../manage-runas-account.md).

Wenn das Zertifikat für Ihr ausführendes Konto abgelaufen ist, befolgen Sie die Schritte unter [Erneuerung eines selbstsignierten Zertifikat](../manage-runas-account.md#cert-renewal) zur Erneuerung des Zertifikats.

Wenn Berechtigungen fehlen, finden Sie weitere Informationen unter [Schnellstart: Anzeigen der zugewiesenen Rollen von Benutzern mit dem Azure-Portal](../../role-based-access-control/check-access.md). Sie müssen die Anwendungs-ID für den Dienstprinzipal angeben, der vom ausführenden Konto verwendet wird. Sie können diesen Wert abrufen, indem Sie Ihr Automation-Konto im Azure-Portal aufrufen. Klicken Sie dann unter **Kontoeinstellungen** auf **Ausführende Konten**, und wählen Sie dann das entsprechende ausführende Konto aus.

## <a name="scenario-my-problem-isnt-listed-here"></a><a name="other"></a>Szenario: Mein Problem ist hier nicht aufgeführt

### <a name="issue"></a>Problem

Ihr Problem oder unerwartetes Ergebnis bei Verwendung der Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten ist auf dieser Seite nicht aufgeführt.

### <a name="cause"></a>Ursache

Oft können Fehler durch Verwendung einer alten und veralteten Version der Lösung verursacht werden.

> [!NOTE]
> Die Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten wurde mit den Azure-Modulen getestet, die bei der Bereitstellung der Lösung in Ihr Azure Automation-Konto importiert werden. Die Lösung funktioniert derzeit nicht mit neueren Versionen des Azure-Moduls. Diese Einschränkung betrifft nur das Automation-Konto, mit dem Sie die Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten ausführen. In Ihren anderen Automation-Konten können Sie wie unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](../automation-update-azure-modules.md) beschrieben weiterhin neuere Versionen des Azure-Moduls verwenden.

### <a name="resolution"></a>Lösung

Sie können viele Fehler auflösen, indem Sie [die Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten entfernen und aktualisieren](../automation-solution-vm-management.md#update-the-solution). Sie können auch die [Auftragsdatenströme](../automation-runbook-execution.md#job-statuses) auf Fehler überprüfen. 

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem hier nicht aufgeführt wird, oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
* Stellen Sie eine Verbindung mit [@AzureSupport](https://twitter.com/azuresupport) her, dem offiziellen Microsoft Azure-Konto zum Verbessern der Kundenfreundlichkeit. Der Azure-Support verbindet die Azure-Community mit Antworten, Support und Experten.
* Erstellen Sie einen Azure-Supportfall. Wechseln Sie zur [Azure-Supportwebsite](https://azure.microsoft.com/support/options/), und wählen Sie **Support erhalten** aus.