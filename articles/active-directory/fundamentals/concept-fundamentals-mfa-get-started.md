---
title: Azure Multi-Factor Authentication für Ihre Organisation – Azure Active Directory
description: Erfahren Sie mehr über die verfügbaren Features von Azure Multi-Factor Authentication für Ihre Organisation auf der Grundlage Ihres Lizenzmodells.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 03/18/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 38f3a6d9cea1aa1ebcb76f61882dcf2615dc4832
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85554651"
---
# <a name="overview-of-azure-multi-factor-authentication-for-your-organization"></a>Übersicht über Azure Multi-Factor Authentication für Ihre Organisation

Es gibt mehrere Möglichkeiten, Azure Multi-Factor Authentication für Ihre Benutzer von Azure Active Directory (Azure AD) basierend auf den Lizenzen zu aktivieren, die Ihre Organisation besitzt. 

![Signale untersuchen und bei Bedarf MFA erzwingen](./media/concept-fundamentals-mfa-get-started/verify-signals-and-perform-mfa-if-required.png)

Basierend auf unseren Studien ist Ihr Konto bei Verwendung von MultiFactor Authentication (MFA) mehr als 99,9 % weniger wahrscheinlich gefährdet.

Wie kann Ihre Organisation MFA sogar kostenlos aktivieren, bevor sie Teil einer Statistik wird?

## <a name="free-option"></a>Kostenlose Option

Kunden, die die kostenlosen Vorteile von Azure AD nutzen, können mithilfe von [Sicherheitsstandards](../fundamentals/concept-fundamentals-security-defaults.md) die mehrstufige Authentifizierung in ihrer Umgebung aktivieren.

## <a name="microsoft-365-business-e3-or-e5"></a>Microsoft 365 Business, E3 oder E5

Kunden, die über Office 365 verfügen, stehen zwei Optionen zur Verfügung:

* Azure Multi-Factor Authentication ist für alle Benutzer und alle Anmeldeereignisse entweder aktiviert oder deaktiviert. Es gibt keine Möglichkeit, die mehrstufige Authentifizierung nur für eine Teilmenge von Benutzern oder nur bei bestimmten Szenarien zu aktivieren. Die Verwaltung erfolgt über das Office 365-Portal. 
* Führen Sie zugunsten einer optimierten Benutzerumgebung ein Upgrade auf Azure AD Premium P1 oder P2 durch, und verwenden Sie den bedingten Zugriff. Weitere Informationen finden Sie unter „Schützen von Office 365-Ressourcen mit der mehrstufigen Authentifizierung“.

## <a name="azure-ad-premium-p1"></a>Azure AD Premium P1

Für Kunden, die über Azure AD Premium P1- oder ähnliche Lizenzen mit dieser Funktionalität verfügen (z. B. Enterprise Mobility + Security E3, Microsoft 365 F1 oder Microsoft 365 E3), gilt Folgendes: 

Mit dem [bedingten Zugriff von Azure AD](../authentication/tutorial-enable-azure-mfa.md) können Sie Benutzer bei bestimmten Szenarien oder Ereignissen zur mehrstufigen Authentifizierung auffordern und auf diese Weise Ihre geschäftlichen Anforderungen erfüllen.

## <a name="azure-ad-premium-p2"></a>Azure AD Premium P2

Für Kunden, die über Azure AD Premium P2- oder ähnliche Lizenzen mit dieser Funktionalität verfügen (z. B. Enterprise Mobility + Security E5 oder Microsoft 365 E5), gilt Folgendes: 

Bietet die höchste Sicherheitsstufe und eine verbesserte Benutzerumgebung. Erweitert die Funktionen von Azure AD Premium P1 um den [risikobasierten bedingten Zugriff](../conditional-access/howto-conditional-access-policy-risk.md), der sich an Benutzermuster anpasst und Eingabeaufforderungen für die mehrstufige Authentifizierung minimiert.

## <a name="authentication-methods"></a>Authentifizierungsmethoden

| Methode | Standardwerte für die Sicherheit | Alle anderen Methoden |
| --- | --- | --- |
| Benachrichtigung über mobile App | X | X |
| Prüfcode aus mobiler App oder Hardwaretoken |   | X |
| Textnachricht an Telefon |   | X |
| Auf Telefon anrufen |   | X |

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich zum Einstieg das Tutorial zum [Schützen von Benutzeranmeldeereignissen mit Azure Multi-Factor Authentication](../authentication/tutorial-enable-azure-mfa.md) an.

Weitere Informationen zur Lizenzierung finden Sie unter [Features und Lizenzen für Azure Multi-Factor Authentication](../authentication/concept-mfa-licensing.md).
