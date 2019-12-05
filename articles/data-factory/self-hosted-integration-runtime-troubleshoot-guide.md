---
title: Problembehandlung bei der selbstgehosteten Integration Runtime in Azure Data Factory
description: Hier erfahren Sie, wie Sie eine Problembehandlung im Fall von Problemen bei der selbstgehosteten Integration Runtime in Azure Data Factory durchführen können.
services: data-factory
author: nabhishek
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 11/07/2019
ms.author: abnarain
ms.openlocfilehash: 9adbc3d7d30aeb8c7cb2b89c326ac2b39a2e8d2b
ms.sourcegitcommit: 6dec090a6820fb68ac7648cf5fa4a70f45f87e1a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2019
ms.locfileid: "73907222"
---
# <a name="troubleshoot-self-hosted-integration-runtime"></a>Problembehandlung bei der selbstgehosteten Integration Runtime

In diesem Artikel werden allgemeine Methoden zur Problembehandlung bei selbstgehosteter Integration Runtime in Azure Data Factory erläutert.

## <a name="common-errors-and-resolutions"></a>Häufige Fehler und deren Lösungen

### <a name="error-message-self-hosted-integration-runtime-is-unable-to-connect-to-cloud-service"></a>Fehlermeldung: Self-hosted integration runtime is unable to connect to cloud service. (Selbstgehostete Integration Runtime kann keine Verbindung mit dem Clouddienst herstellen.)

- **Symptom**: 

    ![Verbindungsproblem bei der selbstgehosteten Integration Runtime (IR)](media/self-hosted-integration-runtime-troubleshoot-guide/unable-to-connect-to-cloud-service.png)

- **Ursache:** Die selbstgehostete Integration Runtime kann keine Verbindung mit dem Data Factory-Dienst (Back-End) herstellen. Dies wird meistens durch Netzwerkeinstellungen in der Firewall verursacht.

- **Lösung:** 

    1. Überprüfen Sie, ob der Windows-Dienst "Integration Runtime-Dienst" ausgeführt wird.
    
        ![Ausführungsstatus des selbstgehosteten Integration Runtime-Diensts](media/self-hosted-integration-runtime-troubleshoot-guide/integration-runtime-service-running-status.png)
    
    2. Wenn der in [1] gezeigte Windows-Dienst ausgeführt wird, führen Sie die nachstehenden Anleitungen je nach Bedarf aus:

        1. Wenn „Proxy“ nicht für die selbstgehostete Integration Runtime konfiguriert ist (die Standardeinstellungen lauten „no proxy configuration“ (keine Proxykonfiguration)), führen Sie den folgenden PowerShell-Befehl auf dem Computer aus, auf dem die selbstgehostete Integration Runtime installiert wurde: 
            
            ```powershell
            (New-Object System.Net.WebClient).DownloadString("https://wu2.frontend.clouddatahub.net/")
            ```
            > [!NOTE] 
            > Die Dienst-URL kann je nach Ihrem Data Factory-Speicherort variieren. Die Dienst-URL können Sie finden unter „ADF UI“ -> „Connections“ -> „Integration runtimes“ -> „Edit Self-hosted IR“ -> „Nodes“ -> „View Service URLs“ (ADF-Benutzeroberfläche -> Verbindungen -> Integration Runtimes -> Selbstgehostete IR bearbeiten -> Knoten -> Dienst-URLs anzeigen).
            
            Dies ist die erwartete Antwort:
            
            ![Antwort des PowerShell-Befehls](media/self-hosted-integration-runtime-troubleshoot-guide/powershell-command-response.png)
            
            Wenn die Antwort anders lautet, führen Sie die nachstehenden Anleitungen je nach Bedarf aus:
            
            * Wenn der Fehler „Der Remotename konnte nicht aufgelöst werden“ angezeigt wird, liegt ein Problem beim DNS vor. Wenden Sie sich dann an das Netzwerkteam, damit das DNS-Auflösungsproblem behoben wird! 
            * Wenn der Fehler „SSL/TLS-Zertifikat ist nicht vertrauenswürdig“ angezeigt wird, überprüfen Sie, ob das Zertifikat für „https://wu2.frontend.clouddatahub.net/“ auf dem Computer vertrauenswürdig ist, und installieren Sie das öffentliche Zertifikat mithilfe des Zertifikat-Managers. Dadurch sollte dieses Problem behoben werden.
            * Überprüfen Sie „Windows“-> „Ereignisanzeige (Protokolle)“ > „Anwendungs- und Dienstprotokolle“-> „Integration Runtime“ auf einen Fehler, der meistens durch DNS, Firewallregel und Netzwerkeinstellungen des Unternehmens verursacht wird (Erzwungenes Schließen der Verbindung). Wenden Sie sich bei diesem Problem an Ihr Netzwerkteam, um Hilfe bei der weiteren Problembehandlung zu erhalten, weil es in jedem Unternehmen angepasste Netzwerkeinstellungen gibt.

        2. Wenn „Proxy“ in der selbstgehosteten Integration Runtime konfiguriert wurde, überprüfen Sie, ob Ihr Proxyserver auf unseren Dienstendpunkt zugreifen kann. Einen Beispielbefehl finden Sie [hier](https://stackoverflow.com/questions/571429/powershell-web-requests-and-proxies).    
                
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

            Dies ist die erwartete Antwort:
            
            ![Antwort 2 des PowerShell-Befehls](media/self-hosted-integration-runtime-troubleshoot-guide/powershell-command-response.png)

            > [!NOTE] 
            > Proxy-Aspekte:
            > * Überprüfen Sie, ob der Proxyserver Whitelists erfordert. Wenn das zutrifft, fügen Sie [diese Domänen](https://docs.microsoft.com/azure/data-factory/data-movement-security-considerations#firewall-requirements-for-on-premisesprivate-network) der Whitelist hinzu.
            > * Überprüfen Sie, ob das TLS/SSL-Zertifikat für „wu2.frontend.clouddatahub.net/“ auf dem Proxyserver vertrauenswürdig ist.
            > * Wenn Sie im Proxy die Active Directory-Authentifizierung verwenden, ändern Sie das Dienstkonto in das Benutzerkonto, das als „Integration Runtime-Dienst“ auf den Proxy zugreifen kann.

### <a name="error-message-self-hosted-integration-runtime-node-logical-shir-is-in-inactive-running-limited-state"></a>Fehlermeldung: „Self-hosted integration runtime node/ logical SHIR is in Inactive/ „Running (Limited)“ state“ (Knoten der selbstgehosteten Integration Runtime / logische SHIR befindet sich im Status „Inaktiv/Wird ausgeführt (eingeschränkt)“)

- **Ursache:** Möglicherweise wird der Knoten der selbstgehosteten IR im inaktiven Status angezeigt, wie im nachstehenden Screenshot zu sehen ist:

    ![Inaktiver Knoten der selbstgehosteten IR](media/self-hosted-integration-runtime-troubleshoot-guide/inactive-self-hosted-ir-node.png)

    Dies geschieht, wenn Knoten nicht miteinander kommunizieren können. 

- **Lösung:** 

    Melden Sie sich bei dem auf dem Knoten gehosteten virtuellen Computer an, und öffnen Sie die Ereignisansicht. Filtern Sie unter “Anwendungs- und Dienstprotokolle“ -> „Integration Runtime“ alle Fehlerprotokolle. 

     1. Wenn das Fehlerprotokoll Folgendes enthält: 
    
        **Fehlerprotokoll**: System.ServiceModel.EndpointNotFoundException: Es konnte keine Verbindung mit „net.tcp://xxxxxxx.bwld.com:8060/ExternalService.svc/WorkerManager“ hergestellt werden. Der Verbindungsversuch hat für einen Zeitraum von „00:00:00.9940994“ angedauert. TCP-Fehlercode „10061“: Es konnte keine Verbindung hergestellt werden, weil der Zielcomputer sie aktiv verweigert hat – 10.2.4.10:8060.  ---> System.Net.Sockets.SocketException: Es konnte keine Verbindung hergestellt werden, weil der Zielcomputer sie aktiv verweigert hat – 10.2.4.10:8060
    
           unter „System.Net.Sockets.Socket.DoConnect(EndPoint endPointSnapshot, SocketAddress socketAddress)“
           
           unter „System.Net.Sockets.Socket.Connect(EndPoint remoteEP)“
           
           unter „System.ServiceModel.Channels.SocketConnectionInitiator.Connect(Uri uri, TimeSpan timeout)“
    
        **Lösung:** Starten Sie die Befehlszeile: „telnet 10.2.4.10 8060“
        
        Wenn einer der nachstehenden Fehler angezeigt wird, wenden Sie sich an Ihre IT-Mitarbeiter, damit sie Ihnen bei der Behebung dieses Problems helfen. Nachdem Sie Telnet erfolgreich ausführen konnten, beim IR-Knotenstatus aber weiterhin Probleme auftreten, wenden Sie sich an den Microsoft-Support.
        
        ![Befehlszeilenfehler](media/self-hosted-integration-runtime-troubleshoot-guide/command-line-error.png)
        
     2. Wenn das Fehlerprotokoll Folgendes enthält:
     
        **Fehlerprotokoll:** Es kann keine Verbindung mit dem Worker-Manager hergestellt werden: „net.tcp://xxxxxx:8060/ExternalService.sv/“. Für Host „azranlcir01r1“ sind keine DNS-Einträge vorhanden. Es ist kein solcher Host bekannt. Ausnahmedetail: System.ServiceModel.EndpointNotFoundException: Für den Host „xxxxx“ sind keine DNS-Einträge vorhanden. ---> System.Net.Sockets.SocketException: Es ist kein solcher Host bekannt unter „System.Net.Dns.GetAddrInfo(String name)“ unter „System.Net.Dns.InternalGetHostByName(String hostName, Boolean includeIPv6)“ unter „System.Net.Dns.GetHostEntry(String hostNameOrAddress)“ unter „System.ServiceModel.Channels.DnsCache.Resolve(Uri uri)“ --- Ende der inneren Ausnahmestapelüberwachung --- Serverstapelüberwachung: unter „System.ServiceModel.Channels.DnsCache.Resolve(Uri uri)“ 
    
        **Lösung:** Eine der beiden folgenden Aktionen kann Ihnen bei der Behebung des Problems helfen:
         1. Platzieren Sie alle Knoten in derselben Domäne.
         2. Fügen Sie die IP-Adresse zur Hostzuordnung in allen Hostdateien des gehosteten virtuellen Computers hinzu.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Problembehandlung finden Sie in diesen Ressourcen:

*  [Data Factory-Blog](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory-Funktionsanfragen](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure-Videos](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [MSDN-Forum](https://social.msdn.microsoft.com/Forums/home?sort=relevancedesc&brandIgnore=True&searchTerm=data+factory)
*  [Stack Overflow-Forum für Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Twitter-Informationen über Data Factory](https://twitter.com/hashtag/DataFactory)
*  [Anleitung zur Leistung und Optimierung der Mapping Data Flow-Funktion](concepts-data-flow-performance.md)
