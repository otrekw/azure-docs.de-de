---
title: Aktualisieren von Azure PowerShell-Modulen in Azure Automation
description: In diesem Artikel wird beschrieben, wie Sie jetzt häufig verwendete Azure PowerShell-Module, die standardmäßig in Azure Automation bereitgestellt werden, aktualisieren können.
services: automation
ms.subservice: process-automation
ms.date: 06/14/2019
ms.topic: conceptual
ms.openlocfilehash: bb73b3d644e96f9596f887faaf62eb15f01956ab
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2020
ms.locfileid: "81769669"
---
# <a name="update-azure-powershell-modules-in-azure-automation"></a>Aktualisieren von Azure PowerShell-Modulen in Azure Automation

Zum Aktualisieren der Azure-Module in Ihrem Automation-Konto müssen Sie das [Runbook „Update Azure Modules“](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) verwenden, das als Open Source verfügbar ist. Um in die Verwendung des **Update-AutomationAzureModulesForAccount**-Runbooks zum Aktualisieren Ihrer Azure-Module einzusteigen, laden Sie es aus dem [Update Azure Modules-Runbook-Repository](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) auf GitHub herunter. Anschließend können Sie es in Ihr Automation-Konto importieren oder es als Skript ausführen. Informationen zum Importieren eines Runbooks in Ihr Automation-Konto finden Sie unter [Importieren eines Runbooks](manage-runbooks.md#importing-a-runbook).

Die am häufigsten verwendeten PowerShell-Module werden standardmäßig in jedem Automation-Konto bereitgestellt. Das Azure-Team aktualisiert die Azure-Module regelmäßig. Daher sollten Sie das Runbook [Update-AutomationAzureModulesForAccount](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) verwenden, um die Module in Ihren Automation-Konten auf dem neuesten Stand zu halten.

Da Module von der Produktgruppe regelmäßig aktualisiert werden, können in den enthaltenen Cmdlets Änderungen auftreten. Diese Änderungen, z. B. das Umbenennen eines Parameters oder das vollständige Einstellen eines Cmdlets, können nachteilige Auswirkungen auf Ihre Runbooks haben.

Um Auswirkungen auf Ihre Runbooks und die Prozesse zu vermeiden, die sie automatisieren, führen Sie vor dem Fortsetzen des Vorgangs Tests und Überprüfungen aus. Wenn Sie für diesen Zweck kein dediziertes Automation-Konto haben, sollten Sie eines erstellen, damit Sie während der Entwicklung Ihrer Runbooks viele verschiedene Szenarios testen können. Diese Tests sollten iterative Änderungen wie das Aktualisieren der PowerShell-Module einschließen.

Wenn Sie Ihre Skripts lokal entwickeln, wird empfohlen, lokal die gleichen Modulversionen wie in Ihrem Automation-Konto beim Testen zur Verfügung zu haben, um sicherzustellen, dass Sie die gleichen Ergebnisse erhalten. Nach dem Überprüfen der Ergebnisse und dem Anwenden aller erforderlichen Änderungen können Sie die Änderungen in die Produktion übernehmen.

> [!NOTE]
> Ein neues Automation-Konto enthält ggf. nicht die neuesten Module.

> [!NOTE]
> Sie können keine globalen Module löschen, also Module, die im Lieferumfang der Automatisierung bereitstellt werden.

>[!NOTE]
>Dieser Artikel wurde aktualisiert und beinhaltet jetzt das neue Az-Modul von Azure PowerShell. Sie können das AzureRM-Modul weiterhin verwenden, das bis mindestens Dezember 2020 weiterhin Fehlerbehebungen erhält. Weitere Informationen zum neuen Az-Modul und zur Kompatibilität mit AzureRM finden Sie unter [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Einführung in das neue Az-Modul von Azure PowerShell). Installationsanweisungen für das Az-Modul auf Ihrem Hybrid Runbook Worker finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). 

## <a name="considerations"></a>Überlegungen

Die folgenden Überlegungen sollten bei der Anwendung dieses Artikels zum Aktualisieren Ihrer Azure-Module berücksichtigt werden:

* Das in diesem Artikel beschriebene Runbook unterstützt standardmäßig die Aktualisierung der Azure-, AzureRM- und Az-Module. Weitere Informationen zum Aktualisieren von Az.Automation-Modulen mit diesem Runbook finden Sie in der [README-Datei zum Runbook zum Aktualisieren von Azure-Modulen](https://github.com/microsoft/AzureAutomation-Account-Modules-Update/blob/master/README.md). Es gibt weitere wichtige Faktoren, die Sie bei der Verwendung der Az-Module in Ihrem Automation-Konto berücksichtigen müssen. Weitere Informationen finden Sie unter [Verwalten von Modulen in Azure Automation](shared-resources/modules.md).

* Bevor Sie dieses Runbook starten, vergewissern Sie sich, dass für Ihr Automation-Konto [Azure Run As-Kontoanmeldeinformationen](manage-runas-account.md) erstellt wurden.

* Sie können diesen Code als reguläres PowerShell-Skript anstelle eines Runbooks verwenden. Melden Sie sich einfach zuerst mit dem Cmdlet [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-3.7.0) bei Azure an, und übergeben Sie dann `-Login $false` an das Skript.

* Zur Ausführung dieses Runbooks in Sovereign Clouds verwenden Sie den `AzEnvironment`-Parameter, um die richtige Umgebung an das Runbook zu übergeben.  Zulässige Werte sind „AzureCloud“ (öffentliche Azure-Cloud), „AzureChinaCloud“, „AzureGermanCloud“ und „AzureUSGovernment“. Diese Werte können mithilfe von `Get-AzEnvironment | select Name` abgerufen werden. Wenn Sie keinen Wert an dieses Cmdlet übergeben, wird das Runbook standardmäßig auf AzureCloud festgelegt.

* Falls Sie eine bestimmte Azure PowerShell-Modulversion anstelle des neuesten im PowerShell-Katalog verfügbaren Moduls verwenden möchten, übergeben Sie diese Versionen im optionalen Parameter `ModuleVersionOverrides` des **Update-AutomationAzureModulesForAccount**-Runbooks. Beispiele finden Sie im [Update-AutomationAzureModulesForAccount.ps1](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update/blob/master/Update-AutomationAzureModulesForAccount.ps1
)-Runbook. Azure PowerShell-Module, die im Parameter `ModuleVersionOverrides` nicht erwähnt sind, werden mit den aktuellen Modulversionen aus dem PowerShell-Katalog aktualisiert. Wenn Sie keine Argumente an den Parameter `ModuleVersionOverrides` übergeben, werden alle Module mit den aktuellen Modulversionen aus dem PowerShell-Katalog aktualisiert. Dieses Verhalten entspricht dem der Schaltfläche **Azure-Module aktualisieren**.

## <a name="next-steps"></a>Nächste Schritte

Besuchen Sie die Seite [Update Azure Modules-Runbook](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update), um mehr darüber zu erfahren.
