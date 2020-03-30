---
title: Azure Automation – Häufig gestellte Fragen | Microsoft-Dokumentation
description: Hier finden Sie Antworten auf häufig gestellte Fragen zu Azure Automation.
services: automation
ms.subservice: ''
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 02/25/2020
ms.openlocfilehash: 129a5316c2e7be329b479c79706791e993d20b74
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77921757"
---
# <a name="azure-automation-frequently-asked-questions"></a>Häufig gestellte Fragen zu Azure Automation

Dieser Microsoft-Artikel enthält eine Liste häufig gestellter Fragen zu Azure Automation. Wenn Sie weiteren Fragen zu dessen Funktionen haben, besuchen Sie das Diskussionsforum, und stellen Sie Ihre Fragen. Wenn eine Frage häufiger gestellt wird, fügen wir sie diesem Artikel hinzu, damit sie schnell und einfach gefunden werden kann.

## <a name="update-management-solution"></a>Lösung für die Updateverwaltung

### <a name="can-i-prevent-unexpected-os-level-upgrades"></a>Kann ich unerwartete Upgrades auf Betriebssystemebene verhindern?

Bei einigen Linux-Varianten (z. B. Red Hat Enterprise Linux) werden unter Umständen Upgrades auf Betriebssystemebene über Pakete durchgeführt. Dies kann ggf. zur Ausführung der Updateverwaltung führen, wodurch sich die Versionsnummer des Betriebssystems ändert. Dieses Verhalten ist beabsichtigt, da die Updateverwaltung die gleichen Methoden für Updatepakete verwendet, die auch ein Administrator lokal auf dem Linux-Computer nutzt.

Um zu vermeiden, dass die Betriebssystemversion durch Updateverwaltungsbereitstellungen aktualisiert wird, verwenden Sie das Feature **Ausschluss**.

In Red Hat Enterprise Linux lautet der Name des auszuschließenden Pakets „redhat-release-server.x86_64“.

### <a name="why-arent-criticalsecurity-updates-applied"></a>Warum werden kritische Updates/Sicherheitsupdates nicht angewendet?

Beim Bereitstellen von Updates für einen Linux-Computer können Sie Updateklassifizierungen auswählen. Durch diese Option werden die auf den Computer angewendeten Updates gefiltert, die die definierten Kriterien erfüllen. Dieser Filter wird lokal auf dem Computer angewendet, wenn das Update bereitgestellt wird.

Da die Updateverwaltung die Updateanreicherung in der Cloud durchführt, werden einige Updates in der Updateverwaltung möglicherweise als Update mit Sicherheitsauswirkung gekennzeichnet, obwohl der lokale Computer nicht über diese Information verfügt. Daher gibt es beim Anwenden kritischer Updates auf einem Linux-Computer möglicherweise Updates, die für diesen Computer nicht als Update mit Sicherheitsauswirkung gekennzeichnet sind und somit nicht angewendet werden. Es kann allerdings sein, dass der Computer von der Updateverwaltung weiterhin als nicht konform gemeldet wird, da sie über zusätzliche Informationen zum relevanten Update verfügt.

Das Bereitstellen von Updates nach Updateklassifizierung funktioniert unter RTM-Versionen von CentOS nicht. Wählen Sie zum ordnungsgemäßen Bereitstellen von Updates für CentOS alle Klassifizierungen aus, um sicherzustellen, dass die Updates angewendet werden. Wenn für SUSE als Klassifizierung *nur* die Option **Weitere Updates** ausgewählt wird, kann dies dazu führen, dass auch einige Sicherheitsupdates installiert werden, falls zuerst Sicherheitsupdates im Zusammenhang mit zypper (Paket-Manager) oder dessen Abhängigkeiten erforderlich sind. Dieses Verhalten ist eine Einschränkung von zypper. In einigen Fällen muss die Updatebereitstellung ggf. erneut ausgeführt werden. Überprüfen Sie das Updateprotokoll.

### <a name="can-i-deploy-updates-across-azure-tenants"></a>Kann ich Updates über Azure-Mandanten hinweg bereitstellen?

Wenn Sie Computer patchen müssen, die einem anderen Azure-Mandanten angehören, der der Updateverwaltung unterliegt, müssen Sie wie folgt vorgehen, um sie in die Planung aufzunehmen. Verwenden Sie das Cmdlet [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) mit dem Schalter `-ForUpdate`, um einen Zeitplan zu erstellen, und das Cmdlet [New-AzureRmAutomationSoftwareUpdateConfiguration](/powershell/module/azurerm.automation/new-azurermautomationsoftwareupdateconfiguration
), um die Computer aus dem anderen Mandanten an den Parameter `-NonAzureComputer` zu übergeben. Dies wird anhand des folgenden Beispiels veranschaulicht:

```azurepowershell-interactive
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$sched = New-AzureRmAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdate

New-AzureRmAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName <automationAccountName> -Schedule $sched -Windows -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihre Frage hier nicht beantwortet wurde, finden Sie in den folgenden Foren weitere Fragen und Antworten.

- [Azure Automation](https://social.msdn.microsoft.com/Forums/home?forum=azureautomation&filter=alltypes&sort=lastpostdesc)

Allgemeines Feedback zur Lösung für die Updateverwaltung finden Sie im [Feedbackforum](https://feedback.azure.com/forums/905242-update-management).