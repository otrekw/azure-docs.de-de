---
title: Behandeln von Problemen beim Onboarding von Azure Automation-Verwaltungslösungen
description: Erfahren Sie, wie Sie Probleme beim Onboarding von Azure Automation-Lösungen behandeln.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 05/22/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 371094ecba5168fd32a7af9fb81a71eb722efc91
ms.sourcegitcommit: 11572a869ef8dbec8e7c721bc7744e2859b79962
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "82836528"
---
# <a name="troubleshoot-solution-onboarding"></a>Behandeln von Problemen beim Onboarding von Lösungen

Beim Onboarding der Lösungen „Updateverwaltung“, „Änderungsnachverfolgung“ oder „Bestand“ von Azure Automation erhalten Sie ggf. Fehlermeldungen. In diesem Artikel wird beschrieben, welche Fehler auftreten und wie diese behoben werden können.

## <a name="known-issues"></a>Bekannte Probleme

### <a name="scenario-renaming-a-registered-node-requires-unregister-or-register-again"></a><a name="node-rename"></a>Szenario: Das Umbenennen eines registrierten Knotens erfordert das Aufheben der Registrierung und das erneute Registrieren.

#### <a name="issue"></a>Problem

Ein Knoten ist bei Azure Automation registriert, und dann wird der Computername im Betriebssystem geändert. Berichte von diesem Knoten werden weiterhin mit dem ursprünglichen Namen angezeigt.

#### <a name="cause"></a>Ursache

Beim Umbenennen registrierter Knoten wird der Knotenname in Azure Automation nicht aktualisiert.

#### <a name="resolution"></a>Lösung

Heben Sie die Registrierung des Knotens in Azure Automation State Configuration auf, und registrieren Sie ihn erneut. Berichte, die vor diesem Zeitpunkt im Dienst veröffentlicht wurden, sind anschließend nicht mehr verfügbar.

### <a name="scenario-re-signing-certificates-via-https-proxy-isnt-supported"></a><a name="resigning-cert"></a>Szenario: Das erneute Signieren von Zertifikaten über HTTPS-Proxy wird nicht unterstützt

#### <a name="issue"></a>Problem

Beim Herstellen einer Verbindung über eine Proxylösung, die den HTTPS-Datenverkehr beendet und dann den Datenverkehr mit einem neuen Zertifikat erneut verschlüsselt, lässt der Dienst die Verbindung nicht zu.

#### <a name="cause"></a>Ursache

Azure Automation unterstützt kein erneutes Signieren von Zertifikaten, die zum Verschlüsseln von Datenverkehr verwendet werden.

#### <a name="resolution"></a>Lösung

Für dieses Problem gibt es derzeit keine Problemumgehung.

## <a name="general-errors"></a>Allgemeine Fehler

### <a name="scenario-onboarding-fails-with-the-message-the-solution-cannot-be-enabled"></a><a name="missing-write-permissions"></a>Szenario: Fehler beim Onboarding mit der Meldung, dass die Lösung nicht aktiviert werden kann

#### <a name="issue"></a>Problem

Eine der folgenden Meldungen wird angezeigt, wenn Sie versuchen, einen virtuellen Computer in eine Lösung zu integrieren (Onboarding):

```error
The solution cannot be enabled due to missing permissions for the virtual machine or deployments
```

```error
The solution cannot be enabled on this VM because the permission to read the workspace is missing
```

#### <a name="cause"></a>Ursache

Dieser Fehler wird durch falsche oder fehlende Berechtigungen auf dem virtuellen Computer oder im Arbeitsbereich oder für den Benutzer verursacht.

#### <a name="resolution"></a>Lösung

Stellen Sie sicher, dass Sie über die richtigen [Berechtigungen verfügen, die für das Onboarding von Computern erforderlich sind](../automation-role-based-access-control.md#onboarding-permissions), und wiederholen Sie dann das Onboarding der Lösung. Wenn Sie die Fehlermeldung `The solution cannot be enabled on this VM because the permission to read the workspace is missing` erhalten, stellen Sie sicher, dass Sie über die Berechtigung `Microsoft.OperationalInsights/workspaces/read` verfügen, um herausfinden zu können, ob die VM in einen Arbeitsbereich integriert ist.

### <a name="scenario-onboarding-fails-with-the-message-failed-to-configure-automation-account-for-diagnostic-logging"></a><a name="diagnostic-logging"></a>Szenario: Fehler beim Onboarding mit der Meldung „Fehler beim Konfigurieren eines Automation-Kontos für die Diagnoseprotokollierung“

#### <a name="issue"></a>Problem

Die folgende Meldung wird angezeigt, wenn Sie versuchen, einen virtuellen Computer in eine Lösung zu integrieren (Onboarding):

```error
Failed to configure automation account for diagnostic logging
```

#### <a name="cause"></a>Ursache

Dieser Fehler kann auftreten, wenn der Tarif nicht mit dem Abrechnungsmodell des Abonnements übereinstimmt. Weitere Informationen finden Sie unter [Überwachen der Nutzung und geschätzten Kosten in Azure Monitor](https://aka.ms/PricingTierWarning).

#### <a name="resolution"></a>Lösung

Erstellen Sie Ihren Log Analytics-Arbeitsbereich manuell, und wiederholen Sie den Onboardingprozess, um den erstellten Arbeitsbereich auszuwählen.

### <a name="scenario-computergroupqueryformaterror"></a><a name="computer-group-query-format-error"></a>Szenario: ComputerGroupQueryFormatError

#### <a name="issue"></a>Problem

Dieser Fehlercode bedeutet, dass die Abfrage einer gespeicherten Suchcomputergruppe für die Ziellösung nicht ordnungsgemäß formatiert ist. 

#### <a name="cause"></a>Ursache

Möglicherweise haben Sie oder das System die Abfrage geändert.

#### <a name="resolution"></a>Lösung

Sie können die Abfrage für die Lösung löschen und dann ein erneutes Onboarding der Lösung durchführen, wodurch die Abfrage neu erstellt wird. Die Abfrage finden Sie in Ihrem Arbeitsbereich unter **Gespeicherte Suchvorgänge**. Der Name der Abfrage lautet **MicrosoftDefaultComputerGroup**, und die Kategorie der Abfrage ist der Name der zugeordneten Lösung. Wenn mehrere Lösungen aktiviert sind, wird die Abfrage **MicrosoftDefaultComputerGroup** unter **Speicherte Suchvorgänge** mehrfach angezeigt.

### <a name="scenario-policyviolation"></a><a name="policy-violation"></a>Szenario: PolicyViolation

#### <a name="issue"></a>Problem

Dieser Fehlercode zeigt an, dass bei der Bereitstellung ein Fehler aufgrund einer oder mehrerer Richtlinienverletzungen aufgetreten ist.

#### <a name="cause"></a>Ursache 

Eine Richtlinie blockiert den Abschluss des Vorgangs.

#### <a name="resolution"></a>Lösung

Für eine erfolgreiche Bereitstellung der Lösung müssen Sie eine Änderung der angegebenen Richtlinie in Betracht ziehen. Da viele verschiedene Richtlinientypen definiert werden können, hängen die erforderlichen Änderungen von der Richtlinie ab, gegen die verstoßen wurde. Wenn z. B. eine Richtlinie für eine Ressourcengruppe definiert ist, die die Berechtigung zum Ändern des Inhalts einiger enthaltener Ressourcen verweigert, können Sie eine der folgenden Korrekturen auswählen:

* Entfernen Sie die Richtlinie vollständig.
* Versuchen Sie, das Onboarding der Lösung in einer anderen Ressourcengruppe durchzuführen.
* Wählen Sie als neues Ziel der Richtlinie eine andere spezifische Ressource aus, z. B. ein Automation-Konto.
* Überarbeiten Sie die Gruppe der Ressourcen, für deren Ablehnung diese Richtlinie konfiguriert wurde.

Prüfen Sie die Benachrichtigungen rechts oben im Azure-Portal, oder navigieren Sie zum Anzeigen der fehlerhaften Bereitstellung zur Ressourcengruppe, die Ihr Automation-Konto enthält, und wählen Sie unter **Einstellungen** die Option **Bereitstellungen** aus. Weitere Informationen zu Azure Policy finden Sie unter [Übersicht zu Azure-Richtlinien](../../governance/policy/overview.md?toc=%2fazure%2fautomation%2ftoc.json).

### <a name="scenario-errors-trying-to-unlink-a-workspace"></a><a name="unlink"></a>Szenario: Fehler beim Versuch, die Verknüpfung eines Arbeitsbereichs aufzuheben

#### <a name="issue"></a>Problem

Beim Versuch, die Verknüpfung eines Arbeitsbereichs aufzuheben, erhalten Sie die folgende Fehlermeldung:

```error
The link cannot be updated or deleted because it is linked to Update Management and/or ChangeTracking Solutions.
```

#### <a name="cause"></a>Ursache

Dieser Fehler tritt auf, wenn Sie noch über aktive Lösungen in Ihrem Log Analytics-Arbeitsbereich verfügen, die davon abhängig sind, dass Ihr Automation-Konto und Ihr Log Analytics-Arbeitsbereich verknüpft sind.

### <a name="resolution"></a>Lösung

Entfernen Sie die folgenden Lösungen aus Ihrem Arbeitsbereich, sofern Sie sie verwenden:

* Updateverwaltung
* Änderungsnachverfolgung und Bestand
* Starten/Beenden von VMs außerhalb der Kernzeit

Nachdem Sie die Lösungen entfernt haben, können Sie die Verknüpfung mit Ihrem Arbeitsbereich aufheben. Es ist wichtig, alle vorhandenen Artefakte dieser Lösungen aus Ihrem Arbeitsbereich und Ihrem Automation-Konto zu entfernen:

* Für „Updateverwaltung“ entfernen Sie **Updatebereitstellungen (Zeitpläne)** aus Ihrem Automation-Konto.
* Für „VMs außerhalb der Geschäftszeiten starten/beenden“ entfernen Sie alle Sperren für die Komponenten der Lösung in Ihrem Automation-Konto unter **Einstellungen** > **Sperren**. Weitere Informationen finden Sie unter [Lösung „VMs außerhalb der Geschäftszeiten starten/beenden“](../automation-solution-vm-management.md#remove-the-solution).

## <a name="log-analytics-for-windows-extension-failures"></a><a name="mma-extension-failures"></a>Fehler der Log Analytics für Windows-Erweiterung

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 

Eine Installation des Log Analytics-Agents für die Windows-Erweiterung kann aus einer Vielzahl von Gründen fehlschlagen. Im folgenden Abschnitt werden die Onboardingprobleme beschrieben, die während der Bereitstellung des Log Analytics-Agents für die Windows-Erweiterung Fehler verursachen können.

>[!NOTE]
>„Log Analytics-Agent für Windows“ ist der Name, der aktuell in Azure Automation für den Microsoft Monitoring Agent (MMA) verwendet wird.

### <a name="scenario-an-exception-occurred-during-a-webclient-request"></a><a name="webclient-exception"></a>Szenario: Ausnahme während einer WebClient-Anforderung

Die Log Analytics für Windows-Erweiterung auf dem virtuellen Computer kann nicht mit externen Ressourcen kommunizieren, und die Bereitstellung schlägt fehl.

#### <a name="issue"></a>Problem

Im Folgenden finden Sie Beispiele für Fehlermeldungen, die zurückgegeben werden:

```error
Please verify the VM has a running VM agent, and can establish outbound connections to Azure storage.
```

```error
'Manifest download error from https://<endpoint>/<endpointId>/Microsoft.EnterpriseCloud.Monitoring_MicrosoftMonitoringAgent_australiaeast_manifest.xml. Error: UnknownError. An exception occurred during a WebClient request.
```

#### <a name="cause"></a>Ursache

Mögliche Ursachen für diesen Fehler:

* Ein in der VM konfigurierter Proxy lässt nur bestimmte Ports zu.
* Eine Firewalleinstellung hat den Zugriff auf die benötigten Ports und Adressen blockiert.

#### <a name="resolution"></a>Lösung

Stellen Sie sicher, dass die ordnungsgemäßen Ports und Adressen für die Kommunikation geöffnet sind. Eine Liste der Ports und Adressen finden Sie unter [Planen Ihres Netzwerks](../automation-hybrid-runbook-worker.md#network-planning).

### <a name="scenario-install-failed-because-of-transient-environment-issues"></a><a name="transient-environment-issue"></a>Szenario: Fehler bei der Installation aufgrund vorübergehender Umgebungsprobleme

Die Installation von Log Analytics für Windows-Erweiterung ist während der Bereitstellung aufgrund einer anderen Installation oder Aktion fehlgeschlagen, die die Installation blockiert.

#### <a name="issue"></a>Problem

Im Folgenden finden Sie Beispiele für Fehlermeldungen, die ggf. zurückgegeben werden:

```error
The Microsoft Monitoring Agent failed to install on this machine. Please try to uninstall and reinstall the extension. If the issue persists, please contact support.
```

```error
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1618'
```

```error
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.2) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.2\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1601'
```

#### <a name="cause"></a>Ursache

Mögliche Ursachen für diesen Fehler:

* Eine weitere Installation erfolgt gerade.
* Auf dem System wird während der Vorlagenbereitstellung ein Neustart ausgelöst.

#### <a name="resolution"></a>Lösung

Dieser Fehler ist von vorübergehender Natur. Wiederholen Sie die Bereitstellung, um die Erweiterung zu installieren.

### <a name="scenario-installation-timeout"></a><a name="installation-timeout"></a>Szenario: Zeitlimit für Installation

Die Installation der Log Analytics für Windows-Erweiterung wurde aufgrund eines Timeouts nicht abgeschlossen.

#### <a name="issue"></a>Problem

Im Folgenden finden Sie ein Beispiel für eine Fehlermeldung, die zurückgegeben werden kann:

```error
Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 15614
```

#### <a name="cause"></a>Ursache

Diese Art von Fehler tritt auf, weil der virtuelle Computer während der Installation unter großer Last steht.

### <a name="resolution"></a>Lösung

Versuchen Sie, die Log Analytics für Windows-Erweiterung zu installieren, wenn die VM unter geringerer Last steht.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem hier nicht aufgeführt wird, oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
* Stellen Sie eine Verbindung mit [@AzureSupport](https://twitter.com/azuresupport) her, dem offiziellen Microsoft Azure-Konto zum Verbessern der Kundenfreundlichkeit. Der Azure-Support verbindet die Azure-Community mit Antworten, Support und Experten.
* Erstellen Sie einen Azure-Supportfall. Wechseln Sie zur [Azure-Supportwebsite](https://azure.microsoft.com/support/options/), und wählen Sie **Support erhalten** aus.
