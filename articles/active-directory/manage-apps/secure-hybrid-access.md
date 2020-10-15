---
title: Sicherer Hybridzugriff auf Azure AD | Microsoft-Dokumentation
description: In diesem Artikel werden Partnerlösungen für die Integration Ihrer lokalen Legacy-, Public Cloud- oder Private Cloud-Anwendungen in Azure AD beschrieben. Schützen Sie Ihre Legacy-Apps, indem Sie App-Übermittlungscontroller oder Netzwerke mit Azure AD verbinden.
services: active-directory
author: gargi-sinha
manager: martinco
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: how-to
ms.workload: identity
ms.date: 9/10/2020
ms.author: gasinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4d64f7423d537958b6d3c388cb12f23bd2e30e36
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90087081"
---
# <a name="secure-hybrid-access-secure-legacy-apps-with-azure-active-directory"></a>Sicherer Hybridzugriff: Schützen von Legacy-Anwendungen mit Azure Active Directory

Sie können jetzt Ihre lokalen und cloudbasierten Legacy-Authentifizierungsanwendungen schützen, indem Sie sie mithilfe folgender Methoden mit Azure Active Directory (Azure AD) verbinden:

- [Azure AD-Anwendungsproxy](#secure-hybrid-access-sha-through-azure-ad-application-proxy)

- [Vorhandene Controller und Netzwerke für die Anwendungsbereitstellung](#sha-through-networking-and-delivery-controllers)

- [Virtuelle private Netzwerke (VPN) und SDP-Anwendungen (Software-Defined Perimeter)](#sha-through-vpn-and-sdp-applications)

Mit Azure AD-Funktionen wie [Bedingter Zugriff](https://docs.microsoft.com/azure/active-directory/conditional-access/overview) und [Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/identity-protection/overview-identity-protection) können Sie die Lücken schließen und den Sicherheitsstatus anwendungsübergreifend erhöhen.

## <a name="secure-hybrid-access-sha-through-azure-ad-application-proxy"></a>Sicherer Hybridzugriff über Azure AD-Anwendungsproxy
  
Mit dem [Anwendungsproxy](https://aka.ms/whyappproxy) können Sie einen [sicheren Remotezugriff](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy) auf Ihre lokalen Webanwendungen bereitstellen. Ihre Benutzer müssen kein VPN verwenden. Benutzer profitieren nach dem [einmaligen Anmelden](https://docs.microsoft.com/azure/active-directory/manage-apps/add-application-portal-setup-sso) von einer einfachen Verbindung mit ihren Anwendungen von jedem Gerät aus. Der Anwendungsproxy stellt den Remotezugriff als Dienst bereit und ermöglicht die [einfache Veröffentlichung Ihrer lokalen Anwendungen](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application) für Benutzer außerhalb des Unternehmensnetzwerks. Unterstützung bietet er auch bei der Skalierung Ihrer Cloudzugriffsverwaltung, ohne dass Sie Ihre lokalen Anwendungen ändern müssen. [Planen Sie die Bereitstellung eines Azure AD-Anwendungsproxys](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-deployment-plan) als nächsten Schritt.

## <a name="azure-ad-partner-integrations"></a>Azure AD-Partnerintegrationen

### <a name="sha-through-networking-and-delivery-controllers"></a>Sicherer Hybridzugriff über Netzwerke und Bereitstellungscontroller

Zusätzlich zum [Azure AD-Anwendungsproxy](https://aka.ms/whyappproxy) können Sie das [Zero Trust-Framework](https://www.microsoft.com/security/blog/2020/04/02/announcing-microsoft-zero-trust-assessment-tool/) (Microsoft-Partner mit Drittanbietern) verwenden. Mithilfe Ihres vorhandenen Netzwerks und den Bereitstellungscontrollern können Sie auf einfache Weise Legacy-Anwendungen schützen, die für Ihre Geschäftsprozesse wichtig sind, bisher aber nicht mit Azure AD geschützt werden konnten. Wahrscheinlich erfüllen Sie bereits alle Voraussetzungen, um sofort mit dem Schützen dieser Anwendungen zu beginnen.

![Die Abbildung zeigt den sicheren Hybridzugriff mit Netzwerkpartnern und Anwendungsproxy](./media/secure-hybrid-access/secure-hybrid-access.png)

Die folgenden Netzwerkanbieter stellen vorgefertigte Lösungen und eine ausführliche Anleitung für die Integration mit Azure AD bereit.

- [Akamai Enterprise Application Access (EAA)](https://docs.microsoft.com/azure/active-directory/saas-apps/akamai-tutorial)

- [Citrix Application Delivery Controller (ADC)](https://docs.microsoft.com/azure/active-directory/saas-apps/citrix-netscaler-tutorial)

- [F5 Big-IP APM](https://docs.microsoft.com/azure/active-directory/saas-apps/headerf5-tutorial)

- [Kemp](https://docs.microsoft.com/azure/active-directory/saas-apps/kemp-tutorial)

### <a name="sha-through-vpn-and-sdp-applications"></a>Sicherer Hybridzugriff über VPN und SDP-Anwendungen

Mit VPN- und SDP-Lösungen können Sie jederzeit, von jedem Gerät und von jedem Ort aus den sicheren Zugriff auf Ihr Unternehmensnetzwerk bereitstellen und gleichzeitig die Daten Ihres Unternehmens schützen. Mit Azure AD als Identitätsanbieter können Sie moderne Authentifizierungs- und Autorisierungsmethoden wie [Einmaliges Anmelden](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on) und [Multi-Factor Authentication](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks) von Azure AD verwenden, um Ihre lokalen Legacy-Anwendungen zu schützen.  

![Die Abbildung zeigt den sicheren Hybridzugriff mit VPN-Partnern und Anwendungsproxy ](./media/secure-hybrid-access/app-proxy-vpn.png)

Die folgenden VPN- und SDP-Anbieter stellen vorgefertigte Lösungen und eine ausführliche Anleitung für die Integration mit Azure AD bereit.

• [Cisco AnyConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-anyconnect)

• [Fortinet](https://docs.microsoft.com/azure/active-directory/saas-apps/fortigate-ssl-vpn-tutorial)

• [F5 Big-IP APM](https://aka.ms/f5-hybridaccessguide)

• [Palo Alto Networks Global Protect](https://docs.microsoft.com/azure/active-directory/saas-apps/paloaltoadmin-tutorial)

• [Zscaler Private Access (ZPA)](https://aka.ms/zscaler-hybridaccessguide)
