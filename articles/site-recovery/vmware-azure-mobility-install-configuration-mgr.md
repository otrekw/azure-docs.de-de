---
title: Automatisieren der Mobility Service-Installation für die Notfallwiederherstellung in Azure Site Recovery
description: Vorgehensweise zur automatischen Installation des Mobility Service für die Notfallwiederherstellung von VMware-VMs/physischen Servern mit Azure Site Recovery
author: Rajeswari-Mamilla
ms.topic: how-to
ms.date: 12/22/2019
ms.author: ramamill
ms.openlocfilehash: 43e6a39a52eb81573b4a4ba8ad63d48d0e51dedd
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2020
ms.locfileid: "76514810"
---
# <a name="automate-mobility-service-installation"></a>Automatisieren der Mobility Service-Installation

In diesem Artikel wird beschrieben, wie Sie Installation und Updates für den Mobility Service-Agent in [Azure Site Recovery](site-recovery-overview.md) automatisieren.

Wenn Sie Site Recovery für die Notfallwiederherstellung von lokalen VMware-VMs und physischen Servern in Azure bereitstellen, wird der Mobility Service-Agent auf jedem Computer installiert, den Sie replizieren möchten. Der Mobility Service erfasst Datenschreibvorgänge auf dem Computer und leitet sie zur Replikation an den Site Recovery-Prozessserver weiter. Es gibt mehrere Möglichkeiten, den Mobility Service bereitzustellen:

- **Pushinstallation**: Der Mobility Service-Agent wird von Site Recovery installiert, wenn Sie die Replikation für einen Computer im Azure-Portal aktivieren.
- **Manuelle Installation**: Der Mobility Service wird manuell auf den einzelnen Computern installiert. Weitere Informationen zur Pushinstallation und manuellen Installation finden Sie [hier](/vmware-physical-mobility-service-overview.md).
- **Automatisierte Bereitstellung**: Automatisieren Sie die Installation mithilfe von Softwarebereitstellungstools wie Microsoft Endpoint Configuration Manager oder Tools von Drittanbietern wie Intigua JetPatch.

Die automatisierte Installation und Aktualisierung kommen in folgenden Fällen als Lösung infrage:

- Ihre Organisation erlaubt keine Pushinstallation auf geschützten Servern.
- Ihre Unternehmensrichtlinie schreibt vor, dass Kennwörter regelmäßig geändert werden. Sie müssen ein Kennwort für die Pushinstallation angeben.
- Ihre Sicherheitsrichtlinie verbietet das Hinzufügen von Firewallausnahmen für bestimmte Computer.
- Sie sind Hostingdienstanbieter und möchten keine Anmeldeinformationen für die Computer von Kunden bereitstellen, die für die Pushinstallation mit Site Recovery erforderlich sind.
- Sie müssen die Installation den Agenten auf vielen Server gleichzeitig durchführen.
- Die Durchführung von Installationen und Upgrades soll in geplanten Wartungszeitfenstern erfolgen.



## <a name="prerequisites"></a>Voraussetzungen

Für eine automatisierte Installation müssen folgende Voraussetzungen erfüllt sein:

- Eine bereitgestellte Lösung für die Softwareinstallation, z. B. [Configuration Manager](https://docs.microsoft.com/configmgr/) oder [JetPatch](https://jetpatch.com/microsoft-azure-site-recovery/). 
-  Die Bereitstellungsvoraussetzungen für die VMware-Notfallwiederherstellung oder für die Notfallwiederherstellung von [physischen Servern](physical-azure-disaster-recovery.md) müssen in [Azure](tutorial-prepare-azure.md) und [lokal](vmware-azure-tutorial-prepare-on-premises.md) erfüllt sein. Sie sollten sich auch über die [Supportanforderungen](vmware-physical-azure-support-matrix.md) für die Notfallwiederherstellung informieren.

## <a name="prepare-for-automated-deployment"></a>Vorbereiten der automatisierten Bereitstellung

Die folgende Tabelle enthält eine Übersicht über die Tools und Prozesse zum Automatisieren der Mobility Service-Bereitstellung.

**Tool** | **Details** | **Anweisungen**
--- | --- | ---
**Konfigurations-Manager** | 1. Vergewissern Sie sich, dass die oben aufgeführten [Voraussetzungen](#prerequisites) erfüllt sind. <br/><br/>2. Stellen Sie die Notfallwiederherstellung durch Einrichten der Quellumgebung bereit. Dies umfasst u. a. das Herunterladen einer OVA-Datei für die Bereitstellung des Site Recovery-Konfigurationsservers als VMware-VM mithilfe einer OVF-Vorlage.<br/><br/> 2. Registrieren Sie den Konfigurationsserver beim Site Recovery-Dienst, richten Sie die Azure-Zielumgebung ein und konfigurieren Sie eine Replikationsrichtlinie.<br/><br/> 3. Erstellen Sie für eine automatisierte Mobility Service-Bereitstellung eine Netzwerkfreigabe, die die Passphrase für den Konfigurationsserver und die Mobility Service-Installationsdateien enthält.<br/><br/> 4. Erstellen Sie ein Configuration Manager-Paket mit der Installation oder den Updates und bereiten Sie die Mobility Service-Bereitstellung vor.<br/><br/> 5. Anschließend können Sie die Replikation nach Azure für die Computer aktivieren, auf denen der Mobility Service installiert ist. | [Automatisierung mit Configuration Manager](#automate-with-configuration-manager).
**JetPatch** | 1. Vergewissern Sie sich, dass die oben aufgeführten [Voraussetzungen](#prerequisites) erfüllt sind. <br/><br/> 2. Stellen Sie die Notfallwiederherstellung durch Einrichten der Quellumgebung bereit. Dies umfasst u. a. das Herunterladen und Bereitstellen von JetPatch Agent Manager für Azure Site Recovery in Ihrer Site Recovery-Umgebung mithilfe einer OVF-Vorlage.<br/><br/> 2. Registrieren Sie den Konfigurationsserver bei Site Recovery, richten Sie die Azure-Zielumgebung ein und konfigurieren Sie eine Replikationsrichtlinie.<br/><br/> 3. Initialisieren und vervollständigen Sie für eine automatische Bereitstellung die JetPatch Agent Manager-Konfiguration.<br/><br/> 4. In JetPatch können Sie eine Site Recovery-Richtlinie erstellen, um Bereitstellung und Upgrade des Mobility Service-Agents zu automatisieren. <br/><br/> 5. Anschließend können Sie die Replikation nach Azure für die Computer aktivieren, auf denen der Mobility Service installiert ist. | [Automatisierung mit JetPatch Agent Manager](https://jetpatch.com/microsoft-azure-site-recovery-deployment-guide/).<br/><br/> [Beheben von Fehlern bei der Agent-Installation](https://kc.jetpatch.com/hc/articles/360035981812) in JetPatch.

## <a name="automate-with-configuration-manager"></a>Automatisierung mit Configuration Manager

### <a name="prepare-the-installation-files"></a>Vorbereiten der Installationsdateien

1. Vergewissern Sie sich, dass die Voraussetzungen erfüllt sind.
2. Eine sichere Netzwerkdateifreigabe (SMB-Freigabe), auf die der Computer, auf dem der Konfigurationsserver ausgeführt wird, zugreifen kann.
3. [Klassifizieren Sie die Server](https://docs.microsoft.com/sccm/core/clients/manage/collections/automatically-categorize-devices-into-collections), auf denen Sie den Mobility Service installieren oder aktualisieren möchten, in Configuration Manager. Eine Sammlung sollte alle Windows-Server, eine andere alle Linux-Server enthalten. 
5. Erstellen Sie auf der Netzwerkfreigabe einen Ordner:

    - Erstellen Sie für die Installation auf Windows-Computern einen Ordner namens **MobSvcWindows**.
    - Erstellen Sie für die Installation auf Linux-Computern einen Ordner namens **MobSvcLinux**.

6. Melden Sie sich auf dem Konfigurationsservercomputer an.
7. Öffnen Sie auf dem Computer eine Administratoreingabeaufforderung.
8. Führen Sie den folgenden Befehl aus, um die Passphrasedatei zu generieren:

    ```
    cd %ProgramData%\ASR\home\svsystems\bin
    genpassphrase.exe -v > MobSvc.passphrase
    ```
9. Kopieren Sie die Datei "MobSvc.passphrase" in den Windows-Ordner und in den Linux-Ordner.
10. Führen Sie den folgenden Befehl aus, um zum Ordner zu navigieren, der die Installationsdateien enthält:

    ```
    cd %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository
    ```

11. Kopieren Sie die folgenden Installationsdateien in die Netzwerkfreigabe:

    - Kopieren Sie **Microsoft-ASR_UA_version_Windows_GA_date_Release.exe** in **MobSvcWindows**.
    - Kopieren Sie folgende Dateien in **MobSvcLinux**:
        - Microsoft-ASR_UA*RHEL6-64*release.tar.gz
        - Microsoft-ASR_UA*RHEL7-64*release.tar.gz
        - Microsoft-ASR_UA*SLES11-SP3-64*release.tar.gz
        - Microsoft-ASR_UA*SLES11-SP4-64*release.tar.gz
        - Microsoft-ASR_UA*OL6-64*release.tar.gz
        - Microsoft-ASR_UA*UBUNTU-14.04-64*release.tar.gz
      
12. Kopieren Sie Code in die Windows-oder Linux-Ordner, wie nachstehend beschrieben. Dabei gelten folgende Annahmen:
    - Die IP-Adresse des Konfigurationsservers ist 192.168.3.121.
    - Die sichere Netzwerkdateifreigabe ist **\\\ContosoSecureFS\MobilityServiceInstallers**.

### <a name="copy-code-to-the-windows-folder"></a>Kopieren des Codes in den Windows-Ordner

Kopieren Sie den folgenden Code:

- Speichern Sie ihn als **install.bat** im Ordner **MobSvcWindows**.
- Ersetzen Sie den Platzhalter [CSIP] in diesem Skript durch die tatsächlichen Werte der IP-Adresse des Konfigurationsserver.
- Das Skript unterstützt Neuinstallationen des Mobility Service-Agents und Updates bereits installierter Agents.

```DOS
Time /t >> C:\Temp\logfile.log
REM ==================================================
REM ==== Clean up the folders ========================
RMDIR /S /q %temp%\MobSvc
MKDIR %Temp%\MobSvc
MKDIR C:\Temp
REM ==================================================

REM ==== Copy new files ==============================
COPY M*.* %Temp%\MobSvc
CD %Temp%\MobSvc
REN Micro*.exe MobSvcInstaller.exe
REM ==================================================

REM ==== Extract the installer =======================
MobSvcInstaller.exe /q /x:%Temp%\MobSvc\Extracted
REM ==== Wait 10s for extraction to complete =========
TIMEOUT /t 10
REM =================================================

REM ==== Perform installation =======================
REM =================================================

CD %Temp%\MobSvc\Extracted
whoami >> C:\Temp\logfile.log
SET PRODKEY=HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall
REG QUERY %PRODKEY%\{275197FC-14FD-4560-A5EB-38217F80CBD1}
IF NOT %ERRORLEVEL% EQU 0 (
    echo "Product is not installed. Goto INSTALL." >> C:\Temp\logfile.log
    GOTO :INSTALL
) ELSE (
    echo "Product is installed." >> C:\Temp\logfile.log

    echo "Checking for Post-install action status." >> C:\Temp\logfile.log
    GOTO :POSTINSTALLCHECK
)

:POSTINSTALLCHECK
    REG QUERY "HKLM\SOFTWARE\Wow6432Node\InMage Systems\Installed Products\5" /v "PostInstallActions" | Find "Succeeded"
    If %ERRORLEVEL% EQU 0 (
        echo "Post-install actions succeeded. Checking for Configuration status." >> C:\Temp\logfile.log
        GOTO :CONFIGURATIONCHECK
    ) ELSE (
        echo "Post-install actions didn't succeed. Goto INSTALL." >> C:\Temp\logfile.log
        GOTO :INSTALL
    )

:CONFIGURATIONCHECK
    REG QUERY "HKLM\SOFTWARE\Wow6432Node\InMage Systems\Installed Products\5" /v "AgentConfigurationStatus" | Find "Succeeded"
    If %ERRORLEVEL% EQU 0 (
        echo "Configuration has succeeded. Goto UPGRADE." >> C:\Temp\logfile.log
        GOTO :UPGRADE
    ) ELSE (
        echo "Configuration didn't succeed. Goto CONFIGURE." >> C:\Temp\logfile.log
        GOTO :CONFIGURE
    )


:INSTALL
    echo "Perform installation." >> C:\Temp\logfile.log
    UnifiedAgent.exe /Role MS /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
    IF %ERRORLEVEL% EQU 0 (
        echo "Installation has succeeded." >> C:\Temp\logfile.log
        (GOTO :CONFIGURE)
    ) ELSE (
        echo "Installation has failed." >> C:\Temp\logfile.log
        GOTO :ENDSCRIPT
    )

:CONFIGURE
    echo "Perform configuration." >> C:\Temp\logfile.log
    cd "C:\Program Files (x86)\Microsoft Azure Site Recovery\agent"
    UnifiedAgentConfigurator.exe  /CSEndPoint "[CSIP]" /PassphraseFilePath %Temp%\MobSvc\MobSvc.passphrase
    IF %ERRORLEVEL% EQU 0 (
        echo "Configuration has succeeded." >> C:\Temp\logfile.log
    ) ELSE (
        echo "Configuration has failed." >> C:\Temp\logfile.log
    )
    GOTO :ENDSCRIPT

:UPGRADE
    echo "Perform upgrade." >> C:\Temp\logfile.log
    UnifiedAgent.exe /Platform "VmWare" /Silent
    IF %ERRORLEVEL% EQU 0 (
        echo "Upgrade has succeeded." >> C:\Temp\logfile.log
    ) ELSE (
        echo "Upgrade has failed." >> C:\Temp\logfile.log
    )
    GOTO :ENDSCRIPT

:ENDSCRIPT
    echo "End of script." >> C:\Temp\logfile.log


```
### <a name="copy-code-to-the-linux-folder"></a>Kopieren des Codes in den Linux-Ordner

Kopieren Sie den folgenden Code:

- Speichern Sie ihn als **install_linux.sh** im Ordner**MobSvcLinux**.
- Ersetzen Sie den Platzhalter [CSIP] in diesem Skript durch die tatsächlichen Werte der IP-Adresse des Konfigurationsserver.
- Das Skript unterstützt Neuinstallationen des Mobility Service-Agents und Updates bereits installierter Agents.

```Bash
#!/usr/bin/env bash

rm -rf /tmp/MobSvc
mkdir -p /tmp/MobSvc
INSTALL_DIR='/usr/local/ASR'
VX_VERSION_FILE='/usr/local/.vx_version'

echo "=============================" >> /tmp/MobSvc/sccm.log
echo `date` >> /tmp/MobSvc/sccm.log
echo "=============================" >> /tmp/MobSvc/sccm.log

if [ -f /etc/oracle-release ] && [ -f /etc/redhat-release ]; then
    if grep -q 'Oracle Linux Server release 6.*' /etc/oracle-release; then
        if uname -a | grep -q x86_64; then
            OS="OL6-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *OL6*.tar.gz /tmp/MobSvc
        fi
    fi
elif [ -f /etc/redhat-release ]; then
    if grep -q 'Red Hat Enterprise Linux Server release 6.* (Santiago)' /etc/redhat-release || \
        grep -q 'CentOS Linux release 6.* (Final)' /etc/redhat-release || \
        grep -q 'CentOS release 6.* (Final)' /etc/redhat-release; then
        if uname -a | grep -q x86_64; then
            OS="RHEL6-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *RHEL6*.tar.gz /tmp/MobSvc
        fi
    elif grep -q 'Red Hat Enterprise Linux Server release 7.* (Maipo)' /etc/redhat-release || \
        grep -q 'CentOS Linux release 7.* (Core)' /etc/redhat-release; then
        if uname -a | grep -q x86_64; then
            OS="RHEL7-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *RHEL7*.tar.gz /tmp/MobSvc
                fi
    fi
elif [ -f /etc/SuSE-release ] && grep -q 'VERSION = 11' /etc/SuSE-release; then
    if grep -q "SUSE Linux Enterprise Server 11" /etc/SuSE-release && grep -q 'PATCHLEVEL = 3' /etc/SuSE-release; then
        if uname -a | grep -q x86_64; then
            OS="SLES11-SP3-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *SLES11-SP3*.tar.gz /tmp/MobSvc
        fi
    elif grep -q "SUSE Linux Enterprise Server 11" /etc/SuSE-release && grep -q 'PATCHLEVEL = 4' /etc/SuSE-release; then
        if uname -a | grep -q x86_64; then
            OS="SLES11-SP4-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *SLES11-SP4*.tar.gz /tmp/MobSvc
        fi
    fi
elif [ -f /etc/lsb-release ] ; then
    if grep -q 'DISTRIB_RELEASE=14.04' /etc/lsb-release ; then
       if uname -a | grep -q x86_64; then
           OS="UBUNTU-14.04-64"
           echo $OS >> /tmp/MobSvc/sccm.log
           cp *UBUNTU-14*.tar.gz /tmp/MobSvc
       fi
    fi
else
    exit 1
fi

if [ -z "$OS" ]; then
    exit 1
fi

Install()
{
    echo "Perform Installation." >> /tmp/MobSvc/sccm.log
    ./install -q -d ${INSTALL_DIR} -r MS -v VmWare
    RET_VAL=$?
    echo "Installation Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Installation has succeeded. Proceed to configuration." >> /tmp/MobSvc/sccm.log
        Configure
    else
        echo "Installation has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

Configure()
{
    echo "Perform configuration." >> /tmp/MobSvc/sccm.log
    ${INSTALL_DIR}/Vx/bin/UnifiedAgentConfigurator.sh -i [CSIP] -P MobSvc.passphrase
    RET_VAL=$?
    echo "Configuration Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Configuration has succeeded." >> /tmp/MobSvc/sccm.log
    else
        echo "Configuration has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

Upgrade()
{
    echo "Perform Upgrade." >> /tmp/MobSvc/sccm.log
    ./install -q -v VmWare
    RET_VAL=$?
    echo "Upgrade Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Upgrade has succeeded." >> /tmp/MobSvc/sccm.log
    else
        echo "Upgrade has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

cp MobSvc.passphrase /tmp/MobSvc
cd /tmp/MobSvc

tar -zxvf *.tar.gz

if [ -e ${VX_VERSION_FILE} ]; then
    echo "${VX_VERSION_FILE} exists. Checking for configuration status." >> /tmp/MobSvc/sccm.log
    agent_configuration=$(grep ^AGENT_CONFIGURATION_STATUS "${VX_VERSION_FILE}" | cut -d"=" -f2 | tr -d " ")
    echo "agent_configuration=$agent_configuration" >> /tmp/MobSvc/sccm.log
     if [ "$agent_configuration" == "Succeeded" ]; then
        echo "Agent is already configured. Proceed to Upgrade." >> /tmp/MobSvc/sccm.log
        Upgrade
    else
        echo "Agent is not configured. Proceed to Configure." >> /tmp/MobSvc/sccm.log
        Configure
    fi
else
    Install
fi

cd /tmp

```


### <a name="create-a-package"></a>Erstellen eines Pakets

1. Melden Sie sich bei der Configuration Manager-Konsole an und wählen Sie **Softwarebibliothek** > **Anwendungsverwaltung** > **Pakete** aus.
2. Klicken Sie mit der rechten Maustaste auf **Pakete** > **Paket erstellen**.
3. Geben Sie Paketdetails wie Name, Beschreibung, Hersteller, Sprache und Version an.
4. Wählen Sie **Dieses Paket enthält Quelldateien** aus.
5. Klicken Sie auf **Durchsuchen**, und wählen Sie die Netzwerkfreigabe sowie den Ordner mit dem relevanten Installationsprogramm aus (MobSvcWindows oder MobSvcLinux). Klicken Sie dann auf **Weiter**.

   ![Screenshot des Assistenten zum Erstellen von Paketen und Programmen](./media/vmware-azure-mobility-install-configuration-mgr/create_sccm_package.png)

7. Wählen Sie **Standardprogramm** > **Weiter** auf der Seite **Typ des zu erstellenden Programms auswählen** aus.

   ![Screenshot des Assistenten zum Erstellen von Paketen und Programmen](./media/vmware-azure-mobility-install-configuration-mgr/sccm-standard-program.png)

8. Geben Sie auf der Seite **Informationen zu diesem Standardprogramm angeben** die folgenden Werte an:

    **Parameter** | **Windows-Wert** | **Linux-Wert**
    --- | --- | ---
    **Name** | Microsoft Azure Mobility Service installieren (Windows) | Microsoft Azure Mobility Service installieren (Linux)
    **Befehlszeile** | install.bat | ./install_linux.sh
    **Programm kann ausgeführt werden** | Unabhängig von Benutzeranmeldung | Unabhängig von Benutzeranmeldung
    **Weitere Parameter** | Standardeinstellung verwenden | Standardeinstellung verwenden

   ![Screenshot des Assistenten zum Erstellen von Paketen und Programmen](./media/vmware-azure-mobility-install-configuration-mgr/sccm-program-properties.png)

9. Gehen Sie in **Anforderungen für dieses Standardprogramm anzugeben** wie folgt vor:

    - Wählen Sie für Windows-Computer **Dieses Programm kann nur auf bestimmten Plattformen ausgeführt werden** aus. Wählen Sie dann die [unterstützten Windows-Betriebssysteme](vmware-physical-azure-support-matrix.md#replicated-machines) aus. Klicken Sie dann auf **Weiter**.
    - Wählen Sie für Linux-Computer **Dieses Programm kann auf jeder Plattform ausgeführt werden** aus. Klicken Sie dann auf **Weiter**.
   
10. Beenden Sie den Assistenten.



### <a name="deploy-the-package"></a>Bereitstellen des Pakets

1. Klicken Sie in der Configuration Manager-Konsole mit der rechten Maustaste auf das Paket und wählen Sie **Inhalt verteilen** aus.
   ![Screenshot der Configuration Manager-Konsole](./media/vmware-azure-mobility-install-configuration-mgr/sccm_distribute.png)
2. Wählen Sie die Verteilungspunkte aus, auf die die Pakete kopiert werden sollen. [Weitere Informationen](https://docs.microsoft.com/sccm/core/servers/deploy/configure/install-and-configure-distribution-points)
3. Schließen Sie den Assistenten ab. Dann beginnt die Replikation des Pakets auf die angegebenen Verteilungspunkte.
4. Klicken Sie nach Abschluss der Paketverteilung mit der rechten Maustaste auf das Paket und wählen Sie **Bereitstellen** aus.
   ![Screenshot der Configuration Manager-Konsole](./media/vmware-azure-mobility-install-configuration-mgr/sccm_deploy.png)
5. Wählen Sie die zuvor erstellte Windows- oder Linux-Gerätesammlung aus.
6. Wählen Sie auf der Seite **Das Inhaltsziel angeben** die **Verteilungspunkte** aus.
7. Legen Sie den **Zweck** auf der Seite **Geben Sie Einstellungen an, um die Bereitstellung dieser Software zu steuern** auf **Erforderlich** fest.

   ![Screenshot des Assistenten zum Bereitstellen von Software](./media/vmware-azure-mobility-install-configuration-mgr/sccm-deploy-select-purpose.png)

8. Richten Sie in **Zeitplan für diese Bereitstellung angeben** einen Zeitplan ein. [Weitere Informationen](https://docs.microsoft.com/sccm/apps/deploy-use/deploy-applications#bkmk_deploy-sched)

    - Der Mobility Service wird dem von Ihnen angegebenen Zeitplan entsprechend installiert. 
    - Um unnötige Neustarts zu vermeiden, planen Sie die Installation des Pakets während Ihrer monatlichen Wartungsfensters oder der Zeitfenster für Softwareupdates.
9. Konfigurieren Sie die Einstellungen auf der Seite **Verteilungspunkte** und beenden Sie den Assistenten.
10. Überwachen Sie den Fortschritt der Bereitstellung in der Configuration Manager-Konsole. Wechseln Sie zu **Überwachung** > **Bereitstellungen** >  *[Ihr Paketname]* .





### <a name="uninstall-the-mobility-service"></a>Deinstallieren von Mobility Service
Sie können Configuration Manager-Pakete erstellen, um Mobility Service zu deinstallieren. Führen Sie dazu folgendes Skript aus:

```
Time /t >> C:\logfile.log
REM ==================================================
REM ==== Check if Mob Svc is already installed =======
REM ==== If not installed no operation required ========
REM ==== Else run uninstall command =====================
REM ==== {275197FC-14FD-4560-A5EB-38217F80CBD1} is ====
REM ==== guid for Mob Svc Installer ====================
whoami >> C:\logfile.log
NET START | FIND "InMage Scout Application Service"
IF  %ERRORLEVEL% EQU 1 (GOTO :INSTALL) ELSE GOTO :UNINSTALL
:NOOPERATION
                echo "No Operation Required." >> c:\logfile.log
                GOTO :ENDSCRIPT
:UNINSTALL
                echo "Uninstall" >> C:\logfile.log
                MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
:ENDSCRIPT

```

## <a name="next-steps"></a>Nächste Schritte
[Aktivieren Sie jetzt den Schutz](vmware-azure-enable-replication.md) für virtuelle Computer.
