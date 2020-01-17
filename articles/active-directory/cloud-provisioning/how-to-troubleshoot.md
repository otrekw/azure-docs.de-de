---
title: 'Azure AD Connect-Cloudbereitstellung: Problembehandlung'
description: In diesem Artikel wird beschrieben, wie Sie Probleme beheben, die beim Cloudbereitstellungs-Agent auftreten können.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 12/02/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e41be4b76245f2567015eb0ede317830120ee61a
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/31/2019
ms.locfileid: "75549484"
---
# <a name="cloud-provisioning-troubleshooting"></a>Problembehandlung bei der Cloudbereitstellung

Die Cloudbereitstellung berührt viele verschiedene Bereiche und weist viele verschiedene Abhängigkeiten auf. Diese große Bandbreite kann zu verschiedenen Fehlern führen. Dieser Artikel hilft Ihnen, diese Fehler zu beheben. Sie lernen in ihm die typischen Fehlerbereiche kennen, auf die Sie sich konzentrieren sollten, und Sie erfahren, wie Sie zusätzliche Informationen abrufen können. Außerdem werden die verschiedenen Verfahren vorgestellt, mit denen die Fehler behoben werden können.


## <a name="common-troubleshooting-areas"></a>Allgemeine Problembehandlungsbereiche

|Name|BESCHREIBUNG|
|-----|-----|
|[Agent-Probleme](#agent-problems)|Hier überprüfen Sie, ob der Agent korrekt installiert wurde und vergewissern sich, dass er mit Azure Active Directory (Azure AD) kommuniziert.|
|[Objektsynchronisierungsprobleme](#object-synchronization-problems)|Hier verwenden Sie Bereitstellungsprotokolle, um Probleme bei der Objektsynchronisierung zu beheben.|
|[Bereitstellungsprobleme im Status „Quarantäne“](#provisioning-quarantined-problems)|Hier lernen Sie Bereitstellungsprobleme im Status „Quarantäne“ und deren Behebung kennen.|


## <a name="agent-problems"></a>Agent-Probleme
Zu den ersten Punkten, die Sie beim Agent überprüfen sollten, zählen folgende:

-  Ist der Agent installiert?
-  Wird der Agent lokal ausgeführt?
-  Befindet sich der Agent im Portal?
-  Ist der Agent als fehlerfrei gekennzeichnet?

Diese Punkte können Sie im Azure-Portal und auf dem lokalen Server überprüfen, auf dem der Agent ausgeführt wird.

### <a name="azure-portal-agent-verification"></a>Agent-Überprüfung im Azure-Portal

Führen Sie die folgenden Schritte aus, um zu überprüfen, ob der Agent von Azure erkannt wird und fehlerfrei funktioniert:

1. Melden Sie sich beim Azure-Portal an.
1. Wählen Sie auf der linken Seite **Azure Active Directory** > **Azure AD Connect** aus. Wählen Sie in der Mitte **Bereitstellung verwalten (Vorschau)** aus.
1. Wählen Sie auf dem Bildschirm **Azure AD-Bereitstellung (Vorschau)** die Option **Alle Agents überprüfen** aus.

   ![Option „Alle Agents überprüfen“](media/how-to-install/install7.png)</br>
 
1. Auf dem Bildschirm **On-premises provisioning agents** (Lokale Bereitstellungs-Agents) werden die von Ihnen installierten Agents angezeigt. Vergewissern Sie sich, dass der betreffende Agent aufgeführt wird und als *Fehlerfrei* markiert ist.

   ![Bildschirm „On-premises provisioning agents“ (Lokale Bereitstellungs-Agents)](media/how-to-install/install8.png)</br>

### <a name="verify-the-port"></a>Überprüfen des Ports

Verwenden Sie das folgende Tool, um zu überprüfen, ob Azure an Port 443 lauscht und ob der Agent damit kommunizieren kann:

https://aadap-portcheck.connectorporttest.msappproxy.net/ 

Mithilfe dieses Tests wird überprüft, ob Ihre Agents über Port 443 mit Azure kommunizieren können. Öffnen Sie einen Browser, und navigieren Sie von dem Server, auf dem der Agent installiert ist, zur obigen URL.

![Überprüfung der Porterreichbarkeit](media/how-to-install/verify2.png)

### <a name="on-the-local-server"></a>Auf dem lokalen Server

Führen Sie die folgenden Schritte aus, um sicherzustellen, dass der Agent ausgeführt wird:

1. Öffnen Sie auf dem Server, auf dem der Agent installiert ist, die Seite **Dienste**, indem Sie zu dieser Seite oder zu **Start** > **Ausführen** > **Services.msc** navigieren.
1. Vergewissern Sie sich, dass unter **Dienste** die Dienste **Microsoft Azure AD Connect Agent Updater** und **Microsoft Azure AD Connect Provisioning Agent** mit dem Status *Wird ausgeführt* angezeigt werden.

   ![Bildschirm „Dienste“](media/how-to-troubleshoot/troubleshoot1.png)

### <a name="common-agent-installation-problems"></a>Allgemeine Probleme bei der Agent-Installation

In den folgenden Abschnitten werden einige häufiger auftretende Probleme bei der Agent-Installation sowie gängige Lösungsansätze erläutert.

#### <a name="agent-failed-to-start"></a>Fehler beim Starten des Agents

Möglicherweise wird eine Fehlermeldung angezeigt, die folgendermaßen lautet:

**Der Dienst „Microsoft Azure AD Connect Provisioning Agent“ wurde nicht gestartet. Überprüfen Sie, ob Sie ausreichende Berechtigungen zum Starten von Systemdiensten besitzen.** 

Dieses Problem wird in der Regel von einer Gruppenrichtlinie verursacht, die verhindert, dass Berechtigungen auf das vom Installationsprogramm erstellte Anmeldekonto für den lokalen NT-Dienst (NT SERVICE\AADConnectProvisioningAgent) angewendet werden. Diese Berechtigungen sind zum Starten des Diensts erforderlich.

Gehen Sie folgendermaßen vor, um dieses Problem zu beheben:

1. Melden Sie sich beim Server mit einem Administratorkonto an.
1. Öffnen Sie die Seite **Dienste**, indem Sie zu dieser Seite navigieren oder **Start** > **Ausführen** > **Services.msc** ausführen.
1. Doppelklicken Sie unter **Dienste** auf **Microsoft Azure AD Connect Provisioning Agent**.
1. Ändern Sie auf der Registerkarte **Anmelden** den Eintrag für **Dieses Konto** in das Konto eines Domänenadministrators. Starten Sie den Dienst dann neu. 

   ![Registerkarte „Anmelden“](media/how-to-troubleshoot/troubleshoot3.png)

#### <a name="agent-times-out-or-certificate-is-invalid"></a>Bei der Ausführung des Agents tritt ein Timeout auf, oder das Zertifikat ist ungültig.

Beim Versuch, den Agent zu registrieren, kann die folgende Fehlermeldung angezeigt werden:

![Fehlermeldung über einen Timeout](media/how-to-troubleshoot/troubleshoot4.png)

Dieses Problem wird in der Regel dadurch verursacht, dass der Agent keine Verbindung mit dem Hybrididentitätsdienst herstellen kann und die Konfiguration eines HTTP-Proxys erforderlich ist. Konfigurieren Sie zur Problembehebung einen Proxy für den ausgehenden Datenverkehr. 

Der Bereitstellungs-Agent unterstützt die Verwendung von Proxys für ausgehenden Datenverkehr. Dies können Sie durch das Konfigurieren der Konfigurationsdatei des Agent *C:\Program Files\Microsoft Azure AD Connect Provisioning Agent\AADConnectProvisioningAgent.exe.config* konfigurieren. Fügen Sie in der Datei gegen Ende unmittelbar vor dem `</configuration>`-Tag die folgenden Zeilen ein.
Ersetzen Sie die Variablen `[proxy-server]` und `[proxy-port]` durch den Namen Ihres Proxyservers und die Portwerte.

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

Möglicherweise erhalten Sie während der Installation des Cloudbereitstellungs-Agent eine Fehlermeldung.

Dieses Problem wird in der Regel dadurch verursacht, dass der Agent die PowerShell-Registrierungsskripts aufgrund von lokalen PowerShell-Ausführungsrichtlinien nicht ausführen kann.

Ändern Sie zur Lösung dieses Problems die PowerShell-Ausführungsrichtlinien auf dem Server. Sie müssen die Computer- und Benutzerrichtlinien auf *Undefined* oder *RemoteSigned* festlegen. Wenn sie auf *Unrestricted* festgelegt sind, wird diese Fehlermeldung angezeigt. Weitere Informationen finden Sie unter [PowerShell-Ausführungsrichtlinien](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6). 

### <a name="log-files"></a>Protokolldateien

Standardmäßig gibt der Agent minimalistische Fehlermeldungen und Überwachungsinformationen aus. Diese Überwachungsprotokolle finden Sie im Ordner *C:\ProgramData\Microsoft\Azure AD Connect Provisioning Agent\Trace*.

Führen Sie die folgenden Schritte aus, um weitere Details zur Behandlung von Problemen mit dem Agent zu sammeln.

1. Beenden Sie den Dienst **Microsoft Azure AD Connect Provisioning Agent**.
1. Erstellen Sie eine Kopie der ursprünglichen Konfigurationsdatei *C:\Programme\Microsoft Azure AD Connect Provisioning Agent\AADConnectProvisioningAgent.exe.config*.
1. Ersetzen Sie den vorhandenen Abschnitt `<system.diagnostics>` durch Folgendes, damit alle Überwachungsmeldungen in der Datei *ProvAgentTrace.log* gespeichert werden.

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
1. Starten Sie den Dienst **Microsoft Azure AD Connect Provisioning Agent**.
1. Mit dem folgenden Befehl können Sie die Datei abrufen, um sie dann zu kürzen und Probleme zu debuggen. 
    ```
    Get-Content “C:/ProgramData/Microsoft/Azure AD Connect Provisioning Agent/Trace/ProvAgentTrace.log” -Wait
    ```
## <a name="object-synchronization-problems"></a>Objektsynchronisierungsprobleme

Der folgende Abschnitt enthält Informationen zur Behandlung von Problemen bei der Objektsynchronisierung.

### <a name="provisioning-logs"></a>Bereitstellungsprotokolle

Im Azure-Portal können Sie mit Bereitstellungsprotokollen Probleme bei der Objektsynchronisierung ermitteln und beheben. Wählen Sie **Protokolle** aus, um die Protokolle anzuzeigen.

![Schaltfläche „Protokolle“](media/how-to-troubleshoot/log1.png)

In den Bereitstellungsprotokollen finden Sie sehr viele Informationen zum Status der Objekte, die zwischen der lokalen Active Directory-Umgebung und Azure synchronisiert werden.

![Bildschirm „Bereitstellungsprotokolle“](media/how-to-troubleshoot/log2.png)

Mithilfe der Dropdownfelder oben auf der Seite können Sie die Ansicht filtern, um sich auf bestimmte Probleme wie Datumsangaben zu konzentrieren. Doppelklicken Sie auf ein einzelnes Ereignis, damit nähere Informationen angezeigt werden.

![Dropdownfeld mit Informationen aus Bereitstellungsprotokollen](media/how-to-troubleshoot/log3.png)

Diese Informationen enthalten ausführliche Schritte und geben an, wo das Synchronisierungsproblem auftritt. So können Sie herausfinden, wo genau ein Problem aufgetreten ist.


## <a name="provisioning-quarantined-problems"></a>Bereitstellungsprobleme im Status „Quarantäne“

Die Cloudbereitstellung überwacht die Integrität Ihrer Konfiguration und versetzt fehlerhafte Objekte in den Status „Quarantäne“. Wenn die meisten oder alle Aufrufe an das Zielsystem aufgrund eines Fehlers (z. B. bei ungültigen Administratoranmeldeinformationen) dauerhaft nicht erfolgreich sind, wird der Bereitstellungsauftrag in den Quarantänestatus versetzt.

![Quarantänestatus](media/how-to-troubleshoot/quarantine1.png)

Durch Klicken auf den Status können Sie zusätzliche Informationen zur Quarantäne anzeigen. Sie können auch den Fehlercode und die Fehlermeldung abrufen.

![Informationen zum Quarantänestatus](media/how-to-troubleshoot/quarantine2.png)

### <a name="resolve-a-quarantine"></a>Aufheben einer Quarantäne

- Verwenden Sie das Azure-Portal, um den Bereitstellungsauftrag neu zu starten. Wählen Sie auf der Seite mit der Agent-Konfiguration die Option **Bereitstellung erneut starten** aus.

  ![Erneutes Starten der Bereitstellung](media/how-to-troubleshoot/quarantine3.png)

- Verwenden Sie Microsoft Graph, um [den Bereitstellungsauftrag neu zu starten](https://docs.microsoft.com/graph/api/synchronization-synchronizationjob-restart?view=graph-rest-beta&tabs=http). Sie haben vollständige Kontrolle über die Elemente, die Sie neu starten. Sie können auswählen, dass Folgendes geschieht:
  - Der Zählerwert wird auf 0 zurückgesetzt und steigt wieder an, sobald neue Elemente in Quarantäne verschoben werden.
  - Eine Anwendung wird aus der Quarantäne entfernt.
  - Grenzwerte werden geändert. 
  
  Verwenden Sie die folgende Anforderung:
 
  `POST /servicePrincipals/{id}/synchronization/jobs/{jobId}/restart`

## <a name="next-steps"></a>Nächste Schritte 

- [Was ist die Identitätsbereitstellung?](what-is-provisioning.md)
- [Was ist die Azure AD Connect-Cloudbereitstellung?](what-is-cloud-provisioning.md)



