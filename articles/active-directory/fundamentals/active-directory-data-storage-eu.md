---
title: Identitätsdatenspeicher für europäische Kunden – Azure AD
description: Erfahren Sie, wo identitätsbezogene Daten für europäische Kunden durch Azure Active Directory gespeichert werden.
services: active-directory
author: ajburnle
manager: daveba
ms.author: ajburnle
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 03/04/2019
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: d57b7a559464bfff0d3d1c6436641257c925d75e
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89318470"
---
# <a name="identity-data-storage-for-european-customers-in-azure-active-directory"></a>Identitätsdatenspeicher für europäische Kunden in Azure Active Directory
Identitätsdaten werden von Azure AD an einem geografischen Standort basierend auf der Adresse gespeichert, die Ihre Organisation beim Abonnieren eines Microsoft-Onlinediensts wie Office 365 und Azure angibt. Informationen darüber, wo Ihre Identitätsdaten gespeichert werden, finden Sie im Microsoft Trust Center im Abschnitt [Wo wir Ihre Daten speichern](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located).

Azure AD speichert die meisten Identitätsdaten von Kunden, die eine Adresse in Europa angegeben haben, in europäischen Rechenzentren. Dieses Dokument enthält Informationen zu Daten, die von Azure AD-Diensten außerhalb von Europa gespeichert werden.

## <a name="microsoft-azure-multi-factor-authentication-mfa"></a>Microsoft Azure Multi-Factor Authentication (MFA)
    
- Alle zweistufigen Authentifizierungen per Telefon oder SMS stammen aus Rechenzentren in den USA und werden auch von globalen Anbietern weitergeleitet.
- Von der Microsoft Authenticator-App gesendete Pushbenachrichtigungen stammen aus Rechenzentren in den USA. Darüber hinaus können auch geräteherstellerspezifische Dienste hinzukommen, die möglicherweise außerhalb Europas angeboten werden.
- OATH-Codes werden immer in den USA überprüft. 

Weitere Informationen zu den Benutzerinformationen, die vom Azure MFA-Server (Multi-Factor Authentication Server) und von der cloudbasierten Azure MFA gesammelt werden, finden Sie unter [Azure Multi-Factor Authentication – Erfassen von Benutzerdaten](../authentication/howto-mfa-reporting-datacollection.md).

## <a name="microsoft-azure-active-directory-b2c-azure-ad-b2c"></a>Microsoft Azure Active Directory B2C (Azure AD B2C)

Azure AD B2C-Richtlinienkonfigurationsdaten und Schlüsselcontainer werden in Rechenzentren in den USA gespeichert. Diese enthalten keine persönlichen Benutzerdaten. Weitere Informationen zu Richtlinienkonfigurationen finden Sie im Artikel [Azure Active Directory B2C: Integrierte Richtlinien](../../active-directory-b2c/user-flow-overview.md).

## <a name="microsoft-azure-active-directory-b2b-azure-ad-b2b"></a>Microsoft Azure Active Directory B2B (Azure AD B2B) 
    
Azure AD B2B speichert Einladungen mit Informationen zu Einlösungslinks und Umleitungs-URLs in Rechenzentren in den USA. Darüber hinaus werden auch die E-Mail-Adressen von Benutzern, die B2B-Einladungen abbestellen, in Rechenzentren in den USA gespeichert.

## <a name="microsoft-azure-active-directory-domain-services-azure-ad-ds"></a>Microsoft Azure Active Directory Domain Services (Azure AD DS)

Für Azure AD DS werden die Benutzerdaten an dem Ort gespeichert, der vom Kunden für das virtuelle Azure-Netzwerk ausgewählt wurde. Wenn das Netzwerk also außerhalb von Europa liegt, werden die Daten außerhalb Europas repliziert und gespeichert.

## <a name="federation-in-microsoft-exchange-server-2013"></a>Verbund in Microsoft Exchange Server 2013
    
- Anwendungsbezeichner (AppID): eine eindeutige Zahl, die vom Azure Active Directory-Authentifizierungssystem zur Identifizierung von Exchange-Organisationen generiert wird.
- Liste mit genehmigten Verbunddomänen für die Anwendung
- Öffentlicher Schlüssel für das Signieren von Anwendungstoken 

Weitere Informationen zum Verbund in Microsoft Exchange Server finden Sie Hilfeartikel zum [Verbund: Exchange 2013](/exchange/federation-exchange-2013-help).


## <a name="other-considerations"></a>Weitere Überlegungen

Dienste und Anwendungen, die in Azure AD integriert werden, haben Zugriff auf Identitätsdaten. Evaluieren Sie alle von Ihnen verwendeten Dienste und Anwendungen, um zu ermitteln, wie Identitätsdaten vom jeweiligen Dienst und von der jeweiligen Anwendung verarbeitet werden und ob die Datenspeicheranforderungen Ihres Unternehmens erfüllt werden.

Weitere Informationen zur Datenaufbewahrung finden Sie im Microsoft Trust Center im Abschnitt [Wo sich Ihre Daten befinden](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu den oben beschriebenen Features und Funktionen finden Sie in diesen Artikeln:
- [Was ist Multi-Factor Authentication?](../authentication/concept-mfa-howitworks.md)

- [Self-Service-Kennwortzurücksetzung in Azure AD für IT-Experten](../authentication/concept-sspr-howitworks.md)

- [Was ist Azure Active Directory B2C?](../../active-directory-b2c/overview.md)

- [Was ist die Azure AD B2B-Zusammenarbeit?](../external-identities/what-is-b2b.md)

- [Azure Active Directory (AD) Domain Services](../../active-directory-domain-services/overview.md)