---
title: Bereitstellen eines Hybrid Runbook Workers unter Linux in Azure Automation
description: In diesem Artikel erfahren Sie, wie Sie einen Azure Automation-Hybrid Runbook Worker bereitstellen, mit dem Sie Runbooks auf Linux-basierten Computern in Ihrem lokalen Rechenzentrum oder einer Cloudumgebung ausführen können.
services: automation
ms.subservice: process-automation
ms.date: 03/02/2020
ms.topic: conceptual
ms.openlocfilehash: a6cf348142d694a03da24f32793fc72325701931
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83835222"
---
# <a name="deploy-a-linux-hybrid-runbook-worker"></a>Bereitstellen eines Linux-Hybrid Runbook Workers

Sie können mit dem Azure Automation-Feature „Hybrid Runbook Worker“ Runbooks direkt auf dem Computer ausführen, der die Rolle hostet, und außerhalb für Ressourcen in der Umgebung zur Verwaltung dieser lokalen Ressourcen. Linux Hybrid Runbook Worker führt Runbooks als spezieller Benutzer mit erhöhten Rechten aus, um Befehle ausführen zu können, für die erhöhte Rechte erforderlich sind. Runbooks werden in Azure Automation gespeichert und verwaltet und an einen oder mehrere dafür vorgesehene Computer übermittelt.

Dieser Artikel beschreibt, wie Sie den Hybrid Runbook Worker auf einem Linux-Computer installieren, den Worker entfernen und eine Hybrid Runbook Worker-Gruppe entfernen.

## <a name="supported-linux-operating-systems"></a>Unterstützte Linux-Betriebssysteme

Die Hybrid Runbook Worker-Funktion unterstützt die folgenden Distributionen:

* Amazon Linux 2012.09 bis 2015.09 (x86/x64)
* CentOS Linux 5, 6 und 7 (x86/x64)
* Oracle Linux 5, 6 und 7 (x86/x64)
* Red Hat Enterprise Linux Server 5, 6 und 7 (x86/x64)
* Debian GNU/Linux 6, 7 und 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS, 16.04 LTS und 18.04 (x86/x64)
* SUSE Linux Enterprise Server 11 und 12 (x86/x64)

## <a name="supported-runbook-types"></a>Unterstützte Runbooktypen

Linux Hybrid Runbook Worker unterstützen nicht alle Runbooktypen in Azure Automation.

Die folgenden Runbooktypen werden auf einem Linux Hybrid Worker unterstützt:

* Python 2
* PowerShell

  > [!NOTE]
  > PowerShell-Runbooks erfordern, dass PowerShell Core auf dem Linux-Computer installiert wird. Unter [Installieren von PowerShell Core unter Linux](/powershell/scripting/install/installing-powershell-core-on-linux) erfahren Sie, wie Sie PowerShell Core installieren.

Die folgenden Runbooktypen werden auf einem Linux Hybrid Worker nicht unterstützt:

* PowerShell-Workflow
* Grafisch
* Grafischer PowerShell-Workflow

## <a name="deployment-requirements"></a>Bereitstellungsanforderungen

Die Mindestanforderungen für einen Linux Hybrid Runbook Worker sind:

* Zwei Kerne
* 4 GB RAM
* Port 443 (ausgehend)

### <a name="package-requirements"></a>Paketanforderungen

| **Erforderliches Paket** | **Beschreibung** | **Mindestversion**|
|--------------------- | --------------------- | -------------------|
|Glibc |GNU C-Bibliothek| 2.5-12 |
|Openssl| OpenSSL-Bibliotheken | 1.0 (TLS 1.1 und TLS 1.2 werden unterstützt)|
|Curl | cURL-Webclient | 7.15.5|
|Python-ctypes | Python 2.x ist erforderlich. |
|PAM | Module für austauschbare Authentifizierung|
| **Optionale Pakete** | **Beschreibung** | **Mindestversion**|
| PowerShell Core | Zum Ausführen von PowerShell-Runbooks muss PowerShell installiert werden. Unter [Installieren von PowerShell Core unter Linux](/powershell/scripting/install/installing-powershell-core-on-linux) erfahren Sie, wie Sie PowerShell installieren.  | 6.0.0 |

## <a name="install-a-linux-hybrid-runbook-worker"></a>Installieren eines Linux Hybrid Runbook Workers

Um einen Hybrid Runbook Worker auf Ihrem Linux-Computer zu installieren und zu konfigurieren, führen Sie ein einfaches manuelles Verfahren aus. Dazu ist die Aktivierung von Hybrid Runbook Worker in Ihrem Azure Log Analytics-Arbeitsbereich erforderlich sowie das Ausführen einiger Befehle, um den Computer als Worker zu registrieren und einer Gruppe hinzuzufügen.

Bevor Sie fortfahren, beachten Sie den Log Analytics-Arbeitsbereich, mit dem Ihr Automation-Konto verknüpft ist. Beachten Sie außerdem den Primärschlüssel für Ihr Automation-Konto. Sie finden beides im Azure-Portal. Wählen Sie Ihr Automation-Konto aus und dann **Arbeitsbereich** für die Arbeitsbereichs-ID sowie **Schlüssel** für den Primärschlüssel. Weitere Informationen zu Ports und Adressen, die für Hybrid Runbook Worker erforderlich sind, finden Sie unter [Konfigurieren des Netzwerks](automation-hybrid-runbook-worker.md#network-planning).

>[!NOTE]
> Das Konto [nxautomation](automation-runbook-execution.md#log-analytics-agent-for-linux) mit den entsprechenden sudo-Berechtigungen muss bei der Installation des Linux-Hybrid Workers vorhanden sein. Wenn Sie versuchen, den Worker zu installieren, und das Konto nicht vorhanden ist oder nicht über die entsprechenden Berechtigungen verfügt, tritt bei der Installation ein Fehler auf.

1. Aktivieren Sie Hybrid Runbook Worker in Azure über eine der folgenden Methoden:

   * Fügen Sie Hybrid Runbook Worker Ihrem Abonnement mithilfe des Verfahrens unter [Hinzufügen von Azure Monitor-Protokollen zu Ihrem Arbeitsbereich](../log-analytics/log-analytics-add-solutions.md) hinzu.
   * Führen Sie das folgende Cmdlet aus:

        ```azurepowershell-interactive
         Set-AzOperationalInsightsIntelligencePack -ResourceGroupName  <ResourceGroupName> -WorkspaceName <WorkspaceName> -IntelligencePackName  "AzureAutomation" -Enabled $true
        ```

1. Installieren Sie den Log Analytics-Agent für Linux, indem Sie den folgenden Befehl ausführen. Ersetzen Sie \<WorkspaceID\> und \<WorkspaceKey\> mit den entsprechenden Werten aus dem Arbeitsbereich.

   [!INCLUDE [log-analytics-agent-note](../../includes/log-analytics-agent-note.md)]

   ```bash
   wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
   ```

1. Führen Sie den folgenden Befehl aus. Ändern Sie die Werte für die Parameter *-w*, *-k*, *-g* und *-e*. Ersetzen Sie den Wert für den Parameter *-g* durch den Namen der Hybrid Runbook Worker-Gruppe, der der neue Linux Hybrid Runbook Worker beitreten soll. Wenn der Name in Ihrem Automation-Konto nicht vorhanden ist, wird eine neue Hybrid Runbook Worker-Gruppe mit diesem Namen erstellt.

   ```bash
   sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/onboarding.py --register -w <LogAnalyticsworkspaceId> -k <AutomationSharedKey> -g <hybridgroupname> -e <automationendpoint>
   ```

1. Nachdem der Befehl abgeschlossen ist, werden auf der Seite „Hybrid Worker-Gruppen“ im Azure-Portal die neue Gruppe und die Anzahl der Mitglieder angezeigt. Wenn die Gruppe bereits vorhanden ist, wird die Anzahl der Mitglieder erhöht. Sie können die Gruppe in der Liste auf der Seite „Hybrid Worker-Gruppen“ und dann die Kachel **Hybrid Worker** auswählen. Auf der Seite „Hybrid Worker“ werden die einzelnen Mitglieder der Gruppe aufgelistet.

> [!NOTE]
> Wenn Sie die Azure Monitor-VM-Erweiterung für Linux für einen virtuellen Azure-Computer verwenden, empfehlen wir, `autoUpgradeMinorVersion` auf FALSE festzulegen, da automatische Upgrades von Versionen Probleme mit dem Hybrid Runbook Worker verursachen können. Informationen zum manuellen Upgrade der Erweiterung finden Sie unter [Azure CLI-Bereitstellung ](../virtual-machines/extensions/oms-linux.md#azure-cli-deployment).

## <a name="turn-off-signature-validation"></a>Deaktivieren der Signaturüberprüfung

Standardmäßig erfordern Linux Hybrid Runbook Worker eine Überprüfung der Signatur. Wenn Sie ein unsigniertes Runbook für einen Worker ausführen, wird der Fehler `Signature validation failed` angezeigt. Um die Überprüfung der Signatur zu deaktivieren, führen Sie den folgenden Befehl aus. Ersetzen Sie den zweiten Parameter durch Ihre Log Analytics-Arbeitsbereichs-ID.

 ```bash
 sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --false <LogAnalyticsworkspaceId>
 ```

## <a name="remove-the-hybrid-runbook-worker-from-an-on-premises-linux-computer"></a><a name="remove-linux-hybrid-runbook-worker"></a>Entfernen des Hybrid Runbook Workers von einem lokalen Linux-Computer

Sie können den Befehl `ls /var/opt/microsoft/omsagent` auf dem Hybrid Runbook Worker verwenden, um die Arbeitsbereichs-ID abzurufen. Es wird ein Ordner mit der Arbeitsbereichs-ID als Name erstellt.

```bash
sudo python onboarding.py --deregister --endpoint="<URL>" --key="<PrimaryAccessKey>" --groupname="Example" --workspaceid="<workspaceId>"
```

> [!NOTE]
> Mit diesem Code wird der Log Analytics-Agent für Linux nicht vom Computer entfernt. Er entfernt nur die Funktionalität und Konfiguration der Hybrid Runbook Worker-Rolle.

## <a name="remove-a-hybrid-worker-group"></a>Entfernen einer Hybrid Worker-Gruppe

Um eine Hybrid Runbook Worker-Gruppe von Linux-Computern zu entfernen, befolgen Sie die gleichen Schritte wie für eine Hybrid Worker-Gruppe unter Windows. Weitere Informationen finden Sie unter [Entfernen einer Hybrid Worker-Gruppe](automation-windows-hrw-install.md#remove-a-hybrid-worker-group).

## <a name="next-steps"></a>Nächste Schritte

* Um zu erfahren, wie Sie Ihre Runbooks für die Automatisierung von Prozessen in Ihrem lokalen Rechenzentrum oder in einer anderen Cloudumgebung konfigurieren, lesen Sie sich [Ausführen von Runbooks auf einem Hybrid Runbook Worker](automation-hrw-run-runbooks.md) durch.
* Informationen zum Behandeln von Problemen mit Ihren Hybrid Runbook Workern finden Sie unter [Problembehandlung von Hybrid Runbook Workern – Linux](troubleshoot/hybrid-runbook-worker.md#linux).
