---
title: Problembehandlung bei der selbstgehosteten Integration Runtime in Azure Data Factory
description: Hier erfahren Sie, wie Sie eine Problembehandlung im Fall von Problemen bei der selbstgehosteten Integration Runtime in Azure Data Factory durchführen können.
services: data-factory
author: nabhishek
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 11/07/2019
ms.author: abnarain
ms.openlocfilehash: 94e214c55a0109beb85cd08ce87303e5bd0f8016
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83835426"
---
# <a name="troubleshoot-self-hosted-integration-runtime"></a>Problembehandlung bei der selbstgehosteten Integration Runtime

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

In diesem Artikel werden allgemeine Methoden zur Problembehandlung bei selbstgehosteter Integration Runtime in Azure Data Factory erläutert.

## <a name="common-errors-and-resolutions"></a>Häufige Fehler und deren Lösungen

### <a name="error-message-self-hosted-integration-runtime-cant-connect-to-cloud-service"></a>Fehlermeldung: Selbstgehostete Integration Runtime kann keine Verbindung mit dem Clouddienst herstellen

![Verbindungsproblem bei der selbstgehosteten Integration Runtime (IR)](media/self-hosted-integration-runtime-troubleshoot-guide/unable-to-connect-to-cloud-service.png)

#### <a name="cause"></a>Ursache 

Die selbstgehostete Integration Runtime kann keine Verbindung zum Azure Data Factory-Dienst herstellen (Back-End). Dieses Problem wird in der Regel durch Netzwerkeinstellungen in der Firewall verursacht.

#### <a name="resolution"></a>Lösung

1. Überprüfen Sie, ob der Integration Runtime-Dienst ausgeführt wird.
    
   ![Ausführungsstatus des selbstgehosteten Integration Runtime-Diensts](media/self-hosted-integration-runtime-troubleshoot-guide/integration-runtime-service-running-status.png)
    
1. Wenn der Dienst ausgeführt wird, gehen Sie zu Schritt 3.

1. Wenn kein Proxy für die selbstgehostete Integration Runtime konfiguriert ist (Standardeinstellung), führen Sie den folgenden PowerShell-Befehl auf dem Computer aus, auf dem die selbstgehostete Integration Runtime installiert wurde:

    ```powershell
    (New-Object System.Net.WebClient).DownloadString("https://wu2.frontend.clouddatahub.net/")
    ```
        
   > [!NOTE]     
   > Die Dienst-URL kann abhängig vom jeweiligen Data Factory-Speicherort abweichen. Sie finden die Dienst-URL unter **ADF-Benutzeroberfläche** > **Verbindungen** > **Integration Runtimes** > **Selfgehostete IR bearbeiten** > **Knoten** > **Dienst-URLs anzeigen**.
            
    Die folgende Antwort wird erwartet:
            
    ![Antwort des PowerShell-Befehls](media/self-hosted-integration-runtime-troubleshoot-guide/powershell-command-response.png)
            
1. Wenn Sie nicht die erwartete Antwort erhalten, verwenden Sie abhängig von der jeweiligen Situation eine der folgenden Methoden:
            
    * Wenn Sie die Meldung erhalten, dass der Remotename nicht aufgelöst werden konnte, gibt es ein Problem mit dem Domain Name System (DNS). Wenden Sie sich an das Netzwerkteam, um dieses Problem zu beheben.
    * Wenn Sie die Meldung erhalten, dass das SSL/TLS-Zertifikat ist nicht vertrauenswürdig ist, überprüfen Sie, ob das Zertifikat für https://wu2.frontend.clouddatahub.net/ auf dem Computer vertrauenswürdig ist, und installieren Sie dann das öffentliche Zertifikat mit dem Zertifikat-Manager. Durch diese Aktion sollte das Problem behoben werden.
    * Wechseln Sie zu **Windows** > **Ereignisanzeige (Protokolle)**  > **Anwendungs- und Dienstprotokolle** > **Integration Runtime**, und prüfen Sie, ob Fehler auftreten, die durch DNS, eine Firewallregel oder Unternehmensnetzwerkeinstellungen verursacht werden. (Wenn Sie einen solchen Fehler feststellen, müssen Sie die Verbindung zwangsbeenden.) Da jedes Unternehmen über angepasste Netzwerkeinstellungen verfügt, wenden Sie sich an das Netzwerkteam, um diese Probleme zu beheben.

1. Wenn „Proxy“ in der selbstgehosteten Integration Runtime konfiguriert wurde, überprüfen Sie, ob Ihr Proxyserver auf den Dienstendpunkt zugreifen kann. Ein Beispiel für einen Befehl finden Sie unter [PowerShell, Webanforderungen und Proxys](https://stackoverflow.com/questions/571429/powershell-web-requests-and-proxies).    
                
    ```powershell
    $user = $env:username
    $webproxy = (get-itemproperty 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet
    Settings').ProxyServer
    $pwd = Read-Host "Password?" -assecurestring
    $proxy = new-object System.Net.WebProxy
    $proxy.Address = $webproxy
    $account = new-object System.Net.NetworkCredential($user,[Runtime.InteropServices.Marshal]::PtrToStringAuto([Runtime.InteropServices.Marshal]::SecureStringToBSTR($pwd)), "")
    $proxy.credentials = $account
    $url = "https://wu2.frontend.clouddatahub.net/"
    $wc = new-object system.net.WebClient
    $wc.proxy = $proxy
    $webpage = $wc.DownloadData($url)
    $string = [System.Text.Encoding]::ASCII.GetString($webpage)
    $string
    ```

Die folgende Antwort wird erwartet:
            
![Antwort 2 des PowerShell-Befehls](media/self-hosted-integration-runtime-troubleshoot-guide/powershell-command-response.png)

> [!NOTE] 
> Proxy-Aspekte:
> *    Überprüfen Sie, ob der Proxyserver in die Liste der sicheren Empfänger eingefügt werden muss. Stellen Sie in diesem Fall sicher, dass [diese Domänen](https://docs.microsoft.com/azure/data-factory/data-movement-security-considerations#firewall-requirements-for-on-premisesprivate-network) in der Liste der sicheren Empfänger enthalten sind.
> *    Überprüfen Sie, ob das TLS/SSL-Zertifikat „wu2.frontend.clouddatahub.net/“ auf dem Proxyserver vertrauenswürdig ist.
> *    Wenn Sie im Proxy die Active Directory-Authentifizierung verwenden, ändern Sie das Dienstkonto in das Benutzerkonto, das als „Integration Runtime-Dienst“ auf den Proxy zugreifen kann.

### <a name="error-message-self-hosted-integration-runtime-node-logical-shir-is-in-inactive-running-limited-state"></a>Fehlermeldung: „Self-hosted integration runtime node/ logical SHIR is in Inactive/ „Running (Limited)“ state“ (Knoten der selbstgehosteten Integration Runtime / logische SHIR befindet sich im Status „Inaktiv/Wird ausgeführt (eingeschränkt)“)

#### <a name="cause"></a>Ursache 

Der selbstgehostete Integrated Runtime-Knoten weist wie im folgenden Screenshot möglicherweise den Status **Inaktiv** auf:

![Inaktiver Knoten der selbstgehosteten IR](media/self-hosted-integration-runtime-troubleshoot-guide/inactive-self-hosted-ir-node.png)

Dieses Verhalten tritt auf, wenn Knoten nicht miteinander kommunizieren können.

#### <a name="resolution"></a>Lösung

1. Melden Sie sich bei dem virtuellen Computer an, der auf dem Knoten gehostet wird. Öffnen Sie unter **Anwendungs-und Dienstprotokolle** > **Integration Runtime** die Ereignisanzeige und filtern Sie alle Fehlerprotokolle.

1. Überprüfen Sie, ob das Fehlerprotokoll den folgenden Fehler enthält: 
    
    ```System.ServiceModel.EndpointNotFoundException: Could not connect to net.tcp://xxxxxxx.bwld.com:8060/ExternalService.svc/WorkerManager. The connection attempt lasted for a time span of 00:00:00.9940994. TCP error code 10061: No connection could be made because the target machine actively refused it 10.2.4.10:8060. 
    System.Net.Sockets.SocketException: No connection could be made because the target machine actively refused it. 
    10.2.4.10:8060
        
    at System.Net.Sockets.Socket.DoConnect(EndPoint endPointSnapshot, SocketAddress socketAddress)
               
    at System.Net.Sockets.Socket.Connect(EndPoint remoteEP)
               
    at System.ServiceModel.Channels.SocketConnectionInitiator.Connect(Uri uri, TimeSpan timeout)
       
1. If you see this error, run the following on a command line: 

   **telnet 10.2.4.10 8060**.
1. If you receive the following error, contact your IT department for help with fixing this issue. After you can successfully telnet, contact Microsoft Support if you still have issues with the integrative runtime node status.
        
   ![Command-line error](media/self-hosted-integration-runtime-troubleshoot-guide/command-line-error.png)
        
1.    Check whether the error log contains the following:

    ```Error log: Cannot connect to worker manager: net.tcp://xxxxxx:8060/ExternalService.svc/ No DNS entries exist for host azranlcir01r1. No such host is known Exception detail: System.ServiceModel.EndpointNotFoundException: No DNS entries exist for host xxxxx. ---> System.Net.Sockets.SocketException: No such host is known at System.Net.Dns.GetAddrInfo(String name) at System.Net.Dns.InternalGetHostByName(String hostName, Boolean includeIPv6) at System.Net.Dns.GetHostEntry(String hostNameOrAddress) at System.ServiceModel.Channels.DnsCache.Resolve(Uri uri) --- End of inner exception stack trace --- Server stack trace: at System.ServiceModel.Channels.DnsCache.Resolve(Uri uri)```
    
1. To resolve the issue, try one or both of the following methods:
    - Put all the nodes in the same domain.
    - Add the IP to host mapping in all the hosted VM's host files.


## Next steps

For more help with troubleshooting, try the following resources:

*  [Data Factory blog](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory feature requests](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure videos](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Microsoft Q&A question page](https://docs.microsoft.com/answers/topics/azure-data-factory.html)
*  [Stack overflow forum for Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Twitter information about Data Factory](https://twitter.com/hashtag/DataFactory)
*  [Mapping data flows performance guide](concepts-data-flow-performance.md)
