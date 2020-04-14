---
title: Verwalten des Azure Arc für Server-Agent (Vorschau)
description: In diesem Artikel werden die verschiedenen Verwaltungsaufgaben beschrieben, die Sie typischerweise während des Lebenszyklus des Azure Arc für Server-Connected Machine-Agent ausführen.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
author: mgoedtel
ms.author: magoedte
ms.date: 04/01/2020
ms.topic: conceptual
ms.openlocfilehash: 8bcf59ee863bb2fd2a3213480372ad215c2fc00d
ms.sourcegitcommit: c5661c5cab5f6f13b19ce5203ac2159883b30c0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2020
ms.locfileid: "80528588"
---
# <a name="managing-and-maintaining-the-connected-machine-agent"></a>Verwalten des Connected Machine-Agent

Nach der erstmaligen Bereitstellung des Azure Arc für Server-Connected Machine-Agent (Vorschau) für Windows oder Linux müssen Sie den Agent neu konfigurieren, ihn aktualisieren oder vom Computer entfernen, wenn er in seinem Lebenszyklus die Deaktivierungsphase erreicht hat. Sie können diese Routinewartungsaufgaben einfach manuell oder durch Automatisierung bewältigen, wodurch sowohl Betriebsfehler als auch Kosten reduziert werden.

## <a name="upgrading-agent"></a>Aktualisierung des Agent

Der Azure Connected Machine-Agent für Windows und Linux kann abhängig von Ihren Anforderungen manuell oder automatisch auf das neueste Release aktualisiert werden. In der folgenden Tabelle werden die Methoden beschrieben, die für das Agent-Upgrade unterstützt werden.

| Betriebssystem | Aktualisierungsmethode |
|------------------|----------------|
| Windows | Manuell<br> Windows-Update |
| Ubuntu | [Apt](https://help.ubuntu.com/lts/serverguide/apt.html) |
| SUSE Linux Enterprise Server | [zypper](https://en.opensuse.org/SDB:Zypper_usage_11.3) |
| RedHat Enterprise, Amazon, CentOS Linux | [yum](https://wiki.centos.org/PackageManagement/Yum) | 

### <a name="windows-agent"></a>Windows-Agent

Um den Agent auf einem Windows-Computer auf die neueste Version zu aktualisieren, ist der Agent über Microsoft Update verfügbar und kann mithilfe des vorhandenen Softwareupdate-Verwaltungsprozesses bereitgestellt werden. Er kann auch manuell über die Eingabeaufforderung, mit einem Skript oder einer anderen Automatisierungslösung oder über den Benutzeroberflächen-Assistenten ausgeführt werden, indem Sie `AzureConnectedMachine.msi` ausführen. 

> [!NOTE]
> * Zum Upgraden des Agent sind *Administratorberechtigungen* erforderlich.
> * Laden Sie für das manuelle Upgrade zunächst das Installer-Paket herunter, und kopieren Sie es in einen Ordner auf dem Zielserver, oder verwenden Sie einen freigegebenen Netzwerkordner. 

Wenn Sie mit den Befehlszeilenoptionen für Windows Installer-Pakete nicht vertraut sind, lesen Sie die Artikel [Standardmäßige Installer-Befehlszeilenoptionen](https://docs.microsoft.com/windows/win32/msi/standard-installer-command-line-options) und [Befehlszeilenoptionen](https://docs.microsoft.com/windows/win32/msi/command-line-options).

#### <a name="to-upgrade-using-the-setup-wizard"></a>So aktualisieren Sie mit dem Setup-Assistenten

1. Melden Sie sich auf dem Computer mit einem Konto an, das über Administratorrechte verfügt.

2. Führen Sie **AzureConnectedMachineAgent.msi** aus, um den Setup-Assistenten zu starten.

Der Setup-Assistent ermittelt, ob eine frühere Version vorhanden ist, und führt dann automatisch ein Upgrade des Agent aus. Wenn das Upgrade abgeschlossen ist, wird der Setup-Assistent automatisch geschlossen.

#### <a name="to-upgrade-from-the-command-line"></a>So aktualisieren Sie über die Befehlszeile

1. Melden Sie sich auf dem Computer mit einem Konto an, das über Administratorrechte verfügt.

2. Führen Sie den folgenden Befehl aus, um den Agent im Hintergrund zu aktualisieren und eine Setupprotokolldatei im Ordner `C:\Support\Logs` zu erstellen.

    ```dos
    msiexec.exe /i AzureConnectedMachineAgent.msi /qn /l*v "C:\Support\Logs\Azcmagentupgradesetup.log"
    ```

### <a name="linux-agent"></a>Linux-Agent

Mit zwei Befehlen wird der Agent auf einem Linux-Computer auf die neueste Version aktualisiert. Ein Befehl aktualisiert den lokalen Paketindex mit der Liste der neuesten verfügbaren Pakete aus den Repositorys und ein weiterer Befehl führt ein Upgrade des lokalen Pakets durch. 

> [!NOTE]
> Um ein Upgrade des Agents vorzunehmen, benötigen Sie *root*-Zugriffsberechtigungen oder ein Konto mit erhöhten Rechten, das Sudo verwendet.

#### <a name="upgrade-ubuntu"></a>Ubuntu-Upgrade

1. Führen Sie den folgenden Befehl aus, um den lokalen Paketindex mit den neuesten Änderungen in den Repositorys zu aktualisieren:

    ```bash
    apt update
    ```

2. Führen Sie den folgenden Befehl aus, um Ihr System upzugraden:

    ```bash
    apt upgrade
    ```

Aktionen des Befehls [apt](https://help.ubuntu.com/lts/serverguide/apt.html) wie Installation und Entfernung von Paketen werden in der `/var/log/dpkg.log`-Protokolldatei protokolliert.

#### <a name="upgrade-red-hatcentosamazon-linux"></a>Upgraden von Red Hat/CentOS/Amazon Linux

1. Führen Sie den folgenden Befehl aus, um den lokalen Paketindex mit den neuesten Änderungen in den Repositorys zu aktualisieren:

    ```bash
    yum check-update
    ```

2. Führen Sie den folgenden Befehl aus, um Ihr System upzugraden:

    ```bash
    yum update
    ```

Aktionen des Befehls [yum](https://access.redhat.com/articles/yum-cheat-sheet) wie Installation und Entfernung von Paketen werden in der `/var/log/yum.log`-Protokolldatei protokolliert. 

#### <a name="upgrade-suse-linux-enterprise"></a>Upgrade von SUSE Linux Enterprise

1. Führen Sie den folgenden Befehl aus, um den lokalen Paketindex mit den neuesten Änderungen in den Repositorys zu aktualisieren:

    ```bash
    zypper refresh
    ```

2. Führen Sie den folgenden Befehl aus, um Ihr System upzugraden:

    ```bash
    zypper update
    ```

Aktionen des Befehls [zypper](https://en.opensuse.org/Portal:Zypper) wie Installation und Entfernung von Paketen werden in der `/var/log/zypper.log`-Protokolldatei protokolliert. 

## <a name="remove-the-agent"></a>Entfernen des Agents

Führen Sie eine der folgenden Methoden aus, um den Conncected Machine-Agent für Windows oder Linux von dem Computer zu deinstallieren. Wenn Sie den Agent entfernen, wird die Registrierung des Computers bei Arc für Server (Vorschau) nicht aufgehoben. Dies ist ein gesonderter Prozess, den Sie ausführen, wenn Sie den Computer nicht mehr in Azure verwalten müssen.

### <a name="windows-agent"></a>Windows-Agent

Beider der folgenden Methoden entfernen den Agent, aber sie entfernen nicht den Ordner *C:\Programme\AzureConnectedMachineAgent* auf dem Computer.

#### <a name="uninstall-from-control-panel"></a>Deinstallieren über die Systemsteuerung

1. Gehen Sie zum Deinstallieren des Windows-Agents auf dem Computer wie folgt vor:

    a. Melden Sie sich bei dem Computer mit einem Konto an, das über Administratorberechtigungen verfügt.  
    b. Wählen Sie in der **Systemsteuerung** die Option **Programme und Features** aus.  
    c. Wählen Sie unter **Programme und Features** Folgendes aus: **Azure Connected Machine-Agent** >**Deinstallieren** > **Ja**.  

    >[!NOTE]
    > Sie können auch auf das Installer-Paket **AzureConnectedMachineAgent.msi** doppelklicken, um den Setup-Assistenten für den Agent auszuführen.

#### <a name="uninstall-from-the-command-line"></a>Deinstallieren über die Befehlszeile

Um den Agent manuell über die Eingabeaufforderung zu deinstallieren oder eine automatisierte Methode wie z. B. ein Skript zu verwenden, können Sie das folgende Beispiel nutzen. Zuerst müssen Sie den Produktcode, d. h. eine GUID, die der Prinzipalbezeichner des Anwendungspakets ist, vom Betriebssystem abrufen. Die Deinstallation erfolgt mithilfe der Msiexec.exe-Befehlszeile – `msiexec /x {Product Code}`.
    
1. Öffnen Sie den Registrierungs-Editor.

2. Suchen Sie unter dem Registrierungsschlüssel `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Uninstall` nach der Produktcode-GUID, und kopieren Sie sie.

3. Anschließend können Sie den Agent mithilfe von Msiexec gemäß der folgenden Beispiele deinstallieren:

   * Eingabe über die Befehlszeile:

       ```dos
       msiexec.exe /x {product code GUID} /qn
       ```

   * Sie können die gleichen Schritte auch mithilfe von PowerShell ausführen:

       ```powershell
       Get-ChildItem -Path HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall | `
       Get-ItemProperty | `
       Where-Object {$_.DisplayName -eq "Azure Connected Machine Agent"} | `
       ForEach-Object {MsiExec.exe /x "$($_.PsChildName)" /qn}
       ```

### <a name="linux-agent"></a>Linux-Agent

> [!NOTE]
> Um den Agent zu deinstallieren, benötigen Sie *root*-Zugriffsberechtigungen oder ein Konto mit erhöhten Rechten, das Sudo verwendet.

Beim Deinstallieren des Linux-Agents ist der zu verwendende Befehl vom Linux-Betriebssystem abhängig.

- Führen Sie für Ubuntu den folgenden Befehl aus:

    ```bash
    sudo apt purge azcmagent
    ```

- Führen Sie für RHEL, CentOS und Amazon Linux den folgenden Befehl aus:

    ```bash
    sudo yum remove azcmagent
    ```

- Führen Sie für SLES den folgenden Befehl aus:

    ```bash
    sudo zypper remove azcmagent
    ```

## <a name="unregister-machine"></a>Aufheben der Registrierung eines Computers

Wenn Sie beabsichtigen, die Verwaltung des Computers mit unterstützenden Diensten in Azure zu beenden, führen Sie die folgenden Schritte aus, um die Registrierung des Computers bei Arc für Server (Vorschau) aufzuheben. Sie können diesen Schritt entweder vor oder nach dem Entfernen des Connected Machine-Agents von dem Computer ausführen.

1. Öffnen Sie Azure Arc für Server (Vorschauversion), indem Sie zum [Azure-Portal](https://aka.ms/hybridmachineportal) navigieren.

2. Wählen Sie den Computer in der Liste aus. Wählen Sie dann die Auslassungspunkte ( **...** ) und anschließend **Löschen** aus.
