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
ms.openlocfilehash: f7ed1d4c49c46673f1a1c2b5cb08b2467490acae
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89565120"
---
# <a name="identity-data-storage-for-australian-and-new-zealand-customers-in-azure-active-directory"></a>Identitätsdatenspeicher für Kunden in Australien und Neuseeland in Azure Active Directory

Identitätsdaten werden von Azure AD an einem geografischen Standort gespeichert, der auf der Adresse beruht, die Ihre Organisation beim Abonnieren eines Microsoft-Onlinediensts wie Microsoft 365 und Azure angegeben hat. Informationen darüber, wo die identitätsbezogenen Kundendaten gespeichert werden, finden Sie im Microsoft Trust Center im Abschnitt [Wo wir Ihre Daten speichern](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located).

> [!NOTE]
> Dienste und Anwendungen, die in Azure AD integriert werden, haben Zugriff auf identitätsbezogene Kundendaten. Evaluieren Sie alle von Ihnen verwendeten Dienste und Anwendungen, um zu ermitteln, wie identitätsbezogenen Kundendaten vom jeweiligen Dienst und von der jeweiligen Anwendung verarbeitet werden und ob die Datenspeicheranforderungen Ihres Unternehmens erfüllt werden. Weitere Informationen zur Datenaufbewahrung bei Microsoft-Diensten finden Sie im Microsoft Trust Center im Abschnitt „Wo wir Ihre Daten speichern“.

Für Kunden, die eine Adresse in Australien oder Neuseeland angegeben haben, speichert Azure AD Identitätsdaten für die folgenden Dienste in australischen Rechenzentren: 
- Azure AD Directory-Verwaltung 
- Authentication

Alle anderen Azure AD-Dienste speichern Kundendaten in globalen Rechenzentren. Informationen zum Ermitteln des Rechenzentrums für einen Dienst finden Sie unter [Wo wir Ihre Daten speichern](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located).

## <a name="microsoft-azure-multi-factor-authentication-mfa"></a>Microsoft Azure Multi-Factor Authentication (MFA)

MFA speichert identitätsbezogene Kundendaten in globalen Rechenzentren. Weitere Informationen zu den Benutzerinformationen, die von den cloudbasierten Diensten Azure MFA und Azure MFA Server erfasst und gespeichert werden, finden Sie unter [Datenresidenz und Kundendaten für Azure Multi-Factor Authentication](../authentication/concept-mfa-data-residency.md).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu den oben beschriebenen Features und Funktionen finden Sie in diesen Artikeln:
- [Was ist Multi-Factor Authentication?](../authentication/concept-mfa-howitworks.md)