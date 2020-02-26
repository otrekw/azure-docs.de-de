---
title: 'Identitätsdatenspeicher für Kunden in Australien und Neuseeland: Azure AD'
description: Hier erfahren Sie, wo identitätsbezogene Daten für australische Kunden durch Azure Active Directory gespeichert werden.
services: active-directory
author: msaburnley
manager: daveba
ms.author: ajburnle
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 12/13/2019
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 850298719d5636e964b0c338d7a2a4cc9bb8aece
ms.sourcegitcommit: f255f869c1dc451fd71e0cab340af629a1b5fb6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/16/2020
ms.locfileid: "77370295"
---
# <a name="identity-data-storage-for-australian-and-new-zealand-customers-in-azure-active-directory"></a>Identitätsdatenspeicher für Kunden in Australien und Neuseeland in Azure Active Directory

Identitätsdaten werden von Azure AD an einem geografischen Standort basierend auf der Adresse gespeichert, die Ihre Organisation beim Abonnieren eines Microsoft-Onlinediensts wie Office 365 und Azure angibt. Informationen darüber, wo die identitätsbezogenen Kundendaten gespeichert werden, finden Sie im Microsoft Trust Center im Abschnitt [Wo wir Ihre Daten speichern](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located).

> [!NOTE]
> Dienste und Anwendungen, die in Azure AD integriert werden, haben Zugriff auf identitätsbezogene Kundendaten. Evaluieren Sie alle von Ihnen verwendeten Dienste und Anwendungen, um zu ermitteln, wie identitätsbezogenen Kundendaten vom jeweiligen Dienst und von der jeweiligen Anwendung verarbeitet werden und ob die Datenspeicheranforderungen Ihres Unternehmens erfüllt werden. Weitere Informationen zur Datenaufbewahrung bei Microsoft-Diensten finden Sie im Microsoft Trust Center im Abschnitt „Wo wir Ihre Daten speichern“.

Für Kunden, die eine Adresse in Australien oder Neuseeland angegeben haben, speichert Azure AD Identitätsdaten für die folgenden Dienste in australischen Rechenzentren: 
- Azure AD Directory-Verwaltung 
- Authentifizierung

Alle anderen Azure AD-Dienste speichern Kundendaten in globalen Rechenzentren. Informationen zum Ermitteln des Rechenzentrums für einen Dienst finden Sie unter [Wo wir Ihre Daten speichern](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located).

## <a name="microsoft-azure-multi-factor-authentication-mfa"></a>Microsoft Azure Multi-Factor Authentication (MFA)

MFA speichert identitätsbezogene Kundendaten in globalen Rechenzentren. Weitere Informationen zu den Benutzerinformationen, die von den cloudbasierten Diensten Azure MFA und Azure MFA Server erfasst und gespeichert werden, finden Sie unter [Datenresidenz und Kundendaten für Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-data-residency).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu den oben beschriebenen Features und Funktionen finden Sie in diesen Artikeln:
- [Was ist Multi-Factor Authentication?](https://docs.microsoft.com/azure/active-directory/authentication/multi-factor-authentication)
