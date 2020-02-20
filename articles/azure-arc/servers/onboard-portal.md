---
title: Verbinden von Hybridcomputern mit Azure über das Azure-Portal
description: In diesem Artikel erfahren Sie, wie Sie Azure Arc für Server (Vorschauversion) im Azure-Portal verwenden, um den Agent zu installieren und Computer mit Azure zu verbinden.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
author: mgoedtel
ms.author: magoedte
ms.date: 02/12/2020
ms.topic: conceptual
ms.openlocfilehash: 3f4954b3a33cdd73c1e004ad1e643f9e24abf1e4
ms.sourcegitcommit: 98a5a6765da081e7f294d3cb19c1357d10ca333f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2020
ms.locfileid: "77485232"
---
# <a name="connect-hybrid-machines-to-azure-from-the-azure-portal"></a>Verbinden von Hybridcomputern mit Azure über das Azure-Portal

Azure Arc für Server (Vorschauversion) kann mithilfe einiger manueller Schritte für einen oder einige wenige Windows- oder Linux-Computer in Ihrer Umgebung aktiviert werden. Alternativ steht auch eine automatisierte Methode in Form eines uns bereitgestellten Vorlagenskripts zur Verfügung. Dieses Skript automatisiert das Herunterladen und Installieren beider Agents.

Für diese Methode müssen Sie auf dem Computer über Administratorberechtigungen verfügen, um den Agent installieren und konfigurieren zu können. Unter Linux muss hierfür das root-Konto verwendet werden. Unter Windows müssen Sie der Gruppe der lokalen Administratoren angehören.

Bevor Sie beginnen, sollten Sie die [Voraussetzungen](overview.md#prerequisites) überprüfen und sicherstellen, dass Ihr Abonnement und Ihre Ressourcen auch diese Anforderungen erfüllen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="generate-the-installation-script-from-the-azure-portal"></a>Generieren des Installationsskripts über das Azure-Portal

Das Skript zum Automatisieren des Download- und Installationsvorgangs sowie zum Herstellen der Verbindung mit Azure Arc ist über das Azure-Portal verfügbar. Gehen Sie wie folgt vor:

1. Navigieren Sie in Ihrem Browser zum [Azure-Portal](https://aka.ms/hybridmachineportal).

1. Wählen Sie auf der Seite **Computer – Azure Arc** entweder **Hinzufügen** (links oben) oder die Option **Computer erstellen – Azure Arc** (unten im mittleren Bereich) aus. 

1. Wählen Sie auf der Seite **Methode auswählen** die Kachel **Computer über ein interaktives Skript hinzufügen** und anschließend **Skript generieren** aus.

1. Wählen Sie auf der Seite **Skript generieren** das Abonnement und die Ressourcengruppe für die Verwaltung des Computers in Azure aus. Wählen Sie einen Azure-Standort zum Speichern der Computermetadaten aus.

    >[!NOTE]
    >Von Azure Arc für Server (Vorschauversion) werden nur folgende Regionen unterstützt:
    >- WestUS2
    >- Europa, Westen
    >- WestAsia
    >
    >Überprüfen Sie weitere Überlegungen bei der Auswahl einer Region [hier](overview.md#supported-regions) im Übersichtsartikel.

1. Wählen Sie auf der Seite **Skript generieren** in der Dropdownliste **Betriebssystem** das Betriebssystem aus, unter dem das Skript ausgeführt wird.

1. Falls der Computer über einen Proxyserver kommuniziert, um eine Internetverbindung herzustellen, wählen Sie **Weiter: Proxyserver** aus. 
1. Geben Sie auf der Registerkarte **Proxyserver** die IP-Adresse des Proxyservers oder den Namen und die Portnummer für die Kommunikation mit dem Proxyserver an. Geben Sie den Wert im Format `http://<proxyURL>:<proxyport>` ein. 
1. Wählen Sie **Überprüfen + generieren** aus.

1. Überprüfen Sie die auf der Registerkarte **Überprüfen + generieren** zusammengefassten Informationen, und wählen Sie dann **Herunterladen** aus. Sollten noch Änderungen erforderlich sein, wählen Sie **Zurück** aus.

## <a name="install-and-validate-the-agent-on-windows"></a>Installieren und Überprüfen des Agents unter Windows

### <a name="install-manually"></a>Manuelle Installation
Sie können den Azure Connected Machine-Agent manuell installieren, indem Sie das Windows Installer-Paket *AzureConnectedMachineAgent.msi* ausführen. 

> [!NOTE]
> * Zum Installieren und Deinstallieren des Agents sind *Administratorberechtigungen* erforderlich.
> * Laden Sie zunächst das Installer-Paket herunter, und kopieren Sie es in einen Ordner auf dem Zielserver, oder verwenden Sie einen freigegebenen Netzwerkordner. Wenn Sie das Installer-Paket ohne Optionen ausführen, wird ein Setup-Assistent zur interaktiven Installation des Agents gestartet.

Wenn der Computer über einen Proxyserver mit dem Dienst kommunizieren muss, muss nach der Installation des Agents ein weiter unten in diesem Artikel beschriebener Befehl ausgeführt werden. Dadurch wird die Systemumgebungsvariable `https_proxy` für den Proxyserver festgelegt.

In der folgenden Tabelle sind die Parameter hervorgehoben, die von Setup für den Agent auf der Befehlszeile unterstützt werden.

| Parameter | BESCHREIBUNG |
|:--|:--|
| /? | Gibt eine Liste der Befehlszeilenoptionen zurück. |
| /S | Führt eine automatische Installation ohne Benutzereingriff aus. |

Geben Sie beispielsweise `msiexec.exe /i AzureConnectedMachineAgent.msi /?` ein, um das Installationsprogramm mit dem Parameter `/?` auszuführen.

Dateien für den Azure Connected Machine-Agent werden standardmäßig unter *C:\Programme\AzureConnectedMachineAgent* installiert. Sollte der Agent nach Abschluss des Setups nicht gestartet werden, suchen Sie in den Protokollen nach ausführlichen Fehlerinformationen. Das Protokollverzeichnis ist *%Programfiles%\AzureConnectedMachineAgentAgent\logs*.

### <a name="install-with-the-scripted-method"></a>Installieren mit der Skriptmethode

1. Melden Sie sich beim Server an.

1. Öffnen Sie eine PowerShell-Eingabeaufforderung mit erhöhten Rechten.

1. Navigieren Sie zu dem Ordner bzw. zu der Freigabe, in den bzw. in die Sie das Skript kopiert haben, und führen Sie es auf dem Server aus, indem Sie das Skript `./OnboardingScript.ps1` ausführen.

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

`%ProgramFiles%\AzureConnectedMachineAgent\azcmagent.exe" connect --resource-group "<resourceGroupName>" --tenant-id "<tenantID>" --location "<regionName>" --subscription-id "<subscriptionID>"`

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

`/opt/azcmagent/bin/azcmagent.exe" connect --resource-group "<resourceGroupName>" --tenant-id "<tenantID>" --location "<regionName>" --subscription-id "<subscriptionID>"`

## <a name="verify-the-connection-with-azure-arc"></a>Überprüfen der Verbindung mit Azure Arc

Vergewissern Sie sich im Azure-Portal, dass die Serververbindung erfolgreich hergestellt wurde, nachdem Sie den Agent installiert und für die Verbindungsherstellung mit Azure Arc für Server (Vorschauversion) konfiguriert haben. Zeigen Sie Ihre Computer im [Azure-Portal](https://aka.ms/hybridmachineportal) an.

![Erfolgreiche Serververbindung](./media/onboard-portal/arc-for-servers-successful-onboard.png)

## <a name="clean-up"></a>Bereinigung

Gehen Sie wie folgt vor, um die Verbindung zwischen einem Computer und Azure Arc für Server (Vorschauversion) zu trennen:

1. Öffnen Sie Azure Arc für Server (Vorschauversion), indem Sie zum [Azure-Portal](https://aka.ms/hybridmachineportal) navigieren.

1. Wählen Sie den Computer in der Liste aus. Wählen Sie dann die Auslassungspunkte ( **...** ) und anschließend **Löschen** aus.

1. Gehen Sie zum Deinstallieren des Windows-Agents auf dem Computer wie folgt vor:

    a. Melden Sie sich bei dem Computer mit einem Konto an, das über Administratorberechtigungen verfügt.  
    b. Wählen Sie in der **Systemsteuerung** die Option **Programme und Features** aus.  
    c. Wählen Sie unter **Programme und Features** Folgendes aus: **Azure Connected Machine-Agent** >**Deinstallieren** > **Ja**.  

    >[!NOTE]
    > Sie können auch auf das Installer-Paket **AzureConnectedMachineAgent.msi** doppelklicken, um den Setup-Assistenten für den Agent auszuführen.

    Wenn Sie ein Skript für das Entfernen des Agents erstellen möchten, können Sie das folgende Beispiel verwenden. In diesem Beispiel wird der Produktcode abgerufen und der Agent über die Msiexec-Befehlszeile (`msiexec /x {Product Code}`) deinstalliert. Gehen Sie folgendermaßen vor:  
    
    a. Öffnen Sie den Registrierungs-Editor.  
    b. Suchen Sie unter dem Registrierungsschlüssel `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Uninstall` nach der Produktcode-GUID, und kopieren Sie sie.  
    c. Anschließend können Sie den Agent mithilfe von Msiexec deinstallieren.

    Im folgenden Beispiel wird das Deinstallieren des Agents gezeigt:

    ```powershell
    Get-ChildItem -Path HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall | `
    Get-ItemProperty | `
    Where-Object {$_.DisplayName -eq "Azure Connected Machine Agent"} | `
    ForEach-Object {MsiExec.exe /x "$($_.PsChildName)" /qn}
    ```

1. Führen Sie zum Deinstallieren des Linux-Agents den folgenden Befehl aus:

      ```bash
      sudo apt purge azcmagent
      ```

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie Ihren Computer mithilfe von [Azure Policy](../../governance/policy/overview.md) verwalten, wie z. B. bei der VM-[Gastkonfiguration](../../governance/policy/concepts/guest-configuration.md), dem Überprüfen, ob der Computer dem erwarteten Log Analytics-Arbeitsbereich Bericht erstattet, beim Aktivieren der Überwachung mit [Azure Monitor mit VMs](../../azure-monitor/insights/vminsights-enable-at-scale-policy.md) und vieles mehr.

- Weitere Informationen zum [Log Analytics-Agent](../../azure-monitor/platform/log-analytics-agent.md). Der Log Analytics-Agent für Windows und Linux ist erforderlich, wenn Sie das Betriebssystem und die Workloads auf dem Computer proaktiv überwachen, den Computer mithilfe von Automation-Runbooks oder Lösungen wie der Updateverwaltung verwalten oder andere Azure-Dienste wie [Azure Security Center](../../security-center/security-center-intro.md) verwenden möchten.
