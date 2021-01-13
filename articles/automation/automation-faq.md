---
title: Azure Automation – Häufig gestellte Fragen | Microsoft-Dokumentation
description: Dieser Artikel enthält Antworten auf häufig gestellte Fragen zu Azure Automation.
services: automation
ms.subservice: ''
ms.topic: conceptual
ms.date: 12/17/2020
ms.openlocfilehash: 2b40cc3d4cea4476ffde8bee8cec694975eb5083
ms.sourcegitcommit: a4533b9d3d4cd6bb6faf92dd91c2c3e1f98ab86a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/22/2020
ms.locfileid: "97724271"
---
# <a name="azure-automation-frequently-asked-questions"></a>Häufig gestellte Fragen zu Azure Automation

Dieser Microsoft-Artikel enthält eine Liste häufig gestellter Fragen zu Azure Automation. Wenn Sie andere Fragen zu dessen Funktionen haben, besuchen Sie das Diskussionsforum, und stellen Sie Ihre Fragen. Wenn eine Frage häufiger gestellt wird, fügen wir sie diesem Artikel hinzu, damit sie schnell und einfach gefunden werden kann.

## <a name="update-management"></a>Updateverwaltung

### <a name="can-i-prevent-unexpected-os-level-upgrades"></a>Kann ich unerwartete Upgrades auf Betriebssystemebene verhindern?

Bei einigen Linux-Varianten (z. B. Red Hat Enterprise Linux) werden unter Umständen Upgrades auf Betriebssystemebene über Pakete durchgeführt. Dies kann ggf. zur Ausführung der Updateverwaltung und so zu einer Änderung der Versionsnummer des Betriebssystems führen. Dieses Verhalten ist beabsichtigt, da die Updateverwaltung die gleichen Methoden für Updatepakete verwendet, die auch ein Administrator lokal auf dem Linux-Computer nutzt.

Um zu vermeiden, dass die Betriebssystemversion durch Updateverwaltungsbereitstellungen aktualisiert wird, verwenden Sie das Feature **Ausschluss**.

In Red Hat Enterprise Linux lautet der Name des auszuschließenden Pakets `redhat-release-server.x86_64`.

### <a name="why-arent-criticalsecurity-updates-applied"></a>Warum werden kritische Updates/Sicherheitsupdates nicht angewendet?

Beim Bereitstellen von Updates für einen Linux-Computer können Sie Updateklassifizierungen auswählen. Durch diese Option werden die Updates gefiltert, die die definierten Kriterien erfüllen. Dieser Filter wird lokal auf dem Computer angewendet, wenn das Update bereitgestellt wird.

Da die Updateverwaltung die Updateanreicherung in der Cloud durchführt, können Sie einige Updates in der Updateverwaltung als Update mit Sicherheitsauswirkung kennzeichnen, auch wenn der lokale Computer nicht über diese Informationen verfügt. Es kann beim Anwenden kritischer Updates auf einem Linux-Computer möglicherweise Updates geben, die für diesen Computer nicht als Update mit Sicherheitsauswirkung gekennzeichnet sind und somit nicht angewandt werden. Es kann allerdings sein, dass der Computer von der Updateverwaltung weiterhin als nicht konform gemeldet wird, da sie über zusätzliche Informationen zum relevanten Update verfügt.

Das Bereitstellen von Updates nach Updateklassifizierung funktioniert unter RTM-Versionen von CentOS nicht. Wählen Sie zum ordnungsgemäßen Bereitstellen von Updates für CentOS alle Klassifizierungen aus, um sicherzustellen, dass die Updates angewendet werden. Wenn für SUSE als Klassifizierung NUR die Option **Weitere Updates** ausgewählt wird, kann dies dazu führen, dass durch die Klassifizierung andere Sicherheitsupdates installiert werden, wenn diese mit zypper (Paket-Manager) im Zusammenhang stehen oder dessen Abhängigkeiten zuerst erforderlich sind. Dieses Verhalten ist eine Einschränkung von zypper. In einigen Fällen müssen Sie die Updatebereitstellung u. U. erneut ausführen und dann die Bereitstellung über das Updateprotokoll überprüfen.

### <a name="can-i-deploy-updates-across-azure-tenants"></a>Kann ich Updates über Azure-Mandanten hinweg bereitstellen?

Wenn Sie Computer in einem anderen Azure-Mandanten patchen müssen, der der Updateverwaltung unterliegt, müssen Sie wie folgt vorgehen, um sie in die Planung aufzunehmen. Sie können das Cmdlet [New-AzAutomationSchedule](/powershell/module/Az.Automation/New-AzAutomationSchedule) mit dem Parameter `ForUpdateConfiguration` verwenden, um einen Zeitplan zu erstellen. Sie können das Cmdlet [New-AzAutomationSoftwareUpdateConfiguration](/powershell/module/Az.Automation/New-AzAutomationSoftwareUpdateConfiguration) verwenden und die Computer im anderen Mandanten an den Parameter `NonAzureComputer` übergeben. Das folgende Beispiel zeigt die erforderliche Vorgehensweise.

```azurepowershell-interactive
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$sched = New-AzAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdateConfiguration

New-AzAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName <automationAccountName> -Schedule $sched -Windows -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

## <a name="process-automation---python-runbooks"></a>Prozessautomatisierung: Python-Runbooks

### <a name="which-python-3-version-is-supported-in-azure-automation"></a>Welche Python 3-Version wird in Azure Automation unterstützt?

Für Cloudaufträge wird Python 3.8 unterstützt. Skripts und Pakete aus einer beliebigen 3.x-Version funktionieren möglicherweise, wenn der Code mit mehreren Versionen kompatibel ist.

Für Hybridaufträge auf Windows Hybrid Runbook Workers können Sie jede beliebige 3.x-Version installieren, die Sie verwenden möchten. Für Hybridaufträge auf Linux Hybrid Runbook Workers ist die auf dem Computer installierte Version von Python 3 zum Ausführen von DSC OMSConfig und der Linux Hybrid Worker erforderlich. Es wird empfohlen, Version 3.6 zu installieren. Andere Versionen sollten jedoch ebenfalls funktionieren, wenn zwischen den Versionen von Python 3 keine Breaking Changes bei Methodensignaturen oder Verträgen aufgetreten sind.

### <a name="can-python-2-and-python-3-runbooks-run-in-same-automation-account"></a>Können Python 2- und Python 3-Runbooks im selben Automation-Konto ausgeführt werden?

Ja, es gibt keine Einschränkung für die Verwendung von Python 2- und Python 3-Runbooks im selben Automation-Konto.  

### <a name="what-is-the-plan-for-migrating-existing-python-2-runbooks-and-packages-to-python-3"></a>Wie sieht der Plan für die Migration vorhandener Python 2-Runbooks und -Pakete zu Python 3 aus?

Azure Automation plant nicht, Python 2-Runbooks und -Pakete zu Python 3 zu migrieren. Diese Migration müssen Sie selbst durchführen. Vorhandene und neue Python 2-Runbooks und -Pakete werden weiterhin funktionieren.

### <a name="what-are-the-packages-supported-by-default-in-python-3-environment"></a>Welche Pakete werden standardmäßig in der Python 3-Umgebung unterstützt?

Das Azure-Paket 4.0.0 wird standardmäßig in der Automation-Umgebung von Python 3 installiert. Sie können manuell eine höhere Version des Azure-Pakets importieren, um die Standardversion außer Kraft zu setzen.

### <a name="what-if-i-run-a-python-3-runbook-that-references-a-python-2-package-or-vice-versa"></a>Was geschieht, wenn ein Python 3-Runbook ausgeführt wird, das auf ein Python 2-Paket verweist oder umgekehrt?

Python 2 und Python 3 verfügen über unterschiedliche Ausführungsumgebungen. Während ein Python 2-Runbook ausgeführt wird, können nur Python 2-Pakete importiert werden. Für Python 3 gilt dasselbe Prinzip.

### <a name="how-do-i-differentiate-between-python-2-and-python-3-runbooks-and-packages"></a>Wie kann man zwischen Python 2- und Python 3-Runbooks und -Paketen unterscheiden?

Python 3 ist eine neue Runbookdefinition, die zwischen Python 2- und Python 3-Runbooks unterscheidet. Ebenso wird ein weiterer Pakettyp für Python 3-Pakete eingeführt.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihre Frage hier nicht beantwortet wurde, finden Sie in den folgenden Quellen weitere Fragen und Antworten.

- [Azure Automation](/answers/topics/azure-automation.html)
- [Feedbackforum](https://feedback.azure.com/forums/905242-update-management)
