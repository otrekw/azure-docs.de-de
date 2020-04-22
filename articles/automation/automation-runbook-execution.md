---
title: Ausführen von Runbooks in Azure Automation
description: Beschreibt ausführlich, wie ein Runbook in Azure Automation verarbeitet wird.
services: automation
ms.subservice: process-automation
ms.date: 04/14/2020
ms.topic: conceptual
ms.openlocfilehash: a7dd9de1f2ae41b20d94cf31de48e92fbb71ca6a
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81405636"
---
# <a name="runbook-execution-in-azure-automation"></a>Ausführen von Runbooks in Azure Automation

Die Prozessautomatisierung in Azure Automation ermöglicht Ihnen das Erstellen und Verwalten von PowerShell-, PowerShell Workflow- und grafischen Runbooks. Einzelheiten finden Sie unter [Azure Automation-Runbooks](automation-runbook-types.md). 

Automation führt Ihre Runbooks auf der Grundlage der in ihnen definierten Logik aus. Wenn ein Runbook unterbrochen wird, startet es am Anfang neu. Dieses Verhalten erfordert, dass Sie Runbooks schreiben, die einen Neustart unterstützen, wenn vorübergehende Probleme auftreten.

Wenn Sie ein Runbook in Azure Automation starten, wird ein Auftrag erstellt, bei dem es sich um eine einzelne Ausführungsinstanz des Runbooks handelt. Jeder Auftrag erhält Zugriff auf Azure-Ressourcen, indem er eine Verbindung mit Ihrem Azure-Abonnement herstellt. Der Auftrag hat nur Zugriff auf Ressourcen in Ihrem Rechenzentrum, wenn aus der öffentlichen Cloud auf diese Ressourcen zugegriffen werden kann.

Azure Automation weist einen Worker zu, um jeden Auftrag während der Runbookausführung auszuführen. Wenngleich Worker von vielen Azure-Konten gemeinsam genutzt werden, sind die Aufträge von verschiedenen Automation-Konten voneinander isoliert. Sie können nicht steuern, welcher Workerdienst Ihre Auftragsanforderung verarbeitet.

Wenn Sie die Liste der Runbooks im Azure-Portal einsehen, wird der Status jedes Auftrags angezeigt, der für jedes Runbook gestartet wurde. Azure Automation speichert Auftragsprotokolle maximal 30 Tage.

Im folgenden Diagramm wird der Lebenszyklus eines Runbookauftrags für [PowerShell-Runbooks](automation-runbook-types.md#powershell-runbooks), [PowerShell-Workflow-Runbooks](automation-runbook-types.md#powershell-workflow-runbooks) und [grafische Runbooks](automation-runbook-types.md#graphical-runbooks) gezeigt.

![Auftragsstatus – PowerShell-Workflow](./media/automation-runbook-execution/job-statuses.png)

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

>[!NOTE]
>Dieser Artikel wurde aktualisiert und beinhaltet jetzt das neue Az-Modul von Azure PowerShell. Sie können das AzureRM-Modul weiterhin verwenden, das bis mindestens Dezember 2020 weiterhin Fehlerbehebungen erhält. Weitere Informationen zum neuen Az-Modul und zur Kompatibilität mit AzureRM finden Sie unter [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Einführung in das neue Az-Modul von Azure PowerShell). Installationsanweisungen für das Az-Modul auf Ihrem Hybrid Runbook Worker finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). In Ihrem Automation-Konto können Sie die Module mithilfe der Informationen unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](automation-update-azure-modules.md) auf die neueste Version aktualisieren.

## <a name="where-to-run-your-runbooks"></a>Ausführungsort Ihrer Runbooks

Runbooks in Azure Automation können entweder in einer Azure-Sandbox oder einem [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md) ausgeführt werden. Wenn Runbooks für das Authentifizieren und Ausführen von Ressourcen in Azure entworfen sind, werden sie in einer Azure-Sandbox ausgeführt, einer freigegebenen Umgebung, die von mehreren Aufträgen verwendet werden kann. Aufträge, die die gleiche Sandbox verwenden, werden durch die Ressourceneinschränkungen der Sandbox gebunden.

>[!NOTE]
>Interaktive Vorgänge werden von der Azure-Sandboxumgebung nicht unterstützt. Außerdem müssen lokale MOF-Dateien für Runbooks verwendet werden, die Win32-Aufrufe durchführen.

Sie können einen Hybrid Runbook Worker verwenden, um Runbooks direkt auf dem Computer, der die Rolle hostet, und mit lokalen Ressourcen in der Umgebung auszuführen. Azure Automation speichert und verwaltet Runbooks und übermittelt sie dann an einen oder mehrere zugewiesene Computer.

In der folgenden Tabelle sind einige Runbook-Ausführungsaufgaben mit der jeweils empfohlenen Ausführungsumgebung aufgelistet.

|Aufgabe|Empfehlung|Notizen|
|---|---|---|
|Integration in Azure-Ressourcen|Azure-Sandbox|Bei Hosting in Azure ist die Authentifizierung einfacher. Wenn Sie einen Hybrid Runbook Worker auf einer Azure-VM verwenden, können Sie [verwaltete Identitäten für Azure-Ressourcen](automation-hrw-run-runbooks.md#managed-identities-for-azure-resources) verwenden.|
|Optimale Leistung zum Verwalten von Azure-Ressourcen erzielen|Azure-Sandbox|Das Skript wird in der gleichen Umgebung ausgeführt, was zu weniger Latenz führt.|
|Betriebskosten minimieren|Azure-Sandbox|Kein Computemehraufwand, keine VM erforderlich.|
|Zeitintensives Skript ausführen|Hybrid Runbook Worker|Für Azure-Sandboxes gelten [Ressourcenlimits](../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits).|
|Interagieren mit lokalen Diensten|Hybrid Runbook Worker|Kann direkt auf den Hostcomputer oder auf Ressourcen in anderen Cloudumgebungen oder Ihrer lokalen Umgebung zugreifen. |
|Software und ausführbare Dateien von Drittanbietern erforderlich.|Hybrid Runbook Worker|Sie verwalten das Betriebssystem und können Software installieren.|
|Datei oder Ordner mit einem Runbook überwachen|Hybrid Runbook Worker|Wenden Sie einen [Watcher-Task](automation-watchers-tutorial.md) auf einen Hybrid Runbook Worker an.|
|Ressourcenintensives Skript ausführen|Hybrid Runbook Worker| Für Azure-Sandboxes gelten [Ressourcenlimits](../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits).|
|Module mit spezifischen Anforderungen verwenden| Hybrid Runbook Worker|Beispiele:</br> WinSCP: Abhängigkeit von „winscp.exe“ </br> IIS-Verwaltung: Abhängigkeit zum Aktivieren oder Verwalten von IIS.|
|Modul mit einem Installationsprogramm installieren|Hybrid Runbook Worker|Module für Sandbox müssen Kopieren unterstützen.|
|Verwenden von Runbooks oder Modulen, die eine andere .NET Framework-Version als 4.7.2 erfordern|Hybrid Runbook Worker|Automation-Sandboxes unterstützen .NET Framework 4.7.2; Upgrades auf eine andere Version werden nicht unterstützt.|
|Skripts ausführen, für die eine Rechteerweiterung erforderlich ist|Hybrid Runbook Worker|Sandboxes lassen keine Rechteerweiterung zu. Mit einem Hybrid Runbook Worker können Sie die Benutzerkontensteuerung deaktivieren und [Invoke-Command](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/invoke-command?view=powershell-7) verwenden, wenn Sie den Befehl ausführen, für den eine Rechteerweiterung erforderlich ist.|
|Ausführen von Skripts, die Zugriff auf die Windows-Verwaltungsinstrumentation (WMI) benötigen|Hybrid Runbook Worker|Aufträge, die in Sandboxes in der Cloud ausgeführt werden, haben keinen Zugriff auf WMI-Anbieter. |

## <a name="runbook-behavior"></a>Runbook-Verhalten

### <a name="creating-resources"></a>Erstellen von Ressourcen

Wenn Ihr Runbook eine Ressource erstellt, sollte das Skript prüfen, ob die Ressource bereits vorhanden ist, bevor versucht wird, sie erneut zu erstellen. Hier ist ein einfaches Beispiel.

```powershell
$vmName = "WindowsVM1"
$resourceGroupName = "myResourceGroup"
$myCred = Get-AutomationPSCredential "MyCredential"
$vmExists = Get-AzResource -Name $vmName -ResourceGroupName $resourceGroupName

if(!$vmExists)
    {
    Write-Output "VM $vmName does not exist, creating"
    New-AzVM -Name $vmName -ResourceGroupName $resourceGroupName -Credential $myCred
    }
else
    {
    Write-Output "VM $vmName already exists, skipping"
    }
```

### <a name="supporting-time-dependent-scripts"></a>Unterstützung zeitabhängiger Skripts

Ihre Runbooks müssen zuverlässig sein und mit vorübergehenden Fehlern umgehen können, die zu einem Neustart oder Ausfall führen können. Wenn ein Runbook ausfällt, versucht Azure Automation die erneute Ausführung.

Wenn Ihr Runbook normalerweise innerhalb einer bestimmten Zeitspanne ausgeführt wird, lassen Sie die Implementierungslogik des Skripts die Ausführungszeit überprüfen. Diese Überprüfung stellt sicher, dass Vorgänge wie Starten, Herunterfahren oder Aufskalieren nur zu bestimmten Zeiten erfolgen.

> [!NOTE]
> Die Ortszeit des Azure-Sandboxprozesses ist auf UTC festgelegt. Berechnungen für Datum und Uhrzeit in Ihren Runbooks müssen dies berücksichtigen.

### <a name="tracking-progress"></a>Nachverfolgung des Verlaufs

Es empfiehlt sich, Runbooks so zu schreiben, dass sie modular aufgebaut sind und eine Logik aufweisen, die problemlos wiederverwendet und neu gestartet werden kann. Die Nachverfolgung des Verlaufs in einem Runbook ist eine gute Möglichkeit sicherzustellen, dass das Runbook ordnungsgemäß ausgeführt wird, falls Probleme auftreten. Der Fortschritt eines Runbooks lässt sich mithilfe einer externen Quelle, wie z. B. einem Speicherkonto, einer Datenbank oder gemeinsam genutzten Dateien, verfolgen. Sie können Logik in Ihrem Runbook erstellen, um zunächst den Status der letzten Aktion zu überprüfen. Dann kann die Logik auf Grundlage der Ergebnisse der Überprüfung bestimmte Aufgaben im Runbook entweder überspringen oder fortsetzen.

### <a name="preventing-concurrent-jobs"></a>Verhindern gleichzeitiger Aufträge

Einige Runbooks verhalten sich möglicherweise merkwürdig, wenn sie in mehreren Aufträgen gleichzeitig ausgeführt werden. In diesem Fall ist es wichtig, dass ein Runbook Logik implementiert, um festzustellen, ob bereits ein laufender Auftrag vorhanden ist. Hier ist ein einfaches Beispiel.

```powershell
# Authenticate to Azure
$connection = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzAccount -ServicePrincipal -Tenant $connection.TenantID `
-ApplicationId $connection.ApplicationID -CertificateThumbprint $connection.CertificateThumbprint

$AzContext = Select-AzSubscription -SubscriptionId $connection.SubscriptionID

# Check for already running or new runbooks
$runbookName = "<RunbookName>"
$rgName = "<ResourceGroupName>"
$aaName = "<AutomationAccountName>"
$jobs = Get-AzAutomationJob -ResourceGroupName $rgName -AutomationAccountName $aaName -RunbookName $runbookName -AzContext $AzureContext

# Check to see if it is already running
$runningCount = ($jobs | ? {$_.Status -eq "Running"}).count

If (($jobs.status -contains "Running" -And $runningCount -gt 1 ) -Or ($jobs.Status -eq "New")) {
    # Exit code
    Write-Output "Runbook is already running"
    Exit 1
} else {
    # Insert Your code here
}
```

### <a name="working-with-multiple-subscriptions"></a>Verwenden mehrerer Abonnements

Für den Umgang mit mehreren Abonnements muss Ihr Runbook das Cmdlet [Disable-AzContextAutosave](https://docs.microsoft.com/powershell/module/Az.Accounts/Disable-AzContextAutosave?view=azps-3.5.0) verwenden. Mit diesem Cmdlet wird sichergestellt, dass der Authentifizierungskontext nicht von einem anderen Runbook abgerufen wird, das in derselben Sandbox ausgeführt wird. Das Runbook verwendet auch den Parameter `AzContext` für die Cmdlets des Az-Moduls und übergibt dafür den richtigen Kontext.

```powershell
# Ensures that you do not inherit an AzContext in your runbook
Disable-AzContextAutosave –Scope Process

$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzAccount -ServicePrincipal `
-Tenant $Conn.TenantID `
-ApplicationId $Conn.ApplicationID `
-CertificateThumbprint $Conn.CertificateThumbprint

$context = Get-AzContext

$ChildRunbookName = 'ChildRunbookDemo'
$AutomationAccountName = 'myAutomationAccount'
$ResourceGroupName = 'myResourceGroup'

Start-AzAutomationRunbook `
    -ResourceGroupName $ResourceGroupName `
    -AutomationAccountName $AutomationAccountName `
    -Name $ChildRunbookName `
    -DefaultProfile $context
```

### <a name="handling-exceptions"></a>Behandeln von Ausnahmen

In diesem Abschnitt werden einige Möglichkeiten zur Behandlung von Ausnahmen oder zeitweiligen Problemen in Ihren Runbooks vorgestellt.

#### <a name="erroractionpreference"></a>ErrorActionPreference

Die Variable [ErrorActionPreference](/powershell/module/microsoft.powershell.core/about/about_preference_variables#erroractionpreference) gibt an, wie PowerShell auf einen Fehler ohne Abbruch reagiert. Fehler mit Abbruch führen immer zu einer Beendigung und sind nicht von `ErrorActionPreference` betroffen.

Bei Verwendung von `ErrorActionPreference` für das Runbook verhindert ein Fehler, der normalerweise nicht zu einem Abbruch führt (z. B. `PathNotFound` im Cmdlet [Get-ChildItem](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-childitem?view=powershell-7)), dass die Ausführung des Runbooks abgeschlossen wird. Im folgenden Beispiel wird die Verwendung von `ErrorActionPreference` veranschaulicht. Der abschließende Befehl [Write-Output](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/write-output?view=powershell-7) wird nie ausgeführt, da das Skript beendet wird.

```powershell-interactive
$ErrorActionPreference = 'Stop'
Get-ChildItem -path nofile.txt
Write-Output "This message will not show"
```

#### <a name="try-catch-finally"></a>Try Catch Finally

[Try Catch Finally](/powershell/module/microsoft.powershell.core/about/about_try_catch_finally) wird in PowerShell-Skripts verwendet, um Fehler mit Abbruch zu behandeln. Das Skript kann diesen Mechanismus nutzen, um spezifische oder allgemeine Ausnahmen abzufangen. Die `catch`-Anweisung sollte zum Nachverfolgen oder Behandeln von Fehlern verwendet werden. Im folgenden Beispiel wird versucht, eine Datei herunterzuladen, die es nicht gibt. Die Ausnahme `System.Net.WebException` wird abgefangen, und bei anderen Ausnahmen wird der letzte Wert zurückgegeben.

```powershell-interactive
try
{
   $wc = new-object System.Net.WebClient
   $wc.DownloadFile("http://www.contoso.com/MyDoc.doc")
}
catch [System.Net.WebException]
{
    "Unable to download MyDoc.doc from http://www.contoso.com."
}
catch
{
    "An error occurred that could not be resolved."
}
```

#### <a name="throw"></a>Throw

Mit [Throw](/powershell/module/microsoft.powershell.core/about/about_throw) können Sie einen Fehler mit Abbruch generieren. Dies kann nützlich sein, wenn Sie in einem Runbook eigene Logik definieren. Wenn das Skript ein Kriterium für die Beendigung erfüllt, kann es zum Beenden die `throw`-Anweisung verwenden. Das folgende Beispiel verwendet diese Anweisung, um einen erforderlichen Funktionsparameter zu zeigen.

```powershell-interactive
function Get-ContosoFiles
{
  param ($path = $(throw "The Path parameter is required."))
  Get-ChildItem -Path $path\*.txt -recurse
}
```

### <a name="using-executables-or-calling-processes"></a>Verwenden von ausführbaren Dateien oder Aufrufen von Prozessen

In Azure-Sandboxes ausgeführte Runbooks unterstützen keine aufrufenden Prozesse, wie z. B. ausführbare Dateien (**EXE**-Dateien) oder Unterprozesse. Der Grund dafür ist, dass es sich bei einer Azure-Sandbox um einen gemeinsam genutzten Prozess handelt, der in einem Container ausgeführt wird und möglicherweise nicht auf alle zugrunde liegenden APIs Zugriff hat. Für Szenarien, die Software von Drittanbietern oder Aufrufe von Unterprozessen erfordern, sollten Sie das Runbook in einem [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md) ausführen.

### <a name="accessing-device-and-application-characteristics"></a>Zugreifen auf Geräte- und Anwendungsmerkmale

Runbookaufträge, die in Azure-Sandboxes ausgeführt werden, haben keinen Zugriff auf Geräte- oder Anwendungsmerkmale. Die am häufigsten verwendete API zur Abfrage von Leistungsmetriken unter Windows ist WMI, wobei einige der gängigen Metriken die Arbeitsspeicher- und CPU-Auslastung sind. Es spielt jedoch keine Rolle, welche API verwendet wird, da in der Cloud ausgeführte Aufträge keinen Zugriff auf die Microsoft-Implementierung von Web-Based Enterprise Management (WBEM) haben. Diese Plattform beruht auf dem Common Information Model (CIM), das die Branchenstandards für die Definition von Geräte- und Anwendungseigenschaften vorgibt.

## <a name="handling-errors"></a>Behandlung von Fehlern

Ihre Runbooks müssen in der Lage sein, Fehler zu behandeln. PowerShell unterscheidet zwei Arten von Fehlern: Fehler mit Abbruch und Fehler ohne Abbruch. Bei Fehlern mit Abbruch wird die Runbookausführung beendet, sobald sie auftreten. Das Runbook wird mit dem Auftragsstatus „Fehler“ angehalten.

Fehler ohne Abbruch ermöglichen einem Skript, auch nach ihrem Auftreten fortzufahren. Ein Beispiel für einen Fehler ohne Abbruch ist ein Fehler, der auftritt, wenn ein Runbook das Cmdlet `Get-ChildItem` mit einem nicht vorhandenen Pfad verwendet. PowerShell erkennt, dass der Pfad nicht vorhanden ist, löst einen Fehler aus und fährt mit dem nächsten Ordner fort. Der Fehler legt in diesem Fall den Status des Runbookauftrags nicht auf „Fehler“ fest, sodass der Auftrag unter Umständen sogar abgeschlossen wird. Um ein Runbook zu veranlassen, bei einem Fehler ohne Abbruch anzuhalten, können Sie `ErrorAction Stop` für das Cmdlet verwenden.

## <a name="handling-jobs"></a>Verarbeiten von Aufträgen

Die Ausführungsumgebung für Aufträge kann im gleichen Automation-Konto wiederverwendet werden. Für ein einzelnes Runbook können viele Aufträge gleichzeitig ausgeführt werden. Je mehr Aufträge Sie zur gleichen Zeit ausführen, desto häufiger können diese an dieselbe Sandbox weitergeleitet werden.

Im selben Sandboxprozess ausgeführte Aufträge können sich gegenseitig beeinflussen. Ein Beispiel ist das Ausführen des Cmdlets [Disconnect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/disconnect-azaccount?view=azps-3.7.0). Bei Ausführen dieses Cmdlets wird jeder Runbookauftrag im gemeinsam genutzten Sandboxprozess getrennt.

PowerShell-Aufträge, die in einem Runbook gestartet werden, das in einer Azure-Sandbox ausgeführt wird, können möglicherweise nicht im vollständigen [PowerShell-Sprachmodus](/powershell/module/microsoft.powershell.core/about/about_language_modes) ausgeführt werden. Weitere Informationen zur Interaktion mit Aufträgen in Azure Automation finden Sie unter [Abrufen des Auftragsstatus mithilfe von PowerShell](#retrieving-job-status-using-powershell).

### <a name="job-statuses"></a>Auftragsstatuswerte

Die folgende Tabelle beschreibt die für einen Auftrag möglichen Status.

| Status | BESCHREIBUNG |
|:--- |:--- |
| Abgeschlossen |Der Auftrag wurde erfolgreich abgeschlossen. |
| Fehler |Bei einem grafischen oder PowerShell-Workflow-Runbook ist die Kompilierung fehlgeschlagen. Ein PowerShell-Skriptrunbook konnte nicht gestartet werden, oder beim Auftrag ist eine Ausnahme aufgetreten. Weitere Informationen finden Sie unter [Azure Automation-Runbooktypen](automation-runbook-types.md).|
| Fehler, auf Ressourcen wird gewartet |Beim Auftrag ist ein Fehler aufgetreten, da der Auftrag dreimal den Grenzwert für die [gleichmäßige Verteilung](#fair-share) erreicht hat und jedes Mal vom gleichen Prüfpunkt oder vom Anfang des Runbooks gestartet wurde. |
| In Warteschlange |Der Auftrag wartet darauf, dass Ressourcen für einen Automation-Worker verfügbar werden, damit er gestartet werden kann. |
| Wird gestartet |Der Auftrag wurde einem Worker zugewiesen, und das System startet ihn. |
| Wird fortgesetzt |Das System setzt den Auftrag fort, nachdem er angehalten wurde. |
| Wird ausgeführt |Der Auftrag wird ausgeführt. |
| Wird ausgeführt, auf Ressourcen wird gewartet |Der Auftrag wurde entladen, da er den Grenzwert für die gleichmäßige Verteilung erreicht hat. Er wird in Kürze vom letzten Prüfpunkt wiederaufgenommen. |
| Beendet |Der Auftrag wurde vom Benutzer beendet, bevor er abgeschlossen wurde. |
| Wird beendet |Das System beendet den Auftrag. |
| Ausgesetzt |Gilt nur für [grafische und PowerShell-Workflow-Runbooks](automation-runbook-types.md). Der Auftrag wurde vom Benutzer, vom System oder von einem Befehl im Runbook angehalten. Wenn ein Runbook keinen Prüfpunkt aufweist, wird der Vorgang am Anfang gestartet. Wenn ein Prüfpunkt vorhanden ist, kann es erneut starten und den Vorgang bei seinem letzten Prüfpunkt fortsetzen. Das Runbook wird nur vom System angehalten, wenn eine Ausnahme auftritt. Standardmäßig ist die Variable `ErrorActionPreference` auf „Continue“ festgelegt. Dies bedeutet, dass der Auftrag bei einem Fehler weiter ausgeführt wird. Wenn diese Einstellungsvariable auf „Stop“ festgelegt wird, wird der Auftrag bei einem Fehler angehalten.  |
| Wird angehalten |Gilt nur für [grafische und PowerShell-Workflow-Runbooks](automation-runbook-types.md). Das System versucht, den Auftrag auf Anforderung des Benutzers anzuhalten. Das Runbook muss den nächsten Prüfpunkt erreichen, bevor es angehalten werden kann. Wenn der letzte Prüfpunkt bereits passiert wurde, wird das Runbook abgeschlossen, bevor es angehalten werden kann. |

### <a name="viewing-job-status-from-the-azure-portal"></a>Anzeigen des Auftragsstatus im Azure-Portal

Im Azure-Portal können Sie eine Statuszusammenfassung aller Runbookaufträge anzeigen oder weitere Details zu einem bestimmten Runbookauftrag aufführen. Sie können auch die Integration mit Ihrem Log Analytics-Arbeitsbereich konfigurieren, um den Status von Runbookaufträgen und Auftragsdatenströmen weiterzuleiten. Weitere Informationen zur Integration mit Azure Monitor-Protokollen finden Sie unter [Weiterleiten von Auftragsstatus und Auftragsdatenströmen von Automation an Log Analytics](automation-manage-send-joblogs-log-analytics.md).

Rechts in Ihrem ausgewählten Automation-Konto sehen Sie auf der Kachel **Auftragsstatistik** die Zusammenfassung aller Runbookaufträge.

![Die Kachel „Auftragsstatistik“](./media/automation-runbook-execution/automation-account-job-status-summary.png)

Auf dieser Kachel wird die Anzahl aller ausgeführten Aufträge neben einer grafischen Darstellung des Auftragsstatus angezeigt.

Wenn Sie auf die Kachel klicken, wird die Seite „Aufträge“ angezeigt, die eine Liste mit einer Zusammenfassung aller ausgeführten Aufträge enthält. Auf dieser Seite werden der Status, Runbookname sowie die Start- und Abschlusszeit jedes Auftrags angezeigt.

![Die Seite „Aufträge“ im Automation-Konto](./media/automation-runbook-execution/automation-account-jobs-status-blade.png)

Sie können die Auftragsliste filtern, indem Sie **Aufträge filtern** auswählen. Filtern Sie anhand eines bestimmten Runbooks, eines Auftragsstatus oder einer Option in der Dropdownliste, und geben Sie die Zeitspanne für die Suche an.

![Filtern des Auftragsstatus](./media/automation-runbook-execution/automation-account-jobs-filter.png)

Alternativ können Sie für ein bestimmtes Runbook eine Auftragszusammenfassung anzeigen, indem Sie in Ihrem Automation-Konto zuerst auf der Seite „Runbooks“ das Runbook und dann die Kachel **Aufträge** auswählen. Mit dieser Aktion wird die Seite „Aufträge“ angezeigt. Wenn Sie dort auf einen Auftragsdatensatz klicken, werden dessen Details und Ausgabe angezeigt.

![Die Seite „Aufträge“ im Automation-Konto](./media/automation-runbook-execution/automation-runbook-job-summary-blade.png)

### <a name="viewing-the-job-summary"></a>Anzeigen der Auftragszusammenfassung

Die zuvor beschriebene Auftragszusammenfassung ermöglicht Ihnen, eine Liste aller für ein bestimmtes Runbook erstellten Aufträge und deren letzten Status einzusehen. Um detaillierte Informationen und die Ausgabe für einen Auftrag anzuzeigen, klicken Sie auf den Namen des Auftrags in der Liste. Die Detailansicht des Auftrags enthält die Werte für die Runbookparameter, die für diesen Auftrag bereitgestellt wurden.

Zeigen Sie die Aufträge für ein Runbook mithilfe der folgenden Schritte an.

1. Wählen Sie im Azure-Portal die Option **Automation** aus, und wählen Sie anschließend den Namen eines Automation-Kontos aus.
2. Wählen Sie im Hub unter **Prozessautomatisierung** die Option **Runbooks** aus.
3. Wählen Sie auf der Seite „Runbooks“ in der Liste ein Runbook aus.
3. Klicken Sie auf der Seite für das ausgewählte Runbook auf die Kachel **Aufträge**.
4. Wenn Sie auf einen der Aufträge in der Liste klicken, können Sie auf der Seite mit den Runbook-Auftragsdetails die Details und Ausgabe einsehen.

### <a name="retrieving-job-status-using-powershell"></a>Abrufen des Auftragsstatus mithilfe von Windows PowerShell

Verwenden Sie das Cmdlet [Get-AzureAutomationJob](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationJob?view=azps-3.7.0), um die für ein Runbook erstellten Aufträge und die Details zu einem bestimmten Auftrag anzuzeigen. Wenn Sie ein Runbook mit PowerShell starten, indem Sie `Start-AzAutomationRunbook` verwenden, wird der entsprechende Auftrag zurückgegeben. Verwenden Sie [Get-AzAutomationJobOutput](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationJobOutput?view=azps-3.5.0), um die Auftragsausgabe abzurufen.

Die folgenden Beispielbefehle rufen den letzten Auftrag für ein Beispielrunbook ab und zeigen seinen Status, die für die Runbookparameter bereitgestellten Werte und die Auftragsausgabe an.

```azurepowershell-interactive
$job = (Get-AzAutomationJob –AutomationAccountName "MyAutomationAccount" `
–RunbookName "Test-Runbook" -ResourceGroupName "ResourceGroup01" | sort LastModifiedDate –desc)[0]
$job.Status
$job.JobParameters
Get-AzAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAcct" -Id $job.JobId –Stream Output
```

Das folgende Beispiel ruft die Ausgabe für einen bestimmten Auftrag ab und gibt die einzelnen Datensätze zurück. Wenn es eine Ausnahme für einen der Datensätze gibt, schreibt das Skript die Ausnahme anstelle des Werts. Dieses Verhalten ist hilfreich, da Ausnahmen zusätzliche Informationen liefern können, die bei der Ausgabe normalerweise nicht protokolliert werden.

```azurepowershell-interactive
$output = Get-AzAutomationJobOutput -AutomationAccountName <AutomationAccountName> -Id <jobID> -ResourceGroupName <ResourceGroupName> -Stream "Any"
foreach($item in $output)
{
    $fullRecord = Get-AzAutomationJobOutputRecord -AutomationAccountName <AutomationAccountName> -ResourceGroupName <ResourceGroupName> -JobId <jobID> -Id $item.StreamRecordId
    if ($fullRecord.Type -eq "Error")
    {
        $fullRecord.Value.Exception
    }
    else
    {
    $fullRecord.Value
    }
}
```

## <a name="getting-details-from-the-activity-log"></a>Abrufen von Details aus dem Aktivitätsprotokoll

Aus dem Aktivitätsprotokoll des Automation-Kontos können Sie Runbookdetails abrufen, z. B. die Person oder das Konto, die bzw. das das Runbook gestartet hat. Das folgende PowerShell-Beispiel gibt den letzten Benutzer an, der das angegebene Runbook ausgeführt hat.

```powershell-interactive
$SubID = "00000000-0000-0000-0000-000000000000"
$AutomationResourceGroupName = "MyResourceGroup"
$AutomationAccountName = "MyAutomationAccount"
$RunbookName = "MyRunbook"
$StartTime = (Get-Date).AddDays(-1)
$JobActivityLogs = Get-AzLog -ResourceGroupName $AutomationResourceGroupName -StartTime $StartTime `
                                | Where-Object {$_.Authorization.Action -eq "Microsoft.Automation/automationAccounts/jobs/write"}

$JobInfo = @{}
foreach ($log in $JobActivityLogs)
{
    # Get job resource
    $JobResource = Get-AzResource -ResourceId $log.ResourceId

    if ($JobInfo[$log.SubmissionTimestamp] -eq $null -and $JobResource.Properties.runbook.name -eq $RunbookName)
    {
        # Get runbook
        $Runbook = Get-AzAutomationJob -ResourceGroupName $AutomationResourceGroupName -AutomationAccountName $AutomationAccountName `
                                            -Id $JobResource.Properties.jobId | ? {$_.RunbookName -eq $RunbookName}

        # Add job information to hashtable
        $JobInfo.Add($log.SubmissionTimestamp, @($Runbook.RunbookName,$Log.Caller, $JobResource.Properties.jobId))
    }
}
$JobInfo.GetEnumerator() | sort key -Descending | Select-Object -First 1
```

## <a name="sharing-resources-among-runbooks"></a><a name="fair-share"></a>Gemeinsame Nutzung von Ressourcen unter Runbooks

Damit Ressourcen von allen Runbooks in der Cloud verwendet werden können, beendet oder entlädt Azure Automation jeden Auftrag vorübergehend, der seit mehr als drei Stunden ausgeführt wird. Aufträge für [PowerShell-Runbooks](automation-runbook-types.md#powershell-runbooks) und [Python-Runbooks](automation-runbook-types.md#python-runbooks) werden beendet und nicht neu gestartet, und der Auftragsstatus ändert sich in „Stopped“.

Für zeitintensive Aufgaben sollten Sie einen Hybrid Runbook Worker verwenden. Hybrid Runbook Worker unterliegen nicht der gleichmäßigen Verteilung und keiner Einschränkung hinsichtlich der Ausführungszeit eines Runbooks. Die anderen [Grenzwerte](../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits) für Aufträge gelten sowohl für Azure-Sandboxes als auch für Hybrid Runbook Workers. Wenngleich Hybrid Runbook Worker nicht der dreistündigen Begrenzung für die gleichmäßige Verteilung unterliegen, sollten Sie Runbooks entwickeln, die auf den Workern laufen, die einen Neustart nach unerwarteten lokalen Infrastrukturproblemen unterstützen.

Eine weitere Möglichkeit ist das Optimieren eines Runbooks durch Verwendung untergeordneter Runbooks. Beispielsweise kann Ihr Runbook die gleiche Funktion auf mehreren Ressourcen durchlaufen, wie z. B. bei einem Datenbankvorgang in mehreren Datenbanken. Sie können diese Funktion in ein [untergeordnetes Runbook](automation-child-runbooks.md) verschieben und veranlassen, dass Ihr Runbook es mit [Start-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/az.automation/start-azautomationrunbook?view=azps-3.7.0) aufruft. Untergeordnete Runbooks werden in separaten Prozessen parallel ausgeführt.

Das Verwenden untergeordneter Runbooks führt dazu, dass die Gesamtdauer der Verarbeitung für das übergeordnete Runbook verringert wird. Ihr Runbook kann mit dem Cmdlet [Get-AzAutomationJob](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationjob?view=azps-3.7.0) den Auftragsstatus für ein untergeordnetes Runbook überprüfen, wenn nach Abschluss des untergeordneten Runbooks noch Vorgänge erfolgen müssen.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Arbeiten mit Runbooks finden Sie unter [Verwalten von Runbooks in Azure Automation](manage-runbooks.md).
* Weitere Informationen zu den Methoden zum Starten eines Runbooks in Azure Automation finden Sie unter [Starten eines Runbooks in Azure Automation](automation-starting-a-runbook.md).
* Weitere Informationen zur PowerShell, einschließlich Sprachreferenz und Lernmodulen, finden Sie in der [PowerShell-Dokumentation](https://docs.microsoft.com/powershell/scripting/overview).
* Eine Referenz zu den PowerShell-Cmdlets finden Sie unter [Az.Automation](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation
).
