---
title: Problembehandlung beim Starten/Beenden von VMs außerhalb der Geschäftszeiten
description: Dieser Artikel enthält Informationen zur Behandlung von Problemen mit der Lösung zum Starten/Beenden von VMs.
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 04/04/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 003c2c5a2c09957e7a3a4ac0a26b87a9ac43dace
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81679160"
---
# <a name="troubleshoot-the-startstop-vms-during-off-hours-solution"></a>Problembehandlung beim Starten/Beenden von VMs außerhalb der Geschäftszeiten

Dieser Artikel enthält Informationen zur Behandlung von Problemen, die bei der Arbeit mit der Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten auftreten können.

>[!NOTE]
>Dieser Artikel wurde aktualisiert und beinhaltet jetzt das neue Az-Modul von Azure PowerShell. Sie können das AzureRM-Modul weiterhin verwenden, das bis mindestens Dezember 2020 weiterhin Fehlerbehebungen erhält. Weitere Informationen zum neuen Az-Modul und zur Kompatibilität mit AzureRM finden Sie unter [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Einführung in das neue Az-Modul von Azure PowerShell). Installationsanweisungen für das Az-Modul auf Ihrem Hybrid Runbook Worker finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). In Ihrem Automation-Konto können Sie die Module mithilfe der Informationen unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](../automation-update-azure-modules.md) auf die neueste Version aktualisieren.

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

1. Es ist bereits ein Automation-Konto mit demselben Namen in der ausgewählten Region vorhanden.
2. Eine Richtlinie lässt die Bereitstellung der Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten nicht zu.
3. Der Ressourcentyp `Microsoft.OperationsManagement`, `Microsoft.Insights` oder `Microsoft.Automation` ist nicht registriert.
4. Ihr Log Analytics-Arbeitsbereich ist gesperrt.
5. Sie verwenden eine veraltete Version der AzureRM-Module oder der Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten.

### <a name="resolution"></a>Lösung

Sehen Sie sich die folgenden möglichen Lösungen für Ihr Problem an:

* Automation-Konten müssen innerhalb einer Azure-Region eindeutig sein, auch wenn sie sich in unterschiedlichen Ressourcengruppen befinden. Überprüfen Sie Ihre vorhandenen Automation-Konten in der Zielregion.
* Eine vorhandene Richtlinie verhindert, dass eine Ressource, die für die Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten erforderlich ist, bereitgestellt werden kann. Navigieren Sie zu Ihren Richtlinienzuweisungen im Azure-Portal, und überprüfen Sie, ob Sie eine Richtlinienzuweisung verwenden, die die Bereitstellung dieser Ressource nicht zulässt. Weitere Informationen hierzu finden Sie unter [RequestDisallowedByPolicy](../../azure-resource-manager/templates/error-policy-requestdisallowedbypolicy.md).
* Um die Lösung zum Starten/Beenden von VMs bereitzustellen, muss Ihr Abonnement in den folgenden Azure-Ressourcennamespaces registriert sein:

    * `Microsoft.OperationsManagement`
    * `Microsoft.Insights`
    * `Microsoft.Automation`

   Weitere Informationen zu Fehlern beim Registrieren von Anbietern finden Sie unter [Beheben von Fehlern bei der Ressourcenanbieterregistrierung](../../azure-resource-manager/templates/error-register-resource-provider.md).
* Wenn eine Sperre für Ihren Log Analytics-Arbeitsbereich vorhanden ist, navigieren Sie im Azure-Portal zu Ihrem Arbeitsbereich, und entfernen Sie alle Sperren für die Ressource.
* Wenn die oben genannten Lösungen Ihr Problem nicht beheben, befolgen Sie die Anleitungen unter [Aktualisieren der Lösung](../automation-solution-vm-management.md#update-the-solution), um die Lösung zum Starten/Beenden erneut bereitzustellen.

## <a name="scenario-all-vms-fail-to-start-or-stop"></a><a name="all-vms-fail-to-startstop"></a>Szenario: Keine VM kann gestartet oder beendet werden

### <a name="issue"></a>Problem

Sie haben die Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten konfiguriert, aber keine VM wird gestartet oder beendet.

### <a name="cause"></a>Ursache

Dieser Fehler kann einen der folgenden Gründe haben:

1. Ein Zeitplan ist nicht ordnungsgemäß konfiguriert.
2. Das ausführende Konto wurde möglicherweise nicht ordnungsgemäß konfiguriert.
3. Bei der Ausführung eines Runbooks sind möglicherweise Fehler aufgetreten.
4. Die VMs wurden möglicherweise ausgeschlossen.

### <a name="resolution"></a>Lösung

Sehen Sie sich die folgenden möglichen Lösungen für Ihr Problem an:

* Überprüfen Sie, ob der Zeitplan für die Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten ordnungsgemäß konfiguriert wurde. Wie Sie einen Zeitplan konfigurieren, erfahren Sie im Artikel [Planen eines Runbooks in Azure Automation](../automation-schedules.md).

* Überprüfen Sie die [Auftragsdatenströme](../automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) auf Fehler. Suchen Sie nach Aufträgen von einem der folgenden Runbooks:

  * **AutoStop_CreateAlert_Child**
  * **AutoStop_CreateAlert_Parent**
  * **AutoStop_Disable**
  * **AutoStop_VM_Child**
  * **ScheduledStartStop_Base_Classic**
  * **ScheduledStartStop_Child_Classic**
  * **ScheduledStartStop_Child**
  * **ScheduledStartStop_Parent**
  * **SequencedStartStop_Parent**

* Überprüfen Sie, ob Ihr [ausführendes Konto](../manage-runas-account.md) über die erforderlichen Berechtigungen für die VMs verfügt, die Sie starten oder beenden möchten. Wie Sie die Berechtigungen auf einer Ressource überprüfen, erfahren Sie unter [Schnellstart: Anzeigen der zugewiesenen Rollen von Benutzern mit dem Azure-Portal](../../role-based-access-control/check-access.md). Sie müssen die Anwendungs-ID für den Dienstprinzipal angeben, der vom ausführenden Konto verwendet wird. Sie können diesen Wert abrufen, indem Sie zu Ihrem Automation-Konto im Azure-Portal wechseln, unter **Kontoeinstellungen** die Option **Ausführende Konten** auswählen und auf das entsprechende ausführende Konto klicken.

* Virtuelle Computer können nicht gestartet oder beendet werden, wenn sie explizit ausgeschlossen sind. Ausgeschlossene VMs sind in dem Automation-Konto, in dem die Lösung bereitgestellt ist, in der Variablen `External_ExcludeVMNames` festgelegt. Das folgende Beispiel zeigt, wie Sie diesen Wert mit PowerShell abfragen können.

  ```powershell-interactive
  Get-AzAutomationVariable -Name External_ExcludeVMNames -AutomationAccountName <automationAccountName> -ResourceGroupName <resourceGroupName> | Select-Object Value
  ```

## <a name="scenario-some-of-my-vms-fail-to-start-or-stop"></a><a name="some-vms-fail-to-startstop"></a>Szenario: Einige meiner virtuellen Computer können nicht gestartet oder beendet werden

### <a name="issue"></a>Problem

Sie haben die Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten konfiguriert, aber einige der konfigurierten VMs werden nicht gestartet bzw. beendet.

### <a name="cause"></a>Ursache

Dieser Fehler kann einen der folgenden Gründe haben:

1. Im Sequenzszenario könnte ein Tag fehlen oder falsch sein.
2. Die VM könnte ausgeschlossen sein.
3. Das ausführende Konto verfügt möglicherweise nicht über ausreichende Berechtigungen auf der VM.
4. Möglicherweise verhindert ein Problem auf der VM das Starten oder Beenden der VM.

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

* Um VMs zu starten und zu beenden, muss das ausführende Konto für das Automation-Konto über die entsprechenden Berechtigungen für die VM verfügen. Wie Sie die Berechtigungen auf einer Ressource überprüfen, erfahren Sie unter [Schnellstart: Anzeigen der zugewiesenen Rollen von Benutzern mit dem Azure-Portal](../../role-based-access-control/check-access.md). Sie müssen die Anwendungs-ID für den Dienstprinzipal angeben, der vom ausführenden Konto verwendet wird. Sie können diesen Wert abrufen, indem Sie zu Ihrem Automation-Konto im Azure-Portal wechseln, unter **Kontoeinstellungen** die Option **Ausführende Konten** auswählen und auf das entsprechende ausführende Konto klicken.

* Wenn beim Starten oder Aufheben der Zuordnung der VM Probleme auftreten, kann ein Problem auf der VM selbst vorliegen. Möglicherweise wird ein Update angewendet, während die VM versucht herunterzufahren, oder ein Dienst reagiert nicht mehr. Weitere Gründe sind möglich. Navigieren Sie zu Ihrer VM-Ressource, und überprüfen Sie die **Aktivitätsprotokolle**, um festzustellen, ob Fehler in den Protokollen vorhanden sind. Sie können auch versuchen, sich bei der VM anzumelden, um festzustellen, ob Fehler im Ereignisprotokoll vorhanden sind. Weitere Informationen zur Problembehandlung Ihrer VM finden Sie unter [Problembehandlung von virtuellen Azure-Computern](../../virtual-machines/troubleshooting/index.yml).

* Überprüfen Sie die [Auftragsdatenströme](../automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) auf Fehler. Wechseln Sie im Portal zu Ihrem Automation-Konto, und wählen Sie unter **Prozessautomatisierung** die Option **Aufträge** aus.

## <a name="scenario-my-custom-runbook-fails-to-start-or-stop-my-vms"></a><a name="custom-runbook"></a>Szenario: Mein benutzerdefiniertes Runbook startet oder beendet meine virtuellen Computer nicht

### <a name="issue"></a>Problem

Sie haben ein benutzerdefiniertes Runbook erstellt oder eines aus dem PowerShell-Katalog heruntergeladen, und es funktioniert nicht ordnungsgemäß.

### <a name="cause"></a>Ursache

Dieser Fehler kann vielfältige Ursachen haben. Wechseln Sie im Azure-Portal zu Ihrem Automation-Konto, und wählen Sie unter **Prozessautomatisierung** die Option **Aufträge** aus. Suchen Sie auf der Seite „Aufträge“ nach Aufträgen von Ihrem Runbook, um mögliche Auftragsfehler anzuzeigen.

### <a name="resolution"></a>Lösung

Folgendes wird empfohlen:

* Verwenden Sie die [Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten](../automation-solution-vm-management.md), um virtuelle Computer in Azure Automation zu starten und zu beenden. Diese Lösung stammt von Microsoft. 

* Beachten Sie, dass Microsoft keine benutzerdefinierten Runbooks unterstützt. Möglicherweise finden Sie eine Lösung für Ihr benutzerdefiniertes Runbook im Artikel [Beheben von Fehlern bei Runbooks](runbooks.md). Überprüfen Sie die [Auftragsdatenströme](../automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) auf Fehler. 

## <a name="scenario-vms-dont-start-or-stop-in-the-correct-sequence"></a><a name="dont-start-stop-in-sequence"></a>Szenario: Virtuelle Computer werden nicht in der richtigen Reihenfolge gestartet oder beendet

### <a name="issue"></a>Problem

Die virtuellen Computer, die Sie in der Lösung konfiguriert haben, werden nicht in der richtigen Reihenfolge gestartet oder beendet.

### <a name="cause"></a>Ursache

Dieses Problem wird durch falsche Tags auf den VMs verursacht.

### <a name="resolution"></a>Lösung

Stellen Sie mit den folgenden Schritten sicher, dass die Lösung ordnungsgemäß konfiguriert ist.

1. Stellen sicher, dass alle zu startenden oder beendenden virtuellen Computer je nach Situation mit einem `sequencestart`- oder `sequencestop`-Tag versehen sind. Diese Tags benötigen eine positive ganze Zahl als Wert. VMs werden basierend auf diesem Wert in aufsteigender Reihenfolge verarbeitet.
2. Stellen Sie sicher, dass die Ressourcengruppen für die zu startenden oder beendenden virtuellen Computer sich je nach Situation in der Variablen `External_Start_ResourceGroupNames` oder `External_Stop_ResourceGroupNames` befinden.
3. Testen Sie Ihre Änderungen durch Ausführung des `SequencedStartStop_Parent`-Runbooks mit auf „true“ festgelegtem `WHATIF`-Parameter, um eine Vorschau der Änderungen anzuzeigen.
4. Weitere Informationen zum Verwenden der Lösung zum Starten und Beenden von VMs in einer bestimmten Reihenfolge finden Sie unter [Starten/Beenden von VMs der Reihe nach](../automation-solution-vm-management.md).

## <a name="scenario-startstop-vms-during-off-hours-job-fails-with-403-forbidden-error"></a><a name="403"></a>Szenario: Fehler „403 – verboten“ bei Aufträgen zum Starten/Beenden von VMs außerhalb der Geschäftszeiten

### <a name="issue"></a>Problem

Sie finden Aufträge, bei denen in den Runbooks zum Starten/Beenden von VMs außerhalb der Geschäftszeiten ein `403 forbidden`-Fehler aufgetreten ist.

### <a name="cause"></a>Ursache

Dieses Problem kann durch ein nicht ordnungsgemäß konfiguriertes oder abgelaufenes ausführendes Konto verursacht werden. Unzureichende Berechtigungen des ausführenden Kontos für die VM-Ressourcen kommen auch als Ursache infrage.

### <a name="resolution"></a>Lösung

Um zu überprüfen, ob Ihr ausführendes Konto ordnungsgemäß konfiguriert ist, wechseln Sie im Azure-Portal zu Ihrem Automation-Konto, und wählen Sie unter **Kontoeinstellungen** die Option **Ausführende Konten** aus. Wenn ein ausführendes Konto abgelaufen oder nicht ordnungsgemäß konfiguriert ist, wird diese Bedingung im Status angezeigt.

Wenn Ihr ausführendes Konto falsch konfiguriert ist, müssen Sie es löschen und neu erstellen. Weitere Informationen finden Sie unter [Verwalten von ausführenden Azure Automation-Konten](../manage-runas-account.md).

Wenn das Zertifikat für Ihr ausführendes Konto abgelaufen ist, befolgen Sie die Schritte unter [Erneuerung eines selbstsignierten Zertifikat](../manage-runas-account.md#cert-renewal), um das Zertifikat zu erneuern.

Wenn Berechtigungen fehlen, finden Sie weitere Informationen unter [Schnellstart: Anzeigen der zugewiesenen Rollen von Benutzern mit dem Azure-Portal](../../role-based-access-control/check-access.md). Sie müssen die Anwendungs-ID für den Dienstprinzipal angeben, der vom ausführenden Konto verwendet wird. Sie können diesen Wert abrufen, indem Sie zu Ihrem Automation-Konto im Azure-Portal wechseln, unter **Kontoeinstellungen** die Option **Ausführende Konten** auswählen und auf das entsprechende ausführende Konto klicken.

## <a name="scenario-my-problem-isnt-listed-above"></a><a name="other"></a>Szenario: Mein Problem ist oben nicht aufgeführt

### <a name="issue"></a>Problem

Ihr Problem oder unerwartetes Ergebnis bei Verwendung der Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten ist auf dieser Seite nicht aufgeführt.

### <a name="cause"></a>Ursache

Oft können Fehler durch Verwendung einer alten und veralteten Version der Lösung verursacht werden.

> [!NOTE]
> Die Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten wurde mit den Azure-Modulen getestet, die bei der Bereitstellung der Lösung in Ihr Azure Automation-Konto importiert werden. Die Lösung funktioniert derzeit nicht mit neueren Versionen des Azure-Moduls. Dies betrifft nur das Automation-Konto, mit dem Sie die Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten ausführen. In Ihren anderen Automation-Konten können Sie weiterhin neuere Versionen des Azure-Moduls verwenden, wie in [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](../automation-update-azure-modules.md) beschrieben.

### <a name="resolution"></a>Lösung

Entfernen und [aktualisieren Sie die Lösung zum Starten/Beheben von VMs außerhalb der Geschäftszeiten](../automation-solution-vm-management.md#update-the-solution) – damit lassen sich viele Fehler beseitigen. Überprüfen Sie außerdem die [Auftragsdatenströme](../automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) auf Fehler. 

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt wird oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.
* Erstellen Sie einen Azure-Supportfall. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.