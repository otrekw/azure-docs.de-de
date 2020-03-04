---
title: Fehlerbehandlung in grafischen Azure Automation-Runbooks
description: In diesem Artikel wird beschrieben, wie Sie die Fehlerbehandlungslogik in grafischen Azure Automation-Runbooks implementieren.
services: automation
ms.subservice: process-automation
ms.date: 03/16/2018
ms.topic: conceptual
ms.openlocfilehash: dec715ec6741f4429d8b1d4f620ef3cb82d4c1d3
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2020
ms.locfileid: "77649975"
---
# <a name="error-handling-in-azure-automation-graphical-runbooks"></a>Fehlerbehandlung in grafischen Azure Automation-Runbooks

Ein wichtiges Entwurfsprinzip, das Sie für Ihr grafisches Azure Automation-Runbook beachten sollten, ist das Ermitteln von Problemen, auf die das Runbook bei der Ausführung möglicherweise stößt. z. B. Erfolg, erwartete Fehlerzustände und unerwartete Fehlerbedingungen.

Wenn bei einer Runbookaktivität ein Fehler ohne Abbruch auftritt, verarbeitet Windows PowerShell die Aktivität häufig trotzdem durch Verarbeiten der Folgeaktivitäten unabhängig vom Fehler. Wahrscheinlich generiert der Fehler eine Ausnahme, aber die Ausführung der nächsten Aktivität wird dennoch zugelassen.

Ihr grafisches Runbook sollte Fehlerbehandlungscode für Ausführungsprobleme enthalten. Wenn Sie die Ausgabe einer Aktivität überprüfen oder einen Fehler behandeln möchten, können Sie eine PowerShell-Codeaktivität verwenden, eine bedingte Logik für den Ausgabelink der Aktivität definieren oder eine andere Methode anwenden.

Grafische Azure Automation-Runbooks wurden verbessert und um die Fehlerbehandlung erweitert. Sie können Ausnahmen jetzt in Fehler ohne Abbruch ändern und Fehlerlinks zwischen Aktivitäten erstellen. Mithilfe des verbesserten Verfahrens können Sie in Ihren Runbooks Fehler abfangen und erkannte bzw. unerwartete Bedingungen behandeln. 

>[!NOTE]
>Dieser Artikel wurde aktualisiert und beinhaltet jetzt das neue Az-Modul von Azure PowerShell. Sie können das AzureRM-Modul weiterhin verwenden, das bis mindestens Dezember 2020 weiterhin Fehlerbehebungen erhält. Weitere Informationen zum neuen Az-Modul und zur Kompatibilität mit AzureRM finden Sie unter [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Einführung in das neue Az-Modul von Azure PowerShell). Installationsanweisungen für das Az-Modul auf Ihrem Hybrid Runbook Worker finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). In Ihrem Automation-Konto können Sie die Module mithilfe der Informationen unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](automation-update-azure-modules.md) auf die neueste Version aktualisieren.

## <a name="powershell-error-types"></a>PowerShell-Fehlertypen

Während der Ausführung können PowerShell-Fehler mit oder ohne Abbruch auftreten.
 
### <a name="terminating-error"></a>Fehler mit Abbruch

Ein Fehler mit Abbruch ist ein ernster Fehler bei der Ausführung, durch den der Befehl bzw. die Skriptausführung vollständig unterbrochen wird. Beispiele hierfür sind nicht vorhandene Cmdlets, Syntaxfehler, die die Ausführung eines Cmdlets verhindern, und andere schwerwiegende Fehler.

### <a name="non-terminating-error"></a>Fehler ohne Abbruch

Ein Fehler ohne Abbruch ist ein nicht schwerwiegender Fehler, bei dem die Ausführung fortgesetzt werden kann. Beispiele hierfür sind Ablauffehler (z. B. eine nicht gefundene Datei) und Berechtigungsprobleme.

## <a name="when-to-use-error-handling"></a>Einsatzgebiete der Fehlerbehandlung

Verwenden Sie die Fehlerbehandlung in Ihrem Runbook, wenn eine kritische Aktivität einen Fehler oder eine Ausnahme auslöst. Es ist wichtig, die Verarbeitung der nächsten Aktivität im Runbook zu verhindern und den Fehler auf passende Weise zu behandeln. Die Behandlung des Fehlers ist besonders wichtig, wenn Sie mit Ihren Runbooks einen Geschäfts- oder Dienstprozess unterstützen.

Für jede Aktivität, die einen Fehler erzeugen kann, können Sie einen Fehlerlink zu einer anderen Aktivität hinzufügen. Die Zielaktivität kann einen beliebigen Typ aufweisen: Codeaktivitäten, Aufruf eines Cmdlets, Aufruf eines anderen Runbooks usw. Die Zielaktivität kann auch über ausgehende Links verfügen, bei denen es sich um normale oder Fehlerlinks handeln kann. Durch die Links kann im Runbook eine komplexe Fehlerbehandlungslogik implementiert werden, ohne eine Codeaktivität einbinden zu müssen.

Es wird empfohlen, ein dediziertes Runbook für die Fehlerbehandlung mit allgemeiner Funktionalität zu erstellen. Dies ist jedoch nicht zwingend erforderlich. Beispielsweise ist auch ein Runbook möglich, das versucht, einen virtuellen Computer zu starten und darauf eine Anwendung zu installieren. Wenn der virtuelle Computer nicht richtig gestartet wird, geschieht Folgendes:

1. Er sendet eine Benachrichtigung zu diesem Problem.
2. Er startet ein anderes Runbook, mit dem automatisch stattdessen eine neue VM bereitgestellt wird.

Eine Lösung besteht darin, im Runbook über einen Fehlerlink auf eine Aktivität zur Behandlung von Schritt 1 zu verweisen. Das Runbook kann z. B. das Cmdlet **Write-Warning** mit einer Aktivität für Schritt 2 verbinden – z. B. dem Cmdlet [Start-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/az.automation/start-azautomationrunbook?view=azps-3.5.0).

Sie können dieses Verhalten auch für die Verwendung in vielen Runbooks verallgemeinern, indem Sie diese beiden Aktivitäten in ein separates Runbook für die Fehlerbehandlung einfügen und dabei die zuvor beschriebenen Anweisungen befolgen. Bevor Ihr ursprüngliches Runbook diese Runbooks für die Fehlerbehandlung aufruft, kann es aus den Daten eine benutzerdefinierte Nachricht erstellen und diese als Parameter an das Runbook für die Fehlerbehandlung übergeben.

## <a name="how-to-use-error-handling"></a>Verwenden der Fehlerbehandlung

Jede Aktivität in Ihrem Runbook weist eine Konfigurationseinstellung auf, die Ausnahmen in Fehler ohne Abbruch umwandelt. Diese Einstellung ist standardmäßig deaktiviert. Es wird empfohlen, diese Einstellung für alle Aktivitäten zu aktivieren, bei denen das Runbook Fehler behandelt. Durch das Aktivieren dieser Konfiguration wird sichergestellt, dass sowohl Fehler mit als auch ohne Abbruch in der Aktivität über einen Fehlerlink als Fehler ohne Abbruch behandelt werden.  

Nach dem Aktivieren der Konfigurationseinstellung erstellen Sie in Ihrem Runbook eine Aktivität, die den Fehler behandelt. Wenn die Aktivität einen Fehler erzeugt, wird den ausgehenden Fehlerlinks gefolgt. Den regulären Links wird nicht gefolgt, selbst wenn die Aktivität auch eine reguläre Ausgabe erzeugt.<br><br> ![Fehlerlink für Automation-Runbook – Beispiel](media/automation-runbook-graphical-error-handling/error-link-example.png)

Im folgenden Beispiel ruft ein Runbook eine Variable ab, die den Computernamen einer VM enthält. Anschließend versucht es, die VM mit der nächsten Aktivität zu starten.<br><br> ![Beispiel für die Fehlerbehandlung mit einem Automation-Runbook](media/automation-runbook-graphical-error-handling/runbook-example-error-handling.png)<br><br>      

Die Aktivität **Get-AutomationVariable** und das Cmdlet [Start-AzVM](https://docs.microsoft.com/powershell/module/Az.Compute/Start-AzVM?view=azps-3.5.0) sind so konfiguriert, dass Ausnahmen in Fehler umgewandelt werden. Falls beim Abrufen der Variable oder beim Starten der VM Probleme auftreten, generiert der Code Fehler.<br><br> ![Aktivitätseinstellungen für die Fehlerbehandlung mit einem Automation-Runbook](media/automation-runbook-graphical-error-handling/activity-blade-convertexception-option.png)

Fehlerlinks zeigen von diesen Aktivitäten auf eine einzelne Codeaktivität zur **Fehlerverwaltung**. Diese ist mit einem einfachen PowerShell-Ausdruck konfiguriert, der das Schlüsselwort *Throw* verwendet, um die Verarbeitung zu beenden. Außerdem wird mit `$Error.Exception.Message` die Meldung abgerufen, in der die aktuelle Ausnahme beschrieben wird.<br><br> ![Codebeispiel für die Fehlerbehandlung mit einem Automation-Runbook](media/automation-runbook-graphical-error-handling/runbook-example-error-handling-code.png)

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Links und Linktypen in grafischen Runbooks finden Sie unter [Grafische Erstellung in Azure Automation](automation-graphical-authoring-intro.md#links-and-workflow).

* Weitere Informationen zum Ausführen von Runbooks und zum Überwachen von Runbookaufträgen sowie andere technische Details finden Sie unter [Ausführen von Runbooks in Azure Automation](automation-runbook-execution.md).