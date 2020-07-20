---
title: Problembehandlung bei der selbstgehosteten Integration Runtime in Azure Data Factory
description: Hier erfahren Sie, wie Sie eine Problembehandlung im Fall von Problemen bei der selbstgehosteten Integration Runtime in Azure Data Factory durchführen können.
services: data-factory
author: nabhishek
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 06/24/2020
ms.author: abnarain
ms.openlocfilehash: e77d621d5699c434e691de0a523e58e49166d8d6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85315072"
---
# <a name="troubleshoot-self-hosted-integration-runtime"></a>Problembehandlung bei der selbstgehosteten Integration Runtime

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

In diesem Artikel werden allgemeine Methoden zur Problembehandlung bei selbstgehosteter Integration Runtime in Azure Data Factory erläutert.

## <a name="common-errors-and-resolutions"></a>Häufige Fehler und deren Lösungen

### <a name="error-message"></a>Fehlermeldung: 

`Self-hosted integration runtime can't connect to cloud service`

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

### <a name="error-message"></a>Fehlermeldung: 
`Self-hosted integration runtime node/ logical SHIR is in Inactive/ "Running (Limited)" state`

#### <a name="cause"></a>Ursache 

Der selbstgehostete Integrated Runtime-Knoten weist wie im folgenden Screenshot möglicherweise den Status **Inaktiv** auf:

![Inaktiver Knoten der selbstgehosteten IR](media/self-hosted-integration-runtime-troubleshoot-guide/inactive-self-hosted-ir-node.png)

Dieses Verhalten tritt auf, wenn Knoten nicht miteinander kommunizieren können.

#### <a name="resolution"></a>Lösung

1. Melden Sie sich bei dem virtuellen Computer an, der auf dem Knoten gehostet wird. Öffnen Sie unter **Anwendungs-und Dienstprotokolle** > **Integration Runtime** die Ereignisanzeige und filtern Sie alle Fehlerprotokolle.

1. Überprüfen Sie, ob das Fehlerprotokoll den folgenden Fehler enthält: 
    
    ```
    System.ServiceModel.EndpointNotFoundException: Could not connect to net.tcp://xxxxxxx.bwld.com:8060/ExternalService.svc/WorkerManager. The connection attempt lasted for a time span of 00:00:00.9940994. TCP error code 10061: No connection could be made because the target machine actively refused it 10.2.4.10:8060. 
    System.Net.Sockets.SocketException: No connection could be made because the target machine actively refused it. 
    10.2.4.10:8060
    at System.Net.Sockets.Socket.DoConnect(EndPoint endPointSnapshot, SocketAddress socketAddress)
    at System.Net.Sockets.Socket.Connect(EndPoint remoteEP)
    at System.ServiceModel.Channels.SocketConnectionInitiator.Connect(Uri uri, TimeSpan timeout)
    ```
       
1. Wenn dieser Fehler angezeigt wird, führen Sie den folgenden Befehl in einer Befehlszeile aus: 

   ```
   telnet 10.2.4.10 8060
   ```
   
1. Wenn der folgende Fehler angezeigt wird, wenden Sie sich an Ihre IT-Abteilung, um Hilfe bei der Lösung dieses Problems zu erhalten. Nachdem Sie Telnet erfolgreich eingerichtet haben und verwenden können, wenden Sie sich an den Microsoft-Support, wenn Sie weiterhin Probleme mit dem Status des Integrated Runtime-Knotens haben.
        
   ![Befehlszeilenfehler](media/self-hosted-integration-runtime-troubleshoot-guide/command-line-error.png)
        
1. Überprüfen Sie, ob das Fehlerprotokoll Folgendes enthält:

    ```
    Error log: Cannot connect to worker manager: net.tcp://xxxxxx:8060/ExternalService.svc/ No DNS entries exist for host azranlcir01r1. No such host is known Exception detail: System.ServiceModel.EndpointNotFoundException: No DNS entries exist for host xxxxx. ---> System.Net.Sockets.SocketException: No such host is known at System.Net.Dns.GetAddrInfo(String name) at System.Net.Dns.InternalGetHostByName(String hostName, Boolean includeIPv6) at System.Net.Dns.GetHostEntry(String hostNameOrAddress) at System.ServiceModel.Channels.DnsCache.Resolve(Uri uri) --- End of inner exception stack trace --- Server stack trace: at System.ServiceModel.Channels.DnsCache.Resolve(Uri uri)
    ```
    
1. Probieren Sie zur Lösung des Problems eine folgenden Methoden (oder beide) aus:
    - Platzieren Sie alle Knoten in derselben Domäne.
    - Fügen Sie die IP-Adresse zur Hostzuordnung in allen Hostdateien des gehosteten virtuellen Computers hinzu.


## <a name="troubleshoot-connectivity-issue"></a>Behandeln von Konnektivitätsproblemen

### <a name="troubleshoot-connectivity-issue-between-self-hosted-ir-and-data-factory-or-self-hosted-ir-and-data-sourcesink"></a>Behandeln von Konnektivitätsproblemen zwischen der selbstgehosteten IR und Data Factory oder zwischen der selbstgehosteten IR und der Datenquelle/-senke

Um ein Problem mit der Netzwerkkonnektivität zu beheben, müssen Sie wissen, wie Sie [die Netzwerkablaufverfolgung erfassen](#how-to-collect-netmon-trace) und verwenden und wie Sie [die Netmon-Ablaufverfolgung analysieren](#how-to-analyze-netmon-trace), bevor Sie die Netmon-Tools in realen Fällen mit der selbstgehosteten IR anwenden.

Bei der Behandlung von Konnektivitätsproblemen, wie z. B. zwischen der selbstgehosteten IR und Data Factory: 

![Fehler bei HTTP-Anforderung](media/self-hosted-integration-runtime-troubleshoot-guide/http-request-error.png)

Oder zwischen der selbstgehosteten IR und der Datenquelle/-senke treten die folgenden Fehler auf:

**Fehlermeldung:** 
`Copy failed with error:Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,Message=Cannot connect to SQL Server: ‘IP address’`

**Fehlermeldung:** 
`One or more errors occurred. An error occurred while sending the request. The underlying connection was closed: An unexpected error occurred on a receive. Unable to read data from the transport connection: An existing connection was forcibly closed by the remote host. An existing connection was forcibly closed by the remote host Activity ID.`

**Lösung:** Wenn die oben genannten Probleme auftreten, finden Sie im Folgenden Anweisungen zur Problembehandlung:

Führen Sie eine weitere Analyse der Netmon-Ablaufverfolgung durch.
- Legen Sie den Filter zunächst so fest, dass alle Zurücksetzungen vom Server auf die Clientseite angezeigt werden. Im folgenden Beispiel sehen Sie, dass die Serverseite der Data Factory-Server ist.

    ![Data Factory-Server](media/self-hosted-integration-runtime-troubleshoot-guide/data-factory-server.png)

- Wenn Sie das Zurücksetzungspaket erhalten, finden Sie die Konversation unter TCP.

    ![Suchen der Konversation](media/self-hosted-integration-runtime-troubleshoot-guide/find-conversation.png)

- Anschließend können Sie die Konvertierung zwischen Client und Data Factory-Server abrufen, indem Sie den Filter entfernen.

    ![Abrufen der Konversation](media/self-hosted-integration-runtime-troubleshoot-guide/get-conversation.png)
- Basierend auf der erfassten Netmon-Ablaufverfolgung ist zu erkennen, dass die Gültigkeitsdauer (TimeToLive, TTL) 64 beträgt. Gemäß den **Standardwerten für die Gültigkeitsdauer und maximale Anzahl an Weiterleitungen**, die in [diesem Artikel](https://packetpushers.net/ip-time-to-live-and-hop-limit-basics/) erwähnt werden (siehe unten), sehen wir, dass das Linux-System dafür verantwortlich ist, dass das Paket zurückgesetzt und die Verbindung getrennt wird.

    Die Standardwerte für die Gültigkeitsdauer und maximale Anzahl an Weiterleitungen variieren je nach Betriebssystem. Im Folgenden finden Sie einige Standardwerte:
    - Linux-Kernel 2.4 (etwa 2001): 255 für TCP, UDP und ICMP
    - Linux-Kernel 4.10 (2015): 64 für TCP, UDP und ICMP
    - Windows XP (2001): 128 für TCP, UDP und ICMP
    - Windows 10 (2015): 128 für TCP, UDP und ICMP
    - Windows Server 2008: 128 für TCP, UDP und ICMP
    - Windows Server 2019 (2018): 128 für TCP, UDP und ICMP
    - macOS (2001): 64 für TCP, UDP und ICMP

    ![TTL 61](media/self-hosted-integration-runtime-troubleshoot-guide/ttl-61.png)
    
    Im obigen Beispiel ist 61 anstelle von 64 angezeigt, denn wenn das Netzwerkpaket sein Ziel erreicht, muss es verschiedene Hops wie Router/Netzwerkgeräte durchlaufen. Die Anzahl der Router/Netzwerkgeräte wird von der endgültigen Gültigkeitsdauer abgezogen.
    In diesem Fall sehen Sie, dass die Zurücksetzung vom Linux-System mit TTL 64 gesendet werden kann.

- Wir müssen den vierten Hop von der selbstgehosteten IR überprüfen, um zu bestätigen, woher das zurücksetzende Gerät stammt.
 
    *Netzwerkpaket von Linux-System A mit TTL 64-> B TTL 64 minus 1 = 63-> C TTL 63 minus 1 = 62-> TTL 62 minus 1 = 61 selbstgehostete IR*

- Idealerweise ist die Gültigkeitsdauer 128, was bedeutet, dass die Data Factory vom Windows-System ausgeführt wird. *128 - 107 = 21 Hops*, wie im obigen Beispiel gezeigt, bedeutet, dass 21 Hops für das Paket während des TCP 3-Handshakes von Data Factory an die selbstgehostete IR gesendet wurden.
 
    ![TTL 107](media/self-hosted-integration-runtime-troubleshoot-guide/ttl-107.png)

    Wenden Sie sich daher an das Netzwerkteam, um zu überprüfen, welcher der vierte Hop von der selbstgehosteten IR ist. Wenn es sich um die Firewall als Linux-System handelt, überprüfen Sie die Protokolle dazu, warum dieses Gerät das Paket nach dem TCP 3-Handshake zurücksetzt. Wenn Sie jedoch nicht sicher sind, wo Sie die Untersuchung durchführen sollen, versuchen Sie, die Netmon-Ablaufverfolgung von der selbstgehosteten IR und der Firewall während der problematischen Zeit zusammenzustellen, um herauszufinden, welches Gerät dieses Paket zurücksetzt und die Verbindung trennt. In diesem Fall müssen Sie sich auch an Ihr Netzwerkteam wenden, um fortzufahren.

### <a name="how-to-collect-netmon-trace"></a>Erfassen der Netmon-Ablaufverfolgung

1.  Laden Sie die Netmon-Tools von [dieser Website](https://www.microsoft.com/en-sg/download/details.aspx?id=4865)herunter, und installieren Sie sie auf Ihrem Servercomputer (je nachdem, auf welchem Server das Problem auftritt) und auf dem Client (z. B. selbstgehostete IR).

2.  Erstellen Sie einen Ordner, z. B. im folgenden Pfad: *D:\netmon*. Stellen Sie sicher, dass genügend Speicherplatz zum Speichern des Protokolls vorhanden ist.

3.  Erfassen Sie die IP-Adresse und die Portinformationen. 
    1. Starten Sie eine CMD-Eingabeaufforderung.
    2. Wählen Sie „Als Administrator ausführen“ aus, und führen Sie den folgenden Befehl aus:
       
        ```
        Ipconfig /all >D:\netmon\IP.txt
        netstat -abno > D:\netmon\ServerNetstat.txt
        ```

4.  Erfassen Sie die Netmon-Ablaufverfolgung (Netzwerkpaket).
    1. Starten Sie eine CMD-Eingabeaufforderung.
    2. Wählen Sie „Als Administrator ausführen“ aus, und führen Sie den folgenden Befehl aus:
        
        ```
        cd C:\Program Files\Microsoft Network Monitor 3
        ```
    3. Sie können drei verschiedene Befehle verwenden, um die Netzwerkseite zu erfassen:
        - Option A: RoundRobin-Dateibefehl (dadurch wird nur eine Datei erfasst, und alte Protokolle werden überschrieben)

            ```
            nmcap /network * /capture /file D:\netmon\ServerConnection.cap:200M
            ```         
        - Option B: Verketteter Dateibefehl (dadurch wird eine neue Datei erstellt, wenn 200 MB erreicht sind)
        
            ```
            nmcap /network * /capture /file D:\netmon\ServerConnection.chn:200M
            ```          
        - Option C: Geplanter Dateibefehl

            ```
            nmcap /network * /capture /StartWhen /Time 10:30:00 AM 10/28/2011 /StopWhen /Time 11:30:00 AM 10/28/2011 /file D:\netmon\ServerConnection.chn:200M
            ```  

5.  Betätigen Sie **STRG + C**, um die Erfassung der Netmon-Ablaufverfolgung anzuhalten.
 
> [!NOTE]
> Wenn Sie die Netmon-Ablaufverfolgung nur auf dem Clientcomputer erfassen können, rufen Sie die IP-Adresse des Servers ab, um die Ablaufverfolgung besser analysieren zu können.

### <a name="how-to-analyze-netmon-trace"></a>Analysieren der Netmon-Ablaufverfolgung

Wenn Sie versuchen, Telnet **8.8.8.8 888** mit der erfassten Netmon-Ablaufverfolgung auszuführen, sollte Ihnen folgende Ablaufverfolgung angezeigt werden:

![Netmon-Ablaufverfolgung 1](media/self-hosted-integration-runtime-troubleshoot-guide/netmon-trace-1.png)

![Netmon-Ablaufverfolgung 2](media/self-hosted-integration-runtime-troubleshoot-guide/netmon-trace-2.png)
 

Dies bedeutet, dass Sie keine TCP-Verbindung mit der Serverseite **8.8.8.8** basierend auf Port **888** herstellen können. Daher sehen Sie zwei zusätzliche **SynReTransmit**-Pakete. Da die Quelle **SELF-HOST2** keine Verbindung mit **8.8.8.8** beim ersten Paket herstellen konnte, wird die Verbindung weiterhin hergestellt.

> [!TIP]
> - Klicken Sie auf **Filter laden** -> **Standard Filter (Standardfilter)**  -> **Adressen** -> **IPv4-Adressen**.
> - Geben Sie **IPv4.Address == 8.8.8.8** als Filter ein, und klicken Sie auf **Anwenden**. Anschließend wird Ihnen nur die Kommunikation vom lokalen Computer zum Ziel **8.8.8.8** angezeigt.

![Filteradressen 1](media/self-hosted-integration-runtime-troubleshoot-guide/filter-addresses-1.png)
        
![Filteradressen 2](media/self-hosted-integration-runtime-troubleshoot-guide/filter-addresses-2.png)

Das folgende Beispiel zeigt, wie ein gutes Szenario aussehen würde. 

- Wenn Telnet **8.8.8.8 53** problemlos funktioniert, sehen Sie, dass ein TCP 3-Handshake stattfindet, und die Sitzung wird mit einem TCP 4-Handshake beendet.

    ![Gutes Szenario Beispiel 1](media/self-hosted-integration-runtime-troubleshoot-guide/good-scenario-1.png)
     
    ![Gutes Szenario Beispiel 2](media/self-hosted-integration-runtime-troubleshoot-guide/good-scenario-2.png)

- Basierend auf dem obigen TCP 3-Handshake sehen Sie den folgenden Workflow:

    ![TCP 3-Handshake-Workflow](media/self-hosted-integration-runtime-troubleshoot-guide/tcp-3-handshake-workflow.png)
 
- Der TCP 4-Handshake zum Beenden der Sitzung und der zugehörige Workflow werden wie folgt angezeigt:

    ![TCP 4-Handshake](media/self-hosted-integration-runtime-troubleshoot-guide/tcp-4-handshake.png)

    ![TCP 4-Handshake-Workflow](media/self-hosted-integration-runtime-troubleshoot-guide/tcp-4-handshake-workflow.png) 


## <a name="next-steps"></a>Nächste Schritte

Weitere Hilfe zur Problembehandlung finden Sie in den folgenden Ressourcen:

*  [Data Factory-Blog](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory-Funktionsanfragen](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure-Videos](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Frageseite von Microsoft Q&A (Fragen und Antworten)](https://docs.microsoft.com/answers/topics/azure-data-factory.html)
*  [Stack Overflow-Forum für Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Twitter-Informationen über Data Factory](https://twitter.com/hashtag/DataFactory)
*  [Anleitung zur Leistung der Mapping Data Flow-Funktion](concepts-data-flow-performance.md)
