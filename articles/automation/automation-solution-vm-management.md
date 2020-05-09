---
title: Lösung „VMs außerhalb der Geschäftszeiten starten/beenden“
description: Mit dieser Lösung für die VM-Verwaltung werden Ihre virtuellen Azure-Computer nach einem Zeitplan gestartet und beendet und mit Azure Monitor-Protokollen proaktiv überwacht.
services: automation
ms.subservice: process-automation
ms.date: 04/28/2020
ms.topic: conceptual
ms.openlocfilehash: f7e30fd0d53af7ee61d919b56e9ffcd1f1b6bd36
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82207597"
---
# <a name="startstop-vms-during-off-hours-solution-in-azure-automation"></a>Lösung „VMs außerhalb der Geschäftszeiten starten/beenden“ in Azure Automation

Die Lösung **VMs außerhalb der Geschäftszeiten starten/beenden** startet bzw. beendet Ihre virtuellen Azure-Computer. Damit können Sie Computer nach benutzerdefinierten Zeitplänen starten und beenden und außerdem über Azure Monitor-Protokolle Erkenntnisse aus Ihren Daten ziehen und durch die Nutzung von [Aktionsgruppen](../azure-monitor/platform/action-groups.md) optional E-Mails senden. Die Lösung unterstützt in den meisten Szenarien sowohl Azure Resource Manager-VMs als auch klassische VMs. 

Diese Lösung verwendet das Cmdlet [Start-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/start-azurermvm?view=azurermps-6.13.0) zum Starten von VMs. Zum Beenden von VMs wird [Stop-AzureRmVM](https://docs.microsoft.com/powershell/module/AzureRM.Compute/Stop-AzureRmVM?view=azurermps-6.13.0) verwendet.

> [!NOTE]
> Die Lösung **VMs außerhalb der Geschäftszeiten starten/beenden** wurde dahin gehend aktualisiert, dass sie die neuesten verfügbaren Versionen der Azure-Module unterstützt. Die aktualisierte Version dieser Lösung, die im Marketplace verfügbar ist, bietet keine Unterstützung für AzureRM-Module, da eine Migration von AzureRM- zu Az-Modulen durchgeführt wurde.

Die Lösung bietet eine dezentrale und kostengünstige Automatisierungsoption für Benutzer, die ihre VM-Kosten optimieren möchten. Mit dieser Lösung haben Sie folgende Möglichkeiten:

- [Starten und Beenden von VMs nach Zeitplan](automation-solution-vm-management-config.md#schedule).
- Starten und Beenden von VMs nach Zeitplan in aufsteigender Reihenfolge [mithilfe von Azure-Tags](automation-solution-vm-management-config.md#tags) (wird für klassische VMs nicht unterstützt).
- [Automatisches Beenden von VMs bei geringer CPU-Auslastung](automation-solution-vm-management-config.md#cpuutil).

Die aktuelle Lösung hat folgende Einschränkungen:

- Sie ermöglicht die Verwaltung von VMs in allen Regionen. Allerdings lässt sie sich nur unter demselben Abonnement wie Ihr Azure Automation-Konto verwenden.
- Sie ist in Azure und Azure Government für jede Region verfügbar, die einen Log Analytics-Arbeitsbereich, ein Azure Automation-Konto und Warnungen unterstützt. Azure Government-Regionen unterstützen derzeit keine E-Mail-Funktionalität.

## <a name="solution-prerequisites"></a>Voraussetzungen für die Lösung

Für die Runbooks für diese Lösung wird ein [ausführendes Azure-Konto](automation-create-runas-account.md) verwendet. Das ausführende Konto ist die bevorzugte Authentifizierungsmethode, da anstelle eines Kennworts, das ablaufen oder sich häufig ändern kann, eine Zertifikatauthentifizierung verwendet wird.

Sie sollten für die Lösung **VMs außerhalb der Geschäftszeiten starten/beenden** ein separates Automation-Konto verwenden. Die Azure-Modulversionen werden häufig aktualisiert, und ihre Parameter können sich ändern. Die Lösung wird nicht mit derselben Häufigkeit aktualisiert, sodass sie eventuell nicht mit neueren Versionen der verwendeten Cmdlets funktioniert. Sie sollten zum Testen auf Modulupdates ein Automation-Testkonto verwenden, bevor Sie sie in Ihre Automation-Konten für die Produktion importieren.

## <a name="solution-permissions"></a>Berechtigungen für die Lösung

Sie müssen über bestimmte Berechtigungen verfügen, um die Lösung **VMs außerhalb der Geschäftszeiten starten/beenden** bereitzustellen. Die Berechtigungen unterscheiden sich je nachdem, ob das in der Lösung verwendete Automation-Konto und der Log Analytics Arbeitsbereich vorab erstellt wurden oder die Lösung während der Bereitstellung ein neues Konto und einen neuen Arbeitsbereich erstellt. 

Wenn Sie Mitwirkender im Abonnement und globaler Administrator in Ihrem Azure Active Directory-Mandanten sind, müssen Sie keine Berechtigungen konfigurieren. Wenn Sie nicht über diese Rechte verfügen oder eine benutzerdefinierte Rolle konfigurieren möchten, stellen Sie sicher, dass Sie über die unten beschriebenen Berechtigungen verfügen.

### <a name="permissions-for-pre-existing-automation-account-and-log-analytics-workspace"></a>Berechtigungen bei bereits vorhandenem Automation-Konto und Log Analytics-Arbeitsbereich

Um die Lösung **VMs außerhalb der Geschäftszeiten starten/beenden** für ein vorhandenes Automation-Konto und einen vorhandenen Log Analytics-Arbeitsbereich bereitzustellen, muss der Benutzer, der die Lösung bereitstellt, über die folgenden Berechtigungen für den Gültigkeitsbereich der Ressourcengruppe verfügen. Weitere Informationen zu Rollen finden Sie unter [Benutzerdefinierte Rollen für Azure-Ressourcen](../role-based-access-control/custom-roles.md).

| Berechtigung | `Scope`|
| --- | --- |
| Microsoft.Automation/automationAccounts/read | Ressourcengruppe |
| Microsoft.Automation/automationAccounts/variables/write | Ressourcengruppe |
| Microsoft.Automation/automationAccounts/schedules/write | Ressourcengruppe |
| Microsoft.Automation/automationAccounts/runbooks/write | Ressourcengruppe |
| Microsoft.Automation/automationAccounts/connections/write | Ressourcengruppe |
| Microsoft.Automation/automationAccounts/certificates/write | Ressourcengruppe |
| Microsoft.Automation/automationAccounts/modules/write | Ressourcengruppe |
| Microsoft.Automation/automationAccounts/modules/read | Ressourcengruppe |
| Microsoft.automation/automationAccounts/jobSchedules/write | Ressourcengruppe |
| Microsoft.Automation/automationAccounts/jobs/write | Ressourcengruppe |
| Microsoft.Automation/automationAccounts/jobs/read | Ressourcengruppe |
| Microsoft.OperationsManagement/solutions/write | Ressourcengruppe |
| Microsoft.OperationalInsights/workspaces/* | Ressourcengruppe |
| Microsoft.Insights/diagnosticSettings/write | Ressourcengruppe |
| Microsoft.Insights/ActionGroups/Write | Ressourcengruppe |
| Microsoft.Insights/ActionGroups/read | Ressourcengruppe |
| Microsoft.Resources/subscriptions/resourceGroups/read | Ressourcengruppe |
| Microsoft.Resources/deployments/* | Ressourcengruppe |

### <a name="permissions-for-new-automation-account-and-new-log-analytics-workspace"></a>Berechtigungen bei neuem Automation-Konto und Log Analytics-Arbeitsbereich

Sie können die Lösung **VMs außerhalb der Geschäftszeiten starten/beenden** unter einem neuen Automation-Konto sowie einem neuen Log Analytics-Arbeitsbereich bereitstellen. In diesem Fall benötigt der Benutzer, der die Lösung bereitstellt, die im vorherigen Abschnitt definierten Berechtigungen und darüber hinaus die in diesem Abschnitt definierten Berechtigungen. 

Der Benutzer, der die Lösung bereitstellt, benötigt die folgenden Rollen:

- Co-Administrator für das Abonnement. Diese Rolle ist erforderlich, um das klassische ausführende Konto zu erstellen, wenn Sie klassische VMs verwalten möchten. [Klassische ausführende Konten](automation-create-standalone-account.md#create-a-classic-run-as-account) werden nicht mehr standardmäßig erstellt.
- Mitglied der Rolle „Anwendungsentwickler“ in [Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md). Weitere Informationen zum Konfigurieren von ausführenden Konten finden Sie unter [Berechtigungen zum Konfigurieren von ausführenden Konten](manage-runas-account.md#permissions).
- Mitwirkender im Abonnement oder die folgenden Berechtigungen.

| Berechtigung |`Scope`|
| --- | --- |
| Microsoft.Authorization/Operations/read | Subscription|
| Microsoft.Authorization/permissions/read |Subscription|
| Microsoft.Authorization/roleAssignments/read | Subscription |
| Microsoft.Authorization/roleAssignments/write | Subscription |
| Microsoft.Authorization/roleAssignments/delete | Subscription |
| Microsoft.Automation/automationAccounts/connections/read | Ressourcengruppe |
| Microsoft.Automation/automationAccounts/certificates/read | Ressourcengruppe |
| Microsoft.Automation/automationAccounts/write | Ressourcengruppe |
| Microsoft.OperationalInsights/workspaces/write | Ressourcengruppe |

## <a name="solution-components"></a>Lösungskomponenten

Die Lösung **VMs außerhalb der Geschäftszeiten starten/beenden** enthält vorkonfigurierte Runbooks, Zeitpläne und eine Integration mit Azure Monitor-Protokollen. Mithilfe dieser Elemente können Sie das Starten und Herunterfahren Ihrer VMs an Ihre geschäftlichen Anforderungen anpassen.

### <a name="runbooks"></a>Runbooks

In der folgenden Tabelle sind die mit dieser Lösung in Ihrem Automation-Konto bereitgestellten Runbooks aufgeführt. Nehmen Sie am Runbookcode KEINE Änderungen vor. Schreiben Sie stattdessen Ihr eigenes Runbook, um neue Funktionen nutzen zu können.

> [!IMPORTANT]
> Führen Sie keine Runbooks direkt aus, an deren Name der Zusatz **child** angefügt ist.

Alle übergeordneten Runbooks enthalten den Parameter `WhatIf`. Bei der Festlegung auf TRUE wird durch den Parameter die Beschreibung des exakten Verhaltens des Runbooks unterstützt, wenn es ohne den Parameter ausgeführt wird, und es wird überprüft, ob die richtigen VMs als Ziel verwendet werden. Ein Runbook führt seine zugehörigen definierten Aktionen nur aus, wenn der Parameter `WhatIf` auf FALSE festgelegt ist.

|Runbook | Parameter | BESCHREIBUNG|
| --- | --- | ---|
|AutoStop_CreateAlert_Child | VMObject <br> AlertAction <br> WebHookURI | Wird über das übergeordnete Runbook aufgerufen. Dieses Runbook erstellt für das Auto-Stop-Szenario Warnungen pro Ressource.|
|AutoStop_CreateAlert_Parent | VMList<br> WhatIf: „true“ oder „false“  | Erstellt oder aktualisiert Azure-Warnungsregeln auf VMs im Zielabonnement oder den Zielressourcengruppen. <br> `VMList` ist eine durch Trennzeichen getrennte Liste mit VMs. Beispiel: `vm1, vm2, vm3`.<br> `WhatIf` ermöglicht die Überprüfung der Runbooklogik ohne Ausführung.|
|AutoStop_Disable | Keine | Deaktiviert Auto-Stop-Warnungen und den Standardzeitplan.|
|AutoStop_VM_Child | WebHookData | Wird über das übergeordnete Runbook aufgerufen. Warnungsregeln rufen dieses Runbook auf, um einen klassischen virtuellen Computer zu beenden.|
|AutoStop_VM_Child_ARM | WebHookData |Wird über das übergeordnete Runbook aufgerufen. Warnungsregeln rufen dieses Runbook auf, um einen virtuellen Computer zu beenden.  |
|ScheduledStartStop_Base_Classic | CloudServiceName<br> Aktion: Starten oder Beenden<br> VMList  | Führt Aktionen zum Starten oder Beenden in der klassischen VM-Gruppe durch Cloud Services aus. |
|ScheduledStartStop_Child | VMName <br> Aktion: Starten oder Beenden <br> ResourceGroupName | Wird über das übergeordnete Runbook aufgerufen. Führt für den geplanten Beendigungsvorgang eine Aktion zum Starten oder Beenden aus.|
|ScheduledStartStop_Child_Classic | VMName<br> Aktion: Starten oder Beenden<br> ResourceGroupName | Wird über das übergeordnete Runbook aufgerufen. Führt für den geplanten Beendigungsvorgang eine Aktion zum Starten oder Beenden für klassische VMs aus. |
|ScheduledStartStop_Parent | Aktion: Starten oder Beenden <br>VMList <br> WhatIf: „true“ oder „false“ | Das Starten oder Beenden wirkt sich auf alle virtuellen Computer im Abonnement aus. Bearbeiten Sie die Variablen `External_Start_ResourceGroupNames` und `External_Stop_ResourceGroupNames` so, dass sie nur für diese Zielressourcengruppen ausgeführt werden. Sie können zudem bestimmte VMs ausschließen, indem Sie die Variable `External_ExcludeVMNames` aktualisieren.|
|SequencedStartStop_Parent | Aktion: Starten oder Beenden <br> WhatIf: „true“ oder „false“<br>VMList| Erstellen Sie auf jeder VM, für die Sie die Aktivität zum Starten/Beenden verwenden möchten, Tags mit den Namen **sequencestart** und **sequencestop**. Bei diesen Tagnamen wird zwischen Groß- und Kleinschreibung unterschieden. Der Wert des Tags muss eine positive ganze Zahl (1, 2, 3) sein, die der Reihenfolge entspricht, in der das Starten oder Beenden durchgeführt werden soll. <br>**Hinweis**: Die VMs müssen sich innerhalb von Ressourcengruppen befinden, die in den Variablen `External_Start_ResourceGroupNames`, `External_Stop_ResourceGroupNames` und `External_ExcludeVMNames` definiert sind. Diese müssen über die entsprechenden Tags verfügen, damit Aktionen wirksam werden.|

### <a name="variables"></a>Variables

In der folgenden Tabelle sind die in Ihrem Automation-Konto erstellten Variablen aufgeführt. Ändern Sie nur Variablen, die das Präfix `External` aufweisen. Wenn Sie Variablen mit dem Präfix `Internal` ändern, hat dies unerwünschte Auswirkungen.

> [!NOTE]
> Die Einschränkungen beim VM-Namen und der Ressourcengruppe sind größtenteils durch die Variablengröße bedingt. Weitere Informationen finden Sie unter [Variable Objekte in Azure Automation](https://docs.microsoft.com/azure/automation/shared-resources/variables).

|Variable | BESCHREIBUNG|
|---------|------------|
|External_AutoStop_Condition | Der bedingte Operator, der zum Konfigurieren der Bedingung erforderlich ist, bevor eine Warnung ausgelöst wird. Mögliche Werte sind `GreaterThan`, `GreaterThanOrEqual`, `LessThan` und `LessThanOrEqual`.|
|External_AutoStop_Description | Die Warnung zum Beenden des virtuellen Computers, wenn der CPU-Prozentsatz den Schwellenwert überschreitet.|
|External_AutoStop_Frequency | Die Auswertungshäufigkeit für die Regel. Für diesen Parameter können Zeiträume eingegeben werden. Mögliche Werte reichen von 5 Minuten bis 6 Stunden. |
|External_AutoStop_MetricName | Der Name der Leistungsmetrik, für die die Azure-Warnungsregel konfiguriert werden muss.|
|External_AutoStop_Severity | Der Schweregrad der Metrikwarnung, der zwischen 0 und 4 liegen kann. |
|External_AutoStop_Threshold | Der Schwellenwert für die Azure-Warnungsregel, der in der Variable `External_AutoStop_MetricName` angegeben ist. Prozentwerte können zwischen 1 und 100 liegen.|
|External_AutoStop_TimeAggregationOperator | Der Zeitaggregationsoperator, der auf die ausgewählte Fenstergröße angewandt wird, um die Bedingung auszuwerten. Mögliche Werte sind `Average`, `Minimum`, `Maximum`, `Total` und `Last`.|
|External_AutoStop_TimeWindow | Das Zeitfenster, in dem Azure ausgewählte Metriken zum Auslösen einer Warnung analysiert. Für diesen Parameter können Zeiträume eingegeben werden. Mögliche Werte reichen von 5 Minuten bis 6 Stunden.|
|External_EnableClassicVMs| Dieser Wert gibt an, ob die Lösung auf klassische virtuelle Computer abzielt. Der Standardwert lautet „True“. Legen Sie diese Variable für Azure Cloud Solution Provider-Abonnements (CSP) auf FALSE fest. Für klassische VMs ist ein [klassisches ausführendes Konto](automation-create-standalone-account.md#create-a-classic-run-as-account) erforderlich.|
|External_ExcludeVMNames | Durch Trennzeichen getrennte Liste mit den Namen der auszuschließenden VMs (begrenzt auf 140 VMs). Wenn Sie dieser Liste mehr als 140 VMs hinzufügen, kann es bei VMs, für die Ausschluss festgelegt ist, zu unbeabsichtigtem Starten oder Beenden kommen.|
|External_Start_ResourceGroupNames | Durch Trennzeichen getrennte Liste mit mindestens einer Ressourcengruppe als Ziel für die Startaktionen.|
|External_Stop_ResourceGroupNames | Durch Trennzeichen getrennte Liste mit mindestens einer Ressourcengruppe als Ziel für die Beendigungsaktionen.|
|External_WaitTimeForVMRetrySeconds |Die Wartezeit in Sekunden für die Aktionen, die mit den VMs für das Runbook **SequencedStartStop_Parent** ausgeführt werden sollen. Diese Variable ermöglicht es dem Runbook, die angegebene Anzahl von Sekunden auf die untergeordneten Vorgänge zu warten, bevor es mit der nächsten Aktion fortfährt. Die maximale Wartezeit beträgt 10.800 Sekunden (3 Stunden). Der Standardwert beträgt 2.100 Sekunden.|
|Internal_AutomationAccountName | Gibt den Namen des Automation-Kontos an.|
|Internal_AutoSnooze_ARM_WebhookURI | Der Webhook-URI, der für das AutoStop-Szenario für VMs aufgerufen wird.|
|Internal_AutoSnooze_WebhookUri | Der Webhook-URI, der für das AutoStop-Szenario für klassische VMs aufgerufen wird.|
|Internal_AzureSubscriptionId | Die Azure-Abonnement-ID.|
|Internal_ResourceGroupName | Der Ressourcengruppennamen des Automation-Kontos.|

>[!NOTE]
>Der Standardwert für die Variable `External_WaitTimeForVMRetryInSeconds` wurde von 600 auf 2.100 aktualisiert. 

In allen Szenarien sind die Variablen `External_Start_ResourceGroupNames`, `External_Stop_ResourceGroupNames` und `External_ExcludeVMNames` für die Ziel-VMs erforderlich. Ausgenommen sind die VMs in den durch Trennzeichen getrennten Listen für die Runbooks **AutoStop_CreateAlert_Parent**, **SequencedStartStop_Parent** und **ScheduledStartStop_Parent**. Dies bedeutet, dass sich die VMs in Zielressourcengruppen befinden müssen, damit Aktionen zum Starten und Beenden durchgeführt werden. Die Logik ähnelt der von Azure Policy, da Sie das Abonnement oder die Ressourcengruppe als Ziel verwenden können und neu erstellte VMs Aktionen erben. Hierdurch wird vermieden, dass für jeden virtuellen Computer jeweils ein separater Zeitplan gepflegt werden muss und dass Aktionen zum Starten und Beenden für die Skalierung verwaltet werden müssen.

### <a name="schedules"></a>Zeitpläne

In der folgenden Tabelle sind die einzelnen in Ihrem Automation-Konto erstellten Standardzeitpläne aufgeführt. Sie können sie ändern oder Ihre eigenen benutzerdefinierten Zeitpläne erstellen. Standardmäßig sind alle Zeitpläne deaktiviert, mit Ausnahme von **Scheduled_StartVM** und **Scheduled_StopVM**.

Aktivieren Sie nicht alle Zeitpläne, da dies zu sich überlappenden Zeitplanaktionen führen kann. Am besten ermitteln Sie, welche Optimierungen Sie ausführen möchten, und nehmen dann die entsprechenden Änderungen vor. Weitere Erläuterungen finden Sie in den Beispielszenerien im Übersichtsabschnitt.

|Zeitplanname | Häufigkeit | BESCHREIBUNG|
|--- | --- | ---|
|Schedule_AutoStop_CreateAlert_Parent | Alle 8 Stunden | Führt das Runbook **AutoStop_CreateAlert_Parent** alle 8 Stunden aus. Damit werden wiederum die VM-basierten Werte in den Variablen `External_Start_ResourceGroupNames`, `External_Stop_ResourceGroupNames` und `External_ExcludeVMNames` beendet. Alternativ können Sie mithilfe des Parameters `VMList` eine durch Kommas getrennte Liste mit VMs angeben.|
|Scheduled_StopVM | Benutzerdefiniert, täglich | Führt das Runbook **ScheduledStopStart_Parent** mit dem Parameter `Stop` jeden Tag zum angegebenen Zeitpunkt aus. Beendet automatisch alle VMs, für die die Regeln der Variablenressourcen erfüllt sind. Aktivieren Sie den zugehörigen Zeitplan (**Scheduled-StartVM**).|
|Scheduled_StartVM | Benutzerdefiniert, täglich | Führt das Runbook **ScheduledStopStart_Parent** mit dem Parameterwert `Start` jeden Tag zum angegebenen Zeitpunkt aus. Startet automatisch alle VMs, für die die Regeln der Variablenressourcen erfüllt sind. Aktivieren Sie den zugehörigen Zeitplan (**Scheduled-StopVM**).|
|Sequenced-StopVM | 01:00 Uhr (UTC), jeden Freitag | Führt das Runbook **Sequenced_StopStop_Parent** mit dem Parameterwert `Stop` jeden Freitag zum angegebenen Zeitpunkt aus. Beendet der Reihe nach (in aufsteigender Reihenfolge) alle VMs mit dem in den jeweiligen Variablen definierten Tag **SequenceStop**. Weitere Informationen zu Tagwerten und Objektvariablen finden Sie unter [Runbooks](#runbooks). Aktivieren Sie den dazugehörigen Zeitplan (**Sequenced-StartVM**).|
|Sequenced-StartVM | 13:00 Uhr (UTC), jeden Montag | Führt das Runbook **SequencedStopStart_Parent** mit dem Parameterwert `Start` jeden Montag zum angegebenen Zeitpunkt aus. Startet der Reihe nach (in absteigender Reihenfolge) alle VMs mit dem in den jeweiligen Variablen definierten Tag **SequenceStart**. Weitere Informationen zu Tagwerten und Variablenobjekten finden Sie unter [Runbooks](#runbooks). Aktivieren Sie den dazugehörigen Zeitplan (**Sequenced-StopVM**).

## <a name="use-of-the-solution-with-classic-vms"></a>Verwenden der Lösung mit klassischen VMs

Wenn Sie die Lösung **VMs außerhalb der Geschäftszeiten starten/beenden** für klassische VMs verwenden, verarbeitet Automation alle Ihre VMs sequenziell pro Clouddienst. Virtuelle Computer werden weiterhin parallel in verschiedenen Clouddiensten verarbeitet. 

Wenn Sie diese Lösung mit klassischen VMs verwenden möchten, benötigen Sie ein klassisches ausführendes Konto. Dieses wird nicht standardmäßig erstellt. Anweisungen zum Erstellen eines klassischen ausführenden Kontos finden Sie unter [Erstellen klassischer ausführender Konten](automation-create-standalone-account.md#create-a-classic-run-as-account).

Wenn Sie über mehr als 20 VMs pro Clouddienst verfügen, beachten Sie folgende Empfehlungen:

* Erstellen Sie mehrere Zeitpläne mit dem übergeordneten Runbook **ScheduledStartStop_Parent**, und geben Sie pro Zeitplan 20 VMs an. 
* Geben Sie die VM-Namen in den Zeitplaneigenschaften mithilfe des Parameters `VMList` als durch Trennzeichen getrennte Liste an. 

Wenn andernfalls der Automatisierungsauftrag für diese Lösung mehr als drei Stunden ausgeführt wird, wird er gemäß dem Limit für [gleichmäßige Verteilung](automation-runbook-execution.md#fair-share) vorübergehend entladen oder angehalten.

Azure CSP-Abonnements unterstützen nur das Azure Resource Manager-Modell. Dienste, die nicht auf Azure Resource Manager basieren, sind in diesem Programm nicht verfügbar. Wenn die Lösung **VMs außerhalb der Geschäftszeiten starten/beenden** ausgeführt wird, werden möglicherweise Fehler ausgegeben, da sie Cmdlets für die Verwaltung klassischer Ressourcen enthält. Weitere Informationen zu CSP finden Sie unter [Verfügbare Dienste in CSP-Abonnements](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-available-services). Wenn Sie ein CSP-Abonnement verwenden, sollten Sie die Variable [External_EnableClassicVMs](#variables) nach der Bereitstellung auf FALSE festlegen.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="enable-the-solution"></a>Aktivieren der Lösung

Um mit der Verwendung der Lösung zu beginnen, führen Sie die Schritte in [Aktivieren der Lösung zum Starten/Beenden von VMs](automation-solution-vm-management-enable.md) aus.

## <a name="view-the-solution"></a>Anzeigen der Lösung

Sie können über eine der folgenden Vorgehensweisen auf die Lösung zugreifen, nachdem Sie sie aktiviert haben:

* Wählen Sie in Ihrem Automation-Konto unter **Verwandtes Ressourcen** die Option **VM starten/beenden** aus. Wählen Sie auf der Seite „VM starten/beenden“ auf der rechten Seite unter **Lösungen zum Starten/Beenden von VMs verwalten** die Option **Lösung verwalten** aus.

* Navigieren Sie zu dem Log Analytics-Arbeitsbereich, der mit Ihrem Automation-Konto verknüpft ist. Nachdem Sie den Arbeitsbereich ausgewählt haben, wählen Sie im linken Bereich **Lösungen** aus. Wählen Sie auf der Seite „Lösungen“ in der Liste die Lösung **Start-Stop-VM[Arbeitsbereich]** aus.  

Nach dem Auswählen der Lösung wird die Lösungsseite **Start-Stop-VM[Arbeitsbereich]** angezeigt. Hier können Sie wichtige Details überprüfen, z. B. die Informationen auf der Kachel **StartStopVM**. Wie im Log Analytics-Arbeitsbereich auch, werden auf dieser Kachel die Anzahl und eine grafische Darstellung der Runbookaufträge für die Lösung angezeigt, die gestartet und erfolgreich abgeschlossen wurden.

![Seite der Automation-Lösung „Updateverwaltung“](media/automation-solution-vm-management/azure-portal-vmupdate-solution-01.png)

Sie können eine weitergehende Analyse der Auftragsdatensätze durchführen, indem Sie auf die Ringkachel klicken. Im Lösungsdashboard werden der Auftragsverlauf und vordefinierte Protokollsuchabfragen angezeigt. Wechseln Sie zum erweiterten Log Analytics-Portal, um basierend auf Ihren Suchabfragen eine Suche durchzuführen.

## <a name="update-the-solution"></a>Aktualisieren der Lösung

Wenn Sie eine frühere Version dieser Lösung bereitgestellt haben, müssen Sie sie vor der Bereitstellung eines aktualisierten Releases aus Ihrem Konto löschen. Führen Sie dazu die Schritte zum [Entfernen der Lösung](#remove-the-solution) und anschließend die Schritte zum [Bereitstellen der Lösung](automation-solution-vm-management-enable.md) aus.

## <a name="remove-the-solution"></a>Entfernen der Lösung

Wenn Sie die Lösung nicht mehr benötigen, können Sie sie aus dem Automation-Konto löschen. Beim Löschen der Lösung werden nur die Runbooks entfernt. Die Zeitpläne oder Variablen, die beim Hinzufügen der Lösung erstellt wurden, werden nicht gelöscht. Entfernen Sie diese Ressourcen manuell, wenn Sie sie nicht mit anderen Runbooks verwenden.

So löschen Sie die Lösung

1. Wählen Sie in Ihrem Automation-Konto unter **Verwandte Ressourcen** die Option **Verknüpfter Arbeitsbereich** aus.

2. Klicken Sie auf **Zu Arbeitsbereich wechseln**.

3. Klicken Sie unter **Allgemein** auf **Lösungen**. 

4. Wählen Sie auf der Seite „Lösungen“ die Lösung **Start-Stop-VM[Arbeitsbereich]** aus. 

5. Wählen Sie auf der Seite **VMManagementSolution[Arbeitsbereich]** im Menü die Option **Löschen** aus.<br><br> ![Löschen der VM-Mgmt-Lösung](media/automation-solution-vm-management/vm-management-solution-delete.png)

6. Bestätigen Sie im Fenster **Lösung löschen**, dass Sie die Lösung löschen möchten.

7. Während die Informationen überprüft werden und die Lösung gelöscht wird, können Sie den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen. Nachdem der Vorgang zum Löschen der Lösung begonnen hat, kehren Sie zur Seite „Lösungen“ zurück.

Das Automation-Konto und der Log Analytics-Arbeitsbereich werden bei diesem Vorgang nicht gelöscht. Wenn Sie den Log Analytics-Arbeitsbereich nicht beibehalten möchten, müssen Sie ihn manuell im Azure-Portal löschen:

1. Suchen Sie nach **Log Analytics-Arbeitsbereiche**, und wählen Sie diese Option aus.

2. Wählen Sie auf der Seite „Log Analytics-Arbeitsbereich“ den Arbeitsbereich aus.

3. Wählen Sie auf der Seite mit den Einstellungen für den Arbeitsbereich die Menüoption **Löschen** aus.

4. Wenn Sie die [Lösungskomponenten](#solution-components) des Azure Automation-Kontos nicht behalten möchten, können Sie sie manuell löschen.

## <a name="next-steps"></a>Nächste Schritte

[Aktivieren](automation-solution-vm-management-enable.md) Sie die Lösung **VMs außerhalb der Geschäftszeiten starten/beenden** für Ihre Azure-VMs.
