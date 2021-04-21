---
title: 'Identitätsdatenspeicher für Kunden in Australien und Neuseeland: Azure AD'
description: Hier erfahren Sie, wo identitätsbezogene Daten für australische und neuseeländische Kunden durch Azure Active Directory gespeichert werden.
services: active-directory
author: ajburnle
manager: daveba
ms.author: ajburnle
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 12/13/2019
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4bb095e93a3728835e26cbe283f79569c91b7487
ms.sourcegitcommit: afb79a35e687a91270973990ff111ef90634f142
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107479065"
---
# <a name="identity-data-storage-for-australian-and-new-zealand-customers-in-azure-active-directory"></a>Identitätsdatenspeicher für Kunden in Australien und Neuseeland in Azure Active Directory

Identitätsdaten werden von Azure AD an einem geografischen Standort gespeichert, der auf der Adresse beruht, die Ihre Organisation beim Abonnieren eines Microsoft-Onlinediensts wie Microsoft 365 und Azure angegeben hat. Informationen darüber, wo die identitätsbezogenen Kundendaten gespeichert werden, finden Sie im Microsoft Trust Center im Abschnitt [Wo wir Ihre Daten speichern](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located).

> [!NOTE]
> Dienste und Anwendungen, die in Azure AD integriert werden, haben Zugriff auf identitätsbezogene Kundendaten. Evaluieren Sie alle von Ihnen verwendeten Dienste und Anwendungen, um zu ermitteln, wie identitätsbezogenen Kundendaten vom jeweiligen Dienst und von der jeweiligen Anwendung verarbeitet werden und ob die Datenspeicheranforderungen Ihres Unternehmens erfüllt werden. Weitere Informationen zur Datenaufbewahrung bei Microsoft-Diensten finden Sie im Microsoft Trust Center im Abschnitt „Wo wir Ihre Daten speichern“.

Für Kunden, die eine Adresse in Australien oder Neuseeland angegeben haben, speichert Azure AD Identitätsdaten für die folgenden Dienste in australischen Rechenzentren: 
- Azure AD Directory-Verwaltung 
- Authentication

Alle anderen Azure AD-Dienste speichern Kundendaten in globalen Rechenzentren. Informationen zum Ermitteln des Rechenzentrums für einen Dienst finden Sie unter [Wo wir Ihre Daten speichern](https://aka.ms/AADDataMap).

## <a name="microsoft-azure-ad-multi-factor-authentication-mfa"></a>Microsoft Azure AD Multi-Factor Authentication (MFA)

MFA speichert identitätsbezogene Kundendaten in globalen Rechenzentren. Weitere Informationen zu den Benutzerinformationen, die vom cloudbasierten Dienst Azure AD MFA und vom Azure MFA-Server erfasst und gespeichert werden, finden Sie unter [Azure AD Multi-Factor Authentication: Benutzerdatensammlung](../authentication/concept-mfa-data-residency.md).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu den oben beschriebenen Features und Funktionen finden Sie in diesen Artikeln:
- [Was ist Multi-Factor Authentication?](../authentication/concept-mfa-howitworks.md)
