---
title: Installieren des Agents für die Azure AD Connect-Cloudbereitstellung
description: In diesem Artikel wird das Installieren des Agents für die Azure AD Connect-Cloudbereitstellung beschrieben.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/26/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: e4581ab89f74425682f569425f62714ead2c27c5
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77620933"
---
# <a name="install-the-azure-ad-connect-cloud-provisioning-agent"></a>Installieren des Agents für die Azure AD Connect-Cloudbereitstellung
In diesem Dokument erhalten Sie Informationen zum Installationsvorgang für den Azure AD Connect-Bereitstellungs-Agent (Azure Active Directory) und dessen Erstkonfiguration im Azure-Portal.

>[!IMPORTANT]
>In den folgenden Installationsanweisungen wird davon ausgegangen, dass alle [Voraussetzungen](how-to-prerequisites.md) erfüllt sind.

Unter den folgenden Links erhalten Sie Informationen zur Installation und Konfiguration des Azure AD Connect-Bereitstellungs-Agents:
    
- [Installieren des Agents](#install-the-agent)
- [Überprüfen der Agent-Installation](#verify-agent-installation)


## <a name="install-the-agent"></a>Installieren des Agents
Führen Sie die folgenden Schritte aus, um den Agent zu installieren.

1. Melden Sie sich mit Berechtigungen eines Unternehmensadministrators bei dem Server an, den Sie verwenden werden.
1. Öffnen Sie das Azure-Portal. Wählen Sie im linken Bereich **Azure Active Directory** aus.
1. Wählen Sie **Bereitstellung verwalten (Vorschau)**  > **Alle Agents überprüfen** aus.
1. Laden Sie den Azure AD Connect-Bereitstellungs-Agent über das Azure-Portal herunter.

   ![Herunterladen des lokalen Agents](media/how-to-install/install9.png)</br>
1. Führen Sie das Installationsprogramm für den Azure AD Connect-Bereitstellungs-Agent aus („AADConnectProvisioningAgent.Installer“).
1. Akzeptieren Sie auf dem Bildschirm **Microsoft Azure AD Connect-Bereitstellungs-Agent-Paket** die Lizenzbedingungen, und wählen Sie **Installieren** aus.

   ![Bildschirm „Microsoft Azure AD Connect-Bereitstellungs-Agent-Paket“](media/how-to-install/install1.png)</br>

1. Nach Abschluss dieses Vorgangs wird der Konfigurations-Assistent gestartet. Melden Sie sich mit dem Konto Ihres globalen Azure AD-Administrators an.
1. Wählen Sie auf dem Bildschirm **Active Directory verbinden** die Option **Verzeichnis hinzufügen** aus. Melden Sie sich dann mit Ihrem Active Directory-Administratorkonto an. Dadurch wird Ihr lokales Verzeichnis hinzugefügt. Wählen Sie **Weiter** aus.

   ![Bildschirm „Active Directory verbinden“](media/how-to-install/install3.png)</br>

1. Wählen Sie auf dem Bildschirm **Konfiguration abgeschlossen** die Option **Bestätigen** aus. Damit wird der Agent registriert und neu gestartet.

   ![Bildschirm „Konfiguration abgeschlossen“](media/how-to-install/install4.png)</br>

1. Nach Abschluss dieses Vorgangs sollte der Hinweis **Ihre Agent-Konfiguration wurde erfolgreich überprüft** angezeigt werden. Wählen Sie **Beenden** aus.

   ![Schaltfläche „Beenden“](media/how-to-install/install5.png)</br>
1. Wenn weiterhin der erste Bildschirm **Microsoft Azure AD Connect-Bereitstellungs-Agent-Paket** angezeigt wird, wählen Sie **Schließen** aus.

## <a name="verify-agent-installation"></a>Überprüfen der Agent-Installation
Die Agent-Überprüfung erfolgt im Azure-Portal und auf dem lokalen Server, auf dem der Agent ausgeführt wird.

### <a name="azure-portal-agent-verification"></a>Agent-Überprüfung im Azure-Portal
Führen Sie die folgenden Schritte aus, um zu überprüfen, ob der Agent von Azure erkannt wird.

1. Melden Sie sich beim Azure-Portal an.
1. Wählen Sie auf der linken Seite **Azure Active Directory** > **Azure AD Connect** aus. Wählen Sie in der Mitte **Bereitstellung verwalten (Vorschau)** aus.

   ![Azure-Portal](media/how-to-install/install6.png)</br>

1.  Wählen Sie auf dem Bildschirm **Azure AD-Bereitstellung (Vorschau)** die Option **Alle Agents überprüfen** aus.

    ![Option „Alle Agents überprüfen“](media/how-to-install/install7.png)</br>
 
1. Auf dem Bildschirm **Lokale Bereitstellungs-Agents** werden die von Ihnen installierten Agents angezeigt. Vergewissern Sie sich, dass der betreffende Agent aufgeführt wird und als *Aktiviert* markiert ist.

   ![Bildschirm „Lokale Bereitstellungs-Agents“](media/how-to-install/verify1.png)</br>

### <a name="verify-the-port"></a>Überprüfen des Ports
Anhand der folgenden Schritte können Sie überprüfen, ob Azure an Port 443 lauscht und ob der Agent mit diesem kommunizieren kann.

https://aadap-portcheck.connectorporttest.msappproxy.net/ 

Mithilfe dieses Tests wird überprüft, ob Ihre Agents über Port 443 mit Azure kommunizieren können. Öffnen Sie einen Browser, und navigieren Sie von dem Server, auf dem der Agent installiert ist, zur obigen URL.

![Überprüfung der Porterreichbarkeit](media/how-to-install/verify2.png)

### <a name="on-the-local-server"></a>Auf dem lokalen Server
Führen Sie die folgenden Schritte aus, um zu überprüfen, ob der Agent ausgeführt wird.

1.  Melden Sie sich auf dem Server mit einem Administratorkonto an.
1.  Öffnen Sie die Seite **Dienste**, indem Sie zu dieser Seite navigieren oder **Start** > **Run** > **Services.msc** ausführen.
1.  Vergewissern Sie sich, dass unter **Dienste** die Dienste **Microsoft Azure AD Connect Agent Updater** und **Microsoft Azure AD Connect Provisioning Agent** mit dem Status *Wird ausgeführt* angezeigt werden.

    ![Bildschirm „Dienste“](media/how-to-troubleshoot/troubleshoot1.png)

>[!IMPORTANT]
>Nachdem Sie den Agent installiert haben, muss er konfiguriert und aktiviert werden, bevor er mit der Synchronisierung von Benutzern beginnen kann. Informationen zum Konfigurieren eines neuen Agents finden Sie unter [Erstellen einer neuen Konfiguration für die cloudbasierte Azure AD Connect-Bereitstellung](how-to-configure.md).



## <a name="next-steps"></a>Nächste Schritte 

- [Was ist die Identitätsbereitstellung?](what-is-provisioning.md)
- [Was ist die Azure AD Connect-Cloudbereitstellung?](what-is-cloud-provisioning.md)
 
