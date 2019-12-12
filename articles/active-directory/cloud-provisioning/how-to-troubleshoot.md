---
title: 'Azure AD Connect-Cloudbereitstellung: Problembehandlung'
description: In diesem Dokument wird beschrieben, wie Sie Probleme beheben, die beim Cloudbereitstellungs-Agent auftreten können.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 12/02/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4100886e0a24fa961b9085bd507ae3f4ebfdd6eb
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74795449"
---
# <a name="cloud-provisioning-troubleshooting"></a>Problembehandlung bei der Cloudbereitstellung

Die Cloudbereitstellung berührt viele verschiedene Bereiche und weist viele verschiedene Abhängigkeiten auf.  Naturgemäß kann dies zu diversen Problemen führen.  Dieses Dokument soll Ihnen den Einstieg in die Behebung dieser Probleme erleichtern.  In diesem Dokument werden die typischen Bereiche erläutert, auf die Sie sich konzentrieren sollten. Sie erfahren, wie Sie zusätzliche Informationen sammeln, und lernen verschiedene Techniken zum Ermitteln von Problemen kennen.  


## <a name="common-troubleshooting-areas"></a>Allgemeine Problembehandlungsbereiche

|NAME|BESCHREIBUNG|
|-----|-----|
|[Agent-Probleme](#agent-issues)|Vergewissern Sie sich, dass der Agent richtig installiert wurde und mit Azure AD kommuniziert.|
|[Objektsynchronisierungsprobleme](#object-synchronization-issues)|Verwenden Sie Bereitstellungsprotokolle, um Probleme bei der Objektsynchronisierung zu beheben.|
|[Bereitstellungsprobleme im Status „Quarantäne“](#provisioning-quarantined-issues)|Informieren Sie sich über Bereitstellungsprobleme im Status „Quarantäne“ und deren Behebung.|


## <a name="agent-issues"></a>Agent-Probleme
Zu den ersten Punkten, die Sie beim Agent überprüfen sollten, zählen folgende:


-  Ist der Agent installiert?
-  Wird der Agent lokal ausgeführt?
-  Befindet sich der Agent im Portal?
-  Ist der Agent als fehlerfrei gekennzeichnet?  

Diese Elemente können Sie im Azure-Portal und auf dem lokalen Server überprüfen, auf dem der Agent ausgeführt wird.

### <a name="azure-portal-agent-verification"></a>Agent-Überprüfung im Azure-Portal

Führen Sie die folgenden Schritte aus, um zu überprüfen, ob der Agent von Azure erkannt wird und fehlerfrei funktioniert:

1. Melden Sie sich beim Azure-Portal an.
2. Wählen Sie auf der linken Seite **Azure Active Directory** aus, klicken Sie auf **Azure AD Connect**, und wählen Sie im mittleren Bereich **Bereitstellung verwalten (Vorschau)** aus.
3.  Klicken Sie auf dem Bildschirm **Azure AD-Bereitstellung (Vorschau)** auf **Alle Agents überprüfen**.
 ![Azure AD-Bereitstellung](media/how-to-install/install7.png)</br>
 
4. Auf dem Bildschirm **Lokale Bereitstellungs-Agents** werden die von Ihnen installierten Agents angezeigt.  Vergewissern Sie sich, dass der betreffende Agent aufgeführt wird und als **Fehlerfrei** markiert ist.
 ![Bereitstellungs-Agents](media/how-to-install/install8.png)</br>

### <a name="verify-the-port"></a>Überprüfen des Ports

Um zu überprüfen, ob Azure an Port 443 lauscht und der Agent damit kommunizieren kann, können Sie das folgende Tool verwenden:

https://aadap-portcheck.connectorporttest.msappproxy.net/ 

Mit diesem Test wird überprüft, ob Ihre Agents über Port 443 mit Azure kommunizieren können.  Öffnen Sie einen Browser, und navigieren Sie zu der obigen URL des Servers, auf dem der Agent installiert ist.
 ![Dienste](media/how-to-install/verify2.png)

### <a name="on-the-local-server"></a>Auf dem lokalen Server

Führen Sie die folgenden Schritte aus, um sicherzustellen, dass der Agent ausgeführt wird:

1.  Öffnen Sie auf dem Server, auf dem der Agent installiert ist, die Seite **Dienste**, indem Sie zu dieser Seite oder zu „Start/Ausführen/Services.msc“ navigieren.
2.  Vergewissern Sie sich, dass unter **Dienste** die Dienste **Microsoft Azure AD Connect Agent Updater** und **Microsoft Azure AD Connect Provisioning Agent** angezeigt werden und deren Status **Wird ausgeführt** lautet.
 ![Dienste](media/how-to-troubleshoot/troubleshoot1.png)

### <a name="common-agent-installation-issues"></a>Allgemeine Probleme bei der Agent-Installation

Nachfolgend finden Sie einige häufige Probleme mit der Agent-Installation und die typischen Lösungen.

#### <a name="agent-failed-to-start"></a>Fehler beim Starten des Agents

Es wird eine Fehlermeldung angezeigt, die besagt:

**Dienst „Microsoft Azure AD Connect Provisioning Agent“ konnte nicht gestartet werden.  Überprüfen Sie, ob Sie ausreichende Berechtigungen zum Starten von Systemdiensten besitzen.** 

Dies wird in der Regel durch eine Gruppenrichtlinie verursacht, die verhindert, dass Berechtigungen auf das vom Installationsprogramm erstellte „Anmeldekonto“ für den lokalen NT-Dienst (NT SERVICE\AADConnectProvisioningAgent) angewendet werden. Diese Berechtigungen sind zum Starten des Diensts erforderlich.

Führen Sie zum Beheben dieses Problems die folgenden Schritte aus:

1.  Melden Sie sich beim Server mit einem Administratorkonto an.
2.  Öffnen Sie die Seite **Dienste**, indem Sie zu dieser Seite oder zu „Start/Run/Services.msc“ navigieren.
3.  Doppelklicken Sie unter **Dienste** auf **Microsoft Azure AD Connect Provisioning Agent**.
4. Ändern Sie auf der Registerkarte das „Anmeldekonto“ in ein Domänenadministratorkonto, und starten Sie den Dienst neu. 

 ![Dienste](media/how-to-troubleshoot/troubleshoot3.png)

#### <a name="agent-times-out-or-certificate-is-invalid"></a>Bei der Ausführung des Agents tritt ein Timeout auf, oder das Zertifikat ist ungültig.

Wenn Sie versuchen, den Agent zu registrieren, erhalten Sie u.U. die folgenden Fehlermeldungen.

 ![Dienste](media/how-to-troubleshoot/troubleshoot4.png)

Dies wird in der Regel dadurch verursacht, dass der Agent keine Verbindung mit dem Hybrididentitätsdienst herstellen kann und die Konfiguration eines HTTP-Proxys erforderlich ist.  Konfigurieren Sie zur Problembehebung einen Proxy für den ausgehenden Datenverkehr. 

Der Agent-Bereitstellung unterstützt die Verwendung von Proxys für ausgehenden Datenverkehr. Dies können Sie durch das Konfigurieren der Konfigurationsdatei des Agent **C:\Program Files\Microsoft Azure AD Connect Provisioning Agent\AADConnectProvisioningAgent.exe.config** konfigurieren. Fügen Sie die folgenden Zeilen zum Ende der Datei unmittelbar vor dem `</configuration>`-Tag ein.
Ersetzen Sie der Variablen [[proxy-server] ] und [proxy-port] durch den Namen Ihres Proxyservers und die Portwerte.

```xml
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
                usesystemdefault="true"
                proxyaddress="http://[proxy-server]:[proxy-port]"
                bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

#### <a name="agent-registration-fails-with-security-error"></a>Fehler bei der Agent-Registrierung mit Sicherheitsfehler

Bei der Installation des Cloudbereitstellungs-Agents wird möglicherweise der folgende Fehler angezeigt.

Dies wird in der Regel dadurch verursacht, dass der Agent die PowerShell-Registrierungsskripts aufgrund von lokalen PowerShell-Ausführungsrichtlinien nicht ausführen kann.

Ändern Sie zur Problembehebung die PowerShell-Ausführungsrichtlinien auf dem Server. Sie müssen die Computer- und Benutzerrichtlinien auf „Undefined“ oder „RemoteSigned“ festlegen. Wenn sie auf „Unrestricted“ festgelegt sind, wird dieser Fehler angezeigt.  Weitere Informationen finden Sie unter [PowerShell-Ausführungsrichtlinien](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6). 

### <a name="log-files"></a>Protokolldateien

Standardmäßig gibt der Agent recht minimalistische Fehlermeldungen und Stapelüberwachungsinformationen aus. Diese Überwachungsprotokolle finden Sie im Ordner **C:\ProgramData\Microsoft\Azure AD Connect Provisioning Agent\Trace**

Führen Sie die folgenden Schritte aus, um weitere Details zur Behandlung von Problemen mit dem Agent zu sammeln.

1. Beenden Sie den Dienst **Microsoft Azure AD Connect Provisioning Agent**.
2. Erstellen Sie eine Kopie der ursprünglichen Konfigurationsdatei **C:\Programme\Microsoft Azure AD Connect Provisioning Agent\AADConnectProvisioningAgent.exe.config**.
3. Ersetzen Sie den vorhandenen Abschnitt <system.diagnostics> durch Folgendes, damit alle Überwachungsmeldungen in der Datei **ProvAgentTrace.log** gespeichert werden.

      ```xml
        <system.diagnostics>
            <sources>
            <source name="AAD Connect Provisioning Agent">
                <listeners>
                <add name="console"/>
                <add name="etw"/>
                <add name="textWriterListener"/>
                </listeners>
            </source>
            </sources>
            <sharedListeners>
            <add name="console" type="System.Diagnostics.ConsoleTraceListener" initializeData="false"/>
            <add name="etw" type="System.Diagnostics.EventLogTraceListener" initializeData="Azure AD Connect Provisioning Agent">
                <filter type="System.Diagnostics.EventTypeFilter" initializeData="All"/>
            </add>
            <add name="textWriterListener" type="System.Diagnostics.TextWriterTraceListener" initializeData="C:/ProgramData/Microsoft/Azure AD Connect Provisioning Agent/Trace/ProvAgentTrace.log"/>
            </sharedListeners>
        </system.diagnostics>

      ```
4. Starten Sie den Dienst **Microsoft Azure AD Connect Provisioning Agent**.
5. Mit dem folgenden Befehl können Sie die Datei angeben und Probleme debuggen: 
    ```
    Get-Content “C:/ProgramData/Microsoft/Azure AD Connect Provisioning Agent/Trace/ProvAgentTrace.log” -Wait
    ```
## <a name="object-synchronization-issues"></a>Objektsynchronisierungsprobleme

Der folgende Abschnitt enthält Informationen zur Behandlung von Problemen bei der Objektsynchronisierung.

### <a name="provisioning-logs"></a>Bereitstellungsprotokolle

Im Azure-Portal können Sie mit Bereitstellungsprotokollen Probleme bei der Objektsynchronisierung ermitteln und beheben.  Wählen Sie **Protokolle** aus, um die Protokolle anzuzeigen.
 ![Bereitstellungsprotokolle](media/how-to-troubleshoot/log1.png)

Die Bereitstellungsprotokolle bieten eine Fülle von Informationen zum Status der Objekte, die zwischen der lokalen AD-Umgebung und Azure synchronisiert werden.

 ![Bereitstellungsprotokolle](media/how-to-troubleshoot/log2.png)

Mithilfe der Dropdownfelder oben auf der Seite können Sie die Ansicht filtern, um sich auf bestimmte Probleme, Datumsangaben usw. zu konzentrieren.  Wenn Sie auf ein einzelnes Ereignis doppelklicken, werden zusätzliche ausführliche Informationen bereitgestellt.

 ![Bereitstellungsprotokolle](media/how-to-troubleshoot/log3.png)

Diese Informationen enthalten ausführliche Schritte und geben an, wo das Synchronisierungsproblem auftritt.  Auf diese Weise können Sie sich auf das Problem konzentrieren und die genaue Ursache ermitteln.


## <a name="provisioning-quarantined-issues"></a>Bereitstellungsprobleme im Status „Quarantäne“

Die Cloudbereitstellung überwacht die Integrität Ihrer Konfiguration und versetzt fehlerhafte Objekte in den Status „Quarantäne“. Wenn die meisten oder alle Aufrufe an das Zielsystem aufgrund eines Fehlers (z. B. bei ungültigen Administratoranmeldeinformationen) dauerhaft nicht erfolgreich sind, wird der Bereitstellungsauftrag in den Quarantänestatus versetzt.

 ![Quarantäne](media/how-to-troubleshoot/quarantine1.png)

Durch Klicken auf den Status können Sie zusätzliche Informationen zur Quarantäne anzeigen.  Sie können auch den Fehlercode und die Fehlermeldung abrufen.

 ![Quarantäne](media/how-to-troubleshoot/quarantine2.png)

### <a name="resolving-a-quarantine"></a>Aufheben einer Quarantäne

- Verwenden Sie das Azure-Portal, um den Bereitstellungsauftrag neu zu starten. Wählen Sie auf der Seite mit der Agent-Konfiguration die Option **Bereitstellung neu starten** aus.

  ![Quarantäne](media/how-to-troubleshoot/quarantine3.png)

- Verwenden Sie Microsoft Graph, um [den Bereitstellungsauftrag neu zu starten](https://docs.microsoft.com/graph/api/synchronization-synchronizationjob-restart?view=graph-rest-beta&tabs=http). Sie haben vollständige Kontrolle über die Elemente, die Sie neu starten. Sie können auswählen, ob Hinterlegungen (zum Neustarten des Hinterlegungszählers, der in Richtung Quarantänestatus läuft) gelöscht, die Quarantäne (zum Entfernen der Anwendung aus der Quarantäne) oder die Wasserzeichen gelöscht werden soll(en). Verwenden Sie die folgende Anforderung:
 
  `POST /servicePrincipals/{id}/synchronization/jobs/{jobId}/restart`

## <a name="next-steps"></a>Nächste Schritte 

- [Was ist eine Bereitstellung?](what-is-provisioning.md)
- [Was ist die Azure AD Connect-Cloudbereitstellung?](what-is-cloud-provisioning.md)



