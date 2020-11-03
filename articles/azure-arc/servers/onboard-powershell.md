---
title: Verbinden von Hybridcomputern mit Azure mithilfe von PowerShell
description: In diesem Artikel erfahren Sie, wie Sie Azure Arc-fähige Server mit PowerShell verwenden, um den Agent zu installieren und einen Computer mit Azure zu verbinden.
ms.date: 10/28/2020
ms.topic: conceptual
ms.openlocfilehash: 0755846ef02377edade98b69e478908a111ab247
ms.sourcegitcommit: 693df7d78dfd5393a28bf1508e3e7487e2132293
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2020
ms.locfileid: "92901531"
---
# <a name="connect-hybrid-machines-to-azure-using-powershell"></a>Verbinden von Hybridcomputern mit Azure mithilfe von PowerShell

Azure Arc-fähige Server können mithilfe einiger manueller Schritte für Windows- oder Linux-Computer in Ihrer Umgebung aktiviert werden. Alternativ können Sie das PowerShell-Cmdlet [Connect-AzConnectedMachine](/powershell/module/az.connectedmachine/remove-azconnectedmachine) verwenden, um den Connected Machine-Agent herunterzuladen und zu installieren und den Computer bei Azure Arc zu registrieren. Sie können das Windows Installer-Paket für den Windows-Agent im Microsoft Download Center und das Paket für den Linux-Agent im Microsoft-Paketrepository herunterladen.

Für diese Methode müssen Sie auf dem Computer über Administratorberechtigungen verfügen, um den Agent installieren und konfigurieren zu können. Unter Linux muss hierfür das root-Konto verwendet werden. Unter Windows müssen Sie der Gruppe der lokalen Administratoren angehören. Sie können diesen Prozess mithilfe von [PowerShell-Remoting](/powershell/scripting/learn/ps101/08-powershell-remoting) interaktiv oder remote auf einem Windows-Server ausführen.

Bevor Sie beginnen, sollten Sie die [Voraussetzungen](agent-overview.md#prerequisites) überprüfen und sicherstellen, dass Ihr Abonnement und Ihre Ressourcen auch diese Anforderungen erfüllen. Weitere Informationen zu unterstützten Regionen und andere Überlegungen finden Sie unter [Unterstützte Azure-Regionen](overview.md#supported-regions).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Computer mit Azure PowerShell. Anweisungen hierzu finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/).

Bevor Sie Azure PowerShell zum Verwalten von VM-Erweiterungen auf Ihrem Hybridserver, der von Arc-fähigen Servern verwaltet wird, verwenden können, müssen Sie das Modul `Az.ConnectedMachine` installieren. Führen Sie auf Ihrem Arc-fähigen Server den folgenden Befehl aus:

```powershell
Install-Module -Name Az.ConnectedMachine
```

Nach Abschluss der Installation wird die folgende Meldung zurückgegeben:

`The installed extension ``Az.ConnectedMachine`` is experimental and not covered by customer support. Please use with discretion.`

## <a name="install-the-agent-and-connect-to-azure"></a>Installieren des Agents und Herstellen einer Verbindung mit Azure

1. Öffnen Sie eine PowerShell-Konsole mit erhöhten Rechten.

2. Melden Sie sich bei Azure an, indem Sie den Befehl `Connect-AzAccount` ausführen.

3. Verwenden Sie `Connect-AzConnectedMachine` mit den Parametern `-Name`, `-ResourceGroupName` und `-Location`, um den Connected Machine-Agent zu installieren. Verwenden Sie den Parameter `-SubscriptionId`, um das Standardabonnement als Ergebnis des Azure-Kontexts zu überschreiben, das nach der Anmeldung erstellt wurde. Führen Sie einen der folgenden Befehle aus:

    * Führen Sie den folgenden Befehl aus, um den Connected Machine-Agent auf dem Zielcomputer zu installieren, der direkt mit Azure kommunizieren kann:

        ```azurepowershell
        Connect-AzConnectedMachine -ResourceGroupName myResourceGroup -Name myMachineName -Location <region> -SubscriptionId 978ab182-6cf0-4de3-a58b-53c8d0a3235e
        ```
    
    * Führen Sie den folgenden Befehl aus, um den Connected Machine-Agent auf dem Zielcomputer zu installieren, der über einen Proxyserver kommunizieren kann:
        
        ```azurepowershell
        Connect-AzConnectedMachine -ResourceGroupName myResourceGroup -Name myMachineName -Location <region> -SubscriptionId 978ab182-6cf0-4de3-a58b-53c8d0a3235e -proxy http://<proxyURL>:<proxyport>
        ```

Sollte der Agent nach Abschluss des Setups nicht gestartet werden, suchen Sie in den Protokollen nach ausführlichen Fehlerinformationen. Bei Windows finden Sie diese unter *%ProgramData%\AzureConnectedMachineAgent\Log\himds.log* und bei Linux unter */var/opt/azcmagent/log/himds.log*.

## <a name="install-and-connect-using-powershell-remoting"></a>Installieren und Verbinden mithilfe von PowerShell-Remoting

Führen Sie die folgenden Schritte aus, um einen oder mehrere Windows-Server mit Azure Arc-fähigen Servern zu konfigurieren. PowerShell-Remoting muss auf dem Remotecomputer aktiviert sein. Verwenden Sie das Cmdlet `Enable-PSRemoting`, um PowerShell-Remoting zu aktivieren.

1. Öffnen Sie eine PowerShell-Konsole als Administrator.

2. Melden Sie sich bei Azure an, indem Sie den Befehl `Connect-AzAccount` ausführen.

3. Verwenden Sie `Connect-AzConnectedMachine` mit den Parametern `-Name`, `-ResourceGroupName` und `-Location`, um den Connected Machine-Agent zu installieren. Verwenden Sie den Parameter `-SubscriptionId`, um das Standardabonnement als Ergebnis des Azure-Kontexts zu überschreiben, das nach der Anmeldung erstellt wurde.

    * Führen Sie den folgenden Befehl aus, um den Connected Machine-Agent auf dem Zielcomputer zu installieren, der direkt mit Azure kommunizieren kann:
    
        ```azurepowershell
        $session = Connect-PSSession -ComputerName myMachineName
        Connect-AzConnectedMachine -ResourceGroupName myResourceGroup -Name myMachineName -Location <region> -PSSession $session
        ```
    
    * Um den Connected Machine-Agent gleichzeitig auf mehreren Remotecomputern zu installieren, fügen Sie eine Liste mit den durch Kommas getrennten Namen der Remotecomputer hinzu.

        ```azurepowershell
        $session = Connect-PSSession -ComputerName myMachineName1, myMachineName2, myMachineName3
        Connect-AzConnectedMachine -ResourceGroupName myResourceGroup -Name myMachineName -Location <region> -PSSession $session
        ```

    Im folgenden Beispiel werden die Ergebnisse des Befehls mit einem einzelnen Computer als Ziel aufgeführt:
    
    ```azurepowershell
    time="2020-08-07T13:13:25-07:00" level=info msg="Onboarding Machine. It usually takes a few minutes to complete. Sometimes it may take longer depending on network and server load status."
    time="2020-08-07T13:13:25-07:00" level=info msg="Check network connectivity to all endpoints..."
    time="2020-08-07T13:13:29-07:00" level=info msg="All endpoints are available... continue onboarding"
    time="2020-08-07T13:13:50-07:00" level=info msg="Successfully Onboarded Resource to Azure" VM Id=f65bffc7-4734-483e-b3ca-3164bfa42941
    
    Name           Location OSName   Status     ProvisioningState
    ----           -------- ------   ------     -----------------
    myMachineName  eastus   windows  Connected  Succeeded
    ```

## <a name="verify-the-connection-with-azure-arc"></a>Überprüfen der Verbindung mit Azure Arc

Vergewissern Sie sich im Azure-Portal, dass die Serververbindung erfolgreich hergestellt wurde, nachdem Sie den Agent installiert und für die Verbindungsherstellung mit Azure Arc-fähigen Servern konfiguriert haben. Zeigen Sie Ihre Computer im [Azure-Portal](https://portal.azure.com) an.

![Erfolgreiche Serververbindung](./media/onboard-portal/arc-for-servers-successful-onboard.png)

## <a name="next-steps"></a>Nächste Schritte

* Informationen zur Problembehandlung finden Sie im [Handbuch zur Problembehandlung des Connected Machine-Agents](troubleshoot-agent-onboard.md).

* Erfahren Sie, wie Sie Ihren Computer mithilfe von [Azure Policy](../../governance/policy/overview.md) verwalten, wie z. B. bei der VM-[Gastkonfiguration](../../governance/policy/concepts/guest-configuration.md), dem Überprüfen, ob der Computer dem erwarteten Log Analytics-Arbeitsbereich Bericht erstattet, beim Aktivieren der Überwachung mit [Azure Monitor mit VMs](../../azure-monitor/insights/vminsights-enable-policy.md) und vieles mehr.

* Weitere Informationen zum [Log Analytics-Agent](../../azure-monitor/platform/log-analytics-agent.md). Der Log Analytics-Agent für Windows und Linux ist erforderlich, wenn Sie Daten zur Betriebssystem- und Workloadüberwachung erfassen, diese mithilfe von Automation Runbooks oder Funktionen wie Updateverwaltung oder andere Azure-Dienste wie [Azure Security Center](../../security-center/security-center-introduction.md) nutzen möchten.