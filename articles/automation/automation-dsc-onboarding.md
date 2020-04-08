---
title: Onboarding von Computern zur Verwaltung durch Azure Automation DSC
description: Hier erfahren Sie, wie Sie Computer mit Azure Automation DSC (Desired State Configuration) für die Verwaltung einrichten.
services: automation
ms.service: automation
ms.subservice: dsc
author: mgoedtel
ms.author: magoedte
ms.topic: conceptual
ms.date: 12/10/2019
manager: carmonm
ms.openlocfilehash: 554a4c64700bb189b4b9f085bd7c259312a36b4b
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2020
ms.locfileid: "80410937"
---
# <a name="onboarding-machines-for-management-by-azure-automation-state-configuration"></a>Onboarding von Computern zur Verwaltung durch Azure Automation DSC

## <a name="why-manage-machines-with-azure-automation-state-configuration"></a>Gründe für das Verwalten von Computern mit Azure Automation DSC

Azure Automation DSC ist ein Konfigurationsverwaltungsdienst für DSC-Knoten (Desired State Configuration) in jedem beliebigen Cloud- oder lokalen Rechenzentrum. Sie können über das Azure-Portal auf diesen zugreifen, indem Sie unter **Konfigurationsverwaltung** auf **State configuration (DSC)** klicken. 

Mit diesem Dienst können Sie Skalierbarkeit über Tausende von Computern schnell und einfach von einem zentralen, sicheren Ort aus erreichen. Sie können problemlos das Onboarding für Computer durchführen, ihnen deklarative Konfigurationen zuweisen und Berichte dazu anzeigen, inwieweit jeder Computer mit dem gewünschten Zustand, den Sie angegeben haben, kompatibel ist.

Der Azure Automation DSC-Dienst ist für DSC, was die Azure Automation-Runbooks für PowerShell-Skripts sind. So hilft Azure Automation auf die gleiche Weise wie beim Verwalten von PowerShell-Skripts bei der Verwaltung von DSC-Konfigurationen. Weitere Informationen zu den Vorteilen von Azure Automation DSC finden Sie unter [Azure Automation DSC – Übersicht](automation-dsc-overview.md).

Mit Azure Automation DSC können zahlreiche Computer verwaltet werden:

- Virtuelle Azure-Computer
- Virtuelle Azure-Computer (klassisch)
- Physische/virtuelle Windows-Computer, die lokal oder in einer anderen Cloud als Azure (einschließlich AWS-EC2-Instanzen) ausgeführt werden
- Physische/virtuelle Linux-Computer, lokal, in Azure oder in einer anderen Cloud als Azure

Wenn Sie Computerkonfigurationen noch nicht in der Cloud verwalten möchten, können Sie Azure Automation State Configuration auch ausschließlich als Endpunkt für Berichte verwenden. Mit diesem Feature können Sie Konfigurationen mittels DSC festlegen (pushen) und Berichterstellungsdetails in Azure Automation anzeigen.

> [!NOTE]
> Das Verwalten von Azure-VMs mit Azure Automation State Configuration ist kostenlos möglich, wenn die installierte Version der Azure-VM-Erweiterung für DSC höher als 2.70 ist. Weitere Informationen hierzu finden Sie unter [**Automation – Preise**](https://azure.microsoft.com/pricing/details/automation/).

In den folgenden Abschnitten dieses Artikels wird beschrieben, wie Sie das Onboarding für die oben aufgeführten Computer in Azure Automation State Configuration durchführen können.

## <a name="onboarding-azure-vms"></a>Durchführen des Onboardings für Azure-VMs

Mit Azure Automation State Configuration können Sie Azure-VMs problemlos für die Konfigurationsverwaltung integrieren, indem Sie das Azure-Portal, Azure Resource Manager-Vorlagen oder PowerShell verwenden. Ohne Remotezugriff eines Administrators auf den virtuellen Computer registriert die Azure-VM-Erweiterung für DSC im Hintergrund den virtuellen Computer bei Azure Automation State Configuration. Da die Azure-Erweiterung asynchron ausgeführt wird, finden Sie die Schritte zum Verfolgen des Fortschritts oder zur Problembehandlung im Abschnitt [Problembehandlung bei der Integration virtueller Azure-Computer](#troubleshooting-azure-virtual-machine-onboarding) dieses Artikels.

> [!NOTE]
>Beim Bereitstellen von DSC für einen Linux-Knoten wird der Ordner **/tmp** verwendet. Module wie `nxautomation` werden temporär zur Überprüfung heruntergeladen, bevor Sie an den entsprechenden Speicherorten installiert werden. Zum Sicherstellen, dass die Module ordnungsgemäß installiert werden, benötigt der Log Analytics-Agent für Linux Lese-/Schreibberechtigungen für den Ordner **/tmp**.<br><br>
>Der Log Analytics-Agent für Linux wird als Benutzer `omsagent` ausgeführt. Damit der `omsagent`-Benutzer die erforderliche Schreibberechtigung erhält, führen Sie den Befehl `setfacl -m u:omsagent:rwx /tmp` aus.

### <a name="onboard-a-vm-using-azure-portal"></a>Onboarding eines virtuellen Computers über das Azure-Portal

So gliedern Sie einen virtuellen Azure-Computer in Azure Automation State Configuration über das [Azure-Portal](https://portal.azure.com/) ein:

1. Navigieren Sie zu dem Azure Automation-Konto, in das Sie VMs integrieren möchten. 

2. Klicken Sie auf der State Configuration-Seite auf der Registerkarte **Knoten** auf **Hinzufügen**.

3. Wählen Sie einen virtuellen Computer für das Onboarding aus.

4. Wenn auf dem Computer nicht die PowerShell-Erweiterung für DSC installiert ist und der Energiezustand ausgeführt wird, klicken Sie auf **Verbinden**.

5. Geben Sie unter **Registrierung** die [Werte des lokalen Konfigurations-Managers von PowerShell DSC](/powershell/scripting/dsc/managing-nodes/metaConfig) ein, die für Ihren Anwendungsfall erforderlich sind. Optional können Sie eine Knotenkonfiguration eingeben, die dem virtuellen Computer zugewiesen werden soll.

![Onboarding](./media/automation-dsc-onboarding/DSC_Onboarding_6.png)

### <a name="onboard-a-vm-using-azure-resource-manager-templates"></a>Onboarding eines virtuellen Computers mithilfe von Azure Resource Manager-Vorlagen

Sie können eine VM bereitstellen und mithilfe von Azure Resource Manager-Vorlagen in Azure Automation State Configuration integrieren. Unter [Vom DSC-Dienst verwalteter Server](https://azure.microsoft.com/resources/templates/101-automation-configuration/) finden Sie eine Beispielvorlage zum Integrieren eines vorhandenen virtuellen Computers in Azure Automation DSC. Wenn Sie eine VM-Skalierungsgruppe verwalten, finden Sie Informationen in der Beispielvorlage unter [Von Azure Automation verwaltete VM-Skalierungsgruppenkonfiguration](https://azure.microsoft.com/resources/templates/201-vmss-automation-dsc/).

### <a name="onboard-machines-using-powershell"></a>Onboarding von Computern mithilfe von PowerShell

Sie können das Cmdlet [Register-AzAutomationDscNode](/powershell/module/az.automation/register-azautomationdscnode) in PowerShell verwenden, um VMs in Azure Automation State Configuration einzugliedern. 

> [!NOTE]
>Das Cmdlet `Register-AzAutomationDscNode` ist jedoch zurzeit nur für Computer mit Windows implementiert, da es nur die Windows-Erweiterung auslöst.

### <a name="register-vms-across-azure-subscriptions"></a>Registrieren von VMs über Azure-Abonnements hinweg

Die beste Methode zum Registrieren virtueller Computer aus anderen Azure-Abonnements ist die Verwendung der DSC-Erweiterung in einer Azure Resource Manager-Bereitstellungsvorlage. Beispiele finden Sie in [Erweiterung zum Konfigurieren des gewünschten Zustands mit Azure Resource Manager-Vorlagen](https://docs.microsoft.com/azure/virtual-machines/extensions/dsc-template).

Informationen zum Suchen von Registrierungsschlüssel und Registrierungs-URL, die als Parameter in der Vorlage verwendet werden, finden Sie im Abschnitt [Sicheres Onboarding mithilfe einer Registrierung](#onboarding-securely-using-registration) in diesem Artikel.

## <a name="onboarding-physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azure-including-aws-ec2-instances"></a>Onboarding physischer/virtueller Windows-Computer, die lokal oder in einer anderen Cloud als Azure (einschließlich AWS-EC2-Instanzen) ausgeführt werden

Sie können Windows-Server, die lokal oder in anderen Cloudumgebungen ausgeführt werden, in Azure Automation State Configuration eingliedern. Die Server müssen allerdings über [ausgehenden Zugriff auf Azure](automation-dsc-overview.md#network-planning) verfügen.

1. Stellen Sie sicher, dass die neueste Version von [WMF 5](https://aka.ms/wmf5latest) auf den Computern installiert ist, die Sie in Azure Automation State Configuration integrieren möchten. Außerdem muss WMF 5 auf dem Computer installiert sein, den Sie für den Onboarding-Vorgang verwenden.
1. Führen Sie die Schritte im Abschnitt [Generieren von DSC-Metakonfigurationen](#generating-dsc-metaconfigurations) weiter unten aus, um einen Ordner mit den erforderlichen DSC-Metakonfigurationen zu erstellen. 
1. Verwenden Sie das folgende Cmdlet, um die PowerShell DSC-Metakonfigurationen remote auf die Computer anzuwenden, die Sie integrieren möchten. 

   ```powershell
   Set-DscLocalConfigurationManager -Path C:\Users\joe\Desktop\DscMetaConfigs -ComputerName MyServer1, MyServer2
   ```

1. Wenn Sie die PowerShell DSC-Metakonfigurationen nicht remote anwenden können, kopieren Sie den Ordner **metaconfigurations** (Metakonfigurationen) auf die Computer, die Sie integrieren möchten. Fügen Sie dann Code hinzu, um `Set-DscLocalConfigurationManager` lokal auf den Computern aufzurufen.
1. Stellen Sie über das Azure-Portal oder mithilfe von Cmdlets sicher, dass die Computer, die Sie integrieren möchten, als State Configuration-Knoten angezeigt werden, die in Ihrem Azure Automation-Konto registriert sind.

## <a name="onboarding-physicalvirtual-linux-machines-on-premises-or-in-a-cloud-other-than-azure"></a>Onboarding physischer/virtueller Linux-Computer, die lokal oder in einer anderen Cloud als Azure ausgeführt werden

Sie können Linux-Server, die lokal oder in anderen Cloudumgebungen ausgeführt werden, in Azure Automation State Configuration eingliedern. Die Server müssen allerdings über [ausgehenden Zugriff auf Azure](automation-dsc-overview.md#network-planning) verfügen.

1. Stellen Sie sicher, dass die neueste Version der [PowerShell Desired State Configuration für Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux) auf den Computern installiert ist, die Sie in Azure Automation State Configuration integrieren möchten.
2. Gehen Sie folgendermaßen vor, wenn die [Standardwerte des lokalen Konfigurations-Managers von PowerShell DSC](/powershell/scripting/dsc/managing-nodes/metaConfig4) zu Ihrem Anwendungsfall passen und Sie Computer so integrieren möchten, dass diese sowohl Informationen von Azure Automation State Configuration abrufen als auch Informationen an Azure Automation State Configuration senden:

   - Verwenden Sie auf jedem Linux-Computer, den Sie in Azure Automation State Configuration integrieren möchten, `Register.py` für das Onboarding mit den Standardwerten des lokalen Konfigurations-Managers von PowerShell DSC.

     `/opt/microsoft/dsc/Scripts/Register.py <Automation account registration key> <Automation account registration URL>`

   - Informationen zum Suchen von Registrierungsschlüssel und Registrierungs-URL für Ihr Automation-Konto finden Sie im Abschnitt [Sicheres Onboarding mithilfe einer Registrierung](#onboarding-securely-using-registration) dieses Artikels.

3. Wenn die Standardwerte des lokalen Konfigurations-Managers (LCM) von PowerShell DSC nicht zu Ihrem Anwendungsfall passen, oder Sie Computer so integrieren möchten, dass diese nur Informationen an Azure Automation State Configuration senden, befolgen Sie die Schritte 4 bis 7. Fahren Sie andernfalls direkt mit Schritt 7 fort.

4. Führen Sie die Schritte im Abschnitt [Generieren von DSC-Metakonfigurationen](#generating-dsc-metaconfigurations) aus, um einen Ordner mit den erforderlichen DSC-Metakonfigurationen zu generieren.

5. Stellen Sie sicher, dass die neueste Version von [WMF 5](https://aka.ms/wmf5latest) auf dem Computer installiert ist, der für das Onboarding verwendet wird.

6. Fügen Sie Code wie den folgenden hinzu, um die PowerShell DSC-Metakonfigurationen remote auf die Computer anzuwenden, die Sie integrieren möchten.

    ```powershell
    $SecurePass = ConvertTo-SecureString -String '<root password>' -AsPlainText -Force
    $Cred = New-Object System.Management.Automation.PSCredential 'root', $SecurePass
    $Opt = New-CimSessionOption -UseSsl -SkipCACheck -SkipCNCheck -SkipRevocationCheck

    # need a CimSession for each Linux machine to onboard
    $Session = New-CimSession -Credential $Cred -ComputerName <your Linux machine> -Port 5986 -Authentication basic -SessionOption $Opt

    Set-DscLocalConfigurationManager -CimSession $Session -Path C:\Users\joe\Desktop\DscMetaConfigs
    ```

7. Wenn Sie die PowerShell DSC-Metakonfigurationen nicht remote anwenden können, kopieren Sie die Metakonfigurationen für die Remotecomputer aus dem in Schritt 4 beschriebenen Ordner auf die Linux-Computer.

8. Fügen Sie Code hinzu, um `Set-DscLocalConfigurationManager.py` lokal auf jedem Linux-Computer aufzurufen, den Sie in Azure Automation State Configuration integrieren möchten.

   `/opt/microsoft/dsc/Scripts/SetDscLocalConfigurationManager.py -configurationmof <path to metaconfiguration file>`

9. Stellen Sie über das Azure-Portal oder mithilfe von Cmdlets sicher, dass die Computer, die Sie integrieren möchten, jetzt als DSC-Knoten angezeigt werden, die in Ihrem Azure Automation-Konto registriert sind.

## <a name="generating-dsc-metaconfigurations"></a>Generieren von DSC-Metakonfigurationen

Sie können eine [DSC-Metakonfiguration](/powershell/scripting/dsc/managing-nodes/metaConfig) generieren, um jeden beliebigen Computer in Azure Automation State Configuration einzugliedern. Mit dieser Konfiguration wird der DSC-Agent dazu aufgefordert, von Azure Automation State Configuration zu pullen und/oder Informationen an Azure Automation State Configuration zu senden. Sie können eine DSC-Metakonfiguration für Azure Automation State Configuration sowohl über eine PowerShell DSC-Konfiguration als auch über die Azure Automation PowerShell-Cmdlets generieren.

> [!NOTE]
> DSC-Metakonfigurationen enthalten die notwendigen geheimen Schlüssel, um einen Computer in ein Automation-Konto für die Verwaltung zu integrieren. Stellen Sie den ordnungsgemäßen Schutz aller von Ihnen generierten DSC-Metakonfigurationen sicher oder löschen Sie diese nach der Verwendung.

Die Proxy-Unterstützung für Metakonfigurationen wird durch den LCM gesteuert, bei dem es sich um die Windows PowerShell DSC-Engine handelt. Der LCM wird auf allen Zielknoten ausgeführt und ist zuständig für das Aufrufen der Konfigurationsressourcen, die in einem DSC-Metakonfigurationsskript enthalten sind. Sie können die Proxy-Unterstützung in eine Metakonfiguration einschließen, indem Sie Definitionen der Proxy-URL sowie die Proxy-Anmeldeinformationen einbeziehen, wie sie in den Blöcken `ConfigurationRepositoryWeb`, `ResourceRepositoryWeb` und `ReportServerWeb` benötigt werden. Informationen finden Sie unter [Konfigurieren des lokalen Konfigurations-Managers](https://docs.microsoft.com/powershell/scripting/dsc/managing-nodes/metaconfig?view=powershell-7).

### <a name="generate-dsc-metaconfigurations-using-a-dsc-configuration"></a>Generieren von DSC-Metakonfigurationen mithilfe einer DSC-Konfiguration

1. Öffnen Sie VS Code (oder Ihren bevorzugten Editor) als Administrator auf einem Computer in Ihrer lokalen Umgebung. Auf dem Computer muss die neueste Version von [WMF 5](https://aka.ms/wmf5latest) installiert sein.
1. Kopieren Sie das folgende Skript in Ihre lokale Umgebung. Dieses Skript enthält eine PowerShell DSC-Konfiguration zum Erstellen von Metakonfigurationen und einen Befehl zum Starten der Metakonfigurationserstellung.

    > [!NOTE]
    > Bei State Configuration-Knotenkonfigurationsnamen wird im Azure-Portal die Groß-/Kleinschreibung beachtet. Bei abweichender Groß-/Kleinschreibung wird der Knoten nicht auf der Registerkarte **Knoten** angezeigt.

   ```powershell
   # The DSC configuration that will generate metaconfigurations
   [DscLocalConfigurationManager()]
   Configuration DscMetaConfigs
   {
        param
        (
            [Parameter(Mandatory=$True)]
            [String]$RegistrationUrl,

            [Parameter(Mandatory=$True)]
            [String]$RegistrationKey,

            [Parameter(Mandatory=$True)]
            [String[]]$ComputerName,

            [Int]$RefreshFrequencyMins = 30,

            [Int]$ConfigurationModeFrequencyMins = 15,

            [String]$ConfigurationMode = 'ApplyAndMonitor',

            [String]$NodeConfigurationName,

            [Boolean]$RebootNodeIfNeeded= $False,

            [String]$ActionAfterReboot = 'ContinueConfiguration',

            [Boolean]$AllowModuleOverwrite = $False,

            [Boolean]$ReportOnly
        )

        if(!$NodeConfigurationName -or $NodeConfigurationName -eq '')
        {
            $ConfigurationNames = $null
        }
        else
        {
            $ConfigurationNames = @($NodeConfigurationName)
        }

        if($ReportOnly)
        {
            $RefreshMode = 'PUSH'
        }
        else
        {
            $RefreshMode = 'PULL'
        }

        Node $ComputerName
        {
            Settings
            {
                RefreshFrequencyMins           = $RefreshFrequencyMins
                RefreshMode                    = $RefreshMode
                ConfigurationMode              = $ConfigurationMode
                AllowModuleOverwrite           = $AllowModuleOverwrite
                RebootNodeIfNeeded             = $RebootNodeIfNeeded
                ActionAfterReboot              = $ActionAfterReboot
                ConfigurationModeFrequencyMins = $ConfigurationModeFrequencyMins
            }

            if(!$ReportOnly)
            {
            ConfigurationRepositoryWeb AzureAutomationStateConfiguration
                {
                    ServerUrl          = $RegistrationUrl
                    RegistrationKey    = $RegistrationKey
                    ConfigurationNames = $ConfigurationNames
                }

                ResourceRepositoryWeb AzureAutomationStateConfiguration
                {
                    ServerUrl       = $RegistrationUrl
                    RegistrationKey = $RegistrationKey
                }
            }

            ReportServerWeb AzureAutomationStateConfiguration
            {
                ServerUrl       = $RegistrationUrl
                RegistrationKey = $RegistrationKey
            }
        }
   }

    # Create the metaconfigurations
    # NOTE: DSC Node Configuration names are case sensitive in the portal.
    # TODO: edit the below as needed for your use case
   $Params = @{
        RegistrationUrl = '<fill me in>';
        RegistrationKey = '<fill me in>';
        ComputerName = @('<some VM to onboard>', '<some other VM to onboard>');
        NodeConfigurationName = 'SimpleConfig.webserver';
        RefreshFrequencyMins = 30;
        ConfigurationModeFrequencyMins = 15;
        RebootNodeIfNeeded = $False;
        AllowModuleOverwrite = $False;
        ConfigurationMode = 'ApplyAndMonitor';
        ActionAfterReboot = 'ContinueConfiguration';
        ReportOnly = $False;  # Set to $True to have machines only report to AA DSC but not pull from it
   }

   # Use PowerShell splatting to pass parameters to the DSC configuration being invoked
   # For more info about splatting, run: Get-Help -Name about_Splatting
   DscMetaConfigs @Params
   ```

1. Geben Sie den Registrierungsschlüssel und die URL für Ihr Automation-Konto sowie die Namen der Computer ein, die integriert werden sollen. Alle anderen Parameter sind optional. Informationen zum Suchen von Registrierungsschlüssel und Registrierungs-URL für Ihr Automation-Konto finden Sie im Abschnitt [Sicheres Onboarding mithilfe einer Registrierung](#onboarding-securely-using-registration).

1. Wenn die Computer zwar DSC-Statusinformationen an Azure Automation State Configuration senden, jedoch keine Konfigurations- oder PowerShell-Module abrufen sollen, legen Sie den `ReportOnly`-Parameter auf TRUE fest.

1. Wenn `ReportOnly` nicht festgelegt wurde, senden die Computer DSC-Statusinformationen an Azure Automation State Configuration und pullen Konfigurations- oder PowerShell-Module. Legen Sie die Parameter in den Blöcken `ConfigurationRepositoryWeb`, `ResourceRepositoryWeb` und `ReportServerWeb` entsprechend fest.

1. Führen Sie das Skript aus. Ihr Arbeitsverzeichnis sollte nun einen Ordner namens **DscMetaConfigs** mit den PowerShell DSC-Metakonfigurationen der (als Administrator) zu integrierenden Computer enthalten.

    ```powershell
    Set-DscLocalConfigurationManager -Path ./DscMetaConfigs
    ```

### <a name="generate-dsc-metaconfigurations-using-azure-automation-cmdlets"></a>Generieren von DSC-Metakonfigurationen mithilfe von Azure Automation-Cmdlets

Wenn die Standardwerte des LCM von PowerShell DSC zu Ihrem Anwendungsfall passen und Sie Computer so integrieren möchten, dass diese sowohl Informationen von Azure Automation State Configuration abrufen als auch Informationen an Azure Automation State Configuration senden, können Sie die benötigten DSC-Metakonfigurationen mit den Azure Automation-Cmdlets einfacher generieren.

1. Öffnen Sie die PowerShell-Konsole oder VS Code als Administrator auf einem Computer in Ihrer lokalen Umgebung.
2. Stellen Sie mit `Connect-AzAccount` eine Verbindung mit Azure Resource Manager her.
3. Laden Sie von dem Automation-Konto, in dem Sie Knoten einrichten möchten, die PowerShell DSC-Metakonfigurationen für die Computer herunter, die Sie integrieren möchten.

   ```powershell
   # Define the parameters for Get-AzAutomationDscOnboardingMetaconfig using PowerShell Splatting
   $Params = @{
       ResourceGroupName = 'ContosoResources'; # The name of the Resource Group that contains your Azure Automation Account
       AutomationAccountName = 'ContosoAutomation'; # The name of the Azure Automation Account where you want a node on-boarded to
       ComputerName = @('web01', 'web02', 'sql01'); # The names of the computers that the meta configuration will be generated for
       OutputFolder = "$env:UserProfile\Desktop\";
   }
   # Use PowerShell splatting to pass parameters to the Azure Automation cmdlet being invoked
   # For more info about splatting, run: Get-Help -Name about_Splatting
   Get-AzAutomationDscOnboardingMetaconfig @Params
   ```

1. Ihr Arbeitsverzeichnis sollte nun einen Ordner namens **DscMetaConfigs**mit den PowerShell DSC-Metakonfigurationen der zu integrierenden Computer (als Administrator) enthalten.

    ```powershell
    Set-DscLocalConfigurationManager -Path $env:UserProfile\Desktop\DscMetaConfigs
    ```

## <a name="onboarding-securely-using-registration"></a>Sicheres Onboarding mithilfe einer Registrierung

Computer können über das WMF 5-DSC-Registrierungsprotokoll sicher in ein Azure Automation-Konto integriert werden. Mit diesem Protokoll kann sich ein DSC-Knoten bei einem PowerShell DSC-Pull- oder Berichtsserver, einschließlich Azure Automation State Configuration, authentifizieren. Der Knoten wird beim Server unter der Registrierungs-URL registriert und mit einem Registrierungsschlüssel authentifiziert. Während der Registrierung handeln der DSC-Knoten und der DSC-Pull-/Berichtsserver ein eindeutiges Zertifikat für den Knoten aus, das nach der Registrierung zur Authentifizierung beim Server verwendet wird. Durch diesen Prozess wird verhindert, dass integrierte Knoten die Identität eines anderen Knotens annehmen, z. B. wenn ein Knoten kompromittiert wurde und böswillige Absichten verfolgt. Nach der Registrierung wird der Registrierungsschlüssel nicht wieder für die Authentifizierung verwendet und vom Knoten gelöscht.

Die für das DSC-Registrierungsprotokoll erforderlichen Informationen finden Sie im Azure-Portal in den **Kontoeinstellungen** unter **Schlüssel**. 

![Azure Automation-Schlüssel und -URL](./media/automation-dsc-onboarding/DSC_Onboarding_4.png)

- Die Registrierungs-URL finden Sie unter dem Feld „URL“ auf der Seite „Schlüssel“.
- Beim Registrierungsschlüssel handelt es sich um den Wert des Felds **Primärer Zugriffsschlüssel** oder **Sekundärer Zugriffsschlüssel** auf der Seite „Schlüssel“. Beide Schlüssel können verwendet werden.

Sie können den primären und den sekundären Zugriffsschlüssel eines Automation-Kontos jederzeit auf der Seite „Schlüssel“ erneut generieren, was für zusätzliche Sicherheit sorgt. Durch die Neugenerierung der Schlüssel wird verhindert, dass bei zukünftigen Knotenregistrierungen vorherige Schlüssel verwendet werden.

## <a name="re-registering-a-node"></a>Erneute Registrierung eines Knoten

Nach der Registrierung eines Computers als DSC-Knoten in Azure Automation State Configuration gibt es mehrere Gründe, warum Sie diesen Knoten in Zukunft möglicherweise erneut registrieren müssen.

- **Zertifikatverlängerung:** Bei älteren Windows Server-Versionen als Windows Server 2019 handelt jeder Knoten automatisch ein eindeutiges Zertifikat für die Authentifizierung aus, das nach einem Jahr abläuft. Wenn ein Zertifikat ohne Verlängerung abläuft, kann der Knoten nicht mehr mit Azure Automation kommunizieren und wird als `Unresponsive` gekennzeichnet. Gegenwärtig kann das PowerShell DSC-Registrierungsprotokoll Zertifikate nicht automatisch verlängern, wenn sie sich dem Ablaufdatum nähern, weshalb Sie die Knoten nach einem Jahr noch mal registrieren müssen. Stellen Sie vor einer erneuten Registrierung sicher, dass auf jedem Knoten WMF 5 RTM ausgeführt wird. 

    Eine erneute Registrierung, die frühestens 90 Tage vor dem Ablaufzeitpunkt des Zertifikats oder zu einem beliebigen Zeitpunkt nach dessen Ablaufzeitpunkt durchgeführt wird, hat zur Folge, dass ein neues Zertifikat generiert und verwendet wird. Eine Lösung für dieses Problem ist in Windows Server 2019 und höher enthalten.

- **Änderungen an DSC-LCM-Werten:** Möglicherweise müssen Sie die [Werte des LCM von PowerShell DSC](/powershell/scripting/dsc/managing-nodes/metaConfig4) ändern, die Sie während der ersten Registrierung des Knotens festgelegt haben, z. B. `ConfigurationMode`. Diese DSC-Agent-Werte können Sie derzeit nur über eine erneute Registrierung ändern. Die einzige Ausnahme ist der Knotenkonfigurationswert, der dem Knoten zugewiesen wurde. Diesen können Sie direkt in Azure Automation DSC ändern.

Sie können einen Knoten auf die gleiche Weise wie bei der ersten Registrierung des Knotens mithilfe einer der in diesem Dokument beschriebenen Onboarding-Methoden erneut registrieren. Sie müssen die Registrierung eines Knotens in Azure Automation DSC nicht aufheben, bevor Sie ihn erneut registrieren.

## <a name="troubleshooting-azure-virtual-machine-onboarding"></a>Problembehandlung bei der Integration virtueller Azure-Computer

Mit Azure Automation DSC können Sie problemlos Azure-VMs für die Konfigurationsverwaltung integrieren. Die Azure-VM-Erweiterung für DSC wird verwendet, um virtuelle Computer bei Azure Automation DSC zu registrieren. Da die Azure-VM-Erweiterung für DSC asynchron ausgeführt wird, können die Überwachung des Fortschritts und die Problembehandlung bei der Ausführung wichtig sein.

> [!NOTE]
> Beim Onboarding einer Azure-VM in Azure Automation State Configuration mithilfe der Azure-VM-Erweiterung für DSC kann es bis zu einer Stunde dauern, bis in Azure Automation angezeigt wird, dass der Knoten registriert ist. Der Grund für diese Verzögerung ist die für das Onboarding des virtuellen Computers in Azure Automation State Configuration erforderliche Installation von WMF 5 auf dem virtuellen Computer durch die Azure-VM-Erweiterung für DSC.

So beheben Sie Probleme mit der Azure-VM-Erweiterung für DSC oder zeigen deren Status an:

1. Navigieren Sie im Azure-Portal zu dem virtuellen Computer, der integriert wird.
2. Klicken Sie unter **Einstellungen** auf **Erweiterungen**. 
3. Wählen Sie dann je nach Betriebssystem **DSC** oder **DSCForLinux** aus. 
4. Weitere Details erhalten Sie, indem Sie auf **Detaillierten Status anzeigen**klicken.

Weitere Informationen zur Problembehandlung finden Sie unter [Behandeln von Problemen mit der Azure Automation-Konfiguration des gewünschten Zustands (Desired State Configuration, DSC)](./troubleshoot/desired-state-configuration.md).

## <a name="next-steps"></a>Nächste Schritte

- Eine Einführung finden Sie unter [Erste Schritte mit Azure Automation State Configuration](automation-dsc-getting-started.md).
- Wie Sie DSC-Konfigurationen kompilieren und anschließend Zielknoten zuweisen, erfahren Sie unter [Kompilieren von DSC-Konfigurationen in Azure Automation DSC](automation-dsc-compile.md).
- Eine PowerShell-Cmdlet-Referenz ist unter [Azure Automation State Configuration-Cmdlets](/powershell/module/az.automation#automation) verfügbar.
- Eine Preisübersicht finden Sie unter [Automation – Preise](https://azure.microsoft.com/pricing/details/automation/).
- Ein Beispiel für die Verwendung von Azure Automation State Configuration in einer Continuous-Deployment-Pipeline finden Sie unter [Anwendungsbeispiel: Continuous Deployment auf virtuellen Computern mit Automation State Configuration und Chocolatey](automation-dsc-cd-chocolatey.md).
