---
title: Installieren des Azure AD Connect-Bereitstellungs-Agents
description: Erfahren Sie, wie Sie den Azure AD Connect-Bereitstellungs-Agent installieren und im Azure-Portal konfigurieren.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 11/16/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 742b9fc79489feba8192b6e62a6431bb37f55ad4
ms.sourcegitcommit: 8a74ab1beba4522367aef8cb39c92c1147d5ec13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2021
ms.locfileid: "98612666"
---
# <a name="install-the-azure-ad-connect-provisioning-agent"></a>Installieren des Azure AD Connect-Bereitstellungs-Agents
In diesem Dokument erhalten Sie Informationen zum Installationsvorgang für den Azure AD Connect-Bereitstellungs-Agent (Azure Active Directory) und dessen Erstkonfiguration im Azure-Portal.

>[!IMPORTANT]
>In den folgenden Installationsanweisungen wird davon ausgegangen, dass alle [Voraussetzungen](how-to-prerequisites.md) erfüllt sind.

Die Installation und Konfiguration der Azure AD Connect-Cloudsynchronisierung erfolgt im Rahmen der folgenden Schritte:

- [Gruppenverwaltete Dienstkonten](#group-managed-service-accounts) 
- [Installieren des Agents](#install-the-agent)
- [Überprüfen der Agent-Installation](#verify-agent-installation)


## <a name="group-managed-service-accounts"></a>Gruppenverwaltete Dienstkonten
Ein gruppenverwaltetes Dienstkonto ist ein verwaltetes Domänenkonto, das eine automatische Kennwortverwaltung, eine vereinfachte Verwaltung von Dienstprinzipalnamen (Service Principal Name, SPN) und die Möglichkeit bietet, die Verwaltung an andere Administratoren zu delegieren, wobei diese Funktionalität auch auf mehrere Server erweitert wird.  Azure AD Connect Cloud Sync unterstützt und empfiehlt die Verwendung eines gruppenverwalteten Dienstkontos für die Ausführung des Agents.  Weitere Informationen zu einem gruppenverwalteten Dienstkonto (Group Managed Service Account, GMSA) finden Sie unter [Gruppenverwaltete Dienstkonten](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview). 


### <a name="upgrading-an-existing-agent-to-use-the-gmsa-account"></a>Aktualisieren eines vorhandenen Agents für die Verwendung des gruppenverwalteten Dienstkontos
Um einen vorhandenen Agent für die Verwendung des während der Installation erstellten gruppenverwalteten Dienstkontos zu aktualisieren, müssen Sie einfach den Agent-Dienst auf die neueste Version aktualisieren, indem Sie die Datei „AADConnectProvisioningAgent.msi“ ausführen.  Dadurch wird der Dienst auf die neueste Version aktualisiert.  Führen Sie jetzt den Installations-Assistenten erneut aus, und geben Sie bei entsprechender Aufforderung die Anmeldeinformationen ein, um das Konto zu erstellen.



## <a name="install-the-agent"></a>Installieren des Agents
Führen Sie die folgenden Schritte aus, um den Agent zu installieren.

 1. Melden Sie sich mit Berechtigungen eines Unternehmensadministrators bei dem Server an, den Sie verwenden werden.
 2. Melden Sie sich beim Azure-Portal an, und navigieren Sie zu **Azure Active Directory**.
 3. Wählen Sie im linken Menü die Option **Azure AD Connect** aus.
 4. Wählen Sie **Cloudsynchronisierung verwalten** > **Alle Agents überprüfen** aus.
 5. Laden Sie den Azure AD Connect-Bereitstellungs-Agent über das Azure-Portal herunter.
   ![Herunterladen des lokalen Agents](media/how-to-install/install-9.png)</br>
 6. Akzeptieren Sie die Bedingungen, und klicken Sie auf „Herunterladen“.
 7. Führen Sie das Installationsprogramm für den Azure AD Connect-Bereitstellungs-Agent („AADConnectProvisioningAgentSetup.msi“) aus.
 8. Akzeptieren Sie auf dem Bildschirm **Microsoft Azure AD Connect-Bereitstellungs-Agent-Paket** die Lizenzbedingungen, und wählen Sie **Installieren** aus.
   ![Bildschirm „Microsoft Azure AD Connect-Bereitstellungs-Agent-Paket“](media/how-to-install/install-1.png)</br>
 9. Nach Abschluss dieses Vorgangs wird der Konfigurations-Assistent gestartet. Melden Sie sich mit dem Konto Ihres globalen Azure AD-Administrators an.
 10. Wählen Sie auf dem Bildschirm **Dienstkonto konfigurieren** entweder die Option **GMSA erstellen** oder **Benutzerdefiniertes GMSA verwenden** aus.  Wenn Sie das Konto vom Agent erstellen lassen, wird dem Konto der Name „provAgentgMSA$“ zugewiesen. Wenn Sie **Benutzerdefiniertes GMSA verwenden** auswählen, werden Sie aufgefordert, dieses Konto anzugeben.
 11. Geben Sie die Anmeldeinformationen des Domänenadministrators ein, um das gruppenverwaltete Dienstkonto zu erstellen, das zum Ausführen des Agent-Diensts verwendet wird. Klicken Sie auf **Weiter**.  
   ![Gruppenverwaltetes Dienstkonto erstellen](media/how-to-install/install-12.png)</br>
 12. Wählen Sie auf dem Bildschirm **Active Directory verbinden** die Option **Verzeichnis hinzufügen** aus. Melden Sie sich dann mit Ihrem Active Directory-Administratorkonto an. Dadurch wird Ihr lokales Verzeichnis hinzugefügt. 
 13. Optional können Sie die Präferenz der vom Agent verwendeten Domänencontroller verwalten, indem Sie das Kontrollkästchen **Domänencontrollerpriorität auswählen** aktivieren und die Liste der Domänencontroller entsprechend sortieren.   Klicken Sie auf **OK**.
  ![Ändern der Reihenfolge von Domänencontrollern](media/how-to-install/install-2a.png)</br>
 14. Wählen Sie **Weiter** aus.
  ![Bildschirm „Active Directory verbinden“](media/how-to-install/install-3a.png)</br>
 15.  Überprüfen Sie auf dem Bildschirm **Agent-Installation** die Einstellungen und das zu erstellende Konto, und klicken Sie dann auf **Bestätigen**.
  ![Bestätigen der Einstellungen](media/how-to-install/install-11.png)</br>
 16. Nach Abschluss dieses Vorgangs sollte die Meldung **Ihre Agent-Installation ist abgeschlossen** angezeigt werden. Wählen Sie **Beenden** aus.
  ![Bildschirm „Konfiguration abgeschlossen“](media/how-to-install/install-4a.png)</br>
 17. Wenn weiterhin der erste Bildschirm **Microsoft Azure AD Connect-Bereitstellungs-Agent-Paket** angezeigt wird, wählen Sie **Schließen** aus.

## <a name="verify-agent-installation"></a>Überprüfen der Agent-Installation
Die Agent-Überprüfung erfolgt im Azure-Portal und auf dem lokalen Server, auf dem der Agent ausgeführt wird.

### <a name="azure-portal-agent-verification"></a>Agent-Überprüfung im Azure-Portal
Führen Sie die folgenden Schritte aus, um zu überprüfen, ob der Agent von Azure erkannt wird.

 1. Melden Sie sich beim Azure-Portal an.
 2. Wählen Sie auf der linken Seite **Azure Active Directory** > **Azure AD Connect** aus. Wählen Sie in der Mitte die Option **Cloudsynchronisierung verwalten** aus.

   ![Azure-Portal](media/how-to-install/install-6.png)</br>

 3.  Wählen Sie auf dem Bildschirm **Azure AD Connect-Cloudsynchronisierung** die Option **Alle Agents überprüfen** aus.

   ![Option „Alle Agents überprüfen“](media/how-to-install/install-7.png)</br>
 
 4. Auf dem Bildschirm **Lokale Bereitstellungs-Agents** werden die von Ihnen installierten Agents angezeigt. Vergewissern Sie sich, dass der betreffende Agent aufgeführt wird und als *Aktiviert* markiert ist.

   ![Bildschirm „Lokale Bereitstellungs-Agents“](media/how-to-install/verify-1.png)</br>



### <a name="on-the-local-server"></a>Auf dem lokalen Server
Führen Sie die folgenden Schritte aus, um zu überprüfen, ob der Agent ausgeführt wird.

1.  Melden Sie sich auf dem Server mit einem Administratorkonto an.
1.  Öffnen Sie die Seite **Dienste**, indem Sie zu dieser Seite navigieren oder **Start** > **Run** > **Services.msc** ausführen.
1.  Vergewissern Sie sich, dass unter **Dienste** die Dienste **Microsoft Azure AD Connect Agent Updater** und **Microsoft Azure AD Connect Provisioning Agent** mit dem Status *Wird ausgeführt* angezeigt werden.

    ![Bildschirm „Dienste“](media/how-to-install/troubleshoot-1.png)

>[!IMPORTANT]
>Nachdem Sie den Agent installiert haben, muss er konfiguriert und aktiviert werden, bevor er mit der Synchronisierung von Benutzern beginnen kann. Informationen zum Konfigurieren eines neuen Agents finden Sie unter [Erstellen einer neuen Konfiguration für die Azure AD Connect-Cloudsynchronisierung](how-to-configure.md).




## <a name="next-steps"></a>Nächste Schritte 

- [Was ist die Identitätsbereitstellung?](what-is-provisioning.md)
- [Worum handelt es sich bei der Azure AD Connect-Cloudsynchronisierung?](what-is-cloud-sync.md)
- [Erstellen einer neuen Konfiguration für die Azure AD Connect-Cloudsynchronisierung](how-to-configure.md)

