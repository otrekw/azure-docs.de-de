---
title: 'Azure AD Connect: Überlegungen zur Hybrididentität für Azure Government'
description: Spezielle Überlegungen zum Bereitstellen von Azure AD Connect in der Government-Cloud.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 04/14/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2ad28beb68afb5207e7b36c5d76c4dac808c5114
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2020
ms.locfileid: "81377555"
---
# <a name="hybrid-identity-considerations-for-azure-government"></a>Überlegungen zur Hybrididentität für Azure Government 
Im folgenden Dokument werden Überlegungen zum Implementieren einer Hybridumgebung mit der Azure Government-Cloud beschrieben.  Diese Informationen dienen als Referenz für Administratoren und Architekten, die mit der Azure Government-Cloud arbeiten.  
> [!NOTE] 
> Um eine lokale AD-Umgebung in die Azure Government-Cloud zu integrieren, müssen Sie ein Upgrade auf das neueste Release von [Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) durchführen. 

> [!NOTE] 
> Eine vollständige Liste der DoD-Endpunkte der US-Regierung finden Sie in der [Dokumentation](https://docs.microsoft.com/office365/enterprise/office-365-u-s-government-dod-endpoints). 

## <a name="pass-through-authentication"></a>Passthrough-Authentifizierung 
Die folgenden Informationen werden für die Implementierung der Pass-Through-Authentifizierung (PTA) und der Azure Government-Cloud bereitgestellt.

### <a name="allow-access-to-urls"></a>Zulassen des Zugriffs auf URLs  
Überprüfen Sie vor dem Bereitstellen des Pass-Through-Authentifizierungs-Agents, ob zwischen den Servern und Azure AD eine Firewall vorhanden ist. Wenn Ihre Firewall oder Ihr Proxy DNS-Whitelists zulässt, fügen Sie die folgenden Verbindungen hinzu: 
> [!NOTE]
> Die folgende Anleitung gilt auch für die Installation des [Anwendungsproxyconnectors](https://aka.ms/whyappproxy) für Azure Government-Umgebungen.

|URL |Wie diese verwendet wird|
|-----|-----| 
|*.msappproxy.us *.servicebus.usgovcloudapi.net|Kommunikation zwischen dem Agent und dem Azure AD-Clouddienst |
|mscrl.microsoft.us:80 crl.microsoft.us:80 </br>ocsp.msocsp.us:80 www.microsoft.us:80| Der Agent verwendet diese URLs zum Überprüfen von Zertifikaten.| 
|login.windows.us secure.aadcdn.microsoftonline-p.com *.microsoftonline.us </br>* .microsoftonline-p.us </br>*.msauth.net </br>* .msauthimages.net </br>*.msecnd.net</br>* .msftauth.net </br>*.msftauthimages.net</br>* .phonefactor.net </br>enterpriseregistration.windows.net</br>management.azure.com </br>policykeyservice.dc.ad.msft.net</br>ctdl.windowsupdate.us:80| Der Agent verwendet diese URLs während der Registrierung.| 

### <a name="install-the-agent-for-the-azure-government-cloud"></a>Installieren des Agents für die Azure Government-Cloud 
Um den Agent für die Azure Government-Cloud zu installieren, müssen Sie diese Schritte ausführen: Navigieren Sie im Befehlszeilenterminal zu dem Ordner, in dem sich die ausführbare Datei für die Installation des Agents befindet. Führen Sie den folgenden Befehl aus, der eine Installation für Azure Government angibt. 

Für die Pass-Through-Authentifizierung: 
```
AADConnectAuthAgentSetup.exe ENVIRONMENTNAME="AzureUSGovernment"
```

Für den Anwendungsproxy:
```
AADApplicationProxyConnectorInstaller.exe ENVIRONMENTNAME="AzureUSGovernment" 
```

## <a name="single-sign-on"></a>Einmaliges Anmelden 
Richten Sie Ihren Azure AD Connect-Server ein: Wenn Sie die Pass-Through-Authentifizierung als Anmeldemethode verwenden, ist keine zusätzliche Überprüfung der Voraussetzungen erforderlich. Wenn Ihre Anmeldemethode die Kennworthashsynchronisierung ist und eine Firewall zwischen Azure AD Connect und Azure AD vorhanden ist, sollten Sie Folgendes sicherstellen:
- Sie verwenden Azure AD Connect 1.1.644.0 oder eine höhere Version. 
- Wenn Ihre Firewall oder Ihr Proxy DNS-Whitelisting zulässt, fügen Sie die URLs von *.msapproxy.us an Port 443 hinzu. Aktivieren Sie andernfalls den Zugriff auf die IP-Adressbereiche für das Azure-Rechenzentrum, die wöchentlich aktualisiert werden. Diese Voraussetzung gilt nur, wenn Sie das Feature aktivieren. Sie ist für tatsächliche Benutzeranmeldungen nicht erforderlich. 

### <a name="rolling-out-seamless-sso"></a>Einführen von nahtlosem SSO 
Sie können mithilfe der unten stehenden Anleitung nach und nach das Rollout des nahtlosen einmaligen Anmeldens für Ihre Benutzer ausführen. Sie fügen zuerst mithilfe der Gruppenrichtlinie in Active Directory den Intranetzoneneinstellungen aller oder ausgewählter Benutzer die folgende Azure AD-URL hinzu: https://autologon.microsoft.us 

Darüber hinaus müssen Sie mithilfe der Gruppenrichtlinie die Richtlinieneinstellung für eine Intranetzone namens „Aktualisierungen der Statusleiste per Skript zulassen“ aktivieren. Überlegungen zum Browser Mozilla Firefox (alle Plattformen): Mozilla Firefox verwendet nicht automatisch die Kerberos-Authentifizierung. Jeder Benutzer muss den Firefox-Einstellungen manuell die Azure AD-URL mithilfe der folgenden Schritte hinzufügen: 
1. Führen Sie Firefox aus, und geben Sie in die Adressleiste „about:config“ ein. Schließen Sie alle Benachrichtigungen, die Sie sehen. 
2. Suchen Sie nach der Einstellung „network.negotiate-auth.trusted-uris“. In dieser Einstellung werden in Firefox die vertrauenswürdigen Sites für die Kerberos-Authentifizierung aufgeführt. 
3. Klicken Sie mit der rechten Maustaste darauf, und wählen Sie dann „Ändern“ aus. 
4. Geben Sie https://autologon.microsoft.us in das Feld ein.
5. Wählen Sie „OK“ aus, und öffnen Sie den Browser erneut. 

### <a name="microsoft-edge-based-on-chromium-all-platforms"></a>Microsoft Edge auf Chromium-Basis (alle Plattformen) 
Wenn Sie die Richtlinieneinstellungen `AuthNegotiateDelegateAllowlist` oder `AuthServerAllowlist` in Ihrer Umgebung außer Kraft gesetzt haben, stellen Sie sicher, dass Sie ihnen auch die URL von Azure AD (https://autologon.microsoft.us) hinzufügen. 

### <a name="google-chrome-all-platforms"></a>Google Chrome (alle Plattformen) 
Wenn Sie die Richtlinieneinstellungen `AuthNegotiateDelegateWhitelist` oder `AuthServerWhitelist` in Ihrer Umgebung außer Kraft gesetzt haben, stellen Sie sicher, dass Sie ihnen auch die URL von Azure AD (https://autologon.microsoft.us) hinzufügen. 

## <a name="next-steps"></a>Nächste Schritte
[Pass-Through-Authentifizierung](how-to-connect-pta-quick-start.md#step-1-check-the-prerequisites)
[Einmaliges Anmelden](how-to-connect-sso-quick-start.md#step-1-check-the-prerequisites) 
