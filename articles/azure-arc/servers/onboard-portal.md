---
title: Verbinden von Hybridcomputern mit Azure über das Azure-Portal
description: In diesem Artikel erfahren Sie, wie Sie Azure Arc für Server (Vorschauversion) im Azure-Portal verwenden, um den Agent zu installieren und Computer mit Azure zu verbinden.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
author: mgoedtel
ms.author: magoedte
ms.date: 05/18/2020
ms.topic: conceptual
ms.custom: references_regions
ms.openlocfilehash: 459360e72c2d35cafedb0291642bf081bfcad96c
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86103992"
---
# <a name="connect-hybrid-machines-to-azure-from-the-azure-portal"></a>Verbinden von Hybridcomputern mit Azure über das Azure-Portal

Azure Arc für Server (Vorschauversion) kann mithilfe einiger manueller Schritte für einen oder einige wenige Windows- oder Linux-Computer in Ihrer Umgebung aktiviert werden. Alternativ steht auch eine automatisierte Methode in Form eines uns bereitgestellten Vorlagenskripts zur Verfügung. Dieses Skript automatisiert das Herunterladen und Installieren beider Agents.

Für diese Methode müssen Sie auf dem Computer über Administratorberechtigungen verfügen, um den Agent installieren und konfigurieren zu können. Unter Linux muss hierfür das root-Konto verwendet werden. Unter Windows müssen Sie der Gruppe der lokalen Administratoren angehören.

Bevor Sie beginnen, sollten Sie die [Voraussetzungen](agent-overview.md#prerequisites) überprüfen und sicherstellen, dass Ihr Abonnement und Ihre Ressourcen auch diese Anforderungen erfüllen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="generate-the-installation-script-from-the-azure-portal"></a>Generieren des Installationsskripts über das Azure-Portal

Das Skript zum Automatisieren des Download- und Installationsvorgangs sowie zum Herstellen der Verbindung mit Azure Arc ist über das Azure-Portal verfügbar. Gehen Sie wie folgt vor:

1. Navigieren Sie in Ihrem Browser zum [Azure-Portal](https://aka.ms/hybridmachineportal).

1. Wählen Sie auf der Seite **Computer – Azure Arc** entweder **Hinzufügen** (links oben) oder die Option **Computer erstellen – Azure Arc** (unten im mittleren Bereich) aus. 

1. Wählen Sie auf der Seite **Methode auswählen** die Kachel **Computer über ein interaktives Skript hinzufügen** und anschließend **Skript generieren** aus.

1. Wählen Sie auf der Seite **Skript generieren** das Abonnement und die Ressourcengruppe für die Verwaltung des Computers in Azure aus. Wählen Sie einen Azure-Standort zum Speichern der Computermetadaten aus.

    >[!NOTE]
    >Von Azure Arc für Server (Vorschauversion) werden nur folgende Regionen unterstützt:
    >- EastUS
    >- WestUS2
    >- Europa, Westen
    >- SoutheastAsia
    >
    >Überprüfen Sie weitere Überlegungen bei der Auswahl einer Region [hier](overview.md#supported-regions) im Übersichtsartikel.

1. Wählen Sie auf der Seite **Skript generieren** in der Dropdownliste **Betriebssystem** das Betriebssystem aus, unter dem das Skript ausgeführt wird.

1. Falls der Computer über einen Proxyserver kommuniziert, um eine Internetverbindung herzustellen, wählen Sie **Weiter: Proxyserver** aus. 
1. Geben Sie auf der Registerkarte **Proxyserver** die IP-Adresse des Proxyservers oder den Namen und die Portnummer für die Kommunikation mit dem Proxyserver an. Geben Sie den Wert im Format `http://<proxyURL>:<proxyport>` ein. 
1. Wählen Sie **Überprüfen + generieren** aus.

1. Überprüfen Sie die auf der Registerkarte **Überprüfen + generieren** zusammengefassten Informationen, und wählen Sie dann **Herunterladen** aus. Sollten noch Änderungen erforderlich sein, wählen Sie **Zurück** aus.

## <a name="install-and-validate-the-agent-on-windows"></a>Installieren und Überprüfen des Agents unter Windows

### <a name="install-manually"></a>Manuelle Installation

Sie können den Azure Connected Machine-Agent manuell installieren, indem Sie das Windows Installer-Paket *AzureConnectedMachineAgent.msi* ausführen. Sie können die neueste Version des [Windows Installer-Pakets für den Windows-Agent](https://aka.ms/AzureConnectedMachineAgent) vom Microsoft Download Center herunterladen. 

> [!NOTE]
> * Zum Installieren und Deinstallieren des Agents sind *Administratorberechtigungen* erforderlich.
> * Laden Sie zunächst das Installer-Paket herunter, und kopieren Sie es in einen Ordner auf dem Zielserver, oder verwenden Sie einen freigegebenen Netzwerkordner. Wenn Sie das Installer-Paket ohne Optionen ausführen, wird ein Setup-Assistent zur interaktiven Installation des Agents gestartet.

Wenn der Computer über einen Proxyserver mit dem Dienst kommunizieren muss, muss nach der Installation des Agents ein weiter unten in diesem Artikel beschriebener Befehl ausgeführt werden. Dadurch wird die Systemumgebungsvariable `https_proxy` für den Proxyserver festgelegt.

Wenn Sie nicht mit den Befehlszeilenoptionen für Windows Installer-Pakete vertraut sind, lesen Sie die Artikel [Standardmäßige Installer-Befehlszeilenoptionen](/windows/win32/msi/standard-installer-command-line-options) und [Befehlszeilenoptionen](/windows/win32/msi/command-line-options).

Führen Sie das Installationsprogramm z. B. mit dem Parameter `/?` aus, um die Hilfe- und Kurzübersichtsoption anzuzeigen. 

```dos
msiexec.exe /i AzureConnectedMachineAgent.msi /?
```

Führen Sie den folgenden Befehl aus, um den Agent im Hintergrund zu installieren und eine Setupprotokolldatei im vorhandenen Ordner `C:\Support\Logs` zu erstellen.

```dos
msiexec.exe /i AzureConnectedMachineAgent.msi /qn /l*v "C:\Support\Logs\Azcmagentsetup.log"
```

Sollte der Agent nach Abschluss des Setups nicht gestartet werden, suchen Sie in den Protokollen nach ausführlichen Fehlerinformationen. Das Protokollverzeichnis ist *%Programfiles%\AzureConnectedMachineAgentAgent\logs*.

### <a name="install-with-the-scripted-method"></a>Installieren mit der Skriptmethode

1. Melden Sie sich beim Server an.

1. Öffnen Sie eine PowerShell-Eingabeaufforderung mit erhöhten Rechten.

1. Navigieren Sie zu dem Ordner bzw. zu der Freigabe, in den bzw. in die Sie das Skript kopiert haben, und führen Sie es auf dem Server aus, indem Sie das Skript `./OnboardingScript.ps1` ausführen.

Sollte der Agent nach Abschluss des Setups nicht gestartet werden, suchen Sie in den Protokollen nach ausführlichen Fehlerinformationen. Das Protokollverzeichnis ist *%Programfiles%\AzureConnectedMachineAgentAgent\logs*.

### <a name="configure-the-agent-proxy-setting"></a>Konfigurieren der Proxyeinstellung für den Agent

Führen Sie den folgenden Befehl aus, um die Umgebungsvariable für den Proxyserver festzulegen:

```powershell
# If a proxy server is needed, execute these commands with the proxy URL and port.
[Environment]::SetEnvironmentVariable("https_proxy", "http://{proxy-url}:{proxy-port}", "Machine")
$env:https_proxy = [System.Environment]::GetEnvironmentVariable("https_proxy","Machine")
# For the changes to take effect, the agent service needs to be restarted after the proxy environment variable is set.
Restart-Service -Name himds
```

>[!NOTE]
>Das Festlegen der Proxyauthentifizierung wird in dieser Vorschau vom Agent nicht unterstützt.
>

### <a name="configure-agent-communication"></a>Konfigurieren der Agent-Kommunikation

Nachdem Sie den Agent installiert haben, müssen Sie den folgenden Befehl ausführen, um den Agent für die Kommunikation mit dem Azure Arc-Dienst zu konfigurieren:

`"%ProgramFiles%\AzureConnectedMachineAgent\azcmagent.exe" connect --resource-group "<resourceGroupName>" --tenant-id "<tenantID>" --location "<regionName>" --subscription-id "<subscriptionID>"`

## <a name="install-and-validate-the-agent-on-linux"></a>Installieren und Überprüfen des Agents unter Linux

Der Linux-Agent für verbundene Computer wird im bevorzugten Paketformat für die Distribution (RPM oder DEB) bereitgestellt, die im [Paketrepository](https://packages.microsoft.com/) von Microsoft gehostet wird. Durch das [Shell-Skriptpaket `Install_linux_azcmagent.sh`](https://aka.ms/azcmagent) werden folgende Aktionen ausgeführt:

- Konfigurieren des Hostcomputers für das Herunterladen des Agent-Pakets von „packages.microsoft.com“
- Installieren des Hybridressourcenanbieter-Pakets

Durch Einschließen des Parameters `--proxy "{proxy-url}:{proxy-port}"` kann der Agent optional mit Ihren Proxyinformationen konfiguriert werden.

Das Skript enthält auch Logik zum Identifizieren der unterstützten und nicht unterstützten Distributionen, und es überprüft die Berechtigungen, die zum Ausführen der Installation erforderlich sind. 

Im folgenden Beispiel wird der Agent heruntergeladen und installiert:

```bash
# Download the installation package.
wget https://aka.ms/azcmagent -O ~/Install_linux_azcmagent.sh

# Install the connected machine agent. 
bash ~/Install_linux_azcmagent.sh
```

Wenn Sie den Agent herunterladen und installieren und dabei den Parameter `--proxy` einschließen möchten, um den Agent für die Kommunikation über Ihren Proxyserver zu konfigurieren, führen Sie die folgenden Befehle aus:

```bash
# Download the installation package.
wget https://aka.ms/azcmagent -O ~/Install_linux_azcmagent.sh

# Install the connected machine agent. 
bash ~/Install_linux_azcmagent.sh --proxy "{proxy-url}:{proxy-port}"
```

### <a name="configure-the-agent-communication"></a>Konfigurieren der Agent-Kommunikation

Führen Sie nach Abschluss der Agent-Installation den folgenden Befehl aus, um den Agent für die Kommunikation mit dem Azure Arc-Dienst zu konfigurieren:

`azcmagent connect --resource-group "<resourceGroupName>" --tenant-id "<tenantID>" --location "<regionName>" --subscription-id "<subscriptionID>"`

## <a name="verify-the-connection-with-azure-arc"></a>Überprüfen der Verbindung mit Azure Arc

Vergewissern Sie sich im Azure-Portal, dass die Serververbindung erfolgreich hergestellt wurde, nachdem Sie den Agent installiert und für die Verbindungsherstellung mit Azure Arc für Server (Vorschauversion) konfiguriert haben. Zeigen Sie Ihre Computer im [Azure-Portal](https://aka.ms/hybridmachineportal) an.

![Erfolgreiche Serververbindung](./media/onboard-portal/arc-for-servers-successful-onboard.png)

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie Ihren Computer mithilfe von [Azure Policy](../../governance/policy/overview.md) verwalten, wie z. B. bei der VM-[Gastkonfiguration](../../governance/policy/concepts/guest-configuration.md), dem Überprüfen, ob der Computer dem erwarteten Log Analytics-Arbeitsbereich Bericht erstattet, beim Aktivieren der Überwachung mit [Azure Monitor mit VMs](../../azure-monitor/insights/vminsights-enable-at-scale-policy.md) und vieles mehr.

- Weitere Informationen zum [Log Analytics-Agent](../../azure-monitor/platform/log-analytics-agent.md). Der Log Analytics-Agent für Windows und Linux ist erforderlich, wenn Sie das Betriebssystem und die Workloads auf dem Computer proaktiv überwachen, den Computer mithilfe von Automation-Runbooks oder Lösungen wie der Updateverwaltung verwalten oder andere Azure-Dienste wie [Azure Security Center](../../security-center/security-center-intro.md) verwenden möchten.
