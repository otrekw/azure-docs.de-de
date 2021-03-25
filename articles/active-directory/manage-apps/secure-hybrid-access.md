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
ms.date: 2/16/2021
ms.author: gasinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: a793ebb6d2b58718a6ee42c69c38b9da1b124722
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "104589394"
---
# <a name="secure-hybrid-access-secure-legacy-apps-with-azure-active-directory"></a>Sicherer Hybridzugriff: Schützen von Legacy-Anwendungen mit Azure Active Directory

Sie können jetzt Ihre lokalen und cloudbasierten Legacy-Authentifizierungsanwendungen schützen, indem Sie sie mithilfe folgender Methoden mit Azure Active Directory (Azure AD) verbinden:

- [Azure AD-Anwendungsproxy](#secure-hybrid-access-sha-through-azure-ad-application-proxy)

- [Vorhandene Controller und Netzwerke für die Anwendungsbereitstellung](#sha-through-networking-and-delivery-controllers)

- [Virtuelle private Netzwerke (VPN) und SDP-Anwendungen (Software-Defined Perimeter)](#sha-through-vpn-and-sdp-applications)

Mit Azure AD-Funktionen wie [Bedingter Zugriff](../conditional-access/overview.md) und [Azure AD Identity Protection](../identity-protection/overview-identity-protection.md) können Sie die Lücken schließen und den Sicherheitsstatus anwendungsübergreifend erhöhen.

## <a name="secure-hybrid-access-sha-through-azure-ad-application-proxy"></a>Sicherer Hybridzugriff über Azure AD-Anwendungsproxy
  
Mit dem [Anwendungsproxy](./what-is-application-proxy.md) können Sie einen [sicheren Remotezugriff](./application-proxy.md) auf Ihre lokalen Webanwendungen bereitstellen. Ihre Benutzer müssen kein VPN verwenden. Benutzer profitieren nach dem [einmaligen Anmelden](./add-application-portal-setup-sso.md) von einer einfachen Verbindung mit ihren Anwendungen von jedem Gerät aus. Der Anwendungsproxy stellt den Remotezugriff als Dienst bereit und ermöglicht die [einfache Veröffentlichung Ihrer lokalen Anwendungen](./application-proxy-add-on-premises-application.md) für Benutzer außerhalb des Unternehmensnetzwerks. Unterstützung bietet er auch bei der Skalierung Ihrer Cloudzugriffsverwaltung, ohne dass Sie Ihre lokalen Anwendungen ändern müssen. [Planen Sie die Bereitstellung eines Azure AD-Anwendungsproxys](./application-proxy-deployment-plan.md) als nächsten Schritt.

## <a name="azure-ad-partner-integrations"></a>Azure AD-Partnerintegrationen

### <a name="sha-through-networking-and-delivery-controllers"></a>Sicherer Hybridzugriff über Netzwerke und Bereitstellungscontroller

Zusätzlich zum [Azure AD-Anwendungsproxy](./what-is-application-proxy.md) können Sie das [Zero Trust-Framework](https://www.microsoft.com/security/blog/2020/04/02/announcing-microsoft-zero-trust-assessment-tool/) (Microsoft-Partner mit Drittanbietern) verwenden. Mithilfe Ihres vorhandenen Netzwerks und den Bereitstellungscontrollern können Sie auf einfache Weise Legacy-Anwendungen schützen, die für Ihre Geschäftsprozesse wichtig sind, bisher aber nicht mit Azure AD geschützt werden konnten. Wahrscheinlich erfüllen Sie bereits alle Voraussetzungen, um sofort mit dem Schützen dieser Anwendungen zu beginnen.

![Die Abbildung zeigt den sicheren Hybridzugriff mit Netzwerkpartnern und Anwendungsproxy](./media/secure-hybrid-access/secure-hybrid-access.png)

Die folgenden Netzwerkanbieter stellen vorgefertigte Lösungen und eine ausführliche Anleitung für die Integration mit Azure AD bereit.

- [Akamai Enterprise Application Access (EAA)](../saas-apps/akamai-tutorial.md)

- [Citrix Application Delivery Controller (ADC)](../saas-apps/citrix-netscaler-tutorial.md)

- [F5 Big-IP APM](./f5-aad-integration.md)

- [Kemp](../saas-apps/kemp-tutorial.md)

- [Pulse Secure Virtual Traffic Manager (VTM)](../saas-apps/pulse-secure-virtual-traffic-manager-tutorial.md)

### <a name="sha-through-vpn-and-sdp-applications"></a>Sicherer Hybridzugriff über VPN und SDP-Anwendungen

Mit VPN- und SDP-Lösungen können Sie jederzeit, von jedem Gerät und von jedem Ort aus den sicheren Zugriff auf Ihr Unternehmensnetzwerk bereitstellen und gleichzeitig die Daten Ihres Unternehmens schützen. Mit Azure AD als Identitätsanbieter können Sie moderne Authentifizierungs- und Autorisierungsmethoden wie [Einmaliges Anmelden](./what-is-single-sign-on.md) und [Multi-Factor Authentication](../authentication/concept-mfa-howitworks.md) von Azure AD verwenden, um Ihre lokalen Legacy-Anwendungen zu schützen.  

![Die Abbildung zeigt den sicheren Hybridzugriff mit VPN-Partnern und Anwendungsproxy ](./media/secure-hybrid-access/app-proxy-vpn.png)

Die folgenden VPN-Anbieter stellen vorgefertigte Lösungen und eine ausführliche Anleitung für die Azure AD-Integration bereit:

- [Cisco AnyConnect](../saas-apps/cisco-anyconnect.md)

- [Fortinet](../saas-apps/fortigate-ssl-vpn-tutorial.md)

- [F5 Big-IP APM](./f5-aad-password-less-vpn.md)

- [Palo Alto Networks Global Protect](../saas-apps/paloaltoadmin-tutorial.md)

- [Pulse Secure Pulse Connect Secure (PCS)](../saas-apps/pulse-secure-pcs-tutorial.md)

Die folgenden SDP-Anbieter stellen vorgefertigte Lösungen und eine ausführliche Anleitung für die Azure AD-Integration bereit:

- [Datawiza Access Broker](./add-application-portal-setup-oidc-sso.md)

- [Perimeter 81](../saas-apps/perimeter-81-tutorial.md)


- [Silverfort Authentication Platform](./add-application-portal-setup-oidc-sso.md)

- [Strata](../saas-apps/maverics-identity-orchestrator-saml-connector-tutorial.md)

- [Zscaler Private Access (ZPA)](../saas-apps/zscalerprivateaccess-tutorial.md)
