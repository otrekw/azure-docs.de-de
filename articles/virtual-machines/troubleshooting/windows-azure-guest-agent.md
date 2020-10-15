---
title: Problembehandlung beim Windows Azure-Gast-Agent
description: Problembehandlung bei Fehlern des Typs „Windows Azure-Gast-Agent funktioniert nicht“
services: virtual-machines-windows
ms.service: virtual-machines-windows
author: dkdiksha
manager: dcscontentpm
editor: ''
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/15/2020
ms.author: genli
ms.openlocfilehash: 738c2a240ad6c88186357e69b02d33b40d366d7f
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91977064"
---
# <a name="troubleshooting-windows-azure-guest-agent"></a>Problembehandlung beim Windows Azure-Gast-Agent

Der Windows Azure-Gast-Agent ist ein Agent für virtuelle Computer (Virtual Machines, VMs). Er ermöglicht dem virtuellen Computer die Kommunikation mit dem Fabric Controller (dem zugrunde liegenden physischen Server, auf dem der virtuelle Computer gehostet wird) über die IP-Adresse 168.63.129.16. Dies ist eine virtuelle öffentliche IP-Adresse, die verwendet wird, um die Kommunikation zu vereinfachen. Unter [Was ist die IP-Adresse 168.63.129.16?](../../virtual-network/what-is-ip-address-168-63-129-16.md) finden Sie weitere Informationen dazu.

 Auf VMs, die aus der lokalen Umgebung zu Azure migriert oder mithilfe benutzerdefinierter Images erstellt werden, ist der Windows Azure-Gast-Agent nicht installiert. In diesen Szenarien müssen Sie den VM-Agent manuell installieren. Weitere Informationen zum Installieren des VM-Agents finden Sie unter [Übersicht über den Agent für virtuelle Azure-Computer](../extensions/agent-windows.md).

Nach der erfolgreichen Installation des Windows Azure-Gast-Agents werden auf der VM in der Datei „services.msc“ die folgenden Dienste aufgeführt:
 
- Windows Azure-Gast-Agent-Dienst
- Telemetriedienst
- RD-Agent-Dienst

**Windows Azure-Gast-Agent-Dienst**: Dieser Dienst ist für die gesamte Protokollierung in „WAppAgent.log“ verantwortlich. Der Dienst ist für die Konfiguration verschiedener Erweiterungen und für die Kommunikation zwischen Gast und Host verantwortlich. 

**Telemetriedienst**: Dieser Dienst ist für das Senden der Telemetriedaten der VM an den Back-End-Server verantwortlich.

**RD-Agent-Dienst**: Dieser Dienst ist für die Installation des Gast-Agents verantwortlich. Das transparente Installationsprogramm ist auch eine Komponente des RD-Agents, mit der andere Komponenten und Dienste des Gast-Agents aktualisiert werden können. RDAgent ist auch für das Senden von Heartbeats von der Gast-VM an den Host-Agent auf dem physischen Server verantwortlich.

> [!NOTE]
> Ab Version 2.7.41491.971 des VM-Gast-Agents ist die Telemetriekomponente im RDAgent-Dienst enthalten. Daher wird dieser Telemetriedienst auf neu erstellten VMs möglicherweise nicht aufgeführt.

## <a name="checking-agent-status-and-version"></a>Überprüfen des Agent-Status und der -Version

Navigieren Sie im Azure-Portal zur Seite mit den VM-Eigenschaften, und überprüfen Sie den **Agent-Status**. Wenn der Windows Azure-Gast-Agent ordnungsgemäß funktioniert, wird der Status **Bereit** angezeigt. Wenn der VM-Agent den Status **Nicht bereit** aufweist, funktionieren die Erweiterungen und **Skriptausführung** im Azure-Portal nicht.

## <a name="troubleshooting-vm-agent-that-is-in-not-ready-status"></a>Problembehandlung beim VM-Agent, der den Status „Nicht bereit“ aufweist

### <a name="step-1-check-whether-the-windows-azure-guest-agent-service-is-installed"></a>Schritt 1: Überprüfen, ob der Windows Azure-Gast-Agent-Dienst installiert ist

- Suchen nach dem Paket

    Suchen Sie den Ordner „C:\WindowsAzure“. Wenn der Ordner „GuestAgent“ mit der Versionsnummer angezeigt wird, bedeutet dies, dass der Windows Azure-Gast-Agent auf der VM installiert wurde. Sie können auch nach dem installierten Paket suchen.  Wenn der Windows Azure-Gast-Agent auf der VM installiert ist, wird das Paket am folgenden Speicherort gespeichert: `C:\windows\OEM\GuestAgent\VMAgentPackage.zip`.
    
    Sie können den folgenden PowerShell-Befehl ausführen, um zu überprüfen, ob der VM-Agent auf der VM bereitgestellt wurde:
    
    `Get-AzVM -ResourceGroupName "RGNAME" -Name "VMNAME" -DisplayHint expand`
    
    Suchen Sie in der Ausgabe nach der Eigenschaft **ProvisionVMAgent**, und überprüfen Sie, ob der Wert auf **True** festgelegt ist. Wenn dies der Fall ist, bedeutet das, dass der Agent auf der VM installiert ist.
    
- Überprüfen der Dienste und Prozesse

   Navigieren Sie zur Konsole „Dienste“ („services.msc“), und überprüfen Sie den Status der folgenden Dienste: Windows Azure-Gast-Agent-Dienst, RDAgent-Dienst, Windows Azure-Telemetriedienst und Windows Azure-Netzwerk-Agent-Dienst.
 
    Sie können auch überprüfen, ob diese Dienste ausgeführt werden, indem Sie den Task-Manager auf die folgenden Prozesse untersuchen:
       
    - WindowsAzureGuestAgent.exe: Windows Azure-Gast-Agent-Dienst
    - WaAppAgent.exe: RDAgent-Dienst
    - WindowsAzureNetAgent.exe: Windows Azure-Netzwerk-Agent-Dienst
    - WindowsAzureTelemetryService.exe: Windows Azure-Telemetriedienst

   Wenn Sie diese Prozesse und Dienste nicht finden können, deutet dies darauf hin, dass der Windows Azure-Gast-Agent nicht installiert ist.

- Überprüfen des Programms und Features

    Wechseln Sie in der Systemsteuerung zu **Programme und Features**, um zu ermitteln, ob der Windows Azure-Gast-Agent-Dienst installiert ist.

Wenn Sie keine ausgeführten Pakete, Dienste und Prozesse finden und der Windows Azure-Gast-Agent unter „Programme und Features“ nicht angezeigt wird, versuchen Sie, den [Windows Azure-Gast-Agent-Dienst zu installieren](../extensions/agent-windows.md). Wenn der Gast-Agent nicht ordnungsgemäß installiert wird, können Sie [den VM-Agent offline installieren](./install-vm-agent-offline.md).

Wenn die Dienste angezeigt und ausgeführt werden, starten Sie den Dienst neu, um zu überprüfen, ob das Problem behoben ist. Wenn die Dienste beendet wurden, starten Sie die Dienste, und warten Sie einige Minuten. Überprüfen Sie dann, ob der **Agent-Status** als **Bereit** angezeigt wird. Wenn Sie feststellen, dass diese Dienste abstürzen, verursachen möglicherweise einige Drittanbieterprozesse den Absturz dieser Dienste. Wenden Sie sich zur weiteren Problembehandlung an den [Microsoft-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="step-2-check-whether-auto-update-is-working"></a>Schritt 2: Überprüfen, ob die automatische Aktualisierung funktioniert

Der Windows Azure-Gast-Agent verfügt über eine Funktion für die automatische Aktualisierung. Sie sucht automatisch nach neuen Updates und installiert diese. Wenn die Funktion für die automatische Aktualisierung nicht ordnungsgemäß funktioniert, versuchen Sie, den Windows Azure-Gast-Agent durch Ausführen der folgenden Schritte zu deinstallieren und zu installieren:

1. Wenn der Windows Azure-Gast-Agent unter **Programme und Features** angezeigt wird, deinstallieren Sie den Agent.

2. Öffnen Sie ein Eingabeaufforderungsfenster mit Administratorberechtigungen.

3. Beenden Sie die Gast-Agent-Dienste. Wenn die Dienste nicht beendet werden, müssen Sie die Dienste auf **manuellen Start** festlegen und dann die VM neu starten.
    ```
    net stop rdagent
    net stop WindowsAzureGuestAgent
    net stop WindowsAzureTelemetryService
    ```
1. Löschen Sie die Gast-Agent-Dienste:
    ```
    sc delete rdagent
    sc delete WindowsAzureGuestAgent
    sc delete WindowsAzureTelemetryService
    ```
1. Erstellen Sie unter `C:\WindowsAzure` einen Ordner mit dem Namen „ALT“.

1. Verschieben Sie alle Ordner, die den Namen „Packages“ oder „GuestAgent“ aufweisen, in den Ordner „ALT“.

1. Laden Sie die aktuelle Version des Agent-Installationspakets [über diesen Link](https://go.microsoft.com/fwlink/?linkid=394789&clcid=0x409) herunter, und installieren Sie das Paket. Zum Durchführen der Installation benötigen Sie Administratorberechtigungen.

1. Installieren Sie den Gast-Agent mithilfe des folgenden Befehls:

    ```
    msiexec.exe /i c:\VMAgentMSI\WindowsAzureVmAgent.2.7.<version>.fre.msi /quiet /L*v c:\VMAgentMSI\msiexec.log
    ```
    Überprüfen Sie dann, ob die Gast-Agent-Dienste ordnungsgemäß starten.
 
    In seltenen Fällen, in denen der Gast-Agent nicht ordnungsgemäß installiert wird, können Sie [den VM-Agent offline installieren](./install-vm-agent-offline.md).
    

### <a name="step-3-check-whether-the-vm-can-connect-to-the-fabric-controller"></a>Schritt 3: Überprüfen, ob die VM eine Verbindung mit dem Fabric Controller herstellen kann

Verwenden Sie ein Tool wie PsPing, um zu testen, ob die VM über die Ports 80, 32526 und 443 eine Verbindung mit 168.63.129.16 herstellen kann. Wenn die VM nicht wie erwartet eine Verbindung herstellt, überprüfen Sie, ob die ausgehende Kommunikation über die Ports 80, 443 und 32526 in Ihrer lokalen Firewall auf der VM offen ist. Wenn diese IP-Adresse blockiert wurde, zeigt der VM-Agent in verschiedenen Szenarien möglicherweise unerwartetes Verhalten.

## <a name="advanced-troubleshooting"></a>Erweiterte Problembehandlung

Ereignisse der Problembehandlung beim Windows Azure-Gast-Agent werden in den folgenden Protokolldateien aufgezeichnet:

- C:\WindowsAzure\Logs\WaAppAgent.log
- C:\WindowsAzure\Logs\TransparentInstaller.log

Im Folgenden finden Sie einige häufige Szenarien, in denen der Windows Azure-Gast-Agent den Status **Nicht bereit** aufweisen kann oder nicht wie erwartet funktioniert.

### <a name="agent-stuck-on-starting"></a>Agent hängt beim Startvorgang

In der Protokolldatei „WaAppAgent.log“ können Sie sehen, dass der Agent beim Startvorgang hängen bleibt und nicht gestartet werden kann.

**Protokollieren von Informationen**
 
[00000007] [05/28/2019 12:58:50.90] [INFO] WindowsAzureGuestAgent starting. Version 2.7.41491.901

**Analyse**
 
Auf der VM wird noch die ältere Version des Windows Azure-Gast-Agents ausgeführt. Im Ordner „C:\WindowsAzure“ sehen Sie möglicherweise, dass mehrere Instanzen des Windows Azure-Gast-Agents installiert sind, einschließlich mehrerer derselben Version. Weil mehrere Agent-Instanzen installiert sind, kann die neueste Version des Windows Azure-Gast-Agents nicht auf der VM gestartet werden.

**Lösung**

Deinstallieren Sie den Windows Azure-Gast-Agent manuell, und installieren Sie ihn dann erneut. Führen Sie dazu die folgenden Schritte aus:

1. Öffnen Sie „Systemsteuerung“ > „Programme und Features“, und deinstallieren Sie den Windows Azure-Gast-Agent.
1. Öffnen Sie den Task-Manager, und beenden Sie die folgenden Dienste: Windows Azure-Gast-Agent-Dienst, RDAgent-Dienst, Windows Azure-Telemetriedienst und Windows Azure-Netzwerk-Agent-Dienst.
1. Erstellen Sie unter „C:\WindowsAzure“ einen Ordner mit dem Namen „ALT“.
1. Verschieben Sie alle Ordner, die den Namen „Packages“ oder „GuestAgent“ aufweisen, in den Ordner „ALT“. Verschieben Sie auch alle im Verzeichnis „C:\WindowsAzure\logs“ enthaltenen „GuestAgent“-Ordner, die mit „GuestAgent_x.x.xxxxx“ beginnen, in den Ordner „ALT“.
1. Laden Sie die aktuelle Version des MSI-Agents herunter, und installieren Sie den Agent. Sie benötigen Administratorberechtigungen, um die Installation durchführen zu können.
1. Installieren Sie den Gast-Agent mithilfe des folgenden MSI-Befehls:
    ```
    msiexec.exe /i c:\VMAgentMSI\WindowsAzureVmAgent.2.7.<version>.fre.msi /quiet /L*v c:\VMAgentMSI\msiexec.log`
    ```
1. Vergewissern Sie sich, dass der RDAgent-Dienst, der Windows Azure-Gast-Agent-Dienst und der Windows Azure-Telemetriedienst jetzt ausgeführt werden.
 
1. Überprüfen Sie die Datei „WaAppAgent.log“, um sicherzustellen, dass die aktuelle Version des Windows Azure-Gast-Agents ausgeführt wird.
 
1. Löschen Sie den Ordner „ALT“ aus dem Verzeichnis „C:\WindowsAzure“.
  
### <a name="unable-to-connect-to-wireserver-ip-host-ip"></a>Es kann keine Verbindung mit der WireServer-IP (Host-IP) hergestellt werden 

Sie bemerken die folgenden Fehlereinträge in den Dateien „WaAppAgent.log“ und „Telemetry.log“:

**Protokollieren von Informationen**

```Log sample
[ERROR] GetVersions() failed with exception: Microsoft.ServiceModel.Web.WebProtocolException: Server Error: Service Unavailable (ServiceUnavailable) ---> 
System.Net.WebException: The remote server returned an error: (503) Server Unavailable.
```
```Log sample
[00000011] [12/11/2018 06:27:55.66] [WARN]  (Ignoring) Exception while fetching supported versions from HostGAPlugin: System.Net.WebException: Unable to connect to the remote server 
---> System.Net.Sockets.SocketException: An attempt was made to access a socket in a way forbidden by its access permissions 168.63.129.16:32526
at System.Net.Sockets.Socket.DoConnect(EndPoint endPointSnapshot, SocketAddress socketAddress)
at System.Net.ServicePoint.ConnectSocketInternal(Boolean connectFailure, Socket s4, Socket s6, Socket& socket, IPAddress& address, ConnectSocketState status, IAsyncResult asyncResult, Exception& exception)
--- End of inner exception stack trace ---
at System.Net.WebClient.DownloadDataInternal(Uri address, WebRequest& request)
at System.Net.WebClient.DownloadString(Uri address)
at Microsoft.GuestAgentHostPlugin.Client.GuestInformationServiceClient.GetVersions()
at Microsoft.WindowsAzure.GuestAgent.ContainerStateMachine.HostGAPluginUtility.UpdateHostGAPluginAvailability()`
```
**Analyse**

Die VM kann den WireServer-Hostserver nicht erreichen.

**Lösung**

1. Da WireServer nicht erreichbar ist, stellen Sie über Remotedesktop eine Verbindung mit der VM her, und versuchen Sie dann, über einen Internetbrowser auf die folgende URL zuzugreifen: http://168.63.129.16/?comp=versions 
1. Wenn Sie die URL aus Schritt 1 nicht erreichen können, überprüfen Sie die Netzwerkschnittstelle, um zu ermitteln, ob Sie als DHCP-fähig festgelegt ist und über DNS verfügt. Führen Sie zum Überprüfen des DHCP-Status der Netzwerkschnittstelle den Befehl `netsh interface ip show config` aus.
1. Wenn DHCP deaktiviert ist, führen Sie den folgenden Befehl aus. Stellen Sie dabei sicher, dass Sie den Wert in Gelb in den Namen Ihrer Schnittstelle ändern: `netsh interface ip set address name="Name of the interface" source=dhcp`.
1. Suchen Sie nach Problemen, die möglicherweise von einer Firewall, einem Proxy oder einer anderen Quelle verursacht werden, die den Zugriff auf die IP-Adresse 168.63.129.16 blockieren könnten.
1. Überprüfen Sie, ob die Windows-Firewall oder eine Firewall eines Drittanbieters den Zugriff auf die Ports 80, 443 und 32526 blockiert. Unter [Was ist die IP-Adresse 168.63.129.16?](../../virtual-network/what-is-ip-address-168-63-129-16.md) finden Sie weitere Informationen dazu, warum diese Adresse nicht blockiert werden sollte.

### <a name="guest-agent-is-stuck-stopping"></a>Der Gast-Agent hängt beim Beenden  

Sie bemerken die folgenden Fehlereinträge in der Datei „WaAppAgent.log“:

**Protokollieren von Informationen** 

```Log sample
[00000007] [07/18/2019 14:46:28.87] [WARN] WindowsAzureGuestAgent stopping.
[00000007] [07/18/2019 14:46:28.89] [INFO] Uninitializing StateExecutor with WaitForTerminalStateReachedOnEnd : True
[00000004] [07/18/2019 14:46:28.89] [WARN] WindowsAzureGuestAgent could not be stopped. Exception: System.NullReferenceException: Object reference not set to an instance of an object.
at Microsoft.WindowsAzure.GuestAgent.ContainerStateMachine.GoalStateExecutorBase.WaitForExtensionWorkflowComplete(Boolean WaitForTerminalStateReachedOnEnd)
at Microsoft.WindowsAzure.GuestAgent.ContainerStateMachine.GoalStateExecutorBase.Uninitialize(Boolean WaitForTerminalStateReachedOnEnd)
at Microsoft.WindowsAzure.GuestAgent.ContainerStateMachine.GoalStateExecutorForCloud.Uninitialize(Boolean WaitForTerminalStateReachedOnEnd)
at Microsoft.WindowsAzure.GuestAgent.AgentCore.AgentCore.Stop(Boolean waitForTerminalState)
at Microsoft.WindowsAzure.GuestAgent.AgentCore.AgentService.DoStopService()
at Microsoft.WindowsAzure.GuestAgent.AgentCore.AgentService.<>c__DisplayClass2.<OnStopProcessing>b__1()
```
**Analyse**

Der Windows Azure-Gast-Agent bleibt beim Beenden hängen.

**Lösung**

1. Stellen Sie sicher, dass die Datei „WaAppAgent.exe“ auf der VM ausgeführt wird. Wenn die Datei nicht ausgeführt wird, starten Sie den RDAgent-Dienst neu, und warten Sie fünf Minuten. Wenn „WaAppAgent.exe“ ausgeführt wird, beenden Sie den Prozess „WindowsAzureGuest.exe“.
2. Wenn das Problem durch Schritt 1 nicht behoben werden konnte, entfernen Sie die aktuell installierte Version, und installieren Sie die aktuelle Version des Agents manuell.

### <a name="npcap-loopback-adapter"></a>Npcap-Loopbackadapter 

Sie bemerken die folgenden Fehlereinträge in der Datei „WaAppAgent.log“:
 
```Log sample
[00000006] [06/20/2019 07:44:28.93] [INFO]  Attempting to discover fabric address on interface Npcap Loopback Adapter.
[00000024] [06/20/2019 07:44:28.93] [WARN]  Empty DHCP option data returned
[00000006] [06/20/2019 07:44:28.93] [ERROR] Did not discover fabric address on interface Npcap Loopback Adapter
```
**Analyse**

Der Npcap-Loopbackadapter wurde von Wireshark auf der VM installiert. Wireshark ist ein Open-Source-Tool zur Profilerstellung des Netzwerkdatenverkehrs und zum Analysieren von Paketen. Ein solches Tool wird häufig als Netzwerkanalysetool, Netzwerkprotokoll-Analysetool oder Sniffer bezeichnet.

**Lösung**

Der Npcap-Loopbackadapter wurde wahrscheinlich von Wireshark installiert. Versuchen Sie, ihn zu deaktivieren, und überprüfen Sie dann, ob das Problem behoben ist.

## <a name="next-steps"></a>Nächste Schritte

Zur weiteren Problembehandlung des Fehlers „Windows Azure-Gast-Agent funktioniert nicht“ [wenden Sie sich an den Microsoft-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).