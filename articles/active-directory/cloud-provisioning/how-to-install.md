---
title: Installieren des Azure AD Connect-Bereitstellungs-Agents
description: In diesem Artikel wird ausführlich beschrieben, wie Sie den Bereitstellungs-Agent installieren können.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/02/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8cf479d4962f6d7aa9a0ba43b48f99bd67566fb6
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74795525"
---
# <a name="install-azure-ad-connect-provisioning-agent"></a>Installieren des Azure AD Connect-Bereitstellungs-Agents
In diesem Dokument erhalten Sie Informationen zum Installationsvorgang für den Azure AD Connect-Bereitstellungs-Agent und dessen Konfiguration im Azure-Portal.

>[!IMPORTANT]
>In den folgenden Installationsanweisungen wird davon ausgegangen, dass alle [Voraussetzungen](how-to-prerequisites.md) erfüllt sind.

Unter den folgenden Links erhalten Sie Informationen zur Installation und Konfiguration des Azure AD Connect-Bereitstellungs-Agents:
    
- [Installieren des Agents](#install-the-agent)
- [Überprüfen der Agent-Installation](#verify-agent-installation)


## <a name="install-the-agent"></a>Installieren des Agents

1. Melden Sie sich bei dem Server an, den Sie mit den Berechtigungen eines Unternehmensadministrators verwenden werden.
2. Navigieren Sie zum Azure-Portal, und klicken Sie auf der linken Seite auf Azure Active Directory.
3. Klicken Sie erst auf **Bereitstellung verwalten (Vorschau)** und dann auf **Alle Agents überprüfen**.
3. Laden Sie den Azure AD Connect-Bereitstellungs-Agent über das Azure-Portal herunter.
![Bildschirm „Willkommen“](media/how-to-install/install9.png)</br>
3. Führen Sie den Azure AD Connect-Bereitstellungs-Agent aus (AADConnectProvisioningAgent.Installer).
3. **Akzeptieren** Sie auf dem Begrüßungsbildschirm die Lizenzbedingungen, und klicken Sie auf **Installieren**.</br>
![Bildschirm „Willkommen“](media/how-to-install/install1.png)</br>

4. Nach Abschluss dieses Vorgangs wird der Konfigurations-Assistent gestartet.  Melden Sie sich mit dem Konto Ihres globalen Azure AD-Administrators an.
5. Klicken Sie auf dem Bildschirm **Connect Active Directory** (Active Directory verbinden) auf **Verzeichnis hinzufügen**, und melden Sie sich dann mit Ihrem Active Directory-Administratorkonto an.  Dadurch wird Ihr lokales Verzeichnis hinzugefügt.  Klicken Sie auf **Weiter**.</br>
![Bildschirm „Willkommen“](media/how-to-install/install3.png)</br>

6. Klicken Sie auf dem Bildschirm **Konfiguration abgeschlossen** auf **Bestätigen**.  Dadurch wird der Agent registriert und neu gestartet.</br>
![Bildschirm „Willkommen“](media/how-to-install/install4.png)</br>

7. Sobald dieser Vorgang abgeschlossen ist, sollte der Hinweis angezeigt werden, dass **die Agent-Konfiguration erfolgreich überprüft wurde**.  Sie können dann auf **Beenden** klicken.</br>
![Bildschirm „Willkommen“](media/how-to-install/install5.png)</br>
8. Falls der erste Begrüßungsbildschirm weiterhin angezeigt wird, klicken Sie auf **Schließen**.


## <a name="verify-agent-installation"></a>Überprüfen der Agent-Installation
Die Agent-Überprüfung erfolgt im Azure-Portal und auf dem lokalen Server, auf dem der Agent ausgeführt wird.

### <a name="azure-portal-agent-verification"></a>Agent-Überprüfung im Azure-Portal
Führen Sie die folgenden Schritte aus, um zu überprüfen, ob der Agent von Azure erkannt wird:

1. Melden Sie sich beim Azure-Portal an.
2. Wählen Sie auf der linken Seite **Azure Active Directory** aus, klicken Sie auf **Azure AD Connect**, und wählen Sie im mittleren Bereich **Bereitstellung verwalten (Vorschau)** aus.</br>
![Azure-Portal](media/how-to-install/install6.png)</br>

3.  Klicken Sie auf dem Bildschirm **Azure AD-Bereitstellung (Vorschau)** auf **Alle Agents überprüfen**.
![Azure AD-Bereitstellung](media/how-to-install/install7.png)</br>
 
4. Auf dem Bildschirm **On-premises provisioning agents** (Lokale Bereitstellungs-Agents) werden die von Ihnen installierten Agents angezeigt.  Vergewissern Sie sich, dass der betreffende Agent aufgeführt wird und als **Aktiviert** markiert ist.
![Bereitstellungs-Agents](media/how-to-install/verify1.png)</br>

### <a name="verify-the-port"></a>Überprüfen des Ports
Sie können den folgenden Link verwenden, um zu überprüfen, ob Azure Port 443 überwacht und ob der Agent mit diesem kommunizieren kann:

https://aadap-portcheck.connectorporttest.msappproxy.net/ 

Mithilfe dieses Tests wird überprüft, ob Ihre Agents über Port 443 mit Azure kommunizieren können.  Öffnen Sie einen Browser, und navigieren Sie zu der obigen URL des Servers, auf dem der Agent installiert ist.
![Dienste](media/how-to-install/verify2.png)

### <a name="on-the-local-server"></a>Auf dem lokalen Server
Führen Sie die folgenden Schritte aus, um sicherzustellen, dass der Agent ausgeführt wird:

1.  Melden Sie sich beim Server mit einem Administratorkonto an.
2.  Öffnen Sie **Dienste**, indem Sie zu dieser Seite oder zu „Start/Run/Services.msc“ navigieren.
3.  Vergewissern Sie sich, dass unter **Dienste** die Dienste **Microsoft Azure AD Connect Agent Updater** und **Microsoft Azure AD Connect-Bereitstellungs-Agent** angezeigt werden und ihr Status **Wird ausgeführt** lautet.
![Dienste](media/how-to-troubleshoot/troubleshoot1.png)

>[!IMPORTANT]
>Nachdem Sie den Agent installiert haben, muss er konfiguriert und aktiviert werden, bevor er mit der Synchronisierung von Benutzern beginnt.  Informationen zum Konfigurieren eines neuen Agents finden Sie unter [Konfigurieren des neuen Azure AD Connect-Bereitstellungs-Agents](how-to-configure.md).



## <a name="next-steps"></a>Nächste Schritte 

- [Was ist die Identitätsbereitstellung?](what-is-provisioning.md)
- [Was ist die Azure AD Connect-Cloudbereitstellung?](what-is-cloud-provisioning.md)
 
