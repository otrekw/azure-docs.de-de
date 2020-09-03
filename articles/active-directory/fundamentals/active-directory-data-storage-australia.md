---
title: 'Identitätsdatenspeicher für Kunden in Australien und Neuseeland: Azure AD'
description: Hier erfahren Sie, wo identitätsbezogene Daten für australische Kunden durch Azure Active Directory gespeichert werden.
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
ms.openlocfilehash: 529c0ffa6b629dee124236e334c862c049f472ad
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89318538"
---
# <a name="identity-data-storage-for-australian-and-new-zealand-customers-in-azure-active-directory"></a>Identitätsdatenspeicher für Kunden in Australien und Neuseeland in Azure Active Directory

Identitätsdaten werden von Azure AD an einem geografischen Standort basierend auf der Adresse gespeichert, die Ihre Organisation beim Abonnieren eines Microsoft-Onlinediensts wie Office 365 und Azure angibt. Informationen darüber, wo die identitätsbezogenen Kundendaten gespeichert werden, finden Sie im Microsoft Trust Center im Abschnitt [Wo wir Ihre Daten speichern](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located).

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