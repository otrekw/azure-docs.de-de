---
title: 'Identitätsdatenspeicher für australische Kunden: Azure AD'
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
ms.openlocfilehash: 0dc659f3d580646c1e86e5e97940268311e5546f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79460533"
---
# <a name="identity-data-storage-for-australian-and-new-zealand-customers-in-azure-active-directory"></a>Identitätsdatenspeicher für Kunden in Australien und Neuseeland in Azure Active Directory

Identitätsdaten werden von Azure AD an einem geografischen Standort basierend auf der Adresse gespeichert, die Ihre Organisation beim Abonnieren eines Microsoft-Onlinediensts wie Office 365 und Azure angibt. Informationen darüber, wo die identitätsbezogenen Kundendaten gespeichert werden, finden Sie im Microsoft Trust Center im Abschnitt [Wo wir Ihre Daten speichern](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located).

> [!NOTE]
> Dienste und Anwendungen, die in Azure AD integriert werden, haben Zugriff auf identitätsbezogene Kundendaten. Evaluieren Sie alle von Ihnen verwendeten Dienste und Anwendungen, um zu ermitteln, wie identitätsbezogenen Kundendaten vom jeweiligen Dienst und von der jeweiligen Anwendung verarbeitet werden und ob die Datenspeicheranforderungen Ihres Unternehmens erfüllt werden. Weitere Informationen zur Datenaufbewahrung bei Microsoft-Diensten finden Sie im Microsoft Trust Center im Abschnitt „Wo wir Ihre Daten speichern“.

Für Kunden, die eine Adresse in Australien oder Neuseeland angegeben haben und die Azure AD Free-Edition nutzen, speichert Azure AD ruhende personenbezogene Informationen in australischen Rechenzentren: 

Alle anderen Azure AD Premium-Dienste speichern Kundendaten in globalen Rechenzentren. Informationen zum Ermitteln des Rechenzentrums für einen Dienst finden Sie unter [Wo wir Ihre Daten speichern](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located).

## <a name="microsoft-azure-multi-factor-authentication-mfa"></a>Microsoft Azure Multi-Factor Authentication (MFA)

Der MFA-Dienst in Azure AD speichert identitätsbezogene Kundendaten im Ruhezustand in globalen Rechenzentren. Weitere Informationen zu den Benutzerinformationen, die von den cloudbasierten Diensten Azure MFA und Azure MFA Server erfasst und gespeichert werden, finden Sie unter [Datenresidenz und Kundendaten für Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-data-residency). Wenn Kunden MFA verwenden, werden ihre Daten außerhalb von Rechenzentren in Australien im Ruhezustand gespeichert. 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu den oben beschriebenen Features und Funktionen finden Sie in diesen Artikeln:
- [Was ist Multi-Factor Authentication?](https://docs.microsoft.com/azure/active-directory/authentication/multi-factor-authentication)
